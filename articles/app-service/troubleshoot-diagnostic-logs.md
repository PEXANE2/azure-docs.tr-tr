---
title: Uygulamalar için tanılama günlüğünü etkinleştirme-Azure App Service
description: Tanılama günlüğünü etkinleştirme ve uygulamanıza izleme ekleme ve Azure tarafından günlüğe kaydedilen bilgilere erişme hakkında bilgi edinin.
services: app-service
documentationcenter: .net
author: cephalin
manager: erikre
editor: jimbe
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.service: app-service
ms.workload: na
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 06/06/2016
ms.author: cephalin
ms.custom: seodec18
ms.openlocfilehash: af6d8b61c5d49ae219e90513abb93185f957222e
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70074066"
---
# <a name="enable-diagnostics-logging-for-apps-in-azure-app-service"></a>Azure App Service uygulamalar için tanılama günlüğünü etkinleştirme
## <a name="overview"></a>Genel Bakış
Azure, [App Service bir uygulamada](https://go.microsoft.com/fwlink/?LinkId=529714)hata ayıklamaya yardımcı olmak için yerleşik tanılama sağlar. Bu makalede, tanılama günlüğünü etkinleştirmeyi ve uygulamanıza nasıl araç ekleneceğini ve Azure tarafından günlüğe kaydedilen bilgilere nasıl erişebileceğinizi öğreneceksiniz.

Bu makalede tanılama günlükleri ile çalışmak için [Azure Portal](https://portal.azure.com) ve Azure CLI kullanılmaktadır. Visual Studio kullanarak tanılama günlükleri ile çalışma hakkında daha fazla bilgi için bkz. [Visual Studio 'Da Azure sorunlarını giderme](troubleshoot-dotnet-visual-studio.md).

## <a name="whatisdiag"></a>Web sunucusu tanılama ve Uygulama Tanılama
App Service, hem Web sunucusundan hem de Web uygulamasından günlüğe bilgi günlüğü için tanılama işlevselliği sağlar. Bunlar, **Web sunucusu tanılama** ve **Uygulama Tanılama**ile mantıksal olarak ayrılır.

### <a name="web-server-diagnostics"></a>Web sunucusu tanılaması
Aşağıdaki türlerde günlükleri etkinleştirebilir veya devre dışı bırakabilirsiniz:

* **Ayrıntılı hata günlüğü** -http durum kodu 400 veya üzeri bir istek ile sonuçlanan istekler için ayrıntılı bilgiler. Sunucunun neden hata kodunu döndürmediğini belirlemede yardımcı olabilecek bilgiler içerebilir. Uygulamanın dosya sistemindeki her hata için bir HTML dosyası oluşturulur ve 50 ' e kadar hata (dosyalar) tutulur. HTML dosyaları sayısı 50 ' i aşarsa, en eski 26 dosya otomatik olarak silinir.
* **Başarısız Istek izleme** -isteği işlemek IÇIN kullanılan IIS bileşenlerinin izlenmesi ve her bileşende geçen süre dahil olmak üzere başarısız istekler hakkında ayrıntılı bilgiler. Site performansını artırmak veya belirli bir HTTP hatasını yalıtmak istiyorsanız yararlı olur. Uygulamanın dosya sistemindeki her hata için bir klasör oluşturulur. Dosya bekletme ilkeleri, yukarıdaki ayrıntılı hata hatasıyla aynıdır.
* **Web sunucusu günlüğü** - [W3C Genişletilmiş günlük dosyası biçimini](/windows/desktop/Http/w3c-logging)kullanarak http işlemleri hakkında bilgiler. İşlenen istek sayısı veya belirli bir IP adresinden kaç istek olduğu gibi genel site ölçümleri belirlenirken yararlı olur.

### <a name="application-diagnostics"></a>Uygulama tanılamaları
Uygulama Tanılama, bir Web uygulaması tarafından üretilen bilgileri yakalamanızı sağlar. ASP.NET uygulamaları, uygulama tanılama günlüğüne bilgileri günlüğe kaydetmek için [System. Diagnostics. Trace](/dotnet/api/system.diagnostics.trace) sınıfını kullanabilir. Örneğin:

    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");

Çalışma zamanında, sorun gidermenize yardımcı olması için bu günlükleri alabilirsiniz. Daha fazla bilgi için bkz. [Visual Studio 'Da sorun giderme Azure App Service](troubleshoot-dotnet-visual-studio.md).

Ayrıca, bir uygulamaya içerik yayımlarken App Service dağıtım bilgilerini günlüğe kaydeder. Otomatik olarak gerçekleşir ve dağıtım günlüğü için yapılandırma ayarları yoktur. Dağıtım günlüğü, dağıtımın neden başarısız olduğunu belirlemenizi sağlar. Örneğin, özel bir dağıtım betiği kullanıyorsanız, betiğin neden başarısız olduğunu anlamak için dağıtım günlüğünü kullanabilirsiniz.

## <a name="enablediag"></a>Tanılamayı etkinleştirme
[Azure Portal](https://portal.azure.com)tanılamayı etkinleştirmek için uygulamanızın sayfasına gidin ve **Ayarlar > tanılama günlükleri**' ne tıklayın.

<!-- todo:cleanup dogfood addresses in screenshot -->
![Günlükler bölümü](./media/web-sites-enable-diagnostic-log/logspart.png)

**Uygulama tanılamayı**etkinleştirdiğinizde, **düzeyini**de seçersiniz. Aşağıdaki tabloda her bir düzeyin içerdiği günlüklerin kategorileri gösterilmektedir:

| Düzey| Dahil edilen günlük kategorileri |
|-|-|
|**Devre dışı** | Yok. |
|**Hata:** | Hata, kritik |
|**Uyarı** | Uyarı, hata, kritik|
|**Bilgi** | Bilgi, uyarı, hata, kritik|
|**Seçeneini** | Trace, Debug, Info, uyarı, hata, kritik (tüm kategoriler) |
|-|-|

**Uygulama günlüğü**için, hata ayıklama amacıyla dosya sistemi seçeneğini geçici olarak açabilirsiniz. Bu seçenek, 12 saat içinde otomatik olarak devre dışı bırakır. Ayrıca, günlüklerin yazılacağı bir blob kapsayıcısını seçmek için BLOB depolama seçeneğini etkinleştirebilirsiniz.

> [!NOTE]
> Şu anda yalnızca .NET uygulama günlükleri blob depolamaya yazılabilir. Java, PHP, Node. js, Python uygulama günlükleri yalnızca dosya sisteminde depolanabilir (günlükleri dış depolamaya yazmak için kod değişiklikleri olmadan).
>
>

**Web sunucusu günlüğü**için **depolama** veya **dosya sistemi**' ni seçebilirsiniz. **Depolama** seçme, bir depolama hesabı ve ardından günlüklerin yazıldığı bir blob kapsayıcısı seçmenizi sağlar. 

Günlükleri dosya sisteminde depoluiyorsa, dosyalara FTP tarafından erişilebiliyor veya Azure CLı kullanılarak ZIP arşivi olarak indirilebilir.

Varsayılan olarak, Günlükler otomatik olarak silinmez ( **uygulama günlüğü (dosya sistemi)** hariç). Günlükleri otomatik olarak silmek için **bekletme süresi (gün)** alanını ayarlayın.

> [!NOTE]
> [Depolama hesabınızın erişim anahtarlarını](../storage/common/storage-create-storage-account.md)yeniden oluşturursanız, güncelleştirilmiş anahtarları kullanmak için ilgili günlük yapılandırmasını sıfırlamanız gerekir. Bunu yapmak için:
>
> 1. **Yapılandır** sekmesinde ilgili günlük özelliğini **kapalı**olarak ayarlayın. Ayarınızı kaydedin.
> 2. Depolama hesabı blobuna yeniden günlük kaydını etkinleştirin. Ayarınızı kaydedin.
>
>

Dosya sistemi veya blob Storage 'ın herhangi bir birleşimi aynı anda etkinleştirilebilir ve ayrı günlük düzeyi yapılandırmalara sahip olabilir. Örneğin, hataları ve uyarıları, uzun süreli bir günlük çözümü olarak blob depolamaya kaydetmek, dosya sistemi günlüğünü ayrıntılı bir şekilde etkinleştirmek isteyebilirsiniz.

Depolama konumlarının her ikisi de günlüğe kaydedilen olaylar için aynı temel bilgileri sağlarken, **BLOB depolaması** örnek kimliği, Iş parçacığı kimliği ve **dosya sistemine**günlük kaydı daha ayrıntılı bir zaman damgası (değer biçimi) gibi ek bilgileri günlüğe kaydeder.

> [!NOTE]
> **BLOB depolamada** depolanan bilgilere yalnızca bir depolama istemcisi veya bu depolama sistemleriyle doğrudan çalışabilecek bir uygulama kullanılarak erişilebilir. Örneğin, Visual Studio 2013 blob depolamayı araştırmak için kullanılabilecek bir Depolama Gezgini içerir ve HDInsight, BLOB depolama alanında depolanan verilere erişebilir. Azure depolama alanına erişen bir uygulamayı [Azure SDK 'lardan](https://azure.microsoft.com/downloads/)birini kullanarak da yazabilirsiniz.
>

## <a name="download"></a>Nasıl Yapılır: İndirme günlükleri
Uygulama dosyası sistemine depolanmış tanılama bilgilerine doğrudan FTP kullanılarak erişilebilir. Ayrıca, Azure CLı kullanılarak bir ZIP arşivi olarak da indirilebilir.

Günlüklerin depolandığı dizin yapısı aşağıdaki gibidir:

* **Uygulama günlükleri** -/LogFiles/Application/. Bu klasör, uygulama günlüğü tarafından üretilen bilgileri içeren bir veya daha fazla metin dosyası içeriyor.
* **Başarısız Istek izlemeleri** -/LogFiles/W3SVC # # # # # # # # #/. Bu klasör bir XSL dosyası ve bir veya daha fazla XML dosyası içerir. Xsl dosyası, Internet Explorer 'da görüntülendiklerinde XML dosyalarının içeriklerini biçimlendirme ve filtreleme işlevleri sağladığından, XSL dosyasını XML dosyaları ile aynı dizine indirdiğinizden emin olun.
* **Ayrıntılı hata günlükleri** -/LogFiles/detailederrors/. Bu klasör, oluşan tüm HTTP hataları için kapsamlı bilgiler sağlayan bir veya daha fazla. htm dosyası içeriyor.
* **Web sunucusu günlükleri** -/LogFiles/http/RawLogs. Bu klasör, [W3C Genişletilmiş günlük dosyası biçimi](/windows/desktop/Http/w3c-logging)kullanılarak biçimlendirilen bir veya daha fazla metin dosyası içeriyor.
* **Dağıtım günlükleri** -/LogFiles/git. Bu klasör, Azure App Service tarafından kullanılan iç dağıtım işlemlerine ve git dağıtımları günlüklerine göre oluşturulan günlükleri içerir. Ayrıca, D:\home\site\deploymentyolunda dağıtım günlüklerini de bulabilirsiniz.

### <a name="ftp"></a>FTP

Uygulamanızın FTP sunucusuna FTP bağlantısı açmak için bkz. [FTP/s kullanarak Azure App Service uygulamanızı dağıtma](deploy-ftp.md).

Uygulamanızın FTP/S sunucusuna bağlandıktan sonra, günlük dosyalarının depolandığı **LogFiles** klasörünü açın.

### <a name="download-with-azure-cli"></a>Azure CLı ile indir
Azure komut satırı arabirimini kullanarak günlük dosyalarını indirmek için yeni bir komut istemi, PowerShell, bash veya terminal oturumu açın ve aşağıdaki komutu girin:

    az webapp log download --resource-group resourcegroupname --name appname

Bu komut, ' AppName ' adlı uygulamanın günlüklerini geçerli dizinde **webapp_logs. zip** adlı bir dosyaya kaydeder.

> [!NOTE]
> Azure CLı yüklemediyseniz veya Azure aboneliğinizi kullanmak üzere yapılandırmadıysanız, bkz. [Azure CLI kullanma](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest).
>
>

## <a name="how-to-view-logs-in-application-insights"></a>Nasıl yapılır: Günlükleri Application Insights görüntüle
Visual Studio Application Insights, günlüklere filtre uygulamak ve arama yapmak ve günlükleri ve diğer olayları ve diğer olayları ilişkilendirmek için araçlar sağlar.

1. Visual Studio 'da Application Insights SDK 'sını projenize ekleyin.
   * Çözüm Gezgini, projenize sağ tıklayın ve Application Insights Ekle ' yi seçin. Arabirim, bir Application Insights kaynağı oluşturmayı kapsayan adımlarda size rehberlik eder. [Daha fazla bilgi edinin](../azure-monitor/app/asp-net.md)
2. Izleme dinleyicisi paketini projenize ekleyin.
   * Projenize sağ tıklayın ve NuGet Paketlerini Yönet ' i seçin. `Microsoft.ApplicationInsights.TraceListener` [Daha fazla bilgi](../azure-monitor/app/asp-net-trace-logs.md) 'yi seçin
3. Projenizi karşıya yükleyin ve günlük verileri oluşturmak için çalıştırın.
4. [Azure Portal](https://portal.azure.com/)yeni Application Insights kaynağına gidin ve **Ara**' yı açın. Günlük verilerinizi, istek, kullanım ve diğer telemetri ile birlikte görmeniz gerekir. Bazı telemetrinin gelmesi birkaç dakika sürebilir: Yenile ' ye tıklayın. [Daha fazla bilgi edinin](../azure-monitor/app/diagnostic-search.md)

[Application Insights ile performans izleme hakkında daha fazla bilgi edinin](../azure-monitor/app/azure-web-apps.md)

## <a name="streamlogs"></a>Nasıl Yapılır: Akış günlükleri
Bir uygulama geliştirirken genellikle günlük bilgileri neredeyse gerçek zamanlı olarak görmeniz yararlı olur. Azure CLı kullanarak geliştirme ortamınıza günlüğe kaydetme bilgilerini akışını sağlayabilirsiniz.

> [!NOTE]
> Bazı günlük arabelleği türleri günlük dosyasına yazılır, bu da akıştaki sıra olayları oluşmasına neden olabilir. Örneğin, bir Kullanıcı bir sayfayı ziyaret ettiğinde oluşan bir uygulama günlüğü girişi, sayfa isteği için karşılık gelen HTTP günlük girişinden önce akışta görüntülenebilir.
>
> [!NOTE]
> Günlük akışı, **D:\\Home\\LogFiles\\**  klasöründe depolanan herhangi bir metin dosyasına yazılan bilgileri de akışlar.
>
>

### <a name="streaming-with-azure-cli"></a>Azure CLı ile akış
Günlüğe kaydetme bilgilerini akışa almak için yeni bir komut istemi, PowerShell, bash veya terminal oturumu açın ve aşağıdaki komutu girin:

    az webapp log tail --name appname --resource-group myResourceGroup

Bu komut ' AppName ' adlı uygulamaya bağlanır ve uygulama üzerinde günlük olayları gerçekleşmesi halinde bilgileri pencereye akışa başlatır. /LogFiles dizininde (d:/Home/LogFiles) depolanan. txt,. log veya. htm ile biten dosyalara yazılan tüm bilgiler yerel konsola akışla kaydedilir.

Hatalar gibi belirli olayları filtrelemek için **--Filter** parametresini kullanın. Örneğin:

    az webapp log tail --name appname --resource-group myResourceGroup --filter Error

HTTP gibi belirli günlük türlerini filtrelemek için **--Path** parametresini kullanın. Örneğin:

    az webapp log tail --name appname --resource-group myResourceGroup --path http

> [!NOTE]
> Azure CLı yüklemediyseniz veya Azure aboneliğinizi kullanmak üzere yapılandırmadıysanız, bkz. [Azure CLI kullanma](../cli-install-nodejs.md).
>
>

## <a name="understandlogs"></a>Nasıl Yapılır: Tanılama günlüklerini anlama
### <a name="application-diagnostics-logs"></a>Uygulama tanılama günlükleri
Uygulama Tanılama, günlükleri dosya sistemine veya blob depolama alanına depolayıp depolamadığınıza bağlı olarak .NET uygulamaları için belirli bir biçimde depolar. 

Depolanan temel veri kümesi, her iki depolama türünde de aynıdır; olayın gerçekleştiği tarih ve saat, olayı üreten işlem KIMLIĞI, olay türü (bilgi, uyarı, hata) ve olay iletisi. Günlük depolama için dosya sistemini kullanmak, günlük dosyaları anında yakın bir şekilde güncelleştirildiğinden bir sorunu gidermek için anında erişime ihtiyacınız olduğunda faydalıdır. BLOB depolama, dosyalar önbelleğe alındığından ve sonra bir zamanlamaya göre depolama kapsayıcısına temizlendiğinden arşiv amacıyla kullanılır.

**Dosya sistemi**

Dosya sistemine kaydedilen veya akış kullanılarak alınan her satır aşağıdaki biçimdedir:

    {Date}  PID[{process ID}] {event type/level} {message}

Örneğin, aşağıdaki örneğe benzer bir hata olayı görünür:

    2014-01-30T16:36:59  PID[3096] Error       Fatal error on the page!

Dosya sisteminde günlüğe kaydetme, yalnızca saat, işlem KIMLIĞI, olay düzeyi ve ileti sağlayan üç mevcut yöntemin en temel bilgilerini sağlar.

**Blob depolama**

Blob depolamaya oturum açarken, veriler virgülle ayrılmış değerler (CSV) biçiminde depolanır. Olay hakkında daha ayrıntılı bilgi sağlamak için ek alanlar günlüğe kaydedilir. Aşağıdaki özellikler CSV içindeki her satır için kullanılır:

| Özellik adı | Değer/biçim |
| --- | --- |
| Date |Olayın gerçekleştiği tarih ve saat |
| Düzey |Olay düzeyi (örneğin, hata, uyarı, bilgi) |
| ApplicationName |Uygulama adı |
| Örnek kimliği |Olayın gerçekleştiği uygulamanın örneği |
| EventTickCount |Olayın oluştuğu tarih ve saat (değer biçiminde) (daha büyük duyarlık) |
| EventID |Bu olayın olay KIMLIĞI<p><p>Belirtilmemişse varsayılan değer 0 ' dır |
| Kişisel |İşlem Kimliği |
| Değeri |Olayı üreten iş parçacığının iş parçacığı KIMLIĞI |
| `Message` |Olay Ayrıntısı iletisi |

Blob 'da depolanan veriler aşağıdaki örneğe benzer şekilde görünür:

    date,level,applicationName,instanceId,eventTickCount,eventId,pid,tid,message
    2014-01-30T16:36:52,Error,mywebapp,6ee38a,635266966128818593,0,3096,9,An error occurred

> [!NOTE]
> ASP.NET Core için, günlük kaydı [Microsoft. Extensions. Logging. AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) sağlayıcısı kullanılarak gerçekleştirilir. bu sağlayıcı, blob kapsayıcısına ek günlük dosyaları kakap. Daha fazla bilgi için bkz. [Azure 'da günlüğe kaydetme ASP.NET Core](/aspnet/core/fundamentals/logging).
>
>

### <a name="failed-request-traces"></a>Başarısız istek izlemeleri
Başarısız istek izlemeleri **fr # # # # # #. xml**adlı XML dosyalarında depolanır. Günlüğe kaydedilen bilgileri görüntülemeyi kolaylaştırmak için, **Freb. xsl** ADLı bir XSL stıl sayfası XML dosyalarıyla aynı dizinde sağlanır. Internet Explorer 'da XML dosyalarından birini açarsanız, Internet Explorer aşağıdaki örneğe benzer şekilde, izleme bilgilerinin biçimlendirilen bir görüntüsünü sağlamak için XSL stil sayfasını kullanır:

![tarayıcıda görüntülenen başarısız istek](./media/web-sites-enable-diagnostic-log/tws-failedrequestinbrowser.png)

> [!NOTE]
> Biçimlendirilen başarısız istek izlemelerinin görüntülemenin kolay bir yolu, portaldaki uygulamanızın sayfasına gitmenin bir yoludur. Sol menüden **Tanıla ve sorunları çöz**' ü seçin, sonra **başarısız istek izleme günlüklerini**arayın ve ardından istediğiniz izlemeye gözatıp görüntülemek için simgeye tıklayın.
>

### <a name="detailed-error-logs"></a>Ayrıntılı hata günlükleri
Ayrıntılı hata günlükleri, oluşan HTTP hatalarıyla ilgili daha ayrıntılı bilgi sağlayan HTML belgelerdir. Yalnızca HTML belgeleri olduklarından, bir Web tarayıcısı kullanılarak görüntülenebilirler.

### <a name="web-server-logs"></a>Web sunucu günlükleri
Web sunucusu günlükleri, [W3C Genişletilmiş günlük dosyası biçimi](/windows/desktop/Http/w3c-logging)kullanılarak biçimlendirilir. Bu bilgiler bir metin Düzenleyicisi kullanılarak okunabilir veya [Günlük ayrıştırıcı](https://go.microsoft.com/fwlink/?LinkId=246619)gibi yardımcı programlar kullanılarak ayrıştırılabilir.

> [!NOTE]
> Azure App Service tarafından üretilen Günlükler **s-ComputerName**, **s-ip**veya **CS-Version** alanlarını desteklemez.
>
>

## <a name="nextsteps"></a> Sonraki adımlar
* [Azure App Service Izleme](web-sites-monitor.md)
* [Visual Studio 'da Azure App Service sorunlarını giderme](troubleshoot-dotnet-visual-studio.md)
* [HDInsight 'ta uygulama günlüklerini çözümleme](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
