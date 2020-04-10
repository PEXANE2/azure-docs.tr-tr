---
title: Önbellek ASP.NET Oturum Devlet Sağlayıcısı
description: Redis için Azure Önbelleğini kullanarak ASP.NET Oturum Durumu bellekte nasıl depolayacağınız hakkında bilgi edinin.
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 05/01/2017
ms.openlocfilehash: 8083efe833ec80290713fc14d9cb89acd8263fa2
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81010911"
---
# <a name="aspnet-session-state-provider-for-azure-cache-for-redis"></a>Azure için Redis Cache için ASP.NET Oturum Durumu Sağlayıcısı

Redis için Azure Önbelleği, oturum durumunuzu SQL Server veritabanı yerine Azure Önbelleği redis için bellekte depolamak için kullanabileceğiniz bir oturum durumu sağlayıcısı sağlar. Önbelleğe alma oturumu durum sağlayıcısını kullanmak için önce önbelleğinizi yapılandırın ve ardından Redis Session State NuGet paketini kullanarak önbellek için ASP.NET uygulamanızı yapılandırın.

Bir kullanıcı oturumu için bir tür durum depolamaktan kaçınmak için gerçek dünya bulut uygulamasında genellikle pratik değildir, ancak bazı yaklaşımlar performansı ve ölçeklenebilirliği diğerlerinden daha fazla etkiler. Durumu depolamanız gerekiyorsa, en iyi çözüm durum miktarını küçük tutmak ve tanımlama bilgilerinde saklamaktır. Bu mümkün değilse, sonraki en iyi çözüm, dağıtılmış, bellek içi önbellek için bir sağlayıcı ile ASP.NET oturum durumu kullanmaktır. Performans ve ölçeklenebilirlik açısından en kötü çözüm veritabanı destekli oturum durum sağlayıcısı kullanmaktır. Bu konu, Redis için Azure Önbelleği için ASP.NET Oturum Durum Sağlayıcısı'nı kullanma konusunda kılavuz sağlar. Diğer oturum durumu seçenekleri hakkında bilgi için, [ASP.NET Oturum Durumu seçeneklerine](#aspnet-session-state-options)bakın.

## <a name="store-aspnet-session-state-in-the-cache"></a>Önbellekte ASP.NET oturumu durumu depolama

Redis Session State NuGet paketini kullanarak Visual Studio'daki bir istemci uygulamasını yapılandırmak için **Araçlar** menüsünden **NuGet Paket Yöneticisi**, Paket **Yöneticisi Konsolu'nu** tıklatın.

`Package Manager Console` penceresinden aşağıdaki komutu çalıştırın.
    

```powershell
Install-Package Microsoft.Web.RedisSessionStateProvider
```

> [!IMPORTANT]
> Premium katmandaki kümeleme özelliğini kullanıyorsanız, [RedisSessionStateProvider](https://www.nuget.org/packages/Microsoft.Web.RedisSessionStateProvider) 2.0.1 veya daha yüksek kullanmanız gerekir veya bir özel durum atılır. 2.0.1 veya daha yüksek bir kırılma değişimidir; daha fazla bilgi için [v2.0.0 Breaking Change Ayrıntıları'na](https://github.com/Azure/aspnet-redis-providers/wiki/v2.0.0-Breaking-Change-Details)bakın. Bu makale güncelleştirmesırasında, bu paketin geçerli sürümü 2.2.3'tür.
> 
> 

Redis Session Devlet Sağlayıcı NuGet paketi StackExchange.Redis.StrongName paketine bağımlıdır. StackExchange.Redis.StrongName paketi projenizde yoksa yüklenir.

>[!NOTE]
>Güçlü adlı StackExchange.Redis.StrongName paketine ek olarak, StackExchange.Redis olmayan güçlü adlı sürümü de vardır. Projeniz güçlü olmayan StackExchange.Redis sürümünü kullanıyorsa, onu kaldırmanız gerekir, aksi takdirde projenizde adlandırma çakışmaları elde elabilirsiniz. Bu paketler hakkında daha fazla bilgi için [yapılandırır .NET önbellek istemcileri.](cache-dotnet-how-to-use-azure-redis-cache.md#configure-the-cache-clients)
>
>

NuGet paketi indirir ve gerekli montaj referansları ekler ve web.config dosyanıza aşağıdaki bölümü ekler. Bu bölüm, ASP.NET uygulamanızın Redis Session State Provider için Azure Önbelleğini kullanması için gerekli yapılandırmayı içerir.

```xml
<sessionState mode="Custom" customProvider="MySessionStateStore">
  <providers>
    <!-- Either use 'connectionString' OR 'settingsClassName' and 'settingsMethodName' OR use 'host','port','accessKey','ssl','connectionTimeoutInMilliseconds' and 'operationTimeoutInMilliseconds'. -->
    <!-- 'throwOnError','retryTimeoutInMilliseconds','databaseId' and 'applicationName' can be used with both options. -->
    <!--
      <add name="MySessionStateStore" 
        host = "127.0.0.1" [String]
        port = "" [number]
        accessKey = "" [String]
        ssl = "false" [true|false]
        throwOnError = "true" [true|false]
        retryTimeoutInMilliseconds = "5000" [number]
        databaseId = "0" [number]
        applicationName = "" [String]
        connectionTimeoutInMilliseconds = "5000" [number]
        operationTimeoutInMilliseconds = "1000" [number]
        connectionString = "<Valid StackExchange.Redis connection string>" [String]
        settingsClassName = "<Assembly qualified class name that contains settings method specified below. Which basically return 'connectionString' value>" [String]
        settingsMethodName = "<Settings method should be defined in settingsClass. It should be public, static, does not take any parameters and should have a return type of 'String', which is basically 'connectionString' value.>" [String]
        loggingClassName = "<Assembly qualified class name that contains logging method specified below>" [String]
        loggingMethodName = "<Logging method should be defined in loggingClass. It should be public, static, does not take any parameters and should have a return type of System.IO.TextWriter.>" [String]
        redisSerializerType = "<Assembly qualified class name that implements Microsoft.Web.Redis.ISerializer>" [String]
      />
    -->
    <add name="MySessionStateStore" type="Microsoft.Web.Redis.RedisSessionStateProvider"
         host=""
         accessKey=""
         ssl="true" />
  </providers>
</sessionState>
```

Yorumlanan bölüm, her öznitelik için özniteliklerin ve örnek ayarların bir örneğini sağlar.

Öznitelikleri Microsoft Azure portalındaki önbellek bıçağınızdaki değerlerle yapılandırın ve diğer değerleri istediğiniz gibi yapılandırın. Önbellek özelliklerinize erişim le ilgili talimatlar [için](cache-configure.md#configure-azure-cache-for-redis-settings)bkz.

* **ana bilgisayar** – önbellek bitiş noktanızı belirtin.
* **bağlantı noktası** – TLS ayarlarına bağlı olarak TLS/SSL olmayan bağlantı noktasınızı veya TLS/SSL bağlantı noktasınızı kullanın.
* **accessKey** – önbelleğiniz için birincil veya ikincil anahtarı kullanın.
* **ssl** – TLS ile önbellek/istemci iletişimini güvence altına almak istiyorsanız doğrudur; aksi takdirde yanlış. Doğru bağlantı noktasını belirttiğinden emin olun.
  * TLS olmayan bağlantı noktası, yeni önbellekler için varsayılan olarak devre dışı bırakılır. TLS bağlantı noktasını kullanmak için bu ayarın doğru olduğunu belirtin. TLS olmayan bağlantı noktasını etkinleştirme hakkında daha fazla bilgi için, önbellek konusunu [Yapılandır'daki](cache-configure.md) [Erişim Bağlantı Noktaları](cache-configure.md#access-ports) bölümüne bakın.
* **throwOnError** – bir hata varsa bir özel durum atılması istiyorsanız doğru, ya da işlemin sessizce başarısız olmasını istiyorsanız yanlış. Statik Microsoft.Web.Redis.RedisSessionStateProvider.LastException özelliğini denetleyerek bir hata olup olmadığını denetleyebilirsiniz. Varsayılan değer doğrudur.
* **retryTimeoutInMilliseconds** – Başarısız işlemler milisaniye cinsinden belirtilen bu aralıkta yeniden denener. İlk yeniden deneme 20 milisaniye sonra gerçekleşir ve yeniden denemetimeoutInMilliseconds aralığı sona erene kadar her saniye oluşur. Bu aralıktan hemen sonra, işlem son bir kez yeniden denendir. İşlem hala başarısız olursa, throwOnError ayarına bağlı olarak özel durum arayana geri atılır. Varsayılan değer 0'dır, bu da yeniden deneme yapılmadığı anlamına gelir.
* **databaseId** – Önbellek çıktısı verileri için hangi veritabanının kullanılacağını belirtir. Belirtilmemişse, 0'ın varsayılan değeri kullanılır.
* **applicationName** – Anahtarlar redis `{<Application Name>_<Session ID>}_Data`olarak saklanır . Bu adlandırma düzeni, birden çok uygulamanın aynı Redis örneğini paylaşmasını sağlar. Bu parametre isteğe bağlıdır ve bunu sağlamazsanız varsayılan bir değer kullanılır.
* **connectionTimeoutInMilliseconds** – Bu ayar StackExchange.Redis istemcisinde connectTimeout ayarını geçersiz kılmanızı sağlar. Belirtilmemişse, varsayılan 5000 bağlantıTimeout ayarı kullanılır. Daha fazla bilgi için [StackExchange.Redis yapılandırma modeline](https://go.microsoft.com/fwlink/?LinkId=398705)bakın.
* **operationTimeoutInMilliseconds** – Bu ayar StackExchange.Redis istemcisinde syncTimeout ayarını geçersiz kılmanızı sağlar. Belirtilmemişse, varsayılan eşitlemeTimeout ayarı 1000 kullanılır. Daha fazla bilgi için [StackExchange.Redis yapılandırma modeline](https://go.microsoft.com/fwlink/?LinkId=398705)bakın.
* **redisSerializerType** - Bu ayar, Redis'e gönderilen oturum içeriğinin özel serileştirmesini belirtmenizi sağlar. Belirtilen tür, `Microsoft.Web.Redis.ISerializer` kamu parametresiz oluşturucuyu uygulamalı ve beyan etmelidir. Varsayılan `System.Runtime.Serialization.Formatters.Binary.BinaryFormatter` olarak kullanılır.

Bu özellikler hakkında daha fazla bilgi için, [Redis için ASP.NET Oturum Devlet Sağlayıcı Duyurusu](https://blogs.msdn.com/b/webdev/archive/2014/05/12/announcing-asp-net-session-state-provider-for-redis-preview-release.aspx)orijinal blog yazısı duyuru bakın.

Web.config'inizdeki standart InProc oturumu devlet sağlayıcı bölümüne yorum yapmayı unutmayın.

```xml
<!-- <sessionState mode="InProc"
     customProvider="DefaultSessionProvider">
     <providers>
        <add name="DefaultSessionProvider"
              type="System.Web.Providers.DefaultSessionStateProvider,
                    System.Web.Providers, Version=1.0.0.0, Culture=neutral,
                    PublicKeyToken=31bf3856ad364e35"
              connectionStringName="DefaultConnection" />
      </providers>
</sessionState> -->
```

Bu adımlar gerçekleştirildikten sonra, uygulamanız Redis Session State Provider için Azure Önbelleği'ni kullanacak şekilde yapılandırılır. Uygulamanızda oturum durumunu kullandığınızda, Redis örneğinde bir Azure Önbelleğinde depolanır.

> [!IMPORTANT]
> Önbellekte depolanan veriler, varsayılan bellek içi ASP.NET Oturum Durumu Sağlayıcısı'nda depolanabilecek verilerin aksine serileştirilebilir olmalıdır. Redis için Oturum Devlet Sağlayıcısı kullanıldığında, oturum durumunda depolanan veri türlerinin serileştirilebilir olduğundan emin olun.
> 
> 

## <a name="aspnet-session-state-options"></a>ASP.NET Oturum Durumu seçenekleri

* Bellek Oturumu Durum Sağlayıcısı ' nda - Bu sağlayıcı Oturum Durumunu bellekte depolar. Bu sağlayıcı kullanmanın yararı basit ve hızlı olmasıdır. Ancak, dağıtılmaması için bellek sağlayıcısında kullanıyorsanız Web Uygulamalarınızı ölçeklendiremezsiniz.
* Sql Server Session State Provider - Bu sağlayıcı Oturum Durumunu Sql Server'da depolar. Oturum durumunu kalıcı depolama alanında depolamak istiyorsanız bu sağlayıcıyı kullanın. Web Uygulamanızı ölçeklendirebilirsiniz, ancak Oturum için Sql Server'ı kullanmanın Web Uygulamanız üzerinde bir performans etkisi vardır. Performansı artırmaya yardımcı olmak için bu sağlayıcıyı [Bellek içi OLTP yapılandırmasıyla](https://blogs.msdn.microsoft.com/sqlserverstorageengine/2017/11/28/asp-net-session-state-with-sql-server-in-memory-oltp/) da kullanabilirsiniz.
* Redis Session State Provider için Azure Önbelleği gibi Bellek Oturumu Devlet Sağlayıcısı'nda Dağıtılmış - Bu sağlayıcı, size her iki dünyanın da en iyisini sunar. Web Uygulamanız basit, hızlı ve ölçeklenebilir bir Oturum Devlet Sağlayıcısına sahip olabilir. Bu sağlayıcı Oturum durumunu önbellekte depoladığı için, uygulamanızın geçici ağ hataları gibi Bir BellekÖnbelleği Dağıtılır konuşurken ilişkili tüm özellikleri göz önünde bulundurması gerekiyor. Önbellek kullanma yla ilgili en iyi uygulamalar için, Microsoft Desenleri & Uygulamaları [Azure Bulut Uygulama Tasarımı ve Uygulama Kılavuzu'ndan](https://github.com/mspnp/azure-guidance) [önbelleğe alma kılavuzuna](../best-practices-caching.md) bakın.

Oturum durumu ve diğer en iyi uygulamalar hakkında daha fazla bilgi için Web [Geliştirme En İyi Uygulamaları (Azure ile Gerçek Dünya Bulut Uygulamaları Oluşturma) adlı](https://www.asp.net/aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/web-development-best-practices)sayfaya bakın.

## <a name="third-party-session-state-providers"></a>Üçüncü taraf oturum devlet sağlayıcıları

* [NÖnbellek](https://www.alachisoft.com/ncache/session-index.html)
* [Apaçi tutuşturmak](https://apacheignite-net.readme.io/docs/aspnet-session-state-caching)

## <a name="next-steps"></a>Sonraki adımlar

[Redis için Azure Önbelleği için ASP.NET Çıktı Önbelleği Sağlayıcısına](cache-aspnet-output-cache-provider.md)göz atın.
