# Configuration environment variables for the voice gateway

All configuration of Voice Gateway for Watson is currently handled through Docker environment variables. Where you specify the environment variables depends on where you deploy the voice gateway:
* **IBM Containers for Bluemix:** Set the variables in the **quickstart/bluemix/docker.env** file.
* **Docker Engine:** Set the variables in a YAML file, such as the default **docker-compose.yml** file in the root of the [sample.voice.gateway.for.watson Github repository](https://github.com/WASdev/sample.voice.gateway.for.watson).

The **docker.env** and **docker-compose.yml** files each contain every item that can be configured for both the SIP Orchestrator and the Media Relay.

Although you can define both the SIP Orchestrator and the Media Relay in a single file, each component uses distinct environment variables, which are outlined in the following sections.

* [SIP Orchestrator environment variables](#sip-orchestrator-environment-variables)
* [Media Relay environment variables](#media-relay-environment-variables)


### SIP Orchestrator environment variables
The following table lists all Docker environment variables that can be used to configure the SIP Orchestrator:

| Environment variable | Default              | Details |
| -------------- | -------------------- | --------------------------------------------------------------- |
| RTP_RELAY_HOST | localhost:8080 | Host name of the Media Relay. Typically set to the Media Relay service name (e.g. cgw.media.relay:8080) |
| SIP_HOST | n/a | External IP of the SIP Orchestrator Docker container where the SIP server is listening. Typically set to ${EXTERNAL_IP}. If fronted by a load balancer, set this variable to the load balancer's host address. |
| SIP_PORT | 5060 | External SIP port |
| CONV_WORKSPACE_ID | n/a | Workspace ID for the Watson Conversation API. |
| CONV_USERNAME | n/a | User name for the Watson Conversation service. |
| CONV_PASSWORD | n/a | Password for the Watson Conversation service. |
| CONV_ENDPOINT | n/a | Endpoint URL for the Conversation service. |
| DIALOG_NAME | n/a | Name of the Watson dialog. Required when using the Watson Dialog service and no DIALOG_ID is specified.|
| DIALOG_ID | n/a | ID of the Watson dialog. Required when using the Watson Dialog service and no DIALOG_NAME is specified. |
| DIALOG_USERNAME | n/a | User name for the Watson Dialog service. |
| DIALOG_PASSWORD | n/a | Password for the Watson Dialog service. |
| DIALOG_ENDPOINT | n/a | Endpoint URL for Watson Dialog service. |
| NUMCONV_PLUGIN_ENABLE | false | Set to true to enable the number (date) conversion plug-in. |
| MQTT_PLUGIN_ENABLE | false | Set to true to enable the MQTT publisher of transcription events. |
| ENABLE_AUDIT_MESSAGES | true | Set to false to disable audit messages. |
| ENABLE_TRANSCRIPTION_AUDIT_MESSAGES | false | Set to true to enable audit transcription messages. |
| LATENCY_REPORTING_THRESHOLD | 20 | Threshold in milliseconds for reporting round trip Conversation latency. |
| RELAY_LATENCY_REPORTING_THRESHOLD | 1000 | Threshold in milliseconds for reporting media relay related latencies. |
| CUSTOM_SIP_SESSION_HEADER | Call-ID | Specifies the SIP header to use as the global session ID in all audit messages and all in the cgwSessionID state variable passed to the conversation. |
| MULTI_TENANT_CONFIG_FILE | n/a | File path to the multi-tenant configuration file. This configuration file is also used to define the whitelist of called numbers that can connect to the gateway. Example: ${server.config.dir}/cgwConfig.json |
| POST_RESPONSE_TIMEOUT | 7000 | Time in milliseconds to wait before sending a "blank" utterance to the Conversation API to indicate no response from the caller. |
| SEND_SIP_CALL_ID_TO_CONVERSATION | false | When true, the SIP call ID to be passed to the Conversation in this state variable: cgwSIPCallID |
| SEND_SIP_REQUEST_URI_TO_CONVERSATION | false | When true, the SIP request URI to be passed to the Conversation in this state variable: cgwSIPRequestURI |
| SEND_SIP_TO_URI_TO_CONVERSATION | false | When true, the SIP To URI to be passed to the Conversation in this state variable: cgwSIPToURI |
| SEND_SIP_FROM_URI_TO_CONVERSATION | false | When true, the SIP From URI to be passed to the Conversation in this state variable: cgwSIPFromURI |
| CUSTOM_SIP_INVITE_HEADER | n/a | When set, the specified SIP header will be passed to the Conversation in this state variable: cgwSIPCustomInviteHeader |
| LOG_LEVEL | audit | This is the log level for the SIP Orchestrator. Valid values include off, fatal, severe, warning, fine, finest, and all. |
| CONVERSATION_FAILED_REPLY_MESSAGE | Call being transferred to an agent due to a technical problem. Good bye. | Message streamed to the caller if the Conversation service fails. |
| TRANSFER_DEFAULT_TARGET | none | Identifies the target transfer to endpoint. Must be valid SIP or tel URI (e.g. sip:10.10.10.10). This is a default transfer target that is used only when a failure occurs and the call transfer target can't be obtained from the Conversation API. |
| TRANSFER_FAILED_REPLY_MESSAGE | Call transfer to an agent failed. Please try again later. Good bye. | Message streamed to the caller if the call transfer fails. |
| WHITELIST_FROM_URI | none | Gateway will accept only calls that contain the specified string within the SIP from URI. |
| WHITELIST_TO_URI | none | Gateway will accept only calls that contain the specified string within the SIP to URI. |

### Media Relay environment variables
The following table lists all Docker environment variables that can be used to configure the Media Relay:

| Environment variable | Default |  Details |
| --- |--- | ---|
| RTP_PORT| 8080 |   the port to listen on|
| WATSON_RELAY_SDP_ADDRESS | localhost | Address to use in the Answer SDP for SIP. |
| RTP_UDP_PORT_RANGE | '16384-16394'|Port range for UDP, set as a String. |
| CLUSTER_WORKERS | 1 | Number of Cluster Workers to spawn. Set to 0 for NumCPUS-1. |
| WATSON_STT_USER | n/a | User name for the Watson Speech to Text service. |
| WATSON_STT_PASSWORD | n/a | Password for the Watson Speech to Text service.|
| WATSON_STT_ENDPOINT | wss://stream.watsonplatform.net/speech-to-text/api/v1/recognize| Watson Speech to Text service endpoint.|
| WATSON_STT_EXTRAS | n/a | Extra values for the Watson Speech to Text service. Do not set. |
| WATSON_STT_MODEL | en_US_NarrowbandModel | [Watson Speech to Text model](https://www.ibm.com/watson/developercloud/doc/speech-to-text/input.shtml#models). The default narrowband model is best for offline decoding of telephone speech. |
| WATSON_STT_OPTOUT | true | Opt out of saving data that passes through the Speech to Text service on Watson servers.|
| WATSON_STT_SILENCE | 500 | Speech to Text silence duration. Used to determine when to start the STT latency timer, which is stopped when a final utterance is received.  |
| WATSON_STT_MAXALTERNATIVES | 1 | Speech to Text Number of speech recognition alternatives to return |
| WATSON_STT_CONFIDENCE_SCORE_THRESHOLD | 0 | Confidence threshold of messages from the Speech to Text service. Messages with a confidence score that is under the threshold will not be used as a response. The default value of 0 means that all responses will be used. The recommended values are between 0 and 1.|
| WATSON_STT_MODEL_CUSTOMIZATION_ID | n/a | Used to set a custom language model for recognition. |
| WATSON_STT_PROFANITY_FILTER | true | Indicates whether profanity is filtered on the transcripts that come from the Watson Speech to Text service.|
| WATSON_STT_SMART_FORMATTING | false | Indicates whether dates, times, series of digits and numbers, phone numbers, currency values, and Internet addresses are to be converted into more readable, conventional representations in the final transcript of a recognition request. |
| WATSON_TTS_USER | n/a | User name for the Watson Text to Speech service.|
| WATSON_TTS_PASSWORD | n/a | Password for the Watson Text to Speech service.|
| WATSON_TTS_URL | sdk default| Watson Text to Speech service endpoint URL.|
| WATSON_TTS_VOICE | en_US_AllisonVoice | Voice used by the Watson Text to Speech service.|
| WATSON_TTS_MODEL_CUSTOMIZATION_ID | n/a | Used to set a custom voice model for text to speech |
| WATSON_ENERGY_BARGEIN_DURATION | 1500  | Speech utterance duration before a barge-in occurs. Used to prevent noise from causing a barge-in |
| WATSON_ENERGY_BARGEIN_ENABLED | false | Enable or disable the media relay using energy detection for barge-in |
|RTP_RELAY_RECORD | false | Set to true to enable recording on the relay. |
|RTP_RELAY_LOGLEVEL| INFO | Set the bunyan log to INFO, DEBUG, or TRACE. |