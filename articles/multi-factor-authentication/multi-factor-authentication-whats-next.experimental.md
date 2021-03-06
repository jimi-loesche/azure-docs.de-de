---
title: Konfigurieren von Azure MFA | Microsoft-Dokumentation
description: "Dies ist die Seite \"Azure Multi-Factor Authentication\", auf der beschrieben wird, was die nächsten Schritte mit Multi-Factor Authentication sind.  Dazu gehören Berichte, Betrugswarnung, Einmalumgehung, benutzerdefinierte Sprachnachrichten, Zwischenspeicherung, vertrauenswürdige IP-Adressen und App-Kennwörter."
services: multi-factor-authentication
documentationcenter: 
author: kgremban
manager: femila
editor: yossib
ms.assetid: 75af734e-4b12-40de-aba4-b68d91064ae8
ms.service: multi-factor-authentication
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/21/2017
ms.author: kgremban
ms.openlocfilehash: 9d77b9329116afcf2fdde48d672c95020738138c
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="configure-azure-multi-factor-authentication-settings"></a>Konfigurieren von Azure Multi-Factor Authentication-Einstellungen
Dieser Artikel bietet Unterstützung bei der Verwaltung der Azure Multi-Factor Authentication, nachdem Sie nun bereit für die Ausführung sind.  Der Artikel umfasst eine Vielzahl von Themen, die Ihnen dabei helfen, Azure Multi-Factor Authentication optimal zu nutzen.  Nicht alle Funktionen sind in jeder Version von Azure Multi-Factor Authentication verfügbar.

| Funktion | Beschreibung | 
|:--- |:--- |
| [Betrugswarnung](#fraud-alert) |Die Betrugswarnung kann so konfiguriert und eingerichtet werden, dass Ihre Benutzer betrügerische Versuche, auf ihre Ressourcen zuzugreifen, melden können. |
| [Einmalumgehung](#one-time-bypass) |Mit einer Einmalumgehung kann sich ein Benutzer ein einziges Mal authentifizieren, indem er die mehrstufige Authentifizierung "umgeht". |
| [Benutzerdefinierte Sprachnachrichten](#custom-voice-messages) |Mit benutzerdefinierten Sprachnachrichten können Sie Ihre eigenen Aufzeichnungen oder Begrüßungen mit mehrstufiger Authentifizierung verwenden. |
| [Zwischenspeichern](#caching-in-azure-multi-factor-authentication) |Durch Zwischenspeichern können Sie einen bestimmten Zeitraum festlegen, sodass nachfolgende Authentifizierungsversuche automatisch erfolgreich sind. |
| [Vertrauenswürdige IPs](#trusted-ips) |Mit vertrauenswürdigen IP-Adressen können Administratoren eines verwalteten Mandanten oder Verbundmandanten die Überprüfung in zwei Schritten für Benutzer umgehen, die sich über das lokale Intranet des Unternehmens anmelden. |
| [App-Kennwörter](#app-passwords) |Durch ein App-Kennwort kann eine Anwendung, die MFA nicht erkennt, Multi-Factor Authentication umgehen, und weiter ausgeführt werden. |
| [Speichern der Multi-Factor Authentication für gespeicherte Geräte und Browser](#remember-multi-factor-authentication-for-devices-that-users-trust) |Mit dieser Funktion können Sie Geräte für eine festgelegte Anzahl von Tagen speichern, nachdem ein Benutzer erfolgreich mit MFA angemeldet wurde. |
| [Auswählbare Verifizierungsmethoden](#selectable-verification-methods) |Ermöglicht Ihnen die Auswahl der Authentifizierungsmethoden, die Sie den Benutzern zur Verfügung stellen möchten. |

## <a name="access-the-azure-mfa-management-portal"></a>Zugreifen auf das Azure MFA-Verwaltungsportal

Die in diesem Artikel beschriebenen Features werden im Azure Multi-Factor Authentication-Verwaltungsportal konfiguriert. Es gibt zwei Möglichkeiten, über das klassische Azure-Portal auf das MFA-Verwaltungsportal zuzugreifen: Einerseits über die Verwaltung des Multi-Factor Authentication-Anbieters. Andererseits über die Diensteinstellungen der MFA. 

### <a name="use-an-authentication-provider"></a>Verwenden eines Authentifizierungsanbieters

Wenn Sie einen Multi-Factor Authentication-Anbieter für verbrauchsbasierte MFA verwenden, greifen Sie mithilfe dieser Methode auf das Verwaltungsportal zu.

Melden Sie sich als Administrator beim klassischen Azure-Portal an, und wählen Sie die Option „Active Directory“, um über einen Multi-Factor Authentication-Anbieter auf das MFA-Verwaltungsportal zuzugreifen. Klicken Sie auf die Registerkarte **Anbieter für mehrstufige Authentifizierung**, wählen Sie Ihr Verzeichnis aus, und klicken Sie auf unten auf **Verwalten**.

### <a name="use-the-mfa-service-settings-page"></a>Verwenden der Seite mit den MFA-Diensteinstellungen 

Wenn Sie über eine der folgenden Lizenzen verfügen, können Sie die Seite mit den MFA-Diensteinstellungen verwenden:
- Azure MFA
- Azure AD Premium 
- Enterprise Mobility + Security

Melden Sie sich als Administrator beim klassischen Azure-Portal an, und wählen Sie die Option „Active Directory“, um über die Seite mit den Diensteinstellungen für die MFA auf das MFA-Verwaltungsportal zuzugreifen. Klicken Sie auf Ihr Verzeichnis und anschließend auf die Registerkarte „ **Konfigurieren** “. Klicken Sie im Abschnitt „Multi-Factor Authentication“ auf „ **Diensteinstellungen verwalten**“. Klicken Sie am unteren Rand der Seite mit den Einstellungen für den MFA-Dienst auf den Link „ **Portal aufrufen** “.


## <a name="fraud-alert"></a>Betrugswarnung
Die Betrugswarnung kann so konfiguriert und eingerichtet werden, dass Ihre Benutzer betrügerische Versuche, auf ihre Ressourcen zuzugreifen, melden können.  Benutzer können entweder mit der mobilen App oder über ihre Rufnummer den Betrug melden.

### <a name="set-up-fraud-alert"></a>Einrichten einer Betrugswarnung
1. Folgen Sie den Anweisungen im oberen Teil dieser Seite, um zum MFA-Verwaltungsportal zu gelangen.
2. Klicken Sie im Abschnitt „Konfigurieren“ des Azure Multi-Factor Authentication-Verwaltungsportals auf **Einstellungen**.
3. Aktivieren Sie im Abschnitt „Betrugswarnung“ auf der Seite „Einstellungen“ das Kontrollkästchen **Benutzern die Ausgabe von Betrugswarnungen erlauben**.
4. Klicken Sie zum Übernehmen der Änderungen auf **Speichern**. 

### <a name="configuration-options"></a>Konfigurationsoptionen

- **Benutzer bei Betrugsmeldung sperren**: Wenn ein Benutzer einen Betrug meldet, wird sein Konto gesperrt.
- **Code zur Meldung von Betrug während der Begrüßung**: Benutzer drücken in der Regel die #-TASTE, um die Überprüfung in zwei Schritten zu bestätigen. Wenn sie einen Betrug melden möchten, geben sie vor dem Drücken der #-TASTE einen Code ein. Dieser Code ist standardmäßig **0**, Sie können ihn jedoch anpassen.

> [!NOTE]
> In Microsofts Standardansage wird der Benutzer aufgefordert, zum Senden einer Betrugswarnung die Zeichenfolge „0#“ einzugeben. Wenn Sie einen anderen Code als „0“ verwenden wollen, sollten Sie eine benutzerdefinierte Ansage mit den passenden Anweisungen aufnehmen.

![Optionen für Betrugswarnungen (Screenshot)](./media/multi-factor-authentication-whats-next/fraud.png)

### <a name="how-users-report-fraud"></a>Betrugsmeldung durch Benutzer 
Eine Betrugswarnung kann auf zwei Arten gemeldet werden.  Entweder über die mobile App oder telefonisch.  

#### <a name="report-fraud-with-the-mobile-app"></a>Melden eines Betrugs mit der mobilen App
1. Wenn eine Überprüfung an Ihr Smartphone gesendet wird, wählen Sie diese aus, um die Microsoft Authenticator-App zu öffnen.
2. Wählen Sie **Verweigern** in der Benachrichtigung. 
3. Wählen Sie **Betrug melden**.
4. Schließen Sie die App.

#### <a name="report-fraud-with-a-phone"></a>Melden eines Betrugs mithilfe eines Telefons
1. Beantworten Sie den Überprüfungsanruf an Ihr Telefon.  
2. Geben Sie zum Melden eines Betrugs den Betrugscode (Standardwert: 0) und anschließend das #-Zeichen ein. Sie werden dann benachrichtigt, dass eine Betrugswarnung gesendet wurde.
3. Beenden Sie den Anruf.

### <a name="view-fraud-reports"></a>Anzeigen von Betrugsberichten
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich "Active Directory" aus.
3. Wählen Sie im oberen Bereich **Anbieter für mehrstufige Authentifizierung** aus, um eine Liste der Multi-Factor Authentication-Anbieter anzuzeigen.
4. Wählen Sie den Multi-Factor Authentication-Anbieter aus, und klicken Sie unten auf der Seite auf **Verwalten**. Das Azure Multi-Factor Authentication-Verwaltungsportal wird geöffnet.
5. Klicken Sie im Azure Multi-Factor Authentication-Verwaltungsportal unter „Einen Bericht anzeigen“ auf **Betrugswarnung**.
6. Geben Sie den Datumsbereich an, der im Bericht angezeigt werden soll. Sie können auch Benutzernamen, Telefonnummern und den Benutzerstatus angeben.
7. Klicken Sie auf **Ausführen**, um einen Bericht über Betrugswarnungen anzuzeigen. Klicken Sie auf **In CSV-Datei exportieren**, wenn Sie den Bericht exportieren möchten.

## <a name="one-time-bypass"></a>Einmalumgehung
Mit einer Einmalumgehung kann sich ein Benutzer ein einziges Mal authentifizieren, ohne die Überprüfung in zwei Schritten auszuführen. Die Umgehung ist vorübergehend und läuft nach einer angegebenen Anzahl von Sekunden ab. Falls die mobile Anwendung oder das Telefon keine Benachrichtigung bzw. keinen Telefonanruf empfängt, können Sie eine Einmalumgehung aktivieren, damit der Benutzer auf die gewünschte Ressource zugreifen kann.

### <a name="create-a-one-time-bypass"></a>Erstellen einer Einmalumgehung
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Folgen Sie den Anweisungen im oberen Teil dieser Seite, um zum MFA-Verwaltungsportal zu gelangen.
3. Wenn links vom Namen Ihres Azure-MFA-Anbieters das Zeichen **+** angezeigt wird, klicken Sie auf das **+**. Daraufhin werden verschiedene MFA Server-Replikationsgruppen und die Gruppe „Azure Default“ angezeigt. Wählen Sie die entsprechende Gruppe aus.
4. Wählen Sie unter „Benutzerverwaltung“ die Option **Einmalumgehung**.
5. Klicken Sie auf der Seite „Einmalumgehung“ auf **Neue Einmalumgehung**.

  ![Cloud](./media/multi-factor-authentication-whats-next/create1.png)

6. Geben Sie den Benutzernamen, die Dauer der Umgehung sowie den Grund für die Umgehung ein. Klicken Sie auf **Umgehen**.
7. Das Zeitlimit gilt sofort. Daher muss sich der Benutzer anmelden, bevor die Einmalumgehung abläuft. 

### <a name="view-the-one-time-bypass-report"></a>Anzeigen des Berichts für die Einmalumgehung
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Wählen Sie im linken Bereich "Active Directory" aus.
3. Wählen Sie im oberen Bereich **Anbieter für mehrstufige Authentifizierung** aus, um eine Liste der Multi-Factor Authentication-Anbieter anzuzeigen.
4. Wählen Sie den Multi-Factor Authentication-Anbieter aus, und klicken Sie unten auf der Seite auf **Verwalten**. Das Azure Multi-Factor Authentication-Verwaltungsportal wird geöffnet.
5. Klicken Sie im Azure Multi-Factor Authentication-Verwaltungsportal auf der linken Seite unter „Einen Bericht anzeigen“ auf **Einmalumgehung**.
6. Geben Sie den Datumsbereich an, der im Bericht angezeigt werden soll. Sie können auch Benutzernamen, Telefonnummern und den Benutzerstatus angeben.
7. Klicken Sie auf **Ausführen**, um einen Bericht über Umgehungen anzuzeigen. Klicken Sie auf **In CSV-Datei exportieren**, wenn Sie den Bericht exportieren möchten.

## <a name="custom-voice-messages"></a>Benutzerdefinierte Sprachnachrichten
Mit benutzerdefinierten Sprachnachrichten können Sie Ihre eigenen Aufzeichnungen oder Begrüßungen für die Überprüfung in zwei Schritten verwenden. Sie können benutzerdefinierte Sprachnachrichten zusätzlich zu Microsoft-Aufzeichnungen oder anstelle von diesen verwenden.

Bevor Sie beginnen, sollten Sie folgende Punkte beachten:

* Die unterstützten Dateiformate sind WAV und MP3.
* Die maximale Dateigröße beträgt 5 MB.
* Authentifizierungsnachrichten sollten kürzer als 20 Sekunden sein. Nachrichten, die länger als 20 Sekunden sind, lassen den Benutzern nicht genügend Zeit zum Antworten, bevor die Überprüfungszeit abläuft.

### <a name="set-up-a-custom-message"></a>Einrichten einer benutzerdefinierten Nachricht

Eine benutzerdefinierte Nachricht wird in zwei Schritten erstellt: Zuerst laden Sie die Nachricht hoch, und anschließend aktivieren Sie sie für Ihre Benutzer.

So laden Sie Ihre benutzerdefinierte Nachricht hoch:

1. Erstellen Sie eine Sprachnachricht in einem der unterstützten Dateiformate.
2. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
3. Folgen Sie den Anweisungen im oberen Teil dieser Seite, um zum MFA-Verwaltungsportal zu gelangen.
4. Klicken Sie im Abschnitt „Konfigurieren“ des Azure Multi-Factor Authentication-Verwaltungsportals auf **Sprachnachrichten**.
5. Klicken Sie auf der Seite „Konfigurieren: Sprachnachrichten“ auf **Neue Sprachnachricht**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom1.png)
6. Klicken Sie auf der Seite „Konfigurieren: Neue Sprachnachrichten“ auf **Audiodateien verwalten**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom2.png)
7. Klicken Sie auf der Seite „Konfigurieren: Audiodateien“ auf **Audiodatei hochladen**.
   ![Cloud](./media/multi-factor-authentication-whats-next/custom3.png)
8. Klicken Sie auf der Seite „Konfigurieren: Audiodatei hochladen“ auf **Durchsuchen**, navigieren Sie zu Ihrer Sprachnachricht, und klicken Sie dann auf **Öffnen**.
9. Fügen Sie eine Beschreibung hinzu, und klicken Sie auf **Hochladen**.
10. Sobald die Sprachnachricht hochgeladen wurde, wird in einer Meldung bestätigt, dass Sie die Datei erfolgreich hochgeladen haben.

So aktivieren Sie die Nachricht für die Benutzer

1. Klicken Sie auf der linken Seite auf **Sprachnachrichten**.
2. Klicken Sie im Abschnitt „Sprachnachrichten“ auf **Neue Sprachnachricht**.
3. Wählen Sie eine Sprache aus der Dropdownliste "Sprache".
4. Wenn diese Nachricht für eine bestimmte Anwendung gedacht ist, geben Sie dies im Feld "Anwendung" ein.
5. Wählen Sie in der Dropdownliste „Nachrichtentyp“ den Nachrichtentyp aus, der von der neuen benutzerdefinierten Nachricht überschrieben wird.
6. Wählen Sie in der Dropdownliste „Audiodatei“ die Audiodatei aus, die Sie im ersten Schritt hochgeladen haben.
7. Klicken Sie auf **Erstellen**. Eine Nachricht bestätigt, dass Sie erfolgreich eine Sprachnachricht erstellt haben.
    ![Cloud](./media/multi-factor-authentication-whats-next/custom5.png)</center>

## <a name="caching-in-azure-multi-factor-authentication"></a>Zwischenspeichern in Azure Multi-Factor Authentication
Durch Zwischenspeichern können Sie einen bestimmten Zeitraum festlegen, sodass nachfolgende Authentifizierungsversuche in diesem Zeitraum automatisch erfolgreich sind. Zwischenspeichern wird verwendet, wenn lokale Systeme, z.B. VPNs, viele Überprüfungsanforderungen senden, während die erste Anforderung noch bearbeitet wird. Dadurch werden die nachfolgenden Anforderungen automatisch erfolgreich ausgeführt, nachdem die erste Überprüfungsanforderung für den Benutzer erfolgreich ausgeführt wurde. 

Das Zwischenspeichern ist nicht für Anmeldungen bei Azure AD gedacht.

### <a name="set-up-caching"></a>Einrichten der Zwischenspeicherung 
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Folgen Sie den Anweisungen im oberen Teil dieser Seite, um zum MFA-Verwaltungsportal zu gelangen.
3. Klicken Sie im Abschnitt „Konfigurieren“ des Azure Multi-Factor Authentication-Verwaltungsportals auf **Zwischenspeichern**.
4. Klicken Sie auf der Seite „Zwischenspeicherung konfigurieren“ auf **Neuer Cache**.
5. Wählen Sie den Cache-Typ und die Aufbewahrungsdauer aus. Klicken Sie auf **Erstellen**.

<center>![Cloud](./media/multi-factor-authentication-whats-next/cache.png)</center>

## <a name="trusted-ips"></a>Vertrauenswürdige IP-Adressen
Mit vertrauenswürdigen IP-Adressen der Azure MFA können Administratoren eines verwalteten Mandanten oder Verbundmandanten die Überprüfung in zwei Schritten umgehen. Dieses Verfahren wird für Benutzer verwendet, die sich über das lokale Intranet des Unternehmens anmelden. Dieses Feature ist in der Vollversion von Azure Multi-Factor Authentication, aber nicht in der kostenlosen Version für Administratoren verfügbar. Weitere Informationen zum Beziehen der Vollversion von Azure Multi-Factor Authentication finden Sie unter [Azure Multi-Factor Authentication](multi-factor-authentication.md).

| Typen von Azure AD-Mandanten | Verfügbare vertrauenswürdige IP-Optionen |
|:--- |:--- |
| Verwaltet |<li>Bestimmte IP-Adressbereiche: Administratoren können einen Bereich von IP-Adressen angeben, die die Überprüfung in zwei Schritten für Benutzer umgehen können, die sich vom Intranet des Unternehmens aus anmelden.</li> |
| Im Verbund |<li>Alle Verbundbenutzer: Alle Verbundbenutzer, die sich von innerhalb des Unternehmen aus anmelden, umgehen die Überprüfung in zwei Schritten mithilfe eines von AD FS ausgestellten Anspruchs.</li><br><li>Bestimmte IP-Adressbereiche: Administratoren können einen Bereich von IP-Adressen angeben, die die Überprüfung in zwei Schritten für Benutzer umgehen können, die sich vom Intranet des Unternehmens aus anmelden. |

Diese Umgehung funktioniert nur von innerhalb des Intranets eines Unternehmens. 

**Endbenutzererfahrung innerhalb des Unternehmensnetzwerks:**

Wenn vertrauenswürdige IPs deaktiviert sind, ist für Browserflüsse die Überprüfung in zwei Schritten erforderlich. Für ältere Rich Client-Apps werden App-Kennwörter benötigt. 

Wenn vertrauenswürdige IP-Adressen aktiviert sind, ist die Überprüfung in zwei Schritten für Browserflüsse *nicht* erforderlich. App-Kennwörter sind für ältere Rich Client-Apps *nicht* erforderlich, sofern der Benutzer nicht bereits ein App-Kennwort erstellt hat. Sobald ein App-Kennwort verwendet wird, ist es erforderlich. 

**Endbenutzererfahrung außerhalb des Unternehmensnetzwerks:**

Unabhängig davon, ob vertrauenswürdige IPs aktiviert sind, ist die Überprüfung in zwei Schritten für Browserflüsse erforderlich, und für ältere Rich Client-Apps werden App-Kennwörter benötigt. 

### <a name="to-enable-trusted-ips"></a>Aktivieren von vertrauenswürdigen IP-Adressen
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie gemäß den Anweisungen am Anfang dieses Artikels zur Seite mit den MFA-Diensteinstellungen.
3. Auf der Seite „Diensteinstellungen“ haben Sie unter „Vertrauenswürdige IPs“ zwei Optionen:
   
   * **Für Anforderungen von Partnerbenutzern, die aus meinem Intranet stammen**: Aktivieren Sie das Kontrollkästchen. Alle Verbundbenutzer, die sich vom Unternehmensnetzwerk aus anmelden, umgehen die Überprüfung in zwei Schritten mithilfe eines von AD FS ausgestellten Anspruchs.
   * **Für Anforderungen aus einem bestimmten Bereich öffentlicher IPs**: Geben Sie mithilfe der CIDR-Notation die IP-Adressen in das Textfeld ein. Beispiel: xxx.xxx.xxx.0/24 für IP-Adressen im Bereich xxx.xxx.xxx. 1 – xxx.xxx.xxx. 254 oder xxx.xxx.xxx.xxx/32 für eine einzelne IP-Adresse. Sie können bis zu 50 IP-Adressbereiche eingeben. Benutzer, die sich über diese IP-Adressen anmelden, umgehen die Überprüfung in zwei Schritten.
4. Klicken Sie auf **Speichern**.
5. Sobald die Updates angewendet wurden, klicken Sie auf **Schließen**.

![Vertrauenswürdige IP-Adressen](./media/multi-factor-authentication-whats-next/trustedips3.png)

## <a name="app-passwords"></a>App-Kennwörter
Einige Apps wie Office 2010 oder ältere Versionen und Apple Mail unterstützen die Überprüfung in zwei Schritten nicht. Sie sind nicht für die Annahme einer zweiten Überprüfung konfiguriert. Um diese Apps zu verwenden, müssen Sie „App-Kennwörter“ anstelle Ihres herkömmlichen Kennworts angeben. Mit App-Kennwörtern kann die Anwendung die Überprüfung in zwei Schritten umgehen und weiter ausgeführt werden.

> [!NOTE]
> Moderne Authentifizierung für Office 2013-Clients
> 
> Office 2013-Clients (einschließlich Outlook) und neuere Clients unterstützen moderne Authentifizierungsprotokolle und können für die Überprüfung in zwei Schritten aktiviert werden. Nach der Aktivierung sind für diese Clients keine App-Kennwörter erforderlich.  Weitere Informationen finden Sie unter [Office 2013 modern authentication public preview announced](https://blogs.office.com/2015/03/23/office-2013-modern-authentication-public-preview-announced/) („Öffentliche Preview für moderne Authentifizierung in Office 2013“, in englischer Sprache).

### <a name="important-things-to-know-about-app-passwords"></a>Wichtige Informationen zu App-Kennwörtern
Es folgt eine Liste wichtiger Dinge, die Sie über App-Kennwörter wissen sollten.

* App-Kennwörtern sollten einmal pro App im Eingabefeld eingegeben werden. So müssen sich Benutzer ihre Kennwörter nicht merken und jedes Mal erneut eingeben.
* Das eigentliche Kennwort wird automatisch erzeugt und nicht vom Benutzer festgelegt. Automatisch generierte Kennwörter sind sicherer und für einen Angreifer schwerer zu erraten.
* Pro Benutzer können maximal 40 Kennwörter festgelegt werden. 
* Anwendungen, die Kennwörter zwischenspeichern und in lokalen Szenarios nutzen, könnten fehlschlagen, weil das App-Kennwort außerhalb der Organisations-ID nicht bekannt ist. Ein Beispiel sind Exchange-E-Mails, die lokal vorhanden sind, bei denen sich die archivierten Nachrichten jedoch in der Cloud befinden. Das gleiche Kennwort funktioniert nicht.
* Wenn Multi-Factor Authentication gestartet wird, können Sie Ihr Kennwort für einige Nicht-Browser-Clients verwenden. Sie können App-Kennwörter nicht für administrative Aufgaben verwenden. Stellen Sie sicher, dass Sie zum Ausführen von PowerShell-Skripts ein Dienstkonto mit einem sicheren Kennwort erstellen und für dieses Konto die Überprüfung in zwei Schritten nicht aktivieren.

> [!WARNING]
> App-Kennwörter funktionieren nicht in Hybridumgebungen, in denen Clients sowohl mit lokalen AutoErmittlung-Endpunkten als auch mit solchen in der Cloud kommunizieren. Domänenkennwörter sind für die lokale Authentifizierung und App-Kennwörter für die Authentifizierung in der Cloud erforderlich.

### <a name="naming-guidance-for-app-passwords"></a>Benennungsrichtlinien für App-Kennwörter
Die Namen der App-Kennwörter sollten auf das Gerät hinweisen, auf dem sie verwendet werden. Wenn Sie beispielsweise einen Laptop haben, der über Nicht-Browser-Apps verfügt, erstellen Sie ein App-Kennwort mit dem Namen „Laptop“ und verwenden dieses App-Kennwort. Anschließend erstellen Sie ein weiteres App-Kennwort namens „Desktop“ für die gleichen Anwendungen auf dem Desktopcomputer. 

Microsoft empfiehlt, jeweils ein App-Kennwort pro Gerät (und nicht pro Anwendung) zu erstellen.

### <a name="federated-sso-app-passwords"></a>App-Kennwörter im Verbund (SSO)
Azure AD unterstützt den Verbund (Einmaliges Anmelden) mit lokalen Windows Server Active Directory Domain Services (AD DS). Wenn Ihre Organisation im Verbund mit Azure AD besteht und Sie Azure Multi-Factor Authentication verwenden möchten, sind die Informationen zu App-Kennwörtern wichtig für Sie. Dieser Abschnitt gilt nur für Verbundkunden (SSO).

* App-Kennwörter werden von Azure AD überprüft, sodass der Verbund umgangen wird. Der Verbund wird nur beim Einrichten von App-Kennwörtern aktiv verwendet.
* Im Gegensatz zum passiven Ablauf gehen wir bei Verbundbenutzern (SSO) nie zum Identitätsanbieter (Identity Provider, IdP). Die Kennwörter werden in der Organisations-ID gespeichert. Wenn der Benutzer das Unternehmen verlässt, muss diese Information an die Organisations-ID übergeben werden, und zwar mithilfe von DirSync in Echtzeit. Nach dem Deaktivieren oder Löschen von Konten kann die Synchronisierung bis zu drei Stunden dauern, sodass das Deaktivieren bzw. Löschen des App-Kennworts in Azure AD verzögert wird.
* Lokale Einstellungen für die Clientzugriffssteuerung werden vom App-Kennwort nicht berücksichtigt.
* Für das App-Kennwort ist keine lokale Funktion zur Protokollierung oder Überwachung der Authentifizierung verfügbar.
* In bestimmten erweiterten Architekturentwürfen ist bei der Verwendung der Überprüfung in zwei Schritten mit Clients möglicherweise eine Kombination aus Organisationsbenutzername/-kennwörtern und App-Kennwörtern erforderlich. Dies hängt jedoch davon ab, wo Sie sich authentifizieren. Bei Clients, die eine lokale Infrastruktur authentifizieren, verwenden Sie einen Organisationsbenutzernamen und -kennwort. Für Clients, die bei Azure AD authentifizieren, verwenden Sie das App-Kennwort.

  Nehmen wir beispielsweise an, Sie verfügen über eine Architektur, die folgende Instanzen umfasst:

  * Sie werden Ihre lokale Instanz von Active Directory mit Azure AD in Verbund setzen.
  * Sie verwenden Exchange online.
  * Sie verwenden Lync, das speziell lokal ist.
  * Sie verwenden Azure Multi-Factor Authentication,

  ![Proofup](./media/multi-factor-authentication-whats-next/federated.png)

  In diesen Instanzen müssen Sie die folgenden Schritte ausführen:

  * Verwenden Sie beim Anmelden bei Lync den Benutzernamen und das Kennwort Ihres Unternehmens.
  * Verwenden Sie ein App-Kennwort beim Versuch, über einen Outlook-Client auf das Adressbuch zuzugreifen, der die Verbindung mit Exchange Online herstellt.

### <a name="allow-app-password-creation"></a>Zulassen der Erstellung von App-Kennwörtern
Standardmäßig können Benutzer keine App-Kennwörter erstellen, Sie können jedoch das Feature selbst aktivieren. Gehen Sie wie folgt vor, um Benutzern die Erstellung von App-Kennwörtern zu ermöglichen:

1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie gemäß den Anweisungen am Anfang dieses Artikels zur Seite mit den MFA-Diensteinstellungen.
3. Wählen Sie das Optionsfeld neben **Benutzern das Erstellen von App-Kennwörtern zum Anmelden bei Anwendungen gestatten, die nicht auf Browsern basieren**.

![App-Kennwörter erstellen](./media/multi-factor-authentication-whats-next/trustedips3.png)

### <a name="create-app-passwords"></a>Erstellen von App-Kennwörter
Benutzer können App-Kennwörter während ihrer ersten Registrierung erstellen. Am Ende des Registrierungsprozesses wird eine Option angezeigt, mit der sie App-Kennwörter erstellen können.

Benutzer können auch App-Kennwörter nach der Registrierung erstellen. Sie können zum Erstellen der App-Kennwörter ihre Einstellungen im Azure-Portal oder im Office 365-Portal ändern. Weitere Informationen und detaillierte Schritte für Ihre Benutzer finden Sie unter [Welchen Zweck erfüllen App-Kennwörter bei Azure Multi-Factor Authentication?](./end-user/multi-factor-authentication-end-user-app-passwords.md).

## <a name="remember-multi-factor-authentication-for-devices-that-users-trust"></a>Speichern der Multi-Factor Authentication für Geräte, denen Benutzer vertrauen
Die Speicherung der Multi-Factor Authentication für Geräte und Browser, denen Benutzer vertrauen, ist eine kostenlose Funktion für alle MFA-Benutzer. Multi-Factor Authentication ermöglicht es den Benutzern, die MFA für eine bestimmte Anzahl von Tagen nach der Anmeldung zu umgehen. Bei Verwendung dieses Features muss der Benutzer auf einem Gerät seltener eine Überprüfung in zwei Schritten durchführen, was die Benutzerfreundlichkeit verbessert.

Ist ein Konto oder Gerät gefährdet, kann das Speichern von MFA für vertrauenswürdige Geräte jedoch die Sicherheit beeinträchtigen. Wenn ein Unternehmenskonto kompromittiert wird oder ein vertrauenswürdiges Gerät verloren geht oder gestohlen wird, müssen Sie die [Multi-Factor Authentication auf allen Geräten wiederherstellen](multi-factor-authentication-manage-users-and-devices.md#restore-mfa-on-all-remembered-devices-for-a-user). Durch diese Aktion wird der vertrauenswürdige Status aller Geräte widerrufen, und der Benutzer muss wieder die Überprüfung in zwei Schritten ausführen. Sie können Ihre Benutzer auch anweisen, MFA auf ihren eigenen Geräten anhand der Anweisungen unter [Verwalten der Einstellungen für die Überprüfung in zwei Schritten](./end-user/multi-factor-authentication-end-user-manage-settings.md#require-two-step-verification-again-on-a-device-youve-marked-as-trusted) wiederherzustellen.

### <a name="how-it-works"></a>So funktioniert's

Das Speichern der Multi-Factor Authentication funktioniert durch das Festlegen eines permanenten Cookies im Browser, wenn ein Benutzer das Kontrollkästchen „Die nächsten **X** Tage nicht erneut fragen“ aktiviert. Der Benutzer wird von diesem Browser bis zum Ablauf des Cookies nicht erneut zur MFA aufgefordert. Wenn der Benutzer einen anderen Browser auf dem gleichen Gerät öffnet oder seine Cookies löscht, wird er wieder zur Verifizierung aufgefordert. 

Das Kontrollkästchen „Die nächsten **X** Tage nicht erneut fragen“ wird in Nicht-Browser-Apps nicht angezeigt, unabhängig davon, ob sie die moderne Authentifizierung unterstützen. Diese Apps verwenden Aktualisierungstoken, die jede Stunde neue Zugriffstoken bereitstellen. Bei der Überprüfung eines Aktualisierungstokens überprüft Azure AD den letzten Zeitpunkt innerhalb der konfigurierten Anzahl von Tagen, an dem die Überprüfung in zwei Schritten durchgeführt wurde. 

Das bedeutet, dass das Speichern der MFA auf einem vertrauenswürdigen Gerät die Anzahl der Authentifizierungen in Web-Apps (die normalerweise jedes Mal zur Authentifizierung auffordern) verringert. Gleichzeitig erhöht sich jedoch die Anzahl der Authentifizierungen für Clients mit moderner Authentifizierung (die normalerweise alle 90 Tage zur Authentifizierung auffordern).

> [!NOTE]
>Dieses Feature ist nicht kompatibel mit dem AD FS-Feature „Angemeldet bleiben“, bei dem Benutzer die Überprüfung in zwei Schritten für AD FS über Azure MFA Server oder eine MFA-Lösung von Drittanbietern ausführen. Wenn Ihre Benutzer in AD FS „Angemeldet bleiben“ auswählen und ihr Gerät außerdem als für MFA vertrauenswürdig markieren, können sie sich nach Ablauf der Anzahl von Tagen, die unter „MFA speichern“ angegeben wurde, nicht mehr verifizieren. Azure AD fordert eine neue Überprüfung in zwei Schritten an, aber AD FS gibt ein Token mit dem ursprünglichen MFA-Anspruch und Datum zurück, statt die Überprüfung in zwei Schritten erneut durchzuführen. Dadurch entsteht eine Schleife bei der Überprüfung zwischen Azure AD und AD FS. 

### <a name="enable-remember-multi-factor-authentication"></a>Aktivieren der Speicherung der mehrstufigen Authentifizierung
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie gemäß den Anweisungen am Anfang dieses Artikels zur Seite mit den MFA-Diensteinstellungen.
3. Aktivieren Sie auf der Seite „Diensteinstellungen“ unter „Geräteeinstellungen von Benutzern verwalten“ die Option **Benutzern das Speichern der mehrstufigen Authentifizierung auf vertrauenswürdigen Geräten ermöglichen**.
   ![Speichern von Geräten](./media/multi-factor-authentication-whats-next/remember.png)
4. Legen Sie fest, für wie viele Tage die vertrauenswürdigen Geräte die Überprüfung in zwei Schritten umgehen können. Der Standardwert ist 14 Tage.
5. Klicken Sie auf **Speichern**.
6. Klicken Sie auf **Schließen**.

### <a name="mark-a-device-as-trusted"></a>Markieren eines Geräts als vertrauenswürdig

Nach der Aktivierung dieser Funktion können Benutzer bei der Anmeldung ein Gerät als vertrauenswürdig markieren, indem sie **Nicht mehr nachfragen** aktivieren.

![Nicht mehr nachfragen (Screenshot)](./media/multi-factor-authentication-whats-next/trusted.png)

## <a name="selectable-verification-methods"></a>Auswählbare Verifizierungsmethoden
Sie können auswählen, welche Verifizierungsmethoden für Ihre Benutzer verfügbar sind. In der folgenden Tabelle finden Sie eine kurze Übersicht über jede Methode.

Wenn Ihre Benutzer Ihre Konten für MFA registrieren, wählen sie ihre bevorzugte Verifizierungsmethode aus den Optionen aus, die Sie zur Verfügung gestellt haben. Anleitungen zum Registrierungsprozess finden Sie unter [Einrichten meines Kontos für die Überprüfung in zwei Schritten](multi-factor-authentication-end-user-first-time.md)

| Methode | Beschreibung |
|:--- |:--- |
| Auf Telefon anrufen |Startet einen automatisierten Sprachanruf. Der Benutzer nimmt den Anruf an und drückt die #-Taste auf der Telefontastatur, um sich zu authentifizieren. Diese Telefonnummer wird nicht mit dem lokalen Active Directory synchronisiert. |
| Textnachricht an Telefon |Sendet eine Textnachricht mit einem Überprüfungscode. Der Benutzer wird aufgefordert, mit dem Überprüfungscode auf die Textnachricht zu antworten oder den Überprüfungscode auf der Anmeldeseite einzugeben. |
| Benachrichtigung über mobile App |Sendet eine Pushbenachrichtigung an Ihr Telefon oder registriertes Gerät. Der Benutzer zeigt die Benachrichtigung an und wählt **Überprüfen**, um die Überprüfung abzuschließen. <br>Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar. |
| Überprüfungscode von der mobilen App |Die Microsoft Authenticator-App generiert alle 30 Sekunden einen neuen OATH-Überprüfungscode. Der Benutzer gibt diesen Überprüfungscode auf der Anmeldeoberfläche ein.<br>Die Microsoft Authenticator-App ist für [Windows Phone](http://go.microsoft.com/fwlink/?Linkid=825071), [Android](http://go.microsoft.com/fwlink/?Linkid=825072) und [IOS](http://go.microsoft.com/fwlink/?Linkid=825073) verfügbar. |

### <a name="how-to-enabledisable-authentication-methods"></a>Aktivieren/Deaktivieren von Authentifizierungsmethoden
1. Melden Sie sich beim [klassischen Azure-Portal](https://manage.windowsazure.com)an.
2. Navigieren Sie gemäß den Anweisungen am Anfang dieses Artikels zur Seite mit den MFA-Diensteinstellungen.
3. Aktivieren oder deaktivieren Sie auf der Seite „Diensteinstellungen“ unter „Überprüfungsoptionen“ die Optionen, die Sie verwenden bzw. nicht verwenden möchten.
   ![Überprüfungsoptionen](./media/multi-factor-authentication-whats-next/authmethods.png)
4. Klicken Sie auf **Speichern**.
5. Klicken Sie auf **Schließen**.
