---
title: Azure ön kapı hizmeti için bir etki alanında coğrafi filtreleme
description: Bu makalede Azure Front Door hizmeti için coğrafi filtreleme ilkesi hakkında bilgi edineceksiniz
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.topic: conceptual
ms.date: 03/10/2020
ms.author: victorh
ms.reviewer: tyao
ms.openlocfilehash: fcd7a0fe60639bbb17661a906d15136996b325e4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87005455"
---
# <a name="what-is-geo-filtering-on-a-domain-for-azure-front-door-service"></a>Azure ön kapı hizmeti için bir etki alanında coğrafi filtreleme nedir?

Azure Front Door Hizmeti, varsayılan olarak kullanıcı isteklerine, isteğin geldiği konumdan bağımsız olarak yanıt verir. Ancak bazı durumlarda, Web uygulamalarınıza olan erişimi ülkeye/bölgeye göre kısıtlamak isteyebilirsiniz. Ön kapıda Web uygulaması güvenlik duvarı (WAF) hizmeti, belirli ülkelerde/bölgelerde erişime izin vermek veya erişimi engellemek için uç noktanıza özel erişim kuralları kullanarak bir ilke tanımlamanızı sağlar. 

WAF ilkesi genellikle bir dizi özel kural içerir. Bir kural eşleşme koşullarından, bir eylemden ve bir öncelikten oluşur. Eşleşme koşulunda bir eşleşme değişkeni, işleç ve eşleşme değeri tanımlarsınız.  Coğrafi filtreleme kuralı için, eşleşen değişken REMOTE_ADDR, işleç GeoMatch, değer ilgilendiğiniz iki harfli ülke/bölge kodudur. Yola göre bir coğrafi filtreleme kuralı oluşturmak için bir GeoMatch koşulunu ve bir REQUEST_URI dizesi eşleşme koşulunu birleştirebilirsiniz.

[Azure PowerShell](waf-front-door-tutorial-geo-filtering.md) kullanarak veya [hızlı başlangıç şablonumuzu](https://github.com/Azure/azure-quickstart-templates/tree/master/101-front-door-geo-filtering)kullanarak, ön kapıya yönelik bir coğrafi filtreleme ilkesi yapılandırabilirsiniz.

## <a name="countryregion-code-reference"></a>Ülke/bölge kodu başvurusu

|Ülke/bölge kodu | Ülke? Bölge adı |
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
| BA | Bosna-Hersek|
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
| BZ | Belize|
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
| GF | Fransız Guyanası|
| GH | Gana|
| GN | Gine|
| GP | Guadeloupe|
| GR | Yunanistan|
| GT | Guatemala|
| GY | Guyana|
| HK | Hong Kong ÖİB|
| HN | Honduras|
| HR | Hırvatistan|
| HT | Haiti|
| HU | Macaristan|
| ID | Endonezya|
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
| KY | Cayman Adaları|
| KZ | Kazakistan|
| LA | Laos Demokratik Halk Cumhuriyeti|
| LB | Lübnan|
| LI | Liechtenstein|
| LK | Sri Lanka|
| LR | Liberya|
| LS | Lesotho|
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
| MQ | Martinique|
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
| RW | Ruanda|
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
| ABD | Birleşik Devletler|
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

- [Front Door ile uygulama katmanı güvenliği](../../frontdoor/front-door-application-security.md) hakkında bilgi edinin.
- [Front Door oluşturmayı](../../frontdoor/quickstart-create-front-door.md) öğrenin.
