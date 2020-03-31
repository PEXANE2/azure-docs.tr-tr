---
title: Gerçek zamanlı veri analitiğini görselleştirmek için Azure Cosmos DB değiştirme akışını kullanma
description: Bu makalede, değişiklik akışı bir perakende şirketi tarafından kullanıcı desenlerini anlamak, gerçek zamanlı veri analizi ve görselleştirme yapmak için nasıl kullanılabileceğini açıklar
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: conceptual
ms.date: 05/28/2019
ms.author: sngun
ms.openlocfilehash: c0c1a28dc399d3f176f92e656621fec1bc92dbfc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76513518"
---
# <a name="use-azure-cosmos-db-change-feed-to-visualize-real-time-data-analytics"></a>Gerçek zamanlı veri analitiğini görselleştirmek için Azure Cosmos DB değiştirme akışını kullanma

Azure Cosmos DB değişiklik akışı, bu kayıtlar oluşturulurken veya değiştirilirken bir Azure Cosmos kapsayıcısından sürekli ve artımlı bir kayıt akışı elde etmek için bir mekanizmadır. Değişiklik için kapsayıcıyı dinleyerek özet akışı desteğini değiştirin çalışır. Ardından çıkış olarak, değiştirilen belgelerin değiştirilme zamanına göre sıralandığı bir belge listesi oluşturur. Değişiklik akışı hakkında daha fazla bilgi edinmek [için, değişiklik akışı makalesiyle çalışmaya](change-feed.md) bakın. 

Bu makalede, değişiklik akışı kullanıcı desenlerini anlamak, gerçek zamanlı veri analizi ve görselleştirme gerçekleştirmek için bir e-ticaret şirketi tarafından nasıl kullanılabileceğini açıklamaktadır. Bir kullanıcının bir öğeyi görüntülemesi, sepetine öğe ekleme veya bir öğe satın alma gibi olayları analiz erecektir. Bu olaylardan biri oluştuğunda, yeni bir kayıt oluşturulur ve değişiklik akışı bu kaydı kaydeder. Akış değişikliği, şirket performansını ve etkinliğini analiz eden ölçümlerin görselleştirilmesiyle sonuçlanan bir dizi adımı tetikler. Görselleştirebileceğiniz örnek ölçümler arasında gelir, benzersiz site ziyaretçileri, en popüler öğeler ve sepete eklenen ve satın alınan adedine eklenen öğelerin ortalama fiyatı yer almaktadır. Bu örnek ölçümler, bir e-ticaret şirketinin sitenin popülerliğini değerlendirmesine, reklam ve fiyatlandırma stratejilerini geliştirmesine ve hangi envantere yatırım yapacağına ilişkin kararlar alabilecek.

Başlamadan önce çözüm hakkında bir video izlemek isteyen, aşağıdaki videoyu görmek:

> [!VIDEO https://www.youtube.com/embed/AYOiMkvxlzo]
>

## <a name="solution-components"></a>Çözüm bileşenleri
Aşağıdaki diyagram, veri akışını ve çözümle ilgili bileşenleri temsil eder:

![Proje görsel](./media/changefeed-ecommerce-solution/project-visual.png)
 
1. **Veri Oluşturma:** Veri simülatörü, bir öğeyi görüntüleyen, sepetine öğe ekleyen ve bir öğeyi satın alma gibi olayları temsil eden perakende verileri oluşturmak için kullanılır. Veri oluşturucuyu kullanarak büyük örnek veri kümesi oluşturabilirsiniz. Oluşturulan örnek veriler aşağıdaki biçimde belgeler içerir:
   
   ```json
   {      
     "CartID": 2486,
     "Action": "Viewed",
     "Item": "Women's Denim Jacket",
     "Price": 31.99
   }
   ```

2. **Kozmos DB:** Oluşturulan veriler bir Azure Cosmos kapsayıcısında depolanır.  

3. **Akışı Değiştir:** Değişiklik akışı, Azure Cosmos kapsayıcıdaki değişiklikleri dinler. Koleksiyona her yeni belge eklendiğinde (örneğin, bir kullanıcı nın bir öğeyi görüntülemesi, sepetine öğe eklemesi veya bir öğe satın alma gibi bir olay meydana geldiğinde), değişiklik akışı bir [Azure İşlevi'ni](../azure-functions/functions-overview.md)tetikler.  

4. **Azure İşlevi:** Azure İşlevi yeni verileri işler ve bir [Azure Etkinlik Hub'ına](../event-hubs/event-hubs-about.md)gönderir.  

5. **Olay Merkezi:** Azure Etkinlik Hub'ı bu etkinlikleri depolar ve daha fazla analiz yapmak üzere [Azure Akış Analizi'ne](../stream-analytics/stream-analytics-introduction.md) gönderir.  

6. **Azure Akış Analizi:** Azure Akış Analizi, olayları işlemek ve gerçek zamanlı veri çözümlemesi gerçekleştirmek için sorguları tanımlar. Bu veriler daha sonra [Microsoft Power BI'ye](https://docs.microsoft.com/power-bi/desktop-what-is-desktop)gönderilir.  

7. **Güç BI:** Power BI, Azure Stream Analytics tarafından gönderilen verileri görselleştirmek için kullanılır. Ölçümlerin gerçek zamanlı olarak nasıl değiştiğini görmek için bir pano oluşturabilirsiniz.  

## <a name="prerequisites"></a>Ön koşullar

* Microsoft .NET Framework 4.7.1 veya üzeri

* Microsoft .NET Core 2.1 (veya üstü)

* Universal Windows Platform geliştirme, .NET masaüstü geliştirme ve ASP.NET ve web geliştirme iş yüklerine sahip Visual Studio

* Microsoft Azure Aboneliği

* Microsoft Power BI Hesabı

* Azure [Cosmos DB değişiklik besleme satını](https://github.com/Azure-Samples/azure-cosmos-db-change-feed-dotnet-retail-sample) GitHub'dan indirin. 

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma 

Azure kaynaklarını oluşturun - Azure Cosmos DB, Depolama hesabı, Etkinlik Hub'ı, Çözüm tarafından gerekli stream Analytics. Bu kaynakları bir Azure Kaynak Yöneticisi şablonu aracılığıyla dağıtmış olursunuz. Bu kaynakları dağıtmak için aşağıdaki adımları kullanın: 

1. Windows PowerShell yürütme ilkesini **Sınırsız**olarak ayarlayın. Bunu yapmak için **Windows PowerShell'i Yönetici olarak** açın ve aşağıdaki komutları çalıştırın:

   ```powershell
   Get-ExecutionPolicy
   Set-ExecutionPolicy Unrestricted 
   ```

2. Önceki adımda indirdiğiniz GitHub deposundan **Azure Kaynak Yöneticisi** klasörüne gidin ve **parameters.json** dosyası adlı dosyayı açın.  

3. **parametreler.json** dosyasında belirtildiği gibi cosmosdbaccount_name, eventhubnamespace_name, storageaccount_name, parametreler için değerler sağlayın. Kaynaklarınızın her birine verdiğiniz adları daha sonra kullanmanız gerekir.  

4. **Windows PowerShell'den** **Azure Kaynak Yöneticisi** klasörüne gidin ve aşağıdaki komutu çalıştırın:

   ```powershell
   .\deploy.ps1
   ```
5. İstendiğinde, kaynak grubu adı için Azure **Abonelik Kimliğinizi,** **changefeedlab'ınızı** ve dağıtım adı için **run1'i** girin. Kaynaklar dağıtmaya başladıktan sonra tamamlanması 10 dakika kadar sürebilir.

## <a name="create-a-database-and-the-collection"></a>Veritabanı ve koleksiyon oluşturma

Şimdi e-ticaret sitesi etkinlikleri tutmak için bir koleksiyon oluşturacaksınız. Bir kullanıcı bir öğeyi görüntülediğinde, sepetine bir öğe eklediğinde veya bir öğe satın aldığında, koleksiyon eylemi ("görüntülenen", "eklenen" veya "satın alınan"), ilgili öğenin adını, ilgili öğenin fiyatını ve kullanıcı sepetinin kimlik numarasını içeren bir kayıt alır Dahil.

1. Azure [portalına](https://portal.azure.com/) gidin ve şablon dağıtımı tarafından oluşturulan **Azure Cosmos DB Hesabını** bulun.  

2. Veri **Gezgini** bölmesinden **Yeni Koleksiyon'u** seçin ve formu aşağıdaki ayrıntılarla doldurun:  

   * Veritabanı **kimliği** alanı için yeni **oluştur'u**seçin ve **sonra changefeedlabdatabase**girin. Provision **veritabanı iş oluşturma** kutusunu işaretlenmemiş bırakın.  
   * **Tahsilat** kimliği alanı için **changefeedlabcollection'ı**girin.  
   * Bölüm **tuşu** alanı için **/Öğe**girin. Bu büyük/küçük harf duyarlıdır, bu nedenle doğru girdiğinizi unutmayın.  
   * İş **İbiş** alanı için **10000**girin.  
   * **Tamam** düğmesini seçin.  

3. Sonraki değişiklik akışı işleme için **kiralar** adlı başka bir koleksiyon oluşturun. Kira toplama, değişiklik akışınıbirden çok işçi arasında işlemeyi koordine eder. Ayrı bir koleksiyon, kiralamaları bölüm başına bir kira ile depolamak için kullanılır.  

4. **Veri Gezgini** bölmesine dönün ve **Yeni Koleksiyon'u** seçin ve formu aşağıdaki ayrıntılarla doldurun:

   * Veritabanı **kimliği** alanı için **varolan kullan'ı**seçin, ardından **changefeedlabdatabase**girin.  
   * Tahsilat **kimliği** alanı için **kiraları**girin.  
   * **Depolama kapasitesi**için **Sabit'i**seçin.  
   * Elde **etme** alanını varsayılan değerine bırak.  
   * **Tamam** düğmesini seçin.

## <a name="get-the-connection-string-and-keys"></a>Bağlantı dizesini ve tuşlarını alın

### <a name="get-the-azure-cosmos-db-connection-string"></a>Azure Cosmos DB bağlantı dizesini alın

1. Azure [portalına](https://portal.azure.com/) gidin ve şablon dağıtımı tarafından oluşturulan **Azure Cosmos DB Hesabını** bulun.  

2. **Keys** bölmesine gidin, BIRINCIL BAĞLANTI STRING'ini kopyalayın ve bunu laboratuvar genelinde erişebilecek bir not defterine veya başka bir belgeye kopyalayın. **Cosmos DB Bağlantı Dize**etiketli gerekir. Dizeyi daha sonra kodunuza kopyalamanız gerekir, bu nedenle bir not alın ve nerede depoladığınızı unutmayın.

### <a name="get-the-storage-account-key-and-connection-string"></a>Depolama hesabı anahtarını ve bağlantı dizesini alın

Azure Depolama Hesapları, kullanıcıların verileri depolamasına olanak tanır. Bu laboratuvarda, Azure İşlevi tarafından kullanılan verileri depolamak için bir depolama hesabı kullanırsınız. Azure İşlevi, koleksiyonda herhangi bir değişiklik yapıldığında tetiklenir.

1. Kaynak grubunuza dönün ve daha önce oluşturduğunuz depolama hesabını açın  

2. Sol taraftaki menüden **Access tuşlarını** seçin.  

3. **1 tuşu** altındaki değerleri bir not defterine veya laboratuar genelinde erişebilecek başka bir belgeye kopyalayın. **Key'i** Depolama **Anahtarı** ve **Bağlantı dizesini** **Depolama Bağlantı Dizesi**olarak etiketlemeniz gerekir. Bu dizeleri daha sonra kodunuza kopyalamanız gerekir, bu nedenle bir not alın ve bunları nerede depoladığınızı unutmayın.  

### <a name="get-the-event-hub-namespace-connection-string"></a>Olay merkezi ad alanı bağlantı dizesini alın

Bir Azure Etkinlik Hub'ı olay verilerini alır, verileri depolar, işler ve iletir. Bu laboratuvarda, Azure Etkinlik Hub'ı yeni bir olay her gerçekleştiğinde (örneğin, bir öğe kullanıcı tarafından görüntülenir, kullanıcının sepetine eklenir veya bir kullanıcı tarafından satın alınır) bir belge alır ve ardından bu belgeyi Azure Akış Analizi'ne iletir.

1. Kaynak grubunuza dönün ve ön laboratuarda oluşturduğunuz ve adını aldığınız **Olay Hub Adı Alanını** açın.  

2. Sol taraftaki menüden **Paylaşılan erişim ilkelerini** seçin.  

3. **RootManageSharedAccessKey'i**seçin. Bağlantı **dizebirincil anahtarını** bir not defterine veya laboratuar genelinde erişebilecek başka bir belgeye kopyalayın. Olay Hub **Ad Alanı** bağlantı dizesi etiketlemeniz gerekir. Dizeyi daha sonra kodunuza kopyalamanız gerekir, bu nedenle bir not alın ve nerede depoladığınızı unutmayın.

## <a name="set-up-azure-function-to-read-the-change-feed"></a>Değişiklik akışını okumak için Azure İşlevi'ni ayarlama

Yeni bir belge oluşturulduğunda veya geçerli bir belge Cosmos kapsayıcısında değiştirildiğinde, değişiklik akışı otomatik olarak değiştirilen belgeyi koleksiyon değişikliklerinin geçmişine ekler. Şimdi, değişiklik akışını işleyen bir Azure İşlevi oluşturup çalıştırabilirsiniz. Oluşturduğunuz koleksiyonda bir belge oluşturulduğunda veya değiştirildiğinde, Azure İşlevi değişiklik akışı tarafından tetiklenir. Ardından Azure İşlevi, değiştirilen belgeyi Olay Hub'ına gönderir.

1. Cihazınızda klonladığınız depoya geri dönün.  

2. **ChangeFeedLabSolution.sln** adlı dosyaya sağ tıklayın ve **Visual Studio ile Aç'ı**seçin.  

3. Visual **Studio'da local.settings.json'a** gidin. Ardından, boşlukları doldurmak için daha önce kaydettiğiniz değerleri kullanın.  

4. **ChangeFeedProcessor.cs**gidin. **Run** işlevinin parametrelerinde aşağıdaki eylemleri gerçekleştirin:  

   * **BURADA TOPLAMA ADI'ni** koleksiyonunuzun adı ile değiştirin. Daha önceki yönergeleri izlediyseniz, koleksiyonunuzun adı changefeedlabcollection'dır.  
   * **LEASES COLLECTION NAME HERE** metnini kira koleksiyonunuzun adı ile değiştirin. Daha önceki yönergeleri izlediyseniz, kira koleksiyonunuzun adı **kiradır.**  
   * Visual Studio'nun üst kısmında, yeşil okun solundaki Başlangıç Projesi kutusunun **ChangeFeedFunction**yazdığından emin olun.  
   * Programı çalıştırmak için sayfanın en üstünden **Başlat'ı** seçin  
   * Konsol uygulaması "İş ana bilgisayar başladı" dediğinde işlevin çalıştığını doğrulayabilirsiniz.

## <a name="insert-data-into-azure-cosmos-db"></a>Azure Cosmos DB'ye veri ekleme 

Değişiklik akışının bir e-ticaret sitesindeki yeni eylemleri nasıl işlediğini görmek için, kullanıcıların ürün kataloğundan öğeleri görüntülemesini, bu öğeleri sepetlerine eklemesini ve sepetlerindeki öğeleri satın almalarını temsil eden verileri simüle etmesi gerekir. Bu veriler rasgele ve bir E-ticaret sitesindeki verilerin nasıl görüneceğini çoğaltmak amacıyla dır.

1. Dosya Gezgini'ndeki depoya geri gidin ve yeni bir Visual Studio penceresinde yeniden açmak için **ChangeFeedFunction.sln'e** sağ tıklayın.  

2. **App.config** dosyasına gidin. `<appSettings>` Blok içinde, daha önce aldığınız Azure Cosmos DB hesabınızın bitiş noktasını ve benzersiz **PRIMARY KEY'i** ekleyin.  

3. **Koleksiyon** ve **veritabanı** adlarını ekleyin. (Sizinkine farklı isim vermeyi seçmediğiniz sürece bu adlar **changefeedlabcollection** ve **changefeedlabdatabase** olmalıdır.)

   ![Bağlantı dizelerini güncelleştirme](./media/changefeed-ecommerce-solution/update-connection-string.png)
 
4. Düzenlenen tüm dosyalardaki değişiklikleri kaydedin.  

5. Visual Studio üst kısmında, yeşil ok sol **daki Başlangıç Projesi** kutusu **DataGenerator**diyor emin olun. Ardından programı çalıştırmak için sayfanın en üstünden **Başlat'ı** seçin.  
 
6. Programın çalışmasını bekleyin. Yıldızlar, verilerin geldiği anlamına geliyor! Programı çalıştırmaya devam edin - çok sayıda verinin toplanması önemlidir.  

7. [Azure portalına](https://portal.azure.com/) , ardından kaynak grubunuzdaki Cosmos DB hesabına, sonra **data explorer'a,** **changefeedlabcollection'ınızda** alınan rasgele verileri görürsünüz.
 
   ![Portalda oluşturulan veriler](./media/changefeed-ecommerce-solution/data-generated-in-portal.png)

## <a name="set-up-a-stream-analytics-job"></a>Akış analizi işi ayarlama

Azure Akış Analizi, akış verilerinin gerçek zamanlı işlenmesi için tamamen yönetilen bir bulut hizmetidir. Bu laboratuvarda, Olay Hub'ından yeni olayları işlemek için akış analitiği (örneğin, bir öğe görüntülendiğinde, bir sepete eklendiğinde veya satın alındığında), bu olayları gerçek zamanlı veri analizine dahil etmek ve görselleştirme için Power BI'ye göndermek için kullanılacaktır.

1. Azure [portalından](https://portal.azure.com/)kaynak grubunuza, ardından **streamjob1'e** (prelab'da oluşturduğunuz akış analizi işi) gidin.  

2. Aşağıda gösterildiği gibi **Girişleri** seçin.  

   ![Giriş oluşturma](./media/changefeed-ecommerce-solution/create-input.png)

3. Select **+ Akış girişi ekle**. Ardından açılan menüden **Olay Hub'ını** seçin.  

4. Yeni giriş formunu aşağıdaki ayrıntılarla doldurun:

   * **Giriş** diğer adı alanına **giriş girin.**  
   * **Aboneliklerinizden Etkinlik Hub'ı Seçin**seçeneğini seçin.  
   * **Abonelik** alanını aboneliğinize ayarlayın.  
   * Olay **Hub ad alanı** alanına, ön laboratuar sırasında oluşturduğunuz Event Hub Ad Alanı'nın adını girin.  
   * Olay **Hub'ı adı** alanında, **varolan kullanım** seçeneğini seçin ve açılan menüden olay **hub1'i** seçin.  
   * **Olay Hub ilke** adı alanını varsayılan değerine bırakın.  
   * **Olay serileştirme biçimini** **JSON**olarak bırakın.  
   * **Kodlama alanını** **UTF-8'e**bırakın.  
   * **Olay sıkıştırma türü** alanını **Yok'a**bırakın.  
   * **Kaydet** düğmesini seçin.

5. Akış analizi iş sayfasına geri gidin ve **Çıktılar'ı**seçin.  

6. **+ Ekle** öğesini seçin. Ardından açılır menüden **Power BI'yi** seçin.  

7. Ortalama fiyatı görselleştirmek için yeni bir Power BI çıkışı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

   * Çıktı **diğer adı** alanında, **averagePriceOutput**girin.  
   * Çalışma **alanlarını** yüklemek için **bağlantıyı Yetkilendirmek için**Grup çalışma alanı kümesini bırakın.  
   * **Dataset ad** alanına **ortalamaFiyat**girin.  
   * Tablo **adı** alanına **ortalamaFiyat**girin.  
   * **Yetkilendirme** düğmesini seçin ve ardından Power BI bağlantısını yetkilendirmek için yönergeleri izleyin.  
   * **Kaydet** düğmesini seçin.  

8. Sonra **streamjob1'e** geri dönün ve **sorguyup düzenle'yi**seçin.

   ![Sorguyu düzenle](./media/changefeed-ecommerce-solution/edit-query.png)
 
9. Aşağıdaki sorguyu sorgu penceresine yapıştırın. **ORTALAMA Fİyat** sorgusu, kullanıcılar tarafından görüntülenen tüm öğelerin ortalama fiyatını, kullanıcıların sepetlerine eklenen tüm öğelerin ortalama fiyatını ve kullanıcılar tarafından satın alınan tüm öğelerin ortalama fiyatını hesaplar. Bu metrik, e-ticaret şirketlerinin hangi fiyatlarla ürün satacağına ve hangi envantere yatırım yapacaklarına karar vermelerine yardımcı olabilir. Örneğin, görüntülenen maddelerin ortalama fiyatı satın alınan maddelerin ortalama fiyatından çok daha yüksekse, bir şirket envanterine daha az pahalı maddeler eklemeyi seçebilir.

   ```sql
   /*AVERAGE PRICE*/      
   SELECT System.TimeStamp AS Time, Action, AVG(Price)  
    INTO averagePriceOutput  
    FROM input  
    GROUP BY Action, TumblingWindow(second,5) 
   ```
10. Ardından sol üst köşede **Kaydet'i** seçin.  

11. Şimdi **streamjob1'e** dönün ve sayfanın üst kısmındaki **Başlat** düğmesini seçin. Azure Akış Analizi'nin başlatılması birkaç dakika sürebilir, ancak sonunda "Başlangıç"tan "Çalıştırma"ya değiştiğini göreceksiniz.

## <a name="connect-to-power-bi"></a>Power BI'a bağlanma

Power BI, veri çözümlemek ve öngörü paylaşmak için kullanılan iş analizi araçlarından oluşan bir hizmettir. Analiz edilen verileri stratejik olarak nasıl görselleştirebileceğinizin harika bir örneğidir.

1. Power BI'de oturum açın ve sayfanın sol tarafındaki menüyü açarak **Çalışma Alanım'a** gidin.  

2. Sağ üst köşede **+ Oluştur'u** seçin ve ardından pano oluşturmak için **Pano'yu** seçin.  

3. Seç + Sağ üst köşede **kiremit ekleyin.**  

4. **Özel Akış Verileri'ni**seçin ve **ardından Sonraki** düğmesini seçin.  
 
5. **DATASETS'inizden** **ortalamaFiyat'ı** seçin ve **ardından İleri'yi**seçin.  

6. **Visualization Type** alanında, açılan menüden **Kümelenmiş çubuk grafiğini** seçin. **Axis**altında, eylem ekleyin. Hiçbir şey eklemeden **Legend'ı** atlayın. Daha sonra, **Değer**adlı bir sonraki bölümün altında, **avg**ekleyin. **İleri'yi**seçin, ardından grafiğinizi başlıklayın ve **Uygula'yı**seçin. Panonuzda yeni bir grafik görmelisiniz!  

7. Şimdi, daha fazla ölçüm görselleştirmek istiyorsanız, **streamjob1'e** geri dönüp aşağıdaki alanları içeren üç çıktı daha oluşturabilirsiniz.

   a. **Çıktı diğer adı:** gelenGelirÇıktı, Dataset adı: gelenGelir, Tablo adı: gelenGelir  
   b. **Çıktı diğer adı:** top5Output, Dataset adı: top5, Tablo adı: top5  
   c. **Çıktı adı:** uniqueVisitorCountOutput, Dataset adı: uniqueVisitorCount, Tablo adı: uniqueVisitorCount

   Ardından **sorguyu edit'i** seçin ve aşağıdaki sorguları daha önce yazdığınız sorgunun **üzerine** yapıştırın.

   ```sql
    /*TOP 5*/
    WITH Counter AS
    (
    SELECT Item, Price, Action, COUNT(*) AS countEvents
    FROM input
    WHERE Action = 'Purchased'
    GROUP BY Item, Price, Action, TumblingWindow(second,30)
    ), 
    top5 AS
    (
    SELECT DISTINCT
    CollectTop(5)  OVER (ORDER BY countEvents) AS topEvent
    FROM Counter
    GROUP BY TumblingWindow(second,30)
    ), 
    arrayselect AS 
    (
    SELECT arrayElement.ArrayValue
    FROM top5
    CROSS APPLY GetArrayElements(top5.topevent) AS arrayElement
    ) 
    SELECT arrayvalue.value.item, arrayvalue.value.price,   arrayvalue.value.countEvents
    INTO top5Output
    FROM arrayselect

    /*REVENUE*/
    SELECT System.TimeStamp AS Time, SUM(Price)
    INTO incomingRevenueOutput
    FROM input
    WHERE Action = 'Purchased'
    GROUP BY TumblingWindow(hour, 1)

    /*UNIQUE VISITORS*/
    SELECT System.TimeStamp AS Time, COUNT(DISTINCT CartID) as uniqueVisitors
    INTO uniqueVisitorCountOutput
    FROM input
    GROUP BY TumblingWindow(second, 5)
   ```
   
   TOP 5 sorgusu, satın alındıkları kaç sayıya göre sıralanan en iyi 5 öğeyi hesaplar. Bu metrik, e-ticaret şirketlerinin hangi öğelerin en popüler olduğunu değerlendirmelerine yardımcı olabilir ve şirketin reklam, fiyatlandırma ve stok kararlarını etkileyebilir.

   GELIR sorgusu, her dakika satın alınan tüm maddelerin fiyatlarını özetleyerek geliri hesaplar. Bu metrik, e-ticaret şirketlerinin finansal performansını değerlendirmelerine ve aynı zamanda günün hangi saatlerinde çoğu gelire katkıda bulunduklarının anlaşılmasına yardımcı olabilir. Bu genel şirket stratejisi, özellikle pazarlama etkileyebilir.

   TEKSTİl ZİYARETÇİler sorgusu, benzersiz sepet likimliği algılayarak sitede her 5 saniyede kaç tane tekil ziyaretçi olduğunu hesaplar. Bu metrik, e-ticaret şirketlerinin site faaliyetlerini değerlendirmelerine ve daha fazla müşteri edinme konusunda nasıl strateji oluşturmalarına yardımcı olabilir.

8. Artık bu veri kümeleri için de kutucukekleyebilirsiniz.

   * Top 5 için, eksen olarak öğeleri ve değer olarak sayım ile kümelenmiş bir sütun grafiği yapmak mantıklı olacaktır.  
   * Gelir için, eksen olarak zaman ve değer olarak fiyatların toplamı ile bir çizgi grafik yapmak mantıklı olacaktır. Görüntülemek için zaman penceresi mümkün olduğunca çok bilgi sunmak için mümkün olan en büyük olmalıdır.  
   * Benzersiz Ziyaretçiler için, değer olarak benzersiz ziyaretçi sayısı ile bir kart görselleştirme yapmak mantıklı olacaktır.

   Örnek pano bu grafiklerle şu şekilde görünür:

   ![görselleştirmeler](./media/changefeed-ecommerce-solution/visualizations.png)

## <a name="optional-visualize-with-an-e-commerce-site"></a>İsteğe bağlı: Bir E-ticaret sitesi ile görselleştirin

Şimdi gerçek bir e-ticaret sitesine bağlanmak için yeni veri analizi aracınızı nasıl kullanabileceğinizi gözlemleyeceksiniz. E-ticaret sitesini oluşturmak için, ürün kategorileri (Kadınlar, Erkekler, Unisex), ürün kataloğu ve en popüler öğelerin listesini depolamak için bir Azure Cosmos veritabanı kullanın.

1. [Azure portalına,](https://portal.azure.com/)ardından **Cosmos DB hesabınıza,** ardından **Data Explorer'a**gidin.  

   **Changefeedlabdatabase** - **ürünleri** ve sabit depolama kapasitesine sahip **kategoriler** altında iki koleksiyon ekleyin.

   **Changefeedlabdatabase** altında **topItems** ve **/Item** adlı başka bir koleksiyon ekleyin.

2. TopItems koleksiyonunu seçin ve **Ölçek ve Ayarlar'ın** **altında,** **topÖğelerin** her 30 saniyede bir güncelleşecek şekilde Yaşam Süresini **30 saniye** olarak ayarlayın.

   ![Yaşam süresi](./media/changefeed-ecommerce-solution/time-to-live.png)

3. **TopItems** koleksiyonunu en sık satın alınan öğelerle doldurmak için **streamjob1'e** geri gidin ve yeni bir **Çıktı**ekleyin. **Cosmos DB'yi**seçin.

4. Aşağıda resimde belirtildiği gibi gerekli alanları doldurun.

   ![Cosmos çıkışı](./media/changefeed-ecommerce-solution/cosmos-output.png)
 
5. İsteğe bağlı TOP 5 sorgusunu laboratuvarın önceki bölümüne eklediyseniz, bölüm 5a'ya devam edin. Değilse, bölüm 5b devam edin.

   5a. **Streamjob1'de** **sorguyu düzenle'yi** seçin ve Top 5 sorgusunun altında, ancak sorguların geri kalanının altında Azure Akış Analizi sorgu düzenleyicinizde aşağıdaki sorguyu yapıştırın.

   ```sql
   SELECT arrayvalue.value.item AS Item, arrayvalue.value.price, arrayvalue.value.countEvents
   INTO topItems
   FROM arrayselect
   ```
   5b. **streamjob1'de** **sorguyu düzenle'yi** seçin ve azure akışı analytics sorgu düzenleyicinizde diğer tüm sorguların üzerinde aşağıdaki sorguyu yapıştırın.

   ```sql
   /*TOP 5*/
   WITH Counter AS
   (
   SELECT Item, Price, Action, COUNT(*) AS countEvents
   FROM input
   WHERE Action = 'Purchased'
   GROUP BY Item, Price, Action, TumblingWindow(second,30)
   ), 
   top5 AS
   (
   SELECT DISTINCT
   CollectTop(5)  OVER (ORDER BY countEvents) AS topEvent
   FROM Counter
   GROUP BY TumblingWindow(second,30)
   ), 
   arrayselect AS 
   (
   SELECT arrayElement.ArrayValue
   FROM top5
   CROSS APPLY GetArrayElements(top5.topevent) AS arrayElement
   ) 
   SELECT arrayvalue.value.item AS Item, arrayvalue.value.price, arrayvalue.value.countEvents
   INTO topItems
   FROM arrayselect
   ```

6. **EcommerceWebApp.sln'yi** açın ve Solution **Explorer'daki Web.config** dosyasına gidin. **Solution Explorer**  

7. `<appSettings>` Blok içinde, daha önce kaydettiğiniz **URI** ve **BIRINCIL ANAHTAR'ı** ekleyin ve **burada URI'niz** ve **birincil anahtarınız burada**yazıyor. Ardından veritabanı **adınızı** ve **koleksiyon adınızı** belirtildiği gibi ekleyin. (Sizinkine farklı isim vermediğiniz sürece bu adlar **changefeedlabdatabase** ve **changefeedlabcollection** olmalıdır.)

   **Ürün koleksiyonu**adınızı, **kategorilerin koleksiyon adınızı**ve en çok kullanılan koleksiyon **adınızı** girin. (Bu adlar, sizinkine farklı isim vermediğiniz sürece **ürünler, kategoriler ve topÖğeler** olmalıdır.)  

8. **EcommerceWebApp.sln** içindeki **Ödeme klasörüne** gidin ve açın. Ardından bu klasörün içindeki **Web.config** dosyasını açın.  

9. Blok içinde, daha önce kaydettiğiniz **URI** ve BIRINCIL ANAHTAR'ı belirtilen yerlerde ekleyin. **PRIMARY KEY** `<appSettings>` Ardından veritabanı **adınızı** ve **koleksiyon adınızı** belirtildiği gibi ekleyin. (Sizinkine farklı isim vermediğiniz sürece bu adlar **changefeedlabdatabase** ve **changefeedlabcollection** olmalıdır.)  

10. Programı çalıştırmak için sayfanın en üstündeki **Başlat'a** basın.  

11. Artık e-ticaret sitesinde oynayabilirsiniz. Bir öğeyi görüntülediğinizde, sepetinize bir öğe eklediğinizde, sepetinizdeki bir öğenin miktarını değiştirdiğinizde veya bir öğe satın aldığınızda, bu olaylar Cosmos DB değişiklik akışından Olay Hub' ı, ASA ve ardından Power BI' ye aktarılır. Biz önemli web trafik verileri oluşturmak ve e-ticaret sitesinde "Sıcak Ürünler" gerçekçi bir dizi sağlamak için DataGenerator çalıştırmak için devam öneririz.

## <a name="delete-the-resources"></a>Kaynakları silme

Bu laboratuvar sırasında oluşturduğunuz kaynakları silmek için [Azure portalındaki](https://portal.azure.com/)kaynak grubuna gidin , ardından sayfanın üst kısmındaki menüden **kaynak grubunu sil'i** seçin ve sağlanan yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar 
  
* Değişiklik akışı hakkında daha fazla bilgi edinmek için [Azure Cosmos DB'de değişiklik akışı desteğiyle çalışmaya](change-feed.md) bakın 
