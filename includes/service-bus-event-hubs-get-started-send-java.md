## Sending messages to Event Hubs

The Java client library for Event Hubs is available for use in Maven projects from the Maven Central Repository, and can be referenced using the
following dependency declaration inside of your Maven project file:    

```XML
    <dependency> 
   		<groupId>com.microsoft.azure</groupId> 
   		<artifactId>azure-eventhubs-clients</artifactId> 
   		<version>0.6.0</version> 
   	</dependency>   
 ```
 
For different types of build environments, the latest released JAR files can also be [explicitly obtained from the 
Maven Central Repository](https://search.maven.org/#search%7Cga%7C1%7Ca%3A%22azure-eventhubs%22) or from [the Release distribution point on GitHub](https://github.com/Azure/azure-event-hubs/releases).  

For a simple event publisher, you'll need to import the *com.microsoft.azure.eventhubs* package for the Event Hub client classes
and the *com.microsoft.azure.servicebus* package for utility classes like common exceptions that are shared with the  
Azure Service Bus Messaging client. 

For the sample below, first create a new Maven project for a console/shell application in your favorite Java IDE. The class will be
called ```Send```.     

``` Java

import java.io.IOException;
import java.nio.charset.*;
import java.util.*;
import java.util.concurrent.ExecutionException;

import com.microsoft.azure.eventhubs.*;
import com.microsoft.azure.servicebus.*;

public class Send
{
	public static void main(String[] args) 
			throws ServiceBusException, ExecutionException, InterruptedException, IOException
	{
```

Replace the Namespace name and the Event Hub name with the values used when you created the Event Hub. The ```sasKeyName```
and the ```sasKey``` correspond to name and key of the Send rule you created earlier. With that information, you 
create a connection string.

``` Java

		final String namespaceName = "----ServiceBusNamespaceName-----";
		final String eventHubName = "----EventHubName-----";
		final String sasKeyName = "-----SharedAccessSignatureKeyName-----";
		final String sasKey = "---SharedAccessSignatureKey----";
		ConnectionStringBuilder connStr = new ConnectionStringBuilder(namespaceName, eventHubName, sasKeyName, sasKey);
```

Then we create a singular event by turning a string into its UTF-8 byte encoding. We then create a new Event Hub client
instance from the connection string and send the message.   

``` Java 
				
		byte[] payloadBytes = "Test AMQP message from JMS".getBytes("UTF-8");
		EventData sendEvent = new EventData(payloadBytes);
		
		EventHubClient ehClient = EventHubClient.createFromConnectionStringSync(connStr.toString());
		ehClient.sendSync(sendEvent);
	}
}

``` 
