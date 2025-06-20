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
         \"appType\": \"custom\",
         \"subject\": \"<EndUserClientIdentifier>\",
         \"role\": \"EndUser\"
      },
      \"errorCode\": \"<ErrorCode>\"
    }
  }]
};type=application/json" http://localhost:8085/api/v1/interactions
```
Note: Effective Spring ‘24, payloads with `"entryType": "MessageDeliveryFailed"` must set `"appType": "custom"`, and `"role": "EndUser"`. If `appType` and `role` are set to any other values, the request fails.

- Interaction request with Routing Attributes

```
curl -v -H $'Authorization: Bearer <AccessToken>' -H "OrgId: <OrgId>" -H "AuthorizationContext: <AuthorizationContext>" -H "content-type:multipart/form-data" -H "RequestId: <RequestId>" -X POST -F "json={
  \"to\": \"<ChannelAddressIdentifier>\",
  \"from\": \"<EndUserClientIdentifier>\",
  \"routingAttributes\" : {\"<channelVariable>\" : \"<value>\", \"<channelVariable>\" : \"<value>\"},
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
Note: routingAttributes for Interaction request will only be applicable for when RoutingOwner in ConversationChannelDefinition is "Salesforce". For RoutingOwner set to "Partners", routingAttributes are to be passed via RouteWork API.

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

### Consent Status API

Example request to update the consent status of a Messaging End User, be sure to replace placeholder values `<..>`

- Interaction request to update Consent Status

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H "content-type:application/json" -H "OrgId:<OrgId>" -H "AuthorizationContext:<AuthorizationContext>"  -H "RequestId: <RequestId>" -X PATCH -d "{
  \"endUserClientId\" : \"<endUserClientId>\",
  \"channelAddressIdentifier\" : \"<channelAddressIdentifier>\",
  \"consentStatus\" : \"<ConsentStatusEnum>\"
  }
}" <local/staging test url>/api/v1/consent
```

The initial app is also configured with the management service available on port 7080. To access this, you will need to use a token created with
the `generate_admin_key.sh` script.

### Route API

Example request to route the workItem be sure to replace placeholder values `<..>`

- Interaction request to route Conversation based on Flow ID and fallback Queue ID and optional routing attributes.

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H 'AuthorizationContext: <AuthorizationContext>' -H 'RequestId: <RequestId>' -H 'OrgId: <OrgId>' -H 'Content-Type: application/json' -X POST -d "{
    \"conversationIdentifier\": \"<conversationIdentifier>\",
    \"routingType\": \"<routingType>\",
    \"routingInfo\": {
        \"flow\":{
            \"flowId\": \"<FlowDefinitionViewId>\",
            \"queueId\": \"<QueueId>\"
        },
        \"routingAttributes\": {
            \"<channelVariable>\" : \"<value>\"
        }
    }
}" <local/staging test url>/api/v1/route
```

- Interaction request to route Conversation based on Queue ID

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H 'AuthorizationContext: <AuthorizationContext>' -H 'RequestId: <RequestId>' -H 'OrgId: <OrgId>' -H 'Content-Type: application/json' -X POST -d "{
    \"conversationIdentifier\": \"<conversationIdentifier>\",
    \"routingType\": \"<routingType>\",
    \"routingInfo\": {
      \"queueId\": \"<QueueId>\"
    }
}" <local/staging test url>/api/v1/route
```

- Interaction request to route Conversation based on configuration in Messaging Channel

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H 'AuthorizationContext: <AuthorizationContext>' -H 'RequestId: <RequestId>' -H 'OrgId: <OrgId>' -H 'Content-Type: application/json' -X POST -d "{
    \"conversationIdentifier\": \"<conversationIdentifier>\",
    \"routingType\": \"<routingType>\"
}" <local/staging test url>/api/v1/route
```

- Interaction request to Delete PSRs for a given Conversation ID and optional cancel reason

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H 'AuthorizationContext: <AuthorizationContext>' -H 'RequestId: <RequestId>' -H 'OrgId: <OrgId>' -H 'Content-Type: application/json' -X DELETE -d "{
    \"conversationIdentifier\": \"<conversationIdentifier>\",
    \"cancelReason\": \"<cancelReason>\"
}" <local/staging test url>/api/v1/route
```
#### RoutingResult API

Example request to notify routing result, be sure to replace placeholder values <..> routingType being optional

- Request to notify success routing result

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H 'AuthorizationContext: <AuthorizationContext>' -H 'RequestId: <RequestId>' -H 'OrgId: <OrgId>' -H 'Content-Type: application/json' -X POST -d "{
    \"routingType\":\"<routingType>\"
    \"workItemId\":\"<WorkItemId>\",
    \"conversationIdentifier\":\"<ConversationId>\",
    \"success\":true,
    \"externallyRouted\":true
}" http://localhost:8085/api/v1/routingResult
```

- Request to notify failure routing result routingType being optional

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H 'AuthorizationContext: <AuthorizationContext>' -H 'RequestId: <RequestId>' -H 'OrgId: <OrgId>' -H 'Content-Type: application/json' -X POST -d "{
    \"routingType\":\"<routingType>\",
    \"workItemId\":\"<WorkItemId>\",
    \"conversationIdentifier\":\"<ConversationId>\",
    \"success\":false,
    \"externallyRouted\":true,
    \"errorMessage\":\"<ErrorMessage>\"
}" http://localhost:8085/api/v1/routingResult
```

#### Capabilities API

The API allows to register different capabilities to support messaging components
Example request for registering capability, be sure to replace placeholder values `<..>`
You need to register for the messageType and formatType which your integration supports

```bash
curl -v \
-H "Authorization: Bearer <AccessToken>" \
-H "content-type: application/json" \
-H "OrgId: <OrgId>" \
-H "AuthorizationContext: <AuthorizationContext>" \
-H "RequestId: <RequestId>" \
-X PATCH -d '{
    "capabilities": {
        "appType": "custom",
        "channelCapabilities": [
            {
                "channelType": "custom",
                "customIntegration": {
                    "customIntegrationType": "CustomChannelIntegration",
                    "integrationNamespace": "<orgId | developer namespace>",
                    "conversationChannelDefinitionDevName": "<ConversationChannelDefinition developer Name>"
                },
                "messageTypeCapabilities": [
                    {
                        "messageType": "StaticContentMessage",
                        "formatTypeCapabilities": [
                            {
                                "formatType": "Attachments",
                                "capabilityFieldRestriction": [
                                    {
                                        "fieldJsonPath": "$.staticContent.attachments.mimeType",
                                        "restriction": {
                                            "fieldRestrictionType": "MimeTypeRestriction",
                                            "mimeType": "image/png"
                                        }
                                    }
                                ]
                            }
                        ]
                    },
                    {
                        "messageType": "ChoicesMessage",
                        "formatTypeCapabilities": [
                            {
                                "formatType": "Buttons"
                            }
                        ]
                    },
                    {
                        "messageType": "FormMessage",
                        "formatTypeCapabilities": [
                            {
                                "formatType": "Inputs"
                            }
                        ]
                    }
                ]
            }
        ]
    }
}' http://localhost:8085/api/v1/capabilities
```

#### Acknowledgement API

Example request to acknowledge delivery / read of messages sent from agent. Be sure to replace placeholder values `<..>`

```bash
curl -v -H $'Authorization: Bearer <AccessToken>' -H 'AuthorizationContext: <AuthorizationContext>' -H 'RequestId: <RequestId>' -H 'OrgId: <OrgId>' -H 'Content-Type: application/json' -X POST -d "{
    \"sender\": {
      \"appType\" : \"<AppType>\",
      \"subject\" : \"<endUserClientId>\",
      \"role\" : \"<ParticipantRole>\"
    },
    \"conversationIdentifier\" : \"<conversation Identifier>\",
    \"acknowledgements\" : [ {
      \"acknowledgementTimestamp\" : <Timestamp in Unix Epoch>,
      \"acknowledgementType\" : \"<Acknowledgement Type>\",
      \"acknowledgedConversationEntryIdentifier\" : \"<Acknowledged Conversation Entry Identifier>\",
      \"acknowledgmentCreatedConversationEntryIdentifier\" : \"<Acknowledgment Created Conversation Entry Identifier>\"
    }, {..}, {..} . . . {..}]
}" http://localhost:8085/api/v1/acknowledgement
```

Note: Only maximum of 25 acknowledgments are supported per request

#### Conversation API

Example request for establishing a conversation, be sure to replace placeholder values `<..>`

```bash
curl -v \
-H $'Authorization: Bearer <AccessToken>' \
-H "content-type: application/json" \
-H "OrgId: <OrgId>" \
-H "AuthorizationContext: <AuthorizationContext>" \
-H "RequestId: <RequestId>" \
-X POST -d '{
  "channelAddressIdentifier": "<channelAddressIdentifier>",
  "participants": [
    {
      "subject": "<endUserClientIdentifier>",
      "role": "EndUser",
      "appType": "custom"
    }
  ]
}' http://localhost:8085/api/v1/conversation
```

#### ConversationHistory API

The API supports validation to ensure that partners don’t import conversations older than 24 hrs.
Additionally, the leftTime field is required for Chatbot participants.

Example request for sending text-based conversation history for establishing a new session, be sure to replace other placeholder values `<..>`

```bash
curl -v \
-H "Authorization: Bearer <AccessToken>" \
-H "content-type: application/json" \
-H "OrgId: <OrgId>" \
-H "AuthorizationContext: <AuthorizationContext>" \
-H "AuthorizationContextType: <AuthorizationContextType>" \
-H "RequestId: <RequestId>" \
-X POST -d "{
  \"channelAddressIdentifier\": \"<channelAddressIdentifier>\",
  \"conversationParticipants\": [
    {
      \"displayName\": \"<displayName>\",
      \"participant\": {
            \"subject\": \"<unique identifier for the participant>\",
            \"role\": \"<ParticipantRole>\",
            \"appType\": \"<AppType>\"
        },
      \"joinedTime\": <Timestamp in Unix Epoch Milliseconds>,
      \"leftTime\":<Timestamp in Unix Epoch>
    }
  ],
  \"conversationEntries\": [
    {
        \"clientTimestamp\": <Timestamp in Unix Epoch Milliseconds>,
        \"sender\": {
            \"subject\": \"<unique identifier for the sender>\",
            \"role\": \"<ParticipantRole>\",
            \"appType\": \"<AppType>\"
        },
        \"entryPayload\": {
            \"entryType\": \"Message\",
            \"id\": \"<payloadId>\",
            \"abstractMessage\": {
                \"messageType\": \"StaticContentMessage\",
                \"id\": \"<messageId>\",
                \"staticContent\": {
                    \"formatType\": \"Text\",
                    \"text\": \"Hi There\"
                }
            }
        }
    }
  ],
   \"messagingSession\": {
    \"messagingSessionRequestType\": \"EstablishMessagingSession\",
    \"payload\": {
      \"startTime\": \"<Timestamp in Unix Epoch>\",
      \"endTime\":  \"<Timestamp in Unix Epoch>\" //optional
    }
  }
}" http://localhost:8085/api/v1/conversationHistory
```

Example request for sending an attachment-based conversation history request for establishing a new session, be sure to replace other placeholder values `<..>`

```bash
curl -v \
-H "Authorization: Bearer <AccessToken>" \
-H "content-type: application/json" \
-H "OrgId: <OrgId>" \
-H "AuthorizationContext: <AuthorizationContext>" \
-H "AuthorizationContextType: <AuthorizationContextType>" \
-H "RequestId: <RequestId>" \
-X POST -d "{
  \"channelAddressIdentifier\": \"<channelAddressIdentifier>\",
  \"conversationParticipants\": [
    {
      \"displayName\": \"<displayName>\",
      \"participant\": {
            \"subject\": \"<unique identifier for the participant>\",
            \"role\": \"<ParticipantRole>\",
            \"appType\": \"<AppType>\"
        },
      \"joinedTime\":  <Timestamp in Unix Epoch Milliseconds>,
      \"leftTime\":<Timestamp in Unix Epoch>
    }
  ],
  \"conversationEntries\": [
    {
        \"clientTimestamp\":  <Timestamp in Unix Epoch Milliseconds>,
        \"sender\": {
            \"subject\": \"<unique identifier for the participant>\",
            \"role\": \"<ParticipantRole>\",
            \"appType\": \"<AppType>\"
        },
        \"entryPayload\": {
            \"entryType\": \"Message\",
            \"id\": \"<payloadId>\",
            \"abstractMessage\": {
                \"messageType\": \"StaticContentMessage\",
                \"inReplyToMessageId\": <inReplyToMessageId>,
                \"id\": \"<messageId>\",
                 \"references\": [{
                    \"recordId\": \"<ContentVersionId>\",
                    \"id\": \"<referenceId>\"
                  }],
                \"staticContent\": {
                    \"formatType\": \"Attachments\",
                    \"text\": <text>,
                    \"attachments\": [
                      {
                        \"name\": \"pdf-sample.pdf\",
                        \"attachmentUploadResult\": <attachmentUploadResult>,
                        \"id\": \"<attachmentId>\",
                        \"mimeType\": \"<supported-mimeTypes>\",
                        \"url\": \"<publicURl-of-uploaded-file>\",
                        \"referenceId\": \"<referenceId>\"
                      }
                    ]
                }
            }
        }
    }
  ],
   \"messagingSession\": {
    \"messagingSessionRequestType\": \"EstablishMessagingSession\",
    \"payload\": {
      \"startTime\": \"<Timestamp in Unix Epoch>\",
      \"endTime\":  \"<Timestamp in Unix Epoch>\" //optional
    }
  }
}" http://localhost:8085/api/v1/conversationHistory
```

Example request for sending text-based conversation history for an existing session, be sure to replace other placeholder values `<..>`

```bash
curl -v \
-H "Authorization: Bearer <AccessToken>" \
-H "content-type: application/json" \
-H "OrgId: <OrgId>" \
-H "AuthorizationContext: <AuthorizationContext>" \
-H "AuthorizationContextType: CONVERSATIONCHANNELDEFINITION" \
-H "RequestId: <RequestId>" \
-X POST -d '{
  "channelAddressIdentifier": "<channelAddressIdentifier>",
  "conversationParticipants": [
    {
      "displayName": "<displayName>",
      "participant": {
            "subject": "<participantSubject>",
            "role": "<participantRole>",
            "appType": "<participantAppType>"
        },
      "joinedTime":<Timestamp in Unix Epoch>
      "leftTime":<Timestamp in Unix Epoch>
    }
  ],
  "conversationEntries": [
    {
        "clientTimestamp": <Timestamp in Unix Epoch>,
        "sender": {
            "subject": "<senderSubject>",
            "role": "<senderRole>",
            "appType": "<senderAppType>"
        },
        "entryPayload": {
            "entryType": "<entryType>",
            "id": "<payloadId>",
            "abstractMessage": {
                "messageType": "<messageType>",
                "id": "<messageId>",
                "staticContent": {
                    "formatType": "<formatType>",
                    "text": "<text>"
                }
            }
        }
    }
  ],
   "messagingSession": {
    "messagingSessionRequestType": "AttachMessagingSession",
    "payload": {
      "sessionId": "<sessionId>"
    }
  }
}' http://localhost:8085/api/v1/conversationHistory
```

Example request for sending an attachment-based conversation history request for an existing session, be sure to replace other placeholder values `<..>`

```bash
curl -v \
-H "Authorization: Bearer <AccessToken>" \
-H "content-type: application/json" \
-H "OrgId: <OrgId>" \
-H "AuthorizationContext: <AuthorizationContext>" \
-H "AuthorizationContextType: <AuthorizationContextType>" \
-H "RequestId: <RequestId>" \
-X POST -d "{
  \"channelAddressIdentifier\": \"<channelAddressIdentifier>\",
  \"conversationParticipants\": [
    {
      \"displayName\": \"<displayName>\",
      \"participant\": {
            \"subject\": \"<unique identifier for the participant>\",
            \"role\": \"<ParticipantRole>\",
            \"appType\": \"<AppType>\"
        },
      \"joinedTime\":  <Timestamp in Unix Epoch Milliseconds>,
      \"leftTime\":<Timestamp in Unix Epoch>
    }
  ],
  \"conversationEntries\": [
    {
        \"clientTimestamp\":  <Timestamp in Unix Epoch Milliseconds>,
        \"sender\": {
            \"subject\": \"<unique identifier for the participant>\",
            \"role\": \"<ParticipantRole>\",
            \"appType\": \"<AppType>\"
        },
        \"entryPayload\": {
            \"entryType\": \"Message\",
            \"id\": \"<payloadId>\",
            \"abstractMessage\": {
                \"messageType\": \"StaticContentMessage\",
                \"inReplyToMessageId\": <inReplyToMessageId>,
                \"id\": \"<messageId>\",
                 \"references\": [{
                    \"recordId\": \"<ContentVersionId>\",
                    \"id\": \"<referenceId>\"
                  }],
                \"staticContent\": {
                    \"formatType\": \"Attachments\",
                    \"text\": <text>,
                    \"attachments\": [
                      {
                        \"name\": \"pdf-sample.pdf\",
                        \"attachmentUploadResult\": <attachmentUploadResult>,
                        \"id\": \"<attachmentId>\",
                        \"mimeType\": \"<supported-mimeTypes>\",
                        \"url\": \"<publicURl-of-uploaded-file>\",
                        \"referenceId\": \"<referenceId>\"
                      }
                    ]
                }
            }
        }
    }
  ],
   \"messagingSession\": {
    \"messagingSessionRequestType\": \"AttachMessagingSession\",
    \"payload\": {
      \"sessionId\":\"<sessionId>\"
    }
  }
}" http://localhost:8085/api/v1/conversationHistory
```
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
        string: '{"conversationIdentifier":"9bc16b48-cd9b-4d8b-b7f2-a376f0a05c09","channelAddressIdentifier":"b0ffeafe-0d89-4338-b14a-172ad203f22a","recipient":{"appType":"custom","subject":"David Wood","role":"EndUser"},"payload":{"senderDisplayName":"John Dow","identifier":"80bb78b1-8240-4e22-ac55-5d517231ca1e","entryType":"Message","entryPayload":{"entryType":"Message","id":"80bb78b1-8240-4e22-ac55-5d517231ca1e","abstractMessage":{"messageType":"StaticContentMessage","id":"80bb78b1-8240-4e22-ac55-5d517231ca1e","staticContent":{"formatType":"Attachments","text":"did you get my file","attachments":[{"name":"BYO-Middleware-Impl.png","attachmentUploadResult":null,"id":"4f3c9d65-acd6-4151-8ac7-6ae1ae46531a","mimeType":"image/png","url":"https://byoccom4-dev-ed--c.stmfa.stm.documentforce.com/sfc/dist/version/download/?oid=00DRM000000O8Qy&ids=068RM0000002ACb&d=%2Fa%2FRM00000000uI%2FcEus0p20sO6Kwp6NKwMI2WeprWXvR9ke7Zp7f0_MTrE&asPdf=false","referenceId":"1e544040-52c8-40f6-a14d-5bc123c5cae4"}]}},"messageReason":null},"sender":{"appType":"agent","subject":"005RM000002ks3p","role":"Agent"},"transcriptedTimestamp":1690679469883,"clientTimestamp":1690679468868,"clientDuration":0}}'
      },
      my__event__EventType__c: anonymous { string: 'Interaction' }
    }
  }
}
```

Note: In the payload examples above, the key "my__event__e" is the developer name for the outbound message customer event configured in salesforce setup. The keys "my__event__chnlAddrIdField__c", "my__event__payloadField__c", "my__event__recipientField__c" and "my__event__EventType__c" are corresponding custom fields "Custom Event Channel Address Id Field", "Custom Event Payload Field", "Custom Event Recipient Field", "Custom event type Field" defined in the outbound message customer event.

### The RoutingRequested event 
```
{
    "channelAddressIdentifier": "afc64d6f-ad61-4df2-82f7-b2fa83540c78",
    "recipient": {
        "appType": "custom",
        "subject": "meu003",
        "role": "EndUser"
    },
    "payload": {
        "conversationIdentifier": "06f6c69b-bf42-4ec2-a6fa-9ae595218ea0",
        "routingParameters": {
            "queueId": null,
            "channelData": "{\"eventId\":\"d3bdacb1-49b4-4fbd-9adb-919319631db4\",\"routerParticipantSubject\":\"cb1e4929-d5fd-41a1-9478-a420667f58c8\",\"routingType\":\"Transfer\",\"conversationId\":\"06f6c69b-bf42-4ec2-a6fa-9ae595218ea0\"}",
            "isTransfer": true,
            "routingConfigId": null,
            "userId": null,
            "isFallbackOnPsrNotCreated": null,
            "serviceChannelId": null,
            "isEstimatedWaitTimeRequested": null,
            "skillsList": [],
            "botId": null,
            "flowId": "300SG00000jJder",
            "isScrt2": true,
            "isUserRequired": null
        },
        "routingType": "Transfer",
        "workRecordId": null
    }
}
```

## Additional Notes
Use of the code in this repository with Salesforce products or services should be used in accordance with any applicable developers guides on [developer.salesforce.com](https://developer.salesforce.com/) and may be subject to additional terms of use, including but not limited to the [Salesforce Program Agreement - Program Terms for the Salesforce Developers Program](https://www.salesforce.com/company/program-agreement/#devs).

