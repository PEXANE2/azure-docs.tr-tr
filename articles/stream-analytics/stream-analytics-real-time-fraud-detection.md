---
title: Azure Stream Analytics kullanarak gerçek zamanlı sahtekarlık algılama
description: Stream Analytics ile gerçek zamanlı dolandırıcılık algılama çözümü oluşturmayı öğrenin. Gerçek zamanlı olay işleme için bir olay hub'ı kullanın.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 03/24/2020
ms.custom: seodec18
ms.openlocfilehash: c0b2943e1f0d7f2386ec09da03d297a570eede7a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80276487"
---
# <a name="get-started-using-azure-stream-analytics-real-time-fraud-detection"></a>Azure Akış Analizi: Gerçek zamanlı sahtekarlık algılama

Bu öğretici, Azure Akış Analizi'nin nasıl kullanılacağına ilişkin uçtan uca bir örnek sağlar. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz: 

* Akış olaylarını Azure Etkinlik Hub'ları örneğine getirin. Bu eğitimde, cep telefonu meta veri kayıtları akışını simüle eden bir uygulama kullanırsınız.

* Verileri dönüştürmek, bilgileri bir araya getirmek veya desenler aramak için SQL benzeri Akış Analizi sorguları yazın. Gelen akışı incelemek ve sahte olabilecek aramaları aramak için sorguyu nasıl kullanacağınızı görürsünüz.

* Sonuçları, ek öngörüler için analiz edebileceğiniz bir çıktı lavabosuna (depolama) gönderin. Bu durumda, şüpheli arama verilerini Azure Blob depolama alanına gönderirsiniz.

Bu öğretici, telefon görüşmesi verilerine dayalı gerçek zamanlı dolandırıcılık algılama örneğini kullanır. Resimli teknik, kredi kartı dolandırıcılığı veya kimlik hırsızlığı gibi diğer dolandırıcılık algılama türleri için de uygundur. 

## <a name="scenario-telecommunications-and-sim-fraud-detection-in-real-time"></a>Senaryo: Gerçek zamanlı olarak Telekomünikasyon ve SIM dolandırıcılık algılama

Bir telekomünikasyon şirketi gelen aramalar için büyük miktarda veriye sahiptir. Şirket, müşterileri bilgilendirmek veya belirli bir numara için hizmeti kapatmak için sahte aramaları gerçek zamanlı olarak algılamak ister. Bir TÜR SIM sahtekarlığı, aynı anda ancak coğrafi olarak farklı konumlarda aynı kimlikten birden fazla arama içerir. Bu tür bir sahtekarlığı tespit etmek için, şirketin gelen telefon kayıtlarını incelemesi ve farklı ülkelerde/bölgelerde aynı anda yapılan aramalar için belirli desenleri incelemesi gerekir. Bu kategoriye giren tüm telefon kayıtları sonraki analiziçin depolamaya yazılır.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticide, örnek telefon görüşmesi meta verileri oluşturan bir istemci uygulaması kullanarak telefon görüşmesi verilerini simüle edeyim. Uygulamanın ürettiği bazı kayıtlar sahte aramalara benzer. 

Başlamadan önce şunlara sahip olduğunuzdan emin olun:

* Bir Azure hesabı.
* Çağrı-olay jeneratör uygulaması, [TelcoGenerator.zip](https://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip), Microsoft Download Center indirilebilir. Bu paketi bilgisayarınızdaki bir klasöre dönüştürün. Kaynak kodunu görmek ve uygulamayı hata ayıklayıcıda çalıştırmak istiyorsanız, uygulama kaynak kodunu [GitHub'dan](https://aka.ms/azure-stream-analytics-telcogenerator)alabilirsiniz. 

    >[!NOTE]
    >Windows indirilen .zip dosyasını engelleyebilir. Fermuarını açamıyorsanız, dosyayı sağ tıklatın ve **Özellikler'i**seçin. "Bu dosya başka bir bilgisayardan geldi ve bu bilgisayarın korunmasına yardımcı olmak için engellenmiş olabilir" iletisini görüyorsanız, **Engeli Kaldır** seçeneğini seçin ve ardından **Uygula'yı**tıklatın.

Akış Analizi işinin sonuçlarını incelemek istiyorsanız, bir Azure Blob Depolama kapsayıcısının içeriğini görüntülemek için de bir araca ihtiyacınız vardır. Visual Studio kullanıyorsanız, Visual Studio veya [Visual Studio Cloud Explorer](https://docs.microsoft.com/azure/vs-azure-tools-resources-managing-with-cloud-explorer)için Azure [Araçlarını](https://docs.microsoft.com/azure/vs-azure-tools-storage-resources-server-explorer-browse-manage) kullanabilirsiniz. Alternatif olarak, [Azure Depolama Gezgini](https://storageexplorer.com/) veya [Cerulean](https://www.cerebrata.com/products/cerulean/features/azure-storage)gibi bağımsız araçları yükleyebilirsiniz. 

## <a name="create-an-azure-event-hubs-to-ingest-events"></a>Etkinlikleri yutmak için Azure Etkinlik Hub'ları oluşturma

Bir veri akışını çözümlemek için, veri akışını Azure'a *sindirirsiniz.* Verileri yutmanın tipik bir yolu, saniyede milyonlarca olayı yutmanızı ve ardından olay bilgilerini işlemenizi ve depolamanızı sağlayan [Azure Etkinlik Hub'larını](../event-hubs/event-hubs-what-is-event-hubs.md)kullanmaktır. Bu öğretici için, bir olay hub'ı oluşturacak ve ardından çağrı-olay jeneratörü uygulamasının bu olay merkezine çağrı verilerini göndermesini sağlayacaksınız. Etkinlik merkezleri hakkında daha fazla bilgi için [Azure Hizmet Veri Merkezi belgelerine](https://docs.microsoft.com/azure/service-bus/)bakın.

>[!NOTE]
>Bu yordamın daha ayrıntılı bir sürümü için, [Azure portalını kullanarak bir Olay Hub'ları ad alanı ve etkinlik merkezi oluşturun'a](../event-hubs/event-hubs-create.md)bakın. 

### <a name="create-a-namespace-and-event-hub"></a>Ad alanı ve olay merkezi oluşturma
Bu yordamda, önce bir olay hub'ı ad alanı oluşturursunuz ve ardından bu ad alanına bir olay hub'ı eklersiniz. Olay hub ad alanları mantıksal olarak ilişkili olay veri ağacı örneklerini gruplandırmak için kullanılır. 

1. Azure portalında oturum açın ve ekranın sol üst kısmında **kaynak oluştur'u** tıklatın.

2. Sol menüdeki **Tüm hizmetleri** seçin ve **Analytics** kategorisinde **Olay Hub'larının** yanındaki **yıldız ()`*`** seçeneğini belirleyin. Olay **Hub'larının** sol daki gezinme menüsünde **SıK Kullanılanlar'a** eklenmesini onaylayın. 

   ![Etkinlik Hub'larını ara](./media/stream-analytics-real-time-fraud-detection/select-event-hubs-menu.png)

3. Sol gezinti menüsünde **SıK Kullanılanlar** altında Olay **Hub'larını** seçin ve araç çubuğuna **Ekle'yi** seçin.

   ![Ekle düğmesi](./media/stream-analytics-real-time-fraud-detection/event-hubs-add-toolbar.png)

4. Ad **alanı oluştur** bölmesine, . gibi `<yourname>-eh-ns-demo`bir ad alanı adı girin Ad alanı için herhangi bir ad kullanabilirsiniz, ancak ad bir URL için geçerli olmalı ve Azure genelinde benzersiz olmalıdır. 
    
5. Bir abonelik seçin ve bir kaynak grubu oluşturun veya seçin, ardından **Oluştur'u**tıklatın.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-namespace-new-portal.png" alt="Create event hub namespace in Azure portal" width="300px"/>

6. Ad alanı dağıtımı tamamlandığında, Azure kaynakları listenizdeki etkinlik merkezi ad alanını bulun. 

7. Yeni ad alanını tıklatın ve ad alanı bölmesinde **Olay Hub'ını**tıklatın.

   ![Yeni bir olay hub'ı oluşturmak için Olay Hub'ı Ekle düğmesi](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-button-new-portal.png)    
 
8. Yeni olay merkezini `asa-eh-frauddetection-demo`adlandırın. Farklı bir ad kullanabilirsiniz. Bunu yaparsanız, bunu not edin, çünkü daha sonra isme ihtiyacınız var. Şu anda etkinlik merkezi için başka seçenekler ayarlamanız gerekmez.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-eventhub-new-portal.png" alt="Name event hub in Azure portal" width="400px"/>
    
9. **Oluştur'u**tıklatın.

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Olay hub’ına erişim verme ve bir bağlantı dizesi alma

Bir işlem olay hub'ına veri gönderebilmek için, olay merkezinin uygun erişime izin veren bir ilkesi olması gerekir. Erişim ilkesi, yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1. Olay ad alanı **bölmesinde, Olay Hub'larını** tıklatın ve ardından yeni etkinlik hub'ınızın adını tıklatın.

2. Olay hub bölmesi bölmesi, **Paylaşılan erişim ilkeleri** ve sonra ** + &nbsp;Ekle'yi**tıklatın.

    > [!NOTE]
    > Olay hub'ı ad alanıyla değil, olay hub'ıyla çalıştığınızdan emin olun.

3. Adlandırılmış `asa-policy-manage-demo` bir ilke ekleyin ve **Claim**için **Yönet'i**seçin.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-shared-access-policy-manage-new-portal.png" alt="Create shared access policy for Stream Analytics" width="300px"/>
 
4. **Oluştur'u**tıklatın.

5. İlke dağıtıldıktan sonra, paylaşılan erişim ilkeleri listesinde tıklatın.

6. **CONNECTION STRING-PRIMARY KEY** etiketli kutuyu bulun ve bağlantı dizesinin yanındaki kopya düğmesini tıklatın. 

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-shared-access-policy-copy-connection-string-new-portal.png" alt="Stream Analytics shared access policy" width="300px"/>
 
7. Bağlantı dizesini bir metin düzenleyicisine yapıştırın. Bazı küçük yapılanlar yaptıktan sonra, bir sonraki bölüm için bu bağlantı dizegerekir.

    Bağlantı dizesi aşağıdaki gibi görünür:

    `Endpoint=sb://YOURNAME-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=asa-policy-manage-demo;SharedAccessKey=Gw2NFZwU1Di+rxA2T+6hJYAtFExKRXaC2oSQa0ZsPkI=;EntityPath=asa-eh-frauddetection-demo`

    Bağlantı dizesi, yarı iki nokta üst üste ayrılmış `Endpoint`birden `SharedAccessKeyName` `SharedAccessKey`çok `EntityPath`anahtar değeri çifti içerdiğine dikkat edin: , , ve .  


## <a name="configure-and-start-the-event-generator-application"></a>Yapılandırır ve olay jeneratörü uygulamasını başlatın

TelcoGenerator uygulamasını başlatmadan önce, oluşturduğunuz etkinlik merkezine çağrı kayıtları gönderecek şekilde yapılandırmanız gerekir.

### <a name="configure-the-telcogenerator-app"></a>TelcoGenerator uygulamasını yapılandırın

1. Bağlantı dizesini kopyaladığınız editörde, değeri `EntityPath` not edin ve `EntityPath` ardından çifti kaldırın (ondan önceki yarım nokta nokta noktasını kaldırmayı unutmayın). 

2. TelcoGenerator.zip dosyasının fermuarını açtığınız klasörde, bir editörde telcodatagen.exe.config dosyasını açın. (Birden fazla .config dosyası vardır, bu nedenle doğru dosyayı açtığından emin olun.)

3. Öğede: `<appSettings>`

   * Anahtar değerini `EventHubName` olay merkezi adı (diğer bir şekilde varlık yolunun değerine) ayarlayın.
   * Anahtarın `Microsoft.ServiceBus.ConnectionString` değerini bağlantı dizesine ayarlayın. 

   Bölüm `<appSettings>` aşağıdaki örnek gibi görünecektir:

    ```xml
    <appSettings>
     <!-- Service Bus specific app setings for messaging connections -->
     <add key="EventHubName" value="asa-eh-ns-demo"/>
     <add key="Microsoft.ServiceBus.ConnectionString" value="Endpoint=sb://asa-eh-ns-demo.servicebus.windows.net/;SharedAccessKeyName=asa-policy-manage-demo;SharedAccessKey=GEcnTKf2//1MRn6SN1A2u0O76MP9pj3v0Ccyf1su4Zo="/>
   </appSettings>
    ```
 
4. Dosyayı kaydedin. 

### <a name="start-the-app"></a>Uygulamayı başlatın

1. Bir komut penceresi açın ve TelcoGenerator uygulamasının fermuarsız olduğu klasöre değiştirin.

2. Aşağıdaki komutu girin:

    ```console
    telcodatagen.exe 1000 0.2 2
    ```

   Parametreler şunlardır: 

   * Saatte CdR sayısı. 
   * SIM Kart Sahtekarlığı Olasılığı: Tüm aramaların yüzdesi olarak uygulamanın sahte bir aramayı simüle etmesi ne sıklıkta. 0.2 değeri arama kayıtlarının %20'sinin sahte görüneceğini anlamına gelir.
   * Saat içinde süre. Uygulamanın çalışması gereken saat sayısı. Ayrıca komut satırında Ctrl+C tuşuna basarak uygulamayı istediğiniz zaman durdurabilirsiniz.

   Birkaç saniye sonra uygulama, telefon araması kayıtlarını olay hub'ına gönderirken ekranda bu kayıtları görüntülemeye başlar.

Bu gerçek zamanlı dolandırıcılık algılama uygulamasında kullanmak olacağınız önemli alanlardan bazıları şunlardır:

|**Kayıt**|**Tanım**|
|----------|--------------|
|`CallrecTime`|Arama başlangıç zamanı için zaman damgası. |
|`SwitchNum`|Aramayı bağlamak için kullanılan telefon anahtarı. Bu örnekte, anahtarlar menşe ülke/bölgeyi (ABD, Çin, İngiltere, Almanya veya Avustralya) temsil eden dizeleridir. |
|`CallingNum`|Arayanın telefon numarası. |
|`CallingIMSI`|Uluslararası Mobil Abone Kimliği (IMSI). Bu, arayanın benzersiz tanımlayıcısıdır. |
|`CalledNum`|Arama alıcısının telefon numarası. |
|`CalledIMSI`|Uluslararası Mobil Abone Kimliği (IMSI). Bu, arama alıcısının benzersiz tanımlayıcısıdır. |


## <a name="create-a-stream-analytics-job-to-manage-streaming-data"></a>Akış verilerini yönetmek için Bir Akış Analizi işi oluşturun

Artık bir çağrı etkinlikleri akışınız olduğuna göre, bir Stream Analytics işi ayarlayabilirsiniz. İş, ayarladığınız olay hub'ından verileri okur. 

### <a name="create-the-job"></a>İşi oluşturma 

1. Azure portalında, **Bir** > **Internet of Things** > Kaynak Nesnelerin İnterneti**Oluşturma Akış Analizi işini**tıklatın.

2. İşi `asa_frauddetection_job_demo`adlandırın, abonelik, kaynak grubu ve konum belirtin.

    En iyi performans için işi ve etkinlik merkezini aynı bölgeye yerleştirmek ve böylece bölgeler arasında veri aktarmak için ödeme yapmazsanız iyi bir fikirdir.

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-job-new-portal.png" alt="Create Stream Analytics job in portal" width="300px"/>

3. **Oluştur'u**tıklatın.

    İş oluşturulur ve portal iş ayrıntılarını görüntüler. Henüz hiçbir şey çalışıyor, ancak—başlatılmadan önce işi yapılandırmanız gerekir.

### <a name="configure-job-input"></a>İş girişi yapılandırma

1. Panoda veya **Tüm kaynaklar** `asa_frauddetection_job_demo` bölmesinde, Akış Analizi işini bulun ve seçin. 
2. Akış Analizi iş bölmesi Genel **Bakış** bölümünde **Giriş** kutusunu tıklatın.

   ![Akış Analizi iş bölmesinde Topoloji altında giriş kutusu](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-input-box-new-portal.png)
 
3. **Akış girişi ekle'yi** tıklatın ve **Olay Hub'ını**seçin. Ardından Yeni giriş sayfasını aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı  |  CallStream   |  İş girişini tanımlamak için bir ad girin.   |
   |Abonelik   |  \<Aboneliğiniz\> |  Oluşturduğunuz Etkinlik Hub'ına sahip Azure aboneliğini seçin.   |
   |Olay hub'ı ad alanı  |  asa-eh-ns-demo |  Olay Hub ad alanının adını girin.   |
   |Olay Hub'ı adı  | asa-eh-frauddetection-demo | Etkinlik Hub'ınızın adını seçin.   |
   |Olay Hub'ı ilke adı  | asa-politika-yönet-demo | Daha önce oluşturduğunuz erişim ilkesini seçin.   |

    </br>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sa-input-new-portal.png" alt="Create Stream Analytics input in portal" width="300px"/>


4. **Oluştur'u**tıklatın.

## <a name="create-queries-to-transform-real-time-data"></a>Gerçek zamanlı verileri dönüştürmek için sorgular oluşturma

Bu noktada, gelen bir veri akışını okumak için ayarlanmış bir Akış Analizi işiniz vardır. Bir sonraki adım, verileri gerçek zamanlı olarak çözümleyen bir sorgu oluşturmaktır. Akış Analizi, gerçek zamanlı işleme dönüşümlerini açıklayan basit, bildirimsel bir sorgu modelini destekler. Sorgular, Akış Analizi'ne özgü bazı uzantıları olan SQL benzeri bir dil kullanır. 

Basit bir sorgu sadece gelen tüm verileri okuyabilir. Ancak, genellikle belirli verileri veya verilerdeki ilişkileri arayan sorgular oluşturursunuz. Öğreticinin bu bölümünde, çözümleme için giriş akışını dönüştürebileceğiniz birkaç yol öğrenmek için birkaç sorgu oluşturur ve sınarsınız. 

Burada oluşturduğunuz sorgular yalnızca dönüştürülmüş verileri ekrana görüntüler. Daha sonraki bir bölümde, bir çıktı lavabosu ve dönüştürülmüş verileri bu lavaboya yazan bir sorgu yapılandırırsınız.

Dil hakkında daha fazla bilgi edinmek için [Azure Akışı Analitik Sorgu Dili Başvurusu'na](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)bakın.

### <a name="get-sample-data-for-testing-queries"></a>Test sorguları için örnek veri alma

TelcoGenerator uygulaması etkinlik merkezine çağrı kayıtları gönderiyor ve Stream Analytics işiniz etkinlik merkezinden okunacak şekilde yapılandırıldı. Doğru okuduğundan emin olmak için işi sınamak için bir sorgu kullanabilirsiniz. Azure konsolunda bir sorguyu sınamak için örnek verilere ihtiyacınız vardır. Bu izbin için, olay merkezine gelen akıştan örnek veriler ayıklanırsınız.

1. TelcoGenerator uygulamasının çalışan ve arama kayıtları üreten emin olun.
2. Portalda, Akış Analizi iş bölmesine geri dönün. (Bölmeyi kapattıysanız, Tüm `asa_frauddetection_job_demo` **kaynaklar** bölmesinde arama yapın.)
3. **Sorgu** kutusunu tıklatın. Azure, iş için yapılandırılan girdi ve çıktıları listeler ve çıktıya gönderilirken giriş akışını dönüştürmenize olanak tanıyan bir sorgu oluşturmanıza olanak tanır.
4. **Sorgu** bölmesi'nde, girişin yanındaki `CallStream` noktaları tıklatın ve ardından **girişten Örnek verileri**seçin.

   ![Akış Analizi iş girişi için örnek verileri kullanmak için menü seçenekleri, "Girişten örnek veri" seçili](./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-sample-data-from-input.png)


5. **Dakikaları** 3'e ayarla ve ardından **Tamam'ı**tıklatın. 
    
   ![3 dakika seçili giriş akışı örnekleme seçenekleri](./media/stream-analytics-real-time-fraud-detection/stream-analytics-input-create-sample-data.png)

    Azure, giriş akışından 3 dakikalık veri örnekleri ve örnek veriler hazır olduğunda sizi size haber verirken. (Bu kısa bir süre alır.) 

Örnek veriler geçici olarak depolanır ve sorgu penceresi açıkken kullanılabilir. Sorgu penceresini kapatırsanız örnek veriler atılır ve yeni bir örnek veri kümesi oluşturmanız gerekir. 

Alternatif olarak, [GitHub'dan](https://github.com/Azure/azure-stream-analytics/blob/master/Sample%20Data/telco.json)örnek veri içeren bir .json dosyası alabilir ve giriş için örnek veri olarak `CallStream` kullanmak üzere .json dosyasını yükleyebilirsiniz. 

### <a name="test-using-a-pass-through-query"></a>Geçiş sorgusu kullanarak test

Her olayı arşivlemek istiyorsanız, olayın yükündeki tüm alanları okumak için geçiş sorgusu kullanabilirsiniz.

1. Sorgu penceresinde, bu sorguyu girin:
        
   ```SQL
   SELECT 
       *
   FROM 
       CallStream
   ```

    >[!NOTE]
    >SQL'de olduğu gibi, anahtar kelimeler büyük/küçük harf duyarlı değildir ve beyaz alan önemli değildir.

    Bu sorguda, `CallStream` girişi oluşturduğunuzda belirttiğiniz diğer ad dır. Farklı bir takma ad kullandıysanız, bunun yerine bu adı kullanın.

2. **Test'i**tıklatın.

    Akış Analizi işi sorguyu örnek verilere karşı çalıştırıyor ve çıktıyı pencerenin alt kısmında görüntüler. Sonuçlar, Olay Hub'ı ve Akış Analizi işinin doğru şekilde yapılandırılmış olduğunu gösterir. (Belirtildiği gibi, daha sonra sorgunun veri yazabileceği bir çıktı lavabosu oluşturursunuz.)

   ![Oluşturulan 73 kaydı gösteren Stream Analytics iş çıktısı](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output.png)

    Gördüğünüz kayıtların tam sayısı, 3 dakikalık örneğinizde kaç kayıt yakalandığına bağlıdır.
 
### <a name="reduce-the-number-of-fields-using-a-column-projection"></a>Sütun projeksiyonu kullanarak alan sayısını azaltın

Çoğu durumda, çözümlemenizin giriş akışındaki tüm sütunlara ihtiyacı yoktur. Bir sorguyu, geçiş sorgusundan daha küçük bir döndürülen alan kümesini yansıtmak için kullanabilirsiniz.

1. Kod düzenleyicisindeki sorguyu aşağıdakiyle değiştirin:

    ```SQL
    SELECT CallRecTime, SwitchNum, CallingIMSI, CallingNum, CalledNum 
    FROM 
        CallStream
    ```

2. **Tekrar Test'i** tıklatın. 

   ![Projeksiyon için Stream Analytics iş çıktısı 25 kayıt gösterir](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-projection.png)
 
### <a name="count-incoming-calls-by-region-tumbling-window-with-aggregation"></a>Bölgeye göre gelen aramaları say: Toplama ile takla atan pencere

Bölge başına gelen arama sayısını saymak istediğinizi varsayalım. Veri akışında, sayma gibi toplu işlevleri gerçekleştirmek istediğinizde, akışı zamansal birimlere bölmeniz gerekir (veri akışının kendisi etkin bir şekilde sonsuz olduğundan). Bunu Akış Analitiği [pencere işlevini](stream-analytics-window-functions.md)kullanarak yaparsınız. Daha sonra bir birim olarak bu pencerenin içindeki verilerle çalışabilirsiniz.

Bu dönüşüm için, çakışan bir zamansal pencere dizisi istiyorsunuz- her pencerede gruplayabildiğiniz ve tokarta alabileceğiniz ayrı bir veri kümesi olacaktır. Bu pencere türüne *Yuvarlanan pencere*denir. Yuvarlanma penceresinde, çağrının geldiği ülkeyi/bölgeyi temsil `SwitchNum`eden gruplanmış gelen çağrıların sayısını alabilirsiniz. 

1. Kod düzenleyicisindeki sorguyu aşağıdakiyle değiştirin:

    ```SQL
    SELECT 
        System.Timestamp as WindowEnd, SwitchNum, COUNT(*) as CallCount 
    FROM
        CallStream TIMESTAMP BY CallRecTime 
    GROUP BY TUMBLINGWINDOW(s, 5), SwitchNum
    ```

    Bu sorgu, `Timestamp By` giriş akışında Takla penceresini tanımlamak için hangi zaman damgası alanını kullanacağını belirtmek için `FROM` yan tümcedeki anahtar sözcüğü kullanır. Bu durumda, pencere verileri her kayıttaki alana `CallRecTime` göre bölümlere böler. (Alan belirtilmemişse, pencereleme işlemi her olayın olay merkezine vardığı zamanı kullanır. [Stream Analytics Query Language Reference'da](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)"Varış Saatine Göre Uygulama Süresine Karşı" bölümüne bakın. 

    Projeksiyon, `System.Timestamp`her pencerenin sonu için bir zaman damgası döndürür içerir. 

    Yuvarlanan pencereyi kullanmak istediğinizi belirtmek için, `GROUP BY` yan tümcedeki [TUMBLINGWINDOW](https://docs.microsoft.com/stream-analytics-query/tumbling-window-azure-stream-analytics) işlevini kullanırsınız. İşlevde, bir zaman birimi (mikrosaniyeden bir güne kadar herhangi bir yerde) ve bir pencere boyutu (kaç birim) belirtirsiniz. Bu örnekte, Yuvarlanma penceresi 5 saniyelik aralıklardan oluşur, böylece her 5 saniyelik aramalar için ülkeye/bölgeye göre bir sayım alırsınız.

2. **Tekrar Test'i** tıklatın. Sonuçlarda, **WindowEnd** altındaki zaman damgalarının 5 saniyelik artışlarla olduğunu unutmayın.

   ![13 kayıt gösteren toplama için Akış Analizi iş çıktısı](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-aggregation.png)
 
### <a name="detect-sim-fraud-using-a-self-join"></a>Kendi kendine birleşin kullanarak SIM sahtekarlığını algılama

Bu örnekte, sahte kullanımı, aynı kullanıcıdan gelen ancak birbirinden 5 saniye içinde farklı konumlarda bulunan aramalar olarak düşünün. Örneğin, bir kullanıcı mantıksal olarak aynı anda hem ABD’den hem de Avustralya’dan arama yapamaz. 

Bu durumları denetlemek için, akış verisinin kendi kendine birleştirilmesini kullanarak akışı `CallRecTime` kendi başına birleştirebilirsiniz. Daha sonra, değerin `CallingIMSI` (kaynak numarası) aynı olduğu, ancak değerin `SwitchNum` (menşe ülke/bölge) aynı olmadığı arama kayıtlarını arayabilirsiniz.

Akış verileriyle birleştirilmiş birbirleştirme kullandığınızda, birleştirme, eşleşen satırların zaman içinde ne kadar ayrılabildiğine ilişkin bazı sınırlar sağlamalıdır. (Daha önce belirtildiği gibi, akış verileri etkili bir şekilde sonsuzdur.) İlişki için zaman `ON` `DATEDIFF` sınırları, işlevi kullanarak birleştirme yan tümcesi içinde belirtilir. Bu durumda, birleştirme çağrı verilerinin 5 saniyelik aralığını temel alar.

1. Kod düzenleyicisindeki sorguyu aşağıdakiyle değiştirin: 

    ```SQL
    SELECT  System.Timestamp as Time, 
        CS1.CallingIMSI, 
        CS1.CallingNum as CallingNum1, 
        CS2.CallingNum as CallingNum2, 
        CS1.SwitchNum as Switch1, 
        CS2.SwitchNum as Switch2 
    FROM CallStream CS1 TIMESTAMP BY CallRecTime 
        JOIN CallStream CS2 TIMESTAMP BY CallRecTime 
        ON CS1.CallingIMSI = CS2.CallingIMSI 
        AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5 
    WHERE CS1.SwitchNum != CS2.SwitchNum
    ```

    Bu sorgu, birleştirme işlevi dışında `DATEDIFF` herhangi bir SQL birleştirme gibidir. Bu sürüm `DATEDIFF` Akış Analizi'ne özgüdür ve `ON...BETWEEN` yan tümcede görünmesi gerekir. Parametreler bir zaman birimi (bu örnekte saniye) ve birleştirme için iki kaynağın diğer adlarıdır. Bu, standart SQL `DATEDIFF` işlevinden farklıdır.

    Yan `WHERE` tümce, sahte aramayı işaretleyen koşulu içerir: kaynak anahtarları aynı değildir. 

2. **Tekrar Test'i** tıklatın. 

   ![Kendi kendine katılım için Akış Analizi iş çıktısı, oluşturulan 6 kaydı gösteren](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-sample-output-self-join.png)

3. Akış Analizi işinin bir parçası olarak kendi kendine katılma sorgusunu kaydetmek için **Kaydet'i** tıklatın. (Örnek verileri kaydetmez.)

    <br/><img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-query-editor-save-button-new-portal.png" alt="Save Stream Analytics query in portal" width="300px"/>

## <a name="create-an-output-sink-to-store-transformed-data"></a>Dönüştürülen verileri depolamak için çıktı lavabosu oluşturma

Olayları yutmak için bir olay akışı, olay merkezi girişi ve akış üzerinde dönüşüm gerçekleştirmek için bir sorgu tanımladınız. Son adım, iş için bir çıktı lavabotanımlamak için-yani, dönüştürülmüş akışı yazmak için bir yer. 

Sql Server veritabanı, tablo depolama, Veri Gölü depolama, Güç BI ve hatta başka bir olay hub'ı gibi birçok kaynağı çıktı lavabosu olarak kullanabilirsiniz. Bu öğretici için akışı, yapılandırılmamış verileri barındırdığı için daha sonraki analizler için olay bilgilerini toplamak için tipik bir seçenek olan Azure Blob Depolama'ya yazarsınız.

Varolan bir blob depolama hesabınız varsa, bunu kullanabilirsiniz. Bu öğretici için, yeni bir depolama hesabı oluşturmayı öğreneceksiniz.

### <a name="create-an-azure-blob-storage-account"></a>Azure Blob Depolama hesabı oluşturma

1. Azure portalının sol üst köşesinden > kaynak**Depolama Deposu** > **hesabı** **oluştur'u**seçin. Depolama hesabı iş sayfasını **Ad** "asaehstorage", **Yer** "Doğu ABD", **Kaynak grubu** "asa-eh-ns-rg" olarak ayarlanmış olarak ayarlı olarak doldurun (depolama hesabını artan performans için Akış işiyle aynı kaynak grubunda barındırın). Diğer ayarlar varsayılan değerlerinde bırakılabilir.  

   ![Azure portalında depolama hesabı oluşturma](./media/stream-analytics-real-time-fraud-detection/stream-analytics-storage-account-create.png)

2. Azure portalında, Akış Analizi iş bölmesine geri dönün. (Bölmeyi kapattıysanız, Tüm `asa_frauddetection_job_demo` **kaynaklar** bölmesinde arama yapın.)

3. İş **Topolojisi** bölümünde **Çıktı** kutusunu tıklatın.

4. **Çıktılar** bölmesinde **Ekle'yi** tıklatın ve **Blob depolama alanını**seçin. Ardından Yeni çıktı sayfasını aşağıdaki bilgilerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Çıktı diğer adı  |  CallStream-FraudulentCalls   |  İş çıktısını tanımlamak için bir ad girin.   |
   |Abonelik   |  \<Aboneliğiniz\> |  Oluşturduğunuz depolama hesabını içeren Azure aboneliğini seçin. Depolama hesabı, aynı veya farklı bir abonelikte olabilir. Bu örnekte, aynı abonelikte depolama hesabı oluşturduğunuz varsayılır. |
   |Depolama hesabı  |  asaehstorage |  Oluşturduğunuz depolama hesabının adını girin. |
   |Kapsayıcı  | asa-fraudulentcalls-demo | Yeni Oluştur'u seçin ve bir kapsayıcı adı girin. |

    <br/>
    <img src="./media/stream-analytics-real-time-fraud-detection/stream-analytics-create-output-blob-storage-new-console.png" alt="Create blob output for Stream Analytics job" width="300px"/>
    
5. **Kaydet**'e tıklayın. 


## <a name="start-the-streaming-analytics-job"></a>Akış Analizi işini başlatın

İş şimdi yapılandırıldı. Bir giriş (olay hub'ı), bir dönüşüm (sahte aramaları aramak için sorgu) ve bir çıktı (blob depolama) belirttiniz. Artık işe başlayabilirsin. 

1. TelcoGenerator uygulamasının çalıştığından emin olun.

2. İş bölmesinde **Başlat'ı**tıklatın. İş çıktısı başlangıç zamanı için **Başlat iş** bölmesinde **Şimdi'i**seçin. 

   ![Akış Analizi işini başlatın](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-start.png)



## <a name="examine-the-transformed-data"></a>Dönüştürülen verileri inceleme

Artık tam bir Akış Analizi işiniz var. İş, bir telefon görüşmesi meta veri akışını incelemek, gerçek zamanlı olarak sahte telefon görüşmeleri aramak ve depolamaya yapılan sahte aramalar hakkında bilgi yazmaktır. 

Bu öğreticiyi tamamlamak için, Akış Analizi işi tarafından yakalanan verilere bakmak isteyebilirsiniz. Veriler, Azure Blog Depolama'ya yığınlar halinde (dosyalar) yazılıyor. Azure Blob Depolama'yı okuyan herhangi bir aracı kullanabilirsiniz. Önkoşullar bölümünde belirtildiği gibi, Visual Studio'da Azure uzantılarını kullanabilir veya Azure [Depolama Gezgini](https://storageexplorer.com/) veya [Cerulean](https://www.cerebrata.com/products/cerulean/features/azure-storage)gibi bir aracı kullanabilirsiniz. 

Blob depolama bir dosyanın içeriğini incelediğinızda, aşağıdaki gibi bir şey görürsünüz:

   ![Streaming Analytics çıktısı ile Azure blob depolama](./media/stream-analytics-real-time-fraud-detection/stream-analytics-sa-job-blob-storage-view.png)
 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Dolandırıcılık algılama senaryosuyla devam eden ve bu öğreticide oluşturduğunuz kaynakları kullanan ek makaleler vardır. Devam etmek istiyorsanız, **Sonraki adımlar**altındaki önerilere bakın.

Ancak, bittiyseniz ve oluşturduğunuz kaynaklara ihtiyacınız yoksa, gereksiz Azure ücretlerine maruz kalmamanız için bunları silebilirsiniz. Bu durumda, aşağıdakileri yapmanızı öneririz:

1. Akış Analizi işini durdurun. **İşler** bölmesinde, en üstte **Durdur'u** tıklatın.
2. Telco Generator uygulamasını durdurun. Uygulamayı başlattığınız komut penceresinde Ctrl+C tuşuna basın.
3. Sadece bu öğretici için yeni bir blob depolama hesabı oluşturduysanız, silin. 
4. Akış Analizi işini silin.
5. Olay merkezini silin.
6. Olay merkezi ad alanını silin.

## <a name="get-support"></a>Destek alın

Daha fazla yardım için [Azure Akışı Analizi forumuna](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)katılın.

## <a name="next-steps"></a>Sonraki adımlar

Aşağıdaki makale ile bu öğretici devam edebilirsiniz:

* [Akış Analizi ve Güç BI: Veri akışı için gerçek zamanlı analiz panosu.](stream-analytics-power-bi-dashboard.md) Bu makalede, Stream Analytics işinin TelCo çıktısını gerçek zamanlı görselleştirme ve analiz için Power BI'ye nasıl göndereceğiniz gösterilmektedir.

Genel olarak Stream Analytics hakkında daha fazla bilgi için şu makalelere bakın:

* [Azure Akış Analizine Giriş](stream-analytics-introduction.md)
* [Azure Akış Analizi işlerini ölçeklendirme](stream-analytics-scale-jobs.md)
* [Azure Akış Analizi Sorgu Dili Başvurusu](https://docs.microsoft.com/stream-analytics-query/stream-analytics-query-language-reference)
* [Azure Akış Analizi Yönetimi REST API'si Başvurusu](https://msdn.microsoft.com/library/azure/dn835031.aspx)
