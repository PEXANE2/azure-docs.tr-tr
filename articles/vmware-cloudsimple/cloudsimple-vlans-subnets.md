---
title: CloudSimple tarafından Azure VMware Solution'daki V'ler ve alt ağlar
description: CloudSimple Private Cloud'da V'n'ler ve alt ağlar hakkında bilgi edinin
author: sharaths-cs
ms.author: dikamath
ms.date: 08/15/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: 2451fbb69636624db354006df2a7925ef9e75459
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77024900"
---
# <a name="vlans-and-subnets-overview"></a>VUN'lara ve alt ağlara genel bakış

CloudSimple, CloudSimple hizmetinizin dağıtıldığı bölge başına bir ağ sağlar.  Ağ, varsayılan olarak etkin olan tek bir TCP Katmanı 3 adres alanıdır.  Bu bölgede oluşturulan tüm Özel Bulutlar ve alt ağlar, ek yapılandırma olmadan birbirleriyle iletişim kurabilir.  VLAN'ları kullanarak vCenter'da dağıtılmış bağlantı noktası grupları oluşturabilirsiniz.

![CloudSimple Ağ Topolojisi](media/cloudsimple-network-topology.png)

## <a name="vlans"></a>Vlan

Her Özel Bulut için bir VLAN (Katman 2 ağı) oluşturulur.  Katman 2 trafiği, Özel Bulut'un sınırları içinde kalır ve Özel Bulut içindeki yerel trafiği yalıtmanızı sağlar.  Özel Bulut'ta oluşturulan bir VLAN, yalnızca o Özel Bulut'ta dağıtılmış bağlantı noktası grupları oluşturmak için kullanılabilir.  Özel Bulut'ta oluşturulan bir VLAN, Özel Bulut'un ana bilgisayarlarına bağlı tüm anahtarlar üzerinde otomatik olarak yapılandırılır.

## <a name="subnets"></a>Alt ağlar

Alt netin adres alanını tanımlayarak bir VLAN oluşturduğunuzda bir alt ağ oluşturabilirsiniz. Adres alanından bir IP adresi alt ağ geçidi olarak atanır. Müşteri ve bölge başına tek bir özel Katman 3 adres alanı atanır. RFC 1918 örtüşmeyen adres alanını, şirket içi ağınızla veya Azure sanal ağınızla ağ bölgenizde yapılandırabilirsiniz.

Tüm alt ağlar varsayılan olarak birbirleriyle iletişim kurarak Özel Bulutlar arasında yönlendirme için yapılandırma ek yükü azaltılabilir. Aynı bölgedeki bilgisayarlar arasında doğu-batı verileri aynı Katman 3 ağında kalır ve bölge içindeki yerel ağ altyapısı üzerinden aktarılabiliyor. Bir bölgedeki Özel Bulutlar arasındaki iletişim için çıkış gerekmez. Bu yaklaşım, farklı Özel Bulutlar'da farklı iş yüklerini dağıtmada herhangi bir WAN/çıkış performans cezasını ortadan kaldırır.

## <a name="vspherevsan-subnets-cidr-range"></a>vSphere/vSAN alt ağları CIDR aralığı

Özel Bulut, vCenter sunucusu tarafından yönetilen yalıtılmış bir VMware yığını (ESXi ana bilgisayarları, vCenter, vSAN ve NSX) ortamı olarak oluşturulur.  Yönetim bileşenleri vSphere/vSAN alt ağları CIDR için seçilen ağda dağıtılır.  Ağ CIDR aralığı dağıtım sırasında farklı alt ağlara ayrılmıştır.

* Minimum vSphere/vSAN alt ağları CIDR aralığı öneki: **/24**
* Maksimum vSphere/vSAN alt ağları CIDR aralığı öneki: **/21**

> [!CAUTION]
> vSphere/vSAN CIDR serisindeki IP adresleri Özel Bulut altyapısı tarafından kullanılmak üzere ayrılmıştır.  Bu aralıktaki IP adresini herhangi bir sanal makinede kullanmayın.

### <a name="vspherevsan-subnets-cidr-range-limits"></a>vSphere/vSAN alt ağları CIDR aralığı limitleri

vSphere/vSAN alt ağlarıCIDR aralığı boyutunun seçilmesi Özel Bulut'unuzun boyutu üzerinde etkilidir.  Aşağıdaki tabloda vSphere/vSAN alt ağları CIDR boyutuna göre sahip olabileceğiniz maksimum düğüm sayısı gösterilmektedir.

| Belirtilen vSphere/vSAN alt ağları CIDR öneki uzunluğu | Maksimum düğüm sayısı |
|---------------------------------------------------|-------------------------|
| /24 | 26 |
| /23 | 58 |
| /22 | 118 |
| /21 | 220 |

### <a name="management-subnets-created-on-a-private-cloud"></a>Özel Bulut'ta oluşturulan yönetim alt ağları

Özel Bulut oluşturduğunuzda aşağıdaki yönetim alt ağları oluşturulur.

* **Sistem yönetimi**. ESXi'nin yönetim ağı, DNS sunucusu, vCenter sunucusu için VLAN ve alt ağı.
* **VMotion**. ESXi'nin vMotion ağlarını barındıran VLAN ve alt ağı.
* **VSAN**. ESXi'nin vSAN ağına ev sahipliği yapan VLAN ve subnet.
* **NsxtEdgeUplink1**. VLAN için VLAN ve alt ağ harici bir ağa uplinks.
* **NsxtEdgeUplink2**. VLAN için VLAN ve alt ağ harici bir ağa uplinks.
* **NsxtEdgeTransport**. Taşıma bölgeleri için VLAN ve alt ağ, NSX-T'deki Katman 2 ağlarının erişimini kontrol eder.
* **NsxtHostTransport**. VLAN ve ana taşıma bölgesi için alt ağ.

### <a name="management-network-cidr-range-breakdown"></a>Yönetim ağı CIDR aralık dökümü

vSphere/vSAN subnets CIDR aralığı belirtilen birden fazla alt ağlara ayrılmıştır.  Aşağıdaki tablo, izin verilen önekler için dökümübir örnek gösterir.  Örnek, CIDR aralığı olarak 192.168.0.0 kullanır.

Örnek:

| Belirtilen vSphere/vSAN alt ağları CIDR/öneki | 192.168.0.0/21 | 192.168.0.0/22 | 192.168.0.0/23 | 192.168.0.0/24 |
|---------------------------------|----------------|----------------|----------------|----------------|
| Sistem yönetimi | 192.168.0.0/24 | 192.168.0.0/24 | 192.168.0.0/25 | 192.168.0.0/26 |
| vMotion | 192.168.1.0/24 | 192.168.1.0/25 | 192.168.0.128/26 | 192.168.0.64/27 |
| vSAN | 192.168.2.0/24 | 192.168.1.128/25 | 192.168.0.192/26 | 192.168.0.96/27 |
| NSX-T Ana Taşıyıcı | 192.168.4.0/23 | 192.168.2.0/24 | 192.168.1.0/25 | 192.168.0.128/26 |
| NSX-T Kenar Taşıma | 192.168.7.208/28 | 192.168.3.208/28 | 192.168.1.208/28 | 192.168.0.208/28 |
| NSX-T Kenar Uplink1 | 192.168.7.224/28 | 192.168.3.224/28 | 192.168.1.224/28 | 192.168.0.224/28 |
| NSX-T Kenar uplink2 | 192.168.7.240/28 | 192.168.3.240/28 | 192.168.1.240/28 | 192.168.0.240/28 |

## <a name="next-steps"></a>Sonraki adımlar

* [VLAN ve alt ağlar oluşturma ve yönetme](create-vlan-subnet.md)
