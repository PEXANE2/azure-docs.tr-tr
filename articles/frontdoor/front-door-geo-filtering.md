---
title: Azure ön kapısı için bir etki alanında coğrafi filtreleme | Microsoft Docs
description: Bu makalede, Azure ön kapısının coğrafi filtreleme ilkesi hakkında bilgi edinirsiniz
services: frontdoor
documentationcenter: ''
author: KumudD
editor: ''
ms.service: frontdoor
ms.workload: infrastructure-services
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 03/21/2019
ms.author: kumud
ms.reviewer: tyao
ms.openlocfilehash: 2cdde705d0e9f0905d4c33648b5415758d838b06
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80411174"
---
# <a name="what-is-geo-filtering-on-a-domain-for-azure-front-door"></a>Azure Front Door için bir etki alanında coğrafi filtreleme nedir?

Varsayılan olarak, Azure ön kapısı isteği yapan kullanıcının konumundan bağımsız olarak Kullanıcı isteklerine yanıt verir. Ancak bazı durumlarda, Web uygulamalarınıza olan erişimi ülkeye/bölgeye göre kısıtlamak isteyebilirsiniz. Ön kapıda Web uygulaması güvenlik duvarı (WAF) hizmeti, belirli ülkelerde/bölgelerde erişime izin vermek veya erişimi engellemek için uç noktanıza özel erişim kuralları kullanarak bir ilke tanımlamanızı sağlar. 

WAF ilkesi genellikle bir dizi özel kural içerir. Bir kural eşleşme koşullarından, bir eylemden ve bir öncelikten oluşur. Eşleşme koşulunda bir eşleşme değişkeni, işleç ve eşleşme değeri tanımlarsınız.  Coğrafi filtreleme kuralı için eşleşme değişkeni REMOTE_ADDR, işleç GeoMatch, değer ise söz konusu ülkenin iki harfli kodudur. Yola göre bir coğrafi filtreleme kuralı oluşturmak için bir GeoMatch koşulunu ve bir REQUEST_URI dizesi eşleşme koşulunu birleştirebilirsiniz.

[Azure PowerShell](front-door-tutorial-geo-filtering.md) kullanarak veya [hızlı başlangıç şablonumuzu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering)kullanarak, ön kapıya yönelik bir coğrafi filtreleme ilkesi yapılandırabilirsiniz.

## <a name="country-code-reference"></a>Ülke kodu başvurusu

|Ülke kodu | Ülke adı |
| ----- | ----- |
| AD | Andorra |
| AE | Birleşik Arap Emirlikleri|
| AF | Afganistan|
| AG | Antigua ve Barbuda|
| AL | Arnavutluk|
| AM | Ermenistan|
| AO | Angola|
| AR | Arjantin|
| AS | Amerikan Samoası|
| AT | Avusturya|
| AU | Avustralya|
| AZ | Azerbaycan|
| BA | Bosna Hersek|
| BB | Barbados|
| BD | Bangladeş|
| BE | Belçika|
| BF | Burkina Faso|
| BG | Bulgaristan|
| BH | Bahreyn|
| BI | Burundi|
| BJ | Benin|
| BL | Saint Barthélimy|
| BN | Barış Yurdu Brunei Devleti|
| BO | Bolivya|
| BR | Brezilya|
| BS | Bahamalar|
| BT | Butan|
| BW | Botsvana|
| BY | Belarus|
| BZ | Beliz|
| CA | Kanada|
| CD | Demokratik Kongo Cumhuriyeti|
| CF | Orta Afrika Cumhuriyeti|
| CH | İsviçre|
| CI | Fildişi Kıyısı|
| CL | Şili|
| CM | Kamerun|
| CN | Çin|
| CO | Kolombiya|
| CR | Kosta Rika|
| CU | Küba|
| CV | Cabo Verde|
| CY | Kıbrıs|
| CZ | Çek Cumhuriyeti|
| DE | Almanya|
| DK | Danimarka|
| DO | Dominik Cumhuriyeti|
| DZ | Cezayir|
| EC | Ekvador|
| EE | Estonya|
| EG | Mısır|
| ES | İspanya|
| ET | Etiyopya|
| FI | Finlandiya|
| FJ | Fiji|
| FM | Mikronezya Federe Devletleri|
| GS | Fransa|
| GB | Birleşik Krallık|
| GE | Gürcistan|
| GF | Fransız Ginesi|
| GH | Gana|
| GN | Gine|
| GP | Guadalupe|
| GR | Yunanistan|
| GT | Guatemala|
| GY | Guyana|
| HK | Hong Kong ÖİB|
| HN | Honduras|
| HR | Hırvatistan|
| HT | Haiti|
| HU | Macaristan|
| Kimlik | Endonezya|
| IE | İrlanda|
| IL | İsrail|
| IN | Hindistan|
| IQ | Irak|
| IR | İran İslam Cumhuriyeti|
| IS | İzlanda|
| BT | İtalya|
| JM | Jamaika|
| JO | Ürdün|
| JP | Japonya|
| KE | Kenya|
| KG | Kırgızistan|
| KH | Kamboçya|
| KI | Kiribati|
| KN | Saint Kitts ve Nevis|
| KP | Kore Demokratik Halk Cumhuriyeti|
| KR | Kore Cumhuriyeti|
| KW | Kuveyt|
| KY | Kayman Adaları|
| KZ | Kazakistan|
| LA | Laos Demokratik Halk Cumhuriyeti|
| LB | Lübnan|
| LI | Lihtenştayn|
| LK | Sri Lanka|
| LR | Liberya|
| LS | Lesoto|
| LT | Litvanya|
| LU | Lüksemburg|
| LV | Letonya|
| LY | Libya |
| MA | Fas|
| MD | Moldova Cumhuriyeti|
| MG | Madagaskar|
| MK | Kuzey Makedonya|
| ML | Mali|
| MM | Myanmar|
| MN | Moğolistan|
| MO | Makao ÖİB|
| MQ | Martinik|
| MR | Moritanya|
| MT | Malta|
| MV | Maldivler|
| MW | Malavi|
| MX | Meksika|
| MY | Malezya|
| MZ | Mozambik|
| NA | Namibya|
| NE | Nijer|
| NG | Nijerya|
| NI | Nikaragua|
| NL | Hollanda|
| NO | Norveç|
| NP | Nepal|
| NR | Nauru|
| NZ | Yeni Zelanda|
| OM | Umman|
| PA | Panama|
| PE | Peru|
| PH | Filipinler|
| PK | Pakistan|
| PL | Polonya|
| PR | Porto Riko|
| PT | Portekiz|
| PW | Palau|
| PY | Paraguay|
| QA | Katar|
| RE | Reunion|
| RO | Romanya|
| RS | Sırbistan|
| RU | Rusya Federasyonu|
| RW | Rwanda|
| SA | Suudi Arabistan|
| SD | Sudan|
| SE | İsveç|
| SG | Singapur|
| SI | Slovenya|
| SK | Slovakya|
| SN | Senegal|
| SO | Somali|
| SR | Surinam|
| SS | Güney Sedan|
| SV | El Salvador|
| SY | Suriye Arap Cumhuriyeti|
| SZ | Svaziland|
| TC | Turks ve Caicos Adaları|
| TG | Togo|
| TH | Tayland|
| TN | Tunus|
| TR | Türkiye|
| TT | Trinidad ve Tobago|
| TW | Tayvan|
| TZ | Tanzanya Birleşik Cumhuriyeti|
| UA | Ukrayna|
| UG | Uganda|
| ABD | Amerika Birleşik Devletleri|
| UY | Uruguay|
| UZ | Özbekistan|
| VC | Saint Vincent ve Grenadinler|
| VE | Venezuela|
| VG | Virgin Adaları, İngiliz|
| VI | Virgin Adaları, ABD|
| VN | Vietnam|
| ZA | Güney Afrika|
| ZM | Zambiya|
| ZW | Zimbabve|

## <a name="next-steps"></a>Sonraki adımlar

- [Front Door ile uygulama katmanı güvenliği](front-door-application-security.md) hakkında bilgi edinin.
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
