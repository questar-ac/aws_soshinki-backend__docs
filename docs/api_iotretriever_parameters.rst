.. _section-api-iotretriever-parameters:


.. _section-api-iotretriever-parameters-sql:

operation.parameters.sql
========================

.. _section-api-iotretriever-parameters-sql-wheretimephrase:

whereTimePhrase
^^^^^^^^^^^^^^^

.. list-table::
    :header-rows: 1
    :widths: 2, 3

    * - Notation
      - Description

    * - ``time BETWEEN '2025-05-14 06:00:00.000' AND '2025-05-14 17.59.59.999'``
      - 2025/5/14 6時0分0秒0ミリ秒〜17時59分59秒999ミリ秒（6時丁度〜18時丁度）
        
        2025/5/14 6:0:0.0 msec - 17:59:59.999 msec (6:00 exactly - 18:00 exactly)
    * - ``time BETWEEN ago(1d) AND now()``
      - 現時刻の1日（24時間）前〜現時刻
        
        one day (24 hours) before the current time to the current time
    * - ``date(time) = '2025-05-14'``
      - 2025/5/14、1日間
        
        2025/5/14, one day
    * - ``date(time) BETWEEN '2025-05-12' AND '2025-05-21'``
      - 2025/5/12〜2025/5/21、10日間
        
        2025/5/12〜2025/5/21, 10 days
    * - ``date(time) BETWEEN '2025-05-14' AND date(date_add('day', 6, '2025-05-14'))``
      - ``'yyyy-MM-dd'`` で指定した日付から1週間 (2025/5/10〜2025/5/16)
        
        one week starting on the date specified by ``'yyyy-MM-dd'``
    * - ``date(time) BETWEEN date(date_trunc('week', '2025-05-14')) AND date(date_add('day', 6, date_trunc('week', '2025-05-14')))``
      - ``'yyyy-MM-dd'`` で指定した日付を含む1週間 (2025/5/12〜2025/5/18, 月〜日曜)
        
        one week including the date specified by ``'yyyy-MM-dd'``, Monday - Sunday
    * - ``date(time) BETWEEN date(date_trunc('week', now())) AND date(date_add('day', 6, date_trunc('week', now())))``
      - 本日を含む1週間, 月〜日曜 (本日が2025/05/14の場合、2025/5/12〜2025/5/18)
        
        one week including today, Monday - Sunday, 2025/5/12〜2025/5/18 if today is 2025/05/14
    * - ``date(time) BETWEEN '2025-05-01' AND last_day_of_month('2025-05-01')``
      - ``'yyyy-MM-dd'`` で指定した日付から1ヶ月間 (2025/5/1〜2025/5/31)
        
        one month starting on the date specified by ``'yyyy-MM-dd'``
    * - ``date(time) BETWEEN date(date_trunc('month', '2025-05-14')) AND last_day_of_month('2025-05-14')``
      - ``'yyyy-MM-dd'`` で指定した日付を含む1ヶ月間 (2025/5/1〜2025/5/31)
        
        one month including the date specified by ``'yyyy-MM-dd'``
    * - ``date(time) BETWEEN date(date_trunc('month', now())) AND last_day_of_month(now())``
      - 本日を含む1ヶ月間 (本日が2025/05/14の場合、2025/5/1〜2025/5/31)
        
        one month including today, 2025/5/1〜2025/5/31 if today is 2025/5/14

.. _section-api-iotretriever-parameters-sql-timebininterval:

timeBinInterval
^^^^^^^^^^^^^^^

.. list-table::
    :header-rows: 1
    :widths: 1, 3

    * - Notation
      - Description

    * - ``1s``
      - 1秒
        
        1 second
    * - ``10s``
      - 10秒
        
        10 seconds
    * - ``1m``
      - 1分
        
        1 minute
    * - ``10m``
      - 10分
        
        10 minutes
    * - ``30m``
      - 30分
        
        30 minutes
    * - ``1h``
      - 1時間
        
        1 hour
    * - ``12h``
      - 12時間
        
        12 hours
    * - ``1d``
      - 1日
        
        1 day
    * - ``1week``
      - 1週
        
        1 week
    * - ``1month``
      - 1ヶ月
        
        1 month
