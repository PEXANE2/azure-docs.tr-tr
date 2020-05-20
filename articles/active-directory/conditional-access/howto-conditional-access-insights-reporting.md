---
title: Koşullu erişim öngörüleri ve raporlama çalışma kitabı-Azure Active Directory
description: İlke sorunlarını gidermek için Azure AD koşullu erişim öngörülerini ve raporlama çalışma kitabını kullanma
services: active-directory
ms.service: active-directory
ms.subservice: conditional-access
ms.topic: article
ms.date: 05/01/2020
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: dawoo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 0c4feeca1cbe7eb88aace811829e4d9c2db5f38e
ms.sourcegitcommit: fdec8e8bdbddcce5b7a0c4ffc6842154220c8b90
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83641599"
---
# <a name="conditional-access-insights-and-reporting"></a>Koşullu erişim öngörüleri ve raporlama

Koşullu erişim öngörüleri ve raporlama çalışma kitabı, kuruluşunuzda koşullu erişim ilkelerinin zaman içinde etkisini anlamanıza olanak sağlar. Oturum açma sırasında, bir veya daha fazla koşullu erişim ilkesi uygulanabilir, bazı izin denetimleri karşılanmıyorsa veya erişimi reddettikten sonra erişim izni verebilir. Her oturum açma sırasında birden fazla koşullu erişim ilkesi değerlendirilebildiğinden, Öngörüler ve raporlama çalışma kitabı, tek bir ilkenin veya tüm ilkelerin bir alt kümesinin etkisini incelemenizi sağlar.  

## <a name="prerequisites"></a>Önkoşullar

Öngörüler ve raporlama çalışma kitabını etkinleştirmek için, kiracınızın oturum açma günlüğü verilerini sürdürmek için bir Log Analytics çalışma alanına sahip olması gerekir. Kullanıcıların koşullu erişimi kullanabilmesi için Azure AD Premium P1 veya P2 Lisansı olmalıdır.

Aşağıdaki roller öngörülere ve raporlamaya erişebilir:  

- Koşullu Erişim Yöneticisi 
- Güvenlik okuyucusu 
- Güvenlik yöneticisi 
- Genel okuyucu 
- Genel yönetici 

Kullanıcılar ayrıca aşağıdaki Log Analytics çalışma alanı rollerinden birine de ihtiyaç duyar:  

- Katılımcı  
- Sahip 

### <a name="stream-sign-in-logs-from-azure-ad-to-azure-monitor-logs"></a>Azure AD 'den Azure Izleyici günlüklerine oturum açma günlükleri akışı 

Azure AD günlüklerini Azure Izleyici günlükleriyle tümleştirdiyseniz, çalışma kitabı yüklenmeden önce aşağıdaki adımları uygulamanız gerekir:  

1. [Azure izleyici 'de bir Log Analytics çalışma alanı oluşturun](../../azure-monitor/learn/quick-create-workspace.md).
1. Azure [ad günlüklerini Azure izleyici günlükleriyle tümleştirin](../reports-monitoring/howto-integrate-activity-logs-with-log-analytics.md).

## <a name="how-it-works"></a>Nasıl çalışır? 

Öngörüler ve raporlama çalışma kitabına erişmek için:  

1. **Azure portalında** oturum açın.
1. **Azure Active Directory**  >  **güvenlik**  >  **koşullu erişim**  >  **öngörülerini ve raporlamayı**inceleyin.

### <a name="get-started-select-parameters"></a>Kullanmaya başlayın: parametreleri seçin 

Öngörüler ve raporlama panosu, bir veya daha fazla koşullu erişim ilkesinin belirli bir süre boyunca etkisini görmenizi sağlar. Çalışma kitabının en üstünde parametrelerin her birini ayarlayarak başlayın. 

![Azure portal koşullu erişim öngörüleri ve raporlama panosu](./media/howto-conditional-access-insights-reporting/conditional-access-insights-and-reporting-dashboard.png)

**Koşullu erişim ilkesi**: birleştirilmiş etkilerini görüntülemek için bir veya daha fazla koşullu erişim ilkesi seçin. İlkeler iki gruba ayrılmıştır: etkin ve yalnızca rapor ilkeleri. Varsayılan olarak, etkinleştirilen tüm ilkeler seçilidir. Bu etkinleştirilen ilkeler, kiracınızda uygulanmakta olan ilkelerdir.  

**Zaman aralığı**: 4 saat olarak 90 gün kadar bir zaman aralığı seçin. Azure AD günlüklerini Azure Izleyici ile tümleştirdikten sonra daha sonra bir zaman aralığı seçerseniz, yalnızca tümleştirme zamanından sonraki oturum açma işlemleri görüntülenir.  

**Kullanıcı**: Pano varsayılan olarak tüm kullanıcılar için seçili ilkelerin etkisini gösterir. Tek bir kullanıcıya göre filtrelemek için kullanıcının adını metin alanına yazın. Tüm kullanıcılara göre filtrelemek için metin alanına "tüm kullanıcılar" yazın veya parametreyi boş bırakın. 

**Uygulama**: varsayılan olarak, Pano tüm uygulamalar için seçili ilkelerin etkisini gösterir. Tek bir uygulamaya göre filtrelemek için, metin alanına uygulamanın adını yazın. Tüm uygulamalara göre filtrelemek için, metin alanına "tüm uygulamalar" yazın veya parametreyi boş bırakın. 

**Veri görünümü**: panonun sonuçları Kullanıcı sayısı veya oturum açma sayısı açısından göstermesini isteyip istemediğinizi seçin. Tek bir Kullanıcı, belirli bir zaman aralığı boyunca birçok farklı sonuç içeren birçok uygulama için yüzlerce oturum açma içerebilir. Kullanıcılar için veri görünümünü seçerseniz, bir kullanıcı hem başarı hem de hata sayılarına dahil edilebilir (örneğin, 10 Kullanıcı varsa, 8 ' in son 30 gün içinde başarılı bir sonucu olmuş olabilir ve 9 ' un son 30 gün içinde arıza sonucu almış olabilir).

## <a name="impact-summary"></a>Etki Özeti 

Parametreler ayarlandıktan sonra, etki Özeti yüklenir. Özet, seçilen ilkeler değerlendirildiğinde, zaman aralığı boyunca kaç kullanıcının veya oturum açma süresinin "başarılı", "başarısız", "Kullanıcı eylemi gerekli" veya "uygulanmadı" ile sonuçlanabileceğini gösterir.  

![Koşullu erişim çalışma kitabındaki etki Özeti](./media/howto-conditional-access-insights-reporting/workbook-impact-summary.png)

**Toplam**: seçili ilkelerin en az birinin değerlendirildiği zaman dilimi boyunca Kullanıcı veya oturum açma sayısı.

**Başarılı**: seçili ilkelerin birleştirilmiş sonucunun "başarılı" veya "yalnızca rapor: Success" olduğu zaman dilimi boyunca Kullanıcı veya oturum açma sayısı.

**Hata**: seçilen ilkelerden en az birinin sonucu "hata" veya "yalnızca rapor: hata" olduğu zaman diliminde Kullanıcı veya oturum açma sayısı.

**Kullanıcı eylemi gerekli**: seçili ilkelerin birleştirilmiş sonucunun "yalnızca rapor: Kullanıcı eylemi gerekli" olduğu zaman diliminde Kullanıcı veya oturum açma sayısı. Yalnızca rapor koşullu erişim ilkesi için Multi-Factor Authentication gibi bir etkileşimli izin denetimi Gerektiğinde Kullanıcı eylemi gereklidir. Etkileşimli verme denetimleri yalnızca rapor ilkeleri tarafından zorlanmadığından, başarı veya başarısızlık belirlenemez.  

**Uygulanmadı**: seçili ilkelerin hiçbirinin uygulanmadığı zaman diliminde Kullanıcı veya oturum açma sayısı.

### <a name="understanding-the-impact"></a>Etkiyi anlama 

![Koşul ve durum başına çalışma kitabı dökümü](./media/howto-conditional-access-insights-reporting/workbook-breakdown-condition-and-status.png)

Koşulların her biri için kullanıcıların veya oturum açma işlemlerinin dökümünü görüntüleyin. Çalışma kitabının en üstündeki Özet kutucuklarının seçimini yaparak belirli bir sonucun (örneğin, başarı veya başarısızlık) oturum açma işlemlerini filtreleyebilirsiniz. Her bir koşullu erişim koşulu için oturum açma işlemlerinin dökümünü görebilirsiniz: cihaz durumu, cihaz platformu, istemci uygulaması, konum, uygulama ve oturum açma riski.  

## <a name="sign-in-details"></a>Oturum açma ayrıntıları 

![Çalışma kitabı oturum açma ayrıntıları](./media/howto-conditional-access-insights-reporting/workbook-sign-in-details.png)

Ayrıca, panonun alt kısmındaki oturum açma işlemlerini arayarak belirli bir kullanıcının oturum açma işlemlerini araştırabilirsiniz. Soldaki sorgu en sık kullanılan kullanıcıları görüntüler. Bir kullanıcı seçildiğinde sorgu sağa filtreedilir.  

## <a name="troubleshooting"></a>Sorun giderme

### <a name="why-is-the-workbook-taking-a-long-time-to-load"></a>Çalışma kitabının yüklenmesi uzun sürüyor mu?  

Seçilen zaman aralığına ve kiracınızın boyutuna bağlı olarak, çalışma kitabı, genellikle çok sayıda oturum açma olayını değerlendiriyor olabilir. Büyük kiracılar için, oturum açma işlemlerinin hacmi Log Analytics sorgu kapasitesini aşabilir. Çalışma kitabının yüklenip yüklenolmadığını görmek için zaman aralığını 4 saate kadar kısaltmayı deneyin.  

### <a name="after-loading-for-a-few-minutes-why-is-the-workbook-returning-zero-results"></a>Birkaç dakika sonra, çalışma kitabı neden sıfır sonuçlar döndürüyor? 

Oturum açma birimi Log Analytics sorgu kapasitesini aşarsa çalışma kitabı sıfır sonuç döndürür. Çalışma kitabının yüklenip yüklenolmadığını görmek için zaman aralığını 4 saate kadar kısaltmayı deneyin.  

### <a name="can-i-save-my-parameter-selections"></a>Parametre seçimlerimi kaydedebilir miyim?  

**Azure Active Directory**çalışma  >  **kitaplarında**  >  **koşullu erişim öngörülerine ve raporlamaya**giderek, çalışma kitabının en üstündeki parametre seçimlerinizi kaydedebilirsiniz. Burada, çalışma kitabını düzenleyebileceğiniz ve parametre seçimleri de dahil olmak üzere çalışma alanınıza bir kopyayı, **raporlardaki** veya **paylaşılan raporlardaki**çalışma kitabında kaydedebileceğiniz çalışma kitabı şablonunu bulabilirsiniz. 

### <a name="can-i-edit-and-customize-the-workbook-with-additional-queries"></a>Çalışma kitabını ek sorgularla düzenleyebilir ve özelleştirebilir miyim? 

Çalışma kitabı **Azure Active Directory**  >  **Workbooks**  >  **koşullu erişim öngörülerini ve raporlamayı**Azure Active Directory giderek çalışma kitabını düzenleyebilir ve özelleştirebilirsiniz. Burada, çalışma kitabını düzenleyebileceğiniz ve parametre seçimleri de dahil olmak üzere çalışma alanınıza bir kopyayı, **raporlardaki** veya **paylaşılan raporlardaki**çalışma kitabında kaydedebileceğiniz çalışma kitabı şablonunu bulabilirsiniz. Sorguları düzenlemeye başlamak için çalışma kitabının en üstünde **Düzenle** ' ye tıklayın.  
 
## <a name="next-steps"></a>Sonraki adımlar

[Koşullu erişim yalnızca rapor modu](concept-conditional-access-report-only.md)
