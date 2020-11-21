---
title: Öğretici-Azure portal kullanarak Stream Analytics işi oluşturma ve yönetme
description: Bu öğreticide, telefon araması akışındaki sahte aramaların analiz edilmesi için Azure Stream Analytics’in nasıl kullanılacağını gösteren kapsamlı bir örnek sunulmaktadır.
author: mamccrea
ms.author: mamccrea
ms.service: stream-analytics
ms.topic: tutorial
ms.custom: mvc
ms.date: 10/30/2020
ms.openlocfilehash: 70acc696f1cb366d25299f616744e52491a54471
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95024186"
---
# <a name="tutorial-analyze-phone-call-data-with-stream-analytics-and-visualize-results-in-power-bi-dashboard"></a>Öğretici: Stream Analytics ile telefon araması verilerini analiz edin ve sonuçları Power BI panoda görselleştirin

Bu öğreticide Azure Stream Analytics'i kullanarak telefon araması verilerini analiz etme adımları gösterilmektedir. Bir istemci uygulaması tarafından oluşturulan telefon araması verileri, Stream Analytics işi tarafından filtrelenecektir bazı sahte çağrılar içerir.

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:

> [!div class="checklist"]
> * Örnek telefon araması verileri oluşturma ve Azure Event Hubs'a gönderme
> * Akış Analizi işi oluşturma
> * İş girişini ve çıkışını yapılandırma
> * Sahte çağrıları filtrelemek için sorgu tanımlama
> * İşi test etme ve başlatma
> * Sonuçları Power BI’da görselleştirme

## <a name="prerequisites"></a>Önkoşullar

Başlamadan önce aşağıdaki işlemleri yapın:

* Azure aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/) oluşturun.
* [Azure portalında](https://portal.azure.com/) oturum açın.
* [TelcoGenerator.zip](https://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) telefon araması olay oluşturucu uygulamasını Microsoft İndirme Merkezi’nden indirin veya [GitHub](https://aka.ms/azure-stream-analytics-telcogenerator)’dan kaynak kodu edinin.
* Power BI hesabınız olmalıdır.

## <a name="create-an-azure-event-hub"></a>Azure Olay Hub’ı oluşturma

Stream Analytics’in sahte arama veri akışını analiz edebilmesi için verilerin Azure'a gönderilmesi gerekir. Bu öğreticide, [Azure Event Hub’ları](../event-hubs/event-hubs-about.md) kullanarak verileri Azure’a göndereceksiniz.

Bir olay hub'ı oluşturmak ve arama verilerini bu olay hub'ına göndermek için aşağıdaki adımları izleyin:

1. [Azure portalında](https://portal.azure.com/) oturum açın.
2. Event Hubs **nesnelerin interneti kaynak oluştur**' u seçin  >  **Internet of Things**  >  **Event Hubs**.

   ![Portalda bir Azure Olay Hub 'ı oluşturma](media/stream-analytics-manage-job/find-event-hub-resource.png)
3. **Ad Alanı Oluştur** bölmesini aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer** |**Açıklama**  |
   |---------|---------|---------|
   |Name     | asaTutorialEventHub        |  Olay hub'ı ad alanını tanımlamak için benzersiz bir ad.       |
   |Abonelik     |   \<Your subscription\>      |   Olay hub'ını oluşturmak istediğiniz Azure aboneliğini seçin.      |
   |Kaynak grubu     |   MyASADemoRG      |  **Yeni Oluştur**’u seçin ve hesabınız için yeni bir kaynak grubu adı girin.       |
   |Konum     |   Batı ABD 2      |    Olay hub'ı ad alanının dağıtılabildiği konum.     |

4. Kalan ayarlarda varsayılan seçenekleri kullanın ve **gözden geçir + oluştur**' u seçin. Ardından, dağıtımı başlatmak için **Oluştur** ' u seçin.

   ![Azure portal olay hub 'ı ad alanı oluşturma](media/stream-analytics-manage-job/create-event-hub-namespace.png)

5. Ad alanı dağıtımı bitirdiğinde, **tüm kaynaklar** ' a gidin ve Azure kaynakları listesinde *asaTutorialEventHub* bulun. Açmak için *asaTutorialEventHub* öğesini seçin.

6. İleri ' yi seçin ve olay **hub 'ı için** bir **ad** girin. **Bölüm sayısını** *2* olarak ayarlayın.  Kalan ayarlarda varsayılan seçenekleri kullanın ve **Oluştur**' u seçin. Ardından dağıtımın başarıyla tamamlanmasını bekleyin.

   ![Azure portal 'de Olay Hub 'ı yapılandırması](media/stream-analytics-manage-job/create-event-hub-portal.png)

### <a name="grant-access-to-the-event-hub-and-get-a-connection-string"></a>Olay hub’ına erişim verme ve bir bağlantı dizesi alma

Bir uygulamanın Azure Olay Hub’larına veri gönderebilmesi için olay hub’ında ilgili erişime izin veren bir ilke olması gerekir. Erişim ilkesi, yetkilendirme bilgilerini içeren bir bağlantı dizesi oluşturur.

1. Önceki adımda oluşturduğunuz Olay Hub 'ına gidin, *MyEventHub*. **Ayarlar**'ın altında **Paylaşılan erişim ilkeleri**'ni ve ardından **+ Ekle**'yi seçin.

2. İlkeye **MyPolicy** adını verin ve **Yönet** seçeneğinin işaretli olduğundan emin olun. Ardından **Oluştur**’u seçin.

   ![Olay hub'ı paylaşılan erişim ilkesi oluşturma](media/stream-analytics-manage-job/create-event-hub-access-policy.png)

3. İlke oluşturulduktan sonra ilkeyi açmak için ilke adına tıklayın. **Bağlantı dizesini bulur – birincil anahtar**. Bağlantı dizesinin yanındaki **Kopyala** düğmesini seçin.

   ![Paylaşılan erişim ilkesi bağlantı dizesini kaydetme](media/stream-analytics-manage-job/save-connection-string.png)

4. Bağlantı dizesini bir metin düzenleyicisine yapıştırın. Sonraki bölümde bu bağlantı dizesine ihtiyacınız olacaktır.

   Bağlantı dizesi şu şekilde görünür:

   `Endpoint=sb://<Your event hub namespace>.servicebus.windows.net/;SharedAccessKeyName=<Your shared access policy name>;SharedAccessKey=<generated key>;EntityPath=<Your event hub name>`

   Bağlantı dizesinin noktalı virgülle ayrılmış birden çok anahtar-değer çifti içerdiğine dikkat edin: **Endpoint**, **sharedaccesskeyname**, **Sharedaccesskey** ve **entitypath**.

## <a name="start-the-event-generator-application"></a>Olay oluşturucu uygulamasını başlatma

TelcoGenerator uygulamasını başlatmadan önce bunu, daha önce oluşturduğunuz Azure Olay Hub’ına veri gönderecek şekilde yapılandırmanız gerekir.

1. [TelcoGenerator.zip](https://download.microsoft.com/download/8/B/D/8BD50991-8D54-4F59-AB83-3354B69C8A7E/TelcoGenerator.zip) dosyasının içeriklerini ayıklayın.
2. `TelcoGenerator\TelcoGenerator\telcodatagen.exe.config`Dosyayı seçtiğiniz bir metin düzenleyicisinde açın (birden fazla. config dosyası varsa, doğru olanı seçtiğinizden emin olun.)

3. Config dosyasındaki `<appSettings>` öğesini şu bilgilerle güncelleştirin:

   * *Eventhubname* anahtarının değerini bağlantı dizesindeki entitypath değerine ayarlayın.
   * *Microsoft. ServiceBus. ConnectionString* anahtarının değerini, entitypath değeri olmadan bağlantı dizesine ayarlayın (bundan önce gelen noktalı virgülü kaldırmayı unutmayın).

4. Dosyayı kaydedin.
5. Daha sonra bir komut penceresi açıp, TelcoGenerator uygulamasının sıkıştırmasını açtığınız klasöre geçin. Ardından aşağıdaki komutu girin:

   ```cmd
   telcodatagen.exe 1000 0.2 2
   ```

   Bu komut aşağıdaki parametreleri alır:
   * Saat başına arama verisi kaydının sayısı.
   * Sahtekarlık olasılığının yüzdesi, uygulamanın ne sıklıkta sahte arama benzetimi gerçekleştirmesi gerektiği. 0.2 değeri arama kayıtlarının %20'sinin sahte görüneceğini anlamına gelir.
   * Saat cinsinden süre, uygulamanın çalışması gereken saat sayısı. Ayrıca, komut satırında işlemi sonlandırarak (**CTRL + C**) uygulamayı istediğiniz zaman durdurabilirsiniz.

   Birkaç saniye sonra uygulama, telefon araması kayıtlarını olay hub'ına gönderirken ekranda bu kayıtları görüntülemeye başlar. Telefon araması verileri aşağıdaki alanları içerir:

   |**Kayıt**  |**Tanım**  |
   |---------|---------|
   |CallrecTime    |  Arama başlangıç zamanı için zaman damgası.       |
   |SwitchNum     |  Aramayı bağlamak için kullanılan telefon anahtarı. Bu örnekte, anahtarlar menşei ülke/bölge (ABD, Çin, UK, Almanya veya Avustralya) temsil eden dizelerdir.       |
   |CallingNum     |  Arayanın telefon numarası.       |
   |CallingIMSI     |  Uluslararası Mobil Abone Kimliği (IMSI). Bu, arayanın benzersiz tanımlayıcısıdır.       |
   |CalledNum     |   Arama alıcısının telefon numarası.      |
   |CalledIMSI     |  Uluslararası Mobil Abone Kimliği (IMSI). Bu, arama alıcısının benzersiz tanımlayıcısıdır.       |

## <a name="create-a-stream-analytics-job"></a>Akış Analizi işi oluşturma

Arama olaylarından oluşan bir akışa sahip olduğunuza göre artık olay hub'ından veri okuyan bir Stream Analytics işi oluşturabilirsiniz.

1. Stream Analytics bir iş oluşturmak için [Azure Portal](https://portal.azure.com/)gidin.

2. **Kaynak oluştur** ve **Stream Analytics işi** ara ' yı seçin. **Stream Analytics iş** kutucuğunu seçin ve **Oluştur**' u seçin.

3. **Yeni Stream Analytics işi** formunu aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |İş adı     |  ASATutorial       |   Olay hub'ı ad alanını tanımlamak için benzersiz bir ad.      |
   |Abonelik    |  \<Your subscription\>   |   İşi oluşturmak istediğiniz Azure aboneliğini seçin.       |
   |Kaynak grubu   |   MyASADemoRG      |   **Var olanı kullan** seçeneğini belirleyin ve hesabınız için yeni bir kaynak grubu adı girin.      |
   |Konum   |    Batı ABD 2     |      İşin dağıtılabileceği konum. En iyi performans için ve bölgeler arasında veri aktarımına yönelik ödeme yapmamanız için işin ve olay hub’ının aynı bölgeye yerleştirilmesi önerilir.      |
   |Barındırma ortamı    | Bulut        |     Stream Analytics işleri buluta veya uca dağıtılabilir. Bulut, Azure bulutuna dağıtmanıza olanak tanır ve Edge bir IoT Edge cihazına dağıtmanıza olanak tanır.    |
   |Akış birimleri     |    1       |      Akış birimleri, bir işin yürütülmesi için gereken bilgi işlem kaynaklarını temsil eder. Varsayılan olarak, bu değer 1 olarak ayarlanır. Akış birimlerini ölçeklendirme hakkında bilgi edinmek için [akış birimlerini anlama ve ayarlama](stream-analytics-streaming-unit-consumption.md) başlıklı makaleye bakın.      |

4. Kalan ayarlarda varsayılan seçenekleri kullanın, **Oluştur**' u seçin ve dağıtımın başarılı olmasını bekleyin.

   ![Azure Stream Analytics işi oluşturma](media/stream-analytics-manage-job/create-stream-analytics-job.png)

## <a name="configure-job-input"></a>İş girişi yapılandırma

Bir sonraki adım, önceki bölümde oluşturduğunuz olay hub'ını kullanarak işin veri okuyacağı bir giriş kaynağı tanımlamaktır.

1. Azure portal **tüm kaynaklar** sayfasını açın ve *asaöğreticisi* Stream Analytics işini bulun.

2. Stream Analytics işinin **Iş topolojisi** bölümünde **girişler**' i seçin.

3. **+ Akış girişi ekle**'yi ve **Olay hub'ı** girişini seçin. Giriş formunu aşağıdaki değerlerle doldurun:

   |**Ayar**  |**Önerilen değer**  |**Açıklama**  |
   |---------|---------|---------|
   |Girdi diğer adı     |  CallStream       |  Girişinizi tanımlayan bir kolay ad girin. Giriş diğer adı alfasayısal karakter, kısa çizgi ve alt çizgi içerebilir, ayrıca 3 ila 63 karakter uzunluğunda olmalıdır.       |
   |Abonelik    |   \<Your subscription\>      |   Olay hub’ını oluşturduğunuz Azure aboneliğini seçin. Olay hub’ı Stream Analytics işiyle aynı abonelikte veya bundan farklı bir abonelikte olabilir.       |
   |Olay hub’ı ad alanı    |  asaTutorialEventHub       |  Önceki bölümde oluşturduğunuz olay hub’ı ad alanını seçin. Geçerli aboneliğinizde kullanılabilen tüm olay hub’ı ad alanları açılır menüde listelenir.       |
   |Olay Hub'ı adı    |   MyEventHub      |  Önceki bölümde oluşturduğunuz olay hub’ını seçin. Geçerli aboneliğinizde kullanılabilen tüm olay hub’ları açılır menüde listelenir.       |
   |Olay Hub'ı ilke adı   |  MyPolicy       |  Önceki bölümde oluşturduğunuz olay hub’ı paylaşılan erişim ilkesini seçin. Geçerli aboneliğinizde kullanılabilen tüm olay hub’ı ilkeleri açılır menüde listelenir.       |

4. Kalan ayarlarda varsayılan seçenekleri kullanın ve **Kaydet**’i seçin.

   ![Azure Stream Analytics girişi yapılandırma](media/stream-analytics-manage-job/configure-stream-analytics-input.png)

## <a name="configure-job-output"></a>İş çıkışını yapılandırma

Son adım, işin dönüştürülen verileri yazabilmesi için bir çıkış havuzu tanımlamaktır. Bu öğreticide verileri Power BI ile çıkarıp görselleştireceksiniz.

1. Azure portal, **tüm kaynaklar**' ı açın ve *asaöğreticisi* Stream Analytics işini seçin.

2. Stream Analytics işinin **Iş topolojisi** bölümünde **çıktılar** seçeneğini belirleyin.

3. **+ Power BI Ekle**' yi seçin  >  **Power BI**. Ardından **Yetkilendir** ' i seçin ve Power BI kimlik doğrulaması yapmak için istemleri izleyin.

:::image type="content" source="media/stream-analytics-manage-job/authorize-power-bi.png" alt-text="Power BI için yetkilendir düğmesi":::

4. Çıkış formunu aşağıdaki ayrıntılarla doldurup **Kaydet**' i seçin:

   |**Ayar**  |**Önerilen değer**  |
   |---------|---------|
   |Çıktı diğer adı  |  MyPBIoutput  |
   |Grup çalışma alanı| Çalışma alanım |
   |Veri kümesi adı  |   ASAdataset  |
   |Tablo adı |  ASATable  |
   | Kimlik doğrulaması modu | Kullanıcı belirteci |

   ![Stream Analytics çıkışını yapılandırma](media/stream-analytics-manage-job/configure-stream-analytics-output.png)

   Bu öğretici, *Kullanıcı belirteci* kimlik doğrulama modunu kullanır. Yönetilen kimliği kullanmak için, bkz. [Azure Stream Analytics işinizin kimliğini doğrulamak Için yönetilen kimliği kullanma Power BI](powerbi-output-managed-identity.md).

## <a name="define-a-query-to-analyze-input-data"></a>Giriş verilerini analiz etmek için sorgu tanımlama

Bir sonraki adım, verileri gerçek zamanlı olarak analiz eden bir dönüşüm oluşturmaktır. Dönüştürme sorgusunu [Stream Analytics sorgu dilini](/stream-analytics-query/stream-analytics-query-language-reference) kullanarak tanımlarsınız. Bu öğreticide kullanılan sorgu, telefon verilerindeki sahte aramaları algılar.

Bu örnekte sahte aramalar, aynı kullanıcı tarafından beş saniye içinde ancak farklı konumlardan gerçekleştirilmiştir. Örneğin, bir kullanıcı mantıksal olarak aynı anda hem ABD’den hem de Avustralya’dan arama yapamaz. Stream Analytics işiniz için dönüştürme sorgusu tanımlama amacıyla:

1. Azure portal, **tüm kaynaklar** bölmesini açın ve daha önce oluşturduğunuz **aşama öğreticisi** Stream Analytics işine gidin.

2. Stream Analytics işinin **Iş topolojisi** bölümünde **sorgu** seçeneğini belirleyin. Sorgu penceresi, iş için yapılandırılmış giriş ve çıkışları listeler ve giriş akışını dönüştürmek için bir sorgu oluşturmanıza olanak sağlar.

3. Düzenleyicideki mevcut sorguyu aşağıdaki sorguyla değiştirin. Bu sorgu, arama verilerinin 5 saniyelik aralığına denk gelen bir iç birleşim gerçekleştirir:

   ```sql
   SELECT System.Timestamp AS WindowEnd, COUNT(*) AS FraudulentCalls
   INTO "MyPBIoutput"
   FROM "CallStream" CS1 TIMESTAMP BY CallRecTime
   JOIN "CallStream" CS2 TIMESTAMP BY CallRecTime
   ON CS1.CallingIMSI = CS2.CallingIMSI
   AND DATEDIFF(ss, CS1, CS2) BETWEEN 1 AND 5
   WHERE CS1.SwitchNum != CS2.SwitchNum
   GROUP BY TumblingWindow(Duration(second, 1))
   ```

   Sahte aramaları denetlemek için `CallRecTime` değerine göre akış verilerinde iç birleşim uygulayabilirsiniz. Daha sonra `CallingIMSI` değerin (kaynak numarası) aynı olduğu, ancak `SwitchNum` değer (ülke/kaynak bölge) farklı olan çağrı kayıtlarını arayabilirsiniz. Akış verileriyle bir JOIN işlemi kullandığınızda birleştirme, eşleşen satırların zaman içinde ne kadar ayrılabildiğine ilişkin bazı sınırlar sağlamalıdır. Veri akışı sonsuz olduğundan ilişki için zaman sınırları, birleştirme işleminin **ON** yan tümcesi içinde [DATEDIFF](/stream-analytics-query/datediff-azure-stream-analytics) işlevi kullanılarak belirtilir.

   Bu sorgu, **DATEDIFF** işlevi dışında normal bir SQL JOIN gibi olur. Bu sorguda kullanılan **DATEDIFF** işlevi Stream Analytics’e özeldir ve `ON...BETWEEN` yan tümcesi içinde görünmelidir.

4. Sorguyu **kaydedin**.

   ![Portalda Stream Analytics sorgusu tanımlama](media/stream-analytics-manage-job/define-stream-analytics-query.png)

## <a name="test-your-query"></a>Sorgunuzu test etme

Sorgu Düzenleyicisi 'nden bir sorguyu test edebilirsiniz. Sorguyu test etmek için aşağıdaki adımları çalıştırın:

1. TelcoGenerator uygulamasının çalıştığından ve telefon araması kayıtları oluşturduğundan emin olun.

2. Sorguyu test etmek için **Test**'i seçin. Aşağıdaki sonuçları görmeniz gerekir:

   ![Stream Analytics sorgu testinin çıktısı](media/stream-analytics-manage-job/sample-test-output-restuls.png)

## <a name="start-the-job-and-visualize-output"></a>İş başlatma ve çıkışı görselleştirme

1. İşi başlatmak için işe **Genel Bakış ' a** gidin ve **Başlat**' ı seçin.

2. İş çıkışı başlangıç saati için **Şimdi**’yi seçip **Başlat** seçeneğini belirleyin. İş durumunu bildirim çubuğunda durumu görüntüleyebilirsiniz.

3. İş başarıyla tamamlandıktan sonra [Power BI](https://powerbi.com/)'a gidip iş veya okul hesabınızla oturum açın. Stream Analytics işi sorgusu sonuç üretiyorsa oluşturduğunuz *ASAdataset* veri kümesi **Veri kümeleri** sekmesinde yer alır.

4. Power BI çalışma alanınızdan **+ Oluştur**'u seçerek *Fraudulent Calls* adında yeni bir pano oluşturun.

5. Pencerenin üst kısmında **Düzenle** ve **kutucuk Ekle**' yi seçin. Ardından **Özel Akış Verileri**'ni ve **İleri**'yi seçin. **Veri kümeleriniz** bölümünden **ASAdataset** girişini seçin. **Görselleştirme türü** açılan menüsünden **kart** ' ı seçin ve alanlara **sahte çağrılar** ekleyin **Fields**. **İleri**'yi seçip ad belirledikten sonra **Uygula**'yı seçerek kutucuğu oluşturun.

   ![Power BI Pano kutucukları oluşturma](media/stream-analytics-manage-job/create-power-bi-dashboard-tiles.png)

6. Aşağıdaki seçeneklerle 5. adımı tekrar uygulayın:
   * Görselleştirme Türü’ne geldiğinizde Çizgi grafik seçeneğini belirleyin.
   * Eksen ekleyin ve **windowend** seçeneğini belirleyin.
   * Değer ekleyip **fraudulentcalls** seçeneğini belirleyin.
   * **Görüntülenecek zaman penceresini** için son 10 dakikayı seçin.

7. İki kutucuk da eklendikten sonra panonuz aşağıdaki örneğe benzer olmalıdır. Olay Hub 'ı gönderen uygulamanız ve akış analizi uygulamanız çalışıyorsa, Power BI panonuz düzenli aralıklarla yeni veri geldiğinde güncelleştiğine dikkat edin.

   ![Power BI panodaki sonuçları görüntüleme](media/stream-analytics-manage-job/power-bi-results-dashboard.png)

## <a name="embedding-your-power-bi-dashboard-in-a-web-application"></a>Power BI panonuzu bir Web uygulamasına katıştırma

Öğreticinin bu bölümünde, panonuzu eklemek için Power BI ekibi tarafından oluşturulan örnek bir [ASP.net](https://asp.net/) Web uygulamasını kullanacaksınız. Pano ekleme hakkında daha fazla bilgi için [Power BI ile ekleme](/power-bi/developer/embedding) başlıklı makaleye bakın.

Uygulamayı ayarlamak için [PowerBI-Developer-Samples](https://github.com/Microsoft/PowerBI-Developer-Samples) GitHub deposuna gidin ve **verilerin sahip olduğu Kullanıcı** bölümündeki yönergeleri izleyin ( **tümleştirin-Web-App** alt bölümünün altındaki yeniden yönlendirme ve ana sayfa URL 'lerini kullanın). Pano örneğini kullandığımızdan, [GitHub deposunda](https://github.com/microsoft/PowerBI-Developer-Samples/tree/master/.NET%20Framework/Embed%20for%20your%20organization/)bulunan **tümleştirin-Web-App** örnek kodunu kullanın.
Uygulamayı tarayıcınızda çalıştırmaya başladıktan sonra, daha önce oluşturduğunuz panoyu web sayfasına eklemek için şu adımları uygulayın:

1. **Power BI Için oturum aç**' ı seçin. Bu, uygulamaya Power BI hesabınızdaki panolara erişim izni verir.

2. Hesabınızın Panolarını bir tabloda görüntüleyen **Pano Al** düğmesini seçin. Daha önce oluşturduğunuz panonun adını, **PowerBI-Embedded-panosunu** bulun ve karşılık gelen **EmbedUrl** kopyalayın.

3. Son olarak, **EmbedUrl**’yi ilgili metin alanına yapıştırıp **Panoyu Ekle** seçeneğini belirleyin. Artık bir web uygulamasının içine eklenen panoyu görüntüleyebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, basit bir Stream Analytics işi oluşturdunuz, gelen verileri analiz ettiniz ve sonuçları bir Power BI panosunda sundunuz. Stream Analytics işleri hakkında daha fazla bilgi edinmek için sonraki öğreticiye geçin:

> [!div class="nextstepaction"]
> [Stream Analytics işlerinde Azure İşlevleri’ni çalıştırma](stream-analytics-with-azure-functions.md)
