---
title: Hava durumu iş ortaklarından hava durumu verileri alın
description: Bu makalede, ortaklardan hava durumu verilerinin nasıl alınılabildiğini açıklanmaktadır.
author: sunasing
ms.topic: article
ms.date: 03/31/2020
ms.author: sunasing
ms.openlocfilehash: 66fa4e7d3edf839ab1e497e86362bcfc979dc279
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81266168"
---
# <a name="get-weather-data-from-weather-partners"></a>Hava durumu iş ortaklarından hava durumu verileri alın

Azure FarmBeats, docker tabanlı Bir Bağlayıcı Çerçevesi kullanarak hava durumu veri sağlayıcınızdan(lar) hava durumu verileri getirmenize yardımcı olur. Bu çerçeveyi kullanarak, hava durumu veri sağlayıcıları FarmBeats ile entegre edilebilen bir docker uygular. Şu anda aşağıdaki hava durumu veri sağlayıcıları desteklenir:

[Azure Açık Veri Kümelerinden NOAA verileri](https://azure.microsoft.com/services/open-datasets/)

Hava durumu verileri, farmbeats'te işlem yapılabilir öngörüler oluşturmak ve AI/ML modelleri oluşturmak için kullanılabilir.

## <a name="before-you-start"></a>Başlamadan önce

Hava durumu verilerini almak için FarmBeats yüklü olduğundan emin olun. **Hava durumu entegrasyonu sürüm 1.2.11 veya daha yüksek olarak desteklenir.** Azure FarmBeats'i yüklemek için [FarmBeats'i yükleyin.](https://aka.ms/farmbeatsinstalldocumentation)

## <a name="enable-weather-integration-with-farmbeats"></a>FarmBeats ile hava durumu entegrasyonunu etkinleştirin

FarmBeats Veri hub'ınızda hava durumu verilerini almaya başlamak için aşağıdaki adımları izleyin:

1. FarmBeats Veri merkezinize gidin (https://yourdatahub.azurewebsites.net/swagger)

2. /İş Ortağı API'sine gidin ve aşağıdaki giriş yüküyle bir POST isteğinde bulunun:

   ```json
   {  
 
     "dockerDetails": {  
       "credentials": { 
         "username": "<credentials to access private docker - not required for public docker>", 
         "password": "<credentials to access private docker – not required for public docker>"  
       },  
       "imageName" : "<docker image name. Default is azurefarmbeats/fambeats-noaa>",
       "imageTag" : "<docker image tag, default:latest>",
       "azureBatchVMDetails": {  
         "batchVMSKU" : "<VM SKU. Default is standard_d2_v2>",  
         "dedicatedComputerNodes" : 1,
         "nodeAgentSKUID": "<Node SKU. Default is batch.node.ubuntu 18.04>"  
       }
     },
     "partnerCredentials": {  
       "key1": "value1",  
       "key2": "value2"  
     },  
     "partnerType": "Weather",  
     "name": "<Name of the partner>",  
     "description": "<Description>",
     "properties": {  }  
   }  
   ```

   Örneğin, Azure Açık Veri Kümeleri ile NOAA'dan hava durumu verilerini almak için aşağıdaki yükü kullanın. Tercihinize göre adı ve açıklamayı değiştirebilirsiniz.

   ```json
   {
 
     "dockerDetails": {
       "imageName": "azurefarmbeats/farmbeats-noaa",
       "imageTag": "latest",
       "azureBatchVMDetails": {
         "batchVMSKU": "standard_d2_v2",
         "dedicatedComputerNodes": 1,
         "nodeAgentSKUID": "batch.node.ubuntu 18.04"
       }
     },
     "partnerType": "Weather",
     "name": "ods-noaa",
     "description": "NOAA data from Azure Open Datasets registered as a Weather Partner"
   }  
   ```

   > [!NOTE]
   > Ortak nesnesi hakkında daha fazla bilgi için [Ek'e](get-weather-data-from-weather-partner.md#appendix) bakın

   Önceki adım, docker'ın müşterinin FarmBeats ortamında çalışmasını sağlamak için kaynakları sağlayacaktır.  

   Yukarıdaki kaynakların sağlanması yaklaşık 10-15 dakika sürer.

3. Adım 2'de oluşturduğunuz /İş Ortağı nesnesinin durumunu denetleyin. Bunu yapmak için/Ortak API'da GET isteğinde bulunun ve ortak nesnenin **durumunu** denetleyin. FarmBeats ortağı başarıyla bir şekilde bir şekilde karşıladıktan sonra, durum **Etkin**olarak ayarlanır.

4. /JobType API'ye gidin ve aynı konuda get isteğinde bulunun. Adım 2'deki İş Ortağı ekleme işleminin bir parçası olarak oluşturulan hava durumu işlerini denetleyin. Hava işlerinde **pipelineName** alanı aşağıdaki formatta olacaktır: "ortak-name_partner-type_job-name".

5. Şimdi FarmBeats örneğinizin etkin bir hava durumu veri ortağı vardır ve belirli bir konum (enlem/boylam) ve bir tarih aralığı için hava durumu verileri istemek için işleri çalıştırabilirsiniz. JobType(lar) hava işleri çalıştırmak için gereken parametreler hakkında ayrıntılı bilgi olacaktır.

   Örneğin, Azure Açık Veri Kümelerinden NOAA verileri için Aşağıdaki İş Tipi(ler) oluşturulur:

   - get_weather_data (ISD/geçmiş hava durumu verilerini alın)
   - get_weather_forecast_data (GFS/tahmin hava durumu verilerini alın)

6. **Id** ve JobType(lar) parametrelerini not edin.

7. /Jobs API'sine gidin ve /Jobs'ta aşağıdaki giriş yüküyle bir POST isteğinde bulunun:

   ```json
    {
         "typeId": "<id of the JobType>",
         "arguments": {
           "additionalProp1": {},
           "additionalProp2": {},
           "additionalProp3": {}
         },
         "name": "<name of the job>",
         "description": "<description>",
         "properties": {}
       }
   ```

   Örneğin, **get_weather_data**çalıştırmak için aşağıdaki yükü kullanın:

   ```json
   {
 
         "typeId": "<id of the JobType>",
         "arguments": {
               "latitude": 47.620422,
               "longitude": -122.349358,
               "start_date": "yyyy-mm-dd",
               "end_date": "yyyy-mm-dd"
         },
         "name": "<name of the job>",
         "description": "<description>",
         "properties": {}
   }
   ```

8. Önceki adım, iş ortağı docker'da tanımlandığı gibi hava durumu işlerini çalıştıracak ve farmbeats'e hava durumu verilerini yutacaktır. /Jobs'ta GET isteği nde bulunarak işin durumunu kontrol edebilir ve yanıtta **currentState'i** alabilirsiniz. Tamamlandıktan sonra, geçerliDurum **Başarılı**olarak ayarlanır.

## <a name="query-ingested-weather-data"></a>Yutulan hava durumu verilerini sorgulama

Hava durumu işleri tamamlandıktan sonra, modelleri veya işlem yapılabilir öngörüler oluşturmak için yutulan hava durumu verilerini sorgulayabilirsiniz. FarmBeats'ten hava durumu verilerine erişmenin ve sorgulanmanın iki yolu vardır:

- API ve
- Zaman Serisi Öngörüleri (TSI).

### <a name="query-using-rest-api"></a>REST API kullanarak sorgu

FarmBeats REST API'yi kullanarak hava durumu verilerini sorgulamak için aşağıdaki adımları izleyin:

1. FarmBeats Veri hub'ınızdahttps://yourdatahub.azurewebsites.net/swagger)(, WeatherDataLocation API'ye gidin ve get isteğinde bulunun). Yanıt, iş çalışmasının bir parçası olarak belirtilen konum (enlem/boylam) için /WeatherDataLocation nesnesi(ler) oluşturulur. **Kimliği** ve nesnenin **weatherDataModelId(lar)** bir not ayapın.

2. Adım 1'de belirtildiği gibi **weatherDataModelId** için /WeatherDataModel API'de GET/{id} yapın. "Hava Durumu Veri Modeli" yutulan hava verileri yle ilgili tüm meta verilere ve ayrıntılara sahiptir. Örneğin, Hava Durumu **Veri Modeli** nesnesi içindeki **Hava Durumu Ölçümü,** hangi hava durumu bilgilerinin desteklendirilip hangi tür ve birimlerde desteklendirilenhakkında ayrıntılara sahiptir. Örneğin,

   ```json
   {
       "name": "Temperature <name of the weather measure - this is what we will receive as part of the queried weather data>",
       "dataType": "Double <Data Type - eg. Double, Enum>",
       "type": "AmbientTemperature <Type of measure eg. AmbientTemperature, Wind speed etc.>",
       "unit": "Celsius <Unit of measure eg. Celsius, Percentage etc.>",
       "aggregationType": "None <either of None, Average, Maximum, Minimum, StandardDeviation, Sum, Total>",
       "description": "<Description of the measure>"
   }
   ```

   Hava Durumu Veri Modeli için GET/{id} çağrısından yanıta dikkat edin.

3. **Telemetri** API'sine gidin ve aşağıdaki giriş yüküyle bir POST isteğinde bulunun:

   ```json
   {
       "weatherDataLocationId": "<id from step 1 above>",
     "searchSpan": {
       "from": "2020-XX-XXT07:30:00Z",
       "to": "2020-XX-XXT07:45:00Z"
     }
   }
   ```

4. Belirtilen zaman aralığı için kullanılabilir hava durumu verilerini içeren yanıt aşağıdaki gibi görünecektir:

   ```json
   {
     "timestamps": [
       "2020-XX-XXT07:30:00Z",
       "2020-XX-XXT07:45:00Z"
     ],
     "properties": [
       {
         "values": [
           "<id of the weatherDataLocation>",
           "<id of the weatherDataLocation>"
         ],
         "name": "Id",
         "type": "String"
       },
       {
         "values": [
           29.1,
           30.2
         ],
         "name": "Temperature <name of the WeatherMeasure as defined in the WeatherDataModel object>",
         "type": "Double <Data Type of the value - eg. Double>"
       }
     ]
   }
   ```

Önceki örnekte, yanıt, ölçü adı ("Sıcaklık") ile birlikte iki zaman damgası için veri ve iki zaman damgası bildirilen hava durumu verilerinin değerleri vardır. Bildirilen değerlerin türünü ve birimini yorumlamak için ilişkili Hava Durumu Veri Modeline (yukarıdaki adım 2'de açıklandığı gibi) başvurmanız gerekir.

### <a name="query-using-azure-time-series-insights-tsi"></a>Azure Zaman Serisi Öngörülerini Kullanarak Sorgula (TSI)

FarmBeats, IoT ölçeğinde verileri yüksek düzeyde bağlamsallaştırılmış ve zaman serileri için optimize edilmiş verileri yutmak, depolamak, sorgulamak ve görselleştirmek için [Azure Time Series Öngörüleri 'ni (TSI)](https://azure.microsoft.com/services/time-series-insights/) kullanır.

Hava durumu verileri bir EventHub'dan alınır ve farmbeats kaynak grubu içindeki bir TSI ortamına itilir. Veriler daha sonra doğrudan TSI'den sorgulanabilir. Daha fazla bilgi için [TSI belgelerine](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer)bakın.

TSI'deki verileri görselleştirmek için aşağıdaki adımları izleyin:

1. Azure **portalı** > **FarmBeats DataHub kaynak grubuna** gidin > **Zaman Serisi Öngörüleri** ortamını (tsi-xxxx) > Veri Erişim **İlkeleri'ni**seçin. Reader veya Contributor erişimi olan kullanıcı ekleyin.

2. **Time Series Insights** ortamının **Genel Bakış** sayfasına (tsi-xxxx) gidin ve Time **Series Insights Explorer URL'sini**seçin. Artık yutulan hava durumu verilerini görselleştirebilirsiniz.

TSI, hava durumu verilerinin depolanması, sorgulanması ve görselleştirilmesinin yanı sıra Power BI panosuna tümleştirme de sağlar. Daha fazla bilgi için Power [BI'deki Time Series Insights'taki verileri Görselleştir'e](https://docs.microsoft.com/azure/time-series-insights/how-to-connect-power-bi)bakın.

## <a name="appendix"></a>Ek

|        İş Ortağı   |  Ayrıntılar   |
| ------- | -------             |
|     DockerDetails - imageName         |          Docker resim adı. Örneğin, docker.io/azurefarmbeats/farmbeats-noaa (hub.docker.com resim) VEYA myazureacr.azurecr.io/mydockerimage (Azure Kapsayıcı Kayıt Defteri'ndeki resim) vb. Kayıt defteri sağlanmadıysa, varsayılan hub.docker.com      |
|          DockerDetails - resimTag             |         Docker görüntüsünün etiket adı. Varsayılan "en son"     |
|  DockerDetails - kimlik bilgileri      |  Özel docker erişmek için kimlik bilgileri. Bu müşteriye ortak tarafından sağlanacaktır   |
|  DockerDetails - azureBatchVMDetails - batchVMSKU     |    Azure Toplu VM SKU. Mevcut tüm Linux sanal makineleri için [buraya](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın. Geçerli değerler şunlardır: "Küçük', 'ExtraLarge', 'Büyük', 'A8', 'A9', 'Orta', 'A5', 'A6', 'A7', 'STANDARD_D1', 'STANDARD_D2', 'STANDARD_D3', 'STANDARD_D4', 'STANDARD_D11', 'STANDARD_D12', 'STANDARD_D13', 'STANDARD_D14', 'A10', 'A11', 'STANDARD_D1_V2', 'STANDARD_D2_V2', 'STANDARD_D3_V2', 'STANDARD_D4_V2', 'STANDARD_D11_V2', 'STANDARD_D12_V2', 'STANDARD_D13_V2', 'STANDARD_D14_V2', 'STANDARD_G1', 'STANDARD_G2'STANDARD_G3, 'STANDARD_G4' , 'STANDARD_G5', 'STANDARD_D5_V2', 'BASIC_A1', 'BASIC_A2', 'BASIC_A3', 'BASIC_A4', 'STANDARD_A1', 'STANDARD_A2', 'STANDARD_A3', 'STANDARD_A4', 'STANDARD_A5', 'STANDARD_A6', 'STANDARD_A7', 'STANDARD_A8', 'STANDARD_A9', 'STANDARD_A10', 'STANDARD_A11', 'STANDARD_D15_V2', 'STANDARD_F1', 'STANDARD_F2', 'STANDARD_F4', 'STANDARD_F8', 'STANDARD_F16', 'STANDARD_NV6', 'STANDARD_NV12', 'STANDARD_NV24'STANDARD_NC24r STANDARD_NC24, 'STANDARD_NC6', 'STANDARD_NC12' STANDARD_NC12 , 'STANDARD_H8', 'STANDARD_H8m', 'STANDARD_H16', 'STANDARD_H16m', 'STANDARD_H16mr', 'STANDARD_H16r', 'STANDARD_A1_V2', 'STANDARD_A2_V2', 'STANDARD_A4_V2', 'STANDARD_A8_V2', 'STANDARD_A2m_V2', 'STANDARD_A4m_V2', 'STANDARD_A8m_V2', 'STANDARD_M64ms', 'STANDARD_M128s', 'STANDARD_D2_V3'. **Varsayılan "standard_d2_v2"**  |
|    DockerDetails - azureBatchVMDetails - dedicatedComputerNodes   |  Hayır. toplu havuz için özel bilgisayar düğümleri. Varsayılan değer 1'dir. |
|    DockerDetails - azureBatchVMDetails - nodeAgentSKUID          |    Azure Toplu İş Düğümü Aracısı SKU Kimliği. Şu anda yalnızca "batch.node.ubuntu 18.04" toplu düğüm aracısı desteklenir.    |
| DockerDetails - ortak Kimlik Bilgileri | docker ortağı nda ortak API'yi aramak için kimlik bilgileri. İş ortağı, örneğin desteklenen auth mekanizmasına dayalı olarak müşterilerine bu bilgileri vermek gerekir. Kullanıcı adı/parola veya API Tuşları. |
| partnerYazın | "Hava durumu" (FarmBeats'deki diğer ortak tipleri "Sensör" ve "Görüntüler")  |
|  ad   |   FarmBeats sisteminde ortağın istenilen adı   |
|  açıklama |  Açıklama   |

## <a name="next-steps"></a>Sonraki adımlar

Azure FarmBeats örneğinizden gelen sensör verilerini şimdi sorguladınız. Şimdi, çiftlikleriniz için haritaları nasıl [oluşturacağınızı](generate-maps-in-azure-farmbeats.md#generate-maps) öğrenin.
