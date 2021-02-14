---
title: Azure App Service için rezervasyon indirimleri
description: Azure App Service Premium v3 örnekleri ve yalıtılmış damgalar için rezervasyon iskontolarının nasıl uygulanacağını öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.subservice: reservations
ms.topic: conceptual
ms.date: 02/12/2021
ms.author: banders
ms.openlocfilehash: c599c64ce4b22bbf7bece77602b22fef6629d07c
ms.sourcegitcommit: d4734bc680ea221ea80fdea67859d6d32241aefc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100369739"
---
# <a name="how-reservation-discounts-apply-to-azure-app-service-premium-v3-instances-and-isolated-stamps"></a>Azure App Service Premium v3 örnekleri ve yalıtılmış damgalar için rezervasyon iskontoları nasıl uygulanır

Bu makale, indirimlerin Azure App Service Premium v3 örneklerine ve yalıtılmış damgalara nasıl uygulanacağını anlamanıza yardımcı olur.

## <a name="how-reservation-discounts-apply-to-premium-v3-instances"></a>Rezervasyon iskontolarının Premium v3 örneklerine nasıl uygulandığı

Azure App Service Premium v3 ile ayrılmış bir örnek satın aldıktan sonra rezervasyon indirimi, rezervasyonun öznitelikleriyle ve miktarıyla eşleşen App Service örneklerine otomatik olarak uygulanır. Bir rezervasyon, Premium v3 örneklerinizin maliyetini içerir. 

### <a name="how-the-discount-is-applied-to-azure-app-service"></a>İndirimin Azure App Service nasıl uygulandığı 

Rezervasyon indirimi *kullanın-it-veya-kaybeder*. Bu nedenle, herhangi bir saat için eşleşen kaynaklarınız yoksa, o saate ait rezervasyon miktarını kaybedersiniz. Kullanılmayan ayrılmış saatleri devredemezsiniz.
Bir kaynağı kapattığınızda rezervasyon indirimi, belirtilen kapsamdaki başka bir eşleşen kaynağa otomatik olarak uygulanır. Belirtilen kapsamda eşleşen kaynak bulunamazsa, ayrılan saatler kaybedilir.

### <a name="reservation-discount-for-premium-v3-instances"></a>Premium v3 örnekleri için rezervasyon indirimi

Azure rezervasyon indirimi, Premium v3 örneklerinin saatlik olarak çalıştırılması için geçerlidir. Satın aldığınız ayırmalar, çalışan Premium v3 örnekleri tarafından sunulan kullanım ile eşleştirilir ve rezervasyon iskontosunu uygular. Tam saati çalıştırmayan Premium v3 örnekleri için, ayırma eşzamanlı çalışan örnekler de dahil olmak üzere ayırma kullanmayan diğer örneklerden doldurulacak. Saatin sonunda, saat içindeki örneklerin rezervasyon uygulaması kilitlenir. Bir örnek bir saat veya saat içindeki eşzamanlı örnekler ayırmanın saatini doldurmadığında, bu saat için rezervasyon daha az kullanılır. Aşağıdaki grafikte rezervasyonun faturalanabilir VM kullanımına uygulanması gösterilir. Çizimde bir rezervasyon satın alımı ve iki eşleşen VM örneği temel alınmıştır.

![Faturalandırılabilir VM kullanımına ayırmanın uygulamasını gösteren resim](./media/reservation-discount-app-service/reserved-premium-v3-instance-application.png)

1.  Rezervasyon çizgisinin üstündeki kullanımlar normal kullandıkça öde fiyatlarından ücretlendirilir. Rezervasyon çizgisinin altındaki kullanımlardan ücretlendirilmezsiniz çünkü rezervasyon alışverişi kapsamında zaten ödenmiştir.
2.  1\. saatte 1. örnek 0,75 saat ve 2. örnek 0,5 saat çalışıyor. 1\. saat için toplam kullanım 1,25 saattir. Artan 0,25 saat için kullandıkça öde fiyatlarından ücretlendirilirsiniz.
3.  2\. saat ve 3. saat için her iki örnek de 1 saat çalışıyor. Örneklerden biri rezervasyonun kapsamına girer ve diğeri kullandıkça öde fiyatlarından ücretlendirilir.
4.  4\. saatte 1. örnek 0,5 saat ve 2. örnek 1 saat çalışıyor. 1\. örnek tümüyle rezervasyon kapsamındadır ve 2. örneğin 0,5 saatlik kısmı kapsama gider. Artan 0,5 saat için kullandıkça öde fiyatından ücretlendirilirsiniz.

Faturalama kullanım raporlarında Azure rezervasyonlarınızın uygulanmasını anlamak ve görüntülemek için bkz. [Rezervasyon kullanımınızı anlama](understand-reserved-instance-usage-ea.md).

## <a name="how-reservation-discounts-apply-to-isolated-stamps"></a>Rezervasyon iskontolarının yalıtılmış damgalar için nasıl uygulandığı

App Service Yalıtılmış Damga Pulu Ücreti ayrılmış kapasitesini satın almanızın ardından, bir bölgedeki Damga Pulu Ücretine otomatik olarak rezervasyon indirimi uygulanır. Rezervasyon indirimi, Yalıtılmış Damga Pulu Ücreti ölçümü tarafından gösterilen kullanıma uygulanır. Çalışanlar, ek Ön Uçlar ve damga puluyla ilişkili diğer tüm kaynaklar normal fiyatla faturalandırılmaya devam eder.

### <a name="reservation-discount-application"></a>Rezervasyon indirimi uygulama

App Service Yalıtılmış Damga Pulu Ücreti indirimi, çalıştırılan damga pullarına saat bazında uygulanır. Bir saat için dağıtılmış bir damga pulunuz yoksa, o saat için ayrılmış kapasite boşa gider. Devredilmez.

Satın alma işleminden sonra, satın aldığınız rezervasyon, belirli bir bölgede çalıştırılan yalıtılmış bir damga puluyla eşleştirilir. Bu damga pulunu kapatırsanız, bölgede çalıştırılan diğer tüm damga pullarına otomatik olarak rezervasyon indirimleri uygulanır. Bir damga pulu yoksa, bölgede oluşturulan sonraki damgaya rezervasyon uygulanır.

Damga pulları tam bir saat boyunca çalışmadığında aynı saat boyunca aynı bölgedeki diğer eşleşen damga pullarına otomatik olarak rezervasyon uygulanır.

### <a name="choose-a-stamp-type---windows-or-linux"></a>Damga pulu türü seçme - Windows veya Linux

Boş bir Yalıtılmış Damga Pulu varsayılan olarak Windows damga pulu ölçümünü gösterir. Örneğin, herhangi bir çalışan dağıtılmadığında. Windows çalışanları dağıtıldığında ölçümü göstermeye devam eder. Linux çalışanı dağıtırsanız ölçüm, Linux damga pulu ölçümüne değişir. Damga pulu, hem Linux hem de Windows çalışanları dağıtıldığında Windows ölçümünü gösterir.

Sonuç olarak damga pulu ölçümü, damga pulunun ömrü boyunca Windows ve Linux arasında değişebilir. Bu arada rezervasyonlar işletim sistemine özgüdür. Damga pulunu dağıtmayı planladığınız çalışanları destekleyen bir rezervasyon satın almanız gerekir. Yalnızca Windows damga pulları ve karma damga pulları, Windows rezervasyonunu kullanır. Yalnızca Linux çalışanları olan damga pulları, Linux rezervasyonunu kullanır.

Yalnızca damga pulunda _sırf_ Linux çalışanları olmasını planladığınızda Linux rezervasyonu satın almanız gerekir.

### <a name="discount-examples"></a>İndirim örnekleri

Aşağıdaki örneklerde, dağıtıma bağlı olarak Yalıtılmış Damga Pulu Ücreti ayrılmış örneğinin nasıl uygulandığı gösterilmektedir.

- **Örnek 1**: App Service Yalıtılmış damga pulları olmayan bir bölgede tek bir Yalıtılmış Ayrılmış Damga Pulu kapasitesi örneği satın alırsınız. Bölgede yeni bir damga dağıtır ve bu damga için ayrılmış ücretler ödersiniz.
- **Örnek 2**: App Service Yalıtılmış damga pulu önceden dağıtılmış olan bir bölgede tek bir Yalıtılmış Ayrılmış Damga Pulu kapasitesi örneği satın alırsınız. Dağıtılan damga pulu için ayrılmış ücret almaya başlarsınız.
- **Örnek 3**: App Service Yalıtılmış damga pulu önceden dağıtılmış olan bir bölgede tek bir Yalıtılmış Ayrılmış Damga Pulu kapasitesi örneği satın alırsınız. Dağıtılan damga pulu için ayrılmış ücret almaya başlarsınız. Daha sonra damga pulunu silip yeni birini dağıtırsınız. Yeni damga pulu için ayrılmış ücret alırsınız. Dağıtılan damga pulları olmayan süreler için indirimler devredilmez.
- **Örnek 4**: Bir bölgede tek bir Yalıtılmış Linux Ayrılmış Damga Pulu kapasitesi satın alır ve sonra bu bölgeye yeni bir damga pulu dağıtırsınız. Damga pulu başlangıçta çalışanlar olmadan dağıtıldığında, Windows damga pulu ölçümünü gösterir. Bir indirim alınmaz. Damga puluna ilk Linux çalışanı dağıtıldığında, Linux Damga Pulu ölçümünü gösterir ve rezervasyon indirimi uygulanır. Daha sonra damga puluna bir Windows çalışanı dağıtılırsa, damga pulu ölçümü Windows’a geri döndürülür. Yalıtılmış Linux Ayrılmış Damga Pulu rezervasyonu için artık indirim almazsınız.

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyonu yönetme hakkında bilgi edinmek için bkz. [Azure Ayrılmış Sanal Makine Örnekleri’ni Yönetme](manage-reserved-vm-instance.md).
- Para tasarrufu için ön satın alma App Service Premium v3 ve yalıtılmış damga ayrılmış kapasitesi hakkında daha fazla bilgi edinmek için bkz. [ayrılmış kapasiteye sahip Azure App Service Için ön ödeme](prepay-app-service.md).
- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
  - [Azure’da Rezervasyonları Yönetme](manage-reserved-vm-instance.md)
  - [Kullandıkça öde fiyatlarına tabi bir abonelik için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
