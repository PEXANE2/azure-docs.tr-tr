---
title: Uzaktan İşleme Oturumları
description: Uzaktan İşleme oturumunun ne olduğunu açıklar
author: jakrams
ms.author: jakras
ms.date: 02/21/2020
ms.topic: conceptual
ms.openlocfilehash: 91a59e1398bf5e68799ad16a20dfb824904edc8a
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681694"
---
# <a name="remote-rendering-sessions"></a>Uzaktan İşleme Oturumları

Azure Uzaktan İşleme 'de (ARR) *oturum* önemli bir kavramdır. Bu makalede, bir oturumun tam olarak ne olduğu açıklanmaktadır.

## <a name="overview"></a>Genel Bakış

Azure Uzaktan İşlem, karmaşık işleme görevlerini buluta boşaltarak çalışır. Çoğu bulut sunucusunda GPU olmadığı için bu işleme görevleri herhangi bir sunucu tarafından yerine getirilememektedir. Söz konusu veri miktarı ve etkileşimli kare hızlarında sonuç üretmenin zor gereksinimi nedeniyle, daha yaygın web trafiği için mümkün olabileceği gibi, kullanıcı isteğinin de başka bir makineye teslim edilemediği sunucunun sorumluluğu.

Bu, Azure Uzaktan İşleme'yi kullandığınızda, gerekli donanım özelliklerine sahip bir bulut sunucusunun yalnızca işleme isteklerinizi işlemek için rezerve edilmesi gerektiği anlamına gelir. *Oturum,* bu sunucuyla etkileşimiçeren her şeyi ifade eder. Bu, kullanımınız için bir makineyi rezerve etmek *(kiralamak)* için ilk istekle başlar, modelleri yüklemek ve işlemek için tüm komutlarla devam eder ve bulut sunucusunda kirayı serbest bırakmakla sona erer.

## <a name="managing-sessions"></a>Oturumları yönetme

Oturumları yönetmenin ve oturumlarla etkileşimkurmanın birden çok yolu vardır. Oturum oluşturmanın, güncelleştirmenin ve kapatmanın dilden bağımsız yolu [oturum yönetimi REST API'dir.](../how-tos/session-rest-api.md) C# ve C++'da bu işlemler `AzureFrontend` sınıflar `AzureSession`ve . Unity uygulamaları için `ARRServiceUnity` bileşen tarafından sağlanan diğer yardımcı program işlevleri vardır.

Etkin bir oturuma *bağlandıktan* sonra, [modelleri yükleme](models.md) ve sahneyle etkileşim `AzureSession` gibi işlemler sınıf boyunca açığa alınır.

### <a name="managing-multiple-sessions-simultaneously"></a>Aynı anda birden çok oturumu yönetme

Tek bir cihazdan birden çok oturuma tam *olarak bağlanmak* mümkün değildir. Ancak, tek bir uygulamadan istediğiniz kadar oturum oluşturabilir, gözlemleyebilir ve kapatabilirsiniz. Uygulamanın bir oturuma bağlanması amaçlanmadığı sürece, HoloLens 2 gibi bir cihazda da çalıştırılması gerekmez. Oturumları merkezi bir mekanizma aracılığıyla denetlemek istiyorsanız, böyle bir uygulama için bir kullanım örneği olabilir. Örneğin, birden çok tablet ve HoloLenses oturum açabilirsiniz bir web uygulaması, inşa edebilirsiniz. Ardından uygulama tabletlerde hangi CAD modelinin görüntülenmesi gibi seçenekleri görüntüleyebilir. Bir kullanıcı bir seçim yaparsa, bu bilgiler paylaşılan bir deneyim oluşturmak için tüm HoloLenses'a iletilir.

## <a name="session-phases"></a>Oturum aşamaları

Her oturum birden fazla aşamadan geçer.

### <a name="session-startup"></a>Oturum başlatma

[ArR'dan yeni bir oturum oluşturmasını](../how-tos/session-rest-api.md#create-a-session)istediğinizde, yaptığı ilk şey bir oturumu [UUID](https://en.wikipedia.org/wiki/Universally_unique_identifier)döndürmektir. Bu UUID, oturumla ilgili bilgileri sorgulamanızı sağlar. UUID ve oturumla ilgili bazı temel bilgiler 30 gün süreyle devam ettirilir, böylece oturum durdurulduktan sonra bile bu bilgileri sorgulayabilirsiniz. Bu noktada, **oturum durumu** **Başlangıç**olarak bildirilecektir.

Ardından, Azure Uzaktan İşleme, oturumunuzu barındırabilecek bir sunucu bulmaya çalışır. Bu arama için iki parametre vardır. İlk olarak, yalnızca [bölgenizdeki](../reference/regions.md)sunucuları rezerve edecektir. Bunun nedeni, bölgeler arasında ağ gecikmesinin iyi bir deneyimi garanti edemeyecek kadar yüksek olmasıdır. İkinci faktör, belirttiğiniz *boyuttır.* Her bölgede, *Standart* veya *Premium* boyut isteğini yerine getirebilecek sınırlı sayıda sunucu vardır. Sonuç olarak, istenen boyutun tüm sunucuları bölgenizde şu anda kullanılıyorsa, oturum oluşturma başarısız olur. Hata nedeni [sorgulanabilir.](../how-tos/session-rest-api.md#get-sessions-properties)

> [!IMPORTANT]
> *Standart* VM boyutu talep ederseniz ve yüksek talep nedeniyle istek başarısız olursa, bu *Premium* sunucu istemenin de başarısız olacağı anlamına gelmez. Bu nedenle, sizin için bir seçenekse, *Premium* VM'ye geri dönmeyi deneyebilirsiniz.

Hizmet uygun bir sunucu bulduğunda, uygun sanal makineyi (VM) üzerine kopyalayarak azure uzaktan render ana bilgisayarına dönüştürmesi gerekir. Bu işlem birkaç dakika sürer. Daha sonra VM önyüklenir ve **oturum durumu** **Hazır'a**geçilir.

Bu noktada, sunucu yalnızca girişinizi bekliyor. Bu da hangi hizmet için fatura almak noktasıdır.

### <a name="connecting-to-a-session"></a>Oturuma bağlanma

Oturum *hazır*olduğunda, oturuma *bağlanabilirsiniz.* Aygıt bağlıyken modelleri yüklemek ve değiştirmek için komutlar gönderebilir. Her ARR ana bilgisayar aynı anda yalnızca bir istemci aygıtı na hizmet eder, bu nedenle bir istemci oturuma bağlandığında, işlenen içerik üzerinde özel denetime sahiptir. Bu aynı zamanda, render performansının denetiminiz dışındaki nedenlerle asla değişmeyeceği anlamına gelir.

> [!IMPORTANT]
> Bir oturuma yalnızca bir istemci *bağlanabilse* de, geçerli durumları gibi oturumlar hakkındaki temel bilgiler bağlanmadan sorgulanabilir.

Bir aygıt oturuma bağlıyken, diğer aygıtların bağlanma girişimleri başarısız olur. Ancak, bağlı aygıt bağlantıyı kestiğinde, gönüllü olarak veya bir tür hata nedeniyle oturum başka bir bağlantı isteğini kabul eder. Önceki tüm durum (yüklenen modeller ve benzeri) bir sonraki bağlantı aygıtıtemiz bir sayfa alır gibi atılır. Böylece oturumlar farklı aygıtlar tarafından birçok kez yeniden kullanılabilir ve çoğu durumda son kullanıcıdan oturum başlangıç yükü gizlemek mümkün olabilir.

> [!IMPORTANT]
> Uzak sunucu, istemci tarafındaki verilerin durumunu hiçbir zaman değiştirmez. Tüm veri mutasyonları (dönüşüm güncelleştirmeleri ve yükleme istekleri gibi) istemci uygulaması tarafından gerçekleştirilmelidir. Tüm eylemler istemci durumunu hemen güncelleştirin.

### <a name="session-end"></a>Oturum sonu

Yeni bir oturum isteğinde, genellikle bir ila sekiz saat aralığında *maksimum kira süresi*belirtirsiniz. Bu, ev sahibinin girişinizi kabul edeceği süredir.

Bir oturumun sona ermesinin iki düzenli nedeni vardır. Oturumun el ile durdurulmasını istersiniz veya maksimum kira süresi nin dolmasını istersiniz. Her iki durumda da, ana bilgisayara herhangi bir etkin bağlantı hemen kapatılır ve hizmet bu sunucuda kapatılır. Sunucu daha sonra Azure havuzuna geri verilir ve başka amaçlarla talep edilebilir. Oturumu durdurma geri alınamaz veya iptal edilemez. Oturumun durdurulduğu **oturumda** sorgulanması, el ile kapatılıp kapatılmadığına veya maksimum kira süresine ulaşıldığından bağlı olarak **Durduruldu** veya **Süresi Doldu**döndürülecektir.

Bir oturum da bazı hata nedeniyle durdurulabilir.

Her durumda, bir oturum durdurulduğunda daha fazla faturalandırılmazsınız.

> [!WARNING]
> Bir oturuma bağlanıp bağlanmadığınız ve ne kadar süreyle faturalandırmayı etkilemez. Hizmet için ödediğiniz ödeme, *oturum süresine*bağlıdır , bu da sunucunun yalnızca sizin için ayrılmış olduğu süre ve istenen donanım özellikleri (VM boyutu) anlamına gelir. Bir oturum alar, beş dakika bağlanın ve sonra oturumu durdurmazsa, kira süresi dolana kadar çalışmaya devam ederse, tam oturum kiralama süresi için faturalandırılırsınız. Tersine, *maksimum kira süresi* çoğunlukla bir güvenlik ağıdır. Sekiz saatlik kira süresine sahip bir oturum isteyip istemediğiniz önemli değildir, daha sonra oturumu el ile durdurursanız yalnızca beş dakika kullanın.

#### <a name="extend-a-sessions-lease-time"></a>Oturumun kiralama süresini uzatma

Etkin bir [oturumun kira süresini,](../how-tos/session-rest-api.md#update-a-session) daha uzun süre ihtiyacınız olduğu ortaya çıkarsa uzatabilirsiniz.

## <a name="example-code"></a>Örnek kod

Aşağıdaki kod, bir oturumu başlatma, *hazır* durumu bekleme, bağlanma ve ardından bağlantıyı kesme ve yeniden kapatma gibi basit bir uygulama gösterir.

``` cs
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

Birden `AzureFrontend` `AzureSession` çok ve örnek, koddan tutulabilir, işlenebilir ve sorgulanabilir. Ancak aynı anda yalnızca `AzureSession` tek bir aygıt bağlanabilir.

Sanal bir makinenin ömrü `AzureFrontend` örne veya `AzureSession` örne bağlı değildir. `AzureSession.StopAsync`oturumu durdurmak için çağrılmalıdır.

Kalıcı oturum kimliği üzerinden `AzureSession.SessionUUID()` sorgulanabilir ve yerel olarak önbelleğe alınabilir. Bu kimlikle, bir `AzureFrontend.OpenSession` uygulama bu oturuma bağlanmayı arayabilir.

Doğru `AzureSession.IsConnected` olduğunda, `AzureSession.Actions` [modelleri yüklemek,](models.md) [varlıkları](entities.md)işlemek ve işlenen sahne hakkında [sorgu bilgilerini](../overview/features/spatial-queries.md) içeren `RemoteManager`bir örneği döndürür.

## <a name="next-steps"></a>Sonraki adımlar

* [Varlıklar](entities.md)
* [Modeller](models.md)
