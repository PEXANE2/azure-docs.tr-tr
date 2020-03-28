---
title: 'Öğretici: Toplu iş algılama ve Power BI kullanarak anomalileri görselleştirin'
titleSuffix: Azure Cognitive Services
description: Zaman serisi verilerinizdeki anormallikleri görselleştirmek için Anomali Dedektörü API'sını ve Güç BI'yi nasıl kullanacağınızı öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: anomaly-detector
ms.topic: tutorial
ms.date: 03/05/2020
ms.author: aahi
ms.openlocfilehash: 1b486aaf0ce33e31433c2c3d0f7a1ff2c7089132
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "78402654"
---
# <a name="tutorial-visualize-anomalies-using-batch-detection-and-power-bi"></a>Öğretici: Toplu iş algılama ve Power BI kullanarak anomalileri görselleştirin

Bir toplu iş olarak bir zaman serisi veri kümesi içinde anomalileri bulmak için bu öğretici kullanın. Power BI masaüstünü kullanarak bir Excel dosyası alır, Anomali Dedektörü API'sı için verileri hazırlar ve tüm genelinde istatistiksel anormallikleri görselleştirirsiniz.

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:

> [!div class="checklist"]
> * Zaman serisi veri kümesini almak ve dönüştürmek için Power BI Desktop'ı kullanın
> * Toplu anomali tespiti için Power BI Desktop'ı Anomali Dedektörü API ile tümleştirin
> * Verilerinizde bulunan, beklenen ve görülen değerler ve anomali algılama sınırları da dahil olmak üzere bulunan anormallikleri görselleştirin.

## <a name="prerequisites"></a>Ön koşullar
* [Azure aboneliği](https://azure.microsoft.com/free/)
* [Microsoft Power BI Desktop](https://powerbi.microsoft.com/get-started/), ücretsiz olarak kullanılabilir.
* Zaman serisi veri noktaları içeren bir excel dosyası (.xlsx). Bu hızlı başlatma için örnek veriler [GitHub'da](https://go.microsoft.com/fwlink/?linkid=2090962) bulunabilir
* Azure aboneliğinizi aldıktan <a href="https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesTextAnalytics"  title="sonra, Anahtar ve"  target="_blank">bitiş noktanızı <span class="docon docon-navigate-external x-hidden-focus"></span> </a> almak için Azure portalında bir Metin Analizi kaynağı oluşturun. 
    * Uygulamanızı Text Analytics API'sine bağlamak için oluşturduğunuz kaynaktan gelen anahtar ve bitiş noktasına ihtiyacınız olacaktır. Bunu daha sonra hızlı bir şekilde yapacaksınız.

[!INCLUDE [cognitive-services-anomaly-detector-data-requirements](../../../../includes/cognitive-services-anomaly-detector-data-requirements.md)]

## <a name="load-and-format-the-time-series-data"></a>Zaman serisi verilerini yükleme ve biçimlendirme

Başlamak için Power BI Desktop'ı açın ve ön koşullardan indirdiğiniz zaman serisi verilerini yükleyin. Bu excel dosyası, bir dizi Eşgüdümlü Evrensel Zaman (UTC) zaman damgası ve değer çiftleri içerir.  

> [!NOTE]
> Power BI,.csv dosyaları, SQL veritabanları, Azure blob depolama ve daha fazlası gibi çok çeşitli kaynaklardan gelen verileri kullanabilir.  

Ana Power BI Masaüstü penceresinde **Ana Şerit'i** tıklatın. Şeridin **Dış veri** grubunda, Veri **Al** açılır menüsünü açın ve **Excel'i**tıklatın.

![Power BI'de "Veri Al" düğmesinin görüntüsü](../media/tutorials/power-bi-get-data-button.png)

İletişim kutusu görüntülendikten sonra ,xlsx örneğini indirdiğiniz klasöre gidin ve seçin. **Gezgin** diyaloğu göründükten sonra **Sayfa1'i**tıklatın ve sonra **edit.**

![Power BI veri kaynağı "Navigator" ekran bir görüntü](../media/tutorials/navigator-dialog-box.png)

Power BI, ilk sütundaki zaman damgalarını `Date/Time` veri türüne dönüştürür. Bu zaman damgalarının Anomali Dedektörü API'sine gönderilebilmesi için metne dönüştürülmesi gerekir. Güç Sorgusu düzenleyicisi otomatik olarak açılmıyorsa, ev sekmesinde **Sorguları Edit'i** tıklatın. 

Güç Sorgusu Düzenleyicisi'ndeki **Dönüşüm** şeridini tıklatın. Herhangi **bir Sütun** grubunda, **Veri Türünü açın:** açılır menü ve **Metin'i**seçin.

![Power BI veri kaynağı "Navigator" ekran bir görüntü](../media/tutorials/data-type-drop-down.png)

Sütun türünü değiştirme hakkında bir bildirim aldığınızda, **Geçerliyi Değiştir'i**tıklatın. Daha **sonra, Ana şeritte** Uygula veya **Uygula** **& kapat'ı** tıklatın. 

## <a name="create-a-function-to-send-the-data-and-format-the-response"></a>Verileri göndermek ve yanıtı biçimlendirmek için bir işlev oluşturma

Veri dosyasını biçimlendirmek ve Anomali Dedektörü API'sine göndermek için, yukarıda oluşturulan tabloda bir sorgu çağırabilirsiniz. Güç Sorgusu Düzenleyicisi'nde, **Ana şeritten,** **Yeni Kaynak** açılır menüsünü açın ve **Boş Sorgu'yu**tıklatın.

Yeni sorgunuzun seçildiğinden emin olun, ardından **Gelişmiş Düzenleyici'yi**tıklatın. 

![Power BI'deki "Advanced Editor" düğmesinin görüntüsü](../media/tutorials/advanced-editor-screen.png)

Gelişmiş Düzenleyici'de, sütunları tablodan ayıklamak ve API'ye göndermek için aşağıdaki Power Query M snippet'i kullanın. Daha sonra, sorgu JSON yanıtından bir tablo oluşturur ve döndürür. Değişkeni `apiKey` geçerli Anomali Dedektörü API `endpoint` anahtarınızla ve bitiş noktanızla değiştirin. Sorguyu Gelişmiş Düzenleyici'ye girdikten sonra **Bitti'yi**tıklatın.

```M
(table as table) => let

    apikey      = "[Placeholder: Your Anomaly Detector resource access key]",
    endpoint    = "[Placeholder: Your Anomaly Detector resource endpoint]/anomalydetector/v1.0/timeseries/entire/detect",
    inputTable = Table.TransformColumnTypes(table,{{"Timestamp", type text},{"Value", type number}}),
    jsontext    = Text.FromBinary(Json.FromValue(inputTable)),
    jsonbody    = "{ ""Granularity"": ""daily"", ""Sensitivity"": 95, ""Series"": "& jsontext &" }",
    bytesbody   = Text.ToBinary(jsonbody),
    headers     = [#"Content-Type" = "application/json", #"Ocp-Apim-Subscription-Key" = apikey],
    bytesresp   = Web.Contents(endpoint, [Headers=headers, Content=bytesbody]),
    jsonresp    = Json.Document(bytesresp),

    respTable = Table.FromColumns({
                    
                     Table.Column(inputTable, "Timestamp")
                     ,Table.Column(inputTable, "Value")
                     , Record.Field(jsonresp, "IsAnomaly") as list
                     , Record.Field(jsonresp, "ExpectedValues") as list
                     , Record.Field(jsonresp, "UpperMargins")as list
                     , Record.Field(jsonresp, "LowerMargins") as list
                     , Record.Field(jsonresp, "IsPositiveAnomaly") as list
                     , Record.Field(jsonresp, "IsNegativeAnomaly") as list

                  }, {"Timestamp", "Value", "IsAnomaly", "ExpectedValues", "UpperMargin", "LowerMargin", "IsPositiveAnomaly", "IsNegativeAnomaly"}
               ),
    
    respTable1 = Table.AddColumn(respTable , "UpperMargins", (row) => row[ExpectedValues] + row[UpperMargin]),
    respTable2 = Table.AddColumn(respTable1 , "LowerMargins", (row) => row[ExpectedValues] -  row[LowerMargin]),
    respTable3 = Table.RemoveColumns(respTable2, "UpperMargin"),
    respTable4 = Table.RemoveColumns(respTable3, "LowerMargin"),

    results = Table.TransformColumnTypes(

                respTable4,
                {{"Timestamp", type datetime}, {"Value", type number}, {"IsAnomaly", type logical}, {"IsPositiveAnomaly", type logical}, {"IsNegativeAnomaly", type logical},
                 {"ExpectedValues", type number}, {"UpperMargins", type number}, {"LowerMargins", type number}}
              )

 in results
```

`Sheet1` **Parametre yi girin**ve Çağır'ı tıklatın aşağıdan seçerek veri sayfanızdaki sorguyu **çağırın.** 

!["Gelişmiş Editör" düğmesinin görüntüsü](../media/tutorials/invoke-function-screenshot.png)

## <a name="data-source-privacy-and-authentication"></a>Veri kaynağı gizliliği ve kimlik doğrulama

> [!NOTE]
> Kuruluşunuzun veri gizliliği ve erişim politikalarına dikkat edin. Daha fazla bilgi için [Power BI Masaüstü gizlilik düzeylerine](https://docs.microsoft.com/power-bi/desktop-privacy-levels) bakın.

Dış veri kaynağı kullandığından sorguyu çalıştırmayı denediğinizde bir uyarı iletisi alabilirsiniz. 

![Power BI tarafından oluşturulan bir uyarıyı gösteren bir görüntü](../media/tutorials/blocked-function.png)

Bunu düzeltmek için **Dosya**ve **Seçenekler ve ayarlar'ı**tıklatın. Ardından **Seçenekler'i**tıklatın. **Geçerli Dosyanın**Altında **Gizlilik'i**seçin ve Gizlilik **Düzeylerini Yoksayın ve performansı artırabilirsiniz.** 

Ayrıca, API'ye nasıl bağlanmak istediğinizi belirtmenizi isteyen bir ileti alabilirsiniz.

![Erişim kimlik bilgilerini belirtmek için isteği gösteren bir resim](../media/tutorials/edit-credentials-message.png)

Bunu düzeltmek için iletideki **Kimlik Bilgilerini Edit'i** tıklatın. Diyalog kutusu göründükten sonra, API'ye anonim olarak bağlanmak için **Anonim'i** seçin. Ardından **Bağlan**’a tıklayın. 

Daha sonra, değişiklikleri uygulamak için **Giriş** şeridinde **kapat & Uygula'yı** tıklatın.

## <a name="visualize-the-anomaly-detector-api-response"></a>Anomali Dedektörü API yanıtını görselleştirin

Ana Power BI ekranında, verileri görselleştirmek için yukarıda oluşturulan sorguları kullanmaya başlayın. **Görselleştirmelerde**Ilk **seçilecek Çizgi Grafiği.** Ardından çağrılan işlevden zaman damgasını satır grafiğinin **Eksenine**ekleyin. Üzerine sağ tıklayın ve **Timestamp'ı**seçin. 

![Zaman damgası değerini sağ tıklatma](../media/tutorials/timestamp-right-click.png)

**Çağrılan İşlev'den** grafiğin **Değerler** alanına aşağıdaki alanları ekleyin. Grafiğinizi oluşturmaya yardımcı olmak için aşağıdaki ekran görüntüsünü kullanın.

    * Değer
    * Üst Marjlar
    * Alt Marjlar
    * Beklenen Değerler

![Yeni hızlı ölçüm ekranının görüntüsü](../media/tutorials/chart-settings.png)

Alanları ekledikten sonra, grafiğe tıklayın ve tüm veri noktalarını göstermek için yeniden boyutlandırın. Grafiğiniz aşağıdaki ekran görüntüsüne benzer olacaktır:

![Yeni hızlı ölçüm ekranının görüntüsü](../media/tutorials/chart-visualization.png)

### <a name="display-anomaly-data-points"></a>Anomali veri noktalarını görüntüleme

Power BI penceresinin sağ tarafında, **FIELDS** bölmesinin altında, **Çağrılan İşlev sorgusunun**altındaki **Değer'e** sağ tıklayın ve **Yeni hızlı ölçü'ye**tıklayın.

![Yeni hızlı ölçüm ekranının görüntüsü](../media/tutorials/new-quick-measure.png)

Görünen ekranda, hesaplama olarak **Filtrelenmiş değeri** seçin. **Temel değeri** `Sum of Value`' ye ayarlama Ardından `IsAnomaly` Çağrılan **İşlev** alanlarından **Filtre'ye**sürükleyin. Filtre `True` açılır **Filter** menüsünden seçin.

![Yeni hızlı ölçüm ekranının görüntüsü](../media/tutorials/new-quick-measure-2.png)

**Tamam'ı**tıklattıktan sonra, alanlarınızın listesinin en altında bir `Value for True` alan olacaktır. Sağ tıklayın ve **Anomaly**olarak yeniden adlandırmak . Grafiğin **Değerleri'ne**ekleyin. Ardından **Biçim** aracını seçin ve X ekseni türünü **Kategorik**olarak ayarlayın.

![Yeni hızlı ölçüm ekranının görüntüsü](../media/tutorials/format-x-axis.png)

**Biçimlendirme** aracına ve **Veri renklerine**tıklayarak grafiğinize renk uygulayın. Grafiğiniz aşağıdaki gibi görünmelidir:

![Yeni hızlı ölçüm ekranının görüntüsü](../media/tutorials/final-chart.png)

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
>[Azure Databricks ile anormallik algılama akışı](anomaly-detection-streaming-databricks.md)
