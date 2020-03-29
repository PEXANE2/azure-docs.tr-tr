---
title: Egemen Bulutlar - Konuşma hizmeti
titleSuffix: Azure Cognitive Services
description: Sovereign Clouds'u nasıl kullanacağınızı öğrenin
services: cognitive-services
author: cbasoglu
manager: xdh
ms.service: cognitive-services
ms.subservice: speech-service
ms.topic: conceptual
ms.date: 1/14/2020
ms.author: cbasoglu
ms.openlocfilehash: b41967033b00144ca5bd52ce23cf8aabcea6749e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78228094"
---
# <a name="speech-services-with-sovereign-clouds"></a>Egemen bulutlarla konuşma hizmetleri

## <a name="azure-government-united-states"></a>Azure Devlet (ABD)

Sadece ABD federal, eyalet, yerel ve kabile hükümetleri ve ortakları, taranmış ABD vatandaşları tarafından kontrol edilen operasyonlarla bu özel örne erişebilir.
- Bölgeler: ABD Gov Virginia
- SR SpeechSDK içinde:*config. FromHost("wss://virginia.stt.speech.azure.us",\<" your-key\>");*
- Konuşmalarda TTS: *config. FromHost("https://virginia.tts.speech.azure.us",[]()\<" your-key\>");*
- Kimlik Doğrulama Belirteçleri: https://virginia.api.cognitive.microsoft.us/sts/v1.0/issueToken[]()
- Azure Portalı:https://portal.azure.us  
- Özel Konuşma Portalı:https://virginia.cris.azure.us/Home/CustomSpeech
- Mevcut SKUs: S0
- Desteklenen özellikler:
  - Konuşmayı Metne Dönüştürme
  - Özel Konuşma (Akustik/dil uyarlaması)
  - Metin Okuma
  - Konuşma Çevirmeni
- Desteklenmeyen özellikler
  - Özel Ses
  - Metinden konuşmaya nöral sesler
- Desteklenen yerel: Aşağıdaki dillerin yerel alaları desteklenir.
  - Arapça (ar-*)
  - Çince (zh-*)
  - İngilizce (tr-*)
  - Fransızca (fr-*)
  - Almanca (de-*)
  - Hintçe
  - Korece
  - Rusça
  - İspanyolca (es-*)

## <a name="microsoft-azure-china"></a>Microsoft Azure Çin

Çin'de bulunan, Çinli kullanıcıların yüksek hızlı ve istikrarlı yerel ağ erişim deneyimi sağlaması için China Mobile, China Telecom, China Unicom ve diğer büyük taşıyıcı omurga ağına doğrudan erişimi olan bir Azure veri merkezidir.
- Bölgeler: Çin Doğu 2 (Şangay)
- SR SpeechSDK içinde: *config. FromHost("wss://chinaeast2.stt.speech.azure.cn",\<" your-key\>");*
- Konuşmalarda TTS: *config. FromHost("https://chinaeast2.tts.speech.azure.cn",[]()\<" your-key\>");*
- Kimlik Doğrulama Belirteçleri: https://chinaeast2.api.cognitive.azure.cn/sts/v1.0/issueToken[]()
- Azure Portalı:https://portal.azure.cn
- Özel Konuşma Portalı:https://speech.azure.cn/CustomSpeech
- Mevcut SKUs: S0
- Desteklenen özellikler:
  - Konuşmayı Metne Dönüştürme
  - Özel Konuşma (Akustik/dil uyarlaması)
  - Metin Okuma
  - Konuşma Çevirmeni
- Desteklenmeyen özellikler
  - Özel Ses
  - Metinden konuşmaya nöral sesler
- Desteklenen yerel: Aşağıdaki dillerin yerel alaları desteklenir.
  - Arapça (ar-*)
  - Çince (zh-*)
  - İngilizce (tr-*)
  - Fransızca (fr-*)
  - Almanca (de-*)
  - Hintçe
  - Korece
  - Rusça
  - İspanyolca (es-*)

