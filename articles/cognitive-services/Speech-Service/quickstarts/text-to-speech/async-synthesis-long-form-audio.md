---
title: 'Hızlı başlangıç: uzun biçimli ses (Önizleme) için zaman uyumsuz birleştirme-konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Zaman uyumsuz olarak metni konuşmaya dönüştürmek için uzun ses API 'sini kullanın ve hizmet tarafından sunulan bir URI 'den ses çıkışını alın. Bu REST API, 10.000 karakterden büyük veya 50 paragrafından oluşan metin dosyalarını sentezleştirilmiş konuşmaya dönüştürmeli içerik sağlayıcıları için idealdir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.custom: devx-track-python
ms.openlocfilehash: 330bd3593ea10e4a4a03e430025b8acf1f7ab21a
ms.sourcegitcommit: dea88d5e28bd4bbd55f5303d7d58785fad5a341d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/06/2020
ms.locfileid: "87875396"
---
# <a name="quickstart-asynchronous-synthesis-for-long-form-audio-in-python-preview"></a>Hızlı başlangıç: Python 'da uzun biçimli ses için zaman uyumsuz birleştirme (Önizleme)

Bu hızlı başlangıçta, metin okumayı zaman uyumsuz olarak dönüştürmek ve hizmet tarafından sunulan bir URI 'den ses çıktısını almak için uzun ses API 'sini kullanacaksınız. Bu REST API, metinden 5.000 karakterden (veya uzunluğu 10 dakikadan uzun) daha büyük bir sesi birleştirmesini gerektiren içerik sağlayıcıları için idealdir. Daha fazla bilgi için bkz. [uzun ses API 'si](../../long-audio-api.md).

Uzun biçimli ses için zaman uyumsuz birleştirme, her biri belirli bir dili ve diyalekt destekleyen [genel sinir seslerle](https://docs.microsoft.com/azure/cognitive-services/speech-service/language-support#neural-voices) ve [özel sinir seslerle](../../how-to-custom-voice.md#custom-neural-voices)kullanılabilir. 

## <a name="prerequisites"></a>Önkoşullar

Bu hızlı başlangıç şunları gerektirir:

* Python 2.7. x veya 3. x.
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download)veya sık kullandığınız metin düzenleyiciniz.
* Bir Azure aboneliği ve bir konuşma hizmeti abonelik anahtarı. Anahtarı almak için bir [Azure hesabı oluşturun](../../get-started.md#new-resource) ve [bir konuşma kaynağı oluşturun](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-resource) . Konuşma kaynağını oluştururken, fiyatlandırma katmanınızın **S0**olarak ayarlandığından ve konumun [desteklenen bir bölgeye](../../regions.md#standard-and-neural-voices)ayarlandığından emin olun.

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Sonra bu kod parçacığını adlı bir dosyaya kopyalayın `voice_synthesis_client.py` .

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

> [!NOTE]
> Bu modülleri kullanmadıysanız, programınızı çalıştırmadan önce bunları yüklemeniz gerekir. Bu paketleri yüklemek için şunu çalıştırın: `pip install requests urllib3`.

Bu modüller, bağımsız değişkenleri ayrıştırmak, HTTP isteğini oluşturmak ve metinden konuşmaya uzun ses REST API çağırmak için kullanılır.

## <a name="get-a-list-of-supported-voices"></a>Desteklenen seslerin listesini al

Bu kod, kullanabileceğiniz belirli bir bölgeye/uç noktaya ait seslerin tam listesini almanızı sağlar. Lütfen [desteklenen bölgeyi/uç noktayı](../../long-audio-api.md)denetleyin. Kodu şu şekilde ekleyin `voice_synthesis_client.py` :

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

### <a name="test-your-code"></a>Kodunuza test etme

Şimdiye kadar yaptığınız işlemi test edelim. Aşağıdaki istekte birkaç şeyi güncelleştirmeniz gerekir:

* `<your_key>`Konuşma hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<region>`Konuşma kaynağınızın oluşturulduğu bölgeyle değiştirin (örneğin: `eastus` veya `westus` ). Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.

Şu komutu çalıştırın:

```console
python voice_synthesis_client.py --voices -key <your_key> -region <Region>
```

Aşağıdakine benzer bir çıktı görürsünüz:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

**Publicvoice** parametresi **true**ise, Voice genel sinir sestir. Aksi takdirde, özel sinir sestir. 

## <a name="prepare-input-files"></a>Giriş dosyalarını hazırlama

Giriş metin dosyası hazırlayın. Düz metin veya SSML metni olabilir. Giriş dosyası gereksinimleri için bkz. [birleştirme için içerik hazırlama](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#prepare-content-for-synthesis).

## <a name="convert-text-to-speech"></a>Metni konuşmaya Dönüştür

Giriş metin dosyasını hazırladıktan sonra, konuşma birleştirme için şu kodu ekleyin `voice_synthesis_client.py` :

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

### <a name="test-your-code"></a>Kodunuza test etme

Kaynak olarak giriş dosyanızı kullanarak metni sentezleştirme isteği oluşturalım. Aşağıdaki istekte birkaç şeyi güncelleştirmeniz gerekir:

* `<your_key>`Konuşma hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<region>`Konuşma kaynağınızın oluşturulduğu bölgeyle değiştirin (örneğin: `eastus` veya `westus` ). Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<input>`Metin okuma için hazırladığınız metin dosyasının yoluyla değiştirin.
* `<locale>`İstenen çıkış yerel ayarıyla değiştirin. Daha fazla bilgi için bkz. [dil desteği](../../language-support.md#neural-voices).
* `<voice_guid>`İstenen çıkış sesiyle değiştirin. [Desteklenen seslerin listesini al](#get-a-list-of-supported-voices)tarafından döndürülen sesden birini kullanın.

Bu komutla metni konuşmaya dönüştürün:

```console
python voice_synthesis_client.py --submit -key <your_key> -region <Region> -file <input> -locale <locale> -voiceId <voice_guid>
```

> [!NOTE]
> 1 ' den fazla giriş dosyası varsa, birden fazla istek göndermeniz gerekir. Farkında olması gereken bazı sınırlamalar vardır. 
> * İstemcinin her bir Azure abonelik hesabı için saniyede en fazla **5** istek göndermesine izin verilir. Sınırlama aştıysa, istemci 429 hata kodu (çok fazla istek) alır. Lütfen saniye başına istek miktarını azaltın
> * Sunucunun her bir Azure abonelik hesabı için en fazla **120** istek çalıştırmasına ve sıraya alma yapmasına izin verilir. Sınırlama aşıldıysa, sunucu 429 hata kodu (çok fazla istek) döndürür. Lütfen bekleyin ve bazı istekler tamamlanana kadar yeni istek gönderilmesini önleyin

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

## <a name="remove-previous-requests"></a>Önceki istekleri kaldır

Sunucu, her bir Azure aboneliği hesabı için en fazla **20.000** istek tutar. İstek miktarınız bu sınırlamayı aşarsa, lütfen yenilerini oluşturmadan önce önceki istekleri kaldırın. Mevcut istekleri kaldırmazsanız bir hata bildirimi alırsınız.

Kodu şu şekilde ekleyin `voice_synthesis_client.py` :

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

### <a name="test-your-code"></a>Kodunuza test etme

Şimdi, daha önce gönderdiğiniz istekleri görmeyi denetlim. Devam etmeden önce, bu istekteki birkaç şeyi güncelleştirmeniz gerekir:

* `<your_key>`Konuşma hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<region>`Konuşma kaynağınızın oluşturulduğu bölgeyle değiştirin (örneğin: `eastus` veya `westus` ). Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.

Şu komutu çalıştırın:

```console
python voice_synthesis_client.py --syntheses -key <your_key> -region <Region>
```

Bu, yaptığınız sensıs isteklerinin bir listesini döndürür. Aşağıdakine benzer bir çıktı görürsünüz:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Şimdi, daha önce gönderilen bir isteği kaldıralım. Aşağıdaki kodda birkaç şeyi güncelleştirmeniz gerekir:

* `<your_key>`Konuşma hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<region>`Konuşma kaynağınızın oluşturulduğu bölgeyle değiştirin (örneğin: `eastus` veya `westus` ). Bu bilgiler, [Azure Portal](https://aka.ms/azureportal)kaynağınızın **genel bakış** sekmesinde bulunabilir.
* `<synthesis_id>`Önceki istekte döndürülen değerle değiştirin.

> [!NOTE]
> Durumu ' Running '/' bekliyor ' olan istekler kaldırılamaz veya silinemez.

Şu komutu çalıştırın:

```console
python voice_synthesis_client.py --delete -key <your_key> -region <Region> -synthesisId <synthesis_id>
```

Aşağıdakine benzer bir çıktı görürsünüz:

```console
delete voice synthesis xxx
delete successful
```

## <a name="get-the-full-client"></a>Tam istemciyi al

Tamamlandı, `voice_synthesis_client.py` [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py)' da indirilebilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uzun ses API 'SI hakkında daha fazla bilgi edinin](../../long-audio-api.md)
