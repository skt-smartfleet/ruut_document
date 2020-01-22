RUUT TPEG 서비스 가이드
=======================================
.. _tpeg2:

인증
--------------------------
:ref:`RUUT 인증 프로세스 <general_authentication>` 참조

TPEG 응답 교통 정보 요청
--------------------------

RUUT는 TPEG2 표준을 기반으로 3개의 TPEG applications 을 제공합니다. 사용자는 RUUT API 사용과 동일한 사전 작업을 통하여 권한을 획득하고 API 호출을 통해 원하는 사양의 TPEG 메시지를 수신할 수 있습니다.

- Header

.. rst-class:: table-width-fix
.. rst-class:: table-width-full
.. rst-class:: text-align-justify

+---------------------+--------+------------------+--------------+
| option              | Type   | Default          | Description  |
+=====================+========+==================+==============+
| Content-Type        | string | application/json | content type |
+---------------------+--------+------------------+--------------+
| X-Authorization     | string | {accessToken}    | API Key      |
+---------------------+--------+------------------+--------------+

모든 TPEG 메시지는 사용자 편의를 위하여 2가지 메시지 포맷을 지원합니다. 

* base64xml : TPEG 메시지 binary 를 base64 encoding 한 후 xml 형태로 응답
* tpegMl : TPEG 표준에서 정의한 xsd 에 따른 xml 응답

TFP (교통 정보)
''''''''''''''''''''''''''

TFP는 Traffic Flow and Predictions 의 약어로 실시간 교통 정보 및 예측 교통 정보를 제공하는 어플리케이션 입니다. 
TFP 메시지를 수신하시려면 아래 항목을 검토 작성하셔야 합니다.

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`app`)
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, TFP application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619, 127.112465&radius=10&app=tec&format=tpegMl


TEC (사고/이벤트 정보)
''''''''''''''''''''''''''

TEC는 Traffic Event Compact 의 약어로 실시간 교통 이벤트 정보를 (사고, 통제, 공사 등) 제공하는 어플리케이션 입니다. 
TEC 메시지를 수신하시려면 아래 항목을 검토 작성하셔야 합니다.

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`app`)
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, TEC application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619, 127.112465&radius=10&app=tfp&format=tpegMl

WEA (날씨 정보)
''''''''''''''''''''''''''

WEA는 Weather 의 약어로 실시간 날씨 정보를 제공하는 어플리케이션 입니다. 
WEA 메시지를 수신하시려면 아래 항목을 검토 작성하셔야 합니다.

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`app`)
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, WEA application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619, 127.112465&radius=10&app=wea&format=tpegMl

TPEG 어플리케이션 조합
''''''''''''''''''''''''''

위에서 설명한 TPEG 어플리케이션은 사용자 편의에 따라 조합하여 요청할 수 있습니다. 하나 이상의 어플리케이션 정보를 확인하려면 아래 항목을 검토 작성하셔야 합니다.

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`app`). (쉼표 ',') 를 통해 구분하여 여러 어플리케이션 조합 요청 가능
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, WEA, TFP application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619, 127.112465&radius=10&app=wea,tfp&format=tpegMl


