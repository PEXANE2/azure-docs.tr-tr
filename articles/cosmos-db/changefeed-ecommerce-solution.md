---
title: Gerçek zamanlı veri analizlerini görselleştirmek için Azure Cosmos DB değişiklik akışını kullanın
description: Bu makalede, Kullanıcı düzenlerini anlamak, gerçek zamanlı veri analizi ve görselleştirme gerçekleştirmek için bir perakende şirketi tarafından değişiklik akışını nasıl kullanabileceğiniz açıklanır
author: SnehaGunda
ms.service: cosmos-db
ms.devlang: java
ms.topic: how-to
ms.date: 05/28/2019
ms.author: sngun
ms.custom: devx-track-java
ms.openlocfilehash: c9abc4dc89651eec7df635fb415314b2c12da3a6
ms.sourcegitcommit: a76ff927bd57d2fcc122fa36f7cb21eb22154cfa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87319771"
---
# <a name="use-azure-cosmos-db-change-feed-to-visualize-real-time-data-analytics"></a>Gerçek zamanlı veri analizlerini görselleştirmek için Azure Cosmos DB değişiklik akışını kullanın

Azure Cosmos DB değişiklik akışı, bu kayıtlar oluşturulduğu veya değiştirildiği için Azure Cosmos kapsayıcısından sürekli ve artımlı bir kayıt akışı almanın bir mekanizmadır. Değişiklik akışı desteği, herhangi bir değişiklik için kapsayıcıyı dinleyerek işe yarar. Ardından çıkış olarak, değiştirilen belgelerin değiştirilme zamanına göre sıralandığı bir belge listesi oluşturur. Değişiklik akışı hakkında daha fazla bilgi için bkz. [değişiklik akışı ile çalışma](change-feed.md) makalesi. 

Bu makalede, Kullanıcı düzenlerini anlamak, gerçek zamanlı veri analizi ve görselleştirme gerçekleştirmek için bir e-ticaret şirketi tarafından değişiklik akışını nasıl kullanabileceğiniz açıklanır. Bir öğeyi görüntüleyen Kullanıcı, sepetine bir öğe ekleme veya bir öğe satın alma gibi olayları analiz edersiniz. Bu olaylardan biri gerçekleştiğinde, yeni bir kayıt oluşturulur ve değişiklik akışı bu kaydı kaydeder. Daha sonra akışı Değiştir, şirket performansını ve etkinliğini çözümleyen ölçüm görselleştirmesine neden olan bir dizi adımı tetikler. Görselleştirebileceğiniz örnek ölçümler, gelir, benzersiz site ziyaretçileri, en popüler öğeler ve görüntülenen öğelerin, satın alınan bir sepete dahil edilen ortalama fiyatını içerir. Bu örnek ölçümler, bir e-ticaret şirketinin site popülerliğini değerlendirmesini, reklam ve fiyatlandırma stratejilerini geliştirmesini ve yatırım yapılacak envanterle ilgili kararlar almasına yardımcı olabilir.

Çalışmaya başlamadan önce çözüm hakkında bir video izlemeye ilgi, aşağıdaki videoya bakın:

> [!VIDEO https://www.youtube.com/embed/AYOiMkvxlzo]
>

## <a name="solution-components"></a>Çözüm bileşenleri
Aşağıdaki diyagram, çözüme dahil olan veri akışını ve bileşenlerini temsil eder:

:::image type="content" source="./media/changefeed-ecommerce-solution/project-visual.png" alt-text="Proje görseli" border="false":::
 
1. **Veri oluşturma:** Veri simülatörü, bir kullanıcının bir öğeyi görüntüleme, sepetine bir öğe ekleme ve bir öğe satın alma gibi olayları temsil eden perakende verileri oluşturmak için kullanılır. Veri oluşturucuyu kullanarak büyük örnek veri kümesi oluşturabilirsiniz. Oluşturulan örnek veriler aşağıdaki biçimdeki belgeleri içerir:
   
   ```json
   {      
     "CartID": 2486,
     "Action": "Viewed",
     "Item": "Women's Denim Jacket",
     "Price": 31.99
   }
   ```

2. **Cosmos DB:** Oluşturulan veriler bir Azure Cosmos kapsayıcısında depolanır.  

3. **Değişiklik akışı:** Değişiklik akışı, Azure Cosmos kapsayıcısındaki değişiklikleri dinler. Koleksiyona yeni bir belge eklendiğinde (bir olay, bir öğeyi görüntüleyen bir olay oluştuğunda, sepetine bir öğe ekliyor veya bir öğe satın alırken), değişiklik akışı bir [Azure işlevi](../azure-functions/functions-overview.md)tetikleyecektir.  

4. **Azure işlevi:** Azure Işlevi, yeni verileri işler ve bunu bir [Azure Olay Hub 'ına](../event-hubs/event-hubs-about.md)gönderir.  

5. **Olay Hub 'ı:** Azure Olay Hub 'ı bu olayları depolar ve daha fazla analiz yapmak için bunları [Azure Stream Analytics](../stream-analytics/stream-analytics-introduction.md) gönderir.  

6. **Azure Stream Analytics:** Azure Stream Analytics olayları işlemek ve gerçek zamanlı veri analizi gerçekleştirmek için sorguları tanımlar. Bu veriler daha sonra [Microsoft Power BI](https://docs.microsoft.com/power-bi/desktop-what-is-desktop)'e gönderilir.  

7. **Power BI:** Power BI, Azure Stream Analytics tarafından gönderilen verileri görselleştirmek için kullanılır. Ölçümlerin gerçek zamanlı olarak nasıl değişmediklerini görmek için bir pano oluşturabilirsiniz.  

## <a name="prerequisites"></a>Önkoşullar

* Microsoft .NET Framework 4.7.1 veya üzeri

* Microsoft .NET Core 2,1 (veya üzeri)

* Evrensel Windows Platformu geliştirme, .NET masaüstü geliştirme ve ASP.NET ve Web geliştirme iş yükleriyle Visual Studio

* Microsoft Azure aboneliği

* Microsoft Power BI hesabı

* [Azure Cosmos DB değişiklik akışı laboratuvarını](https://github.com/Azure-Samples/azure-cosmos-db-change-feed-dotnet-retail-sample) GitHub 'dan indirin. 

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma 

Çözüm için gereken Azure kaynakları-Azure Cosmos DB, depolama hesabı, Olay Hub 'ı Stream Analytics oluşturun. Bu kaynakları bir Azure Resource Manager şablonuyla dağıtacaksınız. Bu kaynakları dağıtmak için aşağıdaki adımları kullanın: 

1. Windows PowerShell yürütme ilkesini **Kısıtlanmamış**olarak ayarlayın. Bunu yapmak için, **Windows PowerShell 'ı yönetici olarak** açın ve aşağıdaki komutları çalıştırın:

   ```powershell
   Get-ExecutionPolicy
   Set-ExecutionPolicy Unrestricted 
   ```

2. Önceki adımda indirdiğiniz GitHub deposundan **Azure Resource Manager** klasörüne gidin ve dosya **üzerindeparameters.js** adlı dosyayı açın.  

3. Dosyadaki **parameters.js** gösterildiği gibi cosmosdbaccount_name, eventhubnamespace_name, storageaccount_name, parametreleri için değerler sağlayın. Kaynaklarınızın her birine verdiğiniz adları daha sonra kullanmanız gerekir.  

4. **Windows PowerShell**'de **Azure Resource Manager** klasöre gidin ve aşağıdaki komutu çalıştırın:

   ```powershell
   .\deploy.ps1
   ```
5. İstendiğinde, Azure **ABONELIK kimliğinizi**, kaynak grubu adı için **changefeedlab** ve dağıtım adı için **Run1** girin. Kaynaklar dağıtmaya başladıktan sonra, tamamlanması 10 dakika kadar sürebilir.

## <a name="create-a-database-and-the-collection"></a>Veritabanı ve koleksiyon oluşturma

Artık, e-ticaret sitesi olaylarını tutmak için bir koleksiyon oluşturacaksınız. Bir Kullanıcı bir öğeyi görüntülediğinde, sepetine bir öğe ekliyor veya bir öğe satın aldığında, koleksiyon, eylemi ("görüntülenen", "eklenen" veya "satın alınan"), ilgili öğenin adını, ilgili öğenin fiyatını ve dahil edilen Kullanıcı sepetinin KIMLIK numarasını içeren bir kayıt alır.

1. [Azure Portal](https://portal.azure.com/) gidin ve şablon dağıtımı tarafından oluşturulan **Azure Cosmos DB hesabı** bulun.  

2. **Veri Gezgini** bölmesinden **Yeni koleksiyon** ' ı seçin ve formu aşağıdaki ayrıntılarla doldurabilirsiniz:  

   * **Veritabanı kimliği** alanı Için **Yeni oluştur**' u seçin ve ardından **changefeedlabdatabase**yazın. **Veritabanı Işleme sağlama** kutusunu işaretlenmemiş olarak bırakın.  
   * **Koleksiyon** kimliği alanı için **changefeedlabcollection**yazın.  
   * **Bölüm anahtarı** alanı için **/Item**yazın. Bu, büyük/küçük harfe duyarlıdır, bu nedenle doğru girdiğinizden emin olun.  
   * **Verimlilik** alanı için **10000**girin.  
   * **Tamam** düğmesini seçin.  

3. Daha sonra değişiklik akışı işleme için **kiralamalar** adlı başka bir koleksiyon oluşturun. Kira koleksiyonu, değişiklik akışını birden çok çalışan genelinde işlemeye yönelik koordinatları. Ayrı bir koleksiyon, kiralamaları bölüm başına bir kira ile depolamak için kullanılır.  

4. **Veri Gezgini** bölmesine dönün ve **Yeni koleksiyon** ' ı seçin ve formu aşağıdaki ayrıntılarla doldurur:

   * **Veritabanı kimliği** alanı için **var olanı kullan**' ı seçin ve ardından **changefeedlabdatabase**yazın.  
   * **Koleksiyon kimliği** alanı için **kiralamalar**girin.  
   * **Depolama kapasitesi**için **sabit**' i seçin.  
   * **Üretilen iş** alanını varsayılan değerine ayarlayın.  
   * **Tamam** düğmesini seçin.

## <a name="get-the-connection-string-and-keys"></a>Bağlantı dizesini ve anahtarları al

### <a name="get-the-azure-cosmos-db-connection-string"></a>Azure Cosmos DB bağlantı dizesini al

1. [Azure Portal](https://portal.azure.com/) gidin ve şablon dağıtımı tarafından oluşturulan **Azure Cosmos DB hesabı** bulun.  

2. **Anahtarlar** bölmesine gıdın, BIRINCIL bağlantı dizesini kopyalayın ve laboratuvar genelinde erişiminizin olacağı bir not defteri veya başka bir belgeye kopyalayın. **Bağlantı dizesinin Cosmos DB**etiketlenmesi gerekir. Dizeyi daha sonra kodunuza kopyalamanız gerekir, bu nedenle bir göz atın ve nerede depolayabileceğinizi unutmayın.

### <a name="get-the-storage-account-key-and-connection-string"></a>Depolama hesabı anahtarını ve bağlantı dizesini al

Azure depolama hesapları kullanıcıların veri depolamasına izin verir. Bu laboratuvarda, Azure Işlevi tarafından kullanılan verileri depolamak için bir depolama hesabı kullanacaksınız. Azure Işlevi, koleksiyonda herhangi bir değişiklik yapıldığında tetiklenir.

1. Kaynak grubunuza dönün ve daha önce oluşturduğunuz depolama hesabını açın  

2. Sol taraftaki menüden **erişim tuşları** ' nı seçin.  

3. **Ana 1** altındaki değerleri bir not defteri veya laboratuvar genelinde erişiminizin olacağı başka bir belgeye kopyalayın. **Anahtarı** **depolama anahtarı** ve **bağlantı dizesi** olarak, **depolama bağlantı dizesi**olarak etiketlemelidir. Bu dizeleri daha sonra kodunuza kopyalamanız gerekir, bu nedenle bir göz atın ve bunları nerede depolayabileceğinizi unutmayın.  

### <a name="get-the-event-hub-namespace-connection-string"></a>Olay Hub 'ı ad alanı bağlantı dizesini al

Azure Olay Hub 'ı, olay verilerini alır, depolar, işler ve verileri iletir. Bu laboratuvarda, Azure Olay Hub 'ı her yeni olay meydana geldiğinde bir belge alır (yani bir öğe bir kullanıcı tarafından görüntülenir, kullanıcının sepetine eklenir veya bir kullanıcı tarafından satın alınır) ve ardından belgeyi Azure Stream Analytics iletir.

1. Kaynak grubunuza dönün ve oluşturduğunuz ve ön laboratuvarda adlandırdığınız **Olay Hub 'ı ad alanını** açın.  

2. Sol taraftaki menüden **paylaşılan erişim ilkeleri** ' ni seçin.  

3. **RootManageSharedAccessKey**öğesini seçin. **Bağlantı dizesi-birincil anahtarı** , laboratuvar genelinde erişiminizin olacağı bir not defteri veya başka bir belgeye kopyalayın. Bu **Olay Hub 'ı ad alanı** bağlantı dizesinin etiketlenmesi gerekir. Dizeyi daha sonra kodunuza kopyalamanız gerekir, bu nedenle bir göz atın ve nerede depolayabileceğinizi unutmayın.

## <a name="set-up-azure-function-to-read-the-change-feed"></a>Değişiklik akışını okumak için Azure Işlevini ayarlama

Yeni bir belge oluşturulduğunda veya Cosmos kapsayıcısında geçerli bir belge değiştirildiğinde, değişiklik akışı otomatik olarak bu değiştirilen belgeyi koleksiyon değişikliklerinin geçmişine ekler. Şimdi değişiklik akışını işleyen bir Azure Işlevi derlemenize ve çalıştıracaksınız. Oluşturduğunuz koleksiyonda bir belge oluşturulduğunda veya değiştirildiğinde, Azure Işlevi değişiklik akışı tarafından tetiklenir. Ardından, Azure Işlevi değiştirilen belgeyi Olay Hub 'ına gönderir.

1. Cihazınızda Klonladığınız depoya dönün.  

2. **Changefeedlabsolution. sln** adlı dosyaya sağ tıklayın ve **Visual Studio ile aç**' ı seçin.  

3. Visual Studio 'da **local.settings.js** gidin. Ardından, daha önce kaydettiğiniz değerleri kullanarak boş olanları doldurmanız gerekir.  

4. **ChangeFeedProcessor.cs**adresine gidin. **Run** işlevinin parametrelerinde, aşağıdaki eylemleri gerçekleştirin:  

   * **Koleksıyon adınızın** metnini, koleksiyonunuzun adıyla değiştirin. Önceki yönergeleri izlediyseniz, koleksiyonunuzun adı changefeedlabcollection olur.  
   * **Kıra KOLEKSIYONU adınızın** metnini, kira koleksiyonlarınızın adıyla değiştirin. Önceki yönergeleri izlediyseniz, kira koleksiyonunuzun adı **kiralamalar**olur.  
   * Visual Studio 'nun sol tarafında yeşil okun solundaki başlangıç projesi kutusunun **Changefeedfunction**olduğunu doğrulayın.  
   * Programı çalıştırmak için sayfanın en üstünde **Başlat** ' ı seçin  
   * Konsol uygulaması "Iş Konağı başlatıldı" diyorsa işlevin çalıştığını doğrulayabilirsiniz.

## <a name="insert-data-into-azure-cosmos-db"></a>Azure Cosmos DB veri ekleme 

Değişiklik akışı 'nın bir e-ticaret sitesinde yeni eylemleri nasıl işleyene bakmak için, ürün kataloğundaki öğeleri görüntüleyen kullanıcıları, bu öğeleri yüzlerine ekleyerek ve yüzlerinde öğeleri satın alarak kullanıcılara benzetim yapmanız gerekir. Bu veriler rastgele ve bir eCommerce sitesindeki verilerin nasıl görüneceğine ilişkin bir amaç için.

1. Dosya Gezgini 'nde depoya geri gidin ve yeni bir Visual Studio penceresinde bir kez daha açmak için **Changefeedfunction. sln** öğesine sağ tıklayın.  

2. **App.config** dosyasına gidin. Bloğu içinde `<appSettings>` , daha önce aldığınız Azure Cosmos DB hesabınızın uç noktasını ve benzersiz **birincil anahtarını** ekleyin.  

3. **Koleksiyonu** ve **veritabanı** adlarını ekleyin. (Bu **Adların, farklı** bir şekilde ad olarak isimsiz olarak isimsiz olarak isimsiz olarak **isimsiz olarak** isimsiz olarak isimsiz bir şekilde

   :::image type="content" source="./media/changefeed-ecommerce-solution/update-connection-string.png" alt-text="Bağlantı dizelerini Güncelleştir":::
 
4. Değişiklikleri düzenlenen tüm dosyalar üzerinde kaydedin.  

5. Visual Studio 'nun sol tarafında yeşil okun solundaki **Başlangıç projesi** kutusunun **DataGenerator**olduğunu doğrulayın. Sonra, programı çalıştırmak için sayfanın en üstündeki **Başlat** ' ı seçin.  
 
6. Programın çalışmasını bekleyin. Yıldızlar verilerin geldiği anlamına gelir! Programı çalışır durumda tutun; büyük miktarda verinin toplanması önemlidir.  

7. [Azure Portal](https://portal.azure.com/) ' a ve ardından kaynak grubunuzdaki Cosmos DB hesaba giderek **Veri Gezgini**istiyorsanız, değişiklik yaptığınız rastgele verileri **changefeedlabcollection** içinde görürsünüz.
 
   :::image type="content" source="./media/changefeed-ecommerce-solution/data-generated-in-portal.png" alt-text="Portalda oluşturulan veriler":::

## <a name="set-up-a-stream-analytics-job"></a>Stream Analytics işini ayarlama

Azure Stream Analytics, akış verilerinin gerçek zamanlı işlemesi için tam olarak yönetilen bir bulut hizmetidir. Bu laboratuvarda, Olay Hub 'ından yeni olayları işlemek için Stream Analytics 'i kullanacaksınız (yani bir öğe görüntülenirken, bir sepete eklendiğinde veya satın alındığında), bu olayları gerçek zamanlı veri analizinde birleştirir ve görselleştirme için Power BI gönderebilirsiniz.

1. [Azure Portal](https://portal.azure.com/), kaynak grubunuza ve ardından **streamjob1** (ön laboratuvarda oluşturduğunuz Stream Analytics işi) bölümüne gidin.  

2. Aşağıda gösterildiği gibi **girdileri** seçin.  

   :::image type="content" source="./media/changefeed-ecommerce-solution/create-input.png" alt-text="Giriş oluştur":::

3. **+ Akış girişi Ekle**' yi seçin. Ardından açılan menüden **Olay Hub 'ını** seçin.  

4. Yeni giriş formunu aşağıdaki ayrıntılarla doldur:

   * **Giriş** diğer adı alanına **giriş**' i girin.  
   * **Aboneliklerinizden Olay Hub 'ı Seç**seçeneğini belirleyin.  
   * **Abonelik alanını aboneliğiniz** olarak ayarlayın.  
   * **Olay Hub 'ı ad** alanı alanına, ön laboratuvar sırasında oluşturduğunuz Olay Hub 'ı ad alanının adını girin.  
   * **Olay Hub 'ı adı** alanında, **var olanı kullan** seçeneğini belirleyin ve açılır menüden **Event-HUB1** öğesini seçin.  
   * **Olay Hub 'ı ilke** adı alanını varsayılan değerine ayarlı bırakın.  
   * **Olay serileştirme biçimini** **JSON**olarak bırakın.  
   * **Kodlama alanını** **UTF-8**olarak ayarlayın.  
   * **Olay sıkıştırma türü** alanı **none**olarak ayarlanmış kalsın.  
   * **Kaydet** düğmesini seçin.

5. Stream Analytics işi sayfasına dönün ve **çıktılar**' i seçin.  

6. **+ Ekle**'yi seçin. Sonra açılan menüden **Power BI** ' ı seçin.  

7. Ortalama fiyatı görselleştirmek üzere yeni bir Power BI çıkışı oluşturmak için aşağıdaki eylemleri gerçekleştirin:

   * **Çıkış diğer adı** alanında **averagepriceoutput**yazın.  
   * **Grup çalışma** alanı alanını, **yükleme çalışma alanları için yetki ver**olarak ayarlanmış olarak bırakın.  
   * **Veri kümesi adı** alanında **averageprice**girin.  
   * **Tablo adı** alanında **averageprice**girin.  
   * **Yetkilendir** düğmesini seçin, ardından Power BI bağlantıyı yetkilendirmek için yönergeleri izleyin.  
   * **Kaydet** düğmesini seçin.  

8. Ardından **streamjob1** adresine dönün ve **Sorguyu Düzenle**' yi seçin.

   :::image type="content" source="./media/changefeed-ecommerce-solution/edit-query.png" alt-text="Sorguyu Düzenle":::
 
9. Sorgu penceresine aşağıdaki sorguyu yapıştırın. **Ortalama fiyat** sorgusu, kullanıcılar tarafından görüntülenen tüm öğelerin ortalama fiyatını, kullanıcıların HTS öğelerine eklenen tüm öğelerin ortalama fiyatını ve kullanıcılar tarafından satın alınan tüm öğelerin ortalama fiyatını hesaplar. Bu ölçüm, e-ticaret şirketlerinin, ne kadar madde satacağına ve hangi envanterde yatırmaya karar vermesine yardımcı olabilir. Örneğin, görüntülenen öğelerin ortalama fiyatı satın alınan öğelerin ortalama fiyatından çok daha yüksekse, bir şirket envanterine daha ucuz öğeler eklemeyi tercih edebilir.

   ```sql
   /*AVERAGE PRICE*/      
   SELECT System.TimeStamp AS Time, Action, AVG(Price)  
    INTO averagePriceOutput  
    FROM input  
    GROUP BY Action, TumblingWindow(second,5) 
   ```
10. Sonra sol üst köşedeki **Kaydet** ' i seçin.  

11. Şimdi **streamjob1** sayfasına dönüp sayfanın üst kısmındaki **Başlat** düğmesini seçin. Azure Stream Analytics başlatılması birkaç dakika sürebilir, ancak sonunda "başlangıç" iken "çalışıyor" olarak değiştirilir.

## <a name="connect-to-power-bi"></a>Power BI'a bağlanma

Power BI, veri çözümlemek ve öngörü paylaşmak için kullanılan iş analizi araçlarından oluşan bir hizmettir. Çözümlenmiş verileri nasıl stratejik olarak görselleştirebileceğiniz hakkında harika bir örnektir.

1. Power BI oturum açın ve sayfanın sol tarafındaki menüyü açarak **çalışma alanım** ' a gidin.  

2. Sağ üst köşede **+ Oluştur** ' u seçin ve ardından Pano oluşturmak için **Pano** ' yı seçin.  

3. Sağ üst köşedeki **+ kutucuk Ekle** ' yi seçin.  

4. **Özel akış verileri**' ni seçin ve ardından **İleri** düğmesini seçin.  
 
5. **Veri kümelerinizde** **averageprice** ' i seçin ve ardından **İleri**' yi seçin.  

6. **Görselleştirme türü** alanında, açılan menüden **kümelenmiş çubuk grafik** ' i seçin. **Eksen**altında eylem ekleyin. Herhangi bir şey eklemeden **Göstergeyi** atlayın. Ardından, bir sonraki bölüm altında, **değer**olarak, **Ort**ekleyin. **İleri**' yi seçin, sonra grafiğinizi unvanın ve **Uygula**' yı seçin. Panonuzda yeni bir grafik görmeniz gerekir!  

7. Artık daha fazla ölçüm görselleştirmek istiyorsanız, **streamjob1** adresine dönüp aşağıdaki alanlarla üç çıkış daha oluşturabilirsiniz.

   a. **Çıkış diğer adı:** IncomingRevenueOutput, DataSet Name: ıncominggelir, tablo adı: ıncominggelirleri  
   b. **Çıkış diğer adı:** Top5Output, DataSet Name: TOP5, table name: TOP5  
   c. **Çıkış diğer adı:** UniqueVisitorCountOutput, DataSet Name: UniqueVisitorCount, table name: uniqueVisitorCount

   Ardından, **Sorguyu Düzenle** ' yi seçin ve aşağıdaki sorguları zaten yazdığınız bir **yukarıya** yapıştırın.

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
   
   Ilk 5 sorgu ilk 5 öğeyi, satın aldıkları sayı sayısına göre sıralanmış olarak hesaplar. Bu ölçüm, e-ticaret şirketlerinin hangi öğelerin en popüler olduğunu değerlendirmenize yardımcı olabilir ve şirketin reklam, fiyatlandırma ve stok kararlarını etkileyebilir.

   GELIR sorgusu, her dakikada satın alınan tüm öğelerin fiyatlarını toplayarak geliri hesaplar. Bu ölçüm, e-ticaret şirketlerinin mali performansını değerlendirmesini ve ayrıca günün en fazla gelirine katkıda bulunduğunu anlamalarına yardımcı olabilir. Bu, özellikle pazarlama açısından pazarlama açısından genel olarak şirket stratejisini etkileyebilir.

   BENZERSIZ ZIYARETÇILER sorgusu, benzersiz sepet KIMLIĞI ' ni algılayarak sitede kaç benzersiz ziyaretçi olduğunu her 5 saniyede bir hesaplar. Bu ölçüm, e-ticaret şirketlerinin site etkinliklerini değerlendirmesini ve daha fazla müşterinin nasıl elde alınacağını stratejik hale getirmenize yardımcı olabilir.

8. Artık bu veri kümeleri için de kutucuk ekleyebilirsiniz.

   * Ilk 5 için, eksen ve sayı olarak sayı olarak bir kümelenmiş sütun grafiği yapmak mantıklı olur.  
   * Gelir açısından, eksen olarak saat ve fiyat toplamı değeri olarak bir çizgi grafik yapmak mantıklı olur. Görüntülenecek zaman penceresi, mümkün olduğunca fazla bilgi teslim etmek için mümkün olan en büyük bir tarih olmalıdır.  
   * Benzersiz ziyaretçiler için, değer olarak benzersiz ziyaretçi sayısı ile kart görselleştirmesi yapmak mantıklı olur.

   Örnek Pano şu grafiklerle nasıl görünür:

   :::image type="content" source="./media/changefeed-ecommerce-solution/visualizations.png" alt-text="görüntüler":::

## <a name="optional-visualize-with-an-e-commerce-site"></a>İsteğe bağlı: bir E-ticaret sitesiyle görselleştirin

Artık gerçek bir e-ticaret sitesiyle bağlantı kurmak için yeni veri analizi aracınızı nasıl kullanabileceğinizi gözlemleyeceksiniz. E-ticaret sitesini derlemek için bir Azure Cosmos veritabanını kullanarak ürün kategorilerinin listesini (kadınlar, Erkek, Unisex), ürün kataloğunu ve en popüler öğelerin bir listesini depolayın.

1. [Azure Portal](https://portal.azure.com/), ardından **Cosmos DB hesabınıza**ve ardından **Veri Gezgini**' e gidin.  

   Değişiklik, sabit depolama kapasitesine sahip **changefeedlabdatabase**  -  **ürünlerinin** ve **kategorilerinin** altına iki koleksiyon ekleyin.

   Bölüm anahtarı olarak, **TopItems** ve **/item** adlı **changefeedlabdatabase** altına başka bir koleksiyon ekleyin.

2. TopItems **topItems** koleksiyonunu seçin ve **ölçek ve ayarlar** ' ın **altında,** TopItems her **30 saniyede bir** güncelleştirilir.

   :::image type="content" source="./media/changefeed-ecommerce-solution/time-to-live.png" alt-text="Yaşam süresi":::

3. **TopItems** koleksiyonunu en sık satın alınan öğelerle doldurmak için, **streamjob1** adresine gidin ve yeni bir **Çıkış**ekleyin. **Cosmos DB**seçin.

4. Gerekli alanları aşağıda gösterildiği gibi girin.

   :::image type="content" source="./media/changefeed-ecommerce-solution/cosmos-output.png" alt-text="Cosmos çıkışı":::
 
5. Laboratuvarın önceki bölümünde isteğe bağlı Ilk 5 sorgu eklediyseniz, 5A bölümüne ilerleyin. Aksi takdirde, bölüm 5b ' e geçin.

   5a. **Streamjob1**' de, **Sorguyu Düzenle** ' yi seçin ve aşağıdaki sorguyu en üstteki 5 sorgunun altına, ancak sorguların geri kalanında Azure Stream Analytics sorgu düzenleyicinize yapıştırın.

   ```sql
   SELECT arrayvalue.value.item AS Item, arrayvalue.value.price, arrayvalue.value.countEvents
   INTO topItems
   FROM arrayselect
   ```
   5b. **Streamjob1**' de, **Sorguyu Düzenle** ' yi seçin ve aşağıdaki sorguyu diğer tüm sorguların üzerine Azure Stream Analytics sorgu düzenleyicinize yapıştırın.

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

6. **EcommerceWebApp. sln** dosyasını açın ve **Çözüm Gezgini** **Web.config** dosyasına gidin.  

7. Bloğu içinde `<appSettings>` , daha önce, **URI 'nizi buraya** ve **birincil anahtarınızı**burada belirten, daha önce kaydettiğiniz **URI** ve **birincil anahtarı** ekleyin. Ardından **veritabanı adınızı** ve **koleksiyon adınızı** gösterildiği gibi ekleyin. (Bu adların, farklı şekilde ad olarak isimsiz olarak isimsiz olarak isimsiz olarak isimsiz olarak, **changefeedlabdatabase** ve **changefeed**

   **Ürün koleksiyonu adı**, **Kategoriler koleksiyonu adı**ve **en üst öğe koleksiyonu adını** gösterildiği gibi girin. (Bu adların, farklı bir şekilde ad olarak isimsiz olarak isimsiz olarak isimsiz olarak mı yoksa **, ürünler ve Topgıtems**  

8. Adresine gidin ve **EcommerceWebApp. sln** Içindeki **kullanıma al klasörünü** açın. Sonra bu klasörde **Web.config** dosyasını açın.  

9. Bloğu içinde `<appSettings>` , daha önce belirtilen yerlerde kaydettiğiniz **URI** ve **birincil anahtarı** ekleyin. Ardından **veritabanı adınızı** ve **koleksiyon adınızı** gösterildiği gibi ekleyin. (Bu adların, farklı şekilde ad olarak isimsiz olarak isimsiz olarak isimsiz olarak isimsiz olarak, **changefeedlabdatabase** ve **changefeed**  

10. Programı çalıştırmak için sayfanın en üstünde bulunan **Başlat** ' a basın.  

11. Artık e-ticaret sitesinde dolaşçalabilirsiniz. Bir öğeyi görüntülerken, sepetinize bir öğe eklediğinizde, sepetinizdeki bir öğenin miktarını değiştirdiğinizde veya bir öğe satın aldığınızda, bu olaylar Cosmos DB değişiklik akışından olay hub 'ına, ASA 'ya ve ardından Power BI geçirilir. Önemli web trafiği verileri oluşturmak ve e-ticaret sitesinde gerçekçi bir "etkin ürünler" kümesi sağlamak için DataGenerator 'ı çalıştırmaya devam etmenizi öneririz.

## <a name="delete-the-resources"></a>Kaynakları silme

Bu laboratuvar sırasında oluşturduğunuz kaynakları silmek için [Azure Portal](https://portal.azure.com/)kaynak grubuna gidin, ardından sayfanın en üstündeki menüden **kaynak grubunu sil** ' i seçin ve belirtilen yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar 
  
* Değişiklik akışı hakkında daha fazla bilgi için bkz. [Azure Cosmos DB değişiklik akışı desteğiyle çalışma](change-feed.md) 
