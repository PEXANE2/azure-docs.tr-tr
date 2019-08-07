---
title: Azure ayrılmış kapasitesi ile SQL veri ambarı ücretleri için ön ödeme
description: Para tasarrufu sağlamak için ayrılmış kapasiteye sahip SQL veri ambarı ücretleri için nasıl ön ödeme yapabileceğinizi öğrenin.
services: billing
author: yashesvi
manager: yashar
ms.service: billing
ms.topic: conceptual
ms.date: 08/02/2019
ms.author: banders
ms.openlocfilehash: 6ee7a661434bb756c6cf196937229db19d06b373
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779976"
---
# <a name="prepay-for-sql-data-warehouse-charges-with-reserved-capacity"></a>Ayrılmış kapasiteye sahip SQL veri ambarı ücretleri için ön ödeme

CDWU kullanımınız için bir veya üç yıl boyunca ön ödeme yaparak Azure SQL veri ambarı ile para tasarrufu yapabilirsiniz. SQL veri ambarı ayrılmış kapasitesini satın almak için Azure bölgesini ve terimini seçmeniz gerekir. Sonra, sepetinize SQL veri ambarı SKU 'sunu ekleyin ve satın almak istediğiniz cDWU birimi miktarını seçin.

Bir ayırma satın aldığınızda, rezervasyon öznitelikleriyle eşleşen SQL veri ambarı kullanımı artık Kullandıkça Öde tarifesine göre ücretlendirilir.

Bir ayırma, SQL veri ambarı kullanımıyla ilişkili depolama veya ağ ücretlerini kapsamaz.

Ayrılmış kapasitenin süresi dolmuşsa SQL veri ambarı örnekleri çalışmaya devam eder, ancak Kullandıkça Öde tarifesine göre faturalandırılır. Ayırmalar otomatik olarak yenilenmez.

Fiyatlandırma bilgileri için bkz. [SQL veri ambarı ayrılmış kapasitesi teklifi](https://azure.microsoft.com/pricing/details/sql-data-warehouse/gen2/).

Azure SQL veri ambarı ayrılmış kapasitesini [Azure Portal](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade)satın alabilirsiniz. Ayrılmış kapasiteyi satın almak için:

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
3. Abonelik seçin. Ayrılmış kapasite için ödeme yapmak üzere kullanılan aboneliği seçmek için abonelik listesini kullanın. Aboneliğin ödeme yöntemi, ayrılmış kapasitenin ön maliyetlerine göre ücretlendirilir. Abonelik türü bir kurumsal anlaşma olmalıdır (teklif numaraları: MS-AZR-0017P veya MS-AZR-0148P) veya Kullandıkça Öde (teklif numaraları: MS-AZR-0003P veya MS-AZR-0023P).
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
  - [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)
