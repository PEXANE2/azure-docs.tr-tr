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
ms.openlocfilehash: 3b96ded52aa2148df9e96d6f1e878d50b821abab
ms.sourcegitcommit: d29e7d0235dc9650ac2b6f2ff78a3625c491bbbf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/17/2020
ms.locfileid: "76170473"
---
# <a name="speech-services-with-sovereign-clouds"></a>Sogeign bulutları ile konuşma Hizmetleri

## <a name="azure-government-united-states"></a>Azure Kamu (Birleşik Devletler)

Denetim altındaki ABD vatandaşları tarafından kontrol edilen işlemlerle bu adanmış örneğe yalnızca ABD’deki federal, eyalet, yerel ve kabile devlet kurumları ile bunların iş ortakları erişebilir.
- Bölgeler: US Gov Virginia
- SpeechSDK 'de SR:*config. FromHost ("WSS://Virginia.STT.Speech.Azure.us", "\<anahtarınızı\>");*
- SpeechSDK: config içinde TTS *. FromHost ("https[]()://Virginia.TTS.Speech.Azure.us", "\<anahtarınızı\>");*
- Kimlik doğrulama belirteçleri:[]()https://Virginia.api.Cognitive.Microsoft.us/STS/v1.0/issueToken
- Azure portalı: https://portal.azure.us  
- Özel Konuşma Tanıma Portalı: https://virginia.cris.azure.us/Home/CustomSpeech
- Kullanılabilir SKU 'Lar: S0
- Desteklenen Özellikler:
  - Konuşmayı Metne Dönüştürme
  - Özel Konuşma Tanıma (akustik/dil uyarlama)
  - Metin Okuma
  - Konuşma Çeviricisi
- Desteklenmeyen özellikler
  - Özel Ses
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
- SpeechSDK 'de SR: *config. FromHost ("WSS://chinaeast2.STT.Speech.Azure.cn", "\<anahtarınızı\>");*
- SpeechSDK: config içinde TTS *. FromHost ("https[]()://chinaeast2.TTS.Speech.Azure.cn", "\<anahtarınızı\>");*
- Kimlik doğrulama belirteçleri:[]()https://chinaeast2.api.Cognitive.Microsoft.cn/STS/v1.0/issueToken
- Azure portalı: https://portal.azure.cn
- Özel Konuşma Tanıma Portalı: https://chinaeast2.cris.azure.cn/Home/CustomSpeech
- Kullanılabilir SKU 'Lar: S0
- Desteklenen Özellikler:
  - Konuşmayı Metne Dönüştürme
  - Özel Konuşma Tanıma (akustik/dil uyarlama)
  - Metin Okuma
  - Konuşma Çeviricisi
- Desteklenmeyen özellikler
  - Özel Ses
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

