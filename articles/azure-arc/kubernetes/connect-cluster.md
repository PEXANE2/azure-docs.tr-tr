---
title: Azure Arc özellikli Kubernetes kümesini (Önizleme) bağlama
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure Arc ile Azure Arc etkin bir Kubernetes kümesi bağlama
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: b4ab84153eaaf81c668d8589fec7516853aca5f9
ms.sourcegitcommit: 49ea056bbb5957b5443f035d28c1d8f84f5a407b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/09/2021
ms.locfileid: "100008120"
---
# <a name="connect-an-azure-arc-enabled-kubernetes-cluster-preview"></a>Azure Arc özellikli Kubernetes kümesini (Önizleme) bağlama

Bu makalede, Azure 'daki AKS-Engine, Azure Stack hub 'daki AKS-Engine, GKE, EKS ve VMware vSphere Cluster for Azure Arc gibi herhangi bir bulut Yerel Bilgi Işlem altyapısı (CNCF) sertifikalı Kubernetes kümesi bağlama işlemi ele alınmaktadır.

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki önkoşulları hazırladığınızdan emin olun:

* Çalışır duruma bir Kubernetes kümesi. Mevcut bir Kubernetes kümeniz yoksa, bir test kümesi oluşturmak için aşağıdaki kılavuzlardan birini kullanabilirsiniz:
  * [Docker 'Da Kubernetes](https://kind.sigs.k8s.io/)kullanarak bir Kubernetes kümesi oluşturun (tür).
  * [Mac](https://docs.docker.com/docker-for-mac/#kubernetes) veya [Windows](https://docs.docker.com/docker-for-windows/#kubernetes)için Docker kullanarak bir Kubernetes kümesi oluşturun.
* Yay özellikli Kubernetes aracılarının dağıtımı için kümedeki kümeye ve Küme Yöneticisi rolüne erişmek için bir kubeconfig dosyası.
* Ve komutlarıyla kullanılan Kullanıcı veya hizmet sorumlusu `az login` `az connectedk8s connect` ' Microsoft. Kubernetes/connectedkümeler ' kaynak türü üzerinde ' Read ' ve ' Write ' izinlerine sahip olmalıdır. "Kubernetes kümesi-Azure yay ekleme" rolü bu izinlere sahiptir ve Kullanıcı veya hizmet sorumlusu üzerinde rol atamaları için kullanılabilir.
* Connectedk8s uzantısını kullanarak kümeyi ekleme için helb 3. Bu gereksinimi karşılamak için [Held 3 ' ün en son sürümünü yükler](https://helm.sh/docs/intro/install) .
* Azure Arc-etkinleştirilmiş Kubernetes CLı uzantılarını yüklemek için Azure CLı sürüm 2.15 +. [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) 'yı veya güncelleştirme 'yi en son sürüme güncelleştirin.
* Arc etkin Kubernetes CLı uzantılarını yükler:
  
  * `connectedk8s`Kubernetes kümelerini Azure 'a bağlamanıza yardımcı olan uzantıyı yükleyebilirsiniz:
  
  ```azurecli
  az extension add --name connectedk8s
  ```
  
   * Uzantıyı yükler `k8sconfiguration` :
  
  ```azurecli
  az extension add --name k8sconfiguration
  ```

  * Bu uzantıları daha sonra güncelleştirmek istiyorsanız aşağıdaki komutları çalıştırın:
  
  ```azurecli
  az extension update --name connectedk8s
  az extension update --name k8sconfiguration
  ```

## <a name="supported-regions"></a>Desteklenen bölgeler

* Doğu ABD
* West Europe

## <a name="network-requirements"></a>Ağ gereksinimleri

Azure Arc aracıları, aşağıdaki protokollerin/bağlantı noktalarının/giden URL 'Lerin çalışmasını gerektirir:

* 443 numaralı bağlantı noktasında TCP: `https://:443`
* 9418 numaralı bağlantı noktasında TCP: `git://:9418`

| Uç nokta (DNS)                                                                                               | Description                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `https://management.azure.com`                                                                                 | Aracının Azure 'a bağlanması ve kümeyi kaydetmesi için gereklidir.                                                        |
| `https://eastus.dp.kubernetesconfiguration.azure.com`, `https://westeurope.dp.kubernetesconfiguration.azure.com` | Aracının durumu iletme ve yapılandırma bilgilerini getirme işlemi için veri düzlemi uç noktası.                                      |
| `https://login.microsoftonline.com`                                                                            | Azure Resource Manager belirteçleri getirmek ve güncelleştirmek için gereklidir.                                                                                    |
| `https://mcr.microsoft.com`                                                                            | Azure Arc aracıları için kapsayıcı görüntülerini çekmek için gereklidir.                                                                  |
| `https://eus.his.arc.azure.com`, `https://weu.his.arc.azure.com`                                                                            |  Sistem tarafından atanan yönetilen kimlik sertifikalarını çekmek için gereklidir.                                                                  |

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Azure Arc özellikli Kubernetes için iki sağlayıcıyı kaydedin:

```console
az provider register --namespace Microsoft.Kubernetes

az provider register --namespace Microsoft.KubernetesConfiguration
```

Kayıt zaman uyumsuz bir işlemdir ve yaklaşık 10 dakika sürebilir. Aşağıdaki komutlarla kayıt işlemini izleyebilirsiniz:

```console
az provider show -n Microsoft.Kubernetes -o table
```

```console
az provider show -n Microsoft.KubernetesConfiguration -o table
```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kümenizin meta verilerini depolamak için bir kaynak grubu kullanın.

İlk olarak, bağlı küme kaynağını barındıracak bir kaynak grubu oluşturun.

```console
az group create --name AzureArcTest -l EastUS -o table
```

**Çıktıların**

```console
Location    Name
----------  ------------
eastus      AzureArcTest
```

## <a name="connect-a-cluster"></a>Bir kümeyi bağlama

Sonra, Kubernetes kümemizi kullanarak Azure 'a bağlayacağız `az connectedk8s connect` :

1. Aşağıdakilerden birini kullanarak Kubernetes kümenize bağlantıyı doğrulayın:
   1. `KUBECONFIG`
   1. `~/.kube/config`
   1. `--kube-config`
1. Held 3 kullanarak Kubernetes için Azure Arc aracılarını `azure-arc` ad alanına dağıtın:

```console
az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest
```

**Çıktıların**

```console
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Helm release deployment succeeded

{
  "aadProfile": {
    "clientAppId": "",
    "serverAppId": "",
    "tenantId": ""
  },
  "agentPublicKeyCertificate": "...",
  "agentVersion": "0.1.0",
  "id": "/subscriptions/57ac26cf-a9f0-4908-b300-9a4e9a0fb205/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1",
  "identity": {
    "principalId": null,
    "tenantId": null,
    "type": "None"
  },
  "kubernetesVersion": "v1.15.0",
  "location": "eastus",
  "name": "AzureArcTest1",
  "resourceGroup": "AzureArcTest",
  "tags": {},
  "totalNodeCount": 1,
  "type": "Microsoft.Kubernetes/connectedClusters"
}
```

## <a name="verify-connected-cluster"></a>Bağlı Kümeyi Doğrula

Bağlı kümelerinizi listelemek için aşağıdaki komutu kullanın:

```console
az connectedk8s list -g AzureArcTest -o table
```

**Çıktıların**

```console
Command group 'connectedk8s' is in preview. It may be changed/removed in a future release.
Name           Location    ResourceGroup
-------------  ----------  ---------------
AzureArcTest1  eastus      AzureArcTest
```

Ayrıca, bu kaynağı [Azure Portal](https://portal.azure.com/)görüntüleyebilirsiniz. Portalı tarayıcınızda açın ve komutta daha önce kullanılan kaynak adı ve kaynak grubu adı girdilerine göre kaynak grubuna ve Azure Arc etkin Kubernetes kaynağına gidin `az connectedk8s connect` .

> [!NOTE]
> Küme eklendikten sonra, küme meta verileri (küme sürümü, aracı sürümü, düğüm sayısı vb.) için, Azure portal içinde Azure Arc özellikli Kubernetes kaynağının genel bakış sayfasında yüzey olarak 5 ila 10 dakika sürer.

## <a name="connect-using-an-outbound-proxy-server"></a>Giden proxy sunucusu kullanarak bağlanma

Kümeniz giden bir proxy sunucusunun arkasındaysa, Azure CLı ve yay özellikli Kubernetes aracılarının isteklerini giden ara sunucu aracılığıyla yönlendirmesi gerekir:

1. `connectedk8s`Makinenizde yüklü olan uzantının sürümünü denetleyin:

    ```console
    az -v
    ```

    `connectedk8s`Giden ara sunucu ile aracıları ayarlamak için uzantı sürümü 0.2.5 + olmalıdır. Makinenizde sürüm 0.2.3 veya daha eski bir sürümü varsa, en son uzantı sürümünü makinenizde almak için [güncelleştirme adımlarını](#before-you-begin) izleyin.

2. Azure CLı için gereken ortam değişkenlerini giden proxy sunucusunu kullanacak şekilde ayarlayın:

    * Bash kullanıyorsanız, aşağıdaki komutu uygun değerlerle çalıştırın:

        ```bash
        export HTTP_PROXY=<proxy-server-ip-address>:<port>
        export HTTPS_PROXY=<proxy-server-ip-address>:<port>
        export NO_PROXY=<cluster-apiserver-ip-address>:<port>
        ```

    * PowerShell kullanıyorsanız, aşağıdaki komutu uygun değerlerle çalıştırın:

        ```powershell
        $Env:HTTP_PROXY = "<proxy-server-ip-address>:<port>"
        $Env:HTTPS_PROXY = "<proxy-server-ip-address>:<port>"
        $Env:NO_PROXY = "<cluster-apiserver-ip-address>:<port>"
        ```

3. Connect komutunu belirtilen proxy parametreleriyle Çalıştır:

    ```console
    az connectedk8s connect -n <cluster-name> -g <resource-group> --proxy-https https://<proxy-server-ip-address>:<port> --proxy-http http://<proxy-server-ip-address>:<port> --proxy-skip-range <excludedIP>,<excludedCIDR> --proxy-cert <path-to-cert-file>
    ```

> [!NOTE]
> 1. Altında belirtme, `excludedCIDR` `--proxy-skip-range` aracılar için küme içi iletişimin kesilmemesini sağlamak açısından önemlidir.
> 2. , `--proxy-http` `--proxy-https` Ve `--proxy-skip-range` çoğu giden proxy ortamları için beklenirken, `--proxy-cert` yalnızca Proxy 'den gelen güvenilen sertifikaların, aracı pods 'nin güvenilen sertifika deposuna eklenmesi gerekiyorsa gereklidir.
> 3. Yukarıdaki proxy belirtimi Şu anda yalnızca Arc aracıları için uygulanabilir ve sourcecontrolconfiguration içinde kullanılan Flox pod için geçerli değildir. Yay etkin Kubernetes ekibi bu özellik üzerinde etkin bir şekilde çalışıyor ve yakında kullanıma sunulacaktır.

## <a name="azure-arc-agents-for-kubernetes"></a>Kubernetes için Azure Arc aracıları

Azure Arc-etkinleştirilmiş Kubernetes, ad alanına birkaç işleç dağıtır `azure-arc` . Bu dağıtımları ve diğer 'leri kullanarak şu şekilde görüntüleyebilirsiniz:

```console
kubectl -n azure-arc get deployments,pods
```

**Çıktıların**

```console
NAME                                        READY      UP-TO-DATE  AVAILABLE  AGE
deployment.apps/cluster-metadata-operator     1/1             1        1      16h
deployment.apps/clusteridentityoperator       1/1             1        1      16h
deployment.apps/config-agent                  1/1             1        1      16h
deployment.apps/controller-manager            1/1             1        1      16h
deployment.apps/flux-logs-agent               1/1             1        1      16h
deployment.apps/metrics-agent                 1/1             1        1      16h
deployment.apps/resource-sync-agent           1/1             1        1      16h

NAME                                           READY    STATUS   RESTART AGE
pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
```

Azure Arc etkin Kubernetes, kümenizde çalışan ve ad alanına dağıtılan birkaç aracıdan (işleçlerden) oluşur `azure-arc` .

| Aracılar (Işleçler)                                                                                               | Description                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `deployment.apps/config-agent`                                                                                 | Kümede ve güncelleştirme uyumluluk durumunda uygulanan kaynak denetimi yapılandırma kaynakları için bağlı kümeyi izler.                                                        |
| `deployment.apps/controller-manager` | Azure Arc bileşenleri arasındaki etkileşimleri düzenleyen bir işleç operatörü.                                      |
| `deployment.apps/metrics-agent`                                                                            | Diğer yay aracılarının performans ölçümlerini toplar.                                                                                    |
| `deployment.apps/cluster-metadata-operator`                                                                            | Küme sürümü, düğüm sayısı ve Azure Arc aracı sürümü gibi küme meta verilerini toplar.                                                                  |
| `deployment.apps/resource-sync-agent`                                                                            |  Yukarıdaki belirtilen küme meta verilerini Azure 'a eşitler.                                                                  |
| `deployment.apps/clusteridentityoperator`                                                                            |  Azure Arc etkin Kubernetes Şu anda sistem tarafından atanan kimliği desteklemektedir. `clusteridentityoperator` diğer aracıların Azure ile iletişim kurmak için kullandığı yönetilen hizmet kimliği (MSI) sertifikasını korur.                                                                  |
| `deployment.apps/flux-logs-agent`                                                                            |  Kaynak denetimi yapılandırmasının bir parçası olarak dağıtılan Flox işleçlerinden günlükleri toplar.                                                                  |

## <a name="delete-a-connected-cluster"></a>Bağlı kümeyi silme

`Microsoft.Kubernetes/connectedcluster`Azure CLI veya Azure Portal kullanarak bir kaynağı silebilirsiniz.


* **Azure CLI kullanarak silme**: Azure Arc özellikli Kubernetes kaynağını silme işlemini başlatmak Için AŞAĞıDAKI Azure CLI komutunu kullanın.
  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```
  Bu komut, `Microsoft.Kubernetes/connectedCluster` Azure 'daki kaynağı ve ilişkili `sourcecontrolconfiguration` kaynakları kaldırır. Azure CLı, `helm uninstall` küme üzerinde çalışan aracıları da kaldırmak için kullanır.

* **Azure Portal silme**: Azure Arc etkin Kubernetes kaynağını Azure Portal üzerinde silme Işlemi, `Microsoft.Kubernetes/connectedcluster` Azure 'daki kaynağı ve ilişkili `sourcecontrolconfiguration` kaynakları siler, ancak kümede çalışan aracıları kaldırmaz.  

  Kümede çalışan aracıları kaldırmak için aşağıdaki komutu çalıştırın:

  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Bağlı kümede GitOps kullanma](./use-gitops-connected-cluster.md)
* [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
