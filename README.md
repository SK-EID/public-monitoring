**SK ID Solutions AS**

**SK Public Monitoring Interface Specification**

Version 1.0.11

Last update 07.05.2018



| **Version** | **Date** | **Author** | **Notes** |
| --- | --- | --- | --- |
| 1.0.1 | 31.03.2014 | Alvar Nõmmik | First version of the document. |
| 1.0.2 | 02.04.2014 | Alvar Nõmmik | Added description of monitoring logic, minor fixes |
| 1.0.3 | 11.07.2014 | Alvar Nõmmik | Added check\_rc\_getmnoid to Bite and Tele2 LT Added check\_dds2\_mssp\_bite\_rc to BiteExplanations about hard/softstate |
| 1.0.4 | 29.07.2014 | Alvar Nõmmik | Added information about SK OCSP check, minor fixes |
| 1.0.5 | 09.10.2014 | Alvar Nõmmik,Urmo Keskel | Monitoring logic changes  see p .1.2, new state UNKNOWN added Change of plugin output pattern, new format "Failure rate zz%, Failed xx of yy"Updated the sample of json and XML output |
| 1.0.7 | 02.04.2015 | Alvar Nõmmik | Added check for SMSC status (Tele2EE and EMT) - check\_\*\_smscExamples updated and added CRITICAL scenario |
| 1.0.8 | 05.06.2017 | Alvar Nõmmik | check\_dds2\_mssp\_elisa (Legacy Elisa mssp check) - removedcheck\_rc\_getmnoid – removedomnitel\_sk business process - removedMinor fixes in wordingcheck\_\*\_smsc – check described |
| 1.0.9 | 24.04.2018 | Kristjan Koskor | Converted to .md format. <br /> Published on github. <br />Minor formating fixes|
| 1.0.10 | 26.04.2018 | Kristjan Koskor |Added LT-SK business process identifiers for Telia, Tele2 and Bite. |
| 1.0.11 | 26.04.2018| Alvar Nõmmik | Documentation formating changed |
| 1.0.12 | 07.05.2018| Kristjan Koskor | Added Smart-ID description of Smart-ID monitoring |


# Table of Contents
* [1. SK public monitoring interface](#1-sk-public-monitoring-interface)
    * [1.1. Structure](#11-structure)
    * [1.2. Description of monitoring logic and failure rates of different components](#12-description-of-monitoring-logic-and-failure-rates-of-different-components)
    * [1.3. Hard and soft state](#13-hard-and-soft-state)
* [2. Examples](#2-examples)
    * [2.1. Example json output](#21-example-json-output)
    * [2.2. Example XML output](#22-example-xml-output)



# 1. SK public monitoring interface

SK public monitoring interface provides JSON and XML based information about availability of Mobile-ID service to mobile operator clients.

Interface generates ".json" files (from Nagios Business Processes interface) and xml.php - provides same output in XML format (generated from .json files). Monitoring data is generated every minute.

Location of files:

- --JSON: [sk.ee/util/public_monitoring/operator_identifier.json](http://www.sk.ee/util/public_monitoring/operator_identifier.json)
- --XML: [sk.ee/util/public_monitoring/xml.php?id=operator_identifier](http://www.sk.ee/util/public_monitoring/xml.php?id=operator_identifier)

Where "operator_identifier" is the _identifier_ of the business process.

List of business processes:

| **Identifier** | **Description** |
| --- | --- |
| _emt_ | Mobile-ID service for Telia (EE) customers |
| _elisa_ | Mobile-ID service for Elisa (EE) customers |
| _tele2ee_ | Mobile-ID service for Tele2 (EE) customers |
| _omnitel_rc_ | Mobile-ID service for Omnitel (LT) customers with RC certificates |
| _tele2lt_ | Mobile-ID service for Tele2 (LT) customers |
| _bite_ | Mobile-ID service for (LT) Bite customers |
| _telia_sk_lt_ | Mobile-ID service for (LT) Telia customers |
| _tele2_sk_lt_ | Mobile-ID service for (LT) Tele2 customers |
| _bite_sk_lt_ | Mobile-ID service for (LT) Bite customers |
| _smart_ | Smart-ID Authnetcation and Signing transactions |



## 1.1. Structure

**business_process**

Business process block – contains information about sub list

**hardstate**

Status of business process (if one sub component is not operational then entire business process is failing)

**components**

Business process sub components

| **hardstate** | Status of sub-process. (etc. OCSP service)Changes when _n_ number of checks failed. **States:** OK – component is operationalWARNING – component is operational; some of the requests are failing.CRITICAL – component is not working correctlyUNKNOWN – Some of requests are failing, but the amount of requests is too low to decide is the component operational or not. |
| --- | --- |
| **plugin\_output** | Nagios plugin output. Usually contains detailed information about service status "SERVICE\_NAME Failure rate: x%, Failed yy of zz""OK" – Unable to read failure rate information from monitoring plugin. |
| **service** | Name of the service: **check\_dds2\_mssp\_\*** - MSSP (Mobile Signature Service Provider) checks **check\_dds\_certstore\_\*** - External certificate store checks **check\_ocsp\_\*** – OCSP checks **check\_\*\_smsc** – Status of SMSCqueue = number of SMS-s in queuelink = link status (1 – link up; 0 – link down)plugin output example: (tele2 OK: queue=0, link=1") |

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
| **Last 30 min** | at least 20 queries and 25% queries failed | at least 20 queries and 40% queries failed | less than 20 queries and at least 1 failed |
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
| _telia_sk_lt_ | 30 min. |
| _tele2_sk_lt_ | 30 min. |
| _bite_sk_lt_ | 30 min. |


## 1.4. Hard and soft state

In order to prevent false alarms from transient problems, Nagios allows define how many times a service or host should (re)checked before considered to have a "real" problem.

plugin_output – is changing when soft state changed.

Actual alert is issued when "hardstate" is changing to alarm state (there has to be x amount of checks to confirm that change really happened).

Currently multiple check logic is used only in check_ocsp_ocsp.sk.ee service. Therefore in this component hardstate and plugin output may show different statuses

More information: https://assets.nagios.com/downloads/nagioscore/docs/nagioscore/3/en/statetypes.html

# 2. Examples
## 2.1. Example json output


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

## 2.2. Example XML output


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
    
# 3. Smart-ID

## 3.1 Structure
|  **Key** | **Type** | **Desctiption** |
| --- | --- | --- |
| _public_monitoring_ | tag | Root element |
| _itemN_ | tag | child element where N is a row number |
| Use | _str_ | Use case / type of transaction("Authentication" or "Signing") |
| Total | _int_ | Total number of transactions for a particular use case |
| Success | _int_ | Number of successfully completed transactions of particular use case |
| Failed | _int_ | Number of failed transactions of a particular use case <br />* **NB!**  This count also includes end user error cases<br /> such as when the user cancels a transaction; <br />when the user fails to repond in time; <br />when the users PIN is blocked etc. *
|Failrate |_float_|Percent value of failed transactions from the total number of transactions of a particular use case|
|Status|_str_|**State of service:** <br /> "OK" = failrate is lower than 5% <br />"WARN" = failrate is greater than 5%<br />"CRITICAL" = failrate is greater than 75%|


## 3.2. Example xml output


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
