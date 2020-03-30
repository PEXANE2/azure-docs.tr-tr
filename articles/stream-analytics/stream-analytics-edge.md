---
title: IoT Edge üzerinde Azure Stream Analytics
description: Azure Akış Analitiği'nde kenar işleri oluşturun ve bunları Azure IoT Edge çalıştıran aygıtlara dağıtın.
ms.service: stream-analytics
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.topic: conceptual
ms.date: 03/16/2020
ms.custom: seodec18
ms.openlocfilehash: 8bb1bd018866bda9270b78507f0462b6c4d4ea17
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79475901"
---
# <a name="azure-stream-analytics-on-iot-edge"></a>IoT Edge üzerinde Azure Stream Analytics
 
IoT Edge üzerinde Azure Stream Analytics (ASA), geliştiricilerin neredeyse gerçek zamanlı analitik zekayı IoT cihazlarına daha yakın bir biçimde dağıtmasına ve böylece cihaz tarafından üretilen verilerin tüm değerini ortaya çıkarabilmesine olanak tanır. Azure Stream Analytics düşük gecikme süresi, dayanıklılık, bant genişliğinin verimli kullanımı ve uyumluluk için tasarlanmıştır. Şimdi kuruluşlar bulutta yapılan endüstriyel işlemlerin ve tamamlayıcı Büyük Veri analizinin yakınında denetim mantığı dağıtımı yapabilir.  

IoT Edge'deki Azure Akış Analizi, [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/) çerçevesi içinde çalışır. İş ASA'da oluşturulduktan sonra, IoT Hub'ı kullanarak dağıtabilir ve yönetebilirsiniz.

## <a name="scenarios"></a>Senaryolar
![IoT Edge'in üst düzey diyagramı](media/stream-analytics-edge/ASAedge-highlevel-diagram.png)

* **Düşük gecikmeli komut ve kontrol**: Örneğin, üretim güvenlik sistemleri operasyonel verilere ultra düşük gecikme süreleriyle yanıt vermelidir. IoT Edge'deki ASA ile sensör verilerini neredeyse gerçek zamanlı olarak analiz edebilir ve bir makineyi durdurmak veya uyarıları tetiklemek için anormallikleri algıladığında komutlar verebilirsiniz.
*   **Buluta sınırlı bağlantı**: Uzaktan madencilik ekipmanları, bağlı gemiler veya açık deniz sondajı gibi kritik görev sistemleri, bulut bağlantısı aralıklı olsa bile verileri analiz etmeli ve bunlara tepki vermeli. ASA ile akış mantığınız ağ bağlantısından bağımsız olarak çalışır ve daha fazla işlem veya depolama için buluta ne göndereceğinizi seçebilirsiniz.
* **Sınırlı bant genişliği**: Jet motorları veya bağlı otomobiller tarafından üretilen verilerin hacmi o kadar büyük olabilir ki, buluta gönderilmeden önce verilerin filtrelenmesi veya önceden işlenmesi gerekir. ASA'yı kullanarak buluta gönderilmesi gereken verileri filtreleyebilir veya topluabilirsiniz.
* **Uyumluluk**: Mevzuata uygunluk, bazı verilerin buluta gönderilmeden önce yerel olarak anonim hale getirilmesi veya biraraya getirilmesi gerektirebilir.

## <a name="edge-jobs-in-azure-stream-analytics"></a>Azure Akış Analizi'nde kenar işleri
### <a name="what-is-an-edge-job"></a>"Kenar" işi nedir?

ASA Edge işleri [Azure IoT Edge aygıtlarına](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)dağıtılan kapsayıcılarda çalışır. İki bölümden oluşurlar:
1.  İş tanımından sorumlu bir bulut bölümü: kullanıcılar bulutta girişleri, çıktıları, sorguyu ve diğer ayarları (sipariş dışı olaylar, vb.) tanımlar.
2.  IoT cihazlarınızda çalışan bir modül. ASA altyapısını içerir ve iş tanımını buluttan alır. 

ASA, kenar işlerini aygıta dağıtmak için IoT Hub'ı kullanır. [IoT Edge dağıtımı](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring)hakkında daha fazla bilgiyi burada bulabilirsiniz.

![Azure Akış Analizi Edge işi](media/stream-analytics-edge/stream-analytics-edge-job.png)


### <a name="installation-instructions"></a>Yükleme yönergeleri
Üst düzey adımlar aşağıdaki tabloda açıklanmıştır. Daha fazla bilgi aşağıdaki bölümlerde verilmiştir.

|      |Adım   | Notlar   |
| ---   | ---   |  ---      |
| 1   | **Depolama kapsayıcısı oluşturma**   | Depolama kapsayıcıları, iş tanımınızı IoT aygıtlarınız tarafından erişilebilen yerlerde kaydetmek için kullanılır. <br>  Varolan herhangi bir depolama kapsayıcısı yeniden kullanabilirsiniz.     |
| 2   | **ASA kenar işi oluşturma**   |  Yeni bir iş oluşturun, **barındırma ortamı**olarak **Edge'i** seçin. <br> Bu işler buluttan oluşturulur/yönetilir ve kendi IoT Edge aygıtlarınızda çalıştırılır.     |
| 3   | **IoT Edge ortamınızı cihazınıza(lar) kur**   | [Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) veya [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux)için talimatlar .          |
| 4   | **IoT Edge cihazınıza ASA'yı dağıtın**   |  ASA iş tanımı, daha önce oluşturulan depolama kapsayıcısına dışa aktarılır.       |

IoT [Edge'deki](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics) ilk ASA işinizi dağıtmak için bu adım adım öğreticiyi takip edebilirsiniz. Aşağıdaki video, Bir IoT kenar aygıtında Bir Akış Analizi işini çalıştırma işlemini anlamanıza yardımcı olur:  


> [!VIDEO https://channel9.msdn.com/Events/Connect/2017/T157/player]

#### <a name="create-a-storage-container"></a>Depolama kapsayıcısı oluşturma
ASA derlenmiş sorguyu ve iş yapılandırmasını dışa aktarmak için bir depolama kapsayıcısı gereklidir. ASA Docker görüntüsünü özel sorgunuzla yapılandırmak için kullanılır. 
1. Azure portalından bir depolama hesabı oluşturmak için [bu yönergeleri](https://docs.microsoft.com/azure/storage/common/storage-create-storage-account) izleyin. Bu hesabı ASA ile kullanmak için tüm varsayılan seçenekleri tutabilirsiniz.
2. Yeni oluşturulan depolama hesabında bir blob depolama kapsayıcısı oluşturun:
    1. **Blobs**tıklayın, sonra **+ Konteyner**. 
    2. Bir ad girin ve kapsayıcıyı **Özel**olarak tutun.

#### <a name="create-an-asa-edge-job"></a>ASA Edge işi oluşturma
> [!Note]
> Bu öğretici, Azure portalını kullanarak ASA iş oluşturma üzerine odaklanır. Ayrıca [bir ASA Edge iş oluşturmak için Visual Studio eklentisi kullanabilirsiniz](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)

1. Azure portalından yeni bir "Akış Analizi işi" oluşturun. [Burada yeni bir ASA iş oluşturmak için doğrudan bağlantı](https://ms.portal.azure.com/#create/Microsoft.StreamAnalyticsJob).

2. Oluşturma ekranında, barındırma **ortamı** olarak **Kenar'ı** seçin (aşağıdaki resme bakın)

   ![Edge'de Stream Analytics işi oluşturun](media/stream-analytics-edge/create-asa-edge-job.png)
3. İş Tanımı
    1. **Giriş Akışı(lar) tanımlayın.** İşiniz için bir veya birkaç giriş akışı tanımlayın.
    2. Başvuru verilerini tanımlayın (isteğe bağlı).
    3. **Çıkış Akışı(lar)ı tanımlayın.** İşiniz için bir veya birkaç çıktı akışı tanımlayın. 
    4. **Sorgutanımlayın.** Satır düzenleyicisini kullanarak buluttaki ASA sorgusunu tanımlayın. Derleyici, ASA kenarı için etkin olan sözdizimini otomatik olarak denetler. Ayrıca örnek veriler yükleyerek sorgunuzu test edebilirsiniz. 

4. Depolama kapsayıcısı bilgilerini **IoT Edge ayarları** menüsünde ayarlayın.

5. İsteğe bağlı ayarları ayarlama
    1. **Olay sıralama**. Portalda sıra dışı ilkeyi yapılandırabilirsiniz. Dokümantasyona [buradan](https://docs.microsoft.com/stream-analytics-query/time-skew-policies-azure-stream-analytics)ulaşabilirsiniz.
    2. **Yerel olarak.** İçleştirme biçimini ayarlayın.



> [!Note]
> Bir dağıtım oluşturulduğunda, ASA iş tanımını bir depolama kapsayıcısına dışa karamaz. Bu iş tanımı, dağıtım sırasında aynı kalır. Sonuç olarak, kenarda çalışan bir işi güncelleştirmek istiyorsanız, IŞI ASA'da güncelleştirmeniz ve ardından IoT Hub'da yeni bir dağıtım oluşturmanız gerekir.


#### <a name="set-up-your-iot-edge-environment-on-your-devices"></a>IoT Edge ortamınızı cihazınızda ayarlama(lar)
Kenar işleri, Azure IoT Edge çalıştıran aygıtlarda dağıtılabilir.
Bunun için aşağıdaki adımları izlemeniz gerekir:
- Bir Iot Hub oluşturun.
- Docker ve IoT Edge çalışma süresini kenar aygıtlarınıza yükleyin.
- IoT Hub'da cihazlarınızı **IoT Edge aygıtları** olarak ayarlayın.

Bu [adımlar, Windows](https://docs.microsoft.com/azure/iot-edge/quickstart) veya [Linux](https://docs.microsoft.com/azure/iot-edge/quickstart-linux)için IoT Edge belgelerinde açıklanmıştır.  


####  <a name="deployment-asa-on-your-iot-edge-devices"></a>IoT Edge cihazınızda ASA dağıtımı
##### <a name="add-asa-to-your-deployment"></a>Dağıtımınıza ASA ekleme
- Azure portalında IoT Hub'ı açın, **IoT Edge'e** gidin ve bu dağıtım için hedeflemek istediğiniz aygıta tıklayın.
- **Modülleri Ayarla'yı**seçin, ardından **+ Ekle'yi** seçin ve **Azure Akış Analizi Modülü'nü**seçin.
- Oluşturduğunuz abonelik ve ASA Edge işini seçin. Kaydet’e tıklayın.
![Dağıtımınızda ASA modülü ekleme](media/stream-analytics-edge/add-stream-analytics-module.png)


> [!Note]
> Bu adımda, ASA depolama kapsayıcısında "Kenar İşleri" adlı bir klasör oluşturur (zaten yoksa). Her dağıtım için "Kenar İşleri" klasöründe yeni bir alt klasör oluşturulur.
> İşinizi IoT Edge aygıtlarına dağıttığınızda, ASA iş tanımı dosyası için paylaşılan bir erişim imzası (SAS) oluşturur. SAS tuşu, aygıt ikizi kullanılarak IoT Edge aygıtlarına güvenli bir şekilde iletilir. Bu anahtarın süresi, kurulduğu günden itibaren üç yıldır. Bir IoT Edge işini güncellediğinizde, SAS değişecektir, ancak görüntü sürümü değişmez. **Güncelleştirmek'te**dağıtım iş akışını izleyin ve aygıtta bir güncelleştirme bildirimi günlüğe kaydedilir.


IoT Edge dağıtımları hakkında daha fazla bilgi için [bu sayfaya](https://docs.microsoft.com/azure/iot-edge/module-deployment-monitoring)bakın.


##### <a name="configure-routes"></a>Rotaları yapılandırma
IoT Edge, iletileri modüller arasında ve modüller ve IoT Hub arasında bildirimsel olarak yönlendirmek için bir yol sağlar. Tam sözdizimi [burada](https://docs.microsoft.com/azure/iot-edge/module-composition)açıklanmıştır.
ASA işinde oluşturulan girdi ve çıktıların adları yönlendirme için uç nokta olarak kullanılabilir.  

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
Bu örnek, aşağıdaki resimde açıklanan senaryonun yollarını gösterir. "**ASA**" adlı bir kenar işi, "**sıcaklık**" adlı bir giriş ve "**alert**" adlı bir çıktı içerir.
![İleti yönlendirme diyagramı örneği](media/stream-analytics-edge/edge-message-routing-example.png)

Bu örnekte aşağıdaki yolları tanımlar:
- **tempSensor'dan** gelen her mesaj **ASA** **adlı**modüle sıcaklık adı verilen girişe gönderilir,
- **ASA** modülünün tüm çıkışları bu cihaza bağlı IoT Hub'ına gönderilir ($upstream),
- **ASA** modülünün tüm çıkışları **tempSensor'un** **kontrol** bitiş noktasına gönderilir.


## <a name="technical-information"></a>Teknik bilgiler
### <a name="current-limitations-for-iot-edge-jobs-compared-to-cloud-jobs"></a>IoT Edge işleri için bulut işleri ile karşılaştırıldığında geçerli sınırlamalar
Amaç, IoT Edge işleri ile bulut işleri arasında eşitliğe sahip olmaktır. Çoğu SQL sorgu dili özelliği desteklenir ve hem bulutta hem de IoT Edge'de aynı mantığı çalıştırmayı sağlar.
Ancak kenar işleri için aşağıdaki özellikler henüz desteklenmez:
* JavaScript'te kullanıcı tanımlı işlevler (UDF). [UDF, IoT Edge işleri için C#](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-edge-csharp-udf) (önizleme) olarak kullanılabilir.
* Kullanıcı tanımlı agregalar (UDA).
* Azure ML işlevleri.
* Tek bir adımda 14'ten fazla toplam kullanma.
* Giriş/çıkış için AVRO formatı. Şu anda yalnızca CSV ve JSON desteklenir.
* Aşağıdaki SQL işleçleri:
    * BÖLÜM E GÖRE
    * GetMetadataPropertyValue
* Geç varış politikası

### <a name="runtime-and-hardware-requirements"></a>Çalışma süresi ve donanım gereksinimleri
IoT Edge'de ASA çalıştırmak için [Azure IoT Edge](https://azure.microsoft.com/campaigns/iot-edge/)çalıştırabilen aygıtlara ihtiyacınız vardır. 

ASA ve Azure IoT Edge, birden çok ana bilgisayar işletim sisteminde (Windows, Linux) çalışan taşınabilir bir çözüm sağlamak için **Docker** kapsayıcılarını kullanır.

Asa IoT Edge Windows ve Linux görüntüleri olarak kullanılabilir hale getirilir, hem x86-64 veya ARM (Gelişmiş RISC Makineleri) mimarileri çalışan. 


### <a name="input-and-output"></a>Girdi ve çıktı
#### <a name="input-and-output-streams"></a>Giriş ve Çıkış Akışları
ASA Edge işleri, IoT Edge aygıtlarında çalışan diğer modüllerden giriş ve çıktı alabilir. Belirli modüllere bağlanmak için yönlendirme yapılandırmasını dağıtım zamanında ayarlayabilirsiniz. Daha fazla bilgi [IoT Edge modül kompozisyon belgeleri](https://docs.microsoft.com/azure/iot-edge/module-composition)açıklanmıştır.

Hem giriş hem de çıkışlar için CSV ve JSON biçimleri desteklenir.

ASA işinizde oluşturduğunuz her giriş ve çıkış akışı için, dağıtılan modülünüzde karşılık gelen bir uç nokta oluşturulur. Bu uç noktalar dağıtımınızın yollarında kullanılabilir.

Şu anda desteklenen tek akış girişi ve akış çıktısı türleri Edge Hub'dır. Başvuru girişi başvuru dosyası türünü destekler. Diğer çıktılara akış aşağı bir bulut iş kullanılarak ulaşılabilir. Örneğin, Edge'de barındırılan bir Akış Analizi işi, çıktıyı Edge Hub'a gönderir ve çıktıyı IoT Hub'a gönderebilir. IoT Hub'dan gelen girdiyle ikinci bir bulut barındırılan Azure Akışı Analytics işini ve Power BI veya başka bir çıktı türüne çıkış kullanabilirsiniz.



##### <a name="reference-data"></a>Referans verileri
Başvuru verileri (arama tablosu olarak da bilinir), statik veya doğada yavaş değişen sonlu bir veri kümesidir. Bir arama gerçekleştirmek veya veri akışınız ile ilişkilendirmek için kullanılır. Azure Akış Analizi işinizde referans verilerini kullanmak için, genellikle sorgunuzda bir [Referans Veri BİrLEŞTİrME](https://docs.microsoft.com/stream-analytics-query/reference-data-join-azure-stream-analytics) kullanırsınız. Daha fazla bilgi [için, Stream Analytics'teki aramalar için başvuru verilerini kullanma'ya](stream-analytics-use-reference-data.md)bakın.

Yalnızca yerel başvuru verileri desteklenir. Bir iş IoT Edge aygıtına dağıtıldığında, kullanıcı tanımlı dosya yolundan referans verilerini yükler.

Edge'de başvuru verileri olan bir iş oluşturmak için:

1. İşiniz için yeni bir girdi oluşturun.

2. **Kaynak Türü**olarak **Başvuru verilerini** seçin.

3. Aygıtta bir başvuru veri dosyası hazır bulun. Bir Windows kapsayıcısı için, başvuru veri dosyasını yerel sürücüye koyun ve yerel sürücüyü Docker kapsayıcısıyla paylaşın. Bir Linux kapsayıcısı için bir Docker birimi oluşturun ve veri dosyasını ses düzeyine doldurun.

4. Dosya yolunu ayarlayın. Windows Host OS ve Windows kapsayıcısı `E:\<PathToFile>\v1.csv`için mutlak yolu kullanın: . Bir Windows Host OS ve Linux kapsayıcısı veya Linux işletim sistemi `<VolumeName>/file1.txt`ve Linux kapsayıcısı için, ses düzeyindeki yolu kullanın: .

![IoT Edge'deki Azure Akış Analizi işi için yeni başvuru veri girişi](./media/stream-analytics-edge/Reference-Data-New-Input.png)

IoT Edge güncelleştirmesindeki başvuru verileri bir dağıtım tarafından tetiklenir. Tetiklendikten sonra, ASA modülü çalışan işi durdurmadan güncelleştirilmiş verileri seçer.

Başvuru verilerini güncelleştirmenin iki yolu vardır:
* ASA işinizdeki başvuru veri yolunu Azure portalından güncelleştirin.
* IoT Edge dağıtımını güncelleştirin.

## <a name="license-and-third-party-notices"></a>Lisans ve üçüncü taraf bildirimleri
* [IoT Edge lisansında Azure Akış Analizi.](https://go.microsoft.com/fwlink/?linkid=862827) 
* [IoT Edge'deki Azure Akış Analizi için üçüncü taraf bildirimi.](https://go.microsoft.com/fwlink/?linkid=862828)

## <a name="azure-stream-analytics-module-image-information"></a>Azure Akış Analizi modülü görüntü bilgileri 

Bu sürüm bilgileri en son 2019-06-27 tarihinde güncellenmiştir:

- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5-linux-amd64`
   - temel resim: microsoft/dotnet:2.1.6-runtime-alpine3.7
   - Platform:
      - Mimari: amd64
      - os: linux
  
- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5-linux-arm32v7`
   - temel görüntü: microsoft/dotnet:2.1.6-runtime-bionic-arm32v7
   - Platform:
      - mimari: kol
      - os: linux
  
- Görüntü: `mcr.microsoft.com/azure-stream-analytics/azureiotedge:1.0.5-windows-amd64`
   - temel görüntü: microsoft/dotnet:2.1.6-runtime-nanoserver-1809
   - Platform:
      - Mimari: amd64
      - os: windows
      
      
## <a name="get-help"></a>Yardım alın
Daha fazla yardım için [Azure Akışı Analizi forumuna](https://social.msdn.microsoft.com/Forums/azure/home?forum=AzureStreamAnalytics)katılın.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Iot Edge hakkında daha fazla bilgi](https://docs.microsoft.com/azure/iot-edge/how-iot-edge-works)
* [ASA Üzerinde IoT Edge öğretici](https://docs.microsoft.com/azure/iot-edge/tutorial-deploy-stream-analytics)
* [Visual Studio araçlarını kullanarak Stream Analytics Edge işlerini geliştirin](https://docs.microsoft.com/azure/stream-analytics/stream-analytics-tools-for-visual-studio-edge-jobs)
* [API'leri kullanarak Stream Analytics için CI/CD uygulayın](stream-analytics-cicd-api.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
