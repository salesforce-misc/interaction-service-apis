# Interaction API error codes

## Client Errors
| Error Code | Description |
|--|--|
| *AUTH_CONTEXT_CHANNEL_INVALID* | The Dev name of the Conversation Channel Definition in the request isn't valid for the provided value in the *"to":"\<channelAddressIdentifier\>"* field. |
| *CHANNEL_TYPE_INVALID* | The channel type for the provided *"to":"\<channelAddressIdentifier\>"* field isn't valid. |
| *AUTH_SCOPE_INVALID* | The supplied Authorization access token is missing the required Connected App scope. Required value: interaction_api. |
| *AUTH_TOKEN_INVALID* | The supplied Authorization access token is not valid. |
| *AUTH_CONTEXT_BASED_ON_TOKEN_INVALID* | The AuthorizationContext for the request isn't valid based on the supplied Authorization access token. |
| *CHANNEL_PROVIDER_INVALID* | The channel provider for the provided *"to":"\<channelAddressIdentifier\>"* field isn't valid |
| *FILE_ID_MISSING_ERROR* | The file ID field is empty|
| *FILE_SIZE_EXCEEDS_LIMIT_ERROR* | The file size exceeds the maximum limit of 25 MB|
| *FILE_DATA_EMPTY_ERROR* | The file is empty or doesn’t exist|
| *FILE_CONTENT_LENGTH_MISSING* | The interaction contentLength field is empty|
| *ATTACHMENTS_INPUT_ERROR* | The number of interactions of type AttachmentInteraction does not match the number of attachment files|
| *WORKITEM_ID_INVALID* | Enter a valid workItemId for the MessagingSession ID.|
| *CAPACITY_INVALID* | Enter either a capacityPercentage or a capacityWeight, but not both.|
| *CAPACITY_WEIGHT_OUT_OF_RANGE* | Enter a capacity weight greater than or equal to 0.|
| *CAPACITY_PERCENTAGE_OUT_OF_RANGE* | Enter a capacity percentage between 0 and 100, inclusive.|
| *API_HEADER_INVALID* | Enter a valid header.|
| *CHANNEL_ADDRESS_IDENTIFIER_INVALID* | The channelAddressIdentifier provided does not exist.|
| *CONVERSATION_IDENTIFIER_INVALID* | The conversation identifier is not valid.|
| *SERVICE_IN_READ_ONLY_MODE* | The service is in read-only mode because an org migration is in progress. Try again later.|
| *END_USER_CLIENT_INVALID* | The endUserClientId provided is not valid|
| *CONSENT_STATUS_INVALID* | Enter a valid Consent Status Enum value|
| *CONSENT_OWNER_INVALID* | Cannot update Consent Status where the Consent Owner of the channel is not set to Partner|
| *SUPPORTS_KEYWORDS_INVALID* | Cannot update Consent Status where the SupportsKeywords of the channel is true|
| *INVALID_FLOW_ROUTING_CONFIG_ERROR* | For Flow based Routing, flow ID and Fallback Queue ID are required fields|
| *CONVERSATION_ID_NOT_FOUND_ERROR* | The conversation id isn't found based on channelAddressIdentifier and CUSTOM channel type.|
| *CHANNEL_DEFINITION_MISSING* | The conversaction channel defnition for the provided *"to":"\<channelAddressIdentifier\>"* field is missing.|
| *TO_FIELD_INVALID* | The provided *"to"* field isn't valid.|
| *FROM_FIELD_INVALID* | The provided *"from"* field isn't valid.|
| *INTERACTION_LIST_INVALID* | The provided interaction list isn't valid.|
| *INVALID_ROUTING_CONFIG_ERROR* | Specify either a flow or a queue to Route API|
| *CHANNEL_ADDRESS_IS_INACTIVE* | The channelAddressIdentifier provided is not active.|
| *INVALID_QUEUE_ROUTING_CONFIG_ERROR* | Specified Queue ID is invalid|
| *PASS_ERROR_MESSAGE_WHEN_SUCCESS* | Should not pass in error message when routing result success.|
| *ERROR_MESSAGE_MISSING* | Error message missing when routing result false.|
| *MESSAGING_CHANNEL_TYPE_INVALID* | The messaging channel is not a type of CUSTOM.|
| *INVALID_PARTICIPANT* | The participant does not exist|
| *INVALID_APP_TYPE* | The AppType provided is not supported|
| *INVALID_PARTICIPANT_ROLE* | The Participant Role provided is not supported|
| *EMPTY_PARTICIPANTS_LIST* | Add participants to your conversation.|
| *EMPTY_PARTICIPANT_ROLE_NOT_ALLOWE* | Provide a participant role.|
| *EMPTY_SUBJECT_NOT_ALLOWED* | Provide a participant subject.|
| *NO_END_USER_PARTICIPANT* | Include one end user participant in the conversation.|
| *INVALID_CONVERSATION_HISTORY_PARTICIPANT_ROLE* | Invalid participant role. Specify a Chatbot or EndUser role for all participants.|
| *MULTIPLE_END_USER_PARTICIPANTS* | Include only one end user participant in the conversation.|
| *INVALID_MESSAGE_ENTRY* | Provide a valid message entry format.|
| *INVALID_ENTRY_TIMESTAMP* | Invalid clientTimestamp. Send messages within the allowed response time frame.|
| *INVALID_MESSAGE_TYPE_ERROR* | Specified Message Type is invalid|
| *INVALID_FORMAT_TYPE_ERROR* | Specified Format Type is invalid|
| *INVALID_MESSAGE_TYPE_FORMAT_TYPE_ERROR* | Specified Message Type and Format Type combination is invalid|
| *INVALID_CUSTOM_INTEGRATION_ERROR* | Specified custom integration is invalid|
| *CONTENT_LENGTH_ERROR* | The file size (in bytes) of uploaded attachment is incorrect.|
| *INVALID_CHANNEL_ADDRESS_IDENTIFIER* | The "channelAddressIdentifier" field set in post conversation request isn't valid.|
| *INVALID_OR_EMPTY_PARTICIPANT_LIST* | The "participants" field set in post conversation request is either invalid or empty.|
| *INVALID_PARTICIPANT_SUBJECT* |Invalid subject of participant in conversation request.|
| *CONVERSATION_PRECONDITION_FAILED* | Conversation precondition failed. Please retry. |
| *CONVERSATION_PROCESSING_ABORTED* | Conversation processing aborted. Please retry. |
| *CONVERSATION_HISTORY_INVALID_VENDOR_TYPE* | Looks like you don't have access to the ConversationHistory and AttachmentHistory APIs. Only Bring Your Own Channel for CCaaS partners can access them. Your Salesforce admin can help with that. |

## Service Errors
| Error Code | Description |
|--|--|
| *CONVERSATION_LIST_RETRIEVAL_ERROR* | An error occured when retrieve conversation list based on endUserAddress. |
| *EMPTY_INTERACTION_LIST_ERROR* | Empty interaction list in request.|

## Other Errors
| Error Code | Description |
|--|--|
| *GENERIC_INTERNAL_ERROR* | Special case error for catch-all scenario when we don't want to provide specific error to the end user.|
