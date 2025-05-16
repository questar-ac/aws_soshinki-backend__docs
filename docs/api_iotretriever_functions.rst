.. _section-api-iotretriever-functions:


.. _section-api-iotretriever-functions-config:

config
======

.. _section-api-iotretriever-functions-config-get:

config.get
^^^^^^^^^^

.. http:put:: /

    本APIの構成設定情報を取得する

    **Request Example**

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: 1e783884????????????????????????????????????????????????????????

        {"type": "config.get", "operation": {"configValues": ["timeMinuteOffset", "timeOutputFormat", "timeBinIntervalDefault"]}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | Bodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"config.get"``
    :<json array<string> operation.configValues[<ValueName>]: | <ValueName> - 取得対象設定値名
                                                              | ``"timeMinuteOffset"``       : ローカル時刻のUTC時間に対する分単位差分
                                                              |                                Minute offset of local time to UTC time
                                                              | ``"timeOutputFormat"``       : 日付時刻値の出力形式フォーマット
                                                              |                                Output format of timestamp
                                                              | ``"timeBinIntervalDefault"`` : `command.periodics`_ リクエストのパラメータ **Request JSON Object: operation.parameters.sql.timeBinInterval** に対するデフォルト設定値
                                                              |                                Default value for **Request JSON Object: operation.parameters.sql.timeBinInterval** parameter of `command.periodics`_ request

    **Response Example**

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "configValues": {
            "timeMinuteOffset": 540,
            "timeOutputFormat": "yyyy-MM-dd HH:mm:ss.SSS",
            "timeBinIntervalDefault": "10m"
          }
        }

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accepts :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the config.get request

    :>json string operation.configValues.timeMinuteOffset: | ローカル時刻のUTC時間に対する分単位差分（現在値）
                                                           | Minute offset of local time to UTC time, current setting
    :>json string operation.configValues.timeOutputFormat: | 日付時刻値の出力形式フォーマット（現在値）
                                                           | Output format of timestamp, current setting
                                                           | (*) See `format_datetime function - Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html#:~:text=format_datetime%28timestamp,%20varchar%28x%29%29>`_
    :>json string operation.configValues.timeBinIntervalDefault: | `command.periodics`_ リクエストのパラメータ **Request JSON Object: operation.parameters.sql.timeBinInterval** に対するデフォルト設定値（現在値）
                                                                 | Default value for **Request JSON Object: operation.parameters.sql.timeBinInterval** parameter of  `command.periodics`_ request, current setting

.. note ::

    | **timeMinuteOffset** は IotRetrieverFunction のデプロイ先AWSリージョンによって以下の値に設定されます :
    | **timeMinuteOffset** is set to the following values depending on AWS region where IotRetrieverFunction is deployed :

+----------------+--------------------------------+-------------------------------------------+------------+
| Region         | timeMinuteOffset Default Value | City (Timezone Location)                  | UTC Offset |
+================+================================+===========================================+============+
| ap-northeast-1 | 540                            | Asia/Tokyo                                | UTC+9      |
+----------------+--------------------------------+-------------------------------------------+------------+
| ap-south-1     | 330                            | Mumbai (Asia/Kolkata)                     | UTC+5:30   |
+----------------+--------------------------------+-------------------------------------------+------------+
| ap-southeast-2 | 330                            | Australia/Sydney                          | UTC+10     |
+----------------+--------------------------------+-------------------------------------------+------------+
| eu-central-1   | 120                            | Frankfurt (Europe/Berlin)                 | UTC+2      |
+----------------+--------------------------------+-------------------------------------------+------------+
| eu-west-1      | 60                             | Ireland (Europe/Dublin)                   | UTC+1      |
+----------------+--------------------------------+-------------------------------------------+------------+
| us-east-1      | -240                           | US North Virginia (America/New_York)      | UTC-4      |
+----------------+--------------------------------+-------------------------------------------+------------+
| us-east-2      | -240                           | US Ohio ((America/New_York))              | UTC-4      |
+----------------+--------------------------------+-------------------------------------------+------------+
| us-gov-west-1  | -420                           | US North California (America/Los_Angeles) | UTC-7      |
+----------------+--------------------------------+-------------------------------------------+------------+
| us-west-2      | -420                           | US Oregon (America/Los_Angeles)           | UTC-7      |
+----------------+--------------------------------+-------------------------------------------+------------+

.. _section-api-iotretriever-functions-config-set:

config.set
^^^^^^^^^^

.. http:put:: /
    :synopsis: config.set

    本APIの構成設定情報を変更する

    **Request Example**

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: 674987b8????????????????????????????????????????????????????????

        {"type": "config.set", "operation": {"configValues": {"timeMinuteOffset": 600, "timeOutputFormat": "yyyy-MM-dd HH:mm:ss", "timeBinIntervalDefault": "30m"}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | Bodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"config.set"``
    :<json string operation.configValues.timeMinuteOffset: | ローカル時刻のUTC時間に対する分単位差分
                                                           | Minute offset of local time to UTC time
    :<json string operation.configValues.timeOutputFormat: | 日付時刻値の出力形式フォーマット
                                                           | Output format of timestamp
                                                           | (*) See `format_datetime function - Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html#:~:text=format_datetime%28timestamp,%20varchar%28x%29%29>`_
    :<json string operation.configValues.timeBinIntervalDefault: | ``command.periodics`` リクエストのパラメータ **Request JSON Object: operation.parameters.sql.timeBinInterval** に対するのデフォルト設定値
                                                                 | Default value for **Request JSON Object: operation.parameters.sql.timeBinInterval** parameter of ``command.periodics`` request

    **Response Example**

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "configValues": {
            "timeMinuteOffset": 600,
            "timeOutputFormat": "yyyy-MM-dd HH:mm:ss",
            "timeBinIntervalDefault": "30m"
          }
        }

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accepts :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the config.set request

    :>json string configValues.timeMinuteOffset: | ローカル時刻のUTC時間に対する分単位差分（変更後の値）
                                                 | Minute offset of local time to UTC time, after the request was performed
    :>json string configValues.timeOutputFormat: | 日付時刻値の出力形式フォーマット（変更後の値）
                                                 | Output format of timestamp, after the request was performed
                                                 | (*) See `format_datetime function - Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html#:~:text=format_datetime%28timestamp,%20varchar%28x%29%29>`_
    :>json string configValues.timeBinIntervalDefault: | ``command.periodics`` リクエストのパラメータ **Request JSON Object: operation.parameters.sql.timeBinInterval** に対するデフォルト設定値（変更後の値）
                                                       | Default value for **Request JSON Object: operation.parameters.sql.timeBinInterval** parameter of ``command.periodics`` request, after the request was performed

.. _section-api-iotretriever-functions-parameter:

parameter
=========

.. _section-api-iotretriever-functions-parameter-get:

parameter.get
^^^^^^^^^^^^^

.. http:put:: /

    計測データに対するデータベース・パラメータを取得する

    **Request Example**

    - **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: 93ee6aba????????????????????????????????????????????????????????

        {"type": "parameter.get", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}}}

    - **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: 0a9b2433????????????????????????????????????????????????????????

        {"type": "parameter.get", "operation": {"target": {"deviceType": "noise1"}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | Bodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"parameter.get"``
    :>json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JSON Object: parameters.sql.wherePhrase** に全端末を対象とする SQL クエリ文 ``WHERE`` 句が返される 
                                             | Can be omitted. If omitted, ``WHERE`` phrase of SQL query statement to all terminals will be returned with **Response JSON Object: parameters.sql.wherePhrase**.
    :>json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_

    **Response Example**

    - **operation.target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceId": "SK000010",
            "deviceType": "noise1"
          },
          "parameters": {
            "databaseName": "soshinki_noise1_20250502",
            "tableName": "noise1_chunk",
            "sql": {
            "fromPhrase": "FROM soshinki_noise1_20250502.noise1_chunk",
            "wherePhrase": "WHERE measure_name = 'multi' AND device_id = 'SK000010'"
            }
          }
        }

    - **operation.target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceType": "noise1"
          },
          "parameters": {
            "databaseName": "soshinki_noise1_20250502",
            "tableName": "noise1_chunk",
            "sql": {
            "fromPhrase": "FROM soshinki_noise1_20250502.noise1_chunk",
            "wherePhrase": "WHERE measure_name = 'multi'"
            }
          }
        }

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accepts :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the parameter.get request

    :>json string target.deviceId: | 端末ID（ **Request JSON Object: operation.target.deviceId** で指定した値と同一 ）
                                   | Terminal ID（ Same value as the one specified by **Request JSON Object: operation.target.deviceId** ）
    :>json string target.deviceType: | Device Type（ **Request JSON Object: operation.target.deviceType** で指定した値と同一 ）
    :>json string parameters.databaseName: | 対象計測データ格納されている Timestream DB のデータベース名
                                           |  Database name of Timestream DB on which target measure data is stored
    :>json string parameters.tableName: | 対象計測データ格納されている Timestream DB のテーブル名
                                        | Table name of Timestream DB on which target measure date is stored
    :>json string parameters.sql.fromPhrase: | 対象計測データ格納されている Timestream DB に対する SQL クエリ文の ``FROM`` 句
                                             | ``FROM`` phrase of SQL query statement to target Timesteram DB
    :>json string parameters.sql.wherePhrase: | 対象計測データ格納されている Timestream DB に対する SQL クエリ文の ``WHERE`` 句
                                              | ``WHERE`` phrase of SQL query statement to target Timesteram DB

.. _section-api-iotretriever-functions-command:

command
=======

.. _section-api-iotretriever-functions-command-count:

command.count
^^^^^^^^^^^^^

.. http:put:: /

    計測データの個数を取得する

    **Request Example**

    - **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: f72eed43????????????????????????????????????????????????????????

        {"type": "command.count", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "time BETWEEN ago(1d) AND now()", "queryStatement": false}, "requestValues": ["Lp_count"]}}}

    - **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: a8f3292b????????????????????????????????????????????????????????

        {"type": "command.count", "operation": {"target": {"deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "time BETWEEN ago(1d) AND now()", "queryStatement": false}, "requestValues": ["Lp_count"]}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | Bodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"command.count"``
    :<json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JOSN Object: responseValues** に全端末の要求された値が返される
                                             | Can be omitted. If omitted, the requested values to all terminals will be returned with **Response JOSN Object: responseValues**.
    :<json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_
    :<json string operation.parameters.sql.whereTimePhrase: | SQL クエリ文の ``WHERE`` 節に指定する時間指定記述
                                                            | Time phrase to ``WHERE`` section of SQL query statement
                                                            | Examples: `whereTimePhrase <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#wheretimephrase>`_
                                                            | (*) See `Date / time operators - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-operators.html>`_ , `Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html>`_
    :<json boolean operation.parameters.sql.queryStatement: | 本リクエスト実行のために生成した SQL クエリ文文字列を返すかどうかを指定する
                                                            | Whether a string of SQL query statement to execute this request should be returned or not
    :<json array<string> operation.parameters.requestValues[<ValueName>]: | **<ValueName>** - 取得対象の値名文字列
                                                                          |                   Name string specifying required value
                                                                          | - Specifiable values in case of **deviceType == "noise1" OR "noise2" OR "noise4"**
                                                                          | ``"Lp_count"`` : 騒音計測データの個数
                                                                          |                  Number of noise measure data
                                                                          | - Specifiable values in case of **deviceType == "vibration1" OR "vibration4"**
                                                                          | ``"Lv_count"`` : 振動計測データの個数 
                                                                          |                  Number of vibration measure data
                                                                          | - Specifiable values in case of **deviceType == "weather1"**
                                                                          | ``"Wh_count"`` : 気象計測データの個数
                                                                          |                  Number of weather measure data

    **Response Example**

    - **operation.target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceId": "SK000010",
            "deviceType": "noise1"
          },
          "responseValues": [
            {
              "Lp_count": 63860
            }
          ]
        }

    - **target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceType": "noise1"
          },
          "responseValues": [
            {
              "device_id": "SK000010",
              "Lp_count": 63860
            },
            {
              "device_id": "SK000020",
              "Lp_count": 63780
            },
            {
              "device_id": "SK000030",
              "Lp_count": 63840
            }
          ]
        }

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accepts :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the command.count request

    :>json string target.deviceId: | 端末ID（ **Request JSON Object: operation.target.deviceId** で指定した値と同一 ）
                                   | Terminal ID（ Same value as the one specified by **Request JSON Object: operation.target.deviceId** ）
                                   | **Request JSON Object: operation.target.deviceId** を省略した場合、本項目は存在しない
                                   | This item is not present if **Request JSON Object: operation.target.deviceId** was omitted.
    :>json string target.deviceType: | Device Type（ **Request JSON Object: operation.target.deviceType** で指定した値と同一 ）
                                     | Device Type（ Same value as the one specified by **Request JSON Object: operation.target.deviceType** ）
    :>json array<object> responseValues[<ValueObject>]: | **<ValueObject>** -- 戻り値を含むオブジェクト
                                                        |                      Object containing returned value
                                                        | - Returned value if **Request JSON Object: operation.target.deviceId** was omitted
                                                        | ``device_id`` : 端末ID
                                                        |                 Terminal ID
                                                        | - Returned value in case of **deviceType == "noise1" OR "noise2" OR "noise4"**
                                                        | ``Lp_count``  : 指定期間内の騒音計測データの個数
                                                        |                 Number of noise measure data within the time period
                                                        | - Returned value in case of **deviceType == "vibration1" OR "vibration4"**
                                                        | ``Lv_count``  : 指定期間内の振動計測データの個数
                                                        |                 Number of vibration measure data within the time period
                                                        | - Returned value in case of **deviceType == "weather1"**
                                                        | ``Wh_count``  : 指定期間内の気象計測データの個数
                                                        |                 Number of weather measure data within the time period
    :>json string sql.queryStatement: | リクエスト実行のために生成した SQL クエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.

.. _section-api-iotretriever-functions-command-aggregate:

command.aggregate
^^^^^^^^^^^^^^^^^

.. http:put:: /
    :synopsis: command.aggregate

    単一期間の計測データを集計値の取得する

    **Request Example**

    - **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: ebf33f5e????????????????????????????????????????????????????????

        {"type": "command.aggregate", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-05-14'", "queryStatement": false}, "requestValues": ["Lp_max", "Lp_max_time", "Lp_min", "Lp_min_time", "Lp_avg", "Lp_l5", "Lp_l10", "Lp_l50", "Lp_l90", "Lp_l95"]}}}

    - **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: 2436e332????????????????????????????????????????????????????????

        {"type": "command.aggregate", "operation": {"target": {"deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-05-14'", "queryStatement": false}, "requestValues": ["Lp_max", "Lp_max_time", "Lp_min", "Lp_min_time", "Lp_avg", "Lp_l5", "Lp_l10", "Lp_l50", "Lp_l90", "Lp_l95"]}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | Bodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"command.aggregate"``
    :<json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JOSN Object: responseValues** に全端末の要求された値が返される
                                             | Can be omitted. If omitted, the requested values to all terminals will be returned with **Response JOSN Object: responseValues**.
    :<json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_
    :<json string operation.parameters.sql.whereTimePhrase: | SQL クエリ文の ``WHERE`` 節に指定する時間指定記述
                                                            | Time phrase to ``WHERE`` section of SQL query statement
                                                            | Examples: `whereTimePhrase <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#wheretimephrase>`_
                                                            | (*) See `Date / time operators - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-operators.html>`_ , `Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html>`_
    :<json boolean operation.parameters.sql.queryStatement: | 本リクエスト実行のために生成した SQL クエリ文文字列を返すかどうかを指定する
                                                            | Whether a string of SQL query statement to execute this request should be returned or not
    :<json array<string> operation.parameters.requestValues[<ValueNames>]: | **<ValueNames>** -- 取得対象の値名文字列
                                                                           |                     Name strings specifying required values
                                                                           | - Specifiable values in case of **deviceType == "noise1" OR "noise2" OR "noise4"**
                                                                           | ``"Lp_max"``           : 集計期間内での騒音最大値
                                                                           |                          Maximum noise value within the aggregate time period
                                                                           | ``"Lp_max_time"``      : Lp_max 値の取得時刻
                                                                           |                          Time at which the Lp_max value was acquired
                                                                           | ``"Lp_min"``           : 集計期間内での騒音最小値
                                                                           |                          Minimum noise value within the aggregate time period
                                                                           | ``"Lp_min_time"``      : Lp_min 値の取得時刻
                                                                           |                          Time at which the Lp_min value was acquired
                                                                           | ``"Lp_avg"``           : 集計期間内での騒音平均値
                                                                           |                          Average noise value within the aggregate time period
                                                                           | ``"Lp_l5"``            : 集計期間内での騒音L5値
                                                                           |                          L5 noise value within the aggregate time period
                                                                           | ``"Lp_l10"``           : 集計期間内での騒音L10値
                                                                           |                          L10 noise value within the aggregate time period
                                                                           | ``"Lp_l50"``           : 集計期間内での騒音L50値
                                                                           |                          L50 noise value within the aggregate time period
                                                                           | ``"Lp_l90"``           : 集計期間内での騒音L90値
                                                                           |                          L90 noise value within the aggregate time period
                                                                           | ``"Lp_l95"``           : 集計期間内での騒音L95値
                                                                           |                          L95 noise value within the aggregate time period
                                                                           | - Specifiable values in case of **deviceType == "vibration1" OR "vibration4"**
                                                                           | ``"Lv_max"``           : 集計期間内での振動最大値
                                                                           |                          Maximum vibration value within the aggregate time period
                                                                           | ``"Lv_max_time"``      : Lv_max 値の取得時刻
                                                                           |                          Time at which the Lv_max value was acquired
                                                                           | ``"Lv_min"``           : 集計期間内での振動最小値
                                                                           |                          Minimum vibration value within the aggregate time period
                                                                           | ``"Lv_min_time"``      : Lv_min 値の取得時刻
                                                                           |                          Time at which the Lv_min value was acquired
                                                                           | ``"Lv_avg"``           : 集計期間内での振動平均値
                                                                           |                          Average vibration value within the aggregate time period
                                                                           | ``"Lv_l5"``            : 集計期間内での振動L5値
                                                                           |                          L5 vibration value within the aggregate time period
                                                                           | ``"Lv_l10"``           : 集計期間内での振動L10値
                                                                           |                          L10 vibration value within the aggregate time period
                                                                           | ``"Lv_l50"``           : 集計期間内での振動L50値
                                                                           |                          L50 vibration value within the aggregate time period
                                                                           | ``"Lv_l90"``           : 集計期間内での振動L90値
                                                                           |                          L90 vibration value within the aggregate time period
                                                                           | ``"Lv_l95"``           : 集計期間内での振動L95値
                                                                           |                          L95 vibration value within the aggregate time period
                                                                           | - Specifiable values in case of **deviceType == "weather1"**
                                                                           | ``"Wh_temp_max"``      : 集計期間内での温度最大値
                                                                           |                          Maximum temperature value within the aggregate time period
                                                                           | ``"Wh_temp_max_time"`` : Wh_temp_max 値の取得時刻
                                                                           |                          Time at which the Wh_temp_max value was acquired
                                                                           | ``"Wh_temp_min"``      : 集計期間内での温度最小値
                                                                           |                          Minimum temperature value within the aggregate time period
                                                                           | ``"Wh_temp_min_time"`` : Wh_temp_min 値の取得時刻
                                                                           |                          Time at which the Wh_temp_min value was acquired
                                                                           | ``"Wh_temp_avg"``      : 集計期間内での温度平均値
                                                                           |                          Average temperature value within the aggregate time period
                                                                           | ``"Wh_hum_max"``       : 集計期間内での湿度最大値
                                                                           |                          Maximum humidity value within the aggregate time period
                                                                           | ``"Wh_hum_max_time"``  : Wh_hum_max 値の取得時刻
                                                                           |                          Time at which the Wh_temp_max value was acquired
                                                                           | ``"Wh_hum_min"``       : 集計期間内での湿度最小値
                                                                           |                          Minimum humidity value within the aggregate time period
                                                                           | ``"Wh_hum_min_time"``  : Wh_hum_min 値の取得時刻
                                                                           |                          Time at which the Wh_hum_min value was acquired
                                                                           | ``"Wh_hum_avg"``       : 集計期間内での湿度平均値
                                                                           |                          Average humidity value within the aggregate time period
                                                                           | ``"Wh_ws_max"``        : 集計期間内での風速最大値
                                                                           |                          Maximum wind speed value within the aggregate time period
                                                                           | ``"Wh_ws_max_time"``   : Wh_ws_max 値の取得時刻
                                                                           |                          Time at which the Wh_ws_max value was acquired
                                                                           | ``"Wh_ws_min"``        : 集計期間内での風速最小値
                                                                           |                          Minimum wind speed value within the aggregate time period
                                                                           | ``"Wh_ws_min_time"``   : Wh_ws_min 値の取得時刻
                                                                           |                          Time at which the Wh_ws_min value was acquired
                                                                           | ``"Wh_ws_avg"``        : 集計期間内での風速平均値
                                                                           |                          Average wind speed value within the aggregate time period
                                                                           | ``"Wh_gws_max"``       : 集計期間内での瞬間風速最大値
                                                                           |                          Maximum gust wind speed value within the aggregate time period
                                                                           | ``"Wh_gws_max_time"``  : Wh_gws_max 値の取得時刻
                                                                           |                          Time at which the Wh_gws_max value was acquired
                                                                           | ``"Wh_gws_min"``       : 集計期間内での瞬間風速最小値
                                                                           |                          Minimum gust wind speed value within the aggregate time period
                                                                           | ``"Wh_gws_min_time"``  : Wh_gws_min 値の取得時刻
                                                                           |                          Time at which the Wh_gws_min value was acquired
                                                                           | ``"Wh_gws_avg"``       : 集計期間内での瞬間風速平均値
                                                                           |                          Average gust wind speed value within the aggregate time period
                                                                           | ``"Wh_arf_max"``       : 集計期間内での積算雨量最大値
                                                                           |                          Maximum accumulation rainfall value within the aggregate time period
                                                                           | ``"Wh_arf_max_time"``  : Wh_arf_max 値の取得時刻
                                                                           |                          Time at which the Wh_arf_max value was acquired
                                                                           | ``"Wh_arf_min"``       : 集計期間内での積算雨量最小値
                                                                           |                          Minimum accumulation rainfall value within the aggregate time period
                                                                           | ``"Wh_arf_min_time"``  : Wh_gws_min 値の取得時刻
                                                                           |                          Time at which the Wh_arf_min value was acquired
                                                                           | ``"Wh_arf_avg"``       : 集計期間内での積算雨量平均値
                                                                           |                          Average accumulation rainfall value within the aggregate time period
                                                                           | ``"Wh_uv_max"``        : 集計期間内での紫外線量最大値
                                                                           |                          Maximum UV index value within the aggregate time period
                                                                           | ``"Wh_uv_max_time"``   : Wh_arf_max 値の取得時刻
                                                                           |                          Time at which the Wh_uv_max value was acquired
                                                                           | ``"Wh_uv_min"``        : 集計期間内での紫外線量最小値
                                                                           |                          Minimum UV index value within the aggregate time period
                                                                           | ``"Wh_uv_min_time"``   : Wh_gws_min 値の取得時刻
                                                                           |                          Time at which the Wh_uv_min value was acquired
                                                                           | ``"Wh_uv_avg"``        : 集計期間内での紫外線量平均値
                                                                           |                          Average UV index value within the aggregate time period
                                                                           | ``"Wh_li_max"``        : 集計期間内での光照度最大値
                                                                           |                          Maximum light illuminance value within the aggregate time period
                                                                           | ``"Wh_li_max_time"``   : Wh_arf_max 値の取得時刻
                                                                           |                          Time at which the Wh_li_max value was acquired
                                                                           | ``"Wh_li_min"``        : 集計期間内での光照度最小値
                                                                           |                          Minimum light illuminance value within the aggregate time period
                                                                           | ``"Wh_li_min_time"``   : Wh_li_min 値の取得時刻
                                                                           |                          Time at which the Wh_li_min value was acquired
                                                                           | ``"Wh_li_avg"``        : 集計期間内での光照度平均値
                                                                           |                          Average light illuminance value within the aggregate time period

    **Response Example**

    - **target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceId": "SK000010",
            "deviceType": "noise1"
          },
          "responseValues": [
            {
            "Lp_max": 83.7,
            "Lp_max_time": "2025-05-14 12:19:46.497",
            "Lp_min": 36.2,
            "Lp_min_time": "2025-05-14 11:52:18.510",
            "Lp_avg": 61.8,
            "Lp_l5": 78.3,
            "Lp_l10": 76.9,
            "Lp_l50": 63.2,
            "Lp_l90": 39.9,
            "Lp_l95": 37.5
            }
          ]
        }

    - **target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceType": "noise1"
          },
          "responseValues": [
            {
              "device_id": "SK000010",
              "Lp_max": 83.7,
              "Lp_max_time": "2025-05-14 12:19:46.497",
              "Lp_min": 36.2,
              "Lp_min_time": "2025-05-14 11:52:18.510",
              "Lp_avg": 61.8,
              "Lp_l5": 78.3,
              "Lp_l10": 76.9,
              "Lp_l50": 63.2,
              "Lp_l90": 39.9,
              "Lp_l95": 37.5
            },
            {
              "device_id": "SK000020",
              "Lp_max": 85.1,
              "Lp_max_time": "2025-05-14 13:53:57.790",
              "Lp_min": 36.3,
              "Lp_min_time": "2025-05-14 09:00:05.251",
              "Lp_avg": 68.5,
              "Lp_l5": 79,
              "Lp_l10": 78.1,
              "Lp_l50": 74.8,
              "Lp_l90": 38.4,
              "Lp_l95": 37.9
            },
            {
              "device_id": "SK000030",
              "Lp_max": 87.3,
              "Lp_max_time": "2025-05-14 10:45:47.944",
              "Lp_min": 36.1,
              "Lp_min_time": "2025-05-14 09:07:03.951",
              "Lp_avg": 49.5,
              "Lp_l5": 67.4,
              "Lp_l10": 65.8,
              "Lp_l50": 45.1,
              "Lp_l90": 37,
              "Lp_l95": 36.8
            }
          ]
        }

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accept :http:get:`/` only. The access was http.method != :http:method:`get` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the command.aggregate request

    :>json string target.deviceId: | 端末ID（ **Request JSON Object: operation.target.deviceId** で指定した値と同一 ）
                                   | Terminal ID（ Same value as the one specified by **Request JSON Object: operation.target.deviceId** ）
                                   | **Request JSON Object: operation.target.deviceId** を省略した場合は、本項目は存在しない
                                   | This item is not present if **Request JSON Object: operation.target.deviceId** was omitted.
    :>json string target.deviceType: | Device Type（ **Request JSON Object: operation.target.deviceType** で指定した値と同一 ）
                                     | Device Type（ Same value as the one specified by **Request JSON Object: operation.target.deviceType** ）
    :>json array<object> responseValues[<ValuesObject>]: | **<ValueObject>** -- 戻り値を含むオブジェクト
                                                         |                      Object containing returned values
                                                         | - Returned value if **Request JSON Object: operation.target.deviceId** was omitted
                                                         | ``device_id``        : 端末ID
                                                         |                        Terminal ID
                                                         | - Returned values in case of **deviceType == "noise1" OR "noise2" OR "noise4"**
                                                         | ``Lp_max``           : 集計期間内での騒音最大値
                                                         |                        Maximum noise value within the aggregate time period
                                                         | ``Lp_max_time``      : Lp_max 値の取得時刻
                                                         |                        Time at which the Lp_max value was acquired
                                                         | ``Lp_min``           : 集計期間内での騒音最小値
                                                         |                        Minimum noise value within the aggregate time period
                                                         | ``Lp_min_time``      : Lp_min 値の取得時刻
                                                         |                        Time at which the Lp_min value was acquired
                                                         | ``Lp_avg``           : 集計期間内での騒音平均値
                                                         |                        Average noise value within the aggregate time period
                                                         | ``Lp_l5``            : 集計期間内での騒音L5値
                                                         |                        L5 noise value within the aggregate time period
                                                         | ``Lp_l10``           : 集計期間内での騒音L10値
                                                         |                        L10 noise value within the aggregate time period
                                                         | ``Lp_l50``           : 集計期間内での騒音L50値
                                                         |                        L50 noise value within the aggregate time period
                                                         | ``Lp_l90``           : 集計期間内での騒音L90値
                                                         |                        L90 noise value within the aggregate time period
                                                         | ``Lp_l95``           : 集計期間内での騒音L95値
                                                         |                        L95 noise value within the aggregate time period
                                                         | ``Lv_max``           : 集計期間内での振動最大値
                                                         | - Returned values in case of **deviceType == "vibration1" OR "vibration4"**
                                                         |                        Maximum vibration value within the aggregate time period
                                                         | ``Lv_max_time``      : Lv_max 値の取得時刻
                                                         |                        Time at which the Lv_max value was acquired
                                                         | ``Lv_min``           : 集計期間内での振動最小値
                                                         |                        Minimum vibration value within the aggregate time period
                                                         | ``Lv_min_time``      : Lv_min 値の取得時刻
                                                         |                        Time at which the Lv_min value was acquired
                                                         | ``Lv_avg``           : 集計期間内での振動平均値
                                                         |                        Average vibration value within the aggregate time period
                                                         | ``Lv_l5``            : 集計期間内での振動L5値
                                                         |                        L5 vibration value within the aggregate time period
                                                         | ``Lv_l10``           : 集計期間内での振動L10値
                                                         |                        L10 vibration value within the aggregate time period
                                                         | ``Lv_l50``           : 集計期間内での振動L50値
                                                         |                        L50 vibration value within the aggregate time period
                                                         | ``Lv_l90``           : 集計期間内での振動L90値
                                                         |                        L90 vibration value within the aggregate time period
                                                         | ``Lv_l95``           : 集計期間内での振動L95値
                                                         |                        L95 vibration value within the aggregate time period
                                                         | - Returned values in case of **deviceType == "weather1"**
                                                         | ``Wh_temp_max``      : 集計期間内での温度最大値
                                                         |                        Maximum temperature value within the aggregate time period
                                                         | ``Wh_temp_max_time`` : Wh_temp_max 値の取得時刻
                                                         |                        Time at which the Wh_temp_max value was acquired
                                                         | ``Wh_temp_min``      : 集計期間内での温度最小値
                                                         |                        Minimum temperature value within the aggregate time period
                                                         | ``Wh_temp_min_time`` : Wh_temp_min 値の取得時刻
                                                         |                        Time at which the Wh_temp_min value was acquired
                                                         | ``Wh_temp_avg``      : 集計期間内での温度平均値
                                                         |                        Average temperature value within the aggregate time period
                                                         | ``Wh_hum_max``       : 集計期間内での湿度最大値
                                                         |                        Maximum humidity value within the aggregate time period
                                                         | ``Wh_hum_max_time``  : Wh_hum_max 値の取得時刻
                                                         |                        Time at which the Wh_temp_max value was acquired
                                                         | ``Wh_hum_min``       : 集計期間内での湿度最小値
                                                         |                        Minimum humidity value within the aggregate time period
                                                         | ``Wh_hum_min_time``  : Wh_hum_min 値の取得時刻
                                                         |                        Time at which the Wh_hum_min value was acquired
                                                         | ``Wh_hum_avg``       : 集計期間内での湿度平均値
                                                         |                        Average humidity value within the aggregate time period
                                                         | ``Wh_ws_max``        : 集計期間内での風速最大値
                                                         |                        Maximum wind speed value within the aggregate time period
                                                         | ``Wh_ws_max_time``   : Wh_ws_max 値の取得時刻
                                                         |                        Time at which the Wh_ws_max value was acquired
                                                         | ``Wh_ws_min``        : 集計期間内での風速最小値
                                                         |                        Minimum wind speed value within the aggregate time period
                                                         | ``Wh_ws_min_time``   : Wh_ws_min 値の取得時刻
                                                         |                        Time at which the Wh_ws_min value was acquired
                                                         | ``Wh_ws_avg``        : 集計期間内での風速平均値
                                                         |                        Average wind speed value within the aggregate time period
                                                         | ``Wh_gws_max``       : 集計期間内での瞬間風速最大値
                                                         |                        Maximum gust wind speed value within the aggregate time period
                                                         | ``Wh_gws_max_time``  : Wh_gws_max 値の取得時刻
                                                         |                        Time at which the Wh_gws_max value was acquired
                                                         | ``Wh_gws_min``       : 集計期間内での瞬間風速最小値
                                                         |                        Minimum gust wind speed value within the aggregate time period
                                                         | ``Wh_gws_min_time``  : Wh_gws_min 値の取得時刻
                                                         |                        Time at which the Wh_gws_min value was acquired
                                                         | ``Wh_gws_avg``       : 集計期間内での瞬間風速平均値
                                                         |                        Average gust wind speed value within the aggregate time period
                                                         | ``Wh_arf_max``       : 集計期間内での積算雨量最大値
                                                         |                        Maximum accumulation rainfall value within the aggregate time period
                                                         | ``Wh_arf_max_time``  : Wh_arf_max 値の取得時刻
                                                         |                        Time at which the Wh_arf_max value was acquired
                                                         | ``Wh_arf_min``       : 集計期間内での積算雨量最小値
                                                         |                        Minimum accumulation rainfall value within the aggregate time period
                                                         | ``Wh_arf_min_time``  : Wh_gws_min 値の取得時刻
                                                         |                        Time at which the Wh_arf_min value was acquired
                                                         | ``Wh_arf_avg``       : 集計期間内での積算雨量平均値
                                                         |                        Average accumulation rainfall value within the aggregate time period
                                                         | ``Wh_uv_max``        : 集計期間内での紫外線量最大値
                                                         |                        Maximum UV index value within the aggregate time period
                                                         | ``Wh_uv_max_time``   : Wh_arf_max 値の取得時刻
                                                         |                        Time at which the Wh_uv_max value was acquired
                                                         | ``Wh_uv_min``        : 集計期間内での紫外線量最小値
                                                         |                        Minimum UV index value within the aggregate time period
                                                         | ``Wh_uv_min_time``   : Wh_gws_min 値の取得時刻
                                                         |                        Time at which the Wh_uv_min value was acquired
                                                         | ``Wh_uv_avg``        : 集計期間内での紫外線量平均値
                                                         |                        Average UV index value within the aggregate time period
                                                         | ``Wh_li_max``        : 集計期間内での光照度最大値
                                                         |                        Maximum light illuminance value within the aggregate time period
                                                         | ``Wh_li_max_time``   : Wh_arf_max 値の取得時刻
                                                         |                        Time at which the Wh_li_max value was acquired
                                                         | ``Wh_li_min``        : 集計期間内での光照度最小値
                                                         |                        Minimum light illuminance value within the aggregate time period
                                                         | ``Wh_li_min_time``   : Wh_li_min 値の取得時刻
                                                         |                        Time at which the Wh_li_min value was acquired
                                                         | ``Wh_li_avg``        : 集計期間内での光照度平均値
                                                         |                        Average light illuminance value within the aggregate time period
    :>json string sql.queryStatement: | リクエスト実行のために生成した SQL クエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.

.. _section-api-iotretriever-functions-command-periodics:

command.periodics
^^^^^^^^^^^^^^^^^

.. http:put:: /

    周期分割期間の計測データの集計値を取得する

    **Request Example**

    - **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: d3845ae6330abd8b6c8b790c7ffac06517052e0620d881a0d96c8010803f81f9

        {"type": "command.periodics", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-05-14'", "timeBinInterval": "1h", "queryStatement": false}, "requestValues": ["Lp_binned_time", "Lp_max", "Lp_max_time", "Lp_min", "Lp_min_time", "Lp_avg", "Lp_l5", "Lp_l10", "Lp_l50", "Lp_l90", "Lp_l95"]}}}

    - **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: f8985830????????????????????????????????????????????????????????

        {"type": "command.periodics", "operation": {"target": {"deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-05-14'", "timeBinInterval": "1h", "queryStatement": true}, "requestValues": ["Lp_binned_time", "Lp_max", "Lp_max_time", "Lp_min", "Lp_min_time", "Lp_avg", "Lp_l5", "Lp_l10", "Lp_l50", "Lp_l90", "Lp_l95"]}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | Bodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"command.periodics"``
    :<json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JOSN Object: responseValues** に全端末の要求された値が返される
                                             | Can be omitted. If omitted, the requested values to all terminals will be returned with **Response JOSN Object: responseValues**.
    :<json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_
    :<json string operation.parameters.sql.whereTimePhrase: | SQL クエリ文の ``WHERE`` 節に指定する時間指定記述
                                                            | Time phrase to ``WHERE`` section of SQL query statement
                                                            | Examples: `whereTimePhrase <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#wheretimephrase>`_
                                                            | (*) See `Date / time operators - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-operators.html>`_ , `Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html>`_
    :<json string operation.parameters.sql.timeBinInterval: | SQL 文 ``BIN`` 関数の引数 ``interval`` として指定する時間間隔指定記述
                                                            | Time phrase as ``interval`` argument to ``BIN`` function of SQL statement
                                                            | 本項目は省略可能、省略された場合は **configValues.timeBinIntervalDefault** の設定値が使われる
                                                            | Can be omitted. If omitted, the value as **configValues.timeBinIntervalDefault** setting will be used.
                                                            | Examples: `timeBinInterval <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#timebininterval>`_
                                                            | (*) See `bin function - Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html#:~:text=bin%28timestamp%2C%20interval%29>`_
    :<json boolean operation.parameters.sql.queryStatement: | 本リクエスト実行のために生成した SQL クエリ文文字列を返すかどうかを指定する
                                                            | Whether a string of SQL query statement to execute this request should be returned or not
    :<json array<string> operation.parameters.requestValues[<ValueNames>]: | **<ValueNames>** -- 取得対象の値名文字列
                                                                           |                     Name strings specifying required values
                                                                           | - Specifiable values in case of **deviceType == "noise1" OR "noise2" OR "noise4"**
                                                                           | ``"Lp_binned_time"``   : 分割集計時刻（騒音データ）
                                                                           |                          Split aggregate time, noise data
                                                                           | ``"Lp_max"``           : 分割集計期間内での騒音最大値
                                                                           |                          Maximum noise value within the split tabulation time period
                                                                           | ``"Lp_max_time"``      : Lp_max 値の取得時刻
                                                                           |                          Time at which the Lp_max value was acquired
                                                                           | ``"Lp_min"``           : 分割期間内での騒音最小値
                                                                           |                          Minimum noise value within the split tabulation time period
                                                                           | ``"Lp_min_time"``      : Lp_min 値の取得時刻
                                                                           |                          Time at which the Lp_min value was acquired
                                                                           | ``"Lp_avg"``           : 分割集計期間内での騒音平均値
                                                                           |                          Average noise value within the split tabulation time period
                                                                           | ``"Lp_l5"``            : 分割集計期間内での騒音L5値
                                                                           |                          L5 noise value within the split tabulation time period
                                                                           | ``"Lp_l10"``           : 分割集計期間内での騒音L10値
                                                                           |                          L10 noise value within the split tabulation time period
                                                                           | ``"Lp_l50"``           : 分割集計期間内での騒音L50値
                                                                           |                          L50 noise value within the split tabulation time period
                                                                           | ``"Lp_l90"``           : 分割集計期間内での騒音L90値
                                                                           |                          L90 noise value within the split tabulation time period
                                                                           | ``"Lp_l95"``           : 分割集計期間内での騒音L95値
                                                                           |                          L95 noise value within the split tabulation time period
                                                                           | - Specifiable values in case of **deviceType == "vibration1" OR "vibration4"**
                                                                           | ``"Lv_binned_time"``   : 分割集計時刻（振動データ）
                                                                           |                          Split aggregate time, vibration data
                                                                           | ``"Lv_max"``           : 分割集計期間内での振動最大値
                                                                           |                          Maximum vibration value within the split tabulation time period
                                                                           | ``"Lv_max_time"``      : Lv_max 値の取得時刻
                                                                           |                          Time at which the Lv_max value was acquired
                                                                           | ``"Lv_min"``           : 分割集計期間内での振動最小値
                                                                           |                          Minimum vibration value within the split tabulation time period
                                                                           | ``"Lv_min_time"``      : Lv_min 値の取得時刻
                                                                           |                          Time at which the Lv_min value was acquired
                                                                           | ``"Lv_avg"``           : 分割集計期間内での振動平均値
                                                                           |                          Average vibration value within the split tabulation time period
                                                                           | ``"Lv_l5"``            : 分割集計期間内での振動L5値
                                                                           |                          L5 vibration value within the split tabulation time period
                                                                           | ``"Lv_l10"``           : 分割集計期間内での振動L10値
                                                                           |                          L10 vibration value within the split tabulation time period
                                                                           | ``"Lv_l50"``           : 分割集計期間内での振動L50値
                                                                           |                          L50 vibration value within the split tabulation time period
                                                                           | ``"Lv_l90"``           : 分割集計期間内での振動L90値
                                                                           |                          L90 vibration value within the split tabulation time period
                                                                           | ``"Lv_l95"``           : 分割集計期間内での振動L95値
                                                                           |                          L95 vibration value within the split tabulation time period
                                                                           | - Specifiable values in case of **deviceType == "weather1"**
                                                                           | ``"Wh_binned_time"``   : 分割集計時刻（気象データ）
                                                                           |                          Split aggregate time, weather data
                                                                           | ``"Wh_temp_max"``      : 分割集計期間内での温度最大値
                                                                           |                          Maximum temperature value within the split tabulation time period
                                                                           | ``"Wh_temp_max_time"`` : Wh_temp_max 値の取得時刻
                                                                           |                          Time at which the Wh_temp_max value was acquired
                                                                           | ``"Wh_temp_min"``      : 分割集計期間内での温度最小値
                                                                           |                          Minimum temperature value within the split tabulation time period
                                                                           | ``"Wh_temp_min_time"`` : Wh_temp_min 値の取得時刻
                                                                           |                          Time at which the Wh_temp_min value was acquired
                                                                           | ``"Wh_temp_avg"``      : 分割集計期間内での温度平均値
                                                                           |                          Average temperature value within the split tabulation time period
                                                                           | ``"Wh_hum_max"``       : 分割集計期間内での湿度最大値
                                                                           |                          Maximum humidity value within the split tabulation time period
                                                                           | ``"Wh_hum_max_time"``  : Wh_hum_max 値の取得時刻
                                                                           |                          Time at which the Wh_temp_max value was acquired
                                                                           | ``"Wh_hum_min"``       : 分割集計期間内での湿度最小値
                                                                           |                          Minimum humidity value within the split tabulation time period
                                                                           | ``"Wh_hum_min_time"``  : Wh_hum_min 値の取得時刻
                                                                           |                          Time at which the Wh_hum_min value was acquired
                                                                           | ``"Wh_hum_avg"``       : 分割集計期間内での湿度平均値
                                                                           |                          Average humidity value within the split tabulation time period
                                                                           | ``"Wh_ws_max"``        : 分割集計期間内での風速最大値
                                                                           |                          Maximum wind speed value within the split tabulation time period
                                                                           | ``"Wh_ws_max_time"``   : Wh_ws_max 値の取得時刻
                                                                           |                          Time at which the Wh_ws_max value was acquired
                                                                           | ``"Wh_ws_min"``        : 分割集計期間内での風速最小値
                                                                           |                          Minimum wind speed value within the split tabulation time period
                                                                           | ``"Wh_ws_min_time"``   : Wh_ws_min 値の取得時刻
                                                                           |                          Time at which the Wh_ws_min value was acquired
                                                                           | ``"Wh_ws_avg"``        : 分割集計期間内での風速平均値
                                                                           |                          Average wind speed value within the split tabulation time period
                                                                           | ``"Wh_gws_max"``       : 分割集計期間内での瞬間風速最大値
                                                                           |                          Maximum gust wind speed value within the split tabulation time period
                                                                           | ``"Wh_gws_max_time"``  : Wh_gws_max 値の取得時刻
                                                                           |                          Time at which the Wh_gws_max value was acquired
                                                                           | ``"Wh_gws_min"``       : 分割集計期間内での瞬間風速最小値
                                                                           |                          Minimum gust wind speed value within the split tabulation time period
                                                                           | ``"Wh_gws_min_time"``  : Wh_gws_min 値の取得時刻
                                                                           |                          Time at which the Wh_gws_min value was acquired
                                                                           | ``"Wh_gws_avg"``       : 分割集計期間内での瞬間風速平均値
                                                                           |                          Average gust wind speed value within the split tabulation time period
                                                                           | ``"Wh_arf_max"``       : 分割集計期間内での積算雨量最大値
                                                                           |                          Maximum accumulation rainfall value within the split tabulation time period
                                                                           | ``"Wh_arf_max_time"``  : Wh_arf_max 値の取得時刻
                                                                           |                          Time at which the Wh_arf_max value was acquired
                                                                           | ``"Wh_arf_min"``       : 分割集計期間内での積算雨量最小値
                                                                           |                          Minimum accumulation rainfall value within the split tabulation time period
                                                                           | ``"Wh_arf_min_time"``  : Wh_gws_min 値の取得時刻
                                                                           |                          Time at which the Wh_arf_min value was acquired
                                                                           | ``"Wh_arf_avg"``       : 分割集計期間内での積算雨量平均値
                                                                           |                          Average accumulation rainfall value within the split tabulation time period
                                                                           | ``"Wh_uv_max"``        : 分割集計期間内での紫外線量最大値
                                                                           |                          Maximum UV index value within the split tabulation time period
                                                                           | ``"Wh_uv_max_time"``   : Wh_arf_max 値の取得時刻
                                                                           |                          Time at which the Wh_uv_max value was acquired
                                                                           | ``"Wh_uv_min"``        : 分割集計期間内での紫外線量最小値
                                                                           |                          Minimum UV index value within the split tabulation time period
                                                                           | ``"Wh_uv_min_time"``   : Wh_gws_min 値の取得時刻
                                                                           |                          Time at which the Wh_uv_min value was acquired
                                                                           | ``"Wh_uv_avg"``        : 分割集計期間内での紫外線量平均値
                                                                           |                          Average UV index value within the split tabulation time period
                                                                           | ``"Wh_li_max"``        : 分割集計期間内での光照度最大値
                                                                           |                          Maximum light illuminance value within the split tabulation time period
                                                                           | ``"Wh_li_max_time"``   : Wh_arf_max 値の取得時刻
                                                                           |                          Time at which the Wh_li_max value was acquired
                                                                           | ``"Wh_li_min"``        : 分割集計期間内での光照度最小値
                                                                           |                          Minimum light illuminance value within the split tabulation time period
                                                                           | ``"Wh_li_min_time"``   : Wh_li_min 値の取得時刻
                                                                           |                          Time at which the Wh_li_min value was acquired
                                                                           | ``"Wh_li_avg"``        : 分割集計期間内での光照度平均値
                                                                           |                          Average light illuminance value within the split tabulation time period
    :>json string sql.queryStatement: | リクエスト実行のために生成した SQL クエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.

    **Response Example**

    - **target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "target": {
                "deviceId": "SK000010",
                "deviceType": "noise1"
            },
            "responseValues": [
                {
                    "Lp_binned_time": "2025-05-14 05:00:00.000",
                    "Lp_max": 46.1,
                    "Lp_max_time": "2025-05-14 05:58:07.251",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 05:53:34.700",
                    "Lp_avg": 36.3,
                    "Lp_l5": 40.1,
                    "Lp_l10": 38.1,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "Lp_binned_time": "2025-05-14 06:00:00.000",
                    "Lp_max": 64.9,
                    "Lp_max_time": "2025-05-14 06:01:45.663",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 06:13:27.107",
                    "Lp_avg": 36.3,
                    "Lp_l5": 39.7,
                    "Lp_l10": 37.5,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "Lp_binned_time": "2025-05-14 07:00:00.000",
                    "Lp_max": 77.4,
                    "Lp_max_time": "2025-05-14 07:44:51.191",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 07:16:50.852",
                    "Lp_avg": 46,
                    "Lp_l5": 68.4,
                    "Lp_l10": 66.1,
                    "Lp_l50": 38.3,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "Lp_binned_time": "2025-05-14 08:00:00.000",
                    "Lp_max": 72.2,
                    "Lp_max_time": "2025-05-14 08:10:26.266",
                    "Lp_min": 34.7,
                    "Lp_min_time": "2025-05-14 08:07:15.819",
                    "Lp_avg": 36.1,
                    "Lp_l5": 38.6,
                    "Lp_l10": 36.8,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "Lp_binned_time": "2025-05-14 09:00:00.000",
                    "Lp_max": 78.3,
                    "Lp_max_time": "2025-05-14 09:51:35.226",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 09:25:25.066",
                    "Lp_avg": 46.6,
                    "Lp_l5": 68.8,
                    "Lp_l10": 67.2,
                    "Lp_l50": 37.2,
                    "Lp_l90": 35.5,
                    "Lp_l95": 35.4
                },
                {
                    "Lp_binned_time": "2025-05-14 10:00:00.000",
                    "Lp_max": 60.4,
                    "Lp_max_time": "2025-05-14 10:59:41.672",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 10:09:26.170",
                    "Lp_avg": 36.4,
                    "Lp_l5": 41.1,
                    "Lp_l10": 38.4,
                    "Lp_l50": 35.6,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "Lp_binned_time": "2025-05-14 11:00:00.000",
                    "Lp_max": 86.2,
                    "Lp_max_time": "2025-05-14 11:32:24.350",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 11:06:30.350",
                    "Lp_avg": 67.3,
                    "Lp_l5": 79.3,
                    "Lp_l10": 78.1,
                    "Lp_l50": 73.8,
                    "Lp_l90": 37.3,
                    "Lp_l95": 36.3
                },
                {
                    "Lp_binned_time": "2025-05-14 12:00:00.000",
                    "Lp_max": 88.7,
                    "Lp_max_time": "2025-05-14 12:19:12.062",
                    "Lp_min": 36.9,
                    "Lp_min_time": "2025-05-14 12:24:54.782",
                    "Lp_avg": 75.8,
                    "Lp_l5": 80.9,
                    "Lp_l10": 79.8,
                    "Lp_l50": 76.7,
                    "Lp_l90": 71.5,
                    "Lp_l95": 67.8
                },
                {
                    "Lp_binned_time": "2025-05-14 13:00:00.000",
                    "Lp_max": 82.5,
                    "Lp_max_time": "2025-05-14 13:11:34.974",
                    "Lp_min": 37.2,
                    "Lp_min_time": "2025-05-14 13:08:41.665",
                    "Lp_avg": 67.9,
                    "Lp_l5": 78.6,
                    "Lp_l10": 77.8,
                    "Lp_l50": 73,
                    "Lp_l90": 39,
                    "Lp_l95": 38.2
                },
                {
                    "Lp_binned_time": "2025-05-14 14:00:00.000",
                    "Lp_max": 82.3,
                    "Lp_max_time": "2025-05-14 14:16:18.271",
                    "Lp_min": 36.4,
                    "Lp_min_time": "2025-05-14 14:58:07.856",
                    "Lp_avg": 66.1,
                    "Lp_l5": 77.9,
                    "Lp_l10": 77.1,
                    "Lp_l50": 73.6,
                    "Lp_l90": 37.8,
                    "Lp_l95": 37.5
                },
                {
                    "Lp_binned_time": "2025-05-14 15:00:00.000",
                    "Lp_max": 93.8,
                    "Lp_max_time": "2025-05-14 15:08:21.024",
                    "Lp_min": 36.7,
                    "Lp_min_time": "2025-05-14 15:02:33.383",
                    "Lp_avg": 59.7,
                    "Lp_l5": 71.5,
                    "Lp_l10": 70,
                    "Lp_l50": 62.5,
                    "Lp_l90": 43.2,
                    "Lp_l95": 39.4
                },
                {
                    "Lp_binned_time": "2025-05-14 16:00:00.000",
                    "Lp_max": 78.8,
                    "Lp_max_time": "2025-05-14 16:10:01.281",
                    "Lp_min": 39,
                    "Lp_min_time": "2025-05-14 16:52:28.097",
                    "Lp_avg": 59.5,
                    "Lp_l5": 70.5,
                    "Lp_l10": 69.1,
                    "Lp_l50": 61.7,
                    "Lp_l90": 45.6,
                    "Lp_l95": 42.4
                },
                {
                    "Lp_binned_time": "2025-05-14 17:00:00.000",
                    "Lp_max": 82.6,
                    "Lp_max_time": "2025-05-14 17:15:54.565",
                    "Lp_min": 35.5,
                    "Lp_min_time": "2025-05-14 17:32:48.396",
                    "Lp_avg": 59.5,
                    "Lp_l5": 78,
                    "Lp_l10": 77,
                    "Lp_l50": 63.7,
                    "Lp_l90": 37.7,
                    "Lp_l95": 36.7
                },
                {
                    "Lp_binned_time": "2025-05-14 18:00:00.000",
                    "Lp_max": 80.3,
                    "Lp_max_time": "2025-05-14 18:10:07.574",
                    "Lp_min": 36.8,
                    "Lp_min_time": "2025-05-14 18:18:35.506",
                    "Lp_avg": 59.3,
                    "Lp_l5": 76.6,
                    "Lp_l10": 74.9,
                    "Lp_l50": 62.7,
                    "Lp_l90": 38.5,
                    "Lp_l95": 37.8
                }
            ]
        }

    - **target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
            "target": {
                "deviceType": "noise1"
            },
            "responseValues": [
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 05:00:00.000",
                    "Lp_max": 46.1,
                    "Lp_max_time": "2025-05-14 05:58:07.251",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 05:53:34.700",
                    "Lp_avg": 36.3,
                    "Lp_l5": 40.1,
                    "Lp_l10": 38.1,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 06:00:00.000",
                    "Lp_max": 64.9,
                    "Lp_max_time": "2025-05-14 06:01:45.663",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 06:13:27.107",
                    "Lp_avg": 36.3,
                    "Lp_l5": 39.7,
                    "Lp_l10": 37.5,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 07:00:00.000",
                    "Lp_max": 77.4,
                    "Lp_max_time": "2025-05-14 07:44:51.191",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 07:16:50.852",
                    "Lp_avg": 46,
                    "Lp_l5": 68.4,
                    "Lp_l10": 66.1,
                    "Lp_l50": 38.3,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 08:00:00.000",
                    "Lp_max": 72.2,
                    "Lp_max_time": "2025-05-14 08:10:26.266",
                    "Lp_min": 34.7,
                    "Lp_min_time": "2025-05-14 08:07:15.819",
                    "Lp_avg": 36.1,
                    "Lp_l5": 38.6,
                    "Lp_l10": 36.8,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 09:00:00.000",
                    "Lp_max": 78.3,
                    "Lp_max_time": "2025-05-14 09:51:35.226",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 09:25:25.066",
                    "Lp_avg": 46.6,
                    "Lp_l5": 68.8,
                    "Lp_l10": 67.2,
                    "Lp_l50": 37.2,
                    "Lp_l90": 35.5,
                    "Lp_l95": 35.4
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 10:00:00.000",
                    "Lp_max": 60.4,
                    "Lp_max_time": "2025-05-14 10:59:41.672",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 10:09:26.170",
                    "Lp_avg": 36.4,
                    "Lp_l5": 41.1,
                    "Lp_l10": 38.4,
                    "Lp_l50": 35.6,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 11:00:00.000",
                    "Lp_max": 86.2,
                    "Lp_max_time": "2025-05-14 11:32:24.350",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 11:06:30.350",
                    "Lp_avg": 67.3,
                    "Lp_l5": 79.3,
                    "Lp_l10": 78.1,
                    "Lp_l50": 73.8,
                    "Lp_l90": 37.3,
                    "Lp_l95": 36.3
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 12:00:00.000",
                    "Lp_max": 88.7,
                    "Lp_max_time": "2025-05-14 12:19:12.062",
                    "Lp_min": 36.9,
                    "Lp_min_time": "2025-05-14 12:24:54.782",
                    "Lp_avg": 75.8,
                    "Lp_l5": 80.9,
                    "Lp_l10": 79.8,
                    "Lp_l50": 76.7,
                    "Lp_l90": 71.5,
                    "Lp_l95": 67.8
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 13:00:00.000",
                    "Lp_max": 82.5,
                    "Lp_max_time": "2025-05-14 13:11:34.974",
                    "Lp_min": 37.2,
                    "Lp_min_time": "2025-05-14 13:08:41.665",
                    "Lp_avg": 67.9,
                    "Lp_l5": 78.6,
                    "Lp_l10": 77.8,
                    "Lp_l50": 73,
                    "Lp_l90": 39,
                    "Lp_l95": 38.2
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 14:00:00.000",
                    "Lp_max": 82.3,
                    "Lp_max_time": "2025-05-14 14:16:18.271",
                    "Lp_min": 36.4,
                    "Lp_min_time": "2025-05-14 14:58:07.856",
                    "Lp_avg": 66.1,
                    "Lp_l5": 77.9,
                    "Lp_l10": 77.1,
                    "Lp_l50": 73.6,
                    "Lp_l90": 37.8,
                    "Lp_l95": 37.5
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 15:00:00.000",
                    "Lp_max": 93.8,
                    "Lp_max_time": "2025-05-14 15:08:21.024",
                    "Lp_min": 36.7,
                    "Lp_min_time": "2025-05-14 15:02:33.383",
                    "Lp_avg": 59.7,
                    "Lp_l5": 71.5,
                    "Lp_l10": 70,
                    "Lp_l50": 62.5,
                    "Lp_l90": 43.2,
                    "Lp_l95": 39.4
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 16:00:00.000",
                    "Lp_max": 78.8,
                    "Lp_max_time": "2025-05-14 16:10:01.281",
                    "Lp_min": 39,
                    "Lp_min_time": "2025-05-14 16:52:28.097",
                    "Lp_avg": 59.5,
                    "Lp_l5": 70.5,
                    "Lp_l10": 69.1,
                    "Lp_l50": 61.7,
                    "Lp_l90": 45.6,
                    "Lp_l95": 42.4
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 17:00:00.000",
                    "Lp_max": 82.6,
                    "Lp_max_time": "2025-05-14 17:15:54.565",
                    "Lp_min": 35.5,
                    "Lp_min_time": "2025-05-14 17:32:48.396",
                    "Lp_avg": 59.5,
                    "Lp_l5": 78,
                    "Lp_l10": 77,
                    "Lp_l50": 63.7,
                    "Lp_l90": 37.7,
                    "Lp_l95": 36.7
                },
                {
                    "device_id": "SK000010",
                    "Lp_binned_time": "2025-05-14 18:00:00.000",
                    "Lp_max": 80.3,
                    "Lp_max_time": "2025-05-14 18:10:07.574",
                    "Lp_min": 36.8,
                    "Lp_min_time": "2025-05-14 18:18:35.506",
                    "Lp_avg": 59.3,
                    "Lp_l5": 76.6,
                    "Lp_l10": 74.9,
                    "Lp_l50": 62.7,
                    "Lp_l90": 38.5,
                    "Lp_l95": 37.8
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 05:00:00.000",
                    "Lp_max": 46.1,
                    "Lp_max_time": "2025-05-14 05:58:07.251",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 05:53:34.700",
                    "Lp_avg": 36.3,
                    "Lp_l5": 40.1,
                    "Lp_l10": 38.1,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 06:00:00.000",
                    "Lp_max": 64.9,
                    "Lp_max_time": "2025-05-14 06:01:45.663",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 06:13:27.107",
                    "Lp_avg": 36.3,
                    "Lp_l5": 39.7,
                    "Lp_l10": 37.5,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 07:00:00.000",
                    "Lp_max": 77.4,
                    "Lp_max_time": "2025-05-14 07:44:51.191",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 07:16:50.852",
                    "Lp_avg": 46,
                    "Lp_l5": 68.4,
                    "Lp_l10": 66.1,
                    "Lp_l50": 38.3,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 08:00:00.000",
                    "Lp_max": 72.2,
                    "Lp_max_time": "2025-05-14 08:10:26.266",
                    "Lp_min": 34.7,
                    "Lp_min_time": "2025-05-14 08:07:15.819",
                    "Lp_avg": 36.1,
                    "Lp_l5": 38.6,
                    "Lp_l10": 36.8,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 09:00:00.000",
                    "Lp_max": 78.3,
                    "Lp_max_time": "2025-05-14 09:51:35.226",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 09:25:25.066",
                    "Lp_avg": 46.6,
                    "Lp_l5": 68.8,
                    "Lp_l10": 67.2,
                    "Lp_l50": 37.2,
                    "Lp_l90": 35.5,
                    "Lp_l95": 35.4
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 10:00:00.000",
                    "Lp_max": 60.4,
                    "Lp_max_time": "2025-05-14 10:59:41.672",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 10:09:26.170",
                    "Lp_avg": 36.4,
                    "Lp_l5": 41.1,
                    "Lp_l10": 38.4,
                    "Lp_l50": 35.6,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 11:00:00.000",
                    "Lp_max": 86.2,
                    "Lp_max_time": "2025-05-14 11:32:24.350",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 11:06:30.350",
                    "Lp_avg": 67.3,
                    "Lp_l5": 79.3,
                    "Lp_l10": 78.1,
                    "Lp_l50": 73.8,
                    "Lp_l90": 37.3,
                    "Lp_l95": 36.3
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 12:00:00.000",
                    "Lp_max": 88.7,
                    "Lp_max_time": "2025-05-14 12:19:12.062",
                    "Lp_min": 36.9,
                    "Lp_min_time": "2025-05-14 12:24:54.782",
                    "Lp_avg": 75.8,
                    "Lp_l5": 80.9,
                    "Lp_l10": 79.8,
                    "Lp_l50": 76.7,
                    "Lp_l90": 71.5,
                    "Lp_l95": 67.8
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 13:00:00.000",
                    "Lp_max": 82.5,
                    "Lp_max_time": "2025-05-14 13:11:34.974",
                    "Lp_min": 37.2,
                    "Lp_min_time": "2025-05-14 13:08:41.665",
                    "Lp_avg": 67.9,
                    "Lp_l5": 78.6,
                    "Lp_l10": 77.8,
                    "Lp_l50": 73,
                    "Lp_l90": 39,
                    "Lp_l95": 38.2
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 14:00:00.000",
                    "Lp_max": 82.3,
                    "Lp_max_time": "2025-05-14 14:16:18.271",
                    "Lp_min": 36.4,
                    "Lp_min_time": "2025-05-14 14:58:07.856",
                    "Lp_avg": 66.1,
                    "Lp_l5": 77.9,
                    "Lp_l10": 77.1,
                    "Lp_l50": 73.6,
                    "Lp_l90": 37.8,
                    "Lp_l95": 37.5
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 15:00:00.000",
                    "Lp_max": 93.8,
                    "Lp_max_time": "2025-05-14 15:08:21.024",
                    "Lp_min": 36.7,
                    "Lp_min_time": "2025-05-14 15:02:33.383",
                    "Lp_avg": 59.7,
                    "Lp_l5": 71.5,
                    "Lp_l10": 70,
                    "Lp_l50": 62.5,
                    "Lp_l90": 43.2,
                    "Lp_l95": 39.4
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 16:00:00.000",
                    "Lp_max": 78.8,
                    "Lp_max_time": "2025-05-14 16:10:01.281",
                    "Lp_min": 39,
                    "Lp_min_time": "2025-05-14 16:52:28.097",
                    "Lp_avg": 59.5,
                    "Lp_l5": 70.5,
                    "Lp_l10": 69.1,
                    "Lp_l50": 61.7,
                    "Lp_l90": 45.6,
                    "Lp_l95": 42.4
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 17:00:00.000",
                    "Lp_max": 82.6,
                    "Lp_max_time": "2025-05-14 17:15:54.565",
                    "Lp_min": 35.5,
                    "Lp_min_time": "2025-05-14 17:32:48.396",
                    "Lp_avg": 59.5,
                    "Lp_l5": 78,
                    "Lp_l10": 77,
                    "Lp_l50": 63.7,
                    "Lp_l90": 37.7,
                    "Lp_l95": 36.7
                },
                {
                    "device_id": "SK000020",
                    "Lp_binned_time": "2025-05-14 18:00:00.000",
                    "Lp_max": 80.3,
                    "Lp_max_time": "2025-05-14 18:10:07.574",
                    "Lp_min": 36.8,
                    "Lp_min_time": "2025-05-14 18:18:35.506",
                    "Lp_avg": 59.3,
                    "Lp_l5": 76.6,
                    "Lp_l10": 74.9,
                    "Lp_l50": 62.7,
                    "Lp_l90": 38.5,
                    "Lp_l95": 37.8
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 05:00:00.000",
                    "Lp_max": 46.1,
                    "Lp_max_time": "2025-05-14 05:58:07.251",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 05:53:34.700",
                    "Lp_avg": 36.3,
                    "Lp_l5": 40.1,
                    "Lp_l10": 38.1,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 06:00:00.000",
                    "Lp_max": 64.9,
                    "Lp_max_time": "2025-05-14 06:01:45.663",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 06:13:27.107",
                    "Lp_avg": 36.3,
                    "Lp_l5": 39.7,
                    "Lp_l10": 37.5,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 07:00:00.000",
                    "Lp_max": 77.4,
                    "Lp_max_time": "2025-05-14 07:44:51.191",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 07:16:50.852",
                    "Lp_avg": 46,
                    "Lp_l5": 68.4,
                    "Lp_l10": 66.1,
                    "Lp_l50": 38.3,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 08:00:00.000",
                    "Lp_max": 72.2,
                    "Lp_max_time": "2025-05-14 08:10:26.266",
                    "Lp_min": 34.7,
                    "Lp_min_time": "2025-05-14 08:07:15.819",
                    "Lp_avg": 36.1,
                    "Lp_l5": 38.6,
                    "Lp_l10": 36.8,
                    "Lp_l50": 35.7,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 09:00:00.000",
                    "Lp_max": 78.3,
                    "Lp_max_time": "2025-05-14 09:51:35.226",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 09:25:25.066",
                    "Lp_avg": 46.6,
                    "Lp_l5": 68.8,
                    "Lp_l10": 67.2,
                    "Lp_l50": 37.2,
                    "Lp_l90": 35.5,
                    "Lp_l95": 35.4
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 10:00:00.000",
                    "Lp_max": 60.4,
                    "Lp_max_time": "2025-05-14 10:59:41.672",
                    "Lp_min": 34.8,
                    "Lp_min_time": "2025-05-14 10:09:26.170",
                    "Lp_avg": 36.4,
                    "Lp_l5": 41.1,
                    "Lp_l10": 38.4,
                    "Lp_l50": 35.6,
                    "Lp_l90": 35.3,
                    "Lp_l95": 35.2
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 11:00:00.000",
                    "Lp_max": 86.2,
                    "Lp_max_time": "2025-05-14 11:32:24.350",
                    "Lp_min": 34.9,
                    "Lp_min_time": "2025-05-14 11:06:30.350",
                    "Lp_avg": 67.3,
                    "Lp_l5": 79.3,
                    "Lp_l10": 78.1,
                    "Lp_l50": 73.8,
                    "Lp_l90": 37.3,
                    "Lp_l95": 36.3
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 12:00:00.000",
                    "Lp_max": 88.7,
                    "Lp_max_time": "2025-05-14 12:19:12.062",
                    "Lp_min": 36.9,
                    "Lp_min_time": "2025-05-14 12:24:54.782",
                    "Lp_avg": 75.8,
                    "Lp_l5": 80.9,
                    "Lp_l10": 79.8,
                    "Lp_l50": 76.7,
                    "Lp_l90": 71.5,
                    "Lp_l95": 67.8
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 13:00:00.000",
                    "Lp_max": 82.5,
                    "Lp_max_time": "2025-05-14 13:11:34.974",
                    "Lp_min": 37.2,
                    "Lp_min_time": "2025-05-14 13:08:41.665",
                    "Lp_avg": 67.9,
                    "Lp_l5": 78.6,
                    "Lp_l10": 77.8,
                    "Lp_l50": 73,
                    "Lp_l90": 39,
                    "Lp_l95": 38.2
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 14:00:00.000",
                    "Lp_max": 82.3,
                    "Lp_max_time": "2025-05-14 14:16:18.271",
                    "Lp_min": 36.4,
                    "Lp_min_time": "2025-05-14 14:58:07.856",
                    "Lp_avg": 66.1,
                    "Lp_l5": 77.9,
                    "Lp_l10": 77.1,
                    "Lp_l50": 73.6,
                    "Lp_l90": 37.8,
                    "Lp_l95": 37.5
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 15:00:00.000",
                    "Lp_max": 93.8,
                    "Lp_max_time": "2025-05-14 15:08:21.024",
                    "Lp_min": 36.7,
                    "Lp_min_time": "2025-05-14 15:02:33.383",
                    "Lp_avg": 59.7,
                    "Lp_l5": 71.5,
                    "Lp_l10": 70,
                    "Lp_l50": 62.5,
                    "Lp_l90": 43.2,
                    "Lp_l95": 39.4
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 16:00:00.000",
                    "Lp_max": 78.8,
                    "Lp_max_time": "2025-05-14 16:10:01.281",
                    "Lp_min": 39,
                    "Lp_min_time": "2025-05-14 16:52:28.097",
                    "Lp_avg": 59.5,
                    "Lp_l5": 70.5,
                    "Lp_l10": 69.1,
                    "Lp_l50": 61.7,
                    "Lp_l90": 45.6,
                    "Lp_l95": 42.4
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 17:00:00.000",
                    "Lp_max": 82.6,
                    "Lp_max_time": "2025-05-14 17:15:54.565",
                    "Lp_min": 35.5,
                    "Lp_min_time": "2025-05-14 17:32:48.396",
                    "Lp_avg": 59.5,
                    "Lp_l5": 78,
                    "Lp_l10": 77,
                    "Lp_l50": 63.7,
                    "Lp_l90": 37.7,
                    "Lp_l95": 36.7
                },
                {
                    "device_id": "SK000030",
                    "Lp_binned_time": "2025-05-14 18:00:00.000",
                    "Lp_max": 80.3,
                    "Lp_max_time": "2025-05-14 18:10:07.574",
                    "Lp_min": 36.8,
                    "Lp_min_time": "2025-05-14 18:18:35.506",
                    "Lp_avg": 59.3,
                    "Lp_l5": 76.6,
                    "Lp_l10": 74.9,
                    "Lp_l50": 62.7,
                    "Lp_l90": 38.5,
                    "Lp_l95": 37.8
                }
            ]
        }

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accepts :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the command.periodics event

    :>json string target.deviceId: | 端末ID（ **Request JSON Object: operation.target.deviceId** で指定した値と同一 ）
                                   | Terminal ID（ Same value as the one specified by **Request JSON Object: operation.target.deviceId** ）
                                   | **Request JSON Object: operation.target.deviceId** を省略した場合、本項目は存在しない
                                   | This item is not present if **Request JSON Object: operation.target.deviceId** was omitted.
    :>json string target.deviceType: | Device Type（ **Request JSON Object: operation.target.deviceType** で指定した値と同一 ）
                                     | Device Type（ Same value as the one specified by **Request JSON Object: operation.target.deviceType** ）
    :>json array<object> responseValues[<ValuesObjects>]: | **<ValuesObjects>** -- 戻り値を含むオブジェクト
                                                          |                        Objects containing returned values
                                                          | - Returned value if Request **JSON Object: operation.target.deviceId** was omitted
                                                          | ``device_id``        : 端末ID
                                                          |                        Terminal ID
                                                          | - Returned values in case of **deviceType == "noise1" OR "noise2" OR "noise4"**
                                                          | ``Lp_binned_time``   : 分割集計時刻（騒音データ）
                                                          |                        Split aggregate time, noise data
                                                          | ``Lp_max``           : 分割集計期間内での騒音最大値
                                                          |                        Maximum noise value within the split tabulation time period
                                                          | ``Lp_max_time``      : Lp_max 値の取得時刻
                                                          |                        Time at which the Lp_max value was acquired
                                                          | ``Lp_min``           : 分割集計期間内での騒音最小値
                                                          |                        Minimum noise value within the split tabulation time period
                                                          | ``Lp_min_time``      : Lp_min 値の取得時刻
                                                          |                        Time at which the Lp_min value was acquired
                                                          | ``Lp_avg``           : 分割集計期間内での騒音平均値
                                                          |                        Average noise value within the split tabulation time period
                                                          | ``Lp_l5``            : 分割集計期間内での騒音L5値
                                                          |                        L5 noise value within the split tabulation time period
                                                          | ``Lp_l10``           : 分割集計期間内での騒音L10値
                                                          |                        L10 noise value within the split tabulation time period
                                                          | ``Lp_l50``           : 分割集計期間内での騒音L50値
                                                          |                        L50 noise value within the split tabulation time period
                                                          | ``Lp_l90``           : 分割集計期間内での騒音L90値
                                                          |                        L90 noise value within the split tabulation time period
                                                          | ``Lp_l95``           : 分割集計期間内での騒音L95値
                                                          |                        L95 noise value within the split tabulation time period
                                                          | ``Lv_max``           : 分割集計期間内での振動最大値
                                                          | - Returned values in case of **deviceType == "vibration1" OR "vibration4"**
                                                          | ``Lv_binned_time``   : 分割集計時刻（振動データ）
                                                          |                        Split aggregate time, vibration data
                                                          |                        Maximum vibration value within the split tabulation time period
                                                          | ``Lv_max_time``      : Lv_max 値の取得時刻
                                                          |                        Time at which the Lv_max value was acquired
                                                          | ``Lv_min``           : 分割集計期間内での振動最小値
                                                          |                        Minimum vibration value within the split tabulation time period
                                                          | ``Lv_min_time``      : Lv_min 値の取得時刻
                                                          |                        Time at which the Lv_min value was acquired
                                                          | ``Lv_avg``           : 分割集計期間内での振動平均値
                                                          |                        Average vibration value within the split tabulation time period
                                                          | ``Lv_l5``            : 分割集計期間内での振動L5値
                                                          |                        L5 vibration value within the split tabulation time period
                                                          | ``Lv_l10``           : 分割集計期間内での振動L10値
                                                          |                        L10 vibration value within the split tabulation time period
                                                          | ``Lv_l50``           : 分割集計期間内での振動L50値
                                                          |                        L50 vibration value within the split tabulation time period
                                                          | ``Lv_l90``           : 分割集計期間内での振動L90値
                                                          |                        L90 vibration value within the split tabulation time period
                                                          | ``Lv_l95``           : 分割集計期間内での振動L95値
                                                          |                        L95 vibration value within the split tabulation time period
                                                          | - Returned values in case of **deviceType == "weather1"**
                                                          | ``Wh_binned_time``   : 分割集計時刻（気象データ）
                                                          |                        Split aggregate time, weather data
                                                          | ``Wh_temp_max``      : 分割集計期間内での温度最大値
                                                          |                        Maximum temperature value within the split tabulation time period
                                                          | ``Wh_temp_max_time`` : Wh_temp_max 値の取得時刻
                                                          |                        Time at which the Wh_temp_max value was acquired
                                                          | ``Wh_temp_min``      : 分割集計期間内での温度最小値
                                                          |                        Minimum temperature value within the split tabulation time period
                                                          | ``Wh_temp_min_time`` : Wh_temp_min 値の取得時刻
                                                          |                        Time at which the Wh_temp_min value was acquired
                                                          | ``Wh_temp_avg``      : 分割集計期間内での温度平均値
                                                          |                        Average temperature value within the split tabulation time period
                                                          | ``Wh_hum_max``       : 分割集計期間内での湿度最大値
                                                          |                        Maximum humidity value within the split tabulation time period
                                                          | ``Wh_hum_max_time``  : Wh_hum_max 値の取得時刻
                                                          |                        Time at which the Wh_temp_max value was acquired
                                                          | ``Wh_hum_min``       : 分割集計期間内での湿度最小値
                                                          |                        Minimum humidity value within the split tabulation time period
                                                          | ``Wh_hum_min_time``  : Wh_hum_min 値の取得時刻
                                                          |                        Time at which the Wh_hum_min value was acquired
                                                          | ``Wh_hum_avg``       : 分割集計期間内での湿度平均値
                                                          |                        Average humidity value within the split tabulation time period
                                                          | ``Wh_ws_max``        : 分割集計期間内での風速最大値
                                                          |                        Maximum wind speed value within the split tabulation time period
                                                          | ``Wh_ws_max_time``   : Wh_ws_max 値の取得時刻
                                                          |                        Time at which the Wh_ws_max value was acquired
                                                          | ``Wh_ws_min``        : 分割集計期間内での風速最小値
                                                          |                        Minimum wind speed value within the split tabulation time period
                                                          | ``Wh_ws_min_time``   : Wh_ws_min 値の取得時刻
                                                          |                        Time at which the Wh_ws_min value was acquired
                                                          | ``Wh_ws_avg``        : 分割集計期間内での風速平均値
                                                          |                        Average wind speed value within the split tabulation time period
                                                          | ``Wh_gws_max``       : 分割集計期間内での瞬間風速最大値
                                                          |                        Maximum gust wind speed value within the split tabulation time period
                                                          | ``Wh_gws_max_time``  : Wh_gws_max 値の取得時刻
                                                          |                        Time at which the Wh_gws_max value was acquired
                                                          | ``Wh_gws_min``       : 分割集計期間内での瞬間風速最小値
                                                          |                        Minimum gust wind speed value within the split tabulation time period
                                                          | ``Wh_gws_min_time``  : Wh_gws_min 値の取得時刻
                                                          |                        Time at which the Wh_gws_min value was acquired
                                                          | ``Wh_gws_avg``       : 分割集計期間内での瞬間風速平均値
                                                          |                        Average gust wind speed value within the split tabulation time period
                                                          | ``Wh_arf_max``       : 分割集計期間内での積算雨量最大値
                                                          |                        Maximum accumulation rainfall value within the split tabulation time period
                                                          | ``Wh_arf_max_time``  : Wh_arf_max 値の取得時刻
                                                          |                        Time at which the Wh_arf_max value was acquired
                                                          | ``Wh_arf_min``       : 分割集計期間内での積算雨量最小値
                                                          |                        Minimum accumulation rainfall value within the split tabulation time period
                                                          | ``Wh_arf_min_time``  : Wh_gws_min 値の取得時刻
                                                          |                        Time at which the Wh_arf_min value was acquired
                                                          | ``Wh_arf_avg``       : 分割集計期間内での積算雨量平均値
                                                          |                        Average accumulation rainfall value within the split tabulation time period
                                                          | ``Wh_uv_max``        : 分割集計期間内での紫外線量最大値
                                                          |                        Maximum UV index value within the split tabulation time period
                                                          | ``Wh_uv_max_time``   : Wh_arf_max 値の取得時刻
                                                          |                        Time at which the Wh_uv_max value was acquired
                                                          | ``Wh_uv_min``        : 分割集計期間内での紫外線量最小値
                                                          |                        Minimum UV index value within the split tabulation time period
                                                          | ``Wh_uv_min_time``   : Wh_gws_min 値の取得時刻
                                                          |                        Time at which the Wh_uv_min value was acquired
                                                          | ``Wh_uv_avg``        : 分割集計期間内での紫外線量平均値
                                                          |                        Average UV index value within the split tabulation time period
                                                          | ``Wh_li_max``        : 分割集計期間内での光照度最大値
                                                          |                        Maximum light illuminance value within the split tabulation time period
                                                          | ``Wh_li_max_time``   : Wh_arf_max 値の取得時刻
                                                          |                        Time at which the Wh_li_max value was acquired
                                                          | ``Wh_li_min``        : 分割集計期間内での光照度最小値
                                                          |                        Minimum light illuminance value within the split tabulation time period
                                                          | ``Wh_li_min_time``   : Wh_li_min 値の取得時刻
                                                          |                        Time at which the Wh_li_min value was acquired
                                                          | ``Wh_li_avg``        : 分割集計期間内での光照度平均値
                                                          |                        Average light illuminance value within the split tabulation time period
    :>json string sql.queryStatement: | リクエスト実行のために生成した SQL クエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.
