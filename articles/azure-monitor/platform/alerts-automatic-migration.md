---
title: Azure Monitor klasik uyarılarınız için otomatik geçiş işleminin nasıl çalıştığını anlama
description: Otomatik geçiş işleminin nasıl çalıştığını öğrenin.
ms.topic: conceptual
ms.date: 08/19/2019
ms.subservice: alerts
ms.openlocfilehash: 8df83439d6754440648688ac1cc36ff66556a4e4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77668256"
---
# <a name="understand-the-automatic-migration-process-for-your-classic-alert-rules"></a>Klasik uyarı kurallarınız için otomatik geçiş işlemini anlama

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure Monitor'daki klasik uyarılar Eylül 2019'da kullanımdan kaldırılmaktadır (başlangıçta Temmuz 2019'du). Emeklilik sürecinin bir parçası olarak, müşterilerin geçişi kendileri tetiklemeleri için Azure portalında [bir geçiş aracı](alerts-using-migration-tool.md) kullanılabilir. Geçiş aracını 31 Ağustos 2019'a kadar kullanmadıysanız, Azure Monitor 1 Eylül 2019'dan itibaren klasik uyarılarınızın otomatik geçiş işlemini başlatacaktır.
Bu makale, otomatik geçiş sürecinde size yol ve size rastlayabilirsiniz herhangi bir sorunları çözmenize yardımcı olur.

  > [!NOTE]
  > Bu makale yalnızca Azure genel bulutu için geçerlidir. Azure Devlet bulutu ve Azure China 21Vianet'teki Azure Monitor klasik uyarıları için emeklilik süreci ileriki tarihte duyurulacaktır.

## <a name="what-will-happen-during-the-automatic-migration-process"></a>Otomatik geçiş işlemi sırasında ne olacak?

- **1 Eylül 2019'dan**itibaren, müşteriler [belirli ölçümler](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated)dışında yeni klasik uyarı kuralları oluşturamaz.
  - İstisnalar için, müşteri yeni klasik uyarı kuralları oluşturmaya ve klasik uyarılarını Haziran 2020'ye kadar kullanmaya devam edebilir.
- **1 Eylül 2019'dan**itibaren, klasik uyarılara sahip müşteriler için toplu olarak klasik uyarı geçişi tetiklenecektir.
- Gönüllü geçiş aracında olduğu gibi, geçirilemeyecek bazı klasik uyarı kuralları olduğu gibi bırakılır. Bu klasik uyarı kuralları Haziran 2020'ye kadar desteklenmeye devam edecektir. Ancak, geçersiz klasik uyarı kuralları işlevsel olmadığı için silinir.
Silinen hedef kaynakları veya [artık desteklenmeyen ölçümleri](alerts-understand-migration.md#classic-alert-rules-on-deprecated-metrics) izleyen tüm klasik uyarı kuralları geçersiz kabul edilir.
- Aboneliğiniz için geçiş başladıktan sonra, herhangi bir sorun olmadığı sürece, geçiş bir saat içinde tamamlanmalıdır. Müşteriler, [Azure Monitor'da geçiş bıçağındaki geçiş](https://portal.azure.com/#blade/Microsoft_Azure_Monitoring/MigrationBladeViewModel)durumunu izleyebilir.
- Abonelik sahipleri, geçişin başarıyla tamamlanmasıyla ilgili bir e-posta alır.
- Geçiş sırasında herhangi bir sorun varsa, abonelik sahipleri de aynı onları bildiren bir e-posta alırsınız. Müşteriler sorunun tüm ayrıntılarını görmek için geçiş bıçağını kullanabilir.
- Müşteriden kaynak kilidini geçici olarak devre dışı bırakmak veya bir ilke atamasını değiştirmek gibi bir eylem gerekirse, müşterilerin 31 Ekim 2019'a kadar herhangi bir sorunu çözmeleri gerekir. Sorunlar o zamana kadar çözülmezse, klasik uyarılarınızın başarılı bir şekilde geçişi garanti edilemez.

    > [!NOTE]
    > Otomatik geçiş işleminin başlatılmasını beklemek istemiyorsanız, geçiş aracını kullanarak geçişi isteğe bağlı olarak tetikleyebilirsiniz.

## <a name="important-things-to-note"></a>Unutulmaması gereken önemli şeyler

Geçiş işlemi, klasik uyarı kurallarını yeni, eşdeğer uyarı kurallarına dönüştürür ve eylem grupları oluşturur. Hazırlık olarak, aşağıdaki noktalara dikkat edin:

- Yeni uyarı kuralları için bildirim yükü biçimleri, daha fazla özelliği destekledikleri için klasik uyarı kurallarından farklıdır. Klasik uyarı kuralı tarafından tetiklenen mantık uygulamalarınız, runbook'larınız veya web hook'larınız varsa, bildirim yükleri arasındaki farklılıklar nedeniyle geçiş tamamlandığında beklendiği gibi çalışmayı durdurabilirler. [Geçişe nasıl hazırlanacağınızı öğrenin.](alerts-prepare-migration.md)

- Bazı klasik uyarı kuralları aracı kullanılarak geçirilemez. [Hangi kuralların geçirilemeeceğini ve bunlarla ne yapacağınızı öğrenin.](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated)

    > [!NOTE]
    > Geçiş işlemi, klasik uyarı kurallarınızın değerlendirilmesini etkilemez. Bunlar, geçirilmelerine ve yeni uyarı kuralları yürürlüğe girene kadar çalışmaya ve uyarı göndermeye devam ederler.

## <a name="what-if-the-automatic-migration-fails"></a>Otomatik geçiş başarısız olursa ne olur?

Otomatik geçiş işlemi başarısız olduğunda, abonelik sahipleri sorunu bildiren bir e-posta alır. Sorunun tüm ayrıntılarını görmek için Azure Monitor'daki geçiş bıçağını kullanabilirsiniz.

Geçiş sırasında karşılaşabileceğiniz sorunlarla ilgili yardım için [sorun giderme kılavuzuna](alerts-understand-migration.md#common-problems-and-remedies) bakın.

  > [!NOTE]
  > Müşteriden kaynak kilidini geçici olarak devre dışı bırakmak veya bir ilke atamasını değiştirmek gibi bir eylem gerekirse, müşterilerin 31 Ekim 2019'a kadar herhangi bir sorunu çözmeleri gerekir. Sorunlar o zamana kadar çözülmezse, klasik uyarılarınızın başarılı bir şekilde geçişi garanti edilemez.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş için hazırlama](alerts-prepare-migration.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)
