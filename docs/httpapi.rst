.. _chapter-httpapi:

========
HTTP API
========

| 記憶保存されている計測データをアプリケーションから検索・取得するためのHTTP REST APIを用意しています。
| 端末から送信されるIoT Topic `data_chunk <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#section-iottopicmessages-datachunk>`_ メッセージを継続的に受信しながらTimestream DBへ保存する処理は、別のLambda関数によって行われます。
| 本HTTP APIは、 入力パラメータからTimestream DB上の計測データに対するクエリ・コマンドを生成し、それを実行した結果をレスポンスとしてAPI呼び出し元へ返します。

| We provide a HTTP REST API for an application to retrieve past measurement data stored.
| We implement another Lambda function, **IotReceiverFunction**, which continues to receive and store measurement data in IoT Topic `data_chunk <https://omoikane-fw.readthedocs.io/ja/latest/iot_topic_messages.html#section-iottopicmessages-datachunk>`_ messages from the terminals to Timestream DB.
| This HTTP API will create a query command to measurement data on Timestream DB from the API request parameters, execute it and return its result as a response to the API caller.

.. _section-httpapi-iotretrieverfunction:

IotRetrieverFunction
====================

*API Type*
^^^^^^^^^^

REST HTTP (Implemented by AWS Lambda Function URL)

*URL*
^^^^^

- When stage name of the deployment is "**dev**"

See ``IotRetrieverFunctionUrl`` in https://github.com/questar-ac/aws_soshinki-backend/blob/main/cdk/cdk-outputs.dev.json

- When stage name of the deployment is "**prod**"

See ``IotRetrieverFunctionUrl`` in https://github.com/questar-ac/aws_soshinki-backend/blob/main/cdk/cdk-outputs.prod.json

*Functions*
^^^^^^^^^^^

.. toctree::
    :maxdepth: 2

    ./api_iotretriever_functions

*Parameter Examples*
^^^^^^^^^^^^^^^^^^^^

.. toctree::
    :maxdepth: 3

    ./api_iotretriever_parameters
