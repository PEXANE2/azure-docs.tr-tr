---
title: Düğüm.js en iyi uygulamalar ve sorun giderme
description: Azure Uygulama Hizmeti'nde çalışan Node.js uygulamaları için en iyi uygulamaları ve sorun giderme adımlarını öğrenin.
author: msangapu-msft
ms.assetid: 387ea217-7910-4468-8987-9a1022a99bef
ms.devlang: nodejs
ms.topic: article
ms.date: 11/09/2017
ms.author: msangapu
ms.custom: seodec18
ms.openlocfilehash: 682884d11b298a97e27056af3c10802dfd410e4c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75430557"
---
# <a name="best-practices-and-troubleshooting-guide-for-node-applications-on-azure-app-service-windows"></a>Azure Uygulama Hizmeti Windows'undaki düğüm uygulamaları için en iyi uygulamalar ve sorun giderme kılavuzu

Bu makalede, Azure Uygulama Hizmeti'nde çalışan [düğüm uygulamaları](app-service-web-get-started-nodejs.md) için [(iisnode](https://github.com/azure/iisnode)ile) en iyi uygulamaları ve sorun giderme adımlarını öğreniyorsunuz.

> [!WARNING]
> Üretim sitenizde sorun giderme adımlarını kullanırken dikkatli olun. Öneri, uygulamanızı örneğin evreleme yuvanız gibi üretim dışı bir kurulumda sorun gidermeniz ve sorun giderildiğinde, evreleme yuvanızı üretim yuvanızla değiştirmektir.
>

## <a name="iisnode-configuration"></a>IISNODE yapılandırması

Bu [şema dosyası,](https://github.com/Azure/iisnode/blob/master/src/config/iisnode_schema_x64.xml) iisnode için yapılandırabileceğiniz tüm ayarları gösterir. Uygulamanız için yararlı olan ayarlardan bazıları:

### <a name="nodeprocesscountperapplication"></a>nodeProcessCountPerApplication

Bu ayar, IIS uygulaması başına başlatılan düğüm işlemlerinin sayısını denetler. Varsayılan değer 1’dir. Değeri 0 olarak değiştirerek VM vCPU sayınız kadar node.exs başlatabilirsiniz. Önerilen değer çoğu uygulama için 0'dır, böylece makinenizdeki tüm vCPUs'ları kullanabilirsiniz. Node.exe tek iş parçacığı, böylece bir düğüm.exe maksimum 1 vCPU tüketir. Düğüm uygulamanızdan maksimum performans elde etmek için tüm vCPUs'ları kullanmak istiyorsunuz.

### <a name="nodeprocesscommandline"></a>düğümProcessCommandLine

Bu ayar düğüm.exe yolunu denetler. Bu değeri düğüm.exe sürümünüziçin ayarlayabilirsiniz.

### <a name="maxconcurrentrequestsperprocess"></a>maxConcurrentRequestsPerProcess

Bu ayar, iisnode tarafından her düğüme gönderilen en fazla eşzamanlı istek sayısını denetler. Azure Uygulama Hizmeti'nde varsayılan değer Sonsuzdur. Değeri, uygulamanızın kaç istek aldığına ve uygulamanızın her isteği ne kadar hızlı işlediğine bağlı olarak yapılandırabilirsiniz.

### <a name="maxnamedpipeconnectionretry"></a>maxNamedPipeConnectionRetry

Bu ayar, istekleri düğüm.exe'ye göndermek için adlandırılmış boruüzerinde bağlantı kurarak iisnode yeniden denemelerinin maksimum sayısını denetler. PipeConnectionRetryDelay adlı ile birlikte bu ayar iisnode içinde her isteğin toplam zaman dışarı belirler. Varsayılan değer Azure Uygulama Hizmeti'nde 200'dür. Saniye cinsinden Toplam Zaman Dilimi = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

### <a name="namedpipeconnectionretrydelay"></a>namedPipeConnectionRetryDelay

Bu ayar, isteneni adlandırılmış borunun üzerinden düğüme göndermek için her yeniden deneme arasında bekleyen süreyi (ms'de) denetler. Varsayılan değer 250 ms'dir.
Saniye cinsinden Toplam Zaman Dilimi = (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000

Varsayılan olarak, Azure Uygulama Hizmeti'ndeki iisnode toplam \* zaman acısı 200 250 ms = 50 saniyedir.

### <a name="logdirectory"></a>logDirectory

Bu ayar, iisnode günlükleri stdout / stderr dizin denetler. Varsayılan değer, ana komut dosyası dizinine göre olan iisnod'dur (ana server.js'nin bulunduğu dizi)

### <a name="debuggerextensiondll"></a>debuggerExtensionDll

Bu ayar, düğüm uygulamanızı hata ayıklarken düğüm denetçisi iisnod sürümünün hangi sürümü kullandığını denetler. Şu anda, iisnode-inspector-0.7.3.dll ve iisnode-inspector.dll bu ayar için sadece iki geçerli değerdir. Varsayılan değer iisnode-inspector-0.7.3.dll'dir. Iisnode-inspector-0.7.3.dll sürümü düğüm-inspector-0.7.3 kullanır ve web soketleri kullanır. Bu sürümü kullanmak için Azure web uygulamanızdaki web yuvalarını etkinleştirin. Yeni <https://ranjithblogs.azurewebsites.net/?p=98> düğüm denetçisini kullanmak için iisnode'u nasıl yapılandırılabiliyor daha fazla bilgi için bkz.

### <a name="flushresponse"></a>flushResponse

IIS'nin varsayılan davranışı, yanıt verilerini yıkamadan önce 4 MB'a kadar veya hangisi önce gelirse yanıtın sonuna kadar arabellekte olmasıdır. iisnode bu davranışı geçersiz kılmak için bir yapılandırma ayarı sunar: iisnode düğüm.exe alır almaz yanıt varlık gövdesinin bir parçasını floş için, web.config 'true' iisnode/@flushResponse özniteliği ni ayarlamanız gerekir:

```xml
<configuration>
    <system.webServer>
        <!-- ... -->
        <iisnode flushResponse="true" />
    </system.webServer>
</configuration>
```

Yanıt varlık gövdesinin her parçasının kızarmasını etkinleştirin, sistemin elde çalışmasını ~%5 azaltan performans ek yükü ekler (v0.1.13 itibariyle). Bu ayarı yalnızca yanıt akışı gerektiren uç noktalara göre `<location>` en iyi kapsamda (örneğin, web.config'deki öğeyi kullanarak)

Buna ek olarak, akış uygulamaları için, ayrıca yanıtBufferLimit iisnode işleyicisi 0 ayarlamanız gerekir.

```xml
<handlers>
    <add name="iisnode" path="app.js" verb="\*" modules="iisnode" responseBufferLimit="0"/>
</handlers>
```

### <a name="watchedfiles"></a>izledimDosyalar

Değişiklikler için izlenen dosyaların yarı nokta nokta lı ayrı listesi. Dosyadaki herhangi bir değişiklik, uygulamanın geri dönüştürülmesine neden olur. Her giriş, isteğe bağlı bir dizin adının yanı sıra, ana uygulama giriş noktasının bulunduğu dizine göre gerekli bir dosya adından oluşur. Yalnızca dosya adı bölümünde joker kartlara izin verilir. Varsayılan değer: `*.js;iisnode.yml`

### <a name="recyclesignalenabled"></a>geri dönüşümSignalEnabled

Varsayılan değer false'tur. Etkinse, düğüm uygulamanız adlandırılmış bir boruya (ortam\_\_değişkeni IISNODE CONTROL PIPE) bağlanabilir ve bir "geri dönüşüm" iletisi gönderebilir. Bu w3wp incelikle geri dönüşüm neden olur.

### <a name="idlepageouttimeperiod"></a>idlePageOutTimePeriod

Varsayılan değer 0'dır, bu da bu özelliğin devre dışı bırakıldığı anlamına gelir. 0'dan büyük bir değere ayarlandığında, iisnode tüm alt işlemlerini milisaniyeler içinde her 'idlePageOutTimePeriod' olarak sayfalar. Sayfa nın ne anlama geldiğini anlamak için [belgelere](/windows/desktop/api/psapi/nf-psapi-emptyworkingset) bakın. Bu ayar, yüksek miktarda bellek tüketen ve RAM'i boşaltmak için zaman zaman diske bellek sayfası isteyen uygulamalar için yararlıdır.

> [!WARNING]
> Üretim uygulamalarında aşağıdaki yapılandırma ayarlarını etkinleştirirken dikkatli olun. Öneri, bunları canlı üretim uygulamalarında etkinleştirmemektir.
>

### <a name="debugheaderenabled"></a>debugHeaderEtkin

Varsayılan değer false'tur. Doğru olarak ayarlanmışsa, iisnode üstbilgi değeri gönderir `iisnode-debug` `iisnode-debug` her HTTP yanıt bir HTTP yanıt üstbilgi ekler bir URL'dir. Tek tek tanılama bilgileri URL parçasına bakarak elde edilebilir, ancak bir görselleştirme bir tarayıcıda URL açarak kullanılabilir.

### <a name="loggingenabled"></a>günlükEtkin

Bu ayar iisnode tarafından stdout ve stderr günlüğü denetler. Iisnode başlattığı düğüm işlemlerinden stdout/stderr yakalar ve 'logDirectory' ayarında belirtilen dizine yazar. Bu etkinleştirildikten sonra, uygulamanız dosya sistemine günlükler yazar ve uygulama tarafından yapılan günlük miktarına bağlı olarak performans etkileri olabilir.

### <a name="deverrorsenabled"></a>devErrorsEtkin

Varsayılan değer false'tur. Doğru olarak ayarlandığında, iisnode tarayıcınızda HTTP durum kodunu ve Win32 hata kodunu görüntüler. win32 kodu, belirli türdeki sorunların hata ayıklanmasında yararlıdır.

### <a name="debuggingenabled-do-not-enable-on-live-production-site"></a>hata ayıklamaEtkin (canlı üretim sitesinde etkinleştirin)

Bu ayar hata ayıklama özelliğini denetler. Iisnode düğüm denetçisi ile entegre edilmiştir. Bu ayarı etkinleştirerek, düğüm uygulamanızın hata ayıklanmasını etkinleştirebilirsiniz. Bu ayarı etkinleştirdikten sonra iisnode, düğüm uygulamanıza yapılan ilk hata ayıklama isteğinde 'hata ayıklamaVirtualDir' dizininde düğüm denetçisi dosyaları oluşturur. Düğüm denetçisini `http://yoursite/server.js/debug`bir istek göndererek yükleyebilirsiniz. Hata ayıklama URL kesimini 'hata ayıklamaPathSegment' ayarı ile denetleyebilirsiniz. Varsayılan olarak, hata ayıklamaPathSegment='hata ayıklama'. Örneğin, `debuggerPathSegment` başkaları tarafından keşfedilmenin daha zor olması için bir GUID'e ayarlayabilirsiniz.

Hata ayıklama hakkında daha fazla bilgi için [Windows'daki Hata Ayıklama düğümü.js uygulamalarını](https://tomasz.janczuk.org/2011/11/debug-nodejs-applications-on-windows.html) okuyun.

## <a name="scenarios-and-recommendationstroubleshooting"></a>Senaryolar ve öneriler/sorun giderme

### <a name="my-node-application-is-making-excessive-outbound-calls"></a>Düğüm uygulamam aşırı giden aramalar yapıyor

Birçok uygulama, normal çalışmalarının bir parçası olarak giden bağlantılar yapmak ister. Örneğin, bir istek geldiğinde, düğüm uygulamanız başka bir yerde bir REST API'si ile iletişim kurmak ve isteği işlemek için bazı bilgiler almak ister. Http veya https aramaları yaparken canlı bir aracı kullanmak isteyebilirsiniz. Bu giden aramaları yaparken ajankeepalive modülünüzü canlı ajan olarak kullanabilirsiniz.

Agentkeepalive modülü, soketlerin Azure webapp VM'nizde yeniden kullanılmasını sağlar. Her giden istekte yeni bir soket oluşturmak, uygulamanıza ek yükü ekler. Uygulamanızın giden istekler için soketleri yeniden kullanması, uygulamanızın VM başına ayrılan maksimum Soketleri aşmamasını sağlar. Azure Uygulama Hizmeti'ndeki öneri, agentKeepAlive maxSockets değerini vm başına 160 soket (4 örnek node.exe \* 40 maxSockets/instance) olarak ayarlamaktır.

Örnek [ajanKeepALive](https://www.npmjs.com/package/agentkeepalive) yapılandırması:

```nodejs
let keepaliveAgent = new Agent({
    maxSockets: 40,
    maxFreeSockets: 10,
    timeout: 60000,
    keepAliveTimeout: 300000
});
```

> [!IMPORTANT]
> Bu örnek, VM üzerinde çalışan 4 düğüm.exe varsayıyor. VM'de çalışan farklı sayıda düğüm.exe'niz varsa, maxSockets ayarını buna göre değiştirmeniz gerekir.
>

#### <a name="my-node-application-is-consuming-too-much-cpu"></a>Düğüm uygulamam çok fazla CPU tüketiyor

Portalınızdaki Azure Uygulama Hizmeti'nden yüksek işlemci tüketimi yle ilgili bir öneri alabilirsiniz. Ayrıca belirli ölçümleri izlemek için [monitörler](web-sites-monitor.md)ayarlayabilirsiniz. [Azure Portal Panosu'ndaki](../azure-monitor/app/web-monitor-performance.md)CPU kullanımını kontrol ederken, en yüksek değerleri kaçırmamak için CPU için MAX değerlerini kontrol edin.
Uygulamanızın çok fazla CPU tükettiğini düşünüyorsanız ve nedenini açıklayamıyorsanız, öğrenmek için düğüm uygulamanızın profilini çıkarabilirsiniz.

#### <a name="profiling-your-node-application-on-azure-app-service-with-v8-profiler"></a>V8-Profiler ile Azure Uygulama Hizmeti'nde düğüm uygulamanızın profilini çıkarma

Örneğin, profilini çıkarmak istediğiniz bir merhaba dünya uygulamanız olduğunu varsayalım:

```nodejs
const http = require('http');
function WriteConsoleLog() {
    for(let i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    WriteConsoleLog();
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

Hata Ayıklama Konsolu sitesine git`https://yoursite.scm.azurewebsites.net/DebugConsole`

Sitenize/wwwroot dizininize gidin. Aşağıdaki örnekte gösterildiği gibi bir komut istemi görüyorsunuz:

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_install_v8.png)

`npm install v8-profiler` komutunu çalıştırın.

Bu komut, düğüm\_modülleri dizininin altına v8 profilleyicisini ve tüm bağımlılıklarını yükler.
Şimdi, uygulamanızın profilini çıkarmak için server.js'nizi düzenlemeyin.

```nodejs
const http = require('http');
const profiler = require('v8-profiler');
const fs = require('fs');

function WriteConsoleLog() {
    for(let i=0;i<99999;++i) {
        console.log('hello world');
    }
}

function HandleRequest() {
    profiler.startProfiling('HandleRequest');
    WriteConsoleLog();
    fs.writeFileSync('profile.cpuprofile', JSON.stringify(profiler.stopProfiling('HandleRequest')));
}

http.createServer(function (req, res) {
    res.writeHead(200, {'Content-Type': 'text/html'});
    HandleRequest();
    res.end('Hello world!');
}).listen(process.env.PORT);
```

Önceki kod WriteConsoleLog işlevini profiller ve daha sonra sitenizin wwwroot altında 'profile.cpuprofile' dosyasına profil çıktısı yazar. Başvurunuza bir istek gönderin. Sitenizin wwwroot altında oluşturulan bir 'profile.cpuprofile' dosyası bakın.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/scm_profile.cpuprofile.png)

Bu dosyayı indirin ve Chrome F12 Tools ile açın. Chrome'da F12 tuşuna basın ve **ardından Profiller** sekmesini seçin. **Yükle** düğmesini seçin. İndirdiğiniz profile.cpuprofile dosyanızı seçin. Az önce yüklediğiniz profile tıklayın.

![](./media/app-service-web-nodejs-best-practices-and-troubleshoot-guide/chrome_tools_view.png)

Zamanın %95'inin WriteConsoleLog işlevi tarafından tüketildiğini görebilirsiniz. Çıktı, soruna neden olan tam satır numaralarını ve kaynak dosyalarını da gösterir.

### <a name="my-node-application-is-consuming-too-much-memory"></a>Düğüm uygulamam çok fazla bellek tüketiyor

Uygulamanız çok fazla bellek tüketiyorsa, portalınızda yüksek bellek tüketimiyle ilgili Azure Uygulama Hizmeti'nden bir bildirim görürsünüz. Belirli ölçümleri izlemek için [monitörler](web-sites-monitor.md)ayarlayabilirsiniz. [Azure Portal Panosu'ndaki](../azure-monitor/app/web-monitor-performance.md)bellek kullanımını kontrol ederken, en yüksek değerleri kaçırmamak için bellek için MAX değerlerini kontrol ettiğinizden emin olun.

#### <a name="leak-detection-and-heap-diff-for-nodejs"></a>Düğüm için sızıntı algılama ve Heap Diff

Bellek sızıntılarını belirlemenize yardımcı olmak için [düğüm-memwatch](https://github.com/lloyd/node-memwatch) kullanabilirsiniz.
Tıpkı v8 profilleyicisi gibi yükleyebilir `memwatch` ve uygulamanızdaki bellek sızıntılarını tanımlamak için yığınları yakalamak ve dağıtmak için kodunuzu güncelleyebilirsiniz.

### <a name="my-nodeexes-are-getting-killed-randomly"></a>Benim düğümüm.exe'ler rastgele öldürülüyor.

Node.exe'nin rasgele kapatılmasının birkaç nedeni vardır:

1. Uygulamanız yakalanmamış özel durumlar atıyor\\–\\Kontrol\\\\d: ana LogFiles Uygulama logging-errors.txt dosyasında atılan özel durumla ilgili ayrıntılar için. Bu dosya, hata ayıklama ve uygulamanızı düzeltmek yardımcı olmak için yığın izleme vardır.
2. Uygulamanız çok fazla bellek tüketiyor, bu da diğer işlemleri niçin başlatıldığından etkiliyor. Toplam VM belleği %100'e yakınsa, düğümünüz.exe's işlem yöneticisi tarafından öldürülebilir. İşlem yöneticisi, diğer işlemlerin bazı işleri yapma şansı elde etmesini sağlamak için bazı işlemleri öldürür. Bu sorunu gidermek için, bellek sızıntıları için uygulamanızın profilini. Uygulamanız büyük miktarda bellek gerektiriyorsa, daha büyük bir VM'ye ölçeklendirin (VM'nin kullanabileceği RAM'i artırır).

### <a name="my-node-application-does-not-start"></a>Düğüm uygulamam başlatılamıyor

Uygulamanız başlatıldığında 500 Hata döndürürse, bunun birkaç nedeni olabilir:

1. Düğüm.exe doğru konumda bulunmaz. nodeProcessCommandLine ayarını denetleyin.
2. Ana komut dosyası dosyası doğru konumda bulunmaz. web.config'i denetleyin ve işleyiciler bölümündeki ana komut dosyası dosyasının adının ana komut dosyası dosyasıyla eşleştiğinden emin olun.
3. Web.config yapılandırması doğru değil – ayarlar adlarını/değerlerini denetleyin.
4. Cold Start – Uygulamanızın başlaması çok uzun sürüyor. Uygulamanız (maxNamedPipeConnectionRetry \* namedPipeConnectionRetryDelay) / 1000 saniyeden uzun sürüyorsa, iisnode 500 hata verir. Iisnod'un zamanlamasını ve 500 hatasını döndürmesini önlemek için uygulama başlangıç saatinizle eşleşecek şekilde bu ayarların değerlerini artırın.

### <a name="my-node-application-crashed"></a>Düğüm uygulamam çöktü

Uygulamanız yakalanmamış özel durumlar `d:\\home\\LogFiles\\Application\\logging-errors.txt` atıyor – Atılan özel durumla ilgili ayrıntılar için dosyayı denetleyin. Bu dosya, uygulamanızı tanılamaya ve düzeltmeye yardımcı olacak yığın izine sahiptir.

### <a name="my-node-application-takes-too-much-time-to-start-cold-start"></a>Düğüm uygulamamın başlaması çok fazla zaman alır (Cold Start)

Uzun uygulama başlangıç süreleri için ortak nedeni düğüm\_modülleri dosyaların yüksek sayıda olmasıdır. Uygulama başlatırken bu dosyaların çoğunu yüklemeye çalışır. Varsayılan olarak, dosyalarınız Azure Uygulama Hizmeti'ndeki ağ paylaşımında depolanır, birçok dosyanın yüklenmesi zaman alabilir.
Bu işlemi daha hızlı hale getirmek için bazı çözümler şunlardır:

1. Modüllerinizi yüklemek için npm3 kullanarak düz bir bağımlılık yapısına ve yinelenen bağımlılıklara sahip olduğundan emin olun.
2. Düğüm\_modüllerinizi tembelbir şekilde yüklemeye çalışın ve uygulama başlangıcında tüm modülleri yüklemeyin. Tembel yük modülleri için, modül kodunun ilk yürütülmesinden önce modüle gerçekten ihtiyacınız olduğunda ('modül') gerektirilmesi gereken çağrı yapılmalıdır.
3. Azure Uygulama Hizmeti, yerel önbellek adı verilen bir özellik sunar. Bu özellik, içeriğinizi ağ paylaşımından VM'deki yerel diske kopyalar. Dosyalar yerel olduğundan, düğüm\_modüllerinin yükleme süresi çok daha hızlıdır.

## <a name="iisnode-http-status-and-substatus"></a>IISNODE http durumu ve alt durumu

`cnodeconstants` [Kaynak dosya,](https://github.com/Azure/iisnode/blob/master/src/iisnode/cnodeconstants.h) iisnode'un bir hata nedeniyle döndürülebileceği olası durum/alt durum kombinasyonlarının tümünün listelenir.

Win32 hata kodunu görmek için uygulamanız için FREB'yi etkinleştirin (FREB'yi yalnızca üretim dışı sitelerde performans nedenleriyle etkinleştirdiğinizden emin olun).

| Http Durum | Http Alt Durum | Olası bir sebep mi? |
| --- | --- | --- |
| 500 |1000 |İsteğin IISNODE - Node.exe başlatılıp başlatılmadısını kontrol etme konusunda bazı sorun lar vardı. Node.exe başlarken çökmüş olabilir. Hatalar için web.config yapılandırmanızı kontrol edin. |
| 500 |1001 |- Win32Error 0x2 - Uygulama URL'ye yanıt vermiyor. URL yeniden yazma kurallarını kontrol edin veya ekspres uygulamanızın tanımlı doğru rotalara sahip olup olmadığını kontrol edin. - Win32Error 0x6d – adlı boru meşgul – Boru meşgul olduğu için Node.exe istekleri kabul etmiyor. Yüksek cpu kullanımını kontrol edin. - Diğer hatalar – node.exe çöktü olup olmadığını kontrol edin. |
| 500 |1002 |Node.exe çöktü – d\\\\kontrol\\edin: yığın izleme için ana LogFiles logging-errors.txt. |
| 500 |1003 |Boru yapılandırma Sorunu – Adlandırılmış boru yapılandırması yanlıştır. |
| 500 |1004-1018 |İsteği gönderirken veya yanıtı düğümden/node.exe'den işlerken bazı hatalar oluştu. Node.exe'nin kaza edip olmadığını kontrol et. d:\\ana\\logfiles\\log-errors.txt yığın izleme için kontrol edin. |
| 503 |1000 |Daha fazla adlandırılmış boru bağlantısı ayırmak için yeterli bellek yok. Uygulamanızın neden bu kadar çok bellek tükettiğini kontrol edin. maxConcurrentRequestsPerProcess ayar değerini kontrol edin. Sonsuz değilse ve çok sayıda isteğiniz varsa, bu hatayı önlemek için bu değeri artırın. |
| 503 |1001 |Uygulama geri dönüşüm olduğundan istek düğüm.exe'ye gönderilemedi. Uygulama geri dönüştürüldükten sonra, istekler normal olarak sunulmalıdır. |
| 503 |1002 |Gerçek nedenle win32 hata kodunu denetleyin – İstek bir düğüm.exe'ye gönderilemedi. |
| 503 |1003 |Adlandırılmış boru çok Meşgul – node.exe aşırı CPU tüketiyorsa doğrulayın |

NODE.exe adlı `NODE_PENDING_PIPE_INSTANCES`bir ayarı vardır. Azure Uygulama Hizmeti'nde bu değer 5000 olarak ayarlanır. Yani node.exe adlandırılmış boru üzerinde bir defada 5000 isteği kabul edebilir. Bu değer, Azure Uygulama Hizmeti'nde çalışan çoğu düğüm uygulaması için yeterince iyi olmalıdır. Azure Uygulama Hizmeti'nde 503.1003'ü görmemelisiniz, çünkü`NODE_PENDING_PIPE_INSTANCES`

## <a name="more-resources"></a>Diğer kaynaklar

Azure Uygulama Hizmeti'ndeki düğüm uygulamaları hakkında daha fazla bilgi edinmek için bu bağlantıları izleyin.

* [Azure App Service'te Node.js web uygulamalarını kullanmaya başlama](app-service-web-get-started-nodejs.md)
* [Azure App Service’teki bir Node.js web uygulamasına hata ayıklama](https://blogs.msdn.microsoft.com/azureossds/2018/08/03/debugging-node-js-apps-on-azure-app-services/)
* [Azure uygulamalarıyla Node.js Modüllerini kullanma](../nodejs-use-node-modules-azure-apps.md)
* [Azure Uygulama Hizmeti Web Apps: Node.js](https://blogs.msdn.microsoft.com/silverlining/2012/06/14/windows-azure-websites-node-js/)
* [Node.js Geliştirici Merkezi](../nodejs-use-node-modules-azure-apps.md)
* [Süper Gizli Kudu Hata Ayıklama Konsolunu keşfetme](https://azure.microsoft.com/documentation/videos/super-secret-kudu-debug-console-for-azure-web-sites/)
