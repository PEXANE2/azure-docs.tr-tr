---
title: Ayrılmış kapasiteye sahip Azure App Service yalıtılmış damga ücreti için ön ödeme
description: Para tasarrufu sağlamak için ayrılmış kapasiteye sahip Azure App Service yalıtılmış damga ücreti için ön ödeme yapma hakkında bilgi edinin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 08/02/2019
ms.author: banders
ms.openlocfilehash: f122ec2474c09cdd6c9ada4ddc59b1adb44f619f
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780023"
---
# <a name="prepay-for-azure-app-service-isolated-stamp-fee-with-reserved-capacity"></a>Ayrılmış kapasiteye sahip Azure App Service yalıtılmış damga ücreti için ön ödeme

Üç yıllık bir süre için damga kullanımınız için ön ödeme yaparak Azure App Service yalıtılmış damga ücretlerine tasarruf edebilirsiniz. Yalıtılmış damga ücreti ayrılmış kapasitesini satın almak için, damgasının dağıtılacağı Azure bölgesini ve satın alınacak damgalar sayısını seçmeniz gerekir.

Bir rezervasyon satın aldığınızda, rezervasyon öznitelikleriyle eşleşen yalıtılmış damga ücreti kullanımı artık Kullandıkça Öde tarifesine göre ücretlendirilir. Rezervasyon, ayrılmış kapasite kapsamı ve bölgesiyle eşleşen yalıtılmış damgalar sayısına otomatik olarak uygulanır. Yalıtılmış bir damgaya bir ayırma atamanız gerekmez. Rezervasyon, çalışanlara uygulanmaz, bu nedenle damgasıyla ilişkili diğer tüm kaynaklar ayrı olarak ücretlendirilir.

Ayrılan kapasitenin süresi dolarsa, yalıtılmış damgalar çalışmaya devam eder, ancak Kullandıkça Öde fiyatı üzerinden faturalandırılır. Ayırmalar otomatik olarak yenilenmez.

## <a name="determine-the-right-reservation-to-purchase"></a>Satın almak için doğru ayırmayı belirleme

Bir rezervasyon satın alarak, ayrılmış miktarları sonraki üç yılda bir süre içinde kullanmak için ön ödeme yaparsınız. Düzenli olarak kullandığınız ve gelecekte kullanabileceği App Service Yalıtılmış damgalarının sayısını öğrenmek için kullanım verilerinizi denetleyin.

Ayrıca, yalıtılmış damgasının Linux veya Windows ölçümü yaydığını anladığınızdan emin olun.

- Varsayılan olarak, boş bir yalıtılmış damga Windows damga ölçeri yayar. Örneğin, hiçbir çalışan dağıtılmamışsa. Windows çalışanları damgada dağıtıldığında bu ölçümü yayma devam eder.
- Bir Linux çalışanı dağıtırsanız, ölçüm Linux damga ölçerde değişir.
- Hem Linux hem de Windows çalışanlarının dağıtıldığı durumlarda damga, Windows ölçeri yayar.

Bu nedenle, damga kullanım ömrü boyunca damga ölçümü Windows ve Linux arasında değişebilir.

Damgada bir veya daha fazla Windows çalışananız varsa Windows damga ayırmaları satın alın. Yalnızca damgada Linux çalışanları olmasını planlıyorsanız, bir Linux damga ayırması satın almanız gereken tek zaman.

## <a name="buy-isolated-stamp-reserved-capacity"></a>Yalıtılmış damga ayrılmış kapasitesini satın al

Yalıtılmış damga ayrılmış kapasitesini [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22AppService%22%7D)satın alabilirsiniz. Ayrılan kapasiteyi satın almak için, en az bir kurumsal abonelik veya Kullandıkça Öde tarifesine sahip tek bir abonelik için sahip rolüne sahip olmanız gerekir.

- Kurumsal abonelikler için, **ayrılmış örnekler Ekle** seçeneğinin [EA portalında](https://ea.azure.com/)etkinleştirilmiş olması gerekir. Ya da ayar devre dışıysa, bir EA yöneticisi olmanız gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için, yalnızca yönetici aracıları veya satış aracıları SQL veri ambarı ayrılmış kapasitesini satın alabilir.

**Satın almak için:**

1. [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/CreateBlade/referrer/documentation/filters/%7B%22reservedResourceType%22%3A%22AppService%22%7D)gidin.
1. Abonelik seçin. Ayrılmış kapasite için ödeme yapmak üzere kullanılan aboneliği seçmek için **abonelik** listesini kullanın. Aboneliğin ödeme yöntemi, ayrılmış kapasitenin ön maliyetlerine göre ücretlendirilir. Abonelik türü bir kurumsal anlaşma olmalıdır (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Kullandıkça Öde (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P) veya CSP aboneliği.
    - Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir.
    - Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.
1. Abonelik kapsamı seçmek için bir **kapsam** seçin.
    - **Tek kaynak grubu kapsamı** — yalnızca seçili kaynak grubundaki eşleşen kaynaklara rezervasyon iskontosunu uygular.
    - **Tek abonelik kapsamı** — seçili abonelikteki eşleşen kaynaklara rezervasyon iskontosunu uygular.
    - **Paylaşılan kapsam** — fatura bağlamındaki uygun aboneliklerde bulunan kaynaklara karşılık gelen rezervasyon iskontosunu uygular. Kurumsal Anlaşma müşteriler için, faturalandırma bağlamı kayıt olur. Kullandıkça Öde tarifelerine sahip bireysel abonelikler için faturalandırma kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.
1. Ayrılmış kapasite kapsamındaki bir Azure bölgesi seçmek için bir **bölge** seçin ve ayırmayı sepete ekleyin.
1. Yalıtılmış bir plan türü seçin ve ardından **Seç**' e tıklayın.  
    ![Örneğinde](./media/billing-prepay-app-service-isolated-stamp/app-service-isolated-stamp-select.png)
1. Ayrılacak App Service Yalıtılmış damgaları sayısını girin. Örneğin, üç adet bir bölge, size üç ayrılmış damga verebilir. İleri **' ye tıklayın: Gözden geçir +** satın al.
1. Gözden geçirin ve **Şimdi satın al**' a tıklayın.

Satın aldıktan sonra, satın alma durumunu görüntülemek ve istediğiniz zaman izlemek için [rezervasyonlar](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) ' a gidin.

## <a name="cancel-exchange-or-refund-reservations"></a>İptal, Exchange veya para iadesi rezervasyonları

Belirli sınırlamalara sahip rezervasyonları iptal edebilir, Exchange veya para iadesi yapabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](billing-azure-reservations-self-service-exchange-and-refund.md).

## <a name="discount-application-shown-in-usage-data"></a>Kullanım verilerinde gösterilen indirim uygulaması

Kullanım verilerinizde, bir ayırma indirimi alan kullanım için geçerli bir sıfır fiyatı vardır. Kullanım verileri her rezervasyondaki her damga örneği için rezervasyon iskontosunu gösterir.

Rezervasyon iskontosunun kullanım verilerini nasıl gösterdiği hakkında daha fazla bilgi için, bkz. [Kurumsal Anlaşma rezervasyon maliyetlerini ve kullanımını alma](billing-understand-reserved-instance-usage-ea.md) kurumsal anlaşma (EA) müşterisiyseniz. Aksi takdirde, [Kullandıkça Öde tarifelerine sahip bireysel aboneliğiniz Için Azure rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md).

## <a name="next-steps"></a>Sonraki adımlar

- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
  - [Azure App Service yalıtılmış bir damga rezervasyon iskontosunun nasıl uygulandığını anlayın](billing-reservation-discount-app-service-isolated-stamp.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)
