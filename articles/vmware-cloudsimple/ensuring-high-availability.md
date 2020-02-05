---
title: Azure 'da VMware 'de çalışırken uygulamanın yüksek oranda kullanılabilir olmasını sağlayın
description: AVS özel bulutu 'nda çalışan uygulamalar için ortak uygulama hatası senaryolarına yönelik olarak, AVS 'nin yüksek kullanılabilirlik özelliklerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: b32f7f3f38098f935382cce46d8251340784b940
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77025359"
---
# <a name="ensure-application-high-availability-when-running-in-vmware-on-azure"></a>Azure 'da VMware 'de çalışırken uygulamanın yüksek oranda kullanılabilir olmasını sağlayın

AVS çözümü, Azure ortamında VMware üzerinde çalışan uygulamalarınız için yüksek kullanılabilirlik sağlar. Aşağıdaki tabloda hata senaryoları ve ilişkili yüksek kullanılabilirlik özellikleri listelenmektedir.

| Hata senaryosu | Uygulama korumalı mı? | Platform HA özelliği | VMware HA özelliği | Azure HA özelliği |
------------ | ------------- | ------------ | ------------ | ------------- |
| Disk hatası | YES | Hatalı düğümü hızlı değiştirme | [VSAN varsayılan depolama Ilkesi hakkında](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.virtualsan.doc/GUID-C228168F-6807-4C2A-9D74-E584CAF49A2A.html) |
| Fan hatası | YES | Gereksiz fanlar, başarısız olan düğümün hızlı değiştirilmesi |  |  |
| NIC hatası | YES | Yedekli NIC, başarısız olan düğümün hızlı değiştirilmesi
| Konak güç hatası | YES | Yedekli güç kaynağı |  |  |
| ESXi ana bilgisayar arızası | YES | Hatalı düğümü hızlı değiştirme | [Yüksek kullanılabilirlik VMware vSphere](https://www.vmware.com/products/vsphere/high-availability.html) |  |  |
| VM hatası | YES | [Yük dengeleyiciler](load-balancers.md)  | [Yüksek kullanılabilirlik VMware vSphere](https://www.vmware.com/products/vsphere/high-availability.html) | Durum bilgisiz VMware VM 'Leri için Azure Load Balancer |
| Yaprak anahtar bağlantı noktası hatası | YES | Yedekli NIC |  |  |
| Yaprak anahtar hatası | YES | Yedekli yaprak anahtarları |  |  |
| Raf hatası | YES | Yerleştirme grupları |  |  |
| Şirket içi DC 'ye ağ bağlantısı | YES  | Yedekli ağ hizmetleri |  | Yedekli ER devreleri |
| Azure ile ağ bağlantısı | YES | |  | Yedekli ER devreleri |
| Veri merkezi hatası | YES |  |  | Kullanılabilirlik alanları |
| Bölgesel hata | YES  |  |  | Azure bölgeleri |

AVS tarafından sağlanan Azure VMware çözümü, aşağıdaki yüksek kullanılabilirlik özelliklerini sağlar.

## <a name="fast-replacement-of-failed-node"></a>Hatalı düğümü hızlı değiştirme

AVS denetim düzlemi yazılımı, VMware kümelerinin sistem durumunu sürekli olarak izler ve bir ESXi düğümünün başarısız olduğunu algılar. Daha sonra, etkilenen VMware kümesine otomatik olarak kullanılabilir düğümler havuzundan yeni bir ESXi Konağı ekler ve başarısız olan düğümü kümeden alır. Bu işlevsellik, VMware kümesindeki yedek kapasitenin hızlı bir şekilde geri yüklenmesini sağlar, böylece küme, vSAN ve VMware HA tarafından sağlanmış olan dayanıklılık geri yüklenir.

## <a name="placement-groups"></a>Yerleştirme grupları

Bir AVS özel bulutu oluşturan bir Kullanıcı, seçili bölgede bir Azure bölgesi ve bir yerleştirme grubu seçebilir. Yerleştirme grubu, birden çok rafdaki, ancak aynı sırt ağı segmentinde yayılan bir düğüm kümesidir. Aynı yerleştirme grubu içindeki düğümler, en fazla iki ek anahtar atlaması ile birbirlerine erişebilir. Bir yerleştirme grubu her zaman tek bir Azure kullanılabilirlik bölgesi içinde bulunur ve birden çok raflara yayılır. AVS denetim düzlemi, bir AVS özel bulutunun düğümlerini, en iyi çabaya göre birden çok rafla dağıtır. Farklı yerleştirme gruplarındaki düğümlerin farklı raflara yerleştirilmesi garanti edilir.

## <a name="availability-zones"></a>Kullanılabilirlik alanları

Kullanılabilirlik alanları, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirliğe sahip bir tekliftir. Kullanılabilirlik alanları, bir Azure bölgesi içindeki özel fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Her bölgenin bir kullanılabilirlik alanı vardır. Daha fazla bilgi için bkz. [Azure 'da kullanılabilirlik alanları nedir?](../availability-zones/az-overview.md).

## <a name="redundant-azure-expressroute-circuits"></a>Gereksiz Azure ExpressRoute devreleri

ExpressRoute kullanarak Azure vNet 'e veri merkezi bağlantısı, yüksek oranda kullanılabilir ağ bağlantısı bağlantısı sağlamak için yedekli devrelere sahiptir.

## <a name="redundant-networking-services"></a>Yedekli ağ hizmetleri

AVS özel bulutu (VLAN, güvenlik duvarı, genel IP adresleri, Internet ve VPN dahil) için tüm AVS ağ hizmetleri yüksek kullanılabilirliğe sahip olacak şekilde tasarlanmıştır ve hizmet SLA 'sını destekleyebilir.

## <a name="azure-layer-7-load-balancer-for-stateless-vmware-vms"></a>Durum bilgisiz VMware VM 'Leri için Azure katman 7 Load Balancer

Kullanıcılar, Web katmanı için yüksek kullanılabilirlik elde etmek üzere VMware ortamında çalışan durum bilgisiz Web katmanı VM 'lerinin önüne bir Azure katman 7 Load Balancer yerleştirebilir.

## <a name="azure-regions"></a>Azure bölgeleri

Azure bölgesi, gecikme süresi belirlenmiş bir çevre içinde dağıtılan ve adanmış bölgesel düşük gecikmeli bir ağ ile bağlanan bir veri merkezleri kümesidir. Ayrıntılar için bkz. [Azure bölgeleri](https://azure.microsoft.com/global-infrastructure/regions).
