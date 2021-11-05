[![Build Status](https://travis-ci.com/usdot-jpo-sdc/sdc-dot-webportal.svg?branch=develop)](https://travis-ci.com/usdot-jpo-sdc/sdc-dot-webportal)
[![Quality Gate Status](https://sonarcloud.io/api/project_badges/measure?project=usdot-jpo-sdc_sdc-dot-webportal&metric=alert_status)](https://sonarcloud.io/dashboard?id=usdot-jpo-sdc_sdc-dot-webportal)
[![Coverage](https://sonarcloud.io/api/project_badges/measure?project=usdot-jpo-sdc_sdc-dot-webportal&metric=coverage)](https://sonarcloud.io/dashboard?id=usdot-jpo-sdc_sdc-dot-webportal)

# log4sdc
Logging, alert, and alarm facilities for the SDC Platform

## Table of Contents

[I. Release Notes](#release-notes)

[II. Usage Example](#usage-example)

[III. Configuration](#configuration)

[IV. Installation](#installation)

[V. Design and Architecture](#design-architecture)

[VI. Unit Tests](#unit-tests)

[VII.  File Manifest](#file-manifest)

[VIII.  Development Setup](#development-setup)

[IX.  Release History](#release-history)

[X. Contact Information](#contact-information)

[XI. Contributing](#contributing)

[XII. Known Bugs](#known-bugs)

[XIII. Credits and Acknowledgment](#credits-and-acknowledgement)

[XIV.  CODE.GOV Registration Info](#code-gov-registration-info)

[XV.  Code Quality Monitor](#code-quality-monitor)

---

<!---                           -->
<!---     Release Notes         -->
<!---                           -->

<a name="release-notes"/>

## I. Release Notes

**October 22, 2021. SDC log4sdc Release 1.0**
### What's New in Release 1.0
* AWS Simple Notification Topics to support notifications generated by the log4sdc mechanism.
  * prod-log4sdc-alert-topic: to generate alert email notifications
  * prod-log4sdc-error-topic: to generate error email notifications
  * prod-log4sdc-fatal-topic: to generate fatal email and SMS notifications
* IAM Policy definitions for publish and subscribe permissions for the topics
* Subscriptions for the topics



<!---                           -->
<!---     Usage Example         -->
<!---                           -->

<a name="usage-example"/>

## II. Usage Example


<!---                           -->
<!---     Configuration         -->
<!---                           -->

<a name="configuration"/>

## III. Configuration


<!---                           -->
<!---     Installation          -->
<!---                           -->

<a name="installation"/>

## IV. Installation

### SNS Topic Installation


#### Deployment Plan
* Clone the log4sdc repository into a Linux environment (e.g., SDC build machine)
* Change to the log4sdc folder
* Change to the sns terraform folder
  * cd sns/terraform/
* Execute the following commands to deploy the topics:
  * terraform init
  * terraform apply -var-file=config/dev.tfvars


#### Test Plan
* Log on into the AWS Console for the SDC system account, navigate to the Simple Notification Service configuration
* For all prod-log4sdc-* topics:
  * Publish a test message with arbitrary subject and text
  * Verify that email message have been successfully sent to all subscribed email addresses
  * Verify that SMS message have been successfully sent for the prod-log4sdc-fatal-topic message to all subscribed mobile phone numbers


#### Rollback Plan
* Log on into the AWS Console for the SDC system account, navigate to the Simple Notification Service configuration
* For all prod-log4sdc-* topics:
  * Delete subscriptions
  * Delete topics themselves.


### log4sdc Lambda Payer Installation


#### Deployment Plan
* Clone the log4sdc repository into a Linux environment (e.g., SDC build machine)
* Change to the log4sdc folder
* Change to the lambda-layer deploy folder
  * cd lambda-layer/deploy/
* Execute the following commands to deploy the lambda layer:
  * ./deploy-common-layer.sh


#### Test Plan
* Log on into the AWS Console for the SDC system account, navigate to the Lambda configuration
* Create a test function with Python 3.7+ runtime
  * The layer is compatible with Python 3.7, 3.8, and 3.9 runtimes
* Add this code to the function:

```
import json
from common.logger_utility import *

def lambda_handler(event, context):

    config = {
    'project': 'FOO', 
    'team': 'FOO-BAR', 
    'sdc_service': 'DATA_INGEST', 
    'component': 'log4sdc-common', 
    }
    
    LoggerUtility.init(config=config)
    LoggerUtility.setLevel('DEBUG')
    LoggerUtility.logDebug("DEBUG Test LoggerUtility")
    LoggerUtility.logInfo("INFO Test LoggerUtility")
    LoggerUtility.logWarning("WARN Test LoggerUtility")
    LoggerUtility.logError("ERROR Test LoggerUtility")
    LoggerUtility.logCritical("CRITICAL Test LoggerUtility")
    LoggerUtility.alert("ALERT Test LoggerUtility")
    
    return {
        'statusCode': 200,
        'body': json.dumps('Hello from Log4SDC Lambda!')
    }
```

* Execute function test. Make sure there are no errors and that similar printout is produced:

```
[DEBUG]	2021-10-27T01:04:10.811Z	e81909a0-196c-4bd2-990f-021732be96c6	DEBUG Test LoggerUtility
[INFO]	2021-10-27T01:04:10.811Z	e81909a0-196c-4bd2-990f-021732be96c6	INFO Test LoggerUtility
[WARNING]	2021-10-27T01:04:10.811Z	e81909a0-196c-4bd2-990f-021732be96c6	WARN Test LoggerUtility
[ERROR]	2021-10-27T01:04:10.812Z	e81909a0-196c-4bd2-990f-021732be96c6	ERROR Test LoggerUtility
[CRITICAL]	2021-10-27T01:04:12.052Z	e81909a0-196c-4bd2-990f-021732be96c6	CRITICAL Test LoggerUtility
[ERROR]	2021-10-27T01:04:12.325Z	e81909a0-196c-4bd2-990f-021732be96c6	ALERT Test LoggerUtility
END RequestId: e81909a0-196c-4bd2-990f-021732be96c6
REPORT RequestId: e81909a0-196c-4bd2-990f-021732be96c6	Duration: 1796.73 ms	Billed Duration: 1797 ms	Memory Size: 128 MB	Max Memory Used: 67 MB	Init Duration: 332.22 ms
```


#### Rollback Plan
* Log on into the AWS Console for the SDC system account, navigate to the Lambda configuration
* Navigate to Lambda Layers, locate log4sdc layer.
* Navigate to the latest log4sdc layer version and delete it with console controls.


<!---                                 -->
<!---     Design and Architecture     -->
<!---                                 -->

<a name="design-architecture"/>

## V. Design and Architecture


<!---                           -->
<!---     Unit Tests          -->
<!---                           -->

<a name="unit-tests"/>

## VI. Unit Tests





<!---                           -->
<!---     File Manifest         -->
<!---                           -->

<a name="file-manifest"/>

## VII. File Manifest


<!---                           -->
<!---     Development Setup     -->
<!---                           -->

<a name="development-setup"/>

## VIII. Development Setup



### Prerequisites


<!---                           -->
<!---     Release History       -->
<!---                           -->

<a name="release-history"/>

## IX. Release History

October 22, 2021. SDC log4sdc Release 1.0


<!---                             -->
<!---     Contact Information     -->
<!---                             -->

<a name="contact-information"/>

## X. Contact Information

For any queries you can reach to sdc-support@dot.gov


<!---                           -->
<!---     Contributing          -->
<!---                           -->

<a name="contributing"/>

## XI. Contributing


<!---                           -->
<!---     Known Bugs            -->
<!---                           -->

<a name="known-bugs"/>

## XII. Known Bugs




<!---                                    -->
<!---     Credits and Acknowledgment     -->
<!---                                    -->

<a name="credits-and-acknowledgement"/>

## XIII. Credits and Acknowledgment
Thank you to the Department of Transportation for funding to develop this project.


<!---                                    -->
<!---     CODE.GOV Registration Info     -->
<!---                                    -->

<a name="code-gov-registration-info">

## XIV. CODE.GOV Registration Info
Agency:  DOT

Short Description: The Secure Data Commons is an online data warehousing and analysis platform for transportation researchers.

Status: Production

Tags: transportation, connected vehicles, intelligent transportation systems

Labor Hours:

Contact Name: sdc-support@dot.gov

<!-- Contact Phone: -->

<a name="code-quality-monitor">

## XV. Code Quality Monitor


---
[Back to top](#toc)
