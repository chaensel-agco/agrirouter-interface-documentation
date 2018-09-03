# Global Message Codes

The below listing of messages are those which will be used as output within the ACK* messages pushed to connected endpoints of the AgriRouter.

## Message Placeholders

With the messages we will have place holders for values which would be injected into the message text at runtime. Within the output response of the ACK message we would also have an args section. The args section will contain an object map ov key value pairs for the data injected into the message. The following list defines these keys so that it clear what should be used.

Place Holder            | Key              | Value
------------------------|------------------|--------------
%endpointId%            | endpointId       | The ID of the endpoint. Depending on the message this could be a sender or a recipient
%technicalMessageType%  | technicalMsgType | The ID of the technical message type
%machineId%             | machineId        | The clientName provided within the device description (Also would be the ExternalID from the ArgiRouter Endpoint)
%attributeName%         | attributeName    | The name of a field within a message. Typically used in scenarios where field validations are failing
%messageId%             | messageId        | An AgriRouter message ID

## System Messages

Message Code | When/Why can it occur                                  | Service or Plugin where it is used                  | Type    | Text                                                                                | Result/Action
-------------|--------------------------------------------------------|-----------------------------------------------------|---------|-------------------------------------------------------------------------------------|----------------
SYS_000001   | Unable to obtain details from the configuration module | Message Processing Service: SRT Routing Rule Plugin | Error   | Unable to process message due to system error.                                      | ACK_WITH_FAILURE
SYS_000002   | Unable to retrieve requested feed information          | Feed Service                                        | Error   | Unable to retrieve data for the requested feed query.                               | ACK_WITH_FAILURE
SYS_000004   | Message received by agrirouter but there is a delay in processing it           | Ingest Service                                        | Information   | Message accepted and will be processed with a delay.                               | ACK

## Validation Messages

Message Code | When/Why can it occur                                  | Service or Plugin where it is used                  | Type    | Text                                                                                       | Result/Action
-------------|--------------------------------------------------------|-----------------------------------------------------|---------|--------------------------------------------------------------------------------------------|----------------
VAL_000001   | Missing Endpoint Capabilities                          | Endpoint Message Type Verification Plugin           | Error   | Capabilities for Endpoint %endpointId% are not known.                                      | ACK_WITH_FAILURE
VAL_000002   | Endpoint is not able to send technical message type    | Endpoint Message Type Verification Plugin           | Error   | Endpoint cannot sent Technical Message type %technicalMessageType%.                        | ACK_WITH_FAILURE
VAL_000003   | Missing Information Type assignment                    | Information Derivation Plugin                       | Error   | Technical Message type %technicalMessageType% is not assigned to an information type.      | ACK_WITH_FAILURE
VAL_000004   | No recipients for this sender and info type            | SRT Routing Rule Plugin                             | Error   | No recipients have been identified for %technicalMessageType%.                             | ACK_WITH_FAILURE
VAL_000005   | Recipient is not allowed from this sender              | SRT Routing Rule Plugin                             | Warning | Recipient %endpointId% is not able to receive message type %technicalMessageType%.         | ACK_WITH_MESSAGES
VAL_000006   | Subscription contains invalid message type(s)          | Subscription Plugin                                 | Error   | Subscription to %technicalMessageType% is not valid reported capabilities.                 | ACK_WITH_FAILURE
VAL_000007   | Capabilities contains invalid message type(s)          | Endpoint Capability Plugin                          | Warning | Capability for %technicalMessageType% was ignored as it is not known to the certification. | ACK_WITH_MESSAGES
VAL_000008   | Certification validations do not pass                  | Certification Check Plugin                          | Error   | Certification is not valid or the endpoint is blocked.                                     | ACK_WITH_FAILURE
VAL_000009   | Account does not exist                                 | Account Verification Checks Plugin                  | Error   | Unable to determine account!!                                                              | ACK_WITH_FAILURE
VAL_000010   | Account is not active                                  | Account Verification Checks Plugin                  | Error   | Account is not active.                                                                     | ACK_WITH_FAILURE
VAL_000011   | Endpoint does not exist                                | Inbound Transport                                   | Error   | Endpoint is unknown.                                                                       | ACK_WITH_FAILURE
VAL_000012   | Endpoint is not active                                 | Inbound Transport                                   | Error   | Endpoint is not active within the account.                                                 | ACK_WITH_FAILURE
VAL_000013   | Account is not a Test Account                          | Certification Check Plugin                          | Error   | Account is not a test account and cannot use the certified application.                    | ACK_WITH_FAILURE
VAL_000014   | Device Description Missing Information                 | Device Description Validation Plugin                | Error   | Device %machineId& is missing mandatory field %attributeName%.                             | ACK_WITH_FAILURE
VAL_000015   | Device Descriptions Missing                            | Device Description Validation Plugin                | Error   | No devices provided within the device description.                                         | ACK_WITH_FAILURE
VAL_000016   | Team Set Context ID Missing                            | Device Description Validation Plugin                | Error   | No Team Set Context ID Provided.                                                           | ACK_WITH_FAILURE
VAL_000017   | Message missing required information                   | Multiple                                            | Error   | %attributeName% information required to process message is missing or malformed.           | ACK_WITH_FAILURE
VAL_000018   | Message missing required information                   | Multiple                                            | Error   | Information required to process message is missing or malformed.                           | ACK_WITH_FAILURE
VAL_000200   | Feed contains messages pending confirmation            | Feed Service: Query Payload                         | Error   | Cannot retrieve new messages until pending confirmations are provided.                     | ACK_WITH_FAILURE
VAL_000201   | Feed does not contain message requested                | Feed Service: Query/Confirm Payload                 | Err/Warn| Message with ID: %messageId% is not known or delivery has been cancelled.                  | ACK_WITH_FAILURE or ACK_WITH_MESSAGES
VAL_000202   | Feed does not contain any data for query               | Feed Service: Query Headers and Payload             | Info    | No data is currently available for requested query                                         | ACK_WITH_MESSAGES
VAL_000205   | Feed message is not pending confirmation               | Feed Service: Confirm by ID Handler                 | Warn    | Message %messageId% is not pending confirmation. This ID will be ignored.                  | ACK_WITH_MESSAGES
VAL_000206   | Feed message confirmation confirmed                    | Feed Service: Confirm by ID Handler                 | Info    | Message %messageId% delivery had been confirmed.                                           | ACK_WITH_MESSAGES
VAL_000207   | Feed message cannot be deleted                         | Feed Service: Delete                                | Info    | Message %messageId% cannot be deleted                                                      | ACK_WITH_MESSAGES
VAL_000208   | Feed does not contain any data to be deleted           | Feed Service: Delete                                | Info    | No data is currently available for requested query                                         | ACK_WITH_MESSAGES
VAL_000209   | Feed message deleted                                   | Feed Service: Delete                                | Info    | Message %messageId% deleted                                                                | ACK_WITH_MESSAGES
VAL_000210   | Feed message no pending for confirmation               | Feed Service: Confirm by ID Handler                 | Error   | No messages are pending for confirmation                                 | ACK_WITH_FAILURE
VAL_000211   | Inbound payload size excedeed                          | Inbound Transport: Payload Size Checker             | Error   | : Message with ID %messageId% contains a payload of size %payloadSize%. Max allowed size is %maxPayloadSizeConfigValue%                                 | ACK_WITH_FAILURE
INVALID_APPLICATION | Endpoint Application Specification cannot change        | Ingest Application: Endpoint Cannot Change Application Specification             | Error   | Endpoint cannot change application specification. Only version changes are allowed.     | ACK_WITH_FAILURE
NO_CAPABILITIES_CHANGE        | There are no capability changes                                                           | Ingest Application: No Update of Capabilities present                  | Warning   | Skipping capabilities update because there are no differences                                                                                       | ACK_WITH_MESSAGES
VAL_000300   | Decoding of inbound message failed               | Ingest: Decoding failure                 | Error   | Error occured while decoding the message                                 | ACK_WITH_FAILURE
VAL_000000   | Application Processing Error               | Ingest: Application Processing Error                 | Error   | Application Processing Error                                 | ACK_WITH_FAILURE