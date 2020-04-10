---
title: Redis için Azure Önbelleği için ASP.NET Çıktı Önbelleği Sağlayıcısı
description: Redis için Azure Önbelleğini kullanarak Sayfa Çıktısını nasıl önbelleğe ASP.NET öğrenin. Redis Çıktı Önbellek Sağlayıcısı, çıktı önbelleği verileri için işlem dışı bir depolama mekanizmasıdır.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 04/22/2018
ms.openlocfilehash: f1d8189068278b46e3ec3ea66875d79bb91e5e16
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010214"
---
# <a name="aspnet-output-cache-provider-for-azure-cache-for-redis"></a>Redis için Azure Önbelleği için ASP.NET Çıktı Önbelleği Sağlayıcısı

Redis Çıktı Önbellek Sağlayıcısı, çıktı önbelleği verileri için işlem dışı bir depolama mekanizmasıdır. Bu veriler özellikle tam HTTP yanıtları (sayfa çıkışı önbelleğe alma) içindir. Sağlayıcı, ASP.NET 4'te tanıtılan yeni çıktı önbelleği sağlayıcısı genişletilebilirlik noktasına takılır.

Redis Çıktı Önbellek Sağlayıcısı'nı kullanmak için önce önbelleğinizi yapılandırın ve ardından Redis Output Cache Provider NuGet paketini kullanarak ASP.NET uygulamanızı yapılandırın. Bu konu, uygulamanızı Redis Çıktı Önbellek Sağlayıcısı'nı kullanacak şekilde yapılandırma konusunda kılavuz sağlar. Redis örneği için bir Azure Önbelleği oluşturma ve yapılandırma hakkında daha fazla bilgi için [bkz.](cache-dotnet-how-to-use-azure-redis-cache.md#create-a-cache)

## <a name="store-aspnet-page-output-in-the-cache"></a>ASP.NET sayfa çıktısını önbellekte depolama

Redis Session State NuGet paketini kullanarak Visual Studio'daki bir istemci uygulamasını yapılandırmak için **Araçlar** menüsünden **NuGet Paket Yöneticisi**, Paket **Yöneticisi Konsolu'nu** tıklatın.

`Package Manager Console` penceresinden aşağıdaki komutu çalıştırın.

```
Install-Package Microsoft.Web.RedisOutputCacheProvider
```

Redis Çıktı Önbellek Sağlayıcısı NuGet paketi StackExchange.Redis.StrongName paketine bağımlıdır. StackExchange.Redis.StrongName paketi projenizde yoksa yüklenir. Redis Output Önbellek Sağlayıcısı NuGet paketi hakkında daha fazla bilgi için [RedisOutputCacheProvider](https://www.nuget.org/packages/Microsoft.Web.RedisOutputCacheProvider/) NuGet sayfasına bakın.

>[!NOTE]
>Güçlü adlı StackExchange.Redis.StrongName paketine ek olarak, StackExchange.Redis olmayan güçlü adlı sürümü de vardır. Projeniz güçlü olmayan StackExchange.Redis sürümünü kullanıyorsa, onu kaldırmanız gerekir; aksi takdirde, projenizde çakışmalar adlandırma yaşayacaksınız. Bu paketler hakkında daha fazla bilgi için [yapılandırır .NET önbellek istemcileri.](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)

NuGet paketi indirir ve gerekli montaj referansları ekler ve web.config dosyanıza aşağıdaki bölümü ekler. Bu bölüm, ASP.NET uygulamanızın Redis Çıktı Önbellek Sağlayıcısı'nı kullanması için gerekli yapılandırmayı içerir.

```xml
<caching>
  <outputCache defaultProvider="MyRedisOutputCache">
    <providers>
      <add name="MyRedisOutputCache" type="Microsoft.Web.Redis.RedisOutputCacheProvider"
           host=""
           accessKey=""
           ssl="true" />
    </providers>
  </outputCache>
</caching>
```

Öznitelikleri Microsoft Azure portalındaki önbellek bıçağınızdaki değerlerle yapılandırın ve diğer değerleri istediğiniz gibi yapılandırın. Önbellek özelliklerinize erişim le ilgili talimatlar [için](cache-configure.md#configure-azure-cache-for-redis-settings)bkz.

| Öznitelik | Tür | Varsayılan | Açıklama |
| --------- | ---- | ------- | ----------- |
| *Ana bilgisayar* | string | "localhost" | Redis sunucu IP adresi veya ana bilgisayar adı |
| *Bağlantı noktası* | pozitif tamsayı | 6379 (TLS/SSL dışı)<br/>6380 (TLS/SSL) | Redis sunucu bağlantı noktası |
| *Accesskey* | string | "" | Redis yetkilendirmesi etkinleştirildiğinde Redis sunucu parolası. Değer varsayılan olarak boş dizedir, bu da oturum durumu sağlayıcısının Redis sunucusuna bağlanırken parola kullanmayamayacağı anlamına gelir. **Redis sunucunuz Azure Redis Önbelleği gibi herkese açık bir ağdaysa, güvenliği artırmak ve güvenli bir parola sağlamak için Redis yetkilendirmesini etkinleştirdiğinden emin olun.** |
| *Ssl* | boole | **False** | TLS üzerinden Redis sunucusuna bağlanmak ister. Redis tls'yi kutunun dışında desteklemediği için bu değer varsayılan olarak **yanlıştır.** **SSL'yi kutunun dışında destekleyen Azure Redis Önbelleği kullanıyorsanız, güvenliği artırmak için bunu doğru ayarladığınızdan emin olun.**<br/><br/>TLS olmayan bağlantı noktası, yeni önbellekler için varsayılan olarak devre dışı bırakılır. TLS bağlantı noktasını kullanmak için bu ayarın **doğru** olduğunu belirtin. TLS olmayan bağlantı noktasını etkinleştirme hakkında daha fazla bilgi için, önbellek konusunu [Yapılandır'daki](cache-configure.md) [Erişim Bağlantı Noktaları](cache-configure.md#access-ports) bölümüne bakın. |
| *databaseIdNumber* | pozitif tamsayı | 0 | *Bu öznitelik yalnızca web.config veya AppSettings üzerinden belirtilebilir.*<br/><br/>Hangi Redis veritabanının kullanılacağını belirtin. |
| *bağlantıTimeoutInMilliseconds* | pozitif tamsayı | StackExchange.Redis tarafından sağlanan | StackExchange.Redis.ConnectionMultiplexer oluştururken *ConnectTimeout* ayarlamak için kullanılır. |
| *operationTimeoutInMilliseconds* | pozitif tamsayı | StackExchange.Redis tarafından sağlanan | StackExchange.Redis.ConnectionMultiplexer oluştururken *SyncTimeout* ayarlamak için kullanılır. |
| *connectionString* (Geçerli StackExchange.Redis bağlantı dizesi) | string | *Yok* | AppSettings veya web.config'e bir parametre başvurusu veya geçerli bir StackExchange.Redis bağlantı dizesi. Bu öznitelik ana *bilgisayar,* *bağlantı noktası,* *accessKey*, *ssl*ve diğer StackExchange.Redis öznitelikleri için değerler sağlayabilir. *connectionString'e*daha yakından bakmak için, [Atrite notları](#attribute-notes) bölümünde [bağlantı Ayarı](#setting-connectionstring) bölümüne bakın. |
| *ayarlarClassName*<br/>*ayarlarMethodName* | string<br/>string | *Yok* | *Bu öznitelikler yalnızca web.config veya AppSettings üzerinden belirtilebilir.*<br/><br/>Bağlantı dizesini sağlamak için bu öznitelikleri kullanın. *settingsClassName* *settingsMethodName*tarafından belirtilen yöntemi içeren bir montaj nitelikli sınıf adı olmalıdır.<br/><br/>*settingsMethodName* tarafından belirtilen yöntem, bir **dize**dönüş türü ile ortak, statik ve geçersiz (herhangi bir parametre almaz) olmalıdır. Bu yöntem gerçek bağlantı dizesini döndürür. |
| *günlükClassName*<br/>*günlükMethodName* | string<br/>string | *Yok* | *Bu öznitelikler yalnızca web.config veya AppSettings üzerinden belirtilebilir.*<br/><br/>StackExchange.Redis'teki günlüklerle birlikte Oturum Durumu/Çıktı Önbelleğinden günlükler sağlayarak uygulamanızı hata ayıklamak için bu öznitelikleri kullanın. *loggingClassName,* *loggingMethodName*tarafından belirtilen yöntemi içeren bir derleme nitelikli sınıf adı olmalıdır.<br/><br/>*LoggingMethodName* tarafından belirtilen yöntem **system.iO.TextWriter**bir dönüş türü ile, ortak, statik ve geçersiz (herhangi bir parametre almaz) olmalıdır. |
| *Applicationname* | string | Geçerli işlemin modül adı veya "/" | *Yalnızca SessionStateProvider*<br/>*Bu öznitelik yalnızca web.config veya AppSettings üzerinden belirtilebilir.*<br/><br/>Redis önbelleğinde kullanılacak uygulama adı öneki. Müşteri aynı Redis önbelleğini farklı amaçlar için kullanabilir. Oturum anahtarlarının çarpışmamasını sağlamak için, uygulama adı ile önceden düzeltilebilir. |
| *Throwonerror* | boole | true | *Yalnızca SessionStateProvider*<br/>*Bu öznitelik yalnızca web.config veya AppSettings üzerinden belirtilebilir.*<br/><br/>Bir hata oluştuğunda bir özel durum atmak ister.<br/><br/>*throwOnError*hakkında daha fazla şey için, [Atritnotları](#attribute-notes) bölümünde [ *throwOnError* üzerine Notlar](#notes-on-throwonerror) bölümüne bakın. |>*Microsoft.Web.Redis.RedisSessionStateProvider.LastException*. |
| *retryTimeoutInMilliseconds* | pozitif tamsayı | 5000 | *Yalnızca SessionStateProvider*<br/>*Bu öznitelik yalnızca web.config veya AppSettings üzerinden belirtilebilir.*<br/><br/>Bir işlem başarısız olduğunda yeniden denemenin ne kadar süreceğı. Bu değer *OperationTimeoutInMilliseconds*daha az ise, sağlayıcı yeniden denemez.<br/><br/>*Yeniden denemeTimeoutInMilliseconds*hakkında daha fazla şey için, [Attribute notları](#attribute-notes) bölümünde yeniden [ *denemeTimeoutInMilliseconds* Notlar](#notes-on-retrytimeoutinmilliseconds) bakın. |
| *redisSerializerType* | string | *Yok* | Microsoft.Web.Redis'i uygulayan bir sınıfın derleme nitelikli tür adını belirtir. ISerializer ve değerleri serihale ve deserialize etmek için özel mantık içerir. Daha fazla bilgi için, [Attribute notları](#attribute-notes) bölümünde [ *redisSerializerType* hakkında](#about-redisserializertype) bakın. |

## <a name="attribute-notes"></a>Öznitelik notları

### <a name="setting-connectionstring"></a>*Bağlantı ayarlamaString*

*ConnectionString* değeri, AppSettings'te böyle bir dize varsa, gerçek bağlantı dizesini AppSettings'ten almak için anahtar olarak kullanılır. AppSettings içinde bulunmazsa, *connectionString* değeri web.config **ConnectionString** bölümünden gerçek bağlantı dizesini almak için anahtar olarak kullanılır, bu bölüm varsa. Bağlantı dizesi AppSettings veya web.config **ConnectionString** bölümünde yoksa, StackExchange.Redis.ConnectionMultiplexer oluşturulurken *connectionString'in* gerçek değeri bağlantı dizesi olarak kullanılır.

Aşağıdaki *örneklerde String'in* nasıl kullanıldığı gösterilmektedir.

#### <a name="example-1"></a>Örnek 1

```xml
<connectionStrings>
    <add name="MyRedisConnectionString" connectionString="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</connectionStrings>
```

In `web.config`, gerçek değer yerine parametre değeri olarak yukarıda anahtar kullanın.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-2"></a>Örnek 2

```xml
<appSettings>
    <add key="MyRedisConnectionString" value="mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False" />
</appSettings>
```

In `web.config`, gerçek değer yerine parametre değeri olarak yukarıda anahtar kullanın.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "MyRedisConnectionString"/>
    </providers>
</sessionState>
```

#### <a name="example-3"></a>Örnek 3

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvide"
             name = "MySessionStateStore"
             connectionString = "mycache.redis.cache.windows.net:6380,password=actual access key,ssl=True,abortConnect=False"/>
    </providers>
</sessionState>
```

### <a name="notes-on-throwonerror"></a>*throwOnError* üzerine notlar

Şu anda, bir oturum işlemi sırasında bir hata oluşursa, oturum devlet sağlayıcı bir özel durum atar. Bu, uygulamayı kapatır.

Bu davranış, ASP.NET oturum durum sağlayıcı kullanıcıların beklentilerini destekleyen bir şekilde değiştirilirken, aynı zamanda istenirse özel durumlara göre hareket etme olanağı da sağlar. Varsayılan davranış, diğer ASP.NET oturum durum sağlayıcılarıyla tutarlı bir hata oluştuğunda yine de bir özel durum oluşturur; varolan kod eskisi gibi çalışmalıdır.

*ThrowOnError'ı* **false**olarak ayarlarsanız, hata oluştuğunda özel durum atmak yerine sessizce başarısız olur. Bir hata olup olmadığını görmek ve varsa, özel durum ne olduğunu keşfetmek için, statik özelliği *Microsoft.Web.Redis.RedisSessionStateProvider.LastException*kontrol edin.

### <a name="notes-on-retrytimeoutinmilliseconds"></a>*RetryTimeoutInMilliseconds* üzerine notlar

Bu, bazı oturum işleminin ağ hatası gibi şeyler nedeniyle hata üzerinde yeniden denemesi gereken durumu basitleştirmek için bazı yeniden deneme mantığı sağlarken, yeniden deneme zaman öncesini denetlemenize veya yeniden denemeyi tamamen devre dışı bırakmanıza olanak tanır.

Örneğin 2000 gibi bir sayıya *yeniden tryTimeoutInMilliseconds* ayarlarsanız, bir oturum işlemi başarısız olduğunda, bir hata olarak tedavi etmeden önce 2000 milisaniye için yeniden dener. Yani oturum devlet sağlayıcı bu yeniden deneme mantığını uygulamak için, sadece zaman aşıntını yapılandırmak için. İlk yeniden deneme 20 milisaniye sonra gerçekleşir ve bu durum çoğu durumda bir ağ hatası olduğunda yeterlidir. Bundan sonra, zaman dışarı kadar her saniye yeniden dener. Zaman dolduktan hemen sonra, (en fazla) bir saniye ile zaman dışarı kesmez emin olmak için bir kez daha yeniden dener.

Yeniden denemeniz gerektiğini düşünmüyorsanız (örneğin, Redis sunucusunu uygulamanızla aynı makinede çalıştırırken) veya yeniden deneme mantığını kendiniz işlemek istiyorsanız, *yeniden tryTimeoutInMilliseconds'ı* 0 olarak ayarlayın.

### <a name="about-redisserializertype"></a>*redisSerializerType* Hakkında

Varsayılan olarak, Redis değerlerini depolamak için serileştirme **BinaryFormatter** sınıfı tarafından sağlanan bir ikili biçimde yapılır. **Microsoft.Web.Redis.ISerializer'ı** uygulayan ve değerleri serihale ve deserialize etmek için özel mantığa sahip bir sınıfın montaj nitelikli türü adını belirtmek için *redisSerializerType'ı* kullanın. Örneğin, burada JSON.NET kullanarak bir Json serializer sınıf:

```cs
namespace MyCompany.Redis
{
    public class JsonSerializer : ISerializer
    {
        private static JsonSerializerSettings _settings = new JsonSerializerSettings() { TypeNameHandling = TypeNameHandling.All };

        public byte[] Serialize(object data)
        {
            return Encoding.UTF8.GetBytes(JsonConvert.SerializeObject(data, _settings));
        }

        public object Deserialize(byte[] data)
        {
            if (data == null)
            {
                return null;
            }
            return JsonConvert.DeserializeObject(Encoding.UTF8.GetString(data), _settings);
        }
    }
}
```

Bu sınıfın **MyCompanyDll**adında bir derlemede tanımlandığını varsayarsak, parametre *redisSerializerType'ı* kullanmak üzere ayarlayabilirsiniz:

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
    <providers>
        <add type = "Microsoft.Web.Redis.RedisSessionStateProvider"
             name = "MySessionStateStore"
             redisSerializerType = "MyCompany.Redis.JsonSerializer,MyCompanyDll"
             ... />
    </providers>
</sessionState>
```

## <a name="output-cache-directive"></a>Çıktı önbellek yönergesi

Çıktıyı önbelleğe almak istediğiniz her sayfaya bir OutputCache yönergesi ekleyin.

```xml
<%@ OutputCache Duration="60" VaryByParam="*" %>
```

Önceki örnekte, önbelleğe alınan sayfa verileri önbellekte 60 saniye kalır ve her parametre birleşimi için sayfanın farklı bir sürümü önbelleğe alınır. OutputCache yönergesi hakkında [@OutputCache](https://go.microsoft.com/fwlink/?linkid=320837)daha fazla bilgi için bkz.

Bu adımlar gerçekleştirildikten sonra, uygulamanız Redis Çıktı Önbellek Sağlayıcısı'nı kullanacak şekilde yapılandırılır.

## <a name="third-party-output-cache-providers"></a>Üçüncü taraf çıktı önbellek sağlayıcıları

* [NÖnbellek](https://www.alachisoft.com/blogs/how-to-use-a-distributed-cache-for-asp-net-output-cache/)
* [Apaçi Tutuşturma](https://apacheignite-net.readme.io/docs/aspnet-output-caching)


## <a name="next-steps"></a>Sonraki adımlar

[Redis için Azure Önbelleği için ASP.NET Oturum Devlet Sağlayıcısına](cache-aspnet-session-state-provider.md)göz atın.
