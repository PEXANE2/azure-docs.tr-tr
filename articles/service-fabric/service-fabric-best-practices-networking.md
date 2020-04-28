---
title: Azure Service Fabric ağ en iyi uygulamaları
description: Azure Service Fabric kullanarak ağ bağlantısını yönetmeye yönelik en iyi yöntemler ve tasarım konuları.
author: peterpogorski
ms.topic: conceptual
ms.date: 01/23/2019
ms.author: pepogors
ms.openlocfilehash: de2a74ad2d61de18d2150b72be3251e5b5583f2e
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75551803"
---
# <a name="networking"></a>Ağ

Azure Service Fabric kümelerini oluşturup yönetirken, düğümleriniz ve uygulamalarınız için ağ bağlantısı sağlayabilirsiniz. Ağ kaynakları IP adresi aralıklarını, sanal ağları, yük dengeleyicileri ve ağ güvenlik gruplarını içerir. Bu makalede, bu kaynaklar için en iyi yöntemleri öğreneceksiniz.

Şu özellikleri kullanan kümeler oluşturmayı öğrenmek için Azure [Service Fabric ağ düzenlerini](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking) gözden geçirin: var olan sanal ağ veya alt ağ, STATIK genel IP adresi, yalnızca iç yük dengeleyici veya iç ve dış yük dengeleyici.

## <a name="infrastructure-networking"></a>Altyapı ağı
Kaynak Yöneticisi şablonunuzda Enableivme ağ özelliğini bildirerek sanal makinenizin performansını hızlandırılmış ağ ile en üst düzeye çıkarın. Aşağıdaki kod parçacığı, hızlandırılmış ağ sağlayan bir sanal makine ölçek kümesi Networkınterfaceconfigurations ' dir:

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
Service Fabric küme, [hızlandırılmış ağ Ile Linux](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli)üzerinde sağlanabilir ve [hızlandırılmış ağ ile Windows](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-powershell)'u kullanabilir.

Hızlandırılmış ağ, Azure sanal makine serisi SKU 'Ları için desteklenir: D/DSv2, D/DSv3, E/ESv3, F/FS, FSv2 ve MS/MMS. Hızlandırılmış ağ, Service Fabric bir Windows kümesi için 1/23/2019 Standard_DS8_v3 SKU 'SU kullanılarak başarıyla test edildi ve Service Fabric Linux kümesi için 01/29/2019 üzerinde Standard_DS12_v2 kullanılıyor.

Mevcut bir Service Fabric kümesinde hızlandırılmış ağı etkinleştirmek için, önce aşağıdakileri gerçekleştirmek üzere bir [sanal makine ölçek kümesi ekleyerek bir Service Fabric kümesini ölçeklendirmelisiniz](https://docs.microsoft.com/azure/service-fabric/virtual-machine-scale-set-scale-node-type-scale-out):
1. Hızlandırılmış ağ etkinken bir NodeType sağlama
2. Hızlandırılmış ağ etkinken hizmetlerinizi ve durumlarını sağlanan NodeType öğesine geçirin

Mevcut bir kümede hızlandırılmış ağı etkinleştirmek için genişleme altyapısının ölçeğini ayarlamak gerekir, çünkü yerinde hızlandırılmış ağın etkinleştirilmesi, [mevcut BIR NIC üzerinde hızlandırılmış ağı etkinleştirmeden önce](https://docs.microsoft.com/azure/virtual-network/create-vm-accelerated-networking-cli#enable-accelerated-networking-on-existing-vms), bir kullanılabilirlik kümesindeki tüm sanal makinelerin durdurulması ve serbest kalması gerektiğinden kapalı kalma süresine neden olur.

## <a name="cluster-networking"></a>Küme ağı

* Service Fabric kümeler, [Service Fabric ağ desenlerinde](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking)özetlenen adımları izleyerek mevcut bir sanal ağa dağıtılabilir.

* Ağ güvenlik grupları (NSG 'ler), kümelerinde gelen ve giden trafiği kısıtlayan düğüm türleri için önerilir. NSG 'de gerekli bağlantı noktalarının açık olduğundan emin olun. Örneğin: ![Service Fabric NSG kuralları][NSGSetup]

* Service Fabric sistem hizmetlerinin bulunduğu birincil düğüm türü, dış yük dengeleyici aracılığıyla gösterilmemelidir ve bir [iç yük dengeleyici](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#internal-only-load-balancer) tarafından gösterilebilir.

* Kümeniz için [statik bir genel IP adresi](https://docs.microsoft.com/azure/service-fabric/service-fabric-patterns-networking#static-public-ip-address-1) kullanın.

## <a name="application-networking"></a>Uygulama Ağı

* Windows kapsayıcı iş yüklerini çalıştırmak için, hizmetten hizmete iletişim kurmak üzere [açık ağ modu](https://docs.microsoft.com/azure/service-fabric/service-fabric-networking-modes#set-up-open-networking-mode) ' nu kullanın.

* 80 veya 443 gibi yaygın uygulama bağlantı noktalarını kullanıma sunmak için [Traefik](https://docs.traefik.io/v1.6/configuration/backends/servicefabric/) gibi bir ters proxy veya [Service Fabric ters proxy](https://docs.microsoft.com/azure/service-fabric/service-fabric-reverseproxy) kullanın.

* Azure bulut depolama alanından temel katmanları çekmeden, AIR özellikli makinelerde barındırılan Windows kapsayıcıları için, Docker Daemon 'daki [--Allow-nondağıtılabilir-yapay](https://docs.microsoft.com/virtualization/windowscontainers/about/faq#how-do-i-make-my-container-images-available-on-air-gapped-machines) bayrağını kullanarak yabancı katman davranışını geçersiz kılın.

## <a name="next-steps"></a>Sonraki adımlar

* VM 'lerde veya Windows Server çalıştıran bilgisayarlarda küme oluşturma: [Windows Server için Service Fabric kümesi oluşturma](service-fabric-cluster-creation-for-windows-server.md)
* VM 'lerde veya Linux çalıştıran bilgisayarlarda küme oluşturma: [Linux kümesi oluşturma](service-fabric-cluster-creation-via-portal.md)
* [Service Fabric destek seçenekleri](service-fabric-support.md) hakkında bilgi edinin

[NSGSetup]: ./media/service-fabric-best-practices/service-fabric-nsg-rules.png
