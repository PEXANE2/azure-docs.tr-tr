---
title: Yerelleştirme desteği | Microsoft Azure haritaları
description: Bu makalede Microsoft Azure haritaların Hizmetleri için desteklenen diller hakkında bilgi edineceksiniz.
author: philmea
ms.author: philmea
ms.date: 11/20/2019
ms.topic: conceptual
ms.service: azure-maps
services: azure-maps
manager: philmea
ms.openlocfilehash: 2bab7b101b5461d854bb2db509b5cb243012a592
ms.sourcegitcommit: 493b27fbfd7917c3823a1e4c313d07331d1b732f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/21/2020
ms.locfileid: "83746018"
---
# <a name="localization-support-in-azure-maps"></a>Azure haritalar 'da yerelleştirme desteği

Azure haritalar, ülkeye/bölgeye göre çeşitli dilleri ve görünümleri destekler. Bu makalede, Azure haritalar uygulamanıza kılavuzluk etmenize yardımcı olacak desteklenen diller ve görünümler sağlanmaktadır.


## <a name="azure-maps-supported-languages"></a>Azure haritalar desteklenen diller

Azure haritalar, hizmetleri genelinde çeşitli dillerde yerelleştirilmiştir. Aşağıdaki tabloda her hizmet için desteklenen dil kodları sağlanmaktadır.  
  

| ID         | Name                   |  Haritalar | Arayın | Yönlendirme | Hava durumu | Trafik olayları | JS eşleme denetimi |
|------------|------------------------|:-----:|:------:|:-------:|:--------:|:-----------------:|:--------------:|
| AF-ZA      | Afrikaner              |       |    ✓   |    ✓    |         |                   |                |
| ar-SA      | Arapça                 |   ✓   |    ✓   |    ✓    |    ✓      |         ✓         |        ✓       |
| milyar TL-BD      | Bangla (Bangladeş)    |       |       |         |     ✓    |                   |                |
| milyar TL-ın      | Bangla (Hindistan)         |       |       |         |     ✓    |                   |                |
| BS-BA      | Boşnakça                 |       |       |         |     ✓    |                   |                |
| AB-ES      | Baskça                 |       |    ✓   |         |         |                   |                |
| bg-BG      | Bulgarca              |   ✓   |    ✓   |    ✓    |     ✓     |                   |        ✓       |
| ca-ES      | Katalanca                |       |    ✓   |         |    ✓      |                   |                |
| zh-HanS    | Basitleştirilmiş Çince   |       |  zh-CN |         |     zh-CN   |                   |                |
| zh-HanT    | Çince (Hong Kong ÖIB)  |  |   |    |    zh-HK   |                   |           |
| zh-HanT    | Çince (Tayvan)  | zh-TW |  zh-TW |  zh-TW  |    zh-TW   |                   |      zh-TW     |
| hr-HR      | Hırvatça               |       |    ✓   |         |    ✓      |                   |                |
| cs-CZ      | Çekçe                  |   ✓   |    ✓   |    ✓    |    ✓      |         ✓         |        ✓       |
| da-DK      | Danca                 |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| nl-      | Felemenkçe (Belçika)        |       |    ✓   |         |      ✓    |                   |                |
| nl-NL      | Felemenkçe (Hollanda)    |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| En-AU      | İngilizce (Avustralya)    |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| En-NZ      | İngilizce (Yeni Zelanda)  |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| en-GB      | İngilizce (Büyük Britanya) |   ✓   |    ✓   |    ✓    |     ✓     |         ✓         |        ✓       |
| tr-TR      | İngilizce (ABD)          |   ✓   |    ✓   |    ✓    |      ✓    |         ✓         |        ✓       |
| et-EE      | Estonya Dili               |       |    ✓   |         |      ✓    |         ✓         |                |
| fıfıph     | Filipino               |       |       |         |     ✓    |                   |                |
| fi-FI      | Fince                |   ✓   |    ✓   |    ✓    |      ✓    |         ✓         |        ✓       |
| fr-FR      | Fransızca                 |   ✓   |    ✓   |    ✓    |      ✓    |         ✓         |        ✓       |
| fr-CA      | Fransızca (Kanada)      |       |    ✓   |         |     ✓     |                   |                |
| gl-ES      | Galiçya Dili               |       |    ✓   |         |         |                   |                |
| de-DE      | Almanca                 |   ✓   |    ✓   |    ✓    |   ✓      |         ✓         |        ✓       |
| el-GR      | Yunanca                  |   ✓   |    ✓   |    ✓    |    ✓     |         ✓         |        ✓       |
| Gu-ın      | Gucerat dili                |       |       |         |     ✓    |                   |                |
| he-IL      | İbranice                 |       |    ✓   |         |     ✓    |         ✓         |                |
| hi-IN      | Hintçe                  |       |        |         |     ✓    |                   |                |
| hu-HU      | Macarca              |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| Şu-      | İzlandaca              |       |       |         |     ✓    |                   |                |
| id-ID      | Endonezce             |   ✓   |    ✓    |    ✓    |     ✓    |         ✓         |        ✓       |
| it-IT      | İtalyanca                |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| ja-JP      | Japonca               |       |        |         |     ✓    |                   |                |
| KN-ın      | Kannada dili                |       |       |         |     ✓    |                   |                |
| kk-KZ      | Kazakça                 |       |    ✓   |         |     ✓    |                   |                |
| ko-KR      | Korece                 |   ✓   |        |    ✓    |     ✓    |                   |        ✓       |
| es-419     | Latin Amerika Ispanyolca |       |    ✓   |         |         |                   |                |
| lv-LV      | Letonca                |       |    ✓   |         |     ✓    |         ✓         |                |
| lt-LT      | Litvanca             |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| MK-MK      | CA             |       |       |         |     ✓    |                   |                |
| ms-MY      | Malay dili (Latin)          |   ✓   |    ✓   |    ✓    |    ✓   |                   |        ✓       |
| Mr-ın      | Marathi                 |       |       |         |     ✓    |                   |                |
| nb-NO      | Norveççe Bokmål       |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| NGT        | Yerel betiklerdeki tüm bölgeler için nötr on-resmi diller varsa |   ✓     |        |         |       |        |      ✓          |
| NGT-Latn   | Nötr zemin.-Latin exonbiri. Varsa Latin betiği kullanılacaktır |   ✓     |        |         |         |                |        ✓         |
| pl-PL      | Lehçe                 |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| pt-BR      | Portekizce (Brezilya)    |   ✓   |    ✓   |    ✓    |      ✓   |                   |        ✓       |
| pt-PT      | Portekizce (Portekiz)  |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| PA-ın      | Pencap dili                 |       |       |         |     ✓    |                   |                |
| ro-RO      | Rumence               |       |    ✓    |         |     ✓    |         ✓         |                |
| ru-RU      | Rusça                |   ✓   |    ✓   |    ✓    |      ✓   |         ✓         |        ✓       |
| sr-Cyrl-RS | Sırpça (Kiril)     |       |   SR-RS  |         |    SR-RS     |                   |                |
| sr-Latn-RS | Sırpça (Latin)        |       |       |         |     sr-Latn    |                   |                |
| sk-SK      | Slovakça             |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| SL-SL      | Slovence              |   ✓   |    ✓   |    ✓    |     ✓    |                   |        ✓       |
| es-ES      | İspanyolca                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| es-MX      | İspanyolca (Meksika)       |   ✓   |        |    ✓    |     ✓    |                   |        ✓       |
| sv-SE      | İsveççe                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| ta-ın      | Tamil dili (Hindistan)                 |       |       |         |     ✓    |                   |                |
| te      | Telugu dili (Hindistan)                 |       |       |         |     ✓    |                   |                |
| th-TH      | Tayca                   |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| tr-TR      | Türkçe                |   ✓   |    ✓   |    ✓    |     ✓    |         ✓         |        ✓       |
| uk-UA      | Ukraynaca               |       |    ✓   |         |     ✓    |                   |                |
| -PK      | Urduca                 |       |       |         |     ✓    |                   |                |
| uz-Latn-UZ | Özbekçe                 |       |       |         |     ✓    |                   |                |
| vi-VN      | Vietnamca             |       |    ✓   |         |      ✓    |                  |                |


## <a name="azure-maps-supported-views"></a>Azure haritalar desteklenen görünümler

> [!Note]
> Azure Maps, 1 Ağustos 2019 tarihinde aşağıdaki ülkelerde/bölgelerde yayımlanmıştır:
>  * Arjantin
>  * Hindistan
>  * Fas
>  * Pakistan
>
> 1 Ağustos 2019 ' den sonra, **Görünüm** parametresi yukarıda listelenen yeni bölgeler/ülkeler için döndürülen harita içeriğini tanımlar. Azure Maps **Görünüm** parametresi ("Kullanıcı bölgesi parametresi" olarak da bilinir), haritada görüntülenen Kenarlıklar ve Etiketler dahil olmak üzere Azure haritalar Hizmetleri aracılığıyla hangi coğrafi bölge/bölge için doğru haritaları gösterecek olan ıkı harfli ıso-3166 ülke kodudur. 

REST API 'Leri için gereken **Görünüm** parametresini ve hizmetlerinizin kullandığı SDK 'ları ayarladığınızdan emin olun.
>  
>
>  **REST API 'Leri:**
>  
>  Görünüm parametresini gereken şekilde ayarlamış olduğunuzdan emin olun. View parametresi, Azure haritalar Hizmetleri aracılığıyla hangi geopolitik tartışmalı içerik kümesini döndürüleceğini belirtir. 
>
>  Etkilenen Azure haritalar REST Hizmetleri:
>    
>    * Harita kutucuğunu al
>    * Harita görüntüsünü al 
>    * Aramayı bulanık al
>    * Arama POı al
>    * Arama POı kategorisini al
>    * Yakında arama alın
>    * Arama adresini al
>    * Yapılandırılmış arama adresini al
>    * Arama adresini tersine al
>    * Arama adresini al çapraz cadde
>    * Geometri Içinde arama sonrası
>    * Arama sonrası adres Batch önizlemesi
>    * Arama sonrası adres geri Batch önizlemesi
>    * Rota üzerinde arama sonrası
>    * Arama sonrası benzer toplu Iş önizlemesi
>
>    
>  **SDK**
>
>  **Görünüm** parametresini gereken şekilde ayarlamış olduğunuzdan ve Web SDK 'sının en son sürümüne sahip olduğunuzdan emin olun ve Android SDK. Etkilenen SDK 'lar:
>
>    * Azure Haritalar Web SDK 'Sı
>    * Azure Haritalar Android SDK

Varsayılan olarak, istek içinde tanımlamadığınız halde görünüm parametresi **Unified**olarak ayarlanır. Kullanıcılarınızın konumunu saptayın. Ardından, bu konum için **Görünüm** parametresini doğru olarak ayarlayın. Alternatif olarak, isteğin IP adresine göre harita verilerini döndürecek olan ' View = Auto ' seçeneğini belirleyebilirsiniz.  Azure haritalar 'daki **Görünüm** parametresi, Haritalar, görüntüler ve diğer verilerin ve Azure Maps aracılığıyla erişim yetkisine sahip olduğunuz üçüncü taraf içeriklerin eşlenmesiyle ilgili yasalar dahil olmak üzere, geçerli yasaları ile uyumlu olmalıdır.


Aşağıdaki tabloda desteklenen görünümler sağlanmaktadır.

| Görüntüle         | Açıklama                            |  Haritalar | Arayın | JS Harita Denetimi |
|--------------|----------------------------------------|:-----:|:------:|:--------------:|
| AE           | Birleşik Arap Emirlikleri (Arapça görünüm)    |   ✓   |        |     ✓          |
| AR           | Arjantin (argentinian görünümü)           |   ✓   |    ✓   |     ✓          |
| BH           | Bahreyn (Arapça görünümü)                 |   ✓   |        |     ✓          |
| IN           | Hindistan (Hindistan görünümü)                    |   ✓   |   ✓     |     ✓          |
| IQ           | Irak (Arapça görünümü)                    |   ✓   |        |     ✓          |
| JO           | Ürdün (Arapça görünümü)                  |   ✓   |        |     ✓          |
| KW           | Kuveyt (Arapça görünümü)                  |   ✓   |        |     ✓          |
| LB           | Lübnan (Arapça görünümü)                 |   ✓   |        |     ✓          |
| MA           | Fas (Fas görüntüleyebilir)                |   ✓   |   ✓     |     ✓          |
| OM           | Umman (Arapça görünümü)                    |   ✓   |        |     ✓          |
| PK           | Pakistan (Pasvahili tani görünümü)              |   ✓   |    ✓    |     ✓          |
| PS           | Filistin Yönetimi (Arapça görünüm)    |   ✓   |        |     ✓          |
| QA           | Katar (Arapça görünümü)                   |   ✓   |        |     ✓          |
| SA           | Suudi Arabistan (Arapça görünümü)            |   ✓   |        |     ✓          |
| SY           | Suriye (Arapça görünümü)                   |   ✓   |        |     ✓          |
| Vet           | Yemen (Arapça görünümü)                   |   ✓   |        |     ✓          |
| Otomatik         | İsteğin IP adresine göre harita verilerini döndürün.|   ✓   |    ✓   |     ✓          |
| Standart      | Birleşik görünüm (diğerleri)                  |   ✓   |   ✓     |     ✓          |
