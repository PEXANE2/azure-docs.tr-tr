---
title: İşlem maliyetlerini ayrılmış kapasiteye göre Kaydet
description: İşlem maliyetlerinizi kaydetmek için Azure Data Factory veri akışı ayrılmış kapasitesini satın almayı öğrenin.
ms.topic: conceptual
author: kromerm
ms.author: makromer
ms.service: data-factory
ms.date: 01/25/2021
ms.openlocfilehash: 8bea1ff0dcb945e969553a4f643e289a5157f966
ms.sourcegitcommit: fc8ce6ff76e64486d5acd7be24faf819f0a7be1d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/26/2021
ms.locfileid: "98811754"
---
# <a name="save-costs-for-resources-with-reserved-capacity---azure-data-factory-data-flows"></a>Ayrılmış kapasite Azure Data Factory veri akışları olan kaynaklar için maliyetleri kaydetme

Kullandıkça Öde fiyatlarıyla karşılaştırıldığında işlem kaynakları için bir ayırmaya girerek Azure Data Factory veri akışı maliyetleriyle tasarruf edin. Ayrılmış kapasite ile, işlem maliyetlerine göre önemli bir indirim elde etmek için bir veya üç yıllık bir dönem için ADF veri akışı kullanımı için bir taahhüt yaparsınız. Ayrılmış kapasiteyi satın almak için Azure bölgesini, işlem türünü, çekirdek sayısını ve terimi belirtmeniz gerekir.

Ayırmayı belirli bir fabrika veya tümleştirme çalışma zamanına atamanız gerekmez. Mevcut fabrikalar veya yeni dağıtılan fabrikalar otomatik olarak avantajı alır. Bir rezervasyon satın alarak, bir veya üç yıllık bir dönem için veri akışı işlem maliyetlerine yönelik kullanıma de kayıt yapılır. Bir rezervasyon satın alarak, rezervasyon öznitelikleriyle eşleşen işlem ücretleri artık Kullandıkça Öde tarifelerine göre ücretlendirilir. 

Ayrılmış kapasiteyi [Azure Portal](https://portal.azure.com)satın alabilirsiniz. [Peşin olarak veya aylık ödemelerle](https://docs.microsoft.com/azure/cost-management-billing/reservations/prepare-buy-reservation.md) rezervasyon ödemesi yapın. Ayrılmış kapasite satın almak için:

- Kullandıkça Öde tarifesine sahip en az bir kuruluş veya ayrı bir abonelik için sahip rolünde olmanız gerekir.
- Kurumsal abonelikler için, [EA portal](https://ea.azure.com)’da **Ayrılmış Örnek Ekle** seçeneği etkinleştirilmelidir. Ya da bu ayar devre dışı bırakılırsa, abonelikte bir EA yöneticisi olmanız gerekir. Ayrılmış kapasite.

Kurumsal müşterilerin ve kullandıkça öde müşterilerinin rezervasyon satın alma işlemleri için nasıl ücretlendirilildiği hakkında daha fazla bilgi için bkz. [Kurumsal kaydınız Için Azure ayırma kullanımını anlama](https://docs.microsoft.com/azure/cost-management-billing/reservations/understand-reserved-instance-usage-ea) ve [Kullandıkça Öde aboneliğiniz için Azure rezervasyon kullanımını anlama](https://docs.microsoft.com/azure/cost-management-billing/reservations/understand-reserved-instance-usage.md).

> [!NOTE]
> Satın alma ayrılmış kapasitesi, kullanımınız için belirli altyapı kaynaklarını (sanal makineler veya kümeler) önceden ayırmaz veya ayırmaz.

## <a name="determine-proper-azure-ir-sizes-needed-before-purchase"></a>Satın almadan önce gereken uygun Azure IR boyutlarını belirleme

Ayırma boyutu, aynı işlem katmanını kullanan mevcut veya hemen dağıtılmış veri akışları tarafından kullanılan işlem Toplam miktarına bağlı olmalıdır.

Örneğin, 32 çekirdekle İyileştirilmiş bellek kullanarak saatlik bir işlem hattı yürütülediğinizi varsayalım. Ayrıca, bir sonraki ay içinde, genel amaçlı 64 çekirdekleri kullanan ek bir işlem hattı içinde dağıtmayı planladığınızı varsayalım. Ayrıca, bu kaynaklara en az 1 yıl boyunca ihtiyacınız olacağını bildiğinizi varsayalım. Bu durumda, bellek için iyileştirilmiş veri akışları için 32 çekirdek 1 yıllık ayırma ve genel amaçlı 64 çekirdekli 1 yıllık ayırma satın almalısınız.

## <a name="buy-reserved-capacity"></a>Ayrılmış kapasite satın alın

1. [Azure Portal](https://portal.azure.com) oturum açın.
2. **Tüm hizmetler** > **Rezervasyonlar**’ı seçin.
3. **Ekle** ' yi seçin ve ardından **satın alma rezervasyonları** bölmesinde, ADF veri akışları için yeni bir ayırma satın almak üzere **ADF veri akışları** ' nı seçin.
4. Ayrılan kapasite iskontosunu almak için, seçtiğiniz gerekli alanları ve öznitelikleri girin. İndirimin alınacağı gerçek veri akışı sayısı, seçilen kapsama ve miktara bağlıdır.
5. **Maliyetler** bölümündeki kapasite rezervasyonunun maliyetini gözden geçirin.
6. **Satın al**'ı seçin.
7. Satın alımınızın durumunu görmek için **Bu ayırmayı görüntüle** ' yi seçin.

## <a name="cancel-exchange-or-refund-reservations"></a>Rezervasyonları iptal etme, değiştirme veya para iadesi alma

Belirli sınırlamalarla rezervasyonları iptal edebilir, değiştirebilir veya para iadesi alabilirsiniz. Daha fazla bilgi için bkz. [Azure Ayrılmış Sanal Makine Örnekleri için self servis değişimler ve para iadeleri](https://docs.microsoft.com/azure/cost-management-billing/reservations/exchange-and-refund-azure-reservations.md).

## <a name="need-help-contact-us"></a>Yardıma mı ihtiyacınız var? Bizimle iletişim kurun

Sorularınız varsa ya da yardıma gereksinim duyuyorsanız [destek isteği oluşturun](https://portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest).

## <a name="next-steps"></a>Sonraki adımlar

Azure Ayrılmış Sanal Makine Örnekleri hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Ayrılmış Sanal Makine Örnekleri indirimini anlama](data-flow-understand-reservation-charges.md)
