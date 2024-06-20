# TabularReportsInternalRecipientGenerator
That Tabular Reports Internal Recipient Generator App is a Qlik Sense application suitable for Qlik Cloud deployments. Its purpose is to read the  names and email addresses from the Qlik Cloud tenant as well as the access control lists from the spaces, and Apps within the tenant

# Pre-requisites
    1. A Qlik Cloud Tenant
    2. Successful deployment of the Qlik Cloud Monitoring Apps
    
        To deploy the monitoring applications for Qlik Cloud, including the 'Access Evaluator' please refer to the following resources:

	      Qlik  Cloud Monitoring Apps Workflow Guide:
 	      https://community.qlik.com/t5/Official-Support-Articles/Qlik-Cloud-Monitoring-Apps-Workflow-Guide/ta-p/2134140

	      Qlik Cloud Monitoring Apps Introduction:
	      https://community.qlik.com/t5/Official-Support-Articles/The-Qlik-Sense-Monitoring-Applications-for-Cloud-and-On-Premise/ta-p/1822454

	      Qlik Cloud Monitoring Apps Repository
	      https://github.com/qlik-oss/qlik-cloud-monitoring-apps

      ---> The Tabular Reports Internal Recipient Generator App has been tested with Access Evaluator v2.0.3

# Configuration

    1. Import the application into Qlik Cloud into a personal space or a shared space where you have 'can edit data' access or higher
    2. Open the application and switch to the 'Data Load Editor'
    3. On the "Data Source / Binary Load" section,  enter the AppID of the Access Evaluator application

      IE: Binary '<Enter AppID of Access Evaluator App Here>';
    
    4. On the "Configuration" section: 

        A) Enter 1 or 0 to generate (1) a 'tenant wide' recipient QVD with email addresses for all users in the tenant
        
        IE:   LET vGenerateTenantRecipients=1;

        B) Enter a data connection string that denotes where the QVDs will be stored:

        IE: LET vSpacetoStoreQVDs  '<SpaceName>:DataFiles'
        IE: LET vSpacetoStoreQVDs='Recipient Lists:DataFiles';

        C) (optional) List 0 or more AppIDs. A set of QVDs containing the names and email addresses of all tenant users with access to the app(s) will be generated. App ACLs can be different to their Spaces when 'app sharing' is performed.

        IE: 
        RecipientsForApps:
        LOAD * INLINE [
        AppIDforQVD
        5875bd4c-f1d2-472d-bb88-f942b6221378
        ];

        D) (optional) List 0 or more SpaceIDs. A set of QVDs containing the names and email addresses of all tenant users with access to the space(s) will be generated. Space ACLs are visible by clicking on the members tab of the Space

        IE: 
        RecipientsForSpaces:
        LOAD * INLINE [
        SpaceIDforQVD
        6571da9969edd2a74eb08283
        ];

    5. Click Reload -> to produce QVD recipient lists

Notes:

    - The recipient QVD files contain Names, Emails, Groups and other handy markers from which to load into your reporting applications. The fields are pre-tagged meaning that you do not need to issue a 'TAG FIELD' statement in the load script to ensure they are interpreted as recipients and groups

    - The list of names and email addresses come from the Access Evaluator Application. If you add users or change access control lists in your Qlik Cloud tenant, you will need to reload the Access Evaluator and then reload the Tabular Reports Internal Recipient Generator App in order to see the new users in the respective QVDs

    - The fields in the QVDs are prefixed with %.  Use  SetHidePrefix='%' to keep these fields hidden in your user interface. 
