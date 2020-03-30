---
title: Azure Bildirim Hub'larında bırakılan bildirimleri tanılama
description: Azure Bildirim Hub'larında bırakılan bildirimlerle ilgili sık karşılaşılan sorunları nasıl tanılayabilirsiniz öğrenin.
services: notification-hubs
documentationcenter: Mobile
author: sethmanheim
manager: femila
editor: jwargo
ms.assetid: b5c89a2a-63b8-46d2-bbed-924f5a4cce61
ms.service: notification-hubs
ms.workload: mobile
ms.tgt_pltfrm: NA
ms.devlang: multiple
ms.topic: article
ms.date: 02/25/2020
ms.author: sethm
ms.reviewer: jowargo
ms.lastreviewed: 04/04/2019
ms.openlocfilehash: 1f3c16e6fe1855cf7882d83e620c70d15ce3cb92
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77657592"
---
# <a name="diagnose-dropped-notifications-in-azure-notification-hubs"></a>Azure Bildirim Hub'larında bırakılan bildirimleri tanılama

Azure Bildirim Hub'ları ile ilgili yaygın bir soru, bir uygulamadan gelen bildirimler istemci aygıtlarında görünmediğinde nasıl sorun giderilendir. Müşteriler bildirimlerin nerede ve neden bırakıldığını ve sorunun nasıl düzeltilebildiğini bilmek ister. Bu makalede, bildirimlerin neden bırakılan veya aygıtlar tarafından alınamayan neden olduğu belirlenir. Ayrıca, kök nedenini nasıl belirleyeceklerini de açıklar.

Bildirim Hub'larının bildirimleri bir aygıta nasıl iliştirdi sini anlamak çok önemlidir.

![Bildirim Hub'ları mimarisi][0]

Tipik bir bildirim akışında, ileti uygulama arka *ucundan* Bildirim Hub'larına gönderilir. Bildirim Hub'ları tüm kayıtları işler. Hedefleri belirlemek için yapılandırılan etiketleri ve etiket ifadelerini dikkate alır. Hedefler, anında iletme bildirimini alması gereken kayıtlardır. Bu kayıtlar desteklenen platformlarımızdan herhangi birini kaplayabilir: Android, Baidu (Çin'deki Android cihazlar), Fire OS (Amazon) iOS, Windows ve Windows Phone.

Belirlenen hedeflerle, Bildirim Hub'ları bildirimleri aygıt platformu için *anında iletme bildirim hizmetine* iter. Bunlara örnek olarak iOS ve macOS için Apple Push Bildirim hizmeti (APN' ler) ve Android cihazlar için Firebase Cloud Messaging (FCM) verilebilir. Bildirim Hub'ları, bildirimleri birden çok kayıt grubu arasında bölmeye iter. Azure portalında belirlediğiniz kimlik bilgilerini temel alan ilgili anında iletme bildirim hizmetiyle ilgili anında kimlik doğrulama hizmetiyle **doğrulatır.** Anında iletme bildirimi hizmeti daha sonra bildirimleri ilgili *istemci aygıtlarına*iletilir.

Bildirim tesliminin son ayağı platformun anında iletme bildirim hizmeti ile aygıt arasındadır. Bildirim teslimi, anında bildirim işleminin dört aşamasından herhangi birinde başarısız olabilir (istemci, uygulama arka uç, Bildirim Hub'ları ve platformun anında iletme bildirimi hizmeti). Bildirim Hub'ları mimarisi hakkında daha fazla bilgi için [Bildirim Hub'larına genel bakış]bölümüne bakın.

Bildirimlerin teslim edilmemesi ilk test/evreleme aşamasında oluşabilir. Bu aşamada bırakılan bildirimler bir yapılandırma sorunu gösterebilir. Üretimde bildirim lerin teslim edilmemesi durumunda, bildirimlerin bazıları veya tümü bırakılabilir. Bu durumda daha derin bir uygulama veya ileti deseni sorunu gösterilir.

Sonraki bölümde bildirimlerin sık karşılaşılandan nadire kadar bırakılabileceği senaryolar bakar.

## <a name="notification-hubs-misconfiguration"></a>Bildirim Hub'ları yanlış yapılandırma

Bildirimleri ilgili anında iletme bildirimi hizmetine göndermek için Bildirim Hub'larının uygulamanız bağlamında kendisini doğrulaması gerekir. Hedef platformun bildirim hizmeti (Microsoft, Apple, Google, vb.) içeren bir geliştirici hesabı oluşturmanız gerekir. Daha sonra, başvurunuzu hedef PNS ile çalışmak için kullandığınız bir belirteç veya anahtar aldığınız işletim sistemi ile kaydetmeniz gerekir.

Azure portalına platform kimlik bilgileri eklemeniz gerekir. Aygıta hiçbir bildirim erişilmezse, ilk adım bildirim hub'larında doğru kimlik bilgilerinin yapılandırıldığından emin olmaktır. Kimlik bilgileri, platforma özgü bir geliştirici hesabı altında oluşturulan uygulamayla eşleşmelidir.

Bu işlemi tamamlamak için adım adım yönergeleri için Azure [Bildirim Hub'ları ile başlayın'a]bakın.

Denetlemek için bazı yaygın yanlış yapılandırmalar şunlardır:

### <a name="notification-hub-name-location"></a>Bildirim merkezi adı konumu

Bildirim hub adınızın (yazım hatası olmadan) bu konumların her birinde aynı olduğundan emin olun:

* İstemciden kaydolduğunuz yer
* Arka uçtan bildirim gönderdiğiniz yer
* Anında bildirim hizmeti kimlik bilgilerini yapılandırma

İstemci ve uygulama arka ucunda doğru paylaşılan erişim imza yapılandırma dizeleri kullandığınızdan emin olun. Genellikle, istemcide **DefaultListenSharedAccessSignature** ve uygulamanın arka ucunda **VarsayılanFullSharedAccessSignature** kullanmanız gerekir. Bu, Bildirim Hub'larına bildirim gönderme izni verir.

### <a name="apn-configuration"></a>APN yapılandırması

Biri üretim, diğeri test için olmak. Bir kum havuzu ortamında kullandığınız sertifikayı üretimde kullanacağınız sertifika/hub'dan ayrı bir hub'a yüklemeniz gerekir. Aynı hub'a farklı türde sertifikalar yüklemeye çalışmayın. Bildirim hatalarına neden olur.

Yanlışlıkla aynı hub'a farklı türde sertifikalar yüklerseniz, hub'ı silmeniz ve yeni bir hub ile yeni bir başlangıç yapmalısınız. Hub'ı siyamıyorsanız, en azından hub'daki tüm kayıtları silmeniz gerekir.

### <a name="fcm-configuration"></a>FCM yapılandırması

1. Firebase'den elde ettiğiniz *sunucu anahtarının* Azure portalında kaydettiğiniz sunucu anahtarıyla eşleştiğinden emin olun.

   ![Firebase sunucu anahtarı][3]

2. İstemci üzerinde **Proje Kimliği** yapılandırdığınızdan emin olun. Firebase panosundan **Project ID** değerini alabilirsiniz.

   ![Firebase Proje Kimliği][1]

## <a name="application-issues"></a>Uygulama sorunları

### <a name="tags-and-tag-expressions"></a>Etiketler ve etiket ifadeleri

Hedef kitlenizi segmente etmek için etiketler veya etiket ifadeleri kullanıyorsanız, bildirimi gönderdiğinizde hedef bulunmaz. Bu hata, gönderme çağrınızda belirtilen etiketleri veya etiket ifadelerini temel alınarak yapılır.

Bir bildirim gönderdiğinizde etiketlerin eşleştiğinden emin olmak için kayıtlarınızı gözden geçirin. Ardından, yalnızca bu kayıtlara sahip istemcilerden gelen bildirim makbuzu doğrulayın.

Örneğin, Bildirim Hub'larına yapılan tüm kayıtlarınızda "Politika" etiketini kullandıklarını varsayalım. Daha sonra "Spor" etiketiyle bir bildirim gönderirseniz, bildirim hiçbir cihaza gönderilmez. Karmaşık bir durum, "Etiket A" *veya* "Tag B" kullanarak kaydettiğiniz ancak "Tag A && Tag B"yi hedeflediğiniz etiket ifadelerini içerebilir. Makalenin ilerleyen bölümlerinde kendi kendine teşhis ipuçları bölümünde, kayıtlarınızı ve etiketlerinizi nasıl inceleyeceğiz gösterilmektedir.

### <a name="template-issues"></a>Şablon sorunları

Şablonlar kullanıyorsanız, [Şablonlar'da]açıklanan yönergelere uyduğunuzdan emin olun.

### <a name="invalid-registrations"></a>Geçersiz kayıtlar

Bildirim hub'ı doğru yapılandırıldıysa ve etiketler veya etiket ifadeleri doğru kullanıldıysa, geçerli hedefler bulunur. Bildirimler bu hedeflere gönderilmelidir. Bildirim Hub'ları daha sonra paralel olarak birkaç işleme toplu işlemini kapatır. Her toplu iş bir kayıt kümesine ileti gönderir.

> [!NOTE]
> Bildirim Hub'ları toplu işlemleri paralel olarak işlediğiiçin, bildirimlerin teslim edildiği sıra garanti edilmez.

Bildirim Hub'ları "en çok kez" ileti teslim modeli için optimize edi. Bir cihaza birden fazla bildirim intikal etmesi için çoğaltmagirişiminde bulunun. Kayıtlar, anında iletme bildirimi hizmetine gönderilmeden önce aygıt tanımlayıcısı başına yalnızca bir ileti gönderildiğinden emin olmak için denetlenir.

Her bir toplu iş, kayıtları kabul eden ve doğrulayan anında iletme bildirim hizmetine gönderilir. Bu işlem sırasında, anında iletme bildirimi hizmetinin bir toplu iş partisinde bir veya daha fazla kayıtta bir hata algılaması mümkündür. Anında iletme bildirimi hizmeti daha sonra bildirim hub'larına bir hata döndürür ve işlem durur. Anında iletme bildirimi hizmeti, bu toplu işlemi tamamen düşürür. Bu, özellikle TCP akışı protokolü kullanan APN'ler için geçerlidir.

Bu durumda, hata kaydı veritabanından kaldırılır. Daha sonra, o partideki diğer aygıtlar için bildirim dağıtımını yeniden deneriz.

Bir kayda karşı başarısız teslim girişimi hakkında daha fazla hata bilgisi almak için, Bildirim Hub'ları REST API'leri [Mesaj Telemetrisi başına kullanabilirsiniz: Bildirim iletisi telemetrisi](https://docs.microsoft.com/rest/api/notificationhubs/get-notification-message-telemetry) ve [PNS geri bildirimi](https://msdn.microsoft.com/library/azure/mt705560.aspx)alın. Örnek kod için [REST Gönder örneğine](https://github.com/Azure/azure-notificationhubs-dotnet/tree/master/Samples/SendRestExample/)bakın.

## <a name="push-notification-service-issues"></a>Anında bildirim hizmeti sorunları

Anında iletme bildirimi hizmeti bildirimi aldıktan sonra, bildirimi aygıta teslim eder. Bu noktada, Bildirim Hub'larının bildirimin aygıta teslimi üzerinde hiçbir denetimi yoktur.

Platform bildirim hizmetleri sağlam olduğundan, bildirimler aygıtlara birkaç saniye içinde ulaşma eğilimindedir. Anında iletme bildirimi hizmeti azaltıyorsa, Bildirim Hub'ları üstel bir geri tepme stratejisi uygular. Anında iletme bildirimi hizmeti30 dakika boyunca erişilemeyemezse, süresi dolacak ve iletileri kalıcı olarak bırakmak için bir ilke vardır.

Bir anında iletme bildirimi hizmeti bir bildirim sunmaya çalışırsa ancak aygıt çevrimdışıysa, bildirim anında iletme bildirimi hizmeti tarafından depolanır. Sadece sınırlı bir süre için saklanır. Bildirim, aygıt kullanılabilir olduğunda aygıta teslim edilir.

Her uygulama yalnızca bir son bildirim depolar. Aygıt çevrimdışıyken birden çok bildirim gönderilirse, her yeni bildirim sonuncusunun atılmasına neden olur. Yalnızca en yeni bildirimi tutmak, APN'lerde *birleşerek* FCM'de *çökmeye* denir. (FCM daralan bir anahtar kullanır.) Aygıt uzun süre çevrimdışı kaldığında, aygıt için depolanan bildirimler atılır. Daha fazla bilgi için [APN'lere Genel Bakış] ve [FCM iletileri hakkında bilgi verebilirsiniz.]

Bildirim Hub'ları ile, genel SendNotification API'sini kullanarak bir HTTP üstbilgisi aracılığıyla birleştirme anahtarını geçirebilirsiniz. Örneğin, .NET SDK `SendNotificationAsync`için. SendNotification API'si, ilgili anında iletme bildirimi hizmetine olduğu gibi geçirilen HTTP üstbilgilerini de alır.

## <a name="self-diagnosis-tips"></a>Kendi kendine teşhis ipuçları

Bildirim Hub'larında bırakılan bildirimlerin temel nedenini tanılamak için yollar aşağıda veda edilir.

### <a name="verify-credentials"></a>Kimlik bilgilerini doğrulama

#### <a name="push-notification-service-developer-portal"></a>Anında bildirim hizmeti geliştirici portalı

İlgili anında iletme bildirimi hizmeti geliştirici portalındaki kimlik bilgilerini doğrulayın (APN'ler, FCM, Windows Bildirim Hizmeti vb.). Daha fazla bilgi için [Bkz. Öğretici: Azure Bildirim Hub'larını kullanarak Evrensel Windows Platformu uygulamalarına bildirim gönderin.](https://docs.microsoft.com/azure/notification-hubs/notification-hubs-windows-store-dotnet-get-started-wns-push-notification)

#### <a name="azure-portal"></a>Azure portalında

Kimlik bilgilerini anında bildirim hizmeti geliştirici portalından elde edilenlerle gözden geçirmek ve eşleştirmek için Azure portalındaki **Erişim İlkeleri** sekmesine gidin.

![Azure portalı Erişim İlkeleri][4]

### <a name="verify-registrations"></a>Kayıtları doğrulama

#### <a name="visual-studio"></a>Visual Studio

Visual Studio'da, Bildirim Hub'ları da dahil olmak üzere birden çok Azure hizmetlerini görüntülemek ve yönetmek için Server Explorer aracılığıyla Azure'a bağlanabilirsiniz. Bu kısayol öncelikle geliştirme/test ortamınız için yararlıdır.

![Visual Studio Server Explorer][9]

![Sunucu Gezgini](media/notification-hubs-push-notification-fixer/vsserverexplorer2.png)

Merkezinizdeki tüm kayıtları görüntüleyebilir ve yönetebilirsiniz. Kayıtlar platforma, yerel veya şablon kaydına, etiketine, anında bildirim hizmeti tanımlayıcısı, kayıt kimliği ve son kullanma tarihine göre sınıflandırılabilir. Ayrıca bu sayfada bir kayıt edinebilirsiniz. Özellikle etiketleri düzenlemek için kullanışlıdır.

**Server Explorer'da**bildirim merkezinizi sağ tıklatın ve **Tanıla'yı**seçin. 

![Visual Studio Server Explorer: Tanı menüsü](./media/notification-hubs-push-notification-fixer/diagnose-menu.png)

Aşağıdaki sayfayı görüyorsunuz:

![Visual Studio: Tanı lama sayfası](./media/notification-hubs-push-notification-fixer/diagnose-page.png)

**Cihaz Kayıtları** sayfasına geçin:

![Visual Studio: Cihaz Kayıtları](./media/notification-hubs-push-notification-fixer/VSRegistrations.png)

Test bildirimi iletisi göndermek için **Test Gönder** sayfasını kullanabilirsiniz:

![Visual Studio: Test Gönder](./media/notification-hubs-push-notification-fixer/test-send-vs.png)

> [!NOTE]
> Kayıtları yalnızca geliştirme/test sırasında ve sınırlı sayıda kayıtla birlikte yeniden oluşturmak için Visual Studio'yı kullanın. Kayıtlarınızı toplu olarak değiştirmeniz gerekiyorsa, Nasıl Kullanılır: Toplu Olarak [Kayıt İhraç ve Değiştir'de](https://msdn.microsoft.com/library/dn790624.aspx)açıklanan dışa aktarma ve alma kaydı işlevini kullanmayı düşünün.

#### <a name="service-bus-explorer"></a>Service Bus Explorer

Birçok müşteri bildirim merkezlerini görüntülemek ve yönetmek için [Service Bus Explorer'ı](https://github.com/paolosalvatori/ServiceBusExplorer) kullanır. Service Bus Explorer açık kaynak kodlu bir projedir. 

### <a name="verify-message-notifications"></a>İleti bildirimlerini doğrulama

#### <a name="azure-portal"></a>Azure portalında

**Destek + SORUN GIDERME**altında, bir hizmet geri sonu ve çalışan olmadan müşterilerinize bir test bildirimi göndermek için, **Test Gönder**seçin.

![Azure'da Gönder işlevini test edin][7]

#### <a name="visual-studio"></a>Visual Studio

Visual Studio'dan test bildirimleri de gönderebilirsiniz.

![Visual Studio'da Test Gönder işlevi][10]

Visual Studio Server Explorer ile Bildirim Hub'larını kullanma hakkında daha fazla bilgi için şu makalelere bakın:

* [Bildirim hub'ları için cihaz kayıtları nasıl görüntülenir?](https://docs.microsoft.com/previous-versions/windows/apps/dn792122(v=win.10))
* [Derin dalış: Visual Studio 2013 Güncelleme 2 RC ve Azure SDK 2.3]
* [Visual Studio 2013 Update 3 ve Azure SDK 2.4'ün duyurusu]

### <a name="debug-failed-notifications-and-review-notification-outcome"></a>Hata ayıklama bildirimleri ve gözden geçirme bildirimi sonucu başarısız

#### <a name="enabletestsend-property"></a>EnableTestSend özelliği

Bildirim Hub'ları aracılığıyla bildirim gönderdiğinde, bildirim başlangıçta sıraya alınır. Bildirim Hub'ları doğru hedefleri belirler ve bildirimi anında iletme bildirimi hizmetine gönderir. REST API'sini veya istemci SDK'larından herhangi birini kullanıyorsanız, gönderme çağrınızın iadesi yalnızca iletinin Bildirim Hub'larıyla sıraya alındığı anlamına gelir. Bildirim Hub'ları sonunda bildirimi anında iletme bildirimi hizmetine gönderdiğinde ne olduğu hakkında bilgi sağlamaz.

Bildiriminiz istemci aygıta ulaşmazsa, Bildirim Hub'ları bildirim hizmetine iletmeye çalıştığında bir hata oluşmuş olabilir. Örneğin, taşıma yükü boyutu anında bildirim hizmetitarafından izin verilen maksimum tutarı aşabilir veya Bildirim Hub'larında yapılandırılan kimlik bilgileri geçersiz olabilir.

Anında iletme bildirimi hizmeti hataları hakkında bilgi edinmek için [EnableTestSend] özelliğini kullanabilirsiniz. Portaldan veya Visual Studio istemcisinden test iletileri gönderdiğinde bu özellik otomatik olarak etkinleştirilir. Ayrıntılı hata ayıklama bilgilerini görmek ve api'ler aracılığıyla da bu özelliği kullanabilirsiniz. Şu anda ,NET SDK'da kullanabilirsiniz. Sonunda tüm istemci SDK'larına eklenecektir.

REST çağrısıyla `EnableTestSend` özelliği kullanmak için, gönderme çağrınızın sonuna *test* adı verilen bir sorgu dize parametresi ekleyin. Örnek:

```text
https://mynamespace.servicebus.windows.net/mynotificationhub/messages?api-version=2013-10&test
```

#### <a name="net-sdk-example"></a>.NET SDK örneği

Yerel bir pop-up (tost) bildirimi göndermek için .NET SDK'yı kullanma örneği aşağıda verilmiştir:

```csharp
NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName);
var result = await hub.SendWindowsNativeNotificationAsync(toast);
Console.WriteLine(result.State);
```

Yürütmenin sonunda, `result.State` sadece devletler `Enqueued`. Sonuçlar, anında iletme bildiriminize ne olduğu hakkında herhangi bir fikir vermez.

Sonra, `EnableTestSend` Boolean özelliğini kullanabilirsiniz. Bildirim `EnableTestSend` gönderildiğinde oluşan `NotificationHubClient` anında iletme bildirimi hizmeti hataları hakkında ayrıntılı bir durum elde etmek için başlatma sırasında özelliği kullanın. Bildirimin anında iletme bildirimi hizmetine teslim edilmesi için önce Bildirim Hub'larına ihtiyaç duyduğundan, gönderme çağrısının döndürülmesi ek süre alır.

```csharp
    bool enableTestSend = true;
    NotificationHubClient hub = NotificationHubClient.CreateClientFromConnectionString(connString, hubName, enableTestSend);

    var outcome = await hub.SendWindowsNativeNotificationAsync(toast);
    Console.WriteLine(outcome.State);

    foreach (RegistrationResult result in outcome.Results)
    {
        Console.WriteLine(result.ApplicationPlatform + "\n" + result.RegistrationId + "\n" + result.Outcome);
    }
```

#### <a name="sample-output"></a>Örnek çıktı

```text
DetailedStateAvailable
windows
7619785862101227384-7840974832647865618-3
The Token obtained from the Token Provider is wrong
```

Bu ileti, Bildirim Hub'larında yapılandırılan kimlik bilgilerinin geçersiz olduğunu veya hub'daki kayıtlarla ilgili bir sorun olduğunu gösterir. Bu kaydı silin ve istemcinin iletiyi göndermeden önce kaydı yeniden oluşturmasına izin verin.

> [!NOTE]
> Özelliğin `EnableTestSend` kullanımı ağır şekilde azaltılır. Bu seçeneği yalnızca geliştirme/test ortamında ve sınırlı sayıda kayıtla kullanın. Hata ayıklama bildirimleri yalnızca 10 aygıta gönderilir. Hata ayıklama göndermeişlemlerinin dakikası 10'da bir sınırı da vardır.

### <a name="review-telemetry"></a>Telemetri yi gözden geçirin

#### <a name="azure-portal"></a>Azure portalında

Portalda, bildirim merkezinizdeki tüm etkinliklere hızlı bir genel bakış elde edebilirsiniz.

1. Genel **Bakış** sekmesinde, kayıtların, bildirimlerin ve hataların platforma göre toplu bir görünümünü görebilirsiniz.

   ![Bildirim Hub'larına genel bakış panosu][5]

2. **Monitör** sekmesinde, daha derin bir görünüm için platforma özgü diğer birçok ölçüm ekleyebilirsiniz. Bildirim Hub'ları bildirimi anında iletme bildirimi hizmetine göndermeye çalıştığında döndürülen hatalara özellikle bakabilirsiniz.

   ![Azure portal etkinlik günlüğü][6]

3. **Gelen İletileri,** **Kayıt İşlemleri'ni**ve **Başarılı Bildirimleri**gözden geçirerek başlayın. Ardından, anında iletme bildirimi hizmetine özgü hataları gözden geçirmek için platform başına sekmesine gidin.

4. Bildirim hub'ınızın kimlik doğrulama ayarları yanlışsa, **Ileti PNS Kimlik Doğrulama Hatası** görüntülenir. Anında iletme hizmeti kimlik bilgilerini denetlemek iyi bir göstergedir.

#### <a name="programmatic-access"></a>Programlı erişim

Programlı erişim hakkında daha fazla bilgi için [Programlı erişime](https://docs.microsoft.com/previous-versions/azure/azure-services/dn458823(v=azure.100))bakın.

> [!NOTE]
> API'ler aracılığıyla kayıt ve telemetri erişiminin dışa aktarılabilen ve içe aktarılabilen çeşitli telemetri özellikleri yalnızca Standart hizmet katmanında kullanılabilir. Bu özellikleri Ücretsiz veya Temel hizmet katmanından kullanmaya çalışırsanız, SDK'yı kullanırsanız bir özel durum iletisi alırsınız. Özellikleri doğrudan REST API'lerinden kullanırsanız HTTP 403 (Yasak) hatası alırsınız.
>
> Telemetri ile ilgili özellikleri kullanmak için öncelikle Azure portalında Standart hizmet katmanını kullandığınızdan emin olun.  

<!-- IMAGES -->
[0]: ./media/notification-hubs-push-notification-fixer/Architecture.png
[1]: ./media/notification-hubs-push-notification-fixer/FCMConfigure.png
[3]: ./media/notification-hubs-push-notification-fixer/FCMServerKey.png
[4]: ../../includes/media/notification-hubs-portal-create-new-hub/notification-hubs-connection-strings-portal.png
[5]: ./media/notification-hubs-push-notification-fixer/PortalDashboard.png
[6]: ./media/notification-hubs-push-notification-fixer/PortalAnalytics.png
[7]: ./media/notification-hubs-ios-get-started/notification-hubs-test-send.png
[8]: ./media/notification-hubs-push-notification-fixer/VSRegistrations.png
[9]: ./media/notification-hubs-push-notification-fixer/vsserverexplorer.png
[10]: ./media/notification-hubs-push-notification-fixer/VSTestNotification.png

<!-- LINKS -->
[Bildirim Hub'larına genel bakış]: notification-hubs-push-notification-overview.md
[Azure Bildirim Hub'ları ile başlayın]: notification-hubs-windows-store-dotnet-get-started-wns-push-notification.md
[Şablon]: https://msdn.microsoft.com/library/dn530748.aspx
[APN'lere genel bakış]: https://developer.apple.com/library/content/documentation/NetworkingInternet/Conceptual/RemoteNotificationsPG/APNSOverview.html
[FCM iletileri hakkında]: https://firebase.google.com/docs/cloud-messaging/concept-options
[Export and modify registrations in bulk]: https://msdn.microsoft.com/library/dn790624.aspx
[Service Bus Explorer code]: https://code.msdn.microsoft.com/windowsazure/Service-Bus-Explorer-f2abca5a
[View device registrations for notification hubs]: https://msdn.microsoft.com/library/windows/apps/xaml/dn792122.aspx
[Derin dalış: Visual Studio 2013 Güncelleme 2 RC ve Azure SDK 2.3]: https://azure.microsoft.com/blog/2014/04/09/deep-dive-visual-studio-2013-update-2-rc-and-azure-sdk-2-3/#NotificationHubs
[Visual Studio 2013 Update 3 ve Azure SDK 2.4'ün duyurusu]: https://azure.microsoft.com/blog/2014/08/04/announcing-release-of-visual-studio-2013-update-3-and-azure-sdk-2-4/
[EnableTestSend]: https://docs.microsoft.com/dotnet/api/microsoft.azure.notificationhubs.notificationhubclient.enabletestsend?view=azure-dotnet
[Programmatic telemetry access]: https://msdn.microsoft.com/library/azure/dn458823.aspx
