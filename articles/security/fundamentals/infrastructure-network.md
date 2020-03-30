---
title: Azure ağ mimarisi
description: Bu makalede, Microsoft Azure altyapı ağının genel bir açıklaması verilmektedir.
services: security
documentationcenter: na
author: TerryLanfear
manager: barbkess
editor: TomSh
ms.assetid: 61e95a87-39c5-48f5-aee6-6f90ddcd336e
ms.service: security
ms.subservice: security-fundamentals
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 02/20/2019
ms.author: terrylan
ms.openlocfilehash: c4756c36c2243840df69f3696e7ddac3628f3a00
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68727168"
---
# <a name="azure-network-architecture"></a>Azure ağ mimarisi
Azure ağ mimarisi, farklı donanım katmanlarına sahip endüstri standardı çekirdek/dağıtım/erişim modelinin değiştirilmiş bir sürümünü izler. Katmanlar şunlardır:

- Çekirdek (veri merkezi yönlendiricileri)
- Dağıtım (erişim yönlendiricileri ve L2 toplama). Dağıtım katmanı L3 yönlendirmesini L2 anahtarlamadan ayırır.
- Erişim (L2 ana bilgisayar anahtarları)

Ağ mimarisi, katman 2 anahtarları iki düzeyvardır. Bir katman trafiği diğer katmandan toplar. Artıklık dahil etmek için ikinci katman döngüleri. Mimari daha esnek bir VLAN ayak izi sağlar ve bağlantı noktası ölçekleme geliştirir. Mimari, ağdaki farklı katmanların her birinde donanım kullanımına izin veren ve bir katmandaki hatayı diğer katman(lar)ı etkileyen en aza indiren L2 ve L3'ü farklı tutar. Gövdelerin kullanımı, L3 altyapısına bağlantı gibi kaynak paylaşımına olanak tanır.

## <a name="network-configuration"></a>Ağ yapılandırması
Veri merkezi içindeki bir Azure kümesinin ağ mimarisi aşağıdaki aygıtlardan oluşur:

- Yönlendiriciler (veri merkezi, erişim yönlendiricisi ve kenarlık yaprağı yönlendiricileri)
- Anahtarlar (toplama ve rafüstü anahtarlar)
- Digi CMs
- Güç dağıtım üniteleri

Azure'un iki ayrı mimarisi vardır. Bazı mevcut Azure müşterileri ve paylaşılan hizmetler varsayılan LAN mimarisinde (DLA) yer ederken, yeni bölgeler ve sanal müşteriler Quantum 10 (Q10) mimarisinde yer alır. DLA mimarisi, erişim yönlendiricilerine etkin/pasif erişim yönlendiricileri ve güvenlik erişim denetim listeleri (ALA) uygulanan geleneksel bir ağaç tasarımıdır. Quantum 10 mimarisi, yönlendiricilere ATL uygulanmayan bir Yakın/kafes tasarımıdır. Bunun yerine, ALAK'lar, Yazılım Yük Dengeleme (SLB) veya yazılım tanımlı VLAN'lar aracılığıyla yönlendirmenin altında uygulanır.

Aşağıdaki diyagram, Bir Azure kümesi içindeki ağ mimarisine üst düzey bir genel bakış sağlar:

![Azure ağının diyagramı](./media/infrastructure-network/network-arch.png)

### <a name="quantum-10-devices"></a>Quantum 10 cihazları
Quantum 10 tasarımı, Clos/mesh tasarımında birden fazla cihaza yayılmış katman 3 anahtarlama yapar. Q10 tasarımının avantajları arasında daha büyük yetenek ve varolan ağ altyapısını ölçeklendirme yeteneği daha fazla dır. Tasarım, hata toleransı sağlayan, birden çok rota boyunca kümelere trafik geçmek için sınır yaprağı yönlendiriciler, omurga anahtarları ve üst raf yönlendiriciler kullanır. Yazılım yükü dengeleme, donanım aygıtları yerine, ağ adresi çevirisi gibi güvenlik hizmetlerini işler.

### <a name="access-routers"></a>Erişim yönlendiricileri
Dağıtım/erişim L3 yönlendiricileri (ARs) dağıtım ve erişim katmanları için birincil yönlendirme işlevini gerçekleştirir. Bu aygıtlar bir çift olarak dağıtılır ve alt ağlar için varsayılan ağ geçididir. Her AR çifti, kapasiteye bağlı olarak birden fazla L2 toplama anahtarı çiftini destekleyebilir. Maksimum sayı, aygıtın kapasitesine ve arıza etki alanlarına bağlıdır. Tipik bir sayı AR çifti başına üç L2 toplama anahtarı çiftidir.

### <a name="l2-aggregation-switches"></a>L2 toplama anahtarları  
Bu aygıtlar L2 trafiği için bir toplama noktası olarak hizmet vermektedir. Onlar L2 kumaş için dağıtım katmanı, ve trafik büyük miktarda işleyebilir. Bu aygıtlar trafiği toplamdığından, 802.1q işlevselliği ve bağlantı noktası toplama ve 10GE gibi yüksek bant genişliği teknolojileri gerektirir.

### <a name="l2-host-switches"></a>L2 ana bilgisayar anahtarları
Ana bilgisayarlar bu anahtarlara doğrudan bağlanır. Bunlar rafa monte edilmiş anahtarlar veya şasi dağıtımları olabilir. 802.1q standart bir VLAN yerli VLAN olarak atanması için izin verir, normal olarak bu VLAN tedavi (etiketsiz) Ethernet çerçeveleme. Normal şartlar altında, yerli VLAN çerçeveleri iletilir ve 802.1q gövde bağlantı noktasında etiketsiz alınır. Bu özellik 802.1q'a geçiş ve 802.1q olmayan cihazlarla uyumluluk için tasarlanmıştır. Bu mimaride, yalnızca ağ altyapısı yerel VLAN kullanır.

Bu mimari, yerel VLAN seçimi için bir standart belirtir. Standart, mümkünolduğunda, AR cihazlarının her gövde için benzersiz, yerli vlan'a ve L2Aggregation gövdelerine L2Aggregation'a sahip olmasını sağlar. L2Host Switch gövdeleri L2Aggregation olmayan bir varsayılan yerli VLAN var.

### <a name="link-aggregation-8023ad"></a>Bağlantı toplama (802.3ad)
Bağlantı toplama, birden çok tek bağlantıların bir araya getirilmesine ve tek bir mantıksal bağlantı olarak ele alınmasına olanak tanır. Operasyonel hata ayıklamayı kolaylaştırmak için, bağlantı noktası kanal arabirimlerini belirlemek için kullanılan numara standartlaştırılmalıdır. Ağın geri kalanı, bir bağlantı noktası kanalının her iki ucunda da aynı numarayı kullanır.

L2Agg-L2Host anahtarı için belirtilen numaralar, L2Agg tarafında kullanılan bağlantı noktası kanalı numaralarıdır. Sayıların aralığı L2Host tarafında daha sınırlı olduğundan, standart L2Host tarafında 1 ve 2 numaralarını kullanmaktır. Bunlar, sırasıyla "a" tarafına ve "b" tarafına giden bağlantı noktası kanalına atıfta bulunur.

### <a name="vlans"></a>Vlan
Ağ mimarisi, sunucuları tek bir yayın etki alanında gruplandırmak için VN'leri kullanır. VLAN numaraları 802.1q standardına uygundur ve 1-4094 numaralı VLAN'ları destekler.

### <a name="customer-vlans"></a>Müşteri VLANs
Çözümünüzün ayırma ve mimari gereksinimlerini karşılamak için Azure portalı üzerinden dağıtabileceğiniz çeşitli VLAN uygulama seçenekleriniz vardır. Bu çözümleri sanal makineler aracılığıyla dağıtın. Müşteri başvuru mimarisi örnekleri için Azure [başvuru mimarilerine](https://docs.microsoft.com/azure/architecture/reference-architectures/)bakın.

### <a name="edge-architecture"></a>Kenar mimarisi
Azure veri merkezleri, son derece gereksiz ve iyi sağlanan ağ altyapıları üzerine kurulur. Microsoft, Azure veri merkezleri içindeki ağları "gerek artı bir" (N+1) artıklık mimarisi veya daha iyisi olan olarak uygular. Veri merkezleri içinde ve arasında tam başarısız özellikler ağ ve hizmet kullanılabilirliğini sağlamaya yardımcı olur. Harici olarak, veri merkezleri özel, yüksek bant genişliğiağ devreleri tarafından sunulmaktadır. Bu devreler, dünya çapında 1200'den fazla internet servis sağlayıcısıyla özellikleri birden fazla noktada gereksiz bir şekilde birbirine bağlar. Bu ağ genelinde 2.000 Gbps'den fazla potansiyel kenar kapasitesi sağlar.

Azure ağının kenarındaki ve erişim katmanındaki yönlendiricileri filtrelemek, paket düzeyinde iyi kurulmuş güvenlik sağlar ve Azure'a yetkisiz bağlanma girişimlerini önlemeye yardımcı olur. Yönlendiriciler, paketlerin gerçek içeriğinin beklenen biçimde veri içerdiğinden ve beklenen istemci/sunucu iletişim düzenine uygun olduğundan emin olmaya yardımcı olur. Azure, aşağıdaki ağ ayrımı ve erişim denetimi bileşenlerinden oluşan katmanlı bir mimari uygular:

- **Kenar yönlendiriciler.** Bunlar internetten uygulama ortamını ayırır. Kenar yönlendiriciler, sızdırmazlık koruması sağlamak ve ALA'lar kullanarak erişimi sınırlamak için tasarlanmıştır.
- **Dağıtım (erişim) yönlendiricileri.** Bunlar yalnızca Microsoft onaylı IP adreslerine izin verir, sızdırma önleme sağlar ve ALA'lar kullanarak bağlantılar kurar.

### <a name="ddos-mitigation"></a>DDOS azaltma
Dağıtılmış hizmet reddi (DDoS) saldırıları, çevrimiçi hizmetlerin güvenilirliği için gerçek bir tehdit oluşturmaya devam etmektedir. Saldırılar daha hedefli ve karmaşık hale geldikçe ve Microsoft'un sağladığı hizmetler coğrafi olarak daha çeşitli hale geldikçe, bu saldırıların etkisini belirlemek ve en aza indirmek yüksek bir önceliktir.

[Azure DDoS Koruma Standardı, DDoS](../../virtual-network/ddos-protection-overview.md) saldırılarına karşı savunma sağlar. Bkz. [Azure DDoS Koruması: Daha](ddos-best-practices.md) fazla bilgi edinmek için en iyi uygulamalar ve başvuru mimarileri.

> [!NOTE]
> Microsoft, varsayılan olarak tüm Azure müşterileri için DDoS koruması sağlar.
>
>

## <a name="network-connection-rules"></a>Ağ bağlantısı kuralları
Azure, ağında, Azure'a yetkisiz bağlanma girişimlerini önlemek için paket düzeyinde güvenlik sağlayan kenar yönlendiriciler dağıtıyor. Kenar yönlendiricileri, paketlerin gerçek içeriğinin beklenen biçimde veri içermesini ve beklenen istemci/sunucu iletişim düzenine uygun olmasını sağlar.

Kenar yönlendiriciler internetten uygulama ortamını ayırır. Bu yönlendiriciler, sızdırmazlık koruması sağlamak ve ALA'lar kullanarak erişimi sınırlamak için tasarlanmıştır. Microsoft, kenar yönlendiricilerini ve yönlendiricilere erişmesine izin verilen ağ protokollerini sınırlamak için katmanlı bir ACL yaklaşımı kullanarak kenar yönlendiricilerini yapılandırır.

Microsoft, giriş veya çıkış filtrelerinin uygulandığı sınır noktaları olarak hareket etmek üzere ağ aygıtlarını erişim ve kenar konumlarında konumlandırıyor.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısının güvenliğini sağlamaya yardımcı olmak için neler yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure tesisleri, tesisler ve fiziksel güvenlik](physical-security.md)
- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)


