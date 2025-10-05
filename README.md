# prowlerlogx

prowlerlogx is a group of configuration files for rsyslog, to build and configurate a server to receive the logs from the clients. The configuration files split in server, client, rules and tls for better management and deployment.

# Centralized log collection

Having a log server to collect logs for a network, making useable and in a productivity and efficient way is a must. Even more relevant for security.
Logs play a critical role in the functioning and management of any system or application. They provide valuable insights into the state and activity of the system and are essential for debugging and troubleshooting issues, meeting compliance requirements, and making informed decisions.
Linux has basic and standard yet powerful rsyslog that can be integrated and used by several tools for log analysis and filtering to simplify management and troubleshooting.

## Is important?

**YES!** It is a **MUST!**

Making the data log collection and with the right tool:

- Improve security and compliance by more easily monitoring and auditing the logs to meet regulatory requirements.
- Enhance visibility and debugging capabilities by allowing for easier search and analysis of the logs from multiple sources. Since it can be tough to aggregate all the logs from multiple sources, it is easy to have all the logs in one location.
- Help organizations identify trends and patterns, troubleshoot issues, meet compliance requirements, and improve the performance and reliability of their systems.
- Protect the privacy and security of log data. It should be secure and easy to monitor.

# This

Rsyslog as a log collector can save data in files or database depending on rsyslog server configuration. On the top, we must have a analytical, user friendly visualization, processing and audit tool. In the market there are a lot of solutions and some integrate with rsyslog or including the use of RELP protocol.

In this project, it is not pretended to replace any product, but only a simple way to implement a log collection using rsyslog. then can be integrated with tools like, ELK Stack, logstash, signoz, solarwinds, dynatrace, etc …

# Files

Using standard method in Linux installations, the path "/etc/rsyslog.d" is used to keep the configuration files (.conf). This is the preferred method avoiding to change the base configuration file "/etc/rsyslog.conf" that comes with the package. Depending on the system especific, if there is a change to be made in "/etc/rsyslog.conf", that should be minimal.

The configuration files are divided in server and client configuration files. They were build in this way, to agile deployment in collecting server and client machines, being enough to rename the extension for those not being used (for client machines rename extension of server files, for server rename extension of client files).

- 56-local.conf
    - Common configuration file both for server and client.
- 60-server.conf
    - Base server configuration file. If TLS configuration is used, the lines for tcp load module should be commented (Now they are commented).
- 61-server-tls.conf
    - Server file dedicated to TLS configuration. The tcp module is loaded using the standard port for rsyslog with TLS (6514).
- 64-server-rules.conf
    - Rules configurated for server.
- 68-server-client-rules.conf
    -  Rules configurated for server acting as a client. Server also can act as a client, as it laso send log data.
- 70-client.conf
    - Basic client configuration. Now is empty.
- 71-client-tls.conf
    - TLS configuration for client.
- 74-client-rules.conf
    - Rules applied to client.

So, in the server collector machine, it is enough to rename 7*.conf files, and in a client machine rename 6*.conf files.

# Guidelines

- The driver / modules used for TLS is ossl (openssl). gtls (GnuTls) is another module can be used. The parameters for chipers and encryption for each modules are indicated and commented.
- The deprecated chipers and encryptions algorithm are removed.
- In "60-server.conf" file are included several templates and they were necessary to strip chars of mal-formed **_app-name_***. See the conditional statement below in that file.
- "64-server-rules.conf" include statment to ignore somes messages for some applications, because they pollutes the logs. This is an example to configurate messages processing.