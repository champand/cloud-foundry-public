---

copyright:
  years: 2015, 2020
lastupdated: "2020-09-16"

keywords: cloud foundry

subcollection: cloud-foundry-public



---


{:beta: .beta}
{:codeblock: .codeblock}
{:deprecated: .deprecated}
{:download: .download}
{:external: target="_blank" .external}
{:faq: data-hd-content-type='faq'}
{:gif: data-image-type='gif'}
{:help: data-hd-content-type='help'}
{:important: .important}
{:java: data-hd-programlang="java"}
{:javascript: data-hd-programlang="javascript"}
{:new_window: target="_blank"}
{:note: .note}
{:pre: .pre}
{:preview: .preview}
{:screen: .screen}
{:shortdesc: .shortdesc}
{:support: data-reuse='support'}
{:table: .aria-labeledby="caption"}
{:tip: .tip}
{:troubleshoot: data-hd-content-type='troubleshoot'}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}
{:tsSymptoms: .tsSymptoms}

# Use JConsole to monitor Liberty in {{site.data.keyword.cloud_notm}}
{: #jconsole}

## Monitor the Liberty runtime with JConsole:
{: #steps_to_monitor}

1. Push your app within a server package containing an appropriate `server.xml` file.
2. Start the JConsole app with the appropriate system properties on the command line.
3. Provide the proper Remote Process URL, Username, and Password to JConsole.

### Push the server package
{: #push_server_package}

Push the server package containing your app limiting it to a single instance. Your `server.xml` file must contain the `monitor-1.0` and `restConnector-1.0` features. It must also contain a `basicRegistry` element and administrator-role element.
```
       <featureManager>
           <feature>jsp-2.2</feature>
           <feature>monitor-1.0</feature>
           <feature>restConnector-1.0</feature>
       </featureManager>

       <basicRegistry>
           <user name="jconuser" password="jconpassw0rd"/>
       </basicRegistry>

       <administrator-role>
           <user>jconuser</user>
       </administrator-role>
```
{: codeblock}

Encode the password with the `securityUtility` tool provided by Liberty.
{: note}

### Start the JConsole app
{: start_jconsole_app}

JConsole is included with your Java installation.  To start the JConsole app go to &lt;java-home&gt;/bin and run the following command:
```
    jconsole -J-Djava.class.path=<java-home>/lib/jconsole.jar;<liberty-home>/wlp/clients/restConnector.jar
```
{: pre}

You may have to pass additional options to configure Java trustStore. The following options should work in most cases:
```
    -J-Djavax.net.ssl.trustStore=<java-home>/jre/lib/security/cacerts -J-Djavax.net.ssl.trustStorePassword=changeit -J-Djavax.net.ssl.trustStoreType=jks
```
{: codeblock}

### Complete the connection
{: start_jconsole_app}

1. In the Remote Process field enter the following URL: `service:jmx:rest://<appName>.mybluemix.net:443/IBMJMXConnectorREST`.
2. Specify the Username and Password fields.  Enter a user with an administrator-role, and a password from the `server.xml` file.
3. Click Connect.

When the connection succeeds, JConsole starts monitoring.

If the connection fails, you can produce logs to help diagnose the problem.  First, try collecting client side trace by adding `-J-Djava.util.logging.config.file=c:/tmp/logging.properties` to the `jconsole` command.
Here is a sample logging properties file:
```
    handlers= java.util.logging.FileHandler
    .level=INFO java.util.logging.FileHandler.pattern = /tmp/jmxtrace.log
    java.util.logging.FileHandler.limit = 50000
    java.util.logging.FileHandler.count = 1
    java.util.logging.FileHandler.formatter = java.util.logging.SimpleFormatter
    javax.management.level=FINEST
    javax.management.remote.level=FINER
    com.ibm.level=FINEST
```
{: codeblock}

You can also add `-J-Djavax.net.debug=ssl` to the `jconsole` command. This produces SSL diagnostic tracing in a separate JConsole output window.  Lastly, you can enable tracing on the server side by adding the following to your `server.xml` file:
```
    <logging traceSpecification="com.ibm.ws.jmx.*=all"/>
```
{: codeblock}


