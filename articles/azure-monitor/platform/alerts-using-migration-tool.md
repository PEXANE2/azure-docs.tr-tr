---
title: Gönüllü geçiş aracını kullanarak klasik uyarılarınızı Azure Monitörü'nde geçirin
description: Klasik uyarı kurallarınızı geçirmek için gönüllü geçiş aracını nasıl kullanacağınızı öğrenin.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 03/19/2018
ms.subservice: alerts
ms.openlocfilehash: 537d84639523a74cbd9403d4ad25c34a798b7061
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77665111"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Klasik uyarı kurallarınızı geçirmek için gönüllü geçiş aracını kullanın

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure Monitor'daki klasik uyarılar Eylül 2019'da kullanımdan kaldırılmaktadır (başlangıçta Temmuz 2019'du). Azure portalında, klasik uyarı kuralları kullanan ve geçişi kendileri tetiklemek isteyen müşteriler için bir geçiş aracı kullanılabilir. Bu makalede, otomatik geçiş Eylül 2019'da başlamadan önce klasik uyarı kurallarınızı gönüllü olarak geçirmek için geçiş aracının nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Geçiş aracının kullanıma sunulmasındaki gecikme nedeniyle, klasik uyarı geçişi için emeklilik tarihi 30 Haziran 2019 tarihinden itibaren [31 Ağustos 2019'a uzatıldı.](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/)

## <a name="benefits-of-new-alerts"></a>Yeni uyarıların avantajları

Azure Monitor'da klasik uyarılar yeni ve birleşik uyarılarla değiştiriliyor. Yeni uyarılar platformu aşağıdaki avantajlara sahiptir:

- Çok [daha fazla Azure hizmeti](alerts-metric-near-real-time.md#metrics-and-dimensions-supported)için çeşitli çok boyutlu ölçümler konusunda uyarıda olabilirsiniz.
- Yeni metrik uyarılar, birçok kuralı yönetmenin genel kısmını büyük ölçüde azaltan [çok kaynak uyarı kurallarını](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) destekler.
- Aşağıdakileri destekleyen birleşik bildirim mekanizması:
  - [Eylem grupları,](action-groups.md)tüm yeni uyarı türleri (metrik, günlük ve etkinlik günlüğü) ile çalışan modüler bir bildirim mekanizması.
  - SMS, ses ve ITSM Konektörü gibi yeni bildirim mekanizmaları.
- [Birleşik uyarı deneyimi,](alerts-overview.md) farklı sinyallerdeki (metrik, günlük ve etkinlik günlüğü) tüm uyarıları tek bir yere getirir.

## <a name="before-you-migrate"></a>Göç etmeden önce

Geçiş işlemi, klasik uyarı kurallarını yeni, eşdeğer uyarı kurallarına dönüştürür ve eylem grupları oluşturur. Hazırlık olarak, aşağıdaki noktalara dikkat edin:

- Hem bildirim yükü biçimi hem de yeni uyarı kuralları oluşturmak ve yönetmek için API'ler klasik uyarı kurallarından farklıdır, çünkü daha fazla özelliği desteklerler. [Geçişe nasıl hazırlanacağınızı öğrenin.](alerts-prepare-migration.md)

- Bazı klasik uyarı kuralları aracı kullanılarak geçirilemez. [Hangi kuralların geçirilemeyeceğini ve bunlarla ne yapılacağını öğrenin.](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated)

    > [!NOTE]
    > Geçiş işlemi, klasik uyarı kurallarınızın değerlendirilmesini etkilemez. Bunlar, geçirilmelerine ve yeni uyarı kuralları yürürlüğe girene kadar çalışmaya ve uyarı göndermeye devam ederler.

## <a name="how-to-use-the-migration-tool"></a>Geçiş aracını kullanma

Azure portalında klasik uyarı kurallarınızın geçişini tetiklemek için aşağıdaki adımları izleyin:

1. [Azure portalında,](https://portal.azure.com) **Monitör'ü**seçin.

1. **Uyarıları**seçin ve ardından **uyarı kurallarını yönet'i** veya klasik **uyarıları görüntüleyin'i**seçin.

1. Geçiş açılış sayfasına gitmek için **yeni kurallara geçir'i** seçin. Bu sayfa, tüm aboneliklerinizin ve geçiş durumlarının bir listesini gösterir:

    ![göç-iniş](media/alerts-migration/migration-landing.png "Geçiş kuralları")

    Aracı kullanarak geçirilebilen tüm abonelikler **geçirilmeye hazır**olarak işaretlenir.

    > [!NOTE]
    > Geçiş aracı, klasik uyarı kuralları kullanan tüm aboneliklere aşamalı olarak yayılıyor. Kullanıma sonunun ilk aşamalarında, bazı aboneliklerin geçiş için hazır olmadığı şeklinde işaretlenmiş olarak görebilirsiniz.

1. Bir veya daha fazla abonelik seçin ve ardından **Geçiş önizleme'yi**seçin.

    Ortaya çıkan sayfa, aynı anda bir abonelik için geçirilecek klasik uyarı kurallarının ayrıntılarını gösterir. Ayrıntıları CSV formatında almak **için bu aboneliğin geçiş ayrıntılarını indir'i** de seçebilirsiniz.

    ![geçiş önizleme](media/alerts-migration/migration-preview.png "Geçişönizleme")

1. Geçiş durumu hakkında bilgilendirilmek üzere bir veya daha fazla e-posta adresi belirtin. Geçiş tamamlandığında veya sizden herhangi bir eylem gerekirse e-posta alırsınız.

1. **Geçişe Başlat'ı**seçin. Onay iletişim kutusunda gösterilen bilgileri okuyun ve geçiş işlemini başlatmaya hazır olduğunuzu onaylayın.

    > [!IMPORTANT]
    > Bir abonelik için geçiş işlemini başlattıktan sonra, bu abonelik için klasik uyarı kurallarını kaldıramazsınız veya oluşturamazsınız. Bu kısıtlama, yeni kurallara geçiş sırasında klasik uyarı kurallarınızda hiçbir değişikliğin kaybedilmesini sağlar. Klasik uyarı kurallarınızı değiştiremeseniz de, bunlar çalışmaya ve bunlar geçirilene kadar uyarı vermeye devam eder. Aboneliğiniz için geçiş tamamlandıktan sonra artık klasik uyarı kurallarını kullanamazsınız.

    ![göç-onaylamak](media/alerts-migration/migration-confirm.png "Başlatma geçişini onaylama")

1. Geçiş tamamlandığında veya sizden eylem gerekiyorsa, daha önce sağladığınız adreslerden bir e-posta alırsınız. Ayrıca, portaldaki geçiş açılış sayfasındaki durumu düzenli aralıklarla kontrol edebilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>Aboneliğim neden geçişiçin hazır değil olarak listelendi?

Geçiş aracı aşamalı olarak müşterilere yayılıyor. İlk aşamalarda, aboneliklerinizin çoğu veya tümü **geçiş için hazır değil**olarak işaretlenmiş olabilir. 

Bir abonelik geçiş için hazır olduğunda, abonelik sahibi aracın kullanılabilir olduğunu belirten bir e-posta iletisi alır. Bu mesajiçin bir göz atın.

### <a name="who-can-trigger-the-migration"></a>Göçü kim tetikleyebilir?

Abonelik düzeyinde kendilerine Atanan İzleme Katılımcısı rolüne sahip kullanıcılar geçişi tetikleyebilir. [Geçiş işlemi için Rol Tabanlı Erişim Denetimi hakkında daha fazla bilgi edinin.](alerts-understand-migration.md#who-can-trigger-the-migration)

### <a name="how-long-will-the-migration-take"></a>Göç ne kadar sürecek?

Çoğu abonelik için geçiş bir saatten az bir sürede tamamlanır. Geçiş açılış sayfasında geçiş ilerlemesini izleyebilirsiniz. Geçiş sırasında, uyarılarınızın klasik uyarı sisteminde veya yenisinde çalışmaya devam ettiğinden emin olun.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>Geçiş sırasında bir sorunla karşı lakarşılarım ne yapabilirim?

Geçiş sırasında karşılaşabileceğiniz sorunlarla ilgili yardım için [sorun giderme kılavuzuna](alerts-understand-migration.md#common-problems-and-remedies) bakın. Geçişi tamamlamak için sizden herhangi bir işlem gerekirse, aracı ayarlarken sağladığınız e-posta adreslerinde bilgilendirilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş için hazırlama](alerts-prepare-migration.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)
