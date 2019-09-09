---
title: Azure ayrılmış kapasitesi ile SQL veri ambarı ücretleri maliyetlerini kaydetme
description: Para tasarrufu sağlamak için ayrılmış kapasiteye sahip SQL veri ambarı ücretleri için maliyetleri nasıl kaydedeceğinizi öğrenin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 08/29/2019
ms.author: banders
ms.openlocfilehash: 381a709c74f5fcf6bb1f89f07ad84d5e3af0c5e0
ms.sourcegitcommit: b8578b14c8629c4e4dea4c2e90164e42393e8064
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2019
ms.locfileid: "70806286"
---
# <a name="save-costs-for-sql-data-warehouse-charges-with-reserved-capacity"></a>Ayrılmış kapasiteye sahip SQL veri ambarı ücretleri maliyetlerini kaydetme

Bir veya üç yıllık süre boyunca cDWU kullanımınız için bir ayırmaya geçerek Azure SQL veri ambarı ile tasarruf edebilirsiniz. SQL veri ambarı ayrılmış kapasitesini satın almak için Azure bölgesini ve terimini seçmeniz gerekir. Sonra SQL Veri Ambarı SKU'sunu sepetinize ekleyin ve satın almak istediğiniz cDWU birimi miktarını seçin.

Rezervasyon satın aldığınızda, rezervasyonun öznitelikleriyle eşleşen SQL Veri Ambarı kullanımı artık kullandıkça öde fiyatlarıyla ücretlendirilmez.

SQL Veri Ambarı'nın kullanımıyla ilişkili depolama ve ağ ücretleri rezervasyonun kapsamında değildir.

Ayrılmış kapasitenin süresi dolduğunda SQL Veri Ambarı örnekleri çalışmaya devam eder ama kullandıkça öde fiyatlarıyla faturalanır. Rezervasyonlar otomatik olarak yenilenmez.

Fiyatlandırma bilgileri için bkz. [SQL veri ambarı ayrılmış kapasitesi teklifi](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/).

Azure SQL veri ambarı ayrılmış kapasitesini [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)satın alabilirsiniz. Rezervasyon için [veya aylık ödemeler](billing-monthly-payments-reservations.md)için ödeme yapın. Ayrılmış kapasiteyi satın almak için:

- En az bir kurumsal veya kullandıkça öde aboneliği için sahip rolüne sahip olmanız gerekir.
- Kurumsal abonelikler için, **ayrılmış örnekler Ekle** seçeneğinin [EA portalında](https://ea.azure.com/)etkinleştirilmiş olması gerekir. Ayar devre dışı bırakılmışsa, bir EA yöneticisi olmanız gerekir.
- Bulut çözümü sağlayıcısı (CSP) programı için, yalnızca yönetici aracıları veya satış aracıları SQL veri ambarı ayrılmış kapasitesini satın alabilir.

Kurumsal müşterilerin ve kullandıkça öde müşterilerinin rezervasyon satın alma işlemleri için nasıl ücretlendirilildiği hakkında daha fazla bilgi için bkz. [Kurumsal kaydınız Için Azure ayırma kullanımını anlama](billing-understand-reserved-instance-usage-ea.md) ve [Azure ayırma kullanımını anlama Kullandıkça Öde aboneliği](billing-understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Satın almadan önce doğru boyutu seçin

SQL veri ambarı rezervasyon boyutu, kullandığınız toplam bilgi işlem verileri ambarı birimlerine (cDWU) göre olmalıdır. Satın alma işlemleri 100 cDWU artışlarla yapılır.

Örneğin, toplam SQL veri ambarı tüketiminin DW3000c olduğunu varsayalım. Tümü için ayrılmış kapasite satın almak istiyorsunuz. Bu nedenle, 30 birim cDWU ayrılmış kapasitesi satın almalısınız.

## <a name="buy-sql-data-warehouse-reserved-capacity"></a>SQL veri ambarı ayrılmış kapasitesini satın alma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmet** > **ayırmaları**' ni seçin.
3. Bir abonelik seçin. Ayrılmış kapasite için ödeme yapmak üzere kullanılan aboneliği seçmek için abonelik listesini kullanın. Aboneliğin ödeme yöntemi, ayrılan kapasitenin maliyetlerine göre ücretlendirilir. Abonelik türü bir kurumsal anlaşma olmalıdır (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Kullandıkça Öde (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P).
   - Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir.
   - Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.
4. Kapsam seçin. Kapsam listesini kullanarak bir abonelik kapsamı seçin.
   - **Tek kaynak grubu kapsamı** — yalnızca seçili kaynak grubundaki eşleşen kaynaklara rezervasyon iskontosunu uygular.
   - **Tek abonelik kapsamı** — seçili abonelikteki eşleşen kaynaklara rezervasyon iskontosunu uygular.
   - **Paylaşılan kapsam** — fatura bağlamındaki uygun aboneliklerde bulunan kaynaklara karşılık gelen rezervasyon iskontosunu uygular. Kurumsal Anlaşma müşteriler için, faturalandırma bağlamı kayıt olur. Kullandıkça Öde tarifelerine sahip bireysel abonelikler için faturalandırma kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.
   - Kurumsal müşteriler için, faturalandırma bağlamı EA kaydı ' dır.
   - Kullandıkça Öde müşterileri için, paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde aboneliklerdir.
5. Ayrılmış kapasite kapsamındaki bir Azure bölgesi seçmek için bir bölge seçin.
6. Bir miktar seçin. Satın almak istediğiniz 100 veri ambarı birimi (cDWU) miktarını girin.    
   Örneğin, 30 ' a kadar bir miktar, her saat için ayrılmış kapasite 3.000.
7. **Maliyetler** bölümünde SQL veri ambarı ayrılmış kapasite ayırma maliyeti ' ni gözden geçirin.
8. **Satın al**'ı seçin.
9. Satın alma durumunuzu görmek için **Bu ayırmayı görüntüle** ' yi seçin.

## <a name="cancel-exchange-or-refund-reservations"></a>İptal, Exchange veya para iadesi rezervasyonları

Belirli sınırlamalara sahip rezervasyonları iptal edebilir, Exchange veya para iadesi yapabilirsiniz. Daha fazla bilgi için bkz. [Azure ayırmaları Için self servis değişimlerinin ve para iadesi](billing-azure-reservations-self-service-exchange-and-refund.md).

Bir rezervasyon indirimi, SQL veri ambarı ayrılmış kapasite kapsamı ve bölgesiyle eşleşen SQL veri ambarı örneklerinin sayısına otomatik olarak uygulanır. SQL veri ambarı ayrılmış kapasitesinin kapsamını [Azure Portal](https://portal.azure.com/), POWERSHELL, CLı veya API aracılığıyla güncelleştirebilirsiniz.

## <a name="need-help-contact-us"></a>Yardım mı gerekiyor? Bizimle iletişim kurun

Sorularınız varsa veya yardıma ihtiyacınız [bir destek isteği oluşturma](https://portal.azure.com/).

## <a name="next-steps"></a>Sonraki adımlar

- Rezervasyon iskontolarının Azure SQL veri ambarı 'na nasıl uygulandığı hakkında daha fazla bilgi edinmek için bkz. [rezervasyon Iskontolarının Azure SQL veri ambarı Için nasıl uygulandığı](billing-prepay-sql-data-warehouse-charges-with-reserved-capacity.md).

- Azure ayırmaları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure ayırmaları nelerdir?](billing-save-compute-costs-reservations.md)
  - [Azure Ayırmalarını yönetme](billing-manage-reserved-vm-instance.md)
  - [Azure ayırmaları indirimi 'nı anlama](billing-understand-reservation-charges.md)
  - [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](billing-understand-reserved-instance-usage.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlama](billing-understand-reserved-instance-usage-ea.md)
