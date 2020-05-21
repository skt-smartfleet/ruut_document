메시지 포맷
=======================================

.. rst-class:: table-width-fix

.. _messageformats:

RUUT의 메세지는 크게 3가지로 구분됩니다. 

* Segment : Segment 기반의 실시간 교통 정보 메시지
* Incident : Segment 기반의 이벤트 (사고/공사 등) 정보 메시지
* TPEG : TPEG2 TFP/TEC/WEA 어플리케이션 Binary 또는 TPEG-ML 메시지

.. _segmentformats:

Segment
----------------
RUUT 는 지도 형태 독립적으로 도로를 관리합니다. 이에 교통 정보를 더 효율적으로 관리할 수 있도록 별도의 도로 링크 형태를 활용하며 이를 시스템 내에서 Segment라 부릅니다.하나의 Segment는 시작 노드(Start Node)와 종료 노드(End Node)를 가진 선형 링크(Linear Link)이며, RUUT 의 모든 기본 정보 제공 단위는 Segment이며 해당 Segment를 활용한 맵 매칭을 위해서 openLR, AGORA-C 와 같은 Dynamic Location Referencing 기법에 따른 위치 참조 코드를 제공하고 있습니다. Dynamic Location Referencing 기법을 활용하기 어려운 light 사용자를 위하여 Segment의 시작/끝 지점에 대한 GPS 좌표를 동시에 제공하고 있습니다.

RUUT 를 구성하는 가장 기본 구성 단위이기 때문에 Segment 를 endpoint 로 하는 API 요청 응답은 Segment 기반으로 실시간 교통 정보를 전달하고 해당 메시지 포맷은 아래 코드 블록과 같습니다.

**Segment 메시지 응답**

.. code-block:: json

    {
      "segments": [{
        "segmentId": "ID of the filtered segment",
        "roadCate": "FRC number of the road on the segment",
        "speed": "current speed of the segment",
        "limit": "speed limit of the segment",
        "freeflow": "free flow speed of the segment",
        "travelTime": "estimated time to traverse the segment",
        "openLR": "openLR code of the segment",
        "agoraC": "AGORA-C code of the segment",
        "linkId": "TSD Link ID of the segment",
        "segmentCoordinates":
        {
          "point1":{
            "lat": "latitude of the point1 of the segment",
            "lon": "longitude of the point1 of the segment"       
          },
          "point2":{
            "lat": "latitude of the point2 of the segment",
            "lon": "longitude of the point2 of the segment"       
          }
        },    
        "lane": [
          {
            "laneNumber": "Lane number",
            "laneSpeed": "dedicated speed of the lane on the segment"
          }
        ],    
        "timeStamp": "timestamp of the information"
      }]
    }


Incident
---------------------
Incident 는 도로 위 이벤트 정보 즉, 사고, 재해, 공사, 행사 등의 정보를 다룹니다. 나머지 내용은 :ref:`Segment 포맷 <segmentformats>` 과 동일 합니다.

**Incident 메시지 응답**

.. code-block:: json

  {
    "incidents": [{
      "segmentId": "ID of the segment where the events are occurred",
      "incidentId": "ID of the incident",
      "incidentType": "type of the incident",
      "lane": "lane number of the event",
      "length": "length of the incident has occurred",
      "vehicleKind": "type of the vehicle",
      "description": "detail explanation of the incident",
      "schedule": {
        "isPlanned": "whether the event is planned of not",
        "startTime": "start time of the event (only if scheduled)",
        "endTime": "end time of the event (only if scheduled)",
        "reoccuring":{
          "daysOfWeek": "days when the event is re-occurred",
          "from": "start time of the upcoming event",
          "until": "end time of the upcomfing event"
        }
      },
      "openLr": "openLr code of the segment",
      "agoraC": "AGORA-C code of the segment",
      "linkId": "TSD Link ID of the segment",
      "segmentCoordinates":
      {
        "point1":{
          "lat": "latitude of the point1 of the segment",
          "lon": "longitude of the point1 of the segment"       
        },
        "point2":{
          "lat": "latitude of the point2 of the segment",
          "lon": "longitude of the point2 of the segment"       
        }
      },   
      "timeStamp": "timestamp of the information"
    }]
  }


.. _tpeg2_formats:

TPEG (TPEG ML Only)
---------------------

**TPEG ML TFP**

필드 계층 구조가 복잡 하므로 본 메시지에 대해서만 기술 합니다.

.. code-block:: xml

  <TPEGDocument> 
    <TransportFrame>
        <ServiceData>
          <SID>
              TPEG Service ID
          </SID>
          <ApplicationRootMessageML xsi:type="tfp:TFPMessage" xmlns:tfp="http://www.tisa.org/TPEG/TFP_1_0">
            <tfp:mmt>
                <tfp:optionMessageManagement>
                    <mmc:messageID>"ID"</mmc:messageID>
                    <mmc:versionID>ID</mmc:versionID>
                    <mmc:messageExpiryTime>YYYY-MM-DDTHH: MM: SSZ</mmc:messageExpiryTime>
                    <mmc:cancelFlag>boolean</mmc:cancelFlag>
                    <mmc:messageGenerationTime>YYYY-MM-DDTHH: MM: SSZ</mmc:messageGenerationTime>
                </tfp:optionMessageManagement>
            </tfp:mmt>
            <tfp:method xsi:type="tfp:FlowStatus">
                <tfp:startTime>YYYY-MM-DDTHH: MM: SSZ</tfp:startTime>
                <tfp:duration>0</tfp:duration>
                <tfp:status>
                    <tfp:LOS tfp:code="2" tfp:table="tfp003_LevelOfService"/>
                    <tfp:averageSpeed>37</tfp:averageSpeed>
                    <tfp:freeFlowTravelTime>60</tfp:freeFlowTravelTime>
                </tfp:status>
                <tfp:restriction>
                    <tfp:lanes tfp:code="0" tfp:table="tfp005_laneRestriction"/>
                </tfp:restriction>
                    <tfp:cause tfp:code="0" tfp:table="tfp006_CauseCode"/>
                <tfp:detailedCause>
                    <tfp:messageID>53694</tfp:messageID>
                    <tfp:COID>0</tfp:COID>
                </tfp:detailedCause>
            </tfp:method>
            <tfp:loc>LocationReferenceCode</tfp:loc>
          </ApplicationRootMessageML>   
        </ServiceData>
    </TransportFrame>
  </TPEGDocument>


**TPEG ML TEC**

필드 계층 구조가 복잡 하므로 본 메시지에 대해서만 기술 합니다.

.. code-block:: xml

  <TPEGDocument>
    <TransportFrame>
        <ServiceData>
          <SID>
            TPEG Service ID
          </SID>
          <ApplicationRootMessageML xsi:type="tec:TECMessage" xmlns:tec="http://www.tisa.org/TPEG/TEC_3_2">
            <tec:mmt>
              <tec:optionMessageManagement>
                <mmc:messageID>"ID"</mmc:messageID>
                <mmc:versionID>ID</mmc:versionID>
                <mmc:messageExpiryTime>YYYY-MM-DDTHH: MM: SSZ</mmc:messageExpiryTime>
                <mmc:cancelFlag>false</mmc:cancelFlag>
                <mmc:messageGenerationTime>YYYY-MM-DDTHH: MM: SSZ</mmc:messageGenerationTime>
              </tec:optionMessageManagement>
            </tec:mmt>
            <tec:event>
              <tec:effectCode tec:code="4" tec:table="tec001_EffectCode"/>
              <tec:startTime>YYYY-MM-DDTHH: MM: SSZ</tec:startTime>
              <tec:stopTime>YYYY-MM-DDTHH: MM: SSZ</tec:stopTime>
              <tec:cause>
                <tec:optionDirectCause>
                  <tec:mainCause tec:code="2" tec:table="tec002_CauseCode"/>
                  <tec:warningLevel tec:code="0" tec:table="tec003_WarningLevel"/>
                  <tec:unverifiedInformation>boolean</tec:unverifiedInformation>
                  <tec:subCause tec:code="7" tec:table="tec100_SubCause"/>
                  <tec:laneRestrictionType tec:code="0" tec:table="tec004_LaneRestriction"/>
                  <tec:freeText>
                    <tdt:languageCode tdt:code="85" tdt:table="typ001_LanguageCode"/>
                    <tdt:value><서비스제공자>이벤트 세부 명세</tdt:value>
                  </tec:freeText>
                </tec:optionDirectCause>
              </tec:cause>
            </tec:event>
            <tec:loc>LocationReferenceCode</tec:loc>
          </ApplicationRootMessageML>                        
        </ServiceData>
    </TransportFrame>
  </TPEGDocument>

**TPEG ML WEA**

필드 계층 구조가 복잡 하므로 본 메시지에 대해서만 기술 합니다.

.. code-block:: xml

  <TPEGDocument>
    <TransportFrame>
        <ServiceData>
            <SID>
                TPEG Service ID
            </SID>            
            <ApplicationRootMessageML xsi:type="wea:WeatherMessage" xmlns:wea="http://www.tisa.org/TPEG/WEA_1_1">
                <wea:mmt>
                    <wea:optionMessageManagementContainerLink>
                        <mmc:messageID>"ID"</mmc:messageID>
		                <mmc:versionID>ID</mmc:versionID>
		                <mmc:messageExpiryTime>YYYY-MM-DDTHH: MM: SSZ</mmc:messageExpiryTime>
		                <mmc:cancelFlag>false</mmc:cancelFlag>
		                <mmc:messageGenerationTime>YYYY-MM-DDTHH: MM: SSZ</mmc:messageGenerationTime>
                    </wea:optionMessageManagementContainerLink>
                </wea:mmt>
                <wea:weatherInfo>
                    <wea:geographicalSignificance wea:code="6" wea:table="wea011_GeoSignificance"/>
                    <wea:weatherReport>
                        <wea:reportType wea:code="4" wea:table="wea000_ReportType"/>
                        <wea:weatherDefinition>
                            <wea:period wea:code="0" wea:table="wea001_Period"/>
                            <wea:weatherDescription>
                                <wea:subTableType wea:code="108" wea:table="wea100_ElementType"/>
                                <wea:subTableValue wea:code="7" wea:table="wea099_ElementSubTable"/>
                            </wea:weatherDescription>
                            <wea:start>YYYY-MM-DDTHH: MM: SSZ</wea:start>
                            <wea:stop>YYYY-MM-DDTHH: MM: SSZ</wea:stop>
                            <wea:date>YYYY-MM-DDTHH: MM: SSZ</wea:date>
                            <wea:statistics> 
                                <wea:cloudCover>Coverage of cloud</wea:cloudCover>
                                <wea:pressure>Air pressure</wea:pressure>
                                <wea:temp>Temperature (Celcius)</wea:temp>
                                <wea:tempMax>Max temperature (Celcius)</wea:tempMax>
                                <wea:tempMin>Min temperature (Celcius)</wea:tempMin>
                                <wea:windDirection wea:code="2" wea:table="wea003_Direction"/>
                                <wea:windSpeed>Wind speed</wea:windSpeed>
                                <wea:relativeHumidity>Humidity</wea:relativeHumidity>
                                <wea:sunrise>YYYY-MM-DDTHH: MM: SSZ</wea:sunrise>
                                <wea:sunset>YYYY-MM-DDTHH: MM: SSZ</wea:sunset>
                            </wea:statistics>
                        </wea:weatherDefinition>
                    </wea:weatherReport>
                </wea:weatherInfo>
                <wea:loc>LocationReferenceCode</wea:loc>
            </ApplicationRootMessageML>                        
        </ServiceData>
    </TransportFrame>
  </TPEGDocument>ß
