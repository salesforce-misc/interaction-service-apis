# Interaction API error codes

## Client Errors
| Error Code | Description |
|--|--|
| 10001 | The AuthorizationContext in the request isn't valid for the provided value in the "to"/"channelAddressIdentifier" field.|
| 10002 | The channel type for the provided "to"/"channelAddressIdentifier" field isn't valid.|
| 10003 | The supplied Authorization access token is missing the required Connected App scope. Required value: interaction_api.|
| 10004 | The supplied Authorization access token is not valid.|
| 10005 | The AuthorizationContext for the request isn't valid based on the supplied Authorization access token.|
| 10006 | The channel provider for the provided "to"/"channelAddressIdentifier" field isn't valid.|
| 10007 | The file ID field is empty.|
| 10008 | The file size exceeds the maximum limit of 25 MB.|
| 10009 | The file is empty or doesn’t exist.|
| 10010 | The interaction contentLength field is empty.|
| 10011 | The number of interactions of type AttachmentInteraction does not match the number of attachment files.|
| 10012 | Enter a valid workItemId for the MessagingSession ID.|
| 10013 | Enter either a capacityPercentage or a capacityWeight, but not both.|
| 10014 | Enter a capacity weight greater than or equal to 0.|
| 10015 | Enter a capacity percentage between 0 and 100, inclusive.|
| 10016 | Enter a valid header.|
| 10017 | The channelAddressIdentifier provided does not exist.|
| 10018 | The conversation identifier is not valid.|
| 10019 | The service is in read-only mode because an org migration is in progress. Try again later.|
| 10020 | The endUserClientId provided is not valid.|
| 10021 | Enter a valid Consent Status Enum value.|
| 10022 | Cannot update Consent Status where the Consent Owner of the channel is not set to Partner.|
| 10023 | Cannot update Consent Status where the SupportsKeywords of the channel is true.|
| 10024 | For Flow based Routing, flow ID and Fallback Queue ID are required fields.|
| 10025 | The conversation id isn't found based on channelAddressIdentifier and CUSTOM channel type.|
| 10026 | The conversation channel definition for the provided "to"/"channelAddressIdentifier" field is missing.|
| 10027 | The provided "to" field isn't valid.|
| 10028 | The provided "from" field isn't valid.|
| 10029 | The provided interaction list isn't valid.|
| 10030 | Specify either a flow or a queue to Route API.|
| 10031 | The channelAddressIdentifier provided is not active.|
| 10032 | Specified Queue ID is invalid.|
| 10033 | Should not pass in error message when routing result success.|
| 10034 | Error message missing when routing result false.|
| 10035 | The messaging channel is not a type of CUSTOM.|
| 10036 | The participant does not exist.|
| 10037 | The AppType provided is not supported.|
| 10038 | The Participant Role provided is not supported.|
| 10039 | Add participants to your conversation.|
| 10042 | Include one end user participant in the conversation.|
| 10043 | Invalid participant role. Specify a Chatbot or EndUser role for all participants.|
| 10044 | Include only one end user participant in the conversation.|
| 10045 | Provide a valid message entry format.|
| 10046 | Invalid clientTimestamp. Send messages within the allowed response time frame.|
| 10047 | Specified Message Type is invalid.|
| 10048 | Specified Format Type is invalid.|
| 10049 | Specified Message Type and Format Type combination is invalid.|
| 10050 | Specified custom integration is invalid.|
| 10051 | The file size (in bytes) of uploaded attachment is incorrect.|
| 10052 | The "channelAddressIdentifier" field set in post conversation request isn't valid.|
| 10053 | The "participants" field set in post conversation request is either invalid or empty.|
| 10054 | Invalid subject of participant in conversation request.|
| 10055 | Conversation precondition failed. Please retry.|
| 10056 | Conversation processing aborted. Please retry.|
| 10057 | Looks like you don't have access to the ConversationHistory API. Only Bring Your Own Channel for CCaaS partners can access them. Your Salesforce admin can help with that.|
| 10058 | Customer Error occurred.|
| 10059 | Interaction ID or Message ID is empty.|
| 10060 | Interaction ID and Message ID are mismatched.|
| 10061 | The StartTime provided for messaging session is invalid.|
| 10062 | Invalid message type. Only StaticContentMessage, ChoicesResponseMessage, and FormResponseMessage types are supported for messages sent from an end user.|
| 10063 | Your environment isn't ready to use the ConversationHistory API with a messaging session. Remove the messagingSession parameter from the payload.|
| 10064 | The EndTime provided for messaging session is invalid.|
| 10065 | Invalid end-user participant details. Conversations with end-users remain open and don't have a leftTime.|
| 10066 | Invalid chatbot participant details. leftTime is required for conversations with chatbots when the conversation is ended.|
| 10067 | Invalid joinedTime. Specify a joinedTime that is earlier than the leftTime.|
| 10068 | Invalid participant. The message isn't from an approved participant.|
| 10069 | Specify a valid channel address identifier or check your authorization configuration.|
| 10070 | Provide a channel address identifier that's configured with the specified authorization context.|
| 10071 | Specify a valid session ID.|
| 10072 | Specify a valid session ID.|
| 10073 | Invalid message ID. Provide a unique ID for each message entry.|
| 10074 | Invalid message timestamp. The message Timestamp must occur while the sender is in the conversation.|
| 10075 | Your Salesforce org isn't ready to use the Routing API with conferencing. Upgrade to the latest version or set the routingType to Initial or Transfer.|
| 10076 | Your Salesforce org isn't ready to use the Delete Route API with cancel reason. Upgrade to the latest version or remove cancelReason from the payload.|
| 10077 | Your Salesforce org isn't ready to use the Agent Work API with agent action as conference. Upgrade to the latest version or set the agentAction to Transfer.|
| 10078 | An error occurred due to an invalid payload parameter. Check the parameters and try again. For help, see Send Historical Conversation Entries.|
| 10082 | Invalid workId: Please provide a valid workId.|
| 10083 | Invalid context type: The context type provided is not supported or is invalid.|
| 10084 | Invalid botId: Please provide a valid botId.|
| 10085 | Invalid status: Please provide a valid status.|
| 10086 | The ExternalBotId is invalid.|
| 10087 | The AuthorizationContext in the request isn't valid for the given botId.|
| 10088 | The bot is inactive and can't be used for conversations.|
| 10089 | Invalid parameters passed to API.|
| 10090 | The StartTime provided to fetch Conversation Entries is invalid.|
| 10091 | Invalid EndTime. Provide an EndTime that is after the StartTime.|
| 10092 | Invalid Limit. Provide a value within the allowed range and try again.|
| 10093 | Invalid authorization context type.|
| 10094 | The request failed due to a timeout from a downstream service. Please try again later.|
| 10095 | The MIME type of the uploaded file cannot be determined. Please check the file and try again.|
| 10096 | The channel variable value length in routing attribute payload is larger than the maximum length configured in Messaging Channel setup page.|
| 10097 | Invalid operation. Please provide a valid operation for Participant API.|
| 10098 | The progressIndicatorCapability parameter can't be empty. Please provide a valid value or remove the parameter from the payload and try again.
| 15102 | To use this API, enable Contact Center as a Service.|
| 15103 | The Messaging End User is not Fully Opted In|
| 15104 | Messaging channel is not linked to any Contact Center|
| 15105 | To use this API, add Partner Messaging to the user.|
| 15106 | Invalid User Id parameter|
| 15107 | Enter a conversationId that matches a conversation linked to workItemId \<workItemId\>. conversationId \<conversationId\> doesn't match.|
| 15108 | No service channel was found for this messaging session.|
| 15109 | An active AgentWork record already exists with the same UserId and WorkItemId.|
| 15110 | Enter a valid workItemId \<workItemId\> for the MessagingSession ID.|
| 15111 | WorkItemId \<workItemId\> for the MessagingSession ID not found.|
| 15112 | The agent's status is not associated with the channel for this work.|
## Service Errors
| Error Code | Description |
|--|--|
| 20002 | An error occured when retrieve conversation list based on endUserAddress.|
| 20003 | Empty interaction list in request.|
| 20004 | Pod not found error occurred when fetch Org perms.|
| 25005 | We couldn't find the Messaging End User for workItemId \<workItemId\>. Verify that the Messaging Session exists, then try again.|
| 25006 | We couldn't find the conversation for workItemId \<workItemId\>. Verify that the conversation exists, then try again.|
| 25007 | We couldn't find conversationId for workItemId \<workItemId\>. Verify that the conversation exists, then try again.|

## Other Errors
| Error Code | Description |
|--|--|
| 500 | An internal server error occured. Please contact Salesforce Support.|
