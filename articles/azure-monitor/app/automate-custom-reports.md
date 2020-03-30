---
title: Azure Application Insights verileriyle özel raporları otomatikleştirin
description: Azure Application Insights verileriyle özel günlük/haftalık/aylık raporları otomatikleştirin
ms.topic: conceptual
ms.date: 05/20/2019
ms.reviewer: sdash
ms.openlocfilehash: d91595a863901fcc420611ac644c7856e74320dd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77655132"
---
# <a name="automate-custom-reports-with-azure-application-insights-data"></a>Azure Application Insights verileriyle özel raporları otomatikleştirin

Süreli raporlar, bir ekibin iş açısından kritik hizmetlerinin nasıl olduğu konusunda bilgi sahibi tutulmasına yardımcı olur. Geliştiriciler, DevOps/SRE ekipleri ve yöneticileri, herkesin portalda oturum açmasına gerek kalmadan güvenilir bir şekilde öngörüler sunan otomatik raporlarla üretken olabilir. Bu tür raporlar, gecikme, yükleme veya arıza oranlarındaki ve herhangi bir uyarı kuralını tetiklemeyebilecek kademeli artışların belirlenmesine de yardımcı olabilir.

Her işletmenin benzersiz raporlama ihtiyaçları vardır, örneğin: 

* Bir raporda ölçümlerin veya özel ölçümlerin belirli yüzdelik toplamları.
* Farklı hedef kitleler için günlük, haftalık ve aylık veri toplama ları için farklı raporlara sahip olun.
* Bölge veya ortam gibi özel özniteliklere göre segmentasyon. 
* Farklı aboneliklerde veya kaynak gruplarında olsalar bile, bazı AI kaynaklarını tek bir raporda gruplayın.
* Seçici hedef kitleye gönderilen hassas ölçümleri içeren ayrı raporlar.
* Portal kaynaklarına erişimi olmayan hissedarlara raporlar.

> [!NOTE] 
> Haftalık Application Insights sindirmek e-posta herhangi bir özelleştirme izin vermedi ve aşağıda listelenen özel seçenekler lehine kesilecektir. Son haftalık özet e-posta 11 Haziran 2018 tarihinde gönderilecektir. Benzer özel raporlar almak için lütfen aşağıdaki seçeneklerden birini yapılandırın (aşağıda önerilen sorguyu kullanın).

## <a name="to-automate-custom-report-emails"></a>Özel rapor e-postalarını otomatikleştirmek için

Programda özel raporlar oluşturmak için Uygulama Öngörüleri verilerini [programlı olarak](https://dev.applicationinsights.io/) sorgulayabilirsiniz. Aşağıdaki seçenekler hızlı bir şekilde başlamanıza yardımcı olabilir:

* [Microsoft Flow ile raporları otomatikleştirin](automate-with-flow.md)
* [Logic Apps ile raporları otomatikleştirin](automate-with-logic-apps.md)
* İzleme senaryosunda "Uygulama Öngörüleri zamanlanmış özet" [Azure işlev](https://docs.microsoft.com/azure/azure-functions/functions-create-first-azure-function) şablonunu kullanın. Bu işlev, e-postayı teslim etmek için SendGrid'i kullanır. 

    ![Azure işlev şablonu](./media/automate-custom-reports/azure-function-template.png)

## <a name="sample-query-for-a-weekly-digest-email"></a>Haftalık özet e-posta için örnek sorgu
Aşağıdaki sorgu, rapor gibi haftalık özet e-posta için birden çok veri kümesi arasında birleşiyi gösterir. Gerektiği gibi özelleştirin ve haftalık raporu otomatikleştirmek için yukarıda listelenen seçeneklerden herhangi biriyle kullanın.   

```AIQL
let period=7d;
requests
| where timestamp > ago(period)
| summarize Row = 1, TotalRequests = sum(itemCount), FailedRequests = sum(toint(success == 'False')),
    RequestsDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
| join (
dependencies
| where timestamp > ago(period)
| summarize Row = 1, TotalDependencies = sum(itemCount), FailedDependencies = sum(success == 'False'),
    DependenciesDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row | join (
pageViews
| where timestamp > ago(period)
| summarize Row = 1, TotalViews = sum(itemCount)
) on Row | join (
exceptions
| where timestamp > ago(period)
| summarize Row = 1, TotalExceptions = sum(itemCount)
) on Row | join (
availabilityResults
| where timestamp > ago(period)
| summarize Row = 1, OverallAvailability = iff(isnan(avg(toint(success))), '------', tostring(toint(avg(toint(success)) * 10000) / 100.0)),
    AvailabilityDuration = iff(isnan(avg(duration)), '------', tostring(toint(avg(duration) * 100) / 100.0))
) on Row
| project TotalRequests, FailedRequests, RequestsDuration, TotalDependencies, FailedDependencies, DependenciesDuration, TotalViews, TotalExceptions, OverallAvailability, AvailabilityDuration
```

## <a name="application-insights-scheduled-digest-report"></a>Uygulama Öngörüleri zamanlanmış özet raporu

1. Azure portalından Kaynak > **İşlem** > **İşlevi Uygulaması** **Oluştur'u**seçin.

   ![Azure Kaynak İşlevi Uygulaması ekran görüntüsü oluşturma](./media/automate-custom-reports/function-app-01.png)

2. Uygulamanız için uygun bilgileri girin ve _Oluştur'u_seçin. (Uygulama Öngörüleri _Yalnızca_ Yeni İşlev Uygulama uygulamanızı Uygulama Öngörüleri ile izlemek istiyorsanız gereklidir)

   ![Azure Kaynak İşlevi Uygulama Ayarları ekran görüntüsü oluşturma](./media/automate-custom-reports/function-app-02.png)

3. Yeni İşlev Uygulamanız dağıtımı tamamladıktan sonra **kaynağa git'i**seçin.

4. **Yeni işlevi**seçin.

   ![Yeni bir İşlev ekran görüntüsü oluşturma](./media/automate-custom-reports/function-app-03.png)

5. Uygulama **_Öngörüleri zamanlanmış özet şablonuna_** seçin.

     > [!NOTE]
     > Varsayılan olarak, işlev uygulamaları runtime sürüm 2.x ile oluşturulur. Application Insights zamanlanmış özet şablonunu kullanmak için [Azure İşlevleri çalışma zamanı sürüm](https://docs.microsoft.com/azure/azure-functions/set-runtime-version) **1.x'i** hedeflemeniz gerekir.  ![runtime ekran görüntüsü](./../../../includes/media/functions-view-update-version-portal/function-app-view-version.png)



   ![Yeni Fonksiyon Uygulama Öngörüleri Şablonu ekran görüntüsü](./media/automate-custom-reports/function-app-04.png)

6. Raporunuz için uygun bir alıcı e-posta adresi girin ve **Oluştur'u**seçin.

   ![İşlev Ayarları ekran görüntüsü](./media/automate-custom-reports/function-app-05.png)

7. **İşlev Uygulama** > **Platformu özellikleri** > **uygulama ayarlarınızı**seçin.

    ![Azure İşlevi Uygulama ayarları ekran görüntüsü](./media/automate-custom-reports/function-app-07.png)

8. Uygun karşılık gelen değerlere ``AI_APP_ID`` ``AI_APP_KEY``sahip üç ``SendGridAPI``yeni uygulama ayarı oluşturun ve . **Kaydet'i**seçin.

     ![Fonksiyon tümleştirme arabirimi ekran görüntüsü](./media/automate-custom-reports/function-app-08.png)
    
    (AI_ değerleri, rapor vermek istediğiniz Uygulama Öngörüleri Kaynağı için API Erişimi altında bulunabilir. Uygulama Öngörüleri API Anahtarınız yoksa, **API Anahtarı oluşturma**seçeneği vardır .)
    
   * AI_APP_ID = Başvuru Kimliği
   * AI_APP_KEY = API Anahtarı
   * SendGridAPI =SendGrid API Anahtarı

     > [!NOTE]
     > SendGrid hesabınız yoksa bir hesap oluşturabilirsiniz. SendGrid'in Azure İşlevleri için belgeleri [burada.](https://docs.microsoft.com/azure/azure-functions/functions-bindings-sendgrid) Yalnızca SendGrid kurulumu ve bir API anahtarı oluşturmak için nasıl en az bir açıklama istiyorsanız bu makalenin sonunda sağlanır. 

9. **Tümleştir'i** seçin ve Çıktılar altında **SendGrid'i tıklatın ($return)**.

     ![Çıkış ekran görüntüsü](./media/automate-custom-reports/function-app-09.png)

10. **SendGridAPI Anahtar Uygulaması Ayarı**altında, **SendGridAPI**için yeni oluşturduğunuz Uygulama Ayarını seçin.

     ![İşlev Uygulaması ekran görüntüsünü çalıştır](./media/automate-custom-reports/function-app-010.png)

11. İşlev Uygulamanızı çalıştırın ve test edin.

     ![Test Ekran Görüntüsü](./media/automate-custom-reports/function-app-11.png)

12. İletinin başarılı bir şekilde gönderildiğini/alındığını doğrulamak için e-postanızı kontrol edin.

     ![E-posta konu satırı ekran görüntüsü](./media/automate-custom-reports/function-app-12.png)

## <a name="sendgrid-with-azure"></a>Azure ile SendGrid

Bu adımlar yalnızca zaten yapılandırılmış bir SendGrid hesabınız yoksa geçerlidir.

1. Azure portalından **SendGrid E-posta Teslimi** için **kaynak araması oluşturma** > > **Oluştur'u** tıklatın ve SendGrid'e özgü oluşturma yönergelerini doldurun' u seçin. 

     ![SendGrid Kaynak Ekran Görüntüsü Oluşturma](./media/automate-custom-reports/function-app-13.png)

2. SendGrid Hesapları altında oluşturulduktan sonra **Yönet'i**seçin.

     ![Ayarlar API Anahtar Ekran Görüntüsü](./media/automate-custom-reports/function-app-14.png)

3. Bu SendGrid'in sitesini başlatacak. **Ayarlar** > **API Tuşlarını**seçin.

     ![API Anahtar Uygulaması Ekran Görüntüsü Oluşturma ve Görüntüleme](./media/automate-custom-reports/function-app-15.png)

4. API Anahtarı > **& Görünümü Oluştur'u** seçin (API Anahtarınız için hangi izin düzeyinin uygun olduğunu belirlemek için lütfen SendGrid'in kısıtlı erişimle ilgili belgelerini gözden geçirin. Tam Erişim burada yalnızca amaçlar için seçilir.)

   ![Tam erişim ekran görüntüsü](./media/automate-custom-reports/function-app-16.png)

5. Anahtarın tamamını kopyalayın, Bu değer, İşlev Uygulaması ayarlarınızda ihtiyacınız olan şeydir, SendGridAPI'nin değeri olarak

   ![API anahtar ekran görüntüsünü kopyalama](./media/automate-custom-reports/function-app-17.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Analytics sorguları](../../azure-monitor/log-query/get-started-queries.md)oluşturma hakkında daha fazla bilgi edinin.
* [Uygulama Öngörüleri verilerini programlı olarak sorgulama](https://dev.applicationinsights.io/) hakkında daha fazla bilgi edinin
* [Logic Apps](https://docs.microsoft.com/azure/logic-apps/logic-apps-what-are-logic-apps) hakkında daha fazla bilgi edinin.
* [Microsoft Flow](https://ms.flow.microsoft.com)hakkında daha fazla bilgi edinin.
