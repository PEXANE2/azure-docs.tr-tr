---
title: Sovereign bulutlar-konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Sovereign bulutlarını nasıl kullanacağınızı öğrenin
services: cognitive-services
author: cbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 1/14/2020
ms.author: cbasoglu
ms.openlocfilehash: b41967033b00144ca5bd52ce23cf8aabcea6749e
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "78228094"
---
# <a name="speech-services-with-sovereign-clouds"></a>Sogeign bulutları ile konuşma Hizmetleri

## <a name="azure-government-united-states"></a>Azure Kamu (Birleşik Devletler)

Yalnızca ABD Federal, eyalet, yerel ve kaş kamu kurumları ve iş ortakları, denetimli ABD vatandaşları tarafından denetlenen işlemlerle bu adanmış örneğe erişebilir.
- Bölgeler: US Gov Virginia
- SpeechSDK 'de SR:*config. FromHost ("WSS://Virginia.STT.Speech.Azure.us", "\<anahtarınız\>");*
- SpeechSDK: config içinde TTS *. FromHost ("https[]()://Virginia.TTS.Speech.Azure.us", "\<anahtarınız\>");*
- Kimlik doğrulama belirteçleri:[]()https://Virginia.api.Cognitive.Microsoft.us/STS/v1.0/issueToken
- Azure portalı:https://portal.azure.us  
- Özel Konuşma Tanıma Portalı:https://virginia.cris.azure.us/Home/CustomSpeech
- Kullanılabilir SKU 'Lar: S0
- Desteklenen Özellikler:
  - Konuşmayı Metne Dönüştürme
  - Özel Konuşma Tanıma (akustik/dil uyarlama)
  - Metin Okuma
  - Konuşma Çeviricisi
- Desteklenmeyen özellikler
  - Özel ses
  - Metin okuma için sinir sesleri
- Desteklenen yerel ayarlar: aşağıdaki diller için yerel ayarlar desteklenir.
  - Arapça (ar-*)
  - Çince (zh-*)
  - İngilizce (en-*)
  - Fransızca (fr-*)
  - Almanca (de-*)
  - Hintçe
  - Korece
  - Rusça
  - İspanyolca (es-*)

## <a name="microsoft-azure-china"></a>Microsoft Azure Çin

Çin 'de, Çin kullanıcılarının yüksek hızlı ve kararlı yerel ağ erişim deneyimi sağlaması için Çin Mobile, Çin Teleçliği, Çin Unicom ve diğer büyük taşıyıcı omurga ağına doğrudan erişimi olan bir Azure veri merkezi de bulunur.
- Bölgeler: Çin Doğu 2 (Shanghai)
- SpeechSDK 'de SR: *config. FromHost ("WSS://chinaeast2.STT.Speech.Azure.cn", "\<anahtarınız\>");*
- SpeechSDK: config içinde TTS *. FromHost ("https[]()://chinaeast2.TTS.Speech.Azure.cn", "\<anahtarınız\>");*
- Kimlik doğrulama belirteçleri:[]()https://chinaeast2.api.Cognitive.Azure.cn/STS/v1.0/issueToken
- Azure portalı:https://portal.azure.cn
- Özel Konuşma Tanıma Portalı:https://speech.azure.cn/CustomSpeech
- Kullanılabilir SKU 'Lar: S0
- Desteklenen Özellikler:
  - Konuşmayı Metne Dönüştürme
  - Özel Konuşma Tanıma (akustik/dil uyarlama)
  - Metin Okuma
  - Konuşma Çeviricisi
- Desteklenmeyen özellikler
  - Özel ses
  - Metin okuma için sinir sesleri
- Desteklenen yerel ayarlar: aşağıdaki diller için yerel ayarlar desteklenir.
  - Arapça (ar-*)
  - Çince (zh-*)
  - İngilizce (en-*)
  - Fransızca (fr-*)
  - Almanca (de-*)
  - Hintçe
  - Korece
  - Rusça
  - İspanyolca (es-*)

