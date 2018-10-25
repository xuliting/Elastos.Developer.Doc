# Carrier for Android development document

## 1 Environmental preparation

1. Two Android phones with API21 or higher.
2. A personal computer with AndroidStudio installed.

## 2 Build the project

1. Download android.sdk-debug.tar.gz at <https://github.com/elastos/Elastos.NET.Carrier.Android.SDK/releases>.
2. Create a new android project like CarrierDemo in AndroidStudio, select API21 or above for minimum SDK.
3. Copy the elacarrier.jar and libs/* folders in android.sdk-debug.tar.gz to CarrierDemo/app/libs/.
    ![2.3](../images/Carrier_For_Android/2.3.png)
4. Modify CarrierDemo/app/build.gradle, Add the following to it：
    ````java
    android {
        ......
        sourceSets {
            main {
                jniLibs.srcDirs = ['libs']
            }
        }
    }
    ````
5. Add internet permissions in AndroidManifest.xml.

## 3 Start Carrier

1. Create a new DefaultCarrierOptions.java and inherit it from Carrier.Options. Set BootstrapNodes, you can refer to CarrierDemo/app/src/main/java/org/elastos/carrier/demo/DefaultCarrierOptions.java.
2. Create a new DefaultCarrierHandler.java and inherit from the AbstractCarrierHandler, you can refer to CarrierDemo/app/src/main/java/org/elastos/carrier/demo/DefaultCarrierHandler.java.
3. Create a new CarrierHelper.java to provide a simple API. The new startCarrier function is used to start the Carrier. Add an instance of DefaultCarrierHandler and DefaultCarrierHandler to this function, and finally call Carrier.start(). The implementation can refer to CarrierDemo/app/src/main/java/org/elastos/carrier/demo/CarrierHelper.java.
    ```java
    public final class CarrierHelper {
        ......
        public static void startCarrier(Context context) {
            try {
                String dir = context.getFilesDir().getAbsolutePath();
                Carrier.Options options = new DefaultCarrierOptions(dir);
                CarrierHandler handler = new DefaultCarrierHandler();

                Carrier.getInstance(options, handler);
                Carrier carrier = Carrier.getInstance();

                carrier.start(1000);
                Logger.info("start carrier.");
            } catch (Exception e) {
                Logger.error("Failed to start carrier.", e);
            }
        }
    }
    ```
4. Call CarrierHelper.startCarrier() in MainActivity.java.
5. Override the onConnection() function in DefaultCarrierHandler.java to listen for the connection state of the Carrier and BootstrapNode (Online/Offline).
6. Run CarrierDemo and the carrier will start up.

## 4 Display and scan address (optional)

In order to add friends quickly, the Scan QR code function has been added to CarrierDemo. This function is independent of Carrier usage and can be ignored.

1. Display the address. Add the MyAddr button and click to display the QR code. For details, see the showAddress() function of MainActivity.java.
2. Scan your friend's address. Add CAMERA, VIBRATE and other permissions, add ScanAddr button, and click to scan the QR code. The specific implementation can refer to the scanAddress() function of MainActivity.java.

## 5 Add a friend

1. The two phones are called A and B respectively. CarrierDemo is installed.
2. When both A and B are in the Online state, A obtains the B's friend address and calls the addFriend() function to add B. This function uses B's Address. See addHelp() for CarrierHelper.java.
    ```java
    public final class CarrierHelper {
        ......
        public static void addFriend(String peerAddr) {
            try {
                String userId = Carrier.getIdFromAddress(peerAddr);
                if(Carrier.getInstance().isFriend(userId)) {
                    Logger.info("Carrier ignore to add friend address: " + peerAddr);
                    return;
                }

                Carrier.getInstance().addFriend(peerAddr, CARRIER_HELLO_AUTH);
                Logger.info("Carrier add friend address: " + peerAddr);
            } catch (Exception e) {
                Logger.error("Failed to add friend.", e);
            }
            return;
        }
    }
    ```
3. When A calls addFriend(), the added party receives the onFriendRequest() callback, in which the Carrier converts A's Address to UserId, from which Carrier will use UserId for identification. This function can be overridden in DefaultCarrierHandler.java for friend authentication processing. Refer to acceptHelp() of CarrierHelper.java.
    ```java
    public final class CarrierHelper {
        ......
        public static void acceptFriend(String peerUserId, String hello) {
            try {
                if (hello.equals(CARRIER_HELLO_AUTH) == false) {
                    Logger.error("Ignore to accept friend, not expected.");
                    return;
                }

                Carrier.getInstance().AcceptFriend(peerUserId);
                Logger.info("Carrier accept friend UserId: " + peerUserId);
            } catch (Exception e) {
                Logger.error("Failed to add friend.", e);
            }
        }
    }
    ```
4. After passing B's authentication, A will receive the onFriendAdded() callback, which can be overridden in DefaultCarrierHandler.java for subsequent processing.
5. Existing friends can't be added repeatedly. You can get the list of friends through the getFriends() function.

## 6 Sending a message

1. After both A and B are online, the other party will receive the onFriendConnection() callback, which can be overridden in DefaultCarrierHandler.java for subsequent processing.
    ```java
    public class DefaultCarrierHandler extends AbstractCarrierHandler {
        ......
        @Override
        public void onFriendConnection(Carrier carrier, String friendId, ConnectionStatus status) {
            Logger.info("Carrier friend connect. peer UserId: " + friendId + " status:" + status);
            if(status == ConnectionStatus.Connected) {
                CarrierHelper.setPeerUserId(friendId);
            }
        }
    }
    ```
2. When both A and B are in the Online state, you can send a message to the other party via the sendFriendMessage() function. You can refer to the SendMessage() of CarrierHelper.java.
    ```java
    public final class CarrierHelper {
        ......
        public static void sendMessage(String message) {
            if(sPeerUserId == null) {
                Logger.error("Failed to send message, friend not found.");
                return;
            }

            try {
                Carrier.getInstance().sendFriendMessage(sPeerUserId, message);
                Logger.info("Carrier send message to UserId: " + sPeerUserId
                        + "\nmessage: " + message);
            } catch (Exception e) {
                Logger.error("Failed to send message.", e);
            }
        }
    }
    ```
3. When A sends a message to B, B receives the onFriendMessage() callback, which can be overridden in DefaultCarrierHandler.java for message processing.
    ```java
    public class DefaultCarrierHandler extends AbstractCarrierHandler {
        ......
        @Override
        public void onFriendMessage(Carrier carrier, String from, String message) {
            Logger.info("Carrier receiver message from UserId: " + from
                    + "\nmessage: " + message);
        }
    }
    ```

## 7 Create Session

1. Carrier can establish a P2P connection through session.
2. First you need to initialize the manager of the Session. The manager's callback is triggered when another party makes a connection request. This can be done with reference to the initSessionManager() function of CarrierSessionHelper.java.
    ```java
    public final class CarrierSessionHelper {
        ......
        public static void initSessionManager(ManagerHandler handler) {
            try {
                Manager manager = Manager.getInstance();
                if(manager != null) {
                    return;
                }

                Manager.getInstance(Carrier.getInstance(), handler);
                Logger.info("Session manager initialized.");
            } catch (Exception e) {
                Logger.error("Failed to init session manager.", e);
            }
        }
    }
    ```
3. The session state can be obtained through the StreamHandler.onStateChanged() callback. This function can be overridden in DefaultSessionHandler.java for state processing.

4. A creates session, which can be implemented by referring to the newSessionAndStream() function of CarrierSessionHelper.java.
   * Initialize SessionManager and call Manager.getInstance(Carrier.getInstance(), sessionHandler) to initialize.
   * The Manager.newSession() function creates a session.
   * Add a Stream via the Session.addStream function.
    ```java
    public final class CarrierSessionHelper {
        public static CarrierSessionInfo newSessionAndStream(String peer) {
            CarrierSessionInfo sessionInfo = null;

            try {
                sessionInfo = new CarrierSessionInfo();

                Logger.info("Carrier new session. peer:" + peer);
                Manager carrierSessionManager = Manager.getInstance();
                if (carrierSessionManager == null) {
                    Logger.error("Failed to new session, manager not initialized.");
                    return null;
                }
                sessionInfo.mSession = carrierSessionManager.newSession(peer);

                Logger.info("Carrier add a unreliable stream to session.");
                int dataOptions = 0;
                sessionInfo.mStream0 = sessionInfo.mSession.addStream(StreamType.Application, dataOptions, sessionInfo.mSessionHandler);

                Logger.info("Carrier add a reliable stream to session.");
                dataOptions = Stream.PROPERTY_RELIABLE;
                sessionInfo.mStream1 = sessionInfo.mSession.addStream(StreamType.Text, dataOptions, sessionInfo.mSessionHandler);
            } catch (Exception e) {
                Logger.error("Failed to new session or stream.", e);
            }

            return sessionInfo;
        }
    }
    ```
5. After creating the session and initializing it, after calling the Session.request() function, B will receive the manager's onSessionRequest() callback. In the callback, the newSessionAndStream() function is also called to establish the B-side session.
6. After creating the session and initializing it, call Session.replyRequest() and A will receive a SessionRequestCompleteHandler.onCompletion() callback.
7. When B's Stream state becomes TransportReady, Session.start() is called.
    ```java
    public class MainActivity extends Activity {
        ......
        private ManagerHandler mSessionManagerHandler = new ManagerHandler() {
            @Override
            public void onSessionRequest(Carrier carrier, String from, String sdp) {
                CarrierSessionInfo sessionInfo = CarrierSessionHelper.newSessionAndStream(CarrierHelper.getPeerUserId());
                if(sessionInfo == null) {
                    Logger.error("Failed to new session.");
                    return;
                }
                boolean wait = sessionInfo.mSessionState.waitForState(CarrierSessionInfo.SessionState.SESSION_STREAM_INITIALIZED, 10000);
                if(wait == false) {
                    deleteSession();
                    Logger.error("Failed to wait session initialize.");
                    return;
                }

                CarrierSessionHelper.replyRequest(sessionInfo);
                wait = sessionInfo.mSessionState.waitForState(CarrierSessionInfo.SessionState.SESSION_STREAM_TRANSPORTREADY, 10000);
                if(wait == false) {
                    deleteSession();
                    Logger.error("Failed to wait session initialize.");
                    return;
                }

                sessionInfo.mSdp = sdp;
                CarrierSessionHelper.startSession(sessionInfo);

                mCarrierSessionInfo = sessionInfo;
            }
        }
    }
    ```
8. A waits for onCompletion() and calls Session.start().
    ```java
    public class CarrierSessionInfo {
        ......
        public CarrierSessionInfo() {
            ......
            mSessionListener = new DefaultSessionHandler.OnSessionListener() {
                ......
                @Override
                public void onCompletion(Session session, int state, String reason, String sdp) {
                    mSdp = sdp;
                    mSessionState.maskState(SessionState.SESSION_REQUEST_COMPLETED);
                }
            }
        }
    }

    public class MainActivity extends Activity {
        ......
        private void createSession() {
            ......
            CarrierSessionHelper.requestSession(sessionInfo);
            wait = sessionInfo.mSessionState.waitForState(CarrierSessionInfo.SessionState.SESSION_REQUEST_COMPLETED, 10000);
            if(wait == false) {
                deleteSession();
                Logger.error("Failed to wait session request.");
                return;
            }

            CarrierSessionHelper.startSession(sessionInfo);

            mCarrierSessionInfo = sessionInfo;
        }
    }
    ```
9. When the Stream status of both A and B become Connected, the Session connection is created successfully.

## 8 Sending data via Session

1. When both Sessions A and B are in the Connected state, you can send data to the other party through the Stream.writeData() function. Refer to sendData() of CarrierSessionHelper.java.
    ```java
    public final class CarrierSessionHelper {
        ......
        public static int sendData(Stream stream, byte[] data) {
            int sent = -1;
            try {
                sent = stream.writeData(data);
                Logger.info("Session send data to stream: " + stream
                        + "\ndata: " + new String(data)
                        + "\nsent: " + sent);
            } catch (Exception e) {
                Logger.error("Failed to send session data.", e);
            }

            return sent;
        }
    }
    ```
2. When A sends data to B, B will receive the onStreamData() callback, which can be overridden in DefaultSessionHandler.java for data processing.
    ```java
    public class DefaultSessionHandler extends AbstractStreamHandler implements SessionRequestCompleteHandler {
        ......
        @Override
        public void onStreamData(Stream stream, byte[] data) {
            if(mOnSessionListener != null) {
                mOnSessionListener.onStreamData(stream, data);
            }
        }
    }
    ```
