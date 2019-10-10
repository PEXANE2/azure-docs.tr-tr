---
title: Azure Application Insights hızlı başlangıç | Microsoft Docs
description: Application Insights ve App Center izleme için bir mobil uygulamayı hızlı bir şekilde ayarlamaya yönelik yönergeler sağlar
services: application-insights
keywords: ''
author: mrbullwinkle
ms.author: mbullwin
ms.date: 06/26/2019
ms.service: application-insights
ms.reviewer: daviste
ms.custom: mvc
ms.topic: quickstart
manager: carmonm
ms.openlocfilehash: 2c198ce8af17912d3174269b6cced96b5c1ac8bd
ms.sourcegitcommit: 824e3d971490b0272e06f2b8b3fe98bbf7bfcb7f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/10/2019
ms.locfileid: "72243242"
---
# <a name="start-analyzing-your-mobile-app-with-app-center-and-application-insights"></a>App Center ve Application Insights mobil uygulamanızı analiz etmeye başlayın

Bu hızlı başlangıç, uygulamanızın App Center örneğini Application Insights bağlama konusunda size rehberlik eder. Application Insights ile, App Center [analiz](https://docs.microsoft.com/mobile-center/analytics/) hizmetinde kullanılabilir olandan daha güçlü araçlarla telemetrinizi sorgulayabilir, segmentleyerek, filtreleyip analiz edebilirsiniz.

## <a name="prerequisites"></a>Prerequisites

Bu hızlı başlangıcı tamamlayabilmeniz için şunlar gerekir:

- Bir Azure aboneliği.
- İOS, Android, Xamarin, Evrensel Windows veya tepki verme yerel uygulaması.
 
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-up-with-app-center"></a>App Center kaydolun
Başlamak için bir hesap oluşturun ve [App Center kaydolun](https://appcenter.ms/signup?utm_source=ApplicationInsights&utm_medium=Azure&utm_campaign=docs).

## <a name="onboard-to-app-center"></a>App Center ekleme

Mobil uygulamanızla Application Insights kullanabilmeniz için uygulamanızı [App Center](https://docs.microsoft.com/mobile-center/)eklemeniz gerekir. Application Insights, mobil uygulamanızdan doğrudan telemetri almaz. Bunun yerine, uygulamanız App Center için özel olay telemetrisi gönderir. Daha sonra, App Center bu özel olayların kopyalarını sürekli olarak Application Insights olaylar alındığından dışa aktarır. (Bu, [APPLICATION INSIGHTS js SDK 'sı](https://github.com/Microsoft/ApplicationInsights-JS) için veya Telemetriyi doğrudan Application Insights ' e gönderilen [tepki verme yerel eklentisine](https://github.com/Microsoft/ApplicationInsights-JS/tree/master/vNext/extensions/applicationinsights-react-native) uygulanmaz.)

Uygulamanızı eklemek için uygulamanızın desteklediği her platform için App Center hızlı başlangıcı ' nı izleyin. Her platform için ayrı App Center örnekleri oluşturun:

* [iOS](https://docs.microsoft.com/mobile-center/sdk/getting-started/ios).
* [Android](https://docs.microsoft.com/mobile-center/sdk/getting-started/android).
* [Xamarin](https://docs.microsoft.com/mobile-center/sdk/getting-started/xamarin).
* [Evrensel Windows](https://docs.microsoft.com/mobile-center/sdk/getting-started/uwp).
* [Yerel olarak tepki](https://docs.microsoft.com/mobile-center/sdk/getting-started/react-native)verin.

## <a name="track-events-in-your-app"></a>Uygulamanızdaki olayları izleyin

Uygulamanız App Center için eklendi sonra, App Center SDK kullanılarak özel olay telemetrisi gönderecek şekilde değiştirilmesi gerekir. Özel olaylar, Application Insights 'ye aktarılmış tek App Center telemetri türüdür.

İOS uygulamalarından özel olayları göndermek için App Center SDK 'sında `trackEvent` veya `trackEvent:withProperties` yöntemlerini kullanın. [İOS uygulamalarından olayları izleme hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/mobile-center/sdk/analytics/ios)

```Swift
MSAnalytics.trackEvent("Video clicked")
```

Android uygulamalarından özel olayları göndermek için App Center SDK 'sında `trackEvent` yöntemini kullanın. [Android uygulamalarından olayları izleme hakkında daha fazla bilgi edinin.](https://docs.microsoft.com/mobile-center/sdk/analytics/android)

```Java
Analytics.trackEvent("Video clicked")
```

Diğer uygulama platformlarından özel olayları göndermek için App Center SDK 'lerinde `trackEvent` yöntemlerini kullanın.

Özel olaylarınızın alındığından emin olmak için App Center **analiz** bölümünün altındaki **Olaylar** sekmesine gidin. Olayların, uygulamanızdan gönderildiklerinde gösterilmesi birkaç dakika sürebilir.

## <a name="create-an-application-insights-resource"></a>Application Insights kaynağı oluşturma

Uygulamanız özel olayları gönderdikten ve bu olaylar App Center tarafından alındıktan sonra, Azure portal bir App Center türü Application Insights kaynağı oluşturmanız gerekir:

1. [Azure Portal](https://portal.azure.com/)oturum açın.
2. @No__t **kaynak oluştur**-1**Geliştirici Araçları** > **Application Insights**' yı seçin.

    > [!NOTE]
    > İlk kez bir Application Insights kaynağı oluşturuyorsanız [Application Insights kaynak oluştur belge oluştur](https://docs.microsoft.com/azure/azure-monitor/app/create-new-resource) ' a giderek daha fazla bilgi edinebilirsiniz.

    Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmanız için aşağıdaki tabloyu kullanın.

    | Ayarlar        |  Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Ad**      | "MyApp-iOS" gibi bazı genel benzersiz değerler | İzlemekte olduğunuz uygulamayı tanımlayan ad |
     | **Kaynak grubu**     | Yeni bir kaynak grubu veya menüden var olan bir grup | Yeni Application Insights kaynağın oluşturulacağı kaynak grubu |
   | **Konum** | Menüden bir konum | Size yakın bir konum seçin veya uygulamanızın nerede barındırıldığını neredeyse |

3. **Oluştur**'u tıklatın.

Uygulamanız birden çok platformu (iOS, Android, vb.) destekliyorsa, her platform için bir tane olmak üzere ayrı Application Insights kaynakları oluşturmak en iyisidir.

## <a name="export-to-application-insights"></a>Application Insights dışarı aktar

Yeni Application Insights kaynağında **genel bakış** sayfasında. İzleme anahtarını kaynağından kopyalayın.

Uygulamanızın [App Center](https://appcenter.ms/) örneğinde:

1. **Ayarlar** sayfasında, **dışarı aktar**' a tıklayın.
2. **Yeni dışarı aktarma**' yı seçin, **Application Insights**seçin ve **Özelleştir**' e tıklayın.
3. Application Insights izleme anahtarınızı kutuya yapıştırın.
4. Application Insights kaynağınızı içeren Azure aboneliğinin kullanımının arttırılmasının onayı. Her Application Insights kaynak, ayda alınan ilk 1 GB veri için ücretsizdir. [Application Insights fiyatlandırması hakkında daha fazla bilgi edinin.](https://azure.microsoft.com/pricing/details/application-insights/)

Uygulamanızın desteklediği her platform için bu işlemi tekrarlamaya unutmayın.

[Dışarı aktarma](https://docs.microsoft.com/mobile-center/analytics/export) kurulduktan sonra, App Center tarafından alınan her özel olay Application Insights kopyalanır. Olayların Application Insights gelmesi birkaç dakika sürebilir, bu nedenle hemen gösterilmediğinde daha fazla tanı yapmadan önce bir bit bekleyin.

İlk kez bağlandığınızda daha fazla veri sağlamak için App Center içindeki özel olayların en son 48 saati Application Insights otomatik olarak verilir.

## <a name="start-monitoring-your-app"></a>Uygulamanızı izlemeye başlayın

Application Insights, uygulamalarınızdan özel olay telemetrisini, App Center analiz araçlarının ötesinde sorgulayabilir, segmentleyip çözümleyebilir ve analiz edebilir.

1. **Özel olay telemetrinizi sorgulayın.** Application Insights **genel bakış** sayfasında **Günlükler (Analiz)** öğesini seçin.

   Application Insights kaynağınız ile ilişkili Application Insights günlükleri (Analiz) portalı açılır. Günlükler (Analiz) portalı Log Analytics sorgu dilini kullanarak verilerinizi doğrudan sorgulamanıza olanak tanıyarak, uygulamanız ve kullanıcılarınızla ilgili rastgele karmaşık sorular sorabilirsiniz.
   
   Günlükler (Analiz) portalında yeni bir sekme açın, sonra aşağıdaki sorguyu yapıştırın. Son 24 saat içinde uygulamanızdan her bir özel olayı kaç farklı kullanıcının gönderdiğini, bu ayrı sayımlar halinde sıralanmış bir sayı döndürür.

   ```AIQL
   customEvents
   | where timestamp >= ago(24h)
   | summarize dcount(user_Id) by name 
   | order by dcount_user_Id desc 
   ```

   ![Günlükler (Analiz) portalı](./media/mobile-center-quickstart/analytics-portal-001.png)

   1. Metin düzenleyicisinde sorgu üzerinde herhangi bir yere tıklayarak sorguyu seçin.
   2. Sonra sorguyu çalıştırmak için **Git** ' e tıklayın. 

   [Application Insights Analytics](../../azure-monitor/app/analytics.md) ve [Log Analytics sorgu dili](https://aka.ms/LogAnalyticsLanguageReference)hakkında daha fazla bilgi edinin.


2. **Özel olay telemetrinizi segmentlere ayırın ve filtreleyin.** Application Insights **genel bakış** sayfasında, Içindekiler tablosunda **Kullanıcılar** ' ı seçin.

   ![Kullanıcılar araç simgesi](./media/mobile-center-quickstart/users-icon-001.png)

   Kullanıcılar Aracı, uygulamanızın kaç kullanıcının belirli düğmelere tıkladığını, belirli ekranları ziyaret edildiğini veya App Center SDK ile bir olay olarak izlemekte olduğunuz başka herhangi bir eylemi gerçekleştirmiş olduğunu gösterir. App Center olaylarınızı segmentleyip filtrelemeye yönelik bir yol arıyorsanız, kullanıcılar aracı harika bir seçimdir.

   ![Kullanıcılar aracı](./media/mobile-center-quickstart/users-001.png) 

   Örneğin, **bölme** ölçütü açılır menüsünde **ülke veya bölge** ' yi seçerek, kullanımınızı coğrafya ile segmentleyebilirsiniz.

3. **Uygulamanızda dönüştürme, bekletme ve gezinme düzenlerini çözümleyin.** Application Insights **genel bakış** sayfasında, içindekiler tablosunda **Kullanıcı akışları** ' i seçin.

   ![Kullanıcı Akışları aracı](./media/mobile-center-quickstart/user-flows-001.png)

   Kullanıcı Akışları Aracı, kullanıcıların bir başlangıç olayından sonra hangi olayların gönderileceğini görselleştirir. Kullanıcıların uygulamanızda nasıl gezindiğinden ilgili genel bir resim almak için faydalıdır. Ayrıca, çok sayıda kullanıcının uygulamanızdan aldığı yerleri açığa çıkarır veya aynı eylemleri ve üzerine tekrarlayan şekilde tekrarda bulunabilir.

   Kullanıcı Akışları ek olarak, Application Insights belirli soruları yanıtlamak için birkaç farklı Kullanıcı davranışı analiz aracı vardır:

   * Dönüştürme oranlarını analiz etmek ve izlemek için **funlar** .
   * Uygulamanızın zaman içindeki kullanıcıları ne kadar iyi koruduğunu çözümlemek için **bekletme** .
   * Görsel öğeleri ve metinleri paylaşılabilir bir raporda birleştirmek için **çalışma kitapları** .
   * Daha sonra diğer analiz araçlarından kolayca başvurulabilmeleri için belirli kullanıcı veya olay gruplarını adlandırmak ve kaydetmek üzere **Cohorts** .

## <a name="clean-up-resources"></a>Kaynakları Temizleme

App Center Application Insights kullanmaya devam etmek istemiyorsanız, App Center dışarı aktarmayı kapatın ve Application Insights kaynağını silin. Bu, Application Insights bu kaynak için daha fazla ücretlendirileceğini engeller.

App Center dışarı aktarmayı devre dışı bırakmak için:

1. App Center, **Ayarlar** ' a gidin ve **dışarı aktar**' ı seçin.
2. Silmek istediğiniz Application Insights dışarı aktarmayı tıklatın, ardından alt kısımdaki **dışarı aktarmayı Sil** ' e tıklayın ve onaylayın.

Application Insights kaynağını silmek için:

1. Azure portal sol taraftaki menüsünde **kaynak grupları** ' na tıklayın ve ardından Application Insights kaynağınızın oluşturulduğu kaynak grubunu seçin.
2. Silmek istediğiniz Application Insights kaynağını açın. Sonra kaynağın üst menüsünde **Sil** ' e tıklayın ve onaylayın. Bu işlem, Application Insights aktarılmış verilerin kopyasını kalıcı olarak siler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Müşterilerin uygulamanızı nasıl kullandığını anlayın](../../azure-monitor/app/usage-overview.md)
