---
title: Kopieren von Daten aus Dynamics CRM und 365 mithilfe von Azure Data Factory | Microsoft-Dokumentation
description: "Erfahren Sie, wie Daten aus Dynamics CRM und 365 mithilfe einer Kopieraktivität in eine Azure Data Factory-Pipeline in unterstützte Senkendatenspeicher kopiert werden."
services: data-factory
documentationcenter: 
author: linda33wj
manager: jhubbard
editor: spelluru
ms.service: data-factory
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 08/10/2017
ms.author: jingwang
ms.openlocfilehash: 74a4fca2ff5a0f913b5cb195f42482339e6200cb
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="copy-data-from-dynamics-365dynamics-crm-using-azure-data-factory"></a>Kopieren von Daten aus Dynamics 365 bzw. Dynamics CRM mithilfe von Azure Data Factory
In diesem Artikel wird beschrieben, wie Sie die Kopieraktivität in Azure Data Factory verwenden, um Daten aus Dynamics 365 bzw. Dynamics CRM zu kopieren. Er baut auf dem Artikel zur [Übersicht über die Kopieraktivität](copy-activity-overview.md) auf, der eine allgemeine Übersicht über die Kopieraktivität enthält.

## <a name="supported-scenarios"></a>Unterstützte Szenarien

Sie können Daten aus Dynamics 365 bzw. Dynamics CRM in beliebige unterstützte Senkendatenspeicher kopieren. Eine Liste der Datenspeicher, die als Quellen oder Senken für die Kopieraktivität unterstützt werden, finden Sie in der Tabelle [Unterstützte Datenspeicher](copy-activity-overview.md#supported-data-stores-and-formats).

Der Dynamics-Connector unterstützt insbesondere folgende Dynamics-Versionen und Authentifizierungstypen:

| Dynamics-Versionen | Authentifizierungstypen | Beispiele für verknüpfte Dienste |
|:--- |:--- |:--- |
| Dynamics 365 (online) <br> Dynamics CRM Online | Office 365 | [Dynamics Online- und Office 365-Authentifizierung](#dynamics-365-and-dynamics-crm-online) |
| Dynamics 365 lokal mit IFD <br> Dynamics CRM 2016 lokal mit IFD <br> Dynamics CRM 2015 lokal mit IFD | IFD | [Dynamics lokal mit IFD und IFD-Authentifizierung](#dynamics-365-and-dynamics-crm-on-premises-with-ifd) |

*IFD ist das Akronym für „Internet Facing Deployment“ (Bereitstellung mit Internetzugriff).*

> [!NOTE]
> Um den Dynamics-Connector zu verwenden, speichern Sie Ihr Kennwort in Azure Key Vault, und sorgen Sie dafür, dass die ADF-Kopieraktivität beim Kopieren der Daten dort abgerufen wird. Informationen zur Konfiguration finden Sie im Abschnitt [Eigenschaften verknüpfter Dienste](#linked-service-properties).

## <a name="getting-started"></a>Erste Schritte
Sie können mit dem .NET SDK, Python SDK, Azure PowerShell, der REST-API oder der Azure Resource Manager-Vorlage eine Pipeline mit einer Kopieraktivität erstellen. Im [Tutorial zur Kopieraktivität](quickstart-create-data-factory-dot-net.md) finden Sie detaillierte Anweisungen, wie Sie eine Pipeline mit einer Kopieraktivität erstellen können.

Die folgenden Abschnitte enthalten Details zu Eigenschaften, die zum Definieren von Data Factory-Entitäten speziell für Dynamics verwendet werden.

## <a name="linked-service-properties"></a>Eigenschaften des verknüpften Diensts

Folgende Eigenschaften werden für den mit Dynamics verknüpften Dienst unterstützt:

### <a name="dynamics-365-and-dynamics-crm-online"></a>Dynamics 365 und Dynamics CRM Online

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **Dynamics** festgelegt werden. | Ja |
| deploymentType | Der Bereitstellungstyp der Dynamics-Instanz. Muss für Dynamics Online **Online** lauten. | Ja |
| organizationName | Der Organisationsname der Dynamics-Instanz. | Nein. Er sollte angegeben werden, wenn dem Benutzer mehr als eine Dynamics-Instanz zugeordnet ist. |
| authenticationType | Der Authentifizierungstyp für die Verbindungsherstellung mit dem Dynamics-Server. Geben Sie für Dynamics Online **Office 365** an. | Ja |
| username | Geben Sie einen Benutzernamen für das Herstellen der Verbindung mit Dynamics an. | Ja |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. Sie müssen das Kennwort zu Azure Key Vault hinzufügen und als „AzureKeyVaultSecret“ konfigurieren. Weitere Informationen finden Sie im Artikel [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). | Ja |

**Beispiel: Dynamics Online mithilfe der Office 365-Authentifizierung**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics online linked service using Office365 authentication",
        "typeProperties": {
            "deploymentType": "Online",
            "organizationName": "orga02d9c75",
            "authenticationType": "Office365",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

### <a name="dynamics-365-and-dynamics-crm-on-premises-with-ifd"></a>Dynamics 365 und Dynamics CRM lokal mit IFD

*Die zusätzlichen Eigenschaften im Vergleich zu Dynamics Online lauten „hostName“ und „Port“.*

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **Dynamics** festgelegt werden. | Ja |
| deploymentType | Der Bereitstellungstyp der Dynamics-Instanz. Muss für Dynamics lokal mit IFD **OnPremisesWithIfd** lauten.| Ja |
| **hostName** | Der Hostname des lokalen Dynamics-Servers. | Ja |
| **port** | Der Port des lokalen Dynamics-Servers. | Nein (Standard = 443) |
| organizationName | Der Organisationsname der Dynamics-Instanz. | Ja |
| authenticationType | Der Authentifizierungstyp für die Verbindungsherstellung mit dem Dynamics-Server. Geben Sie für Dynamics lokal mit IFD **Ifd** an. | Ja |
| username | Geben Sie einen Benutzernamen für das Herstellen der Verbindung mit Dynamics an. | Ja |
| password | Geben Sie das Kennwort für das Benutzerkonto an, das Sie für den Benutzernamen angegeben haben. Hinweis: Sie müssen das Kennwort zu Azure Key Vault hinzufügen und als „AzureKeyVaultSecret“ konfigurieren. Weitere Informationen finden Sie im Artikel [Speichern von Anmeldeinformationen in Key Vault](store-credentials-in-key-vault.md). | Ja |

**Beispiel: Dynamics lokal mit IFD mit IFD-Authentifizierung**

```json
{
    "name": "DynamicsLinkedService",
    "properties": {
        "type": "Dynamics",
        "description": "Dynamics on-premises with IFD linked service using IFD authentication",
        "typeProperties": {
            "deploymentType": "OnPremisesWithIFD",
            "hostName": "contosodynamicsserver.contoso.com",
            "port": 443,
            "organizationName": "admsDynamicsTest",
            "authenticationType": "Ifd",
            "username": "test@contoso.onmicrosoft.com",
            "password": {
                "type": "AzureKeyVaultSecret",
                "secretName": "mySecret",
                "store":{
                    "linkedServiceName": "<Azure Key Vault linked service>",
                    "type": "LinkedServiceReference"
                }
            }
        }
    }
}
```

## <a name="dataset-properties"></a>Dataset-Eigenschaften

Eine vollständige Liste mit den Abschnitten und Eigenschaften, die zum Definieren von Datasets zur Verfügung stehen, finden Sie im Artikel zu [Datasets](concepts-datasets-linked-services.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die vom Dynamics-Dataset unterstützt werden.

Legen Sie zum Kopieren von Daten aus Dynamics die type-Eigenschaft des Datasets auf **DynamicsEntity** fest. Folgende Eigenschaften werden unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft des Datasets muss auf **DynamicsEntity** festgelegt werden. |Ja |
| entityName | Der logische Name der abzurufenden Entität. | Nein (wenn „query“ in der Aktivitätsquelle angegeben ist) |

> [!IMPORTANT]
> **Der Abschnitt „structure“ im Dataset ist für Dynamics erforderlich.** Er definiert den Spaltennamen und Datentyp für Dynamics-Daten, die kopiert werden sollen. Erfahren Sie mehr über die [Datasetstruktur](concepts-datasets-linked-services.md#dataset-structure) und [Datentypzuordnung für Dynamics](#data-type-mapping-for-dynamics).

**Beispiel:**

```json
{
    "name": "DynamicsDataset",
    "properties": {
        "type": "DynamicsEntity",
        "structure": [
            {
                "name": "accountid",
                "type": "Guid"
            },
            {
                "name": "name",
                "type": "String"
            },
            {
                "name": "marketingonly",
                "type": "Boolean"
            },
            {
                "name": "modifiedon",
                "type": "Datetime"
            }
        ],
        "typePoperties": {
            "entityName": "account"
        },
        "linkedServiceName": {
            "referenceName": "<Dynamics linked service name>",
            "type": "linkedservicereference"
        }
    }
}
```

## <a name="copy-activity-properties"></a>Eigenschaften der Kopieraktivität

Eine vollständige Liste mit den Abschnitten und Eigenschaften zum Definieren von Aktivitäten finden Sie im Artikel [Pipelines](concepts-pipelines-activities.md). Dieser Abschnitt enthält eine Liste der Eigenschaften, die von der Dynamics-Quelle unterstützt werden.

### <a name="dynamics-as-source"></a>Dynamics als Quelle

Legen Sie zum Kopieren von Daten aus Dynamics den Quelltyp in der Kopieraktivität auf **DynamicsSource** fest. Folgende Eigenschaften werden im Abschnitt **source** der Kopieraktivität unterstützt:

| Eigenschaft | Beschreibung | Erforderlich |
|:--- |:--- |:--- |
| Typ | Die type-Eigenschaft muss auf **DynamicsSource** festgelegt werden.  | Ja |
| query  | FetchXML ist eine proprietäre Abfragesprache, die in Microsoft Dynamics (online und lokal) verwendet wird. Sehen Sie sich das folgende Beispiel an, und erfahren Sie mehr über das [Erstellen von Abfragen mit FetchXML](https://msdn.microsoft.com/en-us/library/gg328332.aspx). | Nein (wenn „entityName“ im Dataset angegeben ist)  |

**Beispiel:**

```json
"activities":[
    {
        "name": "CopyFromDynamics",
        "type": "Copy",
        "inputs": [
            {
                "referenceName": "<Dynamics input dataset>",
                "type": "DatasetReference"
            }
        ],
        "outputs": [
            {
                "referenceName": "<output dataset>",
                "type": "DatasetReference"
            }
        ],
        "typeProperties": {
            "source": {
                "type": "DynamicsSource",
                "query": "<FetchXML Query>"
            },
            "sink": {
                "type": "<sink type>"
            }
        }
    }
]
```

### <a name="sample-fetchxml-query"></a>FetchXML-Beispielabfrage

```xml
<fetch>
  <entity name="account">
    <attribute name="accountid" />
    <attribute name="name" />
    <attribute name="marketingonly" />
    <attribute name="modifiedon" />
    <order attribute="modifiedon" descending="false" />
    <filter type="and">
      <condition attribute ="modifiedon" operator="between">
        <value>2017-03-10 18:40:00z</value>
        <value>2017-03-12 20:40:00z</value>
      </condition>
    </filter>
  </entity>
</fetch>
```

## <a name="data-type-mapping-for-dynamics"></a>Datentypzuordnung für Dynamics

Beim Kopieren von Daten aus Dynamics werden die folgenden Zuordnungen von Dynamics-Datentypen zu Azure Data Factory-Zwischendatentypen verwendet. Unter [Schema- und Datentypzuordnungen](copy-activity-schema-and-type-mapping.md) erfahren Sie, wie Sie Aktivitätszuordnungen für Quellschema und Datentyp in die Senke kopieren.

Konfigurieren Sie anhand der folgenden Zuordnungstabelle den entsprechenden ADF-Datentyp in der Datasetstruktur auf Grundlage des Dynamics-Quelldatentyps:

| Dynamics-Datentyp | Data Factory-Zwischendatentyp |
|:--- |:--- |
| AttributeTypeCode.BigInt | Long |
| AttributeTypeCode.Boolean | Boolean |
| AttributeType.Customer | Guid |
| AttributeType.DateTime | Datetime |
| AttributeType.Decimal | Decimal |
| AttributeType.Double | Doppelt |
| AttributeType.EntityName | String |
| AttributeType.Integer | Int32 |
| AttributeType.Lookup | Guid |
| AttributeType.ManagedProperty | Boolean |
| AttributeType.Memo | String |
| AttributeType.Money | Decimal |
| AttributeType.Owner | Guid |
| AttributeType.Picklist | Int32 |
| AttributeType.Uniqueidentifier | Guid |
| AttributeType.String | String |
| AttributeType.State | Int32 |
| AttributeType.Status | Int32 |

> [!NOTE]
> Die Dynamics-Datentypen „AttributeType.CalendarRules“ und „AttributeType.PartyList“ werden nicht unterstützt.


## <a name="next-steps"></a>Nächste Schritte
Eine Liste der Datenspeicher, die als Quellen und Senken für die Kopieraktivität in Azure Data Factory unterstützt werden, finden Sie unter [Unterstützte Datenspeicher](copy-activity-overview.md##supported-data-stores-and-formats).