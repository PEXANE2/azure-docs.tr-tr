---
title: Hava durumu ortaklarından Hava durumu verileri alın
description: Bu makalede, iş ortaklarından Hava durumu verilerinin nasıl alınacağı açıklanır.
author: sunasing
ms.topic: article
ms.date: 03/31/2020
ms.author: sunasing
ms.openlocfilehash: 4ab42509930e76989a67f45deb33e370e6e9adf4
ms.sourcegitcommit: 3988965cc52a30fc5fed0794a89db15212ab23d7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85194748"
---
# <a name="get-weather-data-from-weather-partners"></a>Hava durumu ortaklarından Hava durumu verileri alın

Azure Farmker, bir Docker tabanlı bağlayıcı çerçevesi kullanarak hava durumu veri sağlayıcılarınızın Hava durumu verilerini taşımanıza yardımcı olur. Bu çerçeveyi kullanarak, hava durumu veri sağlayıcıları Farmker 'Lar ile tümleştirilebilen bir Docker uygular. Şu anda aşağıdaki Hava durumu veri sağlayıcıları desteklenir:

[DTN](https://www.dtn.com/dtn-content-integration/)

Hava durumu verileri, eyleme dönüştürülebilir içgörüler oluşturmak ve Farmtları üzerinde AI/ML modelleri oluşturmak için kullanılabilir.

## <a name="before-you-start"></a>Başlamadan önce

Hava durumu verilerini almak için, Farmtts 'yi yüklediğinizden emin olun. **Hava durumu tümleştirmesi sürüm 1.2.11 veya üzeri sürümlerde desteklenir**. Azure Farmtts 'yi yüklemek için bkz. [Farmtempts 'Yi Install](https://aka.ms/farmbeatsinstalldocumentation).

## <a name="enable-weather-integration-with-farmbeats"></a>Farmtempolar ile hava durumu tümleştirmesini etkinleştirme

Farmtts veri hub 'ında Hava durumu verilerini almaya başlamak için aşağıdaki adımları izleyin:

1. Farmtts Data hub Swagger (https://farmbeatswebsite-api.azurewebsites.net/swagger)

2. /Partner API 'sine gidin ve aşağıdaki giriş yüküne sahip bir POST isteği oluşturun:

   ```json
   {  
 
     "dockerDetails": {  
       "credentials": { 
         "username": "<credentials to access private docker - not required for public docker>", 
         "password": "<credentials to access private docker – not required for public docker>"  
       },  
       "imageName" : "<docker image name",
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

   Örneğin, DTN 'den Hava durumu verileri almak için aşağıdaki yükü kullanın. Ad ve açıklamayı tercihlerinize göre değiştirebilirsiniz.

   ```json
   {
 
     "dockerDetails": {
       "imageName": "dtnweather/dtn-farm-beats",
       "imageTag": "latest",
       "azureBatchVMDetails": {
         "batchVMSKU": "standard_d2_v2",
         "dedicatedComputerNodes": 1,
         "nodeAgentSKUID": "batch.node.ubuntu 18.04"
       }
     },
     "partnerCredentials": {
      "apikey": "<API key from DTN>"
      },
     "partnerType": "Weather",
     "name": "dtn-weather",
     "description": "DTN registered as a Weather Partner in FarmBeats"
   }  
   ```

   > [!NOTE]
   > Iş ortağı nesnesi hakkında daha fazla bilgi için bkz. [ek](get-weather-data-from-weather-partner.md#appendix)

   Önceki adımda, Docker 'ın müşterinin Farmker ortamında çalışmasını sağlamak için kaynaklar sağlanacak.  

   Yukarıdaki kaynakları sağlamak yaklaşık 10-15 dakika sürer.

3. 2. adımda oluşturduğunuz/partner nesnesinin durumunu denetleyin. Bunu yapmak için,/partner API 'sinde bir GET isteği yapın ve iş ortağı nesnesinin **durumunu** denetleyin. Farmtler iş ortağını başarıyla sağlar, durum **etkin**olarak ayarlanır.

4. /JobType API ' ye gidin ve aynı üzerinde bir GET isteği yapın. 2. adımda Iş ortağı ekleme sürecinin bir parçası olarak oluşturulan hava durumu işlerini denetleyin. Hava durumu işlerinde **ardışık düzen adı** alanı şu biçimde olacaktır: "iş ortağı-name_partner-type_job-adı".

5. Artık Farmtts örneğinizin etkin bir hava durumu veri ortağı vardır ve belirli bir konum (Enlem/Boylam) ve tarih aralığı için hava durumu verileri istemek üzere işleri çalıştırabilirsiniz. İş türleri, hava durumu işlerini çalıştırmak için gereken parametrelerin ayrıntılarına sahip olacaktır.

   Örneğin, DTN için şu JobType 'lar oluşturulacak:
   
   - get_dtn_daily_observations (bir konum ve zaman aralığı için günlük gözlemlerinizi al)
   - get_dtn_daily_forecasts (bir konum ve zaman aralığı için günlük tahminleri al)
   - get_dtn_hourly_observations (bir konum ve zaman aralığı için saatlik gözlemleri al)
   - get_dtn_hourly_forecasts (bir konum ve zaman aralığı için saatlik Tahminleri Al)

6. JobType (s) öğesinin **kimliğini** ve parametrelerini bir yere göz önünde alın.

7. /Jobs API 'ye gidin ve aşağıdaki giriş yüküne sahip/Jobs üzerinde bir POST isteği yapın:

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

   Örneğin, **get_dtn_daily_observations**çalıştırmak için aşağıdaki yükü kullanın:

   ```json
   { 
         "typeId": "<id of the JobType>",
         "arguments": {
               "latitude": 47.620422,
               "longitude": -122.349358,
               "days": 5
         },
         "name": "<name of the job>",
         "description": "<description>",
         "properties": {}
   }
   ```

8. Yukarıdaki adım, iş ortağı Docker 'da tanımlanan ve hava durumu verilerini Farmcts 'ye alacak şekilde çalıştırır. İş üzerinde bir GET isteği yaparak işin durumunu denetleyebilir ve yanıtta **CurrentState** araması yapabilirsiniz. Tamamlandıktan sonra, currentState **başarılı**olarak ayarlanır.

## <a name="query-ingested-weather-data"></a>Sorgu alınan hava durumu verileri

Hava durumu işleri tamamlandıktan sonra modeller veya eyleme dönüştürülebilir içgörüler oluşturmak için alınan hava durumu verilerini sorgulayabilirsiniz. Farmman 'daki Hava durumu verilerine erişmek ve bunları sorgulamak için iki yol vardır:

- API ve
- Time Series Insights (TSI).

### <a name="query-using-rest-api"></a>REST API kullanarak sorgulama

REST API kullanarak hava durumu verilerini sorgulamak için aşağıdaki adımları izleyin:

1. Farmkörler veri merkezi Swagger ( https://yourdatahub.azurewebsites.net/swagger) ,/dalgalı iş DATALOCATION API 'sine gidin ve bır get isteği yapın. Yanıtta, işin bir parçası olarak belirtilen konum (Enlem/Boylam) için/dalgalı işlem datalocation nesneleri oluşturulur. Nesne (ler) in **kimliğini** ve **dalgalı veri modetiğine** dikkat edin.

2. Adım 1 ' de belirtildiği gibi, **dalgalı veri ModelId** için/dalgalı therdatamodel API 'sinde Get/{ID} yapın. "Hava durumu veri modeli", alınan hava durumu verileriyle ilgili tüm meta verileri ve ayrıntıları içerir. Örneğin, hava durumu **veri modeli** nesnesi içindeki **Hava durumu ölçümü** , hangi hava durumu bilgilerinin desteklenmekte olduğunu ve hangi tür ve birimler hakkında ayrıntılı bilgiler içerir. Örneğin,

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

   Hava durumu veri modeli için GET/{id} çağrısından gelen yanıtı bir yere unutmayın.

3. **Telemetri** API 'sine gidin ve aşağıdaki giriş yüküne sahıp bir post isteği oluşturun:

   ```json
   {
       "weatherDataLocationId": "<id from step 1 above>",
     "searchSpan": {
       "from": "2020-XX-XXT07:30:00Z",
       "to": "2020-XX-XXT07:45:00Z"
     }
   }
   ```

4. Belirtilen zaman aralığı için kullanılabilir hava durumu verilerini içeren yanıt şöyle görünür:

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

Yukarıdaki örnekte, yanıtın iki zaman damgalarına ilişkin veriler, ölçü adı ("sıcaklık") ve raporlanan Hava durumu verilerinin iki zaman damgalarıyla birlikte verileri vardır. Bildirilen değerlerin türünü ve birimini yorumlamak için ilgili hava durumu veri modeline (yukarıdaki 2. adım bölümünde açıklandığı gibi) başvurmanız gerekir.

### <a name="query-using-azure-time-series-insights-tsi"></a>Azure Time Series Insights kullanarak sorgulama (TSI)

Farmınts, IoT ölçeğinde verileri almak, depolamak, sorgulamak ve görselleştirmek için [Azure Time Series Insights (TSI)](https://azure.microsoft.com/services/time-series-insights/) kullanır. Bu, zaman serisi için en iyi duruma getirilen ve en iyileştirilmiş veri.

Hava durumu verileri bir EventHub üzerinde alınır ve ardından Farmtts kaynak grubu içindeki bir TSI ortamına gönderilir. Veriler daha sonra doğrudan TSI 'dan sorgulanabilir. Daha fazla bilgi için bkz. [TSI belgeleri](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer).

TSI üzerinde verileri görselleştirmek için adımları izleyin:

1. **Azure Portal**  >  **farmrets Datahub kaynak grubuna** gidin > **Time Series Insights** ortamı (TSİ-xxxx >) **veri erişim ilkeleri**' ni seçin. Okuyucu veya katkıda bulunan erişimi olan kullanıcı ekleyin.

2. **Time Series Insights** ortamının (TSİ-xxxx) **genel bakış** sayfasına gidin ve **Time Series Insights Explorer URL 'sini**seçin. Artık alınan hava durumu verilerini görselleştirebilirsiniz.

Hava durumu verilerini depolama, sorgulama ve görselleştirmenin yanı sıra, TSI Power BI panosu ile tümleştirmeyi de sağlar. Daha fazla bilgi için bkz. [Power BI Time Series Insights verileri görselleştirme](https://docs.microsoft.com/azure/time-series-insights/how-to-connect-power-bi).

## <a name="appendix"></a>Ek

|        İş Ortağı   |  Ayrıntılar   |
| ------- | -------             |
|     DockerDetails-GörüntüAdı         |          Docker görüntü adı. Örneğin, docker.io/mydockerimage (görüntü hub.docker.com) veya myazureacr.azurecr.io/mydockerimage (Azure Container Registry resim) ve benzeri. Hiçbir kayıt defteri sağlanmazsa varsayılan değer hub.docker.com      |
|          DockerDetails-ImageTag             |         Docker görüntüsünün etiket adı. Varsayılan değer "en son"     |
|  DockerDetails-kimlik bilgileri      |  Özel Docker 'a erişim için kimlik bilgileri. Bu, iş ortağı tarafından müşteriye sunulacaktır   |
|  DockerDetails-azureBatchVMDetails-batchVMSKU     |    Azure Batch VM SKU 'SU. Kullanılabilir tüm Linux sanal makineleri için [buraya](https://docs.microsoft.com/azure/virtual-machines/linux/sizes?toc=%2fazure%2fvirtual-machines%2flinux%2ftoc.json) bakın. Geçerli değerler şunlardır: "küçük", ' Extrabüyük ', ' büyük ', ' A8 ', ' A9 ', ' Medium ', ' a5 ', ' a6 ', ' a7 ', ' STANDARD_D1 ', ' STANDARD_D2 ', ' STANDARD_D3 ', ' STANDARD_D4 ', ' STANDARD_D11 ', ' STANDARD_D12 ', ' STANDARD_D13 ', ' STANDARD_D14 ', ' a10 ', ' a11 ', ' STANDARD_D1_V2 ', ' STANDARD_D2_V2 ', ' STANDARD_D3_V2 ', ' STANDARD_D4_V2 ', ' STANDARD_D11_V2 ', ' STANDARD_D12_V2 ', ' STANDARD_D13_V2 ', ' STANDARD_D14_V2 ', ' STANDARD_G1 ' , ' STANDARD_G5 ', ' STANDARD_D5_V2 ', ' BASIC_A1 ', ' BASIC_A2 ', ' BASIC_A3 ', ' BASIC_A4 ', ' STANDARD_A1 ', ' STANDARD_A2 ', ' STANDARD_A3 ', ' STANDARD_A4 ', ' STANDARD_A5 ', ' STANDARD_A6 ', ' STANDARD_A7 ', ' STANDARD_A8 ', ' STANDARD_A9 ', ' STANDARD_A10 ', ' STANDARD_A11 ', ' STANDARD_D15_V2 ', ' STANDARD_F1 ', ' STANDARD_F2 ', ' STANDARD_F4 ', ' STANDARD_F8 ', ' STANDARD_F16 ', ' STANDARD_NV6 ', ' STANDARD_NV12 ', ' STANDARD_NV24 ', ' STANDARD_NC6 ' , ' STANDARD_H8 ', ' STANDARD_H8m ', ' STANDARD_H16 ', ' STANDARD_H16m ', ' STANDARD_H16mr ', ' STANDARD_H16r ', ' STANDARD_A1_V2 ', ' STANDARD_A2_V2 ', ' STANDARD_A4_V2 ', ' STANDARD_A8_V2 ', ' STANDARD_A2m_V2 ', ' STANDARD_A4m_V2 ', ' STANDARD_A8m_V2 ', ' STANDARD_M64ms ', ' STANDARD_M128s ', ' STANDARD_D2_V3 '. **Varsayılan değer "standard_d2_v2"**  |
|    DockerDetails-azureBatchVMDetails ile ayrılmış bilgisayar düğümleri   |  Hayır. toplu iş havuzu için ayrılmış bilgisayar düğümleri. Varsayılan değer 1 ' dir. |
|    DockerDetails-azureBatchVMDetails-nodeAgentSKUID          |    Azure Batch düğüm Aracısı SKU KIMLIĞI. Şu anda yalnızca "Batch. Node. Ubuntu 18,04" Batch düğüm Aracısı destekleniyor.    |
| DockerDetails-partnerCredentials | Docker 'da iş ortağı API 'sini çağırma için kimlik bilgileri. Ortağın, bu bilgileri, desteklenen kimlik doğrulama mekanizmasına bağlı olarak müşterilerine vermesi gerekir. Kullanıcı adı/parola veya API anahtarları. |
| partnerType | "Hava durumu" (Farmtörlerde diğer iş ortağı türleri "algılayıcı" ve "Imagery")  |
|  name   |   Farmtempts sisteminde ortağın istenen adı   |
|  açıklama |  Description   |

## <a name="next-steps"></a>Sonraki adımlar

Artık Azure Farmtts örneğinden gelen algılayıcı verilerini sorguladınız. Şimdi, gruplar için [haritalar oluşturmayı](generate-maps-in-azure-farmbeats.md#generate-maps) öğrenin.
