---
title: Azure Uygulama Öngörülerinden Akış Analizini Kullanarak Dışa Aktarma | Microsoft Dokümanlar
description: Akış Analizi, dışa aktardığınız verileri Application Insights'tan sürekli olarak dönüştürebilir, filtreleyebilir ve yönlendirebilir.
ms.topic: conceptual
ms.date: 01/08/2019
ms.openlocfilehash: b93bc49d005e3e54f1e5db84e6ff1adc49e25a65
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77664023"
---
# <a name="use-stream-analytics-to-process-exported-data-from-application-insights"></a>Application Insights'tan dışa aktarılan verileri işlemek için Akış Analizi'ni kullanın
[Azure Akış Analizi,](https://azure.microsoft.com/services/stream-analytics/) [Application Insights'tan dışa aktarılan](export-telemetry.md)verileri işlemek için ideal bir araçtır. Akış Analizi çeşitli kaynaklardan veri çekebilir. Verileri dönüştürüp filtreleyebilir ve ardından çeşitli lavabolara yönlendirebilir.

Bu örnekte, Uygulama Öngörüleri'nden veri alan, bazı alanların yeniden adlarını ve işlemlerini yapan ve power BI'ye aktaran bir bağdaştırıcı oluşturacağız.

> [!WARNING]
> [Power BI'de Application Insights verilerini görüntülemenin](../../azure-monitor/app/export-power-bi.md )çok daha iyi ve daha kolay önerilen yolları vardır. Burada gösterilen yol, dışa aktarılan verilerin nasıl işlenirgibi gösterildiğini gösteren yalnızca bir örnektir.
> 
> 

![SA üzerinden PBI'ye dışa aktarma için blok diyagramı](./media/export-stream-analytics/020.png)

## <a name="create-storage-in-azure"></a>Azure'da depolama alanı oluşturma
Sürekli dışa aktarma, verileri her zaman bir Azure Depolama hesabına aktarabilir, bu nedenle önce depolama alanını oluşturmanız gerekir.

1. [Azure portalında](https://portal.azure.com)aboneliğinizde "klasik" bir depolama hesabı oluşturun.
   
   ![Azure portalında Yeni, Veri, Depolama'yı seçin](./media/export-stream-analytics/030.png)
2. Bir kapsayıcı oluşturma
   
    ![Yeni depolama alanında Kapsayıcılar'ı seçin, Kapsayıcılar döşemesini tıklatın ve sonra Ekle](./media/export-stream-analytics/040.png)
3. Depolama erişim anahtarını kopyalama
   
    Akış analizi hizmetine girişi ayarlamak için yakında ihtiyacınız olacak.
   
    ![Depolama alanında Ayarlar,Anahtarlar'ı açın ve Birincil Erişim Anahtarının bir kopyasını alın](./media/export-stream-analytics/045.png)

## <a name="start-continuous-export-to-azure-storage"></a>Azure depolamasına sürekli dışa aktarma başlatma
[Sürekli dışa aktarma,](export-telemetry.md) Uygulamaları Öngörüler'den Azure depolamaalanına aktarAn verileri taşır.

1. Azure portalında, uygulamanız için oluşturduğunuz Uygulama Öngörüleri kaynağına göz atın.
   
    ![Browse, Application Insights ve uygulamanızı seçin](./media/export-stream-analytics/050.png)
2. Sürekli bir dışa aktarım oluşturun.
   
    ![Ayarları Seçin, Sürekli Dışa Aktarma, Ekle](./media/export-stream-analytics/060.png)

    Daha önce oluşturduğunuz depolama hesabını seçin:

    ![Dışa aktarma hedefini ayarlama](./media/export-stream-analytics/070.png)

    Görmek istediğiniz olay türlerini ayarlayın:

    ![Etkinlik türlerini seçin](./media/export-stream-analytics/080.png)

1. Bazı verilerin birikmesine izin verin. Arkanıza yindirin ve insanlar uygulamanızı bir süre kullansın. Telemetri gelecek ve [tanıaramada](../../azure-monitor/app/diagnostic-search.md)metrik [kaşifve](../../azure-monitor/app/metrics-explorer.md) tek tek olaylardaki istatistiksel grafikleri göreceksiniz. 
   
    Ayrıca, veriler depolama alanınıza dışa aktaracaktır. 
2. Dışa aktarılan verileri inceleyin. Visual Studio'da **Görüntü / Bulut Gezgini'ni**seçin ve Azure / Depolama'yı açın. (Bu menü seçeneğiniz yoksa, Azure SDK'yı yüklemeniz gerekir: Yeni Proje iletişim kutusunu açın ve Visual C# / Cloud / Microsoft Azure SDK'yı .NET için alın.)
   
    ![](./media/export-stream-analytics/04-data.png)
   
    Uygulama adı ve enstrümantasyon anahtarından türetilen yol adının ortak bölümüne dikkat edin. 

Olaylar JSON formatında blob dosyalarına yazılır. Her dosya bir veya daha fazla olay içerebilir. Bu yüzden etkinlik verilerini okumak ve istediğimiz alanları filtrelemek istiyoruz. Verilerle yapabileceğimiz her türlü şey vardır, ancak bugünkü planımız verileri Power BI'ye aktarmak için Stream Analytics'i kullanmaktır.

## <a name="create-an-azure-stream-analytics-instance"></a>Azure Akış Analizi örneği oluşturma
Azure [portalından](https://portal.azure.com/)Azure Akış Analizi hizmetini seçin ve yeni bir Akış Analizi işi oluşturun:

![](./media/export-stream-analytics/SA001.png)

![](./media/export-stream-analytics/SA002.png)

Yeni iş oluşturulduğunda **kaynağa git'i**seçin.

![](./media/export-stream-analytics/SA003.png)

### <a name="add-a-new-input"></a>Yeni giriş ekleme

![](./media/export-stream-analytics/SA004.png)

Sürekli Dışa Aktarma blob'unuzdan giriş alacak şekilde ayarlayın:

![](./media/export-stream-analytics/SA0005.png)

Artık, daha önce belirttiğiniz Depolama Hesabınızdaki Birincil Erişim Anahtarına ihtiyacınız olacak. Bunu Depolama Hesabı Anahtarı olarak ayarlayın.

### <a name="set-path-prefix-pattern"></a>Yol öneki deseni ayarlama

**Tarih Biçimini YYYY-MM-DD olarak (tireli) ayarladıktan emin olun.**

Yol Önek Deseni, Akış Analizi'nin giriş dosyalarını depolama alanında nerede bulduğunu belirtir. Sürekli Dışa Aktarma'nın verileri nasıl depoladığıyla aynı şekilde ayarlamanız gerekir. Şu şekilde ayarlayın:

    webapplication27_12345678123412341234123456789abcdef0/PageViews/{date}/{time}

Bu örnekte:

* `webapplication27`Uygulama Öngörüleri kaynağının adıdır **tüm küçük harf.**
* `1234...`tireatarak, Uygulama Öngörüleri kaynağının enstrümantasyon **anahtarıdır.** 
* `PageViews`çözümlemek istediğiniz veri türüdür. Kullanılabilir türler, Sürekli Dışa Aktarma'da ayarladığınız filtreye bağlıdır. Kullanılabilir diğer türleri görmek için dışa aktarılan verileri inceleyin ve [dışa aktarma veri modeline](export-data-model.md)bakın.
* `/{date}/{time}`kelimenin tam anlamıyla yazılmış bir desendir.

> [!NOTE]
> Yolu doğru yaptığınızdan emin olmak için depoyu inceleyin.
> 

## <a name="add-new-output"></a>Yeni çıktı ekleme
Şimdi iş seçin > **Çıktıları** > **Ekle**.

![](./media/export-stream-analytics/SA006.png)


![Yeni kanalı seçin, Çıktılar'ı tıklatın, Ekle, Güç BI](./media/export-stream-analytics/SA010.png)

Power BI kaynağınıza erişmek için Stream Analytics'e yetki vermesi için iş veya **okul hesabınızı** sağlayın. Sonra çıkış için bir ad icat ve hedef Güç BI veri kümesi ve tablo için.

## <a name="set-the-query"></a>Sorguyu ayarlama
Sorgu, girişten çıktıya çeviriyi yönetir.

Doğru çıktıyı alıp almamanızı denetlemek için Test işlevini kullanın. Giriş sayfasından aldığınız örnek verileri verin. 

### <a name="query-to-display-counts-of-events"></a>Olay sayılarını görüntülemek için sorgula
Bu sorguyı yapıştır:

```SQL

    SELECT
      flat.ArrayValue.name,
      count(*)
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.[event]) as flat
    GROUP BY TumblingWindow(minute, 1), flat.ArrayValue.name
```

* ihracat-giriş biz akış girişi verdi diğer adıdır
* pbi-output tanımladığımız çıktı diğer adıdır
* Olay adı iç içe json dizisinde olduğu için [DıŞ APPLY GetElements'ı](https://docs.microsoft.com/stream-analytics-query/apply-azure-stream-analytics) kullanıyoruz. Daha sonra Select, zaman diliminde bu ada sahip örnek sayısıyla birlikte olay adını seçer. [Grup By](https://docs.microsoft.com/stream-analytics-query/group-by-azure-stream-analytics) maddesi öğeleri bir dakikalık zaman dilimlerine gruplanır.

### <a name="query-to-display-metric-values"></a>Metrik değerleri görüntülemek için sorgu
```SQL

    SELECT
      A.context.data.eventtime,
      avg(CASE WHEN flat.arrayvalue.myMetric.value IS NULL THEN 0 ELSE  flat.arrayvalue.myMetric.value END) as myValue
    INTO
      [pbi-output]
    FROM
      [export-input] A
    OUTER APPLY GetElements(A.context.custom.metrics) as flat
    GROUP BY TumblingWindow(minute, 1), A.context.data.eventtime

``` 

* Bu sorgu, olay zamanını ve metrik değerini almak için telemetri metriklerini demler. Metrik değerler bir dizi içinde, bu nedenle satırları ayıklamak için OUTER APPLY GetElements modelini kullanırız. "myMetric" bu durumda metrik adıdır. 

### <a name="query-to-include-values-of-dimension-properties"></a>Boyut özelliklerinin değerlerini içerecek şekilde sorgula
```SQL

    WITH flat AS (
    SELECT
      MySource.context.data.eventTime as eventTime,
      InstanceId = MyDimension.ArrayValue.InstanceId.value,
      BusinessUnitId = MyDimension.ArrayValue.BusinessUnitId.value
    FROM MySource
    OUTER APPLY GetArrayElements(MySource.context.custom.dimensions) MyDimension
    )
    SELECT
     eventTime,
     InstanceId,
     BusinessUnitId
    INTO AIOutput
    FROM flat

```

* Bu sorgu, boyut dizisinde sabit bir dizinde olan belirli bir boyuta bağlı olmadan boyut özelliklerinin değerlerini içerir.

## <a name="run-the-job"></a>İşi çalıştırma
İşi başlatmak için geçmişte bir tarih seçebilirsiniz. 

![İşi seçin ve Sorgula'yı tıklatın. Aşağıdaki örneği yapıştırın.](./media/export-stream-analytics/SA008.png)

İş bitene kadar bekle.

## <a name="see-results-in-power-bi"></a>Power BI'deki sonuçlara bakın
> [!WARNING]
> [Power BI'de Application Insights verilerini görüntülemenin](../../azure-monitor/app/export-power-bi.md )çok daha iyi ve daha kolay önerilen yolları vardır. Burada gösterilen yol, dışa aktarılan verilerin nasıl işlenirgibi gösterildiğini gösteren yalnızca bir örnektir.
> 
> 

İş veya okul hesabınızla Power BI'yi açın ve Stream Analytics işinin çıktısı olarak tanımladığınız veri kümesini ve tabloyu seçin.

![Power BI'de veri kümenizi ve alanlarınızı seçin.](./media/export-stream-analytics/200.png)

Şimdi bu veri kümesini [Power BI'deki](https://powerbi.microsoft.com)raporlarda ve panolarda kullanabilirsiniz.

![Power BI'de veri kümenizi ve alanlarınızı seçin.](./media/export-stream-analytics/210.png)

## <a name="no-data"></a>Veri yok mu?
* Tarih biçimini YYYY-MM-DD (tireli) olarak doğru [şekilde ayarladığınızdan](#set-path-prefix-pattern) denetlemeyin.

## <a name="video"></a>Video
Noam Ben Zeev, Stream Analytics'i kullanarak dışa aktarılan verilerin nasıl işlenirolduğunu gösterir.

> [!VIDEO https://channel9.msdn.com/Blogs/Azure/Export-to-Power-BI-from-Application-Insights/player]
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* [Sürekli dışarı aktarma](export-telemetry.md)
* [Özellik türleri ve değerleri için ayrıntılı veri modeli başvurusu.](export-data-model.md)
* [Uygulama Bilgileri](../../azure-monitor/app/app-insights-overview.md)

