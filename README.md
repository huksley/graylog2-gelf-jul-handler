gelf-logger
===========

Unified logger for java to send message to Graylog via JUL, Log4j, etc. Zero external dependencies.

Supports sending via GELF to:
  * Graylog (UDP and TCP)
  * Logstash

Supports logging frameworks:
  * Java Util Logging
  * Log4j (preliminary version)
  
Java Util Logging
=================

Add these lines to logging.properties file (specifed via -Djava.util.logging.config.file=logging.properties or framework)
	
	# Add to handlers
	handlers=java.util.logging.ConsoleHandler, ..., com.wizecore.graylog.GelfHandler
	
	# Configure, everything is optional, see below for defaults
	 
	## Set host where graylog2-server is installed	
	# com.wizecore.graylog.GelfHandler.host = localhost
	
	## It is recommended to override facility field
	# com.wizecore.graylog.GelfHandler.facility = gelf-java
	# com.wizecore.graylog.GelfHandler.protocol = udp
	# com.wizecore.graylog.GelfHandler.port = 12201
	
	## Comma-separated list of additional fields to send (name=value, name=value, ...)
	# com.wizecore.graylog.GelfHandler.fields = 
	
	## If set to true, will gather exception, source class and method, logger name and call extended class
	# com.wizecore.graylog.GelfHandler.extended = true
	
	## Special class which implements com.wizecore.graylog.MessageUpdater
	# com.wizecore.graylog.GelfHandler.updater = 
	
	## If exception, add stacktrace to message
	# com.wizecore.graylog.GelfHandler.stacktrace = true	
	
	## Override host field (will be source field in Graylog) to something else
	# com.wizecore.graylog.GelfHandler.originHost = CUSTOM-HOST
	
	## Minimum level of messages to send to graylog
	# com.wizecore.graylog.GelfHandler.level = INFO
	
	## Formatter for message text
	# com.wizecore.graylog.GelfHandler.formatter = java.util.logging.SimpleFormatter
	
	## JUL optional property to configure java.util.logging.Filter
	# com.wizecore.graylog.GelfHandler.filter = 

Log4j
=====

Supports the same set of properties as in Java Util Logging configuration, see above.

If using XML configuration format, declare appender:

    <appender name="graylog2" class="com.wizecore.graylog.GelfAppender">
    	<param name="host" value="localhost"/>
    	<!-- additional params -->
    </appender>
    	
and then add it as a one of appenders:

    <root>
        <priority value="INFO"/>
        <appender-ref ref="graylog2"/>
    </root>

If using log4j.properties format, declare appender:

    # Define the graylog2 destination
    log4j.appender.graylog2=com.wizecore.graylog.GelfAppender
    log4j.appender.graylog2.layout=org.apache.log4j.PatternLayout
	 log4j.appender.graylog2.layout.ConversionPattern=%m
    log4j.appender.graylog2.host=localhost
    # log4j.appender.graylog2.param = value
    
Attach to root logger, or elsewhere:

    # Send all INFO logs to graylog2
    log4j.rootLogger=INFO, graylog2

