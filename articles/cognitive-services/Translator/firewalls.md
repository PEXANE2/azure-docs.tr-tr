---
title: Güvenlik duvarlarının arkasına çevir - Çevirmen Metin API
titleSuffix: Azure Cognitive Services
description: Azure Bilişsel Hizmetler Çevirmeni Metin API, etki alanı adı veya IP filtreleme kullanarak güvenlik duvarlarının arkasına çeviri yapabilir.
services: cognitive-services
author: swmachan
manager: nitinme
ms.service: cognitive-services
ms.subservice: translator-text
ms.topic: conceptual
ms.date: 06/04/2019
ms.author: swmachan
ms.openlocfilehash: cd7904fedd3ab3f64315cb6f98d99b8fd12254f6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "73837395"
---
# <a name="how-to-translate-behind-ip-firewalls-with-the-translator-text-api"></a>Translator Text API ile IP güvenlik duvarlarının arkasına nasıl çevrililir?

Çevirmen Metin API'si, etki alanı adı veya IP filtreleme kullanarak güvenlik duvarlarının arkasına çeviri yapabilir. Etki alanı adı filtreleme tercih edilen yöntemdir. Microsoft Translator'ı IP filtreli bir güvenlik duvarının arkasından **çalıştırmanızı önermiyoruz.** Kurulum gelecekte haber vermeden kırmak olasıdır.

## <a name="translator-ip-addresses"></a>Çevirmen IP Adresleri
api.cognitive.microsofttranslator.com - Microsoft Translator Text API'nin IP adresleri 21 Ağustos 2019 itibariyle:

* **Asya Pasifik:** 20.40.125.208, 20.43.88.240, 20.184.58.62, 40.90.139.163, 104.44.89.44
* **Avrupa:** 40.90.138.4, 40.90.141.99, 51.105.170,64, 52.155.218.251
* **Kuzey Amerika:** 40.90.139.36, 40.90.139.2, 40.119.2.134, 52.224.200.129, 52.249.207.163


## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> [Translator API aramanızda IP güvenlik duvarlarının arkasına çevirin](reference/v3-0-translate.md)
