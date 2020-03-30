---
title: Azure'da VMware'de çalışırken uygulamanın yüksek kullanılabilirlik sağladığından emin olun
description: CloudSimple Private Cloud'da çalışan uygulamalar için yaygın uygulama hatası senaryolarını gidermek için CloudSimple yüksek kullanılabilirlik özelliklerini açıklar
author: sharaths-cs
ms.author: b-shsury
ms.date: 08/20/2019
ms.topic: article
ms.service: azure-vmware-cloudsimple
ms.reviewer: cynthn
manager: dikamath
ms.openlocfilehash: a3eed033ba6a1a6f9237116a53ec7751ae906fe4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77025359"
---
# <a name="ensure-application-high-availability-when-running-in-vmware-on-azure"></a>Azure'da VMware'de çalışırken uygulamanın yüksek kullanılabilirlik sağladığından emin olun

CloudSimple çözümü, Azure ortamında VMware'de çalışan uygulamalarınız için yüksek kullanılabilirlik sağlar. Aşağıdaki tabloda hata senaryoları ve ilişkili yüksek kullanılabilirlik özellikleri listelenmitir.

| Hata senaryosu | Uygulama korunaklı mı? | Platform HA özelliği | VMware HA özelliği | Azure HA özelliği |
------------ | ------------- | ------------ | ------------ | ------------- |
| Disk Hatası | EVET | Başarısız düğümün hızlı değiştirilmesi | [vSAN Varsayılan Depolama İlkesi Hakkında](https://docs.vmware.com/en/VMware-vSphere/6.7/com.vmware.vsphere.virtualsan.doc/GUID-C228168F-6807-4C2A-9D74-E584CAF49A2A.html) |
| Fan Arızası | EVET | Gereksiz fanlar, başarısız düğüm hızlı değiştirme |  |  |
| NIC Hatası | EVET | Gereksiz NIC, başarısız düğümün hızlı değiştirilmesi
| Ana Bilgisayar Güç Kesintisi | EVET | Yedek güç kaynağı |  |  |
| ESXi Ana Bilgisayar Hatası | EVET | başarısız düğüm hızlı değiştirme | [VMware vSphere Yüksek Kullanılabilirlik](https://www.vmware.com/products/vsphere/high-availability.html) |  |  |
| VM Hatası | EVET | [Yük dengeleyiciler](load-balancers.md)  | [VMware vSphere Yüksek Kullanılabilirlik](https://www.vmware.com/products/vsphere/high-availability.html) | Durum dışı VMware VM'ler için Azure Yük Dengeleyicisi |
| Yaprak Anahtarı Bağlantı Noktası Hatası | EVET | Yedek NIC |  |  |
| Yaprak Anahtarı Hatası | EVET | Yedek yaprak anahtarları |  |  |
| Raf Arızası | EVET | Yerleştirme grupları |  |  |
| Şirket içi DC'ye Ağ Bağlantısı | EVET  | Gereksiz ağ hizmetleri |  | YedekLI ER devreleri |
| Azure'a Ağ Bağlantısı | EVET | |  | YedekLI ER devreleri |
| Veri Merkezi Hatası | EVET |  |  | Kullanılabilirlik alanları |
| Bölgesel Başarısızlık | EVET  |  |  | Azure bölgeleri |

CloudSimple tarafından Azure VMware Solution aşağıdaki yüksek kullanılabilirlik özelliklerini sağlar.

## <a name="fast-replacement-of-failed-node"></a>Başarısız düğümün hızlı değiştirilmesi

CloudSimple kontrol düzlemi yazılımı, VMware kümelerinin durumunu sürekli olarak izler ve bir ESXi düğümü başarısız olduğunda algılar. Daha sonra, etkilenen VMware kümesine hazır düğüm havuzundan otomatik olarak yeni bir ESXi ana bilgisayarı ekler ve başarısız düğümü kümeden çıkarır. Bu işlevsellik, VMware kümesindeki yedek kapasitenin hızlı bir şekilde geri yüklenmesini sağlar, böylece vSAN ve VMware HA tarafından sağlanan kümenin esnekliği geri yüklenir.

## <a name="placement-groups"></a>Yerleştirme grupları

Özel Bulut oluşturan bir kullanıcı, seçilen bölge içinde bir Azure bölgesi ve bir yerleşim grubu seçebilir. Yerleşim grubu, birden çok rafa, ancak aynı omurga ağ segmentinde yayılmış düğümler kümesidir. Aynı yerleşim grubundaki düğümler en fazla iki ekstra anahtar atlama ile birbirlerine ulaşabilir. Bir yerleşim grubu her zaman tek bir Azure kullanılabilirlik bölgesindedir ve birden çok rafa yayılır. CloudSimple denetim düzlemi, en iyi çabayı temel alan bir Özel Bulut düğümlerini birden çok rafa dağıtır. Farklı yerleşim gruplarındaki düğümlerin farklı raflara yerleştirilmeleri garanti edilir.

## <a name="availability-zones"></a>Kullanılabilirlik alanları

Kullanılabilirlik bölgeleri, uygulamalarınızı ve verilerinizi veri merkezi hatalarından koruyan yüksek kullanılabilirlik teklifidir. Kullanılabilirlik bölgeleri, Azure bölgesindeki özel fiziksel konumlardır. Her alan bağımsız güç, soğutma ve ağ bağlantısı ile donatılmış bir veya daha fazla veri merkezinden oluşur. Her bölgenin bir kullanılabilirlik bölgesi vardır. Daha fazla bilgi için [Azure'daki Kullanılabilirlik Bölgeleri nelerdir?](../availability-zones/az-overview.md)

## <a name="redundant-azure-expressroute-circuits"></a>Yedek Azure ExpressRoute devreleri

ExpressRoute kullanarak Azure vNet'e veri merkezi bağlantısı, yüksek kullanılabilirağ bağlantısı sağlamak için gereksiz devrelere sahiptir.

## <a name="redundant-networking-services"></a>Gereksiz ağ hizmetleri

Özel Bulut için tüm CloudSimple ağ hizmetleri (VLAN, güvenlik duvarı, genel IP adresleri, Internet ve VPN dahil) son derece kullanılabilir ve hizmet SLA desteklemek mümkün olacak şekilde tasarlanmıştır.

## <a name="azure-layer-7-load-balancer-for-stateless-vmware-vms"></a>Stateless VMware VM'ler için Azure Katmanı 7 Yük Dengeleyicisi

Kullanıcılar, web katmanı için yüksek kullanılabilirlik elde etmek için VMware ortamında çalışan stateless web katmanı VM'lerin önüne bir Azure Katmanı 7 Yük Dengeleyicisi koyabilir.

## <a name="azure-regions"></a>Azure bölgeleri

Azure bölgesi, gecikme tanımlı bir çevre içinde dağıtılan ve özel bir bölgesel düşük gecikme ağı üzerinden bağlanan veri merkezleri kümesidir. Ayrıntılar için Azure [Bölgeleri'ne](https://azure.microsoft.com/global-infrastructure/regions)bakın.
