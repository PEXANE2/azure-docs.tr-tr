---
title: Veri ardışık bir iş tonu oluşturmak için Veri Toplayıcı API'sını kullanma
description: REST API'yi arayabilen herhangi bir istemciden Günlük Analizi çalışma alanına POST JSON verilerini eklemek için Azure Monitor HTTP Veri Toplayıcı API'sini kullanabilirsiniz. Bu makalede, dosyalarda depolanan verilerin otomatik bir şekilde nasıl yüklenir.
ms.subservice: logs
ms.topic: conceptual
author: bwren
ms.author: bwren
ms.date: 08/09/2018
ms.openlocfilehash: 96c64f6a0167b678f14bf0199069ecd6b4c8d57a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80055104"
---
# <a name="create-a-data-pipeline-with-the-data-collector-api"></a>Veri Toplayıcı API'si ile bir veri ardışık

[Azure Monitörü Veri ToplayıcıAPI,](data-collector-api.md) azure monitöründeki bir Günlük Analizi çalışma alanına özel günlük verilerini içe aktarmanıza olanak tanır. Tek gereksinim, verilerin JSON biçimlendirilmiş olması ve 30 MB veya daha az segmente bölünmesidir. Bu, birçok şekilde bağlanabilen tamamen esnek bir mekanizmadır: doğrudan uygulamanızdan gönderilen verilerden, tek seferlik geçici yüklemelere kadar. Bu makalede, ortak bir senaryo için bazı başlangıç noktaları anahat: düzenli, otomatik olarak dosyalarda depolanan verileri yüklemek için ihtiyaç. Burada sunulan boru hattı en performant veya başka optimize olmayacak olsa da, kendi bir üretim boru hattı inşa etmek için bir başlangıç noktası olarak hizmet etmek amaçlanmıştır.

[!INCLUDE [azure-monitor-log-analytics-rebrand](../../../includes/azure-monitor-log-analytics-rebrand.md)]

## <a name="example-problem"></a>Örnek sorun
Bu makalenin geri kalanı için, Uygulama Öngörüleri'ndeki sayfa görünümü verilerini inceleyeceğiz. Varsayımsal senaryomuzda, Application Insights SDK tarafından varsayılan olarak toplanan coğrafi bilgileri, dünyanın her ülkesinin/bölgesinin nüfusunu içeren özel verilerle ilişkilendirmek istiyoruz ve bu da en çok pazarlama dolarını nerede harcamamız gerektiğini belirlemek tir. 

Bu amaçla [BM Dünya Nüfus Beklentileri](https://esa.un.org/unpd/wpp/) gibi genel bir veri kaynağı kullanıyoruz. Veriler aşağıdaki basit şema olacaktır:

![Örnek basit şema](./media/create-pipeline-datacollector-api/example-simple-schema-01.png)

Örneğimizde, kullanılabilir hale gelir gelmez en son yılın verileriyle birlikte yeni bir dosya yükleyeceğimizi varsayıyoruz.

## <a name="general-design"></a>Genel tasarım
Boru hattımızı tasarlamak için klasik bir ETL tipi mantık kullanıyoruz. Mimari aşağıdaki gibi görünecektir:

![Veri toplama boru hattı mimarisi](./media/create-pipeline-datacollector-api/data-pipeline-dataflow-architecture.png)

Bu makale, veri oluşturma veya [Azure Blob Depolama hesabına nasıl yüklenir'](../../storage/blobs/storage-upload-process-images.md)i kapsamaz. Bunun yerine, yeni bir dosya blob yüklenir yüklenmez akışı alıyoruz. Buradan:

1. Bir işlem, yeni verilerin yüklendiğini algılar.  Örneğimiz, bir blob'a yüklenen yeni verileri algılamak için tetikleyicisi olan bir [Azure Mantık Uygulaması](../../logic-apps/logic-apps-overview.md)kullanır.

2. Bir işlemci bu yeni verileri okur ve Azure Monitor'un gerektirdiği biçim olan JSON'a dönüştürür Bu örnekte, işlem kodumuzu yürütmenin hafif ve düşük maliyetli bir yolu olarak bir [Azure İşi](../../azure-functions/functions-overview.md) kullanırız. İşlev, yeni verileri algılamak için kullandığımız Mantık Uygulaması tarafından devreye satılır.

3. Son olarak, JSON nesnesi kullanılabilir olduğunda Azure Monitor'a gönderilir. Aynı Mantık Uygulaması, Log Analytics Data Collector etkinliğinde yerleşik kullanarak verileri Azure Monitor'a gönderir.

Blob depolama, Mantık Uygulaması veya Azure İşlevi'nin ayrıntılı kurulumu bu makalede belirtilmiş olmasa da, belirli ürünlerin sayfalarında ayrıntılı yönergeler mevcuttur.

Bu ardışık hattı izlemek için, Azure İşlevi [ayrıntılarımızı burada](../../azure-functions/functions-monitoring.md)izlemek için Uygulama Öngörüleri'ni ve Burada Mantıksal Uygulama [ayrıntılarımızı](../../logic-apps/logic-apps-monitor-your-logic-apps-oms.md)izlemek için Azure Monitor'u kullanırız. 

## <a name="setting-up-the-pipeline"></a>Boru hattının kurulumu
Ardışık lığı ayarlamak için, önce blob kapsayıcınızın oluşturulduğundan ve yapılandırıldığından emin olun. Aynı şekilde, verileri göndermek istediğiniz Log Analytics çalışma alanının oluşturulduğundan emin olun.

## <a name="ingesting-json-data"></a>JSON verilerini yutma
JSON verilerini yutmak Logic Apps ile önemsizdir ve herhangi bir dönüşüm gerçekleşmesi gerekmediğinden, tüm ardışık hattı tek bir Mantık Uygulamasına dahil edebiliriz. Hem blob kapsayıcısı hem de Log Analytics çalışma alanı yapılandırıldıktan sonra, yeni bir Mantık Uygulaması oluşturun ve aşağıdaki gibi yapılandırın:

![Mantık uygulamaları iş akışı örneği](./media/create-pipeline-datacollector-api/logic-apps-workflow-example-01.png)

Mantık Uygulamanızı kaydedin ve test etmeye devam edin.

## <a name="ingesting-xml-csv-or-other-formats-of-data"></a>XML, CSV veya diğer veri biçimlerini yutma
Logic Apps bugün xml, CSV veya diğer türleri JSON biçimine kolayca dönüştürecek yerleşik özelliklere sahip değildir. Bu nedenle, bu dönüşümü tamamlamak için başka bir araç kullanmanız gerekir. Bu makale için, Azure Fonksiyonları'nın sunucusuz bilgi işlem özelliklerini çok hafif ve maliyet dostu bir şekilde kullanıyoruz. 

Bu örnekte, bir CSV dosyayı ayrışdırıyoruz, ancak diğer dosya türü benzer şekilde işlenebilir. Azure İşlev'in deserializing bölümünü, belirli veri türünüz için doğru mantığı yansıtacak şekilde değiştirmeniz yeterlidir.

1.  İstendiğinde işlev çalışma zamanı v1 ve tüketim tabanlı kullanarak yeni bir Azure İşlevi oluşturun.  Bağlayıcılarınızı istediğimiz şekilde yapılandıran bir başlangıç noktası olarak C#'da hedeflenen **HTTP tetikleyici** şablonunu seçin. 
2.  Sağ bölmedeki **Dosyaları Görüntüle** sekmesinden **project.json** adında yeni bir dosya oluşturun ve kullanmakta olduğumuz NuGet paketlerinden aşağıdaki kodu yapıştırın:

    ![Azure Fonksiyonları örnek proje](./media/create-pipeline-datacollector-api/functions-example-project-01.png)
    
    ``` JSON
    {
      "frameworks": {
        "net46":{
          "dependencies": {
            "CsvHelper": "7.1.1",
            "Newtonsoft.Json": "11.0.2"
          }  
        }  
       }  
     }  
    ```

3. Sağ bölmeden **run.csx'e** geçin ve varsayılan kodu aşağıdakilerle değiştirin. 

    >[!NOTE]
    >Projeniz için, kayıt modelini ("PopulationRecord" sınıfı) kendi veri şemanızla değiştirmeniz gerekir.
    >

    ```   
    using System.Net;
    using Newtonsoft.Json;
    using CsvHelper;
    
    class PopulationRecord
    {
        public String Location { get; set; }
        public int Time { get; set; }
        public long Population { get; set; }
    }

    public static async Task<HttpResponseMessage> Run(HttpRequestMessage req, TraceWriter log)
    {
        string filePath = await req.Content.ReadAsStringAsync(); //get the CSV URI being passed from Logic App
        string response = "";

        //get a stream from blob
        WebClient wc = new WebClient();
        Stream s = wc.OpenRead(filePath);         

        //read the stream
        using (var sr = new StreamReader(s))
        {
            var csvReader = new CsvReader(sr);
    
            var records = csvReader.GetRecords<PopulationRecord>(); //deserialize the CSV stream as an IEnumerable
    
            response = JsonConvert.SerializeObject(records); //serialize the IEnumerable back into JSON
        }    

        return response == null
            ? req.CreateResponse(HttpStatusCode.BadRequest, "There was an issue getting data")
            : req.CreateResponse(HttpStatusCode.OK, response);
     }  
    ```

4. İşlevinizi kaydedin.
5. Kodun doğru çalıştığından emin olmak için işlevi sınama. Testi aşağıdaki gibi yapılandırarak sağ bölmedeki **Test** sekmesine geçin. Örnek verileri içeren bir blob bağlantısını **İstek gövdesi** metin kutusuna yerleştirin. **Çalıştır'ı**tıklattıktan sonra **Çıktı** kutusunda JSON çıktısını görmeniz gerekir:

    ![Fonksiyon Uygulamaları test kodu](./media/create-pipeline-datacollector-api/functions-test-01.png)

Şimdi geri dönmek ve daha önce yutulan ve JSON formatına dönüştürülmüş verileri içerecek şekilde oluşturmaya başladığımız Mantık Uygulaması değiştirmek gerekir.  Görünüm Tasarımcısı'nı kullanarak, aşağıdaki gibi yapılandırın ve ardından Mantık Uygulamanızı kaydedin:

![Mantık Uygulamaları iş akışı tam örnek](./media/create-pipeline-datacollector-api/logic-apps-workflow-example-02.png)

## <a name="testing-the-pipeline"></a>Boru hattını test etme
Şimdi blob daha önce yapılandırılan yeni bir dosya yükleyebilirsiniz ve Mantık App tarafından izlenir. Yakında, Mantık Uygulaması'nın yeni bir örneğinin başlatTığını, Azure İşi'nize çağrıda olduğunu ve ardından verileri Azure Monitor'a başarıyla göndermeniz gerekir. 

>[!NOTE]
>Yeni bir veri türü ilk gönderdiğinde verilerin Azure Monitor'da görünmesi 30 dakika kadar sürebilir.


## <a name="correlating-with-other-data-in-log-analytics-and-application-insights"></a>Log Analytics ve Application Insights'taki diğer verilerle ilişkilendirme
Uygulama Öngörüleri sayfa görüntüleme verilerini özel veri kaynağımızdan aldığımız nüfus verileriyle ilişkilendirme hedefimizi tamamlamak için, Application Insights Analytics pencerenizden veya Log Analytics çalışma alanınızdan aşağıdaki sorguyu çalıştırın:

``` KQL
app("fabrikamprod").pageViews
| summarize numUsers = count() by client_CountryOrRegion
| join kind=leftouter (
   workspace("customdatademo").Population_CL
) on $left.client_CountryOrRegion == $right.Location_s
| project client_CountryOrRegion, numUsers, Population_d
```

Çıktı, şimdi birleşen iki veri kaynağını göstermelidir.  

![Bir arama sonucu örneğinde birleştirilmiş verileri ilişkilendirme](./media/create-pipeline-datacollector-api/correlating-disjoined-data-example-01.png)

## <a name="suggested-improvements-for-a-production-pipeline"></a>Üretim boru hattı için önerilen iyileştirmeler
Bu makalede, gerçek bir üretim kalitesinde çözüm doğru uygulanabilir arkasında mantık çalışan bir prototip sundu. Böyle bir üretim kalitesinde çözüm için aşağıdaki iyileştirmeler önerilir:

* Mantık Uygulama ve İşlevinizde hata işleme ve yeniden deneme mantığı ekleyin.
* 30MB/tek Log Analytics Ingestion API çağrı limitinin aşılmadığından emin olmak için mantık ekleyin. Gerekirse verileri daha küçük bölümlere bölün.
* Blob depolamanızda bir temizleme ilkesi ayarlayın. Log Analytics çalışma alanına başarıyla gönderildikten sonra, ham verileri arşivleme amacıyla kullanılabilir tutmak istemiyorsanız, depolamaya devam etmek için bir neden yoktur. 
* İzlemenin tam ardışık hat boyunca etkinleştirildiğinden ve uygun şekilde izleme noktaları ve uyarılar eklenin.
* İşlevinizin ve Logic App'inizin kodunu yönetmek için kaynak denetiminden yararlanın.
* Şema değişirse, işlev ve Mantık Uygulamaları buna göre değiştirilir gibi uygun bir değişiklik yönetimi ilkesiiz olduğundan emin olun.
* Birden çok farklı veri türü yüklüyorsanız, bunları blob kapsayıcınız içindeki tek tek klasörlere ayırın ve veri türüne göre mantığı dışarı çıkarmak için mantık oluşturun. 


## <a name="next-steps"></a>Sonraki adımlar
Herhangi bir REST API istemcisinden Log Analytics çalışma alanına veri yazmak için [Veri Toplayıcı API'sı](data-collector-api.md) hakkında daha fazla bilgi edinin.
