**SK ID Solutions AS**

**SK Public Monitoring Interface Specification**

Version 1.0.9

Last update 05.06.2017



| **Version** | **Date** | **Author** | **Notes** |
| --- | --- | --- | --- |
| 1.0.1 | 31.03.14 | Alvar Nõmmik | First version of the document. |
| 1.0.2 | 02.04.14 | Alvar Nõmmik | Added description of monitoring logic, minor fixes |
| 1.0.3 | 11.07.14 | Alvar Nõmmik | Added check\_rc\_getmnoid to Bite and Tele2 LT Added check\_dds2\_mssp\_bite\_rc to BiteExplanations about hard/softstate |
| 1.0.4 | 29.07.14 | Alvar Nõmmik | Added information about SK OCSP check, minor fixes |
| 1.0.5 | 09.10.14 | Alvar Nõmmik,Urmo Keskel | Monitoring logic changes  see p .1.2, new state UNKNOWN added Change of plugin output pattern, new format "Failure rate zz%, Failed xx of yy"Updated the sample of json and XML output |
| 1.0.7 | 02.04.15 | Alvar Nõmmik | Added check for SMSC status (Tele2EE and EMT) - check\_\*\_smscExamples updated and added CRITICAL scenario |
| 1.0.8 | 05.06.2017 | Alvar Nõmmik | check\_dds2\_mssp\_elisa (Legacy Elisa mssp check) - removedcheck\_rc\_getmnoid – removedomnitel\_sk business process - removedMinor fixes in wordingcheck\_\*\_smsc – check described |
| 1.0.9 | 24.04.2018 | Kristjan Koskor | Converted to .md format. <br /> Published on github. <br />Minor formating fixes|



Table of Contents
* SK public monitoring interface
* Structure
* Description of monitoring logic and failure rates of different components
* "Hard" and "soft" state
* Examples
* Example json output
* Example XML output



1. 1SK public monitoring interface

SK public monitoring interface provides JSON and XML based information about availability of Mobile-ID service to mobile operator clients.

Interface generates ".json" files (from Nagios Business Processes interface) and xml.php - provides same output in XML format (generated from .json files). Monitoring data is generated every minute.

Location of files:

- --JSON: [sk.ee/util/public\_monitoring/operator\_identifier.json](http://www.sk.ee/util/public_monitoring/operator_identifier.json)
- --XML: [sk.ee/util/public\_monitoring/xml.php?id=operator\_identifie](http://www.sk.ee/util/public_monitoring/xml.php?id=bite)r

Where "operator\_identifier" is the _identifier_ of the business process.

List of business processes:

| **Identifier** | **Description** |
| --- | --- |
| _emt_ | Mobile-ID service for Telia (EE) customers |
| _elisa_ | Mobile-ID service for Elisa (EE) customers |
| _tele2ee_ | Mobile-ID service for Tele2 (EE) customers |
| _omnitel\_rc_ | Mobile-ID service for Omnitel (LT) customers with RC certificates |
| _tele2lt_ | Mobile-ID service for Tele2 (LT) customers |
| _bite_ | Mobile-ID service for (LT) Bite customers |

1.
  1. 1.1Structure

**business\_process**

Business process block – contains information about sub list

**hardstate**

Status of business process (if one sub component is not operational then entire business process is failing)

**components**

Business process sub components

| **hardstate** | Status of sub-process. (etc. OCSP service)Changes when _n_ number of checks failed. **States:** OK – component is operationalWARNING – component is operational; some of the requests are failing.CRITICAL – component is not working correctlyUNKNOWN – Some of requests are failing, but the amount of requests is too low to decide is the component operational or not. |
| --- | --- |
| **plugin\_output** | Nagios plugin output. Usually contains detailed information about service status "SERVICE\_NAME Failure rate: x%, Failed yy of zz""OK" – Unable to read failure rate information from monitoring plugin. |
| **service** | Name of the service: **check\_dds2\_mssp\_\*** - MSSP (Mobile Signature Service Provider) checks **check\_dds\_certstore\_\*** - External certificate store checks **check\_ocsp\_\*** – OCSP checks **check\_\*\_smsc** – Status of SMSCqueue = number of SMS-s in queuelink = link status (1 – link up; 0 – link down)plugin output example: (tele2 OK: queue=0, link=1") |

**bp\_id**

ID of Business process (short identifier)

**display\_name**

Name of Business process

**json\_created**

JSON report generation time

1.
  1. 1.2Description of monitoring logic and failure rates of different components

|
- **External certificate store monitoring (service syntax:**  **check\_dds\_certstore\_\***** )**
- **Mobile operator monitoring (service syntax:**  **check\_dds2\_mssp\_\***** )**
- **External OCSP monitoring (service syntax:**  **check\_ocsp\_\***** )**
 |
| --- |
|   | **WARNING** | **CRITICAL** | **UNKNOWN** |
| **Last 5 min** | - | all queries failed | - |
| **Last 30 min** | at least 20 queries and 25% queries failed | at least 20 queries and 40% queries failed | less than 20 queries and at least 1 failed |
| 1 check to change the state, check interval 4 min |
|
- **SK OCSP monitoring (service: check\_ocsp\_ocsp.sk.ee)**
 |
|   | - | OCSP query failed | - |
| 2 checks to change state, check interval 3 min |



1.
  1. 1.3"Hard" and "soft" state

In order to prevent false alarms from transient problems, Nagios allows define how many times a service or host should (re)checked before considered to have a "real" problem.

plugin\_output – is changing when soft state changed.

Actual alert is issued when "hardstate" is changing to alarm state (there has to be x amount of checks to confirm that change really happened).

Currently multiple check logic is used only in check\_ocsp\_ocsp.sk.ee service. Therefore in this component hardstate and plugin output may show different statuses

More information: http://nagios.sourceforge.net/docs/3\_0/statetypes.html

1. 2Examples
  1. 2.1Example json output

{

   "business\_process" : {

      "hardstate" : "OK",

      "components" : [

         {

            "hardstate" : "OK",

            "plugin\_output" : "OK Failure rate: 0%, Failed 0 of 40",

            "service" : "check\_dds2\_mssp\_bite\_rc"

         },

         {

            "hardstate" : "OK",

            "plugin\_output" : "RC\_WPKITSP\_STORE Failure rate: 0%, Failed 0 of 40",

            "service" : "check\_dds\_certstore\_rc\_wpkitsp\_store"

         },

         {

            "hardstate" : "OK",

            "plugin\_output" : "OK",

            "service" : "check\_ocsp\_ocsp.rcsc.lt"

         }

      ],

      "bp\_id" : "bite",

      "display\_name" : "BITE business process"

   },

   "json\_created" : "2017-05-20 13:37:01"

}

1.
  1. 2.2Example XML output

<public\_monitoring>

        <business\_process>

                <hardstate>OK</hardstate>

                <components>

                        <hardstate>OK</hardstate>

                        <plugin\_output>OK Failure rate: 0%, Failed 0 of 40</plugin\_output>

                        <service>check\_dds2\_mssp\_bite\_rc</service>

                </components>

                <components>

                        <hardstate>OK</hardstate>

                        <plugin\_output>RC\_WPKITSP\_STORE Failure rate: 0%, Failed 0 of 40</plugin\_output>

                        <service>check\_dds\_certstore\_rc\_wpkitsp\_store</service>

                </components>

                <components>

                        <hardstate>OK</hardstate>

                        <plugin\_output>OK</plugin\_output>

                        <service>check\_ocsp\_ocsp.rcsc.lt</service>

                </components>

                <bp\_id>bite</bp\_id>

                <display\_name>BITE business process</display\_name>

        </business\_process>

        <json\_created>2017-05-20 13:37:01</json\_created>

</public\_monitoring>