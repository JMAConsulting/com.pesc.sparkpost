# SparkPost / CiviCRM Integration
Integrates SparkPost to CiviCRM, so email can be sent out over the SparkPost service and bounces can be processed in CiviCRM

## What This Extension Does
* Adds a tag to all outgoing CiviMail messages. The civi-generated return-path is added as a SparkPost tag, because like most SMTP services, SparkPost strips out the return-path header for its own.
* Adds a scheduled job that uses SparkPost API to fetch bounce events and processes their bounces in CiviCRM. I use the hash included in the SparkPost tag
* Adds Bounce type and Pattern that helps with marking emails on hold in CiviCRM that SparkPost has added to it's suppression list

##Setup Steps
1. Create SparkPost Account
2. Add Sending Domain and verify domain
3. Create API key with appropriate permissions, save this API key
  * “Select All” is easiest way, but you may want to change this for your needs
  * Required: Message Events: Read-only, Send via SMTP, Suppression Lists: Read/Write
4. Edit CiviCRM Outbound Email settings
  * Select mailer: SMTP
  * Smtp server: smtp.sparkpostmail.com
  * Port: 587
  * Authentication: Yes
  * SMTP Username: SMTP_Injection
  * SMTP Password: [api key from step 3]
5. Install/Enable CiviCRM / SparkPost Extension
6. Edit/Enable Scheduled Job
  * api_key=[ api key from step 3]
  * events=[leave defaults, remove “- required”]
  * date_filter=[0 OR 1 - optional]

##Scheduled Job Parameters  
* events: comma separated list of SparkPost events that should be considered bounces in CiviCRM. You can usually just leave the defaults, but this can be changed to fit your needs. 
* date_filter: If you have this set to 1, this will only query bounce events from SparkPost that have occurred since the scheduled job last ran successfully. This should make things run a little faster because there will be fewer results to parse through from SparkPost.

NOTE: friendly_froms scheduled job parameter has been removed. This value is now filled with the values from Administer > CiviMail > From Email Addresses
