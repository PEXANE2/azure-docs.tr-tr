---
title: Uzun Ses API (Önizleme) - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Uzun Ses API'sinin uzun biçimli metnin konuşmaya eşzamanlı sentezi için nasıl tasarlandığını öğrenin.
services: cognitive-services
author: trevorbye
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: trbye
ms.openlocfilehash: b7cca314ec59e46cf17751b1aec28b5c3ea029ed
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81401069"
---
# <a name="long-audio-api-preview"></a>Uzun Ses API (Önizleme)

Uzun Ses API konuşma (örneğin: sesli kitaplar) uzun biçimli metin asynchronous sentezi için tasarlanmıştır. Bu API sentezlenmiş sesi gerçek zamanlı olarak döndürmez, bunun yerine beklenti yanıt(lar) için anket yapacak ve çıktı(lar) hizmetten kullanılabilir hale getirilir gibi tüketmek tir. Konuşma SDK tarafından kullanılan konuşma API metin aksine, Uzun Ses API yayıncılar ve ses içerik platformları için ideal hale, 10 dakikadan daha uzun sentezlenmiş ses oluşturabilirsiniz.

Uzun Ses API ek faydaları:

* Hizmet tarafından döndürülen sentezlenmiş konuşma, yüksek doğrulukta ses çıkışları sağlayan sinirsel sesler kullanır.
* Gerçek zamanlı yanıtlar desteklenmediğinden, sesli bitiş noktası dağıtmaya gerek yoktur.

> [!NOTE]
> Uzun Ses API şimdi sadece [Özel Nöral Ses](https://docs.microsoft.com/azure/cognitive-services/speech-service/how-to-custom-voice#custom-neural-voices)destekler.

## <a name="workflow"></a>İş akışı

Genellikle, Uzun Ses API'sini kullanırken, sentezlenecek bir metin dosyası veya dosya gönderirsiniz, durum için anket, sonra durum başarılı olursa, ses çıktısını indirebilirsiniz.

Bu diyagram, iş akışına üst düzey bir genel bakış sağlar.

![Uzun Ses API iş akışı diyagramı](media/long-audio-api/long-audio-api-workflow.png)

## <a name="prepare-content-for-synthesis"></a>İçeriği senteze hazırlama

Metin dosyanızı hazırlarken aşağıdakileri yapın:

* Düz metin (.txt) veya SSML metni (.txt)
* [Byte Order Mark (BOM) ile UTF-8](https://www.w3.org/International/questions/qa-utf8-bom.en#bom) olarak kodlanır
* Tek bir dosya değil, bir zip
* Düz metin için 400'den fazla karakter veya SSML metni için 400'den fazla [faturalandırılabilir karakter](https://docs.microsoft.com/azure/cognitive-services/speech-service/text-to-speech#pricing-note) ve 10.000'den az paragraf içerir
  * Düz metin için her paragraf **Enter/Return** - [Düz metin giriş örneğini](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/en-US.txt) görüntüle tuşuna basarak ayrılır
  * SSML metni için her SSML parçası paragraf olarak kabul edilir. SSML parçaları farklı paragraflarla ayrılacak - [SSML metin giriş örneğini](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/SSMLTextInputSample.txt) görüntüle
> [!NOTE]
> Çince (Anakara), Çince (Hong Kong), Çince (Tayvan), Japonca ve Korece için bir sözcük iki karakter olarak sayılır. 

## <a name="submit-synthesis-requests"></a>Sentez istekleri gönderme

Giriş içeriğini hazırladıktan sonra, isteği göndermek için [uzun biçimli ses sentezini hızlı](https://aka.ms/long-audio-python) bir şekilde takip edin. Birden fazla giriş dosyanız varsa, birden çok istek göndermeniz gerekir. Dikkat edilmesi gereken bazı sınırlamalar vardır: 
* İstemci, her Azure abonelik hesabı için sunucuya saniyede en fazla 5 istek gönderebilir. Sınırlamayı aşarsa, istemci 429 hata kodu (çok fazla istek) alır. Lütfen istek miktarını saniyede azaltın
* Sunucunun her Azure abonelik hesabı için 120'ye kadar istek çalışmasına ve sıraya girmesine izin verilir. Sınırlamayı aşarsa, sunucu 429 hata kodu (çok fazla istek) döndürecektir. Lütfen bazı istekler tamamlanana kadar yeni istek göndermeyi bekleyin ve kaçının
* Sunucu, her Azure abonelik hesabı için en fazla 20.000 istek tutar. Sınırlamayı aşarsa, lütfen yenilerini göndermeden önce bazı istekleri silin

## <a name="audio-output-formats"></a>Ses çıkış biçimleri

Esnek ses çıkış biçimlerini destekliyoruz. Paragraf başına ses çıkışları oluşturabilir veya 'concatenateResult' parametresini ayarlayarak sesleri tek bir çıktıya dönüştürebilirsiniz. Aşağıdaki ses çıkış biçimleri Uzun Ses API'sı tarafından desteklenir:

> [!NOTE]
> Varsayılan ses biçimi riff-16khz-16bit-mono-pcm'dir.

* riff-8khz-16bit-mono-pcm
* riff-16khz-16bit-mono-pcm
* riff-24khz-16bit-mono-pcm
* riff-48khz-16bit-mono-pcm
* ses-16khz-32kbitrate-mono-mp3
* ses-16khz-64kbitrate-mono-mp3
* ses-16khz-128kbitrate-mono-mp3
* ses-24khz-48kbitrate-mono-mp3
* ses-24khz-96kbitrate-mono-mp3
* ses-24khz-160kbitrate-mono-mp3

## <a name="quickstarts"></a>Hızlı Başlangıçlar

Uzun Ses API'sini başarılı bir şekilde çalıştırmanıza yardımcı olmak için tasarlanmış hızlı başlangıçlar sunuyoruz. Bu tablo, dile göre düzenlenen Uzun Ses API hızlı başlatmalarının bir listesini içerir.

* [Hızlı başlangıç: Python](https://aka.ms/long-audio-python)

## <a name="sample-code"></a>Örnek kod
Uzun Ses API'si için örnek kod GitHub'da mevcuttur.

* [Örnek kod: Python](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/Python)
* [Örnek kod: C #](https://github.com/Azure-Samples/Cognitive-Speech-TTS/tree/master/CustomVoice-API-Samples/CSharp)
* [Örnek kod: Java](https://github.com/Azure-Samples/Cognitive-Speech-TTS/blob/master/CustomVoice-API-Samples/Java/)
