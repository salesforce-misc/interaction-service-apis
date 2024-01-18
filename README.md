# interaction-service-apis
The interaction service APIs covers both inbound message interaction request schema (inbound-interaction-api.yaml) and outbound message custom event schema (outbound-custom-event-payload.yaml).


## inbound-interaction-api.yaml
The [inbound-interaction-api.yaml](inbound-interaction-api.yaml) shows the inbound message interaction request schema. 

### Salesforce Core Setup

Follow the instructions below to setup connected app and generate access token which will be used as \<AccessToken\> in example curl commands shown in next section.

#### Setup

When setting up a Connected App there are a few things that need to be specified for the integration to work:

- `Enable OAuth Settings`
- Callback URL can be anything, recommend just using `https://salesforce.com` for now
- `Use digital signature` (see below for generating cert)
- Select these OAuth scopes:
  - `Access Interaction API resources (interaction_api)`
  - `Perform Requests at any time (refresh_token, offline_access)`
- Enable `Opt in to issue JSON Web Token (JWT)-based access tokens for named users`
- Save the app
- From your Connected App, click `Manage` -> `Edit Policy`
  - `Permitted Users` -> `Admin approved users are pre-authorized`
- `Manage Profiles` -> `System Administrators`

##### Generate Connected App Cert

To generate a self-signed cert locally:

```bash
openssl genrsa -des3 -passout pass:password -out server.pass.key 2048;
openssl rsa -passin pass:password -in server.pass.key -out server.key;
openssl req -new -key server.key -out server.csr;
openssl x509 -req -sha256 -days 365 -in server.csr -signkey server.key -out server.crt;
```

`server.crt` is your public-certificate and `server.key` is your private key.

##### Generate AccessToken

The Integration Service requires an AccessToken generated using
the [Oauth 2.0 JWT Bearer Flow](https://help.salesforce.com/s/articleView?id=sf.remoteaccess_oauth_jwt_flow.htm&type=5)

To generate a signed JWT you can use a tool such as [JWT.io](https://jwt.io/)

- In the header set the `alg` to `RS256` (required by SFDC)
- In the payload set:
  - `iss` - your Connected Apps Consumer Key
    - This can be found from the Connected Apps page by clicking the Manager Consumer Details button
  - `sub` - the username of the salesforce user account.
  - `aud` - Set depending on the env
    - local: `https://<machineName>.internal.salesforce.com:6101`
    - test:  `https://login.stmfa.stm.salesforce.com` or `https://login.stmfb.stm.salesforce.com`
    - prod: `https://login.salesforce.com`
  - `exp` - timestamp in MS when the token expires. For local dev and testing set this way in the future to avoid having to regenerate a token
- In the signature past in your public and private key of the cert associated with the Connected App

Use the created JWT to generate an AccessToken

Assuming localhost:

```bash
curl --location --request POST -v https://localhost:6101/services/oauth2/token --header 'Content-Type: application/x-www-form-urlencoded' --data-urlencode "grant_type=urn:ietf:params:oauth:grant-type:jwt-bearer" --data-urlencode "assertion=<JWT>>"
```

From the response copy the `access_token` which can be used to make requests to the Interaction Service. The token should be in the format:

```bash
<orgId>!<token>
```

### Interaction API

Following are examples of the curl commands to test the inbound requests for different interactionType: "EntryInteraction" and "AttachmentInteraction".

Example request sending interaction, be sure to replace placeholder values `<..>` and `attachments` part can be optional.

- Interaction request with entryType "Message"

```
curl -v -H $'Authorization: Bearer <AccessToken>' -H "OrgId: <OrgId>" -H "AuthorizationContext: <AuthorizationContext>" -H "content-type:multipart/form-data" -H "RequestId: <RequestId>" -X POST -F "json={
  \"to\": \"<ChannelAddressIdentifier>\",
  \"from\": \"<EndUserClientIdentifier>\",
  \"interactions\": [{
    \"timestamp\": <Timestamp in Unix Epoch>,
    \"interactionType\": \"EntryInteraction\",
    \"payload\": {
      \"id\": \"<InteractionId>\",
      \"entryType\": \"Message\",
      \"abstractMessage\": {
        \"messageType\": \"StaticContentMessage\",
        \"id\": \"<InteractionId>\",
        \"staticContent\": {
          \"formatType\": \"Text\",
          \"text\": \"Hi there\"
        }
      }
    }
  }]
};type=application/json" http://localhost:8085/api/v1/interactions
```

- Interaction request with entryType "TypingStartedIndicator"

```
curl -v -H $'Authorization: Bearer <AccessToken>' -H "OrgId: <OrgId>" -H "AuthorizationContext: <AuthorizationContext>" -H "content-type:multipart/form-data"  -H "RequestId: <RequestId>" -X POST -F "json={
  \"to\": \"<ChannelAddressIdentifier>\",
  \"from\": \"<EndUserClientIdentifier>\",
  \"interactions\": [{
    \"timestamp\": <Timestamp in Unix Epoch>,
    \"interactionType\": \"EntryInteraction\",
    \"payload\": {
      \"id\": \"<InteractionId>\",
      \"entryType\": \"TypingStartedIndicator\",
      \"timestamp\": <Timestamp in Unix Epoch>
    }
  }]
};type=application/json" http://localhost:8085/api/v1/interactions
```

- Interaction request with entryType "TypingStoppedIndicator"

```
curl -v -H $'Authorization: Bearer <AccessToken>' -H "OrgId: <OrgId>" -H "AuthorizationContext: <AuthorizationContext>" -H "content-type:multipart/form-data" -H "RequestId: <RequestId>" -X POST -F "json={
  \"to\": \"<ChannelAddressIdentifier>\",
  \"from\": \"<EndUserClientIdentifier>\",
  \"interactions\": [{
    \"timestamp\": <Timestamp in Unix Epoch>,
    \"interactionType\": \"EntryInteraction\",
    \"payload\": {
      \"id\": \"<InteractionId>\",
      \"entryType\": \"TypingStoppedIndicator\",
      \"timestamp\": <Timestamp in Unix Epoch>
    }
  }]
};type=application/json" http://localhost:8085/api/v1/interactions
```

- Interaction request with entryType "MessageDeliveryFailed"

```
curl -v -H $'Authorization: Bearer <AccessToken>' -H "OrgId: <OrgId>" -H "AuthorizationContext: <AuthorizationContext>" -H "content-type:multipart/form-data"  -H "RequestId: <RequestId>" -X POST -F "json={
  \"to\": \"<ChannelAddressIdentifier>\",
  \"from\": \"<EndUserClientIdentifier>\",
  \"interactions\": [{
    \"timestamp\": <Timestamp in Unix Epoch>,
    \"interactionType\": \"EntryInteraction\",
    \"payload\": {
      \"id\": \"<InteractionId>\",
      \"entryType\": \"MessageDeliveryFailed\",
      \"failedConversationEntryIdentifier\": \"<FailedConversationEntryIdentifier>\",
      \"recipient\": {
         \"appType\": \"11\",
         \"subject\": \"<EndUserClientIdentifier>\",
         \"role\": \"4\"
      },
      \"errorCode\": \"<ErrorCode>\"
    }
  }]
};type=application/json" http://localhost:8085/api/v1/interactions
```

- Interaction request with file attachment

Prerequisites for getting file attachment upload working in local
- Request access with PCSKDeveloperRole to test1-uswest2 AWS account using https://dashboard.prod.aws.jit.sfdc.sh/
- The above access request should get auto-approved.
- Click on Get Credentials -> Reveal Secrets to view the AWS credentials.
- Set AWS_ACCESS_KEY_ID, AWS_SECRET_ACCESS_KEY & AWS_SESSION_TOKEN in your run configuration environment variables in your IDE. Use the above credential values from the previous step.
- Login into AWS console for the test1-uswest2 account and create a new S3 bucket for local use.
- In applcation.properties in your local, set interactionservice.inbound.files.bucket with the name of the S3 bucket you just created in the previous step.
- Also in application.properties, set other attachment related properties. See application.properties.example file for details.
- Please delete the bucket once local testing is complete.

```
curl -v -H $'Authorization: Bearer <AccessToken>' -H "OrgId: <OrgId>" -H "AuthorizationContext: <AuthorizationContext>" -H "content-type:multipart/form-data" -H "RequestId: <RequestId>" -X POST -F "json={
  \"to\": \"<ChannelAddressIdentifier>\",
  \"from\": \"<EndUserClientIdentifier>\",
  \"interactions\": [{
    \"timestamp\": <Timestamp in Unix Epoch>,
    \"interactionType\": \"AttachmentInteraction\",
    \"id\": \"<InteractionId>\",
    \"attachmentIndex\": 0,
    \"contentLength\": 10000,
    \"text\": \"This is my file\"
  }]
};type=application/json" -F "attachments=@/<path to an image>" http://localhost:8085/api/v1/interactions
```

**Note**: When test the example request commands above, replace the request url "http://localhost:8085/api/v1/interactions" with an url with the pattern of "https://\<your org my domain name\>.my.salesforce-scrt.com/api/v1/interactions".

### AgentWork API

AgentWork API only applies for Partner Messaging for CCaaS. Following are examples of the curl commands to test the agent work requests for different capacity paths.

Example request creating agent work, be sure to replace placeholder values `<..>`

- AgentWork request with capacity percentage

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H "content-type:application/json" -H "OrgId:<OrgId>" -H "AuthorizationContext:<AuthorizationContext>"  -H "RequestId: <RequestId>" -X POST -d "{
  \"userId\": \"<UserId>\",
  \"workItemId\": \"<WorkItemId>\",
  \"capacityPercentage\": <CapacityPercentage>,
   \"routingContext\": {
    \"conversationIdentifier\": \"<ConversationId>\",
    \"routingType\": \"<RoutingType>\",
    \"routingCorrelationId\": \"<EventId>\"
  }
}"  http://localhost:8085/api/v1/agentWork
```

- AgentWork request with capacity weight

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H "content-type:application/json" -H "OrgId:<OrgId>" -H "AuthorizationContext:<AuthorizationContext>"  -H "RequestId: <RequestId>" -X POST -d "{
  \"userId\": \"<UserId>\",
  \"workItemId\": \"<WorkItemId>\",
  \"capacityWeight\": <CapacityWeight>,
   \"routingContext\": {
    \"conversationIdentifier\": \"<ConversationId>\",
    \"routingType\": \"<RoutingType>\",
    \"routingCorrelationId\": \"<EventId>\"
  }
}"  http://localhost:8085/api/v1/agentWork
```

- AgentWork request when psr exist

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H "content-type:application/json" -H "OrgId:<OrgId>" -H "AuthorizationContext:<AuthorizationContext>"  -H "RequestId: <RequestId>" -X POST -d "{
  \"userId\": \"<UserId>\",
  \"workItemId\": \"<WorkItemId>\"
  }
}"  http://localhost:8085/api/v1/agentWork
```

**Note**: When test the example request commands above, replace the request url "http://localhost:8085/api/v1/agentWork" with an url with the pattern of "https://\<your org my domain name\>.my.salesforce-scrt.com/api/v1/agentWork".

## outbound-custom-event-payload.yaml
The [outbound-custom-event-payload.yaml](outbound-custom-event-payload.yaml) shows the outbound message custom event schema. Following are examples of the custom event payload received in "data" listener of the custome event after subscribe the event by topic name "my__event__e" shown in payload below for outbound message.


### The outbound message without attachment (Deprecated in Spring '24, Support this deprecated event structure will be removed starting Summer '24. See below for the new outbound message event structure effective starting Spring '24)

```
{
  replayId: '2278491',
  payload: {
    my__event__e {
      CreatedDate: 1690344475579n,
      CreatedById: '13f7e7ad-2431-4cf7-a048-fe9556f847bc',
      my__event__chnlAddrIdField__c: {
        string: 'b0ffeafe-0d89-4338-b14a-172ad203f22a'   // ChannelAddressIdentifier
      },
      my__event__field2__c: {
        string: '{"senderDisplayName":"John Dow","identifier":"56e10dce-2fd3-4ab6-91bf-f55827ad0280","entryType":"Message","entryPayload":{"entryType":"Message","id":"56e10dce-2fd3-4ab6-91bf-f55827ad0280","abstractMessage":{"messageType":"StaticContentMessage","id":"56e10dce-2fd3-4ab6-91bf-f55827ad0280","staticContent":{"formatType":"Text","text":"Hi, how are you?"}},"messageReason":null},"sender":{"appType":"agent","subject":"005xx0000012345","role":"Agent"},"transcriptedTimestamp":1690344475508,"clientTimestamp":1690344475409,"clientDuration":0}'
      },
      my__event__recipientField__c: {
        string: '{"appType":"custom","subject":"David Wood","role":"EndUser"}'
      }
    }
  }
}
```

Here `"subject":"005xx0000012345"` is the salesforce id of sender.


### The outbound message with attachment (Deprecated in Spring '24, Support for this deprecated event structure will be removed starting Summer '24. See below for the new outbound message event structure effective starting Spring '24)
```
{
  replayId: '2278491',
  payload: {
    my__event__e {
      CreatedDate: 1690344475579n,
      CreatedById: '13f7e7ad-2431-4cf7-a048-fe9556f847bc',
      my__event__chnlAddrIdField__c: {
        string: 'b0ffeafe-0d89-4338-b14a-172ad203f22a'
      },
      my__event__payloadField__c: {
        string: '{"senderDisplayName":"John Dow","identifier":"80bb78b1-8240-4e22-ac55-5d517231ca1e","entryType":"Message","entryPayload":{"entryType":"Message","id":"80bb78b1-8240-4e22-ac55-5d517231ca1e","abstractMessage":{"messageType":"StaticContentMessage","id":"80bb78b1-8240-4e22-ac55-5d517231ca1e","staticContent":{"formatType":"Attachments","text":"did you get my file","attachments":[{"name":"BYO-Middleware-Impl.png","attachmentUploadResult":null,"id":"4f3c9d65-acd6-4151-8ac7-6ae1ae46531a","mimeType":"image/png","url":"https://byoccom4-dev-ed--c.stmfa.stm.documentforce.com/sfc/dist/version/download/?oid=00DRM000000O8Qy&ids=068RM0000002ACb&d=%2Fa%2FRM00000000uI%2FcEus0p20sO6Kwp6NKwMI2WeprWXvR9ke7Zp7f0_MTrE&asPdf=false","referenceId":"1e544040-52c8-40f6-a14d-5bc123c5cae4"}]}},"messageReason":null},"sender":{"appType":"agent","subject":"005RM000002ks3p","role":"Agent"},"transcriptedTimestamp":1690679469883,"clientTimestamp":1690679468868,"clientDuration":0}'
      },
      my__event__recipientField__c: {
        string: '{"appType":"custom","subject":"David Wood","role":"EndUser"}'
      }
    }
  }
}
```

### The outbound message without attachment (Recommended latest schema structure effective starting Spring '24)

```
{
  replayId: '2278491',
  payload: {
    my__event__e {
      CreatedDate: 1690344475579n,
      CreatedById: '13f7e7ad-2431-4cf7-a048-fe9556f847bc',
      my__event__field2__c: {
        string: '{"channelAddressIdentifier":"b0ffeafe-0d89-4338-b14a-172ad203f22a","recipient":{"appType":"custom","subject":"David Wood","role":"EndUser"},"senderDisplayName":"John Dow","identifier":"56e10dce-2fd3-4ab6-91bf-f55827ad0280","entryType":"Message","entryPayload":{"entryType":"Message","id":"56e10dce-2fd3-4ab6-91bf-f55827ad0280","abstractMessage":{"messageType":"StaticContentMessage","id":"56e10dce-2fd3-4ab6-91bf-f55827ad0280","staticContent":{"formatType":"Text","text":"Hi, how are you?"}},"messageReason":null},"sender":{"appType":"agent","subject":"005xx0000012345","role":"Agent"},"transcriptedTimestamp":1690344475508,"clientTimestamp":1690344475409,"clientDuration":0}'
      },
      my__event__EventType__c: anonymous { string: 'Interaction' }
    }
  }
}
```

Here `"subject":"005xx0000012345"` is the salesforce id of sender.


### The outbound message with attachment (Recommended latest schema structure effective starting Spring '24)
```
{
  replayId: '2278491',
  payload: {
    my__event__e {
      CreatedDate: 1690344475579n,
      CreatedById: '13f7e7ad-2431-4cf7-a048-fe9556f847bc',
      my__event__payloadField__c: {
        string: '{"channelAddressIdentifier":"b0ffeafe-0d89-4338-b14a-172ad203f22a","recipient":{"appType":"custom","subject":"David Wood","role":"EndUser"},"payload":{"senderDisplayName":"John Dow","identifier":"80bb78b1-8240-4e22-ac55-5d517231ca1e","entryType":"Message","entryPayload":{"entryType":"Message","id":"80bb78b1-8240-4e22-ac55-5d517231ca1e","abstractMessage":{"messageType":"StaticContentMessage","id":"80bb78b1-8240-4e22-ac55-5d517231ca1e","staticContent":{"formatType":"Attachments","text":"did you get my file","attachments":[{"name":"BYO-Middleware-Impl.png","attachmentUploadResult":null,"id":"4f3c9d65-acd6-4151-8ac7-6ae1ae46531a","mimeType":"image/png","url":"https://byoccom4-dev-ed--c.stmfa.stm.documentforce.com/sfc/dist/version/download/?oid=00DRM000000O8Qy&ids=068RM0000002ACb&d=%2Fa%2FRM00000000uI%2FcEus0p20sO6Kwp6NKwMI2WeprWXvR9ke7Zp7f0_MTrE&asPdf=false","referenceId":"1e544040-52c8-40f6-a14d-5bc123c5cae4"}]}},"messageReason":null},"sender":{"appType":"agent","subject":"005RM000002ks3p","role":"Agent"},"transcriptedTimestamp":1690679469883,"clientTimestamp":1690679468868,"clientDuration":0}}'
      },
      my__event__EventType__c: anonymous { string: 'Interaction' }
    }
  }
}
```

Note: In the payload examples above, the key "my__event__e" is the developer name for the outbound message customer event configured in salesforce setup. The keys "my__event__chnlAddrIdField__c", "my__event__payloadField__c", "my__event__recipientField__c" and "my__event__EventType__c" are corresponding custom fields "Custom Event Channel Address Id Field", "Custom Event Payload Field", "Custom Event Recipient Field", "Custom event type Field" defined in the outbound message customer event.

## Additional Notes
Use of the code in this repository with Salesforce products or services should be used in accordance with any applicable developers guides on [developer.salesforce.com](https://developer.salesforce.com/) and may be subject to additional terms of use, including but not limited to the [Salesforce Program Agreement - Program Terms for the Salesforce Developers Program](https://www.salesforce.com/company/program-agreement/#devs).

