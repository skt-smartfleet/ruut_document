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
''''''''''''''''''''''''''''''''''''''''''''''

TFP는 Traffic Flow and Predictions 의 약어로 실시간 교통 정보 및 예측 교통 정보를 제공하는 어플리케이션 입니다. 
TFP 메시지를 수신하시려면 아래 항목을 검토 작성하셔야 합니다.

:underline:`예) 특정 지역 중심 반경 1km 원형 영역 내 TFP 메시지를 TPEG ML 형태로 요청`

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`tfp`)
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, TFP application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619,127.112465&radius=1&app=tfp&format=tpegMl

:underline:`Response Example`

.. code-block:: none

  ...
  <tfp:method xsi:type="tfp:FlowStatus">
    <tfp:startTime>1970-01-01T00: 00: 00Z</tfp:startTime>
    <tfp:duration>0</tfp:duration>
    <tfp:status>
      <tfp:LOS tfp:code="0" tfp:table="tfp003_LevelOfService"/>
      <tfp:averageSpeed>51</tfp:averageSpeed>
      <tfp:freeFlowTravelTime>70</tfp:freeFlowTravelTime>
    </tfp:status>
    <tfp:restriction>
      <tfp:lanes tfp:code="0" tfp:table="tfp005_laneRestriction"/>
    </tfp:restriction>
      <tfp:cause tfp:code="0" tfp:table="tfp006_CauseCode"/>
    <tfp:detailedCause>
      <tfp:messageID>0</tfp:messageID>
      <tfp:COID>0</tfp:COID>
    </tfp:detailedCause>
  </tfp:method>
  ...


TEC (사고/이벤트 정보)
''''''''''''''''''''''''''

TEC는 Traffic Event Compact 의 약어로 실시간 교통 이벤트 정보를 (사고, 통제, 공사 등) 제공하는 어플리케이션 입니다. 
TEC 메시지를 수신하시려면 아래 항목을 검토 작성하셔야 합니다.

:underline:`예) 특정 지역 중심 반경 1km 원형 영역 내 TEC 메시지를 TPEG ML 형태로 요청`

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`tec`)
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, TEC application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619,127.112465&radius=1&app=tfp&format=tpegMl

:underline:`Response Example`

.. code-block:: none

  ...
  <tec:event>
    <tec:effectCode tec:code="4" tec:table="tec001_EffectCode"/>
    <tec:startTime>2020-01-30T22: 47: 00Z</tec:startTime>
    <tec:stopTime>2020-01-31T09: 00: 00Z</tec:stopTime>
    <tec:cause>
      <tec:optionDirectCause>
        <tec:mainCause tec:code="3" tec:table="tec002_CauseCode"/>
        <tec:warningLevel tec:code="0" tec:table="tec003_WarningLevel"/>
        <tec:unverifiedInformation>false</tec:unverifiedInformation>
        <tec:subCause tec:code="3" tec:table="tec100_SubCause"/>
        <tec:laneRestrictionType tec:code="0" tec:table="tec004_LaneRestriction"/>
        <tec:freeText>
          <tdt:languageCode tdt:code="85" tdt:table="typ001_LanguageCode"/>
          <tdt:value>경찰청제공 공사 방아다리길 탄천로95 에서 방아다리사거리 방향 1차로 도로공사 주의운전</tdt:value>
        </tec:freeText>
      </tec:optionDirectCause>
    </tec:cause>
  </tec:event>
  ...

WEA (날씨 정보)
''''''''''''''''''''''''''

WEA는 Weather 의 약어로 실시간 날씨 정보를 제공하는 어플리케이션 입니다. 
WEA 메시지를 수신하시려면 아래 항목을 검토 작성하셔야 합니다.

:underline:`예) 특정 지역 중심 반경 1km 원형 영역 내 WEA 메시지를 TPEG ML 형태로 요청`

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`wea`)
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, WEA application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619,127.112465&radius=1&app=wea&format=tpegMl

:underline:`Response Example`

.. code-block:: none

  ...
  <wea:weatherInfo>
    <wea:geographicalSignificance wea:code="6" wea:table="wea011_GeoSignificance"/>
    <wea:weatherReport>
      <wea:reportType wea:code="4" wea:table="wea000_ReportType"/>
      <wea:weatherDefinition>
        <wea:period wea:code="0" wea:table="wea001_Period"/>
        <wea:weatherDescription>
          <wea:subTableType wea:code="106" wea:table="wea100_ElementType"/>
          <wea:subTableValue wea:code="0" wea:table="wea099_ElementSubTable"/>
        </wea:weatherDescription>
        <wea:start>0000-00-00T00: 09: 00</wea:start>
        <wea:stop>0000-00-00T01: 09: 00</wea:stop>
        <wea:date>0000-00-27T00: 00: 00</wea:date>
        <wea:statistics>
          <wea:cloudCover>8</wea:cloudCover>
          <wea:pressure>1013</wea:pressure>
          <wea:temp>13.58</wea:temp>
          <wea:tempMax>14</wea:tempMax>
          <wea:tempMin>13</wea:tempMin>
          <wea:windDirection wea:code="0" wea:table="wea003_Direction"/>
          <wea:windSpeed>1</wea:windSpeed>
          <wea:relativeHumidity>188</wea:relativeHumidity>
          <wea:sunrise>2020-03-27T06: 24: 25</wea:sunrise>
          <wea:sunset>2020-03-27T18: 48: 59</wea:sunset>
        </wea:statistics>
      </wea:weatherDefinition>
    </wea:weatherReport>
  </wea:weatherInfo>
  ...

TPEG 어플리케이션 조합
''''''''''''''''''''''''''

위에서 설명한 TPEG 어플리케이션은 사용자 편의에 따라 조합하여 요청할 수 있습니다. 하나 이상의 어플리케이션 정보를 확인하려면 아래 항목을 검토 작성하셔야 합니다.

:underline:`예) 특정 지역 중심 반경 1km 원형 영역 내 TFP/TEC/WEA 메시지를 TPEG ML 형태로 요청`

* :ref:`Geo filtering <geofilter>` 교통 정보 탐색하고자 하는 지리적 영역 규정 (`geoFilter`)
* 획득 하고자 하는 TPEG 어플리케이션 유형 선택 (`tfp,tec,wea`). (쉼표 ',') 를 통해 구분하여 여러 어플리케이션 조합 요청 가능
* 메시지 포맷 선택 (`base64xml`, `tpegMl`)

:underline:`Request Example`

.. code-block:: none

  // 원형 geo filter, WEA, TFP application 표출, TPEG ML 형태로 응답
  ruut/v1/tpeg/getMessage?geoFilter=circle&center=37.397619,127.112465&radius=1&app=tfp,tec,wea&format=tpegMl

:underline:`Response Example`

.. code-block:: none

  // TFP messages
  ...
  <tfp:method xsi:type="tfp:FlowStatus">
    <tfp:startTime>1970-01-01T00: 00: 00Z</tfp:startTime>
    <tfp:duration>0</tfp:duration>
    <tfp:status>
      <tfp:LOS tfp:code="4" tfp:table="tfp003_LevelOfService"/>
      <tfp:averageSpeed>15</tfp:averageSpeed>
      <tfp:freeFlowTravelTime>60</tfp:freeFlowTravelTime>
    </tfp:status>
  ...

  //TEC messages
  ...
  <tec:event>
    <tec:effectCode tec:code="4" tec:table="tec001_EffectCode"/>
    <tec:startTime>2020-01-30T22: 47: 00Z</tec:startTime>
    <tec:stopTime>2020-01-31T09: 00: 00Z</tec:stopTime>
    <tec:cause>
      <tec:optionDirectCause>
        <tec:mainCause tec:code="3" tec:table="tec002_CauseCode"/>
        <tec:warningLevel tec:code="0" tec:table="tec003_WarningLevel"/>
        <tec:unverifiedInformation>false</tec:unverifiedInformation>
        <tec:subCause tec:code="3" tec:table="tec100_SubCause"/>
        <tec:laneRestrictionType tec:code="0" tec:table="tec004_LaneRestriction"/>
        <tec:freeText>
          <tdt:languageCode tdt:code="85" tdt:table="typ001_LanguageCode"/>
          <tdt:value>경찰청제공 공사 방아다리길 탄천로95 에서 방아다리사거리 방향 1차로 도로공사 주의운전</tdt:value>
...

//WEA messages
...
<wea:weatherInfo>
  <wea:geographicalSignificance wea:code="6" wea:table="wea011_GeoSignificance"/>
  <wea:weatherReport>
    <wea:reportType wea:code="4" wea:table="wea000_ReportType"/>
    <wea:weatherDefinition>
      <wea:period wea:code="0" wea:table="wea001_Period"/>
      <wea:weatherDescription>
        <wea:subTableType wea:code="106" wea:table="wea100_ElementType"/>
        <wea:subTableValue wea:code="0" wea:table="wea099_ElementSubTable"/>
      </wea:weatherDescription>
      <wea:start>0000-00-00T00: 09: 00</wea:start>
      <wea:stop>0000-00-00T01: 09: 00</wea:stop>
      <wea:date>0000-00-27T00: 00: 00</wea:date>
      <wea:statistics>
        <wea:cloudCover>8</wea:cloudCover>
        <wea:pressure>1013</wea:pressure>
        <wea:temp>13.58</wea:temp>
...


