---
title: Azure Izleyici uyarı kurallarını geçirme
description: Klasik uyarı kurallarınızı geçirmek için gönüllü geçiş aracını nasıl kullanacağınızı öğrenin.
author: yanivlavi
ms.author: yalavi
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: fa487bec49ab9faa0f7c3dce752a30e4440fb873
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102037685"
---
# <a name="use-the-voluntary-migration-tool-to-migrate-your-classic-alert-rules"></a>Klasik uyarı kurallarınızı geçirmek için gönüllü geçiş aracını kullanın

[Daha önce duyurulduğu](monitoring-classic-retirement.md)gibi, Azure izleyici 'deki klasik uyarılar, genel bulut kullanıcıları için devre dışı bırakılır, ancak yine de **31 Mayıs 2021** tarihine kadar sınırlı kullanımda. Azure Kamu bulutu ve Azure Çin 21Vianet için klasik uyarılar, **29 şubat 2024** tarihinde devre dışı bırakılacak.

Bir geçiş aracı, klasik uyarı kuralları kullanan ve geçiş yapmak isteyen müşterilere Azure portal kullanılabilir. Bu makalede geçiş aracının nasıl kullanılacağı açıklanmaktadır.

## <a name="before-you-migrate"></a>Geçirmeden önce

Geçiş işlemi, klasik uyarı kurallarını yeni, denk uyarı kurallarına dönüştürür ve eylem gruplarını oluşturur. Hazırlık bölümünde aşağıdaki noktalara dikkat edin:

- Bildirim yükü biçimi ve yeni uyarı kuralları oluşturmak ve yönetmek için API 'Ler, daha fazla özelliği desteklediklerinden klasik uyarı kurallarından farklıdır. [Geçişe hazırlanma hakkında bilgi edinin](alerts-prepare-migration.md).

- Bazı klasik uyarı kuralları araç kullanılarak geçirilemez. [Hangi kuralların geçirilemeyeceğini ve bunlarla ne yapılacağını öğrenin](alerts-understand-migration.md#manually-migrating-classic-alerts-to-newer-alerts).

    > [!NOTE]
    > Geçiş işlemi, klasik uyarı kurallarınızın değerlendirmesini etkilemez. Bunlar geçirilmeden ve yeni uyarı kuralları etkinleşene kadar uyarıları çalıştırmaya ve gönderilmeye devam ederler.

## <a name="how-to-use-the-migration-tool"></a>Geçiş aracını kullanma

Klasik uyarı kurallarınızın Azure portal geçişini tetiklemek için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com), **izleme**' yi seçin.

1. **Uyarılar**' ı seçin ve ardından **Uyarı kurallarını yönet** ' i seçin veya **Klasik uyarıları görüntüleyin**.

1. Geçiş giriş sayfasına gitmek için **Yeni kurallara geçir** ' i seçin. Bu sayfa, tüm aboneliklerinizin ve geçiş durumlarının listesini gösterir:

    ![Ekran görüntüsü, uyarı kurallarını geçir sayfasını gösterir.](media/alerts-using-migration-tool/migration-landing.png "Kuralları geçir")

    Aracı kullanılarak geçirilebilen tüm abonelikler geçişe **hazırlanıyor** olarak işaretlenir.

    > [!NOTE]
    > Geçiş Aracı, klasik uyarı kuralları kullanan tüm aboneliklerde aşamalar halinde kullanıma alınıyor. Piyasaya sürülmeye yönelik erken aşamalarda, geçiş için yok olarak işaretlenmiş bazı abonelikler görebilirsiniz.

1. Bir veya daha fazla abonelik seçin ve ardından **geçiş önizlemesi**' ni seçin.

    Sonuçta ortaya çıkan sayfada bir abonelik için aynı anda geçirilecek olan klasik uyarı kurallarının ayrıntıları gösterilir. Ayrıca, ayrıntıları CSV biçiminde almak için **bu aboneliğin geçiş ayrıntılarını indir** ' i de seçebilirsiniz.

    ![Ekran görüntüsü, bu aboneliğin geçiş ayrıntılarını Indirme bağlantısı olan uyarı kurallarını geçir sayfasını gösterir ve geçiş bildirimi için e-posta belirtebilirsiniz.](media/alerts-using-migration-tool/migration-preview.png "Geçiş önizlemesi")

1. Geçiş durumu hakkında bildirim almak için bir veya daha fazla e-posta adresi belirtin. Geçiş tamamlandığında veya sizin için herhangi bir eylemde bulunmanız durumunda e-posta alacaksınız.

1. **Geçişi Başlat**' ı seçin. Onay iletişim kutusunda gösterilen bilgileri okuyun ve geçiş işlemini başlatmaya hazırsınız olduğunu onaylayın.

    > [!IMPORTANT]
    > Bir abonelik için geçiş işlemini başlattıktan sonra, bu abonelik için klasik uyarı kuralları düzenleyemez veya oluşturamazsınız. Bu kısıtlama, yeni kurallara geçiş sırasında klasik uyarı kurallarında hiçbir değişiklik olmamasını sağlar. Klasik uyarı kurallarınızı değiştiremeyeceksiniz, ancak geçirilene kadar uyarıları çalıştırmaya devam eder. Aboneliğiniz için geçiş tamamlandıktan sonra, artık klasik uyarı kurallarını kullanamazsınız.

    ![Ekran görüntüsünde, devam etmeden önce daha fazla bilgi edinmek için bağlantılarla ilgili önemli bilgiler de dahil olmak üzere geçişinizin onay istemi gösterilir](media/alerts-using-migration-tool/migration-confirm.png "Geçişin başlamasını Onayla")

1. Geçiş tamamlandığında veya eylem yapmanız gerekiyorsa, daha önce belirttiğiniz adreslerde bir e-posta alırsınız. Ayrıca, portalda geçiş giriş sayfasında düzenli olarak durumu denetleyebilirsiniz.

## <a name="frequently-asked-questions"></a>Sık sorulan sorular

### <a name="why-is-my-subscription-listed-as-not-ready-for-migration"></a>Aboneliğim neden geçiş için yok olarak listelendi?

Geçiş Aracı, aşamalar halinde müşterilere kullanıma alınıyor. Erken aşamalarda, aboneliklerinizin çoğu veya hepsi **geçiş için hazırlanma** olarak işaretlenebilir. 

Bir abonelik geçişe hazır hale geldiğinde, aboneliğin sahibi aracın kullanılabildiğini belirten bir e-posta iletisi alır. Bu ileti için bir göz atın.

### <a name="who-can-trigger-the-migration"></a>Geçişi kimlerin tetikleyebilen?

Abonelik düzeyinde kendisine atanmış Izleme katılımcısı rolü olan kullanıcılar geçişi tetikleyebilir. [Geçiş işlemi Için Azure rol tabanlı erişim denetimi hakkında daha fazla bilgi edinin](alerts-understand-migration.md#who-can-trigger-the-migration).

### <a name="how-long-will-the-migration-take"></a>Geçiş ne kadar sürer?

Çoğu abonelik için bir saat altında geçiş tamamlanır. Geçiş giriş sayfasında geçiş ilerlemesini izleyebilirsiniz. Geçiş sırasında, uyarılarınızın klasik uyarılar sisteminde veya yeni bir veritabanında çalışmaya devam ettiğinden emin olun.

### <a name="what-can-i-do-if-i-run-into-a-problem-during-migration"></a>Geçiş sırasında bir sorunla karşılaşsam ne yapabilirim?

Geçiş sırasında karşılaşabileceğiniz sorunlar hakkında yardım almak için [sorun giderme kılavuzuna](alerts-understand-migration.md#common-problems-and-remedies) bakın. Geçişi tamamlayabilmeniz için herhangi bir işlem yapmanız gerekiyorsa, aracı ayarlarken girdiğiniz e-posta adreslerinde size bildirilir.

## <a name="next-steps"></a>Sonraki adımlar

- [Geçiş için hazırlama](alerts-prepare-migration.md)
- [Geçiş aracının nasıl çalıştığını anlama](alerts-understand-migration.md)