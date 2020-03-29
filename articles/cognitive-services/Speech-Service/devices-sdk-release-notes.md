---
title: Konuşma Cihazları SDK belgeleri
titleSuffix: Azure Cognitive Services
description: Sürüm notları, Konuşma Aygıtları SDK'da güncelleştirmeler, geliştirmeler, hata düzeltmeleri ve değişikliklerin bir günlüğünü sağlar. Bu makale, Konuşma Aygıtları SDK'nın her sürümüyle güncelleştirilir.
services: cognitive-services
author: wsturman
manager: nitinme
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 02/12/2020
ms.author: wellsi
ms.openlocfilehash: 29dcb3c0e74482fd6670d1a0983e751043379d6e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80371611"
---
# <a name="release-notes-speech-devices-sdk"></a>Yayın notları: Konuşma Cihazları SDK

Aşağıdaki bölümlerde en son sürümlerde değişiklikler listelenerilmiştir.

## <a name="speech-devices-sdk-190"></a>Konuşma Cihazları SDK 1.9.0:

- [Urbetter DDK](https://aka.ms/sdsdk-download-urbetter) (Linux ARM64) için ilk ikililer sağlanmaktadır.
- Roobo v1 şimdi Konuşma SDK için Maven kullanır
- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.9.0 güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.

## <a name="speech-devices-sdk-170"></a>Konuşma Cihazları SDK 1.7.0:

- Linux ARM artık desteklendi.
- [Roobo v2 DDK](https://aka.ms/sdsdk-download-roobov2) için ilk ikililer sağlanmaktadır (Linux ARM64).
- Windows kullanıcıları `AudioConfig.fromDefaultMicrophoneInput()` kullanılacak `AudioConfig.fromMicrophoneInput(deviceName)` mikrofonu kullanabilir veya belirtebilir.
- Kitaplık boyutu optimize edilmiştir.
- Aynı konuşma/niyet tanıyıcı nesnesini kullanarak çoklu dönüş tanıma desteği.
- Tanımayı durdururken oluşacak ara sıra askıda kalma düzeltme.
- Örnek uygulamalar artık dosyanın biçimini göstermek için örnek katılımcılar.özellikler dosyası içerir.
- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.7.0 güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.

## <a name="speech-devices-sdk-160"></a>Konuşma Cihazları SDK 1.6.0:

- Ortak [örnek uygulamayla](https://aka.ms/sdsdk-download) Windows ve Linux'ta Azure [Kinect DK'yi](https://azure.microsoft.com/services/kinect-dk/) destekleyin
- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.6.0 güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.

## <a name="speech-devices-sdk-151"></a>Konuşma Cihazları SDK 1.5.1:

- Örnek uygulamaya [Konuşma Transkripsiyon'u](conversation-transcription-service.md) ekleyin.
- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.5.1 olarak güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.

## <a name="speech-devices-sdk-150-2019-may-release"></a>Konuşma Cihazları SDK 1.5.0: 2019-Mayıs sürümü

- Konuşma Aygıtları SDK artık GA ve artık bir kapılı önizleme.
- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.5.0 olarak güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.
- Yeni anahtar kelime teknolojisi önemli kalite iyileştirmelergetiriyor, Breaking Changes bakın.
- Gelişmiş uzak alan tanıma için yeni ses işleme boru hattı.

**Yeni değişiklikler**

- Yeni anahtar kelime teknolojisi sayesinde tüm anahtar kelimeler geliştirilmiş anahtar kelime portalımızda yeniden oluşturulmalıdır. Eski anahtar kelimeleri cihazdan tamamen kaldırmak için eski uygulamayı kaldırın.
  - adb kaldır com.microsoft.cognitiveservices.speech.samples.sdsdkstarterapp

## <a name="speech-devices-sdk-140-2019-apr-release"></a>Konuşma Cihazları SDK 1.4.0: 2019-Nisan açıklaması

- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.4.0 güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.

## <a name="speech-devices-sdk-131-2019-mar-release"></a>Konuşma Cihazları SDK 1.3.1: 2019-Mar sürümü

- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.3.1 olarak güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.
- Anahtar kelime işleme güncelleştirildi, bkz.
- Örnek uygulama hem konuşma tanıma hem de çeviri için dil seçimi ekler.

**Yeni değişiklikler**

- [Bir anahtar kelime yükleme](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-devices-sdk-create-kws) basitleştirilmiştir, şimdi uygulamanın bir parçası ve cihazda ayrı yükleme gerekmez.
- Anahtar kelime tanıma değişti ve iki olay desteklenir.
  - `RecognizingKeyword,`konuşma sonucunu (doğrulanmamış) anahtar kelime metnini içerdiğini gösterir.
  - `RecognizedKeyword`, anahtar kelime tanımanın verilen anahtar kelimeyi tanımayı tamamladığını gösterir.

## <a name="speech-devices-sdk-110-2018-nov-release"></a>Konuşma Cihazları SDK 1.1.0: 2018-Kasım sürümü

- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.1.0 olarak güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.
- Gelişmiş ses işleme algoritmamızla Uzak Alan Konuşma tanıma doğruluğu artırıldı.
- Örnek uygulama Çince konuşma tanıma desteği eklendi.

## <a name="speech-devices-sdk-101-2018-oct-release"></a>Konuşma Cihazları SDK 1.0.1: 2018-Eki yayın

- Konuşma [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni sürüm 1.0.1 olarak güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](https://aka.ms/csspeech/whatsnew)bakın.
- Geliştirilmiş ses işleme algoritmamızla konuşma tanıma doğruluğu geliştirilecektir
- Bir sürekli tanıma ses oturumu hata sabittir.

**Yeni değişiklikler**

- Bu sürümle bir dizi kırılma değişikliği tanıtıldı. API'ler ile ilgili ayrıntılar için [lütfen bu sayfayı](https://aka.ms/csspeech/breakingchanges_1_0_0) kontrol edin.
- KWS model dosyaları Konuşma Aygıtları SDK 1.0.1 ile uyumlu değildir. Varolan anahtar kelime dosyaları, yeni anahtar kelime dosyaları aygıta yazıldıktan sonra silinir.

## <a name="speech-devices-sdk-050-2018-aug-release"></a>Konuşma Cihazları SDK 0.5.0: 2018-Ağu açıklaması

- Ses işleme kodundaki bir hatayı düzelterek konuşma tanıma nın doğruluğunu artırın.
- Speech [SDK](https://docs.microsoft.com/azure/cognitive-services/speech-service/speech-sdk-reference) bileşeni0.5.0 sürümüne güncelleştirildi. Daha fazla bilgi [için, sürüm notlarına](releasenotes.md#cognitive-services-speech-sdk-050-2018-july-release)bakın.

## <a name="speech-devices-sdk-0212733-2018-may-release"></a>Konuşma Cihazları SDK 0.2.12733: 2018-Mayıs yayın

Bilişsel Hizmetler Konuşma Cihazları SDK ilk genel önizleme sürümü.
