---
title: "Migrieren einer Datenbank mithilfe von Import und Export in Azure-Datenbank für PostgreSQL | Microsoft-Dokumentation"
description: Hier wird beschrieben, wie Sie eine PostgreSQL-Datenbank in eine Skriptdatei extrahieren und die Daten aus dieser Datei in die Zieldatenbank importieren.
services: postgresql
author: SaloniSonpal
ms.author: salonis
manager: jhubbard
editor: jasonwhowell
ms.service: postgresql
ms.topic: article
ms.date: 06/14/2017
ms.openlocfilehash: 5e306d516d04789e4526bfd09bf99139b83573ba
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="migrate-your-postgresql-database-using-export-and-import"></a>Migrieren der PostgreSQL-Datenbank mit Export und Import
Sie können mit [pg_dump](https://www.postgresql.org/docs/9.3/static/app-pgdump.html) eine PostgreSQL-Datenbank in eine Skriptdatei extrahieren und die Daten mit [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) aus dieser Datei in die Zieldatenbank importieren.

## <a name="prerequisites"></a>Voraussetzungen
Zum Ausführen der Schritte in dieser Anleitung benötigen Sie Folgendes:
- Einen [Server für Azure-Datenbank für PostgreSQL](quickstart-create-server-database-portal.md) mit Firewallregeln, um den Zugriff und eine Datenbank darunter zu ermöglichen
- Das Befehlszeilenprogramm [pg_dump](https://www.postgresql.org/docs/9.6/static/app-pgdump.html) muss installiert sein.
- Das Befehlszeilenprogramm [psql](https://www.postgresql.org/docs/9.6/static/app-psql.html) muss installiert sein.

Führen Sie die nachfolgend aufgeführten Schritte zum Exportieren und Importieren Ihrer PostgreSQL-Datenbank aus.

## <a name="create-a-script-file-using-pgdump-that-contains-the-data-to-be-loaded"></a>Erstellen einer Skriptdatei mit pg_dump, die die zu ladenden Daten enthält
Führen Sie zum Exportieren der lokalen oder auf einem virtuellen Computer vorhandenen PostgreSQL-Datenbank in eine SQL-Skriptdatei den folgenden Befehl in Ihrer vorhandenen Umgebung aus:
```bash
pg_dump –-host=<host> --username=<name> --dbname=<database name> --file=<database>.sql
```
Beispiel: Lokaler Server und eine Datenbank namens **testdb**
```bash
pg_dump --host=localhost --username=masterlogin --dbname=testdb --file=testdb.sql
```

## <a name="import-the-data-on-target-azure-database-for-postrgesql"></a>Importieren der Daten in die Azure-Zieldatenbank für PostgreSQL
Sie können die psql-Befehlszeile und den Parameter „-d, --dbname“ verwenden, um die Daten in die erstellte Azure-Datenbank für PostrgeSQL zu importieren und die Daten aus der SQL-Datei zu laden.
```bash
psql --file=<database>.sql --host=<server name> --port=5432 --username=<user@servername> --dbname=<target database name>
```
In diesem Beispiel werden das psql-Hilfsprogramm und eine Skriptdatei namens **testdb.sql** aus dem vorherigen Schritt, um Daten in die Datenbank **mypgsqldb** auf dem Zielserver **mypgserver-20170401.postgres.database.azure.com** zu importieren.
```bash
psql --file=testdb.sql --host=mypgserver-20170401.database.windows.net --port=5432 --username=mylogin@mypgserver-20170401 --dbname=mypgsqldb
```

## <a name="next-steps"></a>Nächste Schritte
- Informationen zum Migrieren einer PostgreSQL-Datenbank durch Sichern und Wiederherstellen finden Sie unter [Migrieren der PostgreSQL-Datenbank durch Sichern und Wiederherstellen](howto-migrate-using-dump-and-restore.md).