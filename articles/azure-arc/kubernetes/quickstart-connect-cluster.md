---
title: 'Hızlı başlangıç: mevcut bir Kubernetes kümesini Azure yaya bağlama'
description: Bu hızlı başlangıçta, Azure Arc etkin bir Kubernetes kümesini bağlamayı öğrenin.
author: mgoedtel
ms.author: magoedte
ms.service: azure-arc
ms.topic: quickstart
ms.date: 03/03/2021
ms.custom: template-quickstart
keywords: Kubernetes, yay, Azure, küme
ms.openlocfilehash: 3fc522c4bdda9eb1047d5258bcc431d0268990b9
ms.sourcegitcommit: dac05f662ac353c1c7c5294399fca2a99b4f89c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/04/2021
ms.locfileid: "102121652"
---
# <a name="quickstart-connect-an-existing-kubernetes-cluster-to-azure-arc"></a>Hızlı başlangıç: mevcut bir Kubernetes kümesini Azure yaya bağlama 

Bu hızlı başlangıçta, Azure Arc 'ın, Kubernetes 'in avantajlarından yararlanmaya ve mevcut bir Kubernetes kümesini Azure yaya bağlamaya başlayacağız. Kümeleri Azure yaya bağlama hakkında kavramsal bir alma için bkz. [Azure Arc etkin Kubernetes aracı mimarisi makalesi](./conceptual-agent-architecture.md).

[!INCLUDE [quickstarts-free-trial-note](../../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [azure-cli-prepare-your-environment.md](../../../includes/azure-cli-prepare-your-environment.md)]

* Şunları doğrulayın:
    * Çalışır duruma bir Kubernetes kümesi.
    * `kubeconfig`Azure yaya bağlamak istediğiniz kümeye işaret eden bir dosya.
    * Kullanıcı veya hizmet sorumlusu için ' Read ' ve ' Write ' izinleri, Azure Arc etkin Kubernetes kaynak türü ( `Microsoft.Kubernetes/connectedClusters` ) oluşturuluyor.
* [Held 3 ' ün en son sürümünü](https://helm.sh/docs/intro/install)yükler.
* Aşağıdaki Azure Arc etkin Kubernetes CLı uzantılarını yükler >= 1.0.0:
  
  ```azurecli
  az extension add --name connectedk8s
  az extension add --name k8s-configuration
  ```
  * Bu uzantıları en son sürüme güncelleştirmek için aşağıdaki komutları çalıştırın:
  
  ```azurecli
  az extension update --name connectedk8s
  az extension update --name k8s-configuration
  ```

>[!NOTE]
>**Desteklenen bölgeler:**
>* Doğu ABD
>* West Europe
>* Orta Batı ABD
>* Orta Güney ABD
>* Güneydoğu Asya
>* Güney Birleşik Krallık
>* Batı ABD 2
>* Doğu Avustralya
>* Doğu ABD 2
>* Kuzey Avrupa

## <a name="meet-network-requirements"></a>Ağ gereksinimlerini karşılayın

>[!IMPORTANT]
>Azure Arc aracıları, aşağıdaki protokollerin/bağlantı noktalarının/giden URL 'Lerin çalışmasını gerektirir:
>* 443 numaralı bağlantı noktasında TCP: `https://:443`
>* 9418 numaralı bağlantı noktasında TCP: `git://:9418`
  
| Uç nokta (DNS) | Açıklama |  
| ----------------- | ------------- |  
| `https://management.azure.com`                                                                                 | Aracının Azure 'a bağlanması ve kümeyi kaydetmesi için gereklidir.                                                        |  
| `https://eastus.dp.kubernetesconfiguration.azure.com`, `https://westeurope.dp.kubernetesconfiguration.azure.com`, `https://westcentralus.dp.kubernetesconfiguration.azure.com`, `https://southcentralus.dp.kubernetesconfiguration.azure.com`, `https://southeastasia.dp.kubernetesconfiguration.azure.com`, `https://uksouth.dp.kubernetesconfiguration.azure.com`, `https://westus2.dp.kubernetesconfiguration.azure.com`, `https://australiaeast.dp.kubernetesconfiguration.azure.com`, `https://eastus2.dp.kubernetesconfiguration.azure.com`, `https://northeurope.dp.kubernetesconfiguration.azure.com` | Aracının durumu iletme ve yapılandırma bilgilerini getirme işlemi için veri düzlemi uç noktası.                                      |  
| `https://login.microsoftonline.com`                                                                            | Azure Resource Manager belirteçleri getirmek ve güncelleştirmek için gereklidir.                                                                                    |  
| `https://mcr.microsoft.com`                                                                            | Azure Arc aracıları için kapsayıcı görüntülerini çekmek için gereklidir.                                                                  |  
| `https://eus.his.arc.azure.com`, `https://weu.his.arc.azure.com`, `https://wcus.his.arc.azure.com`, `https://scus.his.arc.azure.com`, `https://sea.his.arc.azure.com`, `https://uks.his.arc.azure.com`, `https://wus2.his.arc.azure.com`, `https://ae.his.arc.azure.com`, `https://eus2.his.arc.azure.com`, `https://ne.his.arc.azure.com` |  Sistem tarafından atanan Yönetilen Hizmet Kimliği (MSI) sertifikalarını çekmek için gereklidir.                                                                  |

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes için iki sağlayıcıyı kaydetme

1. Aşağıdaki komutları girin:
    ```azurecli
    az provider register --namespace Microsoft.Kubernetes
    az provider register --namespace Microsoft.KubernetesConfiguration
    ```
2. Kayıt işlemini izleyin. Kayıt, en fazla 10 dakika sürebilir.
    ```azurecli
    az provider show -n Microsoft.Kubernetes -o table
    az provider show -n Microsoft.KubernetesConfiguration -o table    
    ```

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Kaynak grubu oluşturun:  

```console
az group create --name AzureArcTest -l EastUS -o table
```

```output
Location    Name
----------  ------------
eastus      AzureArcTest
```

## <a name="connect-an-existing-kubernetes-cluster"></a>Var olan bir Kubernetes kümesini bağlama

1. Aşağıdaki komutu kullanarak Kubernetes kümenizi Azure yaya bağlayın:
    ```console
    az connectedk8s connect --name AzureArcTest1 --resource-group AzureArcTest
    ```

    ```output
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

> [!TIP]
> Yukarıdaki konum parametresi olmadan Yukarıdaki komut, Azure Arc etkin Kubernetes kaynağını kaynak grubuyla aynı konumda oluşturur. Azure Arc etkin Kubernetes kaynağını farklı bir konumda oluşturmak için, komutunu ya da `--location <region>` `-l <region>` komutunu çalıştırırken belirtin `az connectedk8s connect` .

## <a name="verify-cluster-connection"></a>Küme bağlantısını doğrulama

Aşağıdaki komutla bağlı kümelerinizin bir listesini görüntüleyin:  

```console
az connectedk8s list -g AzureArcTest -o table
```

```output
Name           Location    ResourceGroup
-------------  ----------  ---------------
AzureArcTest1  eastus      AzureArcTest
```

> [!NOTE]
> Küme eklendikten sonra, küme meta verileri (küme sürümü, aracı sürümü, düğüm sayısı vb.) için, Azure portal 'de Azure Arc etkin Kubernetes kaynağının genel bakış sayfasında yüzey olarak 5 ila 10 dakika sürer.

## <a name="connect-using-an-outbound-proxy-server"></a>Giden proxy sunucusu kullanarak bağlanma

Kümeniz giden bir proxy sunucusunun arkasında ise, Azure CLı ve Azure Arc etkin Kubernetes aracılarının isteklerini giden ara sunucu aracılığıyla yönlendirmesi gerekir. 


1. Azure CLı için gereken ortam değişkenlerini giden proxy sunucusunu kullanacak şekilde ayarlayın:

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

2. Connect komutunu belirtilen proxy parametreleriyle Çalıştır:

    ```console
    az connectedk8s connect -n <cluster-name> -g <resource-group> --proxy-https https://<proxy-server-ip-address>:<port> --proxy-http http://<proxy-server-ip-address>:<port> --proxy-skip-range <excludedIP>,<excludedCIDR> --proxy-cert <path-to-cert-file>
    ```

> [!NOTE]
> * `excludedCIDR` `--proxy-skip-range` Küme içi iletişimin aracıların kesilmediğinden emin olmak için altında öğesini belirtin.
> * `--proxy-http`, `--proxy-https` ve `--proxy-skip-range` çoğu giden proxy ortamları için beklenmektedir. `--proxy-cert`*yalnızca* proxy tarafından beklenen güvenilen sertifikaları, aracı pods 'nin güvenilen sertifika deposuna eklemeniz gerekiyorsa gereklidir.

## <a name="view-azure-arc-agents-for-kubernetes"></a>Kubernetes için Azure Arc aracılarını görüntüleme

Azure Arc etkin Kubernetes, ad alanına birkaç işleç dağıtır `azure-arc` . 

1. Bu dağıtımları ve diğer bilgisayarları kullanarak görüntüleyin:

    ```console
    kubectl -n azure-arc get deployments,pods
    ```

1. Tüm yığınların bir durumda olduğunu doğrulayın `Running` .

    ```output
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

## <a name="clean-up-resources"></a>Kaynakları temizleme

Aşağıdaki komutu kullanarak Azure Arc etkin Kubernetes kaynağını, ilişkili yapılandırma kaynaklarını *ve* kümede çalışan tüm ARACıLARı Azure CLI kullanarak silebilirsiniz:

```azurecli
az connectedk8s delete --name AzureArcTest1 --resource-group AzureArcTest
```

>[!NOTE]
>Azure portal kullanarak Azure Arc etkin Kubernetes kaynağını silme, ilişkili yapılandırma kaynaklarını kaldırır *, ancak kümede çalışan aracıları kaldırmaz.* En iyi uygulama, Azure Arc etkin Kubernetes kaynağını Azure portal yerine kullanarak silmektir `az connectedk8s delete` .

## <a name="next-steps"></a>Sonraki adımlar

Gilar kullanarak bağlı Kubernetes kümenize yapılandırmaların nasıl dağıtılacağını öğrenmek için sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Gilar kullanarak yapılandırma dağıtma](tutorial-use-gitops-connected-cluster.md)
