---
title: "Power BI-Dashboard für Fahrzeugzustand und Fahrgewohnheiten – Azure | Microsoft-Dokumentation"
description: "Verwenden Sie die Funktionen von Cortana Intelligence zur Echtzeitgewinnung von prädiktiven Einblicken in Fahrzeugzustand und Fahrgewohnheiten."
services: machine-learning
documentationcenter: 
author: bradsev
manager: jhubbard
editor: cgronlun
ms.assetid: aaeb29a5-4a13-4eab-bbf1-885690d86c56
ms.service: machine-learning
ms.workload: data-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/16/2016
ms.author: bradsev
ms.openlocfilehash: 39be936520d62cb1c1c28de9bd72f8f489166082
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="vehicle-telemetry-analytics-solution-template-power-bi-dashboard-setup-instructions"></a>Lösungsvorlage „Analyse von Telemetriedaten für Fahrzeuge“ – Anweisungen zur Einrichtung des Power BI-Dashboards
Dieses **Menü** bietet Links zu den Kapiteln dieses Playbooks. 

[!INCLUDE [cap-vehicle-telemetry-playbook-selector](../../../includes/cap-vehicle-telemetry-playbook-selector.md)]

Die Lösung für Vehicle Telemetry Analytics (Fahrzeugtelemetrieanalyse) zeigt, wie Automobilhändler, Automobilhersteller und Versicherungsgesellschaften die Fähigkeiten von Cortana Intelligence nutzen können, um sich in Echtzeit prädiktive Einblicke zu Fahrzeugstand und Fahrgewohnheiten zu verschaffen. So können in den Bereichen Kundenerlebnis, Forschung und Entwicklung sowie Marketing Verbesserungen vorangetrieben werden. Dieses Dokument enthält schrittweise Anleitungen, wie Sie Power BI-Berichte und das Dashboard konfigurieren können, nachdem die Lösung in Ihrem Abonnement bereitgestellt wurde. 

## <a name="prerequisites"></a>Voraussetzungen
1. Bereitstellen der Lösung für die [Analyse von Telemetriedaten](https://gallery.cortanaintelligence.com/Solution/5bdb23f3abb448268b7402ab8907cc90)  
2. [Installieren Sie Microsoft Power BI Desktop.](http://www.microsoft.com/download/details.aspx?id=45331)
3. Sie benötigen ein [Azure-Abonnement](https://azure.microsoft.com/pricing/free-trial/). Wenn Sie kein Abonnement haben, steigen Sie mit dem kostenlosen Azure-Abonnement ein.
4. Microsoft Power BI-Konto

## <a name="cortana-intelligence-suite-components"></a>Komponenten der Cortana Intelligence Suite
Die nachstehenden Cortana Intelligence-Dienste werden als Teil der Lösungsvorlage für Fahrzeugtelemetrieanalyse (Vehicle Telemetry Analytics) in Ihrem Abonnement bereitgestellt.

* **Event Hub** für das Erfassen von Millionen von Telemetrie-Ereignissen in Azure.
* **Stream Analytics** für das Gewinnen von Einblicken in Echtzeit hinsichtlich des Fahrzeugzustands und Speichern dieser Daten in beständigem Speicher zur umfangreicheren Batchanalyse.
* **Machine Learning** für die Erkennung von Anomalien in Echtzeit und Batchverarbeitung zum Gewinnen prädiktiver Einblicke.
* **HDInsight** wird verwendet, um Daten nach Maß zu transformieren.
* **Data Factory** übernimmt die Orchestrierung, Planung, Ressourcenverwaltung und Überwachung der Batchverarbeitungs-Pipeline.

**Power BI** bietet dieser Lösung ein umfassendes Dashboard für Visualisierungen von Echtzeitdaten und Predictive Analytics. 

Die Lösung verwendet zwei Datenquellen: **Dataset mit simulierten Fahrzeugsignalen** und Diagnosedataset **und** Fahrzeugkatalog.

Zu dieser Lösung gehört ein Simulator für Fahrzeugtelematik. Diese Anwendung gibt Diagnosedaten und Signale entsprechend dem Zustand des Fahrzeugs und Fahrmuster zu einem bestimmten Zeitpunkt aus. 

Der Fahrzeugkatalog ist ein Referenzdataset mit der Fahrgestellnummer (VIN) zur Modellierung der Zuordnung.

## <a name="power-bi-dashboard-preparation"></a>Vorbereitung des Power BI-Dashboards
### <a name="setup-power-bi-real-time-dashboard"></a>Einrichten des Power BI-Echtzeitdashboards

**Starten der Echtzeit-Dashboardanwendung** Nach der Bereitstellung sollten Sie die Anweisungen für den manuellen Betrieb befolgen.

* Laden Sie die Echtzeit-Dashboardanwendung „RealtimeDashboardApp.zip“ herunter, und entzippen Sie sie.
*  Öffnen Sie im entzippten Ordner die App-Konfigurationsdatei „RealtimeDashboardApp.exe.config“, ersetzen Sie „appSettings“ für Event Hub, Blob Storage und ML-Dienstverbindungen durch die Werte in den Anweisungen für den manuellen Betrieb, und speichern Sie die Änderungen.
* Führen Sie die Anwendung „RealtimeDashboardApp.exe“ aus. Ein Anmeldefenster wird angezeigt. Geben Sie Ihre gültigen Power BI-Anmeldeinformationen ein, und klicken Sie auf die Schaltfläche **Accept** (Akzeptieren). Anschließend wird die App ausgeführt.

   ![Anmelden bei Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/5-sign-into-powerbi.png)
   
   ![Power BI-Dashboardberechtigungen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-powerbi-dashboard-permissions.png)

* Melden Sie sich bei der Power BI-Website an, und erstellen Sie das Echtzeitdashboard.

Nun können Sie das Power BI-Dashboard mit umfassenden Visualisierungen konfigurieren, um sich in Echtzeit prädiktive Einblicke zu Fahrzeugzustand und Fahrgewohnheiten zu verschaffen. Es dauert ca. 45-60 Minuten, um alle Berichte zu erstellen und das Dashboard zu konfigurieren. 

### <a name="configure-power-bi-reports"></a>Konfigurieren von Power BI-Berichten
Die Erstellung der Echtzeitberichte und des Dashboards dauert ca. 30 bis 45 Minuten. Navigieren Sie zu [http://powerbi.com](http://powerbi.com), und melden Sie sich an.

![Anmelden bei Power BI](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/6-1-powerbi-signin.png)

Ein neues Dataset wird in Power BI generiert. Klicken Sie auf das Dataset **ConnectedCarsRealtime** .

![Ausgewähltes ConnectedCarsRealtime-Dataset](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/7-select-connected-cars-realtime-dataset.png)

Speichern Sie den leeren Bericht durch Drücken von **STRG+S**.

![Speichern eines leeren Berichts](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/8-save-blank-report.png)

Geben Sie den Berichtsnamen *Vehicle Telemetry Analytics Real Time – Reports*(Fahrzeugtelemetrieanalyse in Echtzeit – Berichte) ein.

![Angeben eines Berichtsnamens](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/9-provide-report-name.png)

## <a name="real-time-reports"></a>Echtzeitberichte
Diese Lösung enthält drei Echtzeitberichte:

1. Vehicles in operation (Fahrzeuge im Einsatz)
2. Vehicles Requiring Maintenance (Fahrzeuge, für die die Wartung erforderlich ist)
3. Vehicles Health Statistics (Fahrzeugzustandsstatistik)

Sie können alle drei Echtzeitberichte konfigurieren oder nach jeder Phase stoppen und mit dem nächsten Abschnitt der Konfiguration von Batchberichten fortfahren. Wir empfehlen Ihnen, alle drei Berichte zu erstellen, um den Echtzeitpfad der Lösung in vollem Umfang zu visualisieren.  

### <a name="1-vehicles-in-operation"></a>1. Vehicles in operation (Fahrzeuge im Einsatz)
Doppelklicken Sie auf **Seite 1**, und benennen Sie sie in „Vehicles in operation“ (Fahrzeuge im Einsatz) um.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Vehicles in operation (Fahrzeuge im Einsatz)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4a.png)  

Wählen Sie unter **Felder** das Feld **vin** und als Visualisierungstyp **Karte** aus.  

Die Kartenvisualisierung erfolgt wie in der Abbildung gezeigt.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Wählen Sie „vin“ aus](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4b.png)

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.  

Wählen Sie unter „Felder“ **City** und **vin** aus. Ändern Sie die Visualisierung in **Zuordnung**. Ziehen Sie **vin** in den Bereich „Values“. Ziehen Sie **City** aus „Felder“ in den Bereich **Legende**.   
    ![Connected Cars (Fahrzeuge im Netzwerk) – Kartenvisualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4c.png)

Wählen Sie unter **Visualizations**(Visualisierungen) den Abschnitt **Format** aus. Klicken Sie auf **Titel**, und ändern Sie den **Text** in **Vehicles in operation by city** (Fahrzeuge im Einsatz nach Stadt).  
    ![Connected Cars - Vehicles in operation by city](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4d.png)   

Die endgültige Visualisierung sieht wie in dieser Abbildung aus.    
    ![Connected Cars (Fahrzeuge im Netzwerk) – Endgültige Visualisierung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4e.png)

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.  

Wählen Sie **City** und **vin** aus, und ändern Sie den Visualisierungstyp in **Gruppiertes Säulendiagramm**. Stellen Sie sicher, dass das Feld **City** im Bereich **Achse** und **vin** im Bereich **Wert** enthalten ist.  

Sortieren Sie das Diagramm nach **Count of vin**.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Count of vin](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4f.png)  

Ändern Sie den **Titel** des Diagramms in **Vehicles in operation by city** (Fahrzeuge im Einsatz nach Stadt).  

Klicken Sie auf den Abschnitt **Format**, und wählen Sie **Datenfarben** aus. Klicken Sie auf **Ein**, um die Option **Alle anzeigen** zu aktivieren.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Alle Datenfarben zeigen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4g.png)  

Ändern Sie die Farbe der einzelnen Städte durch Klicken auf das Farbsymbol.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Farben ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4h.png)  

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.  

Wählen Sie unter „Visualisierungen“ **Gruppiertes Säulendiagramm** aus. Ziehen Sie das Feld **city** in den Bereich **Achse**, **Model** in den Bereich **Legende** und **vin** in den Bereich **Wert**.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Gruppiertes Säulendiagramm](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4i.png)  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Rendering](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4j.png)

Ordnen Sie alle Visualisierungen wie in der Abbildung gezeigt an.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Visualisierungen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4k.png)

Sie haben den Echtzeitbericht „Vehicles in operation“ (Fahrzeuge im Einsatz) erfolgreich konfiguriert. Sie können fortfahren, um den nächsten Echtzeitbericht zu erstellen, oder hier aufhören und das Dashboard konfigurieren. 

### <a name="2-vehicles-requiring-maintenance"></a>2. Vehicles Requiring Maintenance (Fahrzeuge, für die die Wartung erforderlich ist)
Klicken Sie auf ![Hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png), um einen neuen Bericht hinzuzufügen. Benennen Sie ihn in **Vehicles Requiring Maintenance** (Fahrzeuge, für die die Wartung erforderlich ist) um.

![Connected Cars (Fahrzeuge im Netzwerk) – Vehicles Requiring Maintenance (Fahrzeuge, für die die Wartung erforderlich ist)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4l.png)  

Wählen Sie das Feld **vin** aus, und ändern Sie den Visualisierungstyp in **Karte**.  
    ![Connected Cars (Fahrzeuge im Netzwerk) – Kartenvisualisierung für „vin“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4m.png)  

Das Dataset enthält das Feld „MaintenanceLabel“. Dieses Feld kann den Wert „0“ oder „1“ haben. Er wurde vom Azure Machine Learning-Modell festgelegt, das als Teil der Lösung bereitgestellt und in den Echtzeitpfad integriert wurde. Der Wert „1“ gibt an, dass ein Fahrzeug gewartet werden muss. 

So wird der Filter **Seitenebene** hinzugefügt, um Daten von Fahrzeugen anzuzeigen, die gewartet werden müssen. 

1. Ziehen Sie das Feld **MaintenanceLabel** in **Filter auf Seitenebene**.  
   ![Connected Cars (Fahrzeuge im Netzwerk) – Filter auf Seitenebene](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n1.png)  
2. Klicken Sie auf das Menü **Einfaches Filtern** , das sich unten im Filter auf Seitenebene „MaintenanceLabel“ befindet.  
   ![Connected Cars (Fahrzeuge im Netzwerk) – Einfache Filterung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n2.png)  
3. Legen Sie den Filterwert auf **1**   fest.  
   ![Connected Cars (Fahrzeuge im Netzwerk) – Filterwert](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4n3.png)  

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.  

Wählen Sie unter „Visualisierungen“ **Gruppiertes Säulendiagramm** aus.  
![Connected Cars (Fahrzeuge im Netzwerk) – Kartenvisualisierung für „vin“](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4o.png)  
![Connected Cars (Fahrzeuge im Netzwerk) – Gruppiertes Säulendiagramm](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4p.png)

Ziehen Sie das Feld **Model** in den Bereich **Achse** und **Vin** in den Bereich **Wert**. Sortieren Sie die Visualisierung dann nach **Count of vin**.  Ändern Sie den **Titel** des Diagramms in **Vehicles requiring maintenance by model** (Fahrzeuge, für die die Wartung erforderlich ist, nach Modell).  

Ziehen Sie die Felder vom Typ **vin** in den Bereich **Farbsättigung** im Abschnitt **Felder**![Felder](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4field.png) der Registerkarte **Visualisierung**.  
![Connected Cars (Fahrzeuge im Netzwerk) – Farbsättigung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4q.png)  

Ändern Sie **Datenfarben** unter „Visualisierungen“ im Abschnitt **Format**.  
Ändern Sie „Minimumfarbe“ in **F2C812**.  
Ändern Sie „Maximumfarbe“ in **FF6300**.  
![Connected Cars (Fahrzeuge im Netzwerk) – Farbänderung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4r.png)  
![Connected Cars (Fahrzeuge im Netzwerk) – Neue Visualisierungsfarben](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4s.png)  

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.  

Wählen Sie unter „Visualisierungen“ **Gruppiertes Säulendiagramm** aus. Ziehen Sie das Feld **vin** in den Bereich **Wert** und **City** in den Bereich **Achse**. Sortieren Sie das Diagramm nach **Count of vin**. Ändern Sie den **Titel** des Diagramms in **Vehicles requiring maintenance by city** (Fahrzeuge, für die die Wartung erforderlich ist, nach Stadt).   
![Connected Cars (Fahrzeuge im Netzwerk) – Vehicles requiring maintenance by city (Fahrzeuge, für die die Wartung erforderlich ist, nach Stadt)](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4t.png)  

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.  

Wählen Sie unter „Visualisierungen“ die Option **Mehrzeilige Zuordnung**. Ziehen Sie **Model** und **vin** in den Bereich **Felder**.  
![Connected Cars (Fahrzeuge im Netzwerk) – Mehrzeilige Zuordnung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4u.png)    

Nach der Neuanordnung aller Visualisierungen sieht der endgültige Bericht wie folgt aus:   
![Connected Cars (Fahrzeuge im Netzwerk) – Mehrzeilige Zuordnung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4v.png)  

Sie haben den Echtzeitbericht „Vehicles Requiring Maintenance“ (Fahrzeuge, für die die Wartung erforderlich ist) erfolgreich konfiguriert. Sie können fortfahren, um den nächsten Echtzeitbericht zu erstellen, oder hier aufhören und das Dashboard konfigurieren. 

### <a name="3-vehicles-health-statistics"></a>3. Vehicles Health Statistics (Fahrzeugzustandsstatistik)
Klicken Sie auf ![Hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4add.png), um einen neuen Bericht hinzuzufügen. Benennen Sie ihn in **Vehicles Health Statistics** (Fahrzeugzustandsstatistik) um.  

Wählen Sie **Maßstab** in „Visualisierungen“ aus. Ziehen Sie dann das Feld **Geschwindigkeit** in die Bereiche **Wert, Minimumwert, Maximumwert**.  
![Connected Cars (Fahrzeuge im Netzwerk) – Mehrzeilige Zuordnung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4w.png)  

Ändern Sie die Standardaggregation **Geschwindigkeit** im Bereich **Wert** in **Durchschnitt**. 

Ändern Sie die Standardaggregation **Geschwindigkeit** im Bereich **Minimum** in **Minimum**.

Ändern Sie die Standardaggregation **Geschwindigkeit** im Bereich **Maximum** in **Maximum**.

![Connected Cars (Fahrzeuge im Netzwerk) – Mehrzeilige Zuordnung](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4x.png)  

Benennen Sie den Titel von **Maßstab** in **Average speed** um. 

![Connected Cars (Fahrzeuge im Netzwerk) – Maßstab](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4y.png)  

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.  

Fügen Sie auf ähnliche Weise einen **Maßstab** für **Average engine oil**, **Average fuel** und **Average engine temperate** hinzu.  

Ändern Sie die Standardaggregation von Feldern in jedem Maßstab gemäß den für den Maßstab **Durchschnittsgeschwindigkeit** erfolgten Schritten.

![Connected Cars (Fahrzeuge im Netzwerk) – Maßstäbe](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4z.png)

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.

Wählen Sie **Linien- und gruppiertes Säulendiagramm** in „Visualisierungen“ aus. Ziehen Sie das Feld **City** in **Gemeinsame Achse**. Ziehen Sie die Felder **speed, tirepressure** und **engineoil** in den Bereich **Spaltenwerte**. Ändern Sie ihren Aggregationstyp in **Durchschnitt**. 

Ziehen Sie das Feld **engineTemperature** in den Bereich **Linienwerte**. Ändern Sie den Aggregationstyp in **Durchschnitt**. 

![Connected Cars (Fahrzeuge im Netzwerk) – Visualisierungsfelder](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4aa.png)

Ändern Sie den **Titel** des Diagramms in **Average speed, tire pressure, engine oil and engine temperature** (Durchschnitt für Geschwindigkeit, Reifendruck, Motoröl und Motortemperatur).  

![Connected Cars (Fahrzeuge im Netzwerk) – Visualisierungsfelder](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4bb.png)

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.

Wählen Sie **Treemap** in „Visualisierungen“ aus. Ziehen Sie das Feld **Model** in den Bereich **Gruppe** und das Feld **MaintenanceProbability** in den Bereich **Werte**.

Ändern Sie den **Titel** des Diagramms in **Vehicle models requiring maintenance** (Fahrzeugmodelle, für die Wartung erforderlich ist).

![Connected Cars (Fahrzeuge im Netzwerk) – Diagrammtitel ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4cc.png)

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.

Wählen Sie unter „Visualisierungen“ die Option **Gestapeltes Balkendiagramm (100 %)**. Ziehen Sie das Feld **city** in den Bereich **Achse** und die Felder **MaintenanceProbability** und **RecallProbability** in den Bereich **Wert**.

![Connected Cars (Fahrzeuge im Netzwerk) – Neue Visualisierung hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4dd.png)

Klicken Sie auf **Format**, wählen Sie **Datenfarben**, und ändern Sie die Farbe für **MaintenanceProbability** in den Wert **F2C80F**.

Ändern Sie den **Titel** des Diagramms in **Probability of Vehicle Maintenance & Recall by City** (Wahrscheinlichkeit für Fahrzeugwartung und -rückruf nach Ort).

![Connected Cars (Fahrzeuge im Netzwerk) – Neue Visualisierung hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ee.png)

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.

Wählen Sie **Flächendiagramm** in „Visualisierungen“ aus. Ziehen Sie das Feld **Model** in den Bereich **Achse** und die Felder **engineOil, tirepressure, speed und MaintenanceProbability** in den Bereich **Werte**. Ändern Sie ihren Aggregationstyp in **Durchschnitt**. 

![Connected Cars (Fahrzeuge im Netzwerk) – Aggregationstyp ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ff.png)

Ändern Sie den Titel des Diagramms in **Average engine oil, tire pressure, speed and maintenance probability by model**(Durchschnitt für Motoröl, Reifendruck, Geschwindigkeit und Wartungswahrscheinlichkeit nach Modell).

![Connected Cars (Fahrzeuge im Netzwerk) – Diagrammtitel ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4gg.png)

Klicken Sie auf den leeren Bereich, um eine neue Visualisierung hinzufügen.

1. Wählen Sie unter „Visualizations“ (Visualisierungen) die Visualisierung **Punktdiagramm** aus.
2. Ziehen Sie das Feld **Model** in die Bereiche **Details** und **Legende**.
3. Ziehen Sie das Feld **fuel** in den Bereich **X-Achse**. Ändern Sie die Aggregation in **Average** (Durchschnitt).
4. Ziehen Sie das Feld **engineTemparature** in den Bereich **Y-Achse**. Ändern Sie die Aggregation in **Average** (Durchschnitt).
5. Ziehen Sie das Feld **vin** in den Bereich **Größe**.

![Connected Cars (Fahrzeuge im Netzwerk) – Neue Visualisierung hinzufügen](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4hh.png)

Ändern Sie den **Titel** des Diagramms in **Averages of Fuel, Engine Temperature by Model** (Durchschnittswerte für Kraftstoff und Motortemperatur nach Modell).

![Connected Cars (Fahrzeuge im Netzwerk) – Diagrammtitel ändern](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4ii.png)

Der endgültige Bericht sieht wie folgt aus.

![Connected Cars (Fahrzeuge im Netzwerk) – Endgültiger Bericht](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.4jj.png)

### <a name="pin-visualizations-from-the-reports-to-the-real-time-dashboard"></a>Anheften von Visualisierungen der Berichte an das Echtzeitdashboard
Erstellen Sie ein leeres Dashboard. Klicken Sie dazu neben „Dashboards“ auf das Pluszeichen. Geben Sie ihm den Namen „Vehicle Telemetry Analytics Dashboard“.

![Connected Cars (Fahrzeuge im Netzwerk) – Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.5.png)

Heften Sie die Visualisierung aus den obigen Berichten an das Dashboard an. 

![Connected Cars (Fahrzeuge im Netzwerk) – Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-3.6.png)

Das Dashboard sieht wie unten dargestellt aus, wenn alle drei Berichte erstellt und die entsprechenden Visualisierungen an das Dashboard angeheftet werden. Ihr Dashboard sieht unter Umständen anders aus, wenn Sie nicht alle Berichte erstellt haben. 

![Connected Cars (Fahrzeuge im Netzwerk) – Dashboard](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/connected-cars-4.0.png)

Glückwunsch! Sie haben das Echtzeit-Dashboard erfolgreich erstellt. Wenn Sie „CarEventGenerator.exe“ und „RealtimeDashboardApp.exe“ weiter ausführen, sollten im Dashboard Live-Updates zu sehen sein. Es dauert ungefähr 10 bis 15 Minuten, bis die folgenden Schritte abgeschlossen sind.

## <a name="setup-power-bi-batch-processing-dashboard"></a>Einrichten des Power BI-Dashboards für die Batchverarbeitung
> [!NOTE]
> Es dauert ca. zwei Stunden (ab dem erfolgreichen Abschluss der Bereitstellung), bis die Pipeline für die End-to-End-Batchverarbeitung die Ausführung beendet und generierte Daten für den Zeitraum eines Jahres verarbeitet hat. Warten Sie daher auf den Abschluss der Verarbeitung, bevor Sie die nächsten Schritte ausführen. 
> 
> 

**Herunterladen der Power BI-Designer-Datei**

* Eine vorkonfigurierte Power BI-Designer-Datei ist in den Anweisungen für den manuellen Betrieb in der Bereitstellung enthalten.
* Suchen Sie nach „2. Einrichten des Power BI-Dashboards für die Batchverarbeitung“. Sie können hier die PowerBI-Vorlage mit dem Namen **ConnectedCarsPbiReport.pbix** für das Batchverarbeitungsdashboard herunterladen.
* Speichern Sie die Datei lokal.

**Konfigurieren von Power BI-Berichten**

* Öffnen Sie mit Power BI Desktop die Designer-Datei **ConnectedCarsPbiReport.pbix**. Falls noch nicht geschehen, können Sie Power BI Desktop unter [Microsoft Power BI Desktop](http://www.microsoft.com/download/details.aspx?id=45331) herunterladen und installieren. 
* Klicken Sie auf **Abfragen bearbeiten**.

![Bearbeiten einer Power BI-Abfrage](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/10-edit-powerbi-query.png)

* Doppelklicken Sie auf **Quelle**.

![Festlegen einer Power BI-Quelle](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/11-set-powerbi-source.png)

* Aktualisieren Sie die Serververbindungszeichenfolge mit dem Azure SQL-Server, der im Rahmen der Bereitstellung eingerichtet wurde.  Suchen Sie in den Anweisungen für den manuellen Betrieb unter 

    4. Azure SQL-Datenbank
    
    * Server: somethingsrv.database.windows.net
    * Datenbank: connectedcar
    * Benutzername: username
    * Kennwort: Sie können Ihr SQL Server-Kennwort über das Azure-Portal verwalten.

* Behalten Sie für **Datenbank** die Einstellung *connectedcar*bei.

![Festlegen einer Power BI-Datenbank](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/12-set-powerbi-database.png)

* Klicken Sie auf **OK**.
* Die Registerkarte **Windows-Anmeldeinformationen** ist standardmäßig ausgewählt. Wechseln Sie zu **Datenbank-Anmeldeinformationen**, indem Sie rechts auf die Registerkarte **Datenbank** klicken.
* Geben Sie den **Benutzernamen** und das **Kennwort** Ihrer Azure SQL-Datenbank ein, die beim Einrichten der Bereitstellung angegeben wurde.

![Angeben von Anmeldeinformationen für eine Datenbank](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/13-provide-database-credentials.png)

* Klicken Sie auf das Menü **Verbinden**
* Wiederholen Sie die vorherigen Schritte für jede der verbleibenden drei Abfragen im rechten Bereich, und aktualisieren Sie dann die Details der Verbindung mit der Datenquelle.
* Klicken Sie auf **Close and Load**(Schließen und laden). Power BI Desktop-Dateidatasets sind mit SQL Azure-Datenbanktabellen verbunden.
* **Schließen** Sie die Power BI Desktop-Datei.

![Schließen von Power BI Desktop](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/14-close-powerbi-desktop.png)

* Klicken Sie auf die Schaltfläche **Speichern** , um die Änderungen zu speichern. 

Sie haben jetzt alle Berichte konfiguriert, die dem Batchverarbeitungspfad in der Lösung entsprechen. 

## <a name="upload-to-powerbicom"></a>Hochladen auf *powerbi.com*
1. Navigieren Sie zum Power BI-Web-Portal unter http://powerbi.com, und melden Sie sich an.
2. Klicken Sie auf das Menü **Get Data**  
3. Laden Sie die Power BI Desktop-Datei hoch.  
4. Klicken Sie zum Hochladen auf **Get Data > Files Get > Local file** (Daten abrufen > Dateien abrufen > Lokale Datei).  
5. Navigieren Sie zur Datei **„**ConnectedCarsPbiReport.pbix**“**.  
6. Nachdem die Datei hochgeladen wurde, gelangen Sie wieder zu Ihrem Power BI-Arbeitsbereich.  

Ein Dataset, Bericht und leeres Dashboard werden für Sie erstellt.  

Heften Sie Diagramme an ein neues Dashboard mit dem Namen **Vehicle Telemetry Analytics Dashboard** in **Power BI** an. Klicken Sie auf das zuvor erstellte leere Dashboard. Navigieren Sie zum Abschnitt **Berichte**, und klicken Sie auf den zuvor hochgeladenen Bericht.  

![Vehicle Telemetry Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard1.png) 

**Der Bericht hat sechs Seiten:**  
Seite 1: Vehicle density  
Seite 2: Real time vehicle health  
Seite 3: Aggressively Driven Vehicles   
Seite 4: Recalled vehicles  
Seite 5: Fuel Efficiently Driven Vehicles  
Seite 6: Contoso-Logo  

![Connected Cars Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard2.png)

**Auf Seite 3**heften Sie Folgendes an:  

1. Count of vin  
   ![Connected Cars Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard3.png) 
2. Aggressively driven vehicles by model: Wasserfalldiagramm   
   ![Vehicle Telemetry – Anheften von Diagramm 4](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard4.png)

**Auf Seite 5**heften Sie Folgendes an: 

1. Count of vin    
   ![Vehicle Telemetry – Anheften von Diagramm 5](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard5.png)  
2. Fuel efficient vehicles by model (Kraftstoffeffiziente Fahrzeuge nach Modell): Gruppiertes Säulendiagramm   
   ![Vehicle Telemetry – Anheften von Diagramm 6](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard6.png)

**Auf Seite 4**heften Sie Folgendes an:  

1. Count of vin  
   ![Vehicle Telemetry – Anheften von Diagramm 7](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard7.png) 
2. Recalled vehicles by city, model (Zurückgerufene Fahrzeuge nach Ort, Modell): Treemap   
   ![Vehicle Telemetry – Anheften von Diagramm 8](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard8.png)  

**Auf Seite 6**heften Sie Folgendes an:  

1. Contoso Motors-Logo   
   ![Vehicle Telemetry – Anheften von Diagramm 9](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard9.png)

**Organisieren des Dashboards**  

1. Navigieren Sie zum Dashboard.
2. Zeigen Sie auf jedes Diagramm, und benennen Sie es basierend auf der Benennung in der nachstehenden Abbildung des vollständigen Dashboards. Ordnen Sie außerdem die Diagramme so an, dass sie wie im nachstehenden Dashboard aussehen.  
   ![Vehicle Telemetry – Organisieren von Dashboard 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard2.png)  
   ![Vehicle Telemetry Power BI.com](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-dashboard.png)
3. Wenn Sie alle Berichte wie in diesem Dokument beschrieben erstellt haben, sollte das endgültige abgeschlossene Dashboard wie in der folgenden Abbildung aussehen: 

![Vehicle Telemetry – Organisieren von Dashboard 2](./media/cortana-analytics-playbook-vehicle-telemetry-powerbi-dashboard/vehicle-telemetry-organize-dashboard3.png)

Glückwunsch! Sie haben die Berichte und das Dashboard erfolgreich erstellt, um in Bezug auf den Fahrzeugzustand und das Fahrverhalten Informationen in den Bereichen Echtzeit, vorbeugende Wartung und Batchprozesse zu erhalten.  
