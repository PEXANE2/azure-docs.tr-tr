---
title: Azure Monitör'de Konteyner İzleme çözümü | Microsoft Dokümanlar
description: Azure Monitör'deki Kapsayıcı İzleme çözümü, Docker ve Windows kapsayıcı ana bilgisayarlarınızı tek bir konumda görüntülemenize ve yönetmenize yardımcı olur.
ms.subservice: logs
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/22/2019
ms.openlocfilehash: 171f897f6e110e8f759281c139addab477ecede3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77664703"
---
# <a name="container-monitoring-solution-in-azure-monitor"></a>Azure Monitör'de Konteyner İzleme çözümü

![Konteynersembolü](./media/containers/containers-symbol.png)

Bu makalede, Docker ve Windows kapsayıcı ana bilgisayarlarınızı tek bir konumda görüntülemenize ve yönetmenize yardımcı olan Azure Monitor'da Kapsayıcı İzleme çözümünü nasıl ayarladığınızı ve kullanacağınızı açıklar. Docker, BT altyapılarına yazılım dağıtımını otomatikleştiren kapsayıcılar oluşturmak için kullanılan bir yazılım sanallaştırma sistemidir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Çözüm, hangi kapsayıcıların çalıştığını, hangi kapsayıcı görüntüsünü çalıştırdıklarını ve kapsayıcıların nerede çalıştığını gösterir. Kapsayıcılarla birlikte kullanılan komutları gösteren ayrıntılı denetim bilgilerini görüntüleyebilirsiniz. Ayrıca, Docker veya Windows ana bilgisayarlarını uzaktan görüntülemek zorunda kalmadan merkezi günlükleri görüntüleyip arayarak kapsayıcıları giderebilirsiniz. Gürültülü ve aşırı kaynakları tüketen kapsayıcıları bir ana bilgisayarda bulabilirsiniz. Ayrıca, kapsayıcılar için merkezi cpu, bellek, depolama ve ağ kullanımı ve performans bilgilerini görüntüleyebilirsiniz. Windows çalıştıran bilgisayarlarda, Windows Server, Hyper-V ve Docker kapsayıcılarından günlükleri merkezileştirebilir ve karşılaştırabilirsiniz. Çözüm aşağıdaki konteyner orkestratörlerini destekler:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift

[Azure Hizmet Kumaşı'nda](../../service-fabric/service-fabric-overview.md)dağıtılan kapsayıcılarınız varsa, hem Hizmet [Kumaşı çözümünü](../../service-fabric/service-fabric-diagnostics-oms-setup.md) hem de bu çözümü küme olaylarının izlenmesini içerecek şekilde etkinleştirmenizi öneririz. Service Fabric çözümünü etkinleştirmeden önce, ne sağlayıcısını ve kullanılanağını anlamak için [Hizmet Kumaşı çözümünü kullanÄ](../../service-fabric/service-fabric-diagnostics-event-analysis-oms.md)

Azure Kubernetes Hizmetinde (AKS) barındırılan Kubernetes ortamlarına dağıtılan iş yüklerinizin performansını izlemek [istiyorsanız,](../../azure-monitor/insights/container-insights-overview.md)bkz. Konteyner İzleme çözümü bu platformun izlenmesini desteklemez.  

Aşağıdaki diyagram, Azure Monitor ile çeşitli kapsayıcı ana bilgisayarları ve aracılar arasındaki ilişkileri gösterir.

![Kapsayıcılar diyagramı](./media/containers/containers-diagram.png)

## <a name="system-requirements-and-supported-platforms"></a>Sistem gereksinimleri ve desteklenen platformlar

Başlamadan önce, ön koşulları karşıladığınızı doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Docker Orchestrator ve OS platformu için konteyner izleme çözümü desteği

Aşağıdaki tabloda, Azure Monitor ile konteyner envanteri, performansı ve günlüklerinin Docker orkestrasyonu ve işletim sistemi izleme desteği özetlenmektedir.   

| | ACS | Linux | Windows | Kapsayıcı<br>Envanter | Görüntü<br>Envanter | Node<br>Envanter | Kapsayıcı<br>Performans | Kapsayıcı<br>Olay | Olay<br>Günlük | Kapsayıcı<br>Günlük |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mezosfer<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Sürüsü | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Hizmet<br>Fabric | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Kırmızı Şapka Açık<br>Shift | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(tek başına) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux Sunucusu<br>(tek başına) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |

### <a name="docker-versions-supported-on-linux"></a>Docker sürümleri Linux'ta desteklendi

- Docker 1.11 ile 1.13 arası
- Docker CE ve EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>konteyner ana bilgisayar olarak desteklenen x64 Linux dağıtımları

- Ubuntu 14.04 LTS ve 16.04 LTS
- CoreOS(kararlı)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- açıkSUSE LEAP 42.2
- CentOS 7.2 ve 7.3
- SLES 12
- RHEL 7.2 ve 7.3
- Red Hat OpenShift Konteyner Platformu (OCP) 3.4 ve 3.5
- ACS Mezosfer DC/OS 1.7.3 ile 1.8.8 arası
- ACS Kubernetes 1,4,5 ile 1,6 arası
    - Kubernetes etkinlikleri, Kubernetes envanteri ve konteyner işlemleri yalnızca Sürüm 1.4.1-45 ve daha sonra Linux için Log Analytics aracısı ile desteklenir
- ACS Docker Sürüsü

[!INCLUDE [log-analytics-agent-note.md](../../../includes/log-analytics-agent-note.md)] 

### <a name="supported-windows-operating-system"></a>Desteklenen Windows işletim sistemi

- Windows Server 2016
- Windows 10 Anniversary Edition (Profesyonel veya Kurumsal)

### <a name="docker-versions-supported-on-windows"></a>Windows'da desteklenen Docker sürümleri

- Docker 1.12 ve 1.13
- Docker 17.03.0 ve sonrası

## <a name="installing-and-configuring-the-solution"></a>Çözümü yükleme ve yapılandırma

Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

1. [Azure pazaryerinden](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) veya [Çözüm Galerisi'nden İzleme Ekle çözümleri ekle'de](../../azure-monitor/insights/solutions.md)açıklanan işlemi kullanarak Günlük Analizi çalışma alanınıza Konteyner İzleme çözümünü ekleyin.

2. Docker'ı bir Log Analytics aracısıyla yükleyin ve kullanın. İşletim sisteminize ve Docker orchestrator'unuza bağlı olarak aracınızı yapılandırmak için aşağıdaki yöntemleri kullanabilirsiniz.
   - Bağımsız ana bilgisayarlar için:
     - Desteklenen Linux işletim sistemlerinde Docker'ı yükleyin ve çalıştırın ve [ardından Linux için Log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md)yükleyin ve yapılandırın.  
     - CoreOS'ta Linux için Log Analytics aracısını çalıştıramazsınız. Bunun yerine, Linux için Log Analytics aracısının kapsayıcı bir sürümünü çalıştırın. Azure Devlet Bulutu'ndaki kapsayıcılarla çalışıyorsanız, CoreOS veya CoreOS dahil olmak üzere Azure Devlet Linux konteyner ana bilgisayarları dahil Olmak üzere Linux konteyner ana bilgisayarlarını inceleyin.
     - Windows Server 2016 ve Windows 10'da Docker Engine'i yükleyin ve ardından bilgi toplamak ve Azure Monitor'a göndermek için bir aracı bağlayın. Windows ortamınız varsa [Yükle'yi gözden geçirin ve Windows kapsayıcı ana bilgisayarlarını yapılandırın.](#install-and-configure-windows-container-hosts)
   - Docker çok ev sahibi orkestrasyon için:
     - Red Hat OpenShift ortamınız varsa, Red Hat OpenShift için bir Log Analytics aracısı yapılandırın'ı gözden geçirin.
     - Azure Kapsayıcı Hizmetini kullanan bir Kubernetes kümeniz varsa:
       - [Kubernetes için bir Log Analytics Linux aracısı yapılandırın.](#configure-a-log-analytics-linux-agent-for-kubernetes)
       - [Kubernetes için Bir Log Analytics Windows aracısı yapılandırın.](#configure-a-log-analytics-windows-agent-for-kubernetes)
       - Log Analytics aracısını Linux Kubernetes'e dağıtmak için Helm'i kullanın'ı gözden geçirin.
     - Bir Azure Kapsayıcı Hizmeti DC/İşletim sistemi kümeniz varsa, [Azure Monitor ile bir Azure Kapsayıcı Hizmeti DC/OS kümesini izleyin'den](../../container-service/dcos-swarm/container-service-monitoring-oms.md)daha fazla bilgi edinin.
     - Docker Swarm modu ortamınız varsa, Docker Swarm için bir Log Analytics aracısı yapılandırın' dan daha fazla bilgi edinin.
     - Service Fabric kümeniz varsa, [Azure Monitor ile Monitör kapsayıcılarında](../../service-fabric/service-fabric-diagnostics-oms-containers.md)daha fazla bilgi edinin.

Windows çalıştıran bilgisayarlarda Docker Motorunuzu nasıl yükleyip yapılandırılacaknız hakkında ek bilgi için [Windows'ta Docker Engine](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) makalesini inceleyin.

> [!IMPORTANT]
> Docker, konteyner ana bilgisayarlarınıza [Linux için Log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md) yüklemeden **önce** çalışıyor olmalıdır. Docker'ı yüklemeden önce aracıyı zaten yüklediyseniz, Linux için Log Analytics aracısını yeniden yüklemeniz gerekir. Docker hakkında daha fazla bilgi için [Docker web sitesine](https://www.docker.com)bakın.

### <a name="install-and-configure-linux-container-hosts"></a>Linux konteyner ana bilgisayarlarını yükleme ve yapılandırma

Docker'ı yükledikten sonra, aracıyı Docker ile birlikte kullanmak üzere yapılandırmak için konteyner ana aracınızın aşağıdaki ayarlarını kullanın. Öncelikle Azure portalında bulabileceğiniz Log Analytics çalışma alanı kimliğinize ve anahtarınıza ihtiyacınız vardır. Çalışma alanınızda, **Çalışma Alanı Kimliğinizi** ve Birincil **Anahtarınızı**görüntülemek için Hızlı **Başlangıç** > **Bilgisayarları'nı** tıklatın.  Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.

**CoreOS dışındaki tüm Linux konteyner ana bilgisayarları için:**

- Linux için Log Analytics aracısını nasıl yükleyeceğine ilişkin daha fazla bilgi ve adımlar için [Log Analytics temsilcisine genel bakış](../../azure-monitor/platform/log-analytics-agent.md)bölümüne bakın.

**CoreOS dahil tüm Linux konteyner ana bilgisayarları için:**

İzlemek istediğiniz kapsayıcıyı başlatın. Aşağıdaki örneği değiştirin ve kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

**CoreOS dahil tüm Azure Devlet Linux konteyner ev sahipleri için:**

İzlemek istediğiniz kapsayıcıyı başlatın. Aşağıdaki örneği değiştirin ve kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

**Yüklü bir Linux aracısını kullanmaktan bir kapta bir e ve**

Daha önce doğrudan yüklenen aracıkullandıysanız ve bunun yerine bir kapsayıcıda çalışan bir aracı kullanmak istiyorsanız, öncelikle Linux için Log Analytics aracısını kaldırmanız gerekir. Aracıyı nasıl başarıyla kaldırlayacağımı anlamak [için Linux için Log Analytics aracısını kaldırmabölümüne](../../azure-monitor/learn/quick-collect-linux-computer.md) bakın.  

#### <a name="configure-a-log-analytics-agent-for-docker-swarm"></a>Docker Swarm için Bir Log Analytics aracısı yapılandırın

Docker Swarm'da Log Analytics aracısını küresel bir hizmet olarak çalıştırabilirsiniz. Bir Log Analytics aracısı hizmeti oluşturmak için aşağıdaki bilgileri kullanın. Log Analytics Çalışma Alanı Kimliğinizi ve Birincil Anahtarınızı sağlamanız gerekir.

- Ana düğümde aşağıdakileri çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

##### <a name="secure-secrets-for-docker-swarm"></a>Docker Swarm için güvenli sırlar

Docker Swarm için, Çalışma Alanı Kimliği ve Birincil Anahtar için gizli oluşturulduktan sonra, gizli bilgilerinizi oluşturmak için aşağıdaki bilgileri kullanın.

1. Ana düğümde aşağıdakileri çalıştırın.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Sırların doğru oluşturulduğunu doğrulayın.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Sırları kapsayıcı Log Analytics temsilcisine monte etmek için aşağıdaki komutu çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="configure-a-log-analytics-agent-for-red-hat-openshift"></a>Red Hat OpenShift için Log Analytics aracısı yapılandırın

Kapsayıcı izleme verileri toplamaya başlamak için Log Analytics aracısını Red Hat OpenShift'e eklemenin üç yolu vardır.

* [Linux için Log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md) doğrudan her OpenShift düğümüne yükleyin  
* Azure'da bulunan her OpenShift düğümünde [Günlük Analizi VM Uzantısını etkinleştirme](../../azure-monitor/learn/quick-collect-azurevm.md)  
* Log Analytics aracısını OpenShift daemon-set olarak yükleme  

Bu bölümde, Log Analytics aracısını OpenShift daemon-set olarak yüklemek için gereken adımları kapsamaktayız.  

1. OpenShift ana düğümünde oturum açın ve GitHub'dan yaml dosya [ocp-omsagent.yaml'ı](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) ana düğümünüze kopyalayın ve Log Analytics Workspace ID'nizle ve Birincil Anahtarınızla değeri değiştirin.
2. Azure Monitor için bir proje oluşturmak ve kullanıcı hesabını ayarlamak için aşağıdaki komutları çalıştırın.

    ```
    oc adm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oc adm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oc adm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

3. Daemon kümesini dağıtmak için aşağıdakileri çalıştırın:

    `oc create -f ocp-omsagent.yaml`

4. Yapılandırıldığından ve doğru çalıştığını doğrulamak için aşağıdakileri yazın:

    `oc describe daemonset omsagent`  

    ve çıktı benzer olmalıdır:

    ```
    [ocpadmin@khm-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

Log Analytics çalışma alanı kimliğinizi ve Birincil Anahtarınızı günlük Analytics aracısı daemon-set yaml dosyasını kullanırken güvenliğini sağlamak için sırları kullanmak istiyorsanız, aşağıdaki adımları gerçekleştirin.

1. OpenShift ana düğümünde oturum açın ve GitHub'dan [yaml dosyasını ocp-ds-omsagent.yaml](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) ve gizli üreten komut dosyası [ocp-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) kopyalayın.  Bu komut dosyası, salgı bilgilerinizi güvence altına almak için Log Analytics Workspace ID ve Primary Key için yaml dosyasını oluşturur.  
2. Azure Monitor için bir proje oluşturmak ve kullanıcı hesabını ayarlamak için aşağıdaki komutları çalıştırın. Gizli oluşturan komut dosyası Log Analytics Çalışma `<WSID>` Alanı `<KEY>` Kimliğinizi ve Birincil Anahtarınızı sorar ve tamamlandıktan sonra ocp-secret.yaml dosyasını oluşturur.  

    ```
    oc adm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oc adm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oc adm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

3. Aşağıdakileri çalıştırarak gizli dosyayı dağıtın:

    `oc create -f ocp-secret.yaml`

4. Aşağıdakileri çalıştırarak dağıtımı doğrulayın:

    `oc describe secret omsagent-secret`  

    ve çıktı benzer olmalıdır:  

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe secret omsagent-secret  
    Name:           omsagent-secret  
    Namespace:      omslogging  
    Labels:         <none>  
    Annotations:    <none>  

    Type:   Opaque  

    Data  
    ====  
    KEY:    89 bytes  
    WSID:   37 bytes  
    ```

5. Log Analytics aracısı daemon-set yaml dosyasını aşağıdakileri çalıştırarak dağıtın:

    `oc create -f ocp-ds-omsagent.yaml`  

6. Aşağıdakileri çalıştırarak dağıtımı doğrulayın:

    `oc describe ds oms`

    ve çıktı benzer olmalıdır:

    ```
    [ocpadmin@khocp-master-0 ~]$ oc describe ds oms  
    Name:           oms  
    Image(s):       microsoft/oms  
    Selector:       name=omsagent  
    Node-Selector:  zone=default  
    Labels:         agentVersion=1.4.0-12  
                    dockerProviderVersion=10.0.0-25  
                    name=omsagent  
    Desired Number of Nodes Scheduled: 3  
    Current Number of Nodes Scheduled: 3  
    Number of Nodes Misscheduled: 0  
    Pods Status:    3 Running / 0 Waiting / 0 Succeeded / 0 Failed  
    No events.  
    ```

#### <a name="configure-a-log-analytics-linux-agent-for-kubernetes"></a>Kubernetes için Log Analytics Linux aracısı yapılandırın

Kubernetes için, Linux için Log Analytics aracısını yüklemek için Çalışma Alanı Kimliğiniz ve Birincil Anahtarınız için yaml dosyasını oluşturmak için bir komut dosyası kullanırsınız. Log [Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) sayfasında, gizli bilgilerinizle veya olmadan kullanabileceğiniz dosyalar vardır.

- Linux DaemonSet için Varsayılan Log Analytics aracısı gizli bilgilere sahip değildir (omsagent.yaml)
- Linux DaemonSet yaml dosyası için Log Analytics aracısı gizli bilgileri (omsagent-ds-secrets.yaml) gizli nesil komut dosyaları ile gizli nesil komut dosyaları ile sırları yaml (omsagentsecret.yaml) dosya oluşturmak için kullanır.

Omsagent DaemonSets veya sırları olmadan oluşturmak için seçebilirsiniz.

**Varsayılan OMSagent DaemonSet yaml dosyası sırları olmadan**

- Varsayılan Log Analytics aracısı DaemonSet yaml `<WSID>` `<KEY>` dosyası için WSID ve KEY dosyanızı değiştirin. Dosyayı ana düğümünüze kopyalayın ve aşağıdakileri çalıştırın:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

**Varsayılan OMSagent DaemonSet sırları ile yaml dosyası**

1. Gizli bilgileri kullanarak Log Analytics aracısı DaemonSet'i kullanmak için önce sırları oluşturun.
    1. Komut dosyası ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduğundan emin olun.
        - Gizli üreten komut dosyası - secret-gen.sh
        - gizli şablon - gizli şablon.yaml
    2. Aşağıdaki örnekte olduğu gibi komut dosyasını çalıştırın. Komut dosyası Log Analytics Workspace ID ve Birincil Anahtar sorar ve bunları girdikten sonra, komut dosyası çalıştırabilirsiniz böylece gizli bir yaml dosyası oluşturur.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Aşağıdakileri çalıştırarak sırlar bölmesini oluşturun:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Doğrulamak için aşağıdakileri çalıştırın:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Çıktı benzer olmalıdır:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Çıktı benzer olmalıdır:

        ```
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Çalıştırarak omsagent daemon-setinizi oluşturun```sudo kubectl create -f omsagent-ds-secrets.yaml```

2. Log Analytics aracısı DaemonSet'in aşağıdakilere benzer şekilde çalıştığını doğrulayın:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```

Kubernetes için, Linux için Log Analytics aracısı için Workspace ID ve Primary Key için sırlar yaml dosyasını oluşturmak için bir komut dosyası kullanın. Gizli bilgilerinizi güvence altına almak için [omsagent yaml dosyası](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) ile aşağıdaki örnek bilgileri kullanın.

```
keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
Name:           omsagent-secret
Namespace:      default
Labels:         <none>
Annotations:    <none>

Type:   Opaque

Data
====
WSID:   36 bytes
KEY:    88 bytes
```

#### <a name="configure-a-log-analytics-windows-agent-for-kubernetes"></a>Kubernetes için Log Analytics Windows aracısı yapılandırın

Windows Kubernetes için, Log Analytics aracısını yüklemek için Çalışma Alanı Kimliğiniz ve Birincil Anahtarınız için sırlar yaml dosyasını oluşturmak için bir komut dosyası kullanırsınız. Log [Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes/windows) sayfasında, gizli bilgilerinizle kullanabileceğiniz dosyalar vardır.  Ana ve aracı düğümleri için Log Analytics aracısını ayrı olarak yüklemeniz gerekir.  

1. Log Analytics aracısı DaemonSet'i Master düğümündeki gizli bilgileri kullanarak kullanmak için önce oturum açın ve sırları oluşturun.
    1. Komut dosyası ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduğundan emin olun.
        - Gizli üreten komut dosyası - secret-gen.sh
        - gizli şablon - gizli şablon.yaml

    2. Aşağıdaki örnekte olduğu gibi komut dosyasını çalıştırın. Komut dosyası Log Analytics Workspace ID ve Birincil Anahtar sorar ve bunları girdikten sonra, komut dosyası çalıştırabilirsiniz böylece gizli bir yaml dosyası oluşturur.

        ```
        #> sudo bash ./secret-gen.sh
        ```
    3. Çalıştırarak omsagent daemon-setinizi oluşturun```kubectl create -f omsagentsecret.yaml```
    4. Denetlemek için aşağıdakileri çalıştırın:

        ```
        root@ubuntu16-13db:~# kubectl get secrets
        ```

        Çıktı benzer olmalıdır:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        root@ubuntu16-13db:~# kubectl describe secrets omsagent-secret
        Name:           omsagent-secret
        Namespace:      default
        Labels:         <none>
        Annotations:    <none>

        Type:   Opaque

        Data
        ====
        WSID:   36 bytes
        KEY:    88 bytes
        ```

    5. Çalıştırarak omsagent daemon-setinizi oluşturun```kubectl create -f ws-omsagent-de-secrets.yaml```

2. Log Analytics aracısı DaemonSet'in aşağıdakilere benzer şekilde çalıştığını doğrulayın:

    ```
    root@ubuntu16-13db:~# kubectl get deployment omsagent
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   1         1         <none>          1h
    ```

3. Windows çalıştıran İşçi Düğümüne aracıyı yüklemek için, windows kapsayıcı ana bilgisayarlarını [yükleyin ve yapılandırır.](#install-and-configure-windows-container-hosts)

#### <a name="use-helm-to-deploy-log-analytics-agent-on-linux-kubernetes"></a>Log Analytics aracısını Linux Kubernetes'e dağıtmak için Helm'i kullanın

Log Analytics aracısını Linux Kubernetes ortamınızda dağıtmak için dümeni kullanmak için aşağıdaki adımları gerçekleştirin.

1. Çalıştırarak omsagent daemon-setinizi oluşturun```helm install --name omsagent --set omsagent.secret.wsid=<WSID>,omsagent.secret.key=<KEY> stable/msoms```
2. Sonuçlar aşağıdakilere benzer:

    ```
    NAME:   omsagent
    LAST DEPLOYED: Tue Sep 19 20:37:46 2017
    NAMESPACE: default
    STATUS: DEPLOYED

    RESOURCES:
    ==> v1/Secret
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     3s

    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         3s
    ```

3. Omsagent'ı çalıştırarak durumu kontrol ```helm status "omsagent"``` edebilirsiniz: ve çıktı aşağıdakilere benzer:

    ```
    keiko@k8s-master-3814F33-0:~$ helm status omsagent
    LAST DEPLOYED: Tue Sep 19 20:37:46 2017
    NAMESPACE: default
    STATUS: DEPLOYED
 
    RESOURCES:
    ==> v1/Secret
    NAME            TYPE    DATA  AGE
    omsagent-msoms  Opaque  3     17m
 
    ==> v1beta1/DaemonSet
    NAME            DESIRED  CURRENT  READY  UP-TO-DATE  AVAILABLE  NODE-SELECTOR  AGE
    omsagent-msoms  3        3        3      3           3          <none>         17m
    ```
   
    Daha fazla bilgi için lütfen [Konteyner Çözüm Dümen Grafiği'ni](https://aka.ms/omscontainerhelm)ziyaret edin.

### <a name="install-and-configure-windows-container-hosts"></a>Windows kapsayıcı ana bilgisayarlarını yükleme ve yapılandırma

Windows kapsayıcı ana bilgisayarlarını yüklemek ve yapılandırmak için bölümdeki bilgileri kullanın.

#### <a name="preparation-before-installing-windows-agents"></a>Windows aracılarını yüklemeden önce hazırlık

Windows çalıştıran bilgisayarlara aracılar yüklemeden önce Docker hizmetini yapılandırmanız gerekir. Yapılandırma, aracıların Docker daemon'a uzaktan erişebilmeleri ve izleme için veri yakalayabilmeleri için Windows aracısının veya Azure Monitor sanal makine uzantısının Docker TCP soketi kullanmasına olanak tanır.

##### <a name="to-configure-the-docker-service"></a>Docker hizmetini yapılandırmak için  

Windows Server için TCP borusunu ve adlandırılmış boruyu etkinleştirmek için aşağıdaki PowerShell komutlarını gerçekleştirin:

```
Stop-Service docker
dockerd --unregister-service
dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
Start-Service docker
```

Windows Kapsayıcıları ile kullanılan Docker daemon yapılandırması hakkında daha fazla bilgi için [Windows'da Docker Engine'e](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon)bakın.

#### <a name="install-windows-agents"></a>Windows aracılarını yükleme

Windows ve Hyper-V kapsayıcı izlemeyi etkinleştirmek için, kapsayıcı ana bilgisayarları olan Windows bilgisayarlarına Microsoft İzleme Aracısı'nı (MMA) yükleyin. Windows'u şirket içi ortamınızda çalıştıran bilgisayarlar için [bkz.](../../azure-monitor/platform/agent-windows.md) Azure'da çalışan sanal makineler için, bunları [sanal makine uzantısını](../../azure-monitor/learn/quick-collect-azurevm.md)kullanarak Azure Monitor'a bağlayın.

Service Fabric'te çalışan Windows kapsayıcılarını izleyebilirsiniz. Ancak, şu anda yalnızca [Azure'da çalışan sanal makineler](../../azure-monitor/learn/quick-collect-azurevm.md) ve [şirket içi ortamınızda Windows çalıştıran bilgisayarlar](../../azure-monitor/platform/agent-windows.md) Hizmet Kumaşı için desteklenir.

Kapsayıcı İzleme çözümünü Windows için doğru şekilde ayarladığını doğrulayabilirsiniz. Yönetim paketinin düzgün bir şekilde indirilip indirilmediğini kontrol etmek için *ContainerManagement.xxx.* Dosyalar C:\Program Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs klasöründe olmalıdır.

## <a name="solution-components"></a>Çözüm bileşenleri

Azure portalından *Çözümler Galerisi'ne* gidin ve **Kapsayıcı İzleme Çözümü'ne**ekleyin. Windows aracıları kullanıyorsanız, bu çözümü eklediğinizde her bilgisayara bir aracıyla aşağıdaki yönetim paketi yüklenir. Yönetim paketi için yapılandırma veya bakım gerekmez.

- *C:\Program* Files\Microsoft Monitoring Agent\Agent\Health Service State\Management Packs yüklü ContainerManagement.xxx

## <a name="container-data-collection-details"></a>Kapsayıcı veri toplama ayrıntıları

Kapsayıcı İzleme çözümü, etkinleştirdiğiniz aracıları kullanarak konteyner ana bilgisayarlarından ve kapsayıcılardan çeşitli performans ölçümleri ve günlük verileri toplar.

Veriler aşağıdaki aracı türlerine göre her üç dakikada bir toplanır.

- [Linux için Log Analytics aracısı](../../azure-monitor/learn/quick-collect-linux-computer.md)
- [Windows aracısı](../../azure-monitor/platform/agent-windows.md)
- [Günlük Analytics VM uzantısı](../../azure-monitor/learn/quick-collect-azurevm.md)

### <a name="container-records"></a>Konteyner kayıtları

Aşağıdaki tablo, Kapsayıcı İzleme çözümü tarafından toplanan kayıtların örneklerini ve günlük arama sonuçlarında görünen veri türlerini gösterir.

| Veri türü | Günlük Arama'da veri türü | Alanlar |
| --- | --- | --- |
| Ana bilgisayarlar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName %&#40; İşlemci Süresi, Disk OKUR MB, Disk Yazar MB, Bellek Kullanımı MB, Ağ Bayt ları Al, Ağ Gönder Bayt, İşlemci Kullanımı sn, Ağ&#41;, CounterValue,TimeGenerated, CounterPath, SourceSystem |
| Konteyner envanteri | `ContainerInventory` | TimeGenerated, Bilgisayar, konteyner adı, ContainerHostname, Resim, ImageTag, ContainerState, ExitCode, EnvironmentVar, Komut, CreatedTime, StartedTime, FinishedTime, SourceSystem, ContainerID, ImageID |
| Konteyner görüntü envanteri | `ContainerImageInventory` | TimeGenerated, Bilgisayar, Görüntü, ImageTag, ImageSize, VirtualSize, Running, Duraklatılmış, Durduruldu, Başarısız, SourceSystem, ImageID, TotalContainer |
| Konteyner günlüğü | `ContainerLog` | TimeGenerated, Bilgisayar, resim kimliği, konteyner adı, LogEntrySource, LogEntry, SourceSystem, ContainerID |
| Konteyner servis günlüğü | `ContainerServiceLog`  | zaman, bilgisayar, zaman komutu, görüntü, komut, kaynak sistemi, containerID |
| Konteyner düğümü envanteri | `ContainerNodeInventory_CL`| Zamanlı, Bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Kaynak Sistemi|
| Kubernetes envanteri | `KubePodInventory_CL` | Zamanlı, Bilgisayar, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, Kaynak Sistemi |
| Konteyner işlemi | `ContainerProcess_CL` | Zamanlı, Bilgisayar, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, Kaynak Sistemi |
| Kubernetes etkinlikleri | `KubeEvents_CL` | Zaman, Bilgisayar, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, Kaynak Sistemi, Mesaj |

*PodLabel* veri türlerine eklenen etiketler, kendi özel etiketlerinizdir. Tabloda gösterilen eklenen PodLabel etiketleri örneklerdir. Yani, `PodLabel_deployment_s` `PodLabel_deploymentconfig_s`, `PodLabel_docker_registry_s` , ortamınızın veri kümesi farklı olacak `PodLabel_yourlabel_s`ve genel olarak benzer .

## <a name="monitor-containers"></a>Kapsayıcıları izleme
Azure portalında çözümü etkinleştirdikten **sonra, Kapsayıcılar** kutucuğu konteyner ana bilgisayarlarınız ve ana bilgisayarlarda çalışan kapsayıcılar hakkında özet bilgileri gösterir.

![Konteynerler döşeme](./media/containers/containers-title.png)

Döşeme, çevrede kaç kapsayıcınız olduğunu ve başarısız olup olmadıklarını, çalışıp çalışmadıklarını veya durdurulduklarını gösterir.

### <a name="using-the-containers-dashboard"></a>Kapsayıcılar panosunu kullanma

**Kapsayıcılar** döşemesini tıklatın. Buradan size göreceksiniz:

- **Kapsayıcı Olayları** - Kapsayıcı durumunu ve başarısız kapsayıcılı bilgisayarları gösterir.
- **Kapsayıcı Günlükleri** - Zaman içinde oluşturulan kapsayıcı günlük dosyalarının grafiğini ve en yüksek günlük dosyası sayısına sahip bilgisayarların listesini gösterir.
- **Kubernetes Etkinlikleri** - Zaman içinde oluşturulan Kubernetes olaylarının bir grafiğini ve bölmelerin olayları neden oluşturduğunun bir listesini gösterir. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **Kubernetes Namespace Inventory** - Ad alanlarının ve bölmelerin sayısını gösterir ve hiyerarşilerini gösterir. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **Konteyner Düğümü Envanteri** - Konteyner düğümlerinde/ana bilgisayarlarında kullanılan orkestrasyon türlerinin sayısını gösterir. Bilgisayar düğümleri/ana bilgisayarlar da kapsayıcı sayısına göre listelenir. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **Konteyner Görüntüleri Envanteri** - Kullanılan toplam kapsayıcı görüntüsü sayısını ve görüntü türlerinin sayısını gösterir. Görüntü sayısı da görüntü etiketi tarafından listelenir.
- **Kapsayıcılar Durumu** - Kapsayıcıları çalıştıran toplam kapsayıcı düğümü/ana bilgisayar sayısını gösterir. Bilgisayarlar da çalışan ana bilgisayar sayısına göre listelenir.
- **Konteyner İşlemi** - Zaman içinde çalışan konteyner işlemlerinin bir çizgi grafiğini gösterir. Kapsayıcılar, kapsayıcılar içinde komut/işlem çalıştırılarak da listelenir. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **Kapsayıcı CPU Performansı** - Bilgisayar düğümleri/ana bilgisayarlar için zaman içinde ortalama CPU kullanımının bir çizgi grafiğini gösterir. Ayrıca, ortalama CPU kullanımına göre bilgisayar düğümlerini/ana bilgisayarlarını listeler.
- **Kapsayıcı Bellek Performansı** - Zaman içinde bellek kullanımının bir çizgi grafiğini gösterir. Ayrıca, örnek adına göre bilgisayar belleği kullanımını da listeler.
- **Bilgisayar Performansı** - Zaman içinde CPU performansının yüzdesi, zaman içinde bellek kullanımının yüzdesi ve zaman içinde megabaytlar serbest disk alanının satır grafiklerini gösterir. Daha fazla ayrıntı görüntülemek için grafikteki herhangi bir satırın üzerinde gezinebilirsiniz.

Pano'nun her alanı, toplanan veriler üzerinde çalıştırılan bir aramanın görsel bir temsilidir.

![Konteynerler panosu](./media/containers/containers-dash01.png)

![Konteynerler panosu](./media/containers/containers-dash02.png)

Kapsayıcı **Durumu** alanında, aşağıda gösterildiği gibi üst teki alanı tıklatın.

![Kapsayıcılar durumu](./media/containers/containers-status.png)

Log Analytics açılır ve kapsayıcılarınızın durumu hakkında bilgi görüntüler.

![Konteynerler için Günlük Analizi](./media/containers/containers-log-search.png)

Buradan, ilgilendiğiniz belirli bilgileri bulmak için arama sorgusunu değiştirmek için değiştirebilirsiniz. Günlük sorguları hakkında daha fazla bilgi için [Azure Monitor'da Günlük sorgularına](../log-query/log-query-overview.md)bakın.

## <a name="troubleshoot-by-finding-a-failed-container"></a>Başarısız bir kapsayıcı bularak sorun giderme

Log Analytics, bir kapsayıcıyı sıfır olmayan bir çıkış koduyla çıktıysa **Başarısız** olarak işaretler. **Başarısız Kapsayıcılar** alanında ortamdaki hatalara ve hatalara genel bir bakış görebilirsiniz.

### <a name="to-find-failed-containers"></a>Başarısız kapsayıcıları bulmak için

1. Kapsayıcı **Durumu** alanını tıklatın.  
   ![konteyner durumu](./media/containers/containers-status.png)
2. Log Analytics, aşağıdakilere benzer şekilde kapsayıcılarınızın durumunu açar ve görüntüler.  
   ![kapsayıcılar durumu](./media/containers/containers-log-search.png)
3. Başarısız satırı genişletin ve sorguya ölçütlerini eklemek için +'ya tıklayın. Ardından sorgudaki Özetle satırına yorum yapın.
   ![başarısız kapsayıcılar](./media/containers/containers-state-failed-select.png)  
1. Sorguyu çalıştırın ve ardından resim kimliğini görüntülemek için sonuçlardaki bir satırı genişletin.  
   ![başarısız kapsayıcılar](./media/containers/containers-state-failed.png)  
1. Günlük sorgusunda aşağıdakileri yazın. `ContainerImageInventory | where ImageID == <ImageID>`görüntü boyutu ve durdurulmuş ve başarısız görüntü sayısı gibi görüntü yle ilgili ayrıntıları görmek için.  
   ![başarısız kapsayıcılar](./media/containers/containers-failed04.png)

## <a name="query-logs-for-container-data"></a>Kapsayıcı verileri için sorgu günlükleri

Belirli bir hatayı giderirken, ortamınızda nerede oluştuğunu görmenize yardımcı olabilir. Aşağıdaki günlük türleri, istediğiniz bilgileri döndürmek için sorgular oluşturmanıza yardımcı olur.

- **ContainerImageInventory** – Görüntüye göre düzenlenmiş bilgileri bulmaya ve görüntü teşekkülleri veya boyutları gibi görüntü bilgilerini görüntülemeye çalışırken bu türü kullanın.
- **ContainerInventory** – Kapsayıcı konumu, adlarının ne olduğu ve hangi görüntüleri çalıştırdıkları hakkında bilgi istediğinizde bu türü kullanın.
- **ContainerLog** – Belirli hata günlüğü bilgilerini ve girişlerini bulmak istediğinizde bu türü kullanın.
- **ContainerNodeInventory_CL**  Kapsayıcıların bulunduğu ana bilgisayar/düğüm hakkında bilgi istediğinizde bu türü kullanın. Docker sürümü, orkestrasyon türü, depolama ve ağ bilgileri sağlar.
- **ContainerProcess_CL** Kapsayıcı içinde çalışan işlemi hızlı bir şekilde görmek için bu türü kullanın.
- **ContainerServiceLog** – Docker daemon için başlat, durdur, silmek veya komut çekme gibi denetim izi bilgilerini bulmaya çalışırken bu türü kullanın.
- **KubeEvents_CL**  Kubernetes olaylarını görmek için bu türü kullanın.
- **KubePodInventory_CL**  Küme hiyerarşisi bilgilerini anlamak istediğinizde bu türü kullanın.


### <a name="to-query-logs-for-container-data"></a>Kapsayıcı verileri için günlükleri sorgulamak için

* Son zamanlarda başarısız olduğunu bildiğiniz bir resim seçin ve bunun için hata günlükleri bulabilirsiniz. **ContainerInventory** aramasıyla bu görüntüyü çalıştıran bir kapsayıcı adını bularak başlayın. Örneğin,`ContainerInventory | where Image == "ubuntu" and ContainerState == "Failed"`  
    ![Ubuntu konteynerleri ara](./media/containers/search-ubuntu.png)

  Bu kapsayıcının ayrıntılarını görüntülemek için sonuçlardaki satırları genişletin.

## <a name="example-log-queries"></a>Örnek günlük sorguları

Genellikle bir veya iki örnekle başlayıp ortamınıza uyacak şekilde bunları değiştirmek için sorgular oluşturmak yararlıdır. Başlangıç noktası olarak, daha gelişmiş sorgular oluşturmanıza yardımcı olmak için **Örnek Sorgular** alanını deneyebilirsiniz.

![Kapsayıcı sorguları](./media/containers/containers-queries.png)

## <a name="saving-log-queries"></a>Günlük sorgularını kaydetme

Sorguları kaydetme, Azure Monitor'da standart bir özelliktir. Bunları kaydederek, gelecekte kullanmak için yararlı bulduğunuz kişilere sahip olacaksınız.

Yararlı bulduğunuz bir sorgu oluşturduktan sonra, Günlük Arama sayfasının üst **kısmındaki Sık Kullanılanlar'ı** tıklatarak sorguyu kaydedin. Ardından **Panom** sayfasından kolayca erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı kapsayıcı veri kayıtlarını görüntülemek için [günlükleri sorgulayın.](../log-query/log-query-overview.md)
