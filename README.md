**SK ID Solutions AS Public Monitoring and Statistics Interface Specification**

---
> **_NOTE:_**  
> Our pulic monitoring interface is going through some changes!

>As you may have noticed, http://www.sk.ee/util/public_monitoring/ is currently redirected to http://62.65.42.45/util/public_monitoring/.

>This is due to changes in our web servers and hosting. 

>After 15.06.2019 both of these addresses will be depricated and no longer in use.
>The new address will be http://status.sk.ee/v1/.

>We will update this document once the new address is functional.
---

| **Version** | **Date** | **Author** | **Notes** |
| --- | --- | --- | --- |
| 1.0.1 | 31.03.2014 | Alvar Nõmmik | First version of the document. |
| 1.0.2 | 02.04.2014 | Alvar Nõmmik | Added description of monitoring logic, minor fixes |
| 1.0.3 | 11.07.2014 | Alvar Nõmmik | Added check\_rc\_getmnoid to Bite and Tele2 LT Added check\_dds2\_mssp\_bite\_rc to BiteExplanations about hard/softstate |
| 1.0.4 | 29.07.2014 | Alvar Nõmmik | Added information about SK OCSP check, minor fixes |
| 1.0.5 | 09.10.2014 | Alvar Nõmmik, Urmo Keskel | Monitoring logic changes  see p .1.2, new state UNKNOWN added Change of plugin output pattern, new format "Failure rate zz%, Failed xx of yy"Updated the sample of json and XML output |
| 1.0.7 | 02.04.2015 | Alvar Nõmmik | Added check for SMSC status (Tele2EE and EMT) - check\_\*\_smscExamples updated and added CRITICAL scenario |
| 1.0.8 | 05.06.2017 | Alvar Nõmmik | check\_dds2\_mssp\_elisa (Legacy Elisa mssp check) - removedcheck\_rc\_getmnoid – removedomnitel\_sk business process - removedMinor fixes in wording check\_\*\_smsc – check described |
| 1.0.9 | 24.04.2018 | Kristjan Koskor | Converted to .md format. <br /> Published on github. <br />Minor formatting fixes|
| 1.0.10 | 26.04.2018 | Kristjan Koskor |Added LT-SK business process identifiers for Telia, Tele2 and Bite. |
| 1.0.11 | 26.04.2018| Alvar Nõmmik | Documentation formatting changed |
| 1.0.12 | 07.05.2018| Kristjan Koskor | Added Smart-ID description of Smart-ID monitoring |
| 1.0.13 | 06.07.2018 | Kristjan Koskor |Removed business process identifiers for _bite_ and _omnitel_rc_. |
| 1.0.14 | 30.07.2018 | Kristjan Koskor |Added TSA specifications. Adjusted monitoring logic parameters mobile-ID. |
| 1.0.15 | 06.11.2018 | Kalle Keskrand | Added Trust Services specifications. |
| 1.0.16 | 06.11.2018 | Kristjan Koskor | Added general statistics interface specification|
| 1.0.17 | 07.11.2018 | Kristjan Koskor | Added OCSP services |
| 1.0.18 | 03.04.2019 | Kristjan Koskor | Updated Business process subcomponents. Link status info is temporarily unavailable.|
| 1.0.19 | 16.05.2019 | Kristjan Koskor | Added json_created to outputs where it was still missing.|
| 1.0.20 | 16.05.2019 | Kristjan Koskor | Updated json output url path to _status.sk.ee/*_ .|

# Table of Contents
* [1. Mobile-ID](#1-mobile-id)
    * [1.1. Structure](#11-structure)
    * [1.2. Description of monitoring logic and failure rates of different components](#12-description-of-monitoring-logic-and-failure-rates-of-different-components)
    * [1.3. Time windows of monitored data](#13-time-windows-of-monitored-data)
    * [1.4. Hard and soft state](#14-hard-and-soft-state)
* [2. Examples](#2-examples)
    * [2.1. Example json output](#21-example-json-output)
    * [2.2. Example XML output](#22-example-xml-output)
* [3. Smart-ID](#3-smart-id)
    * [3.1 Structure](#31-structure)
    * [3.2 Example-xml-output](#32-example-xml-output)
* [4. TSA](#4-tsa)
    * [4.1 Structure](#41-structure)
    * [4.2 Example-xml-output](#42-example-json-output)
* [5. Trust Services](#5-trust-services)
    * [5.1 Structure](#51-structure)
    * [5.2 Example-json-output](#52-example-json-output)
* [6. General statistics interface](#6-general-statistics-interface)
    * [6.1 Descriptions of data objects](#61-descriptions-of-data-objects)
* [7. OCSP](#7-ocsp)
    * [7.1 Structure](#71-structure)
    * [7.2 Example-json-output](#72-example-json-output)
* [8 AIA OCSP](#8-aia-ocsp)
    * [8.1 Structure](#81-structure)
    * [8.2 Example-json-output](#82-example-json-output)
* [9 PROXY OCSP](#9-proxy-ocsp)
    * [9.1 Structure](#91-structure)
    * [9.2 Example-json-output](#92-example-json-output)
* [10 PROXY OCSP DETAILS](#10-proxy-ocsp-details)
    * [10.1 Structure](#101-structure)
    * [10.2 Example-json-output](#102-example-json-output)    

# 1. Mobile-ID

SK public monitoring interface provides JSON and XML based information about availability of Mobile-ID service to mobile operator clients.

Interface generates ".json" files (from Nagios Business Processes interface) and xml.php - provides same output in XML format (generated from .json files). Monitoring data is generated every minute.

Location of files:

- JSON: [https://status.sk.ee/v1/operator_identifier.json](https://status.sk.ee/v1/operator_identifier.json)
- XML: [sk.ee/util/public_monitoring/xml.php?id=operator_identifier](http://www.sk.ee/util/public_monitoring/xml.php?id=operator_identifier)
>NOTE: XML output will be depricated on 15.06.2019 and will no longer be available.

Where "operator_identifier" is the _identifier_ of the business process.

List of business processes:

| **Identifier** | **Description** |
| --- | --- |
| [_emt_](https://status.sk.ee/v1/emt.json) | Mobile-ID service for Telia (EE) customers |
| [_elisa_](https://status.sk.ee/v1/elisa.json) | Mobile-ID service for Elisa (EE) customers |
| [_tele2ee_](https://status.sk.ee/v1/tele2ee.json) | Mobile-ID service for Tele2 (EE) customers |
| [_tele2lt_](https://status.sk.ee/v1/tele2lt.json) | Mobile-ID service for Tele2 (LT) customers |
| [_telia_sk_lt_](https://status.sk.ee/v1/telia_sk_lt.json) | Mobile-ID service for (LT) Telia customers |
| [_tele2_sk_lt_](https://status.sk.ee/v1/tele2_sk_lt.json) | Mobile-ID service for (LT) Tele2 customers |
| [_bite_sk_lt_](https://status.sk.ee/v1/bite_sk_lt.json) | Mobile-ID service for (LT) Bite customers |
| [_smart_](https://status.sk.ee/v1/smart.json) | Smart-ID Authentication and Signing transactions |
| [_tsa_](https://status.sk.ee/v1/tsa.json) | Time-Stamping Authority statistics |
| [_ocsp_](https://status.sk.ee/v1/ocsp.json) | OCSP Statistics |
| [_ocsp_aia_](https://status.sk.ee/v1/ocsp_aia.json) | AIA OCSP Statistics |
| [_ocsp_proxy_](https://status.sk.ee/v1/ocsp_proxy.json) | Proxy OCSP Statistics |
| [_ocsp_proxy_detail_](https://status.sk.ee/v1/ocsp_proxy_detail.json) | Proxy OCSP Statistics by CA|



## 1.1. Structure

**business_process**

Business process block – contains information about sub list

**hardstate**

Status of business process (if one sub component is not operational then entire business process is failing)

**components**

Business process sub components

| **hardstate** | Status of sub-process. (etc. OCSP service) Changes when _n_ number of checks failed.<br>**States:**<br>OK – component is operational<br>WARNING – component is operational; some of the requests are failing.<br>CRITICAL – component is not working correctly<br>UNKNOWN – Some of requests are failing, but the amount of requests is too low to decide is the component operational or not. |
| --- | --- |
| **plugin\_output** | Nagios plugin output. <br>Usually contains detailed information about service status: <br>"SERVICE\_NAME Failure rate: x%, Failed yy of zz""OK" – Unable to read failure rate information from monitoring plugin. |
| **service** | Name of the service: <br>**check\_dds2\_mssp\_\*** - MSSP (Mobile Signature Service Provider) checks <br>**check\_dds\_certstore\_\*** - External certificate store checks <br>**check\_ocsp\_\*** – OCSP checks <br>**check\_\*\_smsc** – Status of SMSC<br>queue = number of SMS-s in queue<br>~~link = link status (1 – link up; 0 – link down)~~-(link status info is temporarily unavailable as of 03.04.2019) <br>plugin output example: (tele2 OK: queue=0, link=1") |

**bp_id**

ID of Business process (short identifier)

**display_name**

Name of Business process

**json_created**

JSON report generation time

## 1.2. Description of monitoring logic and failure rates of different components


- External certificate store monitoring (service syntax: check_dds_certstore_)
- Mobile operator monitoring (service syntax: check_dds2_mssp_)
- External OCSP monitoring (service syntax: check_ocsp_)

|   | **WARNING** | **CRITICAL** | **UNKNOWN** |
| --- | --- | --- | --- |
| **Last 5 min** | - | all queries failed | - |
| **Last 30 min** | at least 100 queries and 25% queries failed | at least 100 queries and 40% queries failed | less than 100 queries and at least 1 failed |
- 1 check to change the state, check interval 4 min



- **SK OCSP monitoring (service: check_ocsp_ocsp.sk.ee)**

|   | **WARNING** | **CRITICAL** | **UNKNOWN** |
| --- | --- | --- | --- |
|   | - | OCSP query failed | - |

- 2 checks to change state, check interval 3 min

## 1.3. Time windows of monitored data 


- Mobile operator monitoring displays the results for set amount of time. (see table below) 
- The smaller the window the more sensitive the failure rate will be.
- In some cases the window may be longer to compensate for smaller transaction volume.

- Mobile operator monitoring (service syntax: check_dds2_mssp_)


| **Identifier** | **Window of time** |
| --- | --- |
| _emt_ | 5 min. |
| _elisa_ | 5 min. |
| _tele2ee_ | 5 min. |
| _omnitel_rc_ | 5 min. |
| _tele2lt_ | 5 min. |
| _bite_ | 5 min. |
| _telia_sk_lt_ | 5 min. |
| _tele2_sk_lt_ | 5 min. |
| _bite_sk_lt_ | 5 min. |


## 1.4. Hard and soft state

In order to prevent false alarms from transient problems, Nagios allows to define how many times a service or host should be (re)checked before considered to have a "real" problem.

plugin_output – is changing when soft state changed.

Actual alert is issued when "hardstate" is changing to alarm state (there has to be x amount of checks to confirm that change really happened).

Currently multiple check logic is used only in check_ocsp_ocsp.sk.ee service. Therefore in this component hardstate and plugin output may show different statuses

More information: https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/statetypes.html

# 2. Examples
## 2.1. Example json output

```
    {
    "business_process" : {
      "hardstate" : "OK",
      "components" : [
         {
            "hardstate" : "OK",
            "plugin_output" : "OK Failure rate: 0%, Failed 0 of 40",
            "service" : "check_dds2_mssp_bite_rc"
         },
         {
            "hardstate" : "OK",
            "plugin_output" : "RC_WPKITSP_STORE Failure rate: 0%, Failed 0 of 40",
            "service" : "check_dds_certstore_rc_wpkitsp_store"
         },
         {
            "hardstate" : "OK",
            "plugin_output" : "OK",
            "service" : "check_ocsp_ocsp.rcsc.lt"
         }
      ],
      "bp_id" : "bite",
      "display_name" : "BITE business process"
    },
    "json_created" : "2017-05-20 13:37:01"
    }
```
## 2.2. Example XML output

```
    <public_monitoring>
        <business_process>
                <hardstate>OK</hardstate>
                <components>
                        <hardstate>OK</hardstate>
                        <plugin_output>OK Failure rate: 0%, Failed 0 of 40</plugin_output>
                        <service>check_dds2_mssp_bite_rc</service>
                </components>
                <components>
                        <hardstate>OK</hardstate>
                        <plugin_output>RC_WPKITSP_STORE Failure rate: 0%, Failed 0 of 40</plugin_output>
                        <service>check_dds_certstore_rc_wpkitsp_store</service>
                </components>
                <components>
                        <hardstate>OK</hardstate>
                        <plugin_output>OK</plugin_output>
                        <service>check_ocsp_ocsp.rcsc.lt</service>
                </components>
                <bp_id>bite</bp_id>
                <display_name>BITE business process</display_name>
        </business_process>
        <json_created>2017-05-20 13:37:01</json_created>
    </public_monitoring>
```

# 3. Smart-ID

## 3.1 Structure
|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| _public_monitoring_ | tag | Root element |
| _itemN_ | tag | child element where N is a row number |
| Use | _str_ | Use case / type of transaction("Authentication" or "Signing") |
| Total | _int_ | Total number of transactions for a particular use case |
| Success | _int_ | Number of successfully completed transactions of particular use case |
| Failed | _int_ | Number of failed transactions of a particular use case <br />* **NB!**  This count also includes end user error cases<br /> such as when the user cancels a transaction; <br />when the user fails to respond in time; <br />when the users PIN is blocked etc. *
|Failrate |_float_|Percentage of failed transactions of the total number of transactions of a particular use case|
|Status|_str_|**State of service:** <br /> "OK" = failrate is lower than 5% <br />"WARN" = failrate is greater than 5%<br />"CRITICAL" = failrate is greater than 75%|
| json_created | _date/time_ | Date and time when the output was generated |


## 3.2. Example xml output

```
	<public_monitoring>
      <item0>
            <Use>Authentication</Use>
          <Total>1887</Total>
          <Success>1837</Success>
          <Failed>50</Failed>
          <Failrate>2.649709</Failrate>
          <Status>OK</Status>
      </item0>
      <item1>
          <Use>Signing</Use>
          <Total>1543</Total>
          <Success>1499</Success>
          <Failed>44</Failed>
          <Failrate>2.851588</Failrate>
         <Status>OK</Status>
      </item1>
	</public_monitoring>
```

# 4. TSA 
SK’s Time-Stamping Authority public monitoring interface is mainly useful for statistical purposes. It displays the number of requests to the TSA in the past 5 minutes, the time of the latest successful response and the average response time of the TSA.
The statistics are updated every 5 minutes.

## 4.1 Structure

|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| "req_in_5min_ | int | Number of request in the past 5 minutes. |
| latest_OK | datetime | Date and time of the latest succesful response (at the time of json generation) |
| avg_response_ms | float | Average response time over the past 5 minutes. |
| json_created | _date/time_ | Date and time when the output was generated |

## 4.2. Example json output
```
[
  {
    "req_in_5min": "1214", 
    "latest_OK": "10/30/2018 14:50:02", 
    "avg_response_ms": "45.3",
    "json_created": "05/16/2019 15:55:11"
  }
]
```

# 5. Trust Services
The public monitoring of trust services interface provides JSON based information about availability of Mobile-ID and Smart-ID issuance services and CRL validity of critical CAs.</br>
Interface generates ".json" file, using Zabbix API. Generating interval is 10 min.</br>
Location of json file: https://www.sk.ee/util/public_monitoring/trust_srv.json

## 5.1 Structure
|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| status | str | Status can be "OK" or "DOWN". </br>This indicator contains different servers and components according to the current service. Status is "OK" when all necessary servers are up and running and critical services respond for test queries. |
| valid_hours | int | Key for CLR-s. Shows number of hours while CRL is valid. |
| CRL |  | Monitor of CRL validity for next CAs: </br>EECCRCA - root CA, validity 3 months; </br>EID2011, ESTEID2011, ESTEID2015, KLASS3-2010 - validity 12 hours |
| MobileIDIssuance | | Status of Mobile-ID issuance service. |
| Smart-IDIssuanceOnline | | Status of Smart-ID online issuance service. |
| Smart-IDIssuanceRA | | Status of Smart-ID issuance service for bank offices. |
| SmartIDs_issued_in_last_60_min | | Shows the number of Smart-IDs issued in last 60 min. |
| issued | int | Number of issued Smart-IDs. |
| last_issuance | datetime | Time of last Smart-ID issuance. |
| json_created | datetime | Time of creation for current json. </br>NB! This json cannot be older than 10 min, otherwise monitor itself is down. |

## 5.2. Example json output
```
{
    "CRL": {
        "EECCRCA": {
            "status": "OK",
            "valid_hours": "482"
        },
        "EID2011": {
            "status": "OK",
            "valid_hours": "11"
        },
        "ESTEID2011": {
            "status": "OK",
            "valid_hours": "8"
        },
        "ESTEID2015": {
            "status": "OK",
            "valid_hours": "11"
        },
        "KLASS3-2010": {
            "status": "OK",
            "valid_hours": "7"
        },
        "status": "OK"
    },
    "MobileIDIssuance": {
        "status": "OK"
    },
    "Smart-IDIssuanceOnline": {
        "status": "OK"
    },
    "Smart-IDIssuanceRA": {
        "status": "OK"
    },
    "SmartIDs_issued_in_last_60_min": {
        "info": {
            "issued": "614",
            "last_issuance": "2018-11-02 11:07:14"
        }
    },
    "json_created": "2018-11-02 11:10:02"
}
```

# 6. General statistics interface
Where ever needed, you can use these numbers to illustrate your presentations, analyses, website. 
Data is available at  https://minutoimingud.sk.ee/cards.json
Data is generated once a day and includes the data until 23:59 for the previous day.

## 6.1 Descriptions of data objects
|  **Key** | **Description** |
| --- | --- |
| activeSID_LV | Number of active Smart-ID unique users in Latvia |
| last_month | Number of validity confirmations provided for certificates issued by SK through ocsp.sk.ee service for previous calendar month. |
| yesterday_SID | Number of Smart-ID transactions during a day before data was generated |
| activeIDCards | Number of active Estonian ID-cards |
| Signatures | Number of validity confirmations provided for signing certificates issued by SK through ocsp.sk.ee service for the period from 2002 until the day before data was generated. This number must not be interpreted as signatures. |
| Authentications | Number of validity confirmations provided for authentication certificates issued by SK through ocsp.sk.ee service for the period from 2002 until the day before data was generated. This number must not be interpreted as authentications. |
| Yesterday | Number of validity confirmations provided for certificates issued by SK through ocsp.sk.ee service during a day before data was generated. |
| activeMID | Number of active Mobile-ID unique users in Estonia |
| last_month_SID | Number of Smart-ID transactions during previous calendar month | 
| activeSID_EE | Number of active Smart-ID unique users in Estonia | 
| activeSID | Number of active Smart-ID unique users in total (over all countries) | 
| activeSID_LT | Number of active Smart-ID unique users in Lithuania | 
| reportExecuted | Date and time when data was generated |

## 7. OCSP
### 7.1 Structure
|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| req_in_5min | int | Number of requests in the past 5 minutes |
| latest_OK | datetime | Date and time of the last RFC2560 'good' response |
| avg_response_ms|float | Avaerage response time in the past 5 minutes |
| json_created | _date/time_ | Date and time when the output was generated |

### 7.2 Example json output
```
[
  {
    "req_in_5min": "4687",
    "latest_OK": "11/07/2018 20:38:00.143624",
    "avg_response_ms": "6.8",
     "json_created": "05/16/2019 15:55:11"
  }
]
```

### 8 AIA OCSP
### 8.1 Structure
|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| req_in_5min | int | Number of requests in the past 5 minutes |
| latest_OK | datetime | Date and time of the last RFC2560 'good' response |
| avg_response_ms | float | Avaerage response time in the past 5 minutes |
| json_created | _date/time_ | Date and time when the output was generated |

### 8.2 Example json output
```
[
  {
    "req_in_5min": "953",
    "latest_OK": "11/07/2018 20:39:00.015364",
    "avg_response_ms": "5.7",
     "json_created": "05/16/2019 15:55:11"
  }
]
```

### 9 PROXY OCSP 
### 9.1 Structure
|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| req_in_5min | int | Number of requests in the past 5 minutes |
| latest_OK | datetime | Date and time of the last RFC2560 'good' response |
| avg_response_ms | float | Avaerage response time in the past 5 minutes |
| json_created | _date/time_ | Date and time when the output was generated |

### 9.2 Example json output
```
[
  {
    "req_in_5min": "122",
    "latest_OK": "11/07/2018 20:39:25.215708",
    "avg_response_ms": "50.9",
     "json_created": "05/16/2019 15:55:11"
  }
]
```

### 10 PROXY OCSP Details
This interface provides statistics on every proxied CA.
Statistics wil be displayed for every CA that has responded in the past 5 minutes in a separate block.
If a particular CA has not responded in the past 5 minutes - its block will be omitted.

### 10.1 Structure
|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| responder | text | Name of the responding CA |
| resp_in_5min | int | Number of responses in the past 5 minutes from the particular CA |
| latest_OK | datetime | Date and time of the last RFC2560 'good' response |
| avg_response_ms |float | Avaerage response time in the past 5 minutes |
| json_created | _date/time_ | Date and time when the output was generated |

### 10.2 Example json output
```
[
  {
    "responder": "ocsp2.rcsc.lt",
    "resp_in_5min": "122",
    "latest_OK": "11/07/2018 20:39:25.215708",
    "avg_response_ms": "50.9",
    "json_created": "05/16/2019 15:55:11"
  }
]
```
