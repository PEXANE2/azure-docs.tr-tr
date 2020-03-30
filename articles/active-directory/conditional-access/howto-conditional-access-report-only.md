---
title: Koşullu Erişim ilkesini yalnızca rapor modunda yapılandırma - Azure Etkin Dizini
description: Koşullu Erişim'de yalnızca rapor modunu kullanma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: conceptual
ms.date: 03/25/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3d9bec829d7fb3e76eb243bda73423303670585e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80295123"
---
# <a name="configure-a-conditional-access-policy-in-report-only-mode-preview"></a>Yalnızca rapor modunda Koşullu Erişim ilkesini yapılandırma (Önizleme)

Koşullu Erişim ilkesini yalnızca rapor modunda yapılandırmak için:

> [!IMPORTANT]
> Kuruluşunuz zaten yapmadıysa, [Azure Monitor tümleştirmesi ile Azure AD'yi ayarlayın.](#set-up-azure-monitor-integration-with-azure-ad) Verilerin gözden geçirilen kullanıma sunulmadan önce bu işlem yapılmalıdır.

1. Koşullu Erişim yöneticisi, güvenlik yöneticisi veya genel yönetici olarak **Azure portalında** oturum açın.
1. **Azure Etkin Dizin** > **Güvenliği** > **Koşullu Erişim'e**göz atın.
1. **Yeni ilke**yi seçin.
1. İlke koşullarını ve gerekli hibe denetimlerini gerektiği gibi yapılandırın.
1. **Etkinleştir ilkesi** altında yalnızca **Rapor** moduna geçiş ayarlayın.
1. **Kaydet**’i seçin

> [!TIP]
> Varolan bir **ilkenin Etkinleştir ilke** durumunu **Yalnızca** **Rapor'a** kadar etkinleştirme durumunu ancak bunu yapmak ilke yürütmeyi devre dışı kağınızda bulabilirsiniz. 

Yalnızca rapor sonucunu Azure AD oturum açma günlüklerinde görüntüleyin.

Belirli bir oturum açma için yalnızca rapor ilkesinin sonucunu görüntülemek için:

1. Rapor okuyucusu, güvenlik okuyucusu, güvenlik yöneticisi veya genel yönetici olarak **Azure portalında** oturum açın.
1. **Azure Etkin Dizin** > **Oturum Açma'ya**göz atın.
1. Sonuçları daraltmak için oturum açma veya filtre ekleme seçeneğini belirleyin.
1. **Ayrıntılar** çekmecesinde, oturum açma sırasında değerlendirilen ilkeleri görüntülemek için **yalnızca Rapor (Önizleme)** sekmesini seçin.

> [!NOTE]
> Oturum Açma günlüklerini indirirken, Koşullu Erişim raporu sonuç verilerini eklemek için JSON biçimini seçin.

## <a name="set-up-azure-monitor-integration-with-azure-ad"></a>Azure AD ile Azure Monitör tümleştirmesi ayarlama

Yeni Koşullu Erişim Öngörüleri çalışma kitabını kullanarak Koşullu Erişim ilkelerinin toplam etkisini görüntülemek için Azure Monitor'u Azure AD ile tümleştirmeniz ve oturum açma günlüklerini dışa aktarmanız gerekir. Bu tümleştirmeyi ayarlamak için iki adım vardır: 

1. [Azure Monitör aboneliği için kaydolun ve bir çalışma alanı oluşturun.](/azure/azure-monitor/learn/quick-create-workspace)
1. [Oturum açma günlüklerini Azure AD'den Azure Monitor'a dışa aktarın.](/azure/active-directory/reports-monitoring/howto-integrate-activity-logs-with-log-analytics)

Azure Monitor [fiyatlandırması](https://azure.microsoft.com/pricing/details/monitor/)hakkında daha fazla bilgiyi Azure Monitor fiyatlandırma sayfasında bulabilirsiniz. Maliyetleri tahmin etmek, günlük kap ayarlamak veya veri saklama süresini özelleştirmek için kaynaklar makalede bulunabilir, [Azure Monitör Günlükleri ile kullanımı ve maliyetleri yönet.](../../azure-monitor/platform/manage-cost-storage.md#estimating-the-costs-to-manage-your-environment)

## <a name="view-conditional-access-insights-workbook"></a>Koşullu Erişim Öngörüleri çalışma kitabını görüntüleme

Azure AD günlüklerinizi Azure Monitor ile tümleştirdikten sonra, yeni Koşullu Erişim öngörüleri çalışma kitaplarını kullanarak Koşullu Erişim ilkelerinin etkisini izleyebilirsiniz.

1. **Azure portalında** güvenlik yöneticisi veya genel yönetici olarak oturum açın.
1. **Azure Etkin Dizin** > **Çalışma Kitaplarına**göz atın.
1. **Koşullu Erişim Öngörüleri (Önizleme)** seçeneğini belirleyin.
1. **Koşullu Erişim İlkesi** açılır tarihinden bir veya daha fazla ilke seçin. Etkinleştirilmiş tüm ilkeler varsayılan olarak seçilir.
1. Bir zaman aralığı seçin (zaman aralığı kullanılabilir veri kümesini aşıyorsa, rapor kullanılabilir tüm verileri gösterir). **Koşullu Erişim İlkesi** ve **Zaman Aralığı** parametrelerini ayarladıktan sonra rapor yüklenir.
   1. İsteğe bağlı olarak, raporun kapsamını daraltmak için tek tek **Kullanıcıları** veya **Uygulamaları** arayın.
1. Zaman aralığındaki verileri kullanıcı sayısına veya oturum açma sayısına göre görüntüleme arasında seçim yaparak seçim yaptı.
1. **Veri görünümüne**bağlı olarak, **Etki Özeti** seçilen parametreler kapsamında kullanıcı veya oturum açma sayısını görüntüler, Toplam sayı, **Başarı**, **Başarısızlık**, Kullanıcı **eylemi gerekli**ve **uygulanmadı.** Belirli bir sonuç türünün oturum açmalarını incelemek için bir döşeme seçin. 
   1. Çalışma kitabı parametrelerini değiştirdiyseniz, bir kopyasını ileride kullanmak üzere kaydetmeyi seçebilirsiniz. Raporun üst kısmındaki kaydet simgesini seçin ve kaydetmek için bir ad ve konum sağlayın.
1. Her koşul için oturum açma çözümünü görüntülemek için aşağı kaydırın.
1. Yukarıdaki seçimlere göre filtrelenen tek tek oturum açma olaylarını araştırmak için raporun altındaki Oturum Açma **Ayrıntılarını** görüntüleyin.

> [!TIP] 
> Belirli bir sorguyu detaylandırmanız veya oturum açma ayrıntılarını aktarmanız mı gerekiyor? Log Analytics'te sorguyu açmak için herhangi bir sorgunun sağındaki düğmeyi seçin. CSV veya Power BI'ye dışa aktarmak için Dışa Aktar'ı seçin.

## <a name="common-problems-and-solutions"></a>Sık karşılaşılan sorunlar ve çözümler

### <a name="why-are-the-queries-in-the-workbook-failing"></a>Çalışma kitabındaki sorgular neden başarısız oluyor?

Müşteriler, yanlış veya birden çok çalışma alanı çalışma kitabıyla ilişkiliyse sorguların bazen başarısız olduğunu fark etmişolur. Bu sorunu gidermek için çalışma kitabının üst kısmındaki **Edit'i** ve ardından Ayarlar dişlisini tıklatın. Çalışma kitabıyla ilişkili olmayan çalışma alanlarını seçin ve kaldırın. Her çalışma kitabıyla ilişkili yalnızca bir çalışma alanı olmalıdır.

### <a name="why-doesnt-the-conditional-access-policies-dropdown-parameter-contain-my-policies"></a>Koşullu Erişim İlkeleri açılır parametresi neden ilkelerimi içermiyor?

Koşullu Erişim İlkeleri açılır, en son oturum açma ları 4 saatlik bir süre içinde sorgulayarak doldurulur. Kiracının son 4 saat içinde herhangi bir oturum açma ması yoksa, açılan süre boş olabilir. Bu gecikme, sık oturum açma ları olan küçük kiracılar gibi kalıcı bir sorunsa, yöneticiler Koşullu Erişim İlkeleri açılır kaydı için sorguyu güncelleyebilir ve sorgunun süresini 4 saatten uzun bir süreye kadar uzatabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Koşullu Erişim ortak ilkeleri](concept-conditional-access-policy-common.md)

Azure AD çalışma kitapları hakkında daha fazla bilgi için makaleye bakın, [Azure Etkin Dizin raporları için Azure Monitor çalışma kitapları nasıl kullanılır.](../reports-monitoring/howto-use-azure-monitor-workbooks.md)
