---
title: "Erstellen, Ändern oder Löschen einer öffentlichen Azure-IP-Adresse | Microsoft-Dokumentation"
description: "Erfahren Sie, wie Sie eine öffentliche IP-Adresse erstellen, ändern oder löschen können."
services: virtual-network
documentationcenter: na
author: jimdial
manager: jeconnoc
editor: 
tags: azure-resource-manager
ms.assetid: bb71abaf-b2d9-4147-b607-38067a10caf6
ms.service: virtual-network
ms.devlang: NA
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/25/2017
ms.author: jdial
ms.openlocfilehash: 5bffea350061231e1dc664b3abcbf79950a54402
ms.sourcegitcommit: 6699c77dcbd5f8a1a2f21fba3d0a0005ac9ed6b7
ms.translationtype: HT
ms.contentlocale: de-DE
ms.lasthandoff: 10/11/2017
---
# <a name="create-change-or-delete-a-public-ip-address"></a>Erstellen, Ändern oder Löschen einer öffentlichen IP-Adresse

Sie erhalten Informationen über öffentliche IP-Adressen und darüber, wie Sie diese erstellen, ändern und löschen können. Eine öffentliche IP-Adresse ist eine Ressource mit eigenen konfigurierbaren Einstellungen. Wird anderen Azure-Ressourcen eine öffentliche IP-Adresse zugewiesen, wird Folgendes ermöglicht:
- Eingehende Internetkonnektivität mit Ressourcen wie Azure Virtual Machines, Azure-VM-Skalierungsgruppen, Azure-VPN-Gateway, Anwendungsgateways und internetfähigen Instanzen von Azure Load Balancer. Azure-Ressourcen können ohne zugewiesene öffentliche IP-Adresse keine eingehende Kommunikation aus dem Internet empfangen. Bei einigen Azure-Ressourcen ist der Zugriff über öffentliche IP-Adressen von vorneherein gegeben, anderen Ressourcen muss eine IP-Adresse zugewiesen werden, damit sie aus dem Internet erreichbar sind.
- Ausgehende Konnektivität mit dem Internet über eine vorhersagbare IP-Adresse. Beispielsweise kann ein virtueller Computer, obwohl ihm keine öffentliche IP-Adresse zugewiesen ist, ausgehend mit dem Internet kommunizieren, weil seine Netzwerkadresse von Azure in eine nicht vorhersagbare öffentliche Adresse übersetzt wird. Indem Sie Ressourcen eine öffentliche IP-Adresse zuweisen, wissen Sie, welche IP-Adresse für die ausgehende Verbindung verwendet wird. Eine solche Adresse ist zwar vorhersehbar, kann sich aber ändern – je nach ausgewählter Zuweisungsmethode. Weitere Informationen finden Sie unter [Erstellen einer öffentlichen IP-Adresse](#create-a-public-ip-address). Weitere Informationen zu ausgehenden Verbindungen von Azure-Ressourcen finden Sie im Artikel [Grundlegendes zu ausgehenden Verbindungen in Azure](../load-balancer/load-balancer-outbound-connections.md?toc=%2fazure%2fvirtual-network%2ftoc.json).

## <a name="before-you-begin"></a>Voraussetzungen

Führen Sie zuerst die folgenden Aufgaben aus, ehe Sie die Schritte in den Abschnitten dieses Artikels durchführen:

- Lesen Sie den Artikel zu [Einschränkungen bei Azure](../azure-subscription-service-limits.md?toc=%2fazure%2fvirtual-network%2ftoc.json#azure-resource-manager-virtual-networking-limits), um mehr über die Einschränkungen für öffentliche IP-Adressen zu erfahren.
- Melden Sie sich mit einem Azure-Konto beim Azure-[Portal](https://portal.azure.com), der Azure-Befehlszeilenschnittstelle (CLI) oder bei Azure PowerShell an. Falls Sie noch nicht über ein Azure-Konto verfügen, können Sie sich für ein [kostenloses Testkonto](https://azure.microsoft.com/free) registrieren.
- Wenn Sie PowerShell-Befehle verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure PowerShell](/powershell/azureps-cmdlets-docs?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure PowerShell-Cmdlets installiert haben. Hilfe und Beispiele für PowerShell-Befehle erhalten Sie durch Eingabe von `get-help <command> -full`.
- Wenn Sie Befehle der Azure-Befehlszeilenschnittstelle (CLI) verwenden, um Aufgaben in diesem Artikel durchzuführen, [installieren und konfigurieren Sie Azure CLI](/cli/azure/install-azure-cli?toc=%2fazure%2fvirtual-network%2ftoc.json). Stellen Sie sicher, dass Sie die neueste Version der Azure CLI installiert haben. Hilfe zu den Befehlen der Befehlszeilenschnittstelle erhalten Sie durch Eingabe von `az <command> --help`. Anstatt die CLI und ihre Voraussetzungen zu installieren, können Sie die Azure Cloud Shell verwenden. Azure Cloud Shell ist eine kostenlose Bash-Shell, die Sie direkt im Azure-Portal ausführen können. Die Azure CLI ist vorinstalliert und für die Verwendung mit Ihrem Konto konfiguriert. Wenn Sie die Cloud Shell verwenden möchten, klicken Sie oben im [Portal](https://portal.azure.com) auf die Cloud Shell-Schaltfläche **>_**.

Für öffentliche IP-Adressen fällt eine Schutzgebühr an. Informationen zu den Preisen finden Sie auf der Seite [Preise für IP-Adressen](https://azure.microsoft.com/pricing/details/ip-addresses). 

## <a name="create-a-public-ip-address"></a>Erstellen einer öffentlichen IP-Adresse

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *öffentliche IP-Adresse* ein. Wenn **Öffentliche IP-Adressen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem Blatt **Öffentliche IP-Adresse** auf **+ Hinzufügen**.
4. Das Blatt **Öffentliche IP-Adresse erstellen** wird angezeigt. Geben Sie hier direkt oder durch Auswählen Werte für die folgenden Einstellungen ein, und klicken Sie dann auf **Erstellen**:

    |Einstellung|Erforderlich|Details|
    |---|---|---|
    |SKU|Ja|Alle öffentlichen IP-Adressen, die vor der Einführung von SKUs erstellt wurden, sind öffentliche IP-Adressen für eine **Basic**-SKU.  Sie können die SKU nicht ändern, nachdem die öffentliche IP-Adresse erstellt wurde. Ein eigenständiger virtueller Computer, virtuelle Computer innerhalb einer Verfügbarkeitsgruppe oder VM-Skalierungsgruppen können Basic- oder Standard-SKUs verwenden.  Das Mischen von SKUs zwischen virtuellen Computern in Verfügbarkeitsgruppen oder Skalierungsgruppen ist nicht zulässig. **Basic**-SKU: Wenn Sie eine öffentliche IP-Adresse in einer Region erstellen, die Verfügbarkeitszonen unterstützt, wird die Einstellung **Verfügbarkeitszone** standardmäßig auf *Keine* festgelegt. Sie können eine Verfügbarkeitszone auswählen, um eine bestimmte Zone für die öffentliche IP-Adresse zu gewährleisten. **Standard**-SKU: Eine öffentliche IP-Adresse für eine Standard-SKU kann dem Front-End eines virtuellen Computers oder Load Balancers zugeordnet werden. Wenn Sie eine öffentliche IP-Adresse in einer Region erstellen, die Verfügbarkeitszonen unterstützt, wird die Einstellung **Verfügbarkeitszone** standardmäßig auf *Zonenredundant* festgelegt. Weitere Informationen zu Verfügbarkeitszonen finden Sie im Abschnitt **Verfügbarkeitszone**. Die Standard-SKU ist erforderlich, wenn Sie die Adresse einem Standard-Load Balancer zuordnen. Weitere Informationen zum Standard-Load Balancer finden Sie unter [Azure Load Balancer Standard overview](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json) (Übersicht über den Standard-Azure Load Balancer, in englischer Sprache). Die Standard-SKU befindet sich in der Vorschauversion. Vor dem Erstellen einer öffentlichen IP-Adresse für eine Standard-SKU müssen Sie die Schritte zum [Registrieren für die Vorschauversion der Standard-SKU](#register-for-the-standard-sku-preview) ausführen und die öffentliche IP-Adresse an einem unterstützten Standort (Region) erstellen. Eine Liste mit den unterstützten Standorten finden Sie unter [Region availability](../load-balancer/load-balancer-standard-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#region-availability) (Regionale Verfügbarkeit, in englischer Sprache). Auf der Seite [Azure Virtual Network updates](https://azure.microsoft.com/updates/?product=virtual-network) (Updates für Azure Virtual Network, in englischer Sprache) erhalten Sie außerdem zusätzlichen Support für Regionen. Wenn Sie der Netzwerkschnittstelle eines virtuellen Computers eine öffentliche IP-Adresse für eine Standard-SKU zuweisen, müssen Sie den geplanten Datenverkehr explizit mit einer [Netzwerksicherheitsgruppe](security-overview.md#network-security-groups) zulassen. Die Kommunikation mit der Ressource schlägt fehl, bis Sie eine Netzwerksicherheitsgruppe erstellen und zuordnen und den gewünschten Datenverkehr explizit zulassen.|
    |Name|Ja|Der Name muss innerhalb der ausgewählten Ressourcengruppe eindeutig sein.|
    |IP-Version|Ja| Wählen Sie IPv4 oder IPv6 aus. Öffentliche IPv4-Adressen können verschiedenen Azure-Ressourcen zugewiesen werden, eine öffentliche IPv6-Adresse dagegen kann nur einem Load Balancer mit Internetzugriff zugewiesen werden. Der Load Balancer kann den Lastenausgleich für den IPv6-Datenverkehr zu virtuellen Azure-Computern durchführen. Hier erfahren Sie mehr über den [Lastenausgleich des IPv6-Datenverkehrs zu virtuellen Computern](../load-balancer/load-balancer-ipv6-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Wenn Sie die **Standard-SKU** ausgewählt haben, ist *IPv6* nicht zur Auswahl verfügbar. Bei Verwendung der **Standard-SKU** können Sie nur eine IPv4-Adresse erstellen.|
    |IP-Adresszuweisung|Ja|**Dynamisch:** Dynamische Adressen werden nur zugewiesen, nachdem die öffentliche IP-Adresse einer Netzwerkschnittstelle zugewiesen wurde, die mit einem virtuellen Computer verbunden ist, und der virtuelle Computer erstmalig gestartet wurde. Dynamische Adressen können sich ändern, wenn der virtuelle Computer, mit dem die Netzwerkschnittstelle verbunden ist, beendet (Zuordnung aufgehoben) wird. Die Adresse bleibt unverändert, wenn der virtuelle Computer neu gestartet oder (ohne Aufheben der Zuordnung) beendet wird. **Statisch:** Statische Adressen werden zugewiesen, wenn die öffentliche IP-Adresse erstellt wird. Statische Adressen ändern sich nicht, selbst wenn der virtuelle Computer in den Status „Beendet (Zuordnung aufgehoben)“ versetzt wird. Die Adresse wird nur freigegeben, wenn die Netzwerkschnittstelle gelöscht wird. Die Zuweisungsmethode kann nach Erstellung der Netzwerkschnittstelle geändert werden. Wenn Sie *IPv6* als **IP-Version** auswählen, lautet die Zuweisungsmethode *Dynamisch*. Wenn Sie *Standard* für **SKU** auswählen, lautet die Zuweisungsmethode *Statisch*.|
    |Leerlaufzeitüberschreitung (Minuten)|Nein|Gibt an, wie viele Minuten eine TCP- oder HTTP-Verbindung geöffnet bleiben soll, ohne dass Clients Keep-Alive-Meldungen senden müssen. Wenn Sie IPv6 als **IP-Version** auswählen, kann dieser Wert nicht geändert werden. |
    |DNS-Namensbezeichnung|Nein|Muss in dem Azure-Standort, in dem Sie den Namen erstellen, eindeutig sein (über alle Abonnements und Kunden hinweg). Der öffentliche DNS-Dienst von Azure registriert den Namen und die IP-Adresse automatisch, sodass Sie über den Namen eine Verbindung mit der Ressource herstellen können. Azure fügt *location.cloudapp.azure.com* (wobei „location“ der Standort ist, den Sie auswählen) an den von Ihnen bereitgestellten Namen an, um den vollqualifizierten DNS-Namen zu erstellen. Wenn Sie beide Adressversionen für die Erstellung ausgewählt haben, wird sowohl der IPv4- als auch der IPv6-Adresse der gleiche DNS-Name zugewiesen. Der Azure DNS-Dienst enthält IPv4-A- und IPv6-AAAA-Namenseinträge und antwortet beim Nachschlagen des DNS-Namens mit beiden Einträgen. Der Client wählt die Adresse (IPv4 oder IPv6) für die Kommunikation aus.|
    |IPv6-Adresse (oder IPv4-Adresse) erstellen|Nein| Ob IPv6 oder IPv4 angezeigt wird, hängt von Ihrer Auswahl der **IP-Version** ab. Wenn Sie als **IP Version** z.B. **IPv4** ausgewählt haben, wird hier **IPv4** angezeigt. Wenn Sie *Standard* für **SKU** auswählen, haben Sie nicht die Möglichkeit, eine IPv6-Adresse zu erstellen.
    |Name – wird nur angezeigt, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktiviert haben|Ja, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktivieren.|Der Name muss sich von dem Namen unterscheiden, den Sie als ersten **Namen** in diese Liste eingeben. Wenn Sie sowohl eine IPv4- als auch eine IPv6-Adresse erstellen, erstellt das Portal zwei separate öffentliche IP-Adressressourcen, wobei jeder dieser Ressourcen eine IP-Adressversion zugewiesen ist.|
    |IP-Adresszuweisung – wird nur angezeigt, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktiviert haben|Ja, wenn Sie das Kontrollkästchen **IPv6-Adresse erstellen** (bzw. „IPv4-Adresse erstellen“) aktivieren.|Wenn das Kontrollkästchen **IPv4-Adresse erstellen** aktiviert ist, können Sie eine Zuweisungsmethode auswählen. Wenn das Kontrollkästchen **IPv6-Adresse erstellen** aktiviert ist, können Sie keine Zuweisungsmethode auswählen, da die Methode **Dynamisch** lauten muss.|
    |Abonnement|Ja|Muss im selben [Abonnement](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#subscription) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
    |Ressourcengruppe|Ja|Kann in derselben oder in einer anderen [Ressourcengruppe](../azure-glossary-cloud-terminology.md?toc=%2fazure%2fvirtual-network%2ftoc.json#resource-group) wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten|
    |Ort|Ja|Muss am selben [Standort](https://azure.microsoft.com/regions), auch als Region bezeichnet, wie die Ressource vorhanden sein, der Sie die öffentliche IP-Adresse zuordnen möchten.|
    |Verfügbarkeitszone| Nein | Diese Einstellung wird nur angezeigt, wenn Sie einen unterstützten Standort auswählen. Eine Liste der unterstützten Standorte finden Sie unter [Übersicht über Verfügbarkeitszonen](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json). Verfügbarkeitszonen befinden sich derzeit in der Vorschauversion. Bevor Sie eine Zone oder zonenredundante Option auswählen, müssen Sie die Schritte zum [Registrieren für die Vorschauversion der Verfügbarkeitszonen](../availability-zones/az-overview.md?toc=%2fazure%2fvirtual-network%2ftoc.json#get-started-with-the-availability-zones-preview) durchführen. Wenn Sie die **Basic**-SKU ausgewählt haben, wird automatisch *Keine* ausgewählt. Wenn Sie es vorziehen, eine bestimmte Zone zu gewährleisten, können Sie eine bestimmte Zone auswählen. Keine der beiden Optionen ist zonenredundant. Bei Auswahl der **Standard**-SKU: Es wird automatisch die zonenredundante Option ausgewählt, und der Datenpfad wird gegen Zonenausfall stabilisiert. Wenn Sie es vorziehen, eine bestimmte Zone zu gewährleisten, die nicht gegen Zonenausfall stabilisiert ist, können Sie eine bestimmte Zone auswählen.
  

**Befehle**

Obwohl das Portal die Option bietet, zwei öffentliche IP-Adressen zu erstellen (eine IPv4- und eine IPv6-Adresse), erstellen die folgenden CLI- und PowerShell-Befehle eine Ressource mit einer Adresse für jeweils nur eine der IP-Versionen. Wenn Sie zwei öffentliche IP-Adressressourcen benötigen, müssen Sie den folgenden Befehl zweimal ausführen und bei jeder Ausführung unterschiedliche Namen und Versionen für die öffentlichen IP-Adressressourcen angeben. 

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network public-ip create](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#create)|
|PowerShell|[New-AzureRmPublicIpAddress](/powershell/module/azurerm.network/new-azurermpublicipaddress)|

## <a name="view-change-settings-for-or-delete-a-public-ip-address"></a>Anzeigen, Ändern von Einstellungen oder Löschen einer öffentlichen IP-Adresse

1. Melden Sie sich mit einem Konto, dem für Ihr Abonnement mindestens Berechtigungen für die Rolle „Netzwerkmitwirkender“ zugewiesen sind, beim [Azure-Portal](https://portal.azure.com) an. Weitere Informationen zum Zuweisen von Rollen und Berechtigungen zu Konten finden Sie im Artikel [Integrierte Rollen für die rollenbasierte Zugriffssteuerung in Azure](../active-directory/role-based-access-built-in-roles.md?toc=%2fazure%2fvirtual-network%2ftoc.json#network-contributor).
2. Geben Sie im oberen Bereich des Azure-Portals im Feld mit dem Text *Ressourcen suchen* die Zeichenfolge *öffentliche IP-Adresse* ein. Wenn **Öffentliche IP-Adressen** in den Suchergebnissen angezeigt wird, klicken Sie darauf.
3. Klicken Sie auf dem angezeigten Blatt **Öffentliche IP-Adressen** auf den Namen der öffentlichen IP-Adresse, deren Einstellungen Sie anzeigen, ändern oder die Sie löschen möchten.
4. Führen Sie auf dem für die öffentliche IP-Adresse angezeigten Blatt eine der folgenden Optionen aus – je nachdem, ob Sie die öffentliche IP-Adresse anzeigen, löschen oder ändern möchten.
    - **Ansicht**: Im Abschnitt **Übersicht** des Blatts werden die wichtigsten Einstellungen für die öffentliche IP-Adresse angezeigt, darunter die zugeordnete Netzwerkschnittstelle (sofern die Adresse einer Netzwerkschnittstelle zugeordnet ist). Das Portal zeigt die Adressversion nicht an (IPv4 oder IPv6). Um Informationen zur Version zu erhalten, verwenden Sie den PowerShell- oder CLI-Befehl zum Anzeigen der öffentlichen IP-Adresse. Wenn die IP-Adressversion IPv6 lautet, wird die zugewiesene Adresse weder im Portal noch in PowerShell oder über die Befehlszeilenschnittstelle angezeigt. 
    - **Löschen:** Um die öffentliche IP-Adresse zu löschen, klicken Sie im Blattabschnitt **Übersicht** auf **Löschen**. Ist die Adresse zurzeit einer IP-Konfiguration zugeordnet, kann sie nicht gelöscht werden. Ist die Adresse zurzeit einer Konfiguration zugeordnet, klicken Sie auf **Trennen**, um die Adresse von der IP-Konfiguration zu trennen.
    - **Ändern:** Klicken Sie auf **Konfiguration**. Ändern Sie Einstellungen anhand der Informationen aus Schritt 4 des Abschnitts [Erstellen einer öffentlichen IP-Adresse](#create-a-public-ip-address) in diesem Artikel. Möchten Sie die Zuweisung für eine IPv4-Adresse von statisch in dynamisch ändern, müssen Sie die öffentliche IPv4-Adresse zunächst von der IP-Konfiguration trennen, der sie zugeordnet ist. Danach können Sie die Zuweisungsmethode in dynamisch ändern und auf **Zuordnen** klicken, um die IP-Adresse zur selben IP-Konfiguration oder zu einer anderen Konfiguration zuzuordnen. Sie können die Trennung aber auch beibehalten. Um eine öffentliche IP-Adresse von einer IP-Konfiguration zu trennen, klicken Sie in der **Übersicht** auf **Trennen**.

>[!WARNING]
>Wenn Sie die Zuweisungsmethode von statisch in dynamisch ändern, geht die IP-Adresse verloren, die der öffentlichen IP-Adresse zugewiesen war. Während die öffentlichen DNS-Server von Azure eine Zuordnung zwischen statischen oder dynamischen Adressen und jeder DNS-Namensbezeichnung (sofern eine solche definiert ist) verwalten, kann sich eine dynamische IP-Adresse ändern, wenn der virtuelle Computer gestartet wird, nachdem er sich im Status „Beendet (Zuordnung aufgehoben)“ befunden hat. Möchten Sie verhindern, dass sich die Adresse ändert, weisen Sie eine statische IP-Adresse zu.

**Befehle**

|Tool|Befehl|
|---|---|
|Befehlszeilenschnittstelle (CLI)|[az network public-ip-list](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#list) listet die öffentlichen IP-Adressen auf, [az network public-ip-show](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#show) zeigt die Einstellungen an, [az network public-ip update](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#update) führt eine Aktualisierung durch, [az network public-ip delete](/cli/azure/network/public-ip?toc=%2fazure%2fvirtual-network%2ftoc.json#delete) löscht|
|PowerShell|[Get-AzureRmPublicIpAddress](/powershell/module/azurerm.network/get-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) ruft ein öffentliches IP-Adressenobjekt ab und zeigt dessen Einstellungen an, [Set-AzureRmPublicIpAddress](/powershell/resourcemanager/azurerm.network/set-azurermpublicipaddress?toc=%2fazure%2fvirtual-network%2ftoc.json) aktualisiert die Einstellungen, [Remove-AzureRmPublicIpAddress](/powershell/module/azurerm.network/remove-azurermpublicipaddress) löscht|

## <a name="register-for-the-standard-sku-preview"></a>Registrieren für die Vorschauversion der Standard-SKU

> [!NOTE]
> Die Features in der Vorschauversion sind unter Umständen nicht so verfügbar und zuverlässig wie Features in Versionen mit allgemeiner Verfügbarkeit. Vorschaufeatures werden nicht unterstützt, bieten möglicherweise eingeschränkte Funktionen und sind vielleicht nicht an allen Azure-Standorten verfügbar. 

Bevor Sie eine öffentliche IP-Adresse für eine Standard-SKU erstellen können, müssen Sie sich für die Vorschauversion registrieren. Führen Sie die folgenden Schritte aus, um sich für die Vorschau zu registrieren:

1. Installieren und konfigurieren Sie Azure [PowerShell](/powershell/azure/install-azurerm-ps).
2. Führen Sie den Befehl `Get-Module -ListAvailable AzureRM` aus, um festzustellen, welche Version des AzureRM-Moduls Sie installiert haben. Es muss Version 4.4.0 oder höher installiert sein. Wenn dies nicht der Fall ist, können Sie die neueste Version aus dem [PowerShell-Katalog](https://www.powershellgallery.com/packages/AzureRM) installieren.
3. Melden Sie sich mit dem Befehl `login-azurermaccount` bei Azure an.
4. Geben Sie den folgenden Befehl ein, um sich für die Vorschau zu registrieren:
   
    ```powershell
    Register-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

5. Bestätigen Sie, dass Sie für die Vorschauversion registriert sind, indem Sie folgenden Befehl eingeben:

    ```powershell
    Get-AzureRmProviderFeature -FeatureName AllowLBPreview -ProviderNamespace Microsoft.Network
    ```

## <a name="next-steps"></a>Nächste Schritte
Zuweisen von öffentlichen IP-Adressen, wenn die folgenden Azure-Ressourcen erstellt werden:

- [Windows](../virtual-machines/virtual-machines-windows-hero-tutorial.md?toc=%2fazure%2fvirtual-network%2ftoc.json)- oder [Linux](../virtual-machines/linux/quick-create-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)-VMs
- [Azure Load Balancer mit Internetzugriff](../load-balancer/load-balancer-get-started-internet-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure Application Gateway](../application-gateway/application-gateway-create-gateway-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Standort-zu-Standort-Verbindung über ein Azure VPN Gateway](../vpn-gateway/vpn-gateway-howto-site-to-site-resource-manager-portal.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
- [Azure VM-Skalierungsgruppe](../virtual-machine-scale-sets/virtual-machine-scale-sets-portal-create.md?toc=%2fazure%2fvirtual-network%2ftoc.json)
