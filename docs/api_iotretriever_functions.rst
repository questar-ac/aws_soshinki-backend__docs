.. _section-api-iotretriever-functions:


*IotRetrieverFunction Functions*

.. _section-api-iotretriever-functions-config:

config
======

.. _section-api-iotretriever-functions-config-get:

config.get
^^^^^^^^^^

.. http:put:: /

    | 本APIの構成設定を取得する
    | Obtains the configuration settings of this API

    **Request Example**

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: 1e783884????????????????????????????????????????????????????????

        {"type": "config.get", "operation": {"configValues": ["timeMinuteOffset", "timeOutputFormat", "timeBinIntervalDefault"]}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | リクエストのBodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text of the request
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

    :resheader Content-Type: application/json

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

    | **timeMinuteOffset** は IotRetrieverFunction のデプロイ先AWSリージョンによって以下の初期値に設定されます:
    | **timeMinuteOffset** is set to the following initial values depending on AWS region where IotRetrieverFunction is deployed:

    +----------------+--------------------------------+-------------------------------------------+------------+
    | Region         | timeMinuteOffset Default Value | City (Timezone Location)                  | UTC Offset |
    +================+================================+===========================================+============+
    | ap-northeast-1 | 540                            | Asia/Tokyo                                | UTC+9      |
    +----------------+--------------------------------+-------------------------------------------+------------+
    | ap-south-1     | 330                            | Mumbai (Asia/Kolkata)                     | UTC+5:30   |
    +----------------+--------------------------------+-------------------------------------------+------------+
    | ap-southeast-2 | 600                            | Australia/Sydney                          | UTC+10     |
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

    | 本APIの構成設定を変更する
    | Changes the configuration settings of this API

    **Request Example**

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: 674987b8????????????????????????????????????????????????????????

        {"type": "config.set", "operation": {"configValues": {"timeMinuteOffset": 600, "timeOutputFormat": "yyyy-MM-dd HH:mm:ss", "timeBinIntervalDefault": "30m"}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | リクエストのBodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text of the request
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"config.set"``
    :<json string operation.configValues.timeMinuteOffset: | ローカル時刻のUTC時間に対する分単位差分
                                                           | Minute offset of local time to UTC time
    :<json string operation.configValues.timeOutputFormat: | 日付時刻値の出力形式フォーマット
                                                           | Output format of timestamp
                                                           | (*) See `format_datetime function - Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html#:~:text=format_datetime%28timestamp,%20varchar%28x%29%29>`_
    :<json string operation.configValues.timeBinIntervalDefault: | `command.periodics`_ リクエストのパラメータ **Request JSON Object: operation.parameters.sql.timeBinInterval** に対するデフォルト設定値
                                                                 | Default value for **Request JSON Object: operation.parameters.sql.timeBinInterval** parameter of `command.periodics`_ request

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
    :>json string configValues.timeBinIntervalDefault: | `command.periodics`_ リクエストのパラメータ **Request JSON Object: operation.parameters.sql.timeBinInterval** に対するデフォルト設定値（変更後の値）
                                                       | Default value for **Request JSON Object: operation.parameters.sql.timeBinInterval** parameter of `command.periodics`_ request, after the request was performed

.. _section-api-iotretriever-functions-parameter:

parameter
=========

.. _section-api-iotretriever-functions-parameter-get:

parameter.get
^^^^^^^^^^^^^

.. http:put:: /

    | 計測データに対するデータベース・パラメータを取得する
    | Obtains database parameters for measurement data

    **Request Example**

    - | **operation.target.deviceId** を指定する場合
      | **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: 93ee6aba????????????????????????????????????????????????????????

        {"type": "parameter.get", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}}}

    - | **operation.target.deviceId** を指定しない場合
      | **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: 0a9b2433????????????????????????????????????????????????????????

        {"type": "parameter.get", "operation": {"target": {"deviceType": "noise1"}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | リクエストのBodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text of the request
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"parameter.get"``
    :>json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JSON Object: parameters.sql.wherePhrase** に全端末を対象とするSQLクエリ文 ``WHERE`` 句が返される 
                                             | Can be omitted. If omitted, ``WHERE`` phrase of SQL query statement to all terminals will be returned with **Response JSON Object: parameters.sql.wherePhrase**.
    :>json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_

    **Response Example**

    - | **target.deviceId** が存在する (**Request JSON Object: operation.target.deviceId** を指定した場合)
      | **target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

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

    - | **target.deviceId** が存在しない (**Request JSON Object: operation.target.deviceId** を指定しなかった場合)
      | **target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

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

    :resheader Content-Type: application/json

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
    :>json string parameters.sql.fromPhrase: | 対象計測データ格納されているTimestream DBに対するSQLクエリ文の ``FROM`` 句
                                             | ``FROM`` phrase of SQL query statement against target Timesteram DB
    :>json string parameters.sql.wherePhrase: | 対象計測データ格納されているTimestream DBに対するSQLクエリ文の ``WHERE`` 句
                                              | ``WHERE`` phrase of SQL query statement against target Timesteram DB

.. _section-api-iotretriever-functions-command:

command
=======

.. _section-api-iotretriever-functions-command-count:

command.count
^^^^^^^^^^^^^

.. http:put:: /

    | 計測データの個数を取得する
    | Obtain the number of the measured data

    **Request Example**

    - | **operation.target.deviceId** を指定する場合
      | **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: f72eed43????????????????????????????????????????????????????????

        {"type": "command.count", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "time BETWEEN ago(1d) AND now()", "queryStatement": false}, "requestValues": ["Lp_count"]}}}

    - | **operation.target.deviceId** を指定しない場合
      | **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: a8f3292b????????????????????????????????????????????????????????

        {"type": "command.count", "operation": {"target": {"deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "time BETWEEN ago(1d) AND now()", "queryStatement": false}, "requestValues": ["Lp_count"]}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | リクエストのBodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text of the request
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"command.count"``
    :<json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JOSN Object: responseValues** に全端末の要求された値が返される
                                             | Can be omitted. If omitted, the requested values to all terminals will be returned with **Response JOSN Object: responseValues**.
    :<json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_
    :<json string operation.parameters.sql.whereTimePhrase: | SQLクエリ文の ``WHERE`` 節に指定する時間指定記述
                                                            | Time phrase to ``WHERE`` clause of SQL query statement
                                                            | Examples: `whereTimePhrase <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#wheretimephrase>`_
                                                            | (*) See `Date / time operators - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-operators.html>`_ , `Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html>`_
    :<json boolean operation.parameters.sql.queryStatement: | 本リクエスト実行のために生成したSQLクエリ文文字列をレスボンスとして返すかどうかを指定する
                                                            | Whether a string of SQL query statement to execute this request should be returned or not
    :<json array<string> operation.parameters.requestValues[<ValueName>]: | **<ValueName>** -- 取得対象の値名文字列
                                                                          |                    Name string specifying required value
    .. list-table::
        :header-rows: 1
        :widths: 3, 1, 2

        * - Case
          - <ValueName>
          - Description
        * - **deviceType == "noise1"**, **"noise2"** or **"noise4"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "noise1"**, **"noise2"** or **"noise4"**
          - ``"Lp_count"``
          - 騒音計測データの個数
            
            Number of noise measure data
        * - **deviceType == "vibration1"** or **"vibration4"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "vibration1"** or **"vibration4"**
          - ``"Lv_count"``
          - 振動計測データの個数
            
            Number of vibration measure data
        * - **deviceType == "weather1"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "weather1"**
          - ``"Wh_count"``
          - 気象計測データの個数
            
            Number of weather measure data

    **Response Example**

    - | **target.deviceId** が存在する (**Request JSON Object: operation.target.deviceId** を指定した場合)
      | **target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

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

    - | **target.deviceId** が存在しない (**Request JSON Object: operation.target.deviceId** を指定しなかった場合)
      | **target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

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
              "Lp_count": 63800
            },
            {
              "device_id": "SK000020",
              "Lp_count": 63780
            },
            {
              "device_id": "SK000030",
              "Lp_count": 63960
            }
          ]
        }

    :resheader Content-Type: application/json

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
    :>json array<object> responseValues[<ValueObject>]: | **<ValueObjects>** -- 戻り値を含むオブジェクト
                                                        |                       Object containing returned value
    .. list-table::
        :header-rows: 1
        :widths: 3, 1, 2

        * - Case
          - Item of <ValueObjects>
          - Description
        * - **Request JSON Object: operation.target.deviceId** が省略された場合の戻り値
            
            Returned value if **Request JSON Object: operation.target.deviceId** was omitted
          - ``device_id``
          - 端末ID
            
            Terminal ID
        * - **deviceType == "noise1"**, **"noise2"** or **"noise4"** の場合の戻り値
            
            Returned value in case of **deviceType == "noise1"**, **"noise2"** or **"noise4"**
          - ``Lp_count``
          - 指定期間内の騒音計測データの個数
            
            Number of noise measure data within the time period
        * - **deviceType == "vibration1"** or **"vibration4"** の場合の戻り値
            
            Returned value in case of **deviceType == "vibration1"** or **"vibration4"**
          - ``Lv_count``
          - 指定期間内の振動計測データの個数
            
            Number of vibration measure data within the time period
        * - **deviceType == "weather1"** の場合の戻り値
            
            Returned value in case of **deviceType == "weather1"**
          - ``Wh_count``
          - 指定期間内の気象計測データの個数
            
            Number of weather measure data within the time period
    :>json string sql.queryStatement: | リクエスト実行のために生成したSQLクエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.

.. _section-api-iotretriever-functions-command-aggregate:

command.aggregate
^^^^^^^^^^^^^^^^^

.. http:put:: /
    :synopsis: command.aggregate

    | 単一期間の計測データの集計値を取得する
    | Obtains the aggregate values of the measured data for a single period of time

    **Request Example**

    - | **toperation.target.deviceId** を指定する場合
      | **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: b1a6f011????????????????????????????????????????????????????????

        {"type": "command.aggregate", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-06-18'", "queryStatement": false}, "requestValues": ["Lp_max", "Lp_max_time", "Lp_min", "Lp_min_time", "Lp_avg", "Lp_l5", "Lp_l10", "Lp_l50", "Lp_l90", "Lp_l95", "Lp_count"]}}}

    - | **operation.target.deviceId** を指定しない場合
      | **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: 9a35d1b4????????????????????????????????????????????????????????

        {"type": "command.aggregate", "operation": {"target": {"deviceType": "noise1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-06-18'", "queryStatement": true}, "requestValues": ["Lp_max", "Lp_max_time", "Lp_min", "Lp_min_time", "Lp_avg", "Lp_l5", "Lp_l10", "Lp_l50", "Lp_l90", "Lp_l95", "Lp_count"]}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | リクエストのBodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text of the request
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"command.aggregate"``
    :<json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JOSN Object: responseValues** に全端末の要求された値が返される
                                             | Can be omitted. If omitted, the requested values to all terminals will be returned with **Response JOSN Object: responseValues**.
    :<json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_
    :<json string operation.parameters.sql.whereTimePhrase: | SQLクエリ文の ``WHERE`` 節に指定する時間指定記述
                                                            | Time phrase to ``WHERE`` clause of SQL query statement
                                                            | Examples: `whereTimePhrase <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#wheretimephrase>`_
                                                            | (*) See `Date / time operators - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-operators.html>`_ , `Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html>`_
    :<json boolean operation.parameters.sql.queryStatement: | 本リクエスト実行のために生成したSQLクエリ文文字列をレスボンスとして返すかどうかを指定する
                                                            | Whether a string of SQL query statement to execute this request should be returned or not
    :<json array<string> operation.parameters.requestValues[<ValueNames>]: | **<ValueNames>** -- 取得対象の値名文字列
                                                                           |                     Name strings specifying required values
    .. list-table::
        :header-rows: 1
        :widths: 3, 1, 2

        * - Case
          - Item of <ValueNames>
          - Description
        * - **deviceType == "noise1"**, **"noise2"** or **"noise4"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "noise1"**, **"noise2"** or **"noise4"**
          - ``"Lp_max"``
          - 集計期間内での騒音最大値
            
            Maximum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_max_time"``
          - Lp_max 値の取得時刻
            
            Time at which the Lp_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Lp_min"``
          - 集計期間内での騒音最小値
            
            Minimum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_min_time"``
          - Lp_min 値の取得時刻
            
            Time at which the Lp_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Lp_avg"``
          - 集計期間内での騒音平均値
            
            Average noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l5"``
          - 集計期間内での騒音L5値
            
            L5 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l10"``
          - 集計期間内での騒音L10値
            
            L10 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l50"``
          - 集計期間内での騒音L50値
            
            L50 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l90"``
          - 集計期間内での騒音L90値
            
            L90 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l95"``
          - 集計期間内での騒音L95値
            
            L95 noise value within the aggregate time period
        * - **deviceType == "vibration1"** or **"vibration4"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "vibration1"** or **"vibration4"**
          - ``"Lv_max"``
          - 集計期間内での振動最大値
            
            Maximum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_max_time"``
          - Lv_max 値の取得時刻
            
            Time at which the Lv_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Lv_min"``
          - 集計期間内での振動最小値
            
            Minimum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_min_time"``
          - Lv_min 値の取得時刻
            
            Time at which the Lv_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Lv_avg"``
          - 集計期間内での振動平均値
            
            Average vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l5"``
          - 集計期間内での振動L5値
            
            L5 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l10"``
          - 集計期間内での振動L10値
            
            L10 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l50"``
          - 集計期間内での振動L50値
            
            L50 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l90"``
          - 集計期間内での振動L90値
            
            L90 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l95"``
          - 集計期間内での振動L95値
            
            L95 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_count"``
          - 集計期間内の振動計測データの個数
            
            Number of vibration measure data within the aggregate time period
        * - **deviceType == "weather1"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "weather1"**
          - ``"Wh_temp_max"``
          - 集計期間内での温度最大値
            
            Maximum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_temp_max_time"``
          - Wh_temp_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_temp_min"``
          - 集計期間内での温度最小値
            
            Minimum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_temp_min_time"``
          - Wh_temp_min 値の取得時刻
            
            Time at which the Wh_temp_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_temp_avg"``
          - 集計期間内での温度平均値
            
            Average temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_hum_max"``
          - 集計期間内での湿度最大値
            
            Maximum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_hum_max_time"``
          - Wh_hum_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_hum_min"``
          - 集計期間内での湿度最小値
            
            Minimum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_hum_min_time"``
          - Wh_hum_min 値の取得時刻
            
            Time at which the Wh_hum_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_hum_avg"``
          - 集計期間内での湿度平均値
            
            Average humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_ws_max"``
          - 集計期間内での風速最大値
            
            Maximum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_ws_max_time"``
          - Wh_ws_max 値の取得時刻
            
            Time at which the Wh_ws_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_ws_min"``
          - 集計期間内での風速最小値
            
            Minimum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_ws_min_time"``
          - Wh_ws_min 値の取得時刻
            
            Time at which the Wh_ws_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_ws_avg"``
          - 集計期間内での風速平均値
            
            Average wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_gws_max"``
          - 集計期間内での瞬間風速最大値
            
            Maximum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_gws_max_time"``
          - Wh_gws_max 値の取得時刻
            
            Time at which the Wh_gws_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_gws_min"``
          - 集計期間内での瞬間風速最小値
            
            Minimum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_gws_min_time"``
          - Wh_gws_min 値の取得時刻
            
            Time at which the Wh_gws_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_gws_avg"``
          - 集計期間内での瞬間風速平均値
            
            Average gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_arf_max"``
          - 集計期間内での積算雨量最大値
            
            Maximum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_arf_max_time"``
          - Wh_arf_max 値の取得時刻
            
            Time at which the Wh_arf_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_arf_min"``
          - 集計期間内での積算雨量最小値
            
            Minimum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_arf_min_time"``
          - Wh_arf_min 値の取得時刻
            
            Time at which the Wh_arf_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_arf_avg"``
          - 集計期間内での積算雨量平均値
            
            Average accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_uv_max"``
          - 集計期間内での紫外線量最大値
            
            Maximum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_uv_max_time"``
          - Wh_uv_max 値の取得時刻
            
            Time at which the Wh_uv_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_uv_min"``
          - 集計期間内での紫外線量最小値
            
            Minimum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_uv_min_time"``
          - Wh_uv_min 値の取得時刻
            
            Time at which the Wh_uv_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_uv_avg"``
          - 集計期間内での紫外線量平均値
            
            Average UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_li_max"``
          - 集計期間内での光照度最大値
            
            Maximum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_li_max_time"``
          - Wh_li_max 値の取得時刻
            
            Time at which the Wh_li_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_li_min"``
          - 集計期間内での光照度最小値
            
            Minimum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_li_min_time"``
          - Wh_li_min 値の取得時刻
            
            Time at which the Wh_li_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_li_avg"``
          - 集計期間内での光照度平均値
            
            Average light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_count"``
          - 集計期間内の気象計測データの個数
            
            Number of weather measure data within the aggregate time period

    **Response Example**

    - | **target.deviceId** が存在する (**Request JSON Object: operation.target.deviceId** を指定した場合)
      | **target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

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
              "Lp_max": 82,
              "Lp_max_time": "2025-06-18 16:51:53.862",
              "Lp_min": 46.4,
              "Lp_min_time": "2025-06-18 07:37:02.387",
              "Lp_avg": 55.3,
              "Lp_l5": 69.4,
              "Lp_l10": 66.1,
              "Lp_l50": 54.4,
              "Lp_l90": 49.1,
              "Lp_l95": 48.8,
              "Lp_count": 63800
            }
          ]
        }

    - | **target.deviceId** が存在しない (**Request JSON Object: operation.target.deviceId** を指定しなかった場合)
      | **target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

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
              "Lp_max": 82,
              "Lp_max_time": "2025-06-18 16:51:53.862",
              "Lp_min": 46.4,
              "Lp_min_time": "2025-06-18 07:37:02.387",
              "Lp_avg": 55.3,
              "Lp_l5": 69.4,
              "Lp_l10": 66.1,
              "Lp_l50": 54.4,
              "Lp_l90": 49.1,
              "Lp_l95": 48.8,
              "Lp_count": 13800
            },
            {
              "device_id": "SK000020",
              "Lp_max": 86.9,
              "Lp_max_time": "2025-06-18 09:12:01.321",
              "Lp_min": 48.3,
              "Lp_min_time": "2025-06-18 09:13:24.458",
              "Lp_avg": 54.9,
              "Lp_l5": 69.1,
              "Lp_l10": 66.2,
              "Lp_l50": 53.6,
              "Lp_l90": 49.3,
              "Lp_l95": 49.2,
              "Lp_count": 13780
            },
            {
              "device_id": "SK000030",
              "Lp_max": 82.7,
              "Lp_max_time": "2025-06-18 09:47:55.929",
              "Lp_min": 48.7,
              "Lp_min_time": "2025-06-18 09:40:51.166",
              "Lp_avg": 55.1,
              "Lp_l5": 67.9,
              "Lp_l10": 64.7,
              "Lp_l50": 53.6,
              "Lp_l90": 49.8,
              "Lp_l95": 49.5,
              "Lp_count": 13960
            }
          ],
          "sql": {
            "queryStatement": "SELECT device_id, max(Lp) AS Lp_max  ,format_datetime(date_add('minute', 540, max_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS Lp_max_time  ,min(Lp) AS Lp_min  ,format_datetime(date_add('minute', 540, min_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS Lp_min_time  ,round(avg(Lp), 1) AS Lp_avg  ,approx_percentile(Lp, 0.95) AS Lp_l5  ,approx_percentile(Lp, 0.90) AS Lp_l10  ,approx_percentile(Lp, 0.50) AS Lp_l50  ,approx_percentile(Lp, 0.10) AS Lp_l90  ,approx_percentile(Lp, 0.05) AS Lp_l95  ,count(Lp) AS Lp_count FROM soshinki_noise1_20250502.noise1_chunk WHERE date(date_add('minute', 540, time)) = '2025-06-18' AND measure_name = 'multi' GROUP BY device_id ORDER BY device_id ASC"
          }
        }

    :resheader Content-Type: application/json

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accept :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the command.aggregate request

    :>json string target.deviceId: | 端末ID（ **Request JSON Object: operation.target.deviceId** で指定した値と同一 ）
                                   | Terminal ID（ Same value as the one specified by **Request JSON Object: operation.target.deviceId** ）
                                   | **Request JSON Object: operation.target.deviceId** を省略した場合は、本項目は存在しない
                                   | This item is not present if **Request JSON Object: operation.target.deviceId** was omitted.
    :>json string target.deviceType: | Device Type（ **Request JSON Object: operation.target.deviceType** で指定した値と同一 ）
                                     | Device Type（ Same value as the one specified by **Request JSON Object: operation.target.deviceType** ）
    :>json array<object> responseValues[<ValuesObjects>]: | **<ValueObjects>** -- 戻り値を含むオブジェクト
                                                          |                        Object containing returned values
    .. list-table::
        :header-rows: 1
        :widths: 3, 1, 2

        * - Case
          - Item of <ValueObjects>
          - Description
        * - **Request JSON Object: operation.target.deviceId** が省略された場合の戻り値
            
            Returned value if **Request JSON Object: operation.target.deviceId** was omitted
          - ``device_id``
          - 端末ID
            
            Terminal ID
        * - **deviceType == "noise1"**, **"noise2"** or **"noise4"** の場合の戻り値
            
            Returned value in case of **deviceType == "noise1"**, **"noise2"** or **"noise4"**
          - ``Lp_max``
          - 集計期間内での騒音最大値
            
            Maximum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_max_time``
          - Lp_max 値の取得時刻
            
            Time at which the Lp_max value was acquired
        * - 同上
            
            Same as the above
          - ``Lp_min``
          - 集計期間内での騒音最小値
            
            Minimum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_min_time``
          - Lp_min 値の取得時刻
            
            Time at which the Lp_min value was acquired
        * - 同上
            
            Same as the above
          - ``Lp_avg``
          - 集計期間内での騒音平均値
            
            Average noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l5``
          - 集計期間内での騒音L5値
            
            L5 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l10``
          - 集計期間内での騒音L10値
            
            L10 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l50``
          - 集計期間内での騒音L50値
            
            L50 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l90``
          - 集計期間内での騒音L90値
            
            L90 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l95``
          - 集計期間内での騒音L95値
            
            L95 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_count``
          - 集計期間内の騒音計測データの個数
            
            Number of noise measure data within the aggregate time period
        * - **deviceType == "vibration1"** or **"vibration4"** の場合の戻り値
            
            Returned value in case of **deviceType == "vibration1"** or **"vibration4"**
          - ``Lv_max``
          - 集計期間内での振動最大値
            
            Maximum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_max_time``
          - Lv_max 値の取得時刻
            
            Time at which the Lv_max value was acquired
        * - 同上
            
            Same as the above
          - ``Lv_min``
          - 集計期間内での振動最小値
            
            Minimum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_min_time``
          - Lv_min 値の取得時刻
            
            Time at which the Lv_min value was acquired
        * - 同上
            
            Same as the above
          - ``Lv_avg``
          - 集計期間内での振動平均値
            
            Average vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l5``
          - 集計期間内での振動L5値
            
            L5 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l10``
          - 集計期間内での振動L10値
            
            L10 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l50``
          - 集計期間内での振動L50値
            
            L50 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l90``
          - 集計期間内での振動L90値
            
            L90 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l95``
          - 集計期間内での振動L95値
            
            L95 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_count``
          - 集計期間内の振動計測データの個数
            
            Number of vibration measure data within the aggregate time period
        * - **deviceType == "weather1"** の場合の戻り値
            
            Returned value in case of **deviceType == "weather1"**
          - ``Wh_temp_max``
          - 集計期間内での温度最大値
            
            Maximum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_temp_max_time``
          - Wh_temp_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_temp_min``
          - 集計期間内での温度最小値
            
            Minimum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_temp_min_time``
          - Wh_temp_min 値の取得時刻
            
            Time at which the Wh_temp_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_temp_avg``
          - 集計期間内での温度平均値
            
            Average temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_hum_max``
          - 集計期間内での湿度最大値
            
            Maximum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_hum_max_time``
          - Wh_hum_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_hum_min``
          - 集計期間内での湿度最小値
            
            Minimum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_hum_min_time``
          - Wh_hum_min 値の取得時刻
            
            Time at which the Wh_hum_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_hum_avg``
          - 集計期間内での湿度平均値
            
            Average humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_ws_max``
          - 集計期間内での風速最大値
            
            Maximum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_ws_max_time``
          - Wh_ws_max 値の取得時刻
            
            Time at which the Wh_ws_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_ws_min``
          - 集計期間内での風速最小値
            
            Minimum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_ws_min_time``
          - Wh_ws_min 値の取得時刻
            
            Time at which the Wh_ws_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_ws_avg``
          - 集計期間内での風速平均値
            
            Average wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_gws_max``
          - 集計期間内での瞬間風速最大値
            
            Maximum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_gws_max_time``
          - Wh_gws_max 値の取得時刻
            
            Time at which the Wh_gws_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_gws_min``
          - 集計期間内での瞬間風速最小値
            
            Minimum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_gws_min_time``
          - Wh_gws_min 値の取得時刻
            
            Time at which the Wh_gws_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_gws_avg``
          - 集計期間内での瞬間風速平均値
            
            Average gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_arf_max``
          - 集計期間内での積算雨量最大値
            
            Maximum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_arf_max_time``
          - Wh_arf_max 値の取得時刻
            
            Time at which the Wh_arf_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_arf_min``
          - 集計期間内での積算雨量最小値
            
            Minimum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_arf_min_time``
          - Wh_gws_min 値の取得時刻
            
            Time at which the Wh_arf_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_arf_avg``
          - 集計期間内での積算雨量平均値
            
            Average accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_uv_max``
          - 集計期間内での紫外線量最大値
            
            Maximum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_uv_max_time``
          - Wh_uv_max 値の取得時刻
            
            Time at which the Wh_uv_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_uv_min``
          - 集計期間内での紫外線量最小値
            
            Minimum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_uv_min_time``
          - Wh_uv_min 値の取得時刻
            
            Time at which the Wh_uv_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_uv_avg``
          - 集計期間内での紫外線量平均値
            
            Average UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_li_max``
          - 集計期間内での光照度最大値
            
            Maximum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_li_max_time``
          - Wh_li_max 値の取得時刻
            
            Time at which the Wh_li_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_li_min``
          - 集計期間内での光照度最小値
            
            Minimum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_li_min_time``
          - Wh_li_min 値の取得時刻
            
            Time at which the Wh_li_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_li_avg``
          - 集計期間内での光照度平均値
            
            Average light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_count``
          - 集計期間内の気象計測データの個数
            
            Number of weather measure data within the aggregate time period
    :>json string sql.queryStatement: | リクエスト実行のために生成したSQLクエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.

.. _section-api-iotretriever-functions-command-periodics:

command.periodics
^^^^^^^^^^^^^^^^^

.. http:put:: /

    | 周期分割期間の計測データの集計値を取得する
    | Obtains the aggregate values of the measured data for the periodic division period

    **Request Example**

    - | **operation.target.deviceId** を指定する場合
      | **operation.target.deviceId** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: a6fbdf38????????????????????????????????????????????????????????

        {"type": "command.periodics", "operation": {"target": {"deviceId": "SK000010", "deviceType": "vibration1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-06-18'", "timeBinInterval": "1h", "queryStatement": false}, "requestValues": ["Lv_binned_time", "Lv_max", "Lv_max_time", "Lv_min", "Lv_min_time", "Lv_avg", "Lv_l5", "Lv_l10", "Lv_l50", "Lv_l90", "Lv_l95", "Lv_count"]}}}

    - | **operation.target.deviceId** を指定しない場合
      | **operation.target.deviceId** is not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: 6026a1aa????????????????????????????????????????????????????????

        {"type": "command.periodics", "operation": {"target": {"deviceType": "vibration1"}, "parameters": {"sql": {"whereTimePhrase": "date(time) = '2025-06-18'", "timeBinInterval": "1h", "queryStatement": true}, "requestValues": ["Lv_binned_time", "Lv_max", "Lv_max_time", "Lv_min", "Lv_min_time", "Lv_avg", "Lv_l5", "Lv_l10", "Lv_l50", "Lv_l90", "Lv_l95", "Lv_count"]}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | リクエストのBodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text of the request
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"command.periodics"``
    :<json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | 本項目は省略可能、省略された場合は **Response JOSN Object: responseValues** に全端末の要求された値が返される
                                             | Can be omitted. If omitted, the requested values to all terminals will be returned with **Response JOSN Object: responseValues**.
    :<json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_
    :<json string operation.parameters.sql.whereTimePhrase: | SQLクエリ文の ``WHERE`` 節に指定する時間指定記述
                                                            | Time phrase to ``WHERE`` clause of SQL query statement
                                                            | Examples: `whereTimePhrase <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#wheretimephrase>`_
                                                            | (*) See `Date / time operators - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-operators.html>`_ , `Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html>`_
    :<json string operation.parameters.sql.timeBinInterval: | SQL文 ``BIN`` 関数の引数 ``interval`` として指定する時間間隔指定記述
                                                            | Time phrase as ``interval`` argument to ``BIN`` function of SQL statement
                                                            | 本項目は省略可能、省略された場合は **configValues.timeBinIntervalDefault** の設定値が使われる
                                                            | Can be omitted. If omitted, the value as **configValues.timeBinIntervalDefault** setting will be used.
                                                            | Examples: `timeBinInterval <https://aws-soshinki-backend.readthedocs.io/ja/latest/api_iotretriever_parameters.html#timebininterval>`_
                                                            | (*) See `bin function - Date / time functions - Amazon Timestream <https://docs.aws.amazon.com/timestream/latest/developerguide/date-time-functions.html#:~:text=bin%28timestamp%2C%20interval%29>`_
    :<json boolean operation.parameters.sql.queryStatement: | 本リクエスト実行のために生成したSQLクエリ文文字列をレスボンスとして返すかどうかを指定する
                                                            | Whether a string of SQL query statement to execute this request should be returned or not
    :<json array<string> operation.parameters.requestValues[<ValueNames>]: | **<ValueNames>** -- 取得対象の値名文字列
                                                                           |                     Name strings specifying required values
    .. list-table::
        :header-rows: 1
        :widths: 3, 1, 2

        * - Case
          - Item of <ValueNames>
          - Description
        * - **deviceType == "noise1"**, or **"noise2"** or **"noise4"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "noise1"**, or **"noise2"** or **"noise4"**
          - ``"Lp_binned_time"``
          - 分割集計時刻（騒音計測データ）
            
            Split aggregate time, noise measured data
        * - 同上
            
            Same as the above
          - ``"Lp_max"``
          - 分割集計期間内での騒音最大値
            
            Maximum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_max_time"``
          - Lp_max 値の取得時刻
            
            Time at which the Lp_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Lp_min"``
          - 分割集計期間内での騒音最小値
            
            Minimum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_min_time"``
          - Lp_min 値の取得時刻
            
            Time at which the Lp_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Lp_avg"``
          - 分割集計期間内での騒音平均値
            
            Average noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l5"``
          - 分割集計期間内での騒音L5値
            
            L5 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l10"``
          - 分割集計期間内での騒音L10値
            
            L10 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l50"``
          - 分割集計期間内での騒音L50値
            
            L50 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l90"``
          - 分割集計期間内での騒音L90値
            
            L90 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lp_l95"``
          - 分割集計期間内での騒音L95値
            
            L95 noise value within the aggregate time period
        * - **deviceType == "vibration1"** or **"vibration4"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "vibration1"** or **"vibration4"**
          - ``"Lv_binned_time"``
          - 分割集計時刻（振動計測データ）
            
            Split aggregate time, vibration measured data
        * - 同上
            
            Same as the above
          - ``"Lv_max"``
          - 分割集計期間内での振動最大値
            
            Maximum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_max_time"``
          - Lv_max 値の取得時刻
            
            Time at which the Lv_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Lv_min"``
          - 分割集計期間内での振動最小値
            
            Minimum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_min_time"``
          - Lv_min 値の取得時刻
            
            Time at which the Lv_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Lv_avg"``
          - 分割集計期間内での振動平均値
            
            Average vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l5"``
          - 分割集計期間内での振動L5値
            
            L5 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l10"``
          - 分割集計期間内での振動L10値
            
            L10 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l50"``
          - 分割集計期間内での振動L50値
            
            L50 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l90"``
          - 分割集計期間内での振動L90値
            
            L90 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_l95"``
          - 分割集計期間内での振動L95値
            
            L95 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Lv_count"``
          - 分割集計期間内の振動計測データの個数
            
            Number of vibration measure data within the aggregate time period
        * - **deviceType == "weather1"** の場合のみ指定可能な値
            
            Specifiable values in case of **deviceType == "weather1"**
          - ``"Wh_binned_time"``
          - 分割集計時刻（気象計測データ）
            
            Split aggregate time, weather measured data
        * - 同上
            
            Same as the above
          - ``"Wh_temp_max"``
          - 分割集計期間内での温度最大値
            
            Maximum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_temp_max_time"``
          - Wh_temp_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_temp_min"``
          - 分割集計期間内での温度最小値
            
            Minimum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_temp_min_time"``
          - Wh_temp_min 値の取得時刻
            
            Time at which the Wh_temp_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_temp_avg"``
          - 分割集計期間内での温度平均値
            
            Average temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_hum_max"``
          - 分割集計期間内での湿度最大値
            
            Maximum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_hum_max_time"``
          - Wh_hum_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_hum_min"``
          - 分割集計期間内での湿度最小値
            
            Minimum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_hum_min_time"``
          - Wh_hum_min 値の取得時刻
            
            Time at which the Wh_hum_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_hum_avg"``
          - 分割集計期間内での湿度平均値
            
            Average humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_ws_max"``
          - 分割集計期間内での風速最大値
            
            Maximum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_ws_max_time"``
          - Wh_ws_max 値の取得時刻
            
            Time at which the Wh_ws_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_ws_min"``
          - 分割集計期間内での風速最小値
            
            Minimum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_ws_min_time"``
          - Wh_ws_min 値の取得時刻
            
            Time at which the Wh_ws_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_ws_avg"``
          - 分割集計期間内での風速平均値
            
            Average wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_gws_max"``
          - 分割集計期間内での瞬間風速最大値
            
            Maximum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_gws_max_time"``
          - Wh_gws_max 値の取得時刻
            
            Time at which the Wh_gws_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_gws_min"``
          - 分割集計期間内での瞬間風速最小値
            
            Minimum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_gws_min_time"``
          - Wh_gws_min 値の取得時刻
            
            Time at which the Wh_gws_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_gws_avg"``
          - 分割集計期間内での瞬間風速平均値
            
            Average gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_arf_max"``
          - 分割集計期間内での積算雨量最大値
            
            Maximum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_arf_max_time"``
          - Wh_arf_max 値の取得時刻
            
            Time at which the Wh_arf_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_arf_min"``
          - 分割集計期間内での積算雨量最小値
            
            Minimum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_arf_min_time"``
          - Wh_arf_min 値の取得時刻
            
            Time at which the Wh_arf_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_arf_avg"``
          - 分割集計期間内での積算雨量平均値
            
            Average accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_uv_max"``
          - 分割集計期間内での紫外線量最大値
            
            Maximum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_uv_max_time"``
          - Wh_uv_max 値の取得時刻
            
            Time at which the Wh_uv_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_uv_min"``
          - 分割集計期間内での紫外線量最小値
            
            Minimum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_uv_min_time"``
          - Wh_uv_min 値の取得時刻
            
            Time at which the Wh_uv_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_uv_avg"``
          - 分割集計期間内での紫外線量平均値
            
            Average UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_li_max"``
          - 分割集計期間内での光照度最大値
            
            Maximum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_li_max_time"``
          - Wh_li_max 値の取得時刻
            
            Time at which the Wh_li_max value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_li_min"``
          - 分割集計期間内での光照度最小値
            
            Minimum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_li_min_time"``
          - Wh_li_min 値の取得時刻
            
            Time at which the Wh_li_min value was acquired
        * - 同上
            
            Same as the above
          - ``"Wh_li_avg"``
          - 分割集計期間内での光照度平均値
            
            Average light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``"Wh_count"``
          - 分割集計期間内の気象計測データの個数
            
            Number of weather measure data within the aggregate time period

    **Response Example**

    - | **target.deviceId** が存在する (**Request JSON Object: operation.target.deviceId** を指定した場合)
      | **target.deviceId** is present (**Request JSON Object: operation.target.deviceId** was present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceId": "SK000010",
            "deviceType": "vibration1"
          },
          "responseValues": [
            {
              "Lv_binned_time": "2025-06-18 08:00:00.000",
              "Lv_max": 54.7,
              "Lv_max_time": "2025-06-18 08:18:16.804",
              "Lv_min": 25.8,
              "Lv_min_time": "2025-06-18 08:15:35.458",
              "Lv_avg": 34,
              "Lv_l5": 44.6,
              "Lv_l10": 42.1,
              "Lv_l50": 32,
              "Lv_l90": 28.7,
              "Lv_l95": 28.1,
              "Lv_count": 1909
            },
            {
              "Lv_binned_time": "2025-06-18 09:00:00.000",
              "Lv_max": 61.9,
              "Lv_max_time": "2025-06-18 09:15:38.315",
              "Lv_min": 24.4,
              "Lv_min_time": "2025-06-18 09:31:05.172",
              "Lv_avg": 33.1,
              "Lv_l5": 43.9,
              "Lv_l10": 40.7,
              "Lv_l50": 31.3,
              "Lv_l90": 28.4,
              "Lv_l95": 27.7,
              "Lv_count": 1925
            },
            {
              "Lv_binned_time": "2025-06-18 10:00:00.000",
              "Lv_max": 52.7,
              "Lv_max_time": "2025-06-18 10:33:28.380",
              "Lv_min": 23.6,
              "Lv_min_time": "2025-06-18 10:53:05.579",
              "Lv_avg": 30.2,
              "Lv_l5": 37.3,
              "Lv_l10": 33.4,
              "Lv_l50": 29.5,
              "Lv_l90": 27.2,
              "Lv_l95": 26.6,
              "Lv_count": 1704
            },
            {
              "Lv_binned_time": "2025-06-18 11:00:00.000",
              "Lv_max": 49.1,
              "Lv_max_time": "2025-06-18 11:20:54.921",
              "Lv_min": 24.1,
              "Lv_min_time": "2025-06-18 11:06:20.124",
              "Lv_avg": 29.1,
              "Lv_l5": 32.4,
              "Lv_l10": 31.2,
              "Lv_l50": 28.7,
              "Lv_l90": 26.8,
              "Lv_l95": 26.2,
              "Lv_count": 1796
            },
            {
              "Lv_binned_time": "2025-06-18 12:00:00.000",
              "Lv_max": 78.1,
              "Lv_max_time": "2025-06-18 12:48:58.971",
              "Lv_min": 23.6,
              "Lv_min_time": "2025-06-18 12:42:51.267",
              "Lv_avg": 30.7,
              "Lv_l5": 36.4,
              "Lv_l10": 33.6,
              "Lv_l50": 29.9,
              "Lv_l90": 27.6,
              "Lv_l95": 27,
              "Lv_count": 1666
            },
            {
              "Lv_binned_time": "2025-06-18 13:00:00.000",
              "Lv_max": 78.7,
              "Lv_max_time": "2025-06-18 13:39:53.151",
              "Lv_min": 25.1,
              "Lv_min_time": "2025-06-18 13:57:57.141",
              "Lv_avg": 30.6,
              "Lv_l5": 34.5,
              "Lv_l10": 33,
              "Lv_l50": 30,
              "Lv_l90": 27.9,
              "Lv_l95": 27.4,
              "Lv_count": 1639
            },
            {
              "Lv_binned_time": "2025-06-18 14:00:00.000",
              "Lv_max": 79.2,
              "Lv_max_time": "2025-06-18 14:19:48.669",
              "Lv_min": 25.2,
              "Lv_min_time": "2025-06-18 14:12:45.858",
              "Lv_avg": 31.7,
              "Lv_l5": 38.5,
              "Lv_l10": 35.3,
              "Lv_l50": 30.7,
              "Lv_l90": 28.5,
              "Lv_l95": 27.9,
              "Lv_count": 1831
            },
            {
              "Lv_binned_time": "2025-06-18 15:00:00.000",
              "Lv_max": 84.9,
              "Lv_max_time": "2025-06-18 15:23:51.436",
              "Lv_min": 25.4,
              "Lv_min_time": "2025-06-18 15:24:57.557",
              "Lv_avg": 31.5,
              "Lv_l5": 35.4,
              "Lv_l10": 33.6,
              "Lv_l50": 31,
              "Lv_l90": 28.7,
              "Lv_l95": 27.9,
              "Lv_count": 1838
            },
            {
              "Lv_binned_time": "2025-06-18 16:00:00.000",
              "Lv_max": 78.7,
              "Lv_max_time": "2025-06-18 16:36:10.862",
              "Lv_min": 25.3,
              "Lv_min_time": "2025-06-18 16:49:03.261",
              "Lv_avg": 30.7,
              "Lv_l5": 34,
              "Lv_l10": 32.8,
              "Lv_l50": 30.2,
              "Lv_l90": 28.1,
              "Lv_l95": 27.5,
              "Lv_count": 1621
            },
            {
              "Lv_binned_time": "2025-06-18 17:00:00.000",
              "Lv_max": 81.9,
              "Lv_max_time": "2025-06-18 17:25:32.290",
              "Lv_min": 25.7,
              "Lv_min_time": "2025-06-18 17:15:55.245",
              "Lv_avg": 30.9,
              "Lv_l5": 34.8,
              "Lv_l10": 33,
              "Lv_l50": 30.2,
              "Lv_l90": 28.2,
              "Lv_l95": 27.6,
              "Lv_count": 1859
            }
          ]
        }

    - | **target.deviceId** が存在しない (**Request JSON Object: operation.target.deviceId** を指定しなかった場合)
      | **target.deviceId** is not present (**Request JSON Object: operation.target.deviceId** was not present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "target": {
            "deviceType": "vibration1"
          },
          "responseValues": [
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 08:00:00.000",
              "Lv_max": 54.7,
              "Lv_max_time": "2025-06-18 08:18:16.804",
              "Lv_min": 25.8,
              "Lv_min_time": "2025-06-18 08:15:35.458",
              "Lv_avg": 34,
              "Lv_l5": 44.6,
              "Lv_l10": 42.1,
              "Lv_l50": 32,
              "Lv_l90": 28.7,
              "Lv_l95": 28.1,
              "Lv_count": 1909
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 09:00:00.000",
              "Lv_max": 61.9,
              "Lv_max_time": "2025-06-18 09:15:38.315",
              "Lv_min": 24.4,
              "Lv_min_time": "2025-06-18 09:31:05.172",
              "Lv_avg": 33.1,
              "Lv_l5": 43.9,
              "Lv_l10": 40.7,
              "Lv_l50": 31.3,
              "Lv_l90": 28.4,
              "Lv_l95": 27.7,
              "Lv_count": 1925
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 10:00:00.000",
              "Lv_max": 52.7,
              "Lv_max_time": "2025-06-18 10:33:28.380",
              "Lv_min": 23.6,
              "Lv_min_time": "2025-06-18 10:53:05.579",
              "Lv_avg": 30.2,
              "Lv_l5": 37.3,
              "Lv_l10": 33.4,
              "Lv_l50": 29.5,
              "Lv_l90": 27.2,
              "Lv_l95": 26.6,
              "Lv_count": 1704
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 11:00:00.000",
              "Lv_max": 49.1,
              "Lv_max_time": "2025-06-18 11:20:54.921",
              "Lv_min": 24.1,
              "Lv_min_time": "2025-06-18 11:06:20.124",
              "Lv_avg": 29.1,
              "Lv_l5": 32.4,
              "Lv_l10": 31.2,
              "Lv_l50": 28.7,
              "Lv_l90": 26.8,
              "Lv_l95": 26.2,
              "Lv_count": 1796
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 12:00:00.000",
              "Lv_max": 78.1,
              "Lv_max_time": "2025-06-18 12:48:58.971",
              "Lv_min": 23.6,
              "Lv_min_time": "2025-06-18 12:42:51.267",
              "Lv_avg": 30.7,
              "Lv_l5": 36.4,
              "Lv_l10": 33.6,
              "Lv_l50": 29.9,
              "Lv_l90": 27.6,
              "Lv_l95": 27,
              "Lv_count": 1666
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 13:00:00.000",
              "Lv_max": 78.7,
              "Lv_max_time": "2025-06-18 13:39:53.151",
              "Lv_min": 25.1,
              "Lv_min_time": "2025-06-18 13:57:57.141",
              "Lv_avg": 30.6,
              "Lv_l5": 34.5,
              "Lv_l10": 33,
              "Lv_l50": 30,
              "Lv_l90": 27.9,
              "Lv_l95": 27.4,
              "Lv_count": 1639
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 14:00:00.000",
              "Lv_max": 79.2,
              "Lv_max_time": "2025-06-18 14:19:48.669",
              "Lv_min": 25.2,
              "Lv_min_time": "2025-06-18 14:12:45.858",
              "Lv_avg": 31.7,
              "Lv_l5": 38.5,
              "Lv_l10": 35.3,
              "Lv_l50": 30.7,
              "Lv_l90": 28.5,
              "Lv_l95": 27.9,
              "Lv_count": 1831
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 15:00:00.000",
              "Lv_max": 84.9,
              "Lv_max_time": "2025-06-18 15:23:51.436",
              "Lv_min": 25.4,
              "Lv_min_time": "2025-06-18 15:24:57.557",
              "Lv_avg": 31.5,
              "Lv_l5": 35.4,
              "Lv_l10": 33.6,
              "Lv_l50": 31,
              "Lv_l90": 28.7,
              "Lv_l95": 27.9,
              "Lv_count": 1838
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 16:00:00.000",
              "Lv_max": 78.7,
              "Lv_max_time": "2025-06-18 16:36:10.862",
              "Lv_min": 25.3,
              "Lv_min_time": "2025-06-18 16:49:03.261",
              "Lv_avg": 30.7,
              "Lv_l5": 34,
              "Lv_l10": 32.8,
              "Lv_l50": 30.2,
              "Lv_l90": 28.1,
              "Lv_l95": 27.5,
              "Lv_count": 1621
            },
            {
              "device_id": "SK000010",
              "Lv_binned_time": "2025-06-18 17:00:00.000",
              "Lv_max": 81.9,
              "Lv_max_time": "2025-06-18 17:25:32.290",
              "Lv_min": 25.7,
              "Lv_min_time": "2025-06-18 17:15:55.245",
              "Lv_avg": 30.9,
              "Lv_l5": 34.8,
              "Lv_l10": 33,
              "Lv_l50": 30.2,
              "Lv_l90": 28.2,
              "Lv_l95": 27.6,
              "Lv_count": 1859
            }
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 08:00:00.000",
              "Lv_max": 50.4,
              "Lv_max_time": "2025-06-18 08:37:45.935",
              "Lv_min": 25.9,
              "Lv_min_time": "2025-06-18 08:47:53.049",
              "Lv_avg": 29.8,
              "Lv_l5": 32.3,
              "Lv_l10": 31.8,
              "Lv_l50": 29.7,
              "Lv_l90": 27.8,
              "Lv_l95": 27.3,
              "Lv_count": 1565
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 09:00:00.000",
              "Lv_max": 61.9,
              "Lv_max_time": "2025-06-18 09:15:38.315",
              "Lv_min": 24.4,
              "Lv_min_time": "2025-06-18 09:31:05.172",
              "Lv_avg": 33.1,
              "Lv_l5": 43.9,
              "Lv_l10": 40.7,
              "Lv_l50": 31.3,
              "Lv_l90": 28.4,
              "Lv_l95": 27.7,
              "Lv_count": 1925
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 10:00:00.000",
              "Lv_max": 45,
              "Lv_max_time": "2025-06-18 10:02:50.491",
              "Lv_min": 24.7,
              "Lv_min_time": "2025-06-18 10:17:15.275",
              "Lv_avg": 29.8,
              "Lv_l5": 33.4,
              "Lv_l10": 32.2,
              "Lv_l50": 29.5,
              "Lv_l90": 27.4,
              "Lv_l95": 26.9,
              "Lv_count": 1771
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 11:00:00.000",
              "Lv_max": 80.8,
              "Lv_max_time": "2025-06-18 11:52:03.695",
              "Lv_min": 24.8,
              "Lv_min_time": "2025-06-18 11:53:26.845",
              "Lv_avg": 32,
              "Lv_l5": 41.2,
              "Lv_l10": 34.6,
              "Lv_l50": 30.5,
              "Lv_l90": 28.2,
              "Lv_l95": 27.6,
              "Lv_count": 1664
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 12:00:00.000",
              "Lv_max": 50.2,
              "Lv_max_time": "2025-06-18 12:17:29.329",
              "Lv_min": 24.7,
              "Lv_min_time": "2025-06-18 12:19:28.519",
              "Lv_avg": 30.8,
              "Lv_l5": 37,
              "Lv_l10": 34.5,
              "Lv_l50": 30.2,
              "Lv_l90": 27.7,
              "Lv_l95": 27.1,
              "Lv_count": 1796
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 13:00:00.000",
              "Lv_max": 78.7,
              "Lv_max_time": "2025-06-18 13:39:53.151",
              "Lv_min": 25.1,
              "Lv_min_time": "2025-06-18 13:57:57.141",
              "Lv_avg": 30.6,
              "Lv_l5": 34.5,
              "Lv_l10": 33,
              "Lv_l50": 30,
              "Lv_l90": 27.9,
              "Lv_l95": 27.4,
              "Lv_count": 1639
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 14:00:00.000",
              "Lv_max": 49.8,
              "Lv_max_time": "2025-06-18 14:40:33.522",
              "Lv_min": 26.7,
              "Lv_min_time": "2025-06-18 14:37:18.220",
              "Lv_avg": 31.9,
              "Lv_l5": 37.5,
              "Lv_l10": 34.4,
              "Lv_l50": 31.4,
              "Lv_l90": 29.4,
              "Lv_l95": 28.9,
              "Lv_count": 1632
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 15:00:00.000",
              "Lv_max": 84.9,
              "Lv_max_time": "2025-06-18 15:23:51.436",
              "Lv_min": 25.4,
              "Lv_min_time": "2025-06-18 15:24:57.557",
              "Lv_avg": 31.5,
              "Lv_l5": 35.4,
              "Lv_l10": 33.6,
              "Lv_l50": 31,
              "Lv_l90": 28.7,
              "Lv_l95": 27.9,
              "Lv_count": 1838
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 16:00:00.000",
              "Lv_max": 76.1,
              "Lv_max_time": "2025-06-18 16:05:37.980",
              "Lv_min": 25.3,
              "Lv_min_time": "2025-06-18 16:08:29.248",
              "Lv_avg": 31,
              "Lv_l5": 36.5,
              "Lv_l10": 34,
              "Lv_l50": 30.2,
              "Lv_l90": 28.1,
              "Lv_l95": 27.5,
              "Lv_count": 1852
            },
            {
              "device_id": "SK000020",
              "Lv_binned_time": "2025-06-18 17:00:00.000",
              "Lv_max": 81.3,
              "Lv_max_time": "2025-06-18 17:50:05.850",
              "Lv_min": 25.7,
              "Lv_min_time": "2025-06-18 17:55:27.369",
              "Lv_avg": 31.4,
              "Lv_l5": 35.6,
              "Lv_l10": 33.3,
              "Lv_l50": 30.5,
              "Lv_l90": 28.4,
              "Lv_l95": 27.9,
              "Lv_count": 1611
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 08:00:00.000",
              "Lv_max": 81.8,
              "Lv_max_time": "2025-06-18 08:21:30.996",
              "Lv_min": 26,
              "Lv_min_time": "2025-06-18 08:10:22.966",
              "Lv_avg": 31.6,
              "Lv_l5": 38.9,
              "Lv_l10": 34.2,
              "Lv_l50": 30.5,
              "Lv_l90": 28.5,
              "Lv_l95": 27.9,
              "Lv_count": 1839
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 09:00:00.000",
              "Lv_max": 78.7,
              "Lv_max_time": "2025-06-18 09:34:34.053",
              "Lv_min": 24.8,
              "Lv_min_time": "2025-06-18 09:55:21.527",
              "Lv_avg": 30.5,
              "Lv_l5": 36,
              "Lv_l10": 32.9,
              "Lv_l50": 29.5,
              "Lv_l90": 27.3,
              "Lv_l95": 26.6,
              "Lv_count": 1539
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 10:00:00.000",
              "Lv_max": 45,
              "Lv_max_time": "2025-06-18 10:02:50.491",
              "Lv_min": 24.7,
              "Lv_min_time": "2025-06-18 10:17:15.275",
              "Lv_avg": 29.8,
              "Lv_l5": 33.4,
              "Lv_l10": 32.2,
              "Lv_l50": 29.5,
              "Lv_l90": 27.4,
              "Lv_l95": 26.9,
              "Lv_count": 1771
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 11:00:00.000",
              "Lv_max": 80.8,
              "Lv_max_time": "2025-06-18 11:52:03.695",
              "Lv_min": 24.8,
              "Lv_min_time": "2025-06-18 11:53:26.845",
              "Lv_avg": 32,
              "Lv_l5": 41.2,
              "Lv_l10": 34.6,
              "Lv_l50": 30.5,
              "Lv_l90": 28.2,
              "Lv_l95": 27.6,
              "Lv_count": 1664
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 12:00:00.000",
              "Lv_max": 78.1,
              "Lv_max_time": "2025-06-18 12:48:58.971",
              "Lv_min": 23.6,
              "Lv_min_time": "2025-06-18 12:42:51.267",
              "Lv_avg": 30.7,
              "Lv_l5": 36.4,
              "Lv_l10": 33.6,
              "Lv_l50": 29.9,
              "Lv_l90": 27.6,
              "Lv_l95": 27,
              "Lv_count": 1666
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 13:00:00.000",
              "Lv_max": 49,
              "Lv_max_time": "2025-06-18 13:29:59.511",
              "Lv_min": 25,
              "Lv_min_time": "2025-06-18 13:17:38.099",
              "Lv_avg": 30.4,
              "Lv_l5": 35.5,
              "Lv_l10": 33.2,
              "Lv_l50": 29.9,
              "Lv_l90": 27.8,
              "Lv_l95": 27.3,
              "Lv_count": 1834
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 14:00:00.000",
              "Lv_max": 79.2,
              "Lv_max_time": "2025-06-18 14:19:48.669",
              "Lv_min": 25.2,
              "Lv_min_time": "2025-06-18 14:12:45.858",
              "Lv_avg": 31.7,
              "Lv_l5": 38.5,
              "Lv_l10": 35.3,
              "Lv_l50": 30.7,
              "Lv_l90": 28.5,
              "Lv_l95": 27.9,
              "Lv_count": 1831
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 15:00:00.000",
              "Lv_max": 52.8,
              "Lv_max_time": "2025-06-18 15:38:31.063",
              "Lv_min": 25,
              "Lv_min_time": "2025-06-18 15:38:51.102",
              "Lv_avg": 30.8,
              "Lv_l5": 39,
              "Lv_l10": 34.7,
              "Lv_l50": 29.9,
              "Lv_l90": 27.7,
              "Lv_l95": 27.1,
              "Lv_count": 1628
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 16:00:00.000",
              "Lv_max": 76.1,
              "Lv_max_time": "2025-06-18 16:05:37.980",
              "Lv_min": 25.3,
              "Lv_min_time": "2025-06-18 16:08:29.248",
              "Lv_avg": 31,
              "Lv_l5": 36.5,
              "Lv_l10": 34,
              "Lv_l50": 30.2,
              "Lv_l90": 28.1,
              "Lv_l95": 27.5,
              "Lv_count": 1852
            },
            {
              "device_id": "SK000030",
              "Lv_binned_time": "2025-06-18 17:00:00.000",
              "Lv_max": 81.5,
              "Lv_max_time": "2025-06-18 17:48:31.707",
              "Lv_min": 25.1,
              "Lv_min_time": "2025-06-18 17:48:12.682",
              "Lv_avg": 31,
              "Lv_l5": 34.6,
              "Lv_l10": 33,
              "Lv_l50": 30.2,
              "Lv_l90": 28.2,
              "Lv_l95": 27.6,
              "Lv_count": 1633
            }
          ],
          "sql": {
            "queryStatement": "SELECT device_id, format_datetime(date_add('minute',  540, bin(time, 1h)), 'yyyy-MM-dd HH:mm:ss.SSS') AS Lv_binned_time  ,max(Lv) AS Lv_max  ,format_datetime(date_add('minute', 540, max_by(time, Lv)), 'yyyy-MM-dd HH:mm:ss.SSS') AS Lv_max_time  ,min(Lv) AS Lv_min  ,format_datetime(date_add('minute', 540, min_by(time, Lv)), 'yyyy-MM-dd HH:mm:ss.SSS') AS Lv_min_time  ,round(avg(Lv), 1) AS Lv_avg  ,approx_percentile(Lv, 0.95) AS Lv_l5  ,approx_percentile(Lv, 0.90) AS Lv_l10  ,approx_percentile(Lv, 0.50) AS Lv_l50  ,approx_percentile(Lv, 0.10) AS Lv_l90  ,approx_percentile(Lv, 0.05) AS Lv_l95  ,count(Lv) AS Lv_count FROM soshinki_vibration1_20250502.vibration1_chunk WHERE date(date_add('minute', 540, time)) = '2025-06-18' AND measure_name = 'multi' GROUP BY device_id, date_add('minute',  540, bin(time, 1h)) ORDER BY device_id, Lv_binned_time ASC"
          }
        }

    :resheader Content-Type: application/json

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accepts :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the command.periodics request

    :>json string target.deviceId: | 端末ID（ **Request JSON Object: operation.target.deviceId** で指定した値と同一 ）
                                   | Terminal ID（ Same value as the one specified by **Request JSON Object: operation.target.deviceId** ）
                                   | **Request JSON Object: operation.target.deviceId** を省略した場合、本項目は存在しない
                                   | This item is not present if **Request JSON Object: operation.target.deviceId** was omitted.
    :>json string target.deviceType: | Device Type（ **Request JSON Object: operation.target.deviceType** で指定した値と同一 ）
                                     | Device Type（ Same value as the one specified by **Request JSON Object: operation.target.deviceType** ）
    :>json array<object> responseValues[<ValuesObjects>]: | **<ValuesObjects>** -- 戻り値を含むオブジェクト
                                                          |                        Objects containing returned values
    .. list-table::
        :header-rows: 1
        :widths: 3, 1, 2

        * - Case
          - Item of <ValuesObjects>
          - Description
        * - **Request JSON Object: operation.target.deviceId** が省略された場合の戻り値
            
            Returned value if **Request JSON Object: operation.target.deviceId** was omitted
          - ``device_id``
          - 端末ID
            
            Terminal ID
        * - **deviceType == "noise1"**, **"noise2"** or **"noise4"** の場合の戻り値
            
            Returned value in case of **deviceType == "noise1"**, **"noise2"** or **"noise4"**
          - ``Lp_binned_time``
          - 分割集計時刻（騒音計測データ）
            
            Split aggregate time, noise measured data
        * - 同上
            
            Same as the above
          - ``Lp_max``
          - 分割集計期間内での騒音最大値
            
            Maximum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_max_time``
          - Lp_max 値の取得時刻
            
            Time at which the Lp_max value was acquired
        * - 同上
            
            Same as the above
          - ``Lp_min``
          - 分割集計期間内での騒音最小値
            
            Minimum noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_min_time``
          - Lp_min 値の取得時刻
            
            Time at which the Lp_min value was acquired
        * - 同上
            
            Same as the above
          - ``Lp_avg``
          - 分割集計期間内での騒音平均値
            
            Average noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l5``
          - 分割集計期間内での騒音L5値
            
            L5 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l10``
          - 分割集計期間内での騒音L10値
            
            L10 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l50``
          - 分割集計期間内での騒音L50値
            
            L50 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l90``
          - 分割集計期間内での騒音L90値
            
            L90 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_l95``
          - 分割集計期間内での騒音L95値
            
            L95 noise value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lp_count``
          - 分割集計期間内の騒音計測データの個数
            
            Number of noise measure data within the aggregate time period
        * - **deviceType == "vibration1"** or **"vibration4"** の場合の戻り値
            
            Returned value in case of **deviceType == "vibration1"** or **"vibration4"**
          - ``Lv_binned_time``
          - 分割集計時刻（振動計測データ）
            
            Split aggregate time, vibration measured data
        * - 同上
            
            Same as the above
          - ``Lv_max``
          - 分割集計期間内での振動最大値
            
            Maximum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_max_time``
          - Lv_max 値の取得時刻
            
            Time at which the Lv_max value was acquired
        * - 同上
            
            Same as the above
          - ``Lv_min``
          - 分割集計期間内での振動最小値
            
            Minimum vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_min_time``
          - Lv_min 値の取得時刻
            
            Time at which the Lv_min value was acquired
        * - 同上
            
            Same as the above
          - ``Lv_avg``
          - 分割集計期間内での振動平均値
            
            Average vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l5``
          - 分割集計期間内での振動L5値
            
            L5 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l10``
          - 分割集計期間内での振動L10値
            
            L10 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l50``
          - 分割集計期間内での振動L50値
            
            L50 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l90``
          - 分割集計期間内での振動L90値
            
            L90 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_l95``
          - 分割集計期間内での振動L95値
            
            L95 vibration value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Lv_count``
          - 分割集計期間内の振動計測データの個数
            
            Number of vibration measure data within the aggregate time period
        * - **deviceType == "weather1"** の場合の戻り値
            
            Returned value in case of **deviceType == "weather1"**
          - ``Wh_binned_time``
          - 分割集計時刻（気象計測データ）
            
            Split aggregate time, weather measured data
        * - 同上
            
            Same as the above
          - ``Wh_temp_max``
          - 分割集計期間内での温度最大値
            
            Maximum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_temp_max_time``
          - Wh_temp_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_temp_min``
          - 分割集計期間内での温度最小値
            
            Minimum temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_temp_min_time``
          - Wh_temp_min 値の取得時刻
            
            Time at which the Wh_temp_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_temp_avg``
          - 分割集計期間内での温度平均値
            
            Average temperature value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_hum_max``
          - 分割集計期間内での湿度最大値
            
            Maximum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_hum_max_time``
          - Wh_hum_max 値の取得時刻
            
            Time at which the Wh_temp_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_hum_min``
          - 分割集計期間内での湿度最小値
            
            Minimum humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_hum_min_time``
          - Wh_hum_min 値の取得時刻
            
            Time at which the Wh_hum_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_hum_avg``
          - 分割集計期間内での湿度平均値
            
            Average humidity value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_ws_max``
          - 分割集計期間内での風速最大値
            
            Maximum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_ws_max_time``
          - Wh_ws_max 値の取得時刻
            
            Time at which the Wh_ws_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_ws_min``
          - 分割集計期間内での風速最小値
            
            Minimum wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_ws_min_time``
          - Wh_ws_min 値の取得時刻
            
            Time at which the Wh_ws_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_ws_avg``
          - 分割集計期間内での風速平均値
            
            Average wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_gws_max``
          - 分割集計期間内での瞬間風速最大値
            
            Maximum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_gws_max_time``
          - Wh_gws_max 値の取得時刻
            
            Time at which the Wh_gws_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_gws_min``
          - 分割集計期間内での瞬間風速最小値
            
            Minimum gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_gws_min_time``
          - Wh_gws_min 値の取得時刻
            
            Time at which the Wh_gws_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_gws_avg``
          - 分割集計期間内での瞬間風速平均値
            
            Average gust wind speed value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_arf_max``
          - 分割集計期間内での積算雨量最大値
            
            Maximum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_arf_max_time``
          - Wh_arf_max 値の取得時刻
            
            Time at which the Wh_arf_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_arf_min``
          - 分割集計期間内での積算雨量最小値
            
            Minimum accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_arf_min_time``
          - Wh_gws_min 値の取得時刻
            
            Time at which the Wh_arf_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_arf_avg``
          - 分割集計期間内での積算雨量平均値
            
            Average accumulation rainfall value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_uv_max``
          - 分割集計期間内での紫外線量最大値
            
            Maximum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_uv_max_time``
          - Wh_uv_max 値の取得時刻
            
            Time at which the Wh_uv_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_uv_min``
          - 分割集計期間内での紫外線量最小値
            
            Minimum UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_uv_min_time``
          - Wh_uv_min 値の取得時刻
            
            Time at which the Wh_uv_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_uv_avg``
          - 分割集計期間内での紫外線量平均値
            
            Average UV index value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_li_max``
          - 分割集計期間内での光照度最大値
            
            Maximum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_li_max_time``
          - Wh_li_max 値の取得時刻
            
            Time at which the Wh_li_max value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_li_min``
          - 分割集計期間内での光照度最小値
            
            Minimum light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_li_min_time``
          - Wh_li_min 値の取得時刻
            
            Time at which the Wh_li_min value was acquired
        * - 同上
            
            Same as the above
          - ``Wh_li_avg``
          - 分割集計期間内での光照度平均値
            
            Average light illuminance value within the aggregate time period
        * - 同上
            
            Same as the above
          - ``Wh_count``
          - 分割集計期間内の気象計測データの個数
            
            Number of weather measure data within the aggregate time period
    :>json string sql.queryStatement: | リクエスト実行のために生成したSQLクエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.

command.sqlExecute
^^^^^^^^^^^^^^^^^^

.. http:put:: /

    | Timestream DBに対してSQLクエリ文を実行し、その取得値を返す
    | Executes SQL query statement against Timestream DB and returns the obtained values

    **Request Example**

    - | **operation.target.deviceId** と **operation.target.deviceType** を指定する場合
      | **operation.target.deviceId** and **operation.target.deviceType** are present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: */*
        Header: Content-Type: application/json
                X-Api-Signature: 002ff47e????????????????????????????????????????????????????????

        {"type": "command.sqlExecute", "operation": {"target": {"deviceId": "SK000010", "deviceType": "noise1"}, "parameters": {"sql": {"statements": ["SELECT max(Lp) AS noise_max, format_datetime(date_add('hour', 9, max_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS noise_max_time, min(Lp) AS noise_min, format_datetime(date_add('hour', 9, min_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS noise_min_time, round(avg(Lp), 1) AS noise_avg, approx_percentile(Lp, ARRAY[0.95, 0.90, 0.50, 0.10, 0.05]) AS noise_ln", "date(date_add('hour', 9, time)) = '2025-06-18'"], "queryStatement": false}}}}

    - | **operation.target.deviceId** を指定せず、 **operation.target.deviceType** のみ指定する場合
      | **operation.target.deviceId** is not present, only **operation.target.deviceType** is present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: a9a2bdc3????????????????????????????????????????????????????????

        {"type": "command.sqlExecute", "operation": {"target": {"deviceType": "noise1"}, "parameters": {"sql": {"statements": ["SELECT device_id, max(Lp) AS noise_max, format_datetime(date_add('hour', 9, max_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS noise_max_time, min(Lp) AS noise_min, format_datetime(date_add('hour', 9, min_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS noise_min_time, round(avg(Lp), 1) AS noise_avg, approx_percentile(Lp, ARRAY[0.95, 0.90, 0.50, 0.10, 0.05]) AS noise_ln", "WHERE date(date_add('hour', 9, time)) = '2025-06-18'", "GROUP BY device_id ORDER BY device_id ASC"], "queryStatement": true}}}}

    - | **operation.target.deviceId** と **operation.target.deviceType** を指定しない場合
      | **operation.target.deviceId** and **operation.target.deviceType** are not present

    .. sourcecode:: http

        PUT / HTTP/1.1
        Host: <IotRetrieverFunctionUrl>
        Accept: application/json
        Header: Content-Type: application/json
                X-Api-Signature: 378786b8????????????????????????????????????????????????????????

        {"type": "command.sqlExecute", "operation": {"parameters": {"sql": {"statements": ["SELECT device_id, max(Lv) AS vibration_max, format_datetime(date_add('hour', 9, max_by(time, Lv)), 'yyyy-MM-dd HH:mm:ss.SSS') AS vibration_max_time, min(Lv) AS vibration_min, format_datetime(date_add('hour', 9, min_by(time, Lv)), 'yyyy-MM-dd HH:mm:ss.SSS') AS vibration_min_time, round(avg(Lv), 1) AS vibration_avg, approx_percentile(Lv, ARRAY[0.95, 0.90, 0.50, 0.10, 0.05]) AS vibration_ln", "FROM soshinki_vibration1_20250502.vibration1_chunk", "WHERE measure_name = 'multi' AND date(date_add('hour', 9, time)) = '2025-06-18'", "GROUP BY device_id ORDER BY device_id ASC"], "queryStatement": true}}}}

    :reqheader Content-Type: application/json
    :reqheader X-Api-Signature: | リクエストのBodyテキストから計算したシークレット・キー
                                | Secret key calculated from the body text of the request
                                | (*) See `API Signature / HTTP API ( IotRetrieverFunction ) - questar-ac/aws_soshinki-backend <https://github.com/questar-ac/aws_soshinki-backend/blob/main/docs/WebInterface.md#api-signature>`_

    :<json string type: ``"command.sqlExecute"``
    :<json string operation.target.deviceId: | 端末ID
                                             | Terminal ID
                                             | SQLクエリ文文字列 **operation.parameters.sql.statements[<StatementStrings>]** の中に ``WHERE`` 句が含まれる場合は省略可能
                                             | Can be omitted if the SQL query statement strings **operation.parameters.sql.statements[<StatementStrings>]** include a ``WHERE`` phrase.
    :<json string operation.target.deviceType: | `Device Type <https://omoikane-fw.readthedocs.io/ja/latest/common_definition.html#device-type>`_
                                               | SQLクエリ文文字列 **operation.parameters.sql.statements[<StatementStrings>]** の中に ``FROM`` 句が含まれている場合は省略可能
                                               | Can be omitted, if the SQL statement strings **operation.parameters.sql.statements[<StatementStrings>]** include a ``FROM`` phrase
    :<json array<string> operation.parameters.sql.statements[<StatementStrings>]: | **<StatementStrings>** -- SQLクエリ文文字列
                                                                                  |                           Strings of SQL query statement
    :<json boolean operation.parameters.sql.queryStatement: | 本リクエスト実行のために生成したSQLクエリ文文字列をレスボンスとして返すかどうかを指定する
                                                            | Whether a string of SQL query statement to execute this request should be returned or not

    **Response Example**

    - | **target.deviceId** と **target.deviceType** が存在する (**Request JSON Object: operation.target.deviceId** と **operation.target.deviceType** を指定した場合)
      | **target.deviceId** and **target.deviceType** are present (**Request JSON Object: operation.target.deviceId** and **operation.target.deviceType** were present)

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
              "noise_max": 82,
              "noise_max_time": "2025-06-18 16:51:53.862",
              "noise_min": 46.4,
              "noise_min_time": "2025-06-18 07:37:02.387",
              "noise_avg": 55.3,
              "noise_ln": [
                69.4,
                66.1,
                54.4,
                49.1,
                48.8
              ]
            }
          ]
        }

    - | **target.deviceId** が存在せず、 **target.deviceType** は存在する  (**Request JSON Object: operation.target.deviceId** を指定せず、 **operation.target.deviceType** のみ指定した場合)
      | **target.deviceId** is not present, **target.deviceType** is present (**Request JSON Object: operation.target.deviceId** was not present,  only **operation.target.deviceType** was present)

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
              "noise_max": 82,
              "noise_max_time": "2025-06-18 16:51:53.862",
              "noise_min": 46.4,
              "noise_min_time": "2025-06-18 07:37:02.387",
              "noise_avg": 55.3,
              "noise_ln": [
                69.4,
                66.1,
                54.4,
                49.1,
                48.8
              ]
            },
            {
              "device_id": "SK000020",
              "noise_max": 86.9,
              "noise_max_time": "2025-06-18 09:12:01.321",
              "noise_min": 48.3,
              "noise_min_time": "2025-06-18 09:13:24.458",
              "noise_avg": 54.9,
              "noise_ln": [
                69.1,
                66.2,
                53.6,
                49.3,
                49.2
              ]
            },
            {
              "device_id": "SK000030",
              "noise_max": 82.7,
              "noise_max_time": "2025-06-18 09:47:55.929",
              "noise_min": 48.7,
              "noise_min_time": "2025-06-18 09:40:51.166",
              "noise_avg": 55.1,
              "noise_ln": [
                67.9,
                64.7,
                53.6,
                49.8,
                49.5
              ]
            }
          ],
          "sql": {
            "queryStatement": "SELECT device_id, max(Lp) AS noise_max, format_datetime(date_add('hour', 9, max_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS noise_max_time, min(Lp) AS noise_min, format_datetime(date_add('hour', 9, min_by(time, Lp)), 'yyyy-MM-dd HH:mm:ss.SSS') AS noise_min_time, round(avg(Lp), 1) AS noise_avg, approx_percentile(Lp, ARRAY[0.95, 0.90, 0.50, 0.10, 0.05]) AS noise_ln FROM soshinki_noise1_20250502.noise1_chunk WHERE date(date_add('hour', 9, time)) = '2025-06-18' AND measure_name = 'multi' GROUP BY device_id ORDER BY device_id ASC "
          }
        }

    - | **target.deviceId** と **target.deviceType** が存在しない  (**Request JSON Object: operation.target.deviceId** と **operation.target.deviceType** を指定しなかった場合)
      | **target.deviceId** and **target.deviceType** are not present (**Request JSON Object: operation.target.deviceId** and **operation.target.deviceType** were not present)

    .. sourcecode:: http

        HTTP/1.1 200 OK
        Content-Type: application/json

        {
          "responseValues": [
            {
              "device_id": "SK000010",
              "vibration_max": 81.9,
              "vibration_max_time": "2025-06-18 17:25:32.290",
              "vibration_min": 23.6,
              "vibration_min_time": "2025-06-18 10:53:05.579",
              "vibration_avg": 31.2,
              "vibration_ln": [
                39.4,
                35,
                30.3,
                27.9,
                27.2
              ]
            },
            {
              "device_id": "SK000020",
              "vibration_max": 84.9,
              "vibration_max_time": "2025-06-18 15:23:51.436",
              "vibration_min": 24.4,
              "vibration_min_time": "2025-06-18 09:31:05.172",
              "vibration_avg": 31.5,
              "vibration_ln": [
                38.9,
                34.7,
                30.6,
                28.3,
                27.7
              ]
            },
            {
              "device_id": "SK000030",
              "vibration_max": 88.3,
              "vibration_max_time": "2025-06-18 19:26:31.482",
              "vibration_min": 23.6,
              "vibration_min_time": "2025-06-18 12:42:51.267",
              "vibration_avg": 30.7,
              "vibration_ln": [
                36,
                33.4,
                30,
                27.8,
                27.2
              ]
            }
          ],
          "sql": {
            "queryStatement": "SELECT device_id, max(Lv) AS vibration_max, format_datetime(date_add('hour', 9, max_by(time, Lv)), 'yyyy-MM-dd HH:mm:ss.SSS') AS vibration_max_time, min(Lv) AS vibration_min, format_datetime(date_add('hour', 9, min_by(time, Lv)), 'yyyy-MM-dd HH:mm:ss.SSS') AS vibration_min_time, round(avg(Lv), 1) AS vibration_avg, approx_percentile(Lv, ARRAY[0.95, 0.90, 0.50, 0.10, 0.05]) AS vibration_ln FROM soshinki_vibration1_20250502.vibration1_chunk WHERE measure_name = 'multi' AND date(date_add('hour', 9, time)) = '2025-07-02' GROUP BY device_id ORDER BY device_id ASC "
          }
        }

    :resheader Content-Type: application/json

    :statuscode 200: Success
    :statuscode 400: Illegal HTTP access (This request accepts :http:put:`/` only. The access was http.method != :http:method:`put` or http.path != '/')
    :statuscode 400: Invalid JSON payload
    :statuscode 401: Invalid API signature
    :statuscode 500: Unable to handle the command.sqlExecute request

    :>json string target.deviceId: | 端末ID（ **Request JSON Object: operation.target.deviceId** で指定した値と同一 ）
                                   | Terminal ID（ Same value as the one specified by **Request JSON Object: operation.target.deviceId** ）
                                   | **Request JSON Object: operation.target.deviceId** を省略した場合、本項目は存在しない
                                   | This item is not present if **Request JSON Object: operation.target.deviceId** was omitted.
    :>json string target.deviceType: | Device Type（ **Request JSON Object: operation.target.deviceType** で指定した値と同一 ）
                                     | Device Type（ Same value as the one specified by **Request JSON Object: operation.target.deviceType** ）
                                     | **Request JSON Object: operation.target.deviceType** を省略した場合、本項目は存在しない
                                     | This item is not present if **Request JSON Object: operation.target.deviceType** was omitted.
    :>json array<object> responseValues[<ValuesObjects>]: | **<ValuesObjects>** -- 戻り値を含むオブジェクト
                                                          |                        Objects containing returned values
                                                          | SQLクエリ文 **Request JSON Object: operation.parameters.sql.statements[<StatementStrings>]** の中で ``"SELECT device_id"`` と ``"SELECT <calculatation> AS <ValueName>"`` で指定した値名がオブジェクト要素のキーとなる
                                                          | The value names specified by ``"SELECT device_id"`` and ``"SELECT <calculatation> AS <ValueName>"`` in the SQL query statement, **Request JSON Object: operation.parameters.sql.statements[<StatementStrings>]**, become the keys of the object items
    :>json string sql.queryStatement: | リクエスト実行のために生成したSQLクエリ文文字列
                                      | String of SQL query statement to execute the request
                                      | **Request JSON Object: operation.parameters.sql.queryStatement** が ``false`` の場合、本項目は存在しない
                                      | This item is not present if **Request JSON Object: operation.parameters.sql.queryStatement** was ``false``.
