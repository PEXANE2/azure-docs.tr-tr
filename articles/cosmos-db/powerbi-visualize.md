---
title: Azure Cosmos DB konektörü için power BI öğreticisi
description: Azure Cosmos DB ve Power BI bağlayıcısını kullanarak JSON'u içe aktarmak, anlayışlı raporlar oluşturmak ve verileri görselleştirmek için bu Power BI öğreticisini kullanın.
author: SnehaGunda
ms.service: cosmos-db
ms.topic: conceptual
ms.date: 05/21/2019
ms.author: sngun
ms.openlocfilehash: 1dbdd428a54ebf38c7b880bb9530935c0f748226
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "69616818"
---
# <a name="visualize-azure-cosmos-db-data-by-using-the-power-bi-connector"></a>Power BI bağlayıcısını kullanarak Azure Cosmos DB verilerini görselleştirme

[Power BI,](https://powerbi.microsoft.com/) panolar ve raporlar oluşturabileceğiniz ve paylaşabileceğiniz çevrimiçi bir hizmettir. Power BI Desktop bir rapor yazma aracıdır ve çeşitli veri kaynaklarından veri almanıza olanak tanır. Azure Cosmos DB, Power BI Desktop ile kullanabileceğiniz veri kaynaklarından biridir. Power BI için Azure Cosmos DB bağlayıcısını kullanarak Power BI Desktop'ı Azure Cosmos DB'ye bağlayabilirsiniz.  Azure Cosmos DB verilerini Power BI'da içeri aktardıktan sonra bu verileri dönüştürebilir, raporlar oluşturabilir ve raporları Power BI'da yayımlayabilirsiniz.   

Bu makalede Azure Cosmos DB hesabını Power BI Desktop'a bağlamak için gereken adımlar açıklanır. Bağlandıktan sonra bir koleksiyona gider, verileri ayıklar, JSON verilerini tablo biçimine dönüştürür ve Power BI'a rapor yayımlarsınız.

> [!NOTE]
> Azure Cosmos DB'nin Power BI konektörü Power BI Desktop'a bağlanır. Power BI Desktop'da oluşturulan raporlar PowerBI.com için yayınlanabilir. Azure Cosmos DB verilerinin doğrudan çıkarılması PowerBI.com gerçekleştirilemez. 

> [!NOTE]
> Azure Cosmos DB'ye Power BI konektörüyle bağlanmak şu anda yalnızca Azure Cosmos DB SQL API ve Gremlin API hesapları için desteklenir.

## <a name="prerequisites"></a>Ön koşullar
Bu Power BI öğreticisindeki yönergeleri takip etmeden önce, aşağıdaki kaynaklara erişebildiğinizden emin olun:

* [Power BI Desktop'ın en son sürümünü indirin.](https://powerbi.microsoft.com/desktop)

* Örnek [volkan verilerini](https://github.com/Azure-Samples/azure-cosmos-db-sample-data/blob/master/SampleData/VolcanoData.json) GitHub'dan indirin.

* [Azure Cosmos veritabanı hesabı oluşturun](https://azure.microsoft.com/documentation/articles/create-account/) ve Azure [Cosmos DB veri geçiş aracını](import-data.md)kullanarak volcano verilerini aktarın. Veri aktarırken, veri geçiş aracındaki kaynak ve varış noktaları için aşağıdaki ayarları göz önünde bulundurun:

   * **Kaynak parametreleri** 

       * **İthalat:** JSON dosyası(lar)

   * **Hedef parametreleri** 

      * **Bağlantı dizesi:**`AccountEndpoint=<Your_account_endpoint>;AccountKey=<Your_primary_or_secondary_key>;Database= <Your_database_name>` 

      * **Bölüm tuşu:** /Ülke 

      * **Koleksiyon İşlemi:** 1000 

Raporlarınızı PowerBI.com olarak paylaşmak için PowerBI.com bir hesabınız olması gerekir.  Power BI ve Power BI Pro [https://powerbi.microsoft.com/pricing](https://powerbi.microsoft.com/pricing)hakkında daha fazla bilgi edinmek için bkz.

## <a name="lets-get-started"></a>Başlayalım.
Bu eğitimde, dünya çapında volkanlar üzerinde çalışan bir jeolog olduğunuzu düşünelim. Volkan verileri bir Azure Cosmos DB hesabında depolanır ve JSON belge biçimi aşağıdaki gibidir:

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

Volkan verilerini Azure Cosmos DB hesabından alır ve etkileşimli power bi raporunda verileri görselleştirirsiniz.

1. Power BI Masaüstünü çalıştırın.

2. Verileri **Alabilirsiniz,** **Son Kaynaklar'ı**görebilir veya Diğer Raporları doğrudan karşılama ekranından **açabilirsiniz.** Ekranı kapatmak için sağ üst köşedeki "X"i seçin. Power BI Desktop'ın **Rapor** görünümü görüntülenir.
   
   ![Power BI Masaüstü Rapor Görünümü - Power BI konektörü](./media/powerbi-visualize/power_bi_connector_pbireportview.png)

3. Ana **Ekran** şeridini seçin ve **ardından Veri Al'ı**tıklatın.  **Veri Al** penceresi görünmelidir.

4. **Azure'a**tıklayın, **Azure Cosmos DB'yi (Beta)** seçin ve sonra **Bağlan'ı**tıklatın. 

    ![Power BI Desktop Veri Alın - Power BI konektörü](./media/powerbi-visualize/power_bi_connector_pbigetdata.png)   

5. Önizleme **Bağlayıcısı** sayfasında **Devam et'i**tıklatın. **Azure Cosmos DB** penceresi görüntülenir.

6. Aşağıda gösterildiği gibi verileri almak istediğiniz Azure Cosmos DB hesap bitiş noktası URL'sini belirtin ve **ardından Tamam'ı**tıklatın. Kendi hesabınızı kullanmak için, URL'yi Azure portalının **Keys** bıçak kutusundaki URI kutusundan alabilirsiniz. İsteğe bağlı olarak veritabanı adını, koleksiyon adını sağlayabilir veya verilerin nereden geldiğini belirlemek için veritabanını ve koleksiyonu seçmek için gezgini kullanabilirsiniz.
   
7. Bu bitiş noktasına ilk kez bağlanıyorsanız, hesap anahtarı istenir. Kendi hesabınız için, Azure portalının **salt okunur Tuşları** bıçağındaki Birincil **Anahtar** kutusundan anahtarı alın. Uygun anahtarı girin ve sonra **Bağlan'ı**tıklatın.
   
   Raporlar alırken salt okunur anahtarını kullanmanızı öneririz. Bu, ana anahtarın olası güvenlik risklerine karşı gereksiz şekilde maruz kalmasını önler. Salt okunur tuşu Azure portalının **Keys** bıçaklarından edinilebilir. 
    
8. Hesap başarıyla bağlandığında, **Gezgin** bölmesi görüntülenir. **Navigator,** hesabın altındaki veritabanlarının listesini gösterir.

9. Raporun verilerinin geldiği veritabanını tıklatın ve genişletin, **volcanodb'u** seçin (veritabanı adınız farklı olabilir).   

10. Şimdi, alınacak verileri içeren bir koleksiyon seçin, **volcano1'i** seçin (koleksiyon adınız farklı olabilir).
    
    Önizleme bölmesi **Kayıt** öğelerinin listesini gösterir.  Belge, Power BI'de **Kayıt** türü olarak temsil edilir. Benzer şekilde, bir belgenin içindeki iç içe bir JSON bloğu da bir **Kayıttır.**
    
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Gezinme penceresi](./media/powerbi-visualize/power_bi_connector_pbinavigator.png)
12. Verileri dönüştürmek için Sorgu Düzenleyicisini yeni bir pencerede başlatmak için **Düzenley'i** tıklatın.

## <a name="flattening-and-transforming-json-documents"></a>JSON belgelerinin düzemi ve dönüşümü
1. Merkez bölmesindeki **Belge** sütunundaki Power BI Query Editor penceresine geçin.
   ![Power BI Desktop Sorgu Düzenleyicisi](./media/powerbi-visualize/power_bi_connector_pbiqueryeditor.png)
2. **Belge** sütun üstbilgisinin sağ tarafındaki genişleticiye tıklayın.  Alanların listesini içeren bağlam menüsü görüntülenir.  Örneğin, Volkan Adı, Ülke, Bölge, Konum, Yükseklik, Tür, Durum ve Son Patlama yıkıntı. Önek kutusu **olarak orijinal sütun adını kullan'ın** işaretlerini kaldırın ve ardından **Tamam'ı**tıklatın.
   
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Belgeleri genişletme](./media/powerbi-visualize/power_bi_connector_pbiqueryeditorexpander.png)
3. Orta bölme, seçili alanlar ile sonucun önizlemesini görüntüler.
   
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Sonuçları düzleştirmek](./media/powerbi-visualize/power_bi_connector_pbiresultflatten.png)
4. Örneğimizde, Konum özelliği bir belgedeki bir GeoJSON bloğudur.  Gördüğünüz gibi, Konum Power BI Desktop'da **Kayıt** türü olarak temsil edilir.  
5. Document.Location sütun üstbilgisinin sağ tarafındaki genişleticiye tıklayın.  Tür ve koordinat alanları içeren bağlam menüsü görüntülenir.  Koordinatalanını seçelim, **önek seçilmedikçe orijinal sütun adını kullan'ı** ve **Tamam'ı**tıklatın.
   
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Konum kaydı](./media/powerbi-visualize/power_bi_connector_pbilocationrecord.png)
6. Merkez bölmesi şimdi **Liste** türünde bir koordinat sütunu gösterir.  Öğreticinin başında gösterildiği gibi, bu öğreticideki GeoJSON verileri koordinat dizisinde kaydedilen Enlem ve Boylam değerleriile Nokta türündedir.
   
    Koordinatlar[0] öğesi Boylam'ı, koordinatlar ise Enlem'i temsil eder.
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Koordinatlar listesi](./media/powerbi-visualize/power_bi_connector_pbiresultflattenlist.png)
7. Koordinat dizisini düzleştirmek için LatLong adında bir **Özel Sütun** oluşturun.  Sütun **Ekle** şeridini seçin ve **Özel Sütun'a**tıklayın.  **Özel Sütun** penceresi görüntülenir.
8. LatLong gibi yeni sütun için bir ad sağlayın.
9. Ardından, yeni sütun için özel formülü belirtin.  Örneğiniz için, aşağıdaki formülü kullanarak aşağıda gösterildiği gibi virgülle ayrılmış Enlem ve `Text.From([coordinates]{1})&","&Text.From([coordinates]{0})`Boylam değerlerini birleştiririz: . **Tamam**'a tıklayın.
   
    DAX işlevleri de dahil olmak üzere Veri Analizi İfadeleri (DAX) hakkında daha fazla bilgi için, power [BI Masaüstünde DAX Temelleri'ni](https://docs.microsoft.com/power-bi/desktop-quickstart-learn-dax-basics)ziyaret edin.
   
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Özel Sütun Ekle](./media/powerbi-visualize/power_bi_connector_pbicustomlatlong.png)

10. Şimdi, orta bölme değerleri ile doldurulan yeni LatLong sütunları gösterir.
    
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Özel LatLong sütunu](./media/powerbi-visualize/power_bi_connector_pbicolumnlatlong.png)
    
    Yeni sütunda bir Hata alırsanız, Sorgu Ayarları altında uygulanan adımların aşağıdaki şekilde eşleştirdiğinden emin olun:
    
    ![Uygulanan adımlar Kaynak olmalı, Gezinme, Genişletilmiş Belge, Genişletilmiş Belge.Konum, Özel Eklendi](./media/powerbi-visualize/power-bi-applied-steps.png)
    
    Adımlarınız farklıysa, ek adımları silin ve özel sütunu yeniden eklemeyi deneyin. 

11. Veri modelini kaydetmek için **Kapat ve Uygula'yı** tıklatın.
    
    ![Azure Cosmos DB Power BI konektörü için Power BI öğretici - Yakın & Uygula](./media/powerbi-visualize/power_bi_connector_pbicloseapply.png)

<a id="build-the-reports"></a>
## <a name="build-the-reports"></a>Raporları oluşturma
Power BI Desktop Report görünümü, verileri görselleştirmek için raporlar oluşturmaya başlabildiğiniz yerdir.  Alanları **Rapor** tuvaline sürükleyip bırakarak raporlar oluşturabilirsiniz.

![Power BI Masaüstü Rapor Görünümü - Power BI konektörü](./media/powerbi-visualize/power_bi_connector_pbireportview2.png)

Rapor görünümünde şunları bulmanız gerekir:

1. **Alanlar** bölmesi, raporlarınız için kullanabileceğiniz alanlara sahip veri modellerinin listesini burada görebilirsiniz.
2. **Görselleştirmeler** bölmesi. Rapor, tek veya birden çok görsellik içerebilir.  **Görselleştirmeler** bölmesinden ihtiyaçlarınıza uygun görsel türleri seçin.
3. **Rapor** tuvali, raporunuz için görselleri oluşturduğunuz yerdir.
4. **Rapor** sayfası. Power BI Desktop'a birden çok rapor sayfası ekleyebilirsiniz.

Aşağıda basit bir etkileşimli Harita görünümü raporu oluşturmanın temel adımları gösterilmektedir.

1. Örneğin, her volkanın konumunu gösteren bir harita görünümü oluşturacağız.  **Görseller** bölmesinde, yukarıdaki ekran görüntüsünde vurgulandığı gibi Harita görsel türünü tıklatın.  **Rapor** tuvaline boyanmış Harita görsel türünü görmelisiniz.  **Görselleştirme** bölmesi, Harita görsel türüyle ilgili bir dizi özelliği de göstermelidir.
2. Şimdi, LatLong alanını **Alanlar** bölmesinden **Visualization** bölmesinde **Konum** özelliğine sürükleyin ve bırakın.
3. Ardından, Volcano Name alanını **sürükleyin** ve Legend özelliğine bırakın.  
4. Ardından, Yükseklik alanını **Boyut** özelliğine sürükleyin ve bırakın.  
5. Şimdi, volkanın yüksekliğiyle ilişkili kabarcık boyutuna sahip her volkanın yerini gösteren bir dizi kabarcıkları gösteren Harita görselini görmelisiniz.
6. Şimdi temel bir rapor oluşturdunuz.  Daha fazla görselleştirme ekleyerek raporu daha da özelleştirebilirsiniz.  Bizim durumumuzda, raporu etkileşimli hale getirmek için bir Volcano Type dilimleyici ekledik.  
   
7. Dosya menüsünde **Kaydet'i** tıklatın ve dosyayı PowerBITutorial.pbix olarak kaydedin.

## <a name="publish-and-share-your-report"></a>Raporunuzu yayımlayın ve paylaşın
Raporunuzu paylaşmak için PowerBI.com bir hesabınız olması gerekir.

1. Power BI Masaüstü'nde **Ana Ekran** şeridine tıklayın.
2. **Yayımla**’ta tıklayın.  PowerBI.com hesabınızın kullanıcı adını ve şifresini girmeniz istenir.
3. Kimlik bilgisi doğrulandıktan sonra, rapor seçtiğiniz hedefinize göre yayınlanır.
4. PowerBI.com hakkındaki raporunuzu görmek ve paylaşmak için **Power BI'de 'PowerBITutorial.pbix'i açın'ı** tıklatın.
   
    ![Power BI Başarı Yayıncılık! Power BI'de açık öğretici](./media/powerbi-visualize/power_bi_connector_open_in_powerbi.png)

## <a name="create-a-dashboard-in-powerbicom"></a>Power BI.com'da bir pano oluşturun
Artık bir raporunuz olduğuna göre, PowerBI.com

Raporunuzu Power BI Desktop'dan PowerBI.com yayımladığınızda, PowerBI.com kiracınızda bir **Rapor** ve **Veri Kümesi** oluşturur. Örneğin, PowerBI.com için PowerBITutorial adlı bir rapor yayınladıktan sonra, PowerBI.com hem **Raporlar** hem de **Veri Kümeleri** bölümlerinde **PowerBITutorial'i** görürsünüz.

   ![PowerBI.com yeni Rapor ve Dataset ekran görüntüsü](./media/powerbi-visualize/powerbi-reports-datasets.png)

Sharable bir pano oluşturmak için, PowerBI.com raporunuzdaki **Canlı Sayfayı Sabitle** düğmesini tıklatın.

   ![PowerBI.com yeni Rapor ve Dataset ekran görüntüsü](./media/powerbi-visualize/power-bi-pin-live-tile.png)

Ardından, yeni bir pano oluşturmak için [rapordan bir döşemeyi sabitle](https://powerbi.microsoft.com/documentation/powerbi-service-pin-a-tile-to-a-dashboard-from-a-report/#pin-a-tile-from-a-report) yönergeleriizleyin. 

Ayrıca, pano oluşturmadan önce rapor vermek için geçici değişiklikler de yapabilirsiniz. Ancak, değişiklikleri gerçekleştirmek ve raporu PowerBI.com yeniden yayımlamak için Power BI Desktop'ı kullanmanız önerilir.

<!-- ## Refresh data in PowerBI.com
There are two ways to refresh data, ad hoc and scheduled.

For an ad hoc refresh, simply click on the eclipses (…) by the **Dataset**, e.g. PowerBITutorial. You should see a list of actions including **Refresh Now**. Click **Refresh Now** to refresh the data.

![Screenshot of Refresh Now in PowerBI.com](./media/powerbi-visualize/power-bi-refresh-now.png)

For a scheduled refresh, do the following.

1. Click **Schedule Refresh** in the action list. 

    ![Screenshot of the Schedule Refresh in PowerBI.com](./media/powerbi-visualize/power-bi-schedule-refresh.png)
2. In the **Settings** page, expand **Data source credentials**. 
3. Click on **Edit credentials**. 
   
    The Configure popup appears. 
4. Enter the key to connect to the Azure Cosmos DB account for that data set, then click **Sign in**. 
5. Expand **Schedule Refresh** and set up the schedule you want to refresh the dataset. 
6. Click **Apply** and you are done setting up the scheduled refresh.
-->
## <a name="next-steps"></a>Sonraki adımlar
* Power BI hakkında daha fazla bilgi edinmek için Power [BI ile başlayın.](https://powerbi.microsoft.com/documentation/powerbi-service-get-started/)
* Azure Cosmos DB hakkında daha fazla bilgi edinmek için [Azure Cosmos DB belgeleri açılış sayfasına](https://azure.microsoft.com/documentation/services/cosmos-db/)bakın.

