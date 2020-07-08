---
title: Azure Cosmos DB Bağlayıcısı için Power BI öğreticisi
description: JSON 'u içeri aktarmak, öngörülü raporlar oluşturmak ve Azure Cosmos DB ve Power BI bağlayıcısını kullanarak verileri görselleştirmek için bu Power BI öğreticisini kullanın.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: how-to
ms.date: 05/21/2019
ms.author: sngun
ms.openlocfilehash: 3dcadd77866a6c57542a43657a1942791cc4d179
ms.sourcegitcommit: 0100d26b1cac3e55016724c30d59408ee052a9ab
ms.contentlocale: tr-TR
ms.lasthandoff: 07/07/2020
ms.locfileid: "86027778"
---
# <a name="visualize-azure-cosmos-db-data-by-using-the-power-bi-connector"></a>Power BI bağlayıcısını kullanarak Azure Cosmos DB verilerini görselleştirme

[Power BI](https://powerbi.microsoft.com/) , panoları ve raporları oluşturabileceğiniz ve paylaşabileceğiniz bir çevrimiçi hizmettir. Power BI Desktop bir rapor yazma aracıdır ve çeşitli veri kaynaklarından veri almanıza olanak tanır. Azure Cosmos DB, Power BI Desktop ile kullanabileceğiniz veri kaynaklarından biridir. Power BI için Azure Cosmos DB bağlayıcısını kullanarak Power BI Desktop'ı Azure Cosmos DB'ye bağlayabilirsiniz.  Azure Cosmos DB verilerini Power BI'da içeri aktardıktan sonra bu verileri dönüştürebilir, raporlar oluşturabilir ve raporları Power BI'da yayımlayabilirsiniz.   

Bu makalede Azure Cosmos DB hesabını Power BI Desktop'a bağlamak için gereken adımlar açıklanır. Bağlandıktan sonra bir koleksiyona gider, verileri ayıklar, JSON verilerini tablo biçimine dönüştürür ve Power BI'a rapor yayımlarsınız.

> [!NOTE]
> Azure Cosmos DB için Power BI Bağlayıcısı Power BI Desktop 'e bağlanır. Power BI Desktop oluşturulan raporlar PowerBI.com 'e yayımlanabilir. Azure Cosmos DB verilerinin doğrudan ayıklanması PowerBI.com 'ten gerçekleştirilemez. 

> [!NOTE]
> Power BI Bağlayıcısı ile Azure Cosmos DB bağlantı şu anda yalnızca Azure Cosmos DB SQL API ve Gremlin API hesapları için desteklenmektedir.

## <a name="prerequisites"></a>Ön koşullar
Bu Power BI öğreticideki yönergeleri takip etmeden önce aşağıdaki kaynaklara erişiminizin olduğundan emin olun:

* [En son Power BI Desktop sürümünü indirin](https://powerbi.microsoft.com/desktop).

* GitHub 'dan [örnek Volcano verilerini](https://github.com/Azure-Samples/azure-cosmos-db-sample-data/blob/master/SampleData/VolcanoData.json) indirin.

* [Azure Cosmos DB veri geçiş aracını](import-data.md)kullanarak [bir Azure Cosmos veritabanı hesabı oluşturun](https://azure.microsoft.com/documentation/articles/create-account/) ve Volcano verilerini içeri aktarın. Verileri içeri aktarırken, veri geçiş aracında kaynak ve hedefler için aşağıdaki ayarları göz önünde bulundurun:

   * **Kaynak parametreleri** 

       * **Içeri aktarma:** JSON dosyaları

   * **Hedef parametreler** 

      * **Bağlantı dizesi:**`AccountEndpoint=<Your_account_endpoint>;AccountKey=<Your_primary_or_secondary_key>;Database= <Your_database_name>` 

      * **Bölüm anahtarı:** /ülke 

      * **Koleksiyon verimlilik:** 1000 

Raporlarınızı PowerBI.com ' de paylaşmak için, PowerBI.com ' de bir hesabınız olmalıdır.  Power BI ve Power BI Pro hakkında daha fazla bilgi edinmek için bkz [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing) ..

## <a name="lets-get-started"></a>Haydi başlayalım
Bu öğreticide, dünyanın dört bir yanındaki bir tologist verdikcızı olduğunu düşünelim. Volcano verileri bir Azure Cosmos DB hesabında depolanır ve JSON belge biçimi aşağıdaki gibidir:

```json
{
    "Volcano Name": "Rainier",
        "Country": "United States",
        "Region": "US-Washington",
        "Location": {
          "type": "Point",
          "coordinates": [
            -121.758,
            46.87
          ]
        },
        "Elevation": 4392,
        "Type": "Stratovolcano",
        "Status": "Dendrochronology",
        "Last Known Eruption": "Last known eruption from 1800-1899, inclusive"
}
```

Azure Cosmos DB hesabından Volcano verilerini alıp etkileşimli bir Power BI raporundaki verileri görselleştirebilirsiniz.

1. Power BI Desktop çalıştırın.

2. **Veri alabilir**, **en son kaynaklara**bakabilirsiniz veya **diğer raporları** doğrudan hoş geldiniz ekranından açabilirsiniz. Ekranı kapatmak için sağ üst köşedeki "X" i seçin. Power BI Desktop **rapor** görünümü görüntülenir.
   
   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbireportview.png" alt-text="Power BI Desktop rapor görünümü-Power BI Bağlayıcısı":::

3. **Giriş** şeritini seçin ve **veri al**' a tıklayın.  **Veri al** penceresi görünmelidir.

4. **Azure**' a tıklayın, **Azure Cosmos DB (Beta)** öğesini seçin ve ardından **Bağlan**' a tıklayın. 

   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbigetdata.png" alt-text="Veri Al-Power BI Bağlayıcısı Power BI Desktop":::

5. **Önizleme Bağlayıcısı** sayfasında **devam**' a tıklayın. **Azure Cosmos DB** penceresi görüntülenir.

6. Verileri aşağıda gösterildiği gibi almak istediğiniz Azure Cosmos DB hesabı uç nokta URL 'sini belirtin ve ardından **Tamam**' a tıklayın. Kendi hesabınızı kullanmak için Azure portal **anahtarlar** DIKEY penceresindeki URI kutusundan URL 'yi alabilirsiniz. İsteğe bağlı olarak, verilerin nereden geldiğini tanımlayacak veritabanını ve koleksiyonu seçmek için veritabanı adını, koleksiyon adını sağlayabilir veya Gezginini kullanabilirsiniz.
   
7. Bu uç noktaya ilk kez bağlanıyorsanız hesap anahtarı istenir. Kendi hesabınız için, Azure portal **salt okunurdur anahtarlar** dikey penceresindeki **birincil anahtar** kutusundan anahtarı alın. Uygun anahtarı girin ve sonra **Bağlan**' a tıklayın.
   
   Rapor oluştururken salt okunurdur anahtarını kullanmanızı öneririz. Bu, ana anahtarın potansiyel güvenlik riskleri açısından gereksiz şekilde açıklanmasını önler. Salt okuma anahtarı, Azure portal **anahtarlar** dikey penceresinde kullanılabilir. 
    
8. Hesap başarıyla bağlandığında **Gezgin** bölmesi görüntülenir. **Gezgin** , hesap altındaki veritabanlarının listesini gösterir.

9. Raporun verilerinin geldiği veritabanına tıklayın ve genişletin, **volcanodb** ' yi seçin (veritabanınızın adı farklı olabilir).   

10. Şimdi, alınacak verileri içeren bir koleksiyon seçin, **volcano1** (koleksiyon adınız farklı olabilir) seçeneğini belirleyin.
    
    Önizleme bölmesi, **kayıt** öğelerinin bir listesini gösterir.  Belge, Power BI bir **kayıt** türü olarak gösterilir. Benzer şekilde, bir belge içinde iç içe geçmiş bir JSON bloğu da bir **kayıttır**.
    
    :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbinavigator.png" alt-text="Azure Cosmos DB Power BI Bağlayıcısı için Power BI öğreticisi-Gezgin penceresi":::

12. Verileri dönüştürmek için sorgu düzenleyicisini yeni bir pencerede başlatmak üzere **Düzenle** ' ye tıklayın.

## <a name="flattening-and-transforming-json-documents"></a>JSON belgelerini düzleştirme ve dönüştürme
1. Orta bölmedeki **belge** sütununun bulunduğu Power BI Sorgu Düzenleyicisi penceresine geçin.

   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png" alt-text="Power BI Desktop Sorgu Düzenleyicisi":::

1. **Belge** sütun üstbilgisinin sağ tarafındaki genişleticiye tıklayın.  Bir alan listesi ile bağlam menüsü görüntülenir.  Raporunuz için ihtiyacınız olan alanları seçin; örneğin, Volcano adı, ülke, bölge, konum, yükseltme, tür, durum ve son bilgi alma. **Özgün sütun adını önek olarak kullan** kutusunun işaretini kaldırın ve ardından **Tamam**' a tıklayın.
   
   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png" alt-text="Azure Cosmos DB Power BI Bağlayıcısı için Power BI öğreticisi-Belgeleri Genişlet":::

1. Orta bölmede, seçili alanlarla sonucun önizlemesi görüntülenir.
   
   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png" alt-text="Azure Cosmos DB Power BI Bağlayıcısı için Power BI öğreticisi-sonuçları Düzleştir":::

1. Örneğimizde Location özelliği bir belgedeki GeoJSON bloğudur.  Görebileceğiniz gibi, konum Power BI Desktop bir **kayıt** türü olarak gösterilir.  

1. Belge. Location sütun üstbilgisinin sağ tarafındaki genişleticiye tıklayın.  Tür ve koordinat alanları içeren bağlam menüsü görüntülenir.  Koordinatlar alanını seçelim, **ön ek seçili olmadığından özgün sütun adını kullan** ' ın seçili olduğundan emin olun ve **Tamam**' a tıklayın.
   
   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png" alt-text="Azure Cosmos DB Power BI Bağlayıcısı-konum kaydı için Power BI öğreticisi":::

1. Orta bölmede artık **liste** türü bir koordinat sütunu gösteriliyor.  Öğreticinin başlangıcında gösterildiği gibi, bu öğreticideki GeoJSON verileri, koordinatlar dizisine kaydedilmiş Enlem ve boylam değerlerini içeren nokta türüdür.
   
   Koordinatlar [0] öğesi, [1] koordinatları enlem temsil ettiğinde Boylam temsil ediyor.

   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png" alt-text="Azure Cosmos DB Power BI bağlayıcı koordinatları listesi için Power BI öğreticisi":::

1. Koordinatları dizisini düzleştirmek için, LatLong adlı **özel bir sütun** oluşturun.  **Sütun Ekle** şeridini seçin ve **özel sütun**' a tıklayın.  **Özel sütun** penceresi görüntülenir.

1. Yeni sütun için bir ad girin, örneğin, LatLong.

1. Ardından, yeni sütun için özel formül belirtin.  Bizim örneğimizde, aşağıdaki formül kullanılarak aşağıda gösterildiği gibi bir virgülle ayrılmış Enlem ve boylam değerlerini birleştirilecek: `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})` . **Tamam**'a tıklayın.
   
   DAX işlevleri de dahil olmak üzere veri çözümleme Ifadeleri (DAX) hakkında daha fazla bilgi için lütfen [Power BI Desktop Içindeki Dax temel](https://docs.microsoft.com/power-bi/desktop-quickstart-learn-dax-basics)bilgilerini ziyaret edin.
   
   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png" alt-text="Azure Cosmos DB Power BI Bağlayıcısı için Power BI öğreticisi-özel sütun Ekle":::

1. Şimdi, Orta bölmede, değerlerle doldurulan yeni bir ve daha fazla sütun gösterilmektedir.
    
    :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png" alt-text="Azure Cosmos DB Power BI Bağlayıcısı için Power BI öğreticisi-özel LatLong sütunu":::
    
    Yeni sütunda bir hata alırsanız, sorgu ayarları altındaki uygulanan adımların aşağıdaki şekilde eşleştiğinden emin olun:
    
    :::image type="content" source="./media/powerbi-visualize/power-bi-applied-steps.png" alt-text="Uygulanan adımlar kaynak, gezinti, genişletilmiş belge, genişletilmiş belge. location, özel eklendi":::
    
    Adımlarınız farklıysa, ek adımları silip özel sütunu yeniden eklemeyi deneyin. 

1. Veri modelini kaydetmek için **Kapat ve Uygula** ' ya tıklayın.

   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbicloseapply.png" alt-text="Azure Cosmos DB Power BI Bağlayıcısı için Power BI öğreticisi-kapat & Uygula":::

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>Raporları oluşturma

Power BI Desktop rapor görünümü, verileri görselleştirmek için rapor oluşturmaya başlayabileceğiniz yerdir.  **Rapor** tuvalinde alanları sürükleyip bırakarak raporlar oluşturabilirsiniz.

:::image type="content" source="./media/powerbi-visualize/power_bi_connector_pbireportview2.png" alt-text="Power BI Desktop rapor görünümü-Power BI Bağlayıcısı":::

Rapor görünümünde şunları bulmanız gerekir:

1. **Alanlar** bölmesinde, raporlarınız için kullanabileceğiniz alanlarla birlikte veri modellerinin bir listesini görebileceğiniz yerdir.
1. **Görsel öğeler** bölmesi. Bir rapor, tek veya birden çok görselleştirmeler içerebilir.  Görsel **öğeler** bölmesinden ihtiyaçlarınıza uygun görsel türleri seçin.
1. Rapor **tuvali,** raporunuzun görsellerini oluşturduğunuz yerdir.
1. **Rapor** sayfası. Power BI Desktop birden çok rapor sayfası ekleyebilirsiniz.

Aşağıda basit bir etkileşimli harita görünümü raporu oluşturmanın temel adımları gösterilmektedir.

1. Bizim örneğimizde, her bir Volcano 'nun konumunu gösteren bir harita görünümü oluşturacağız.  **Görsel öğeler** bölmesinde, yukarıdaki ekran görüntüsünde vurgulanan şekilde harita görsel türüne tıklayın.  Harita görsel türünün **rapor** tuvalinde boyanmış olduğunu görmeniz gerekir.  **Görselleştirme** bölmesi, harita görsel türüyle ilişkili bir özellikler kümesi de görüntülemelidir.
1. Şimdi, **alanlar** bölmesindeki iade eden alanını **görsel öğeler** bölmesindeki **konum** özelliğine sürükleyip bırakın.
1. Sonra, Vocano Name alanını sürükleyin ve **gösterge** özelliğine bırakın.  
1. Ardından, yükseltme alanını sürükleyin ve **Boyut** özelliğine bırakın.  
1. Şimdi, her bir Volcano 'ın, Volcano 'ın yükseltmesinde bulunan kabarcığun boyutuyla ilgili bir kabarcık kümesini gösteren bir kabarcık kümesini gösteren harita görselini görmeniz gerekir.
1. Artık temel bir rapor oluşturdunuz.  Daha fazla görselleştirme ekleyerek raporu daha ayrıntılı bir şekilde özelleştirebilirsiniz.  Bu durumda, raporu etkileşimli hale getirmek için bir Volcano türü dilimleyici ekledik.  
   
1. Dosya menüsünde **Kaydet** ' e tıklayın ve dosyayı Powerbitusel. pbix olarak kaydedin.

## <a name="publish-and-share-your-report"></a>Raporunuzu yayımlayın ve paylaşabilirsiniz
Raporunuzu paylaşmak için PowerBI.com içinde bir hesabınız olması gerekir.

1. Power BI Desktop, **giriş** şeridi ' ne tıklayın.
1. **Yayımla**’ta tıklayın.  PowerBI.com hesabınız için Kullanıcı adı ve parola girmeniz istenir.
1. Kimlik bilgisinin kimliği doğrulandıktan sonra, rapor seçtiğiniz hedefe yayımlanır.
1. Raporunuzu PowerBI.com üzerinde görmek ve paylaşmak için **Power BI içinde ' Powerbituon. pbix ' öğesini aç** ' a tıklayın.
   
   :::image type="content" source="./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png" alt-text="Power BI başarılı bir şekilde yayımlanıyor! Öğreticiyi Power BI açın":::

## <a name="create-a-dashboard-in-powerbicom"></a>Power BI.com'da bir pano oluşturun
Artık bir raporunuz olduğuna göre, PowerBI.com 'de paylaşma olanağı sağlar

Raporunuzu Power BI Desktop 'den PowerBI.com 'e yayımladığınızda, PowerBI.com kiracınızda bir **rapor** ve **veri kümesi** oluşturur. Örneğin, PowerBI.com 'e **powerbituon** adlı bir rapor yayımladıktan sonra, PowerBI.com 'deki **raporlar** ve **veri kümeleri** bölümlerinde powerbituon ' u görürsünüz.

   :::image type="content" source="./media/powerbi-visualize/powerbi-reports-datasets.png" alt-text="PowerBI.com içinde yeni raporun ve veri kümesinin ekran görüntüsü":::

Paylaşılabilir bir pano oluşturmak için PowerBI.com raporunuzda **canlı sabitleme sayfası** düğmesine tıklayın.

   :::image type="content" source="./media/powerbi-visualize/power-bi-pin-live-tile.png" alt-text="PowerBI.com içinde yeni raporun ve veri kümesinin ekran görüntüsü":::

Ardından, yeni bir pano oluşturmak için [bir rapordan kutucuk sabitleme](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) bölümündeki yönergeleri izleyin. 

Ayrıca, bir panoyu oluşturmadan önce raporlamak için geçici değişiklikler yapabilirsiniz. Ancak, değişiklikleri gerçekleştirmek ve raporu PowerBI.com 'e yeniden yayınlamak için Power BI Desktop kullanmanız önerilir.

<!-- ## Refresh data in PowerBI.com
There are two ways to refresh data, ad hoc and scheduled.

For an ad hoc refresh, simply click on the eclipses (…) by the **Dataset**, e.g. PowerBITutorial. You should see a list of actions including **Refresh Now**. Click **Refresh Now** to refresh the data.

:::image type="content" source="./media/powerbi-visualize/power-bi-refresh-now.png" alt-text="Screenshot of Refresh Now in PowerBI.com":::

For a scheduled refresh, do the following.

1. Click **Schedule Refresh** in the action list. 

    :::image type="content" source="./media/powerbi-visualize/power-bi-schedule-refresh.png" alt-text="Screenshot of the Schedule Refresh in PowerBI.com":::
2. In the **Settings** page, expand **Data source credentials**. 
3. Click on **Edit credentials**. 
   
    The Configure popup appears. 
4. Enter the key to connect to the Azure Cosmos DB account for that data set, then click **Sign in**. 
5. Expand **Schedule Refresh** and set up the schedule you want to refresh the dataset. 
6. Click **Apply** and you are done setting up the scheduled refresh.
-->
## <a name="next-steps"></a>Sonraki adımlar
* Power BI hakkında daha fazla bilgi için bkz. [Power BI kullanmaya başlama](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/).
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için [Azure Cosmos DB belge giriş sayfasına](https://azure.microsoft.com/documentation/services/cosmos-db/)bakın.

