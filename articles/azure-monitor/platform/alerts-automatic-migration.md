---
title: Azure Monitor klasik uyarılarınız için otomatik geçiş işleminin nasıl çalıştığını anlayın
description: Otomatik geçiş işleminin nasıl çalıştığını öğrenin.
ms.topic: conceptual
ms.date: 08/19/2019
ms.subservice: alerts
ms.openlocfilehash: 6a2d032c6aa33b72fe422638df45ca48bf8b1036
ms.sourcegitcommit: 7fe8df79526a0067be4651ce6fa96fa9d4f21355
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87847292"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Klasik uyarı kurallarınız için otomatik geçiş işlemini anlayın

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure izleyici 'deki klasik uyarılar kullanımdan kalkmakta, ancak henüz yeni uyarıları desteklemeyen kaynaklar için hala sınırlı kullanımda. Kullanımdan kaldırma işleminin bir parçası olarak, müşterilerin geçişi tetikleyebilmesi için Azure portal [bir geçiş aracı](alerts-using-migration-tool.md) vardır.
Bu makale, otomatik geçiş sürecinde size yol gösterir ve karşılaşabileceğiniz tüm sorunları çözmenize yardımcı olur.

  > [!NOTE]
  > Bu makale yalnızca Azure genel bulutu için geçerlidir. Azure için kullanımdan kaldırma işlemi Azure Kamu bulutundaki klasik uyarılar ve Azure Çin 21Vianet, gelecekteki tarihte duyurulacaktır.

## <a name="what-will-happen-during-the-automatic-migration-process"></a>Otomatik geçiş işlemi sırasında ne olur?

- Müşteriler, **1 eylül 2019**' den itibaren [belirli ölçümler](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts)dışında yeni bir klasik uyarı kuralı oluşturamayacak.
- Müşteriler özel durumlar için yeni klasik uyarı kuralları oluşturmaya ve daha fazla duyuru yapılıncaya kadar klasik uyarılarını kullanmaya devam edebilir.
- Klasik uyarıların geçirilmesi, **1 eylül 2019**' den itibaren klasik uyarılara sahip müşteriler için toplu olarak tetiklenecektir.
- Gönüllü geçiş aracında olduğu gibi, geçirilip geçirilemeyeceğini denetleyin olmayan bazı klasik uyarı kuralları oldukları gibi kalır. Bu klasik uyarı kuralları, daha fazla duyuru yapılıncaya kadar desteklenmeye devam edecektir. Ancak, tüm geçersiz klasik uyarı kuralları işlevsel olmadıkları için silinir.
Silinen hedef kaynakları izleyen ve [artık desteklenmeyen ölçümlerde](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics) bulunan tüm klasik uyarı kuralları geçersiz olarak kabul edilir.
- Aboneliğiniz için geçiş başladıktan sonra herhangi bir sorun olmadıkça, geçiş bir saat içinde tamamlanmalıdır. Müşteriler, [Azure izleyici 'deki geçiş dikey](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)penceresinde geçişin durumunu izleyebilir.
- Abonelik sahipleri, geçişin başarıyla tamamlandığını bir e-posta alacak.
- Geçiş sırasında herhangi bir sorun varsa, abonelik sahipleri bunlara aynı şekilde bir e-posta gönderilir. Müşteriler, sorunun tüm ayrıntılarını görmek için geçiş dikey penceresini kullanabilir.
- Müşterilerin bir kaynak kilidini geçici olarak devre dışı bırakmak veya bir ilke atamasını değiştirmek gibi bir eyleme ihtiyaç duyulmanız durumunda, müşterilerin bu sorunları çözmesi gerekir. Sorunlar daha sonra çözülemezse, klasik uyarılarınızın başarılı geçirilmesi garanti edilemez.

    > [!NOTE]
    > Otomatik geçiş işleminin başlamasını beklemek istemiyorsanız geçiş aracını kullanarak geçiş işlemini yine de tetikleyebilirsiniz.

## <a name="important-things-to-note"></a>Dikkat edilmesi gereken önemli noktalar

Geçiş işlemi, klasik uyarı kurallarını yeni, denk uyarı kurallarına dönüştürür ve eylem gruplarını oluşturur. Hazırlık bölümünde aşağıdaki noktalara dikkat edin:

- Yeni uyarı kuralları için bildirim yükü biçimleri, daha fazla özelliği desteklediklerinden, klasik uyarı kurallarından farklıdır. Klasik uyarı kuralı tarafından tetiklenen Logic Apps, runbook 'lar veya Web kancaları varsa, bildirim yüklerindeki farklılıklar nedeniyle, geçiş işlemi tamamlandıktan sonra beklendiği gibi çalışmayı durdurabilir. [Geçişe hazırlanma hakkında bilgi edinin](alerts-prepare-migration.md).

- Bazı klasik uyarı kuralları araç kullanılarak geçirilemez. [Hangi kuralların geçirilemeyeceğini ve bunlarla ne yapılacağını öğrenin](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

    > [!NOTE]
    > Geçiş işlemi, klasik uyarı kurallarınızın değerlendirmesini etkilemez. Bunlar geçirilmeden ve yeni uyarı kuralları etkinleşene kadar uyarıları çalıştırmaya ve gönderilmeye devam ederler.

## <a name="what-if-the-automatic-migration-fails"></a>Otomatik geçiş başarısız olursa ne olur?

Otomatik geçiş işlemi başarısız olduğunda, abonelik sahipleri sorunu bildiren bir e-posta alır. Sorunun tüm ayrıntılarını görmek için Azure Izleyici 'deki geçiş dikey penceresini kullanabilirsiniz.

Geçiş sırasında karşılaşabileceğiniz sorunlar hakkında yardım almak için [sorun giderme kılavuzuna](alerts-understand-migration.md#common-problems-and-remedies) bakın.

  > [!NOTE]
  > Müşterilerin bir kaynak kilidini geçici olarak devre dışı bırakmak veya bir ilke atamasını değiştirmek gibi bir eyleme ihtiyaç duyulmanız durumunda, müşterilerin bu sorunları çözmesi gerekir. Sorunlar daha sonra çözülemezse, klasik uyarılarınızın başarılı geçirilmesi garanti edilemez.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş için hazırlama](alerts-prepare-migration.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)
