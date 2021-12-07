**SK ID Solutions AS Public Monitoring and Statistics Interface Specification**

---
> **_NOTICE:_**  

> Since DigiDocService is discontinued as of 01.10.2020 and most e-service providers have migrated to using the MID-REST API
> the older monitoring public monitoring output of https://status.sk.ee/v1/sk-dds-mid.json no longer reflects relevant information. 
> Please refer to https://status.sk.ee/v1/mid-rest_live.json for up-to-date stats.

---
>SK's Public Monitoring and Statistics Interface is provided "AS IS", without warranty of any kind
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
| 1.0.21 | 21.05.2019 | Kristjan Koskor | Add MID REST|
| 1.0.22 | 09.10.2019 | Kristjan Koskor | Update info about tele2lt. Fixed some typos|
| 1.0.23 | 28.01.2020 | Kristjan Koskor | Add 'activeMID_LT' data object|
| 1.0.24 | 28.01.2020 | Kristjan Koskor | Add sk-dds-mid.json file. Remove depricated Mobile-ID monitoring|
| 1.0.24 | 30.04.2020 | Kristjan Koskor | Changed Smart-ID monitoring specification|
| 1.0.25 | 10.09.2020 | Kristjan Koskor | Changed MID-REST monitoring is out of beta specification |
| 1.0.26 | 09.07.2021 | Kristjan Koskor | Changed 'cards.json' URL |
| 1.0.27 | 16.11.2021 | Jevgeni Losak | Added Time Stamping Authority |

# Table of Contents
* [1. Mobile-ID](#1-mobile-id)
    * [1.1. Structure](#11-structure)
    * [1.2. Description of monitoring logic and failure rates of different components](#12-description-of-monitoring-logic-and-failure-rates-of-different-components)
    * [1.3. Example json output](#13-example-json-output)
* [2. Smart-ID](#2-smart-id)
    * [2.1 Structure](#21-structure)
    * [2.2 Example-json-output](#22-example-json-output)
* [3. Time Stamping Authority Stats](#3-time-stamping-authority-stats)
    * [3.1 Structure](#31-structure)
    * [3.2 Example-json-output](#32-example-json-output)
* [4. Time Stamping Authority Status](#4-time-stamping-authority-status)
    * [4.1 Structure](#41-structure)
    * [4.2 Example-json-output](#42-example-json-output)
* [5. Trust Services](#5-trust-services)
    * [5.1 Structure](#51-structure)
    * [5.2 Example-json-output](#52-example-json-output)
* [6. General statistics interface](#6-general-statistics-interface)
    * [6.1 Descriptions of data objects](#61-descriptions-of-data-objects)
* [7. OCSP](#7-ocsp)
    * [7.1 Structure](#71-structure)
    * [7.2 Example-json-output](#72-example-json-output)
* [8. AIA OCSP](#8-aia-ocsp)
    * [8.1 Structure](#81-structure)
    * [8.2 Example-json-output](#82-example-json-output)
* [9. PROXY OCSP](#9-proxy-ocsp)
    * [9.1 Structure](#91-structure)
    * [9.2 Example-json-output](#92-example-json-output)
* [10. PROXY OCSP DETAILS](#10-proxy-ocsp-details)
    * [10.1 Structure](#101-structure)
    * [10.2 Example-json-output](#102-example-json-output)    
* [11. MID REST](#11-mid-rest)
    * [11.1 Structure](#111-structure)
    * [11.2 Example-json-output](#112-example-json-output)    

List of interfaces:

| **Identifier** | **Description** |
| --- | --- |
| [_mobile-id_](https://status.sk.ee/v1/sk-dds-mid.json) | Combined Mobile-ID service monitoring for all Mobile network operators |
| [_smart_](https://status.sk.ee/v1/smart.json) | Smart-ID Authentication and Signing transactions |
| [_tsa_](https://status.sk.ee/v1/tsa.json) | Time-Stamping Authority statistics |
| [_ocsp_](https://status.sk.ee/v1/ocsp.json) | OCSP Statistics |
| [_ocsp_aia_](https://status.sk.ee/v1/ocsp_aia.json) | AIA OCSP Statistics |
| [_ocsp_proxy_](https://status.sk.ee/v1/ocsp_proxy.json) | Proxy OCSP Statistics |
| [_ocsp_proxy_detail_](https://status.sk.ee/v1/ocsp_proxy_detail.json) | Proxy OCSP Statistics by CA|


# 1. Mobile-ID

SK public monitoring interface provides JSON based information about availability of Mobile-ID service to mobile operator clients.
This interface displays information about Mobile-ID transactions that use DigiDoc Service. If you're interested in MID-REST interface numbers, pleas see [11 MID REST](#11-mid-rest)

Interface generates ".json" files.
Monitoring data is generated every 5 minutes.

Location of file:

- JSON: [https://status.sk.ee/v1/sk-dds-mid.json](https://status.sk.ee/v1/sk-dds-mid.json)

>NOTE: Teledema customers has insuficient data to be properly monitored. Hence, the output will only display hardstates of either "OK" or "UNKNOWN".

## 1.1. Structure

 **Operator**
 
 name of Mobile network operator 
 > operators with *-SK-* in their name are using SK's OTA service. 
 
 **Status**
 
 OK, WARNING, CRITICAL, UNKNOWN
 
 **Failrate**
 
 percentage of failed Mobile-ID authentication or signing requests in the last 5 full minutes
 
 **Total**
 
 total number of authentication or signing requests in the last 5 full minutes
 
 **Failed**
 
number of failed authentication or signing requests in the last 5 full minutes

**json_created**

JSON data generation time

## 1.2. Description of monitoring logic and failure rates of different components

**OK**

less than 10% of authentication or signing requests have failed in the last full 5 minutes

**WARNING**

more than 10% of authentication or signing requests have failed in the last full 5 minutes

**CRITICAL**

100% of authentication or signing requests have failed in the last full 5 minutes

**UNKNOWN** 

less than 100 authentication or signing requests in the last full 5 minutes
insuficient data to evaluate status

## 1.3. Example json output

```
  [
  {
    "Status": "OK", 
    "Failrate": "6.134969325153374", 
    "Failed": "10", 
    "json_created": "03/09/2020 18:30:23", 
    "Operator": "BITE-SK-LT", 
    "Total": "163"
  }, 
  ...
]
```

# 2. Smart-ID

>Smart-ID will switch to the described format on Monday, 04.05.2020
>
>We'll be changing the methodology behind Smart-ID public monitoring. 
>We now account for requests sooner in the service pipeline and also consider more failure states.
>
>Transactions are considered as succesful if completed as expected OR where the user refused the transaction on their app OR when a user entered the wrong verification code.
>This will also highlight potential issues with the service in more contrast.
>Due to this, you can expect a change in the failure rate numbers of the ouput. But Don't worry, the service itself has not been changed and is operating just as before.

- JSON: [https://status.sk.ee/v1/smart.json](https://status.sk.ee/v1/smart.json)

## 2.1 Structure
|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| _Status_ | str | **State of service:** <br /> "OK" = failrate is lower than 20% <br />"WARN" = failrate is greater than 15%<br />"CRITICAL" = failrate is greater than 75% <br /> |
| Use | _str_ | Use case ("Authentication" or "Signing") |
| Started | _int_ | Number of started request for a particular use case |
| Failrate |_float_ | Percentage of failed transactions of the total number of transactions of particular use case |
| Finished | _int_ | Number of successfully finished transactions of a particular use case <br />  including user timeouts, PIN validation issues or clone detection. <br /> All other results are considered as failed requests.|
| json_created | _date/time_ | Date and time when the output was generated |

## 2.2. Example json output
```
  [
  {
    "Status": "OK",
    "Use": "Authentication",
    "Started": "10125",
    "Failrate": "6.7",
    "Finished": "9444",
    "json_created": "04/30/2020 14:19:13"
  },
  {
    "Status": "OK",
    "Use": "Signing",
    "Started": "3631",
    "Failrate": "1.9",
    "Finished": "3563",
    "json_created": "04/30/2020 14:19:13"
  }
]
```

# 3. Time Stamping Authority Stats
SK’s Time-Stamping Authority public monitoring interface is mainly useful for statistical purposes. It displays the number of requests to the TSA in the past 5 minutes, the time of the latest successful response and the average response time of the TSA.
The statistics are updated every 5 minutes.

- JSON: [https://status.sk.ee/v1/tsa.json](https://status.sk.ee/v1/tsa.json)

## 3.1 Structure

|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| req_in_5min | int | Number of request in the past 5 minutes. |
| latest_OK | datetime | Date and time of the latest succesful response (at the time of json generation) |
| avg_response_ms | float | Average response time over the past 5 minutes. |
| json_created | _date/time_ | Date and time when the output was generated |

## 3.2. Example json output
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

# 4. Time Stamping Authority Status
SK’s Time-Stamping Authority public monitoring interface is useful for statistical purposes and for service status check. It displays the number of successful requests to the TSA in the past 5 minutes, the time of the latest successful response and the average response time of the TSA.
The statistics are updated every 5 minutes.

- JSON: [https://status.sk.ee/v1/tsa-stat.json](https://status.sk.ee/v1/tsa-stat.json)

## 4.1 Structure

|  **Key** | **Type** | **Description** |
| --- | --- | --- |
| Status | str | General status of the service | "OK" = More or equal to 2000 <br /> "WARNING" = Less or equal to 2000 <br /> "CRITICAL" = Less or equal to 1000  <br /> "DOWN = Less or equal to 500" | 
| req_in_5min | int | Number of request in the past 5 minutes. |
| OK_count | int | Number of successful request in the past 5 minutes. |
| latest_OK | datetime | Date and time of the latest succesful response (at the time of json generation) |
| avg_response_ms | float | Average response time over the past 5 minutes. |
| json_created | _date/time_ | Date and time when the output was generated |

## 4.2 Example json output
```
[
  {
    "status": "OK", 
    "OK_count": "7402", 
    "avg_response_ms": "39.7", 
    "json_created": "11/16/2021 10:10:53", 
    "req_in_5min": "6926", 
    "latest_OK": "11/16/2021 10:10:51"
  }
]
```

# 5. Trust Services
The public monitoring of trust services interface provides JSON based information about availability of Mobile-ID and Smart-ID issuance services and CRL validity of critical CAs.<br>
Interface generates ".json" file, using Zabbix API. Generating interval is 10 min.<br>
Location of json file: https://status.sk.ee/v1/trust_srv.json<br>
Location of test json file: https://status.sk.ee/v1/trust_srv_test.json

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

## 5.2 Example json output
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
Data is available at  https://status.sk.ee/v1/cards.json
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
| activeMID_LT | Number of active Mobile-ID unique users in Lithuania |
| last_month_SID | Number of Smart-ID transactions during previous calendar month | 
| activeSID_EE | Number of active Smart-ID unique users in Estonia | 
| activeSID | Number of active Smart-ID unique users in total (over all countries) | 
| activeSID_LT | Number of active Smart-ID unique users in Lithuania | 
| reportExecuted | Date and time when data was generated |

## 7. OCSP

- JSON: [https://status.sk.ee/v1/tsa.json](https://status.sk.ee/v1/ocsp.json)

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

### 8. AIA OCSP

- JSON: [https://status.sk.ee/v1/ocsp_aia.json](https://status.sk.ee/v1/ocsp_aia.json)

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

### 9. PROXY OCSP 

- JSON: [https://status.sk.ee/v1/ocsp_proxy.json](https://status.sk.ee/v1/ocsp_proxy.json)

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

### 10. PROXY OCSP Details

- JSON: [https://status.sk.ee/v1/ocsp_proxy_detail.json](https://status.sk.ee/v1/ocsp_proxy_detail.json)

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

### 11. MID REST

Location: https://status.sk.ee/v1/mid-rest_live.json </br>
Data is refreshed every 5 minutes. </br>
The results for full 15 minutes, 2 minutes from now(), are counted. (earliest=-17m@m latest=-2m@m) </br>

### 11.1 Structure
|**Key** | **Type** | **Description** | **Possible values** |
| --- | --- | --- | --- |
| Status | str | General status of the service | "OK" = Failrate is lower than 10% <br /> "WARNING" = Failrate is greater than 11% <br /> "CRITICAL" = Failrate is greater than 50% <br /> "UNKNOWN = There are less than 100 Total requests" | 
| Failrate | int | Failure rate percentage of total requests | 0-100 |
| Failed | int | Number of failed requests | "USER_CANCELLED" - User cancelled the operation </br> "DELIVERY_ERROR" - error sending SMS </br> "NOT_MID_CLIENT" - Given user has no active certificates and is not Mobile-ID client </br> "PHONE_ABSENT" - SIM not available </br> "TIMEOUT" - There was a timeout, i.e. end user did not confirm or refuse the operation within a given time </br> "OK" - everything went fine </br> "SIGNATURE_HASH_MISMATCH" - Mobile-ID configuration on user's SIM card differs from what is configured on service provider's side. (User needs to contact his/her mobile operator.) </br> "null" - may be displayed in rare cases but technically counts as a timeout. </br> |
| json_created | date/time | Date and time when the output was generated | "mm/dd/yyy hh:mm:ss" |
| Operator | str | Name of Mobile Network Operator | "BITE-SK-LT" <br /> "ELISA-SK-EE" <br /> "EMT-SK-EE" <br /> "TELE2-SK-EE" <br /> "TELEDEMA-LT" <br /> "TELIA-SK-LT" |
| Total | int | Total number of Mobile-ID requests for a given Operator | "<n>" |


### 11.2 Example json output
```
[
  {
    "Status": "OK", 
    "Failrate": "7.61", 
    "Failed": "21", 
    "json_created": "09/10/2020 12:30:19", 
    "Operator": "'BITE-SK-LT'", 
    "Total": "276"
  }, 
  {
    "Status": "OK", 
    "Failrate": "2.65", 
    "Failed": "22", 
    "json_created": "09/10/2020 12:30:19", 
    "Operator": "'ELISA-SK-EE'", 
    "Total": "831"
  }, 
  {
    "Status": "OK", 
    "Failrate": "2.78", 
    "Failed": "37", 
    "json_created": "09/10/2020 12:30:19", 
    "Operator": "'EMT-SK-EE'", 
    "Total": "1331"
  }, 
  {
    "Status": "OK", 
    "Failrate": "6.44", 
    "Failed": "30", 
    "json_created": "09/10/2020 12:30:19", 
    "Operator": "'TELE2-SK-EE'", 
    "Total": "466"
  }, 
  {
    "Status": "OK", 
    "Failrate": "4.08", 
    "Failed": "23", 
    "json_created": "09/10/2020 12:30:19", 
    "Operator": "'TELE2-SK-LT'", 
    "Total": "564"
  }, 
  {
    "Status": "UNKNOWN", 
    "Failrate": "12.50", 
    "Failed": "1", 
    "json_created": "09/10/2020 12:30:19", 
    "Operator": "'TELEDEMA-LT'", 
    "Total": "8"
  }, 
  {
    "Status": "OK", 
    "Failrate": "3.64", 
    "Failed": "16", 
    "json_created": "09/10/2020 12:30:19", 
    "Operator": "'TELIA-SK-LT'", 
    "Total": "440"
  }
]
```
