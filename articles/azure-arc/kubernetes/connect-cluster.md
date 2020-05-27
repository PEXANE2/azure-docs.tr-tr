---
title: Azure Arc etkin bir Kubernetes kümesine bağlanma (Önizleme)
services: azure-arc
ms.service: azure-arc
ms.date: 05/19/2020
ms.topic: article
author: mlearned
ms.author: mlearned
description: Azure Arc ile Azure Arc etkin bir Kubernetes kümesi bağlama
keywords: Kubernetes, yay, Azure, K8s, kapsayıcılar
ms.openlocfilehash: 962b6a17743ea2beed1e16503739c55c83babbce
ms.sourcegitcommit: 95269d1eae0f95d42d9de410f86e8e7b4fbbb049
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83860554"
---
# <a name="connect-an-azure-arc-enabled-kubernetes-cluster-preview"></a>Azure Arc etkin bir Kubernetes kümesine bağlanma (Önizleme)

Bir Kubernetes kümesini Azure yaya bağlayın. 

## <a name="before-you-begin"></a>Başlamadan önce

Aşağıdaki gereksinimlerin hazırlanaldığını doğrulayın:

* Çalışır duruma sahip bir Kubernetes kümesi
* Kubeconfig ve Cluster-admin erişimiyle erişmeniz gerekir. 
* Ve komutlarıyla kullanılan Kullanıcı veya hizmet sorumlusu `az login` `az connectedk8s connect` ' Microsoft. Kubernetes/connectedkümeler ' kaynak türü üzerinde ' Read ' ve ' Write ' izinlerine sahip olmalıdır. Bu izinlere sahip olan "Kubernetes ekleme için Azure Arc" rolü, ekleme için Azure CLı ile kullanılan Kullanıcı veya hizmet sorumlusu üzerinde rol atamaları için kullanılabilir.
* *Connectedk8s* ve *k8sconfiguration* uzantılarının en son sürümü

## <a name="supported-regions"></a>Desteklenen bölgeler

* Doğu ABD
* Batı Avrupa

## <a name="network-requirements"></a>Ağ gereksinimleri

Azure Arc aracıları için aşağıdaki protokollerin/bağlantı noktalarının/giden URL 'Lerin çalışması gerekir.

* 443 numaralı bağlantı noktasında TCP-->`https://:443`
* 9418 numaralı bağlantı noktasında TCP-->`git://:9418`

| Uç nokta (DNS)                                                                                               | Açıklama                                                                                                                 |
| ------------------------------------------------------------------------------------------------------------ | --------------------------------------------------------------------------------------------------------------------------- |
| `https://management.azure.com`                                                                                 | Aracının Azure 'a bağlanması ve kümeyi kaydetmesi için gereklidir                                                        |
| `https://eastus.dp.kubernetesconfiguration.azure.com`, `https://westeurope.dp.kubernetesconfiguration.azure.com` | Aracının durum ve getirme yapılandırma bilgilerini itilmesi için veri düzlemi uç noktası                                      |
| `https://docker.io`                                                                                            | Kapsayıcı görüntülerini çekmek için gereklidir                                                                                         |
| `https://github.com`, git://github.com                                                                         | Örnek giler depoları GitHub üzerinde barındırılır. Yapılandırma Aracısı, belirttiğiniz git uç noktasına bağlantı gerektirir. |
| `https://login.microsoftonline.com`                                                                            | Azure Resource Manager belirteçleri getirmek ve güncelleştirmek için gereklidir                                                                                    |
| `https://azurearcfork8s.azurecr.io`                                                                            | Azure Arc aracıları için kapsayıcı görüntülerini çekmek için gereklidir                                                                  |

## <a name="register-the-two-providers-for-azure-arc-enabled-kubernetes"></a>Azure Arc etkin Kubernetes için iki sağlayıcıyı kaydedin:

```console
az provider register --namespace Microsoft.Kubernetes
Registering is still on-going. You can monitor using 'az provider show -n Microsoft.Kubernetes'

az provider register --namespace Microsoft.KubernetesConfiguration
Registering is still on-going. You can monitor using 'az provider show -n Microsoft.KubernetesConfiguration'
```

Kayıt, zaman uyumsuz bir işlemdir. Kayıt, yaklaşık 10 dakika sürebilir. Aşağıdaki komutlarla kayıt işlemini izleyebilirsiniz:

```console
az provider show -n Microsoft.Kubernetes -o table
```

```console
az provider show -n Microsoft.KubernetesConfiguration -o table
```

## <a name="install-azure-cli-extensions"></a>Azure CLı uzantıları 'nı yükler

`connectedk8s`Kubernetes kümelerini Azure 'a bağlamanıza yardımcı olan uzantıyı yükleyebilirsiniz:

```console
az extension add --name connectedk8s
```

Uzantıyı yükler `k8sconfiguration` :

```console
az extension add --name k8sconfiguration
```

Uzantıları en son sürümlere güncelleştirmek için aşağıdaki komutları çalıştırın.

```console
az extension update --name connectedk8s
az extension update --name k8sconfiguration
```

## <a name="create-a-resource-group"></a>Kaynak Grubu oluşturma

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

Sonra, Kubernetes kümemizi Azure 'a bağlayacağız. İçin iş akışı `az connectedk8s connect` aşağıdaki gibidir:

1. Kubernetes kümenize bağlantıyı doğrulama: `KUBECONFIG` , `~/.kube/config` veya ile`--kube-config`
1. Held 3 kullanarak Kubernetes için Azure Arc aracılarını `azure-arc` ad alanına dağıtma

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

Bağlı kümelerinizi listeleyin:

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

Azure Arc etkin Kubernetes, ad alanına birkaç işleç dağıtır `azure-arc` . Bu dağıtımları ve pod 'leri buradan görüntüleyebilirsiniz:

```console
kubectl -n azure-arc get deployments,pods
```

**Çıktıların**

```console
NAME                                        READY   UP-TO-DATE AVAILABLE AGE
deployment.apps/cluster-metadata-operator   1/1     1           1        16h
deployment.apps/clusteridentityoperator     1/1     1           1        16h
deployment.apps/config-agent                1/1     1           1        16h
deployment.apps/controller-manager          1/1     1           1        16h
deployment.apps/flux-logs-agent             1/1     1           1        16h
deployment.apps/metrics-agent               1/1     1           1        16h
deployment.apps/resource-sync-agent         1/1     1           1        16h

NAME                                            READY   STATUS   RESTART AGE
pod/cluster-metadata-operator-7fb54d9986-g785b  2/2     Running  0       16h
pod/clusteridentityoperator-6d6678ffd4-tx8hr    3/3     Running  0       16h
pod/config-agent-544c4669f9-4th92               3/3     Running  0       16h
pod/controller-manager-fddf5c766-ftd96          3/3     Running  0       16h
pod/flux-logs-agent-7c489f57f4-mwqqv            2/2     Running  0       16h
pod/metrics-agent-58b765c8db-n5l7k              2/2     Running  0       16h
pod/resource-sync-agent-5cf85976c7-522p5        3/3     Running  0       16h
```

## <a name="azure-arc-agents-for-kubernetes"></a>Kubernetes için Azure Arc aracıları

Azure Arc etkin Kubernetes, kümenizde çalışan ve ad alanına dağıtılan birkaç aracıdan (işleçlerden) oluşur `azure-arc` .

* `deployment.apps/config-agent`: kümede ve güncelleştirmelerin uyumluluk durumunda uygulanan kaynak denetimi yapılandırma kaynakları için bağlı kümeyi izler
* `deployment.apps/controller-manager`: işleçler operatörü ve Azure Arc bileşenleri arasındaki etkileşimleri düzenler
* `deployment.apps/metrics-agent`: Bu aracıların en iyi performansı sergilediğinden emin olmak için diğer yay aracılarının ölçümlerini toplar
* `deployment.apps/cluster-metadata-operator`: küme meta verilerini toplar-küme sürümü, düğüm sayısı ve yay Aracısı sürümü
* `deployment.apps/resource-sync-agent`: Yukarıdaki belirtilen küme meta verilerini Azure 'da eşitler
* `deployment.apps/clusteridentityoperator`: diğer aracıların Azure ile iletişim için kullandığı yönetilen hizmet kimliği (MSI) sertifikasını tutar
* `deployment.apps/flux-logs-agent`: kaynak denetimi yapılandırmasının bir parçası olarak dağıtılan Flox işleçlerinden günlükleri toplar

## <a name="delete-a-connected-cluster"></a>Bağlı kümeyi silme

`Microsoft.Kubernetes/connectedcluster`Azure CLI veya Azure Portal kullanarak bir kaynağı silebilirsiniz.

Azure CLı komutu, `az connectedk8s delete` `Microsoft.Kubernetes/connectedCluster` kaynağı Azure 'da kaldırır. Azure CLı, `sourcecontrolconfiguration` Azure 'da ilişkili tüm kaynakları siler. Azure CLı, kümedeki aracıları kaldırmak için Held kaldırma kullanır.

Azure portal `Microsoft.Kubernetes/connectedcluster` Azure 'daki kaynağı siler ve Azure 'daki tüm ilişkili kaynakları siler `sourcecontrolconfiguration` .

Kümedeki aracıları kaldırmak için veya çalıştırmanız gerekir `az connectedk8s delete` `helm uninstall azurearcfork8s` .

## <a name="next-steps"></a>Sonraki adımlar

* [Bağlı kümede GitOps kullanma](./use-gitops-connected-cluster.md)
* [Küme yapılandırmasını yönetmek için Azure Ilkesini kullanma](./use-azure-policy.md)
