# <a name="make-a-remote-connection-to-a-kubernetes-dcos-or-docker-swarm-cluster"></a>Herstellen einer Remoteverbindung mit einem Kubernetes-, DC/OS- oder Docker Swarm-Cluster
Nach dem Erstellen eines Azure Container Service-Clusters müssen Sie eine Verbindung mit dem Cluster herstellen, um Workloads bereitzustellen und zu verwalten. In diesem Artikel wird beschrieben, wie Sie von einem Remotecomputer aus eine Verbindung mit dem virtuellen Mastercomputer des Clusters herstellen. 

Die Kubernetes-, DC/OS- und Docker Swarm-Cluster stellen HTTP-Endpunkte lokal bereit. Für Kubernetes wird dieser Endpunkt auf sichere Weise im Internet verfügbar gemacht, und Sie können darauf zugreifen, indem Sie das Befehlszeilentool `kubectl` auf einem Computer mit Internetverbindung ausführen. 

Für DC/OS und Docker Swarm empfehlen wir Ihnen die Erstellung eines SSH-Tunnels (Secure Shell) von Ihrem lokalen Computer zum Clusterverwaltungssystem. Nachdem der Tunnel eingerichtet wurde, können Sie Befehle mit den HTTP-Endpunkten verwenden und die Weboberfläche des Orchestrators über Ihr lokales System anzeigen (falls vorhanden). 

## <a name="prerequisites"></a>Voraussetzungen

* Ein Kubernetes-, DC/OS- oder Docker Swarm-Cluster, der [in Azure Container Service bereitgestellt wurde](../articles/container-service/dcos-swarm/container-service-deployment.md).
* SSH-RSA-Datei mit passendem privatem Schlüssel für den öffentlichen Schlüssel, der dem Cluster während der Bereitstellung hinzugefügt wurde. Bei diesen Befehlen wird vorausgesetzt, dass sich der private SSH-Schlüssel auf Ihrem Computer unter `$HOME/.ssh/id_rsa` befindet. Weitere Informationen finden Sie in der Anleitung für [MacOS und Linux](../articles/virtual-machines/linux/mac-create-ssh-keys.md) bzw. [Windows](../articles/virtual-machines/linux/ssh-from-windows.md). Wenn die SSH-Verbindung nicht funktioniert, müssen Sie ggf. [Ihre SSH-Schlüssel zurücksetzen](../articles/virtual-machines/linux/troubleshoot-ssh-connection.md).

## <a name="connect-to-a-kubernetes-cluster"></a>Herstellen einer Verbindung mit einem Kubernetes-Cluster

Führen Sie diese Schritte aus, um `kubectl` auf Ihrem Computer zu installieren und zu konfigurieren.

> [!NOTE] 
> Unter Linux oder MacOS müssen Sie die Befehle dieses Abschnitts unter Umständen mit `sudo` ausführen.
> 

### <a name="install-kubectl"></a>Installieren von kubectl
Eine Möglichkeit zum Installieren dieses Tools ist der Azure CLI 2.0-Befehl `az acs kubernetes install-cli`. Stellen Sie zum Ausführen dieses Befehls sicher, dass Sie die aktuelle Version von Azure CLI 2.0 [installiert](/cli/azure/install-az-cli2) haben und an einem Azure-Konto (`az login`) angemeldet sind.

```azurecli
# Linux or macOS
az acs kubernetes install-cli [--install-location=/some/directory/kubectl]

# Windows
az acs kubernetes install-cli [--install-location=C:\some\directory\kubectl.exe]
```

Alternativ hierzu können Sie den aktuellen `kubectl`-Client direkt über die [Übersicht der verschiedenen Kubernetes-Releases](https://github.com/kubernetes/kubernetes/blob/master/CHANGELOG.md) herunterladen. Weitere Informationen finden Sie unter [Installing and Setting up kubectl](https://kubernetes.io/docs/tasks/kubectl/install/) (Installieren und Einrichten von kubectl).

### <a name="download-cluster-credentials"></a>Herunterladen von Clusteranmeldeinformationen
Nachdem `kubectl` installiert wurde, müssen Sie die Clusteranmeldeinformationen auf Ihren Computer kopieren. Eine Möglichkeit zum Abrufen der Anmeldeinformationen ist der Befehl `az acs kubernetes get-credentials`. Übergeben Sie den Namen der Ressourcengruppe und den Namen der Container Service-Ressource:

```azurecli
az acs kubernetes get-credentials --resource-group=<cluster-resource-group> --name=<cluster-name>
```

Mit diesem Befehl werden die Clusteranmeldeinformationen an den Speicherort `$HOME/.kube/config` heruntergeladen, da dies der von `kubectl` angenommene Speicherort ist.

Sie können alternativ auch `scp` verwenden, um die Datei sicher von `$HOME/.kube/config` auf dem virtuellen Mastercomputer auf Ihren lokalen Computer zu kopieren. Beispiel:

```bash
mkdir $HOME/.kube
scp azureuser@<master-dns-name>:.kube/config $HOME/.kube/config
```

Bei Verwendung von Windows können Sie Bash unter Ubuntu unter Windows, den PuTTY Secure File Copy Client oder ein ähnliches Tool nutzen.

### <a name="use-kubectl"></a>Verwenden von kubectl

Testen Sie die Verbindung nach dem Konfigurieren von `kubectl` durch das Auflisten der Knoten im Cluster:

```bash
kubectl get nodes
```

Sie können auch andere `kubectl`-Befehle ausprobieren. Beispielsweise können Sie das Kubernetes-Dashboard anzeigen. Führen Sie zuerst einen Proxy auf dem Kubernetes-API-Server aus:

```bash
kubectl proxy
```

Die Kubernetes-UI ist jetzt auf der folgenden Seite verfügbar: `http://localhost:8001/ui`.

Weitere Informationen finden Sie in der [Schnellstartanleitung von Kubernetes](http://kubernetes.io/docs/user-guide/quick-start/).

## <a name="connect-to-a-dcos-or-swarm-cluster"></a>Herstellen einer Verbindung mit einem DC/OS- oder Swarm-Cluster

Wenn Sie die von Azure Container Service bereitgestellten DC/OS- und Docker Swarm-Cluster verwenden möchten, erstellen Sie in Ihrem lokalen Linux-, MacOS- oder Windows-System gemäß der folgenden Anleitung einen SSH-Tunnel. 

> [!NOTE]
> Diese Anleitung konzentriert sich auf die Übertragung von TCP-Datenverkehr über einen SSH-Tunnel. Sie können auch eine interaktive SSH-Sitzung mit einem der internen Clusterverwaltungssysteme starten, dies wird jedoch nicht empfohlen. Bei der direkten Bearbeitung eines internen Systems besteht die Gefahr unbeabsichtigter Konfigurationsänderungen.  
> 

### <a name="create-an-ssh-tunnel-on-linux-or-macos"></a>Erstellen eines SSH-Tunnels unter Linux oder MacOS
Als Erstes ermitteln Sie beim Erstellen eines SSH-Tunnels unter Linux oder MacOS den öffentlichen DNS-Namen der Masterelemente mit Lastenausgleich. Folgen Sie diesen Schritten:


1. Navigieren Sie im [Azure-Portal](https://portal.azure.com) zu der Ressourcengruppe, die Ihren Container Service-Cluster enthält. Erweitern Sie die Ressourcengruppe, damit jede Ressource angezeigt wird. 

2. Klicken Sie auf die Ressource **Containerdienst** und anschließend auf **Übersicht**. Der **Master-FQDN** des Clusters wird unter **Zusammenfassung** angezeigt. Speichern Sie diesen Namen für die spätere Verwendung. 

    ![Öffentlicher DNS-Name](./media/container-service-connect/pubdns.png)

    Führen Sie alternativ den Befehl `az acs show` für Ihren Containerdienst aus. Suchen Sie in der Befehlsausgabe nach der Eigenschaft **Master Profile:fqdn**.

3. Öffnen Sie als Nächstes eine Shell, und führen Sie den Befehl `ssh` aus, indem Sie die folgenden Werte angeben: 

    **LOCAL_PORT** ist der dienstseitige TCP-Port des Tunnels, mit dem eine Verbindung hergestellt werden soll. Legen Sie diesen Wert für Swarm auf „2375“ fest. Legen Sie ihn für DC/OS auf „80“ fest. 
    **REMOTE_PORT** ist der Port des Endpunkts, den Sie verfügbar machen möchten. Verwenden Sie für Swarm Port 2375. Verwenden Sie für DC/OS Port 80.  
    **BENUTZERNAME** ist der Benutzername, der bei der Bereitstellung des Clusters angegeben wurde.  
    **DNSPREFIX** ist das DNS-Präfix, das bei der Bereitstellung des Clusters angegeben wurde.  
    **REGION** ist die Region, in der sich die Ressourcengruppe befindet.  
    **PATH_TO_PRIVATE_KEY** [OPTIONAL] ist der Pfad zum privaten Schlüssel, der zu dem öffentlichen Schlüssel passt, den Sie beim Erstellen des Clusters angegeben haben. Verwenden Sie diese Option mit dem Flag `-i`.

    ```bash
    ssh -fNL LOCAL_PORT:localhost:REMOTE_PORT -p 2200 [USERNAME]@[DNSPREFIX]mgmt.[REGION].cloudapp.azure.com
    ```
  
  > [!NOTE]
  > Für die SSH-Verbindung wird Port 2200 und nicht der Standardport 22 verwendet. In einem Cluster mit mehr als einem virtuellen Mastercomputer ist dies der Verbindungsport für den ersten virtuellen Mastercomputer.
  > 

  Der Befehl wird ohne Ausgabe zurückgegeben.

Die folgenden Abschnitte enthalten Beispiele für DC/OS und Swarm.    

### <a name="dcos-tunnel"></a>DC/OS-Tunnel
Führen Sie einen Befehl wie den folgenden aus, um einen Tunnel für DC/OS-Endpunkte zu öffnen:

```bash
sudo ssh -fNL 80:localhost:80 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com 
```

> [!NOTE]
> Stellen Sie sicher, dass Port 80 nicht durch einen anderen lokalen Prozess gebunden ist. Anstelle von Port 80 können Sie bei Bedarf auch einen anderen lokalen Port angeben, z.B. Port 8080. Bei Verwendung dieses Ports funktionieren jedoch einige Links auf Webbenutzeroberflächen unter Umständen nicht.
>

Sie können nun auf Ihrem lokalen System über die folgenden URLs auf die DC/OS-Endpunkte zugreifen (bei Verwendung des lokalen Ports 80):

* DC/OS: `http://localhost:80/`
* Marathon: `http://localhost:80/marathon`
* Mesos: `http://localhost:80/mesos`

Sie erreichen die REST-APIs für jede Anwendung über diesen Tunnel auf ähnliche Weise:

### <a name="swarm-tunnel"></a>Swarm-Tunnel
Führen Sie einen Befehl wie den folgenden aus, um einen Tunnel zum Swarm-Endpunkt zu öffnen:

```bash
ssh -fNL 2375:localhost:2375 -p 2200 azureuser@acsexamplemgmt.japaneast.cloudapp.azure.com
```
> [!NOTE]
> Stellen Sie sicher, dass Port 2375 nicht durch einen anderen lokalen Prozess gebunden ist. Wenn Sie den Docker-Daemon beispielsweise lokal ausführen, wird standardmäßig die Verwendung von Port 2375 festgelegt. Anstelle von Port 2375 können Sie auch einen anderen lokalen Port angeben.
>

Nun können Sie auf den Docker Swarm-Cluster zugreifen, indem Sie die Docker-Befehlszeilenschnittstelle (Docker CLI) auf Ihrem lokalen System verwenden. Eine Installationsanleitung finden Sie [hier](https://docs.docker.com/engine/installation/).

Legen Sie Ihre DOCKER_HOST-Umgebungsvariable auf den lokalen Port fest, den Sie für den Tunnel konfiguriert haben. 

```bash
export DOCKER_HOST=:2375
```

Führen Sie Docker-Befehle aus, bei denen ein Tunnel zum Docker Swarm-Cluster verwendet wird. Beispiel:

```bash
docker info
```

### <a name="create-an-ssh-tunnel-on-windows"></a>Erstellen eines SSH-Tunnels unter Windows
Es gibt mehrere Möglichkeiten, wie Sie SSH-Tunnel unter Windows erstellen können. Wenn Sie Bash unter Ubuntu unter Windows oder ein ähnliches Tool ausführen, können Sie die Anleitung für SSH-Tunnel verwenden, die weiter oben in diesem Artikel für MacOS und Linux angegeben wurde. Als Alternative zu Windows wird in diesem Abschnitt beschrieben, wie Sie PuTTY zum Erstellen des Tunnels verwenden.

1. [Laden Sie PuTTY auf Ihr Windows-System herunter](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html).

2. Führen Sie die Anwendung aus.

3. Geben Sie einen Hostnamen ein, der aus dem Administratorbenutzernamen für den Cluster und dem öffentlichen DNS-Namen für den ersten Master im Cluster besteht. Der **Hostname** hat das Format `azureuser@PublicDNSName`. Geben Sie 2200 als **Port**ein.

    ![PuTTY-Konfiguration 1](./media/container-service-connect/putty1.png)

4. Wählen Sie **SSH > Auth**. Fügen Sie der Datei mit dem privaten Schlüssel (PPK-Format) für die Authentifizierung einen Pfad hinzu. Sie können ein Tool wie [PuTTYgen](http://www.chiark.greenend.org.uk/~sgtatham/putty/download.html) nutzen, um diese Datei aus dem SSH-Schlüssel zu generieren, der zum Erstellen des Clusters verwendet wurde.

    ![PuTTY-Konfiguration 2](./media/container-service-connect/putty2.png)

5. Wählen Sie **SSH > Tunnel**, und konfigurieren Sie die folgenden weitergeleiteten Ports:

    * **Quellport:** Verwenden Sie 80 für DC/OS und 2375 für Swarm.
    * **Ziel**: Verwenden Sie „localhost:80“ für DC/OS bzw. „localhost:2375“ für Swarm.

    Das folgende Beispiel ist für DC/OS konfiguriert, sieht für Docker Swarm aber ähnlich aus.

    > [!NOTE]
    > Port 80 darf beim Erstellen dieses Tunnels nicht verwendet werden.
    > 

    ![PuTTY-Konfiguration 3](./media/container-service-connect/putty3.png)

6. Klicken Sie nach Abschluss der Schritte auf **Sitzung > Speichern**, um die Verbindungskonfiguration zu speichern.

7. Klicken Sie zum Herstellen der Verbindung mit der PuTTY-Sitzung auf **Öffnen**. Beim Herstellen der Verbindung können Sie die Portkonfiguration im PuTTY-Ereignisprotokoll anzeigen.

    ![PuTTY-Ereignisprotokoll](./media/container-service-connect/putty4.png)

Nachdem Sie den Tunnel für DC/OS konfiguriert haben, können Sie wie folgt auf die zugehörigen Endpunkte zugreifen:

* DC/OS: `http://localhost/`
* Marathon: `http://localhost/marathon`
* Mesos: `http://localhost/mesos`

Öffnen Sie nach dem Konfigurieren des Tunnels für Docker Swarm Ihre Windows-Einstellungen, um eine Systemumgebungsvariable mit dem Namen `DOCKER_HOST` mit dem Wert `:2375` zu konfigurieren. Anschließend können Sie über die Docker CLI auf den Swarm-Cluster zugreifen.

## <a name="next-steps"></a>Nächste Schritte
Bereitstellen und Verwalten von Containern in Ihrem Cluster:

* [Microsoft Azure Container Service Engine - Kubernetes Walkthrough (Microsoft Azure Container Service-Modul – Exemplarische Vorgehensweise für Kubernetes)](../articles/container-service/kubernetes/container-service-kubernetes-ui.md)
* [Verwenden von Azure Container Service und DC/OS](../articles/container-service//dcos-swarm/container-service-mesos-marathon-rest.md)
* [Verwenden von Azure Container Service und Docker Swarm](../articles//container-service/dcos-swarm/container-service-docker-swarm.md)

