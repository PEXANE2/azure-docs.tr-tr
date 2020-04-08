---
title: Portaldaki uygulamaları yapılandırma
description: Azure portalındaki bir Uygulama Hizmeti uygulaması için ortak ayarları yapılandırmayı öğrenin. Uygulama ayarları, bağlantı dizeleri, platform, dil yığını, konteyner, vb.
keywords: azure uygulama hizmeti, web uygulaması, uygulama ayarları, ortam değişkenleri
ms.assetid: 9af8a367-7d39-4399-9941-b80cbc5f39a0
ms.topic: article
ms.date: 08/13/2019
ms.custom: seodec18
ms.openlocfilehash: 18469c94b66acab27b58243e8d15eb924843319b
ms.sourcegitcommit: 98e79b359c4c6df2d8f9a47e0dbe93f3158be629
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/07/2020
ms.locfileid: "80811126"
---
# <a name="configure-an-app-service-app-in-the-azure-portal"></a>Azure portalında bir Uygulama Hizmeti uygulamasını yapılandırma

Bu konu, [Azure portalını]kullanarak web uygulamaları, mobil arka uç veya API uygulaması için ortak ayarların nasıl yapılandırılabildiğini açıklar.

## <a name="configure-app-settings"></a>Uygulama ayarlarını yapılandırma

Uygulama Hizmeti'nde, uygulama ayarları uygulama koduna ortam değişkeni olarak geçirilen değişkenlerdir. Linux uygulamaları ve özel kapsayıcılar için, Uygulama Hizmeti, kapsayıcıdaki ortam değişkenini ayarlamak için `--env` bayrağı kullanarak uygulama ayarlarını kapsayıcıya geçirir.

Azure [portalında,] **App Services'ı**arayın ve seçin ve ardından uygulamanızı seçin. 

![Uygulama Hizmetlerini Ara](./media/configure-common/search-for-app-services.png)

Uygulamanın sol menüsünde **Yapılandırma** > **Uygulaması ayarlarını**seçin.

![Uygulama Ayarları](./media/configure-common/open-ui.png)

ASP.NET ve ASP.NET Core geliştiricileri için, Uygulama Hizmeti'ndeki uygulama `<appSettings>` ayarlarını *Web.config* veya *appsettings.json'da*ayarlamak gibidir, ancak App Service'deki değerler *Web.config* veya *appsettings.json'daki*değerleri geçersiz kılar. Geliştirme ayarlarını (örneğin, yerel MySQL parolası) *Web.config* veya *appsettings.json'da*tutabilirsiniz, ancak üretim sırlarını (örneğin, Azure MySQL veritabanı parolası) Uygulama Hizmeti'nde güvende tutabilirsiniz. Yerel hata ayıklama yaptığınızda aynı kod geliştirme ayarlarınızı kullanır ve Azure'a dağıtıldığında üretim sırlarınızı kullanır.

Diğer dil yığınları da aynı şekilde, çalışma zamanında uygulama ayarlarını ortam değişkenleri olarak alır. Dil yığınına özgü adımlar için bkz:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Özel kapsayıcılar](containers/configure-custom-container.md#configure-environment-variables)

Uygulama ayarları her zaman depolandığında şifrelenir (istirahatte şifrelenir).

> [!NOTE]
> Uygulama ayarları Key Vault [referansları](app-service-key-vault-references.md)kullanılarak [Key Vault'tan](/azure/key-vault/) da çözülebilir.

### <a name="show-hidden-values"></a>Gizli değerleri göster

Varsayılan olarak, uygulama ayarlarıiçin değerler güvenlik portalında gizlenir. Bir uygulama ayarının gizli değerini görmek için, bu ayarın **Değer** alanını tıklatın. Tüm uygulama ayarlarının değerlerini görmek için **Değeri Göster** düğmesini tıklatın.

### <a name="add-or-edit"></a>Ekle veya edin

Yeni bir uygulama ayarı eklemek için **Yeni uygulama ayarını**tıklatın. İletişim kutusunda, [ayarı geçerli yuvaya yapıştırabilirsiniz.](deploy-staging-slots.md#which-settings-are-swapped)

Bir ayarı ayarlamak için sağ taraftaki **Edit** düğmesini tıklatın.

Bittiğinde, **Güncelleştir'i**tıklatın. **Yapılandırma** sayfasında geri **kaydet'i** tıklatmayı unutmayın.

> [!NOTE]
> Varsayılan bir Linux kapsayıcısında veya özel bir Linux kapsayıcısında, uygulama ayarı adındaki iç içe `ApplicationInsights__InstrumentationKey` herhangi bir JSON anahtar yapısının, anahtar adı için olduğu gibi App Service'de yapılandırılması `ApplicationInsights:InstrumentationKey` gerekir. Başka bir deyişle, herhangi `:` bir `__` (çift alt çizgi) ile değiştirilmelidir.
>

### <a name="edit-in-bulk"></a>Toplu olarak edin

Uygulama ayarlarını toplu olarak eklemek veya yeniden yapmak için **Gelişmiş edit** düğmesini tıklatın. Bittiğinde, **Güncelleştir'i**tıklatın. **Yapılandırma** sayfasında geri **kaydet'i** tıklatmayı unutmayın.

Uygulama ayarlarında aşağıdaki JSON biçimlendirmesi vardır:

```json
[
  {
    "name": "<key-1>",
    "value": "<value-1>",
    "slotSetting": false
  },
  {
    "name": "<key-2>",
    "value": "<value-2>",
    "slotSetting": false
  },
  ...
]
```

## <a name="configure-connection-strings"></a>Bağlantı dizelerini yapılandırma

Azure [portalında,] **App Services'ı**arayın ve seçin ve ardından uygulamanızı seçin. Uygulamanın sol menüsünde **Yapılandırma** > **Uygulaması ayarlarını**seçin.

![Uygulama Ayarları](./media/configure-common/open-ui.png)

ASP.NET ve ASP.NET Core geliştiricileri için, App Service'deki `<connectionStrings>` bağlantı dizeleri ayarlamak *Web.config'de*ayarlanması gibidir, ancak App Service'de belirlediğiniz değerler *Web.config'dekileri*geçersiz kılar. Geliştirme ayarlarını (örneğin, bir veritabanı dosyası) *Web.config'de* ve üretim sırlarını (örneğin, SQL Veritabanı kimlik bilgileri) Uygulama Hizmeti'nde güvenle tutabilirsiniz. Yerel hata ayıklama yaptığınızda aynı kod geliştirme ayarlarınızı kullanır ve Azure'a dağıtıldığında üretim sırlarınızı kullanır.

Bağlantı dizeleri değerlere erişmek için değişken tuşlarında özel biçimlendirme gerektirdiğinden, diğer dil yığınları için [uygulama ayarlarını](#configure-app-settings) kullanmak daha iyidir. Ancak bir istisna aşağıda dır: Uygulamanızda bağlantı dizelerini yapılandırırsanız, belirli Azure veritabanı türleri uygulamayla birlikte yedeklenir. Daha fazla bilgi için, [nelerin yedeklenir](manage-backup.md#what-gets-backed-up)olduğunu görün. Bu otomatik yedeklemeye ihtiyacınız yoksa, uygulama ayarlarını kullanın.

Çalışma zamanında, bağlantı dizeleri aşağıdaki bağlantı türleri ile önceden belirlenmiş ortam değişkenleri olarak kullanılabilir:

* Sqlserver:`SQLCONNSTR_`  
* Mysql:`MYSQLCONNSTR_` 
* SQLAzure:`SQLAZURECONNSTR_` 
* Özel:`CUSTOMCONNSTR_`
* Postgresql:`POSTGRESQLCONNSTR_`  

Örneğin, *connectionstring1* adlı bir MySql bağlantı dizesi `MYSQLCONNSTR_connectionString1`ortam değişkeni olarak erişilebilir. Dil yığınına özgü adımlar için bkz:

- [ASP.NET Core](containers/configure-language-dotnetcore.md#access-environment-variables)
- [Node.js](containers/configure-language-nodejs.md#access-environment-variables)
- [PHP](containers/configure-language-php.md#access-environment-variables)
- [Python](containers/how-to-configure-python.md#access-environment-variables)
- [Java](containers/configure-language-java.md#data-sources)
- [Ruby](containers/configure-language-ruby.md#access-environment-variables)
- [Özel kapsayıcılar](containers/configure-custom-container.md#configure-environment-variables)

Bağlantı dizeleri her zaman depolandığında şifrelenir (şifreli-istirahat).

> [!NOTE]
> Bağlantı dizeleri, [Key Vault referansları](app-service-key-vault-references.md)kullanılarak [Key Vault'tan](/azure/key-vault/) da çözülebilir.

### <a name="show-hidden-values"></a>Gizli değerleri göster

Varsayılan olarak, bağlantı dizeleri için değerler güvenlik için portalda gizlidir. Bağlantı dizesinin gizli bir değerini görmek için, bu dizedeki **Değer** alanını tıklatmanız yeterlidir. Tüm bağlantı dizelerinin değerlerini görmek için **Değeri Göster** düğmesini tıklatın.

### <a name="add-or-edit"></a>Ekle veya edin

Yeni bir bağlantı dizesi eklemek için **Yeni bağlantı dizesini**tıklatın. İletişim kutusunda, bağlantı [dizesini geçerli yuvaya yapıştırabilirsiniz.](deploy-staging-slots.md#which-settings-are-swapped)

Bir ayarı ayarlamak için sağ taraftaki **Edit** düğmesini tıklatın.

Bittiğinde, **Güncelleştir'i**tıklatın. **Yapılandırma** sayfasında geri **kaydet'i** tıklatmayı unutmayın.

### <a name="edit-in-bulk"></a>Toplu olarak edin

Bağlantı dizelerini toplu olarak eklemek veya tıklatmak için **Gelişmiş edit** düğmesini tıklatın. Bittiğinde, **Güncelleştir'i**tıklatın. **Yapılandırma** sayfasında geri **kaydet'i** tıklatmayı unutmayın.

Bağlantı dizeleri aşağıdaki JSON biçimlendirmevar:

```json
[
  {
    "name": "name-1",
    "value": "conn-string-1",
    "type": "SQLServer",
    "slotSetting": false
  },
  {
    "name": "name-2",
    "value": "conn-string-2",
    "type": "PostgreSQL",
    "slotSetting": false
  },
  ...
]
```

<a name="platform"></a>
<a name="alwayson"></a>

## <a name="configure-general-settings"></a>Genel ayarları yapılandırma

Azure [portalında,] **App Services'ı**arayın ve seçin ve ardından uygulamanızı seçin. Uygulamanın sol menüsünde **Configuration** > **General ayarlarını**seçin.

![Genel ayarlar](./media/configure-common/open-general.png)

Burada, uygulama için bazı ortak ayarları yapılandırabilirsiniz. Bazı [ayarlar, daha yüksek fiyatlandırma katmanlarına kadar ölçeklendirmenizi](manage-scale-up.md)gerektirir.

- **Yığın ayarları**: Dili ve SDK sürümleri ni içeren uygulamayı çalıştırmak için yazılım yığını. Linux uygulamaları ve özel kapsayıcı uygulamaları için isteğe bağlı bir başlatma komutu veya dosya da ayarlayabilirsiniz.
- **Platform ayarları**: Aşağıdakiler de dahil olmak üzere barındırma platformunun ayarlarını yapılandırmanızı sağlar:
    - **Bitness**: 32-bit veya 64-bit.
    - **WebSocket protokolü**: [Örneğin, ASP.NET SignalR] veya [socket.io](https://socket.io/)için.
    - **Always On**: Trafik olmadığında bile uygulamayı yüklü tutun. Sürekli Web İşleri veya CRON ifadesi kullanılarak tetiklenen Webİşler için gereklidir.
      > [!NOTE]
      > Always On özelliği yle bitiş noktasını denetemezsiniz. Her zaman uygulama köküne bir istek gönderir.
    - **Yönetilen boru hattı sürümü**: IIS [boru hattı modu.] IIS'nin eski bir sürümünü gerektiren eski bir uygulamanız varsa **Klasik** olarak ayarlayın.
    - **HTTP sürümü**: [HTTPS/2](https://wikipedia.org/wiki/HTTP/2) protokolü için destek sağlamak için **2.0** olarak ayarlayın.
    > [!NOTE]
    > Çoğu modern tarayıcı sadece TLS üzerinden HTTP/2 protokolünü desteklerken, şifrelenmemiş trafik HTTP/1.1'i kullanmaya devam eder. İstemci tarayıcılarının http/2 ile uygulamanıza bağlanmasını sağlamak için özel DNS adınızı güvence altına alayın. Daha fazla bilgi için Azure [Uygulama Hizmeti'nde TLS/SSL bağlayıcısı olan özel bir DNS adını güvenli](configure-ssl-bindings.md)olarak ele abakın.
    - **ARR yakınlığı**: Çok örnekli dağıtımda, istemcinin oturumun ömrü boyunca aynı örneğe yönlendirilmesini sağlayın. Bu seçeneği, durum dışı uygulamalar için **Kapatma** olarak ayarlayabilirsiniz.
- **Hata ayıklama**: [ASP.NET](troubleshoot-dotnet-visual-studio.md#remotedebug), [ASP.NET Core](/visualstudio/debugger/remote-debugging-azure)veya [Node.js](containers/configure-language-nodejs.md#debug-remotely) uygulamaları için uzaktan hata ayıklama etkinleştirin. Bu seçenek 48 saat sonra otomatik olarak kapanır.
- **Gelen istemci sertifikaları**: [karşılıklı kimlik doğrulama](app-service-web-configure-tls-mutual-auth.md)istemci sertifikaları gerektirir.

## <a name="configure-default-documents"></a>Varsayılan belgeleri yapılandırma

Bu ayar yalnızca Windows uygulamaları içindir.

Azure [portalında,] **App Services'ı**arayın ve seçin ve ardından uygulamanızı seçin. Uygulamanın sol menüsünde **Yapılandırma** > **Varsayılanı belgelerini**seçin.

![Varsayılan belgeler](./media/configure-common/open-documents.png)

Varsayılan belge, bir web sitesinin temel URL'sinde görüntülenen web sayfasıdır. Listedeki ilk eşleşen dosya kullanılır. Yeni varsayılan belge eklemek için **Yeni belgeyi**tıklatın. **Kaydet'i**tıklatmayı unutmayın.

Uygulama statik içerik sunyup url'ye dayalı modüller kullanıyorsa, varsayılan belgelere gerek yoktur.

## <a name="configure-path-mappings"></a>Yol eşlemelerini yapılandırma

Azure [portalında,] **App Services'ı**arayın ve seçin ve ardından uygulamanızı seçin. Uygulamanın sol menüsünde **Configuration** > **Path eşlemelerini**seçin.

![Yol eşlemeleri](./media/configure-common/open-path.png)

**Yol eşlemeleri** sayfası, işletim sistemi türüne göre farklı şeyler gösterir.

### <a name="windows-apps-uncontainerized"></a>Windows uygulamaları (kaptaslanmamış)

Windows uygulamaları için IIS işleyicisi eşlemelerini ve sanal uygulamaları ve dizinleri özelleştirebilirsiniz.

İşleyici eşlemeleri, belirli dosya uzantıları isteklerini işlemek için özel komut dosyası işlemcileri eklemenize izin sağlar. Özel işleyici eklemek için **Yeni işleyiciyi**tıklatın. Işleyiciyi aşağıdaki gibi yapılandırın:

- **Uzantısı**. * \*.php* veya *handler.fcgi*gibi işlemek istediğiniz dosya uzantısı.
- **Komut dosyası işlemci.** Komut dosyası işlemcisinin size mutlak yolu. Dosya uzantısı ile eşleşen dosyalara gelen istekler komut dosyası işlemcisi tarafından işlenir. Uygulamanızın `D:\home\site\wwwroot` kök dizinine başvurmak için yolu kullanın.
- **Bağımsız değişkenler**. Komut dosyası işlemcisi için isteğe bağlı komut satırı bağımsız değişkenleri.

Her uygulama, kodunuzun`/`varsayılan olarak `D:\home\site\wwwroot`dağıtıldığı varsayılan kök yolu ( ) eşlenir. Uygulama root'unuz farklı bir klasördeyse veya deponuzda birden fazla uygulama varsa, buraya sanal uygulamaları ve dizinleri dinleyebilir veya ekleyebilirsiniz. **Yeni sanal uygulama veya dizin'i**tıklatın.

Sanal uygulamaları ve dizinleri yapılandırmak için, her sanal dizin ve web`D:\home`sitesi köküne göre karşılık gelen fiziksel yol belirtin ( ). İsteğe bağlı olarak, sanal bir dizini uygulama olarak işaretlemek için **Uygulama** onay kutusunu seçebilirsiniz.

### <a name="containerized-apps"></a>Kapsayıcı uygulamalar

Kapsayıcı [uygulamanız için özel depolama alanı ekleyebilirsiniz.](containers/how-to-serve-content-from-azure-storage.md) Kapsayıcı uygulamalar, tüm Linux uygulamalarını ve Uygulama Hizmeti'nde çalışan Windows ve Linux özel kapsayıcılarını içerir. **Yeni Azure Depolama Alanı'nı** tıklatın ve özel depolama alanınızı aşağıdaki gibi yapılandırın:

- **Adı**: Görüntü adı.
- **Yapılandırma seçenekleri**: **Temel** veya **Gelişmiş**.
- **Depolama hesapları**: İstediğinizle kapitalın depo hesabı.
- **Depolama türü**: **Azure Blobs** veya **Azure Dosyaları**.
  > [!NOTE]
  > Windows kapsayıcı uygulamaları yalnızca Azure Dosyalarını destekler.
- **Depolama kapsayıcısı**: Temel yapılandırma için istediğiniz kapsayıcı.
- **Paylaşım adı**: Gelişmiş yapılandırma için dosya paylaşım adı.
- **Erişim anahtarı**: Gelişmiş yapılandırma için erişim anahtarı.
- **Montaj yolu**: Özel depolama monte etmek için konteyner mutlak yol.

Daha fazla bilgi için bkz. [Linux üzerinde App Service'te Azure Depolama'dan içerik sunma](containers/how-to-serve-content-from-azure-storage.md).

## <a name="configure-language-stack-settings"></a>Dil yığını ayarlarını yapılandırma

Linux uygulamaları için bkz:

- [ASP.NET Core](containers/configure-language-dotnetcore.md)
- [Node.js](containers/configure-language-nodejs.md)
- [PHP](containers/configure-language-php.md)
- [Python](containers/how-to-configure-python.md)
- [Java](containers/configure-language-java.md)
- [Ruby](containers/configure-language-ruby.md)

## <a name="configure-custom-containers"></a>Özel kapsayıcıları yapılandırma

Bkz. [Azure Uygulama Hizmeti için özel bir Linux kapsayıcısı yapılandırın](containers/configure-custom-container.md)

## <a name="next-steps"></a>Sonraki adımlar

- [Azure App Service'te özel etki alanı adını yapılandırma]
- [Azure App Service’te hazırlık ortamları ayarlama]
- [Azure Uygulama Hizmetinde TLS/SSL bağlama ile özel bir DNS adı güvenliğini sağlama](configure-ssl-bindings.md)
- [Tanılama günlüklerini etkinleştirme](troubleshoot-diagnostic-logs.md)
- [Azure Uygulama Hizmeti'nde bir uygulamayı ölçeklendirin]
- [Azure Uygulama Hizmeti'nde temel leri izleme]
- [applicationHost.config ayarlarını applicationHost.xdt ile değiştirin](https://github.com/projectkudu/kudu/wiki/Xdt-transform-samples)

<!-- URL List -->

[ASP.NET SignalR]: https://www.asp.net/signalr
[Azure Portalı]: https://portal.azure.com/
[Azure App Service'te özel etki alanı adını yapılandırma]: ./app-service-web-tutorial-custom-domain.md
[Azure App Service’te hazırlık ortamları ayarlama]: ./deploy-staging-slots.md
[How to: Monitor web endpoint status]: https://go.microsoft.com/fwLink/?LinkID=279906
[Azure Uygulama Hizmeti'nde temel leri izleme]: ./web-sites-monitor.md
[boru hattı modu]: https://www.iis.net/learn/get-started/introduction-to-iis/introduction-to-iis-architecture#Application
[Azure Uygulama Hizmeti'nde bir uygulamayı ölçeklendirin]: ./manage-scale-up.md
