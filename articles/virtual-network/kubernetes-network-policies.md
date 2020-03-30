---
title: Azure Kubernetes ağ ilkeleri | Microsoft Dokümanlar
description: Kubernetes kümenizi güvence altına almak için Kubernetes ağ ilkeleri hakkında bilgi edinin.
services: virtual-network
documentationcenter: na
author: aanandr
manager: NarayanAnnamalai
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 9/25/2018
ms.author: aanandr
ms.custom: ''
ms.openlocfilehash: 5a6da7e65a9a3e962a2df37b062792fbb990d04d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73159685"
---
# <a name="azure-kubernetes-network-policies-overview"></a>Azure Kubernetes ağ ilkelerine genel bakış

Ağ İlkeleri, Tıpkı Ağ Güvenlik Grupları (NSG'ler) VM'ler için mikro segmentasyon sağladığı gibi bölmeler için mikro segmentasyon sağlar. Azure Ağ İlkesi uygulaması standart Kubernetes Ağ İlkesi belirtimini destekler. Etiketleri bir bölme grubu seçmek ve bu bölmelere izin verilen trafiğin türünü belirten giriş ve çıkış kuralları listesini tanımlamak için kullanabilirsiniz. [Kubernetes belgelerindeki](https://kubernetes.io/docs/concepts/services-networking/network-policies/)Kubernetes ağ ilkeleri hakkında daha fazla bilgi edinin.

![Kubernetes ağ politikalarına genel bakış](./media/kubernetes-network-policies/kubernetes-network-policies-overview.png)

Azure ağ ilkeleri, kapsayıcılar için VNet tümleştirmesi sağlayan Azure CNI ile birlikte çalışır. Günümüzde sadece Linux düğümleri ile desteklenir. Uygulamalar, trafik filtrelemeni zorlamak için tanımlanan ilkelere göre Linux IP Table kurallarını yapılandırır.

## <a name="planning-security-for-your-kubernetes-cluster"></a>Kubernetes kümeniz için planlama güvenliği
Kümeniz için güvenlik uygularken, Kuzey-Güney trafiğini, yani küme alt ağınıza giren ve çıkan trafiği filtrelemek için ağ güvenlik gruplarını (NSGs) kullanın ve Doğu-Batı trafiği için Kubernetes ağ ilkelerini, yani kümenizi.

## <a name="using-azure-kubernetes-network-policies"></a>Azure Kubernetes ağ ilkelerini kullanma
Azure Ağ İlkeleri, bölmeler için mikro segmentasyon sağlamak için aşağıdaki yollarla kullanılabilir.

### <a name="acs-engine"></a>ACS motoru
ACS-Engine, Azure'da bir Kubernetes kümesinin dağıtımı için bir Azure Kaynak Yöneticisi şablonu oluşturan bir araçtır. Küme yapılandırması, şablon oluşturma sırasında araca geçirilen bir JSON dosyasında belirtilir. Desteklenen küme ayarlarının tam listesi ve açıklamaları hakkında daha fazla bilgi edinmek için bkz. Microsoft Azure Container Service Engine - Küme Tanımı.

ACS altyapısı kullanılarak dağıtılan kümeler üzerindeki ilkeleri etkinleştirmek için küme tanım dosyasındaki ağPolitikası ayarını "azure" olarak belirtin.

#### <a name="example-configuration"></a>Örnek yapılandırma

Aşağıdaki JSON örnek yapılandırması yeni bir sanal ağ ve alt ağ oluşturur ve Azure CNI ile içinde bir Kubernetes kümesi dağıtır. JSON dosyasını yeniden kullanmak için "Not Defteri"ni kullanmanızı öneririz. 
```json
{
  "apiVersion": "vlabs",
  "properties": {
    "orchestratorProfile": {
      "orchestratorType": "Kubernetes",
      "kubernetesConfig": {
         "networkPolicy": "azure"
       }
    },
    "masterProfile": {
      "count": 1,
      "dnsPrefix": "<specify a cluster name>",
      "vmSize": "Standard_D2s_v3"
    },
    "agentPoolProfiles": [
      {
        "name": "agentpool",
        "count": 2,
        "vmSize": "Standard_D2s_v3",
        "availabilityProfile": "AvailabilitySet"
      }
    ],
   "linuxProfile": {
      "adminUsername": "<specify admin username>",
      "ssh": {
        "publicKeys": [
          {
            "keyData": "<cut and paste your ssh key here>"
          }
        ]
      }
    },
    "servicePrincipalProfile": {
      "clientId": "<enter the client ID of your service principal here >",
      "secret": "<enter the password of your service principal here>"
    }
  }
}

```
### <a name="creating-your-own-kubernetes-cluster-in-azure"></a>Azure'da kendi Kubernetes kümenizi oluşturma
Uygulama, ACS-Engine gibi araçlara güvenmeden, kendi başınıza dağıttığınız Kubernetes kümelerinde Podlar için Ağ İlkeleri sağlamak için kullanılabilir. Bu durumda, önce CNI eklentisini yükler ve kümedeki her sanal makinede etkinleştirin. Ayrıntılı yönergeler için bkz. [Eklentiyi kendi dağıttığınız Kubernetes kümesi için dağıtma](deploy-container-networking.md#deploy-plug-in-for-a-kubernetes-cluster).

Küme dağıtıldıktan sonra, Azure `kubectl` ağ ilkesi *daemonsetini* indirmek ve kümeye uygulamak için aşağıdaki komutu çalıştırın.

  ```
  kubectl apply -f https://raw.githubusercontent.com/Azure/acs-engine/master/parts/k8s/addons/kubernetesmasteraddons-azure-npm-daemonset.yaml

  ```
Çözüm aynı zamanda açık kaynak kodludur ve kod [Azure Kapsayıcı Ağı deposunda](https://github.com/Azure/azure-container-networking/tree/master/npm)kullanılabilir.



## <a name="next-steps"></a>Sonraki adımlar
- Azure [Kubernetes Hizmeti](../aks/intro-kubernetes.md)hakkında bilgi edinin.
-  Kapsayıcı [ağ](container-networking-overview.md)hakkında bilgi edinin.
- Kubernetes kümeleri veya Docker kapsayıcıları için [eklentiyi dağıtın.](deploy-container-networking.md)
