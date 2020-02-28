---
title: Gönüllü geçiş aracını kullanarak klasik uyarılarınızı Azure Izleyici 'de geçirme
description: Klasik uyarı kurallarınızı geçirmek için gönüllü geçiş aracını nasıl kullanacağınızı öğrenin.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 03/19/2018
ms.subservice: alerts
ms.openlocfilehash: 537d84639523a74cbd9403d4ad25c34a798b7061
ms.sourcegitcommit: 747a20b40b12755faa0a69f0c373bd79349f39e3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/27/2020
ms.locfileid: "77665111"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Klasik uyarı kurallarınızı geçirmek için gönüllü geçiş aracını kullanın

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure izleyici 'deki klasik uyarılar Eylül 2019 ' de kullanımdan kaldırılıyor (ilk olarak 2019 Temmuz idi). Klasik uyarı kuralları kullanan ve geçiş yapmak isteyen müşterilere Azure portal bir geçiş aracı vardır. Bu makalede, Eylül 2019 ' de otomatik geçiş başlamadan önce, klasik uyarı kurallarınızı gönüllü olarak geçirmek için geçiş aracının nasıl kullanılacağı açıklanmaktadır.

> [!NOTE]
> Geçiş Aracı 'nın çıkış gecikmesi nedeniyle, klasik uyarılar geçişinin devre dışı bırakılması tarihi 31 Haziran 2019 tarihinde ilk bildirilen tarihten itibaren [31 ağustos 2019](https://azure.microsoft.com/updates/azure-monitor-classic-alerts-retirement-date-extended-to-august-31st-2019/) tarihine kadar genişletilmiştir.

## <a name="benefits-of-new-alerts"></a>Yeni uyarıların avantajları

Klasik uyarılar, Azure Izleyici 'de yeni ve birleştirilmiş uyarı ile değiştiriliyor. Yeni uyarılar platformunda aşağıdaki avantajlar bulunur:

- [Birçok Azure hizmeti](alerts-metric-near-real-time.md#metrics-and-dimensions-supported)için çeşitli çok boyutlu ölçümler üzerinde uyarı alabilirsiniz.
- Yeni ölçüm uyarıları, birçok kuralı yönetme yükünü önemli ölçüde azaltan [çok kaynak uyarı kurallarını](alerts-metric-overview.md#monitoring-at-scale-using-metric-alerts-in-azure-monitor) destekler.
- Şunları destekleyen Birleşik bildirim mekanizması:
  - Tüm yeni uyarı türleriyle (ölçüm, günlük ve etkinlik günlüğü) birlikte çalışarak bir modüler bildirim mekanizması olan [eylem grupları](action-groups.md).
  - SMS, Voice ve ITSM Bağlayıcısı gibi yeni bildirim mekanizmaları.
- [Birleşik uyarı deneyimi](alerts-overview.md) , tüm uyarıları farklı sinyallere (ölçüm, günlük ve etkinlik günlüğü) tek bir yerde getirir.

## <a name="before-you-migrate"></a>Geçirmeden önce

Geçiş işlemi, klasik uyarı kurallarını yeni, denk uyarı kurallarına dönüştürür ve eylem gruplarını oluşturur. Hazırlık bölümünde aşağıdaki noktalara dikkat edin:

- Bildirim yükü biçimi ve yeni uyarı kuralları oluşturmak ve yönetmek için API 'Ler, daha fazla özelliği desteklediklerinden klasik uyarı kurallarından farklıdır. [Geçişe hazırlanma hakkında bilgi edinin](alerts-prepare-migration.md).

- Bazı klasik uyarı kuralları araç kullanılarak geçirilemez. [Hangi kuralların geçirilemeyeceğini ve bunlarla ne yapılacağını öğrenin](alerts-understand-migration.md#classic-alert-rules-that-will-not-be-migrated).

    > [!NOTE]
    > Geçiş işlemi, klasik uyarı kurallarınızın değerlendirmesini etkilemez. Bunlar geçirilmeden ve yeni uyarı kuralları etkinleşene kadar uyarıları çalıştırmaya ve gönderilmeye devam ederler.

## <a name="how-to-use-the-migration-tool"></a>Geçiş aracını kullanma

Klasik uyarı kurallarınızın Azure portal geçişini tetiklemek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **izleme**' yi seçin.

1. **Uyarılar**' ı seçin ve ardından **Uyarı kurallarını yönet** ' i seçin veya **Klasik uyarıları görüntüleyin**.

1. Geçiş giriş sayfasına gitmek için **Yeni kurallara geçir** ' i seçin. Bu sayfa, tüm aboneliklerinizin ve geçiş durumlarının listesini gösterir:

    ![geçiş-giriş](media/alerts-migration/migration-landing.png "Kuralları geçir")

    Aracı kullanılarak geçirilebilen tüm abonelikler geçişe **hazırlanıyor**olarak işaretlenir.

    > [!NOTE]
    > Geçiş Aracı, klasik uyarı kuralları kullanan tüm aboneliklerde aşamalar halinde kullanıma alınıyor. Piyasaya sürülmeye yönelik erken aşamalarda, geçiş için yok olarak işaretlenmiş bazı abonelikler görebilirsiniz.

1. Bir veya daha fazla abonelik seçin ve ardından **geçiş önizlemesi**' ni seçin.

    Sonuçta ortaya çıkan sayfada bir abonelik için aynı anda geçirilecek olan klasik uyarı kurallarının ayrıntıları gösterilir. Ayrıca, ayrıntıları CSV biçiminde almak için **bu aboneliğin geçiş ayrıntılarını indir** ' i de seçebilirsiniz.

    ![geçiş-Önizleme](media/alerts-migration/migration-preview.png "Geçiş önizlemesi")

1. Geçiş durumu hakkında bildirim almak için bir veya daha fazla e-posta adresi belirtin. Geçiş tamamlandığında veya sizin için herhangi bir eylemde bulunmanız durumunda e-posta alacaksınız.

1. **Geçişi Başlat**' ı seçin. Onay iletişim kutusunda gösterilen bilgileri okuyun ve geçiş işlemini başlatmaya hazırsınız olduğunu onaylayın.

    > [!IMPORTANT]
    > Bir abonelik için geçiş işlemini başlattıktan sonra, bu abonelik için klasik uyarı kuralları düzenleyemez veya oluşturamazsınız. Bu kısıtlama, yeni kurallara geçiş sırasında klasik uyarı kurallarında hiçbir değişiklik olmamasını sağlar. Klasik uyarı kurallarınızı değiştiremeyeceksiniz, ancak geçirilene kadar uyarıları çalıştırmaya devam eder. Aboneliğiniz için geçiş tamamlandıktan sonra, artık klasik uyarı kurallarını kullanamazsınız.

    ![geçiş-Onayla](media/alerts-migration/migration-confirm.png "Geçişin başlamasını Onayla")

1. Geçiş tamamlandığında veya eylem yapmanız gerekiyorsa, daha önce belirttiğiniz adreslerde bir e-posta alırsınız. Ayrıca, portalda geçiş giriş sayfasında düzenli olarak durumu denetleyebilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>Aboneliğim neden geçiş için yok olarak listelendi?

Geçiş Aracı, aşamalar halinde müşterilere kullanıma alınıyor. Erken aşamalarda, aboneliklerinizin çoğu veya hepsi **geçiş için hazırlanma**olarak işaretlenebilir. 

Bir abonelik geçişe hazır hale geldiğinde, aboneliğin sahibi aracın kullanılabildiğini belirten bir e-posta iletisi alır. Bu ileti için bir göz atın.

### <a name="who-can-trigger-the-migration"></a>Geçişi kimlerin tetikleyebilen?

Abonelik düzeyinde kendisine atanmış Izleme katılımcısı rolü olan kullanıcılar geçişi tetikleyebiliyor. [Geçiş işlemi Için rol tabanlı Access Control hakkında daha fazla bilgi edinin](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-will-the-migration-take"></a>Geçiş ne kadar sürer?

Çoğu abonelik için bir saat altında geçiş tamamlanır. Geçiş giriş sayfasında geçiş ilerlemesini izleyebilirsiniz. Geçiş sırasında, uyarılarınızın klasik uyarılar sisteminde veya yeni bir veritabanında çalışmaya devam ettiğinden emin olun.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>Geçiş sırasında bir sorunla karşılaşsam ne yapabilirim?

Geçiş sırasında karşılaşabileceğiniz sorunlar hakkında yardım almak için [sorun giderme kılavuzuna](alerts-understand-migration.md#common-problems-and-remedies) bakın. Geçişi tamamlayabilmeniz için herhangi bir işlem yapmanız gerekiyorsa, aracı ayarlarken girdiğiniz e-posta adreslerinde size bildirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçişe hazırlanma](alerts-prepare-migration.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)
