---
title: 'Öğretici: Azure Notebook - Personalizer'
titleSuffix: Azure Cognitive Services
description: Bu öğretici, Azure Not Defteri'nde _system bir Kisizer döngüyü taklit eder ve bu da müşterinin hangi kahve türünü sipariş etmesi gerektiğini önerir. Kullanıcılar ve tercihleri bir kullanıcı veri kümesinde depolanır. Kahve hakkında bilgi de mevcuttur ve bir kahve veri setinde saklanır.
services: cognitive-services
author: diberry
manager: nitinme
ms.service: cognitive-services
ms.subservice: personalizer
ms.topic: tutorial
ms.date: 02/03/2020
ms.author: diberry
ms.openlocfilehash: 03e8b658f7edf4640d738e5ea3af84953185d0f5
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76986844"
---
# <a name="tutorial-use-personalizer-in-azure-notebook"></a>Öğretici: Azure Not Defteri'nde Personalizer'ı kullanın

Bu öğretici, Bir Azure Not Defteri'nde Bir Kisizer döngüsünü çalıştırarak Bir Kisizer döngüsünün sondan sona erdirilmesini gösterir.

Döngü, müşterinin hangi kahve türünü sipariş etmesi gerektiğini önerir. Kullanıcılar ve tercihleri bir kullanıcı veri kümesinde depolanır. Kahve hakkındaki bilgiler bir kahve veri setinde saklanır.

## <a name="users-and-coffee"></a>Kullanıcılar ve kahve

Bir web sitesiyle kullanıcı etkileşimini simüle eden not defteri, veri setinden rastgele bir kullanıcı, günün saati ve hava durumu türünü seçer. Kullanıcı bilgilerinin bir özeti:

|Müşteriler - bağlam özellikleri|Günün Saatleri|Hava durumu türleri|
|--|--|--|
|Alice<br>Bob<br>Cathy<br>Demirci|Sabah<br>Öğle -den sonra<br>Akşam|Güneşli<br>Yağmurlu<br>Karlı|

Personalizer öğrenmek yardımcı olmak için, zaman içinde, _sistem_ de her kişi için kahve seçimi hakkında ayrıntıları bilir.

|Kahve - aksiyon özellikleri|Sıcaklık türleri|Menşe yerleri|Kızartma çeşitleri|Organik|
|--|--|--|--|--|
|Cappacino|Sık Erişimli|Kenya|Koyu|Organik|
|Soğuk bira|Soğuk|Brezilya|Açık|Organik|
|Buzlu mocha|Soğuk|Etiyopya|Açık|Organik değil|
|Latte|Sık Erişimli|Brezilya|Koyu|Organik değil|

Personalizer döngü **amacı** kullanıcılar ve kahve arasında mümkün olduğunca çok zaman en iyi eşleşmeyi bulmaktır.

Bu öğretici için kod [Personalizer Örnekleri GitHub deposunda](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/azurenotebook)mevcuttur.

## <a name="how-the-simulation-works"></a>Simülasyon nasıl çalışır?

Çalışan sistemin başında, Personalizer gelen öneriler sadece% 20 ila% 30 arasında başarılı. Bu başarı, Personalizer'ın Ödül API'sine 1 puan la geri gönderilen ödülle gösterilir. Bazı Rank ve Reward aramalarından sonra sistem iyileşir.

İlk isteklerden sonra çevrimdışı bir değerlendirme çalıştırın. Bu, Personalizer'ın verileri gözden geçirmesine ve daha iyi bir öğrenme politikası önermesine olanak tanır. Yeni öğrenme ilkesini uygulayın ve önceki istek sayısının %20'si ile not defterini yeniden çalıştırın. Döngü yeni öğrenme politikası ile daha iyi performans gösterecektir.

## <a name="rank-and-reward-calls"></a>Rütbe ve ödül çağrıları

Azure Not Defteri, Personalizer hizmetine yapılan birkaç bin çağrının her biri için **Rank** isteğini REST API'sine gönderir:

* Sıralama/İstek etkinliği için benzersiz bir kimlik
* Bağlam özellikleri - Kullanıcının rastgele seçimi, hava durumu ve günün saati - bir web sitesinde veya mobil cihazda kullanıcıyı taklit etme
* Özellikleri ile Eylemler - _Tüm_ kahve verileri - hangi Personalizer bir öneride bulunuyor

Sistem isteği alır, daha sonra günün ve hava aynı saatinde kullanıcının bilinen seçim ile bu tahmin karşılaştırır. Bilinen seçim öngörülen seçimle aynıysa, **1'in Ödülü** Personalizer'a geri gönderilir. Aksi takdirde geri gönderilen ödül 0'dır.

> [!Note]
> Bu bir simülasyon, bu yüzden ödül algoritması basittir. Gerçek bir senaryoda, algoritma ödül puanını belirlemek için, muhtemelen müşterinin deneyiminin çeşitli yönleri için ağırlıkları olan iş mantığını kullanmalıdır.


## <a name="prerequisites"></a>Ön koşullar

* Azure [Not Defteri](https://notebooks.azure.com/) hesabı.
* Azure [Personalizer kaynağı.](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer)
    * Personalizer kaynağını zaten kullandıysanız, kaynak için Azure portalındaki [verileri temizlediğinizden](how-to-settings.md#clear-data-for-your-learning-loop) emin olun.
* [Bu örnekle](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/azurenotebook) ilgili tüm dosyaları bir Azure Notebook projesine yükleyin.

Dosya açıklamaları:

* [Personalizer.ipynb](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/samples/azurenotebook/Personalizer.ipynb) bu öğretici için Jupyter dizüstü bilgisayar.
* [Kullanıcı veri kümesi](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/samples/azurenotebook/users.json) bir JSON nesnesinde depolanır.
* [Kahve veri kümesi](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/samples/azurenotebook/coffee.json) bir JSON nesnesinde depolanır.
* [Örnek İstek JSON,](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/blob/master/samples/azurenotebook/example-rankrequest.json) Rank API'deki bir POST isteği için beklenen biçimdir.

## <a name="configure-personalizer-resource"></a>Personalizer kaynağını yapılandırma

Azure [portalında, Personalizer kaynağınızı](https://ms.portal.azure.com/#create/Microsoft.CognitiveServicesPersonalizer) 15 saniyeye ayarlanmış **güncelleştirme modeli frekansı** ve 15 **saniyelik ödül bekleme süresiyle** yapılandırın. Bu değerler **[Yapılandırma](how-to-settings.md#configure-service-settings-in-the-azure-portal)** sayfasında bulunur.

|Ayar|Değer|
|--|--|
|model sıklığını güncelleştir|15 saniye|
|ödül bekleme süresi|15 saniye|

Bu değerler, bu öğreticideki değişiklikleri göstermek için çok kısa bir süreye sahiptir. Bu değerler, Personalizer döngünüzle amacınıza ulaşmalarını doğrulamadan bir üretim senaryosunda kullanılmamalıdır.

## <a name="set-up-the-azure-notebook"></a>Azure Not Defteri'ni ayarlama

1. Çekirdeği `Python 3.6`değiştirin.
1. `Personalizer.ipynb` dosyasını açın.

## <a name="run-notebook-cells"></a>Not Defteri hücrelerini çalıştırma

Çalıştırılabilir her hücreyi çalıştırın ve geri dönmesini bekleyin. Hücrenin yanındaki parantezler `*`bir numara yerine bir sayı görüntülediğinde yapıldığını bilirsiniz. Aşağıdaki bölümlerde her hücrenin programlı olarak ne yaptığı ve çıktı için ne beklendiği açıklayınız.

### <a name="include-the-python-modules"></a>Python modüllerini ekleme

Gerekli python modüllerini ekleyin. Hücrenin çıktısı yok.

```python
import json
import matplotlib.pyplot as plt
import random
import requests
import time
import uuid
```

### <a name="set-personalizer-resource-key-and-name"></a>Personalizer kaynak anahtarını ve adını ayarlama

Azure portalından, Personalizer kaynağınızın **Quickstart** sayfasında anahtarınızı ve bitiş noktanızı bulun. Personalizer `<your-resource-name>` kaynağınızın değerini değiştirin. Personalizer `<your-resource-key>` anahtarınızın değerini değiştirin.

```python
# Replace 'personalization_base_url' and 'resource_key' with your valid endpoint values.
personalization_base_url = "https://<your-resource-name>.cognitiveservices.azure.com/"
resource_key = "<your-resource-key>"
```

### <a name="print-current-date-and-time"></a>Geçerli tarih ve saati yazdırma
Yinelemeli işlevin, yinelemelerin başlangıç ve bitiş saatlerini not etmek için bu işlevi kullanın.

Bu hücrelerin çıktısı yok. İşlev çağrıldığında geçerli tarih ve saati çıktıyapar.

```python
# Print out current datetime
def currentDateTime():
    currentDT = datetime.datetime.now()
    print (str(currentDT))
```

### <a name="get-the-last-model-update-time"></a>Son model güncelleştirme süresini alma

İşlev, `get_last_updated`çağrıldığında, işlev modelin güncelleştirilme tarihi ve saatini yazdırır.

Bu hücrelerin çıktısı yok. İşlev çağrıldığında son model eğitim tarihini çıktı yapar.

[İşlev, model özelliklerini almak](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/GetModelProperties)için GET REST API kullanır.

```python
# ititialize variable for model's last modified date
modelLastModified = ""
```

```python
def get_last_updated(currentModifiedDate):

    print('-----checking model')

    # get model properties
    response = requests.get(personalization_model_properties_url, headers = headers, params = None)

    print(response)
    print(response.json())

    # get lastModifiedTime
    lastModifiedTime = json.dumps(response.json()["lastModifiedTime"])

    if (currentModifiedDate != lastModifiedTime):
        currentModifiedDate = lastModifiedTime
        print(f'-----model updated: {lastModifiedTime}')
```

### <a name="get-policy-and-service-configuration"></a>İlke ve hizmet yapılandırması alın

Bu iki REST aramasıyla hizmetin durumunu doğrulayın.

Bu hücrelerin çıktısı yok. İşlev çağrıldığında hizmet değerlerini çıktırıyor.

```python
def get_service_settings():

    print('-----checking service settings')

    # get learning policy
    response = requests.get(personalization_model_policy_url, headers = headers, params = None)

    print(response)
    print(response.json())

    # get service settings
    response = requests.get(personalization_service_configuration_url, headers = headers, params = None)

    print(response)
    print(response.json())
```

### <a name="construct-urls-and-read-json-data-files"></a>URL'ler oluşturma ve JSON veri dosyalarını okuma

Bu hücre

* REST aramalarında kullanılan URL'leri oluşturur
* Personalizer kaynak anahtarınızı kullanarak güvenlik üstbilgisini ayarlar
* Rank olay kimliği için rasgele tohumu ayarlar
* JSON veri dosyalarında okur
* aramalar `get_last_updated` yöntemi - öğrenme politikası örnek çıktı kaldırıldı
* aramalar `get_service_settings` yöntemi

Hücre, çağrıdan `get_last_updated` ve `get_service_settings` işlevlerden çıktıya sahiptir.

```python
# build URLs
personalization_rank_url = personalization_base_url + "personalizer/v1.0/rank"
personalization_reward_url = personalization_base_url + "personalizer/v1.0/events/" #add "{eventId}/reward"
personalization_model_properties_url = personalization_base_url + "personalizer/v1.0/model/properties"
personalization_model_policy_url = personalization_base_url + "personalizer/v1.0/configurations/policy"
personalization_service_configuration_url = personalization_base_url + "personalizer/v1.0/configurations/service"

headers = {'Ocp-Apim-Subscription-Key' : resource_key, 'Content-Type': 'application/json'}

# context
users = "users.json"

# action features
coffee = "coffee.json"

# empty JSON for Rank request
requestpath = "example-rankrequest.json"

# initialize random
random.seed(time.time())

userpref = None
rankactionsjsonobj = None
actionfeaturesobj = None

with open(users) as handle:
    userpref = json.loads(handle.read())

with open(coffee) as handle:
    actionfeaturesobj = json.loads(handle.read())

with open(requestpath) as handle:
    rankactionsjsonobj = json.loads(handle.read())

get_last_updated(modelLastModified)
get_service_settings()

print(f'User count {len(userpref)}')
print(f'Coffee count {len(actionfeaturesobj)}')
```

Çıktının `rewardWaitTime` ve her `modelExportFrequency` ikisinin de 15 saniyeye ayarlı olduğunu doğrulayın.

```console
-----checking model
<Response [200]>
{'creationTime': '0001-01-01T00:00:00+00:00', 'lastModifiedTime': '0001-01-01T00:00:00+00:00'}
-----model updated: "0001-01-01T00:00:00+00:00"
-----checking service settings
<Response [200]>
{...learning policy...}
<Response [200]>
{'rewardWaitTime': '00:00:15', 'defaultReward': 0.0, 'rewardAggregation': 'earliest', 'explorationPercentage': 0.2, 'modelExportFrequency': '00:00:15', 'logRetentionDays': -1}
User count 4
Coffee count 4
```

### <a name="troubleshooting-the-first-rest-call"></a>İlk REST aramasını sorun giderme

Bu önceki hücre Personalizer'a seslenir ilk hücredir. Çıktıdaki REST durum kodunun `<Response [200]>`. 404 gibi bir hata alırsanız, ancak kaynak anahtarınızın ve adınızın doğru olduğundan eminseniz, not defterini yeniden yükleyin.

Kahve ve kullanıcı sayısının 4 olduğundan emin olun. Bir hata alırsanız, 3 JSON dosyasının tümini yüklediğinizi kontrol edin.

### <a name="set-up-metric-chart-in-azure-portal"></a>Azure portalında metrik grafik ayarlama

Bu öğreticinin ilerleyen saatlerinde, 10.000 istekten oluşan uzun süren işlem, tarayıcıdan güncelbir metin kutusuyla görünür. Uzun süren işlem sona erdiğinde, bir grafikte veya toplam toplam olarak görmek daha kolay olabilir. Bu bilgileri görüntülemek için kaynakla sağlanan ölçümleri kullanın. Hizmet eki isteği tamamladıktan sonra grafiği oluşturabilir, ardından uzun süren işlem devam ederken grafiği düzenli aralıklarla yenileyebilirsiniz.

1. Azure portalında Personalizer kaynağınızı seçin.
1. Kaynak gezintisinde, İzleme'nin altındaki **Ölçümler'i** seçin.
1. Grafikte metrik **ekle'yi**seçin.
1. Kaynak ve metrik ad alanı zaten ayarlandı. Yalnızca **başarılı aramaların** metrik ve **toplam**toplama seçmeniz gerekir.
1. Zaman filtresini son 4 saate değiştirin.

    ![Azure portalında metrik grafik ayarlayın ve son 4 saat içinde başarılı aramalar için metrik ekleme.](./media/tutorial-azure-notebook/metric-chart-setting.png)

    Grafikte üç başarılı arama görmeniz gerekir.

### <a name="generate-a-unique-event-id"></a>Benzersiz bir etkinlik kimliği oluşturma

Bu işlev, her sıralı arama için benzersiz bir kimlik oluşturur. Kimlik, rütbe ve ödül arama bilgilerini tanımlamak için kullanılır. Bu değer, web görünümü kimliği veya işlem kimliği gibi bir iş sürecinden gelebilir.

Hücrenin çıktısı yok. İşlev çağrıldığında benzersiz kimliği çıktı yapar.

```python
def add_event_id(rankjsonobj):
    eventid = uuid.uuid4().hex
    rankjsonobj["eventId"] = eventid
    return eventid
```

### <a name="get-random-user-weather-and-time-of-day"></a>Rastgele kullanıcı, hava durumu ve günün saatini alın

Bu işlev benzersiz bir kullanıcı, hava durumu ve günün saatini seçer, ardından bu öğeleri Sıralama isteğine göndermek üzere JSON nesnesine ekler.

Hücrenin çıktısı yok. İşlev çağrıldığında rasgele kullanıcının adını, rasgele hava durumunu ve günün rasgele saatini döndürür.

4 kullanıcı nın listesi ve tercihleri - sadece bazı tercihler kısalık için gösterilir:

```json
{
  "Alice": {
    "Sunny": {
      "Morning": "Cold brew",
      "Afternoon": "Iced mocha",
      "Evening": "Cold brew"
    }...
  },
  "Bob": {
    "Sunny": {
      "Morning": "Cappucino",
      "Afternoon": "Iced mocha",
      "Evening": "Cold brew"
    }...
  },
  "Cathy": {
    "Sunny": {
      "Morning": "Latte",
      "Afternoon": "Cold brew",
      "Evening": "Cappucino"
    }...
  },
  "Dave": {
    "Sunny": {
      "Morning": "Iced mocha",
      "Afternoon": "Iced mocha",
      "Evening": "Iced mocha"
    }...
  }
}
```

```python
def add_random_user_and_contextfeatures(namesoption, weatheropt, timeofdayopt, rankjsonobj):
    name = namesoption[random.randint(0,3)]
    weather = weatheropt[random.randint(0,2)]
    timeofday = timeofdayopt[random.randint(0,2)]
    rankjsonobj['contextFeatures'] = [{'timeofday': timeofday, 'weather': weather, 'name': name}]
    return [name, weather, timeofday]
```


### <a name="add-all-coffee-data"></a>Tüm kahve verilerini ekleyin

Bu işlev, Sıralama isteğine göndermek üzere JSON nesnesine kahvenin tüm listesini ekler.

Hücrenin çıktısı yok. İşlev `rankjsonobj` çağrıldığında değiştirir.


Tek bir kahvenin özelliklerine örnek olarak:

```json
{
    "id": "Cappucino",
    "features": [
    {
        "type": "hot",
        "origin": "kenya",
        "organic": "yes",
        "roast": "dark"

    }
}
```

```python
def add_action_features(rankjsonobj):
    rankjsonobj["actions"] = actionfeaturesobj
```

### <a name="compare-prediction-with-known-user-preference"></a>Tahmini bilinen kullanıcı tercihiyle karşılaştırın

Bu işlev, her yineleme için Rank API çağrıldıktan sonra çağrılır.

Bu işlev, kullanıcının hava durumuna ve günün saatine göre kahve tercihini, bu filtreler için kullanıcı için Personalizer'ın önerisiyle karşılaştırır. Öneri eşleşirse, 1 puan döndürülür, aksi takdirde skor 0'dır. Hücrenin çıktısı yok. İşlev çağrıldığında skoru çıkartır.

```python
def get_reward_from_simulated_data(name, weather, timeofday, prediction):
    if(userpref[name][weather][timeofday] == str(prediction)):
        return 1
    return 0
```

### <a name="loop-through-calls-to-rank-and-reward"></a>Rütbe ve Ödül aramalarında döngü

Bir sonraki hücre, Not Defteri'nin _ana_ işidir, rastgele bir kullanıcı elde etmek, kahve listesini almak, her ikisini de Rank API'ye göndermektir. Tahmini kullanıcının bilinen tercihleriyle karşılaştırın, ardından ödülü Personalizer hizmetine geri gönderir.

Döngü zaman `num_requests` için çalışır. Personalizer bir model oluşturmak için Rank ve Reward birkaç bin çağrı ihtiyacı var.

Rank API'ye gönderilen JSON örneği aşağıdaki gibidir. Kahve listesi tam değil, kısalık için. Kahve için tüm JSON'ı `coffee.json`görebilirsiniz.

JSON Rank API gönderdi:

```json
{
   'contextFeatures':[
      {
         'timeofday':'Evening',
         'weather':'Snowy',
         'name':'Alice'
      }
   ],
   'actions':[
      {
         'id':'Cappucino',
         'features':[
            {
               'type':'hot',
               'origin':'kenya',
               'organic':'yes',
               'roast':'dark'
            }
         ]
      }
        ...rest of coffee list
   ],
   'excludedActions':[

   ],
   'eventId':'b5c4ef3e8c434f358382b04be8963f62',
   'deferActivation':False
}
```

Rank API'den JSON yanıtı:

```json
{
    'ranking': [
        {'id': 'Latte', 'probability': 0.85 },
        {'id': 'Iced mocha', 'probability': 0.05 },
        {'id': 'Cappucino', 'probability': 0.05 },
        {'id': 'Cold brew', 'probability': 0.05 }
    ],
    'eventId': '5001bcfe3bb542a1a238e6d18d57f2d2',
    'rewardActionId': 'Latte'
}
```

Son olarak, her döngü kullanıcının rastgele seçimini, hava durumunu, günün saatini ve belirlenen ödülü gösterir. 1'in ödülü, Personalizer kaynağının verilen kullanıcı, hava durumu ve günün saati için doğru kahve türünü seçtiğini gösterir.

```console
1 Alice Rainy Morning Latte 1
```

İşlev kullanır:

* Sıralaması: bir POST REST API [rütbe almak](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Rank)için .
* Ödül: bir POST REST API [ödül rapor](https://westus2.dev.cognitive.microsoft.com/docs/services/personalizer-api/operations/Reward)etmek .

```python
def iterations(n, modelCheck, jsonFormat):

    i = 1

    # default reward value - assumes failed prediction
    reward = 0

    # Print out dateTime
    currentDateTime()

    # collect results to aggregate in graph
    total = 0
    rewards = []
    count = []

    # default list of user, weather, time of day
    namesopt = ['Alice', 'Bob', 'Cathy', 'Dave']
    weatheropt = ['Sunny', 'Rainy', 'Snowy']
    timeofdayopt = ['Morning', 'Afternoon', 'Evening']


    while(i <= n):

        # create unique id to associate with an event
        eventid = add_event_id(jsonFormat)

        # generate a random sample
        [name, weather, timeofday] = add_random_user_and_contextfeatures(namesopt, weatheropt, timeofdayopt, jsonFormat)

        # add action features to rank
        add_action_features(jsonFormat)

        # show JSON to send to Rank
        print('To: ', jsonFormat)

        # choose an action - get prediction from Personalizer
        response = requests.post(personalization_rank_url, headers = headers, params = None, json = jsonFormat)

        # show Rank prediction
        print ('From: ',response.json())

        # compare personalization service recommendation with the simulated data to generate a reward value
        prediction = json.dumps(response.json()["rewardActionId"]).replace('"','')
        reward = get_reward_from_simulated_data(name, weather, timeofday, prediction)

        # show result for iteration
        print(f'   {i} {currentDateTime()} {name} {weather} {timeofday} {prediction} {reward}')

        # send the reward to the service
        response = requests.post(personalization_reward_url + eventid + "/reward", headers = headers, params= None, json = { "value" : reward })

        # for every N rank requests, compute total correct  total
         total =  total + reward

        # every N iteration, get last updated model date and time
        if(i % modelCheck == 0):

            print("**** 10% of loop found")

            get_last_updated(modelLastModified)

        # aggregate so chart is easier to read
        if(i % 10 == 0):
            rewards.append( total)
            count.append(i)
             total = 0

        i = i + 1

    # Print out dateTime
    currentDateTime()

    return [count, rewards]
```

## <a name="run-for-10000-iterations"></a>10.000 yineleme için çalıştırın
Personalizer döngüyü 10.000 yineleme için çalıştırın. Bu uzun süren bir olaydır. Not defterini çalıştıran tarayıcıyı kapatmayın. Hizmete yapılan toplam çağrıları görmek için Azure portalındaki metrik grafiğini düzenli aralıklarla yenileyin. Yaklaşık 20.000 çağrınız olduğunda, döngünün her yinelemesi için bir rütbe ve ödül çağrısı, yinelemeler yapılır.

```python
# max iterations
num_requests = 200

# check last mod date N% of time - currently 10%
lastModCheck = int(num_requests * .10)

jsonTemplate = rankactionsjsonobj

# main iterations
[count, rewards] = iterations(num_requests, lastModCheck, jsonTemplate)
```



## <a name="chart-results-to-see-improvement"></a>İyileştirmeyi görmek için grafik sonuçları

Bir grafik oluşturun `count` `rewards`ve .

```python
def createChart(x, y):
    plt.plot(x, y)
    plt.xlabel("Batch of rank events")
    plt.ylabel("Correct recommendations per batch")
    plt.show()
```

## <a name="run-chart-for-10000-rank-requests"></a>10.000 sıralama isteği için çalışma grafiği

İşlevi `createChart` çalıştırın.

```python
createChart(count,rewards)
```

## <a name="reading-the-chart"></a>Grafiği okuma

Bu grafik, geçerli varsayılan öğrenme ilkesi için modelin başarısını gösterir.

![Bu grafik, test süresince geçerli öğrenme ilkesinin başarısını gösterir.](./media/tutorial-azure-notebook/azure-notebook-chart-results.png)


İdeal hedef, testin sonunda, döngü yüzde 100 eksi keşif yakın bir başarı oranı ortalama olduğunu. Aramanın varsayılan değeri %20'dir.

`100-20=80`

Bu keşif değeri, Azure portalında, Personalizer kaynağı için, **Yapılandırma** sayfasında bulunur.

Rank API'deki verilerinize dayanarak daha iyi bir öğrenme ilkesi bulmak için, Personalizer döngünüz için portalda [çevrimdışı bir değerlendirme](how-to-offline-evaluation.md) çalıştırın.

## <a name="run-an-offline-evaluation"></a>Çevrimdışı değerlendirme çalıştırma

1. Azure portalında, Personalizer kaynağının **Değerlendirmeler** sayfasını açın.
1. **Değerlendirme Oluştur'u**seçin.
1. Döngü değerlendirmesi için gerekli değerlendirme adının ve tarih aralığının gerekli verilerini girin. Tarih aralığı yalnızca değerlendirmeniz için odaklandığınız günleri içermelidir.
    ![Azure portalında, Personalizer kaynağının Değerlendirmeler sayfasını açın. Değerlendirme Oluştur'u seçin. Değerlendirme adını ve tarih aralığını girin.](./media/tutorial-azure-notebook/create-offline-evaluation.png)

    Bu çevrimdışı değerlendirmeyi çalıştırmanın amacı, bu döngüde kullanılan özellikler ve eylemler için daha iyi bir öğrenme ilkesi olup olmadığını belirlemektir. Daha iyi öğrenme ilkesini bulmak **için, Optimizasyon Bulma'nın** açık olduğundan emin olun.

1. Değerlendirmeye başlamak için **Tamam'ı** seçin.
1. Bu **Değerlendirmeler** sayfası yeni değerlendirmeyi ve geçerli durumunu listeler. Ne kadar veriniz var bağlı olarak, bu değerlendirme biraz zaman alabilir. Sonuçları görmek için birkaç dakika sonra bu sayfaya geri dönebilirsiniz.
1. Değerlendirme tamamlandığında, değerlendirmeyi seçin ve **ardından farklı öğrenme politikalarının karşılaştırılması'nı**seçin. Bu, kullanılabilir öğrenme ilkelerini ve verilerle nasıl olacaklarını gösterir.
1. Tablodaki en çok öğrenme ilkesini seçin ve **Uygula'yı**seçin. Bu, modelinize ve yeniden eğitimlerinize _en iyi_ öğrenme ilkesini uygular.

## <a name="change-update-model-frequency-to-5-minutes"></a>Güncelleştirme modeli sıklığını 5 dakikaya değiştirme

1. Azure portalında, hala Personalizer kaynağında, **Yapılandırma** sayfasını seçin.
1. Model **güncelleştirme sıklığını** ve **ödül bekleme süresini** 5 dakikaya değiştirin ve **Kaydet'i**seçin.

Ödül bekleme [süresi](concept-rewards.md#reward-wait-time) ve [model güncelleştirme sıklığı](how-to-settings.md#model-update-frequency)hakkında daha fazla bilgi edinin.

```python
#Verify new learning policy and times
get_service_settings()
```

Çıktının `rewardWaitTime` ve her `modelExportFrequency` ikisinin de 5 dakika olarak ayarlı olduğunu doğrulayın.
```console
-----checking model
<Response [200]>
{'creationTime': '0001-01-01T00:00:00+00:00', 'lastModifiedTime': '0001-01-01T00:00:00+00:00'}
-----model updated: "0001-01-01T00:00:00+00:00"
-----checking service settings
<Response [200]>
{...learning policy...}
<Response [200]>
{'rewardWaitTime': '00:05:00', 'defaultReward': 0.0, 'rewardAggregation': 'earliest', 'explorationPercentage': 0.2, 'modelExportFrequency': '00:05:00', 'logRetentionDays': -1}
User count 4
Coffee count 4
```

## <a name="validate-new-learning-policy"></a>Yeni öğrenme ilkesini doğrulama

Azure not defterine dönün ve aynı döngüyü çalıştırarak ancak yalnızca 2.000 yineleme için devam edin. Hizmete yapılan toplam çağrıları görmek için Azure portalındaki metrik grafiğini düzenli aralıklarla yenileyin. Yaklaşık 4.000 çağrınız olduğunda, döngünün her yinelemesi için bir rütbe ve ödül çağrısı, yinelemeler yapılır.

```python
# max iterations
num_requests = 2000

# check last mod date N% of time - currently 10%
lastModCheck2 = int(num_requests * .10)

jsonTemplate2 = rankactionsjsonobj

# main iterations
[count2, rewards2] = iterations(num_requests, lastModCheck2, jsonTemplate)
```

## <a name="run-chart-for-2000-rank-requests"></a>2.000 sıralama isteği için çalışma grafiği

İşlevi `createChart` çalıştırın.

```python
createChart(count2,rewards2)
```

## <a name="review-the-second-chart"></a>İkinci grafiği gözden geçirin

İkinci grafik, kullanıcı tercihleriyle hizalayan Sıralama tahminlerinde görünür bir artış göstermelidir.

![İkinci grafik, kullanıcı tercihleriyle hizalayan Sıralama tahminlerinde görünür bir artış göstermelidir.](./media/tutorial-azure-notebook/azure-notebook-chart-results-happy-graph.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Öğretici seriye devam etmeyi düşünmüyorsanız, aşağıdaki kaynakları temizleyin:

* Azure Notebook projenizi silin.
* Personalizer kaynağınızı silin.

## <a name="next-steps"></a>Sonraki adımlar

Bu örnekte kullanılan [Jupyter dizüstü bilgisayar ve veri dosyaları](https://github.com/Azure-Samples/cognitive-services-personalizer-samples/tree/master/samples/azurenotebook) Personalizer için GitHub repo mevcuttur.

