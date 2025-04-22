.. _chapter-websocket:

=========
WebSocket
=========

| 端末からのリアルタイム計測データをアプリケーションが受け取るためのWebSocketが用意されています。
| 本WebSocketは、バックエンドへの接続口であると同時に、端末から送信されるIoT Topic `data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#section-iottopicmessages-datasum>`_ メッセージをアプリケーションへ転送する中継者としても機能します。

| We provide WebSocket for application to receive realtime measurement data that the terminal transmits.
| This WebSocket not only serves as a connection port to the backend, but also works as a relay function for IoT Topic `data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#section-iottopicmessages-datasum>`_ messages from the terminal to be transferred to application.

.. _section-websocket-soshinkiwebsocket:

soshinkiWebIfWebSocket
======================

*API Type*
^^^^^^^^^^

WebSocket (API Gateway)

*URL*
^^^^^

- When stage name of the deployment is "**dev**"

See ``soshinkiWebIfWebSocketUrl`` in https://github.com/questar-ac/aws_soshinki-backend/blob/main/cdk/cdk-outputs.dev.json

- When stage name of the deployment is "**prod**"

See ``soshinkiWebIfWebSocketUrl`` in https://github.com/questar-ac/aws_soshinki-backend/blob/main/cdk/cdk-outputs.prod.json

*Routes*
^^^^^^^^

- **$connect**

| クライアントが本WebSocketへ接続したとき、生成されたコネンションIDを記憶する
| When a client connects to this WebSocket, the generated connection ID is stored.

- **$disconnect**

| クライアントが本WebSocketから切断したとき、記憶済みのコネンションIDを削除する
| Delete the stored connection ID when the client disconnects from this WebSocket.

- **$default**

| `IoT Topics <https://omoikane-fw.readthedocs.io/ja/latest/interface.html#iot-topics>`_ 経由で受信したイベントメッセージを、 `PostToConnectionCommand[< ApiGatewayManagementApiClient] <https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/apigatewaymanagementapi/command/PostToConnectionCommand/>`_ によってコネクション・クライントへ転送する
| Forward event messages received via the `IoT Topics <https://omoikane-fw.readthedocs.io/ja/latest/interface.html#iot-topics>`_ to the client connected by calling `PostToConnectionCommand[< ApiGatewayManagementApiClient] <https://docs.aws.amazon.com/AWSJavaScriptSDK/v3/latest/client/apigatewaymanagementapi/command/PostToConnectionCommand/>`_.

*Message format*
^^^^^^^^^^^^^^^^

``{ "message": <event> }``

*Content of evnet*
^^^^^^^^^^^^^^^^^^

- IoT Topic (``event._context.topic``) == ``'aws/questar/soshinki/noise/data_sum'``

  * リオン [RION] NL-42A
    
    ``<event>`` = `noise1/data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#noise1-data-sum>`_
  * リオン [RION] NL-43
    
    ``<event>`` = `noise2/data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#noise2-data-sum>`_
  * アコー [ACO] TYPE3666 内蔵騒音計
    Noise measure embedded in unit
    
    ``<event>`` = `noise4/data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#noise4-data-sum>`_

- IoT Topic (``event._context.topic``) == ``'aws/questar/soshinki/vibration/data_sum'``

  * リオン [RION] VM-55
    
    ``<event>`` = `vibration1/data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#vibration1-data-sum>`_
  * アコー [ACO] TYPE3666 内蔵振動計
    Vibration measure embedded in unit
    
    ``<event>`` = `vibration4/data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#vibration4-data-sum>`_

- IoT Topic (``event._context.topic``) == ``'aws/questar/soshinki/weather/data_sum'``

  * misol Weather Station WH24C
    
    ``<event>`` = `weather1/data_sum <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#weather1-data-sum>`_

*Sample code of client*
^^^^^^^^^^^^^^^^^^^^^^^

Python
``````

.. code-block:: python

    import json
    import websocket
    
    ws_url = "<soshinkiWebIfWebSocketUrl>"
    
    def handle_event_data(data):
        print("IoT Topic message: ", data)
        #
        # Some processes to handle the event data
        #
    
    def on_message(ws, message):
        print("WebSocket message: ", message)
        message_obj = json.loads(message)
        handle_event_data(message_obj['data'])
    
    def on_error(ws, error):
        print("WebSocket error: " error)
    
    def on_close(ws, close_status_code, close_msg):
        print("WebSocket is disconnected.")
    
    def on_open(ws):
        print("WebSocket is connected.")    
    
    if __name__ == "__main__":
        ws = websocket.WebSocketApp(
            ws_url,
            on_message=on_message,
            on_error=on_error,
            on_close=on_close,
            on_open=on_open,
        )
    
        ws.run_forever()

JavaScript
``````````
.. code-block:: javascript

    const ws_url = "<soshinkiWebIfWebSocketUrl>"
    
    const socket = new WebSocket(ws_url)
    
    socket.addEventListener('open', (event) => {
        console.log("WebSocket is connected.")
    });
    
    socket.addEventListener('close', (event) => {
        console.log("WebSocket is disconnected.")
    });
    
    socket.addEventListener('error', (event) => {
        console.log("WebSocket error: ", event);
    });
    
    socket.addEventListener('message', (event) => {
        console.log("WebSocket message: ", event.data);
        const payload = JSON.parse(event.data);
        handleEventData(payload);
    });
    
    function handleEventData(data) {
        console.log("IoT Topic message: ", data);
    
        const { device_data_type, data_sum, _context } = data.message;
    
        const iotTopic = _context.topic.split('/');
    
        if (iotTopic.includes('noise')) {
            console.log(`Noise level [${data_sum.timestamp}] = ${data_sum.noise_latest}`);
            //
            // Some processes to visualize the noise data
            //
        } else if (iotTopic.includes('vibration')) {
            console.log(`Vibration level [${data_sum.timestamp}] = ${data_sum.vibration_latest}`);
            //
            // Some processes to visualize the vibration data
            //
        } else if (iotTopic.includes('weather')) {
            console.log(`Temperature [${data_sum.timestamp}] = ${data_sum.temperature_latest}`);
            console.log(`Humidity [${data_sum.timestamp}] = ${data_sum.humidity_latest}`);
            //
            // Some processes to visualize the weather data
            //
        } else {
            console.error(`Unimplemented event.device_data_type: ${device_data_type}`);
        }
    }
