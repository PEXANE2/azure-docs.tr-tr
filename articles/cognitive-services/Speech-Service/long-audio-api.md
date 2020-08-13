---
title: Uzun ses API 'SI (Önizleme)-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Uzun ses API 'sinin zaman uyumsuz senkron metin okuma için nasıl tasarlandığına öğrenin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 08/11/2020
ms.author: trbye
ms.openlocfilehash: be38d3e78108a15c9f7875a15156e0eeba5a6211
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88167768"
---
# <a name="long-audio-api-preview"></a>Uzun ses API 'SI (Önizleme)

Uzun ses API 'SI, zaman uyumsuz birleştirme için tasarlanan uzun biçimli metin (örneğin, ses kitapları, haber makaleleri ve belgeler) için tasarlanmıştır. Bu API gerçek zamanlı olarak birleştirilmiş ses döndürmez, bunun yerine beklentiler, yanıtları yoklayacaksınız ve çıkışta hizmetten kullanılabilir hale getirildiklerinden çıktıyı tüketebilir. Konuşma SDK 'Sı tarafından kullanılan metinden konuşmaya dönüştürme API 'sinin aksine, uzun ses API 'SI 10 dakikadan daha uzun bir şekilde birleştirilmiş ses oluşturabilir ve bu da Yayımcılar ve ses içeriği platformları için idealdir.

Uzun ses API 'sinin ek avantajları:

* Hizmet tarafından döndürülen sentezlenmiş konuşma en iyi sinir sesleri kullanır.
* Sesleri hiçbiri gerçek zamanlı toplu iş modunda birleştirdiğinden ses uç noktası dağıtmanız gerekmez.

> [!NOTE]
> Uzun ses API 'SI artık [genel sinir seslerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices) ve [özel sinir seslerini](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices)desteklemektedir.

## <a name="workflow"></a>İş akışı

Genellikle, uzun ses API 'SI kullanılırken, birleştirme için bir metin dosyası ya da dosya gönderir, durum için yoklama yapılır, ardından durum başarılı olursa ses çıkışını indirebilirsiniz.

Bu diyagram, iş akışına üst düzey bir genel bakış sağlar.

![Uzun ses API iş akışı diyagramı](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>Birleştirme için içerik hazırlama

Metin dosyanızı hazırlarken şunları yaptığınızdan emin olun:

* Düz metin (. txt) ya da SSML metni (. txt)
* , [Bayt sıra Işaretiyle UTF-8 olarak kodlanır (BOM)](https://www.w3.org/International/questions/qa-utf8-bom.en#bom)
* ZIP değil tek bir dosyadır
* SSML metni için düz metin veya 400 [faturalanabilir karakter](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech#pricing-note) için 400 karakterden daha uzun ve 10.000 ' den az paragraf içeriyor
  * Düz metin için, her paragraf **Enter/Return** -View [düz metin girişi örneğine](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt) vurarak ayrılır
  * SSML metninde her SSML parçası bir paragraf olarak değerlendirilir. SSML parçaları farklı paragraflar ile ayrılmalıdır- [SSML metin girişi örneğini](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt) görüntüleyin
> [!NOTE]
> Çince (Mainland), Çince (Hong Kong çın ÖIB), Çince (Tayvan), Japonca ve Korece için bir sözcük iki karakter olarak sayılır. 

## <a name="python-example"></a>Python örneği

Bu bölüm, uzun ses API 'sinin temel kullanımını gösteren Python örnekleri içerir. Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Sonra bu kod parçacığını adlı bir dosyaya kopyalayın `voice_synthesis_client.py` .

```python
import argparse
import json
import ntpath
import urllib3
import requests
import time
from json import dumps, loads, JSONEncoder, JSONDecoder
import pickle

urllib3.disable_warnings(urllib3.exceptions.InsecureRequestWarning)
```

Bu kitaplıklar bağımsız değişkenleri ayrıştırmak, HTTP isteğini oluşturmak ve metinden konuşmaya uzun ses REST API çağırmak için kullanılır.

### <a name="get-a-list-of-supported-voices"></a>Desteklenen seslerin listesini al

Bu kod, kullanabileceğiniz belirli bir bölgeye/uç noktaya ait seslerin tam listesini almanızı sağlar. Kodu şu şekilde ekleyin `voice_synthesis_client.py` :

```python
parser = argparse.ArgumentParser(description='Text-to-speech client tool to submit voice synthesis requests.')
parser.add_argument('--voices', action="store_true", default=False, help='print voice list')
parser.add_argument('-key', action="store", dest="key", required=True, help='the speech subscription key, like fg1f763i01d94768bda32u7a******** ')
parser.add_argument('-region', action="store", dest="region", required=True, help='the region information, could be centralindia, canadacentral or uksouth')
args = parser.parse_args()
baseAddress = 'https://%s.customvoice.api.speech.microsoft.com/api/texttospeech/v3.0-beta1/' % args.region

def getVoices():
    response=requests.get(baseAddress+"voicesynthesis/voices", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    voices = json.loads(response.text)
    return voices

if args.voices:
    voices = getVoices()
    print("There are %d voices available:" % len(voices))
    for voice in voices:
        print ("Name: %s, Description: %s, Id: %s, Locale: %s, Gender: %s, PublicVoice: %s, Created: %s" % (voice['name'], voice['description'], voice['id'], voice['locale'], voice['gender'], voice['isPublicVoice'], voice['created']))
```

Komutunu kullanarak betiği çalıştırın `python voice_synthesis_client.py --voices -key <your_key> -region <region>` ve aşağıdaki değerleri değiştirin:

* `<your_key>`Konuşma hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<region>`Konuşma kaynağınızın oluşturulduğu bölgeyle değiştirin (örneğin: `eastus` veya `westus` ). Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.

Aşağıdakine benzer bir çıktı görürsünüz:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

**Publicvoice** parametresi **true**ise, Voice genel sinir sestir. Aksi takdirde, özel sinir sestir.

### <a name="convert-text-to-speech"></a>Metni konuşmaya Dönüştür

Bir giriş metin dosyasını düz metin veya SSML metninde hazırlayın ve ardından aşağıdaki kodu ekleyin `voice_synthesis_client.py` :

> [!NOTE]
> ' concatenateResult ', isteğe bağlı bir parametredir. Bu parametre ayarlanmamışsa, her paragraf için ses çıkışları oluşturulacaktır. Ayrıca, parametresini ayarlayarak sesos 'yi 1 çıkışa ekleyebilirsiniz. Varsayılan olarak, Ses çıktısı Riff-16khz-16bit-mono-PCM olarak ayarlanır. Desteklenen ses çıkışları hakkında daha fazla bilgi için bkz. [ses çıkış biçimleri](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#audio-output-formats).

```python
parser.add_argument('--submit', action="store_true", default=False, help='submit a synthesis request')
parser.add_argument('--concatenateResult', action="store_true", default=False, help='If concatenate result in a single wave file')
parser.add_argument('-file', action="store", dest="file", help='the input text script file path')
parser.add_argument('-voiceId', action="store", nargs='+', dest="voiceId", help='the id of the voice which used to synthesis')
parser.add_argument('-locale', action="store", dest="locale", help='the locale information like zh-CN/en-US')
parser.add_argument('-format', action="store", dest="format", default='riff-16khz-16bit-mono-pcm', help='the output audio format')

def submitSynthesis():
    modelList = args.voiceId
    data={'name': 'simple test', 'description': 'desc...', 'models': json.dumps(modelList), 'locale': args.locale, 'outputformat': args.format}
    if args.concatenateResult:
        properties={'ConcatenateResult': 'true'}
        data['properties'] = json.dumps(properties)
    if args.file is not None:
        scriptfilename=ntpath.basename(args.file)
        files = {'script': (scriptfilename, open(args.file, 'rb'), 'text/plain')}
    response = requests.post(baseAddress+"voicesynthesis", data, headers={"Ocp-Apim-Subscription-Key":args.key}, files=files, verify=False)
    if response.status_code == 202:
        location = response.headers['Location']
        id = location.split("/")[-1]
        print("Submit synthesis request successful")
        return id
    else:
        print("Submit synthesis request failed")
        print("response.status_code: %d" % response.status_code)
        print("response.text: %s" % response.text)
        return 0

def getSubmittedSynthesis(id):
    response=requests.get(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    synthesis = json.loads(response.text)
    return synthesis

if args.submit:
    id = submitSynthesis()
    if (id == 0):
        exit(1)

    while(1):
        print("\r\nChecking status")
        synthesis=getSubmittedSynthesis(id)
        if synthesis['status'] == "Succeeded":
            r = requests.get(synthesis['resultsUrl'])
            filename=id + ".zip"
            with open(filename, 'wb') as f:  
                f.write(r.content)
                print("Succeeded... Result file downloaded : " + filename)
            break
        elif synthesis['status'] == "Failed":
            print("Failed...")
            break
        elif synthesis['status'] == "Running":
            print("Running...")
        elif synthesis['status'] == "NotStarted":
            print("NotStarted...")
        time.sleep(10)
```

Komutunu kullanarak betiği çalıştırın `python voice_synthesis_client.py --submit -key <your_key> -region <region> -file <input> -locale <locale> -voiceId <voice_guid>` ve aşağıdaki değerleri değiştirin:

* `<your_key>`Konuşma hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<region>`Konuşma kaynağınızın oluşturulduğu bölgeyle değiştirin (örneğin: `eastus` veya `westus` ). Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<input>`Metin okuma için hazırladığınız metin dosyasının yoluyla değiştirin.
* `<locale>`İstenen çıkış yerel ayarıyla değiştirin. Daha fazla bilgi için bkz. [dil desteği](language-support.md#neural-voices).
* `<voice_guid>`İstenen çıkış sesiyle değiştirin. Önceki uç noktaya yapılan çağrılarınızın döndürdüğü sesden birini kullanın `/voicesynthesis/voices` .

Aşağıdakine benzer bir çıktı görürsünüz:

```console
Submit synthesis request successful

Checking status
NotStarted...

Checking status
Running...

Checking status
Running...

Checking status
Succeeded... Result file downloaded : xxxx.zip
```

Sonuç, hizmet tarafından oluşturulan giriş metnini ve ses çıktı dosyalarını içerir. Bu dosyaları bir zip dosyasına indirebilirsiniz.

> [!NOTE]
> 1 ' den fazla giriş dosyası varsa, birden fazla istek göndermeniz gerekir. Farkında olması gereken bazı sınırlamalar vardır. 
> * İstemcinin her bir Azure abonelik hesabı için saniyede en fazla **5** istek göndermesine izin verilir. Sınırlama aştıysa, istemci 429 hata kodu (çok fazla istek) alır. Lütfen saniye başına istek miktarını azaltın
> * Sunucunun her bir Azure abonelik hesabı için en fazla **120** istek çalıştırmasına ve sıraya alma yapmasına izin verilir. Sınırlama aşıldıysa, sunucu 429 hata kodu (çok fazla istek) döndürür. Lütfen bekleyin ve bazı istekler tamamlanana kadar yeni istek gönderilmesini önleyin

### <a name="remove-previous-requests"></a>Önceki istekleri kaldır

Hizmet, her bir Azure aboneliği hesabı için en fazla **20.000** istek tutar. İstek miktarınız bu sınırlamayı aşarsa, lütfen yenilerini oluşturmadan önce önceki istekleri kaldırın. Mevcut istekleri kaldırmazsanız bir hata bildirimi alırsınız.

Aşağıdaki kodu `voice_synthesis_client.py` dosyasına ekleyin:

```python
parser.add_argument('--syntheses', action="store_true", default=False, help='print synthesis list')
parser.add_argument('--delete', action="store_true", default=False, help='delete a synthesis request')
parser.add_argument('-synthesisId', action="store", nargs='+', dest="synthesisId", help='the id of the voice synthesis which need to be deleted')

def getSubmittedSyntheses():
    response=requests.get(baseAddress+"voicesynthesis", headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
    syntheses = json.loads(response.text)
    return syntheses

def deleteSynthesis(ids):
    for id in ids:
        print("delete voice synthesis %s " % id)
        response = requests.delete(baseAddress+"voicesynthesis/"+id, headers={"Ocp-Apim-Subscription-Key":args.key}, verify=False)
        if (response.status_code == 204):
            print("delete successful")
        else:
            print("delete failed, response.status_code: %d, response.text: %s " % (response.status_code, response.text))

if args.syntheses:
    synthese = getSubmittedSyntheses()
    print("There are %d synthesis requests submitted:" % len(synthese))
    for synthesis in synthese:
        print ("ID : %s , Name : %s, Status : %s " % (synthesis['id'], synthesis['name'], synthesis['status']))

if args.delete:
    deleteSynthesis(args.synthesisId)
```

`python voice_synthesis_client.py --syntheses -key <your_key> -region <region>`Yaptığınız sensıs isteklerinin bir listesini almak için ' i çalıştırın. Aşağıdakine benzer bir çıktı görürsünüz:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Bir isteği silmek için, öğesini çalıştırın `python voice_synthesis_client.py --delete -key <your_key> -region <Region> -synthesisId <synthesis_id>` ve `<synthesis_id>` önceki istekten döndürülen BIR istek kimliği değeriyle değiştirin.

> [!NOTE]
> Durumu ' Running '/' bekliyor ' olan istekler kaldırılamaz veya silinemez.

Tamamlandı, `voice_synthesis_client.py` [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py)' da kullanılabilir.

## <a name="http-status-codes"></a>HTTP durum kodu

Aşağıdaki tabloda, REST API HTTP yanıt kodlarının ve iletilerinin ayrıntıları verilmiştir.

| API | HTTP durum kodu | Açıklama | Çözüm |
|-----|------------------|-------------|----------|
| Oluştur | 400 | Ses birleştirme bu bölgede etkin değil. | Konuşma abonelik anahtarını desteklenen bir bölge ile değiştirin. |
|        | 400 | Bu bölge için yalnızca **Standart** konuşma aboneliği geçerlidir. | Konuşma abonelik anahtarını "standart" fiyatlandırma katmanıyla değiştirin. |
|        | 400 | Azure hesabı için 20.000 istek sınırını aşıyor. Lütfen yenilerini göndermeden önce bazı istekleri kaldırın. | Sunucu, her bir Azure hesabı için en fazla 20.000 istek tutar. Yeni bir istek göndermeden önce bazı istekleri silin. |
|        | 400 | Bu model şu ses senlimte kullanılamaz: {ModelId}. | {ModelId} durumunun doğru olduğundan emin olun. |
|        | 400 | İsteğin bölgesi şu modelin bölgesiyle eşleşmiyor: {ModelId}. | {ModelId} bölgesinin, isteğin bölgesiyle eşleştiğinden emin olun. |
|        | 400 | Ses birleştirme yalnızca, bayt sırası işaretleyicisi ile UTF-8 Kodlamadaki metin dosyasını destekler. | Giriş dosyalarının, bayt sırası işaretleyicisi ile UTF-8 kodlaması içinde olduğundan emin olun. |
|        | 400 | Ses senssıs isteğinde yalnızca geçerli SSML girdilerine izin verilir. | Giriş SSML ifadelerinin doğru olduğundan emin olun. |
|        | 400 | Giriş dosyasında {voiceName} ses adı bulunamadı. | SSML ses adı girdisi model KIMLIĞIYLE hizalı değil. |
|        | 400 | Giriş dosyasındaki paragraf sayısı 10.000 ' den az olmalıdır. | Dosyadaki paragraf sayısının 10.000 ' den küçük olduğundan emin olun. |
|        | 400 | Giriş dosyası 400 karakterden uzun olmalıdır. | Giriş dosyanızın 400 karakteri aştığından emin olun. |
|        | 404 | Voice sensııd tanımında belirtilen model bulunamıyor: {ModelId}. | {ModelId} ' ın doğru olduğundan emin olun. |
|        | 429 | Etkin ses sensimi sınırını aşıyor. Lütfen bazı isteklerin bitmesini bekleyin. | Sunucunun her bir Azure hesabı için en fazla 120 istek çalıştırmasına ve sıraya alma yapmasına izin verilir. Lütfen bekleyin ve bazı istekler tamamlanana kadar yeni istek gönderilmesini önleyin. |
| Tümü       | 429 | Çok fazla istek var. | İstemcinin her bir Azure hesabı için saniyede en fazla 5 istek göndermesine izin verilir. Lütfen saniye başına istek miktarını azaltın. |
| Sil    | 400 | Ses birleştirme görevi hala kullanımda. | Yalnızca **Tamamlanan** veya **başarısız**olan istekleri silebilirsiniz. |
| GetById   | 404 | Belirtilen varlık bulunamıyor. | Sen, ID 'nin doğru olduğundan emin olun. |

## <a name="regions-and-endpoints"></a>Bölgeler ve uç noktalar

Uzun ses API 'SI, benzersiz uç noktaları olan birden çok bölgede kullanılabilir.

| Bölge | Uç Noktası |
|--------|----------|
| Doğu Avustralya | `https://australiaeast.customvoice.api.speech.microsoft.com` |
| Orta Kanada | `https://canadacentral.customvoice.api.speech.microsoft.com` |
| Doğu ABD | `https://eastus.customvoice.api.speech.microsoft.com` |
| Hindistan Orta | `https://centralindia.customvoice.api.speech.microsoft.com` |
| Orta Güney ABD | `https://southcentralus.customvoice.api.speech.microsoft.com` |
| Güneydoğu Asya | `https://southeastasia.customvoice.api.speech.microsoft.com` |
| Güney Birleşik Krallık | `https://uksouth.customvoice.api.speech.microsoft.com` |
| West Europe | `https://westeurope.customvoice.api.speech.microsoft.com` |
| Batı ABD 2 | `https://westus2.customvoice.api.speech.microsoft.com` |

## <a name="audio-output-formats"></a>Ses çıkış biçimleri

Esnek ses çıkışı biçimlerini destekliyoruz. ' ConcatenateResult ' parametresini ayarlayarak, paragraf başına ses çıkışları oluşturabilir veya ses çıkışlarını tek bir çıkışa birleştirebilirsiniz. Aşağıdaki ses çıkış biçimleri, uzun ses API 'SI tarafından desteklenir:

> [!NOTE]
> Varsayılan ses biçimi Riff-16khz-16bit-mono-PCM ' dir.

* Riff-8khz-16bit-mono-PCM
* Riff-16khz-16bit-mono-PCM
* Riff-24khz-16bit-mono-PCM
* Riff-48kHz-16bit-mono-PCM
* Ses-16khz-32K bit hızı-mono-MP3
* Ses-16khz-64kbit hızı-mono-MP3
* Ses-16khz-128kbit hızı-mono-MP3
* Ses-24khz-48kbit hızı-mono-MP3
* Ses-24khz-96kbit hızı-mono-MP3
* Ses-24khz-160kbit hızı-mono-MP3

## <a name="sample-code"></a>Örnek kod
Uzun ses API 'sinin örnek kodu GitHub ' da kullanılabilir.

* [Örnek kod: Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [Örnek kod: C #](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Örnek kod: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
