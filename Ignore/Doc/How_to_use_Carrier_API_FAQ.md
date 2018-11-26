# Carrier FAQ

## Basic concept introduction

1. What is `Carrier`?

    `Carrier` is a decentralized `F2F`-based messaging and data transfer platform that consists of two parts:

    - `Carrier` module

      The basic module of the whole `Carrier` network completes node friend addition, deletion, and message transmission/reception between friend nodes.

    - `Session` module

      Based on the message of `Carrier` module, an extension module is implemented to complete the data communication between friends of nodes.

2. Decentralized network

    `Carrier` transforms the original `TCP/IP` network based on `IPv4/IPv6` addressing into a virtual communication network based on `Carrier` Node `ID` (`base58` encoding) addressing. This virtual network breaks through the pure `TCP/IP` communication limitations due to the network topology between nodes.

3. Node type

    - `Boostrap` node

      Such nodes must be placed at an accessible public address and resident. And the `Public Key` of the publishing node is used when the ordinary node accesses the `Carrier` network.

      It will not participate in the application business, and will be completely transparent to the use of `Carrier` network.

    - Ordinary nodes

4. what is `F2F` communication?

    "F" is `Friend`, and any node must be `Friend` in order to allow message and data communication. The message and data communication between `Friend` is completely encrypted.

5. Node `UserId` and `Address`

    - `UserId`

      The `UserId` is the unique identifier of the node and will not change after it is generated. Please note that the data cache file of the node is saved.

    - `Address`

      `Address` is a combination of `UserId`, Address Check Code (`Nospam`) and `Checksum`. The current node's `Address` can be obtained through the `ela_get_address()` interface. The peer node `Address` is required only when `ela_add_friend` is called. The node `Address` must be obtained through a third-party channel.

6. The role of `Address`

    To prevent malicious addition of `Friend`, `Carrier` adds `Address` for being added as `Friend` by other nodes. If you are harassed, you can reset the `Address` by calling the `ela_set_nospam()` interface, and the Ignore `Address` will no longer receive any notifications.

## How to use the `Carrier` module

1. `Carrier` creation and operation

    a. Call `ela_new()` to create and initialize the `Carrier` and create a `Carrier` instance.

    b. Call `ela_run()` to run the `Carrier` node.

2. Several callbacks of the representation status of the `Carrier` node

    - `ready`

      This callback is triggered when the node is ready. The application must wait until this callback before calling api to communicate with friend.

    - `connection_status`

      This callback is triggered when the connection state of the node changes.

    - `friend_connection`

      This callback is triggered when the friend's status changes.

3. How to add `Friend`

    To add a `friend` by calling `ela_add_friend`, you need to pass in the `address` of the peer. You must have the peer `online` to be called successfully.

    When a node requests to add a friend, the receiver will trigger `friend_request`, you can choose to call `ela_accept_friend()` to accept the request, or simply ignore the no processing.

4. How to send a message to a friend

    Call `ela_send_friend_message()` to send a message to your friend. When this method is called, its own node must be in the `ready` state, and the peer must be in the `online` state.

## How to use the `Session` module

1. What is `Session`?

    The `Carrier Session` is a `Stream` container. You can add multiple `Streams` in a `Session` (up to 4), but each time you add a `Stream`, the system resources consumed will be multiplied. Therefore, the number of `Stream` is appropriately created according to requirements, not as much as possible.

2. `Sesion` and `Carrier` initialization sequence

    You must have a `Carrier` instance to allow the `ela_session_init()` interface to be called to initialize the `Session` module. You can call `ela_session_init()` to initialize the `Session` module before calling the `ela_run()` interface, or you can call the `ela_session_init()` interface to initialize the `Session` module in the `ElaCallbacks` callback implementation.

3. Prerequisites for calling other interfaces of the `Session` module

    - The `Session` module must be initialized by calling the `ela_session_init()` interface
    - Must ensure that both nodes of the `Session` are `Friend`
    - Must ensure that the peer node is in the `Online` state (this step must be for `ela_session_request/reply_request/start`)

4. `Session` creation process

    - Initiator

      a. The current node calls `ela_session_new()` to create a `Session` instance.

      b. Call `ela_session_add_stream` to add a `Stream` as needed

      c. After waiting for the `Stream` state to be `initilized`, call the `ela_session_request()` interface and send it to the peer friend.

      d. After the peer friend `reply session`, the `session_request_oncomplete` callback will be triggered. At this time, `ela_sesssion_start()` can be called to perform `session` negotiation and start `session` channel creation.

      e. After the `Stream` status is `connected`, data can be transferred via `ela_write_stream()`.

    - Receiving end

      a. In the `session_request callback` trigger reminder to receive a friend `session request`, call `ela_session_new()` to create a `Session` instance.

      b. Call `ela_session_add_stream` to add a `Stream` as required. The `option` of the `Stream` must be the same as the initiator.

      c. After waiting for the `Stream` state to be `initilized`, respond to the `session request` of the initiator through the `ela_session_reply_request()` interface.

      d. After waiting for the `Stream` state to be `tansport ready`, call `ela_sesssion_start()` for `session` negotiation and start `session` channel creation.

      e. After the `Stream` status is `connected`, data can be transferred via `ela_write_stream()`.

    After the data communication is completed, the `ela_session_close()` interface is called to close the `Session`. Before calling the `ela_session_close()` interface, it is recommended to call the interface `ela_session_remove_stream()` to delete all `stream` instances in the `Session` container.

    `Session` creation process timing diagram:

    ![Timing diagram](../images/carrier_session_sequence_diagram.png)

5. `Stream` life cycle

    The `stream` in the `Session` container has the following states:

    - `Initalized` --- Asynchronous trigger after calling `ela_session_add_stream()` interface
    - `Transport ready` --- Asynchronous trigger after calling `ela_session_request/reply_request()` interface
    - `Connecting` --- Calling the `ela_session_start()` interface to trigger
    - `Connected` --- Asynchronous trigger after calling `ela_session_start()` interface
    - `Deactivated` --- Asynchronous trigger after calling `ela_session_start()` interface
    - `Closed` --- Trigger when `ela_session_closed()` is called to close the `session` instance or when the peer closes the `session`.
    - `Failed` --- After the error in the above process

## `Carrier` error analysis

1. `ela_get_error()` interface

    If an error occurs after calling the `Carrier` interface, the error code can be obtained through the `ela_get_error()` interface to analyze the cause of the error.

## Thread in `Carrier`

1. `Carrier` runtime `ElaCallbacks` trigger thread

    All `ElaCallbacks` associated with the `Carrier` instance are triggered by threads that call the `ela_run()` interface, including `friend request/added/removed/friend_connection`, as well as `message` receiving and `invite/invite reply`.

2. Can I complete a `carrier` based application with a single thread (one thread)?

    Yes, but you must consider the application logic with an asynchronous idea. You can create a `Carrier` instance and run the `Carrier` with the `ela_run()` interface on the main thread. The application service can be placed into the `on_idle callback` to be fragmented after being split.

3. Can I call the `Carrier` interface in the `Carrier Callbacks` callback?

    Yes, the `Carrier` interface can be called in `Callbacks` as long as the call dependency condition of the interface is met. For example, you can call the `ela_accept_friend()` interface in the `friend_request callback` to accept the peer as `fiend`.

4. `Session` module `ElaStreamCallbacks` trigger thread

    The `ela_session_new()` interface internally creates a `worker` thread that handles the `Session` related data reception and callback triggers in `ElaStreamCallbacks`, including `stream_data()` to receive stream data.

5. Can I call the `Carrier/Session` interface in the `Session` related `ElaStreamCallbacks` callback?

    Yes, the `Carrier/Session` interface can be called in `Callbacks` as long as the calling dependency of the called interface is met. For example, you can call `ela_session_start` in `session_request_oncomplete` callback to establish a `Session` connection.

6. The `sesssion_request` callback is in which thread is triggered to be called

    The `session_request` and `session_request_oncomplete` callback functions are triggered by the thread that calls the `ela_run()` interface in the `Carrier` module, and will not be triggered in the `Session` related `worker` thread.