---
title: Azure Web Uygulama Güvenlik Duvarı (WAF) Geomatch özel kuralları
description: Bu makale, Azure Uygulama Ağ Geçidi'ndeki Web Uygulaması Güvenlik Duvarı (WAF) geomatch özel kurallarına genel bir bakıştır.
services: web-application-firewall
ms.topic: article
author: vhorne
ms.service: web-application-firewall
ms.date: 01/31/2020
ms.author: victorh
ms.openlocfilehash: 6725e1de21dbd103850071f7511e2800c6bd7b69
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76961094"
---
# <a name="geomatch-custom-rules-preview"></a>Geomatch özel kuralları (önizleme)

Özel kurallar, uygulamalarınızın ve güvenlik ilkelerinizin tam gereksinimlerine uygun özel kurallar oluşturmanıza olanak sağlar. Artık web uygulamalarınıza erişimi ülkeye/bölgeye göre kısıtlayabilirsiniz. Tüm özel kurallarda olduğu gibi, bu mantık uygulamanızın gereksinimlerine uygun diğer kurallarla birleştirilebilir.

Coğrafi filtreleme özel kuralı oluşturmak için, Maç Türü olarak *Coğrafi konum'u* seçin ve ardından uygulamanızdan izin vermek/engellemek istediğiniz ülkeyi veya ülkeleri seçin. Daha fazla bilgi [için Powershell'de özel kuralların ve](configure-waf-custom-rules.md) daha fazla özel kural örneğinin (create-custom-waf-rules.md) nasıl oluşturulabildiğini görün.

> [!IMPORTANT]
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Ayrıntılar için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

## <a name="country-codes"></a>Ülke kodları

Geomatch işleci kullanıyorsanız, seçiciler aşağıdaki iki basamaklı ülke kodlarından biri olabilir. 

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
| BL | Aziz Barthélemy|
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

Özel kuralları öğrendikten [sonra, kendi özel kurallarınızı oluşturun.](create-custom-waf-rules.md)
