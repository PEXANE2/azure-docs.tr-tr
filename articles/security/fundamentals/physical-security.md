---
title: Azure veri merkezlerinin fiziksel güvenliği - Microsoft Azure | Microsoft Dokümanlar
description: Makalede, Microsoft'un fiziksel altyapı, güvenlik ve uyumluluk teklifleri de dahil olmak üzere Azure veri merkezlerini güvence altına almak için ne yaptığı açıklanmaktadır.
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
ms.date: 04/28/2019
ms.author: terrylan
ms.openlocfilehash: 634f89b3123902d981ad07dd1404315387104322
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68726736"
---
# <a name="azure-facilities-premises-and-physical-security"></a>Azure tesisleri, tesisler ve fiziksel güvenlik
Bu makalede, Microsoft'un Azure altyapısını güvence altına almak için ne yaptığı açıklanmaktadır.

## <a name="datacenter-infrastructure"></a>Datacenter altyapısı
Azure, binlerce çevrimiçi hizmeti destekleyen ve dünya çapında 100'den fazla yüksek güvenlikli tesisi [kapsayan, küresel olarak dağıtılan veri merkezi altyapısından](https://azure.microsoft.com/global-infrastructure/)oluşur.

Altyapı, uygulamaları dünyanın dört bir yanındaki kullanıcılara daha da yakınlaştırmak, veri ikametini korumak ve müşteriler için kapsamlı uyumluluk ve esneklik seçenekleri sunmak üzere tasarlanmıştır. Azure'un dünya çapında 52 bölgesi vardır ve 140 ülkede/bölgede kullanılabilir.

Bölge, büyük ve esnek bir ağ üzerinden birbirine bağlı bir veri merkezi kümesidir. Ağ varsayılan olarak içerik dağıtımı, yük dengeleme, artıklık ve şifreleme içerir. Azure, diğer bulut sağlayıcılarından daha fazla küresel bölgeye sahip olarak, size gereken uygulamaları dağıtma esnekliği sağlar.

Azure bölgeleri coğrafyalar halinde düzenlenir. Azure coğrafyası, veri ikameti, egemenlik, uyumluluk ve esneklik gereksinimlerinin coğrafi sınırlar içinde yerine getirilmesini sağlar.

Coğrafyalar, özel veri yerleşikliği ve uyumluluk gereksinimleri olan müşterilerin verileri ile uygulamalarını yakın tutmasına olanak tanır. Coğrafyalar, özel, yüksek kapasiteli ağ altyapısına olan bağlantıları sayesinde tam bir bölge arızasına dayanacak şekilde hataya dayanıklıdır.

Kullanılabilirlik bölgeleri, Azure bölgesindeki fiziksel olarak ayrı konumlardır. Her kullanılabilirlik alanı, bağımsız enerji, soğutma ve ağ kaynaklarıyla donatılmış bir veya daha fazla veri merkezinden oluşur. Kullanılabilirlik bölgeleri, yüksek kullanılabilirlik ve düşük gecikme süresi çoğaltma ile görev açısından kritik uygulamaları çalıştırmanızı sağlar.

Aşağıdaki şekil, Azure global altyapısının yüksek kullanılabilirlik, olağanüstü durum kurtarma ve yedekleme için aynı veri ikamet sınırı içindeki bölge ve kullanılabilirlik bölgelerini nasıl eşleştirdiği gösterilmektedir.

![Veri ikamet sınırını gösteren diyagram](./media/physical-security/data-residency-boundary.png)

Coğrafi olarak dağıtılmış veri merkezleri Microsoft'un müşterilere yakın olmasını, ağ gecikmesini azaltmasını ve coğrafi yedekli yedekleme ve başarısızlığa izin etmesini sağlar.

## <a name="physical-security"></a>Fiziksel güvenlik
Microsoft, verilerinizin depolandığı alanlara fiziksel erişimi sıkı bir şekilde denetler, oluşturur ve çalıştırZ. Microsoft, verilerinizi korumanın önemini anlıyor ve verilerinizi içeren veri merkezlerinin güvenliğini sağlamaya yardımcı olmaya kendini adamıştır. Microsoft'ta Azure'u destekleyen fiziksel tesisleri tasarlamaya, oluşturmaya ve çalıştırmaya adanmış bir bölümüz var. Bu ekip son teknoloji fiziksel güvenliği korumaya yatırım yapar.

Microsoft, yetkisiz kullanıcıların verilere ve veri merkezi kaynaklarına fiziksel erişim kazanma riskini azaltmak için fiziksel güvenliğe katmanlı bir yaklaşım benimser. Microsoft tarafından yönetilen veri merkezleri geniş koruma katmanlarına sahiptir: tesisin çevresinde, binanın çevresinde, binanın içinde ve veri merkezi zemininde erişim onayı. Fiziksel güvenlik katmanları şunlardır:

- **Erişim isteği ve onayı.** Veri merkezine gelmeden önce erişim istemeniz gerekir. Ziyaretiniz için uyumluluk veya denetim amaçları gibi geçerli bir iş gerekçesi sağlamanız gerekir. Tüm istekler, Microsoft çalışanları tarafından erişim emareleri temelinde onaylanır. Erişim gereksinimi temeli, veri merkezlerinde bir görevi tamamlamak için gereken kişi sayısının en aza inmesine yardımcı olur. Microsoft izin verdikten sonra, bir birey yalnızca onaylanan iş gerekçesine bağlı olarak gerekli veri merkezinin ayrık alanına erişebilir. İzinler belirli bir süreyle sınırlıdır ve sonra süresi doluyor.

- **Tesisin çevresi.** Bir veri merkezine vardığınızda, iyi tanımlanmış bir erişim noktasından geçmeniz gerekir. Tipik olarak, çelik ve betondan yapılmış uzun çitler çevrenin her santimini kapsar. Veri merkezlerinin etrafında kameralar var, bir güvenlik ekibi her zaman videolarını izliyor.

- **Bina girişi.** Veri merkezi girişi, sıkı eğitim ve arka plan kontrollerinden geçmiş profesyonel güvenlik görevlileri ile görevlidir. Bu güvenlik görevlileri de rutin veri merkezi devriye ve her zaman veri merkezi içinde kameraların videoları izlemek.

- **Binanın içinde.** Binaya girdikten sonra, veri merkezinde niçin hareket etmeye devam etmek için biyometriyle iki faktörlü kimlik doğrulamasını geçirmeniz gerekir. Kimliğiniz doğrulanırsa, veri merkezinin yalnızca erişimi onayladığınız bölümünü girebilirsiniz. Orada sadece onaylanan süre boyunca kalabilirsiniz.

- **Datacenter katı.** Sadece girmeniz onaylanan kata girmenize izin verilir. Tam bir vücut metal algılama tarama geçmek için gereklidir. Bilgimiz olmadan veri merkezine yetkisiz veri girme veya ayrılma riskini azaltmak için, veri merkezi zeminine yalnızca onaylı aygıtlar girebilir. Ayrıca, video kameralar her sunucu rafının ön ve arka sını izler. Veri merkezi zemininden çıktığınızda, yine tam gövdeli metal algılama taramasından geçmeniz gerekir. Veri merkezini terk etmek için ek bir güvenlik teşlikinden geçmeniz gerekir.

Microsoft, ziyaretçilerin herhangi bir Microsoft tesisinden ayrıldıktan sonra rozetlerini teslim etmesini gerektirir.

## <a name="physical-security-reviews"></a>Fiziksel güvenlik incelemeleri
Veri merkezlerinin Azure güvenlik gereksinimlerini doğru şekilde karşılamasını sağlamak için düzenli olarak tesislerin fiziksel güvenlik incelemelerini yapıyoruz. Veri merkezi barındırma sağlayıcısı personeli Azure hizmet yönetimini sağlamaz. Personel Azure sistemlerinde oturum alamaz ve Azure'daki konumlandırma odasına ve kafeslere fiziksel erişimi yoktur.

## <a name="data-bearing-devices"></a>Veri taşıyan cihazlar
Microsoft en iyi uygulama yordamları ve [NIST 800-88 uyumlu](https://csrc.nist.gov/publications/detail/sp/800-88/archive/2006-09-01)bir silme çözümü kullanır. Silinemez sabit diskler için, onu yok eden ve bilginin kurtarılmasını imkansız hale getiren bir imha işlemi kullanırız. Bu imha işlemi parçalamak, parçalamak, toz haline getirmek veya yakmak olabilir. Elden çıkarma araçlarını varlık türüne göre belirleriz. Yıkımın kayıtlarını tutuyoruz.  

## <a name="equipment-disposal"></a>Ekipman imhası
Bir sistemin kullanım ömrünün sona ermesi üzerine, Microsoft operasyonel personeli, verilerinizi içeren donanımın güvenilmeyen taraflarca kullanıma sunulmamasını sağlamak için sıkı veri işleme ve donanım imha yordamlarını izler. Bunu destekleyen sabit diskler için güvenli silme yaklaşımı kullanıyoruz. Silinemez sabit diskler için, sürücüyü yok eden ve bilgilerin kurtarılmasını imkansız hale getiren bir imha işlemi kullanırız. Bu imha işlemi parçalamak, parçalamak, toz haline getirmek veya yakmak olabilir. Elden çıkarma araçlarını varlık türüne göre belirleriz. Yıkımın kayıtlarını tutuyoruz. Tüm Azure hizmetleri onaylı medya depolama ve imha yönetimi hizmetlerini kullanır.

## <a name="compliance"></a>Uyumluluk
Azure altyapısını, ISO 27001, HIPAA, FedRAMP, SOC 1 ve SOC 2 gibi uluslararası ve sektöre özel geniş bir uyumluluk standartlarını karşılayacak şekilde tasarlar ve yönetiriz. Ayrıca Avustralya IRAP, Uk G-Cloud ve Singapore MTCS gibi ülkeye veya bölgeye özgü standartları da karşılıyoruz. İngiliz Standartlar Enstitüsü tarafından yapılanlar gibi sıkı üçüncü taraf denetimleri, bu standartların gerektirdiği sıkı güvenlik denetimlerine uygunluğu doğrular.

Azure'un bağlı olduğu uyumluluk standartlarının tam listesi [için, Uyumluluk tekliflerine](https://www.microsoft.com/trustcenter/compliance/complianceofferings)bakın.

## <a name="next-steps"></a>Sonraki adımlar
Microsoft'un Azure altyapısının güvenliğini sağlamaya yardımcı olmak için neler yaptığı hakkında daha fazla bilgi edinmek için bkz:

- [Azure altyapısı kullanılabilirliği](infrastructure-availability.md)
- [Azure bilgi sistemi bileşenleri ve sınırları](infrastructure-components.md)
- [Azure ağ mimarisi](infrastructure-network.md)
- [Azure üretim ağı](production-network.md)
- [Azure SQL Veritabanı güvenlik özellikleri](infrastructure-sql.md)
- [Azure üretim işlemleri ve yönetimi](infrastructure-operations.md)
- [Azure altyapı izleme](infrastructure-monitoring.md)
- [Azure altyapı bütünlüğü](infrastructure-integrity.md)
- [Azure müşteri veri koruması](protection-customer-data.md)


