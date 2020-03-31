---
title: Sanal makine teklifleri için fiyatlandırma | Azure Marketi
description: Sanal makine tekliflerinin fiyatlandırmasını belirtmenin üç yöntemini açıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: b96dcfa6a140d5c16208fd8183003a7462b1aa56
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280448"
---
<a name="pricing-for-virtual-machine-offers"></a>Sanal makine tekliflerinin fiyatlandırması
==================================

Sanal makine teklifleri için fiyatlandırma belirlemenin üç yolu vardır: özelleştirilmiş çekirdek fiyatlandırma, çekirdek başına fiyatlandırma ve elektronik tablo fiyatlandırması.


<a name="customized-core-pricing"></a>Özelleştirilmiş çekirdek fiyatlandırma
-----------------------

Fiyatlandırma her bölge ve çekirdek kombinasyonu için özeldir. Satış listesindeki her bölge **sanalMachinePricing**/**bölgesiFiyatları** bölümünde tanım belirtilmelidir.  İsteğinizdeki her [bölge](#regions) için doğru para birimi kodlarını kullanın.  Aşağıdaki örnekte bu gereksinimleri gösteriş verilmiştir:

``` json
    "virtualMachinePricing": 
    {
        ... 
        "coreMultiplier": 
        {
            "currency": "USD",
                "individually": 
                {
                    "sharedcore": 1,
                    "1core": 2,
                    "2core": 2,
                    "4core": 2,
                    "6core": 2,
                    "8core": 2,
                    "10core": 4,
                    "12core": 4,
                    "16core": 4,
                    "20core": 4,
                    "24core": 4,
                    "32core": 6,
                    "36core": 6,
                    "40core": 6,
                    "44core": 6,
                    "48core": 10,
                    "60core": 10,
                    "64core": 10,
                    "72core": 10,
                    "80core": 12,
                    "96core": 12,
                    "120core": 15,
                    "128core": 15,
                    "208core": 20,
                    "416core": 30
                }
        }
        ...
     }
```


<a name="per-core-pricing"></a>Çekirdek başına fiyatlandırma
----------------

Bu durumda, yayıncılar SKU için USD olarak bir fiyat belirtir ler ve diğer tüm fiyatlar otomatik olarak oluşturulur. Çekirdek başına fiyat, istekteki **tek** parametrede belirtilir.

``` json
     "virtualMachinePricing": 
     {
         ...
         "coreMultiplier": 
         {
             "currency": "USD",
             "single": 1.0
         }
     }
```


<a name="spreadsheet-pricing"></a>Elektronik tablo fiyatlandırması
-------------------

Yayımcı ayrıca fiyatlandırma elektronik tablosunu geçici bir depolama konumuna yükleyebilir, ardından uri'yi diğer dosya yapıları gibi isteğe dahil edebilir. Elektronik tablo daha sonra yüklenir, belirtilen fiyat çizelgesini değerlendirmek için tercüme edilir ve son olarak teklifi fiyatlandırma bilgileriyle güncelleştirir. Sonraki TEKLIF için GET istekleri elektronik tablo URI ve bölge için değerlendirilen fiyatlar döndürür.

``` json
     "virtualMachinePricing": 
     {
         ...
         "spreadSheetPricing": 
         {
             "uri": "https://blob.storage.azure.com/<your_spreadsheet_location_here>/prices.xlsx",
         }
     }
```

<a name="new-core-sizes-added-on-722019"></a>7/2/2019 tarihinde eklenen yeni çekirdek boyutları
---------------------------

VM yayıncılarına 2 Temmuz 2019'da yeni Azure sanal makine boyutları için yeni fiyatların eklenmesi (çekirdek sayısına bağlı olarak) bildirilir.  Yeni fiyatlar çekirdek boyutları 10, 44, 48, 60, 120, 208 ve 416 içindir.  Mevcut VM için bu çekirdek boyutları için yeni fiyatlar otomatik olarak cari fiyatlara göre hesaplanmıştır sunuyor.  Yayıncıların ek fiyatları gözden geçirmek ve istenen değişiklikleri yapmak için 1 Ağustos 2019'a kadar zamanları vardır.  Bu tarihten sonra, yayımcı tarafından yeniden yayımlanmazsa, bu yeni çekirdek boyutları için otomatik olarak hesaplanan fiyatlar geçerli olacaktır.


<a name="regions"></a>Bölgeler
-------

Aşağıdaki tablo, özelleştirilmiş çekirdek fiyatlandırma için belirtebileceğiniz farklı bölgeleri ve bunların karşılık gelen para birimi kodlarını gösterir.

| **Bölge** | **Adı**             | **Para birimi kodu** |
|------------|----------------------|-------------------|
| DZ         | Cezayir              | Dzd               |
| AR         | Arjantin            | ARS               |
| AU         | Avustralya            | AUD               |
| AT         | Avusturya              | EUR               |
| BH         | Bahreyn              | Bhd               |
| BY         | Belarus              | RUB               |
| BE         | Belçika              | EUR               |
| BR         | Brezilya               | USD               |
| BG         | Bulgaristan             | Bgn               |
| CA         | Kanada               | CAD               |
| CL         | Şili                | Clp               |
| CO         | Kolombiya             | Polis               |
| CR         | Kosta Rika           | Crc               |
| HR         | Hırvatistan              | Hrk               |
| CY         | Kıbrıs               | EUR               |
| CZ         | Çek Cumhuriyeti       | Czk               |
| DK         | Danimarka              | DKK               |
| DO         | Dominik Cumhuriyeti   | USD               |
| EC         | Ekvador              | USD               |
| EG         | Mısır                | Egp               |
| SV         | El Salvador          | USD               |
| EE         | Estonya              | EUR               |
| FI         | Finlandiya              | EUR               |
| GS         | Fransa               | EUR               |
| DE         | Almanya              | EUR               |
| GR         | Yunanistan               | EUR               |
| GT         | Guatemala            | GTQ               |
| HK         | Hong Kong ÖİB        | HKD               |
| HU         | Macaristan              | Huf               |
| IS         | İzlanda              | ısk               |
| IN         | Hindistan                | INR               |
| Kimlik         | Endonezya            | IDR               |
| IE         | İrlanda              | EUR               |
| IL         | İsrail               | ıls               |
| BT         | İtalya                | EUR               |
| JP         | Japonya                | JPY               |
| JO         | Ürdün               | Jod               |
| KZ         | Kazakistan           | Kzt               |
| KE         | Kenya                | KES               |
| KR         | Güney Kore                | KRW               |
| KW         | Kuveyt               | Kwd               |
| LV         | Letonya               | EUR               |
| LI         | Lihtenştayn        | CHF               |
| LT         | Litvanya            | EUR               |
| LU         | Lüksemburg           | EUR               |
| MK         | Kuzey Makedonya      | Mkd               |
| MY         | Malezya             | MYR               |
| MT         | Malta                | EUR               |
| MX         | Meksika               | MXN               |
| ME         | Karadağ           | EUR               |
| MA         | Fas              | Mad               |
| NL         | Hollanda          | EUR               |
| NZ         | Yeni Zelanda          | NZD               |
| NG         | Nijerya              | Ngn               |
| NO         | Norveç               | NOK               |
| OM         | Umman                 | Özgen               |
| PK         | Pakistan             | Pkr               |
| PA         | Panama               | USD               |
| PY         | Paraguay             | PYG               |
| PE         | Peru                 | Kalem               |
| PH         | Filipinler          | PHP               |
| PL         | Polonya               | Pln               |
| PT         | Portekiz             | EUR               |
| PR         | Porto Riko          | USD               |
| QA         | Katar                | Qar               |
| RO         | Romanya              | Ron               |
| RU         | Rusya               | RUB               |
| SA         | Suudi Arabistan         | SAR               |
| RS         | Sırbistan               | Rsd               |
| SG         | Singapur            | SGD               |
| SK         | Slovakya             | EUR               |
| SI         | Slovenya             | EUR               |
| ZA         | Güney Afrika         | ZAR               |
| ES         | İspanya                | EUR               |
| LK         | Sri Lanka            | USD               |
| SE         | İsveç               | SEK               |
| CH         | İsviçre          | CHF               |
| TW         | Tayvan               | TWD               |
| TH         | Tayland             | Thb               |
| TT         | Trinidad ve Tobago  | Ttd               |
| TN         | Tunus              | Tnd               |
| TR         | Türkiye               | TRY               |
| UA         | Ukrayna              | Uah               |
| AE         | Birleşik Arap Emirlikleri | EUR               |
| GB         | Birleşik Krallık       | GBP               |
| ABD         | Amerika Birleşik Devletleri        | USD               |
| UY         | Uruguay              | UYU               |
| VE         | Venezuela            | USD               |
|  |  |  |
