---
title: Erweiterte Richtlinien in Azure API Management | Microsoft-Dokumentation
description: "Erfahren Sie mehr über die erweiterten Richtlinien, die für die Verwendung in Azure API Management verfügbar sind."
services: api-management
documentationcenter: 
author: vladvino
manager: erikre
editor: 
ms.assetid: 8a13348b-7856-428f-8e35-9e4273d94323
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 01/09/2017
ms.author: apimpm
ms.openlocfilehash: e5a658e0d20d42911870f2522f6c1bab7529ea11
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="api-management-advanced-policies"></a>API Management – Erweiterte Richtlinien
Dieses Thema enthält eine Referenz für die folgenden API Management-Richtlinien. Weitere Informationen zum Hinzufügen und Konfigurieren von Richtlinien finden Sie unter [Richtlinien in API Management](http://go.microsoft.com/fwlink/?LinkID=398186).  
  
##  <a name="AdvancedPolicies"></a> Erweiterte Richtlinien  
  
-   [Ablaufsteuerung](api-management-advanced-policies.md#choose) – Bedingte Anwendung von Richtlinienanweisungen basierend auf den Ergebnissen der Auswertung von booleschen [Ausdrücken](api-management-policy-expressions.md)  
  
-   [Anforderung weiterleiten](#ForwardRequest) – leitet die Anforderung an den Back-End-Dienst.

-   [Parallelität einschränken:](#LimitConcurrency) verhindert die Ausführung der eingeschlossenen Richtlinien durch mehr als die angegebene Anzahl von Anforderungen gleichzeitig.
  
-   [Protokoll an Event Hub](#log-to-eventhub) – Sendet Nachrichten im angegebenen Format an einen von einem Protokollierungstool definierten Event Hub. 

-   [Modellantwort](#mock-response) – bricht die Pipelineausführung ab und gibt die Modellantwort unmittelbar an den Aufrufer zurück.
  
-   [Wiederholen](#Retry) – Wiederholt die Ausführung der eingeschlossenen Richtlinienanweisungen, falls und bis die Bedingung erfüllt ist. Die Ausführung wird mit den angegebenen Zeitintervallen und bis zur angegebenen Anzahl der Wiederholungsversuche wiederholt.  
  
-   [Zurückgegebene Antwort](#ReturnResponse) – bricht die Pipeline-Ausführung ab und gibt die angegebene Antwort unmittelbar an den Aufrufer zurück. 
  
-   [Unidirektionale Anforderung senden](#SendOneWayRequest) – sendet eine Anforderung an die angegebene URL, ohne auf eine Antwort zu warten.  
  
-   [Sendeanforderung](#SendRequest) – Sendet eine Anforderung an die angegebene URL.  

-   [HTTP-Proxy festlegen](#SetHttpProxy): Sie können weitergeleitete Anforderungen über einen HTTP-Proxy leiten.  

-   [Anforderungsmethode festlegen](#SetRequestMethod) – Dient der Vornahme von Änderungen der HTTP-Anforderungsmethode.  
  
-   [Statuscode festlegen](#SetStatus) – Ändert den HTTP-Statuscode in den angegebenen Wert.  
  
-   [Variable festlegen](api-management-advanced-policies.md#set-variable) – Speichert einen Wert in einer benannten [Kontext](api-management-policy-expressions.md#ContextVariables)variablen, um später darauf zugreifen zu können.  

-   [Ablaufverfolgung](#Trace) – Fügt eine Zeichenfolge in der Ausgabe für den [API-Inspektor](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) hinzu.  
  
-   [Warten](#Wait) – Wartet darauf, dass eingeschlossene Richtlinien für [Send request](api-management-advanced-policies.md#SendRequest) (Sendeanforderung), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey) (Wert aus dem Cache abrufen) oder [Control flow](api-management-advanced-policies.md#choose) (Ablaufsteuerung) abgeschlossen werden, bevor der Vorgang fortgesetzt wird.  
  
##  <a name="choose"></a> Ablaufsteuerung  
 Mit der Richtlinie `choose` werden eingeschlossene Richtlinienanweisungen basierend auf dem Ergebnis der Auswertung von booleschen Ausdrücken angewendet. Dies ähnelt „if-then-else“ oder einem Switchkonstrukt in einer Programmiersprache.  
  
###  <a name="ChoosePolicyStatement"></a> Richtlinienanweisung  
  
```xml  
<choose>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <when condition="Boolean expression | Boolean constant">   
        <!— one or more policy statements to be applied if the above condition is true  -->  
    </when>   
    <otherwise>   
        <!— one or more policy statements to be applied if none of the above conditions are true  -->  
</otherwise>   
</choose>  
```  
  
 Die Ablaufsteuerungsrichtlinie muss mindestens ein `<when/>`-Element enthalten. Das `<otherwise/>`-Element ist optional. Bedingungen in `<when/>`-Elementen werden in der Reihenfolge ihrer Anordnung in der Richtlinie ausgewertet. Richtlinienanweisungen, die in das erste `<when/>`-Element eingeschlossen sind und für die das Bedingungsattribut `true` lautet, werden angewendet. Richtlinien, die in das `<otherwise/>`-Element (falls vorhanden) eingeschlossen sind, werden angewendet, wenn alle Bedingungsattribute des `<when/>`-Elements `false` lauten.  
  
### <a name="examples"></a>Beispiele  
  
####  <a name="ChooseExample"></a> Beispiel  
 Das folgende Beispiel enthält eine [set-variable](api-management-advanced-policies.md#set-variable)-Richtlinie und zwei Ablaufsteuerungsrichtlinien.  
  
 Die „set-variable“-Richtlinie befindet sich im Abschnitt für den eingehenden Datenverkehr und erstellt eine boolesche `isMobile`-[Kontext](api-management-policy-expressions.md#ContextVariables)variable, die auf „true“ festgelegt ist, wenn der `User-Agent`-Anforderungsheader den Text `iPad` oder `iPhone` enthält.  
  
 Die erste Ablaufsteuerungsrichtlinie befindet sich ebenfalls im Abschnitt für eingehenden Datenverkehr und wendet bedingungsabhängig eine von zwei Richtlinien vom Typ [Abfrageparameter setzen](api-management-transformation-policies.md#SetQueryStringParameter) an. Dies richtet sich nach dem Wert der Kontextvariablen `isMobile`.  
  
 Die zweite Ablaufsteuerungsrichtlinie befindet sich im Abschnitt für den ausgehenden Datenverkehr und wendet die Richtlinie vom Typ [XML zu JSON konvertieren](api-management-transformation-policies.md#ConvertXMLtoJSON) bedingungsabhängig an, wenn `isMobile` auf `true` festgelegt ist.  
  
```xml  
<policies>  
    <inbound>  
        <set-variable name="isMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
        <base />  
        <choose>  
            <when condition="@(context.Variables.GetValueOrDefault<bool>("isMobile"))">  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>true</value>  
                </set-query-parameter>  
            </when>  
            <otherwise>  
                <set-query-parameter name="mobile" exists-action="override">  
                    <value>false</value>  
                </set-query-parameter>  
            </otherwise>  
        </choose>  
    </inbound>  
    <outbound>  
        <base />  
        <choose>  
            <when condition="@(context.GetValueOrDefault<bool>("isMobile"))">  
                <xml-to-json kind="direct" apply="always" consider-accept-header="false"/>  
            </when>  
        </choose>  
    </outbound>  
</policies>  
```  
  
#### <a name="example"></a>Beispiel  
 In diesem Beispiel wird gezeigt, wie Inhalte gefiltert werden, indem Datenelemente aus der über den Back-End-Dienst empfangenen Antwort entfernt werden, wenn das Produkt `Starter` verwendet wird. Eine Demonstration der Konfiguration und Verwendung dieser Richtlinie finden Sie unter [Cloud Cover Episode 177: More API Management Features with Vlad Vinogradsky](https://azure.microsoft.com/documentation/videos/episode-177-more-api-management-features-with-vlad-vinogradsky/) (Cloud Cover-Episode 177 zu weiteren API Management-Funktionen mit Vlad Vinogradsky). Führen Sie einen schnellen Vorlauf bis 34:30 durch. Beginnen Sie bei 31:50, um eine Übersicht über die [API „The Dark Sky Forecast“](https://developer.forecast.io/) zu erhalten, die für diese Demo verwendet wird.  
  
```xml  
<!-- Copy this snippet into the outbound section to remove a number of data elements from the response received from the backend service based on the name of the api product -->  
<choose>  
  <when condition="@(context.Response.StatusCode == 200 && context.Product.Name.Equals("Starter"))">  
    <set-body>@{  
        var response = context.Response.Body.As<JObject>();  
        foreach (var key in new [] {"minutely", "hourly", "daily", "flags"}) {  
          response.Property (key).Remove ();  
        }  
        return response.ToString();  
      }  
    </set-body>  
  </when>  
</choose>  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|choose|Stammelement|Ja|  
|when|Die Bedingung, die für die Teile `if` oder `ifelse` der `choose`-Richtlinie verwendet werden soll. Wenn die `choose`-Richtlinie über mehrere `when`-Abschnitte verfügt, werden diese nacheinander ausgewertet. Wenn die Bedingung (`condition`) eines when-Elements bei der Auswertung `true` ergibt, werden keine weiteren `when`-Bedingungen ausgewertet.|Ja|  
|otherwise|Enthält den Richtliniencodeausschnitt, der verwendet werden soll, wenn die Auswertung für keine `when`-Bedingung `true` ergibt.|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|  
|---------------|-----------------|--------------|  
|condition="boolescher Ausdruck &#124; boolesche Konstante"|Der boolesche Ausdruck bzw. die Konstante, der bzw. die ausgewertet werden soll, wenn die enthaltende `when`-Richtlinienanweisung ausgewertet wird.|Ja|  
  
###  <a name="ChooseUsage"></a> Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  <a name="ForwardRequest"></a> Anforderung weiterleiten  
 Mit der `forward-request`-Richtlinie wird die eingehende Anforderung an den Back-End-Dienst weitergeleitet, der im Anforderungs[kontext](api-management-policy-expressions.md#ContextVariables) angegeben ist. Die Back-End-Dienst-URL ist in den API-[Einstellungen](https://azure.microsoft.com/documentation/articles/api-management-howto-create-apis/#configure-api-settings) angegeben und kann mit der Richtlinie [Back-End-Dienst festlegen](api-management-transformation-policies.md) geändert werden.  
  
> [!NOTE]
>  Das Entfernen dieser Richtlinie führt dazu, dass die Anforderung nicht an den Back-End-Dienst weitergeleitet wird und dass die Richtlinien im Abschnitt für ausgehenden Datenverkehr sofort ausgewertet werden, nachdem die Richtlinien im Abschnitt für den eingehenden Datenverkehr erfolgreich abgeschlossen wurden.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<forward-request timeout="time in seconds" follow-redirects="true | false"/>  
```  
  
### <a name="examples"></a>Beispiele  
  
#### <a name="example"></a>Beispiel  
 Mit der folgenden Richtlinie auf API-Ebene werden alle Anforderungen mit einem Zeitüberschreitungsintervall von 60 Sekunden an den Back-End-Dienst weitergeleitet.  
  
```xml  
<!-- api level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="60"/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Beispiel  
 Bei dieser Richtlinie auf Vorgangsebene wird das `base`-Element verwendet, um die Back-End-Richtlinie vom übergeordneten API-Ebenenbereich zu erben.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <base/>  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Beispiel  
 Bei dieser Richtlinie auf Vorgangsebene werden alle Anforderungen mit einer Zeitüberschreitung von 120 an den Back-End-Dienst weitergeleitet, und die Back-End-Richtlinie der übergeordneten API-Ebene wird nicht geerbt.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <forward-request timeout="120"/>   
        <!-- effective policy. note the absence of <base/> -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
#### <a name="example"></a>Beispiel  
 Bei dieser Richtlinie auf Vorgangsebene werden Anforderungen nicht an den Back-End-Dienst weitergeleitet.  
  
```xml  
<!-- operation level -->  
<policies>  
    <inbound>  
        <base/>  
    </inbound>  
    <backend>  
        <!-- no forwarding to backend -->  
    </backend>  
    <outbound>  
        <base/>          
    </outbound>  
</policies>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|forward-request|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|timeout="integer"|Das Zeitüberschreitungsintervall in Sekunden, nach dem für den Aufruf des Back-End-Diensts ein Fehler auftritt.|Nein|Keine Zeitüberschreitung|  
|follow-redirects="true &#124; false"|Gibt an, ob Umleitungen vom Back-End-Dienst vom Gateway verfolgt oder an den Aufrufer zurückgegeben werden.|Nein|false|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** backend  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  Einschränken der Parallelität durch <a name="LimitConcurrency"></a>  
 Die Richtlinie `limit-concurrency` verhindert die Ausführung der eingeschlossenen Richtlinien durch mehr als die angegebene Anzahl von Anforderungen gleichzeitig. Beim Überschreiten des Schwellenwerts werden einer Warteschlange neue Anforderungen hinzugefügt, bis die maximale Warteschlangenlänge erreicht ist. Wenn die Warteschlange voll ist, wird für neue Anforderungen sofort ein Fehler ausgelöst.
  
###  <a name="LimitConcurrencyStatement"></a> Richtlinienanweisung  
  
```xml  
<limit-concurrency key="expression" max-count="number" timeout="in seconds" max-queue-length="number">
        <!— nested policy statements -->  
</limit-concurrency>
``` 

### <a name="examples"></a>Beispiele  
  
####  <a name="ChooseExample"></a> Beispiel  
 Das folgende Beispiel veranschaulicht das Beschränken der Anzahl der Anforderungen, die an ein Back-End weitergeleitet werden, basierend auf dem Wert einer Kontextvariable.
 
```xml  
<policies>
  <inbound>…</inbound>
  <backend>
    <limit-concurrency key="@((string)context.Variables["connectionId"])" max-count="3" timeout="60">
      <forward-request timeout="120"/>
    <limit-concurrency/>
  </backend>
  <outbound>…</outbound>
</policies>
```

### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|    
|limit-concurrency|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|--------------|  
|key|Eine Zeichenfolge. Ausdruck zulässig. Gibt den Bereich der Parallelität an. Kann von mehreren Richtlinien verwendet werden.|Ja|N/V|  
|max-count|Eine ganze Zahl. Gibt eine maximale Anzahl von Anforderungen an, die an die Richtlinie weitergeleitet werden können|Ja|N/V|  
|timeout|Eine ganze Zahl. Ausdruck zulässig. Gibt die Anzahl der Sekunden an, die eine Anforderung warten soll, bevor der Fehler „429 Zu viele Anforderungen“ ausgelöst wird|Nein|Infinity|  
|max-queue-length|Eine ganze Zahl. Ausdruck zulässig. Gibt die maximale Warteschlangenlänge an. Eingehende Anforderungen für diese Richtlinie werden sofort mit dem Fehler „429 Zu viele Anforderungen“ beendet, wenn die Warteschlange voll ist.|Nein|Infinity|  
  
###  <a name="ChooseUsage"></a> Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  

##  <a name="log-to-eventhub"></a> Protokoll an Event Hub  
 Mit der `log-to-eventhub`-Richtlinie werden Nachrichten im angegebenen Format an ein von einem Protokollierungstool definiertes Nachrichtenziel gesendet. Wie anhand des Namens bereits erkennbar ist, wird diese Richtlinie zum Speichern von ausgewählten Anforderungs- oder Antwortkontextinformationen für die Online- oder Offlineanalyse verwendet.  
  
> [!NOTE]
>  Eine Schritt-für-Schritt-Anleitung für die Konfiguration eines Event Hub und von Protokollierungsereignissen finden Sie unter [Protokollieren von Ereignissen in Azure Event Hubs mit Azure API Management](https://azure.microsoft.com/documentation/articles/api-management-howto-log-event-hubs/).  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<log-to-eventhub logger-id="id of the logger entity" partition-id="index of the partition where messages are sent" partition-key="value used for partition assignment">  
  Expression returning a string to be logged  
</log-to-eventhub>  
  
```  
  
### <a name="example"></a>Beispiel  
 Eine beliebige Zeichenfolge kann als Wert für die Protokollierung in Event Hubs verwendet werden. In diesem Beispiel werden Datum und Uhrzeit, Bereitstellungsdienstname, Anforderungs-ID, IP-Adresse und Vorgangsname für alle eingehenden Aufrufe in dem Event Hub-Protokollierungstool protokolliert, das unter der `contoso-logger`-ID registriert ist.  
  
```xml  
<policies>  
  <inbound>  
    <log-to-eventhub logger-id ='contoso-logger'>  
      @( string.Join(",", DateTime.UtcNow, context.Deployment.ServiceName, context.RequestId, context.Request.IpAddress, context.Operation.Name) )   
    </log-to-eventhub>  
  </inbound>  
  <outbound>          
  </outbound>  
</policies>  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|log-to-eventhub|Stammelement Der Wert dieses Elements ist die Zeichenfolge für die Protokollierung in Ihrem Event Hub.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|  
|---------------|-----------------|--------------|  
|logger-id|Die ID des Protokollierungstools, das bei Ihrem API Management-Dienst registriert ist.|Ja|  
|partition-id|Gibt den Index der Partition an, an die Nachrichten gesendet werden.|Optional. Dieses Attribut darf nicht genutzt werden, wenn `partition-key` verwendet wird.|  
|partition-key|Gibt den Wert an, der für die Partitionszuweisung verwendet wird, wenn Nachrichten gesendet werden.|Optional. Dieses Attribut darf nicht genutzt werden, wenn `partition-id` verwendet wird.|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  

##  <a name="mock-response"></a> Modellantwort  
Die `mock-response` wird, wie der Name impliziert, dazu verwendet, APIs und Vorgänge zu modellieren. Sie bricht die normale Pipelineausführung ab und gibt die Modellantwort an den Aufrufer zurück. Die Richtlinie versucht immer, möglichst realitätsnahe Antworten zurückgeben. Sie bevorzugt dabei Beispiele für Antwortinhalte, soweit verfügbar. Sie generiert Beispielantworten aus Schemas, sofern Schemas bereitgestellt werden, Beispiele hingegen nicht. Wenn weder Beispiele noch Schemas gefunden wurden, werden Antworten ohne Inhalt zurückgegeben.
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<mock-response status-code="code" content-type="media type"/>  
  
```  
  
### <a name="examples"></a>Beispiele  
  
```xml  
<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code. First found content type is used. If no example or schema is found, the content is empty. -->
<mock-response/>

<!-- Returns 200 OK status code. Content is based on an example or schema, if provided for this 
status code and media type. If no example or schema found, the content is empty. -->
<mock-response status-code='200' content-type='application/json'/>  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|mock-response|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|--------------|  
|status-code|Gibt den Statuscode der Antwort an und wird verwendet, um ein passendes Beispiel oder Schema auszuwählen|Nein|200|  
|Inhaltstyp|Gibt den Headerwert `Content-Type` für die Antwort an und wird verwendet, um ein passendes Beispiel oder Schema auszuwählen|Nein|Keine|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche

##  <a name="Retry"></a> Wiederholen  
 Die `retry`-Richtlinie führt ihre untergeordneten Richtlinien einmal aus und versucht dann, die Ausführung zu wiederholen, bis `condition` für die Wiederholung `false` lautet oder der Wert unter `count` ausgeschöpft ist.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
  
<retry  
    condition="boolean expression or literal"  
    count="number of retry attempts"  
    interval="retry interval in seconds"  
    max-interval="maximum retry interval in seconds"  
    delta="retry interval delta in seconds"  
    first-fast-retry="boolean expression or literal">  
        <!-- One or more child policies. No restrictions -->  
</retry>  
  
```  
  
### <a name="example"></a>Beispiel  
 Im folgenden Beispiel wird bis zu zehnmal versucht, die Anforderungsweiterleitung zu wiederholen, indem der exponentielle Wiederholungsalgorithmus verwendet wird. Da `first-fast-retry` auf „false“ festgelegt ist, unterliegen alle Wiederholungsversuche dem exponentiellen Wiederholungsalgorithmus.  
  
```xml  
  
<retry  
    condition="@(context.Response.StatusCode == 500)"  
    count="10"  
    interval="10"  
    max-interval="100"  
    delta="10"  
    first-fast-retry="false">  
        <forward-request />  
</retry>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|retry|Stammelement Kann beliebige andere Richtlinien als untergeordnete Elemente enthalten.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|condition|Ein boolesches Literal oder ein [Ausdruck](api-management-policy-expressions.md), mit dem angegeben wird, ob Wiederholungsversuche beendet (`false`) oder fortgesetzt (`true`) werden sollen.|Ja|–|  
|count|Eine positive Zahl, mit der die maximale Anzahl von Wiederholungsversuchen angegeben wird.|Ja|N/V|  
|interval|Ein positiver Wert in Sekunden, mit dem das Warteintervall zwischen den Wiederholungsversuchen angegeben wird.|Ja|N/V|  
|max-interval|Ein positiver Wert in Sekunden, mit dem das maximale Warteintervall zwischen den Wiederholungsversuchen angegeben wird. Wird zum Implementieren eines exponentiellen Wiederholungsalgorithmus verwendet.|Nein|–|  
|delta|Ein positiver Wert in Sekunden, mit dem das Inkrement für das Warteintervall angegeben wird. Wird zum Implementieren der linearen und exponentiellen Wiederholungsalgorithmen verwendet.|Nein|–|  
|first-fast-retry|Wenn `true` festgelegt ist, wird der erste Wiederholungsversuch sofort durchgeführt.|Nein|`false`|  
  
> [!NOTE]
>  Wenn nur `interval` angegeben ist, werden Wiederholungsversuche nach **festen** Intervallen durchgeführt.  
>  Wenn nur `interval` und `delta` angegeben sind, wird ein Wiederholungsalgorithmus mit **linearem** Intervall verwendet, bei dem die Wartezeit zwischen den Wiederholungsversuchen mit der folgenden Formel berechnet wird: `interval + (count - 1)*delta`.  
>  Wenn `interval`, `max-interval` und `delta` angegeben sind, wird ein Wiederholungsalgorithmus mit **exponentiellem** Intervall angewendet, bei dem die Wartezeit zwischen den Wiederholungsversuchen gemäß der folgenden Formel vom Wert von `interval` exponentiell auf den Wert `max-interval` anwächst: `min(interval + (2^count - 1) * random(delta * 0.8, delta * 1.2), max-interval)`.  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden. Beachten Sie, dass Nutzungseinschränkungen von untergeordneten Richtlinien von dieser Richtlinie geerbt werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  <a name="ReturnResponse"></a> Rückantwort  
 Mit der `return-response`-Richtlinie wird die Pipelineausführung abgebrochen und entweder eine Standardantwort oder eine benutzerdefinierte Antwort an den Aufrufer zurückgegeben. Die Standardantwort ist `200 OK` ohne Text. Die benutzerdefinierte Antwort kann über eine Kontextvariable oder Richtlinienanweisungen angegeben werden. Bei Angabe von beidem wird die in der Kontextvariablen enthaltene Antwort von den Richtlinienanweisungen geändert, bevor sie an den Aufrufer zurückgegeben wird.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<return-response response-variable-name="existing context variable">  
  <set-header/>  
  <set-body/>  
  <set-status/>  
</return-response>  
  
```  
  
### <a name="example"></a>Beispiel  
  
```xml  
<return-response>  
   <set-status code="401" reason="Unauthorized"/>  
   <set-header name="WWW-Authenticate" exists-action="override">  
      <value>Bearer error="invalid_token"</value>  
   </set-header>  
</return-response>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|return-response|Stammelement|Ja|  
|set-header|Eine [set-header](api-management-transformation-policies.md#SetHTTPheader)-Richtlinienanweisung.|Nein|  
|set-body|Eine [set-body](api-management-transformation-policies.md#SetBody)-Richtlinienanweisung.|Nein|  
|set-status|Eine [set-status](api-management-advanced-policies.md#SetStatus)-Richtlinienanweisung.|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|  
|---------------|-----------------|--------------|  
|response-variable-name|Der Name der Kontextvariablen, auf die beispielsweise von einer vorgelagerten [send-request](api-management-advanced-policies.md#SendRequest)-Richtlinie verwiesen wird und die ein `Response`-Objekt enthält.|Optional.|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  <a name="SendOneWayRequest"></a> Unidirektionale Anforderung senden  
 Die `send-one-way-request`-Richtlinie sendet die bereitgestellte Anforderung an die angegebene URL, ohne auf eine Antwort zu warten.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<send-one-way-request mode="new | copy">  
  <url>...</url>  
  <method>...</method>  
  <header name="" exists-action="override | skip | append | delete">...</header>  
  <body>...</body>  
</send-one-way-request>  
  
```  
  
### <a name="example"></a>Beispiel  
 Diese Beispielrichtlinie zeigt ein Beispiel für die Verwendung der `send-one-way-request`-Richtlinie zum Senden einer Nachricht an einen Slack-Chatraum, wenn der HTTP-Antwortcode größer als oder gleich 500 ist. Weitere Informationen zu diesem Beispiel finden Sie unter [Verwenden externer Dienste über den Azure API Management-Dienst](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|send-one-way-request|Stammelement|Ja|  
|url|Die URL der Anforderung.|„Nein“, wenn „mode=copy“, andernfalls „Ja“.|  
|method|Die HTTP-Methode für die Anforderung.|„Nein“, wenn „mode=copy“, andernfalls „Ja“.|  
|Header|Anforderungsheader. Verwenden Sie mehrere Headerelemente für mehrere Anforderungsheader.|Nein|  
|body|Anforderungstext|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|mode="string"|Bestimmt, ob dies eine neue Anforderung oder eine Kopie der aktuellen Anforderung ist. Im Ausgangsmodus wird der Anforderungstext durch „mode=copy“ nicht initialisiert.|Nein|Neu|  
|Name|Gibt den Namen des festzulegenden Headers an.|Ja|–|  
|exists-action|Gibt die auszuführende Aktion an, wenn ein Header bereits angegeben wurde. Dieses Attribut muss einen der folgenden Werte aufweisen.<br /><br /> – override – Ersetzt den Wert des vorhandenen Headers.<br />– skip – Ersetzt den vorhandenen Headerwert nicht.<br />– append – Fügt den Wert an den vorhandenen Headerwert an.<br />– delete – Entfernt den Header aus der Anforderung.<br /><br /> Bei `override` führt die Auflistung mehrerer Einträge mit demselben Namen dazu, dass der Header gemäß aller Einträge festgelegt wird (die mehrfach aufgeführt sind); nur die aufgelisteten Werte werden im Ergebnis festgelegt.|Nein|override|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  <a name="SendRequest"></a> Anforderung senden  
 Die `send-request`-Richtlinie sendet die bereitgestellte Anforderung an die angegebene URL und wartet nicht länger, als durch den Zeitüberschreitungswert festgelegt ist.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<send-request mode="new|copy" response-variable-name="" timeout="60 sec" ignore-error  
="false|true">  
  <set-url>...</set-url>  
  <set-method>...</set-method>  
  <set-header name="" exists-action="override|skip|append|delete">...</set-header>  
  <set-body>...</set-body>  
</send-request>  
  
```  
  
### <a name="example"></a>Beispiel  
 In diesem Beispiel ist eine Möglichkeit dargestellt, wie Sie ein Verweistoken mit einem Autorisierungsserver überprüfen können. Weitere Informationen zu diesem Beispiel finden Sie unter [Verwenden externer Dienste über den Azure API Management-Dienst](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<inbound>  
  <!-- Extract Token from Authorization header parameter -->  
  <set-variable name="token" value="@(context.Request.Headers.GetValueOrDefault("Authorization","scheme param").Split(' ').Last())" />  
  
  <!-- Send request to Token Server to validate token (see RFC 7662) -->  
  <send-request mode="new" response-variable-name="tokenstate" timeout="20" ignore-error="true">  
    <set-url>https://microsoft-apiappec990ad4c76641c6aea22f566efc5a4e.azurewebsites.net/introspection</set-url>  
    <set-method>POST</set-method>  
    <set-header name="Authorization" exists-action="override">  
      <value>basic dXNlcm5hbWU6cGFzc3dvcmQ=</value>  
    </set-header>  
    <set-header name="Content-Type" exists-action="override">  
      <value>application/x-www-form-urlencoded</value>  
    </set-header>  
    <set-body>@($"token={(string)context.Variables["token"]}")</set-body>  
  </send-request>  
  
  <choose>  
        <!-- Check active property in response -->  
        <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
            <!-- Return 401 Unauthorized with http-problem payload -->  
            <return-response response-variable-name="existing response variable">  
                <set-status code="401" reason="Unauthorized" />  
                <set-header name="WWW-Authenticate" exists-action="override">  
                    <value>Bearer error="invalid_token"</value>  
                </set-header>  
            </return-response>  
        </when>  
    </choose>  
  <base />  
</inbound>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|send-request|Stammelement|Ja|  
|url|Die URL der Anforderung.|„Nein“, wenn „mode=copy“, andernfalls „Ja“.|  
|method|Die HTTP-Methode für die Anforderung.|„Nein“, wenn „mode=copy“, andernfalls „Ja“.|  
|Header|Anforderungsheader. Verwenden Sie mehrere Headerelemente für mehrere Anforderungsheader.|Nein|  
|body|Anforderungstext|Nein|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|mode="string"|Bestimmt, ob dies eine neue Anforderung oder eine Kopie der aktuellen Anforderung ist. Im Ausgangsmodus wird der Anforderungstext durch „mode=copy“ nicht initialisiert.|Nein|Neu|  
|response-variable-name="string"|Falls nicht vorhanden, wird `context.Response` verwendet.|Nein|–|  
|timeout="integer"|Das Zeitüberschreitungsintervall in Sekunden, bis für den Aufruf der URL ein Fehler auftritt.|Nein|60|  
|ignore-error|Bei „true“ und einem Fehler für die Anforderung:<br /><br /> -   Wenn „response-variable-name“ angegeben wurde, ist dies ein Nullwert.<br />-   Wenn „response-variable-name“ nicht angegeben wurde, wird „context.Request“ nicht aktualisiert.|Nein|false|  
|Name|Gibt den Namen des festzulegenden Headers an.|Ja|–|  
|exists-action|Gibt die auszuführende Aktion an, wenn ein Header bereits angegeben wurde. Dieses Attribut muss einen der folgenden Werte aufweisen.<br /><br /> – override – Ersetzt den Wert des vorhandenen Headers.<br />– skip – Ersetzt den vorhandenen Headerwert nicht.<br />– append – Fügt den Wert an den vorhandenen Headerwert an.<br />– delete – Entfernt den Header aus der Anforderung.<br /><br /> Bei `override` führt die Auflistung mehrerer Einträge mit demselben Namen dazu, dass der Header gemäß aller Einträge festgelegt wird (die mehrfach aufgeführt sind); nur die aufgelisteten Werte werden im Ergebnis festgelegt.|Nein|override|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  <a name="SetHttpProxy"></a> HTTP-Proxy festlegen  
 Die `proxy`-Richtlinie können Sie zum Routen von Anforderungen, die an das Back-End weitergeleitet wurden, über einen HTTP-Proxy verwenden. Nur HTTP (nicht HTTPS) wird zwischen dem Gateway und dem Proxy unterstützt. Nur Standard- und NTLM-Authentifizierung.
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<proxy url="http://hostname-or-ip:port" username="username" password="password" />  
  
```  
  
### <a name="example"></a>Beispiel  
Beachten Sie die Verwendung von [Eigenschaften](api-management-howto-properties.md) als Werte für Benutzername und Kennwort, um zu vermeiden, dass vertrauliche Informationen im Richtliniendokument gespeichert werden.  
  
```xml  
<proxy url="http://192.168.1.1:8080" username={{username}} password={{password}} />
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|proxy|Stammelement|Ja|  

### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|url="string"|Proxy-URL im Format http://host:port.|Ja|N/V|  
|username="string"|Der Benutzername, der für die Authentifizierung mit dem Proxy verwendet wird.|Nein|N/V|  
|password="string"|Kennwort, das für die Authentifizierung mit dem Proxy verwendet wird.|Nein|N/V|  

### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound  
  
-   **Richtlinienbereiche:** alle Bereiche  

##  <a name="SetRequestMethod"></a> Anforderungsmethode festlegen  
 Mit der `set-method`-Richtlinie können Sie die HTTP-Anforderungsmethode für eine Anforderung ändern.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<set-method>METHOD</set-method>  
  
```  
  
### <a name="example"></a>Beispiel  
 Diese Beispielrichtlinie, in der die `set-method`-Richtlinie verwendet wird, veranschaulicht ein Beispiel zum Senden einer Nachricht an einen Slack-Chatraum, wenn der HTTP-Antwortcode größer als oder gleich 500 ist. Weitere Informationen zu diesem Beispiel finden Sie unter [Verwenden externer Dienste über den Azure API Management-Dienst](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
    <when condition="@(context.Response.StatusCode >= 500)">  
      <send-one-way-request mode="new">  
        <set-url>https://hooks.slack.com/services/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</set-url>  
        <set-method>POST</set-method>  
        <set-body>@{  
                return new JObject(  
                        new JProperty("username","APIM Alert"),  
                        new JProperty("icon_emoji", ":ghost:"),  
                        new JProperty("text", String.Format("{0} {1}\nHost: {2}\n{3} {4}\n User: {5}",  
                                                context.Request.Method,  
                                                context.Request.Url.Path + context.Request.Url.QueryString,  
                                                context.Request.Url.Host,  
                                                context.Response.StatusCode,  
                                                context.Response.StatusReason,  
                                                context.User.Email  
                                                ))  
                        ).ToString();  
            }</set-body>  
      </send-one-way-request>  
    </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|set-method|Stammelement Mit dem Wert des Elements wird die HTTP-Methode angegeben.|Ja|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  <a name="SetStatus"></a> Statuscode festlegen  
 Mit der `set-status`-Richtlinie wird der HTTP-Statuscode auf den angegebenen Wert festgelegt.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<set-status code="" reason=""/>  
  
```  
  
### <a name="example"></a>Beispiel  
 Dieses Beispiel zeigt, wie Sie eine 401-Antwort zurückgeben, wenn das Autorisierungstoken ungültig ist. Weitere Informationen finden Sie unter [Verwenden externer Dienste über den Azure API Management-Dienst](https://azure.microsoft.com/documentation/articles/api-management-sample-send-request/).  
  
```xml  
<choose>  
  <when condition="@((bool)((IResponse)context.Variables["tokenstate"]).Body.As<JObject>()["active"] == false)">  
    <return-response response-variable-name="existing response variable">  
      <set-status code="401" reason="Unauthorized" />  
      <set-header name="WWW-Authenticate" exists-action="override">  
        <value>Bearer error="invalid_token"</value>  
      </set-header>  
    </return-response>  
  </when>  
</choose>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|set-status|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|code="integer"|Der zurückzugebende HTTP-Statuscode.|Ja|–|  
|reason="string"|Eine Beschreibung des Grunds zum Zurückgeben des Statuscodes.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  

##  <a name="set-variable"></a> Variable festlegen  
 Mit der `set-variable`-Richtlinie wird eine [Kontext](api-management-policy-expressions.md#ContextVariables)variable deklariert und einem Wert zugewiesen, der über einen [Ausdruck](api-management-policy-expressions.md) oder ein Zeichenfolgenliteral angegeben wird. Wenn der Ausdruck ein Literal enthält, wird die Konvertierung in eine Zeichenfolge durchgeführt, und der Typ des Werts lautet `System.String`.  
  
###  <a name="set-variablePolicyStatement"></a> Richtlinienanweisung  
  
```xml  
<set-variable name="variable name" value="Expression | String literal" />  
```  
  
###  <a name="set-variableExample"></a> Beispiel  
 Im folgenden Beispiel wird eine set-variable-Richtlinie im Abschnitt für den eingehenden Datenverkehr veranschaulicht. Diese „set-variable“-Richtlinie erstellt eine boolesche `isMobile`-[Kontext](api-management-policy-expressions.md#ContextVariables)variable, die auf „true“ festgelegt ist, wenn der `User-Agent`-Anforderungsheader den Text `iPad` oder `iPhone` enthält.  
  
```xml  
<set-variable name="IsMobile" value="@(context.Request.Headers["User-Agent"].Contains("iPad") || context.Request.Headers["User-Agent"].Contains("iPhone"))" />  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|set-variable|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|  
|---------------|-----------------|--------------|  
|Name|Der Name der Variablen.|Ja|  
|value|Der Wert der Variablen. Dies kann ein Ausdruck oder ein Literalwert sein.|Ja|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
###  <a name="set-variableAllowedTypes"></a> Zulässige Typen  
 Ausdrücke, die in der `set-variable`-Richtlinie verwendet werden, müssen einen der folgenden einfachen Typen zurückgeben.  
  
-   System.Boolean  
  
-   System.SByte  
  
-   System.Byte  
  
-   System.UInt16  
  
-   System.UInt32  
  
-   System.UInt64  
  
-   System.Int16  
  
-   System. Int32  
  
-   System.Int64  
  
-   System.Decimal  
  
-   System.Single  
  
-   System.Double  
  
-   System.Guid  
  
-   System.String  
  
-   System.Char  
  
-   System.DateTime  
  
-   System.TimeSpan  
  
-   System.Byte?  
  
-   System.UInt16?  
  
-   System.UInt32?  
  
-   System.UInt64?  
  
-   System.Int16?  
  
-   System.Int32?  
  
-   System.Int64?  
  
-   System.Decimal?  
  
-   System.Single?  
  
-   System.Double?  
  
-   System.Guid?  
  
-   System.String?  
  
-   System.Char?  
  
-   System.DateTime?  

##  <a name="Trace"></a> Ablaufverfolgung  
 Mit der `trace`-Richtlinie wird der Ausgabe für den [API-Inspektor](https://azure.microsoft.com/en-us/documentation/articles/api-management-howto-api-inspector/) eine Zeichenfolge hinzugefügt. Die Richtlinie wird nur ausgeführt, wenn die Ablaufverfolgung ausgelöst wird, d.h. der `Ocp-Apim-Trace`-Anforderungsheader ist vorhanden und auf `true` festgelegt, und der `Ocp-Apim-Subscription-Key`-Anforderungsheader ist vorhanden und enthält einen gültigen Schlüssel, der dem Administratorkonto zugeordnet ist.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
  
<trace source="arbitrary string literal"/>  
    <!-- string expression or literal -->  
</trace>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|Ablaufverfolgung|Stammelement|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|Quelle|Das Zeichenfolgenliteral ist für die Ablaufverfolgungsanzeige aussagekräftig und gibt die Quelle der Nachricht an.|Ja|N/V|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend, on-error  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
##  <a name="Wait"></a> Warten  
 Bei der `wait`-Richtlinie werden die unmittelbar untergeordneten Richtlinien parallel ausgeführt, und es wird gewartet, bis entweder alle oder keine unmittelbar untergeordneten Richtlinien abgeschlossen sind, bevor sie selbst abgeschlossen wird. Die wait-Richtlinie kann [Send request](api-management-advanced-policies.md#SendRequest) (Anforderung senden), [Get value from cache](api-management-caching-policies.md#GetFromCacheByKey) (Wert aus dem Cache abrufen) und [Control flow](api-management-advanced-policies.md#choose) (Ablaufsteuerung) als unmittelbar untergeordnete Richtlinien enthalten.  
  
### <a name="policy-statement"></a>Richtlinienanweisung  
  
```xml  
<wait for="all|any">  
  <!--Wait policy can contain send-request, cache-lookup-value,   
        and choose policies as child elements -->  
</wait>  
  
```  
  
### <a name="example"></a>Beispiel  
 Im folgenden Beispiel sind zwei `choose`-Richtlinien als unmittelbar untergeordnete Richtlinien der `wait`-Richtlinie vorhanden. Jede dieser `choose`-Richtlinien wird parallel ausgeführt. Jede `choose`-Richtlinie versucht, einen zwischengespeicherten Wert abzurufen. Bei Auftreten eines Cachefehlers wird ein Back-End-Dienst aufgerufen, um den Wert bereitzustellen. In diesem Beispiel wird die `wait`-Richtlinie nicht abgeschlossen, bis alle unmittelbar untergeordneten Richtlinien abgeschlossen sind, da das `for`-Attribut auf `all` festgelegt ist.   In diesem Beispiel werden die Kontextvariablen (`execute-branch-one`, `value-one`, `execute-branch-two` und `value-two`) außerhalb des Bereichs dieser Beispielrichtlinie deklariert.  
  
```xml  
<wait for="all">  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-one="])">  
      <cache-lookup-value key="key-one" variable-name="value-one" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-one="))">  
          <send-request mode="new" response-variable-name="value-one">  
            <set-url>https://backend-one</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
  <choose>  
    <when condition="@((bool)context.Variables["execute-branch-two="])">  
      <cache-lookup-value key="key-two" variable-name="value-two" />  
      <choose>  
        <when condition="@(!context.Variables.ContainsKey("value-two="))">  
          <send-request mode="new" response-variable-name="value-two">  
            <set-url>https://backend-two</set-url>  
            <set-method>GET</set-method>  
          </send-request>  
        </when>  
      </choose>  
    </when>  
  </choose>  
</wait>  
  
```  
  
### <a name="elements"></a>Elemente  
  
|Element|Beschreibung|Erforderlich|  
|-------------|-----------------|--------------|  
|wait|Stammelement Darf nur die Richtlinien `send-request`, `cache-lookup-value` und `choose` als untergeordnete Elemente enthalten.|Ja|  
  
### <a name="attributes"></a>Attribute  
  
|Attribut|Beschreibung|Erforderlich|Standard|  
|---------------|-----------------|--------------|-------------|  
|for|Bestimmt, ob die `wait`-Richtlinie wartet, bis alle unmittelbar untergeordneten Richtlinien abgeschlossen sind, oder nur eine. Zulässige Werte sind:<br /><br /> -   `all`: Es wird gewartet, bis alle unmittelbar untergeordneten Richtlinien abgeschlossen sind.<br />-   any: Es wird gewartet, bis eine beliebige unmittelbar untergeordnete Richtlinie abgeschlossen ist. Nachdem die erste unmittelbar untergeordnete Richtlinie abgeschlossen wurde, wird die `wait`-Richtlinie abgeschlossen, und die Ausführung aller anderen unmittelbar untergeordneten Richtlinien wird beendet.|Nein|alle|  
  
### <a name="usage"></a>Verwendung  
 Diese Richtlinie kann in den folgenden [Abschnitten](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#sections) und [Bereichen](http://azure.microsoft.com/documentation/articles/api-management-howto-policies/#scopes) von Richtlinien verwendet werden.  
  
-   **Richtlinienabschnitte:** inbound, outbound, backend  
  
-   **Richtlinienbereiche:** alle Bereiche  
  
## <a name="next-steps"></a>Nächste Schritte
Weitere Informationen zur Verwendung von Richtlinien finden Sie unter:
-   [Richtlinien in Azure API Management](api-management-howto-policies.md) 
-   [Richtlinienausdrücke](api-management-policy-expressions.md)
