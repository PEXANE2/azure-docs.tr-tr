---
title: Azure ayrılmış kapasitesiyle Azure Synapse Analytics ücretlerinden tasarruf edin
description: Para tasarrufu sağlamak için ayrılmış kapasite ile Azure Synapse Analytics ücretlerinden nasıl maliyet tasarrufu elde edeceğinizi öğrenin.
author: yashesvi
ms.reviewer: yashar
ms.service: cost-management-billing
ms.topic: conceptual
ms.date: 04/30/2020
ms.author: banders
ms.openlocfilehash: 3706493f0132cd4cc57af589e0d2935c31d6657e
ms.sourcegitcommit: 1895459d1c8a592f03326fcb037007b86e2fd22f
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/01/2020
ms.locfileid: "82629159"
---
# <a name="save-costs-for-azure-synapse-analytics-charges-with-reserved-capacity"></a>Ayrılmış kapasiteyle Azure Synapse Analytics ücretlerinde maliyet tasarrufu sağlayın

Bir ile üç yıl arasındaki cDWU kullanımınız için bir rezervasyon taahhüdü vererek Azure Synapse Analytics’de tasarruf elde edebilirsiniz. Azure Synapse Analytics ayrılmış kapasitesi satın almak için Azure bölgesini ve dönemi seçmeniz gerekir. Sonra Azure Synapse Analytics SKU’sunu sepetinize ekleyin ve satın almak istediğiniz cDWU birimi miktarını seçin.

Rezervasyon satın aldığınızda, rezervasyonun öznitelikleriyle eşleşen Azure Synapse Analytics kullanımı artık kullandıkça öde fiyatlarıyla ücretlendirilmez.

Azure Synapse Analytics’in kullanımıyla ilişkili depolama ve ağ ücretleri rezervasyonun kapsamında değildir.

Ayrılmış kapasitenin süresi dolduğunda Azure Synapse Analytics örnekleri çalışmaya devam eder, ancak kullandıkça öde fiyatlarıyla faturalanır. Rezervasyonlar otomatik olarak yenilenmez.

Fiyatlandırma bilgileri için bkz. [Azure Synapse Analytics ayrılmış kapasite teklifi](https://azure.microsoft.com/pricing/details/synapse-analytics/).

[Azure portalında](https://portal.azure.com/#blade/Microsoft_Azure_Reservations/ReservationsBrowseBlade) Azure Synapse Analytics ayrılmış kapasitesi satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](monthly-payments-reservations.md) rezervasyon ödemesi yapın. Ayrılmış kapasite satın almak için:

- En az bir kurumsal veya Kullandıkça Öde aboneliği için sahip rolüne sahip olmanız gerekir.
- Kurumsal abonelikler için, [EA portal](https://ea.azure.com/)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Ayar devre dışı bırakılırsa EA Yöneticisi olmanız gerekir.
- Bulut Çözümü Sağlayıcısı (CSP) programı için yalnızca yönetici aracılar veya satış temsilcileri, Azure Synapse Analytics ayrılmış kapasitesi satın alabilir.

Kurumsal müşteriler ve Kullandıkça Öde müşterilerinin, rezervasyon satın alımları için nasıl ücretlendirildiği hakkında daha fazla bilgi için bkz. [Kurumsal kaydınız için Azure rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md) ve [Kullandıkça Öde aboneliğiniz için Azure rezervasyon kullanımını anlama](understand-reserved-instance-usage.md).

## <a name="choose-the-right-size-before-purchase"></a>Satın almadan önce doğru boyutu seçme

Azure Synapse Analytics rezervasyon boyutu, kullandığınız toplam işlem veri ambarı birimlerini (cDWU) temel almalıdır. Satın almalar, 100’er cDWU artımlarla gerçekleştirilir.

Örneğin, toplam Azure Synapse Analytics tüketiminizin DW3000c olduğunu varsayın. Tümü için ayrılmış kapasite satın almak istiyorsunuz. Bu nedenle 30 birim cDWU ayrılmış kapasitesi satın almanız gerekir.

## <a name="buy-azure-synapse-analytics-reserved-capacity"></a>Azure Synapse Analytics ayrılmış kapasitesi satın alma

1. [Azure Portal](https://portal.azure.com/) oturum açın.
2. **Tüm hizmetler** > **Rezervasyonlar**’ı seçin.
3. Bir abonelik seçin. Ayrılmış kapasitenin ödemesini yapmak için kullanılan aboneliği seçmek amacıyla Abonelik listesini kullanın. Ayrılmış kapasite maliyetleri, aboneliğin ödeme yöntemiyle ücretlendirilir. Abonelik türü bir kurumsal anlaşma (teklif numarası: MS-AZR-0017P veya MS-AZR-0148P) ya da Kullandıkça Öde (teklif numarası: MS-AZR-0003P veya MS-AZR-0023P).
   - Kurumsal abonelik için ücretler kaydın maddi işlem bakiyesinden düşülür ve fazla kullanım olarak ücretlendirilir.
   - Kullandıkça Öde aboneliğinde ücretler, aboneliğin kredi kartı veya fatura ödeme yöntemi ile faturalandırılır.
4. Bir kapsam seçin. Kapsam listesini kullanarak bir abonelik kapsamı seçin.
   - **Tek kaynak grubu kapsamı**: Yalnızca seçilen kaynak grubunda eşleşen kaynaklara rezervasyon indirimini uygular.
   - **Tek abonelik kapsamı**: Yalnızca seçilen abonelikte eşleşen kaynaklara rezervasyon indirimini uygular.
   - **Paylaşılan kapsam**: Faturalama bağlamında bulunan uygun aboneliklerdeki eşleşen kaynaklara rezervasyon indirimini uygular. Kurumsal Anlaşma müşterileri için faturalama bağlamı kayıttır. Kullandıkça öde fiyatlarına tabi bireysel abonelikler için faturalama kapsamı, hesap yöneticisi tarafından oluşturulan tüm uygun aboneliklerdir.
   - Kurumsal müşteriler için faturalama bağlamı, EA kaydıdır.
   - Kullandıkça Öde müşterileri için paylaşılan kapsam, hesap yöneticisi tarafından oluşturulan tüm Kullandıkça Öde abonelikleridir.
5. Ayrılmış kapasite kapsamındaki bir Azure bölgesi seçmek için bir bölge seçin.
6. Bir miktar seçin. Satın almak istediğiniz 100 Veri Ambarı birimi (cDWU) miktarını girin.    
   Örneğin, 30 miktarı size her saat 3.000 cDWU ayrılmış kapasite sunar.
7. **Maliyetler** bölümünde Azure Synapse Analytics ayrılmış kapasite rezervasyonunu gözden geçirin.
8. **Satın al**'ı seçin.
9. Satın alma durumunuzu görmek için **Bu Rezervasyonu Görüntüle** seçeneğini belirleyin.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](exchange-and-refund-azure-reservations.md).

Azure Synapse Analytics ayrılmış kapasite kapsamı ve bölgesi ile eşleşen Azure Synapse Analytics örneklerine otomatik olarak bir rezervasyon indirimi uygulanır. [Azure portalı](https://portal.azure.com/), PowerShell, CLI ile veya API aracılığıyla Azure Synapse Analytics ayrılmış kapasitesinin kapsamını güncelleştirebilirsiniz.

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/).

## <a name="next-steps"></a>Sonraki adımlar

- Azure Synapse Analytics’e rezervasyon indirimlerinin nasıl uygulanacağı hakkında daha fazla bilgi edinmek için bkz. [Azure Synapse Analytics için rezervasyon indirimleri nasıl uygulanır?](prepay-sql-data-warehouse-charges.md).

- Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
  - [Azure Ayrılmış Sanal Makine Örnekleri nedir?](save-compute-costs-reservations.md)
  - [Azure Ayırmalarını yönetme](manage-reserved-vm-instance.md)
  - [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](understand-reservation-charges.md)
  - [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlama](understand-reserved-instance-usage.md)
  - [Kurumsal kaydınız için rezervasyon kullanımını anlama](understand-reserved-instance-usage-ea.md)
