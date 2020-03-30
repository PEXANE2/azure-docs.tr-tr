---
title: Azure Hizmet Kumaşağı en iyi uygulamaları
description: Azure Service Fabric'i kullanarak ağ bağlantısını yönetmek için en iyi uygulamalar ve tasarım konuları.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: de2a74ad2d61de18d2150b72be3251e5b5583f2e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75551803"
---
# <a name="networking"></a>Ağ Oluşturma

Azure Hizmet Kumaşı kümelerini oluşturur ve yönetirken, düğümleriniz ve uygulamalarınız için ağ bağlantısı sağlarsınız. Ağ kaynakları IP adres aralıklarını, sanal ağları, yük dengeleyicilerini ve ağ güvenlik gruplarını içerir. Bu makalede, bu kaynaklar için en iyi uygulamaları öğreneceksiniz.

Aşağıdaki özellikleri kullanan kümelerin nasıl oluşturulacağımı öğrenmek için Azure [Hizmet Kumaş Ağ Desenleri'ni](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking) gözden geçirin: Varolan sanal ağ veya alt ağ, Statik genel IP adresi, yalnızca dahili yük dengeleyicisi veya Dahili ve harici yük dengeleyicisi.

## <a name="infrastructure-networking"></a>Altyapı Ağı
Virtual Machine'inizin Hızlandırılmış Ağ ile performansını en üst düzeye çıkarın, Kaynak Yöneticisi şablonunuzda enableAcceleratedNetworking özelliğini beyan ederek, aşağıdaki parçacık Sanal Makine Ölçeği Set NetworkInterfaceConfigurations'tan oluşur Hızlandırılmış Ağ sağlar:

```json
"networkInterfaceConfigurations": [
  {
    "name": "[concat(variables('nicName'), '-0')]",
    "properties": {
      "enableAcceleratedNetworking": true,
      "ipConfigurations": [
        {
        <snip>
        }
      ],
      "primary": true
    }
  }
]
```
Service Fabric kümesi [Hızlandırılmış Ağ ile Linux'ta](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)ve [Hızlandırılmış Ağ ile Windows'da](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell)sağlanabilir.

Hızlandırılmış Ağ, Azure Sanal Makine Serisi SK'leri için desteklenir: D/DSv2, D/DSv3, E/ESv3, F/FS, FSv2 ve Ms/Mms. Hızlandırılmış Ağ, 23/1/2019 tarihinde Service Fabric Windows Cluster için sku Standard_DS8_v3 kullanılarak ve 29/01/2019 tarihinde service Fabric Linux Cluster için Standard_DS12_v2 kullanılarak başarıyla test edilmiştir.

Varolan bir Hizmet Kumaşı kümesinde Hızlandırılmış Ağ'ı etkinleştirmek için, aşağıdakileri gerçekleştirmek için sanal makine ölçeği kümesi ekleyerek önce [Bir Hizmet Kumaşı kümesini ölçeklendirmeniz](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out)gerekir:
1. Hızlandırılmış Ağ etkinleştirilmiş bir Düğüm Tipi sağlama
2. Hizmetlerinizi ve durumlarını Hızlandırılmış Ağ etkinleştirilmiş olarak sağlanan Düğüm Türüne geçirin

Varolan bir kümede Hızlandırılmış Ağ'ı etkinleştirmek için altyapıyı ölçekleme gereklidir, çünkü Hızlandırılmış Ağ'ı yerinde etkinleştirmek, kullanılabilirlik kümesindeki tüm sanal makinelerin herhangi bir [varolan NIC'de Hızlandırılmış ağ'ı etkinleştirmeden önce durdurulmasını ve anlaşma sağlamasını](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms)gerektirdiğinden, kapalı kalma süresine neden olur.

## <a name="cluster-networking"></a>Küme Ağ

* Service Fabric [kümeleri, Service Fabric ağ desenlerinde](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking)özetlenen adımları izleyerek mevcut bir sanal ağa dağıtılabilir.

* Ağ güvenlik grupları (NSGs), gelen ve giden trafiği kümelerine kısıtlayan düğüm türleri için önerilir. NSG'de gerekli bağlantı noktalarının açıldığından emin olun. Örneğin: ![Hizmet Kumaş NSG Kuralları][NSGSetup]

* Service Fabric sistem hizmetlerini içeren birincil düğüm tipinin harici yük dengeleyicisi aracılığıyla maruz kalması gerekmez ve [dahili yük dengeleyicisi](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#internal-only-load-balancer) tarafından

* Kümeniz için statik bir [genel IP adresi](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#static-public-ip-address-1) kullanın.

## <a name="application-networking"></a>Uygulama Ağı

* Windows kapsayıcı iş yüklerini çalıştırmak için, hizmet-servis iletişimini kolaylaştırmak için [açık ağ modunu](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) kullanın.

* 80 veya 443 gibi yaygın uygulama bağlantı noktalarını ortaya çıkarmak için [Traefik](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/) veya [Service Fabric ters proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) gibi ters proxy kullanın.

* Azure bulut depolamaalanından temel katmanları çekemeyen hava boşluklu makinelerde barındırılan Windows Kapsayıcıları için, Docker daemon'daki [dağıtılamaz yapılara izin verme](https://docs.microsoft.com/virtualization/windowscontainers/about/faq#how-do-i-make-my-container-images-available-on-air-gapped-machines) bayrağını kullanarak yabancı katman davranışını geçersiz kılın.

## <a name="next-steps"></a>Sonraki adımlar

* Windows Server çalıştıran VM'lerde veya bilgisayarlarda küme [oluşturma: Windows Server için Hizmet Kumaşı küme oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* Linux çalıştıran VM'lerde veya bilgisayarlarda küme oluşturma: [Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png
