---
title: Spark BI mit Datenvisualisierungstools unter Azure HDInsight | Microsoft-Dokumentation
description: "Verwenden von Datenvisualisierungstools für Analysen mithilfe von Apache Spark BI in HDInsight-Clustern"
keywords: Apache Spark Bi, Spark Bi, Spark-Datenvisualisierung, Spark-Business Intelligence
services: hdinsight
documentationcenter: 
author: nitinme
manager: jhubbard
editor: cgronlun
tags: azure-portal
ms.assetid: 1448b536-9bc8-46bc-bbc6-d7001623642a
ms.service: hdinsight
ms.custom: hdinsightactive,hdiseo17may2017
ms.workload: big-data
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 07/21/2017
ms.author: nitinme
ms.openlocfilehash: 869a000909813e607620c47ef802b4043e37dfa9
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="apache-spark-bi-using-data-visualization-tools-with-azure-hdinsight"></a>Apache Spark BI mit Datenvisualisierungstools unter Azure HDInsight

Erfahren Sie, wie Sie Datenvisualisierungstools wie Power BI und Tableau verwenden, um ein unformatiertes Beispieldataset mit Apache Spark BI in HDInsight-Clustern zu analysieren.

> [!NOTE]
> Die in diesem Artikel beschriebenen Verbindungen mit BI-Tools werden unter Spark 2.1 in der Vorschau von Azure HDInsight 3.6 nicht unterstützt. Nur die Spark-Versionen 1.6 und 2.0 (HDInsight 3.4 bzw. 3.5) werden unterstützt.
>

Dieses Tutorial steht auch als Jupyter-Notebook für einen HDInsight Spark-Cluster zur Verfügung. In der Notebook-Umgebung können Sie die Python-Ausschnitte direkt im Notebook ausführen. Wenn Sie das Tutorial innerhalb eines Notebooks ausführen möchten, erstellen Sie einen Spark-Cluster, starten Sie ein Jupyter Notebook (`https://CLUSTERNAME.azurehdinsight.net/jupyter`), und führen Sie dann das Notebook **Use BI tools with Apache Spark on HDInsight.ipynb** im Ordner **Python** aus.

## <a name="prerequisites"></a>Voraussetzungen

* Ein Apache Spark-Cluster unter HDInsight. Eine Anleitung finden Sie unter [Erstellen von Apache Spark-Clustern in Azure HDInsight](hdinsight-apache-spark-jupyter-spark-sql.md).


## <a name="hivetable"></a>Vorbereiten von Daten für die Spark-Datenvisualisierung

In diesem Abschnitt verwenden wir das [Jupyter](https://jupyter.org) Notebook aus einem HDInsight Spark-Cluster zum Ausführen von Aufträgen, bei denen Ihre Beispielrohdaten verarbeitet und dann als Tabelle gespeichert werden. Die Beispieldaten sind in einer CSV-Datei (hvac.csv) enthalten, die standardmäßig auf allen Clustern verfügbar ist. Nachdem Ihre Daten als Tabelle gespeichert wurden, verwenden wir im nächsten Abschnitt BI-Tools, um eine Verbindung mit der Tabelle herzustellen und Datenvisualisierungen durchzuführen.

> [!NOTE]
> Wenn Sie die Schritte in diesem Artikel durchführen, nachdem Sie die Anweisungen in [Run interactive queries on an HDInsight Spark cluster (Ausführen interaktiver Anfragen in einem Cluster von HDInsight Spark)](hdinsight-apache-spark-load-data-run-query.md) schon befolgt haben, können Sie mit Schritt 8 weiter unten fortfahren.
>

1. Klicken Sie im [Azure-Portal](https://portal.azure.com/)im Startmenü auf die Kachel für Ihren Spark-Cluster (sofern Sie die Kachel ans Startmenü angeheftet haben). Sie können auch unter **Alle durchsuchen** > **HDInsight-Cluster** zu Ihrem Cluster navigieren.   

2. Klicken Sie auf dem Blatt für den Spark-Cluster auf **Cluster-Dashboard** und anschließend auf **Jupyter Notebook**. Geben Sie die Administratoranmeldeinformationen für den Cluster ein, wenn Sie dazu aufgefordert werden.

   > [!NOTE]
   > Sie können auch das Jupyter Notebook für Ihren Cluster aufrufen, indem Sie in Ihrem Browser die folgende URL öffnen. Ersetzen Sie **CLUSTERNAME** durch den Namen Ihres Clusters:
   >
   > `https://CLUSTERNAME.azurehdinsight.net/jupyter`
   >
   >

3. Erstellen Sie ein Notebook. Klicken Sie auf **Neu** und dann auf **PySpark**.

    ![Erstellen eines Jupyter-Notebooks für Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/create-jupyter-notebook-for-spark-bi.png "Erstellen eines Jupyter-Notebooks für Apache Spark BI")

4. Ein neues Notebook mit dem Namen „Untitled.pynb“ wird erstellt und geöffnet. Klicken Sie oben auf den Namen des Notebooks, und geben Sie einen Anzeigenamen ein.

    ![Angeben eines Namens für das Notebooks für Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/jupyter-notebook-name-for-spark-bi.png "Angeben eines Namens für das Notebooks für Apache Spark BI")

5. Da Sie ein Notebook mit dem PySpark-Kernel erstellt haben, müssen Sie keine Kontexte explizit erstellen. Die Spark- und Hive-Kontexte werden automatisch für Sie erstellt, wenn Sie die erste Codezelle ausführen. Sie können zunächst die Typen importieren, die für dieses Szenario erforderlich sind. Setzen Sie dazu den Cursor in die Zelle, und drücken Sie **UMSCHALT- + EINGABETASTE**.

        from pyspark.sql import *

6. Laden Sie Beispieldaten in eine temporäre Tabelle. Wenn Sie einen Spark-Cluster in HDInsight erstellen, wird die Beispieldatendatei **hvac.csv** in das zugeordnete Speicherkonto unter **\HdiSamples\HdiSamples\SensorSampleData\hvac** kopiert.

    Fügen Sie den folgenden Codeausschnitt in eine leere Zelle ein, und drücken Sie **UMSCHALT+EINGABE** . Mit diesem Codeausschnitt werden die Daten in einer Tabelle mit dem Namen **hvac** registriert.

        # Create an RDD from sample data
        hvacText = sc.textFile("wasb:///HdiSamples/HdiSamples/SensorSampleData/hvac/HVAC.csv")

        # Create a schema for our data
        Entry = Row('Date', 'Time', 'TargetTemp', 'ActualTemp', 'BuildingID')

        # Parse the data and create a schema
        hvacParts = hvacText.map(lambda s: s.split(',')).filter(lambda s: s[0] != 'Date')
        hvac = hvacParts.map(lambda p: Entry(str(p[0]), str(p[1]), int(p[2]), int(p[3]), int(p[6])))

        # Infer the schema and create a table       
        hvacTable = sqlContext.createDataFrame(hvac)
        hvacTable.registerTempTable('hvactemptable')
        dfw = DataFrameWriter(hvacTable)
        dfw.saveAsTable('hvac')

7. Vergewissern Sie sich, dass die Tabelle erstellt wurde. Sie können die `%%sql` -Magic verwenden, um Hive-Abfragen direkt auszuführen. Weitere Informationen zur `%%sql`-Magic sowie anderen für den PySpark-Kernel verfügbaren Magics finden Sie unter [Verfügbare Kernels für Jupyter Notebooks mit Spark-Clustern unter HDInsight](hdinsight-apache-spark-jupyter-notebook-kernels.md#parameters-supported-with-the-sql-magic).

        %%sql
        SHOW TABLES

    Die Ausgabe sieht etwa wie folgt aus:

        +---------------+-------------+
        |tableName      |isTemporary  |
        +---------------+-------------+
        |hvactemptable  |true        |
        |hivesampletable|false        |
        |hvac           |false        |
        +---------------+-------------+

    Nur die Tabellen, für die in der Spalte **isTemporary** die Option „false“ angegeben ist, sind Hive-Tabellen, die im Metastore gespeichert werden und auf die mit den BI-Tools zugegriffen werden kann. In diesem Tutorial stellen wir eine Verbindung mit der erstellten **hvac**-Tabelle her.

8. Stellen Sie sicher, dass die Tabelle die gewünschten Daten enthält. Kopieren Sie den folgenden Codeausschnitt in eine leere Zelle im Notebook, und drücken Sie UMSCHALT+ **EINGABETASTE**.

        %%sql
        SELECT * FROM hvac LIMIT 10

9. Fahren Sie das Notebook herunter, um die Ressourcen freizugeben. Klicken Sie hierzu im Menü **Datei** des Notebooks auf die Option zum **Schließen und Anhalten**.

## <a name="powerbi"></a>Verwenden von Power BI für die Spark-Datenvisualisierung

> [!NOTE]
> Dieser Abschnitt gilt nur für Spark 1.6 auf HDInsight 3.4 und Spark 2.0 auf HDInsight 3.5.
>
>

Nachdem Sie die Daten als Tabelle gespeichert haben, können Sie Power BI verwenden, um eine Verbindung mit den Daten herzustellen und die Daten zu visualisieren. So können Sie Berichte, Dashboards usw. erstellen.

1. Stellen Sie sicher, dass Sie über Zugriff auf Power BI verfügen. Eine kostenlose Preview-Version von Power BI können Sie unter [http://www.powerbi.com/](http://www.powerbi.com/)abonnieren.

2. Melden Sie sich bei [Power BI](http://www.powerbi.com/) an.

3. Klicken Sie unten links im Bereich auf **Daten abrufen**.

4. Klicken Sie auf der Seite **Daten abrufen** unter **Daten importieren oder mit Daten verbinden** für **Datenbanken** auf **Abrufen**.

    ![Einlesen von Daten in Power BI für Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-import-data-power-bi.png "Einlesen von Daten in Power BI für Apache Spark BI")

5. Klicken Sie auf dem nächsten Bildschirm auf **Spark on Azure HDInsight** und dann auf **Verbinden**. Geben Sie nach Aufforderung die Cluster-URL (`mysparkcluster.azurehdinsight.net`) und die Anmeldeinformationen für die Verbindung mit dem Cluster ein.

    ![Herstellen einer Verbindung mit Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-apache-spark-bi.png "Herstellen einer Verbindung mit Apache Spark BI")

    Nachdem die Verbindung hergestellt wurde, beginnt Power BI mit dem Importieren der Daten aus dem Spark-Cluster unter HDInsight.

6. Power BI importiert die Daten und fügt ein **Spark**-Dataset unter der Überschrift **Datasets** hinzu. Klicken Sie auf das Dataset, um ein neues Arbeitsblatt zum Visualisieren der Daten zu öffnen. Sie können das Arbeitsblatt auch als Bericht speichern. Klicken Sie im Menü **Datei** auf **Speichern**, um ein Arbeitsblatt zu speichern.

    ![Apache Spark BI-Kachel im Power BI-Dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-tile-dashboard.png "Apache Spark BI-Kachel im Power BI-Dashboard")
7. Beachten Sie, dass in der Liste **Felder** auf der rechten Seite die **hvac**-Tabelle aufgeführt ist, die Sie vorhin erstellt haben. Erweitern Sie die Tabelle, um die Felder in der Tabelle anzuzeigen, die Sie im Notebook definiert haben.

      ![Liste der Tabellen im Apache Spark BI-Dashboard](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-display-tables.png "Liste der Tabellen im Apache Spark BI-Dashboard")

8. Erstellen Sie eine Visualisierung, um die Abweichung zwischen Zieltemperatur und Ist-Temperatur für jedes Gebäude anzuzeigen. Wählen Sie **Area Chart** (rot umrandet dargestellt) aus, um Ihre Daten zu visualisieren. Zum Definieren der Achse verschieben Sie das Feld **BuildingID** per Drag & Drop unter **Achse** und die Felder **ActualTemp**/**TargetTemp** unter **Wert**.

    ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-add-value-columns.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

9. Standardmäßig werden in der Visualisierung die Summen für **ActualTemp** und **TargetTemp** angezeigt. Wählen Sie für beide Felder in der Dropdownliste die Option **Mittelwert** aus, um für beide Gebäude den Mittelwert der Ist- und Zieltemperaturen zu erhalten.

    ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-average-of-values.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

10. Ihre Datenvisualisierung sollte in etwa wie im Screenshot aussehen. Bewegen Sie den Cursor über die Visualisierung, um QuickInfos mit relevanten Daten abzurufen.

    ![Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/apache-spark-bi-area-graph.png "Erstellen von Spark-Datenvisualisierungen mithilfe von Apache Spark BI")

11. Klicken Sie im oberen Menü auf **Speichern** , und geben Sie einen Berichtsnamen an. Sie können die Visualisierung auch anheften. Wenn Sie eine Visualisierung anheften, wird sie in Ihrem Dashboard gespeichert, damit Sie jeweils den aktuellen Wert im Blick haben.

   Sie können für ein Dataset beliebig viele Visualisierungen hinzufügen und im Dashboard anheften, um eine Momentaufnahme Ihrer Daten zu erhalten. Außerdem sind Spark-Cluster unter HDInsight per Direktverbindung mit Power BI verbunden. So wird sichergestellt, dass Power BI immer über die aktuellsten Daten aus Ihrem Cluster verfügt, damit Sie keine Aktualisierungen für das Dataset einplanen müssen.

## <a name="tableau"></a>Verwenden von Tableau Desktop für die Spark-Datenvisualisierung

> [!NOTE]
> Dieser Abschnitt gilt nur für Spark 1.5.2-Cluster, die in Azure HDInsight erstellt wurden.
>
>

1. Installieren Sie [Tableau Desktop](http://www.tableau.com/products/desktop) auf dem Computer, auf dem Sie dieses Apache Spark BI-Tutorial durchführen.

2. Stellen Sie sicher, dass auf dem Computer auch der Microsoft Spark ODBC-Treiber installiert ist. Sie können den Treiber [hier](http://go.microsoft.com/fwlink/?LinkId=616229)installieren.

1. Starten Sie Tableau Desktop. Klicken Sie im linken Bereich in der Liste mit den verfügbaren Servern auf **Spark SQL**. Wenn Spark-SQL nicht standardmäßig im linken Bereich aufgeführt ist, finden Sie es durch Klicken auf **Mehr Server**.
2. Geben Sie im Dialogfeld für die Spark-SQL-Verbindung die Werte wie im Screenshot dargestellt ein, und klicken Sie dann auf **OK**.

    ![Herstellen einer Verbindung mit einem Cluster für Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/connect-to-tableau-apache-spark-bi.png "Herstellen einer Verbindung mit einem Cluster für Apache Spark BI")

    In der Dropdownliste für die Authentifizierung wird der **Microsoft Azure HDInsight-Dienst** nur dann als Option aufgeführt, wenn Sie auf dem Computer den [Microsoft Spark-ODBC-Treiber](http://go.microsoft.com/fwlink/?LinkId=616229) installiert haben.
3. Klicken Sie auf dem nächsten Bildschirm in der Dropdownliste **Schema** auf das Symbol **Suchen** und dann auf **Standard**.

    ![Suchen eines Schemas für Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-schema-apache-spark-bi.png "Suchen eines Schemas für Apache Spark BI")
4. Klicken Sie für das Feld **Tabelle** erneut auf das Symbol **Suchen**, um alle Hive-Tabellen aufzulisten, die im Cluster verfügbar sind. Daraufhin sollte die **hvac** -Tabelle angezeigt werden, die Sie zuvor mit dem Notebook erstellt haben.

    ![Suchen einer Tabelle für Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-find-table-apache-spark-bi.png "Suchen einer Tabelle für Apache Spark BI")
5. Legen Sie die Tabelle im oberen Feld auf der rechten Seite ab. Tableau importiert die Daten und zeigt das Schema wie in der Hervorhebung mit dem roten Kasten an.

    ![Hinzufügen von Tabellen zu Tableau für Apache Spark BI](./media/hdinsight-apache-spark-use-bi-tools/tableau-add-table-apache-spark-bi.png "Hinzufügen von Tabellen zu Tableau für Apache Spark BI")
6. Klicken Sie unten links auf die Registerkarte **Blatt1** . Erstellen Sie eine Visualisierung, in der die durchschnittlichen Ziel- und Ist-Temperaturen für alle Gebäude und jedes Datum angezeigt werden. Ziehen Sie **Datum** und **Building ID** (Gebäude-ID) auf **Spalten** und **Actual Temp**/**Target Temp** (Tatsächliche Temperatur/Zieltemperatur) auf **Zeilen**. Wählen Sie unter **Markierungen** die Option **Bereich** aus, um eine Bereichskarte für die Spark-Datenvisualisierung zu verwenden.

     ![Hinzufügen von Feldern für die Spark-Datenvisualisierung](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-add-fields.png "Hinzufügen von Feldern für die Spark-Datenvisualisierung")
7. Standardmäßig werden die Temperaturfelder als Aggregatwerte angezeigt. Wenn Sie stattdessen die durchschnittlichen Temperaturen anzeigen möchten, können Sie die Dropdownliste verwenden, wie unten dargestellt.

    ![Ermitteln der Durchschnittstemperatur für die Spark-Datenvisualisierung](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-average-temperature.png "Ermitteln der Durchschnittstemperatur für die Spark-Datenvisualisierung")

8. Sie können auch eine Temperaturkarte auf die andere Temperaturkarte legen, um ein besseres Gefühl für den Unterschied zwischen Ziel- und Ist-Temperaturen zu erhalten. Bewegen Sie den Mauszeiger in die Ecke der unteren Bereichskarte, bis das Griffsymbol mit einem roten Kreis markiert wird. Ziehen Sie die Karte in die andere Karte, die darüber liegt, und lassen Sie die Maustaste los, wenn die Form im roten Rechteck hervorgehoben wird.

    ![Zusammenführen von Karten für die Spark-Datenvisualisierung](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-merge-maps.png "Zusammenführen von Karten für die Spark-Datenvisualisierung")

     Die Visualisierung der Daten sollte sich wie im Screenshot dargestellt ändern:

    ![Tableau-Ausgabe für die Spark-Datenvisualisierung](./media/hdinsight-apache-spark-use-bi-tools/spark-data-visualization-tableau-output.png "Tableau-Ausgabe für die Spark-Datenvisualisierung")
9. Klicken Sie auf **Speichern** , um das Arbeitsblatt zu speichern. Sie können Dashboards erstellen und ein oder mehrere Blätter hinzufügen.

## <a name="next-steps"></a>Nächste Schritte

Bis hier her haben Sie erfahren, wie Sie ein Cluster und Spark-Datenrahmen zum Abfrage von Daten erstellen können und wie Sie dann auf diese Daten von BI-Tools aus zugreifen. Es stehen Ihnen nun Anweisungen zur Verfügung, wie Sie Clusterressourcen verwalten und Aufträge debuggen können, die ein einem Cluster von HDInsight Spark ausgeführt werden.

* [Verwalten von Ressourcen für den Apache Spark-Cluster in Azure HDInsight](hdinsight-apache-spark-resource-manager.md)
* [Track and debug jobs running on an Apache Spark cluster in HDInsight(Nachverfolgen und Debuggen von Aufträgen in einem Apache Spark-Cluster unter HDInsight)](hdinsight-apache-spark-job-debugging.md)

