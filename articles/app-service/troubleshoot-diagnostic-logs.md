---
title: Tanılama günlüğünü etkinleştirme
description: Tanısal günlüğe kaydetmeyi etkinleştirmeyi ve uygulamanız için enstrümantasyon eklemeyi ve Azure tarafından günlüğe kaydedilen bilgilere nasıl erişilenleri öğrenin.
ms.assetid: c9da27b2-47d4-4c33-a3cb-1819955ee43b
ms.topic: article
ms.date: 09/17/2019
ms.custom: seodec18
ms.openlocfilehash: 6f22d5b2140e42f5f4b8ef5787d22b4be399c7e8
ms.sourcegitcommit: 530e2d56fc3b91c520d3714a7fe4e8e0b75480c8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81272534"
---
# <a name="enable-diagnostics-logging-for-apps-in-azure-app-service"></a>Azure Uygulama Hizmeti'ndeki uygulamalar için tanılama günlüğe kaydetmeyi etkinleştirme
## <a name="overview"></a>Genel Bakış
Azure, bir [App Service uygulamasının](overview.md)hata ayıklanmasına yardımcı olmak için yerleşik tanılama sağlar. Bu makalede, tanısal günlüğe kaydetmeyi etkinleştirmeyi ve uygulamanız için enstrümantasyon eklemeyi ve Azure tarafından günlüğe kaydedilen bilgilere nasıl erişilenleri öğrenirsiniz.

Bu makalede, tanılama günlükleri ile çalışmak için [Azure portalı](https://portal.azure.com) ve Azure CLI kullanır. Visual Studio'u kullanarak tanılama günlükleriyle çalışma hakkında daha fazla bilgi için [Visual Studio'da Sorun Giderme Azure'a](troubleshoot-dotnet-visual-studio.md)bakın.

> [!NOTE]
> Bu makaledeki günlük yönergelerine ek olarak, Azure İzleme ile yeni, tümleşik günlük özelliği de vardır. Bu özellik hakkında daha fazla bilgi bulabilirsiniz, [Azure Monitor'a Gönder günlükleri (önizleme)](#send-logs-to-azure-monitor-preview) bölümünde. 
>
>

|Tür|Platform|Konum|Açıklama|
|-|-|-|-|
| Uygulama günlüğüne kaydetme | Windows, Linux | Uygulama Hizmeti dosya sistemi ve/veya Azure Depolama blobs | Uygulama kodunuz tarafından oluşturulan iletileri günlüğe kaydeder. İletiler, seçtiğiniz web çerçevesi veya doğrudan dilinizin standart günlük deseni kullanılarak uygulama kodunuzdan oluşturulabilir. Her ileti aşağıdaki kategorilerden biri atanır: **Kritik**, **Hata**, **Uyarı**, **Bilgi**, **Hata Ayıklama**, ve **İzleme**. Uygulama günlüğünü etkinleştirdiğinizde önem düzeyini ayarlayarak günlüğe kaydetmenin ne kadar ayrıntılı olmasını istediğinizi seçebilirsiniz.|
| Web sunucusu günlüğü| Windows | Uygulama Hizmeti dosya sistemi veya Azure Depolama blobs| [W3C genişletilmiş günlük dosya biçiminde](/windows/desktop/Http/w3c-logging)Raw HTTP istek verileri . Her günlük iletisi HTTP yöntemi, kaynak URI, istemci IP, istemci bağlantı noktası, kullanıcı aracısı, yanıt kodu gibi verileri içerir. |
| Ayrıntılı Hata Mesajları| Windows | Uygulama Hizmeti dosya sistemi | İstemci tarayıcısına gönderilen *.htm* hata sayfalarının kopyaları. Güvenlik nedenleriyle, üretimdeki istemcilere ayrıntılı hata sayfaları gönderilmemelidir, ancak Uygulama Hizmeti, HTTP kodu 400 veya daha büyük bir uygulama hatası her oluştuğunda hata sayfasını kaydedebilir. Sayfa, sunucunun hata kodunu neden döndürebileceğini belirlemeye yardımcı olabilecek bilgiler içerebilir. |
| Başarısız istek izleme | Windows | Uygulama Hizmeti dosya sistemi | İsteği işlemek için kullanılan IIS bileşenlerinin izini ve her bileşende geçen süre de dahil olmak üzere başarısız istekler hakkındaki ayrıntılı izleme bilgileri. Site performansını artırmak veya belirli bir HTTP hatasını yalıtmak istiyorsanız yararlıdır. XML günlük dosyasını ve günlük dosyasını görüntülemek için XSL stil sayfasını içeren her başarısız istek için bir klasör oluşturulur. |
| Dağıtım günlüğü | Windows, Linux | Uygulama Hizmeti dosya sistemi | Bir uygulamada içerik yayımladığınızda günlük ler. Dağıtım günlüğü otomatik olarak gerçekleşir ve dağıtım günlüğü için yapılandırılabilir ayar yoktur. Dağıtımın neden başarısız olduğunu belirlemenize yardımcı olur. Örneğin, özel bir [dağıtım komut dosyası](https://github.com/projectkudu/kudu/wiki/Custom-Deployment-Script)kullanıyorsanız, komut dosyasının neden başarısız olduğunu belirlemek için dağıtım günlemeyi kullanabilirsiniz. |

> [!NOTE]
> Uygulama Hizmeti, uygulamanızı sorun gidermenize yardımcı olmak için özel, etkileşimli bir tanılama aracı sağlar. Daha fazla bilgi için Azure [Uygulama Hizmeti tanılama](overview-diagnostics.md)genel bakış bakın.
>
> Ayrıca, [Azure Monitor](../azure-monitor/app/azure-web-apps.md)gibi uygulamanızın günlüğe kaydetme ve izleme yeteneklerini geliştirmek için diğer Azure hizmetlerini de kullanabilirsiniz.
>

## <a name="enable-application-logging-windows"></a>Uygulama günlüğe kaydetmeyi etkinleştirme (Windows)

[Azure portalında](https://portal.azure.com)Windows uygulamaları için uygulama günlüğe kaydetmeyi etkinleştirmek için uygulamanıza gidin ve **Uygulama Hizmeti günlüklerini**seçin.

**Uygulama Günlüğe Kaydetme (Filesystem)** veya **Uygulama Günlüğü (Blob)** veya her ikisi için **Aç'ı** seçin. 

**Filesystem** seçeneği geçici hata ayıklama amaçları içindir ve 12 saat içinde kendiliğinden kapanır. **Blob** seçeneği uzun süreli günlüğe kaydetme içindir ve günlükleri yazmak için bir blob depolama kapsayıcısı gerekir.  **Blob** seçeneği ayrıca günlük iletilerinde, günlük iletisinin menşe VM örneğinin kimliği`InstanceId`( ),`Tid`iş parçacığı kimliği ( )[`EventTickCount`](https://docs.microsoft.com/dotnet/api/system.datetime.ticks)ve daha ayrıntılı bir zaman damgası ( ) gibi ek bilgiler içerir.

> [!NOTE]
> Şu anda yalnızca .NET uygulama günlükleri blob depolama yazılabilir. Java, PHP, Node.js, Python uygulama günlükleri yalnızca Uygulama Hizmeti dosya sisteminde depolanabilir (harici depolamaya günlük yazmak için kod değişiklikleri olmadan).
>
> Ayrıca, [depolama hesabınızın erişim anahtarlarını yeniden oluşturursanız,](../storage/common/storage-create-storage-account.md)güncelleştirilmiş erişim anahtarlarını kullanmak için ilgili günlük yapılandırmasını sıfırlamanız gerekir. Bunu yapmak için:
>
> 1. **Yapılandırma** sekmesinde, ilgili günlük özelliğini **Kapalı**olarak ayarlayın. Ayarınızı kaydedin.
> 2. Depolama hesabı blob'una yeniden günlüğe kaydetmeyi etkinleştirin. Ayarınızı kaydedin.
>
>

**Düzey'i**veya günlüğe kaydacak ayrıntı düzeyini seçin. Aşağıdaki tablo, her düzeyde yer alan günlük kategorilerini gösterir:

| Düzey | Dahil edilen kategoriler |
|-|-|
|**Devre dışı** | None |
|**Hata** | Hata, Kritik |
|**Uyarı** | Uyarı, Hata, Kritik|
|**Bilgi** | Bilgi, Uyarı, Hata, Kritik|
|**Ayrıntılı** | İzleme, Hata Ayıklama, Bilgi, Uyarı, Hata, Kritik (tüm kategoriler) |

Tamamlandığında **Kaydet'i**seçin.

## <a name="enable-application-logging-linuxcontainer"></a>Uygulama günlüğe kaydetmeyi etkinleştirme (Linux/Container)

[Azure portalında](https://portal.azure.com)Linux uygulamaları veya özel kapsayıcı uygulamaları için uygulama günlüğe kaydetmeyi etkinleştirmek için uygulamanıza gidin ve **Uygulama Hizmeti günlüklerini**seçin.

**Uygulama günlüğe kaydetmede** **Dosya Sistemi'ni**seçin.

**Kota 'da (MB),** uygulama günlükleri için disk kotasını belirtin. Bekletme Süresi 'nde **(Gün)** günlüklerin tutulması gereken gün sayısını ayarlayın.

Tamamlandığında **Kaydet'i**seçin.

## <a name="enable-web-server-logging"></a>Web sunucusu günlüğe kaydetmeyi etkinleştirme

[Azure portalında](https://portal.azure.com)Windows uygulamaları için web sunucusu günlüğe kaydetmeyi etkinleştirmek için uygulamanıza gidin ve **Uygulama Hizmeti günlüklerini**seçin.

**Web sunucusu günlüğe kaydetme için,** günlükleri blob depolamada depolamak için **Depolama'yı** veya Uygulama Hizmeti dosya sisteminde günlükleri depolamak için **Dosya Sistemi'ni** seçin. 

Bekletme Süresi 'nde **(Gün)** günlüklerin tutulması gereken gün sayısını ayarlayın.

> [!NOTE]
> Depolama [hesabınızın erişim anahtarlarını yeniden oluşturursanız,](../storage/common/storage-create-storage-account.md)güncelleştirilmiş anahtarları kullanmak için ilgili günlük yapılandırmasını sıfırlamanız gerekir. Bunu yapmak için:
>
> 1. **Yapılandırma** sekmesinde, ilgili günlük özelliğini **Kapalı**olarak ayarlayın. Ayarınızı kaydedin.
> 2. Depolama hesabı blob'una yeniden günlüğe kaydetmeyi etkinleştirin. Ayarınızı kaydedin.
>
>

Tamamlandığında **Kaydet'i**seçin.

## <a name="log-detailed-errors"></a>Ayrıntılı hataları günlüğe kaydetme

[Azure portalındaki](https://portal.azure.com)Windows uygulamaları için hata sayfasını kaydetmek veya başarısız istek izleme isteği kaydetmek için uygulamanıza gidin ve **Uygulama Hizmeti günlüklerini**seçin.

**Ayrıntılı Hata Günlüğe Kaydetme** veya Başarısız İstek İzleme **altında,** **Açık'ı**seçin, ardından **Kaydet'i**seçin.

Her iki günlük türü de App Service dosya sisteminde depolanır. En fazla 50 hata (dosya/klasör) korunur. HTML dosya sayısı 50'yi aştığında, en eski 26 hata otomatik olarak silinir.

## <a name="add-log-messages-in-code"></a>Kodda günlük iletileri ekleme

Uygulama kodunuzda, günlük iletilerini uygulama günlüklerine göndermek için olağan günlük olanaklarını kullanırsınız. Örneğin:

- ASP.NET uygulamaları, bilgileri uygulama tanılama günlüğüne günlüğe kaydetmek için [System.Diagnostics.Trace](/dotnet/api/system.diagnostics.trace) sınıfını kullanabilir. Örneğin:

    ```csharp
    System.Diagnostics.Trace.TraceError("If you're seeing this, something bad happened");
    ```

- Varsayılan olarak, ASP.NET Core [Microsoft.Extensions.Logging.AzureAppServices](https://www.nuget.org/packages/Microsoft.Extensions.Logging.AzureAppServices) günlük sağlayıcısını kullanır. Daha fazla bilgi için [Azure'da ASP.NET Core günlüğe kaydetme'ye](https://docs.microsoft.com/aspnet/core/fundamentals/logging/)bakın.

## <a name="stream-logs"></a>Akış günlükleri

Günlükleri gerçek zamanlı olarak akışyapmadan önce, istediğiniz günlük türünü etkinleştirin. .txt, .log veya .htm ile biten dosyalara yazılan ve */LogFiles* dizininde (d:/home/logfiles) depolanan tüm bilgiler App Service tarafından akışlanır.

> [!NOTE]
> Günlük arabellek bazı türleri akış ta düzen dışında olaylarneden olabilir günlük dosyasına yazın. Örneğin, bir kullanıcı bir sayfayı ziyaret ettiğinde oluşan bir uygulama günlüğü girişi, sayfa isteği için ilgili HTTP günlük girişinden önce akışta görüntülenebilir.
>

### <a name="in-azure-portal"></a>Azure portalında

[Azure portalında](https://portal.azure.com)günlükleri akışla aktarmak için uygulamanıza gidin ve **Günlük akışını**seçin. 

### <a name="in-cloud-shell"></a>Bulut Kabuğu nda

[Bulut Kabuğu'nda](../cloud-shell/overview.md)canlı günlük akışı yapmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az webapp log tail --name appname --resource-group myResourceGroup
```

Hatalar gibi belirli olayları filtrelemek için **--Filtre** parametresini kullanın. Örneğin:

```azurecli-interactive
az webapp log tail --name appname --resource-group myResourceGroup --filter Error
```
HTTP gibi belirli günlük türlerine filtre uygulayın, **--Yol** parametresini kullanın. Örneğin:

```azurecli-interactive
az webapp log tail --name appname --resource-group myResourceGroup --path http
```

### <a name="in-local-terminal"></a>Yerel terminalde

Yerel konsolda günlük akışı sağlamak için [Azure CLI'yi yükleyin](https://docs.microsoft.com/cli/azure/install-azure-cli) ve [hesabınızda oturum açın.](https://docs.microsoft.com/cli/azure/authenticate-azure-cli) Oturum açtıktan sonra [Cloud Shell için talimatları](#in-cloud-shell) izleyin

## <a name="access-log-files"></a>Günlük dosyalarına eriş

Günlük türü için Azure Depolama blobs seçeneğini yapılandırırsanız, Azure Depolama ile çalışan bir istemci aracına ihtiyacınız vardır. Daha fazla bilgi için Azure [Depolama İstemci Araçları'na](../storage/common/storage-explorers.md)bakın.

App Service dosya sisteminde depolanan günlükler için en kolay yol tarayıcıda ZIP dosyasını indirmektir:

- Linux/konteyner uygulamaları:`https://<app-name>.scm.azurewebsites.net/api/logs/docker/zip`
- Windows uygulamaları:`https://<app-name>.scm.azurewebsites.net/api/dump`

Linux/konteyner uygulamaları için ZIP dosyası, hem docker ana bilgisayar hem de docker konteyneri için konsol çıkış günlükleri içerir. Ölçeklenmiş bir uygulama için ZIP dosyası, her örnek için bir günlük kümesi içerir. App Service dosya sisteminde, bu günlük dosyaları */home/LogFiles* dizininin içeriğidir.

Windows uygulamaları için ZIP dosyası, Uygulama Hizmeti dosya sistemindeki *D:\Home\LogFiles* dizininin içeriğini içerir. Aşağıdaki yapıya sahiptir:

| Günlük türü | Dizin | Açıklama |
|-|-|-|
| **Uygulama günlükleri** |*/LogFiles/Uygulama/* | Bir veya daha fazla metin dosyası içerir. Günlük iletilerinin biçimi kullandığınız günlük sağlayıcısına bağlıdır. |
| **Başarısız İstek İzleri** | */LogFiles/W3SVC######################* | XML dosyaları ve bir XSL dosyası içerir. Tarayıcıda biçimlendirilmiş XML dosyalarını görüntüleyebilirsiniz. |
| **Ayrıntılı Hata Günlükleri** | */LogFiles/Ayrıntılı Hatalar/* | HTM hata dosyalarını içerir. HTM dosyalarını tarayıcıda görüntüleyebilirsiniz.<br/>Başarısız istek izlerini görüntülemenin başka bir yolu da portaldaki uygulama sayfanıza gitmektir. Sol menüden **Tanıla ve sorunları çöz'ü**seçin, ardından **Başarısız İstek İzleme Günlükleri'ni**arayın, ardından istediğiniz iz için simgeyi tıklatın. |
| **Web Sunucu Günlükleri** | */LogFiles/http/RawLogs/* | [W3C genişletilmiş günlük dosya biçimi](/windows/desktop/Http/w3c-logging)kullanılarak biçimlendirilmiş metin dosyaları içerir. Bu bilgiler bir metin düzenleyicisi veya [Log Parser](https://go.microsoft.com/fwlink/?LinkId=246619)gibi bir yardımcı program kullanılarak okunabilir.<br/>Uygulama `s-computername`Hizmeti, alanları `s-ip`veya `cs-version` alanları desteklemez. |
| **Dağıtım günlükleri** | */LogFiles/Git/* ve */deployments/* | İç dağıtım işlemleri tarafından oluşturulan günlüklerin yanı sıra Git dağıtımları için günlükleri de içerir. |

## <a name="send-logs-to-azure-monitor-preview"></a>Günlükleri Azure Monitör'e gönderme (önizleme)

Yeni [Azure Monitör tümleştirmesi](https://aka.ms/appsvcblog-azmon)ile, günlükleri Depolama Hesapları, Olay Hub'ları ve Günlük Analitiği'ne göndermek için [Tanılama Ayarları (önizleme) oluşturabilirsiniz.](https://azure.github.io/AppService/2019/11/01/App-Service-Integration-with-Azure-Monitor.html#create-a-diagnostic-setting) 

> [!div class="mx-imgBorder"]
> ![Tanılama Ayarları (önizleme)](media/troubleshoot-diagnostic-logs/diagnostic-settings-page.png)

### <a name="supported-log-types"></a>Desteklenen günlük türleri

Aşağıdaki tablodesteklenen günlük türlerini ve açıklamaları gösterir: 

| Günlük türü | Windows desteği | Linux (Docker) desteği | Açıklama |
|-|-|-|
| AppServiceConsoleGünlükler | Tba | Evet | Standart çıktı ve standart hata |
| AppServiceHTTPLogs | Evet | Evet | Web sunucusu günlükleri |
| AppServiceEnvironmentPlatformLogs | Evet | Evet | Uygulama Hizmet Ortamı: ölçekleme, yapılandırma değişiklikleri ve durum günlükleri|
| AppServiceAuditLogs | Evet | Evet | FTP ve Kudu üzerinden giriş etkinliği |
| AppServiceFileAuditLogs | Evet | TBD | FTP ve Kudu ile dosya değişiklikleri |
| AppServiceAppLogs | Tba | Java SE & Tomcat | Uygulama günlükleri |

## <a name="next-steps"></a><a name="nextsteps"></a>Sonraki adımlar
* [Azure Monitor ile günlükleri sorgula](../azure-monitor/log-query/log-query-overview.md)
* [Azure Uygulama Hizmeti Nasıl İzlenir?](web-sites-monitor.md)
* [Visual Studio'da Azure Uygulama Hizmetinde Sorun Giderme](troubleshoot-dotnet-visual-studio.md)
* [HDInsight'ta uygulama Günlüklerini analiz edin](https://gallery.technet.microsoft.com/scriptcenter/Analyses-Windows-Azure-web-0b27d413)
