---
title: Remote Rendering Oturumları
description: Uzaktan Işleme oturumunun ne olduğunu açıklar
author: jakrams
ms.author: jakras
ms.date: 02/21/2020
ms.topic: conceptual
ms.custom: devx-track-csharp
ms.openlocfilehash: 2e7f441a727bea8d1a5d401fb5e9757dee1044fc
ms.sourcegitcommit: f845ca2f4b626ef9db73b88ca71279ac80538559
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/09/2020
ms.locfileid: "89613806"
---
# <a name="remote-rendering-sessions"></a>Remote Rendering Oturumları

Azure uzaktan Işleme (ARR) 'de *oturum* , önemli bir kavramdır. Bu makalede, tam olarak bir oturum olduğu açıklanır.

## <a name="overview"></a>Genel Bakış

Azure uzaktan Işleme, karmaşık işleme görevlerinin buluta yük devrederden işe yarar. Çoğu bulut sunucusunun GPU 'ları olmadığından, bu işleme görevleri yalnızca herhangi bir sunucu tarafından yerine getirilemiyor. Dahil edilen veri miktarı ve etkileşimli çerçeve tarifelerinde sonuçlar üretmenin sabit gereksinimi nedeniyle, daha yaygın web trafiği için mümkün olabilecek şekilde, hangi kullanıcının istek üzerine başka bir makineye de ulaşamadığı bir sorumluluktur.

Bu, Azure uzaktan Işleme kullandığınızda, gerekli donanım özelliklerine sahip bir bulut sunucusunun yalnızca işleme isteklerinizi işlemek için ayrılmış olması gerekir. Bir *oturum* , bu sunucuyla etkileşime geçen her şeyi ifade eder. Kullanım için bir makineyi ayırma (*kira*) başlangıç isteğiyle başlar, model yükleme ve düzenleme için tüm komutlarla devam eder ve kirayı bulut sunucusuna serbest bırakarak sona erer.

## <a name="managing-sessions"></a>Oturumları yönetme

Oturumları yönetmek ve bunlarla etkileşim kurmak için birden çok yol vardır. Oturumların oluşturulması, güncelleştirilmesi ve kapatılmasından bağımsız olarak, [oturum yönetimi REST API](../how-tos/session-rest-api.md). C# ve C++ ' da, bu işlemler sınıflar ve sınıfları aracılığıyla `AzureFrontend` sunulur `AzureSession` . Unity uygulamalarında, bileşen tarafından sağlanan başka yardımcı program işlevleri de vardır `ARRServiceUnity` .

Etkin bir oturuma *bağlandıktan* sonra, [model yükleme](models.md) ve sahne ile etkileşim kurma gibi işlemler sınıfı aracılığıyla sunulur `AzureSession` .

### <a name="managing-multiple-sessions-simultaneously"></a>Birden çok oturumu eşzamanlı olarak yönetme

Bir cihazdan birden çok oturuma tam olarak *bağlanmak* mümkün değildir. Ancak, tek bir uygulamadan istediğiniz sayıda oturum oluşturabilir, gözlemleyebilirsiniz ve kapatabilirsiniz. Uygulamanın bir oturuma hiçbir zaman bağlanmadığından, her iki durumda da HoloLens 2 gibi bir cihazda çalışması gerekmez. Bu tür bir uygulama için kullanım örneği, bir merkezi mekanizmasıyla oturumları denetlemek istediğinizde olabilir. Örneğin, birisi birden çok tablette ve HoloLens cihazlarının oturum açabilbileceği bir Web uygulaması oluşturabilir. Daha sonra uygulama, tabletlerdeki, hangi CAD modelinin görüntüleneceği gibi seçenekleri görüntüleyebilir. Bir Kullanıcı bir seçim yaptığında, bu bilgiler paylaşılan bir deneyim oluşturmak için tüm HoloLens cihazlarına iletilir.

## <a name="session-phases"></a>Oturum aşamaları

Her oturum birden çok aşamaya gider.

### <a name="session-startup"></a>Oturum başlatma

ARR 'nin [Yeni bir oturum oluşturmasını](../how-tos/session-rest-api.md#create-a-session)istediğinizde, ilk şey bir oturum [UUID 'si](https://en.wikipedia.org/wiki/Universally_unique_identifier)döndürmemelidir. Bu UUID, oturum hakkındaki bilgileri sorgulamanızı sağlar. UUID ve oturumla ilgili bazı temel bilgiler 30 gün boyunca kalıcıdır, bu sayede oturum durdurulduktan sonra bile bu bilgileri sorgulayabilirsiniz. Bu noktada, **oturum durumu** **Başlangıç**olarak bildirilir.

Ardından, Azure uzaktan Işleme oturumunuzu barındırabileceğinizi bir sunucu bulmaya çalışır. Bu arama için iki parametre vardır. İlk olarak, yalnızca [bölgenizdeki](../reference/regions.md)sunucuları ayıracaktır. Bunun nedeni, bölgeler arasında ağ gecikmesi, bir deneyim sağlamak için çok yüksek olabilir. İkinci faktör, belirlediğiniz istenen *boyutdır* . Her bölgede, [*Standart*](../reference/vm-sizes.md) veya [*Premium*](../reference/vm-sizes.md) boyut isteğini yerine getirebilecek sınırlı sayıda sunucu vardır. Sonuç olarak, istenen boyuttaki tüm sunucular Şu anda bölgenizde kullanılıyorsa, oturum oluşturma başarısız olur. Hatanın nedeni [sorgulanamaz](../how-tos/session-rest-api.md#get-sessions-properties).

> [!IMPORTANT]
> *Standart* bir sunucu boyutu istemeniz durumunda istek yüksek talebe göre başarısız olursa, bu da bir *Premium* sunucu isteğinin başarısız olacağını göstermez. Bu, sizin için bir seçenek ise, bir *Premium* sunucu boyutuna geri düşmesini deneyebilirsiniz.

Hizmet uygun bir sunucu bulduğunda, bir Azure uzaktan Işleme konağına dönüştürmek için doğru sanal makineyi (VM) üzerine kopyalamanız gerekir. Bu işlem birkaç dakika sürer. Daha sonra VM 'nin önyüklendi ve **oturum durumu** , **Ready**olarak geçiş yapar.

Bu noktada, sunucu özel olarak girişinizi bekliyor. Bu ayrıca hizmet için faturalandırılırsınız.

### <a name="connecting-to-a-session"></a>Bir oturuma bağlanma

Oturum *hazırlanıyor*, buna *bağlanabilirsiniz* . Bağlantı sırasında cihaz, modelleri yüklemek ve değiştirmek için komut gönderebilir. Her ARR ana bilgisayarı tek seferde yalnızca bir istemci cihaza hizmet veriyorsa, bir istemci bir oturuma bağlandığında, işlenmiş içerik üzerinde özel denetim sahibi olur. Bu Ayrıca, işleme performansının denetiminizin dışındaki nedenlerle hiçbir değişiklik olmayacağı anlamına gelir.

> [!IMPORTANT]
> Bir oturuma yalnızca bir istemci *bağlanabilse* de, oturumları hakkındaki temel bilgiler (örneğin, geçerli durumları) bağlanmadan sorgulanamaz.

Bir cihaz bir oturuma bağlıyken, diğer cihazların bağlanmasına yönelik girişimler başarısız olur. Ancak, bağlı cihazın bağlantısı kesildiğinde, gönüllü olarak veya bazı hata türleri nedeniyle, oturum başka bir bağlantı isteği kabul eder. Önceki tüm durum (yüklenen modeller ve bu gibi) atılır ve bir sonraki bağlantı aygıtı temiz bir tablet alır. Böylece oturumlar, farklı cihazlara göre birçok kez yeniden kullanılabilir ve çoğu durumda son kullanıcıdan oturum başlatma yükünü gizlemek mümkün olabilir.

> [!IMPORTANT]
> Uzak sunucu, istemci tarafı verilerinin durumunu hiçbir şekilde değiştirmez. Tüm veri (dönüştürme güncelleştirmeleri ve yükleme istekleri gibi) istemci uygulama tarafından gerçekleştirilmelidir. Tüm eylemler istemci durumunu hemen güncelleştirir.

### <a name="session-end"></a>Oturum sonu

Yeni bir oturum istediğinizde, genellikle bir ile sekiz saat aralığında bir *maksimum kira süresi*belirtirsiniz. Bu, konağın girişinizi kabul edeceği süredir.

Bir oturumun bitmesi için iki normal neden vardır. Oturumu durdurulacak veya en fazla kira süresinin sona ereceğini el ile istemeniz gerekir. Her iki durumda da, ana bilgisayara etkin olan bağlantı hemen kapatılır ve hizmet bu sunucuda kapatılır. Sunucu daha sonra Azure havuzuna geri verilir ve diğer amaçlar için bir talep alabilir. Bir oturumun durdurulması geri alınamaz veya iptal edilemez. Durdurulmuş bir oturumdaki oturum **durumunun** sorgulanması, el ile kapatılıp kapatılmadığına veya en fazla kira süresine ulaşılmasına bağlı olarak **durdurulmuş** veya **süresi sona ermeden**döndürülür.

Bir oturum, bazı hatalar nedeniyle durdurulmuş de olabilir.

Her durumda, bir oturum durdurulduğunda daha fazla faturalandırılmaz.

> [!WARNING]
> Bir oturuma bağlanıp, ne kadar süreyle faturalandırmayı etkilemediği. Hizmetin ödedikleriniz, *oturum süresine*bağlıdır, bu da bir sunucunun sizin için özel olarak ayırdığı zaman ve istenen donanım özellikleri ( [ayrılan boyut](../reference/vm-sizes.md)) anlamına gelir. Bir oturum başlatırsanız, beş dakikaya bağlanıp oturumu durdurmayın, böylece kira süresi dolana kadar çalışmaya devam eder, tam oturum kiralama süresi için faturalandırılırsınız. Buna karşılık, *en fazla kiralama süresi* genellikle bir güvenlik ağı olur. Sekiz saatlik bir kira süresi ile oturum isteyip istemenizden bağımsız olarak, oturumu daha sonra el ile durdurursanız beş dakika boyunca kullanın.

#### <a name="extend-a-sessions-lease-time"></a>Bir oturumun kira süresini genişletme

Etkin bir oturumun kira süresini, daha uzun bir süre sonra da ihtiyacınız olduğunu [uzatabilir](../how-tos/session-rest-api.md#modify-and-query-session-properties) .

## <a name="example-code"></a>Örnek kod

Aşağıdaki kod, bir oturum başlatma, *hazırlık* durumunu bekleme, bağlanma ve sonra yeniden bağlantı kesme ve kapatma gibi basit bir uygulama gösterir.

```cs
RemoteRenderingInitialization init = new RemoteRenderingInitialization();
// fill out RemoteRenderingInitialization parameters...

RemoteManagerStatic.StartupRemoteRendering(init);

AzureFrontendAccountInfo accountInfo = new AzureFrontendAccountInfo();
// fill out accountInfo details...

AzureFrontend frontend = new AzureFrontend(accountInfo);

RenderingSessionCreationParams sessionCreationParams = new RenderingSessionCreationParams();
// fill out sessionCreationParams...

AzureSession session = await frontend.CreateNewRenderingSessionAsync(sessionCreationParams).AsTask();

RenderingSessionProperties sessionProperties;
while (true)
{
    sessionProperties = await session.GetPropertiesAsync().AsTask();
    if (sessionProperties.Status != RenderingSessionStatus.Starting &&
        sessionProperties.Status != RenderingSessionStatus.Unknown)
    {
        break;
    }
    // REST calls must not be issued too frequently, otherwise the server returns failure code 429 ("too many requests"). So we insert the recommended delay of 10s
    await Task.Delay(TimeSpan.FromSeconds(10));
}

if (sessionProperties.Status != RenderingSessionStatus.Ready)
{
    // Do some error handling and either terminate or retry.
}

// Connect to server
Result connectResult = await session.ConnectToRuntime(new ConnectToRuntimeParams()).AsTask();

// Connected!

while(...)
{
    // per frame update

    session.Actions.Update();
}

// Disconnect
session.DisconnectFromRuntime();

// stop the session
await session.StopAsync().AsTask();

// shut down the remote rendering SDK
RemoteManagerStatic.ShutdownRemoteRendering();
```

Birden çok `AzureFrontend` ve `AzureSession` örnek, koddan korunabilir, değiştirilebilir ve sorgulanabilir. Ancak tek seferde yalnızca bir cihaz bağlanabilir `AzureSession` .

Bir sanal makinenin ömrü `AzureFrontend` örneğe veya örneğe bağlı değildir `AzureSession` . `AzureSession.StopAsync` bir oturumu durdurmak için çağrılmalıdır.

Kalıcı oturum KIMLIĞI ile `AzureSession.SessionUUID()` yerel olarak sorgulanabilir ve yerel olarak önbelleğe alınabilir. Bu KIMLIKLE, bir uygulama bu `AzureFrontend.OpenSession` oturuma bağlamak için çağrı yapabilir.

True olduğunda, `AzureSession.IsConnected` `AzureSession.Actions` `RemoteManager` [model yükleme](models.md), [varlıkları](entities.md)işleme ve işlenmiş sahneye ilişkin [sorgu bilgilerini](../overview/features/spatial-queries.md) içeren öğesinin bir örneğini döndürür.

## <a name="api-documentation"></a>API belgeleri

* [C# AzureSession sınıfı](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.azuresession)
* [C# AzureFrontend. CreateNewRenderingSessionAsync ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.azurefrontend.createnewrenderingsessionasync)
* [C# AzureFrontend. OpenRenderingSession ()](https://docs.microsoft.com/dotnet/api/microsoft.azure.remoterendering.azurefrontend.openrenderingsession)
* [C++ AzureSession sınıfı](https://docs.microsoft.com/cpp/api/remote-rendering/azuresession)
* [C++ AzureFrontend:: CreateNewRenderingSessionAsync](https://docs.microsoft.com/cpp/api/remote-rendering/azurefrontend#createnewrenderingsessionasync)
* [C++ AzureFrontend:: OpenRenderingSession](https://docs.microsoft.com/cpp/api/remote-rendering/azurefrontend#openrenderingsession)

## <a name="next-steps"></a>Sonraki adımlar

* [Varlıklar](entities.md)
* [Modeller](models.md)
