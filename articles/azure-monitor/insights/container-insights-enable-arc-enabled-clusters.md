---
title: Azure Arc etkin Kubernetes kümesini kapsayıcılar için Azure Izleyici ile yapılandırma | Microsoft Docs
description: Bu makalede, Azure Arc etkin Kubernetes kümelerinde kapsayıcılar için Azure Izleyici ile izlemenin nasıl yapılandırılacağı açıklanır.
ms.topic: conceptual
ms.date: 06/23/2020
ms.openlocfilehash: b1adbd5c1f9f0710a814c91872d3f8a9f1afb50f
ms.sourcegitcommit: 635114a0f07a2de310b34720856dd074aaf4f9cd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/23/2020
ms.locfileid: "85261061"
---
# <a name="enable-monitoring-of-azure-arc-enabled-kubernetes-cluster"></a>Azure Arc etkin Kubernetes kümesini izlemeyi etkinleştir

Kapsayıcılar için Azure Izleyici, Azure Kubernetes hizmeti (AKS) ve AKS motoru kümeleri için zengin izleme deneyimi sağlar. Bu makalede, Azure 'un dışında barındırılan Kubernetes kümelerinin, benzer bir izleme deneyimi elde etmek için Azure Arc ile etkinleştirilen izlemenin nasıl etkinleştirileceği açıklanır.

Bir PowerShell veya bash betiği kullanan bir veya daha fazla mevcut Kubernetes dağıtımı için kapsayıcılar için Azure Izleyici etkinleştirilebilir.

## <a name="supported-configurations"></a>Desteklenen yapılandırmalar

Kapsayıcılar için Azure Izleyici, [genel bakış](container-insights-overview.md) makalesinde açıklandığı gibi, Azure Arc 'ın etkinleştirilmiş Kubernetes 'in (Önizleme), aşağıdaki özellikler haricinde izlenmesini destekler:

- Canlı veriler (Önizleme)

- Küme düğümlerinden ve yığınlardan [ölçümler toplayın](container-insights-update-metrics.md) ve bunları Azure izleyici ölçümleri veritabanında depolarsınız

Kapsayıcılar için Azure Izleyici ile resmi olarak şunlar desteklenir:

- Kubernetes ve destek ilkesi sürümleri [desteklenen aks](../../aks/supported-kubernetes-versions.md)sürümleriyle aynıdır.

- Şu kapsayıcı çalışma zamanları desteklenir: Docker, Moby, ve CRı ile uyumlu çalışma zamanları, örneğin CRı-O ve ContainerD.

- Desteklenen ana ve çalışan düğümleri için Linux işletim sistemi sürümü: Ubuntu (18,04 LTS ve 16,04 LTS).

## <a name="prerequisites"></a>Ön koşullar

Başlamadan önce, aşağıdakilere sahip olduğunuzdan emin olun:

- Log Analytics çalışma alanı.

    Kapsayıcılar için Azure Izleyici, [bölgeye göre Azure ürünlerinde](https://azure.microsoft.com/global-infrastructure/services/?regions=all&products=monitor)listelenen bölgelerde bir Log Analytics çalışma alanını destekler. Kendi çalışma alanınızı oluşturmak için [Azure Resource Manager](../platform/template-workspace-configuration.md), [PowerShell](../scripts/powershell-sample-create-workspace.md?toc=%2fpowershell%2fmodule%2ftoc.json)aracılığıyla veya [Azure Portal](../learn/quick-create-workspace.md)aracılığıyla oluşturulabilir.

- Kapsayıcılar için Azure Izleyici 'deki özellikleri etkinleştirmek ve bu özelliklere erişmek için, en azından Azure aboneliğindeki Azure *katkıda* bulunan rolünün bir üyesi olmanız ve kapsayıcılar Için Azure izleyici ile yapılandırılmış Log Analytics çalışma alanının [*Log Analytics katkıda*](../platform/manage-access.md#manage-access-using-azure-permissions) bulunan rolünün bir üyesi olmanız gerekir.

- Azure Arc küme kaynağında [katkıda](../../role-based-access-control/built-in-roles.md#contributor) bulunan rolünün bir üyesidir.

- İzleme verilerini görüntülemek için, kapsayıcılar için Azure Izleyici ile yapılandırılmış Log Analytics çalışma alanına [*Log Analytics okuyucu*](../platform/manage-access.md#manage-access-using-azure-permissions) rolü izninin bir üyesi olursunuz.

- Belirtilen Kubernetes kümesine yönelik kapsayıcılar için Azure Izleyici grafiğini eklemek için [Helm istemcisi](https://helm.sh/docs/using_helm/) .

- Linux için Log Analytics aracısının Kapsayıcılı sürümünün Azure Izleyici ile iletişim kurması için aşağıdaki proxy ve güvenlik duvarı yapılandırma bilgileri gereklidir:

    |Aracı Kaynağı|Bağlantı noktaları |
    |------|---------|
    |`*.ods.opinsights.azure.com` |Bağlantı noktası 443 |
    |`*.oms.opinsights.azure.com` |Bağlantı noktası 443 |
    |`*.dc.services.visualstudio.com` |Bağlantı noktası 443 |

- Kapsayıcılı Aracı, `cAdvisor secure port: 10250` `unsecure port :10255` performans ölçümlerini toplamak için, kümedeki tüm düğümlerde Kubelet 'in ve açık olmasını gerektirir. `secure port: 10250`Zaten yapılandırılmamışsa Kubelet 'In Cadvizörü üzerinde yapılandırmanızı öneririz.

- Kapsayıcılı Aracı, envanter verilerini toplamak için kümedeki Kubernetes API hizmetiyle iletişim kurmak üzere kapsayıcıda aşağıdaki çevresel değişkenlerin belirtilmesini gerektirir- `KUBERNETES_SERVICE_HOST` ve `KUBERNETES_PORT_443_TCP_PORT` .

    >[!IMPORTANT]
    >Yay etkin Kubernetes kümelerini izlemek için desteklenen en düşük aracı sürümü ciprod04162020 veya üzeri bir sürüm.

- PowerShell komut dosyalı metodunu kullanarak izlemeyi etkinleştirirseniz [PowerShell Core](https://docs.microsoft.com/powershell/scripting/install/installing-powershell?view=powershell-6) gereklidir.

- Bash komut dosyası yöntemini kullanarak izlemeyi etkinleştirirseniz [Bash sürüm 4](https://www.gnu.org/software/bash/) gerekir.

## <a name="identify-workspace-resource-id"></a>Çalışma alanı kaynak KIMLIĞINI tanımla

Daha önce indirdiğiniz PowerShell veya bash betiğini kullanarak kümenizin izlenmesini etkinleştirmek ve mevcut bir Log Analytics çalışma alanıyla bütünleştirmek için, Log Analytics çalışma alanınızın tam kaynak KIMLIĞINI belirlemek üzere aşağıdaki adımları gerçekleştirin. `workspaceResourceId`Belirtilen çalışma alanına karşı izleme eklentisini etkinleştirmek için komutunu çalıştırdığınızda parametresi için bu gereklidir. Belirtmek için bir çalışma alanınız yoksa, parametresini de atlayabilirsiniz `workspaceResourceId` ve betiğin sizin için yeni bir çalışma alanı oluşturmasına izin verebilirsiniz.

1. Aşağıdaki komutu kullanarak erişiminiz olan tüm abonelikleri listeleyin:

    ```azurecli
    az account list --all -o table
    ```

    Çıktı aşağıdakine benzeyecektir:

    ```azurecli
    Name                                  CloudName    SubscriptionId                        State    IsDefault
    ------------------------------------  -----------  ------------------------------------  -------  -----------
    Microsoft Azure                       AzureCloud   0fb60ef2-03cc-4290-b595-e71108e8f4ce  Enabled  True
    ```

    **SubscriptionID**değerini kopyalayın.

2. Aşağıdaki komutu kullanarak Log Analytics çalışma alanını barındıran aboneliğe geçin:

    ```azurecli
    az account set -s <subscriptionId of the workspace>
    ```

3. Aşağıdaki örnek, aboneliklerinizdeki çalışma alanlarının listesini varsayılan JSON biçiminde görüntüler.

    ```
    az resource list --resource-type Microsoft.OperationalInsights/workspaces -o json
    ```

    Çıktıda, çalışma alanı adını bulun ve alan **kimliği**altında bu Log Analytics çalışma alanının tam kaynak kimliğini kopyalayın.

## <a name="enable-monitoring-using-powershell"></a>PowerShell 'i kullanarak izlemeyi etkinleştir

1. Aşağıdaki komutları kullanarak betiği, izleme eklentisi ile kümenizi yapılandıran yerel bir klasöre yükleyin ve kaydedin:

    ```powershell
    wget https://aka.ms/enable-monitoring-powershell-script -outfile enable-monitoring.ps1
    ```

2. `$azureArcClusterResourceId`İçin karşılık gelen değerleri ayarlayarak `subscriptionId` `resourceGroupName` ve `clusterName` Azure Arc özellikli Kubernetes küme kaynağınızın kaynak kimliğini temsil ederek değişkeni yapılandırın.

    ```powershell
    $azureArcClusterResourceId = "/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. `$kubeContext`Komutunu çalıştırarak değişkeni, kümenizin **kuin bağlamı** ile yapılandırın `kubectl config get-contexts` . Geçerli bağlamı kullanmak istiyorsanız, değerini olarak ayarlayın `""` .

    ```powershell
    $kubeContext = "<kubeContext name of your k8s cluster>"
    ```

4. Mevcut Azure Izleyici Log Analytics çalışma alanını kullanmak istiyorsanız, değişkeni, `$logAnalyticsWorkspaceResourceId` çalışma alanının kaynak kimliğini temsil eden karşılık gelen değerle yapılandırın. Aksi takdirde, değişkenini olarak ayarlayın `""` ve komut dosyası bölgede yoksa, küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturur. Oluşturulan varsayılan çalışma alanı, *defaultworkspace- \<SubscriptionID> - \<Region> *biçimine benzer.

    ```powershell
    $logAnalyticsWorkspaceResourceId = “/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/microsoft.operationalinsights/workspaces/<workspaceName>”
    ```

5. Arc etkin Kubernetes kümeniz bir ara sunucu üzerinden iletişim kuruyorsa, değişkeni `$proxyEndpoint` ara sunucunun URL 'siyle yapılandırın. Küme bir ara sunucu üzerinden iletişim kurmadığından, değerini olarak ayarlayabilirsiniz `""` .  Daha fazla bilgi için bu makalenin ilerleyen kısımlarında [proxy uç noktasını yapılandırma](#configure-proxy-endpoint) konusuna bakın.

6. İzlemeyi etkinleştirmek için aşağıdaki komutu çalıştırın.

    ```powershell
    .\enable-monitoring.ps1 -clusterResourceId $azureArcClusterResourceId -kubeContext $kubeContext -workspaceResourceId $logAnalyticsWorkspaceResourceId -proxyEndpoint $proxyEndpoint
    ```

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

## <a name="enable-using-bash-script"></a>Bash betiğini kullanmayı etkinleştir

Belirtilen Bash betiğini kullanarak izlemeyi etkinleştirmek için aşağıdaki adımları gerçekleştirin.

1. Aşağıdaki komutları kullanarak betiği, izleme eklentisi ile kümenizi yapılandıran yerel bir klasöre yükleyin ve kaydedin:

    ```bash
    curl -o enable-monitoring.sh -L https://aka.ms/enable-monitoring-bash-script
    ```

2. `azureArcClusterResourceId`İçin karşılık gelen değerleri ayarlayarak `subscriptionId` `resourceGroupName` ve `clusterName` Azure Arc özellikli Kubernetes küme kaynağınızın kaynak kimliğini temsil ederek değişkeni yapılandırın.

    ```bash
    export azureArcClusterResourceId="/subscriptions/<subscriptionId>/resourceGroups/<resourceGroupName>/providers/Microsoft.Kubernetes/connectedClusters/<clusterName>"
    ```

3. `kubeContext`Komutunu çalıştırarak değişkeni, kümenizin **kuin bağlamı** ile yapılandırın `kubectl config get-contexts` . Geçerli bağlamı kullanmak istiyorsanız, değerini olarak ayarlayın `""` .

    ```bash
    export kubeContext="<kubeContext name of your k8s cluster>"
    ```

4. Mevcut Azure Izleyici Log Analytics çalışma alanını kullanmak istiyorsanız, değişkeni, `logAnalyticsWorkspaceResourceId` çalışma alanının kaynak kimliğini temsil eden karşılık gelen değerle yapılandırın. Aksi takdirde, değişkenini olarak ayarlayın `""` ve komut dosyası bölgede yoksa, küme aboneliğinin varsayılan kaynak grubunda varsayılan bir çalışma alanı oluşturur. Oluşturulan varsayılan çalışma alanı, *defaultworkspace- \<SubscriptionID> - \<Region> *biçimine benzer.

    ```bash
    export logAnalyticsWorkspaceResourceId=“/subscriptions/<subscriptionId>/resourceGroups/<resourceGroup>/providers/microsoft.operationalinsights/workspaces/<workspaceName>”
    ```

5. Arc etkin Kubernetes kümeniz bir ara sunucu üzerinden iletişim kuruyorsa, değişkeni `proxyEndpoint` ara sunucunun URL 'siyle yapılandırın. Küme bir ara sunucu üzerinden iletişim kurmadığından, değerini olarak ayarlayabilirsiniz `""` . Daha fazla bilgi için bu makalenin ilerleyen kısımlarında [proxy uç noktasını yapılandırma](#configure-proxy-endpoint) konusuna bakın.

6. Kümenizde izlemeyi etkinleştirmek için, dağıtım senaryonuz temelinde sunulan farklı komutlar vardır.

    Geçerli kuas-Context kullanma, varsayılan bir Log Analytics çalışma alanı oluşturma ve bir ara sunucu belirtmeden, varsayılan seçeneklerle izlemeyi etkinleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    bash enable-monitoring.sh --resource-id $azureArcClusterResourceId
    ```

    Varsayılan bir Log Analytics çalışma alanı oluşturmak ve proxy sunucusu belirtmeden aşağıdaki komutu çalıştırın:

    ```bash
   bash enable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext
    ```

    Mevcut bir Log Analytics çalışma alanını ve bir ara sunucu belirtmeden kullanmak için aşağıdaki komutu çalıştırın:

    ```bash
    bash enable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext  --workspace-id $logAnalyticsWorkspaceResourceId
    ```

    Mevcut bir Log Analytics çalışma alanını kullanmak ve bir proxy sunucusu belirtmek için aşağıdaki komutu çalıştırın:

    ```bash
    bash enable-monitoring.sh --resource-id $azureArcClusterResourceId --kube-context $kubeContext  --workspace-id $logAnalyticsWorkspaceResourceId --proxy $proxyEndpoint
    ```

İzlemeyi etkinleştirdikten sonra, küme için sistem durumu ölçümlerini görüntüleyebilmeniz yaklaşık 15 dakika sürebilir.

## <a name="configure-proxy-endpoint"></a>Proxy uç noktasını yapılandırma

Kapsayıcılar için Azure Izleyici Kapsayıcılı aracı ile proxy sunucusu üzerinden iletişim kurmasına izin vermek için bir proxy uç noktası yapılandırabilirsiniz. Kapsayıcılı aracı ve Azure Izleyici arasındaki iletişim bir HTTP veya HTTPS proxy sunucusu olabilir ve hem anonim hem de temel kimlik doğrulaması (Kullanıcı adı/parola) desteklenir.

Ara sunucu yapılandırma değeri aşağıdaki sözdizimine sahiptir:`[protocol://][user:password@]proxyhost[:port]`

> [!NOTE]
>Proxy sunucunuz kimlik doğrulaması gerektirmiyorsa, yine de bir psuedo Kullanıcı adı/parolası belirtmeniz gerekir. Bu, herhangi bir Kullanıcı adı veya parola olabilir.

|Özellik| Açıklama |
|--------|-------------|
|Protokol | http veya https |
|kullanıcı | Proxy kimlik doğrulaması için isteğe bağlı Kullanıcı adı |
|password | Proxy kimlik doğrulaması için isteğe bağlı parola |
|proxyhost | Proxy sunucusunun adresi veya FQDN 'si |
|port | Proxy sunucusu için isteğe bağlı bağlantı noktası numarası |

Örneğin, `http://user01:password@proxy01.contoso.com:3128`

Protokolü **http**olarak BELIRTIRSENIZ, http istekleri SSL/TLS güvenli bağlantı kullanılarak oluşturulur. Ara sunucunuzun SSL/TLS protokollerini desteklemesi gerekir.

### <a name="configure-using-powershell"></a>PowerShell’i kullanarak yapılandırma

Proxy sunucusu için Kullanıcı adı ve parola, IP adresi veya FQDN ve bağlantı noktası numarasını belirtin. Örneğin:

```powershell
$proxyEndpoint = https://<user>:<password>@<proxyhost>:<port>
```

### <a name="configure-using-bash"></a>Bash kullanarak yapılandırma

Proxy sunucusu için Kullanıcı adı ve parola, IP adresi veya FQDN ve bağlantı noktası numarasını belirtin. Örneğin:

```bash
export proxyEndpoint=https://<user>:<password>@<proxyhost>:<port>
```

## <a name="next-steps"></a>Sonraki adımlar

- İzleme etkinken, Arc etkin Kubernetes kümeniz ve üzerinde çalışan iş yüklerinizin sistem durumunu ve kaynak kullanımını toplamaya yönelik olarak, kapsayıcılar için Azure Izleyicisini [nasıl](container-insights-analyze.md) kullanacağınızı öğrenin.

- Varsayılan olarak, Kapsayıcılı Aracı, Kuto-System hariç tüm ad alanlarında çalışan tüm kapsayıcıların stdout/stderr kapsayıcı günlüklerini toplar. Belirli bir ad alanı veya ad alanına özgü kapsayıcı günlüğü koleksiyonunu yapılandırmak için, istenen veri koleksiyonu ayarlarını ConfigMap yapılandırmaları dosyanıza yapılandırmak üzere [kapsayıcı öngörüleri aracı yapılandırmasını](container-insights-agent-config.md) gözden geçirin.

- Kümelediğiniz Prometheus ölçümlerini hurdaya almak ve analiz etmek için bkz. [Prometheus ölçümleri koruması](container-insights-prometheus-integration.md) 'nı inceleyin

- Kapsayıcılar için Azure Izleyici ile yay etkin Kubernetes kümenizi izlemeyi durdurmayı öğrenmek için bkz. [karma kümenizi izlemeyi durdurma](container-insights-optout-hybrid.md#how-to-stop-monitoring-on-arc-enabled-kubernetes).
