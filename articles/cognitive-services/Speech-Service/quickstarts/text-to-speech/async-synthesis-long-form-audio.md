---
title: 'Quickstart: Uzun biçimli ses için asynchronous sentezi (Önizleme) - Konuşma hizmeti'
titleSuffix: Azure Cognitive Services
description: Metni eş senkronize bir şekilde konuşmaya dönüştürmek ve hizmet tarafından sağlanan bir URI'den ses çıktısını almak için Uzun Ses API'sini kullanın. Bu REST API, 10.000 karakterden veya 50 paragraftan büyük metin dosyalarını sentezlenmiş konuşmaya dönüştürmesi gereken içerik sağlayıcıları için idealdir.
services: cognitive-services
author: erhopf
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 11/04/2019
ms.author: erhopf
ms.openlocfilehash: d3cd330001bcf53e7bd4fb9e6955c76a9ef20511
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78331085"
---
# <a name="quickstart-asynchronous-synthesis-for-long-form-audio-in-python-preview"></a>Quickstart: Python'da uzun biçimli ses için asynchronous sentezi (Önizleme)

Bu hızlı başlatmada, metni eş senkronize bir şekilde konuşmaya dönüştürmek ve hizmet tarafından sağlanan bir URI'den ses çıktısını almak için Uzun Ses API'sini kullanırsınız. Bu REST API, 5.000 karakterden (veya 10 dakikadan fazla uzunlukta) metinden ses sentezlesi gereken içerik sağlayıcılar için idealdir. Daha fazla bilgi için [Uzun Ses API'si'ne](../../long-audio-api.md)bakın.

> [!NOTE]
> Uzun formses için asenkron sentezi sadece [Özel Nöral Sesler](../../how-to-custom-voice.md#custom-neural-voices)ile kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıç şunları gerektirir:

* Python 2.7.x veya 3.x.
* [Visual Studio](https://visualstudio.microsoft.com/downloads/), [Visual Studio Code](https://code.visualstudio.com/download), veya en sevdiğiniz metin editörü.
* Azure aboneliği ve Konuşma hizmeti abonelik anahtarı. [Bir Azure hesabı oluşturun](../../get-started.md#new-resource) ve anahtarı almak için [bir konuşma kaynağı oluşturun.](https://docs.microsoft.com/azure/cognitive-services/speech-service/get-started#new-resource) Konuşma kaynağı oluşturulurken, fiyatlandırma katmanınızın **S0'a,** konumun uzun süre desteklenen bir [bölgeye](../../regions.md#standard-and-neural-voices)ayarlandıklarından emin olun.

## <a name="create-a-project-and-import-required-modules"></a>Bir proje oluşturun ve gerekli modülleri içeri aktarın

Favori IDE ortamınızda veya düzenleyicide yeni bir Python projesi oluşturun. Ardından bu kod parçacıklarını adlı `voice_synthesis_client.py`bir dosyaya kopyalayın.

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

Bu modüller bağımsız değişkenleri ayrışdırmak, HTTP isteğini oluşturmak ve metinden konuşmaya uzun ses REST API'sini çağırmak için kullanılır.

## <a name="get-a-list-of-supported-voices"></a>Desteklenen seslerin listesini alma

Bu kod, metinden konuşmaya dönüştürmek için kullanabileceğiniz kullanılabilir seslerin bir listesini alır. Kodu `voice_synthesis_client.py`ekle:

```python
parser = argparse.ArgumentParser(description='Cris client tool to submit voice synthesis requests.')
parser.add_argument('--voices', action="store_true", default=False, help='print voice list')
parser.add_argument('-key', action="store", dest="key", required=True, help='the cris subscription key, like ff1eb62d06d34767bda0207acb1da7d7 ')
parser.add_argument('-region', action="store", dest="region", required=True, help='the region information, could be centralindia, canadacentral or uksouth')
args = parser.parse_args()
baseAddress = 'https://%s.cris.ai/api/texttospeech/v3.0-beta1/' % args.region

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

Şu ana kadar ne yaptığını test edelim. Aşağıdaki istekte birkaç şeyi güncelleştirmeniz gerekir:

* Konuşma `<your_key>` hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.
* Konuşma `<region>` kaynağınızın oluşturulduğu bölgeyle değiştirin `eastus` (örneğin: veya). `westus` Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.

Şu komutu çalıştırın:

```console
python voice_synthesis_client.py --voices -key <your_key> -region <Region>
```

Şuna benzer bir çıktı görürsünüz:

```console
There are xx voices available:

Name: Microsoft Server Speech Text to Speech Voice (en-US, xxx), Description: xxx , Id: xxx, Locale: en-US, Gender: Male, PublicVoice: xxx, Created: 2019-07-22T09:38:14Z
Name: Microsoft Server Speech Text to Speech Voice (zh-CN, xxx), Description: xxx , Id: xxx, Locale: zh-CN, Gender: Female, PublicVoice: xxx, Created: 2019-08-26T04:55:39Z
```

## <a name="prepare-input-files"></a>Giriş dosyalarını hazırlama

Giriş metin dosyası hazırlayın. Düz metin veya SSML metni olabilir. Giriş dosyası gereksinimleri [için, içeriğisenteze](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#prepare-content-for-synthesis)nasıl hazırlayacağınızda görün.

## <a name="convert-text-to-speech"></a>Metni konuşmaya dönüştürme

Giriş metin dosyası hazırlandıktan sonra, konuşma sentezi için bu kodu `voice_synthesis_client.py`ekleyin:

> [!NOTE]
> 'concatenateResult' isteğe bağlı bir parametredir. Bu parametre ayarlanmazsa, ses çıkışları paragraf başına oluşturulur. Ayrıca parametre ayarlayarak 1 çıkış içine ses concatecateed olabilir. Varsayılan olarak, ses çıkışı riff-16khz-16bit-mono-pcm olarak ayarlanır. Desteklenen ses çıkışları hakkında daha fazla bilgi için [Ses çıkış biçimlerine](https://docs.microsoft.com/azure/cognitive-services/speech-service/long-audio-api#audio-output-formats)bakın.

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

Giriş dosyanızı kaynak olarak kullanarak metni sentezlemek için bir istekte bulunalım. Aşağıdaki istekte birkaç şeyi güncelleştirmeniz gerekir:

* Konuşma `<your_key>` hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.
* Konuşma `<region>` kaynağınızın oluşturulduğu bölgeyle değiştirin `eastus` (örneğin: veya). `westus` Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.
* Metinden konuşmaya hazırladığınız metin dosyasına giden yolu değiştirin. `<input>`
* İstediğin `<locale>` çıkış yerel le değiştirin. Daha fazla bilgi için [dil desteğine](../../language-support.md#neural-voices)bakın.
* İstediğiniz çıkış sesi ile değiştirin. `<voice_guid>` [Desteklenen seslerin listesini al](#get-a-list-of-supported-voices)tarafından döndürülen seslerden birini kullanın.

Bu komutla metni konuşmaya dönüştürün:

```console
python voice_synthesis_client.py --submit -key <your_key> -region <Region> -file <input> -locale <locale> -voiceId <voice_guid>
```

> [!NOTE]
> 1'den fazla giriş dosyanız varsa, birden çok istek göndermeniz gerekir. Farkında olması gereken bazı sınırlamalar vardır. 
> * İstemci, her Azure abonelik hesabı için sunucuya saniyede en fazla **5** istek gönderebilir. Sınırlamayı aşarsa, istemci 429 hata kodu (çok fazla istek) alır. Lütfen istek miktarını saniyede azaltın
> * Sunucunun her Azure abonelik hesabı için **120'ye** kadar istek çalışmasına ve sıraya girmesine izin verilir. Sınırlamayı aşarsa, sunucu 429 hata kodu (çok fazla istek) döndürecektir. Lütfen bazı istekler tamamlanana kadar yeni istek göndermeyi bekleyin ve kaçının

Şuna benzer bir çıktı görürsünüz:

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

Sonuç, hizmet tarafından oluşturulan giriş metnini ve ses çıktısı dosyalarını içerir. Bir zip bu dosyaları indirebilirsiniz.

## <a name="remove-previous-requests"></a>Önceki istekleri kaldırma

Sunucu, her Azure abonelik hesabı için en fazla **20.000** istek tutar. İstek tutarınız bu sınırlamayı aşarsa, lütfen yenilerini yapmadan önce önceki istekleri kaldırın. Varolan istekleri kaldırmazsanız, bir hata bildirimi alırsınız.

Kodu `voice_synthesis_client.py`ekle:

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

Şimdi, daha önce hangi istekleri gönderdiğinize bakalım. Devam etmeden önce, bu istekteki birkaç şeyi güncelleştirmeniz gerekir:

* Konuşma `<your_key>` hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.
* Konuşma `<region>` kaynağınızın oluşturulduğu bölgeyle değiştirin `eastus` (örneğin: veya). `westus` Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.

Şu komutu çalıştırın:

```console
python voice_synthesis_client.py --syntheses -key <your_key> -region <Region>
```

Bu, yaptığınız sentez isteklerinin bir listesini döndürecektir. Şöyle bir çıktı görürsünüz:

```console
There are <number> synthesis requests submitted:
ID : xxx , Name : xxx, Status : Succeeded
ID : xxx , Name : xxx, Status : Running
ID : xxx , Name : xxx : Succeeded
```

Şimdi, daha önce gönderilen bir isteği kaldıralım. Aşağıdaki kodda birkaç şeyi güncelleştirmeniz gerekir:

* Konuşma `<your_key>` hizmeti abonelik anahtarınızla değiştirin. Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.
* Konuşma `<region>` kaynağınızın oluşturulduğu bölgeyle değiştirin `eastus` (örneğin: veya). `westus` Bu bilgiler, [Azure portalındaki](https://aka.ms/azureportal)kaynağınız için **Genel Bakış** sekmesinde kullanılabilir.
* Önceki `<synthesis_id>` istekte döndürülen değerle değiştirin.

> [!NOTE]
> 'Çalışan'/'Bekletme' durumu olan istekler kaldırılamaz veya silinemez.

Şu komutu çalıştırın:

```console
python voice_synthesis_client.py --delete -key <your_key> -region <Region> -synthesisId <synthesis_id>
```

Şöyle bir çıktı görürsünüz:

```console
delete voice synthesis xxx
delete successful
```

## <a name="get-the-full-client"></a>Tam istemciyi alın

Tamamlanan `voice_synthesis_client.py` [GitHub](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Python/voiceclient.py)indirmek için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uzun Ses API'si hakkında daha fazla bilgi edinin](../../long-audio-api.md)
