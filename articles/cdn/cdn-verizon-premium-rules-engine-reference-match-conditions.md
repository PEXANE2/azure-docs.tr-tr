---
title: Verizon Premium kural altyapısından eşleşme koşullarına Azure CDN
description: Verizon Premium kuralları altyapısı eşleştirme koşullarından Azure Content Delivery Network için başvuru belgeleri.
services: cdn
author: asudbring
ms.service: azure-cdn
ms.topic: article
ms.date: 05/26/2020
ms.author: allensu
ms.openlocfilehash: efd6e6a93cd4ca79e6c4b6de69f8514e2d71b252
ms.sourcegitcommit: 58ff2addf1ffa32d529ee9661bbef8fbae3cddec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/03/2020
ms.locfileid: "84323323"
---
# <a name="azure-cdn-from-verizon-premium-rules-engine-match-conditions"></a>Verizon Premium kural altyapısından eşleşme koşullarına Azure CDN

Bu makalede, Verizon Premium [kural altyapısından](cdn-verizon-premium-rules-engine.md)Azure Content DELIVERY Network (CDN) için kullanılabilen eşleşme koşullarının ayrıntılı açıklamaları listelenmektedir.

Bir kuralın ikinci bölümü eşleşme durumudur. Bir eşleşme koşulu, bir özellik kümesinin gerçekleştirileceği belirli istek türlerini tanımlar.

Örneğin, bir eşleşme koşulunu şu şekilde kullanabilirsiniz:

- Belirli bir konumdaki içerik isteklerini filtreleyin.
- Belirli bir IP adresi veya ülke/bölge tarafından oluşturulan filtre istekleri.
- İstekleri üst bilgi bilgisine göre filtreleyin.

## <a name="match-conditions"></a><a name="top"></a>Koşulları Eşleştir

* [Her zaman](#always)
* [Cihaz](#device)
* [Konum](#location)
* [Kaynak](#origin)
* [İstek](#request)
* [URL](#url)

### <a name="always"></a><a name="always"></a>Her zaman

[Her zaman eşleşme koşulu,](https://docs.vdms.com/cdn/Content/HRE/M/Always.htm) tüm isteklere varsayılan bir özellik kümesi uygulamak için tasarlanmıştır.

### <a name="device"></a><a name="device"></a>Cihaz

Bu eşleşme koşulları, istemcinin kullanıcı aracısına göre istekleri tanımlamak üzere tasarlanmıştır.

| Name       | Amaç                                                           |
|------------|-------------------------------------------------------------------|
| Marka adı | , Cihazın Marka adının bir ile eşleşip eşleşmediğini tanımlar: <br> **-** Belirli değer ([marka adı sabit](https://docs.vdms.com/cdn/Content/HRE/M/D-Brand-Name-Literal.htm)değeri) <br> **-** Normal ifade ([marka adı Regex](https://docs.vdms.com/cdn/Content/HRE/M/D-Brand-Name-Regex.htm)) <br> **-** Belirli bir model ([marka adı joker karakter](https://docs.vdms.com/cdn/Content/HRE/M/D-Brand-Name-Wildcard.htm)) |
| Cihaz işletim sistemi | Cihaz işletim sisteminin bir ile eşleşip eşleşmediğini belirten istekleri tanımlar: <br> **-** Belirli değer ([cihaz işletim sistemi sabit](https://docs.vdms.com/cdn/Content/HRE/M/D-Device-OS-Literal.htm)değeri) <br> **-** Normal ifade ([cihaz işletim sistemi Regex](https://docs.vdms.com/cdn/Content/HRE/M/D-Device-OS-Regex.htm)) <br> **-** Belirli bir model ([cihaz işletim sistemi joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/D-Device-OS-Wildcard.htm)) |
| Cihaz İşletim Sistemi Sürümü | Cihazın işletim sistemi sürümü ile eşleşen istekleri tanımlar: <br> **-** Belirli değer ([cihaz işletim sistemi sürümü sabit](https://docs.vdms.com/cdn/Content/HRE/M/D-Device-OS-Version-Literal.htm)değeri) <br> **-** Normal ifade ([cihaz işletim sistemi sürümü Regex](https://docs.vdms.com/cdn/Content/HRE/M/D-Device-OS-Version-Regex.htm)) <br> **-** Belirli bir model ([cihaz işletim sistemi sürümü joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/D-Device-OS-Version-Wildcard.htm)) |
| [Çift yönlü mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Dual-Orientation.htm) | Cihazın çift yönlü yönlendirmeyi destekleyip desteklemediğini tanımlar. |
| HTML tercih edilen DTD | Cihazın HTML tercih edilen DTD 'nin bir ile eşleşip eşleşmediğini tanımlar: <br> **-** Belirli değer ([HTML tercih EDILEN DTD sabit değeri](https://docs.vdms.com/cdn/Content/HRE/M/D-HTML-Preferred-DTD-Literal.htm)) <br> **-** Normal ifade ([HTML tercih EDILEN DTD Regex](https://docs.vdms.com/cdn/Content/HRE/M/D-HTML-Preferred-DTD-Regex.htm)) <br> **-** Belirli bir model ([HTML tercih EDILEN DTD joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/D-HTML-Preferred-DTD-Wildcard.htm)) |
| [Görüntü satır içi mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Image-Inlining.htm) | Cihazın Base64 kodlamalı görüntüleri destekleyip desteklemediğini belirtir. |
| [Android mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Android.htm) | Cihazların Android işletim sistemi kullanıp kullanmadığını belirtir. |
| [Uygulama mı?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-App.htm) | Yerel bir uygulamanın içerik isteyip istemediğini belirtir. |
| [Tam masaüstü mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Full-Desktop.htm) | Cihazın tam masaüstü deneyimi sunarak istekleri tanımlar. |
| [İOS mu?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-iOS.htm) | İstekleri cihazın iOS kullanıp kullanmadığını tanımlar. |
| [Robot mı?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Robot.htm) | Cihazların otomatik HTTP istemcisi (ör. bir robot gezgin) olarak kabul edilip edilmeyeceğini tanımlar. |
| [Akıllı TV mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Smart-TV.htm) | , Cihazın akıllı bir TV olup olmadığı istekleri tanımlar. |
| [Smartphone mı?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Smartphone.htm) | Cihazın bir akıllı telefon olup olmadığı istekleri tanımlar.
| [Tablet mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Tablet.htm) | , Cihazın bir tablet olup olmadığı istekleri tanımlar. |
| [Dokunmatik ekran mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Touchscreen.htm) | Cihazın birincil işaret cihazının bir dokunmatik ekran olup olmadığına göre istekleri tanımlar. |
| [Windows Phone?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Windows-Phone.htm) | İstekleri, cihazın bir Windows Mobile 6.5/Windows Phone 7 veya üzeri olduğunu tanımlar. |
| [Kablosuz cihaz mi?](https://docs.vdms.com/cdn/Content/HRE/M/D-Is-Wireless-Device.htm) | , Cihazın kablosuz olup olmadığını gösterir. 
| Pazarlama adı | Cihazın pazarlama adının bir ile eşleşip eşleşmediğini tanımlar: <br> **-** Belirli değer ([Pazarlama adı sabit](https://docs.vdms.com/cdn/Content/HRE/M/D-Marketing-Name-Literal.htm)değeri) <br> **-** Normal ifade ([Pazarlama adı Regex](https://docs.vdms.com/cdn/Content/HRE/M/D-Marketing-Name-Regex.htm)) <br> **-** Belirli bir model ([Pazarlama adı joker karakter](https://docs.vdms.com/cdn/Content/HRE/M/D-Marketing-Name-Wildcard.htm)) |
| Mobil tarayıcı | Cihaz tarayıcısının bir ile eşleşip eşleşmediğini karşılaştırarak istekleri tanımlar: <br> **-** Belirli değer ([mobil tarayıcı sabit değeri](https://docs.vdms.com/cdn/Content/HRE/M/D-Mobile-Browser-Literal.htm)) <br> **-** Normal ifade ([mobil tarayıcı Regex](https://docs.vdms.com/cdn/Content/HRE/M/D-Mobile-Browser-Regex.htm)) <br> **-** Belirli bir model ([mobil tarayıcı joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/D-Mobile-Browser-Wildcard.htm)) |
| Mobil tarayıcı sürümü | Cihazın tarayıcı sürümünün bir ile eşleşip eşleşmediğini belirten istekleri tanımlar: <br> **-** Belirli değer ([mobil tarayıcı sürümü sabit](https://docs.vdms.com/cdn/Content/HRE/M/D-Mobile-Browser-Version-Literal.htm)değeri) <br> **-** Normal ifade ([mobil tarayıcı sürümü Regex](https://docs.vdms.com/cdn/Content/HRE/M/D-Mobile-Browser-Version-Regex.htm)) <br> **-** Belirli bir model ([mobil tarayıcı sürümü joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/D-Mobile-Browser-Version-Wildcard.htm)) |
| Model adı | , Cihazın model adının bir ile eşleşip eşleşmediğini tanımlar: <br> **-** Belirli değer ([model adı sabit](https://docs.vdms.com/cdn/Content/HRE/M/D-Model-Name-Literal.htm)değeri) <br> **-** Normal ifade ([model adı](https://docs.vdms.com/cdn/Content/HRE/M/D-Model-Name-Regex.htm)normal ifadesi) <br> **-** Belirli bir[model (model adı joker karakter](https://docs.vdms.com/cdn/Content/HRE/M/D-Model-Name-Wildcard.htm)) |
| [Aşamalı Indirme?](https://docs.vdms.com/cdn/Content/HRE/M/D-Progressive-Download.htm) | Cihazın aşamalı indirmeyi destekleyip desteklemediğini belirtir. |
| Yayın Tarihi | , Cihazın yayın tarihinin bir ile eşleşip eşleşmediğini belirtir: <br> **-** Belirli değer ([Yayın tarihi değişmez](https://docs.vdms.com/cdn/Content/HRE/M/D-Release-Date-Literal.htm)değeri) <br> **-** Normal ifade ([Yayın tarihi normal](https://docs.vdms.com/cdn/Content/HRE/M/D-Release-Date-Regex.htm)ifadesi) <br> **-** Belirli bir model ([Yayın tarihi joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/D-Release-Date-Wildcard.htm)) |
| [Çözünürlük yüksekliği](https://docs.vdms.com/cdn/Content/HRE/M/D-Resolution-Height.htm) | İstekleri cihazın yüksekliğine göre tanımlar. |
| [Çözünürlük genişliği](https://docs.vdms.com/cdn/Content/HRE/M/D-Resolution-Width.htm) | İstekleri cihazın genişliğine göre tanımlar. |

**[Başa dön](#top)**

### <a name="location"></a><a name="location"></a>Konum

Bu eşleşme koşulları, istek sahibinin konumuna göre istekleri tanımlamak üzere tasarlanmıştır.

| Name       | Amaç                                                           |
|------------|-------------------------------------------------------------------|
| [AS numarası](https://docs.vdms.com/cdn/Content/HRE/M/AS-Number.htm) | Belirli bir ağdan kaynaklanan istekleri tanımlar. |
| Şehir adı | , Adı ile eşleşen bir şehirden kaynaklanıp gelmediğine göre istekleri tanımlar: <br> **-** Belirli değer ([şehir adı sabit](https://docs.vdms.com/cdn/Content/HRE/M/City-Name-Literal.htm)değeri) <br> **-** Normal ifade ([şehir adı normal](https://docs.vdms.com/cdn/Content/HRE/M/City-Name-Regex.htm)ifadesi) |
| [Kıta](https://docs.vdms.com/cdn/Content/HRE/M/Continent.htm) | Belirtilen kıtalardan kaynaklanan istekleri tanımlar. |
| [Ülke](https://docs.vdms.com/cdn/Content/HRE/M/Country.htm) | Belirtilen ülkelerden kaynaklanan istekleri tanımlar. |
| [DMA kodu](https://docs.vdms.com/cdn/Content/HRE/M/DMA-Code.htm) | Belirtilen Metros (belirtilen Pazar alanlarından) kaynaklı istekleri tanımlar. |
| [Enlem](https://docs.vdms.com/cdn/Content/HRE/M/Latitude.htm) | Belirtilen latimsdes 'ten kaynaklanan istekleri tanımlar. |
| [Boylam](https://docs.vdms.com/cdn/Content/HRE/M/Longitude.htm) | Belirtilen Longitudes kaynaklı istekleri tanımlar. |
| [Metro kodu](https://docs.vdms.com/cdn/Content/HRE/M/Metro-Code.htm) | Belirtilen Metros (belirtilen Pazar alanlarından) kaynaklı istekleri tanımlar. |
| [Posta kodu](https://docs.vdms.com/cdn/Content/HRE/M/Postal-Code.htm) | Belirtilen posta kodlarından kaynaklanan istekleri tanımlar. |
| [Bölge kodu](https://docs.vdms.com/cdn/Content/HRE/M/Region-Code.htm) | Belirtilen bölgelerden kaynaklanan istekleri tanımlar. |

> [!NOTE]
> **Metro kodu veya DMA kodu kullanmalıdır mi?** <br>
Bu eşleşme koşullarının her ikisi de aynı özelliği sağlar. Ancak, istekleri DMA 'ya göre belirlemek için Metro kod eşleşme koşulunun kullanılmasını öneririz.

**[Başa dön](#top)**

### <a name="origin"></a><a name="origin"></a>Kaynak

Bu eşleşme koşulları, CDN depolama veya müşteri kaynak sunucusunu işaret eden istekleri belirlemek için tasarlanmıştır.

| Name       | Amaç                                                           |
|------------|-------------------------------------------------------------------|
| [CDN kaynağı](https://docs.vdms.com/cdn/Content/HRE/M/CDN-Origin.htm) | CDN depolamada depolanan içerik isteklerini tanımlar. |
| [Müşteri kaynağı](https://docs.vdms.com/cdn/Content/HRE/M/Customer-Origin.htm) | Belirli bir müşteri kaynağı sunucusunda depolanan içerik isteklerini tanımlar. |

**[Başa dön](#top)**

### <a name="request"></a><a name="request"></a>İstek

Bu eşleşme koşulları, isteklerini özelliklerine göre belirlemek için tasarlanmıştır.

| Name              | Amaç                                                                |
|-------------------|------------------------------------------------------------------------|
| [İstemci IP Adresi](https://docs.vdms.com/cdn/Content/HRE/M/Client-IP-Address.htm) | Belirli bir IP adresinden kaynaklanan istekleri tanımlar. |
| Cookie parametresi  | Bir isteği, ile eşleşen bir tanımlama bilgisi içerip içermediğini tanımlar: <br> **-** Belirli değer ([tanımlama bilgisi parametre sabit değeri](https://docs.vdms.com/cdn/Content/HRE/M/Cookie-Parameter-Literal.htm)) <br> **-** Normal ifade ([tanımlama bilgisi parametresi Regex](https://docs.vdms.com/cdn/Content/HRE/M/Cookie-Parameter-Regex.htm) <br> **-** Belirli bir model ([tanımlama bilgisi parametresi joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/Cookie-Parameter-Wildcard.htm)) |
| [Edge CNAME](https://docs.vdms.com/cdn/Content/HRE/M/Edge-CNAME.htm) | Belirli bir Edge CNAME 'e işaret eden istekleri tanımlar. |
| Başvurulan etki alanı | Bir isteği, ile eşleşen bir ana bilgisayar adı tarafından adlandırıldığına göre tanımlar: <br> **-** Belirli değer ([başvurulan etki alanı sabit](https://docs.vdms.com/cdn/Content/HRE/M/Referring-Domain-Literal.htm)değeri) <br> **-** Belirli bir model ([başvurulan etki alanı joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/Referring-Domain-Wildcard.htm)) |
| İstek Başlığı | Bir isteği, ile eşleşen bir üst bilgi içerip içermediğini tanımlar: <br> **-** Belirli değer ([Istek üst bilgisi sabit](https://docs.vdms.com/cdn/Content/HRE/M/Request-Header-Literal.htm)değeri) <br> **-** Normal ifade ([Istek üstbilgisi Regex](https://docs.vdms.com/cdn/Content/HRE/M/Request-Header-Regex.htm)) <br> **-** Belirli bir model ([Istek üstbilgisi joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/Request-Header-Wildcard.htm)) |
| [Request Yöntemi](https://docs.vdms.com/cdn/Content/HRE/M/Request-Method.htm) | İstekleri HTTP yöntemine göre tanımlar. |
| [İstek düzeni](https://docs.vdms.com/cdn/Content/HRE/M/Request-Scheme.htm) | İstekleri HTTP protokollerine göre tanımlar. |

**[Başa dön](#top)**

### <a name="url"></a><a name="url"></a>URL

| Name              | Amaç                                                                |
|-------------------|------------------------------------------------------------------------|
| URL Yolu | Dosya adı da dahil olmak üzere göreli yolunun bir ile eşleşip eşleşmediğini, istekleri tanımlar: <br> **-** Belirli değer ([URL yolu sabit](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Literal.htm)değeri) <br> **-** Normal ifade ([URL yolu normal](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Regex.htm)ifadesi) <br> **-** Belirli bir model ([URL yolu joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Wildcard.htm)) |
| URL yolu dizini | İstekleri göreli yolunun bir ile eşleşip eşleşmediğini tanımlar: <br> **-** Belirli değer ([URL yolu dizin sabit](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Directory-Literal.htm)değeri) <br> **-** Belirli bir model ([URL yolu dizini joker karakter](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Directory-Wildcard.htm)) |
| URL yolu uzantısı | İstekleri dosya uzantısının ile eşleşip eşleşmediğini tanımlar: <br> **-** Belirli değer ([URL yolu uzantısı sabit](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Extension-Literal.htm)değeri) <br> **-** Belirli bir model ([URL yolu uzantısı joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Extension-Wildcard.htm)) |
| URL yolu dosya adı | Dosya adı ile eşleşen istekleri tanımlar: <br> **-** Belirli değer ([URL yolu dosya adı sabit](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Filename-Literal.htm)değeri) <br> **-** Belirli bir model ([URL yolu dosya adı joker karakter](https://docs.vdms.com/cdn/Content/HRE/M/URL-Path-Filename-Wildcard.htm)) |
| URL sorgusu | Sorgu dizesi ile eşleşen istekleri tanımlar: <br> **-** Belirli değer ([URL sorgu sabit](https://docs.vdms.com/cdn/Content/HRE/M/URL-Query-Literal.htm)değeri) <br> **-** Normal ifade ([URL sorgusu normal](https://docs.vdms.com/cdn/Content/HRE/M/URL-Query-Regex.htm)ifadesi) <br> **-** Belirli bir model ([URL sorgusu joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/URL-Query-Wildcard.htm)) |
| URL sorgu parametresi | İstekleri, bir sorgu dizesi parametresi içerip içermediği bir değere ayarlanmış şekilde tanımlar: <br> **-** Belirli değer ([URL sorgu parametresi sabit](https://docs.vdms.com/cdn/Content/HRE/M/URL-Query-Parameter-Literal.htm)değeri) <br> **-** Belirli bir model ([URL sorgu parametresi joker karakteri](https://docs.vdms.com/cdn/Content/HRE/M/URL-Query-Parameter-Wildcard.htm)) |

**[Başa dön](#top)**

En son eşleşme koşulları için [Verizon Rules Engine belgelerine](https://docs.vdms.com/cdn/index.html#Quick_References/HRE_QR.htm#Conditio)bakın.

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Content Delivery Network genel bakış](cdn-overview.md)
- [Kural altyapısı başvurusu](cdn-verizon-premium-rules-engine-reference.md)
- [Kural altyapısı koşullu ifadeleri](cdn-verizon-premium-rules-engine-reference-conditional-expressions.md)
- [Kural altyapısı özellikleri](cdn-verizon-premium-rules-engine-reference-features.md)
- [Kurallar altyapısını kullanarak varsayılan HTTP davranışını geçersiz kılma](cdn-verizon-premium-rules-engine.md)
