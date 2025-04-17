.. _chapter-httpapi:

========
HTTP API
========

| 記憶保存されている計測データをアプリケーションから検索・取得するためのHTTP APIが用意されています。
| 端末から送信されるIoT Topic `data_chunk <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#section-iottopicmessages-datachunk>`_ メッセージを継続的に受信しながらTimestream DBへ保存する処理は、別のLambda関数によって行われています。
| 本HTTP APIは、 入力パラメータからTimestream DB上の計測データに対するクエリ・コマンドを生成し、それを実行した結果をレスポンスとしてAPI呼び出し元へ返します。

| We provide HTTP API for application to retrieve past measurement data stored.
| We implement another Lambda function, IoTReceiverFunction, that continues to receive and store measurement data in IoT Topic `data_chunk <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#section-iottopicmessages-datachunk>`_ messages from the terminal to Timestream DB.
| This HTTP API will create a query command to measurement data on Timestream DB from the caller's parameters, execute it and return its result as a response to the caller.

.. _section-httpapi-iotretrieverfunction:

IoTRetrieverFunction
====================

*API Type*
^^^^^^^^^^

HTTP (Lambda Function URLs)

*URL*
^^^^^

- When stage name of deployment  is "dev"

See ``IoTRetrieverFunctionUrl`` in https://github.com/questar-ac/aws_soshinki-backend/tree/main/cdk-outputs.dev.json

- When stage name of deployment is "prod"

See ``IoTRetrieverFunctionUrl`` in https://github.com/questar-ac/aws_soshinki-backend/tree/main/cdk-outputs.prod.json

<*Under Construction*>
