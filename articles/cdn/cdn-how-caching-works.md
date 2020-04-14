---
title: Önbelleğe alma nasıl çalışır | Microsoft Dokümanlar
description: Önbelleğe alma, veri isteklerine daha hızlı erişilebilmek için verileri yerel olarak depolama işlemidir.
services: cdn
documentationcenter: ''
author: asudbring
manager: danielgi
editor: ''
ms.assetid: ''
ms.service: azure-cdn
ms.workload: tbd
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/30/2018
ms.author: allensu
ms.openlocfilehash: d0c438aee7f56e96feb7167fad718fd9519a9f76
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253722"
---
# <a name="how-caching-works"></a>Önbelleğe alma nasıl işler?

Bu makalede, genel önbelleğe alma kavramlarına ve [Azure İçerik Teslim Ağı'nın (CDN)](cdn-overview.md) performansı artırmak için önbelleğe nasıl kullandığına genel bir bakış sağlanmaktadır. CDN bitiş noktanızda önbelleğe alma davranışını nasıl özelleştirdiğinizi öğrenmek istiyorsanız, [önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını denetle](cdn-caching-rules.md) ve [sorgu dizeleri ile Azure CDN önbelleğe alma davranışını denetleyin'](cdn-query-string.md)e bakın.

## <a name="introduction-to-caching"></a>Önbelleğe giriş

Önbelleğe alma, veri isteklerine daha hızlı erişilebilmek için verileri yerel olarak depolama işlemidir. Önbelleğe alma nın en yaygın türünde, web tarayıcısı önbelleğe alma, bir web tarayıcısı statik verilerin kopyalarını yerel bir sabit diskte yerel olarak depolar. Önbelleğe alma kullanarak, web tarayıcısı sunucuya birden fazla gidiş-dönüş yapmaktan kaçınabilir ve bunun yerine aynı verilere yerel olarak erişebilir, böylece zamandan ve kaynaklardan tasarruf edebilir. Önbelleğe alma, statik görüntüler, CSS dosyaları ve JavaScript dosyaları gibi küçük, statik verileri yerel olarak yönetmek için uygundur.

Benzer şekilde, önbelleğe alma, kullanıcıya yakın kenar sunucularında bir içerik dağıtım ağı tarafından, menşeine geri seyahat eden istekleri önlemek ve son kullanıcı gecikmesini azaltmak için kullanılır. Yalnızca tek bir kullanıcı için kullanılan bir web tarayıcısı önbelleğinin aksine, CDN'nin paylaşılan bir önbelleği vardır. CDN paylaşılan önbelleğinde, bir kullanıcı tarafından istenen bir dosyaya daha sonra diğer kullanıcılar erişebilir ve bu da kaynak sunucuya gelen istek sayısını büyük ölçüde azaltır.

Sık sık değişen veya tek bir kullanıcıya özgü dinamik kaynaklar önbelleğe alınamaz. Ancak bu tür kaynaklar, performans iyileştirmeleri için Azure İçerik Dağıtım Ağı'ndaki dinamik site hızlandırma (DSA) optimizasyonundan yararlanabilir.

Önbelleğe alma, kaynak sunucusu ile son kullanıcı arasında birden çok düzeyde oluşabilir:

- Web sunucusu: Paylaşılan önbelleği kullanır (birden çok kullanıcı için).
- İçerik dağıtım ağı: Paylaşılan önbellek kullanır (birden çok kullanıcı için).
- Internet servis sağlayıcısı (ISS): Paylaşılan önbelleği (birden çok kullanıcı için) kullanır.
- Web tarayıcısı: Özel bir önbellek kullanır (bir kullanıcı için).

Her önbellek genellikle kendi kaynak tazeliğini yönetir ve bir dosya eskidiğinde doğrulama gerçekleştirir. Bu davranış HTTP önbelleğe alma belirtimi, [RFC 7234](https://tools.ietf.org/html/rfc7234)tanımlanır.

### <a name="resource-freshness"></a>Kaynak tazeliği

Önbelleğe alınmış bir kaynak eski veya eski olabileceğinden (kaynak sunucusundaki ilgili kaynakla karşılaştırıldığında), içeriğin yenilendiğinde önbelleğe alma mekanizmasının denetlemesi önemlidir. Zaman ve bant genişliği tüketiminden tasarruf etmek için önbelleğe alınmış bir kaynak, her erişigeldiğinde başlangıç sunucusundaki sürümle karşılaştırılmaz. Bunun yerine, önbelleğe alınmış bir kaynağın taze olduğu kabul edildiği sürece, en güncel sürüm olduğu varsayılır ve doğrudan istemciye gönderilir. Önbelleğe alınmış bir kaynağın yaşı önbellek ayarı tarafından tanımlanan yaştan veya dönemden küçükse taze olarak kabul edilir. Örneğin, bir tarayıcı bir web sayfasını yeniden yüklediğinde, önbelleğe alınmış her kaynağın yeni olduğunu doğrular ve bu sayfayı yükler. Kaynak taze (eski) değilse, sunucudan güncel bir kopya yüklenir.

### <a name="validation"></a>Doğrulama

Bir kaynağın eski olduğu düşünülürse, kaynak sunucusundan kaynağın doğrulanması, yani önbellekteki verilerin kaynak sunucusundakilerle eşleşip eşleşmediğini belirlemesi istenir. Dosya kaynak sunucusunda değiştirildiyse, önbellek kaynağın sürümünü güncelleştirir. Aksi takdirde, kaynak yeniyse, veriler önce doğrulamadan doğrudan önbellekten teslim edilir.

### <a name="cdn-caching"></a>CDN önbelleğe alma

Önbelleğe alma, bir CDN'nin teslimatı hızlandırma ve görüntüler, yazı tipleri ve videolar gibi statik varlıkların kaynak yükünü azaltmak için çalışma şeklinin ayrılmaz bir parçasıdır. CDN önbelleğe almada, statik kaynaklar seçici olarak kullanıcıya daha yerel olan stratejik olarak yerleştirilmiş sunucularda depolanır ve aşağıdaki avantajları sunar:

- Çoğu web trafiği statik olduğundan (örneğin, görüntüler, yazı tipleri ve videolar), CDN önbelleğe alma, içeriği kullanıcıya yaklaştırarak ağ gecikmesini azaltır ve böylece verilerin kat ettiği mesafeyi azaltır.

- Çalışmayı CDN'ye boşaltarak, önbelleğe alma ağ trafiğini ve kaynak sunucusundaki yükü azaltabilir. Bunu yapmak, çok sayıda kullanıcı olsa bile uygulama için maliyet ve kaynak gereksinimlerini azaltır.

Önbelleğe almanın bir web tarayıcısında nasıl uygulandığına benzer şekilde, önbellek yönergeleri üstbilgilerini göndererek bir CDN'de önbelleğe almanın nasıl gerçekleştirileceğini denetleyebilirsiniz. Önbellek yönergesi üstbilgisi, genellikle başlangıç sunucusu tarafından eklenen HTTP üstbilgidir. Bu üstbilgileri çoğu başlangıçta istemci tarayıcılarda önbelleğe alma adresine tasarlanmış olsa da, artık CDN'ler gibi tüm ara önbellekler tarafından da kullanılmaktadır. 

Önbellek tazeliğini tanımlamak için iki üstbilgi kullanılabilir: `Cache-Control` ve `Expires`. `Cache-Control`daha günceldir ve her `Expires`ikisi de varsa üzerinde önceliklidir. Doğrulama için kullanılan iki tür üstbilgi de vardır `ETag` (doğrulayıcı olarak adlandırılır): ve. `Last-Modified` `ETag`daha günceldir ve her `Last-Modified`ikisi de tanımlanmışsa, üzerinde önceliklidir.  

## <a name="cache-directive-headers"></a>Önbellek yönergesi üstbilgi

> [!IMPORTANT]
> Varsayılan olarak, DSA için en iyi duruma getirilmiş bir Azure CDN bitiş noktası önbellek yönergeleri üstbilgilerini yok sayar ve önbelleğe alma atlar. **Akamai profillerinden** Verizon ve Azure CDN **Standard'dan Azure CDN Standardı** için, önbelleğe alma sağlamak için [CDN önbelleğe alma kurallarını](cdn-caching-rules.md) kullanarak Azure CDN uç noktasının bu üstbilgilerin nasıl davrandığını ayarlayabilirsiniz. **Yalnızca Verizon profillerinden Azure CDN Premium** için önbelleğe alma sağlamak için kurallar [altyapısını](cdn-rules-engine.md) kullanırsınız.

Azure CDN, önbellek süresini ve önbellek paylaşımını tanımlayan aşağıdaki HTTP önbellek yönergesi üstbilgisini destekler.

**Önbellek Denetimi:**
- Web yayıncılarına içerikleri üzerinde daha fazla denetim sağlamak ve `Expires` üstbilginin sınırlamalarını gidermek için HTTP 1.1'de tanıtıldı.
- `Expires` Üstbilgi, hem o hem `Cache-Control` de tanımlanmış olarak geçersiz kılar.
- İstemciden CDN POP'a http isteğinde kullanıldığında, `Cache-Control` varsayılan olarak tüm Azure CDN profilleri tarafından yoksayılır.
- İstemciden CDN POP'a http yanıtında kullanıldığında:
     - Microsoft'tan Verizon ve **Azure CDN Standard'dan** `Cache-Control` **Azure CDN Standard/Premium** tüm yönergeleri destekler.
     - **Akamai'den Azure CDN** Standardı `Cache-Control` yalnızca aşağıdaki yönergeleri destekler; diğerleri göz ardı edilir:
         - `max-age`: Önbellek içeriği belirtilen saniye sayısı için depolayabilir. Örneğin, `Cache-Control: max-age=5`. Bu yönerge, içeriğin taze olarak kabul edilen maksimum süreyi belirtir.
         - `no-cache`: İçeriği önbelleğe getirin, ancak önbellekten teslim etmeden önce içeriği her seferinde doğrulayın. `Cache-Control: max-age=0`Eşdeğeri .
         - `no-store`: İçeriği asla önbelleğe alıp yok edin. Daha önce depolanmışsa içeriği kaldırın.

**Sona eri -yor:**
- HTTP 1.0'da tanıtılan eski başlık; geriye dönük uyumluluk için desteklenir.
- İkinci hassasiyetle tarih tabanlı bir son kullanma süresini kullanır. 
- Benzer `Cache-Control: max-age`.
- Yok `Cache-Control` olmadığında kullanılır.

**Pragma:**
   - Varsayılan olarak Azure CDN tarafından onurlandırılmamak.
   - HTTP 1.0'da tanıtılan eski başlık; geriye dönük uyumluluk için desteklenir.
   - Aşağıdaki yönerge ile istemci istek üstbilgiolarak kullanılır: `no-cache`. Bu yönerge, sunucuya kaynağın yeni bir sürümünü sunmasını bildirir.
   - `Pragma: no-cache`'ye `Cache-Control: no-cache`eşdeğerdir.

## <a name="validators"></a>Doğrulayıcıları

Önbellek eskidiğinde, http önbellek doğrulayıcıları bir dosyanın önbelleğe alınmış sürümünü başlangıç sunucusundaki sürümle karşılaştırmak için kullanılır. **Verizon'daki Azure CDN Standard/Premium** varsayılan olarak hem `ETag` de `Last-Modified` doğrulayıcıları desteklerken, **Microsoft'tan Azure CDN Standard** ve **Akamai'den Azure CDN Standard** yalnızca `Last-Modified` varsayılan olarak destekler.

**Etag:**
- **Verizon'dan Azure CDN Standard/Premium** varsayılan olarak desteklerken, `ETag` **Microsoft'tan Azure CDN Standard** ve **Akamai'den Azure CDN Standard** desteklemiyor.
- `ETag`her dosya ve bir dosya sürümü için benzersiz bir dize tanımlar. Örneğin, `ETag: "17f0ddd99ed5bbe4edffdd6496d7131f"`.
- HTTP 1.1'de tanıtılan ve `Last-Modified`.'den daha günceldir. Son değiştirilen tarihi belirlemek zor olduğunda yararlıdır.
- Hem güçlü doğrulamayı hem de zayıf doğrulamayı destekler; ancak Azure CDN yalnızca güçlü doğrulamayı destekler. Güçlü doğrulama için, iki kaynak gösterimi bayt için-byte aynı olmalıdır. 
- Önbellek, istekte bir `ETag` veya daha `If-None-Match` fazla `ETag` doğrulayıcıiçeren bir üstbilgi göndererek kullanan bir dosyayı doğrular. Örneğin, `If-None-Match: "17f0ddd99ed5bbe4edffdd6496d7131f"`. Sunucunun sürümü listedeki `ETag` bir doğrulayıcıyla eşleşirse, yanıtında durum kodu 304 (Değiştirilmemiş) gönderir. Sürüm farklıysa, sunucu durum kodu 200 (Tamam) ve güncelleştirilmiş kaynakla yanıt verir.

**Son Değiştirilen:**
- Yalnızca **Verizon'dan Gelen Azure CDN Standard/Premium** için, `Last-Modified` HTTP yanıtının bir parçası değilse kullanılır. `ETag` 
- Kaynak sunucusunun kaynağın en son değiştirildiğini belirlediği tarih ve saati belirtir. Örneğin, `Last-Modified: Thu, 19 Oct 2017 09:28:00 GMT`.
- Önbellek, istekte tarih `Last-Modified` ve `If-Modified-Since` saat içeren bir üstbilgi göndererek bir dosyayı kullanır. Kaynak sunucusu bu tarihi en `Last-Modified` son kaynağın üstbilgisiyle karşılaştırır. Kaynak belirtilen zamandan beri değiştirilmemişse, sunucu yanıtında durum kodu 304 (Değiştirilmemiş) döndürür. Kaynak değiştirildiyse, sunucu durum kodu 200 (Tamam) ve güncelleştirilmiş kaynağı döndürür.

## <a name="determining-which-files-can-be-cached"></a>Önbelleğe alınan dosyaların belirlenmesi

Tüm kaynaklar önbelleğe alınamaz. Aşağıdaki tablo, HTTP yanıtı türüne bağlı olarak hangi kaynakların önbelleğe alınabileceğini gösterir. Bu koşulların tümünün karşılanmıyor HTTP yanıtları ile teslim edilen kaynaklar önbelleğe alınamaz. Yalnızca **Verizon'daki Azure CDN Premium** için, bu koşullardan bazılarını özelleştirmek için kurallar altyapısını kullanabilirsiniz.

|                   | Microsoft'tan Azure CDN          | Verizon'dan Azure CDN | Akamai'den Azure CDN        |
|-------------------|-----------------------------------|------------------------|------------------------------|
| HTTP durum kodu | 200, 203, 206, 300, 301, 410, 416 | 200                    | 200, 203, 300, 301, 302, 401 |
| HTTP yöntemleri      | AL, BAŞ                         | GET                    | GET                          |
| Dosya boyutu sınırları  | 300 GB                            | 300 GB                 | - Genel web teslimat optimizasyonu: 1.8 GB<br />- Medya akışı optimizasyonları: 1,8 GB<br />- Büyük dosya optimizasyonu: 150 GB |

**Microsoft'un** bir kaynak üzerinde çalışmak için önbelleğe aldığı Azure CDN Standardı için, kaynak sunucusunun herhangi bir HEAD ve GET HTTP isteklerini desteklemesi gerekir ve içerik uzunluğu değerleri herhangi bir HEAD ve GET HTTP yanıtları için aynı olmalıdır. HEAD isteği için, kaynak sunucusuNUN HEAD isteğini desteklemesi ve GET isteği almış gibi aynı üstbilgiyle yanıt vermesi gerekir.

## <a name="default-caching-behavior"></a>Varsayılan önbelleğe alma davranışı

Aşağıdaki tabloda Azure CDN ürünleri için varsayılan önbelleğe alma davranışı ve bunların optimizasyonları açıklanmaktadır.

|    | Microsoft: Genel web dağıtımı | Verizon: Genel web teslimat | Verizon: DSA | Akamai: Genel web teslimatı | Akamai: DSA | Akamai: Büyük dosya indirme | Akamai: genel veya VOD medya akışı |
|------------------------|--------|-------|------|--------|------|-------|--------|
| **Onur kökeni**       | Evet    | Evet   | Hayır   | Evet    | Hayır   | Evet   | Evet    |
| **CDN önbellek süresi** | 2 gün |7 gün | None | 7 gün | None | 1 gün | 1 yıl |

**Onur kaynağı**: Desteklenen önbellek yönergesi üstbilgilerini, kaynak sunucusundan HTTP yanıtında varsa onurlandırıp onurlandırmayacaklarını belirtir.

**CDN önbellek süresi**: Azure CDN'de kaynağın önbelleğe aldığı süreyi belirtir. Ancak, **Onur kaynağı** Evet ise ve kaynak sunucudan gelen HTTP yanıtı `Expires` `Cache-Control: max-age`önbellek yönergesi üstbilgisini içeriyorsa veya Azure CDN üstbilgitarafından belirtilen süre değerini kullanır. 

## <a name="next-steps"></a>Sonraki adımlar

- Önbelleğe alma kuralları yla CDN'deki varsayılan önbelleğe alma davranışını nasıl özelleştirip geçersiz kakmayı öğrenmek için [önbelleğe alma kurallarıyla Azure CDN önbelleğe alma davranışını](cdn-caching-rules.md)denetle'ye bakın. 
- Önbelleğe alma davranışını denetlemek için sorgu dizelerini nasıl kullanacağınızı öğrenmek için sorgu [dizeleriyle Azure CDN önbelleğe alma davranışını](cdn-query-string.md)denetle'ye bakın.



