---
title: Verwenden von Azure Service Bus-Warteschlangen mit Java | Microsoft Docs
description: "Erfahren Sie mehr über die Verwendung von Service Bus-Warteschlangen in Azure. Die Codebeispiele wurden in Java geschrieben."
services: service-bus-messaging
documentationcenter: java
author: sethmanheim
manager: timlt
ms.assetid: f701439c-553e-402c-94a7-64400f997d59
ms.service: service-bus-messaging
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: Java
ms.topic: article
ms.date: 08/10/2017
ms.author: sethm
ms.openlocfilehash: 170f431525ffdc93a01fc085e48e69c3a774968e
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="how-to-use-service-bus-queues-with-java"></a>Verwenden von Service Bus-Warteschlangen mit Java
[!INCLUDE [service-bus-selector-queues](../../includes/service-bus-selector-queues.md)]

In diesem Artikel wird beschrieben, wie Sie Service Bus-Warteschlangen verwenden. Die Beispiele wurden in Java geschrieben und verwenden das [Azure-SDK für Java][Azure SDK for Java]. Die Szenarios behandeln die Themen **Erstellen von Warteschlangen**, **Senden und Empfangen von Nachrichten** und **Löschen von Warteschlangen**.

[!INCLUDE [howto-service-bus-queues](../../includes/howto-service-bus-queues.md)]

[!INCLUDE [service-bus-create-namespace-portal](../../includes/service-bus-create-namespace-portal.md)]

## <a name="configure-your-application-to-use-service-bus"></a>Konfigurieren Ihrer Anwendung für die Verwendung von Service Bus
Stellen Sie vor dem Erstellen dieses Beispiels sicher, dass Sie das [Azure SDK für Java][Azure SDK for Java] installiert haben. Wenn Sie Eclipse verwenden, können Sie das [Azure Toolkit für Eclipse][Azure Toolkit for Eclipse] installieren, das das Azure SDK für Java enthält. Sie können dann Ihrem Projekt die **Microsoft Azure-Bibliotheken für Java** hinzufügen:

![](./media/service-bus-java-how-to-use-queues/eclipselibs.png)

Fügen Sie die folgenden `import`-Anweisungen am Anfang der Java-Datei ein:

```java
// Include the following imports to use Service Bus APIs
import com.microsoft.windowsazure.services.servicebus.*;
import com.microsoft.windowsazure.services.servicebus.models.*;
import com.microsoft.windowsazure.core.*;
import javax.xml.datatype.*;
```

## <a name="create-a-queue"></a>Erstellen einer Warteschlange
Sie können Verwaltungsvorgänge für Service Bus-Warteschlangen über die **ServiceBusContract**-Klasse durchführen. **ServiceBusContract**-Objekte werden mit einer entsprechenden Konfiguration erstellt, die die SAS-Token mit Berechtigungen für deren Verwaltung kapselt. Die **ServiceBusContract**-Klasse ist der einzige Kommunikationspunkt mit Azure.

Die **ServiceBusService**-Klasse enthält Methoden zum Erstellen, Aufzählen und Löschen von Warteschlangen. Das folgende Beispiel zeigt, wie Sie mit einem Objekt vom Typ **ServiceBusService** eine Warteschlange mit dem Namen `TestQueue` in einem Namespace mit dem Namen `HowToSample` erstellen:

```java
Configuration config =
    ServiceBusConfiguration.configureWithSASAuthentication(
            "HowToSample",
            "RootManageSharedAccessKey",
            "SAS_key_value",
            ".servicebus.windows.net"
            );

ServiceBusContract service = ServiceBusService.create(config);
QueueInfo queueInfo = new QueueInfo("TestQueue");
try
{
    CreateQueueResult result = service.createQueue(queueInfo);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Mit den Methoden in `QueueInfo` können Sie Eigenschaften der Warteschlange anpassen (beispielsweise den Standardwert für die Gültigkeitsdauer, der auf an die Warteschlange gesendete Nachrichten angewendet werden soll). Das folgende Beispiel zeigt, wie eine Warteschlange mit der Bezeichnung `TestQueue` mit einer maximalen Größe von 5 GB erstellt wird:

````java
long maxSizeInMegabytes = 5120;
QueueInfo queueInfo = new QueueInfo("TestQueue");
queueInfo.setMaxSizeInMegabytes(maxSizeInMegabytes);
CreateQueueResult result = service.createQueue(queueInfo);
````

Sie können mit der Methode `listQueues` für Objekte vom Typ **ServiceBusContract** prüfen, ob eine Warteschlange mit einem spezifischen Namen bereits innerhalb eines Dienstnamespaces vorhanden ist.

## <a name="send-messages-to-a-queue"></a>Senden von Nachrichten an eine Warteschlange
Um eine Nachricht an eine Service Bus-Warteschlange zu senden, ruft Ihre Anwendung ein **ServiceBusContract**-Objekt ab. Der folgende Code zeigt, wie Sie eine Nachricht für die zuvor im Namespace `HowToSample` erstellte Warteschlange `TestQueue` senden können.

```java
try
{
    BrokeredMessage message = new BrokeredMessage("MyMessage");
    service.sendQueueMessage("TestQueue", message);
}
catch (ServiceException e)
{
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

Die Nachrichten, die an die Service Bus-Warteschlangen gesendet und von diesen empfangen werden, sind Instanzen der [BrokeredMessage][BrokeredMessage]-Klasse. [BrokeredMessage][BrokeredMessage]-Objekte verfügen über einen Satz von Standardeigenschaften (z.B. [Label](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.label#Microsoft_ServiceBus_Messaging_BrokeredMessage_Label) und [TimeToLive](/dotnet/api/microsoft.servicebus.messaging.brokeredmessage.timetolive#Microsoft_ServiceBus_Messaging_BrokeredMessage_TimeToLive)), ein Wörterbuch, in dem benutzerdefinierte anwendungsspezifische Eigenschaften enthalten sind, sowie einen Bestand beliebiger Anwendungsdaten. Eine Anwendung kann den Haupttext der Nachricht festlegen, indem ein beliebiges serialisierbares Objekt in den Konstruktor von [BrokeredMessage][BrokeredMessage] übergeben wird. Anschließend erfolgt die Serialisierung des Objekts mit dem passenden Serialisierer. Alternativ können Sie ein **java.IO.InputStream**-Objekt bereitstellen.

Das folgende Beispiel veranschaulicht, wie fünf Testnachrichten an den **MessageSender** `TestQueue` gesendet werden, der im vorherigen Codeausschnitt abgerufen wurde:

```java
for (int i=0; i<5; i++)
{
     // Create message, passing a string message for the body.
     BrokeredMessage message = new BrokeredMessage("Test message " + i);
     // Set an additional app-specific property.
     message.setProperty("MyProperty", i);
     // Send message to the queue
     service.sendQueueMessage("TestQueue", message);
}
```

Service Bus-Warteschlangen unterstützen eine maximale Nachrichtengröße von 256 KB für den [Standard-Tarif](service-bus-premium-messaging.md) und 1 MB für den [Premium-Tarif](service-bus-premium-messaging.md). Der Header, der die standardmäßigen und benutzerdefinierten Anwendungseigenschaften enthält, kann eine maximale Größe von 64 KB haben. Bei der Anzahl der Nachrichten, die in einer Warteschlange aufgenommen werden können, besteht keine Beschränkung. Allerdings gilt eine Deckelung bei der Gesamtgröße der in einer Warteschlange aufzunehmenden Nachrichten. Die Warteschlangengröße wird bei der Erstellung definiert. Die Obergrenze beträgt 5 GB.

## <a name="receive-messages-from-a-queue"></a>Empfangen von Nachrichten aus einer Warteschlange
Der einfachste Weg zum Empfangen von Nachrichten aus Warteschlangen sind **ServiceBusContract**-Objekte. Empfangene Nachrichten können in zwei unterschiedlichen Modi funktionieren: **ReceiveAndDelete** und **PeekLock**.

Bei Verwendung des **ReceiveAndDelete**-Modus ist der Nachrichtenempfang ein Single-Shot-Vorgang. Dies bedeutet, wenn Service Bus eine Leseanforderung für eine Nachricht in eine Warteschlange erhält, wird die Nachricht als verarbeitet gekennzeichnet und an die Anwendung zurückgesendet. Der **ReceiveAndDelete**-Modus (der Standardmodus) ist das einfachste Modell. Er wird am besten für Szenarios eingesetzt, bei denen es eine Anwendung tolerieren kann, wenn eine Nachricht bei Auftreten eines Fehlers nicht verarbeitet wird. Um dieses Verfahren zu verstehen, stellen Sie sich ein Szenario vor, in dem der Consumer die Empfangsanforderung ausstellt und dann abstürzt, bevor diese verarbeitet wird.
Da Service Bus die Nachricht als verwendet markiert hat, wird er jene Nachricht auslassen, die vor dem Absturz verwendet wurde, wenn die Anwendung neu startet und erneut mit der Verwendung von Nachrichten beginnt.

Im **PeekLock**-Modus ist der Empfangsvorgang zweistufig. Dadurch können Anwendungen unterstützt werden, die das Auslassen bzw. Fehlen von Nachrichten nicht zulassen können. Wenn Service Bus eine Anfrage erhält, ermittelt der Dienst die nächste zu verarbeitende Nachricht, sperrt diese, um zu verhindern, dass andere Consumer sie erhalten, und sendet sie dann zurück an die Anwendung. Nachdem die Anwendung die Verarbeitung der Nachricht abgeschlossen hat (oder sie zwecks zukünftiger Verarbeitung zuverlässig gespeichert hat), führt sie die zweite Phase des Empfangsprozesses durch Aufrufen von **Delete** für die empfangene Nachricht durch. Wenn Service Bus den **Delete**-Aufruf erkennt, wird die Nachricht als verwendet markiert und aus der Warteschlange entfernt.

Das folgende Beispiel zeigt, wie Nachrichten mit dem Modus **PeekLock** (nicht der Standardmodus) empfangen und verarbeitet werden können. Dieses Beispiel verwendet eine Endlosschleife, und die Nachrichten werden entsprechend ihres Eingangs in die `TestQueue` verarbeitet:

```java
try
{
    ReceiveMessageOptions opts = ReceiveMessageOptions.DEFAULT;
    opts.setReceiveMode(ReceiveMode.PEEK_LOCK);

    while(true)  {
         ReceiveQueueMessageResult resultQM =
                 service.receiveQueueMessage("TestQueue", opts);
        BrokeredMessage message = resultQM.getValue();
        if (message != null && message.getMessageId() != null)
        {
            System.out.println("MessageID: " + message.getMessageId());
            // Display the queue message.
            System.out.print("From queue: ");
            byte[] b = new byte[200];
            String s = null;
            int numRead = message.getBody().read(b);
            while (-1 != numRead)
            {
                s = new String(b);
                s = s.trim();
                System.out.print(s);
                numRead = message.getBody().read(b);
            }
            System.out.println();
            System.out.println("Custom Property: " +
                message.getProperty("MyProperty"));
            // Remove message from queue.
            System.out.println("Deleting this message.");
            //service.deleteMessage(message);
        }  
        else  
        {
            System.out.println("Finishing up - no more messages.");
            break;
            // Added to handle no more messages.
            // Could instead wait for more messages to be added.
        }
    }
}
catch (ServiceException e) {
    System.out.print("ServiceException encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
catch (Exception e) {
    System.out.print("Generic exception encountered: ");
    System.out.println(e.getMessage());
    System.exit(-1);
}
```

## <a name="how-to-handle-application-crashes-and-unreadable-messages"></a>Behandeln von Anwendungsabstürzen und nicht lesbaren Nachrichten
Service Bus stellt Funktionen zur Verfügung, die Sie bei der ordnungsgemäßen Behandlung von Fehlern in der Anwendung oder bei Problemen beim Verarbeiten einer Nachricht unterstützen. Wenn eine empfangene Anwendung eine Nachricht aus einem beliebigen Grund nicht verarbeiten kann, kann sie die **unlockMessage**-Methode für die empfangene Nachricht aufrufen (anstelle der **deleteMessage**-Methode). Dies führt dazu, dass Service Bus die Nachricht innerhalb der Warteschlange entsperrt und verfügbar macht, damit sie erneut empfangen werden kann, und zwar entweder durch dieselbe verarbeitende Anwendung oder durch eine andere verarbeitende Anwendung.

Mit einer innerhalb der Warteschlange gesperrten Nachricht ist außerdem eine Zeitlimitüberschreitung verknüpft. Wenn die Anwendung die Nachricht nicht verarbeiten kann, bevor die Zeitlimitüberschreitung der Sperre abläuft (z. B. falls die Anwendung abstürzt), so entsperrt Service Bus die Nachricht automatisch und macht sie verfügbar, sodass sie wieder empfangen werden kann.

Falls die Anwendung nach der Verarbeitung der Nachricht, aber vor Ausgabe der **deleteMessage**-Anforderung abstürzt, wird die Nachricht wieder an die Anwendung zugestellt, wenn diese neu gestartet wird. Dies wird häufig als *At Least Once Processing*(Verarbeitung mindestens einmal) bezeichnet und bedeutet, dass jede Nachricht mindestens einmal verarbeitet wird, wobei dieselbe Nachricht in bestimmten Situationen möglicherweise erneut zugestellt wird. Wenn eine doppelte Verarbeitung im betreffenden Szenario nicht geeignet ist, sollten Anwendungsentwickler ihrer Anwendung zusätzliche Logik für den Umgang mit der Übermittlung doppelter Nachrichten hinzufügen. Dies wird häufig durch die Verwendung der Methode **getMessageId** der Nachricht erzielt, die über mehrere Zustellversuche hinweg konstant bleibt.

## <a name="next-steps"></a>Nächste Schritte
Nachdem Sie nun mit den Grundlagen von Service Bus-Warteschlangen vertraut sind, finden Sie weitere Informationen unter [Warteschlangen, Themen und Abonnements][Queues, topics, and subscriptions].

Weitere Informationen finden Sie im [Java Developer Center](https://azure.microsoft.com/develop/java/).

[Azure SDK for Java]: http://azure.microsoft.com/develop/java/
[Azure Toolkit for Eclipse]: https://msdn.microsoft.com/library/azure/hh694271.aspx
[Queues, topics, and subscriptions]: service-bus-queues-topics-subscriptions.md
[BrokeredMessage]: /dotnet/api/microsoft.servicebus.messaging.brokeredmessage
