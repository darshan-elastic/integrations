# System Integration

The System integration allows you to monitor servers, personal computers, and more.

Use the System integration to collect metrics and logs from your machines.
Then visualize that data in Kibana, create alerts to notify you if something goes wrong,
and reference data when troubleshooting an issue.

For example, if you wanted to be notified when less than 10% of the disk space is still available, you
could install the System integration to send file system metrics to Elastic.
Then, you could view real-time updates to disk space used on your system in Kibana's _[Metrics System] Overview_ dashboard.
You could also set up a new rule in the Elastic Observability Metrics app to alert you when the percent free is
less than 10% of the total disk space.

## Data streams

The System integration collects two types of data: logs and metrics.

**Logs** help you keep a record of events that happen on your machine.
Log data streams collected by the System integration include application, system, and security events on
machines running Windows and auth and syslog events on machines running macOS or Linux.
See more details in the [Logs reference](#logs-reference).

**Metrics** give you insight into the state of the machine.
Metric data streams collected by the System integration include CPU usage, load statistics, memory usage,
information on network behavior, and more.
See more details in the [Metrics reference](#metrics-reference).

You can enable and disable individual data streams. If _all_ data streams are disabled and the System integration
is still enabled, Fleet uses the default data streams.

## Requirements

You need Elasticsearch for storing and searching your data and Kibana for visualizing and managing it.
You can use our hosted Elasticsearch Service on Elastic Cloud, which is recommended, or self-manage the Elastic Stack on your own hardware.

Each data stream collects different kinds of metric data, which may require dedicated permissions
to be fetched and which may vary across operating systems.
Details on the permissions needed for each data stream are available in the [Metrics reference](#metrics-reference).

## Setup

For step-by-step instructions on how to set up an integration, see the
[Getting started](https://www.elastic.co/guide/en/welcome-to-elastic/current/getting-started-observability.html) guide.

## Troubleshooting

Note that certain data streams may access `/proc` to gather process information,
and the resulting `ptrace_may_access()` call by the kernel to check for
permissions can be blocked by
[AppArmor and other LSM software](https://gitlab.com/apparmor/apparmor/wikis/TechnicalDoc_Proc_and_ptrace), even though the System module doesn't use `ptrace` directly.

In addition, when running inside a container the proc filesystem directory of the host
should be set using `system.hostfs` setting to `/hostfs`.

## Logs reference

### Application

The Windows `application` data stream provides events from the Windows
`Application` event log.

#### Supported operating systems

- Windows

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| error.message | Error message. | match_only_text |
| event.code | Identification code for this event, if one exists. Some event sources use event codes to identify messages unambiguously, regardless of message language or wording adjustments over time. An example of this is the Windows Event ID. | keyword |
| event.created | event.created contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from @timestamp in that @timestamp typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, @timestamp should be used. | date |
| event.dataset | Event dataset. | constant_keyword |
| event.ingested | Timestamp when an event arrived in the central data store. This is different from `@timestamp`, which is when the event originally occurred.  It's also different from `event.created`, which is meant to capture the first time an agent saw the event. In normal conditions, assuming no tampering, the timestamps should chronologically look like this: `@timestamp` \< `event.created` \< `event.ingested`. | date |
| event.module | Event module | constant_keyword |
| event.original | Raw text message of entire event. Used to demonstrate log integrity or where the full log message (before splitting it up in multiple parts) may be required, e.g. for reindex. This field is not indexed and doc_values are disabled. It cannot be searched, but it can be retrieved from `_source`. If users wish to override this and index this field, please see `Field data types` in the `Elasticsearch Reference`. | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host mac addresses. | keyword |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| winlog.activity_id | A globally unique identifier that identifies the current activity. The events that are published with this identifier are part of the same activity. | keyword |
| winlog.api | The event log API type used to read the record. The possible values are "wineventlog" for the Windows Event Log API or "eventlogging" for the Event Logging API. The Event Logging API was designed for Windows Server 2003 or Windows 2000 operating systems. In Windows Vista, the event logging infrastructure was redesigned. On Windows Vista or later operating systems, the Windows Event Log API is used. Winlogbeat automatically detects which API to use for reading event logs. | keyword |
| winlog.channel | The name of the channel from which this record was read. This value is one of the names from the `event_logs` collection in the configuration. | keyword |
| winlog.computer_name | The name of the computer that generated the record. When using Windows event forwarding, this name can differ from `agent.hostname`. | keyword |
| winlog.event_data | The event-specific data. This field is mutually exclusive with `user_data`. If you are capturing event data on versions prior to Windows Vista, the parameters in `event_data` are named `param1`, `param2`, and so on, because event log parameters are unnamed in earlier versions of Windows. | object |
| winlog.event_data.AuthenticationPackageName |  | keyword |
| winlog.event_data.Binary |  | keyword |
| winlog.event_data.BitlockerUserInputTime |  | keyword |
| winlog.event_data.BootMode |  | keyword |
| winlog.event_data.BootType |  | keyword |
| winlog.event_data.BuildVersion |  | keyword |
| winlog.event_data.Company |  | keyword |
| winlog.event_data.CorruptionActionState |  | keyword |
| winlog.event_data.CreationUtcTime |  | keyword |
| winlog.event_data.Description |  | keyword |
| winlog.event_data.Detail |  | keyword |
| winlog.event_data.DeviceName |  | keyword |
| winlog.event_data.DeviceNameLength |  | keyword |
| winlog.event_data.DeviceTime |  | keyword |
| winlog.event_data.DeviceVersionMajor |  | keyword |
| winlog.event_data.DeviceVersionMinor |  | keyword |
| winlog.event_data.DriveName |  | keyword |
| winlog.event_data.DriverName |  | keyword |
| winlog.event_data.DriverNameLength |  | keyword |
| winlog.event_data.DwordVal |  | keyword |
| winlog.event_data.EntryCount |  | keyword |
| winlog.event_data.ExtraInfo |  | keyword |
| winlog.event_data.FailureName |  | keyword |
| winlog.event_data.FailureNameLength |  | keyword |
| winlog.event_data.FileVersion |  | keyword |
| winlog.event_data.FinalStatus |  | keyword |
| winlog.event_data.Group |  | keyword |
| winlog.event_data.IdleImplementation |  | keyword |
| winlog.event_data.IdleStateCount |  | keyword |
| winlog.event_data.ImpersonationLevel |  | keyword |
| winlog.event_data.IntegrityLevel |  | keyword |
| winlog.event_data.IpAddress |  | keyword |
| winlog.event_data.IpPort |  | keyword |
| winlog.event_data.KeyLength |  | keyword |
| winlog.event_data.LastBootGood |  | keyword |
| winlog.event_data.LastShutdownGood |  | keyword |
| winlog.event_data.LmPackageName |  | keyword |
| winlog.event_data.LogonGuid |  | keyword |
| winlog.event_data.LogonId |  | keyword |
| winlog.event_data.LogonProcessName |  | keyword |
| winlog.event_data.LogonType |  | keyword |
| winlog.event_data.MajorVersion |  | keyword |
| winlog.event_data.MaximumPerformancePercent |  | keyword |
| winlog.event_data.MemberName |  | keyword |
| winlog.event_data.MemberSid |  | keyword |
| winlog.event_data.MinimumPerformancePercent |  | keyword |
| winlog.event_data.MinimumThrottlePercent |  | keyword |
| winlog.event_data.MinorVersion |  | keyword |
| winlog.event_data.NewProcessId |  | keyword |
| winlog.event_data.NewProcessName |  | keyword |
| winlog.event_data.NewSchemeGuid |  | keyword |
| winlog.event_data.NewTime |  | keyword |
| winlog.event_data.NominalFrequency |  | keyword |
| winlog.event_data.Number |  | keyword |
| winlog.event_data.OldSchemeGuid |  | keyword |
| winlog.event_data.OldTime |  | keyword |
| winlog.event_data.OriginalFileName |  | keyword |
| winlog.event_data.Path |  | keyword |
| winlog.event_data.PerformanceImplementation |  | keyword |
| winlog.event_data.PreviousCreationUtcTime |  | keyword |
| winlog.event_data.PreviousTime |  | keyword |
| winlog.event_data.PrivilegeList |  | keyword |
| winlog.event_data.ProcessId |  | keyword |
| winlog.event_data.ProcessName |  | keyword |
| winlog.event_data.ProcessPath |  | keyword |
| winlog.event_data.ProcessPid |  | keyword |
| winlog.event_data.Product |  | keyword |
| winlog.event_data.PuaCount |  | keyword |
| winlog.event_data.PuaPolicyId |  | keyword |
| winlog.event_data.QfeVersion |  | keyword |
| winlog.event_data.Reason |  | keyword |
| winlog.event_data.SchemaVersion |  | keyword |
| winlog.event_data.ScriptBlockText |  | keyword |
| winlog.event_data.ServiceName |  | keyword |
| winlog.event_data.ServiceVersion |  | keyword |
| winlog.event_data.ShutdownActionType |  | keyword |
| winlog.event_data.ShutdownEventCode |  | keyword |
| winlog.event_data.ShutdownReason |  | keyword |
| winlog.event_data.Signature |  | keyword |
| winlog.event_data.SignatureStatus |  | keyword |
| winlog.event_data.Signed |  | keyword |
| winlog.event_data.StartTime |  | keyword |
| winlog.event_data.State |  | keyword |
| winlog.event_data.Status |  | keyword |
| winlog.event_data.StopTime |  | keyword |
| winlog.event_data.SubjectDomainName |  | keyword |
| winlog.event_data.SubjectLogonId |  | keyword |
| winlog.event_data.SubjectUserName |  | keyword |
| winlog.event_data.SubjectUserSid |  | keyword |
| winlog.event_data.TSId |  | keyword |
| winlog.event_data.TargetDomainName |  | keyword |
| winlog.event_data.TargetInfo |  | keyword |
| winlog.event_data.TargetLogonGuid |  | keyword |
| winlog.event_data.TargetLogonId |  | keyword |
| winlog.event_data.TargetServerName |  | keyword |
| winlog.event_data.TargetUserName |  | keyword |
| winlog.event_data.TargetUserSid |  | keyword |
| winlog.event_data.TerminalSessionId |  | keyword |
| winlog.event_data.TokenElevationType |  | keyword |
| winlog.event_data.TransmittedServices |  | keyword |
| winlog.event_data.UserSid |  | keyword |
| winlog.event_data.Version |  | keyword |
| winlog.event_data.Workstation |  | keyword |
| winlog.event_data.param1 |  | keyword |
| winlog.event_data.param2 |  | keyword |
| winlog.event_data.param3 |  | keyword |
| winlog.event_data.param4 |  | keyword |
| winlog.event_data.param5 |  | keyword |
| winlog.event_data.param6 |  | keyword |
| winlog.event_data.param7 |  | keyword |
| winlog.event_data.param8 |  | keyword |
| winlog.event_id | The event identifier. The value is specific to the source of the event. | keyword |
| winlog.keywords | The keywords are used to classify an event. | keyword |
| winlog.opcode | The opcode defined in the event. Task and opcode are typically used to identify the location in the application from where the event was logged. | keyword |
| winlog.process.pid | The process_id of the Client Server Runtime Process. | long |
| winlog.process.thread.id |  | long |
| winlog.provider_guid | A globally unique identifier that identifies the provider that logged the event. | keyword |
| winlog.provider_name | The source of the event log record (the application or service that logged the record). | keyword |
| winlog.record_id | The record ID of the event log record. The first record written to an event log is record number 1, and other records are numbered sequentially. If the record number reaches the maximum value (2^32^ for the Event Logging API and 2^64^ for the Windows Event Log API), the next record number will be 0. | keyword |
| winlog.related_activity_id | A globally unique identifier that identifies the activity to which control was transferred to. The related events would then have this identifier as their `activity_id` identifier. | keyword |
| winlog.task | The task defined in the event. Task and opcode are typically used to identify the location in the application from where the event was logged. The category used by the Event Logging API (on pre Windows Vista operating systems) is written to this field. | keyword |
| winlog.user.domain | The domain that the account associated with this event is a member of. | keyword |
| winlog.user.identifier | The Windows security identifier (SID) of the account associated with this event. If Winlogbeat cannot resolve the SID to a name, then the `user.name`, `user.domain`, and `user.type` fields will be omitted from the event. If you discover Winlogbeat not resolving SIDs, review the log for clues as to what the problem may be. | keyword |
| winlog.user.name | Name of the user associated with this event. | keyword |
| winlog.user.type | The type of account associated with this event. | keyword |
| winlog.user_data | The event specific data. This field is mutually exclusive with `event_data`. | object |
| winlog.version | The version number of the event's definition. | long |


### System

The Windows `system` data stream provides events from the Windows `System`
event log.

#### Supported operating systems

- Windows

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| error.message | Error message. | match_only_text |
| event.action | The action captured by the event. This describes the information in the event. It is more specific than `event.category`. Examples are `group-add`, `process-started`, `file-created`. The value is normally defined by the implementer. | keyword |
| event.category | This is one of four ECS Categorization Fields, and indicates the second level in the ECS category hierarchy. `event.category` represents the "big buckets" of ECS categories. For example, filtering on `event.category:process` yields all events relating to process activity. This field is closely related to `event.type`, which is used as a subcategory. This field is an array. This will allow proper categorization of some events that fall in multiple categories. | keyword |
| event.code | Identification code for this event, if one exists. Some event sources use event codes to identify messages unambiguously, regardless of message language or wording adjustments over time. An example of this is the Windows Event ID. | keyword |
| event.created | event.created contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from @timestamp in that @timestamp typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, @timestamp should be used. | date |
| event.dataset | Event dataset. | constant_keyword |
| event.ingested | Timestamp when an event arrived in the central data store. This is different from `@timestamp`, which is when the event originally occurred.  It's also different from `event.created`, which is meant to capture the first time an agent saw the event. In normal conditions, assuming no tampering, the timestamps should chronologically look like this: `@timestamp` \< `event.created` \< `event.ingested`. | date |
| event.kind | This is one of four ECS Categorization Fields, and indicates the highest level in the ECS category hierarchy. `event.kind` gives high-level information about what type of information the event contains, without being specific to the contents of the event. For example, values of this field distinguish alert events from metric events. The value of this field can be used to inform how these kinds of events should be handled. They may warrant different retention, different access control, it may also help understand whether the data coming in at a regular interval or not. | keyword |
| event.module | Name of the module this data is coming from. If your monitoring agent supports the concept of modules or plugins to process events of a given source (e.g. Apache logs), `event.module` should contain the name of this module. | keyword |
| event.original | Raw text message of entire event. Used to demonstrate log integrity or where the full log message (before splitting it up in multiple parts) may be required, e.g. for reindex. This field is not indexed and doc_values are disabled. It cannot be searched, but it can be retrieved from `_source`. If users wish to override this and index this field, please see `Field data types` in the `Elasticsearch Reference`. | keyword |
| event.outcome | This is one of four ECS Categorization Fields, and indicates the lowest level in the ECS category hierarchy. `event.outcome` simply denotes whether the event represents a success or a failure from the perspective of the entity that produced the event. Note that when a single transaction is described in multiple events, each event may populate different values of `event.outcome`, according to their perspective. Also note that in the case of a compound event (a single event that contains multiple logical events), this field should be populated with the value that best captures the overall success or failure from the perspective of the event producer. Further note that not all events will have an associated outcome. For example, this field is generally not populated for metric events, events with `event.type:info`, or any events for which an outcome does not make logical sense. | keyword |
| event.provider | Source of the event. Event transports such as Syslog or the Windows Event Log typically mention the source of an event. It can be the name of the software that generated the event (e.g. Sysmon, httpd), or of a subsystem of the operating system (kernel, Microsoft-Windows-Security-Auditing). | keyword |
| event.sequence | Sequence number of the event. The sequence number is a value published by some event sources, to make the exact ordering of events unambiguous, regardless of the timestamp precision. | long |
| event.type | This is one of four ECS Categorization Fields, and indicates the third level in the ECS category hierarchy. `event.type` represents a categorization "sub-bucket" that, when used along with the `event.category` field values, enables filtering events down to a level appropriate for single visualization. This field is an array. This will allow proper categorization of some events that fall in multiple event types. | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host mac addresses. | keyword |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| winlog.activity_id | A globally unique identifier that identifies the current activity. The events that are published with this identifier are part of the same activity. | keyword |
| winlog.api | The event log API type used to read the record. The possible values are "wineventlog" for the Windows Event Log API or "eventlogging" for the Event Logging API. The Event Logging API was designed for Windows Server 2003 or Windows 2000 operating systems. In Windows Vista, the event logging infrastructure was redesigned. On Windows Vista or later operating systems, the Windows Event Log API is used. Winlogbeat automatically detects which API to use for reading event logs. | keyword |
| winlog.channel | The name of the channel from which this record was read. This value is one of the names from the `event_logs` collection in the configuration. | keyword |
| winlog.computer_name | The name of the computer that generated the record. When using Windows event forwarding, this name can differ from `agent.hostname`. | keyword |
| winlog.event_data | The event-specific data. This field is mutually exclusive with `user_data`. If you are capturing event data on versions prior to Windows Vista, the parameters in `event_data` are named `param1`, `param2`, and so on, because event log parameters are unnamed in earlier versions of Windows. | object |
| winlog.event_data.AuthenticationPackageName |  | keyword |
| winlog.event_data.Binary |  | keyword |
| winlog.event_data.BitlockerUserInputTime |  | keyword |
| winlog.event_data.BootMode |  | keyword |
| winlog.event_data.BootType |  | keyword |
| winlog.event_data.BuildVersion |  | keyword |
| winlog.event_data.Company |  | keyword |
| winlog.event_data.CorruptionActionState |  | keyword |
| winlog.event_data.CreationUtcTime |  | keyword |
| winlog.event_data.Description |  | keyword |
| winlog.event_data.Detail |  | keyword |
| winlog.event_data.DeviceName |  | keyword |
| winlog.event_data.DeviceNameLength |  | keyword |
| winlog.event_data.DeviceTime |  | keyword |
| winlog.event_data.DeviceVersionMajor |  | keyword |
| winlog.event_data.DeviceVersionMinor |  | keyword |
| winlog.event_data.DriveName |  | keyword |
| winlog.event_data.DriverName |  | keyword |
| winlog.event_data.DriverNameLength |  | keyword |
| winlog.event_data.DwordVal |  | keyword |
| winlog.event_data.EntryCount |  | keyword |
| winlog.event_data.ExtraInfo |  | keyword |
| winlog.event_data.FailureName |  | keyword |
| winlog.event_data.FailureNameLength |  | keyword |
| winlog.event_data.FileVersion |  | keyword |
| winlog.event_data.FinalStatus |  | keyword |
| winlog.event_data.Group |  | keyword |
| winlog.event_data.IdleImplementation |  | keyword |
| winlog.event_data.IdleStateCount |  | keyword |
| winlog.event_data.ImpersonationLevel |  | keyword |
| winlog.event_data.IntegrityLevel |  | keyword |
| winlog.event_data.IpAddress |  | keyword |
| winlog.event_data.IpPort |  | keyword |
| winlog.event_data.KeyLength |  | keyword |
| winlog.event_data.LastBootGood |  | keyword |
| winlog.event_data.LastShutdownGood |  | keyword |
| winlog.event_data.LmPackageName |  | keyword |
| winlog.event_data.LogonGuid |  | keyword |
| winlog.event_data.LogonId |  | keyword |
| winlog.event_data.LogonProcessName |  | keyword |
| winlog.event_data.LogonType |  | keyword |
| winlog.event_data.MajorVersion |  | keyword |
| winlog.event_data.MaximumPerformancePercent |  | keyword |
| winlog.event_data.MemberName |  | keyword |
| winlog.event_data.MemberSid |  | keyword |
| winlog.event_data.MinimumPerformancePercent |  | keyword |
| winlog.event_data.MinimumThrottlePercent |  | keyword |
| winlog.event_data.MinorVersion |  | keyword |
| winlog.event_data.NewProcessId |  | keyword |
| winlog.event_data.NewProcessName |  | keyword |
| winlog.event_data.NewSchemeGuid |  | keyword |
| winlog.event_data.NewTime |  | keyword |
| winlog.event_data.NominalFrequency |  | keyword |
| winlog.event_data.Number |  | keyword |
| winlog.event_data.OldSchemeGuid |  | keyword |
| winlog.event_data.OldTime |  | keyword |
| winlog.event_data.OriginalFileName |  | keyword |
| winlog.event_data.Path |  | keyword |
| winlog.event_data.PerformanceImplementation |  | keyword |
| winlog.event_data.PreviousCreationUtcTime |  | keyword |
| winlog.event_data.PreviousTime |  | keyword |
| winlog.event_data.PrivilegeList |  | keyword |
| winlog.event_data.ProcessId |  | keyword |
| winlog.event_data.ProcessName |  | keyword |
| winlog.event_data.ProcessPath |  | keyword |
| winlog.event_data.ProcessPid |  | keyword |
| winlog.event_data.Product |  | keyword |
| winlog.event_data.PuaCount |  | keyword |
| winlog.event_data.PuaPolicyId |  | keyword |
| winlog.event_data.QfeVersion |  | keyword |
| winlog.event_data.Reason |  | keyword |
| winlog.event_data.SchemaVersion |  | keyword |
| winlog.event_data.ScriptBlockText |  | keyword |
| winlog.event_data.ServiceName |  | keyword |
| winlog.event_data.ServiceVersion |  | keyword |
| winlog.event_data.ShutdownActionType |  | keyword |
| winlog.event_data.ShutdownEventCode |  | keyword |
| winlog.event_data.ShutdownReason |  | keyword |
| winlog.event_data.Signature |  | keyword |
| winlog.event_data.SignatureStatus |  | keyword |
| winlog.event_data.Signed |  | keyword |
| winlog.event_data.StartTime |  | keyword |
| winlog.event_data.State |  | keyword |
| winlog.event_data.Status |  | keyword |
| winlog.event_data.StopTime |  | keyword |
| winlog.event_data.SubjectDomainName |  | keyword |
| winlog.event_data.SubjectLogonId |  | keyword |
| winlog.event_data.SubjectUserName |  | keyword |
| winlog.event_data.SubjectUserSid |  | keyword |
| winlog.event_data.TSId |  | keyword |
| winlog.event_data.TargetDomainName |  | keyword |
| winlog.event_data.TargetInfo |  | keyword |
| winlog.event_data.TargetLogonGuid |  | keyword |
| winlog.event_data.TargetLogonId |  | keyword |
| winlog.event_data.TargetServerName |  | keyword |
| winlog.event_data.TargetUserName |  | keyword |
| winlog.event_data.TargetUserSid |  | keyword |
| winlog.event_data.TerminalSessionId |  | keyword |
| winlog.event_data.TokenElevationType |  | keyword |
| winlog.event_data.TransmittedServices |  | keyword |
| winlog.event_data.UserSid |  | keyword |
| winlog.event_data.Version |  | keyword |
| winlog.event_data.Workstation |  | keyword |
| winlog.event_data.param1 |  | keyword |
| winlog.event_data.param2 |  | keyword |
| winlog.event_data.param3 |  | keyword |
| winlog.event_data.param4 |  | keyword |
| winlog.event_data.param5 |  | keyword |
| winlog.event_data.param6 |  | keyword |
| winlog.event_data.param7 |  | keyword |
| winlog.event_data.param8 |  | keyword |
| winlog.event_id | The event identifier. The value is specific to the source of the event. | keyword |
| winlog.keywords | The keywords are used to classify an event. | keyword |
| winlog.opcode | The opcode defined in the event. Task and opcode are typically used to identify the location in the application from where the event was logged. | keyword |
| winlog.process.pid | The process_id of the Client Server Runtime Process. | long |
| winlog.process.thread.id |  | long |
| winlog.provider_guid | A globally unique identifier that identifies the provider that logged the event. | keyword |
| winlog.provider_name | The source of the event log record (the application or service that logged the record). | keyword |
| winlog.record_id | The record ID of the event log record. The first record written to an event log is record number 1, and other records are numbered sequentially. If the record number reaches the maximum value (2^32^ for the Event Logging API and 2^64^ for the Windows Event Log API), the next record number will be 0. | keyword |
| winlog.related_activity_id | A globally unique identifier that identifies the activity to which control was transferred to. The related events would then have this identifier as their `activity_id` identifier. | keyword |
| winlog.task | The task defined in the event. Task and opcode are typically used to identify the location in the application from where the event was logged. The category used by the Event Logging API (on pre Windows Vista operating systems) is written to this field. | keyword |
| winlog.user.domain | The domain that the account associated with this event is a member of. | keyword |
| winlog.user.identifier | The Windows security identifier (SID) of the account associated with this event. If Winlogbeat cannot resolve the SID to a name, then the `user.name`, `user.domain`, and `user.type` fields will be omitted from the event. If you discover Winlogbeat not resolving SIDs, review the log for clues as to what the problem may be. | keyword |
| winlog.user.name | Name of the user associated with this event. | keyword |
| winlog.user.type | The type of account associated with this event. | keyword |
| winlog.user_data | The event specific data. This field is mutually exclusive with `event_data`. | object |
| winlog.version | The version number of the event's definition. | long |



### Security

The Windows `security` data stream provides events from the Windows
`Security` event log.

#### Supported operating systems

- Windows

An example event for `security` looks as following:

```json
{
    "@timestamp": "2019-11-07T10:37:04.226Z",
    "agent": {
        "ephemeral_id": "aa973fb6-b8fe-427e-a9e9-51c411926db8",
        "id": "dbc761fd-dec4-4bc7-acec-8e5cb02a0cb6",
        "name": "docker-fleet-agent",
        "type": "filebeat",
        "version": "8.2.1"
    },
    "data_stream": {
        "dataset": "system.security",
        "namespace": "ep",
        "type": "logs"
    },
    "ecs": {
        "version": "8.0.0"
    },
    "elastic_agent": {
        "id": "dbc761fd-dec4-4bc7-acec-8e5cb02a0cb6",
        "snapshot": true,
        "version": "8.2.1"
    },
    "event": {
        "action": "logging-service-shutdown",
        "agent_id_status": "verified",
        "category": [
            "process"
        ],
        "code": "1100",
        "created": "2022-05-18T06:07:07.204Z",
        "dataset": "system.security",
        "ingested": "2022-05-18T06:07:08Z",
        "kind": "event",
        "original": "\u003cEvent xmlns='http://schemas.microsoft.com/win/2004/08/events/event'\u003e\u003cSystem\u003e\u003cProvider Name='Microsoft-Windows-Eventlog' Guid='{fc65ddd8-d6ef-4962-83d5-6e5cfe9ce148}'/\u003e\u003cEventID\u003e1100\u003c/EventID\u003e\u003cVersion\u003e0\u003c/Version\u003e\u003cLevel\u003e4\u003c/Level\u003e\u003cTask\u003e103\u003c/Task\u003e\u003cOpcode\u003e0\u003c/Opcode\u003e\u003cKeywords\u003e0x4020000000000000\u003c/Keywords\u003e\u003cTimeCreated SystemTime='2019-11-07T10:37:04.226092500Z'/\u003e\u003cEventRecordID\u003e14257\u003c/EventRecordID\u003e\u003cCorrelation/\u003e\u003cExecution ProcessID='1144' ThreadID='4532'/\u003e\u003cChannel\u003eSecurity\u003c/Channel\u003e\u003cComputer\u003eWIN-41OB2LO92CR.wlbeat.local\u003c/Computer\u003e\u003cSecurity/\u003e\u003c/System\u003e\u003cUserData\u003e\u003cServiceShutdown xmlns='http://manifests.microsoft.com/win/2004/08/windows/eventlog'\u003e\u003c/ServiceShutdown\u003e\u003c/UserData\u003e\u003c/Event\u003e",
        "outcome": "success",
        "provider": "Microsoft-Windows-Eventlog",
        "type": [
            "end"
        ]
    },
    "host": {
        "name": "WIN-41OB2LO92CR.wlbeat.local"
    },
    "input": {
        "type": "httpjson"
    },
    "log": {
        "level": "information"
    },
    "tags": [
        "forwarded",
        "preserve_original_event"
    ],
    "winlog": {
        "channel": "Security",
        "computer_name": "WIN-41OB2LO92CR.wlbeat.local",
        "event_id": "1100",
        "keywords": [
            "Audit Success"
        ],
        "level": "information",
        "opcode": "Info",
        "outcome": "success",
        "process": {
            "pid": 1144,
            "thread": {
                "id": 4532
            }
        },
        "provider_guid": "{fc65ddd8-d6ef-4962-83d5-6e5cfe9ce148}",
        "provider_name": "Microsoft-Windows-Eventlog",
        "record_id": "14257",
        "time_created": "2019-11-07T10:37:04.226Z"
    }
}
```

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset name. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |
| event.action | The action captured by the event. This describes the information in the event. It is more specific than `event.category`. Examples are `group-add`, `process-started`, `file-created`. The value is normally defined by the implementer. | keyword |
| event.category | This is one of four ECS Categorization Fields, and indicates the second level in the ECS category hierarchy. `event.category` represents the "big buckets" of ECS categories. For example, filtering on `event.category:process` yields all events relating to process activity. This field is closely related to `event.type`, which is used as a subcategory. This field is an array. This will allow proper categorization of some events that fall in multiple categories. | keyword |
| event.code | Identification code for this event, if one exists. Some event sources use event codes to identify messages unambiguously, regardless of message language or wording adjustments over time. An example of this is the Windows Event ID. | keyword |
| event.created | event.created contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from @timestamp in that @timestamp typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, @timestamp should be used. | date |
| event.dataset | Event dataset. | constant_keyword |
| event.ingested | Timestamp when an event arrived in the central data store. This is different from `@timestamp`, which is when the event originally occurred.  It's also different from `event.created`, which is meant to capture the first time an agent saw the event. In normal conditions, assuming no tampering, the timestamps should chronologically look like this: `@timestamp` \< `event.created` \< `event.ingested`. | date |
| event.kind | This is one of four ECS Categorization Fields, and indicates the highest level in the ECS category hierarchy. `event.kind` gives high-level information about what type of information the event contains, without being specific to the contents of the event. For example, values of this field distinguish alert events from metric events. The value of this field can be used to inform how these kinds of events should be handled. They may warrant different retention, different access control, it may also help understand whether the data coming in at a regular interval or not. | keyword |
| event.module | Event module | constant_keyword |
| event.outcome | This is one of four ECS Categorization Fields, and indicates the lowest level in the ECS category hierarchy. `event.outcome` simply denotes whether the event represents a success or a failure from the perspective of the entity that produced the event. Note that when a single transaction is described in multiple events, each event may populate different values of `event.outcome`, according to their perspective. Also note that in the case of a compound event (a single event that contains multiple logical events), this field should be populated with the value that best captures the overall success or failure from the perspective of the event producer. Further note that not all events will have an associated outcome. For example, this field is generally not populated for metric events, events with `event.type:info`, or any events for which an outcome does not make logical sense. | keyword |
| event.provider | Source of the event. Event transports such as Syslog or the Windows Event Log typically mention the source of an event. It can be the name of the software that generated the event (e.g. Sysmon, httpd), or of a subsystem of the operating system (kernel, Microsoft-Windows-Security-Auditing). | keyword |
| event.sequence | Sequence number of the event. The sequence number is a value published by some event sources, to make the exact ordering of events unambiguous, regardless of the timestamp precision. | long |
| event.type | This is one of four ECS Categorization Fields, and indicates the third level in the ECS category hierarchy. `event.type` represents a categorization "sub-bucket" that, when used along with the `event.category` field values, enables filtering events down to a level appropriate for single visualization. This field is an array. This will allow proper categorization of some events that fall in multiple event types. | keyword |
| file.directory | Directory where the file is located. It should include the drive letter, when appropriate. | keyword |
| file.extension | File extension, excluding the leading dot. Note that when the file name has multiple extensions (example.tar.gz), only the last one should be captured ("gz", not "tar.gz"). | keyword |
| file.name | Name of the file including the extension, without the directory. | keyword |
| file.path | Full path to the file, including the file name. It should include the drive letter, when appropriate. | keyword |
| file.path.text | Multi-field of `file.path`. | match_only_text |
| file.target_path | Target path for symlinks. | keyword |
| file.target_path.text | Multi-field of `file.target_path`. | match_only_text |
| group.domain | Name of the directory the group is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| group.id | Unique identifier for the group on the system/platform. | keyword |
| group.name | Name of the group. | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host mac addresses. | keyword |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| input.type | Type of Filebeat input. | keyword |
| log.file.path | Full path to the log file this event came from, including the file name. It should include the drive letter, when appropriate. If the event wasn't read from a log file, do not populate this field. | keyword |
| log.level | Original log level of the log event. If the source of the event provides a log level or textual severity, this is the one that goes in `log.level`. If your source doesn't specify one, you may put your event transport's severity here (e.g. Syslog severity). Some examples are `warn`, `err`, `i`, `informational`. | keyword |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| process.args | Array of process arguments, starting with the absolute path to the executable. May be filtered to protect sensitive information. | keyword |
| process.args_count | Length of the process.args array. This field can be useful for querying or performing bucket analysis on how many arguments were provided to start a process. More arguments may be an indication of suspicious activity. | long |
| process.command_line | Full command line that started the process, including the absolute path to the executable, and all arguments. Some arguments may be filtered to protect sensitive information. | wildcard |
| process.command_line.text | Multi-field of `process.command_line`. | match_only_text |
| process.entity_id | Unique identifier for the process. The implementation of this is specified by the data source, but some examples of what could be used here are a process-generated UUID, Sysmon Process GUIDs, or a hash of some uniquely identifying components of a process. Constructing a globally unique identifier is a common practice to mitigate PID reuse as well as to identify a specific process over time, across multiple monitored hosts. | keyword |
| process.executable | Absolute path to the process executable. | keyword |
| process.executable.text | Multi-field of `process.executable`. | match_only_text |
| process.name | Process name. Sometimes called program name or similar. | keyword |
| process.name.text | Multi-field of `process.name`. | match_only_text |
| process.parent.executable | Absolute path to the process executable. | keyword |
| process.parent.executable.text | Multi-field of `process.parent.executable`. | match_only_text |
| process.parent.name | Process name. Sometimes called program name or similar. | keyword |
| process.parent.name.text | Multi-field of `process.parent.name`. | match_only_text |
| process.parent.pid | Process id. | long |
| process.pid | Process id. | long |
| process.title | Process title. The proctitle, some times the same as process name. Can also be different: for example a browser setting its title to the web page currently opened. | keyword |
| process.title.text | Multi-field of `process.title`. | match_only_text |
| related.hash | All the hashes seen on your event. Populating this field, then using it to search for hashes can help in situations where you're unsure what the hash algorithm is (and therefore which key name to search). | keyword |
| related.hosts | All hostnames or other host identifiers seen on your event. Example identifiers include FQDNs, domain names, workstation names, or aliases. | keyword |
| related.ip | All of the IPs seen on your event. | ip |
| related.user | All the user names or other user identifiers seen on the event. | keyword |
| service.name | Name of the service data is collected from. The name of the service is normally user given. This allows for distributed services that run on multiple hosts to correlate the related instances based on the name. In the case of Elasticsearch the `service.name` could contain the cluster name. For Beats the `service.name` is by default a copy of the `service.type` field if no name is specified. | keyword |
| service.type | The type of the service data is collected from. The type can be used to group and correlate logs and metrics from one service type. Example: If logs or metrics are collected from Elasticsearch, `service.type` would be `elasticsearch`. | keyword |
| source.as.number | Unique number allocated to the autonomous system. The autonomous system number (ASN) uniquely identifies each network on the Internet. | long |
| source.as.organization.name | Organization name. | keyword |
| source.as.organization.name.text | Multi-field of `source.as.organization.name`. | match_only_text |
| source.domain | The domain name of the source system. This value may be a host name, a fully qualified domain name, or another host naming format. The value may derive from the original event or be added from enrichment. | keyword |
| source.geo.city_name | City name. | keyword |
| source.geo.continent_name | Name of the continent. | keyword |
| source.geo.country_iso_code | Country ISO code. | keyword |
| source.geo.country_name | Country name. | keyword |
| source.geo.location | Longitude and latitude. | geo_point |
| source.geo.name | User-defined description of a location, at the level of granularity they care about. Could be the name of their data centers, the floor number, if this describes a local physical entity, city names. Not typically used in automated geolocation. | keyword |
| source.geo.region_iso_code | Region ISO code. | keyword |
| source.geo.region_name | Region name. | keyword |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |
| source.port | Port of the source. | long |
| tags | List of keywords used to tag each event. | keyword |
| user.changes.name | Short name or login of the user. | keyword |
| user.changes.name.text | Multi-field of `user.changes.name`. | match_only_text |
| user.domain | Name of the directory the user is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| user.effective.domain | Name of the directory the user is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| user.effective.id | Unique identifier of the user. | keyword |
| user.effective.name | Short name or login of the user. | keyword |
| user.effective.name.text | Multi-field of `user.effective.name`. | match_only_text |
| user.id | Unique identifier of the user. | keyword |
| user.name | Short name or login of the user. | keyword |
| user.name.text | Multi-field of `user.name`. | match_only_text |
| user.target.domain | Name of the directory the user is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| user.target.group.domain | Name of the directory the group is a member of. For example, an LDAP or Active Directory domain name. | keyword |
| user.target.group.id | Unique identifier for the group on the system/platform. | keyword |
| user.target.group.name | Name of the group. | keyword |
| user.target.id | Unique identifier of the user. | keyword |
| user.target.name | Short name or login of the user. | keyword |
| user.target.name.text | Multi-field of `user.target.name`. | match_only_text |
| winlog.activity_id | A globally unique identifier that identifies the current activity. The events that are published with this identifier are part of the same activity. | keyword |
| winlog.api | The event log API type used to read the record. The possible values are "wineventlog" for the Windows Event Log API or "eventlogging" for the Event Logging API. The Event Logging API was designed for Windows Server 2003 or Windows 2000 operating systems. In Windows Vista, the event logging infrastructure was redesigned. On Windows Vista or later operating systems, the Windows Event Log API is used. Winlogbeat automatically detects which API to use for reading event logs. | keyword |
| winlog.channel | The name of the channel from which this record was read. This value is one of the names from the `event_logs` collection in the configuration. | keyword |
| winlog.computerObject.domain |  | keyword |
| winlog.computerObject.id |  | keyword |
| winlog.computerObject.name |  | keyword |
| winlog.computer_name | The name of the computer that generated the record. When using Windows event forwarding, this name can differ from `agent.hostname`. | keyword |
| winlog.event_data | The event-specific data. This field is mutually exclusive with `user_data`. If you are capturing event data on versions prior to Windows Vista, the parameters in `event_data` are named `param1`, `param2`, and so on, because event log parameters are unnamed in earlier versions of Windows. | object |
| winlog.event_data.AccessGranted |  | keyword |
| winlog.event_data.AccessList |  | keyword |
| winlog.event_data.AccessListDescription |  | keyword |
| winlog.event_data.AccessMask |  | keyword |
| winlog.event_data.AccessMaskDescription |  | keyword |
| winlog.event_data.AccessReason |  | keyword |
| winlog.event_data.AccessRemoved |  | keyword |
| winlog.event_data.AccountDomain |  | keyword |
| winlog.event_data.AccountExpires |  | keyword |
| winlog.event_data.AccountName |  | keyword |
| winlog.event_data.AllowedToDelegateTo |  | keyword |
| winlog.event_data.AuditPolicyChanges |  | keyword |
| winlog.event_data.AuditPolicyChangesDescription |  | keyword |
| winlog.event_data.AuditSourceName |  | keyword |
| winlog.event_data.AuthenticationPackageName |  | keyword |
| winlog.event_data.Binary |  | keyword |
| winlog.event_data.BitlockerUserInputTime |  | keyword |
| winlog.event_data.BootMode |  | keyword |
| winlog.event_data.BootType |  | keyword |
| winlog.event_data.BuildVersion |  | keyword |
| winlog.event_data.CallerProcessId |  | keyword |
| winlog.event_data.CallerProcessName |  | keyword |
| winlog.event_data.Category |  | keyword |
| winlog.event_data.CategoryId |  | keyword |
| winlog.event_data.ClientAddress |  | keyword |
| winlog.event_data.ClientName |  | keyword |
| winlog.event_data.CommandLine |  | keyword |
| winlog.event_data.Company |  | keyword |
| winlog.event_data.CorruptionActionState |  | keyword |
| winlog.event_data.CrashOnAuditFailValue |  | keyword |
| winlog.event_data.CreationUtcTime |  | keyword |
| winlog.event_data.Description |  | keyword |
| winlog.event_data.Detail |  | keyword |
| winlog.event_data.DeviceName |  | keyword |
| winlog.event_data.DeviceNameLength |  | keyword |
| winlog.event_data.DeviceTime |  | keyword |
| winlog.event_data.DeviceVersionMajor |  | keyword |
| winlog.event_data.DeviceVersionMinor |  | keyword |
| winlog.event_data.DisplayName |  | keyword |
| winlog.event_data.DomainBehaviorVersion |  | keyword |
| winlog.event_data.DomainName |  | keyword |
| winlog.event_data.DomainPolicyChanged |  | keyword |
| winlog.event_data.DomainSid |  | keyword |
| winlog.event_data.DriveName |  | keyword |
| winlog.event_data.DriverName |  | keyword |
| winlog.event_data.DriverNameLength |  | keyword |
| winlog.event_data.Dummy |  | keyword |
| winlog.event_data.DwordVal |  | keyword |
| winlog.event_data.EntryCount |  | keyword |
| winlog.event_data.EventSourceId |  | keyword |
| winlog.event_data.ExtraInfo |  | keyword |
| winlog.event_data.FailureName |  | keyword |
| winlog.event_data.FailureNameLength |  | keyword |
| winlog.event_data.FailureReason |  | keyword |
| winlog.event_data.FileVersion |  | keyword |
| winlog.event_data.FinalStatus |  | keyword |
| winlog.event_data.Group |  | keyword |
| winlog.event_data.GroupTypeChange |  | keyword |
| winlog.event_data.HandleId |  | keyword |
| winlog.event_data.HomeDirectory |  | keyword |
| winlog.event_data.HomePath |  | keyword |
| winlog.event_data.IdleImplementation |  | keyword |
| winlog.event_data.IdleStateCount |  | keyword |
| winlog.event_data.ImpersonationLevel |  | keyword |
| winlog.event_data.IntegrityLevel |  | keyword |
| winlog.event_data.IpAddress |  | keyword |
| winlog.event_data.IpPort |  | keyword |
| winlog.event_data.KerberosPolicyChange |  | keyword |
| winlog.event_data.KeyLength |  | keyword |
| winlog.event_data.LastBootGood |  | keyword |
| winlog.event_data.LastShutdownGood |  | keyword |
| winlog.event_data.LmPackageName |  | keyword |
| winlog.event_data.LogonGuid |  | keyword |
| winlog.event_data.LogonHours |  | keyword |
| winlog.event_data.LogonID |  | keyword |
| winlog.event_data.LogonId |  | keyword |
| winlog.event_data.LogonProcessName |  | keyword |
| winlog.event_data.LogonType |  | keyword |
| winlog.event_data.MachineAccountQuota |  | keyword |
| winlog.event_data.MajorVersion |  | keyword |
| winlog.event_data.MandatoryLabel |  | keyword |
| winlog.event_data.MaximumPerformancePercent |  | keyword |
| winlog.event_data.MemberName |  | keyword |
| winlog.event_data.MemberSid |  | keyword |
| winlog.event_data.MinimumPerformancePercent |  | keyword |
| winlog.event_data.MinimumThrottlePercent |  | keyword |
| winlog.event_data.MinorVersion |  | keyword |
| winlog.event_data.MixedDomainMode |  | keyword |
| winlog.event_data.NewProcessId |  | keyword |
| winlog.event_data.NewProcessName |  | keyword |
| winlog.event_data.NewSchemeGuid |  | keyword |
| winlog.event_data.NewSd |  | keyword |
| winlog.event_data.NewSdDacl0 |  | keyword |
| winlog.event_data.NewSdDacl1 |  | keyword |
| winlog.event_data.NewSdDacl2 |  | keyword |
| winlog.event_data.NewSdSacl0 |  | keyword |
| winlog.event_data.NewSdSacl1 |  | keyword |
| winlog.event_data.NewSdSacl2 |  | keyword |
| winlog.event_data.NewTargetUserName |  | keyword |
| winlog.event_data.NewTime |  | keyword |
| winlog.event_data.NewUACList |  | keyword |
| winlog.event_data.NewUacValue |  | keyword |
| winlog.event_data.NominalFrequency |  | keyword |
| winlog.event_data.Number |  | keyword |
| winlog.event_data.ObjectName |  | keyword |
| winlog.event_data.ObjectServer |  | keyword |
| winlog.event_data.ObjectType |  | keyword |
| winlog.event_data.OemInformation |  | keyword |
| winlog.event_data.OldSchemeGuid |  | keyword |
| winlog.event_data.OldSd |  | keyword |
| winlog.event_data.OldSdDacl0 |  | keyword |
| winlog.event_data.OldSdDacl1 |  | keyword |
| winlog.event_data.OldSdDacl2 |  | keyword |
| winlog.event_data.OldSdSacl0 |  | keyword |
| winlog.event_data.OldSdSacl1 |  | keyword |
| winlog.event_data.OldSdSacl2 |  | keyword |
| winlog.event_data.OldTargetUserName |  | keyword |
| winlog.event_data.OldTime |  | keyword |
| winlog.event_data.OldUacValue |  | keyword |
| winlog.event_data.OriginalFileName |  | keyword |
| winlog.event_data.PackageName |  | keyword |
| winlog.event_data.ParentProcessName |  | keyword |
| winlog.event_data.PasswordHistoryLength |  | keyword |
| winlog.event_data.PasswordLastSet |  | keyword |
| winlog.event_data.Path |  | keyword |
| winlog.event_data.PerformanceImplementation |  | keyword |
| winlog.event_data.PreAuthType |  | keyword |
| winlog.event_data.PreviousCreationUtcTime |  | keyword |
| winlog.event_data.PreviousTime |  | keyword |
| winlog.event_data.PrimaryGroupId |  | keyword |
| winlog.event_data.PrivilegeList |  | keyword |
| winlog.event_data.ProcessId |  | keyword |
| winlog.event_data.ProcessName |  | keyword |
| winlog.event_data.ProcessPath |  | keyword |
| winlog.event_data.ProcessPid |  | keyword |
| winlog.event_data.Product |  | keyword |
| winlog.event_data.ProfilePath |  | keyword |
| winlog.event_data.PuaCount |  | keyword |
| winlog.event_data.PuaPolicyId |  | keyword |
| winlog.event_data.QfeVersion |  | keyword |
| winlog.event_data.Reason |  | keyword |
| winlog.event_data.RelativeTargetName |  | keyword |
| winlog.event_data.ResourceAttributes |  | keyword |
| winlog.event_data.SamAccountName |  | keyword |
| winlog.event_data.SchemaVersion |  | keyword |
| winlog.event_data.ScriptBlockText |  | keyword |
| winlog.event_data.ScriptPath |  | keyword |
| winlog.event_data.Service |  | keyword |
| winlog.event_data.ServiceAccount |  | keyword |
| winlog.event_data.ServiceFileName |  | keyword |
| winlog.event_data.ServiceName |  | keyword |
| winlog.event_data.ServiceSid |  | keyword |
| winlog.event_data.ServiceStartType |  | keyword |
| winlog.event_data.ServiceType |  | keyword |
| winlog.event_data.ServiceVersion |  | keyword |
| winlog.event_data.SessionName |  | keyword |
| winlog.event_data.ShareLocalPath |  | keyword |
| winlog.event_data.ShareName |  | keyword |
| winlog.event_data.ShutdownActionType |  | keyword |
| winlog.event_data.ShutdownEventCode |  | keyword |
| winlog.event_data.ShutdownReason |  | keyword |
| winlog.event_data.SidFilteringEnabled |  | keyword |
| winlog.event_data.SidHistory |  | keyword |
| winlog.event_data.Signature |  | keyword |
| winlog.event_data.SignatureStatus |  | keyword |
| winlog.event_data.Signed |  | keyword |
| winlog.event_data.StartTime |  | keyword |
| winlog.event_data.State |  | keyword |
| winlog.event_data.Status |  | keyword |
| winlog.event_data.StatusDescription |  | keyword |
| winlog.event_data.StopTime |  | keyword |
| winlog.event_data.SubCategory |  | keyword |
| winlog.event_data.SubCategoryGuid |  | keyword |
| winlog.event_data.SubCategoryId |  | keyword |
| winlog.event_data.SubStatus |  | keyword |
| winlog.event_data.SubcategoryGuid |  | keyword |
| winlog.event_data.SubcategoryId |  | keyword |
| winlog.event_data.SubjectDomainName |  | keyword |
| winlog.event_data.SubjectLogonId |  | keyword |
| winlog.event_data.SubjectUserName |  | keyword |
| winlog.event_data.SubjectUserSid |  | keyword |
| winlog.event_data.TSId |  | keyword |
| winlog.event_data.TargetDomainName |  | keyword |
| winlog.event_data.TargetInfo |  | keyword |
| winlog.event_data.TargetLogonGuid |  | keyword |
| winlog.event_data.TargetLogonId |  | keyword |
| winlog.event_data.TargetServerName |  | keyword |
| winlog.event_data.TargetSid |  | keyword |
| winlog.event_data.TargetUserName |  | keyword |
| winlog.event_data.TargetUserSid |  | keyword |
| winlog.event_data.TdoAttributes |  | keyword |
| winlog.event_data.TdoDirection |  | keyword |
| winlog.event_data.TdoType |  | keyword |
| winlog.event_data.TerminalSessionId |  | keyword |
| winlog.event_data.TicketEncryptionType |  | keyword |
| winlog.event_data.TicketEncryptionTypeDescription |  | keyword |
| winlog.event_data.TicketOptions |  | keyword |
| winlog.event_data.TicketOptionsDescription |  | keyword |
| winlog.event_data.TokenElevationType |  | keyword |
| winlog.event_data.TransmittedServices |  | keyword |
| winlog.event_data.UserAccountControl |  | keyword |
| winlog.event_data.UserParameters |  | keyword |
| winlog.event_data.UserPrincipalName |  | keyword |
| winlog.event_data.UserSid |  | keyword |
| winlog.event_data.UserWorkstations |  | keyword |
| winlog.event_data.Version |  | keyword |
| winlog.event_data.Workstation |  | keyword |
| winlog.event_data.WorkstationName |  | keyword |
| winlog.event_data.param1 |  | keyword |
| winlog.event_data.param2 |  | keyword |
| winlog.event_data.param3 |  | keyword |
| winlog.event_data.param4 |  | keyword |
| winlog.event_data.param5 |  | keyword |
| winlog.event_data.param6 |  | keyword |
| winlog.event_data.param7 |  | keyword |
| winlog.event_data.param8 |  | keyword |
| winlog.event_id | The event identifier. The value is specific to the source of the event. | keyword |
| winlog.keywords | The keywords are used to classify an event. | keyword |
| winlog.level | The event severity.  Levels are Critical, Error, Warning and Information, Verbose | keyword |
| winlog.logon.failure.reason | The reason the logon failed. | keyword |
| winlog.logon.failure.status | The reason the logon failed. This is textual description based on the value of the hexadecimal `Status` field. | keyword |
| winlog.logon.failure.sub_status | Additional information about the logon failure. This is a textual description based on the value of the hexidecimal `SubStatus` field. | keyword |
| winlog.logon.id | Logon ID that can be used to associate this logon with other events related to the same logon session. | keyword |
| winlog.logon.type | Logon type name. This is the descriptive version of the `winlog.event_data.LogonType` ordinal. This is an enrichment added by the Security module. | keyword |
| winlog.opcode | The opcode defined in the event. Task and opcode are typically used to identify the location in the application from where the event was logged. | keyword |
| winlog.outcome | Success or Failure of the event. | keyword |
| winlog.process.pid | The process_id of the Client Server Runtime Process. | long |
| winlog.process.thread.id |  | long |
| winlog.provider_guid | A globally unique identifier that identifies the provider that logged the event. | keyword |
| winlog.provider_name | The source of the event log record (the application or service that logged the record). | keyword |
| winlog.record_id | The record ID of the event log record. The first record written to an event log is record number 1, and other records are numbered sequentially. If the record number reaches the maximum value (2^32^ for the Event Logging API and 2^64^ for the Windows Event Log API), the next record number will be 0. | keyword |
| winlog.related_activity_id | A globally unique identifier that identifies the activity to which control was transferred to. The related events would then have this identifier as their `activity_id` identifier. | keyword |
| winlog.task | The task defined in the event. Task and opcode are typically used to identify the location in the application from where the event was logged. The category used by the Event Logging API (on pre Windows Vista operating systems) is written to this field. | keyword |
| winlog.time_created | Time event was created | keyword |
| winlog.trustAttribute |  | keyword |
| winlog.trustDirection |  | keyword |
| winlog.trustType |  | keyword |
| winlog.user.domain | The domain that the account associated with this event is a member of. | keyword |
| winlog.user.identifier | The Windows security identifier (SID) of the account associated with this event. If Winlogbeat cannot resolve the SID to a name, then the `user.name`, `user.domain`, and `user.type` fields will be omitted from the event. If you discover Winlogbeat not resolving SIDs, review the log for clues as to what the problem may be. | keyword |
| winlog.user.name | Name of the user associated with this event. | keyword |
| winlog.user.type | The type of account associated with this event. | keyword |
| winlog.user_data | The event specific data. This field is mutually exclusive with `event_data`. | object |
| winlog.user_data.BackupPath |  | keyword |
| winlog.user_data.Channel |  | keyword |
| winlog.user_data.SubjectDomainName |  | keyword |
| winlog.user_data.SubjectLogonId |  | keyword |
| winlog.user_data.SubjectUserName |  | keyword |
| winlog.user_data.SubjectUserSid |  | keyword |
| winlog.user_data.xml_name |  | keyword |
| winlog.version | The version number of the event's definition. | long |


### Auth

The `auth` data stream provides auth logs.

#### Supported operating systems

- macOS prior to 10.8
- Linux

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |
| error.message | Error message. | match_only_text |
| event.action | The action captured by the event. This describes the information in the event. It is more specific than `event.category`. Examples are `group-add`, `process-started`, `file-created`. The value is normally defined by the implementer. | keyword |
| event.category | This is one of four ECS Categorization Fields, and indicates the second level in the ECS category hierarchy. `event.category` represents the "big buckets" of ECS categories. For example, filtering on `event.category:process` yields all events relating to process activity. This field is closely related to `event.type`, which is used as a subcategory. This field is an array. This will allow proper categorization of some events that fall in multiple categories. | keyword |
| event.code | Identification code for this event, if one exists. Some event sources use event codes to identify messages unambiguously, regardless of message language or wording adjustments over time. An example of this is the Windows Event ID. | keyword |
| event.created | event.created contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from @timestamp in that @timestamp typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, @timestamp should be used. | date |
| event.dataset | Event dataset. | constant_keyword |
| event.ingested | Timestamp when an event arrived in the central data store. This is different from `@timestamp`, which is when the event originally occurred.  It's also different from `event.created`, which is meant to capture the first time an agent saw the event. In normal conditions, assuming no tampering, the timestamps should chronologically look like this: `@timestamp` \< `event.created` \< `event.ingested`. | date |
| event.kind | This is one of four ECS Categorization Fields, and indicates the highest level in the ECS category hierarchy. `event.kind` gives high-level information about what type of information the event contains, without being specific to the contents of the event. For example, values of this field distinguish alert events from metric events. The value of this field can be used to inform how these kinds of events should be handled. They may warrant different retention, different access control, it may also help understand whether the data coming in at a regular interval or not. | keyword |
| event.module | Event module | constant_keyword |
| event.outcome | This is one of four ECS Categorization Fields, and indicates the lowest level in the ECS category hierarchy. `event.outcome` simply denotes whether the event represents a success or a failure from the perspective of the entity that produced the event. Note that when a single transaction is described in multiple events, each event may populate different values of `event.outcome`, according to their perspective. Also note that in the case of a compound event (a single event that contains multiple logical events), this field should be populated with the value that best captures the overall success or failure from the perspective of the event producer. Further note that not all events will have an associated outcome. For example, this field is generally not populated for metric events, events with `event.type:info`, or any events for which an outcome does not make logical sense. | keyword |
| event.provider | Source of the event. Event transports such as Syslog or the Windows Event Log typically mention the source of an event. It can be the name of the software that generated the event (e.g. Sysmon, httpd), or of a subsystem of the operating system (kernel, Microsoft-Windows-Security-Auditing). | keyword |
| event.sequence | Sequence number of the event. The sequence number is a value published by some event sources, to make the exact ordering of events unambiguous, regardless of the timestamp precision. | long |
| event.type | This is one of four ECS Categorization Fields, and indicates the third level in the ECS category hierarchy. `event.type` represents a categorization "sub-bucket" that, when used along with the `event.category` field values, enables filtering events down to a level appropriate for single visualization. This field is an array. This will allow proper categorization of some events that fall in multiple event types. | keyword |
| group.id | Unique identifier for the group on the system/platform. | keyword |
| group.name | Name of the group. | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host mac addresses. | keyword |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.full | Operating system name, including the version or code name. | keyword |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| process.name | Process name. Sometimes called program name or similar. | keyword |
| process.name.text | Multi-field of `process.name`. | match_only_text |
| process.pid | Process id. | long |
| related.hosts | All hostnames or other host identifiers seen on your event. Example identifiers include FQDNs, domain names, workstation names, or aliases. | keyword |
| related.ip | All of the IPs seen on your event. | ip |
| related.user | All the user names or other user identifiers seen on the event. | keyword |
| source.as.number | Unique number allocated to the autonomous system. The autonomous system number (ASN) uniquely identifies each network on the Internet. | long |
| source.as.organization.name | Organization name. | keyword |
| source.as.organization.name.text | Multi-field of `source.as.organization.name`. | match_only_text |
| source.geo.city_name | City name. | keyword |
| source.geo.continent_name | Name of the continent. | keyword |
| source.geo.country_iso_code | Country ISO code. | keyword |
| source.geo.country_name | Country name. | keyword |
| source.geo.location | Longitude and latitude. | geo_point |
| source.geo.region_iso_code | Region ISO code. | keyword |
| source.geo.region_name | Region name. | keyword |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |
| source.port | Port of the source. | long |
| system.auth.ssh.dropped_ip | The client IP from SSH connections that are open and immediately dropped. | ip |
| system.auth.ssh.event | The SSH event as found in the logs (Accepted, Invalid, Failed, etc.) | keyword |
| system.auth.ssh.method | The SSH authentication method. Can be one of "password" or "publickey". | keyword |
| system.auth.ssh.signature | The signature of the client public key. | keyword |
| system.auth.sudo.command | The command executed via sudo. | keyword |
| system.auth.sudo.error | The error message in case the sudo command failed. | keyword |
| system.auth.sudo.pwd | The current directory where the sudo command is executed. | keyword |
| system.auth.sudo.tty | The TTY where the sudo command is executed. | keyword |
| system.auth.sudo.user | The target user to which the sudo command is switching. | keyword |
| system.auth.useradd.home | The home folder for the new user. | keyword |
| system.auth.useradd.shell | The default shell for the new user. | keyword |
| user.effective.name | Short name or login of the user. | keyword |
| user.effective.name.text | Multi-field of `user.effective.name`. | match_only_text |
| user.id | Unique identifier of the user. | keyword |
| user.name | Short name or login of the user. | keyword |
| user.name.text | Multi-field of `user.name`. | match_only_text |
| version | Operating system version as a raw string. | keyword |


### syslog

The `syslog` data stream provides system logs.

#### Supported operating systems

- macOS
- Linux

**Exported fields**

| Field | Description | Type |
|---|---|---|
| @timestamp | Event timestamp. | date |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |
| cloud.image.id | Image ID for the cloud instance. | keyword |
| cloud.instance.id | Instance ID of the host machine. | keyword |
| cloud.instance.name | Instance name of the host machine. | keyword |
| cloud.machine.type | Machine type of the host machine. | keyword |
| cloud.project.id | Name of the project in Google Cloud. | keyword |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |
| cloud.region | Region in which this host is running. | keyword |
| container.id | Unique container id. | keyword |
| container.image.name | Name of the image the container was built on. | keyword |
| container.labels | Image labels. | object |
| container.name | Container name. | keyword |
| data_stream.dataset | Data stream dataset. | constant_keyword |
| data_stream.namespace | Data stream namespace. | constant_keyword |
| data_stream.type | Data stream type. | constant_keyword |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |
| event.action | The action captured by the event. This describes the information in the event. It is more specific than `event.category`. Examples are `group-add`, `process-started`, `file-created`. The value is normally defined by the implementer. | keyword |
| event.category | This is one of four ECS Categorization Fields, and indicates the second level in the ECS category hierarchy. `event.category` represents the "big buckets" of ECS categories. For example, filtering on `event.category:process` yields all events relating to process activity. This field is closely related to `event.type`, which is used as a subcategory. This field is an array. This will allow proper categorization of some events that fall in multiple categories. | keyword |
| event.code | Identification code for this event, if one exists. Some event sources use event codes to identify messages unambiguously, regardless of message language or wording adjustments over time. An example of this is the Windows Event ID. | keyword |
| event.created | event.created contains the date/time when the event was first read by an agent, or by your pipeline. This field is distinct from @timestamp in that @timestamp typically contain the time extracted from the original event. In most situations, these two timestamps will be slightly different. The difference can be used to calculate the delay between your source generating an event, and the time when your agent first processed it. This can be used to monitor your agent's or pipeline's ability to keep up with your event source. In case the two timestamps are identical, @timestamp should be used. | date |
| event.dataset | Event dataset. | constant_keyword |
| event.ingested | Timestamp when an event arrived in the central data store. This is different from `@timestamp`, which is when the event originally occurred.  It's also different from `event.created`, which is meant to capture the first time an agent saw the event. In normal conditions, assuming no tampering, the timestamps should chronologically look like this: `@timestamp` \< `event.created` \< `event.ingested`. | date |
| event.kind | This is one of four ECS Categorization Fields, and indicates the highest level in the ECS category hierarchy. `event.kind` gives high-level information about what type of information the event contains, without being specific to the contents of the event. For example, values of this field distinguish alert events from metric events. The value of this field can be used to inform how these kinds of events should be handled. They may warrant different retention, different access control, it may also help understand whether the data coming in at a regular interval or not. | keyword |
| event.module | Event module | constant_keyword |
| event.outcome | This is one of four ECS Categorization Fields, and indicates the lowest level in the ECS category hierarchy. `event.outcome` simply denotes whether the event represents a success or a failure from the perspective of the entity that produced the event. Note that when a single transaction is described in multiple events, each event may populate different values of `event.outcome`, according to their perspective. Also note that in the case of a compound event (a single event that contains multiple logical events), this field should be populated with the value that best captures the overall success or failure from the perspective of the event producer. Further note that not all events will have an associated outcome. For example, this field is generally not populated for metric events, events with `event.type:info`, or any events for which an outcome does not make logical sense. | keyword |
| event.provider | Source of the event. Event transports such as Syslog or the Windows Event Log typically mention the source of an event. It can be the name of the software that generated the event (e.g. Sysmon, httpd), or of a subsystem of the operating system (kernel, Microsoft-Windows-Security-Auditing). | keyword |
| event.sequence | Sequence number of the event. The sequence number is a value published by some event sources, to make the exact ordering of events unambiguous, regardless of the timestamp precision. | long |
| event.type | This is one of four ECS Categorization Fields, and indicates the third level in the ECS category hierarchy. `event.type` represents a categorization "sub-bucket" that, when used along with the `event.category` field values, enables filtering events down to a level appropriate for single visualization. This field is an array. This will allow proper categorization of some events that fall in multiple event types. | keyword |
| host.architecture | Operating system architecture. | keyword |
| host.containerized | If the host is a container. | boolean |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |
| host.ip | Host ip addresses. | ip |
| host.mac | Host MAC addresses. The notation format from RFC 7042 is suggested: Each octet (that is, 8-bit byte) is represented by two [uppercase] hexadecimal digits giving the value of the octet as an unsigned integer. Successive octets are separated by a hyphen. | keyword |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |
| host.os.build | OS build information. | keyword |
| host.os.codename | OS codename, if any. | keyword |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |
| host.os.full | Operating system name, including the version or code name. | keyword |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |
| host.os.name | Operating system name, without the version. | keyword |
| host.os.name.text | Multi-field of `host.os.name`. | text |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |
| host.os.version | Operating system version as a raw string. | keyword |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |
| process.name | Process name. Sometimes called program name or similar. | keyword |
| process.name.text | Multi-field of `process.name`. | match_only_text |
| process.pid | Process id. | long |


## Metrics reference

### Core

The System `core` data stream provides usage statistics for each CPU core.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- OpenBSD
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host MAC addresses. The notation format from RFC 7042 is suggested: Each octet (that is, 8-bit byte) is represented by two [uppercase] hexadecimal digits giving the value of the octet as an unsigned integer. Successive octets are separated by a hyphen. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.full | Operating system name, including the version or code name. | keyword |  |  |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | match_only_text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| system.core.id | CPU Core number. | keyword |  |  |
| system.core.idle.pct | The percentage of CPU time spent idle. | scaled_float | percent | gauge |
| system.core.idle.ticks | The amount of CPU time spent idle. | long |  | counter |
| system.core.iowait.pct | The percentage of CPU time spent in wait (on disk). | scaled_float | percent | gauge |
| system.core.iowait.ticks | The amount of CPU time spent in wait (on disk). | long |  | counter |
| system.core.irq.pct | The percentage of CPU time spent servicing and handling hardware interrupts. | scaled_float | percent | gauge |
| system.core.irq.ticks | The amount of CPU time spent servicing and handling hardware interrupts. | long |  | counter |
| system.core.nice.pct | The percentage of CPU time spent on low-priority processes. | scaled_float | percent | gauge |
| system.core.nice.ticks | The amount of CPU time spent on low-priority processes. | long |  | counter |
| system.core.softirq.pct | The percentage of CPU time spent servicing and handling software interrupts. | scaled_float | percent | gauge |
| system.core.softirq.ticks | The amount of CPU time spent servicing and handling software interrupts. | long |  | counter |
| system.core.steal.pct | The percentage of CPU time spent in involuntary wait by the virtual CPU while the hypervisor was servicing another processor. Available only on Unix. | scaled_float | percent | gauge |
| system.core.steal.ticks | The amount of CPU time spent in involuntary wait by the virtual CPU while the hypervisor was servicing another processor. Available only on Unix. | long |  | counter |
| system.core.system.pct | The percentage of CPU time spent in kernel space. | scaled_float | percent | gauge |
| system.core.system.ticks | The amount of CPU time spent in kernel space. | long |  | counter |
| system.core.user.pct | The percentage of CPU time spent in user space. | scaled_float | percent | gauge |
| system.core.user.ticks | The amount of CPU time spent in user space. | long |  | counter |


### CPU

The System `cpu` data stream provides CPU statistics.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- OpenBSD
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.cpu.pct | Percent CPU used. This value is normalized by the number of CPU cores and it ranges from 0 to 1. | scaled_float |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.full | Operating system name, including the version or code name. | keyword |  |  |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | match_only_text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| system.cpu.cores | The number of CPU cores present on the host. The non-normalized percentages will have a maximum value of `100% \* cores`. The normalized percentages already take this value into account and have a maximum value of 100%. | long |  | gauge |
| system.cpu.idle.norm.pct | The percentage of CPU time spent idle. | scaled_float | percent | gauge |
| system.cpu.idle.pct | The percentage of CPU time spent idle. | scaled_float | percent | gauge |
| system.cpu.idle.ticks | The amount of CPU time spent idle. | long |  | counter |
| system.cpu.iowait.norm.pct | The percentage of CPU time spent in wait (on disk). | scaled_float | percent | gauge |
| system.cpu.iowait.pct | The percentage of CPU time spent in wait (on disk). | scaled_float | percent | gauge |
| system.cpu.iowait.ticks | The amount of CPU time spent in wait (on disk). | long |  | counter |
| system.cpu.irq.norm.pct | The percentage of CPU time spent servicing and handling hardware interrupts. | scaled_float | percent | gauge |
| system.cpu.irq.pct | The percentage of CPU time spent servicing and handling hardware interrupts. | scaled_float | percent | gauge |
| system.cpu.irq.ticks | The amount of CPU time spent servicing and handling hardware interrupts. | long |  | counter |
| system.cpu.nice.norm.pct | The percentage of CPU time spent on low-priority processes. | scaled_float | percent | gauge |
| system.cpu.nice.pct | The percentage of CPU time spent on low-priority processes. | scaled_float | percent | gauge |
| system.cpu.nice.ticks | The amount of CPU time spent on low-priority processes. | long |  | counter |
| system.cpu.softirq.norm.pct | The percentage of CPU time spent servicing and handling software interrupts. | scaled_float | percent | gauge |
| system.cpu.softirq.pct | The percentage of CPU time spent servicing and handling software interrupts. | scaled_float | percent | gauge |
| system.cpu.softirq.ticks | The amount of CPU time spent servicing and handling software interrupts. | long |  | counter |
| system.cpu.steal.norm.pct | The percentage of CPU time spent in involuntary wait by the virtual CPU while the hypervisor was servicing another processor. Available only on Unix. | scaled_float | percent | gauge |
| system.cpu.steal.pct | The percentage of CPU time spent in involuntary wait by the virtual CPU while the hypervisor was servicing another processor. Available only on Unix. | scaled_float | percent | gauge |
| system.cpu.steal.ticks | The amount of CPU time spent in involuntary wait by the virtual CPU while the hypervisor was servicing another processor. Available only on Unix. | long |  | counter |
| system.cpu.system.norm.pct | The percentage of CPU time spent in kernel space. | scaled_float | percent | gauge |
| system.cpu.system.pct | The percentage of CPU time spent in kernel space. | scaled_float | percent | gauge |
| system.cpu.system.ticks | The amount of CPU time spent in kernel space. | long |  |  |
| system.cpu.total.norm.pct | The percentage of CPU time in states other than Idle and IOWait, normalised by the number of cores. | scaled_float | percent | gauge |
| system.cpu.total.pct | The percentage of CPU time spent in states other than Idle and IOWait. | scaled_float | percent | gauge |
| system.cpu.user.norm.pct | The percentage of CPU time spent in user space. | scaled_float | percent | gauge |
| system.cpu.user.pct | The percentage of CPU time spent in user space. On multi-core systems, you can have percentages that are greater than 100%. For example, if 3 cores are at 60% use, then the `system.cpu.user.pct` will be 180%. | scaled_float | percent | gauge |
| system.cpu.user.ticks | The amount of CPU time spent in user space. | long |  | counter |


### Disk IO

The System `diskio` data stream provides disk IO metrics collected from the
operating system. One event is created for each disk mounted on the system.

#### Supported operating systems

- Linux
- macOS (requires 10.10+)
- Windows
- FreeBSD (amd64)

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.disk.read.bytes | The total number of bytes read successfully in a given period of time. | scaled_float | byte | gauge |
| host.disk.write.bytes | The total number of bytes write successfully in a given period of time. | scaled_float | byte | gauge |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.full | Operating system name, including the version or code name. | keyword |  |  |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | match_only_text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| system.diskio.io.time | The total number of of milliseconds spent doing I/Os. | long |  | counter |
| system.diskio.iostat.await | The average time spent for requests issued to the device to be served. | float |  | gauge |
| system.diskio.iostat.busy | Percentage of CPU time during which I/O requests were issued to the device (bandwidth utilization for the device). Device saturation occurs when this value is close to 100%. | float |  | gauge |
| system.diskio.iostat.queue.avg_size | The average queue length of the requests that were issued to the device. | float | byte | gauge |
| system.diskio.iostat.read.await | The average time spent for read requests issued to the device to be served. | float |  | gauge |
| system.diskio.iostat.read.per_sec.bytes | The number of Bytes read from the device per second. | float |  | gauge |
| system.diskio.iostat.read.request.merges_per_sec | The number of read requests merged per second that were queued to the device. | float |  | gauge |
| system.diskio.iostat.read.request.per_sec | The number of read requests that were issued to the device per second | float |  | gauge |
| system.diskio.iostat.request.avg_size | The average size (in bytes) of the requests that were issued to the device. | float | byte | gauge |
| system.diskio.iostat.service_time | The average service time (in milliseconds) for I/O requests that were issued to the device. | float | ms | gauge |
| system.diskio.iostat.write.await | The average time spent for write requests issued to the device to be served. | float |  | gauge |
| system.diskio.iostat.write.per_sec.bytes | The number of Bytes write from the device per second. | float |  | gauge |
| system.diskio.iostat.write.request.merges_per_sec | The number of write requests merged per second that were queued to the device. | float |  | gauge |
| system.diskio.iostat.write.request.per_sec | The number of write requests that were issued to the device per second | float |  | gauge |
| system.diskio.name | The disk name. | keyword |  |  |
| system.diskio.read.bytes | The total number of bytes read successfully. On Linux this is the number of sectors read multiplied by an assumed sector size of 512. | long | byte | counter |
| system.diskio.read.count | The total number of reads completed successfully. | long |  | counter |
| system.diskio.read.time | The total number of milliseconds spent by all reads. | long |  | counter |
| system.diskio.serial_number | The disk's serial number. This may not be provided by all operating systems. | keyword |  |  |
| system.diskio.write.bytes | The total number of bytes written successfully. On Linux this is the number of sectors written multiplied by an assumed sector size of 512. | long | byte | counter |
| system.diskio.write.count | The total number of writes completed successfully. | long |  | counter |
| system.diskio.write.time | The total number of milliseconds spent by all writes. | long |  | counter |


### Filesystem

The System `filesystem` data stream provides file system statistics. For each file
system, one document is provided.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- OpenBSD
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| system.filesystem.available | The disk space available to an unprivileged user in bytes. | long | byte | gauge |
| system.filesystem.device_name | The disk name. For example: `/dev/disk1` | keyword |  |  |
| system.filesystem.files | The total number of file nodes in the file system. | long |  | gauge |
| system.filesystem.free | The disk space available in bytes. | long | byte | gauge |
| system.filesystem.free_files | The number of free file nodes in the file system. | long |  | gauge |
| system.filesystem.mount_point | The mounting point. For example: `/` | keyword |  |  |
| system.filesystem.total | The total disk space in bytes. | long | byte | gauge |
| system.filesystem.type | The disk type. For example: `ext4` | keyword |  |  |
| system.filesystem.used.bytes | The used disk space in bytes. | long | byte | gauge |
| system.filesystem.used.pct | The percentage of used disk space. | scaled_float | percent | gauge |


### Fsstat

The System `fsstat` data stream provides overall file system statistics.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- OpenBSD
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.full | Operating system name, including the version or code name. | keyword |  |  |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| system.fsstat.count | Number of file systems found. | long |  | gauge |
| system.fsstat.total_files | Total number of files. | long |  | gauge |
| system.fsstat.total_size.free | Total free space. | long | byte | gauge |
| system.fsstat.total_size.total | Total space (used plus free). | long | byte | gauge |
| system.fsstat.total_size.used | Total used space. | long | byte | gauge |


### Load

The System `load` data stream provides load statistics.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- OpenBSD

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Metric Type |
|---|---|---|---|
| @timestamp | Event timestamp. | date |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |
| cloud.region | Region in which this host is running. | keyword |  |
| container.id | Unique container id. | keyword |  |
| container.image.name | Name of the image the container was built on. | keyword |  |
| container.labels | Image labels. | object |  |
| container.name | Container name. | keyword |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |
| data_stream.type | Data stream type. | constant_keyword |  |
| event.dataset | Event dataset. | constant_keyword |  |
| event.module | Event module | constant_keyword |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |
| host.architecture | Operating system architecture. | keyword |  |
| host.containerized | If the host is a container. | boolean |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |
| host.ip | Host ip addresses. | ip |  |
| host.mac | Host mac addresses. | keyword |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |
| host.os.build | OS build information. | keyword |  |
| host.os.codename | OS codename, if any. | keyword |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |
| host.os.full | Operating system name, including the version or code name. | keyword |  |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |
| host.os.name | Operating system name, without the version. | keyword |  |
| host.os.name.text | Multi-field of `host.os.name`. | match_only_text |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |
| host.os.version | Operating system version as a raw string. | keyword |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |
| system.load.1 | Load average for the last minute. | scaled_float | gauge |
| system.load.15 | Load average for the last 15 minutes. | scaled_float | gauge |
| system.load.5 | Load average for the last 5 minutes. | scaled_float | gauge |
| system.load.cores | The number of CPU cores present on the host. | long | gauge |
| system.load.norm.1 | Load for the last minute divided by the number of cores. | scaled_float | gauge |
| system.load.norm.15 | Load for the last 15 minutes divided by the number of cores. | scaled_float | gauge |
| system.load.norm.5 | Load for the last 5 minutes divided by the number of cores. | scaled_float | gauge |


### Memory

The System `memory` data stream provides memory statistics.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- OpenBSD
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.full | Operating system name, including the version or code name. | keyword |  |  |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | match_only_text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| system.memory.actual.free | Actual free memory in bytes. It is calculated based on the OS. On Linux this value will be MemAvailable from /proc/meminfo,  or calculated from free memory plus caches and buffers if /proc/meminfo is not available. On OSX it is a sum of free memory and the inactive memory. On Windows, it is equal to `system.memory.free`. | long | byte | gauge |
| system.memory.actual.used.bytes | Actual used memory in bytes. It represents the difference between the total and the available memory. The available memory depends on the OS. For more details, please check `system.actual.free`. | long | byte | gauge |
| system.memory.actual.used.pct | The percentage of actual used memory. | scaled_float | percent | gauge |
| system.memory.free | The total amount of free memory in bytes. This value does not include memory consumed by system caches and buffers (see system.memory.actual.free). | long | byte | gauge |
| system.memory.hugepages.default_size | Default size for huge pages. | long |  | gauge |
| system.memory.hugepages.free | Number of available huge pages in the pool. | long |  | gauge |
| system.memory.hugepages.reserved | Number of reserved but not allocated huge pages in the pool. | long |  | gauge |
| system.memory.hugepages.surplus | Number of overcommited huge pages. | long |  | gauge |
| system.memory.hugepages.swap.out.fallback | Count of huge pages that must be split before swapout | long |  | gauge |
| system.memory.hugepages.swap.out.pages | pages swapped out | long |  | gauge |
| system.memory.hugepages.total | Number of huge pages in the pool. | long |  | gauge |
| system.memory.hugepages.used.bytes | Memory used in allocated huge pages. | long | byte | gauge |
| system.memory.hugepages.used.pct | Percentage of huge pages used. | long | percent | gauge |
| system.memory.page_stats.direct_efficiency.pct | direct reclaim efficiency percentage. A lower percentage indicates the system is struggling to reclaim memory. | scaled_float | percent | gauge |
| system.memory.page_stats.kswapd_efficiency.pct | kswapd reclaim efficiency percentage. A lower percentage indicates the system is struggling to reclaim memory. | scaled_float | percent | gauge |
| system.memory.page_stats.pgfree.pages | pages freed by the system | long |  | counter |
| system.memory.page_stats.pgscan_direct.pages | pages scanned directly | long |  | counter |
| system.memory.page_stats.pgscan_kswapd.pages | pages scanned by kswapd | long |  | counter |
| system.memory.page_stats.pgsteal_direct.pages | number of pages reclaimed directly | long |  | counter |
| system.memory.page_stats.pgsteal_kswapd.pages | number of pages reclaimed by kswapd | long |  | counter |
| system.memory.swap.free | Available swap memory. | long | byte | gauge |
| system.memory.swap.in.pages | count of pages swapped in | long |  | gauge |
| system.memory.swap.out.pages | count of pages swapped out | long |  | counter |
| system.memory.swap.readahead.cached | swap readahead cache hits | long |  |  |
| system.memory.swap.readahead.pages | swap readahead pages | long |  | counter |
| system.memory.swap.total | Total swap memory. | long | byte | gauge |
| system.memory.swap.used.bytes | Used swap memory. | long | byte | gauge |
| system.memory.swap.used.pct | The percentage of used swap memory. | scaled_float | percent | gauge |
| system.memory.total | Total memory. | long | byte | gauge |
| system.memory.used.bytes | Used memory. | long | byte | gauge |
| system.memory.used.pct | The percentage of used memory. | scaled_float | percent | gauge |


### Network

The System `network` data stream provides network IO metrics collected from the
operating system. One event is created for each network interface.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Date/time when the event originated. This is the date/time extracted from the event, typically representing when the event was generated by the source. If the event source has no original timestamp, this value is typically populated by the first time the event was received by the pipeline. Required field for all events. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| group | The group fields are meant to represent groups that are relevant to the event. | group |  |  |
| group.id | Unique identifier for the group on the system/platform. | keyword |  |  |
| group.name | Name of the group. | keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.network.in.bytes | The number of bytes received on all network interfaces by the host in a given period of time. | scaled_float | byte | counter |
| host.network.in.packets | The number of packets received on all network interfaces by the host in a given period of time. | scaled_float |  | counter |
| host.network.out.bytes | The number of bytes sent out on all network interfaces by the host in a given period of time. | long |  |  |
| host.network.out.packets | The number of packets sent out on all network interfaces by the host in a given period of time. | long |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |  |  |
| process | These fields contain information about a process. These fields can help you correlate metrics information with a process id/name from a log message.  The `process.pid` often stays in the metric itself and is copied to the global field for correlation. | group |  |  |
| process.name | Process name. Sometimes called program name or similar. | keyword |  |  |
| process.name.text | Multi-field of `process.name`. | match_only_text |  |  |
| process.pid | Process id. | long |  |  |
| source | Source fields capture details about the sender of a network exchange/packet. These fields are populated from a network event, packet, or other event containing details of a network transaction. Source fields are usually populated in conjunction with destination fields. The source and destination fields are considered the baseline and should always be filled if an event contains source and destination details from a network transaction. If the event also contains identification of the client and server roles, then the client and server fields should also be populated. | group |  |  |
| source.geo.city_name | City name. | keyword |  |  |
| source.geo.continent_name | Name of the continent. | keyword |  |  |
| source.geo.country_iso_code | Country ISO code. | keyword |  |  |
| source.geo.location | Longitude and latitude. | geo_point |  |  |
| source.geo.region_iso_code | Region ISO code. | keyword |  |  |
| source.geo.region_name | Region name. | keyword |  |  |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |  |  |
| source.port | Port of the source. | long |  |  |
| system.network.in.bytes | The number of bytes received. | long | byte | counter |
| system.network.in.dropped | The number of incoming packets that were dropped. | long |  | counter |
| system.network.in.errors | The number of errors while receiving. | long |  | counter |
| system.network.in.packets | The number or packets received. | long |  | counter |
| system.network.name | The network interface name. | keyword |  |  |
| system.network.out.bytes | The number of bytes sent. | long | byte | counter |
| system.network.out.dropped | The number of outgoing packets that were dropped. This value is always 0 on Darwin and BSD because it is not reported by the operating system. | long |  | counter |
| system.network.out.errors | The number of errors while sending. | long |  | counter |
| system.network.out.packets | The number of packets sent. | long |  | counter |
| user | The user fields describe information about the user that is relevant to the event. Fields can have one entry or multiple entries. If a user has more than one id, provide an array that includes all of them. | group |  |  |
| user.id | Unique identifier of the user. | keyword |  |  |
| user.name | Short name or login of the user. | keyword |  |  |
| user.name.text | Multi-field of `user.name`. | match_only_text |  |  |


### Process

The System `process` data stream provides process statistics. One document is
provided for each process.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- Windows

#### Permissions

Process execution data should be available for an authorized user.
If running as less privileged user, it may not be able to read process data belonging to other users.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| ecs.version | ECS version this event conforms to. `ecs.version` is a required field and must exist in all events. When querying across multiple indices -- which may conform to slightly different ECS versions -- this field lets integrations adjust to the schema version of the events. | keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host MAC addresses. The notation format from RFC 7042 is suggested: Each octet (that is, 8-bit byte) is represented by two [uppercase] hexadecimal digits giving the value of the octet as an unsigned integer. Successive octets are separated by a hyphen. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.full | Operating system name, including the version or code name. | keyword |  |  |
| host.os.full.text | Multi-field of `host.os.full`. | match_only_text |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| process | These fields contain information about a process. These fields can help you correlate metrics information with a process id/name from a log message.  The `process.pid` often stays in the metric itself and is copied to the global field for correlation. | group |  |  |
| process.args | Array of process arguments, starting with the absolute path to the executable. May be filtered to protect sensitive information. | keyword |  |  |
| process.command_line | Full command line that started the process, including the absolute path to the executable, and all arguments. Some arguments may be filtered to protect sensitive information. | wildcard |  |  |
| process.command_line.text | Multi-field of `process.command_line`. | match_only_text |  |  |
| process.cpu.pct | The percentage of CPU time spent by the process since the last event. This value is normalized by the number of CPU cores and it ranges from 0 to 1. | scaled_float |  |  |
| process.cpu.start_time | The time when the process was started. | date |  |  |
| process.executable | Absolute path to the process executable. | keyword |  |  |
| process.executable.text | Multi-field of `process.executable`. | match_only_text |  |  |
| process.memory.pct | The percentage of memory the process occupied in main memory (RAM). | scaled_float |  |  |
| process.name | Process name. Sometimes called program name or similar. | keyword |  |  |
| process.name.text | Multi-field of `process.name`. | match_only_text |  |  |
| process.parent.pid | Process id. | long |  |  |
| process.pgid | Identifier of the group of processes the process belongs to. | long |  |  |
| process.pid | Process id. | long |  |  |
| process.state | The process state. For example: "running". | keyword |  |  |
| process.working_directory | The working directory of the process. | keyword |  |  |
| process.working_directory.text | Multi-field of `process.working_directory`. | match_only_text |  |  |
| service.type | The type of the service data is collected from. The type can be used to group and correlate logs and metrics from one service type. Example: If logs or metrics are collected from Elasticsearch, `service.type` would be `elasticsearch`. | keyword |  |  |
| system.process.cgroup.blkio.id | ID of the cgroup. | keyword |  |  |
| system.process.cgroup.blkio.path | Path to the cgroup relative to the cgroup subsystems mountpoint. | keyword |  |  |
| system.process.cgroup.blkio.total.bytes | Total number of bytes transferred to and from all block devices by processes in the cgroup. | long |  |  |
| system.process.cgroup.blkio.total.ios | Total number of I/O operations performed on all devices by processes in the cgroup as seen by the throttling policy. | long |  |  |
| system.process.cgroup.cgroups_version | The version of cgroups reported for the process | long |  |  |
| system.process.cgroup.cpu.cfs.period.us | Period of time in microseconds for how regularly a cgroup's access to CPU resources should be reallocated. | long |  |  |
| system.process.cgroup.cpu.cfs.quota.us | Total amount of time in microseconds for which all tasks in a cgroup can run during one period (as defined by cfs.period.us). | long |  |  |
| system.process.cgroup.cpu.cfs.shares | An integer value that specifies a relative share of CPU time available to the tasks in a cgroup. The value specified in the cpu.shares file must be 2 or higher. | long |  |  |
| system.process.cgroup.cpu.id | ID of the cgroup. | keyword |  |  |
| system.process.cgroup.cpu.path | Path to the cgroup relative to the cgroup subsystem's mountpoint. | keyword |  |  |
| system.process.cgroup.cpu.pressure.full.10.pct | Pressure over 10 seconds | float |  |  |
| system.process.cgroup.cpu.pressure.full.300.pct | Pressure over 300 seconds | float |  |  |
| system.process.cgroup.cpu.pressure.full.60.pct | Pressure over 60 seconds | float |  |  |
| system.process.cgroup.cpu.pressure.full.total | total Full pressure time | long |  |  |
| system.process.cgroup.cpu.pressure.some.10.pct | Pressure over 10 seconds | float |  |  |
| system.process.cgroup.cpu.pressure.some.300.pct | Pressure over 300 seconds | float |  |  |
| system.process.cgroup.cpu.pressure.some.60.pct | Pressure over 60 seconds | float |  |  |
| system.process.cgroup.cpu.pressure.some.total | total Some pressure time | long |  |  |
| system.process.cgroup.cpu.rt.period.us | Period of time in microseconds for how regularly a cgroup's access to CPU resources is reallocated. | long |  |  |
| system.process.cgroup.cpu.rt.runtime.us | Period of time in microseconds for the longest continuous period in which the tasks in a cgroup have access to CPU resources. | long |  |  |
| system.process.cgroup.cpu.stats.periods | Number of period intervals (as specified in cpu.cfs.period.us) that have elapsed. | long |  |  |
| system.process.cgroup.cpu.stats.system.norm.pct | cgroups v2 normalized system time | float |  |  |
| system.process.cgroup.cpu.stats.system.ns | cgroups v2 system time in nanoseconds | long |  |  |
| system.process.cgroup.cpu.stats.system.pct | cgroups v2 system time | float |  |  |
| system.process.cgroup.cpu.stats.throttled.ns | The total time duration (in nanoseconds) for which tasks in a cgroup have been throttled. | long |  |  |
| system.process.cgroup.cpu.stats.throttled.periods | Number of times tasks in a cgroup have been throttled (that is, not allowed to run because they have exhausted all of the available time as specified by their quota). | long |  |  |
| system.process.cgroup.cpu.stats.throttled.us | The total time duration (in microseconds) for which tasks in a cgroup have been throttled, as reported by cgroupsv2 | long |  |  |
| system.process.cgroup.cpu.stats.usage.norm.pct | cgroups v2 normalized usage | float |  |  |
| system.process.cgroup.cpu.stats.usage.ns | cgroups v2 usage in nanoseconds | long |  |  |
| system.process.cgroup.cpu.stats.usage.pct | cgroups v2 usage | float |  |  |
| system.process.cgroup.cpu.stats.user.norm.pct | cgroups v2 normalized cpu user time | float |  |  |
| system.process.cgroup.cpu.stats.user.ns | cgroups v2 cpu user time in nanoseconds | long |  |  |
| system.process.cgroup.cpu.stats.user.pct | cgroups v2 cpu user time | float |  |  |
| system.process.cgroup.cpuacct.id | ID of the cgroup. | keyword |  |  |
| system.process.cgroup.cpuacct.path | Path to the cgroup relative to the cgroup subsystem's mountpoint. | keyword |  |  |
| system.process.cgroup.cpuacct.percpu | CPU time (in nanoseconds) consumed on each CPU by all tasks in this cgroup. | object |  |  |
| system.process.cgroup.cpuacct.stats.system.norm.pct | Time the cgroup spent in kernel space, as a percentage of total CPU time, normalized by CPU count. | scaled_float |  |  |
| system.process.cgroup.cpuacct.stats.system.ns | CPU time consumed by tasks in user (kernel) mode. | long |  |  |
| system.process.cgroup.cpuacct.stats.system.pct | Time the cgroup spent in kernel space, as a percentage of total CPU time | scaled_float |  |  |
| system.process.cgroup.cpuacct.stats.user.norm.pct | time the cgroup spent in user space, as a percentage of total CPU time, normalized by CPU count. | scaled_float |  |  |
| system.process.cgroup.cpuacct.stats.user.ns | CPU time consumed by tasks in user mode. | long |  |  |
| system.process.cgroup.cpuacct.stats.user.pct | time the cgroup spent in user space, as a percentage of total CPU time | scaled_float |  |  |
| system.process.cgroup.cpuacct.total.norm.pct | CPU time of the cgroup as a percentage of overall CPU time, normalized by CPU count. This is functionally an average of time spent across individual CPUs. | scaled_float |  |  |
| system.process.cgroup.cpuacct.total.ns | Total CPU time in nanoseconds consumed by all tasks in the cgroup. | long |  |  |
| system.process.cgroup.cpuacct.total.pct | CPU time of the cgroup as a percentage of overall CPU time. | scaled_float |  |  |
| system.process.cgroup.id | The ID common to all cgroups associated with this task. If there isn't a common ID used by all cgroups this field will be absent. | keyword |  |  |
| system.process.cgroup.io.id | ID of the cgroup. | keyword |  |  |
| system.process.cgroup.io.path | Path to the cgroup relative to the cgroup subsystems mountpoint. | keyword |  |  |
| system.process.cgroup.io.pressure.full.10.pct | Pressure over 10 seconds | float |  |  |
| system.process.cgroup.io.pressure.full.300.pct | Pressure over 300 seconds | float |  |  |
| system.process.cgroup.io.pressure.full.60.pct | Pressure over 60 seconds | float |  |  |
| system.process.cgroup.io.pressure.full.total | total Some pressure time | long |  |  |
| system.process.cgroup.io.pressure.some.10.pct | Pressure over 10 seconds | float |  |  |
| system.process.cgroup.io.pressure.some.300.pct | Pressure over 300 seconds | float |  |  |
| system.process.cgroup.io.pressure.some.60.pct | Pressure over 60 seconds | float |  |  |
| system.process.cgroup.io.pressure.some.total | total Some pressure time | long |  |  |
| system.process.cgroup.io.stats.\* | per-device IO usage stats | object |  |  |
| system.process.cgroup.io.stats.\*.\* |  | object |  |  |
| system.process.cgroup.io.stats.\*.\*.bytes | per-device IO usage stats | object |  |  |
| system.process.cgroup.io.stats.\*.\*.ios | per-device IO usage stats | object |  |  |
| system.process.cgroup.memory.id | ID of the cgroup. | keyword |  |  |
| system.process.cgroup.memory.kmem.failures | The number of times that the memory limit (kmem.limit.bytes) was reached. | long |  |  |
| system.process.cgroup.memory.kmem.limit.bytes | The maximum amount of kernel memory that tasks in the cgroup are allowed to use. | long |  |  |
| system.process.cgroup.memory.kmem.usage.bytes | Total kernel memory usage by processes in the cgroup (in bytes). | long |  |  |
| system.process.cgroup.memory.kmem.usage.max.bytes | The maximum kernel memory used by processes in the cgroup (in bytes). | long |  |  |
| system.process.cgroup.memory.kmem_tcp.failures | The number of times that the memory limit (kmem_tcp.limit.bytes) was reached. | long |  |  |
| system.process.cgroup.memory.kmem_tcp.limit.bytes | The maximum amount of memory for TCP buffers that tasks in the cgroup are allowed to use. | long |  |  |
| system.process.cgroup.memory.kmem_tcp.usage.bytes | Total memory usage for TCP buffers in bytes. | long |  |  |
| system.process.cgroup.memory.kmem_tcp.usage.max.bytes | The maximum memory used for TCP buffers by processes in the cgroup (in bytes). | long |  |  |
| system.process.cgroup.memory.mem.events.fail | failed threshold | long |  |  |
| system.process.cgroup.memory.mem.events.high | high threshold | long |  |  |
| system.process.cgroup.memory.mem.events.low | low threshold | long |  |  |
| system.process.cgroup.memory.mem.events.max | max threshold | long |  |  |
| system.process.cgroup.memory.mem.events.oom | oom threshold | long |  |  |
| system.process.cgroup.memory.mem.events.oom_kill | oom killer threshold | long |  |  |
| system.process.cgroup.memory.mem.failures | The number of times that the memory limit (mem.limit.bytes) was reached. | long |  |  |
| system.process.cgroup.memory.mem.high.bytes | memory high threshhold | long |  |  |
| system.process.cgroup.memory.mem.limit.bytes | The maximum amount of user memory in bytes (including file cache) that tasks in the cgroup are allowed to use. | long |  |  |
| system.process.cgroup.memory.mem.low.bytes | memory low threshhold | long |  |  |
| system.process.cgroup.memory.mem.max.bytes | memory max threshhold | long |  |  |
| system.process.cgroup.memory.mem.usage.bytes | Total memory usage by processes in the cgroup (in bytes). | long |  |  |
| system.process.cgroup.memory.mem.usage.max.bytes | The maximum memory used by processes in the cgroup (in bytes). | long |  |  |
| system.process.cgroup.memory.memsw.events.fail | failed threshold | long |  |  |
| system.process.cgroup.memory.memsw.events.high | high threshold | long |  |  |
| system.process.cgroup.memory.memsw.events.low | low threshold | long |  |  |
| system.process.cgroup.memory.memsw.events.max | max threshold | long |  |  |
| system.process.cgroup.memory.memsw.events.oom | oom threshold | long |  |  |
| system.process.cgroup.memory.memsw.events.oom_kill | oom killer threshold | long |  |  |
| system.process.cgroup.memory.memsw.failures | The number of times that the memory plus swap space limit (memsw.limit.bytes) was reached. | long |  |  |
| system.process.cgroup.memory.memsw.high.bytes | memory high threshhold | long |  |  |
| system.process.cgroup.memory.memsw.limit.bytes | The maximum amount for the sum of memory and swap usage that tasks in the cgroup are allowed to use. | long |  |  |
| system.process.cgroup.memory.memsw.low.bytes | memory low threshhold | long |  |  |
| system.process.cgroup.memory.memsw.max.bytes | memory max threshhold | long |  |  |
| system.process.cgroup.memory.memsw.usage.bytes | The sum of current memory usage plus swap space used by processes in the cgroup (in bytes). | long |  |  |
| system.process.cgroup.memory.memsw.usage.max.bytes | The maximum amount of memory and swap space used by processes in the cgroup (in bytes). | long |  |  |
| system.process.cgroup.memory.path | Path to the cgroup relative to the cgroup subsystem's mountpoint. | keyword |  |  |
| system.process.cgroup.memory.stats.\* | detailed memory IO stats | object |  |  |
| system.process.cgroup.memory.stats.\*.bytes | detailed memory IO stats | object |  |  |
| system.process.cgroup.memory.stats.active_anon.bytes | Anonymous and swap cache on active least-recently-used (LRU) list, including tmpfs (shmem), in bytes. | long |  |  |
| system.process.cgroup.memory.stats.active_file.bytes | File-backed memory on active LRU list, in bytes. | long |  |  |
| system.process.cgroup.memory.stats.cache.bytes | Page cache, including tmpfs (shmem), in bytes. | long |  |  |
| system.process.cgroup.memory.stats.hierarchical_memory_limit.bytes | Memory limit for the hierarchy that contains the memory cgroup, in bytes. | long |  |  |
| system.process.cgroup.memory.stats.hierarchical_memsw_limit.bytes | Memory plus swap limit for the hierarchy that contains the memory cgroup, in bytes. | long |  |  |
| system.process.cgroup.memory.stats.inactive_anon.bytes | Anonymous and swap cache on inactive LRU list, including tmpfs (shmem), in bytes | long |  |  |
| system.process.cgroup.memory.stats.inactive_file.bytes | File-backed memory on inactive LRU list, in bytes. | long |  |  |
| system.process.cgroup.memory.stats.major_page_faults | Number of times that a process in the cgroup triggered a major fault. "Major" faults happen when the kernel actually has to read the data from disk. | long |  |  |
| system.process.cgroup.memory.stats.mapped_file.bytes | Size of memory-mapped mapped files, including tmpfs (shmem), in bytes. | long |  |  |
| system.process.cgroup.memory.stats.page_faults | Number of times that a process in the cgroup triggered a page fault. | long |  |  |
| system.process.cgroup.memory.stats.pages_in | Number of pages paged into memory. This is a counter. | long |  |  |
| system.process.cgroup.memory.stats.pages_out | Number of pages paged out of memory. This is a counter. | long |  |  |
| system.process.cgroup.memory.stats.rss.bytes | Anonymous and swap cache (includes transparent hugepages), not including tmpfs (shmem), in bytes. | long |  |  |
| system.process.cgroup.memory.stats.rss_huge.bytes | Number of bytes of anonymous transparent hugepages. | long |  |  |
| system.process.cgroup.memory.stats.swap.bytes | Swap usage, in bytes. | long |  |  |
| system.process.cgroup.memory.stats.unevictable.bytes | Memory that cannot be reclaimed, in bytes. | long |  |  |
| system.process.cgroup.path | The path to the cgroup relative to the cgroup subsystem's mountpoint. If there isn't a common path used by all cgroups this field will be absent. | keyword |  |  |
| system.process.cmdline | The full command-line used to start the process, including the arguments separated by space. | keyword |  |  |
| system.process.cpu.start_time | The time when the process was started. | date |  |  |
| system.process.cpu.system.ticks | The amount of CPU time the process spent in kernel space. | long |  | counter |
| system.process.cpu.total.norm.pct | The percentage of CPU time spent by the process since the last event. This value is normalized by the number of CPU cores and it ranges from 0 to 100%. | scaled_float | percent | gauge |
| system.process.cpu.total.pct | The percentage of CPU time spent by the process since the last update. Its value is similar to the %CPU value of the process displayed by the top command on Unix systems. | scaled_float | percent | gauge |
| system.process.cpu.total.ticks | The total CPU time spent by the process. | long |  | counter |
| system.process.cpu.total.value | The value of CPU usage since starting the process. | long |  | counter |
| system.process.cpu.user.ticks | The amount of CPU time the process spent in user space. | long |  | counter |
| system.process.env | The environment variables used to start the process. The data is available on FreeBSD, Linux, and OS X. | object |  |  |
| system.process.fd.limit.hard | The hard limit on the number of file descriptors opened by the process. The hard limit can only be raised by root. | long |  | gauge |
| system.process.fd.limit.soft | The soft limit on the number of file descriptors opened by the process. The soft limit can be changed by the process at any time. | long |  | gauge |
| system.process.fd.open | The number of file descriptors open by the process. | long |  | gauge |
| system.process.memory.rss.bytes | The Resident Set Size. The amount of memory the process occupied in main memory (RAM). On Windows this represents the current working set size, in bytes. | long | byte | gauge |
| system.process.memory.rss.pct | The percentage of memory the process occupied in main memory (RAM). | scaled_float | percent | gauge |
| system.process.memory.share | The shared memory the process uses. | long | byte | gauge |
| system.process.memory.size | The total virtual memory the process has. On Windows this represents the Commit Charge (the total amount of memory that the memory manager has committed for a running process) value in bytes for this process. | long | byte | gauge |
| system.process.state | The process state. For example: "running". | keyword |  |  |
| user | The user fields describe information about the user that is relevant to the event. Fields can have one entry or multiple entries. If a user has more than one id, provide an array that includes all of them. | group |  |  |
| user.name | Short name or login of the user. | keyword |  |  |
| user.name.text | Multi-field of `user.name`. | match_only_text |  |  |


### Process summary

The `process_summary` data stream collects high level statistics about the running
processes.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- Windows

#### Permissions

General process summary data should be available without elevated permissions.
If the process data belongs to the other users, it will be counted as unknown value.

**Exported fields**

| Field | Description | Type | Metric Type |
|---|---|---|---|
| @timestamp | Event timestamp. | date |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |
| cloud.region | Region in which this host is running. | keyword |  |
| container.id | Unique container id. | keyword |  |
| container.image.name | Name of the image the container was built on. | keyword |  |
| container.labels | Image labels. | object |  |
| container.name | Container name. | keyword |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |
| data_stream.type | Data stream type. | constant_keyword |  |
| event.dataset | Event dataset. | constant_keyword |  |
| event.module | Event module | constant_keyword |  |
| group | The group fields are meant to represent groups that are relevant to the event. | group |  |
| group.id | Unique identifier for the group on the system/platform. | keyword |  |
| group.name | Name of the group. | keyword |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |
| host.architecture | Operating system architecture. | keyword |  |
| host.containerized | If the host is a container. | boolean |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |
| host.ip | Host ip addresses. | ip |  |
| host.mac | Host mac addresses. | keyword |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |
| host.os.build | OS build information. | keyword |  |
| host.os.codename | OS codename, if any. | keyword |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |
| host.os.name | Operating system name, without the version. | keyword |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |
| host.os.version | Operating system version as a raw string. | keyword |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |  |
| process | These fields contain information about a process. These fields can help you correlate metrics information with a process id/name from a log message.  The `process.pid` often stays in the metric itself and is copied to the global field for correlation. | group |  |
| process.name | Process name. Sometimes called program name or similar. | keyword |  |
| process.name.text | Multi-field of `process.name`. | match_only_text |  |
| process.pid | Process id. | long |  |
| source | Source fields capture details about the sender of a network exchange/packet. These fields are populated from a network event, packet, or other event containing details of a network transaction. Source fields are usually populated in conjunction with destination fields. The source and destination fields are considered the baseline and should always be filled if an event contains source and destination details from a network transaction. If the event also contains identification of the client and server roles, then the client and server fields should also be populated. | group |  |
| source.geo.city_name | City name. | keyword |  |
| source.geo.continent_name | Name of the continent. | keyword |  |
| source.geo.country_iso_code | Country ISO code. | keyword |  |
| source.geo.location | Longitude and latitude. | geo_point |  |
| source.geo.region_iso_code | Region ISO code. | keyword |  |
| source.geo.region_name | Region name. | keyword |  |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |  |
| source.port | Port of the source. | long |  |
| system.process.summary.dead | Number of dead processes on this host. It's very unlikely that it will appear but in some special situations it may happen. | long | gauge |
| system.process.summary.idle | Number of idle processes on this host. | long | gauge |
| system.process.summary.running | Number of running processes on this host. | long | gauge |
| system.process.summary.sleeping | Number of sleeping processes on this host. | long | gauge |
| system.process.summary.stopped | Number of stopped processes on this host. | long | gauge |
| system.process.summary.total | Total number of processes on this host. | long | gauge |
| system.process.summary.unknown | Number of processes for which the state couldn't be retrieved or is unknown. | long | gauge |
| system.process.summary.zombie | Number of zombie processes on this host. | long | gauge |
| user | The user fields describe information about the user that is relevant to the event. Fields can have one entry or multiple entries. If a user has more than one id, provide an array that includes all of them. | group |  |
| user.id | Unique identifier of the user. | keyword |  |
| user.name | Short name or login of the user. | keyword |  |
| user.name.text | Multi-field of `user.name`. | match_only_text |  |


### Socket summary

The System `socket_summary` data stream provides the summary of open network
sockets in the host system.

It collects a summary of metrics with the count of existing TCP and UDP
connections and the count of listening ports.

#### Supported operating systems

- FreeBSD
- Linux
- macOS
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| group | The group fields are meant to represent groups that are relevant to the event. | group |  |  |
| group.id | Unique identifier for the group on the system/platform. | keyword |  |  |
| group.name | Name of the group. | keyword |  |  |
| host | A host is defined as a general computing instance. ECS host.\* fields should be populated with details about the host on which the event happened, or from which the measurement was taken. Host types include hardware, virtual machines, Docker containers, and Kubernetes nodes. | group |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| message | For log events the message field contains the log message, optimized for viewing in a log viewer. For structured logs without an original message field, other fields can be concatenated to form a human-readable summary of the event. If multiple messages exist, they can be combined into one message. | match_only_text |  |  |
| process | These fields contain information about a process. These fields can help you correlate metrics information with a process id/name from a log message.  The `process.pid` often stays in the metric itself and is copied to the global field for correlation. | group |  |  |
| process.name | Process name. Sometimes called program name or similar. | keyword |  |  |
| process.name.text | Multi-field of `process.name`. | match_only_text |  |  |
| process.pid | Process id. | long |  |  |
| source | Source fields capture details about the sender of a network exchange/packet. These fields are populated from a network event, packet, or other event containing details of a network transaction. Source fields are usually populated in conjunction with destination fields. The source and destination fields are considered the baseline and should always be filled if an event contains source and destination details from a network transaction. If the event also contains identification of the client and server roles, then the client and server fields should also be populated. | group |  |  |
| source.geo.city_name | City name. | keyword |  |  |
| source.geo.continent_name | Name of the continent. | keyword |  |  |
| source.geo.country_iso_code | Country ISO code. | keyword |  |  |
| source.geo.location | Longitude and latitude. | geo_point |  |  |
| source.geo.region_iso_code | Region ISO code. | keyword |  |  |
| source.geo.region_name | Region name. | keyword |  |  |
| source.ip | IP address of the source (IPv4 or IPv6). | ip |  |  |
| source.port | Port of the source. | long |  |  |
| system.socket.summary.all.count | All open connections | integer |  | gauge |
| system.socket.summary.all.listening | All listening ports | integer |  | gauge |
| system.socket.summary.tcp.all.close_wait | Number of TCP connections in _close_wait_ state | integer |  | gauge |
| system.socket.summary.tcp.all.closing | Number of TCP connections in _closing_ state | integer |  | gauge |
| system.socket.summary.tcp.all.count | All open TCP connections | integer |  | gauge |
| system.socket.summary.tcp.all.established | Number of established TCP connections | integer |  | gauge |
| system.socket.summary.tcp.all.fin_wait1 | Number of TCP connections in _fin_wait1_ state | integer |  | gauge |
| system.socket.summary.tcp.all.fin_wait2 | Number of TCP connections in _fin_wait2_ state | integer |  | gauge |
| system.socket.summary.tcp.all.last_ack | Number of TCP connections in _last_ack_ state | integer |  | gauge |
| system.socket.summary.tcp.all.listening | All TCP listening ports | integer |  | gauge |
| system.socket.summary.tcp.all.orphan | A count of all orphaned tcp sockets. Only available on Linux. | integer |  | gauge |
| system.socket.summary.tcp.all.syn_recv | Number of TCP connections in _syn_recv_ state | integer |  | gauge |
| system.socket.summary.tcp.all.syn_sent | Number of TCP connections in _syn_sent_ state | integer |  | gauge |
| system.socket.summary.tcp.all.time_wait | Number of TCP connections in _time_wait_ state | integer |  | gauge |
| system.socket.summary.tcp.memory | Memory used by TCP sockets in bytes, based on number of allocated pages and system page size. Corresponds to limits set in /proc/sys/net/ipv4/tcp_mem. Only available on Linux. | integer | byte | gauge |
| system.socket.summary.udp.all.count | All open UDP connections | integer |  | gauge |
| system.socket.summary.udp.memory | Memory used by UDP sockets in bytes, based on number of allocated pages and system page size. Corresponds to limits set in /proc/sys/net/ipv4/udp_mem. Only available on Linux. | integer | byte | gauge |
| user | The user fields describe information about the user that is relevant to the event. Fields can have one entry or multiple entries. If a user has more than one id, provide an array that includes all of them. | group |  |  |
| user.id | Unique identifier of the user. | keyword |  |  |
| user.name | Short name or login of the user. | keyword |  |  |
| user.name.text | Multi-field of `user.name`. | match_only_text |  |  |


### Uptime

The System `uptime` data stream provides the uptime of the host operating system.

#### Supported operating systems

- Linux
- macOS
- OpenBSD
- FreeBSD
- Windows

#### Permissions

This data should be available without elevated permissions.

**Exported fields**

| Field | Description | Type | Unit | Metric Type |
|---|---|---|---|---|
| @timestamp | Event timestamp. | date |  |  |
| cloud.account.id | The cloud account or organization id used to identify different entities in a multi-tenant environment. Examples: AWS account id, Google Cloud ORG Id, or other unique identifier. | keyword |  |  |
| cloud.availability_zone | Availability zone in which this host is running. | keyword |  |  |
| cloud.image.id | Image ID for the cloud instance. | keyword |  |  |
| cloud.instance.id | Instance ID of the host machine. | keyword |  |  |
| cloud.instance.name | Instance name of the host machine. | keyword |  |  |
| cloud.machine.type | Machine type of the host machine. | keyword |  |  |
| cloud.project.id | Name of the project in Google Cloud. | keyword |  |  |
| cloud.provider | Name of the cloud provider. Example values are aws, azure, gcp, or digitalocean. | keyword |  |  |
| cloud.region | Region in which this host is running. | keyword |  |  |
| container.id | Unique container id. | keyword |  |  |
| container.image.name | Name of the image the container was built on. | keyword |  |  |
| container.labels | Image labels. | object |  |  |
| container.name | Container name. | keyword |  |  |
| data_stream.dataset | Data stream dataset. | constant_keyword |  |  |
| data_stream.namespace | Data stream namespace. | constant_keyword |  |  |
| data_stream.type | Data stream type. | constant_keyword |  |  |
| event.dataset | Event dataset. | constant_keyword |  |  |
| event.module | Event module | constant_keyword |  |  |
| host.architecture | Operating system architecture. | keyword |  |  |
| host.containerized | If the host is a container. | boolean |  |  |
| host.domain | Name of the domain of which the host is a member. For example, on Windows this could be the host's Active Directory domain or NetBIOS domain name. For Linux this could be the domain of the host's LDAP provider. | keyword |  |  |
| host.hostname | Hostname of the host. It normally contains what the `hostname` command returns on the host machine. | keyword |  |  |
| host.id | Unique host id. As hostname is not always unique, use values that are meaningful in your environment. Example: The current usage of `beat.name`. | keyword |  |  |
| host.ip | Host ip addresses. | ip |  |  |
| host.mac | Host mac addresses. | keyword |  |  |
| host.name | Name of the host. It can contain what `hostname` returns on Unix systems, the fully qualified domain name, or a name specified by the user. The sender decides which value to use. | keyword |  |  |
| host.os.build | OS build information. | keyword |  |  |
| host.os.codename | OS codename, if any. | keyword |  |  |
| host.os.family | OS family (such as redhat, debian, freebsd, windows). | keyword |  |  |
| host.os.kernel | Operating system kernel version as a raw string. | keyword |  |  |
| host.os.name | Operating system name, without the version. | keyword |  |  |
| host.os.name.text | Multi-field of `host.os.name`. | text |  |  |
| host.os.platform | Operating system platform (such centos, ubuntu, windows). | keyword |  |  |
| host.os.version | Operating system version as a raw string. | keyword |  |  |
| host.type | Type of host. For Cloud providers this can be the machine type like `t2.medium`. If vm, this could be the container, for example, or other information meaningful in your environment. | keyword |  |  |
| system.uptime.duration.ms | The OS uptime in milliseconds. | long | ms | counter |

