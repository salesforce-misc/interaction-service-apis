# Interaction API error codes

Error codes in the range 10001–19999 are client errors (4xx HTTP status) and codes in the range 20001–29999 are service errors (5xx HTTP status).

## Client Errors
| Error Code | Description                                                                                                                                                                 |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 10001      | The AuthorizationContext in the request isn't valid for the provided value in the "to"/"channelAddressIdentifier" field.                                                    |
| 10002      | The channel type for the provided "to"/"channelAddressIdentifier" field isn't valid.                                                                                        |
| 10003      | The supplied Authorization access token is missing the required Connected App scope. Required value: interaction_api.                                                       |
| 10004      | The supplied Authorization access token is not valid.                                                                                                                       |
| 10005      | The AuthorizationContext for the request isn't valid based on the supplied Authorization access token.                                                                      |
| 10006      | The channel provider for the provided "to"/"channelAddressIdentifier" field isn't valid.                                                                                    |
| 10007      | The file ID field is empty.                                                                                                                                                 |
| 10008      | The file size exceeds the maximum limit of 25 MB.                                                                                                                           |
| 10009      | The file is empty or doesn’t exist.                                                                                                                                         |
| 10010      | The interaction contentLength field is empty.                                                                                                                               |
| 10011      | The number of interactions of type AttachmentInteraction does not match the number of attachment files.                                                                     |
| 10012      | Enter a valid workItemId for the MessagingSession ID.                                                                                                                       |
| 10013      | Enter either a capacityPercentage or a capacityWeight, but not both.                                                                                                        |
| 10014      | Enter a capacity weight greater than or equal to 0.                                                                                                                         |
| 10015      | Enter a capacity percentage between 0 and 100, inclusive.                                                                                                                   |
| 10016      | Enter a valid header.                                                                                                                                                       |
| 10017      | The channelAddressIdentifier provided does not exist.                                                                                                                       |
| 10018      | The conversation identifier is not valid.                                                                                                                                   |
| 10019      | The service is in read-only mode because an org migration is in progress. Try again later.                                                                                  |
| 10020      | The endUserClientId provided is not valid.                                                                                                                                  |
| 10021      | Enter a valid Consent Status Enum value.                                                                                                                                    |
| 10022      | Cannot update Consent Status where the Consent Owner of the channel is not set to Partner.                                                                                  |
| 10023      | Cannot update Consent Status where the SupportsKeywords of the channel is true.                                                                                             |
| 10024      | For Flow based Routing, flow ID and Fallback Queue ID are required fields.                                                                                                  |
| 10025      | The conversation id isn't found based on channelAddressIdentifier and CUSTOM channel type.                                                                                  |
| 10026      | The conversation channel definition for the provided "to"/"channelAddressIdentifier" field is missing.                                                                      |
| 10027      | The provided "to" field isn't valid.                                                                                                                                        |
| 10028      | The provided "from" field isn't valid.                                                                                                                                      |
| 10029      | The provided interaction list isn't valid.                                                                                                                                  |
| 10030      | Specify either a flow or a queue to Route API.                                                                                                                              |
| 10031      | The channelAddressIdentifier provided is not active.                                                                                                                        |
| 10032      | Specified Queue ID is invalid.                                                                                                                                              |
| 10033      | Should not pass in error message when routing result success.                                                                                                               |
| 10034      | Error message missing when routing result false.                                                                                                                            |
| 10035      | The messaging channel is not a type of CUSTOM.                                                                                                                              |
| 10036      | The participant does not exist.                                                                                                                                             |
| 10037      | The AppType provided is not supported.                                                                                                                                      |
| 10038      | The Participant Role provided is not supported.                                                                                                                             |
| 10039      | Add participants to your conversation.                                                                                                                                      |
| 10042      | Include one end user participant in the conversation.                                                                                                                       |
| 10043      | Invalid participant role. Specify a Chatbot or EndUser role for all participants.                                                                                           |
| 10044      | Include only one end user participant in the conversation.                                                                                                                  |
| 10045      | Provide a valid message entry format.                                                                                                                                       |
| 10046      | Invalid clientTimestamp. Send messages within the allowed response time frame.                                                                                              |
| 10047      | Specified Message Type is invalid.                                                                                                                                          |
| 10048      | Specified Format Type is invalid.                                                                                                                                           |
| 10049      | Specified Message Type and Format Type combination is invalid.                                                                                                              |
| 10050      | Specified custom integration is invalid.                                                                                                                                    |
| 10051      | The file size (in bytes) of uploaded attachment is incorrect.                                                                                                               |
| 10052      | The "channelAddressIdentifier" field set in post conversation request isn't valid.                                                                                          |
| 10053      | The "participants" field set in post conversation request is either invalid or empty.                                                                                       |
| 10054      | Invalid subject of participant in conversation request.                                                                                                                     |
| 10055      | Conversation precondition failed. Please retry.                                                                                                                             |
| 10056      | Conversation processing aborted. Please retry.                                                                                                                              |
| 10057      | Looks like you don't have access to the ConversationHistory API. Only Bring Your Own Channel for CCaaS partners can access them. Your Salesforce admin can help with that.  |
| 10058      | Customer Error occurred.                                                                                                                                                    |
| 10059      | Interaction ID or Message ID is empty.                                                                                                                                      |
| 10060      | Interaction ID and Message ID are mismatched.                                                                                                                               |
| 10061      | The StartTime provided for messaging session is invalid.                                                                                                                    |
| 10062      | Invalid message type. Only StaticContentMessage, ChoicesResponseMessage, and FormResponseMessage types are supported for messages sent from an end user.                    |
| 10063      | Your environment isn't ready to use the ConversationHistory API with a messaging session. Remove the messagingSession parameter from the payload.                           |
| 10064      | The EndTime provided for messaging session is invalid.                                                                                                                      |
| 10065      | Invalid end-user participant details. Conversations with end-users remain open and don't have a leftTime.                                                                   |
| 10066      | Invalid chatbot participant details. leftTime is required for conversations with chatbots when the conversation is ended.                                                   |
| 10067      | Invalid joinedTime. Specify a joinedTime that is earlier than the leftTime.                                                                                                 |
| 10068      | Invalid participant. The message isn't from an approved participant.                                                                                                        |
| 10069      | Specify a valid channel address identifier or check your authorization configuration.                                                                                       |
| 10070      | Provide a channel address identifier that's configured with the specified authorization context.                                                                            |
| 10071      | Session is not found.                                                                                                                                                       |
| 10072      | Specify a valid session ID.                                                                                                                                                 |
| 10073      | Invalid message ID. Provide a unique ID for each message entry.                                                                                                             |
| 10074      | Invalid message timestamp. The message Timestamp must occur while the sender is in the conversation.                                                                        |
| 10075      | Your Salesforce org isn't ready to use the Routing API with conferencing. Upgrade to the latest version or set the routingType to Initial or Transfer.                      |
| 10076      | Your Salesforce org isn't ready to use the Delete Route API with cancel reason. Upgrade to the latest version or remove cancelReason from the payload.                      |
| 10077      | Your Salesforce org isn't ready to use the Agent Work API with agent action as conference. Upgrade to the latest version or set the agentAction to Transfer.                |
| 10078      | An error occurred due to an invalid payload parameter. Check the parameters and try again. For help, see Send Historical Conversation Entries.                              |
| 10082      | Invalid workId: Please provide a valid workId.                                                                                                                              |
| 10083      | Invalid context type: The context type provided is not supported or is invalid.                                                                                             |
| 10084      | Invalid botId: Please provide a valid botId.                                                                                                                                |
| 10085      | Invalid status: Please provide a valid status.                                                                                                                              |
| 10086      | The ExternalBotId is invalid.                                                                                                                                               |
| 10087      | The AuthorizationContext in the request isn't valid for the given botId.                                                                                                    |
| 10088      | The bot is inactive and can't be used for conversations.                                                                                                                    |
| 10089      | Invalid parameters passed to API.                                                                                                                                           |
| 10090      | The StartTime provided to fetch Conversation Entries is invalid.                                                                                                            |
| 10091      | Invalid EndTime. Provide an EndTime that is after the StartTime.                                                                                                            |
| 10092      | Invalid Limit. Provide a value within the allowed range and try again.                                                                                                      |
| 10093      | Invalid authorization context type.                                                                                                                                         |
| 10094      | The request failed due to a timeout from a downstream service. Please try again later.                                                                                      |
| 10095      | The MIME type of the uploaded file cannot be determined. Please check the file and try again.                                                                               |
| 10096      | The channel variable value length in routing attribute payload is larger than the maximum length configured in Messaging Channel setup page.                                |
| 10097      | Invalid operation. Please provide a valid operation.                                                                                                                        |
| 10098      | The progressIndicatorCapability parameter can't be empty. Please provide a valid value or remove the parameter from the payload and try again.                              |
| 10099      | Org does not have correct permissions.                                                                                                                                      |
| 10100      | A request body is required and can't be empty. Provide a valid payload and ensure data is included and correctly formatted.                                                 |
| 10101      | We found duplicate participants. Make sure each participant is unique and try again.                                                                                        |
| 10102      | Invalid operation type value for POST messaging session request.                                                                                                            |
| 10103      | Invalid operationBy value for POST messaging session.                                                                                                                       |
| 10104      | No access to MessagingSession record.                                                                                                                                       |
| 10105      | End User not found.                                                                                                                                                         |
| 10106      | Messaging Session ID is missing for Inactivate Messaging Session API.                                                                                                       |
| 10107      | Invalid chatbot prefix. Chatbots with the '1JZ' prefix aren't allowed.                                                                                                      |
| 10108      | Inactivate operation requires operationBy to be SYSTEM, if provided.                                                                                                        |
| 10109      | The file ID field must not exceed 36 characters.                                                                                                                            |
| 15102      | To use this API, enable Contact Center as a Service.                                                                                                                        |
| 15103      | The Messaging End User is not Fully Opted In                                                                                                                                |
| 15104      | Messaging channel is not linked to any Contact Center                                                                                                                       |
| 15105      | To use this API, add Partner Messaging to the user.                                                                                                                         |
| 15106      | Invalid User Id parameter                                                                                                                                                   |
| 15107      | Enter a conversationId that matches a conversation linked to workItemId \<workItemId\>. conversationId \<conversationId\> doesn't match.                                    |
| 15108      | No service channel was found for this messaging session.                                                                                                                    |
| 15109      | An active AgentWork record already exists with the same UserId and WorkItemId.                                                                                              |
| 15110      | Enter a valid workItemId \<workItemId\> for the MessagingSession ID.                                                                                                        |
| 15111      | WorkItemId \<workItemId\> for the MessagingSession ID not found.                                                                                                            |
| 15112      | The agent's status is not associated with the channel for this work.                                                                                                        |
| 15113      | Conversation Id is required.                                                                                                                                                |
| 15114      | Invalid Uuid found for conversation participant.                                                                                                                            |
| 15115      | Conversation not found.                                                                                                                                                     |
| 15116      | Conversations not found.                                                                                                                                                    |
| 15117      | Some conversations not found.                                                                                                                                               |
| 15118      | Conversation has open sessions.                                                                                                                                             |
| 15119      | Conversation was deleted.                                                                                                                                                   |
| 15120      | A conversation participant was deleted for the conversation.                                                                                                                |
| 15121      | platformKey is required to lookup ConversationChannel.                                                                                                                      |
| 15122      | platformKey or channel address identifier is required to lookup ConversationChannel.                                                                                        |
| 15123      | channelType is required to lookup ConversationChannel.                                                                                                                      |
| 15124      | Channel does not exist.                                                                                                                                                     |
| 15125      | Channel id is missing from conversation.                                                                                                                                    |
| 15126      | App is required.                                                                                                                                                            |
| 15127      | Conversation persisted id is invalid.                                                                                                                                       |
| 15128      | At least one conversation entry must be provided to update.                                                                                                                 |
| 15129      | Device Id is required.                                                                                                                                                      |
| 15130      | Device Id is invalid.                                                                                                                                                       |
| 15131      | Device Token is invalid.                                                                                                                                                    |
| 15132      | Device limit must be greater than 0.                                                                                                                                        |
| 15133      | ListDevicesFilter is invalid.                                                                                                                                               |
| 15134      | More than one device exists.                                                                                                                                                |
| 15135      | More than one conversation exists.                                                                                                                                          |
| 15136      | Participant is required.                                                                                                                                                    |
| 15137      | Participant not found.                                                                                                                                                      |
| 15138      | Participant subject is required.                                                                                                                                            |
| 15139      | Participant role is required.                                                                                                                                               |
| 15140      | Participant appType is required.                                                                                                                                            |
| 15141      | No participants exist in the conversation.                                                                                                                                  |
| 15142      | Participant not part of conversation.                                                                                                                                       |
| 15143      | Failed to get participant entity Id.                                                                                                                                        |
| 15144      | Participant entity Id is required.                                                                                                                                          |
| 15145      | Participant entity not found.                                                                                                                                               |
| 15146      | Participant entity is in open sessions.                                                                                                                                     |
| 15147      | Participant entity has restricted child objects that is blocking deletion. Please delete them first.                                                                        |
| 15148      | Device Push Service Provider is required.                                                                                                                                   |
| 15149      | Device Push Service Provider is invalid.                                                                                                                                    |
| 15150      | Invalid request.                                                                                                                                                            |
| 15151      | Unable to route conversation.                                                                                                                                               |
| 15152      | Omni validation failed.                                                                                                                                                     |
| 15153      | Routing failed due to an unhandled exception. Please check the exception message details for more information.                                                              |
| 15154      | Conference routing failed due to an unhandled exception. Please check the exception message details for more information.                                                   |
| 15155      | No routing information available for this conversation.                                                                                                                     |
| 15156      | Nothing to cancel routing on!                                                                                                                                               |
| 15157      | TYPEOF Org Perm not enabled.                                                                                                                                                |
| 15158      | Batch size must be greater than 0.                                                                                                                                          |
| 15159      | Invalid Delete batch size.                                                                                                                                                  |
| 15160      | Invalid Query batch size.                                                                                                                                                   |
| 15161      | Invalid number of processing threads.                                                                                                                                       |
| 15162      | Unauthorized API Access.                                                                                                                                                    |
| 15163      | To create agent work, enter a channelData value.                                                                                                                            |
| 15164      | Enter either a capacityPercentage or a capacityWeight, but not both.                                                                                                        |
| 15165      | There is no messaging session to route.                                                                                                                                     |
| 15166      | No Messaging End user was found.                                                                                                                                            |
| 15167      | Invalid Consent Status provided.                                                                                                                                            |
| 15168      | Invalid messaging channel id or messaging channel is not a type of custom!                                                                                                  |
| 15169      | Messaging End User is not Fully Opted In.                                                                                                                                   |
| 15170      | Failed to establish connection. The requested Org is inactive.                                                                                                              |
| 15171      | Conversation Channel Definition Id is missing.                                                                                                                              |
| 15172      | Conversational capability provided is invalid.                                                                                                                              |
| 15173      | Invalid external Bot Id.                                                                                                                                                    |
| 15174      | Invalid AgentWork Id parameter.                                                                                                                                             |
| 15175      | Invalid AgentWork status.                                                                                                                                                   |
| 15176      | Invalid MessagingChannel type.                                                                                                                                              |
| 15177      | Conversation Participant UUIDs List is required.                                                                                                                            |
| 15178      | AgentWork with Bot routing is not supported.                                                                                                                                |
| 15179      | The channel type provided does not match the existing conversation's channel type.                                                                                          |
| 15180      | The channel platform key provided does not match the existing conversation's channel platform key.                                                                          |
| 15181      | The channel address identifier provided does not match the existing channel conversation's address identifier.                                                              |
| 15182      | Invalid request.                                                                                                                                                            |
| 15183      | Conversation does not exist.                                                                                                                                                |
| 15184      | Invalid User Id parameter.                                                                                                                                                  |
| 15185      | Routing config failed to convert to routing parameter!                                                                                                                      |
## Service Errors
| Error Code | Description                                                                                                                                                                |
|------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 20002      | An error occured when retrieve conversation list based on endUserAddress.                                                                                                  |
| 20003      | Empty interaction list in request.                                                                                                                                         |
| 20004      | Pod not found error occurred when fetch Org perms.                                                                                                                         |
| 25005      | We couldn't find the Messaging End User for workItemId \<workItemId\>. Verify that the Messaging Session exists, then try again.                                           |
| 25006      | We couldn't find the conversation for workItemId \<workItemId\>. Verify that the conversation exists, then try again.                                                      |
| 25007      | We couldn't find conversationId for workItemId \<workItemId\>. Verify that the conversation exists, then try again.                                                        |
| 25008      | Invalid Conversation Entity Ids.                                                                                                                                           |
| 25009      | Participant joinedTimestamp is missing.                                                                                                                                    |
| 25010      | Unexpected error occurred.                                                                                                                                                 |
| 25011      | Routing channel hook threw exception!                                                                                                                                      |
| 25012      | Routing hook failed. Retry.                                                                                                                                                |
| 25013      | Cancel routing hook threw exception!                                                                                                                                       |
| 25014      | Failed to cancel routing!                                                                                                                                                  |
| 25015      | Routing not canceled!                                                                                                                                                      |
| 25016      | Failed to get routing context params!                                                                                                                                      |

## Route API Routing Errors

`POST /v1/route` returns HTTP 202 when the request was accepted but the conversation could not be routed.
In that case, `success` is `false` and the `errors` object describes the failure:

| Field          | Description                                                                             |
|----------------|-----------------------------------------------------------------------------------------|
| `apiErrorCode` | Stable error code. Use this value for handling logic.                                   |
| `errorType`    | Error category: `BadRequest`, `Error`, `INVALID_STATE`, or `RETRYABLE_INTERNAL_ERROR`.  |
| `errorMessage` | Details for logging and troubleshooting. The text can change; don't parse it.           |

Requests that fail validation or authorization return a 4xx or 5xx HTTP status with a numeric code listed in the sections above.

| apiErrorCode                | errorType                | Description                                                                                         | Retry                                                          |
|-----------------------------|--------------------------|-----------------------------------------------------------------------------------------------------|----------------------------------------------------------------|
| ROUTING_ALREADY_IN_PROGRESS | INVALID_STATE            | A routing attempt is already in progress for this conversation.                                     | No. Cancel routing with DELETE /v1/route before routing again. |
| INTERNAL_SERVER_ERROR       | RETRYABLE_INTERNAL_ERROR | A temporary error occurred while processing the routing request.                                    | Yes, with exponential backoff (max 3).                         |
| CONVERSATION_ID_INVALID     | Error                    | The conversation doesn't exist.                                                                     | No                                                             |
| CONVERSATION_DELETED        | Error                    | The conversation was deleted.                                                                       | No                                                             |
| CHANNEL_DOES_NOT_EXIST      | BadRequest               | The conversation channel couldn't be found.                                                         | No                                                             |
| NO_ROUTING_INFO_AVAILABLE   | Error                    | No routing information is available for this conversation's channel.                                | No                                                             |
| NO_SESSION_FOUND_TO_ROUTE   | Error                    | The conversation has no open messaging session to route.                                            | No                                                             |
| NO_MESSAGING_END_USER_FOUND | BadRequest               | The Messaging End User couldn't be found.                                                           | No                                                             |
| USER_NOT_FULLY_OPTED_IN     | BadRequest               | The Messaging End User isn't fully opted in.                                                        | No                                                             |
| INVALID_ROUTING_CONFIG      | BadRequest               | The routing configuration (queue, flow, bot, user, or skills) isn't valid.                          | No. Correct the request.                                       |
| ROUTING_VALIDATION_FAILED   | BadRequest               | Omni-Channel couldn't route the work with the given configuration. See `errorMessage` for details.  | No. Review the routing setup.                                  |
| ROUTING_FAILED              | BadRequest               | Routing failed because of an unexpected error. See `errorMessage` for details.                      | No                                                             |
| ROUTING_HOOK_ERROR          | Error                    | The channel couldn't prepare the routing request.                                                   | No                                                             |
| UNEXPECTED_ERROR            | Error                    | An unexpected error occurred.                                                                       | Once. If it persists, contact Salesforce Support.              |
| Any other value             | —                        | Unrecognized error.                                                                                 | No. Contact Salesforce Support.                                |

## Other Errors
| Error Code | Description                                                                                                                                                                 |
|------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 500        | An internal server error occurred. Please contact Salesforce Support.                                                                                                       |
