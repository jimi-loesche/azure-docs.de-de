---
title: Azure Active Directory Identity Protection-Benachrichtigungen | Microsoft Docs
description: "Erfahren Sie, wie Benachrichtigungen Ihre Untersuchungsaktivitäten unterstützen."
services: active-directory
keywords: Azure Active Directory Identity Protection, Cloud App Discovery, Verwalten von Anwendungen, Sicherheit, Risiko, Risikostufe, Sicherheitsrisiko, Sicherheitsrichtlinie
documentationcenter: 
author: MarkusVi
manager: femila
editor: 
ms.assetid: 65ca79b9-4da1-4d5b-bebd-eda776cc32c7
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 10/17/2017
ms.author: markvi
ms.reviewer: nigu
ms.openlocfilehash: abc0f3926905295a9cf239146cce7fc57da7eb29
ms.sourcegitcommit: 6acb46cfc07f8fade42aff1e3f1c578aa9150c73
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/18/2017
---
# <a name="azure-active-directory-identity-protection-notifications"></a>Azure Active Directory Identity Protection Benachrichtigungen
Von Azure AD Identity Protection werden zwei Arten von automatisierten Benachrichtigungs-E-Mails gesendet, die der Verwaltung von Benutzerrisiko- und Risikoereignissen dienen:

* E-Mail als Warnung vor kompromittiertem Benutzer
* Wöchentliche E-Mail mit Übersicht

## <a name="user-compromised-alert-email"></a>E-Mail als Warnung vor kompromittiertem Benutzer
Eine E-Mail als Warnung vor einem kompromittiertem Benutzer wird generiert, wenn ein Konto von Azure AD Identity Protection als kompromittiert erkannt wird. Die E-Mail enthält einen Link zum Bericht vom Typ „Benutzer mit Risikokennzeichnung“ im Identity Protection Dashboard. Es wird empfohlen, dass Sie Benachrichtigungen zu kompromittierten Konten sofort untersuchen.

## <a name="weekly-digest-email"></a>Wöchentliche E-Mail mit Übersicht
Die wöchentliche E-Mail mit einer Übersicht enthält eine Zusammenfassung der neuen Risikoereignisse.<br>
Sie hat folgenden Inhalt:

* Gefährdete Benutzer
* Verdächtige Aktivitäten
* Erkannte Sicherheitsrisiken
* Links zu verwandten Berichten in Identity Protection

<br>
![Wiederherstellung](./media/active-directory-identityprotection-notifications/400.png "Wiederherstellung")
<br>

Sie können das Senden einer wöchentlichen Übersichts-E-Mail deaktivieren.
<br><br>
![Benutzerrisiken](./media/active-directory-identityprotection-notifications/62.png "Benutzerrisiken")
<br>

**Um den entsprechenden Konfigurationsdialog zu öffnen**:

1. Klicken Sie auf dem Blatt **Azure AD Identity Protection** auf **Einstellungen**.
   <br><br>
   ![Richtlinie zum Benutzerrisiko](./media/active-directory-identityprotection-notifications/401.png "Richtlinie zum Benutzerrisiko")
   <br>
2. Klicken Sie im Abschnitt **Allgemein** auf **Benachrichtigungen**.
   <br><br>
   ![Richtlinie zum Benutzerrisiko](./media/active-directory-identityprotection-notifications/405.png "Richtlinie zum Benutzerrisiko")
   <br>

## <a name="see-also"></a>Weitere Informationen
* [Azure Active Directory Identity Protection](active-directory-identityprotection.md)
