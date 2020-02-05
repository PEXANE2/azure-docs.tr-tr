---
title: Azure VMware çözümleri (AVS)-iş yükü VM 'lerini AVS özel bulutuna geçirme
description: Şirket içi vCenter 'dan AVS özel bulut vCenter 'a sanal makinelerin nasıl geçirileceği açıklanmaktadır
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: af02bd947c73b670306704a10a09ca981b34c9a9
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77020004"
---
# <a name="migrate-workload-vms-from-on-premises-vcenter-to-avs-private-cloud-vcenter-environment"></a>Şirket içi vCenter 'dan AVS özel bulut vCenter ortamına iş yükü VM 'lerini geçirme

Şirket içi bir veri merkezindeki sanal makineleri AVS özel buluta geçirmek için birkaç seçenek mevcuttur. AVS özel bulutu, VMware vCenter 'a yerel erişim sağlar ve VMware tarafından desteklenen araçlar iş yükü geçişi için kullanılabilir. Bu makalede vCenter geçiş seçeneklerinden bazıları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Şirket içi veri merkezinizden VM 'lerin ve verilerin geçirilmesi, veri merkezinizden AVS özel bulut ortamınıza ağ bağlantısı gerektirir. Ağ bağlantısı kurmak için aşağıdaki yöntemlerden birini kullanın:

* Şirket içi ortamınız ile AVS özel bulutunuz arasında [siteden sıteye VPN bağlantısı](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway) .
* ExpressRoute, şirket içi ExpressRoute bağlantı hattı ve bir AVS ExpressRoute bağlantı hattı arasındaki bağlantıyı Global Reach.

Şirket içi vCenter ortamınızdan, vMotion kullanılarak sanal makinelerin geçirilmesi için bir ağ yolu olması gerekir. Şirket içi vCenter 'daki vMotion ağı yönlendirme yeteneklerine sahip olmalıdır. Güvenlik duvarınızın, şirket içi vCenter ve AVS özel bulut vCenter arasındaki tüm vMotion trafiğine izin verdiğini doğrulayın. (AVS özel bulutu üzerinde, vMotion ağı üzerinde yönlendirme varsayılan olarak yapılandırılır.)

## <a name="migrate-isos-and-templates"></a>IOS ve şablonları geçirme

AVS özel bulutunuzda yeni sanal makineler oluşturmak için ISOs ve VM şablonlarını kullanın. IOS ve şablonları AVS özel bulut vCenter 'a yüklemek ve kullanılabilir hale getirmek için aşağıdaki yöntemi kullanın.

1. ISO Kullanıcı arabirimindeki AVS özel bulut vCenter 'a ISO 'yu yükleyin.
2. AVS özel bulut vCenter 'unuzda [bir içerik kitaplığı yayımlayın](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A0F1C13-7336-45CE-B211-610D39A6E1F4.html) :

    1. Şirket içi içerik kitaplığınızı yayımlayın.
    2. AVS özel bulutu vCenter üzerinde yeni bir içerik kitaplığı oluşturun.
    3. Yayımlanan şirket içi içerik kitaplığına abone olun.
    4. Abone olunan içeriğe erişmek için içerik kitaplığını eşitler.

## <a name="migrate-vms-using-powercli"></a>PowerCLI kullanarak VM 'Leri geçirme

Şirket içi vCenter 'dan AVS özel bulut vCenter 'a sanal makineler geçirmek için VMware PowerCLI veya VMware Labs 'den kullanılabilen çapraz vCenter Iş yükü geçiş yardımcı programını kullanın. Aşağıdaki örnek betik PowerCLI geçiş komutlarını göstermektedir.

```
$sourceVC = Connect-VIServer -Server <source-vCenter name> -User <source-vCenter user name> -Password <source-vCenter user password>
$targetVC = Connect-VIServer -Server <target-vCenter name> -User <target-vCenter user name> -Password <target-vCenter user password>
$vmhost = <name of ESXi host on destination>
$vm = Get-VM -Server $sourceVC <name of VM>
Move-VM -VM $vm -VMotionPriority High -Destination (Get-VMhost -Server $targetVC -Name $vmhost) -Datastore (Get-Datastore -Server $targetVC -Name <name of tgt vc datastore>)
```

> [!NOTE]
> Hedef vCenter Server ve ESXi konaklarının adlarını kullanmak için Şirket içinden AVS özel bulutuna DNS iletmeyi yapılandırın.

## <a name="migrate-vms-using-nsx-layer-2-vpn"></a>NSX katman 2 VPN kullanarak VM 'Leri geçirme

Bu seçenek, şirket içi VMware ortamınızdan Azure 'daki AVS özel bulutuna iş yüklerinin dinamik olarak geçirilmesini sağlar. Bu uzatılmış katman 2 ağıyla, şirket içi içindeki alt ağ, AVS özel bulutu 'nda kullanılabilir olacaktır. Geçişten sonra VM 'Ler için yeni IP adresi ataması gerekli değildir.

[Katman 2 uzatılmış ağları kullanarak iş yüklerini geçirme](migration-layer-2-vpn.md) katman 2 ağını, şirket içi ORTAMıNıZDAN AVS özel bulutuna bir katman 2 ağını uzatmak için nasıl kullanacağınızı açıklar.

## <a name="migrate-vms-using-backup-and-disaster-recovery-tools"></a>Yedekleme ve olağanüstü durum kurtarma araçlarını kullanarak VM 'Leri geçirme

VM 'lerin AVS özel bulutuna geçirilmesi, yedekleme/geri yükleme araçları ve olağanüstü durum kurtarma araçları kullanılarak yapılabilir. Bir üçüncü taraf aracı kullanılarak oluşturulan yedeklerden geri yükleme için bir hedef olarak AVS özel bulutu 'nı kullanın. AVS özel bulutu, VMware SRM veya üçüncü taraf bir aracı kullanılarak olağanüstü durum kurtarma için hedef olarak da kullanılabilir.

Bu araçları kullanma hakkında daha fazla bilgi için aşağıdaki konulara bakın:

* [Veeam B & R kullanarak AVS özel bulutundaki iş yükünü sanal makinelerini yedekleme](backup-workloads-veeam.md)
* [Şirket içi VMware iş yükleri için AVS özel bulutunu olağanüstü durum kurtarma sitesi olarak ayarlama](disaster-recovery-zerto.md)
