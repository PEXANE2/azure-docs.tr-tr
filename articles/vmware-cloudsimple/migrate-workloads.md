---
title: CloudSimple'a göre Azure VMware Çözümü - İş yükü VM'leri Özel Bulut'a geçirin
description: Sanal makinelerin şirket içi vCenter'dan CloudSimple Private Cloud vCenter'a nasıl geçirilir olduğunu açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 87b8a112a319519dbde977ee30136a884137212d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77020004"
---
# <a name="migrate-workload-vms-from-on-premises-vcenter-to-private-cloud-vcenter-environment"></a>İş yükü VM'lerini şirket içi vCenter'dan Özel Bulut vCenter ortamına geçirin

VM'leri şirket içi veri merkezinden CloudSimple Private Cloud'unuza geçirmek için çeşitli seçenekler mevcuttur.  Özel Bulut, VMware vCenter'a yerel erişim sağlar ve VMware tarafından desteklenen araçlar iş yükü geçişi için kullanılabilir. Bu makalede, vCenter geçiş seçeneklerinden bazıları açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

Şirket içi veri merkezinizden VM ve verilerin geçişi, veri merkezinden Özel Bulut ortamınıza ağ bağlantısı gerektirir.  Ağ bağlantısını oluşturmak için aşağıdaki yöntemlerden birini kullanın:

* Şirket ortamınız ile Özel Bulut'unuzun arasında [siteden siteye VPN bağlantısı.](vpn-gateway.md#set-up-a-site-to-site-vpn-gateway)
* Şirket içi ExpressRoute devreniz ile CloudSimple ExpressRoute devreniz arasındaki ExpressRoute Global Reach bağlantısı.

Şirket içi vCenter ortamınızdan Özel Bulut'unuza giden ağ yolu, vMotion kullanarak VM'lerin geçişi için kullanılabilir olmalıdır.  Şirket içi vCenter'ınızdaki vMotion ağı yönlendirme yeteneklerine sahip olmalıdır.  Güvenlik duvarınızın şirket içi vCenter ve Private Cloud vCenter arasındaki tüm vMotion trafiğine izin verdiğini doğrulayın. (Özel Bulut'ta, vMotion ağında yönlendirme varsayılan olarak yapılandırılır.)

## <a name="migrate-isos-and-templates"></a>ISO'ları ve şablonları geçirin

Özel Bulut'unuzda yeni sanal makineler oluşturmak için ISO'lar ve VM şablonları kullanın.  ISO'ları ve şablonları Özel Bulut vCenter'ınıza yüklemek ve bunları kullanılabilir hale getirmek için aşağıdaki yöntemi kullanın.

1. ISO'yu vCenter UI'den Özel Bulut vCenter'a yükleyin.
2. Özel Bulut vCenter'ınızda [bir içerik kitaplığı yayımlayın:](https://docs.vmware.com/en/VMware-vSphere/6.5/com.vmware.vsphere.vm_admin.doc/GUID-2A0F1C13-7336-45CE-B211-610D39A6E1F4.html)

    1. Şirket içi içerik kitaplığınızı yayımlayın.
    2. Özel Bulut vCenter'da yeni bir içerik kitaplığı oluşturun.
    3. Yayınlanan şirket içi içerik kitaplığına abone olun.
    4. Abone olunan içeriğe erişmek için içerik kitaplığını senkronize edin.

## <a name="migrate-vms-using-powercli"></a>PowerCLI kullanarak VM'leri geçirin

VM'leri şirket içi vCenter'dan Özel Bulut vCenter'a geçirmek için VMware PowerCLI veya VMware Labs'dan temin edilebilen Cross vCenter İş Yükü Geçiş Yardımcı Programı'nı kullanın.  Aşağıdaki örnek komut dosyası PowerCLI geçiş komutlarını gösterir.

```
$sourceVC = Connect-VIServer -Server <source-vCenter name> -User <source-vCenter user name> -Password <source-vCenter user password>
$targetVC = Connect-VIServer -Server <target-vCenter name> -User <target-vCenter user name> -Password <target-vCenter user password>
$vmhost = <name of ESXi host on destination>
$vm = Get-VM -Server $sourceVC <name of VM>
Move-VM -VM $vm -VMotionPriority High -Destination (Get-VMhost -Server $targetVC -Name $vmhost) -Datastore (Get-Datastore -Server $targetVC -Name <name of tgt vc datastore>)
```

> [!NOTE]
> Hedef vCenter sunucusunun ve ESXi ana bilgisayarlarının adlarını kullanmak için, DNS iletmeişlemlerini şirket içinde Özel Bulut'unuza yapılandırın.

## <a name="migrate-vms-using-nsx-layer-2-vpn"></a>NSX Layer 2 VPN kullanarak VM'leri geçirin

Bu seçenek, şirket içi VMware ortamınızdan Azure'daki Özel Bulut'a iş yüklerinin canlı geçişini sağlar.  Bu genişletilmiş Katman 2 ağıyla, şirket içi alt ağ Özel Bulut'ta kullanılabilir.  Geçişten sonra, VM'ler için yeni IP adresi ataması gerekmez.

[Katman 2 genişletilmiş ağları kullanarak iş yüklerini geçir,](migration-layer-2-vpn.md) Katman 2 ağını şirket ortamınızdan Özel Bulut'unuza genişletmek için Katman 2 VPN'innasıl kullanılacağını açıklar.

## <a name="migrate-vms-using-backup-and-disaster-recovery-tools"></a>Yedekleme ve olağanüstü durum kurtarma araçlarını kullanarak VM'leri geçirme

VM'lerin Özel Bulut'a geçişi yedekleme/geri yükleme araçları ve olağanüstü durum kurtarma araçları kullanılarak yapılabilir.  Özel Bulut'u, üçüncü taraf bir araç kullanılarak oluşturulan yedeklemelerden geri yükleme hedefi olarak kullanın.  Özel Bulut, VMware SRM veya üçüncü taraf bir araç kullanılarak olağanüstü durum kurtarma hedefi olarak da kullanılabilir.

Bu araçları kullanarak daha fazla bilgi için aşağıdaki konulara bakın:

* [Veeam B&R kullanarak CloudSimple Private Cloud'da iş yükü sanal makineleri yedekle](backup-workloads-veeam.md)
* [CloudSimple Private Cloud'u şirket içi VMware iş yükleri için olağanüstü durum kurtarma sitesi olarak ayarlama](disaster-recovery-zerto.md)
