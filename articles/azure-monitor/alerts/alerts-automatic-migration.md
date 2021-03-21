---
title: Azure Monitor klasik uyarılarınız için otomatik geçiş işleminin nasıl çalıştığını anlayın
description: Otomatik geçiş işleminin nasıl çalıştığını öğrenin.
ms.topic: conceptual
ms.date: 02/14/2021
ms.openlocfilehash: 92aaffcea7a7c96cd77aade318520b093eed3e14
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102045471"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Klasik uyarı kurallarınız için otomatik geçiş işlemini anlayın

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure izleyici 'deki klasik uyarılar, genel bulut kullanıcıları için devre dışı bırakılır, ancak yine de **31 Mayıs 2021** tarihine kadar sınırlı kullanımda. Azure Kamu bulutu ve Azure Çin 21Vianet için klasik uyarılar, **29 şubat 2024** tarihinde devre dışı bırakılacak.

Müşterilerin geçişi tetikleyebilmesi için Azure portal [bir geçiş aracı](alerts-using-migration-tool.md) vardır. Bu makalede, 31 Mayıs 2021 ' den sonra başlatılacak olan genel buluttaki otomatik geçiş işlemi açıklanmaktadır. Ayrıca, çalıştırabileceğiniz sorun ve çözümlerin ayrıntılarını da ayrıntılarıyla görebilirsiniz.

## <a name="important-things-to-note"></a>Dikkat edilmesi gereken önemli noktalar

Geçiş işlemi, klasik uyarı kurallarını yeni, denk uyarı kurallarına dönüştürür ve eylem gruplarını oluşturur. Hazırlık bölümünde aşağıdaki noktalara dikkat edin:

- Yeni uyarı kuralları için bildirim yükü biçimleri, daha fazla özelliği desteklediklerinden, klasik uyarı kurallarının yüklerden farklıdır. Logic Apps, runbook 'lar veya Web kancaları içeren klasik bir uyarı kuralınız varsa, yükteki farklılıklar nedeniyle geçişten sonra beklendiği gibi çalışmayı durdurabilir. [Geçişe hazırlanma hakkında bilgi edinin](alerts-prepare-migration.md).

- Bazı klasik uyarı kuralları araç kullanılarak geçirilemez. [Hangi kuralların geçirilemeyeceğini ve bunlarla ne yapılacağını öğrenin](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

## <a name="what-will-happen-during-the-automatic-migration-process-in-public-cloud"></a>Genel buluttaki otomatik geçiş işlemi sırasında ne olur?

- 31 Mayıs 2021 ' den itibaren yeni bir klasik uyarı kuralı oluşturamayacaksınız ve klasik uyarıların geçişi toplu olarak tetiklenecektir.
- Silinen hedef kaynakları izleyen ve [artık desteklenmeyen ölçümlerde](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics) bulunan tüm klasik uyarı kuralları geçersiz olarak kabul edilir.
- Geçersiz olan klasik uyarı kuralları, 31 Mayıs 2021 ' den sonra kaldırılacaktır.
- Aboneliğiniz için geçiş başladıktan sonra bir saat içinde tamamlanmalıdır. Müşteriler, [Azure izleyici 'deki geçiş aracında](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)geçiş durumunu izleyebilir.
- Abonelik sahipleri, başarılı veya geçiş başarısızlığından bir e-posta alır.

    > [!NOTE]
    > Otomatik geçiş işleminin başlamasını beklemek istemiyorsanız geçiş aracını kullanarak geçiş işlemini yine de tetikleyebilirsiniz.

## <a name="what-if-the-automatic-migration-fails"></a>Otomatik geçiş başarısız olursa ne olur?

Otomatik geçiş işlemi başarısız olduğunda, abonelik sahipleri sorunu bildiren bir e-posta alır. Sorunun tüm ayrıntılarını görmek için Azure Izleyici 'de geçiş aracı 'nı kullanabilirsiniz. Geçiş sırasında karşılaşabileceğiniz sorunlar hakkında yardım almak için [sorun giderme kılavuzuna](alerts-understand-migration.md#common-problems-and-remedies) bakın.

  > [!NOTE]
  > Müşterilerin bir kaynak kilidini geçici olarak devre dışı bırakmak veya bir ilke atamasını değiştirmek gibi bir eyleme ihtiyaç duyulmanız durumunda, müşterilerin bu sorunları çözmesi gerekir. Sorunlar daha sonra çözülemezse, klasik uyarılarınızın başarılı geçirilmesi garanti edilemez.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş için hazırlama](alerts-prepare-migration.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)