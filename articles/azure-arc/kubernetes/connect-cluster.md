---
title: Azure Arc etkin bir Kubernetes kümesine bağlanma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 02/17/2021
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure Arc etkin bir Kubernetes kümesini Azure Arc ile bağlama
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.custom: references_regions, devx-track-azurecli
ms.openlocfilehash: 876bc15a3f4db1d12afec37c69656c431e5e6773
ms.sourcegitcommit: 227b9a1c120cd01f7a39479f20f883e75d86f062
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/18/2021
ms.locfileid: "100652522"
---
# <a name="connect-an-azure-arc-enabled-kubernetes-cluster-preview"></a>Azure Arc etkin bir Kubernetes kümesine bağlanma (Önizleme)

Bu makalede, var olan bir Kubernetes kümesini Azure yaya bağlama işlemi anlatılmaktadır. Kümeleri bağlama konusunda kavramsal bir alma için bkz. [Azure Arc etkinleştirilmiş Kubernetes aracı mimarisi makalesi](./conceptual-agent-architecture.md).

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki önkoşulları hazırladığınızdan emin olun:

* Çalışır duruma bir Kubernetes kümesi. Mevcut bir Kubernetes kümeniz yoksa, bir test kümesi oluşturmak için aşağıdaki kılavuzlardan birini kullanabilirsiniz:
  * [Docker 'Da Kubernetes](https://kind.sigs.k8s.io/)kullanarak bir Kubernetes kümesi oluşturun (tür).
  * [Mac](https://docs.docker.com/docker-for-mac/#kubernetes) veya [Windows](https://docs.docker.com/docker-for-windows/#kubernetes)için Docker kullanarak bir Kubernetes kümesi oluşturun.
* Yay etkinleştirilmiş Kubernetes aracılarının dağıtımı için kümedeki kümeye ve Küme Yöneticisi rolüne erişmek için bir kubeconfig dosyası.
* İle kullanılan Kullanıcı veya hizmet sorumlusu için ' Read ' ve ' Write ' izinleri `az login` ve `az connectedk8s connect` kaynak türü üzerinde komutlar `Microsoft.Kubernetes/connectedclusters` . "Kubernetes kümesi-Azure yay ekleme" rolü bu izinlere sahiptir ve Kullanıcı veya hizmet sorumlusu üzerinde rol atamaları için kullanılabilir.
* Uzantıyı kullanarak küme ekleme için helb 3 `connectedk8s` . Bu gereksinimi karşılamak için [Held 3 ' ün en son sürümünü yükler](https://helm.sh/docs/intro/install) .
* Azure Arc 'ın etkin Kubernetes CLı uzantılarını yüklemek için Azure CLı sürüm 2.15 +. [Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest&preserve-view=true) 'yı veya güncelleştirme 'yi en son sürüme güncelleştirin.
* Azure Arc etkin Kubernetes CLı uzantıları:
  
  * `connectedk8s`Kubernetes kümelerini Azure 'a bağlamanıza yardımcı olması için uzantıyı yükleyebilirsiniz:
  
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
| `https://eus.his.arc.azure.com`, `https://weu.his.arc.azure.com`                                                                            |  Sistem tarafından atanan Yönetilen Hizmet Kimliği (MSI) sertifikalarını çekmek için gereklidir.                                                                  |

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes için iki sağlayıcıyı kaydetme

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
   * `KUBECONFIG`
   * `~/.kube/config`
   * `--kube-config`
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
  "agentPublicKeyCertificate": "xxxxxxxxxxxxxxxxxxx",
  "agentVersion": null,
  "connectivityStatus": "Connecting",
  "distribution": "gke",
  "id": "/subscriptions/xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx/resourceGroups/AzureArcTest/providers/Microsoft.Kubernetes/connectedClusters/AzureArcTest1",
  "identity": {
    "principalId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "tenantId": "xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx",
    "type": "SystemAssigned"
  },
  "infrastructure": "gcp",
  "kubernetesVersion": null,
  "lastConnectivityTime": null,
  "location": "eastus",
  "managedIdentityCertificateExpirationTime": null,
  "name": "AzureArcTest1",
  "offering": null,
  "provisioningState": "Succeeded",
  "resourceGroup": "AzureArcTest",
  "tags": {},
  "totalCoreCount": null,
  "totalNodeCount": null,
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

Ayrıca, bu kaynağı [Azure Portal](https://portal.azure.com/)görüntüleyebilirsiniz.
1. Portalı tarayıcınızda açın.
1. Komutta daha önce kullanılan kaynak adı ve kaynak grubu adı girdilerine göre kaynak grubuna ve Azure Arc etkin Kubernetes kaynağına gidin `az connectedk8s connect` .  
> [!NOTE]
> Küme eklendikten sonra, küme meta verileri (küme sürümü, aracı sürümü, düğüm sayısı vb.) için, Azure portal 'de Azure Arc etkin Kubernetes kaynağının genel bakış sayfasında yüzey olarak 5 ila 10 dakika sürer.

## <a name="connect-using-an-outbound-proxy-server"></a>Giden proxy sunucusu kullanarak bağlanma

Kümeniz giden bir proxy sunucusunun arkasında ise, Azure CLı ve Azure Arc etkin Kubernetes aracılarının isteklerini giden ara sunucu aracılığıyla yönlendirmesi gerekir:

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
> * Altında belirtme, `excludedCIDR` `--proxy-skip-range` aracılar için küme içi iletişimin kesilmemesini sağlamak açısından önemlidir.
> * , `--proxy-http` `--proxy-https` Ve `--proxy-skip-range` çoğu giden proxy ortamları için beklenirken, `--proxy-cert` yalnızca Proxy 'den gelen güvenilen sertifikaların, aracı pods 'nin güvenilen sertifika deposuna eklenmesi gerekiyorsa gereklidir.
> * Yukarıdaki proxy belirtimi Şu anda yalnızca Azure Arc aracıları için geçerlidir ve içinde kullanılan Flox pod için değildir `sourceControlConfiguration` . Azure Arc etkin Kubernetes ekibi bu özellik üzerinde etkin bir şekilde çalışıyor.

## <a name="azure-arc-agents-for-kubernetes"></a>Kubernetes için Azure Arc aracıları

Azure Arc etkin Kubernetes, ad alanına birkaç işleç dağıtır `azure-arc` . Bu dağıtımları ve diğer 'leri kullanarak şu şekilde görüntüleyebilirsiniz:

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

Tüm yığınların bir durumda olduğunu doğrulayın `Running` .

## <a name="delete-a-connected-cluster"></a>Bağlı kümeyi silme

`Microsoft.Kubernetes/connectedcluster`Azure CLI veya Azure Portal kullanarak bir kaynağı silebilirsiniz.


* **Azure CLI kullanarak silme**: Azure Arc etkin Kubernetes kaynağını silme işlemini başlatmak Için AŞAĞıDAKI Azure CLI komutunu kullanın.
  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```
  Bu komut, `Microsoft.Kubernetes/connectedCluster` Azure 'daki kaynağı ve ilişkili `sourcecontrolconfiguration` kaynakları kaldırır. Azure CLı, `helm uninstall` küme üzerinde çalışan aracıları da kaldırmak için kullanır.

* **Azure Portal silme**: Azure Arc etkin Kubernetes kaynağını Azure Portal üzerinde silme Işlemi, `Microsoft.Kubernetes/connectedcluster` Azure 'daki kaynağı ve ilişkili kaynakları siler `sourcecontrolconfiguration` , ancak kümede çalışan aracıları kaldırmaz  . 

  Kümede çalışan aracıları kaldırmak için aşağıdaki komutu çalıştırın:

  ```console
  az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
  ```

## <a name="next-steps"></a>Sonraki adımlar

* [Bağlı kümede GitOps kullanma](./use-gitops-connected-cluster.md)
* [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
