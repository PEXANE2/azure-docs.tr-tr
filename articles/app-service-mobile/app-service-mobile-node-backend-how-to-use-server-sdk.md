---
title: Node.js arka uç Server SDK ile çalışma
description: Azure App Service Mobile Apps için Node.js arka uç Server SDK ile nasıl çalışacağınızı öğrenin.
ms.assetid: e7d97d3b-356e-4fb3-ba88-38ecbda5ea50
ms.tgt_pltfrm: mobile-multiple
ms.devlang: node
ms.topic: article
ms.date: 10/01/2016
ms.openlocfilehash: 566b6db829c9694a7e5e83cc25695e71ada0bc8d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79249951"
---
# <a name="how-to-use-the-mobile-apps-nodejs-sdk"></a>Mobil Uygulamalar Node.js SDK nasıl kullanılır?

[!INCLUDE [app-service-mobile-selector-server-sdk](../../includes/app-service-mobile-selector-server-sdk.md)]

Bu makalede, Azure Uygulama Hizmeti'nin Mobil Uygulamalar özelliğinde bir Düğüm.js arka uçta nasıl çalışılabildiğini gösteren ayrıntılı bilgiler ve örnekler sunulmaktadır.

## <a name="introduction"></a><a name="Introduction"></a>Giriş

Mobil Uygulamalar, bir web uygulamasına mobil cihazlar için optimize edilmiş veri erişimi Web API'sı ekleme olanağı sağlar. Mobil Uygulamalar SDK ASP.NET ve Node.js web uygulamaları için sağlanmaktadır. SDK aşağıdaki işlemleri sağlar:

* Veri erişimi için tablo işlemleri (okuma, ekleme, güncelleme, silme)
* Özel API işlemleri

Her iki işlem de Azure Uygulama Hizmeti'nin izin verdiği tüm kimlik sağlayıcılarında kimlik doğrulaması sağlar. Bu sağlayıcılar arasında Facebook, Twitter, Google ve Microsoft gibi sosyal kimlik sağlayıcılarının yanı sıra kurumsal kimlik için Azure Active Directory yer almaktadır.

Her kullanım örneği için örnekleri [GitHub'daki örnek dizinde]bulabilirsiniz.

## <a name="supported-platforms"></a>Desteklenen platformlar

Mobil Uygulamalar Düğümü.js SDK, Düğümün geçerli LTS sürümüve daha sonrasını destekler. Şu anda, en son LTS sürümü Düğüm v4.5.0 olduğunu. Düğüm'ün diğer sürümleri işe yarayabilir, ancak desteklenmez.

Mobil Uygulamalar Düğümü.js SDK iki veritabanı sürücüsüdesteklemektedir: 

* Düğüm-mssql sürücüsü Azure SQL Veritabanı'nı ve yerel SQL Server örneklerini destekler.  
* Sqlite3 sürücüsü yalnızca tek bir örnekte SQLite veritabanlarını destekler.

### <a name="create-a-basic-nodejs-back-end-by-using-the-command-line"></a><a name="howto-cmdline-basicapp"></a>Komut satırını kullanarak temel bir Düğüm.js arka uç oluşturma

Her Mobil Apps Node.js arka uç bir ExpressJS uygulaması olarak başlar. ExpressJS, Node.js için en popüler web hizmeti çerçevesidir. Aşağıdaki gibi temel bir [Express] uygulaması oluşturabilirsiniz:

1. Komut veya PowerShell penceresinde, projeniz için bir dizin oluşturun:

        mkdir basicapp

1. Paket `npm init` yapısını başlatmayı çalıştırın:

        cd basicapp
        npm init

   Komut, `npm init` projeyi başlatması için bir dizi soru sorar. Örnek çıktıya bakın:

   ![npm init çıkışı][0]

1. npm `express` `azure-mobile-apps` deposundan ve kitaplıkları yükleyin:

        npm install --save express azure-mobile-apps

1. Temel mobil sunucuyu uygulamak için bir app.js dosyası oluşturun:

    ```javascript
    var express = require('express'),
        azureMobileApps = require('azure-mobile-apps');

    var app = express(),
        mobile = azureMobileApps();

    // Define a TodoItem table.
    mobile.tables.add('TodoItem');

    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP.
    app.listen(process.env.PORT || 3000);
    ```

Bu uygulama, dinamik bir şema kullanarak temel bir`/tables/TodoItem`SQL veri deposuna kimlik doğrulamasız erişim sağlayan tek bir uç noktası () ile mobil cihazlar için optimize edilmiş bir Web API'si oluşturur. Bu istemci kitaplığı quickstarts aşağıdaki için uygundur:

* [Android istemciquickstart]
* [Apache Cordova istemci quickstart]
* [iOS İstemci hızlı başlat]
* [Windows Mağazası istemcisi hızlı başlat]
* [Xamarin.iOS istemcisi hızlı başlat]
* [Xamarin.Android istemci quickstart]
* [Xamarin.Forms istemci quickstart]

Bu temel uygulamanın kodunu [GitHub'daki basicapp örneğinde]bulabilirsiniz.

### <a name="create-a-nodejs-back-end-by-using-visual-studio-2015"></a><a name="howto-vs2015-basicapp"></a>Visual Studio 2015'i kullanarak Bir Düğüm.js arka uç oluşturma

Visual Studio 2015, IDE içinde Düğüm.js uygulamalarını geliştirmek için bir uzantı gerektirir. Başlamak için [Visual Studio için Düğüm.js Araçları 1.1'i]yükleyin. Yüklemeyi tamamladığınızda, bir Express 4.x uygulaması oluşturun:

1. Yeni **Proje** iletişim kutusunu açın **(Dosya** > **Yeni** > **Proje'den).**
1. **Şablonları** > Genişlet**JavaScript** > **Düğümü.js**.
1. **Temel Azure Düğümü.js Express 4 Uygulamasını**seçin.
1. Proje adını doldurun. **Tamam'ı**seçin.

   ![Visual Studio 2015 yeni projesi][1]
1. **NPM** düğümüne sağ tıklayın ve **Yeni npm paketlerini yükle'yi**seçin.
1. İlk Düğüm.js uygulamanızı oluşturduktan sonra npm kataloğunu yenilemeniz gerekebilir. Gerekirse **Yenile'yi** seçin.
1. Arama kutusuna **azure-mobil uygulamaları** girin. **Azure-mobile-apps 2.0.0** paketini seçin ve ardından **Paketi Yükle'yi**seçin.

   ![Yeni npm paketleri yükleyin][2]
1. **Kapat**'ı seçin.
1. Mobil Uygulamalar SDK desteği eklemek için app.js dosyasını açın. Kitaplık `require` deyimlerinin altındaki satır 6'da aşağıdaki kodu ekleyin:

    ```javascript
    var bodyParser = require('body-parser');
    var azureMobileApps = require('azure-mobile-apps');
    ```

    Diğer `app.use` ifadelerden sonra yaklaşık 27 satırında aşağıdaki kodu ekleyin:

    ```javascript
    app.use('/users', users);

    // Mobile Apps initialization
    var mobile = azureMobileApps();
    mobile.tables.add('TodoItem');
    app.use(mobile);
    ```

    Dosyayı kaydedin.

1. Uygulamayı yerel olarak çalıştırın (API'de `http://localhost:3000`sunulur) veya Azure'da yayımlayın.

### <a name="create-a-nodejs-back-end-by-using-the-azure-portal"></a><a name="create-node-backend-portal"></a>Azure portalını kullanarak bir Düğüm.js arka uç oluşturma

[Azure portalında]bir Mobil Uygulamalar arka uç oluşturabilirsiniz. Mobil uygulama öğreticisi [oluştur'u](app-service-mobile-ios-get-started.md) izleyerek aşağıdaki adımları tamamlayabilir veya birlikte bir istemci ve sunucu oluşturabilirsiniz. Öğretici bu talimatların basitleştirilmiş bir sürümünü içerir ve kavram kanıtı projeleri için en iyisidir.

[!INCLUDE [app-service-mobile-dotnet-backend-create-new-service-classic](../../includes/app-service-mobile-dotnet-backend-create-new-service-classic.md)]

**Başlangıç** bölmesinde, **tablo API'si oluştur**altında, arka uç diliniz olarak **Düğüm.js'yi** seçin.
Bunun tüm **site içeriğinin üzerine yazacağını kabul ettiğim**için kutuyu seçin ve ardından **TodoItem oluştur tablosunu**seçin.

### <a name="download-the-nodejs-back-end-quickstart-code-project-by-using-git"></a><a name="download-quickstart"></a>Git kullanarak Node.js arka uç quickstart kodu projesini indirin

Portalın **Hızlı başlangıç** bölmesini kullanarak bir Node.js Mobile Apps arka uç oluşturduğunuzda, sizin için bir Node.js projesi oluşturulur ve sitenize dağıtılır. Portalda, tablolar ve API'ler ekleyebilir ve Düğüm.js arka uç için kod dosyalarını edebilirsiniz. Tablolar ve API'ler eklemek veya değiştirmek ve sonra projeyi yeniden yayımlamak için arka uç projesini indirmek için çeşitli dağıtım araçlarını da kullanabilirsiniz. Daha fazla bilgi için [Azure Uygulama Hizmeti dağıtım kılavuzuna]bakın.

Aşağıdaki yordam, hızlı başlat proje kodunu indirmek için bir Git deposu kullanır:

1. Git'i yükleyin, zaten yapmadıysanız. Git'i yüklemek için gereken adımlar işletim sistemleri arasında değişir. İşletim sistemine özel dağıtımlar ve yükleme kılavuzu için [bkz.](https://git-scm.com/book/en/Getting-Started-Installing-Git)
2. Bkz. Arka uç siteniz için Git deposunu etkinleştirmek için [deponuzu hazırlayın.](../app-service/deploy-local-git.md#prepare-your-repository) Dağıtım kullanıcı adı ve parolasını not alın.
3. Mobil Uygulamalar arka uç için bölmede, Git **klon URL** ayarı bir not yapın.
4. Git `git clone` klon URL'sini kullanarak komutu çalıştırın. Aşağıdaki örnekte olduğu gibi gerektiğinde parolanızı girin:

        $ git clone https://username@todolist.scm.azurewebsites.net:443/todolist.git

5. Yerel dizine (önceki`/todolist` örnekte) göz atın ve proje dosyalarının indirildiğini unutmayın. Todoitem.json dosyasını dizinde `/tables` bulun. Bu dosya, tablodaki izinleri tanımlar. Ayrıca todoitem.js dosyasını aynı dizinde bulabilirsiniz. Tablo için CRUD işlem komut dosyalarını tanımlar.
6. Proje dosyalarında değişiklik yaptıktan sonra, değişiklikleri eklemek, işlemek ve sonra siteye yüklemek için aşağıdaki komutları çalıştırın:

        $ git commit -m "updated the table script"
        $ git push origin master

   Projeye yeni dosyalar eklediğinizde, önce komutu `git add .` çalıştırmanız gerekir.

Site, yeni bir taahhüt kümesi siteye her itildiğinde yeniden yayınlanır.

### <a name="publish-your-nodejs-back-end-to-azure"></a><a name="howto-publish-to-azure"></a>Düğümünüzü arka uçta Azure'da yayımlayın

Microsoft Azure, Mobil Uygulama Düğümünüzü Azure hizmetine geri yayınlamak için birçok mekanizma sağlar. Bu mekanizmalar Visual Studio'ya entegre dağıtım araçlarını, komut satırı araçlarını ve kaynak denetimine dayalı sürekli dağıtım seçeneklerini içerir. Daha fazla bilgi için [Azure Uygulama Hizmeti dağıtım kılavuzuna]bakın.

Azure Uygulama Hizmeti'nin, arka ucu yayımlamadan önce gözden geçirmeniz gereken Node.js uygulamaları için özel tavsiyeleri vardır:

* Düğüm [sürümünü belirtme]
* Düğüm [modülleri] nasıl kullanılır?

### <a name="enable-a-home-page-for-your-application"></a><a name="howto-enable-homepage"></a>Uygulamanız için bir giriş sayfası etkinleştirme

Birçok uygulama, web ve mobil uygulamaların bir birleşimidir. ExpressJS çerçevesini iki yönü birleştirmek için kullanabilirsiniz. Ancak bazen yalnızca bir mobil arabirim uygulamak isteyebilirsiniz. Uygulama hizmetinin çalışır durumda olduğundan emin olmak için bir ana sayfa sağlamak yararlıdır. Kendi ana sayfanızı sağlayabilir veya geçici bir giriş sayfası etkinleştirebilirsiniz. Geçici bir giriş sayfasını etkinleştirmek için Mobil Uygulamaları anında kullanmak için aşağıdaki kodu kullanın:

```javascript
var mobile = azureMobileApps({ homePage: true });
```

Bu seçeneğin yalnızca yerel olarak gelişirken kullanılabilir olmasını istiyorsanız, bu ayarı azureMobile.js dosyanıza ekleyebilirsiniz.

## <a name="table-operations"></a><a name="TableOperations"></a>Tablo işlemleri

Azure-mobile-apps Node.js Server SDK, Azure SQL Veritabanı'nda depolanan veri tablolarını Web API'sı olarak ortaya çıkaracak mekanizmalar sağlar. Beş işlem sağlar:

| İşlem | Açıklama |
| --- | --- |
| GET /tables/*tablo adı* |Tüm kayıtları masaya getir. |
| GET /tables/*tablo adı*/:id |Tabloya belirli bir kayıt alın. |
| POST /tables/*tablo adı* |Tabloda bir kayıt oluşturun. |
| PATCH /tables/*tablo adı*/:id |Tablodaki kaydı güncelleştirin. |
| DELETE /tables/*tablo adı*/:id |Tablodaki bir kaydı silin. |

Bu Web [API, OData'yı] destekler ve tablo şemasını [çevrimdışı veri eşitlemeyi]desteklemek için genişletir.

### <a name="define-tables-by-using-a-dynamic-schema"></a><a name="howto-dynamicschema"></a>Dinamik bir şema kullanarak tabloları tanımlama

Bir tabloyu kullanamadan önce bunu tanımlamanız gerekir. Tabloları statik bir şema (şemadaki sütunları tanımladığınız yer) kullanarak veya dinamik olarak (SDK'nın gelen isteklere göre şema'yı kontrol ettiği) tanımlayabilirsiniz. Ayrıca, tanıma JavaScript kodu ekleyerek Web API'sının belirli yönlerini denetleyebilirsiniz.

En iyi yöntem olarak, `tables` dizindeki javascript dosyasındaki her tabloyu tanımlamalı ve sonra tabloları almak için `tables.import()` yöntemi kullanmalısınız. Temel uygulama örneğini genişleterek app.js dosyasını ayarlarsınız:

```javascript
var express = require('express'),
    azureMobileApps = require('azure-mobile-apps');

var app = express(),
    mobile = azureMobileApps();

// Define the database schema that is exposed.
mobile.tables.import('./tables');

// Provide initialization of any tables that are statically defined.
mobile.tables.initialize().then(function () {
    // Add the Mobile API so it is accessible as a Web API.
    app.use(mobile);

    // Start listening on HTTP.
    app.listen(process.env.PORT || 3000);
});
```

Tabloyu ./tables/TodoItem.js'de tanımlayın:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Additional configuration for the table goes here.

module.exports = table;
```

Tablolar varsayılan olarak dinamik bir şema kullanır. Dinamik şemayı genel olarak kapatmak için, `MS_DynamicSchema` uygulama ayarını Azure portalında false olarak ayarlayın.

[GitHub'da yapılacak]örnekte tam bir örnek bulabilirsiniz.

### <a name="define-tables-by-using-a-static-schema"></a><a name="howto-staticschema"></a>Statik şema kullanarak tabloları tanımlama

Web API üzerinden ortaya çıkarmak için sütunları açıkça tanımlayabilirsiniz. Azure-mobile-apps Node.js SDK, çevrimdışı veri eşitlemesi için gereken ek sütunları sağladığınız listeye otomatik olarak ekler. Örneğin, hızlı başlatma istemci uygulamaları iki sütunlu `text` bir tablo gerektirir: (dize) ve `complete` (bir Boolean).  
Tablo tablo tanımı JavaScript dosyasında (dizinde `tables` bulunan) aşağıdaki gibi tanımlanabilir:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Define the columns within the table.
table.columns = {
    "text": "string",
    "complete": "boolean"
};

// Turn off the dynamic schema.
table.dynamicSchema = false;

module.exports = table;
```

Tabloları statik olarak tanımlıyorsanız, başlangıç `tables.initialize()` veritabanı şemasını oluşturmak için yöntemi de aramanız gerekir. Yöntem, `tables.initialize()` web hizmetinin veritabanı başlatılmasından önce isteklere hizmet etmemeleri için bir [söz] döndürür.

### <a name="use-sql-server-express-as-a-development-data-store-on-your-local-machine"></a><a name="howto-sqlexpress-setup"></a>SQL Server Express'i yerel makinenizde geliştirme veri deposu olarak kullanın

Mobil Uygulamalar Düğümü.js SDK, verileri kutunun dışına çıkarmak için üç seçenek sunar:

* Kalıcı olmayan bir örnek deposu sağlamak için **bellek** sürücüsünü kullanın.
* Geliştirme için bir SQL Server Express veri deposu sağlamak için **mssql** sürücüsünü kullanın.
* Üretim için bir Azure SQL Veritabanı veri deposu sağlamak için **mssql** sürücüsünü kullanın.

Mobil Apps Node.js SDK, hem SQL Server Express hem de SQL Veritabanı'na bağlantı kurmak ve kullanmak için [mssql Node.js paketini] kullanır. Bu paket, SQL Server Express örneğinde TCP bağlantılarını etkinleştirmenizi gerektirir.

> [!TIP]
> Bellek sürücüsü sınama için tam bir olanak kümesi sağlamaz. Arka uçunuzu yerel olarak test etmek istiyorsanız, bir SQL Server Express veri deposu ve mssql sürücüsü kullanmanızı öneririz.

1. [Microsoft SQL Server 2014 Express'i]indirin ve yükleyin. SQL Server 2014 Express'i Tools sürümüyle yüklediğinizden emin olun. 64 bit desteği açıkça gerektirmediğiniz sürece, 32 bit sürümü çalışırken daha az bellek tüketir.
1. SQL Server 2014 Configuration Manager çalıştırın:

   a. Ağaç menüsündeki **SQL Server Network Configuration** düğümünün genişletilmesi.

   b. **SQLEXPRESS için Protokoller'i**seçin.

   c. **TCP/IP'ye** sağ tıklayın ve **Etkinleştir'i**seçin. Açılan iletişim kutusunda **Tamam'ı** seçin.

   d. **TCP/IP'ye** sağ tıklayın ve **Özellikler'i**seçin.

   e. IP **Adresleri** sekmesini seçin.

   f. **IPAll** düğümlerini bulun. **TCP Bağlantı Noktası** alanına **1433**girin.

      ![TCP/IP için SQL Server Express'i yapılandırma][3]

   g. **Tamam'ı**seçin. Açılan iletişim kutusunda **Tamam'ı** seçin.

   h. Ağaç menüsünde **SQL Server Services'ı** seçin.

   i. **SQL Server (SQLEXPRESS) tuşuna** sağ tıklayın ve **Yeniden Başlat'ı**seçin.

   j. SQL Server 2014 Configuration Manager'ı kapatın.

1. SQL Server 2014 Management Studio'nuzu çalıştırın ve yerel SQL Server Express örneğinize bağlanın:

   1. Nesne Gezgini'nde örneğinize sağ tıklayın ve **Özellikler'i**seçin.
   1. **Güvenlik** sayfasını seçin.
   1. SQL **Server ve Windows Kimlik Doğrulama modunun** seçildiğinden emin olun.
   1. **Tamam'ı**seçin.

      ![SQL Server Express kimlik doğrulamasını yapılandırma][4]
   1. Nesne Gezgini'ndeki **Güvenlik** > **Girişlerini** Genişletin.
   1. **Girişler'e** sağ tıklayın ve **Yeni Giriş'i**seçin.
   1. Giriş adı girin. **SQL Server kimlik doğrulaması**’nı seçin. Bir parola girin ve ardından parolayı **onayla'ya**aynı parolayı girin. Parola, Windows karmaşıklığı gereksinimlerini karşılamalıdır.
   1. **Tamam'ı**seçin.

      ![SQL Server Express'e yeni bir kullanıcı ekleme][5]
   1. Yeni giriş inizi sağ tıklatın ve **Özellikler'i**seçin.
   1. Sunucu **Rolleri** sayfasını seçin.
   1. **dbcreator** sunucu rolü için onay kutusunu seçin.
   1. **Tamam'ı**seçin.
   1. SQL Server 2015 Management Studio'u kapatın.

Seçtiğiniz kullanıcı adını ve parolayı kaydettiğinizden emin olun. Veritabanı gereksinimlerinize bağlı olarak ek sunucu rolleri veya izinler atamanız gerekebilir.

Düğüm.js uygulaması, bu `SQLCONNSTR_MS_TableConnectionString` veritabanının bağlantı dizesi için ortam değişkenini okur. Bu değişkeni ortamınızda ayarlayabilirsiniz. Örneğin, bu ortam değişkenini ayarlamak için PowerShell'i kullanabilirsiniz:

    $env:SQLCONNSTR_MS_TableConnectionString = "Server=127.0.0.1; Database=mytestdatabase; User Id=azuremobile; Password=T3stPa55word;"

TCP/IP bağlantısı üzerinden veritabanına erişin. Bağlantı için bir kullanıcı adı ve parola sağlayın.

### <a name="configure-your-project-for-local-development"></a><a name="howto-config-localdev"></a>Projenizi yerel geliştirme için yapılandırın

Mobile Apps, yerel dosya sisteminden *azureMobile.js* adlı bir JavaScript dosyasını okur. Bu dosyayı, üretimdeki Mobil Uygulamalar SDK'sını yapılandırmak için kullanmayın. Bunun yerine, Azure [portalında] **Uygulama ayarlarını** kullanın.

AzureMobile.js dosyası bir yapılandırma nesnesi dışa aktarmalıdır. En yaygın ayarlar şunlardır:

* Veritabanı ayarları
* Tanılama günlüğe kaydetme ayarları
* Alternatif CORS ayarları

Bu örnek **azureMobile.js** dosyası önceki veritabanı ayarlarını uygular:

```javascript
module.exports = {
    cors: {
        origins: [ 'localhost' ]
    },
    data: {
        provider: 'mssql',
        server: '127.0.0.1',
        database: 'mytestdatabase',
        user: 'azuremobile',
        password: 'T3stPa55word'
    },
    logging: {
        level: 'verbose'
    }
};
```

Parolaların bulutta depolanmasını önlemek için **.gitignore** dosyanıza (veya diğer kaynak kodu denetimi yoksayma dosyasına) **azureMobile.js** eklemenizi öneririz. [Azure portalındaki] **Uygulama ayarlarında** üretim ayarlarını her zaman yapılandırın.

### <a name="configure-app-settings-for-your-mobile-app"></a><a name="howto-appsettings"></a>Mobil uygulamanız için uygulama ayarlarını yapılandırın

azureMobile.js dosyasındaki ayarların çoğunda [Azure portalında]eşdeğer bir uygulama ayarı vardır. **Uygulama ayarlarında**uygulamanızı yapılandırmak için aşağıdaki listeyi kullanın:

| Uygulama ayarı | azureMobile.js ayarı | Açıklama | Geçerli değerler |
|:--- |:--- |:--- |:--- |
| **MS_MobileAppName** |ad |Uygulamanın adı |string |
| **MS_MobileLoggingLevel** |logging.level |Günlük için en az günlük ileti düzeyi |hata, uyarı, bilgi, ayrıntılı, hata ayıklama, aptal |
| **MS_DebugMode** |hata ayıklama |Hata ayıklama modunu etkinleştirir veya devre dışı |doğru, yanlış |
| **MS_TableSchema** |data.schema |SQL tabloları için varsayılan şema adı |string (varsayılan: dbo) |
| **MS_DynamicSchema** |data.dynamicSchema |Hata ayıklama modunu etkinleştirir veya devre dışı |doğru, yanlış |
| **MS_DisableVersionHeader** |sürüm (tanımlanmamış olarak ayarlanmıştır) |X-ZUMO-Server-Version üstbilgisini devre dışı |doğru, yanlış |
| **MS_SkipVersionCheck** |skipversioncheck |İstemci API sürüm denetimini devre dışı |doğru, yanlış |

Uygulama ayarını ayarlamak için:

1. [Azure portalında]oturum açın.
1. **Tüm kaynakları** veya **Uygulama Hizmetlerini**seçin ve ardından mobil uygulamanızın adını seçin.
1. **Ayarlar** bölmesi varsayılan olarak açılır. Yoksa **Ayarlar'ı**seçin.
1. **GENEL** menüde **Uygulama ayarlarını**seçin.
1. **Uygulama ayarları** bölümüne gidin.
1. Uygulama ayarınız zaten varsa, değeri ayarlamak için uygulama ayarı değerini seçin.
   Uygulama ayarınız yoksa, **Anahtar** kutusuna uygulama ayarını ve **Değer** kutusuna değeri girin.
1. **Kaydet'i**seçin.

Çoğu uygulama ayarlarını değiştirmek için bir hizmetyeniden başlatması gerekiyor.

### <a name="use-sql-database-as-your-production-data-store"></a><a name="howto-use-sqlazure"></a>Üretim veri deponuz olarak SQL Veritabanı'nı kullanın

<!--- ALTERNATE INCLUDE - we can't use ../includes/app-service-mobile-dotnet-backend-create-new-service.md - slightly different semantics -->

Azure SQL Veritabanı'nı veri deposu olarak kullanmak, tüm Azure Uygulama Hizmeti uygulama türlerinde aynıdır. Bunu daha önce yapmadıysanız, bir Mobil Uygulama arka uç oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalında]oturum açın.
1. Pencerenin sol üst kısmında **Web + Mobile** > **Mobile App**> **+Yenİ** düğmesini seçin ve ardından Mobil Uygulamalarınız için bir ad sağlayın.
1. Kaynak **Grubu** kutusuna, uygulamanızla aynı adı girin.
1. Varsayılan Uygulama Hizmeti planı seçilir. Uygulama Hizmeti planınızı değiştirmek istiyorsanız:

   a. **Uygulama Hizmet Planı** > **+Yeni Oluştur'u**seçin.

   b. Yeni Uygulama Hizmeti planının adını sağlayın ve uygun bir konum seçin.

   c. Hizmet için uygun bir fiyatlandırma katmanı seçin. **Ücretsiz** ve **Paylaşılan**gibi daha fazla fiyatlandırma seçeneğini görüntülemek için **Tümünü** Görüntüle'yi seçin.

   d. **Seç** düğmesini tıklatın.

   e. **Uygulama Hizmeti planı** bölmesinde Tamam'ı seçin. **OK**
1. **Oluştur'u**seçin.

Mobil Uygulamalar'ın arka uçta sağlanması birkaç dakika sürebilir. Mobil Uygulamalar arka uç sağlandıktan sonra, portal Mobil Uygulamalar için **Ayarlar** bölmesini arka uçta açar.

Varolan bir SQL veritabanını Mobil Uygulamalarınızın arka ucuna bağlamayı veya yeni bir SQL veritabanı oluşturmayı seçebilirsiniz. Bu bölümde bir SQL veritabanı oluşturuyoruz.

> [!NOTE]
> Mobil Uygulamalar arka uçla aynı konumda zaten bir veritabanınız varsa, bunun yerine **varolan bir veritabanını kullan'ı** ve ardından bu veritabanını seçebilirsiniz. Yüksek gecikme süreleri nedeniyle veritabanının farklı bir konumda kullanılmasını önermiyoruz.

1. Yeni Mobil Uygulamalar arka uçta **Ayarlar** > **Mobil Uygulama** > **Verilerini** > **+Ekle'yi**seçin.
1. Veri **bağlantısı ekle** bölmesinde **SQL Veritabanı'nı**seçin - Gerekli ayarları > yapılandırın**Yeni bir veritabanı oluşturun.** **Ad** kutusuna yeni veritabanının adını girin.
1. **Sunucu'yı**seçin. Yeni **sunucu** bölmesine, **Sunucu adı** kutusuna benzersiz bir sunucu adı girin ve uygun bir sunucu yöneticisi girişi ve parola sağlayın. **Sunucuya erişim için azure hizmetlerine Izin** ver'in seçildiğinden emin olun. **Tamam'ı**seçin.

   ![Bir Azure SQL veritabanı oluşturma][6]
1. Yeni **veritabanı** bölmesinde **Tamam'ı**seçin.
1. **Veri bağlantısı ekle** bölmesine geri dön, Bağlantı **dizesini**seçin ve veritabanını oluşturduğunuzda sağladığınız giriş ve parolayı girin. Varolan bir veritabanı kullanıyorsanız, bu veritabanı için giriş kimlik bilgilerini sağlayın. **Tamam'ı**seçin.
1. **Yeniden veri bağlantısı ekle** bölmesinde, veritabanını oluşturmak için **Tamam'ı** seçin.

<!--- END OF ALTERNATE INCLUDE -->

Veritabanının oluşturulması birkaç dakika sürebilir. Dağıtımın ilerlemesini izlemek için **Bildirimler** alanını kullanın. Veritabanı başarıyla dağıtılana kadar ilerleme kaydetmeyin. Veritabanı dağıtıldıktan sonra, Mobil Uygulamalar arka uç uygulama ayarlarınızda SQL Veritabanı örneği için bir bağlantı dizesi oluşturulur. Bu uygulama ayarını **Ayarlar** > **Uygulama ayarları** > **Bağlantı dizelerinde**görebilirsiniz.

### <a name="require-authentication-for-access-to-tables"></a><a name="howto-tables-auth"></a>Tablolara erişmek için kimlik doğrulama gerektirir

Uygulama Hizmeti Kimlik Doğrulaması'nı `tables` bitiş noktasıyla birlikte kullanmak istiyorsanız, önce Azure [portalında] Uygulama Hizmeti Kimlik Doğrulaması'nı yapılandırmanız gerekir. Daha fazla bilgi için, kullanmak istediğiniz kimlik sağlayıcısının yapılandırma kılavuzuna bakın:

* [Azure Active Directory kimlik doğrulamasını yapılandırma]
* [Facebook kimlik doğrulamasını yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft kimlik doğrulamasını yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Her tablo, tabloya erişimi denetlemek için kullanabileceğiniz bir erişim özelliğine sahiptir. Aşağıdaki örnek, kimlik doğrulaması gerekli olan statik olarak tanımlanmış bir tabloyu gösterir.

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Define the columns within the table.
table.columns = {
    "text": "string",
    "complete": "boolean"
};

// Turn off the dynamic schema.
table.dynamicSchema = false;

// Require authentication to access the table.
table.access = 'authenticated';

module.exports = table;
```

Erişim özelliği üç değerden birini alabilir:

* *anonim* istemci uygulaması kimlik doğrulaması olmadan verileri okumak için izin verilir gösterir.
* *kimlik doğrulaması,* istemci uygulamasının istekle birlikte geçerli bir kimlik doğrulama belirteci göndermesi gerektiğini gösterir.
* *devre dışı bırakılmış,* bu tablonun şu anda devre dışı bırakıldığını gösterir.

Erişim özelliği tanımlanmamışsa, kimlik doğrulaması olmayan erişime izin verilir.

### <a name="use-authentication-claims-with-your-tables"></a><a name="howto-tables-getidentity"></a>Tablolarınızla kimlik doğrulama taleplerini kullanma
Kimlik doğrulaması ayarlandığında istenen çeşitli talepler ayarlayabilirsiniz. Bu talepler normalde `context.user` nesne üzerinden kullanılamaz. Ancak, `context.user.getIdentity()` yöntemi kullanarak bunları alabilirsiniz. Yöntem, `getIdentity()` bir nesneye çözüm veren bir söz döndürür. Nesne kimlik doğrulama yöntemi (`facebook`, `google`, `twitter` `microsoftaccount`, `aad`, veya ) tarafından anahtarlanır.

Örneğin, Microsoft hesap kimlik doğrulaması ayarlayıp e-posta adresleri talebini talep ederseniz, aşağıdaki tablo denetleyicisiyle e-posta adresini kayda ekleyebilirsiniz:

```javascript
var azureMobileApps = require('azure-mobile-apps');

// Create a new table definition.
var table = azureMobileApps.table();

table.columns = {
    "emailAddress": "string",
    "text": "string",
    "complete": "boolean"
};
table.dynamicSchema = false;
table.access = 'authenticated';

/**
* Limit the context query to those records with the authenticated user email address
* @param {Context} context the operation context
* @returns {Promise} context execution Promise
*/
function queryContextForEmail(context) {
    return context.user.getIdentity().then((data) => {
        context.query.where({ emailAddress: data.microsoftaccount.claims.emailaddress });
        return context.execute();
    });
}

/**
* Adds the email address from the claims to the context item - used for
* insert operations
* @param {Context} context the operation context
* @returns {Promise} context execution Promise
*/
function addEmailToContext(context) {
    return context.user.getIdentity().then((data) => {
        context.item.emailAddress = data.microsoftaccount.claims.emailaddress;
        return context.execute();
    });
}

// Configure specific code when the client does a request.
// READ: only return records that belong to the authenticated user.
table.read(queryContextForEmail);

// CREATE: add or overwrite the userId based on the authenticated user.
table.insert(addEmailToContext);

// UPDATE: only allow updating of records that belong to the authenticated user.
table.update(queryContextForEmail);

// DELETE: only allow deletion of records that belong to the authenticated user.
table.delete(queryContextForEmail);

module.exports = table;
```

Hangi iddiaların mevcut olduğunu görmek için, sitenizin `/.auth/me` bitiş noktasını görüntülemek için bir web tarayıcısı kullanın.

### <a name="disable-access-to-specific-table-operations"></a><a name="howto-tables-disabled"></a>Belirli tablo işlemlerine erişimi devre dışı

Tabloda görünmeye ek olarak, erişim özelliği tek tek işlemleri denetlemek için kullanılabilir. Dört işlem vardır:

* `read`masadaki RESTful GET işlemidir.
* `insert`masadaki RESTful POST işlemidir.
* `update`masadaki RESTful PATCH işlemidir.
* `delete`tablodaki RESTful DELETE işlemidir.

Örneğin, salt okunur kimliği doğrulanmamış bir tablo sağlamak isteyebilirsiniz:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Read-only table. Only allow READ operations.
table.read.access = 'anonymous';
table.insert.access = 'disabled';
table.update.access = 'disabled';
table.delete.access = 'disabled';

module.exports = table;
```

### <a name="adjust-the-query-that-is-used-with-table-operations"></a><a name="howto-tables-query"></a>Tablo işlemlerinde kullanılan sorguyu ayarlama

Tablo işlemleri için ortak bir gereksinim, verilerin sınırlı bir görünümünü sağlamaktır. Örneğin, yalnızca kendi kayıtlarınızı okuyabildiğiniz veya güncelleştirebileceğiniz, kimlik doğrulaması kullanıcı kimliğiyle etiketlenmiş bir tablo sağlayabilirsiniz. Aşağıdaki tablo tanımı bu işlevselliği sağlar:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Define a static schema for the table.
table.columns = {
    "userId": "string",
    "text": "string",
    "complete": "boolean"
};
table.dynamicSchema = false;

// Require authentication for this table.
table.access = 'authenticated';

// Ensure that only records for the authenticated user are retrieved.
table.read(function (context) {
    context.query.where({ userId: context.user.id });
    return context.execute();
});

// When adding records, add or overwrite the userId with the authenticated user.
table.insert(function (context) {
    context.item.userId = context.user.id;
    return context.execute();
});

module.exports = table;
```

Normalde sorgu çalıştıran işlemler, bir `where` yan tümce kullanarak ayarlayabileceğiniz bir sorgu özelliğine sahiptir. Sorgu özelliği, Bir OData sorgusunu veri arka ucunun işleyebilir bir şey için kullanılan bir [QueryJS] nesnesidir. Basit eşitlik durumları için (önceki gibi), bir harita kullanabilirsiniz. Ayrıca belirli SQL yan tümceleri ekleyebilirsiniz:

```javascript
context.query.where('myfield eq ?', 'value');
```

### <a name="configure-a-soft-delete-on-a-table"></a><a name="howto-tables-softdelete"></a>Tabloda yumuşak silme yapılandırma

Yumuşak bir silme aslında kayıtları silmez. Bunun yerine silinen sütunu doğru olarak ayarlayarak veritabanı içinde silinmiş olarak işaretler. Mobil İstemci SDK kullanmadığı `IncludeDeleted()`sürece Mobil Uygulamalar SDK, yumuşak silinmiş kayıtları sonuçlardan otomatik olarak kaldırır. Bir tabloyu yumuşak bir silme için `softDelete` yapılandırmak için, tablo tanım dosyasındaki özelliği ayarlayın:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Define the columns within the table.
table.columns = {
    "text": "string",
    "complete": "boolean"
};

// Turn off the dynamic schema.
table.dynamicSchema = false;

// Turn on soft delete.
table.softDelete = true;

// Require authentication to access the table.
table.access = 'authenticated';

module.exports = table;
```

Kayıtları silen bir mekanizma oluşturmanız gerekir: istemci uygulaması, WebJob, Azure işlevi veya özel api.

### <a name="seed-your-database-with-data"></a><a name="howto-tables-seeding"></a>Veri tabanınızı verilerle tohumla

Yeni bir uygulama oluştururken, veri içeren bir tablo tohumlamak isteyebilirsiniz. Aşağıdaki gibi tablo tanımı JavaScript dosyası içinde bunu yapabilirsiniz:

```javascript
var azureMobileApps = require('azure-mobile-apps');

var table = azureMobileApps.table();

// Define the columns within the table.
table.columns = {
    "text": "string",
    "complete": "boolean"
};
table.seed = [
    { text: 'Example 1', complete: false },
    { text: 'Example 2', complete: true }
];

// Turn off the dynamic schema.
table.dynamicSchema = false;

// Require authentication to access the table.
table.access = 'authenticated';

module.exports = table;
```

Verilerin tohumlanması yalnızca tabloyu oluşturmak için Mobil Uygulamalar SDK'sını kullandığınızda gerçekleşir. Tablo veritabanında zaten varsa, tabloya veri enjekte edilemez. Dinamik şema açıksa, şema tohumlu verilerden çıkarılır.

Hizmet çalışmaya başladığında tabloyu `tables.initialize()` oluşturmak için yöntemi açıkça aramanızı öneririz.

### <a name="enable-swagger-support"></a><a name="Swagger"></a>Swagger desteğini etkinleştirin
Mobile Apps yerleşik [Swagger] desteğiyle birlikte gelir. Swagger desteğini etkinleştirmek için, önce swagger-ui'yi bağımlılık olarak yükleyin:

    npm install --save swagger-ui

Daha sonra Mobil Uygulamalar oluşturucusunda Swagger desteğini etkinleştirebilirsiniz:

```javascript
var mobile = azureMobileApps({ swagger: true });
```

Muhtemelen yalnızca geliştirme sürümlerinde Swagger desteğini etkinleştirmek istiyorsunuz. Bunu uygulama ayarını `NODE_ENV` kullanarak yapabilirsiniz:

```javascript
var mobile = azureMobileApps({ swagger: process.env.NODE_ENV !== 'production' });
```

Bitiş `swagger` noktası*sitenizin*http:// .azurewebsites.net/swagger'da yer alır. Swagger UI'ye bitiş `/swagger/ui` noktasından erişebilirsiniz. Tüm uygulamanızda kimlik doğrulaması gerektirmeyi seçerseniz, Swagger bir hata üretir. En iyi sonuçlar için, Azure Uygulama Hizmeti Kimlik Doğrulama/Yetkilendirme ayarlarında kimlik doğrulamasına izin `table.access` vermeyi ve ardından özelliği kullanarak kimlik doğrulamasını denetlemeyi seçin.

Yalnızca yerel olarak geliştirmek için Swagger desteği istiyorsanız azureMobile.js dosyanıza Swagger seçeneğini de ekleyebilirsiniz.

## <a name="push-notifications"></a><a name="push"/>Anında iletme bildirimleri

Mobil Uygulamalar, tüm büyük platformlardaki milyonlarca cihaza hedefli anında iletme bildirimleri gönderebilmeniz için Azure Bildirim Hub'larıyla tümleşir. Bildirim Hub'larını kullanarak iOS, Android ve Windows cihazlarına anında iletme bildirimleri gönderebilirsiniz. Bildirim Hub'ları ile yapabilecekleriniz hakkında daha fazla bilgi edinmek için [Bildirim Hub'larına genel bakış'a](../notification-hubs/notification-hubs-push-notification-overview.md)bakın.

### <a name="send-push-notifications"></a><a name="send-push"></a>Anında iletme bildirimleri gönderme

Aşağıdaki kod, kayıtlı iOS aygıtlarına yayın anında iletme bildirimi göndermek için nesnenin nasıl kullanılacağını `push` gösterir:

```javascript
// Create an APNS payload.
var payload = '{"aps": {"alert": "This is an APNS payload."}}';

// Only do the push if configured.
if (context.push) {
    // Send a push notification by using APNS.
    context.push.apns.send(null, payload, function (error) {
        if (error) {
            // Do something or log the error.
        }
    });
}
```

İstemciden bir şablon itme kaydı oluşturarak, bunun yerine desteklenen tüm platformlardaki aygıtlara şablon push iletisi gönderebilirsiniz. Aşağıdaki kod, şablon bildiriminin nasıl gönderilebildiğini gösterir:

```javascript
// Define the template payload.
var payload = '{"messageParam": "This is a template payload."}';

// Only do the push if configured.
if (context.push) {
    // Send a template notification.
    context.push.send(null, payload, function (error) {
        if (error) {
            // Do something or log the error.
        }
    });
}
```

### <a name="send-push-notifications-to-an-authenticated-user-by-using-tags"></a><a name="push-user"></a>Etiketleri kullanarak kimlik doğrulaması yapılan kullanıcıya anında iletme bildirimleri gönderme
Kimlik doğrulaması yapılan bir kullanıcı anında iletme bildirimleri için kaydolduğunda, kullanıcı kimliği etiketi otomatik olarak kayda eklenir. Bu etiketi kullanarak, belirli bir kullanıcı tarafından kayıtlı tüm aygıtlara anında iletme bildirimleri gönderebilirsiniz. Aşağıdaki kod, isteği yapan kullanıcının SID'sini alır ve bu kullanıcı için her aygıt kaydına şablon anında iletme bildirimi gönderir:

```javascript
// Only do the push if configured.
if (context.push) {
    // Send a notification to the current user.
    context.push.send(context.user.id, payload, function (error) {
        if (error) {
            // Do something or log the error.
        }
    });
}
```

Kimlik doğrulaması yapılan bir istemciden gelen anında iletme bildirimleri için kaydolurken, kayıt girişiminde bulunmadan önce kimlik doğrulamanın tamamlandığından emin olun.

## <a name="custom-apis"></a><a name="CustomAPI"></a>Özel API'ler

### <a name="define-a-custom-api"></a><a name="howto-customapi-basic"></a>Özel bir API tanımlama

Son nokta üzerinden Veri Erişim `/tables` API'sine ek olarak, Mobil Uygulamalar özel API kapsamı sağlayabilir. Özel API'ler tablo tanımlarına benzer şekilde tanımlanır ve kimlik doğrulaması da dahil olmak üzere aynı tüm tesislere erişebilir.

Uygulama Hizmeti Kimlik Doğrulaması'nı özel bir API ile kullanmak istiyorsanız, önce [Azure portalında] Uygulama Hizmeti Kimlik Doğrulaması'nı yapılandırmanız gerekir. Daha fazla bilgi için, kullanmak istediğiniz kimlik sağlayıcısının yapılandırma kılavuzuna bakın:

* [Azure Active Directory kimlik doğrulamasını yapılandırma]
* [Facebook kimlik doğrulamasını yapılandırma]
* [Google kimlik doğrulamasını yapılandırma]
* [Microsoft kimlik doğrulamasını yapılandırma]
* [Twitter kimlik doğrulamasını yapılandırma]

Özel API'ler, Tablolar API'si ile aynı şekilde tanımlanır:

1. Bir `api` dizin oluşturun.
1. `api` Dizinde bir API tanımı JavaScript dosyası oluşturun.
1. Dizini almak için `api` alma yöntemini kullanın.

Daha önce kullandığımız temel uygulama örneğine dayalı prototip API tanımı aşağıda verilmiştir:

```javascript
var express = require('express'),
    azureMobileApps = require('azure-mobile-apps');

var app = express(),
    mobile = azureMobileApps();

// Import the custom API.
mobile.api.import('./api');

// Add the Mobile API so it is accessible as a Web API.
app.use(mobile);

// Start listening on HTTP
app.listen(process.env.PORT || 3000);
```

`Date.now()` Yöntemi kullanarak sunucu tarihini döndüren örnek bir API ele alalım. İşte api/date.js dosyası:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};

module.exports = api;
```

Her parametre standart RESTful fiillerden biridir: GET, POST, PATCH veya DELETE. Yöntem, gerekli çıktıyı gönderen standart bir [ExpressJS ara yazılım] işlevidir.

### <a name="require-authentication-for-access-to-a-custom-api"></a><a name="howto-customapi-auth"></a>Özel bir API'ye erişmek için kimlik doğrulamagerektirir

Mobil Uygulamalar SDK, kimlik doğrulamayı hem `tables` bitiş noktası hem de özel API'ler için aynı şekilde uygular. Önceki bölümde geliştirilen API'ye kimlik doğrulama eklemek `access` için bir özellik ekleyin:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    });
};
// All methods must be authenticated.
api.access = 'authenticated';

module.exports = api;
```

Belirli işlemlerde kimlik doğrulaması da belirtebilirsiniz:

```javascript
var api = {
    get: function (req, res, next) {
        var date = { currentTime: Date.now() };
        res.status(200).type('application/json').send(date);
    }
};
// The GET methods must be authenticated.
api.get.access = 'authenticated';

module.exports = api;
```

`tables` Bitiş noktası için kullanılan aynı belirteç, kimlik doğrulaması gerektiren özel API'ler için kullanılmalıdır.

### <a name="handle-large-file-uploads"></a><a name="howto-customapi-auth"></a>Büyük dosya yüklemelerini işleme

Mobile Apps SDK, gönderiminizdeki vücut içeriğini kabul etmek ve çözmek için [vücut parser ara yazılımını](https://github.com/expressjs/body-parser) kullanır. Daha büyük dosya yüklemelerini kabul etmek için gövde ayrıştırıcısını önceden yapılandırabilirsiniz:

```javascript
var express = require('express'),
    bodyParser = require('body-parser'),
    azureMobileApps = require('azure-mobile-apps');

var app = express(),
    mobile = azureMobileApps();

// Set up large body content handling.
app.use(bodyParser.json({ limit: '50mb' }));
app.use(bodyParser.urlencoded({ limit: '50mb', extended: true }));

// Import the custom API.
mobile.api.import('./api');

// Add the Mobile API so it is accessible as a Web API.
app.use(mobile);

// Start listening on HTTP.
app.listen(process.env.PORT || 3000);
```

Dosya, iletimden önce kodlanmış temel-64. Bu kodlama, gerçek yüklemenin boyutunu (ve hesaba katmanız gereken boyutu) artırır.

### <a name="execute-custom-sql-statements"></a><a name="howto-customapi-sql"></a>Özel SQL deyimlerini yürütme

Mobil Uygulamalar SDK, istek nesnesi aracılığıyla tüm içeriğe erişim sağlar. Parametreli SQL deyimlerini tanımlanan veri sağlayıcısına kolayca gerçekleştirebilirsiniz:

```javascript
var api = {
    get: function (request, response, next) {
        // Check for parameters. If not there, pass on to a later API call.
        if (typeof request.params.completed === 'undefined')
            return next();

        // Define the query. Anything that the mssql
        // driver can handle is allowed.
        var query = {
            sql: 'UPDATE TodoItem SET complete=@completed',
            parameters: [{
                completed: request.params.completed
            }]
        };

        // Execute the query. The context for Mobile Apps is available through
        // request.azureMobile. The data object contains the configured data provider.
        request.azureMobile.data.execute(query)
        .then(function (results) {
            response.json(results);
        });
    }
};

api.get.access = 'authenticated';
module.exports = api;
```

## <a name="debugging"></a><a name="Debugging"></a>Hata Ayıklama

### <a name="debug-diagnose-and-troubleshoot-mobile-apps"></a><a name="howto-diagnostic-logs"></a>Mobil Uygulamaları hata ayıklama, tanılama ve sorun giderme

Azure Uygulama Hizmeti, Node.js uygulamaları için çeşitli hata ayıklama ve sorun giderme teknikleri sağlar.
Node.js Mobile Apps'ınızı sorun gidermeye başlamak için aşağıdaki makalelere bakın:

* [Azure Uygulama Hizmetini İzleme]
* [Azure Uygulama Hizmeti'nde tanısal günlüğe kaydetmeyi etkinleştirme]
* [Visual Studio'da Azure Uygulama Hizmeti sorun giderme]

Node.js uygulamaları çok çeşitli tanıgünlüğü araçlarına erişebilir. Dahili olarak, Mobil Uygulamalar Düğümü.js SDK tanısal günlük için [Winston] kullanır. Hata ayıklama modunu etkinleştirdiğinizde veya uygulama `MS_DebugMode` ayarını Azure [portalında]gerçek olacak şekilde ayarladığınızda günlüğe kaydetme otomatik olarak etkinleştirilir. Oluşturulan günlükler [Azure portalındaki]tanılama günlüklerinde görünür.

<!-- Images -->
[0]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/npm-init.png
[1]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-new-project.png
[2]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/vs2015-install-npm.png
[3]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-config.png
[4]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-authconfig.png
[5]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/sqlexpress-newuser-1.png
[6]: ./media/app-service-mobile-node-backend-how-to-use-server-sdk/dotnet-backend-create-db.png

<!-- URLs -->
[Android istemciquickstart]: app-service-mobile-android-get-started.md
[Apache Cordova İstemci hızlı başlangıç]: app-service-mobile-cordova-get-started.md
[iOS İstemci hızlı başlat]: app-service-mobile-ios-get-started.md
[Xamarin.iOS İstemci hızlı başlat]: app-service-mobile-xamarin-ios-get-started.md
[Xamarin.Android İstemci hızlı başlat]: app-service-mobile-xamarin-android-get-started.md
[Xamarin.Forms İstemci hızlı başlat]: app-service-mobile-xamarin-forms-get-started.md
[Windows Mağazası İstemci hızlı başlat]: app-service-mobile-windows-store-dotnet-get-started.md
[çevrimdışı veri eşitleme]: app-service-mobile-offline-data-sync.md
[Azure Active Directory kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-aad.md
[Facebook kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-facebook.md
[Google kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-google.md
[Microsoft kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-microsoft.md
[Twitter kimlik doğrulamasını yapılandırma]: ../app-service/configure-authentication-provider-twitter.md
[Azure Uygulama Hizmeti dağıtım kılavuzu]: ../app-service/deploy-local-git.md
[Azure Uygulama Hizmetini İzleme]: ../app-service/web-sites-monitor.md
[Azure Uygulama Hizmeti'nde tanısal günlüğe kaydetmeyi etkinleştirme]: ../app-service/troubleshoot-diagnostic-logs.md
[Visual Studio'da Azure Uygulama Hizmeti sorun giderme]: ../app-service/troubleshoot-dotnet-visual-studio.md
[Düğüm sürümünü belirtin]: ../nodejs-specify-node-version-azure-apps.md
[Düğüm modüllerini kullanma]: ../nodejs-use-node-modules-azure-apps.md
[Create a new Azure App Service]: ../app-service/
[azure-mobile-apps]: https://www.npmjs.com/package/azure-mobile-apps
[Express]: https://expressjs.com/
[Swagger]: https://swagger.io/

[Azure portalında]: https://portal.azure.com/
[OData]: https://www.odata.org
[Söz]: https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Promise
[GitHub'da basicapp örneği]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/basic-app
[GitHub üzerinde todo örnek]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/todo
[GitHub'da örnek dizini]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples
[static-schema sample on GitHub]: https://github.com/azure/azure-mobile-apps-node/tree/master/samples/static-schema
[QueryJS]: https://github.com/Azure/queryjs
[Görsel Stüdyo için Düğüm.js Araçları 1.1]: https://github.com/Microsoft/nodejstools/releases/tag/v1.1-RC.2.1
[mssql Düğümü.js paketi]: https://www.npmjs.com/package/mssql
[Microsoft SQL Server 2014 Express]: https://www.microsoft.com/en-us/server-cloud/Products/sql-server-editions/sql-server-express.aspx
[ExpressJS ara yazılım]: https://expressjs.com/guide/using-middleware.html
[Winston]: https://github.com/winstonjs/winston
