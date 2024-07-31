# Corelight Pack

----

The Corelight Pack utilizes a range of advanced techniques and functions to effectively receive and manage data from Corelight Sensors. Designed to streamline and optimize data processing, this Pack offers flexible capabilities for enhancing, routing, and reducing Corelight data.

Key benefits of the Corelight Pack include:

- **Data Integration Excellence**: Seamlessly ingest data from various formats exported by Corelight Sensors, simplifying the integration process.

- **Robust Data Normalization**: Ensure consistent parsing and enrichment of data across multiple Corelight log-types for accurate and precise analysis.

- **In-Pack Enrichment**: Enhance select log-types with local artifacts (lookups) and correlate all ingested log-types with Corelight events (UID), streamlining investigations on platforms like Splunk or Elastic Search.

- **Real-time Threat Detection Tagging**: Detect and tag specific security vulnerabilities in real-time, enhancing threat detection and response capabilities.
Corelight sources actively perform threat detection, and the Cribl Corelight Pack enriches the data while tagging each event with results, primarily derived from Corelight itself.

- **Comprehensive Standards Compliance**: Comply with industry-standard data formats, including Common Information Module, Elastic Common Schema, Chronicle Unified Data Model, and MS Sentinel, irrespective of the pack's core functions.

- **Data Transformation Flexibility**: Convert data into at least four different formats, adapting to your specific requirements.

- **Optimized Data Processing**: Minimize the GDI (getting data in) time for all log-types when integrated with destination systems, improving operational efficiency.

- **Tailored for All Environments**: The Corelight Pack provides a scalable foundation for data processing in any environment, offering a blueprint for common use cases and customizable features. Easily adapt and expand to meet your unique needs.

- **Enhanced SOC Operations**: Enrich select data fields with local artifacts (lookups), simplifying analysis for SOC analysts.

The Corelight Pack provides a scalable foundation for data processing in any environment, with a blueprint of common use cases and versatile capabilities that can be easily adapted or expanded to meet specific needs. This Pack is designed to enhance your data management and security operations with a wide range of use cases and customizable features, making it an ideal solution for any size environment.

## Version 2.0.0 (Release Date: 2023-08-29)

Welcome to the Corelight Pack V.2.0.0, an essential component for your SOC analysts, cybersecurity leaders, and architects. This release, V.2.0.0, builds on your valuable feedback and enhances data processing capabilities, elevating your security operations.
## Requirements Section

The Corelight Pack currently supports Syslog, TSV, Elasticsearch, and JSON formats out of the box. The pipelines process for enrichment, shaping, and reducing data are available for the following Corelight's datasets/log-types:

- conn.log
- dns.log
- ssl.log
- http.log
- SSH.log
- files.log
- RDP.log
- SMB_files.log
- SMB_mapping.log
- weird.log
- NTP.log
- x509.log
- dc_rpc.log
- notice.log
- intel.log
- suricata_corelight_logs.log

The Optional UID Correlation across all log types group is turnned of by default.
To utilize the optional Redis correlation functions and content, an instance of Redis is required. Redis is a BSD-licensed, open-source in-memory data structure store that is used as a database, cache, message broker, and streaming engine. 
More information and detailed instalation instructions on Redis can be found at <https://redis.io/>.

Configure the Redis address (with your Redis instance IP address and port)
Go to Knowledge / Global Variables and edit the variable redis_server.
All pipelines have a Redis function in the group "UID Correlation across all log types".9`
After you configure the Global Variable with the Redis IP address turn the UID Correlation across all log type groups back on, and commit & deploy. 



## Pack artifacts:
The Corelight Pack requires and utilizes several lookup files that are included in the Pack, but may need to be updated to fit specific business cases. These lookup files can be found in the Pack's Knowledge > Lookups section, and include:

* **common_sites_actions.csv** - Lookup table to determine treatment of common sites (Suppress, Sample):
* **corelight_conn_state.csv** - List of summarized state for each connection in Corelight.
* **corelight_conn_state_description.csv** - List of common conn state desctipions and actions.
* **history_conn.csv** - List of all Phases, Sides and description in conn logs.
* **ja3_fingerprints_H.csv** - Publically available TLS/SSL list of known malicious fingerprints.
* **top-1k.csv** -  This currently uses a subset of the top 1M domains from Cisco Umbrella.  Given this changes frequently, this needs to be updated regularly.
* **CL_Comm_OCSF-ChainedCL_Conn.csv** - Experimental Normalization for OCSF 
* **CL_Conn_state_Chronicle.csv** - Mapping for Chronical UDM relationships
* **CL_Intel_Chronicle.csv** - Mapping for Chronical UDM relationships
* **CL_SMB_Files_Actions_Chronicle.csv** - Mapping for Chronical UDM relationships
* **CL_SSH_Cipher_algs.csv** - SSH Cipher list for enrichment functions
* **CL_SSH_Interface_Chronicle.csv** - Mapping for Chronical UDM relationships
* **CL_SSL_Ciphers.csv** - SSL Cipher list for enrichment functions
* **CL_SSL_history.csv** - Lookup for SSL history enrichment
* **Sentinel_cols_mapping.csv** - Mapping for MS Sentinel mappings

## Pack Structure

For each Corelight log type, a standard has been created to highlight the core Cribl Stream functions while preserving the event flow and only acting on final formatting options. This ensures data fidelity.

``` Groups:
├─ Parse & Trim
│  ├─ Functions
│  ├─ ...
│  └─ ...
├─ Enrichment
│  ├─ Functions
│  ├─ ...
│  └─ ...
├─ Reduction
│  ├─ Functions
│  ├─ ...
│  └─ ...
├─ UID Correlation
│  ├─ Functions
│  ├─ ...
│  └─ ...
├─ CIM or ECS compliance
│  ├─ Functions
│  ├─ ...
│  └─ ...
└─ Cleanup Functions
   ├─ Functions
   ├─ ...
   └─ ...
```

All functions in a Pipeline are optional (on/off toggle) however some a crucial for the Pack to work properlly.

**Optionals:**
- Drop functions in the Reduction Group
- UID Correlation
- CIM, ECS, Chronicle UDM or MS Sentinel
- Cleanup Functions

##### Main Routes:
"Routes within a Pack" refer to predefined data processing pathways specifically designed and included as part of a packaged solution, such as the Corelight Pack. These routes are crafted to handle distinct data types or sources, such as Corelight logs, in a highly efficient and structured manner.

On the other hand, "Routes (general)" outside of the pack pack have the capability to send data directly to specific final destinations, whether that be other individual pipelines within Cribl Stream or full packs and forward the processed data to external systems. In contrast, routes within a pack primarily route data to specific pipelines and components contained within that pack, enhancing the pack's functionality.

##### Available Pipelines:

* **CL_conn**
* **CL_DNS**
* **CL_SSL**
* **CL_HTTP**
* **CL_SSH**
* **CL_Files**
* **CL_RDP**
* **CL_SMB_Files**
* **CL_SMN_Mapping**
* **CL_Weird**
* **CL_NTP**
* **CL_X509**
* **CL_Notice**
* **CL_Intel**
* **CL_Suricata_Corelight_Logs**
* **CL_Catch_all**

Chainned Pipelines for CIM & ECS compliance are separate from the main pipelines and are optional. This makes it easier to update their functions and maintain the pack. Each pipeline maps field names and calculates specific field values for both Splunk (CIM) and Elasticsearch (ECS). The following pipelines are included:

* **CIM_Web-Chained**
* **CIM_Network_Traffic-Chained**
* **CIM_Network_Sessions-Chained**
* **CIM-Network_Resolution-Chained**
* **CIM_Certificates-Chained**
* **ECS_Corelight-Chained**
* **CL_Chronicle-Chainned**

## Using The Pack

To use the Corelight Pack, follow these steps:

### Installation and configuration

Install the pack from your Cribl Stream instance by navigating to Processing > Packs > Add Pack > Add from Dispensary or by downloading the pack file from the Cribl Pack Dispensary at <https://packs.cribl.io/>.

After installation and a full commit and deploy (if in distributed mode) is necessary

**Validate the values to be used by the pack:**

In Knowledge > Global Variables, adjust the 'index' field value to standardize the index naming for Splunk or Elasticsearch as defined in your environment. 
>Note: the index variable is used as a base (single) name for an index, withing the peack each pipeline has a function that will add an extension to this variable sending data to one index per dataset/log-type ingested from Corelight. That practice would be a decision to be made wheater or not have a single index for all Corelight datasets/log-types or separate them for faster processing in your system of analysis.

**Advantages of Separating Data into Multiple Indexes:**

- Data Isolation: Each index acts as a data container, which provides isolation. This can be valuable for security, access control, and data retention policies. You can apply different access controls and retention settings to different indexes.

- Data Retention: Different data sources might have different data retention policies. Some data might need to be retained for a longer period than others. By separating data into indexes, you can configure retention policies on a per-index basis, ensuring compliance with your organization's data retention requirements.

- Efficient Searches: In some cases, when you're searching for specific types of data, having them in separate indexes can make searches more efficient. For example, if you often search for specific sourcetypes or data sources, you can limit the scope of your searches to a particular index, potentially speeding up search performance.

- Schema Flexibility: Different data sources might have different schemas or field extractions. By separating them, you can apply unique configurations and schema settings to each index.

In essence, using multiple indexes allows you to tailor configurations, access controls, and retention policies to the specific requirements of each data source, making overall data source management more manageable and efficient in Splunk.

## General Recomendations:

Ensure that Drop functions are acceptbale by your Security operaions.  

If the UID correlation has been elected as use case make sure to have your Redis address ready, The pack comes with the funcion disabled and with the Redis value of `redis://your_redis_ip_address>:6379`, change these values accorandly. 

In regards to the Reduction group and its functions and Before Making Any Decisions:
Review Each Field's Purpose: Be sure to understand the context and relevance of each field, especially if your focus is on security or network monitoring.

Consult with Stakeholders: Confirm with relevant departments or team members that you're not removing any fields that could be important for their work.

Test in a Non-Production Environment: Always implement and test these changes in a controlled environment first to ensure that the removals do not impact your organizational goals.

Again, the decision to remove specific fields should be based on your particular needs and operational context. Make sure to confirm these recommendations with your specific use-case in mind.

### Notable Functions

- UID Corrlation with Redis (requires Redis)

- Conn History enrichment
Will match the values of each phase in the 'history' field to a descriptive value in the 'history_conn.csv' file in the Knowledge session of the pack, then it will liste in order these values in a new field 'history_description' as an object. 
>Note: the field 'history_description' is ignored by default in the Serialize function winthin the Cleanup group. 

- SSL Cipher Strength Monitoring and Storage (requires Redis)

This group of functions processes incoming logs from Corelight sensors and focuses on SSL/TLS data. It performs several key tasks:
1. Cipher Classification: Categorizes the strength of each SSL cipher as 'Strong', 'Moderate', or 'Weak' based on pre-defined look-up tables.
2. Storing to Redis: Saves important SSL metadata like cipher strengths and cipher algorithms to a Redis store. Each entry is indexed by the IP address.
3. Annotation: Annotates the original log with the cipher strength classification for better readability and analysis.

>Note: The data stored in Redis will be used for comparison in the SSH pipeline. This enables cross-protocol cipher strength analysis.
 
- SSH / SSL Verification (requires Redis)
The Redis component is crucial for this correlation and needs to be configured in the environment.

- DNS Data Exfiltration Detection 
This is a set of functions designed to scrutinize Corelight DNS logs for signs of potential data exfiltration through Base64-encoded subdomains. These functions actively monitor DNS queries and intelligently flag any queries that exhibit behaviors consistent with data exfiltration attempts. By identifying suspicious DNS activity, this detection mechanism enhances security monitoring and enables timely response to potential threats.

- SSL Ja3 enrichment
Matches a public list of bad SSL fingerprints, will create a new field with a 'true' value in the event matched the provided list in the lookup table named ' ja3_fingerprints_H.csv'.

- HTTP enrichment
Will check for domains from host IPs and correlates actions to suppress, sample, or flag from defined common_sites_actions lookup.

- DNS Tunneling
Will check if proto is equal to 'udp', validate the count of characters in the query is less than 256 and if the query does not contain '.' (dots), if it matches will create a field named 'istunneling' with the value of 'true'.

- DNS 1kdomains enrichment
Will break all domains and subdomains in a query and perform a lookup agains the top-1k.csv in the Knowledge session of the pack, if the domain in the query matches any in the list the function will capture the rank for the given domain and/or subdomains in the list.
>Note: in a subsequent optional function (Drop) events matching a value grater or lower then can be dropped. 


## Release Notes

### Version 2.0.0 - 2023-09-19

Added new Corelight Log Types and their repective pipelines:
* SSH Logs  - CL_SSH
* Files Logs - CL_files
* RDP Logs - CL_RDP
* SMB_files Logs - CL_SMB_files
* SMN_mapping Logs - CL_SMB_mapping
* Weird Logs - CL_weird
* NTP Logs- CL_NTP
* x509 Logs - CL_x509
* Notice Logs - CL_Notice
* Suricata_Corelight_Logs - CL_Suricata_corelight

Corrected the Chained pipele nemes to match the Pack's naming convention and added description to each chained pipeline.

* CIM_Web-Chained
* CIM_Network_Traffic-Chained
* CIM_Network_Sessions-Chained
* CIM-Network_Resolution-Chained
* CIM_Certificates-Chained
* CIM_Endpoint-Chained
* ECS_Corelight-Chained
* CL_Chronicle-Chainned

There is also an Experimental pipeline for the AWS Security Lake compliance (OCSF)
* CL_Conn_OCSF-Chained

##### Additions and Adjustments
- In the CL_HTTP pipeline / Aggregations Function the resulting aggregation now is sent to a separate index named in Knowledge / Global Variables / 'metrics_index' and called from the function via C.vars.metrics_index.
<br><b>Note:</b> A metric index needs to be created in the system of analyis</br>

- Added the compliance option capable of exporting to MS Sentinel Mappings covering Sentinel Name, Asim, and Suffixed name formats.

- Added the compliance option capable of exporting to the Chronicle Unified Data Model (UDM).

- Several Pipelines for Splunk CIM have been aligned with destination Data Models and corrected accordingly.

- Updates have been made to the Experimental OCSF conversion for Conn logs only.

- Improved parsing capabilities across all pipelines, now identifying the "Agent" and/or "HEC" data format from each ingested dataset/log-type.

- Creation of parsers to be used by the TSV log-type, aiding in the extraction of common Zeek/Corelight datasets/log-types.

- Introduced a Catch All Route and Pipeline (**CL_Catch_All**) with minimal functionality, focusing on parsing and normalizing data to the destination Data Models supported by the Pack.

- Cleanup of unused pipelines and lookup tables.


### Version 1.0.2 - 2023-06-21
Network Sessions Pipeline for Splunk CIM corrected.

Experimental OCSF conversion for Conn logs

Cleanup of unused pipelines and lookup tables.

### Version 1.0.1 - 2023-05-15
Corrected the Global Variables in the pack to properly use the redis_server variable.

### Version 1.0.0 - 2023-05-07
Completed RC1 for the Pack!

## Contributing to the Pack

To contribute to the Pack, please connect with Claudio Cruz on [Cribl Community Slack](https://cribl-community.slack.com/). You can suggest new features or offer to collaborate.

## Contact

To contact us please email <ccruz@cribl.io>.

## License

This Pack uses the following license: [`Apache 2.0`](https://github.com/criblio/appscope/blob/master/LICENSE).
