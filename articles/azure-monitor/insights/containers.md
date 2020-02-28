---
title: Azure Izleyici 'de kapsayıcı Izleme çözümü | Microsoft Docs
description: Azure Izleyici 'de kapsayıcı Izleme çözümü, Docker ve Windows kapsayıcı konaklarınızı tek bir konumda görüntülemenize ve yönetmenize yardımcı olur.
ms.subservice: logs
ms.topic: conceptual
author: mgoedtel
ms.author: magoedte
ms.date: 07/22/2019
ms.openlocfilehash: 171f897f6e110e8f759281c139addab477ecede3
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77664703"
---
# <a name="container-monitoring-solution-in-azure-monitor"></a>Azure Izleyici 'de kapsayıcı Izleme çözümü

![Kapsayıcıları simgesi](./media/containers/containers-symbol.png)

Bu makalede, Azure Izleyici 'de kapsayıcı Izleme çözümünün nasıl ayarlanacağı ve kullanılacağı açıklanır. Bu, Docker ve Windows kapsayıcı konaklarınızı tek bir konumda görüntülemenize ve yönetmenize yardımcı olur. Docker, yazılım dağıtımı için BT altyapısını otomatikleştirmek kapsayıcılar oluşturmak için kullanılan bir yazılım sanallaştırma sistemidir.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

Çözüm gösterir hangi kapsayıcıları çalıştırmak, hangi kapsayıcı görüntüsü ister çalışıyor ve kapsayıcıları çalıştığı. Kapsayıcılar ile kullanılan komutları gösteren ayrıntılı denetim bilgileri görüntüleyebilirsiniz. Ayrıca, kapsayıcıları, Docker veya Windows konak uzaktan görüntüleme gerek kalmadan Merkezi günlük arama ve görüntüleme giderebilirsiniz. Bir konakta gürültülü ve alıcı aşırı kaynakları olabilir kapsayıcıları bulabilirsiniz. Ve merkezileştirilmiş CPU, bellek, depolama ve kapsayıcılar için ağ kullanım ve performans bilgilerini görüntüleyebilirsiniz. Windows çalıştıran bilgisayarlarda, merkezileştirme ve Windows Server günlüklerinden karşılaştırın Hyper-V ve Docker kapsayıcıları. Çözüm, aşağıdaki kapsayıcı düzenleyicileri destekler:

- Docker Swarm
- DC/OS
- Kubernetes
- Service Fabric
- Red Hat OpenShift

[Azure Service Fabric](../../service-fabric/service-fabric-overview.md)'te dağıtılan kapsayıcılar varsa, küme olaylarının izlenmesini dahil etmek için hem [Service Fabric çözümü](../../service-fabric/service-fabric-diagnostics-oms-setup.md) hem de bu çözümü etkinleştirmenizi öneririz. Service Fabric çözümünü etkinleştirmeden önce, ne sağladığını ve nasıl kullanılacağını anlamak için [Service Fabric çözümünü kullanarak](../../service-fabric/service-fabric-diagnostics-event-analysis-oms.md) gözden geçirin.

Azure Kubernetes Service (AKS) üzerinde barındırılan Kubernetes ortamlarına dağıtılan iş yüklerinizin performansını izlemeye ilgileniyorsanız, bkz. [Azure Kubernetes hizmetini izleme](../../azure-monitor/insights/container-insights-overview.md). Kapsayıcı Izleme çözümü, bu platformun izlenmesini desteklemiyor.  

Aşağıdaki diyagramda, Azure Izleyici ile çeşitli kapsayıcı konakları ve aracıları arasındaki ilişkiler gösterilmektedir.

![Kapsayıcıları diyagramı](./media/containers/containers-diagram.png)

## <a name="system-requirements-and-supported-platforms"></a>Sistem gereksinimleri ve desteklenen platformlar

Başlamadan önce önkoşulları karşılaması doğrulamak için aşağıdaki ayrıntıları gözden geçirin.

### <a name="container-monitoring-solution-support-for-docker-orchestrator-and-os-platform"></a>Kapsayıcı izleme çözümü desteklemek için Docker Orchestrator ve işletim sistemi platformu

Aşağıdaki tabloda, Azure Izleyici ile kapsayıcı envanteri, performansı ve günlükleri için Docker düzenleme ve işletim sistemi izleme desteği özetlenmektedir.   

| | ACS | Linux | Windows | Kapsayıcı<br>Envanter | Görüntü<br>Envanter | Node<br>Envanter | Kapsayıcı<br>Performans | Kapsayıcı<br>Olay | Olay<br>Günlük | Kapsayıcı<br>Günlük |
|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|-----|
| Kubernetes | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Mesosphere<br>DC/OS | &#8226; | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; |
| Docker<br>Swarm | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Hizmet<br>Fabric | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; |
| Red Hat Aç<br>Shift | | &#8226; | | &#8226; | &#8226;| &#8226; | &#8226; | &#8226; | | &#8226; |
| Windows Server<br>(tek başına) | | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |
| Linux Sunucu<br>(tek başına) | | &#8226; | | &#8226; | &#8226; | &#8226; | &#8226; | &#8226; | | &#8226; |

### <a name="docker-versions-supported-on-linux"></a>Linux üzerinde desteklenen docker sürümleri

- Docker için 1.11 1.13
- Docker CE ve EE v17.06

### <a name="x64-linux-distributions-supported-as-container-hosts"></a>x64 kapsayıcı konakları olarak desteklenen Linux dağıtımları

- Ubuntu 14.04 LTS ve 16.04 LTS
- CoreOS(stable)
- Amazon Linux 2016.09.0
- openSUSE 13.2
- openSUSE 42.2 LEAP
- CentOS 7.2 ve 7.3
- SLES 12
- RHEL 7.2 ve 7.3
- Red Hat OpenShift kapsayıcı Platformu (OCP) 3.4 ve 3.5
- ACS Mesosphere DC/OS için 1.7.3 1.8.8
- ACS Kubernetes için 1.4.5 1.6
    - Kubernetes olayları, Kubernetes Envanter ve kapsayıcı işlemleri daha sonra Linux için Log Analytics Aracısı'nın sürümü 1.4.1-45 ile yalnızca desteklenen
- ACS Docker Swarm

[!INCLUDE [log-analytics-agent-note.md](../../../includes/log-analytics-agent-note.md)] 

### <a name="supported-windows-operating-system"></a>Desteklenen Windows işletim sistemi

- Windows Server 2016
- Windows 10 Anniversary Edition (Professional veya Enterprise)

### <a name="docker-versions-supported-on-windows"></a>Desteklenen Windows docker sürümleri

- Docker 1.12 ve 1.13
- Docker 17.03.0 ve üzeri

## <a name="installing-and-configuring-the-solution"></a>Çözümünü yükleme ve yapılandırma

Çözümü yüklemek ve yapılandırmak için aşağıdaki bilgileri kullanın.

1. Kapsayıcı Izleme çözümünü [Azure Marketi](https://azuremarketplace.microsoft.com/marketplace/apps/Microsoft.ContainersOMS?tab=Overview) 'ndeki Log Analytics çalışma alanınıza veya [Çözüm Galerisi izleme çözümlerini ekleme](../../azure-monitor/insights/solutions.md)bölümünde açıklanan işlemi kullanarak ekleyin.

2. Yükleyin ve Docker ile bir Log Analytics aracısını kullanın. İşletim sistemi ve Docker orchestrator bağlı olarak, aracınızı yapılandırmak için aşağıdaki yöntemleri kullanabilirsiniz.
   - Tek başına konakları için:
     - Desteklenen Linux işletim sistemlerinde Docker 'ı yükleyip çalıştırın ve [Linux için Log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md)yükleyip yapılandırın.  
     - CoreOS üzerinde Linux için Log Analytics aracısını çalıştıramazsınız. Bunun yerine, Linux için Log Analytics aracısını kapsayıcı bir sürümünü çalıştırın. Azure Kamu bulutundaki kapsayıcılarla çalışıyorsanız, CoreOS veya Azure Kamu Linux kapsayıcı Konakları dahil olmak üzere Linux kapsayıcı konaklarını inceleyin.
     - Windows Server 2016 ve Windows 10 ' da Docker altyapısını ve istemcisini yükledikten sonra, bilgi toplamak ve Azure Izleyici 'ye göndermek için bir aracı bağlayın. Windows ortamınız varsa [Windows kapsayıcı konaklarınızı yüklemeyi ve yapılandırmayı](#install-and-configure-windows-container-hosts) gözden geçirin.
   - Docker birden çok konak düzenleme için:
     - Red Hat OpenShift ortamınız varsa, Red Hat OpenShift için Log Analytics Aracısı yapılandırma ' yı gözden geçirin.
     - Azure Container Service kullanan bir Kubernetes kümesi varsa:
       - [Kubernetes için Log Analytics Linux Aracısı yapılandırma](#configure-a-log-analytics-linux-agent-for-kubernetes)konusunu gözden geçirin.
       - [Kubernetes için Log Analytics Windows Aracısı yapılandırma](#configure-a-log-analytics-windows-agent-for-kubernetes)konusunu gözden geçirin.
       - Linux Kubernetes 'te Log Analytics Aracısı dağıtmak için Held kullanın.
     - Bir Azure Container Service DC/OS kümeniz varsa, [Azure izleyici ile Azure CONTAINER SERVICE DC/OS kümesini izleme](../../container-service/dcos-swarm/container-service-monitoring-oms.md)hakkında daha fazla bilgi edinin.
     - Docker Sısınma modu ortamınız varsa, Docker Sısınma için Log Analytics Aracısı yapılandırma hakkında daha fazla bilgi edinin.
     - Bir Service Fabric kümeniz varsa, [Azure izleyici ile kapsayıcıları izlemek](../../service-fabric/service-fabric-diagnostics-oms-containers.md)için daha fazla bilgi edinin.

Windows çalıştıran bilgisayarlarda Docker altyapılarını yüklemek ve yapılandırmak hakkında daha fazla bilgi için [Windows makalesindeki Docker altyapısını](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon) gözden geçirin.

> [!IMPORTANT]
> Kapsayıcı konaklarınıza [Linux için Log Analytics aracısını](../../azure-monitor/learn/quick-collect-linux-computer.md) yüklemeden **önce** Docker çalışıyor olmalıdır. Docker'ı yüklemeden önce aracıyı zaten yüklediyseniz, Linux için Log Analytics aracısını yeniden yüklemeniz gerekir. Docker hakkında daha fazla bilgi için bkz. [Docker Web sitesi](https://www.docker.com).

### <a name="install-and-configure-linux-container-hosts"></a>Yükleme ve yapılandırma Linux kapsayıcı konakları

Docker'ı yükledikten sonra aracıyı kullanmak için Docker ile yapılandırmak için kapsayıcı konağı için aşağıdaki ayarları kullanın. İlk Log Analytics çalışma alanı kimliği ve Azure portalında bulabilirsiniz anahtarı gerekir. Çalışma alanınızda, **çalışma alanı kimliğinizi** ve **birincil anahtarınızı**görüntülemek için **hızlı başlangıç** > **bilgisayarlar** ' a tıklayın.  Her ikisini de kopyalayıp sık kullandığınız bir düzenleyiciye yapıştırın.

**CoreOS hariç tüm Linux kapsayıcı konakları için:**

- Linux için Log Analytics aracısının nasıl yükleneceğine ilişkin daha fazla bilgi ve adım için, bkz. [Log Analytics aracısına genel bakış](../../azure-monitor/platform/log-analytics-agent.md).

**CoreOS dahil tüm Linux kapsayıcı konakları için:**

İzlemek istediğiniz kapsayıcı başlatın. Değiştirin ve aşağıdaki örneği kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -h=`hostname` -p 127.0.0.1:25225:25225 --name="omsagent" --restart=always microsoft/oms
```

**CoreOS dahil tüm Azure Kamu Linux kapsayıcı konakları için:**

İzlemek istediğiniz kapsayıcı başlatın. Değiştirin ve aşağıdaki örneği kullanın:

```
sudo docker run --privileged -d -v /var/run/docker.sock:/var/run/docker.sock -v /var/log:/var/log -v /var/lib/docker/containers:/var/lib/docker/containers -e WSID="your workspace id" -e KEY="your key" -e DOMAIN="opinsights.azure.us" -p 127.0.0.1:25225:25225 -p 127.0.0.1:25224:25224/udp --name="omsagent" -h=`hostname` --restart=always microsoft/oms
```

**Bir kapsayıcıda yüklü bir Linux aracısını kullanarak geçiş yapma**

Daha önce yüklenmiş doğrudan Aracısı kullanılan ve bunun yerine bir kapsayıcıda çalışan bir aracının kullanmak istiyorsanız, önce Linux için Log Analytics aracısını kaldırmanız gerekir. Aracının başarıyla nasıl kaldırılacağını anlamak için bkz. [Linux için Log Analytics Aracısı 'Nı kaldırma](../../azure-monitor/learn/quick-collect-linux-computer.md) .  

#### <a name="configure-a-log-analytics-agent-for-docker-swarm"></a>Docker Swarm için bir Log Analytics aracısını yapılandırma

Log Analytics aracısını, Docker Swarm hakkında genel bir hizmet olarak çalıştırabilirsiniz. Log Analytics Aracısı hizmeti oluşturmak için aşağıdaki bilgileri kullanın. Log Analytics çalışma alanı kimliği ve birincil anahtarı sağlamanız gerekir.

- Aşağıdaki ana düğüm üzerinde çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers -e WSID="<WORKSPACE ID>" -e KEY="<PRIMARY KEY>" -p 25225:25225 -p 25224:25224/udp  --restart-condition=on-failure microsoft/oms
    ```

##### <a name="secure-secrets-for-docker-swarm"></a>Docker Swarm için güvenli bir gizli dizi

Çalışma alanı kimliği ve birincil anahtar için gizli anahtar oluşturulduktan sonra Docker Swarm için gizli bilgilerinizi oluşturmak için aşağıdaki bilgileri kullanın.

1. Aşağıdaki ana düğüm üzerinde çalıştırın.

    ```
    echo "WSID" | docker secret create WSID -
    echo "KEY" | docker secret create KEY -
    ```

2. Gizli dizileri düzgün oluşturulduğunu doğrulayın.

    ```
    keiko@swarmm-master-13957614-0:/run# sudo docker secret ls
    ```

    ```
    ID                          NAME                CREATED             UPDATED
    j2fj153zxy91j8zbcitnjxjiv   WSID                43 minutes ago      43 minutes ago
    l9rh3n987g9c45zffuxdxetd9   KEY                 38 minutes ago      38 minutes ago
    ```

3. Log Analytics aracısını kapsayıcı için gizli dizileri bağlamak için aşağıdaki komutu çalıştırın.

    ```
    sudo docker service create  --name omsagent --mode global  --mount type=bind,source=/var/run/docker.sock,destination=/var/run/docker.sock --mount type=bind,source=/var/lib/docker/containers,destination=/var/lib/docker/containers --secret source=WSID,target=WSID --secret source=KEY,target=KEY  -p 25225:25225 -p 25224:25224/udp --restart-condition=on-failure microsoft/oms
    ```

#### <a name="configure-a-log-analytics-agent-for-red-hat-openshift"></a>Red Hat OpenShift için bir Log Analytics aracısını yapılandırma

Log Analytics aracısını kapsayıcı izleme verilerini toplamaya başlamak için Red Hat OpenShift için eklemenin üç yolu vardır.

* Her OpenShift düğümüne [Linux için Log Analytics aracısını doğrudan yüklemeyi](../../azure-monitor/learn/quick-collect-linux-computer.md)  
* Azure 'da bulunan her OpenShift düğümünde [log ANALYTICS VM uzantısını etkinleştirin](../../azure-monitor/learn/quick-collect-azurevm.md)  
* Log Analytics aracısını OpenShift daemon olarak kurma  

Bu bölümde bir OpenShift arka plan programı kümesi olarak Log Analytics aracısını yüklemek için gerekli adımları ele.  

1. OpenShift ana düğümünde oturum açın ve YAML dosyası [OCP-omsagent. YAML](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-omsagent.yaml) öğesini GitHub 'dan ana düğümünüz ile kopyalayın ve değeri Log Analytics çalışma alanı kimliğinizle ve birincil anahtarınızla değiştirin.
2. Azure Izleyici için bir proje oluşturmak ve Kullanıcı hesabını ayarlamak için aşağıdaki komutları çalıştırın.

    ```
    oc adm new-project omslogging --node-selector='zone=default'
    oc project omslogging  
    oc create serviceaccount omsagent  
    oc adm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oc adm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

3. Arka plan programı kümesini dağıtmak için aşağıdaki komutu çalıştırın:

    `oc create -f ocp-omsagent.yaml`

4. Bunu doğrulamak için yapılandırılır ve çalışma doğru şunu yazın:

    `oc describe daemonset omsagent`  

    ve çıktıya benzemelidir:

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

Log Analytics aracısını arka plan programı kümesi yaml dosyası kullanırken, Log Analytics çalışma alanı kimliği ve birincil anahtarınızı korumak için gizli dizileri kullanmak istiyorsanız, aşağıdaki adımları gerçekleştirin.

1. OpenShift ana düğümünde oturum açın ve YAML dosyası [OCP-DS-omsagent. YAML](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-ds-omsagent.yaml) ve gizli betik oluşturma [OCP-secretgen.sh](https://github.com/Microsoft/OMS-docker/blob/master/OpenShift/ocp-secretgen.sh) öğesini GitHub 'dan kopyalayın.  Bu betik güvenli hale getirmek Log Analytics çalışma alanı kimliği ve birincil anahtar için gizli dizi yaml dosyası oluşturacak, bilgi secrete.  
2. Azure Izleyici için bir proje oluşturmak ve Kullanıcı hesabını ayarlamak için aşağıdaki komutları çalıştırın. Gizli betik oluşturma, Log Analytics çalışma alanı KIMLIĞI `<WSID>` ve birincil anahtar `<KEY>` ister ve tamamlandığında, OCP-Secret. YAML dosyasını oluşturur.  

    ```
    oc adm new-project omslogging --node-selector='zone=default'  
    oc project omslogging  
    oc create serviceaccount omsagent  
    oc adm policy add-cluster-role-to-user cluster-reader   system:serviceaccount:omslogging:omsagent  
    oc adm policy add-scc-to-user privileged system:serviceaccount:omslogging:omsagent  
    ```

3. Gizli dizi dosyasını aşağıdaki komutu çalıştırarak dağıtın:

    `oc create -f ocp-secret.yaml`

4. Aşağıdaki komutu çalıştırarak dağıtımı doğrulama:

    `oc describe secret omsagent-secret`  

    ve çıktıya benzemelidir:  

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

5. Log Analytics aracısını arka plan programı kümesi yaml dosyası, aşağıdaki komutu çalıştırarak dağıtın:

    `oc create -f ocp-ds-omsagent.yaml`  

6. Aşağıdaki komutu çalıştırarak dağıtımı doğrulama:

    `oc describe ds oms`

    ve çıktıya benzemelidir:

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

#### <a name="configure-a-log-analytics-linux-agent-for-kubernetes"></a>Kubernetes için bir Log Analytics Linux Aracısı'nı yapılandırma

Kubernetes için Linux için Log Analytics aracısını yüklemek çalışma alanı kimliği ve birincil anahtar için gizli dizileri yaml dosyası oluşturmak için bir betik kullanın. [Log Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes) sayfasında, gizli bilginiz olmadan veya bunlarla birlikte kullanabileceğiniz dosyalar vardır.

- Linux DaemonSet için varsayılan Log Analytics aracısını gizli bilgileri (omsagent.yaml) yok
- Linux DaemonSet yaml dosyası için Log Analytics aracısını gizli anahtarları (omsagentsecret.yaml) yaml dosyası oluşturmak için gizli dizi oluşturma betikleri ile gizli bilgilere (omsagent-ds-secrets.yaml) kullanır.

İle veya olmadan gizli dizileri omsagent DaemonSets oluşturmayı seçebilirsiniz.

**Gizli olmayan varsayılan OMSagent DaemonSet YAML dosyası**

- Varsayılan Log Analytics Agent DaemonSet YAML dosyası için `<WSID>` ve `<KEY>` yerine WSıD ve ANAHTARıNıZLA değiştirin. Dosyayı, ana düğüme kopyalayın ve aşağıdaki komutu çalıştırın:

    ```
    sudo kubectl create -f omsagent.yaml
    ```

**Gizli olmayan varsayılan OMSagent DaemonSet YAML dosyası**

1. Log Analytics'i kullanmak için aracı gizli bilgileri ile DaemonSet oluşturun gizli dizileri önce.
    1. Betik ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduklarından emin olun.
        - Gizli dizi betiği - gizli gen.sh oluşturuluyor
        - Gizli şablon - gizli template.yaml
    2. Aşağıdaki örnekte olduğu gibi betiği çalıştırın. Log Analytics çalışma alanı kimliği ve birincil anahtar için betik sorar ve bunları girdikten sonra yapmanız çalıştırabilirsiniz betik gizli yaml dosyası oluşturur.   

        ```
        #> sudo bash ./secret-gen.sh
        ```

    3. Gizli dizileri pod, aşağıdaki komutu çalıştırarak oluşturun:
        ```
        sudo kubectl create -f omsagentsecret.yaml
        ```

    4. Doğrulamak için aşağıdaki komutu çalıştırın:

        ```
        keiko@ubuntu16-13db:~# sudo kubectl get secrets
        ```

        Çıktıya benzemelidir:

        ```
        NAME                  TYPE                                  DATA      AGE
        default-token-gvl91   kubernetes.io/service-account-token   3         50d
        omsagent-secret       Opaque                                2         1d
        ```

        ```
        keiko@ubuntu16-13db:~# sudo kubectl describe secrets omsagent-secret
        ```

        Çıktıya benzemelidir:

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

    5. Omsagent Daemon 'nizi oluşturma-```sudo kubectl create -f omsagent-ds-secrets.yaml``` çalıştırarak ayarlama

2. Log Analytics aracısını DaemonSet, aşağıdaki gibi çalıştığını doğrulayın:

    ```
    keiko@ubuntu16-13db:~# sudo kubectl get ds omsagent
    ```

    ```
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   3         3         <none>          1h
    ```

Kubernetes için gizli dizileri yaml dosyası için Linux için Log Analytics aracısını çalışma alanı kimliği ve birincil anahtar oluşturmak için bir betik kullanın. Gizli bilgilerinizin güvenliğini sağlamak için [omsagent YAML dosyası](https://github.com/Microsoft/OMS-docker/blob/master/Kubernetes/omsagent.yaml) ile aşağıdaki örnek bilgileri kullanın.

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

#### <a name="configure-a-log-analytics-windows-agent-for-kubernetes"></a>Kubernetes için bir Log Analytics Windows Aracısı yapılandırın

Windows Kubernetes için Log Analytics aracısını yüklemek çalışma alanı kimliği ve birincil anahtar için gizli dizileri yaml dosyası oluşturmak için bir betik kullanın. [Log Analytics Docker Kubernetes GitHub](https://github.com/Microsoft/OMS-docker/tree/master/Kubernetes/windows) sayfasında, gizli bilgileriniz ile kullanabileceğiniz dosyalar vardır.  Ana ve aracı düğümleri için ayrı ayrı Log Analytics aracısını yüklemeniz gerekir.  

1. Log Analytics aracısını DaemonSet kullanmak için asıl gizli bilgileri kullanarak düğümünü açın ve gizli dizileri ilk oluşturun.
    1. Betik ve gizli şablon dosyasını kopyalayın ve aynı dizinde olduklarından emin olun.
        - Gizli dizi betiği - gizli gen.sh oluşturuluyor
        - Gizli şablon - gizli template.yaml

    2. Aşağıdaki örnekte olduğu gibi betiği çalıştırın. Log Analytics çalışma alanı kimliği ve birincil anahtar için betik sorar ve bunları girdikten sonra yapmanız çalıştırabilirsiniz betik gizli yaml dosyası oluşturur.

        ```
        #> sudo bash ./secret-gen.sh
        ```
    3. Omsagent Daemon 'nizi oluşturma-```kubectl create -f omsagentsecret.yaml``` çalıştırarak ayarlama
    4. Denetlemek için şu komutu çalıştırın:

        ```
        root@ubuntu16-13db:~# kubectl get secrets
        ```

        Çıktıya benzemelidir:

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

    5. Omsagent Daemon 'nizi oluşturma-```kubectl create -f ws-omsagent-de-secrets.yaml``` çalıştırarak ayarlama

2. Log Analytics aracısını DaemonSet, aşağıdaki gibi çalıştığını doğrulayın:

    ```
    root@ubuntu16-13db:~# kubectl get deployment omsagent
    NAME       DESIRED   CURRENT   NODE-SELECTOR   AGE
    omsagent   1         1         <none>          1h
    ```

3. Aracıyı Windows çalıştıran çalışan düğümüne yüklemek için, [Windows kapsayıcı konaklarını yükleyip yapılandırma](#install-and-configure-windows-container-hosts)bölümündeki adımları izleyin.

#### <a name="use-helm-to-deploy-log-analytics-agent-on-linux-kubernetes"></a>Helm Linux Kubernetes Log Analytics aracısını dağıtmak için kullanın

Linux Kubernetes ortamınızı Log Analytics aracısını dağıtmak için Helm kullanmak için aşağıdaki adımları gerçekleştirin.

1. Omsagent Daemon 'nizi oluşturma-```helm install --name omsagent --set omsagent.secret.wsid=<WSID>,omsagent.secret.key=<KEY> stable/msoms``` çalıştırarak ayarlama
2. Sonuçlar aşağıdakine benzer görünecektir:

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

3. Şu komutu çalıştırarak omsagent 'ın durumunu denetleyebilirsiniz: ```helm status "omsagent"``` ve çıkış aşağıdakine benzer şekilde görünür:

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
   
    Daha fazla bilgi için lütfen [kapsayıcı çözümü HELI grafiğini](https://aka.ms/omscontainerhelm)ziyaret edin.

### <a name="install-and-configure-windows-container-hosts"></a>Yükleme ve Windows kapsayıcı konakları yapılandırma

Bilgileri bölümünde yüklemek ve Windows kapsayıcı konakları yapılandırmak için kullanın.

#### <a name="preparation-before-installing-windows-agents"></a>Windows aracıları yüklemeden önce hazırlama

Windows çalıştıran bilgisayarlarda aracıları yüklemeden önce Docker hizmetinin yapılandırmanız gerekir. Yapılandırma, Windows aracısının veya Azure Izleyici sanal makine uzantısının Docker TCP yuvasını kullanmasına izin verir, böylece aracıların Docker Daemon 'ı uzaktan erişip izleme için veri yakalamalarını sağlayabilirsiniz.

##### <a name="to-configure-the-docker-service"></a>Docker hizmetini yapılandırmak için  

Windows Server için TCP kanalını ve adlandırılmış kanalı etkinleştirmek üzere aşağıdaki PowerShell komutlarını gerçekleştirin:

```
Stop-Service docker
dockerd --unregister-service
dockerd --register-service -H npipe:// -H 0.0.0.0:2375  
Start-Service docker
```

Windows kapsayıcılarıyla kullanılan Docker Daemon yapılandırması hakkında daha fazla bilgi için bkz. [Windows 'Da Docker motoru](https://docs.microsoft.com/virtualization/windowscontainers/manage-docker/configure-docker-daemon).

#### <a name="install-windows-agents"></a>Windows aracıları yükleyin

Windows ve Hyper-V kapsayıcı izlemeyi etkinleştirmek için kapsayıcı konaklarının Windows bilgisayarlarda Microsoft Monitoring Agent (MMA) yükleyin. Şirket içi ortamınızda Windows çalıştıran bilgisayarlar için bkz. [Windows bilgisayarlarını Azure izleyici 'ye bağlama](../../azure-monitor/platform/agent-windows.md). Azure 'da çalışan sanal makineler için, [sanal makine uzantısını](../../azure-monitor/learn/quick-collect-azurevm.md)kullanarak bunları Azure izleyici 'ye bağlayın.

Windows kapsayıcıları Service Fabric üzerinde çalışmasını izleyebilirsiniz. Ancak, yalnızca [Azure 'da çalışan sanal makineler](../../azure-monitor/learn/quick-collect-azurevm.md) ve [Şirket içi ortamınızda Windows çalıştıran bilgisayarlar](../../azure-monitor/platform/agent-windows.md) Service Fabric için desteklenmektedir.

Kapsayıcı izleme çözümü için Windows düzgün şekilde ayarlandığını doğrulayabilirsiniz. Yönetim paketinin doğru şekilde indirilip indirilmediğini denetlemek için *ContainerManagement.xxx*bakın. Dosyaları C:\Program Files\Microsoft Monitoring Agent\Agent\Health hizmet State\Management paketleri klasöründe olmalıdır.

## <a name="solution-components"></a>Çözüm bileşenleri

Azure portal, *Çözüm Galerisi* gidin ve **kapsayıcı izleme çözümünü**ekleyin. Windows aracıları kullanıyorsanız, bu çözümü eklediğinizde, ardından aşağıdaki Yönetim Paketi her bilgisayarda bir aracı yüklenir. Yapılandırma veya bakım için Yönetim Paketi gereklidir.

- *ContainerManagement.xxx* , C:\Program Files\Microsoft Monitoring Tors T\k\sistem sağlığı hizmeti State\Management paketlerine yüklendi

## <a name="container-data-collection-details"></a>Kapsayıcı veri koleksiyonu ayrıntıları

Kapsayıcı izleme çözümü, kapsayıcı konağında ve kapsayıcıları etkinleştirdiğiniz aracıları kullanarak çeşitli performans ölçümleri ve günlük verilerini toplar.

Verileri üç dakikada bir şu aracı türleri tarafından toplanır.

- [Linux için Log Analytics Aracısı](../../azure-monitor/learn/quick-collect-linux-computer.md)
- [Windows Aracısı](../../azure-monitor/platform/agent-windows.md)
- [Log Analytics VM Uzantısı](../../azure-monitor/learn/quick-collect-azurevm.md)

### <a name="container-records"></a>Kapsayıcı kayıt

Aşağıdaki tabloda, kapsayıcı izleme çözümü ve günlük araması sonuçlarında görüntülenen veri türleri tarafından toplanan kayıtları örneklerini gösterir.

| Veri türü | Günlük araması'nda veri türü | Alanlar |
| --- | --- | --- |
| Konaklar ve kapsayıcılar için performans | `Perf` | Bilgisayar, ObjectName, CounterName &#40;% işlemci zamanı, Disk okuma MB, MB, bellek kullanımı MB Disk Yazar ağ bayt alma, ağ gönderme bayt, işlemci kullanımı sn, ağ&#41;, Ort, TimeGenerated, sayaç yolu, Analytics'teki |
| Kapsayıcı envanteri | `ContainerInventory` | TimeGenerated, bilgisayar, kapsayıcı adı, ContainerHostname, görüntü, ImageTag, ContainerState, ExitCode, EnvironmentVar, komut, oluşturulma zamanı, StartedTime, FinishedTime, Analytics'teki, Containerıd, ImageID |
| Kapsayıcı görüntüsü envanterinin | `ContainerImageInventory` | TimeGenerated, bilgisayar, görüntü, ImageTag, ImageSize, VirtualSize, çalışıyor, durduruldu, durduruldu, başarısız, Analytics'teki, ImageID TotalContainer |
| Kapsayıcı günlüğü | `ContainerLog` | TimeGenerated, bilgisayar, görüntü kimliği, LogEntrySource LogEntry, Analytics'teki, Containerıd kapsayıcı adı |
| Kapsayıcı hizmeti günlüğü | `ContainerServiceLog`  | TimeGenerated, bilgisayar, TimeOfCommand, görüntü, komut, Analytics'teki, Containerıd |
| Kapsayıcı düğümü envanteri | `ContainerNodeInventory_CL`| TimeGenerated, bilgisayar, ClassName_s, DockerVersion_s, OperatingSystem_s, Volume_s, Network_s, NodeRole_s, OrchestratorType_s, InstanceID_g, Analytics'teki|
| Kubernetes envanteri | `KubePodInventory_CL` | TimeGenerated, bilgisayar, PodLabel_deployment_s, PodLabel_deploymentconfig_s, PodLabel_docker_registry_s, Name_s, Namespace_s, PodStatus_s, PodIp_s, PodUid_g, PodCreationTimeStamp_t, Analytics'teki |
| Kapsayıcı işlemi | `ContainerProcess_CL` | TimeGenerated, bilgisayar, Pod_s, Namespace_s, ClassName_s, InstanceID_s, Uid_s, PID_s, PPID_s, C_s, STIME_s, Tty_s, TIME_s, Cmd_s, Id_s, Name_s, Analytics'teki |
| Kubernetes olayları | `KubeEvents_CL` | TimeGenerated, bilgisayar, Name_s, ObjectKind_s, Namespace_s, Reason_s, Type_s, SourceComponent_s, Analytics'teki, ileti |

*Pod etiketi* veri türlerine eklenen Etiketler kendi özel etiketleridir. Tabloda belirtilen eklenmiş PodLabel etiketleri verilebilir. `PodLabel_deployment_s`, `PodLabel_deploymentconfig_s``PodLabel_docker_registry_s` ortamınızın veri kümesinde ve genel olarak benzerine benzer `PodLabel_yourlabel_s`farklıdır.

## <a name="monitor-containers"></a>Kapsayıcıları izleme
Azure portal çözümü etkinleştirildikten sonra **kapsayıcılar** kutucuğu, kapsayıcı konaklarınız ve konaklarda çalışan kapsayıcılar hakkındaki özet bilgileri gösterir.

![Kapsayıcıları kutucuğu](./media/containers/containers-title.png)

Çalışıyor veya durduruldu, ortam ve mi başarısız, sahip olduğunuz kaç kapsayıcının genel bir bakış kutucuğu gösterir.

### <a name="using-the-containers-dashboard"></a>Kapsayıcıları panosunu kullanma

**Kapsayıcılar** kutucuğuna tıklayın. Buradan görünümler tarafından düzenlenen görürsünüz:

- **Kapsayıcı olayları** -kapsayıcı durumunu ve başarısız kapsayıcıları olan bilgisayarları gösterir.
- **Kapsayıcı günlükleri** -zaman içinde oluşturulan bir kapsayıcı günlük dosyası grafiği ve en çok sayıda günlük dosyası olan bilgisayarların listesini gösterir.
- **Kubernetes olayları** -zaman içinde oluşturulan bir Kubernetes olayı grafiği ve olayların neden üretilme nedenlerinin bir listesini gösterir. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **Kubernetes ad alanı envanteri** -ad alanları ve düğüm sayısını gösterir ve bunların hiyerarşisini gösterir. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **Kapsayıcı düğümü envanteri** -kapsayıcı düğümlerinde/konaklarda kullanılan düzenleme türlerinin sayısını gösterir. Düğümleri/ana bilgisayar kapsayıcıları sayısına göre de listelenir. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **Kapsayıcı görüntüleri envanteri** -kullanılan kapsayıcı görüntülerinin toplam sayısını ve görüntü türü sayısını gösterir. Görüntü sayısını da resim etiketi listelenir.
- **Kapsayıcılar durumu** -çalışan kapsayıcıları olan kapsayıcı düğümlerinin/konak bilgisayarlarının toplam sayısını gösterir. Bilgisayar, ana çalışan sayısı tarafından da listelenir.
- **Kapsayıcı işlemi** -zaman içinde çalışan bir kapsayıcı işlemleri çizgi grafiğini gösterir. Kapsayıcılar, çalıştırarak da listelenen komutu/işlem kapsayıcıları içinde. *Bu veri kümesi yalnızca Linux ortamlarında kullanılır.*
- **KAPSAYıCı CPU performansı** -bilgisayar düğümleri/konaklar için zaman IÇINDE ortalama CPU kullanımının bir çizgi grafiğini gösterir. Ayrıca listeleri düğümleri/barındıran bilgisayar, ortalama CPU kullanımı temel.
- **Kapsayıcı bellek performansı** -zaman içinde bellek kullanımının bir çizgi grafiğini gösterir. Ayrıca bilgisayar belleği kullanımı örneği adına göre listelenir.
- **Bilgisayar performansı** -zaman içinde CPU performansının yüzdesi, zaman içinde bellek kullanımının yüzdesi ve zaman içinde megabayt boş disk alanı hakkında çizgi grafikleri gösterir. Daha fazla ayrıntı görüntülemek için grafikteki herhangi bir satır üzerine gelerek.

Her Pano görsel bir temsilini topladığınız verileri çalıştırma bir arama alanıdır.

![Kapsayıcılar Panosu](./media/containers/containers-dash01.png)

![Kapsayıcılar Panosu](./media/containers/containers-dash02.png)

**Kapsayıcı durumu** alanında, en üstteki alana aşağıda gösterildiği gibi tıklayın.

![Kapsayıcı durumu](./media/containers/containers-status.png)

Log Analytics, kapsayıcılarınızın durumu hakkında bilgi görüntüleyerek açılır.

![Kapsayıcılar için Log Analytics](./media/containers/containers-log-search.png)

Buradan, ilgilendiğiniz belirli bilgileri bulmak için değiştirmeniz arama sorgusu düzenleyebilirsiniz. Günlük sorguları hakkında daha fazla bilgi için bkz. [Azure izleyici 'de günlük sorguları](../log-query/log-query-overview.md).

## <a name="troubleshoot-by-finding-a-failed-container"></a>Başarısız bir kapsayıcı bularak sorunlarını giderme

Log Analytics sıfır olmayan bir çıkış kodu ile çıkış yaptıysanız kapsayıcıyı **başarısız** olarak işaretler. Hatalı **kapsayıcılar** alanındaki ortamdaki hatalara ve hatalara genel bir bakış görebilirsiniz.

### <a name="to-find-failed-containers"></a>Başarısız olan kapsayıcılar bulmak için

1. **Kapsayıcı durumu** alanına tıklayın.  
   ![kapsayıcıları durumu](./media/containers/containers-status.png)
2. Log Analytics açılır ve kapsayıcılarınızın durumunu aşağıdakine benzer şekilde görüntüler.  
   ![kapsayıcı durumu](./media/containers/containers-log-search.png)
3. Başarısız satırı genişletin ve ölçütünü sorguya eklemek için + simgesini tıklatın. Sonra sorgudaki özetleme satırını açıklama satırı yapın.
   başarısız kapsayıcı ![](./media/containers/containers-state-failed-select.png)  
1. Sorguyu çalıştırın ve ardından sonuçlarda bir satırı genişleterek görüntü KIMLIĞINI görüntüleyin.  
   ![başarısız kapsayıcılar](./media/containers/containers-state-failed.png)  
1. Günlük sorgusuna aşağıdakini yazın. görüntü boyutu ve durdurulma ve başarısız görüntü sayısı gibi görüntüyle ilgili ayrıntıları görmek için `ContainerImageInventory | where ImageID == <ImageID>`.  
   başarısız kapsayıcı ![](./media/containers/containers-failed04.png)

## <a name="query-logs-for-container-data"></a>Kapsayıcı verileri için sorgu günlükleri

Belirli bir hata gidermeye çalışıyorsanız, ortamınızda nerede oluştuğunu görmek için yardımcı olabilir. Aşağıdaki günlük türlerini istediğiniz bilgileri döndürmek için sorgular oluşturmanıza yardımcı olur.

- **Containerımageınventory** : görüntüyle düzenlenmiş bilgileri bulmaya ve görüntü kimlikleri veya boyutları gibi görüntü bilgilerini görüntülemeye çalışırken bu türü kullanın.
- **Containerınventory** : kapsayıcı konumu, adlarının ne olduğu ve hangi görüntüleri çalıştırdığı hakkında bilgi edinmek istediğinizde bu türü kullanın.
- **ContainerLog** : belirli hata günlüğü bilgilerini ve girdilerini bulmak istediğinizde bu türü kullanın.
- **ContainerNodeInventory_CL**  Kapsayıcıları bulunan konak/düğüm hakkında bilgi istediğinizde bu türü kullanın. Bu, Docker sürümü, orchestration türünü, depolama ve ağ bilgileri sağlar.
- **ContainerProcess_CL** Kapsayıcıda çalışan işlemi hızlı bir şekilde görmek için bu türü kullanın.
- **ContainerServiceLog** – başlatma, durdurma, silme veya çekme komutları gibi Docker Daemon için denetim izi bilgilerini bulmaya çalışırken bu türü kullanın.
- **KubeEvents_CL**  Kubernetes olaylarını görmek için bu türü kullanın.
- **KubePodInventory_CL**  Küme hiyerarşisi bilgilerini anlamak istediğinizde bu türü kullanın.


### <a name="to-query-logs-for-container-data"></a>Kapsayıcı verileri için günlükleri sorgulamak için

* Bildiğiniz bir görüntü son başarısız oldu ve Hata günlüklerini bulmak için seçin. Bir **Containerınventory** aramayla bu görüntüyü çalıştıran bir kapsayıcı adı bularak başlayın. Örneğin, `ContainerInventory | where Image == "ubuntu" and ContainerState == "Failed"` arayın  
    Ubuntu kapsayıcıları için arama ![](./media/containers/search-ubuntu.png)

  Sonuçlardan herhangi bir satırı genişleterek ilgili kapsayıcının ayrıntılarını görüntüleyin.

## <a name="example-log-queries"></a>Örnek günlük sorguları

Genellikle, bir örnek veya iki ile başlayan ve onları ortamınıza uygun değiştirme sorguları oluşturmak kullanışlıdır. Başlangıç noktası olarak, daha gelişmiş sorgular oluşturmanıza yardımcı olması için **örnek sorgular** alanıyla denemeler yapabilirsiniz.

![Kapsayıcıları sorguları](./media/containers/containers-queries.png)

## <a name="saving-log-queries"></a>Günlük sorguları kaydediliyor

Sorguları kaydetme, Azure Izleyici 'de standart bir özelliktir. Bunları kaydederek bu yararlı buldunuz gerekir gelecekte kullanım için kullanışlı.

Faydalı bulduğunuz bir sorgu oluşturduktan sonra, günlük araması sayfasının en üstündeki **Sık Kullanılanlar** ' a tıklayarak kaydedin. Daha sonra **Pano sayfam** sayfasından daha sonra kolayca erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Ayrıntılı kapsayıcı veri kayıtlarını görüntülemek için [sorgu günlükleri](../log-query/log-query-overview.md) .
