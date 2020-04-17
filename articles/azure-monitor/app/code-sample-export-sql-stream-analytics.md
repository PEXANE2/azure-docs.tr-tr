---
title: Azure Uygulama Öngörülerinden SQL'e Dışa Aktarma | Microsoft Dokümanlar
description: Stream Analytics'i kullanarak Application Insights verilerini SÜREKLI olarak SQL'e aktarın.
ms.topic: conceptual
ms.date: 09/11/2017
ms.openlocfilehash: e67365038b9a481bc0cacf079e5d197cc3139a5f
ms.sourcegitcommit: 31ef5e4d21aa889756fa72b857ca173db727f2c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81536922"
---
# <a name="walkthrough-export-to-sql-from-application-insights-using-stream-analytics"></a>Walkthrough: Stream Analytics'i kullanarak Application Insights'tan SQL'e dışa aktarma
Bu makalede, [Sürekli Dışa Aktarma][export] ve [Azure Akış Analizi'ni](https://azure.microsoft.com/services/stream-analytics/)kullanarak telemetri verilerinizi Azure [Application Insights'tan Azure Application Insights'a][start] nasıl taşıyış gösterebilirsiniz. 

Sürekli dışa aktarma, telemetri verilerinizi JSON formatında Azure Depolama'ya taşır. Azure Akış Analizi'ni kullanarak JSON nesnelerini ayrıştırAcağız ve veritabanı tablosunda satırlar oluşturacağız.

(Daha genel olarak, Sürekli Dışa Aktarma, uygulamalarınızın Uygulama Öngörüleri'ne gönderdiği telemetrinin kendi analizini yapmanın yoludur. Bu kod örneğini, veri toplama gibi dışa aktarılan telemetriyle başka şeyler yapmak üzere uyarlanabilirsiniz.)

İzlemek istediğiniz uygulamaya sahip olduğunuz varsayımıyla başlayacağız.

Bu örnekte, sayfa görünümü verilerini kullanıyor olacağız, ancak aynı desen özel olaylar ve özel durumlar gibi diğer veri türlerine kolayca genişletilebilir. 

## <a name="add-application-insights-to-your-application"></a>Uygulamanıza Uygulama Öngörüleri Ekleyin
Başlamak için:

1. [Web sayfalarınız için Uygulama Öngörüleri ayarlayın.](../../azure-monitor/app/javascript.md) 
   
    (Bu örnekte, istemci tarayıcılardan sayfa görünümü verilerini işlemeye odaklanacağız, ancak [Java](../../azure-monitor/app/java-get-started.md) veya [ASP.NET](../../azure-monitor/app/asp-net.md) uygulama ve işlem isteği, bağımlılık ve diğer sunucu telemetrisinin sunucu tarafı için Uygulama Öngörüleri de ayarlayabilirsiniz.)
2. Uygulamanızı yayınlayın ve Application Insights kaynağınızda görünen telemetri verilerini izleyin.

## <a name="create-storage-in-azure"></a>Azure'da depolama alanı oluşturma
Sürekli dışa aktarma, verileri her zaman bir Azure Depolama hesabına aktarabilir, bu nedenle önce depolama alanını oluşturmanız gerekir.

1. [Azure portalında][portal]aboneliğinizde bir depolama hesabı oluşturun.
   
    ![Azure portalında Yeni, Veri, Depolama'yı seçin. Klasik'i seçin, Oluştur'u seçin. Depolama adı sağlayın.](./media/code-sample-export-sql-stream-analytics/040-store.png)
2. Bir kapsayıcı oluşturma
   
    ![Yeni depolama alanında Kapsayıcılar'ı seçin, Kapsayıcılar döşemesini tıklatın ve sonra Ekle](./media/code-sample-export-sql-stream-analytics/050-container.png)
3. Depolama erişim anahtarını kopyalama
   
    Akış analizi hizmetine girişi ayarlamak için yakında ihtiyacınız olacak.
   
    ![Depolama alanında Ayarlar,Anahtarlar'ı açın ve Birincil Erişim Anahtarının bir kopyasını alın](./media/code-sample-export-sql-stream-analytics/21-storage-key.png)

## <a name="start-continuous-export-to-azure-storage"></a>Azure depolamasına sürekli dışa aktarma başlatma
1. Azure portalında, uygulamanız için oluşturduğunuz Uygulama Öngörüleri kaynağına göz atın.
   
    ![Browse, Application Insights ve uygulamanızı seçin](./media/code-sample-export-sql-stream-analytics/060-browse.png)
2. Sürekli bir dışa aktarım oluşturun.
   
    ![Ayarları Seçin, Sürekli Dışa Aktarma, Ekle](./media/code-sample-export-sql-stream-analytics/070-export.png)

    Daha önce oluşturduğunuz depolama hesabını seçin:

    ![Dışa aktarma hedefini ayarlama](./media/code-sample-export-sql-stream-analytics/080-add.png)

    Görmek istediğiniz olay türlerini ayarlayın:

    ![Etkinlik türlerini seçin](./media/code-sample-export-sql-stream-analytics/085-types.png)


1. Bazı verilerin birikmesine izin verin. Arkanıza yindirin ve insanlar uygulamanızı bir süre kullansın. Telemetri gelecek ve [tanıaramada](../../azure-monitor/app/diagnostic-search.md)metrik [kaşifve](../../azure-monitor/platform/metrics-charts.md) tek tek olaylardaki istatistiksel grafikleri göreceksiniz. 
   
    Ayrıca, veriler depolama alanınıza dışa aktaracaktır. 
2. Dışa aktarılan verileri portalda inceleyin - **Gözat'ı**seçin, depolama hesabınızı seçin ve ardından **Kapsayıcılar** - veya Visual Studio'da. Visual Studio'da **Görüntü / Bulut Gezgini'ni**seçin ve Azure / Depolama'yı açın. (Bu menü seçeneğiniz yoksa, Azure SDK'yı yüklemeniz gerekir: Yeni Proje iletişim kutusunu açın ve Visual C# / Cloud / Microsoft Azure SDK'yı .NET için alın.)
   
    ![Visual Studio'da, Sunucu Tarayıcısı, Azure, Depolama](./media/code-sample-export-sql-stream-analytics/087-explorer.png)
   
    Uygulama adı ve enstrümantasyon anahtarından türetilen yol adının ortak bölümüne dikkat edin. 

Olaylar JSON formatında blob dosyalarına yazılır. Her dosya bir veya daha fazla olay içerebilir. Bu yüzden etkinlik verilerini okumak ve istediğimiz alanları filtrelemek istiyoruz. Verilerle yapabileceğimiz her türlü şey var, ancak bugünkü planımız verileri bir SQL veritabanına taşımak için Stream Analytics'i kullanmak. Bu, birçok ilginç sorgunun çalıştırılmasını kolaylaştırır.

## <a name="create-an-azure-sql-database"></a>Azure SQL Veritabanı oluşturma
[Azure portalındaki][portal]aboneliğinizden başlayarak, verileri yazacağınız veritabanını (ve yeni bir sunucunuz yoksa) oluşturun.

![Yeni, Veri, SQL](./media/code-sample-export-sql-stream-analytics/090-sql.png)

Veritabanı sunucusunun Azure hizmetlerine erişime izin verdiğinden emin olun:

![Göz atın, Sunucular, sunucunuz, Ayarlar, Güvenlik Duvarı, Azure'a Erişime İzin Ver](./media/code-sample-export-sql-stream-analytics/100-sqlaccess.png)

## <a name="create-a-table-in-azure-sql-db"></a>Azure SQL DB'de tablo oluşturma
Tercih ettiğiniz yönetim aracıyla önceki bölümde oluşturulan veritabanına bağlanın. Bu izlenecek yolda, [SQL Server Management Tools](https://msdn.microsoft.com/ms174173.aspx) (SSMS) kullanıyor olacağız.

![](./media/code-sample-export-sql-stream-analytics/31-sql-table.png)

Yeni bir sorgu oluşturun ve aşağıdaki T-SQL'i çalıştırın:

```SQL

CREATE TABLE [dbo].[PageViewsTable](
    [pageName] [nvarchar](max) NOT NULL,
    [viewCount] [int] NOT NULL,
    [url] [nvarchar](max) NULL,
    [urlDataPort] [int] NULL,
    [urlDataprotocol] [nvarchar](50) NULL,
    [urlDataHost] [nvarchar](50) NULL,
    [urlDataBase] [nvarchar](50) NULL,
    [urlDataHashTag] [nvarchar](max) NULL,
    [eventTime] [datetime] NOT NULL,
    [isSynthetic] [nvarchar](50) NULL,
    [deviceId] [nvarchar](50) NULL,
    [deviceType] [nvarchar](50) NULL,
    [os] [nvarchar](50) NULL,
    [osVersion] [nvarchar](50) NULL,
    [locale] [nvarchar](50) NULL,
    [userAgent] [nvarchar](max) NULL,
    [browser] [nvarchar](50) NULL,
    [browserVersion] [nvarchar](50) NULL,
    [screenResolution] [nvarchar](50) NULL,
    [sessionId] [nvarchar](max) NULL,
    [sessionIsFirst] [nvarchar](50) NULL,
    [clientIp] [nvarchar](50) NULL,
    [continent] [nvarchar](50) NULL,
    [country] [nvarchar](50) NULL,
    [province] [nvarchar](50) NULL,
    [city] [nvarchar](50) NULL
)

CREATE CLUSTERED INDEX [pvTblIdx] ON [dbo].[PageViewsTable]
(
    [eventTime] ASC
)WITH (PAD_INDEX = OFF, STATISTICS_NORECOMPUTE = OFF, SORT_IN_TEMPDB = OFF, DROP_EXISTING = OFF, ONLINE = OFF, ALLOW_ROW_LOCKS = ON, ALLOW_PAGE_LOCKS = ON)

```

![](./media/code-sample-export-sql-stream-analytics/34-create-table.png)

Bu örnekte, sayfa görünümlerinden gelen verileri kullanıyoruz. Kullanılabilir diğer verileri görmek için JSON çıktınızı inceleyin ve [dışa aktarma veri modeline](../../azure-monitor/app/export-data-model.md)bakın.

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Akış Analizi örneği oluşturma
Azure [portalından](https://portal.azure.com/)Azure Akış Analizi hizmetini seçin ve yeni bir Akış Analizi işi oluşturun:

![Akış analizi ayarları](./media/code-sample-export-sql-stream-analytics/SA001.png)

![](./media/code-sample-export-sql-stream-analytics/SA002.png)

Yeni iş oluşturulduğunda **kaynağa git'i**seçin.

![Akış analizi ayarları](./media/code-sample-export-sql-stream-analytics/SA003.png)

#### <a name="add-a-new-input"></a>Yeni giriş ekleme

![Akış analizi ayarları](./media/code-sample-export-sql-stream-analytics/SA004.png)

Sürekli Dışa Aktarma blob'unuzdan giriş alacak şekilde ayarlayın:

![Akış analizi ayarları](./media/code-sample-export-sql-stream-analytics/SA0005.png)

Artık, daha önce belirttiğiniz Depolama Hesabınızdaki Birincil Erişim Anahtarına ihtiyacınız olacak. Bunu Depolama Hesabı Anahtarı olarak ayarlayın.

#### <a name="set-path-prefix-pattern"></a>Yol öneki deseni ayarlama

**Tarih Biçimini YYYY-MM-DD olarak (tireli) ayarladıktan emin olun.**

Yol Öneki Deseni, Akış Analizi'nin giriş dosyalarını depolama alanında nasıl bulduğunu belirtir. Sürekli Dışa Aktarma'nın verileri nasıl depoladığıyla aynı şekilde ayarlamanız gerekir. Şu şekilde ayarlayın:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Bu örnekte:

* `webapplication27`Uygulama Öngörüleri kaynağının adıdır, **tüm küçük harf .** 
* `1234...`**kaldırılan tirelerle**Uygulama Öngörüleri kaynağının enstrümantasyon anahtarıdır. 
* `PageViews`analiz etmek istediğimiz veri türüdür. Kullanılabilir türler, Sürekli Dışa Aktarma'da ayarladığınız filtreye bağlıdır. Kullanılabilir diğer türleri görmek için dışa aktarılan verileri inceleyin ve [dışa aktarma veri modeline](../../azure-monitor/app/export-data-model.md)bakın.
* `/{date}/{time}`kelimenin tam anlamıyla yazılmış bir desendir.

Application Insights kaynağınızın adını ve iKey'ini almak için, Temel Bilgileri genel bakış sayfasında açın veya Ayarlar'ı açın.

> [!TIP]
> Giriş yolunu doğru şekilde ayarladığınızdan yararlanmak için Örnek işlevini kullanın. Başarısız olursa: Seçtiğiniz örnek zaman aralığı için depolama alanında veri olup olmadığını denetleyin. Giriş tanımını edin ve depolama hesabını, yol önekini ve tarih biçimini doğru şekilde ayarladığınızdan denetleyin.

 
## <a name="set-query"></a>Sorguayarlama
Sorgu bölümünü açın:

Varsayılan sorguyu aşağıdakilerle değiştirin:

```SQL

    SELECT flat.ArrayValue.name as pageName
    , flat.ArrayValue.count as viewCount
    , flat.ArrayValue.url as url
    , flat.ArrayValue.urlData.port as urlDataPort
    , flat.ArrayValue.urlData.protocol as urlDataprotocol
    , flat.ArrayValue.urlData.host as urlDataHost
    , flat.ArrayValue.urlData.base as urlDataBase
    , flat.ArrayValue.urlData.hashTag as urlDataHashTag
      ,A.context.data.eventTime as eventTime
      ,A.context.data.isSynthetic as isSynthetic
      ,A.context.device.id as deviceId
      ,A.context.device.type as deviceType
      ,A.context.device.os as os
      ,A.context.device.osVersion as osVersion
      ,A.context.device.locale as locale
      ,A.context.device.userAgent as userAgent
      ,A.context.device.browser as browser
      ,A.context.device.browserVersion as browserVersion
      ,A.context.device.screenResolution.value as screenResolution
      ,A.context.session.id as sessionId
      ,A.context.session.isFirst as sessionIsFirst
      ,A.context.location.clientip as clientIp
      ,A.context.location.continent as continent
      ,A.context.location.country as country
      ,A.context.location.province as province
      ,A.context.location.city as city
    INTO
      AIOutput
    FROM AIinput A
    CROSS APPLY GetElements(A.[view]) as flat


```

İlk birkaç özelliğin sayfa görüntüleme verilerine özgü olduğuna dikkat edin. Diğer telemetri türlerinin dışa aktarma özellikleri farklı olacaktır. Özellik [türleri ve değerleri için ayrıntılı veri modeli başvurusuna bakın.](../../azure-monitor/app/export-data-model.md)

## <a name="set-up-output-to-database"></a>Çıktıyı veritabanına ayarlama
Çıktı olarak SQL'i seçin.

![Akış analizinde, Çıktılar'ı seçin](./media/code-sample-export-sql-stream-analytics/SA006.png)

SQL veritabanını belirtin.

![Veritabanınızın ayrıntılarını doldurun](./media/code-sample-export-sql-stream-analytics/SA007.png)

Sihirbazı kapatın ve çıktının ayarlandığına dair bir bildirim bekleyin.

## <a name="start-processing"></a>İşleme yi başlat
İşi eylem çubuğundan başlatın:

![Akış analizinde Başlat'ı tıklatın](./media/code-sample-export-sql-stream-analytics/SA008.png)

Şu andan itibaren verileri işlemeye başlayıp başlamayı, yoksa önceki verilerle mi başlayacağınızı seçebilirsiniz. Zaten bir süre çalışan Sürekli İhracat olduysanız ikincisi yararlıdır.

Birkaç dakika sonra SQL Server Management Tools'a geri dön ve akan verileri izleyin. Örneğin, şu gibi bir sorgu kullanın:

    SELECT TOP 100 *
    FROM [dbo].[PageViewsTable]


## <a name="related-articles"></a>İlgili makaleler:
* [Stream Analytics kullanarak Power BI'ye dışa aktarma](../../azure-monitor/app/export-power-bi.md )
* [Özellik türleri ve değerleri için ayrıntılı veri modeli başvurusu.](../../azure-monitor/app/export-data-model.md)
* [Uygulama Öngörülerinde Sürekli İhracat](../../azure-monitor/app/export-telemetry.md)
* [Uygulama Bilgileri](https://azure.microsoft.com/services/application-insights/)

<!--Link references-->

[diagnostic]: ../../azure-monitor/app/diagnostic-search.md
[export]: ../../azure-monitor/app/export-telemetry.md
[metrics]: ../../azure-monitor/platform/metrics-charts.md
[portal]: https://portal.azure.com/
[start]: ../../azure-monitor/app/app-insights-overview.md

