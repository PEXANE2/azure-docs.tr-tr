---
title: IoT Edge üzerinde Azure Stream Analytics
description: Edge işleri, Azure Stream Analytics'te oluşturmak ve bunları Azure IOT Edge çalıştıran cihazlara dağıtabilirsiniz.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: conceptual
ms.date: 02/14/2020
ms.custom: seodec18
ms.openlocfilehash: 7e4a1cf43931b56cfdb3c31ffe870a07dbaa75af
ms.sourcegitcommit: 333af18fa9e4c2b376fa9aeb8f7941f1b331c11d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77201764"
---
# <a name="azure-stream-analytics-on-iot-edge"></a>IoT Edge üzerinde Azure Stream Analytics
 
IoT Edge üzerinde Azure Stream Analytics (ASA), geliştiricilerin neredeyse gerçek zamanlı analitik zekayı IoT cihazlarına daha yakın bir biçimde dağıtmasına ve böylece cihaz tarafından üretilen verilerin tüm değerini ortaya çıkarabilmesine olanak tanır. Azure Stream Analytics düşük gecikme süresi, dayanıklılık, bant genişliğinin verimli kullanımı ve uyumluluk için tasarlanmıştır. Şimdi kuruluşlar bulutta yapılan endüstriyel işlemlerin ve tamamlayıcı Büyük Veri analizinin yakınında denetim mantığı dağıtımı yapabilir.  

IoT Edge üzerinde Azure Stream Analytics, [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/) Framework içinde çalışır. İçinde ASA işi oluşturulduktan sonra dağıtabilir ve IOT hub'ı kullanarak yönetebilirsiniz.

## <a name="scenarios"></a>Senaryolar
![IOT Edge üst düzey diyagramı](media/stream-analytics-edge/ASAedge-highlevel-diagram.png)

* **Düşük gecikmeli komut ve denetim**: Örneğin, üretim güvenlik sistemleri, işlemsel verilere Ultra düşük gecikme süresiyle yanıt vermelidir. IOT Edge üzerinde ASA ile algılayıcı verileri neredeyse gerçek zamanlı ve bir makine durdurmak veya uyarıları tetiklemek için anomalileri algılayın, komutları sorunu çözümleyebilirsiniz.
*   **Buluta sınırlı bağlantı**: uzaktan araştırma Ekipmanı, bağlı Vessels veya daha fazla detaya gitme gibi görev açısından kritik sistemler, bulut bağlantısı aralıklı olarak olsa bile verileri analiz etmeniz ve bunlara yanıt vermelidirler. ASA, akış mantığınızı ağ bağlantısı bağımsız olarak çalışır ve hangi buluta daha fazla işleme veya depolama için gönderdiğiniz seçebilirsiniz.
* **Sınırlı bant genişliği**: jet motorları veya bağlı otomobiller tarafından oluşturulan veri hacmi, verilerin buluta gönderilmeden önce filtrelenme veya önceden işlenmesi gereken büyüklükte olabilir. ASA kullanarak filtreleyin veya buluta gönderilmesi gereken veri toplama.
* **Uyumluluk**: yasal uyumluluk, bazı verilerin buluta gönderilmeden önce yerel olarak anonimleştirmesini veya toplanmasını gerektirebilir.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Azure Stream Analytics Edge işi
### <a name="what-is-an-edge-job"></a>"Edge" işi nedir?

ASA Edge işleri [Azure IoT Edge cihazlara](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)dağıtılan kapsayıcılar üzerinde çalışır. İki bölümden oluşan bunlar:
1.  İş tanımı için sorumlu olduğu bir bulut bölümü: kullanıcılar, bulutta giriş, çıkış, sorgu ve diğer ayarları (sıralama dışı olaylar, vb.) tanımlayın.
2.  IOT cihazlarınızda modül. Bu, ASA altyapısı içerir ve iş tanımını buluttan alır. 

ASA, edge işleri aygıtlara dağıtmak için IOT hub'ı kullanır. IoT Edge dağıtımı hakkında daha fazla bilgiye [buradan görünebilirler](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring).

![Azure Stream Analytics Edge işi](media/stream-analytics-edge/stream-analytics-edge-job.png)


### <a name="installation-instructions"></a>Yükleme yönergeleri
Üst düzey adımlar aşağıdaki tabloda açıklanmıştır. Daha fazla ayrıntı, aşağıdaki bölümlerde verilmiştir.

|      |Adım   | Notlar   |
| ---   | ---   |  ---      |
| 1   | **Depolama kapsayıcısı oluşturma**   | Depolama kapsayıcıları, IOT cihazlarınızı, burada erişilebilmelerini iş tanımınızı kaydetmek için kullanılır. <br>  Herhangi bir mevcut depolama kapsayıcısını yeniden kullanabilirsiniz.     |
| 2   | **ASA Edge işi oluşturma**   |  Yeni bir iş oluşturun, bir **barındırma ortamı**olarak **Edge** ' i seçin. <br> Bu işlerin buluttan oluşturulan ve yönetilen ve kendi IOT Edge cihazlarında çalıştırın.     |
| 3   | **IoT Edge ortamınızı cihazınızda ayarlayın**   | [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) veya [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux)için yönergeler.          |
| 4   | **IoT Edge cihazlarınızın ASA dağıtımını yapın**   |  ASA işi tanımı, daha önce oluşturduğunuz depolama kapsayıcısına dışarı aktarılır.       |

İlk ASA işinizi IoT Edge dağıtmak için [Bu adım adım öğreticiyi](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics) izleyebilirsiniz. Aşağıdaki videoda bir Stream Analytics işi bir IOT edge Cihazınızda çalıştırmak üzere işlemlerini anlamanıza yardımcı olması:  


> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T157/player]

#### <a name="create-a-storage-container"></a>Bir depolama kapsayıcısı oluşturma
Bir depolama kapsayıcısı derlenmiş ASA sorgu ve işlem yapılandırmasını dışarı aktarmak için gereklidir. ASA Docker görüntüsü, belirli bir sorgu ile yapılandırmak için kullanılır. 
1. Azure portal bir depolama hesabı oluşturmak için [Bu yönergeleri](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) izleyin. Tüm varsayılan seçenekleri ile ASA bu hesabı kullanmak için tutabilirsiniz.
2. Yeni oluşturulan depolama hesabında blob depolama kapsayıcısı oluşturun:
    1. **Bloblar**' a ve ardından **+ kapsayıcı**' ya tıklayın. 
    2. Bir ad girin ve kapsayıcıyı **özel**olarak tutun.

#### <a name="create-an-asa-edge-job"></a>ASA Edge işi oluşturma
> [!Note]
> Bu öğreticide Azure portalını kullanarak ASA işi oluşturma odaklanır. Ayrıca, [Visual Studio eklentisini kullanarak asa Edge işi oluşturabilirsiniz](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)

1. Azure portalında yeni bir "Stream Analytics işi" oluşturun. [Burada yeni BIR asa işi oluşturmak Için doğrudan bağlantı](https://ms.portal.azure.com/#create/Microsoft.StreamAnalyticsJob).

2. Oluşturma ekranında, bir **kenara** **barındırma ortamı** olarak seçin (aşağıdaki resme bakın)

   ![Edge üzerinde Stream Analytics işi oluşturma](media/stream-analytics-edge/create-asa-edge-job.png)
3. İş tanımı
    1. **Giriş akışı (ler) i tanımlayın**. İşiniz için bir veya birden çok giriş akışları tanımlayın.
    2. (İsteğe bağlı) başvuru verileri tanımlar.
    3. **Çıkış akışı (ler) i tanımlayın**. İşiniz için bir veya birden çok çıkış akışları tanımlayın. 
    4. **Sorgu tanımlayın**. ASA sorgu değiştirici kullanarak bulutta tanımlayın. Derleyici, ASA edge için etkin söz dizimi otomatik olarak denetler. Örnek verileri karşıya yükleyerek, sorgunuzu test edebilirsiniz. 

4. **IoT Edge ayarları** menüsünde depolama kapsayıcısı bilgilerini ayarlayın.

5. İsteğe bağlı ayarlar
    1. **Olay sıralaması**. Portalda, sırası ilkesi yapılandırabilirsiniz. Belgeler [burada](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics)bulunabilir.
    2. **Yerel ayar**. İnternalization biçimini ayarlayın.



> [!Note]
> Bir dağıtım oluşturduğunuzda, ASA bir depolama kapsayıcısına iş tanımını dışa aktarır. Bu iş tanımı aynı kalır bir dağıtım süresi boyunca. Sonuç olarak edge üzerinde çalışan bir iş güncelleştirmek istiyorsanız, ASA işi düzenleyin ve ardından IOT Hub'ında yeni bir dağıtım yaratmanız gerekir.


#### <a name="set-up-your-iot-edge-environment-on-your-devices"></a>IOT Edge ortamınızı aygıtlarınızın ayarlama
Edge işleri, Azure IOT Edge çalıştıran cihazlara dağıtılabilir.
Bunun için bu adımları izlemeniz gerekir:
- IOT Hub oluşturun.
- Docker ve IOT Edge çalışma zamanı edge cihazlarınıza yükleyebilir.
- Cihazlarınızı IoT Hub cihaz olarak ayarlayın **IoT Edge** .

Bu adımlar, [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) veya [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux)için IoT Edge belgelerinde açıklanmıştır.  


####  <a name="deployment-asa-on-your-iot-edge-devices"></a>Aygıtlarınızın IOT Edge üzerinde ASA dağıtım
##### <a name="add-asa-to-your-deployment"></a>ASA dağıtıma ekleme
- Azure portal, IoT Hub açın, **IoT Edge** ' e gidin ve bu dağıtım için hedeflemek istediğiniz cihaza tıklayın.
- **Modül ayarla**' yı seçin, ardından **+ Ekle** ' yi seçin ve **Azure Stream Analytics modülünü**seçin.
- Abonelik ve oluşturduğunuz ASA Edge işi seçin. Kaydet'e tıklayın.
dağıtımınıza ![ASA modülü ekleme](media/stream-analytics-edge/add-stream-analytics-module.png)


> [!Note]
> Bu adım sırasında ASA (Bunu zaten mevcut değilse) depolama kapsayıcısında "EdgeJobs" adlı bir klasör oluşturur. Her bir dağıtım için "EdgeJobs" klasöründe yeni bir alt klasör oluşturulur.
> İşinizi IoT Edge cihazlara dağıttığınızda, ASA iş tanımı dosyası için bir paylaşılan erişim imzası (SAS) oluşturur. SAS anahtarını, cihaz ikizi kullanarak IOT Edge cihazları güvenli bir şekilde aktarılır. Bu anahtarı süre sonu oluşturulduğu günden itibaren üç yıldır. Bir IoT Edge işini güncelleştirdiğinizde SAS değişir, ancak görüntü sürümü değişmeyecektir. **Güncelleştirme**yaptıktan sonra dağıtım iş akışını izleyin ve cihazda bir güncelleştirme bildirimi kaydedilir.


IoT Edge dağıtımları hakkında daha fazla bilgi için [Bu sayfaya](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring)bakın.


##### <a name="configure-routes"></a>Yolları Yapılandır
IOT Edge modülleri ve IOT hub'ı arasında ve modüller arasında iletileri bildirimli olarak yönlendirmek için bir yol sağlar. Tam sözdizimi [burada](https://docs.microsoft.com/azure/iot-edge/module-composition)açıklanmıştır.
Giriş ve çıkışları ASA işi oluşturulan adlarını yönlendirme için uç noktalar olarak kullanılabilir.  

###### <a name="example"></a>Örnek

```json
{
    "routes": {
        "sensorToAsa":   "FROM /messages/modules/tempSensor/* INTO BrokeredEndpoint(\"/modules/ASA/inputs/temperature\")",
        "alertsToCloud": "FROM /messages/modules/ASA/* INTO $upstream",
        "alertsToReset": "FROM /messages/modules/ASA/* INTO BrokeredEndpoint(\"/modules/tempSensor/inputs/control\")"
    }
}

```
Bu örnek aşağıdaki resimde açıklanan senaryo için yollar gösterir. "**Sıcaklık**" adlı ve "**Uyarı**" adlı bir çıktı içeren "**asa**" adlı bir kenar işi içerir.
ileti yönlendirme](media/stream-analytics-edge/edge-message-routing-example.png) ![diyagramı örneği

Bu örnek aşağıdaki yolları tanımlar:
- **Tempsensörü** tarafından gönderilen her Ileti, **asa** **adlı girişe,**
- **Asa** modülünün tüm çıktıları bu cihaza bağlı IoT Hub gönderilir ($upstream),
- **Asa** modülünün tüm çıkışları, **tempsensörü** **Denetim** uç noktasına gönderilir.


## <a name="technical-information"></a>Teknik bilgiler
### <a name="current-limitations-for-iot-edge-jobs-compared-to-cloud-jobs"></a>Bulut işlerine kıyasla IOT Edge işleri için geçerli sınırlamalar
Eşlik sahip olmaktır IOT Edge işleri ve bulut işleri arasında. Çoğu SQL sorgu dili özelliği desteklenir, hem bulutta hem de IoT Edge aynı mantığı çalıştırmaya olanak tanır.
Ancak aşağıdaki özellikleri, edge işleri için henüz desteklenmiyor:
* Kullanıcı tanımlı işlevler (UDF) JavaScript içinde. UDF [ C# for IoT Edge işleri](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-edge-csharp-udf) (Önizleme) için kullanılabilir.
* Kullanıcı tanımlı toplamlarda (UDA).
* Azure ML işlevleri.
* Tek bir adımda 14'ten fazla toplamaları kullanma.
* Giriş/Çıkış AVRO biçimi. Şu anda yalnızca CSV ve JSON desteklenir.
* Aşağıdaki SQL işleçleri:
    * BÖLÜMÜ
    * GetMetadataPropertyValue


### <a name="runtime-and-hardware-requirements"></a>Çalışma zamanı ve donanım gereksinimleri
IoT Edge ' de ASA çalıştırmak için [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/)çalıştırabileceği cihazlara ihtiyacınız vardır. 

ASA ve Azure IoT Edge birden çok konak işletim sisteminde (Windows, Linux) çalışan taşınabilir bir çözüm sağlamak için **Docker** kapsayıcıları kullanın.

IoT Edge on, hem x86-64 hem de ARM (Gelişmiş RıSC makineleri) mimarilerinde çalışan Windows ve Linux görüntüleri olarak kullanılabilir hale getirilir. 


### <a name="input-and-output"></a>Giriş ve çıkış
#### <a name="input-and-output-streams"></a>Giriş ve çıkış akışları
ASA Edge işleri, giriş ve çıkışları, IOT Edge cihaz üzerinde çalışan diğer modüllerden alabilirsiniz. Gelen ve belirli modüller bağlanmak için dağıtım sırasında yönlendirme yapılandırması ayarlayabilirsiniz. Daha fazla bilgi [IoT Edge modül oluşturma belgelerinde](https://docs.microsoft.com/azure/iot-edge/module-composition)açıklanmıştır.

Giriş ve çıkışları için CSV ve JSON biçimleri desteklenir.

Her giriş ve çıkış akışına, ASA işi oluşturmak için dağıtılan modülünüzde karşılık gelen bir uç noktası oluşturulur. Bu uç noktaları, dağıtım yolları kullanılabilir.

Akış girişi AT var, yalnızca desteklenen ve Edge hub'ı stream çıktı türleri şunlardır. Başvuru girişi başvuru destekleyen dosya türü. Bulut işi aşağı yönde kullanarak diğer çıkışlar erişilebilir. Örneğin, Edge'de barındırılan bir Stream Analytics işi, daha sonra çıktı IOT Hub'ına gönderebilirsiniz Edge Hub'ına çıktısını gönderir. Power BI veya başka bir çıkış türü için IOT Hub ve çıkış girdilere sahip ikinci bir bulutta barındırılan Azure Stream Analytics işi'ni kullanabilirsiniz.



##### <a name="reference-data"></a>Başvuru verileri
Başvuru verileri (arama tablosu olarak da bilinir) statik veya yavaş doğası gereği değiştirme sınırlı bir veri kümesi var. Bir arama gerçekleştirme ya da, veri akışı ile ilişkilendirmek için kullanılır. Azure Stream Analytics işinizdeki başvuru verilerini kullanmak için, genellikle sorgunuzda bir [başvuru VERISI katılımı](https://docs.microsoft.com/stream-analytics-query/reference-data-join-azure-stream-analytics) kullanacaksınız. Daha fazla bilgi için [Stream Analytics aramalar için başvuru verilerini kullanma](stream-analytics-use-reference-data.md)konusuna bakın.

Yalnızca yerel başvuru verileri desteklenir. IOT Edge cihazı için bir Proje dağıtıldığında, kullanıcı tanımlı dosya yolundan başvuru verileri yükler.

Başvuru verilerinde Edge ile bir iş oluşturmak için:

1. İşiniz için yeni bir giriş oluşturun.

2. **Kaynak türü**olarak **başvuru verileri** ' ni seçin.

3. Bir başvuru veri dosyasının hazır cihaza sahip. Bir Windows kapsayıcı için başvuru veri dosyasının yerel bir sürücüye yerleştirin ve yerel diske Docker kapsayıcısı ile paylaşın. Bir Linux kapsayıcı için Docker birim oluşturma ve birim veri dosyasına doldurun.

4. Dosya yolunu ayarlayın. Windows konak işletim sistemi ve Windows kapsayıcısı için mutlak yolu kullanın: `E:\<PathToFile>\v1.csv`. Bir Windows ana makinesi ve Linux kapsayıcısı veya Linux IŞLETIM sistemi ve Linux kapsayıcısı için, şu birimdeki yolu kullanın: `<VolumeName>/file1.txt`.

![IOT Edge üzerinde Azure Stream Analytics işine ilişkin yeni başvuru veri girişi](./media/stream-analytics-edge/Reference-Data-New-Input.png)

IOT Edge güncelleştirme başvuru verilerinde bir dağıtım tarafından tetiklenir. ASA modülü tetiklenen sonra çalışan işlemi durdurmadan güncelleştirilmiş veriler seçer.

Başvuru verileri güncelleştirmek için iki yolu vardır:
* Azure portalından, ASA işi başvuru veri yoluna güncelleştirin.
* IOT Edge dağıtımı güncelleştirin.

## <a name="license-and-third-party-notices"></a>Lisans ve üçüncü taraf bildirimleri
* [IoT Edge üzerinde Azure Stream Analytics lisansı](https://go.microsoft.com/fwlink/?linkid=862827). 
* [IoT Edge üzerinde Azure Stream Analytics Için üçüncü taraf bildirimi](https://go.microsoft.com/fwlink/?linkid=862828).

## <a name="azure-stream-analytics-module-image-information"></a>Azure Stream Analytics modülü görüntü bilgileri 

Bu sürüm bilgileri 2019-06-27 tarihinde son güncelleştirilme tarihi:

- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5-linux-amd64`
   - temel görüntü: Microsoft/DotNet: 2.1.6-Runtime-alçam 3.7
   - platformunun
      - Mimari: AMD64
      - işletim sistemi: Linux
  
- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5-linux-arm32v7`
   - temel görüntü: Microsoft/DotNet: 2.1.6-Runtime-Bionic-arm32v7
   - platformunun
      - Mimari: ARM
      - işletim sistemi: Linux
  
- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5-windows-amd64`
   - temel görüntü: Microsoft/DotNet: 2.1.6-Runtime-nanoserver-1809
   - platformunun
      - Mimari: AMD64
      - işletim sistemi: Windows
      
      
## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Stream Analytics forumunu](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)deneyin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure IoT Edge hakkında daha fazla bilgi](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)
* [IoT Edge öğreticide ASA](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics)
* [Visual Studio araçlarını kullanarak Stream Analytics Edge işleri geliştirme](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)
* [API 'Leri kullanarak Stream Analytics için CI/CD uygulayın](stream-analytics-cicd-api.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
