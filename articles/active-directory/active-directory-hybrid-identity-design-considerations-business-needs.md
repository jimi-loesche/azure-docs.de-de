---
title: "Überlegungen zum Entwurf der Azure Active Directory-Hybrid-Identität – Ermitteln der Identitätsanforderungen | Microsoft Docs"
description: "Identifizieren der geschäftlichen Anforderungen des Unternehmens, mit denen Sie die Anforderungen für den Entwurf der Hybrid-Identität definieren."
documentationcenter: 
services: active-directory
author: billmath
manager: femila
editor: 
ms.assetid: de690978-84ef-41ad-9dfe-785722d343a1
ms.service: active-directory
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 07/18/2017
ms.author: billmath
ms.openlocfilehash: 6503034b3f5a17a2a42338c73329eef0b01f2f27
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="determine-identity-requirements-for-your-hybrid-identity-solution"></a>Ermitteln der Identitätsanforderungen für Ihre Hybrid-Identitätslösung
Der erste Schritt beim Entwickeln einer Hybrid-Identitätslösung ist, die Anforderungen für das Unternehmen zu bestimmen, das diese Lösung nutzen wird.  Die Hybrid-Identität beginnt als unterstützende Rolle (unterstützt alle anderen Cloud-Lösungen durch Authentifizierung) und bietet dann neue und interessante Funktionen, die Benutzern neue Workloads ermöglichen.  Diese Workloads oder Dienste, die Sie für Ihre Benutzer integrieren möchten, bestimmen die Anforderungen an die Gestaltung der Hybrid-Identität.  Diese Dienste und Workloads müssen Hybrid-Identität sowohl lokal aus auch in der Cloud nutzen.  

Sie müssen diese wichtige Aspekte des Unternehmens prüfen, um zu verstehen, welche Anforderung jetzt vorliegen und was das Unternehmen für die Zukunft plant. Wenn Ihnen die langfristige Strategie für die Entwicklung der Hybrid-Identität fehlt, besteht das Risiko, dass Ihre Lösung nicht skaliert werden kann, wenn Ihr Unternehmen Wachstum und Veränderung braucht.   Das folgende Diagramm zeigt ein Beispiel für eine Hybrid-Identitätsarchitektur und die Workloads, die sich Benutzern eröffnen. Dies ist nur ein Beispiel für die neuen Möglichkeiten, die sich Ihnen durch eine solide Hybrid-Identitätsstrategie eröffnen können. 

Einige Komponenten, die Teil der Hybrid-Identitätsarchitektur sind ![](./media/hybrid-id-design-considerations/hybrid-identity-architechture.png)

## <a name="determine-business-needs"></a>Ermitteln von Geschäftsanforderungen
Jedes Unternehmen hat unterschiedliche Anforderungen – auch wenn diese Unternehmen derselben Branche angehören, können die tatsächlichen geschäftlichen Anforderungen variieren. Sie können weiterhin Best Practices der Branche nutzen, doch letztendlich bestimmen die geschäftlichen Anforderungen des Unternehmens die Anforderungen an die Gestaltung der Hybrid-Identität. 

Stellen Sie sich die folgenden Fragen, um Ihre geschäftlichen Anforderungen zu identifizieren:

* Möchte Ihr Unternehmen die IT-Betriebskosten senken?
* Möchte Ihr Unternehmen Cloud-Ressourcen (SaaS-Apps, Infrastruktur) sichern?
* Möchte Ihr Unternehmen Ihre IT modernisieren?
  * Sind Ihre Benutzer mobiler und erwarten von der IT Ausnahmen in Ihrer DMZ für den Zugriff verschiedener Arten von Datenverkehr auf verschiedene Ressourcen?
  * Verfügt Ihr Unternehmen über Legacy-Apps, die für diese moderne Benutzer veröffentlicht werden sollen, aber nicht einfach umzuschreiben sind?
  * Muss Ihr Unternehmen alle diese Aufgaben erledigen und gleichzeitig unter Kontrolle bringen?
* Möchte Ihr Unternehmen die Benutzeridentitäten schützen und das Risiko durch neue Tools verringern, die die Sicherheitskompetenz von Microsoft Azure lokal nutzen?
* Möchte Ihr Unternehmen die gefürchteten „externen“ Konten in der lokalen Infrastruktur entfernen und in die Cloud auslagern, sodass sie kein ruhendes Risiko mehr für Ihre lokale Umgebung darstellen?

## <a name="analyze-on-premises-identity-infrastructure"></a>Analysieren Ihrer lokalen Identitätsinfrastruktur
Da Sie nun eine Vorstellung von den Bedürfnissen Ihres Unternehmens haben, müssen Sie Ihre lokale Identitätsinfrastruktur prüfen. Das ist wichtig für die Definition der technischen Anforderungen zum Integrieren Ihrer aktuellen Identitätslösung in das Cloud-Identitätsverwaltungssystem. Beantworten Sie die folgenden Fragen:

* Welche Authentifizierungs- und Autorisierungslösung verwendet Ihr Unternehmen lokal? 
* Nutzt Ihr Unternehmen derzeit lokale Synchronisierungsdienste?
* Nimmt Ihr Unternehmen Drittanbieter-Identitätsanbieter (IdP) in Anspruch?

Außerdem müssen Sie wissen, welche Clouddienste in Ihrem Unternehmen genutzt werden. Bewertungen zum Verstehen der aktuellen Integration in Saas-, Iaas oder Paas-Modelle in Ihrer Umgebung sind äußerst wichtig. Beantworten Sie bei dieser Bewertung die folgenden Fragen:

* Besteht bei Ihrem Unternehmen eine Integration mit einem Clouddienstanbieter?
* Falls ja, welche Dienste werden genutzt?
* Befindet sich diese Integration derzeit im Produktivbetrieb oder in einer Pilotphase?

> [!NOTE]
> Wenn Sie keine genaue Zuordnung all Ihrer Apps und Clouddienste haben, können Sie das Cloud App Discovery-Tool nutzen. Mit diesem Tool erhält Ihre IT-Abteilung Einblick in alle geschäftlichen und privaten Cloud-Apps in Ihrem Unternehmen. Mit diesem Modul ist es einfacher denn je, Schatten-IT in Ihrem Unternehmen zu ermitteln. Dies umfasst u. a. Einzelheiten zu Nutzungsmustern und sämtlichen Benutzern, die auf Ihre Cloudanwendungen zugreifen. Informationen zum Einstieg finden Sie unter [Cloud App Discovery](active-directory-cloudappdiscovery-whatis.md).
> 
> 

## <a name="evaluate-identity-integration-requirements"></a>Identitäts-Integrationsanforderungen bewerten
Als Nächstes müssen Sie die Identitäts-Integrationsanforderungen bewerten. Diese Bewertung ist wichtig, um die technischen Vorschriften für die Benutzerauthentifizierung, die Gestaltung der Unternehmenspräsenz in der Cloud, die Autorisierung und die Gestaltung des Benutzererlebnisses zu definieren. Beantworten Sie die folgenden Fragen:

* Wird Ihre Organisation eine Verbundauthentifizierung, Standardauthentifizierung oder beides verwenden?
* Ist die Verbundauthentifizierung erforderlich?  Aus folgenden Gründen:
  * Kerberos-basierte SSO
  * Ihr Unternehmen verfügt über eine lokale Anwendung (intern oder extern entwickelt), die SAML oder vergleichbare Verbunddienste nutzt.
  * MFA über Smartcards. RSA SecurID usw.
  * Client-Zugriffsregeln zu den folgenden Fragen:
    1. Kann ich den gesamten externen Zugriff auf Office 365 basierend auf der IP-Adresse des Clients sperren?
    2. Kann ich den gesamten externen Zugriff auf Office 365, mit Ausnahme von Exchange ActiveSync, sperren?
    3. Kann ich den gesamten externen Zugriff auf Office 365, mit Ausnahme der browserbasierten Apps (OWA, SPO), sperren?
    4. Kann ich den gesamten externen Zugriff auf Office 365 für Mitglieder der angegebenen AD-Gruppen blockieren?
* Sicherheits-/Überwachungsprobleme
* Bereits vorhandene Investitionen in Verbundauthentifizierung
* Welchen Namen wird unsere Organisation für die Domäne in der Cloud verwenden?
* Verfügt die Organisation eine benutzerdefinierte Domäne?
  1. Ist diese Domäne öffentlich und einfach über DNS prüfbar?
  2. Wenn das nicht der Fall ist, verfügen Sie über eine öffentliche Domäne, die zum Registrieren eines alternativen UPN in Active Directory verwendet werden kann.
* Sind die Benutzerkennungen für die Cloud-Darstellung konsistent? 
* Verfügt das Unternehmen über Apps, die für die Integration mit Cloud-Diensten erforderlich sind?
* Verfügt die Organisation über mehrere Domänen und verwenden diese alle die Standard- oder Verbund-Authentifizierung?

## <a name="evaluate-applications-that-run-in-your-environment"></a>Anwendungen bewerten, die in Ihrer Umgebung ausgeführt werden 
Da Sie nun eine Vorstellung von Ihrer lokalen und Cloud-Infrastruktur haben, müssen Sie die Anwendungen bewerten, die in diesen Umgebungen ausgeführt werden. Diese Auswertung ist wichtig, um die technischen Anforderungen zum Integrieren dieser Anwendungen in das Cloud-Identitätsverwaltungssystem zu definieren. Beantworten Sie die folgenden Fragen:

* Wo werden unsere Anwendungen ausgeführt?
* Werden Benutzer auf lokale Anwendungen zugreifen?  In der Cloud? Oder beides?
* Gibt es Pläne, die vorhandenen Anwendungsworkloads in die Cloud auszulagern?
* Gibt es Pläne, neue lokal oder in der Cloud gespeicherte Anwendungen zu entwickeln, die auf die Cloud-Authentifizierung zugreifen?

## <a name="evaluate-user-requirements"></a>Benutzeranforderungen bewerten
Sie müssen zudem die Benutzeranforderungen bewerten. Diese Bewertung ist wichtig für die Definition der Schritte, die für die Integration und Unterstützung von Benutzern beim Umstieg auf die Cloud benötigt werden. Beantworten Sie die folgenden Fragen:

* Werden Benutzer lokal auf Anwendungen zugreifen?
* Werden Benutzer in der Cloud auf Anwendungen zugreifen?
* Wie melden sich Benutzer in der Regel bei ihrer lokalen Umgebung an?
* Wie werden sich Benutzer in der Cloud anmelden?

> [!NOTE]
> Notieren Sie sich alle Antworten, und stellen Sie sicher, dass Ihnen die Begründung der Antwort jeweils klar ist. [Bestimmen der Anforderungen an Reaktionen auf Vorfälle](active-directory-hybrid-identity-design-considerations-incident-response-requirements.md) sind die verfügbaren Optionen und die Pros und Kontras jeder Option beschrieben.  Indem Sie diese Fragen beantworten, wählen Sie aus, welche Option Ihre Geschäftsanforderungen am besten erfüllt.
> 
> 

## <a name="next-steps"></a>Nächste Schritte
[Ermitteln der Anforderungen an die Verzeichnissynchronisierung](active-directory-hybrid-identity-design-considerations-directory-sync-requirements.md)

## <a name="see-also"></a>Siehe auch
[Überlegungen zum Entwurf – Übersicht](active-directory-hybrid-identity-design-considerations-overview.md)

