# Interaction API error codes

## Client Errors
| Error Code | Description |
|--|--|
| *10001* | The "to" value is invalid. Set the value to the ConversationChannelDefinition developer name in the format *{prefix}_{ConnectedAppName}*. The prefix must match the developer name prefix of the corresponding connected app.|
| *10002* | The channel type for the provided *"to":"\<channelAddressIdentifier\>"* field isn't valid. |
| *10003* | The supplied Authorization access token is missing the required Connected App scope. Required value: interaction_api. |
| *10004* | The supplied Authorization access token is not valid. |
| *10005* | The AuthorizationContext for the request isn't valid based on the supplied Authorization access token. |
| *10006* | The channel provider for the provided *"to":"\<channelAddressIdentifier\>"* field isn't valid |
| *10007* | The file ID field is empty|
| *10008* | The file size exceeds the maximum limit of 25 MB|
| *10009* | The file is empty or doesn’t exist|
| *10010* | The 'contentLength' field in interaction request payload is empty.|
| *10011* | The number of interactions of type AttachmentInteraction does not match the number of attachment files|
| *10012* | Specify a valid workItemId for the MessagingSession ID.|
| *10013* | Specify either a capacityPercentage or a capacityWeight, but not both.|
| *10014* | Specify a capacity weight greater than or equal to 0.|
| *10015* | Specify a capacity percentage between 0 and 100, inclusive.|
| *10016* | Specify a valid header.|
| *10017* | The channelAddressIdentifier provided does not exist.|
| *10018* | The conversation identifier is not valid.|
| *10019* | The service is in read-only mode because an org migration is in progress. Try again later.|
| *10020* | The endUserClientId provided is not valid|
| *10021* | Specify a valid Consent Status Enum value|
| *10022* | Cannot update Consent Status where the Consent Owner of the channel is not set to Partner|
| *10023* | Cannot update Consent Status where the SupportsKeywords of the channel is true|
| *10024* | For Flow based Routing, flow ID and Fallback Queue ID are required fields|
| *10025* | The conversation id isn't found based on channelAddressIdentifier and CUSTOM channel type.|
| *10026* | The conversaction channel defnition for the provided *"to":"\<channelAddressIdentifier\>"* field is missing.|
| *10027* | The provided *"to":"\<channelAddressIdentifier\>"* field isn't valid.|
| *10028* | The provided *"from:"\<endUser\>""* field isn't valid.|
| *10029* | The provided interaction list isn't valid.|
| *10030* | Specify either a flow or a queue to Route API|
| *10031* | The channelAddressIdentifier provided is not active.|
| *10032* | Specified Queue ID is invalid|
| *10033* | Should not pass in error message when routing result success.|
| *10034* | Error message missing when routing result false.|
| *10035* | The messaging channel is not a type of CUSTOM.|
| *10036* | The participant does not exist|
| *10037* | The AppType provided is not supported|
| *10038* | The Participant Role provided is not supported|
| *10039* | Add participants to your conversation.|
| *10040* | Provide a participant role.|
| *10041* | Provide a participant subject.|
| *10042* | Include one end user participant in the conversation.|
| *10043* | Invalid participant role. Specify a Chatbot or EndUser role for all participants.|
| *10044* | Include only one end user participant in the conversation.|
| *10045* | Provide a valid message entry format.|
| *10046* | Invalid clientTimestamp. Send messages within the allowed response time frame.|
| *10047* | Specified Message Type is invalid|
| *10048* | Specified Format Type is invalid|
| *10049* | Specified Message Type and Format Type combination is invalid|
| *10050* | Specified custom integration is invalid|
| *10051* | The file size (in bytes) of uploaded attachment is incorrect.|
| *10052* | The "channelAddressIdentifier" field set in post conversation request isn't valid.|
| *10053* | The "participants" field set in post conversation request is either invalid or empty.|
| *10054* |Invalid subject of participant in conversation request.|
| *10055* | Conversation precondition failed. Please retry. |
| *10056* | Conversation processing aborted. Please retry. |
| *10057* | Looks like you don't have access to the ConversationHistory and AttachmentHistory APIs. Only Bring Your Own Channel for CCaaS partners can access them. Your Salesforce admin can help with that. |

## Service Errors
| Error Code | Description |
|--|--|
| *20002* | An error occured when retrieve conversation list based on endUserAddress. |
| *20003* | Empty interaction list in request.|

## Other Errors
| Error Code | Description |
|--|--|
| *500* | Special case error for catch-all scenario when we don't want to provide specific error to the end user.|
