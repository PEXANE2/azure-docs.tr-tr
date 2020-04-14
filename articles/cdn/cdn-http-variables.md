---
title: Azure CDN kuralları altyapısı için HTTP değişkenleri | Microsoft Dokümanlar
description: HTTP değişkenleri, HTTP isteği ve yanıt meta verilerini almanıza olanak sağlar.
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
ms.date: 05/09/2018
ms.author: allensu
ms.openlocfilehash: b9ced5d4a81effcd73e0243d09bb83ed0fe7667c
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81253705"
---
# <a name="http-variables-for-azure-cdn-rules-engine"></a>Azure CDN kuralları altyapısı için HTTP değişkenleri
HTTP değişkenleri, HTTP isteği ve yanıt meta verilerini alabildiğiniz araçları sağlar. Bu meta veriler daha sonra bir isteği veya yanıtı dinamik olarak değiştirmek için kullanılabilir. HTTP değişkenlerinin kullanımı aşağıdaki kurallar motoru özellikleriyle sınırlıdır:

- [Önbellek Anahtarı Yeniden Yazma](cdn-verizon-premium-rules-engine-reference-features.md#cache-key-rewrite)
- [İstemci İstek Üstbilgisini Değiştir](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-request-header)
- [İstemci Yanıt Üstbilgisini Değiştir](cdn-verizon-premium-rules-engine-reference-features.md#modify-client-response-header)
- [URL Yönlendirme](cdn-verizon-premium-rules-engine-reference-features.md#url-redirect)
- [URL Yeniden Yazma](cdn-verizon-premium-rules-engine-reference-features.md#url-rewrite)

## <a name="definitions"></a>Tanımlar
Aşağıdaki tabloda desteklenen HTTP değişkenleri açıklanmaktadır. GEO meta verileri (örneğin, posta kodu) belirli bir istek için kullanılamadığında boş bir değer döndürülür.


| Adı | Değişken | Açıklama | Örnek değer |
| ---- | -------- | ----------- | ------------ |
| ASN (İstekli) | %{geo_asnum} | İsteklinin AS numarasını gösterir. <br /><br />**Amortismana uğradı:** %{virt_dst_asnum}. <br />Bu değişken %{geo_asnum} lehine amortismana uymuştur. Bu amortismana karşı verilen değişkeni kullanan bir kural çalışmaya devam etse de, yeni değişkeni kullanmak için güncelleştirmeniz gerekir. | AS15133 |
| Şehir (İstekli) | %{geo_city} | İsteklinin şehrini gösterir. | Los Angeles |
| Kıta (İstekli) | %{geo_continent} | İstecinin kıtasını kısaltması ile gösterir. <br />Geçerli değerler: <br />AF: Afrika<br />AS: Asya<br />AB: Avrupa<br />NA: Kuzey Amerika<br />OC: Okyanusya<br />SA: Güney Amerika<br /><br />**Amortismana uğradı:** %{virt_dst_continent}. <br />Bu değişken %{geo_continent} lehine amortismana hazırlanmıştır. <br />Bu amortismana karşı verilen değişkeni kullanan bir kural çalışmaya devam etse de, yeni değişkeni kullanmak için güncelleştirmeniz gerekir.| Yok |
| Çerez Değeri | %{cookie_Cookie} | Çerez terimi tarafından tanımlanan çerez anahtarına karşılık gelen değeri verir. | Örnek Kullanımı: <br />%{cookie__utma}<br /><br />Örnek Değer:<br />111662281.2.10.1222100123 |
| Ülke (İstekli) | %{geo_country} | İsteklinin menşe ülkesini ülke koduyla gösterir. <br />**Amortismana uğradı:** %{virt_dst_country}. <br /><br />Bu değişken %{geo_country} lehine amortismana hazırlanmıştır. Bu amortismana karşı verilen değişkeni kullanan bir kural çalışmaya devam etse de, yeni değişkeni kullanmak için güncelleştirmeniz gerekir. | ABD |
| Belirlenmiş Pazar Alanı (İstekli) | %{geo_dma_code} |İsteklinin medya pazarını bölge koduna göre gösterir. <br /><br />Bu alan yalnızca ABD kaynaklı istekler için geçerlidir.| 745 |
| HTTP İstek Yöntemi | %{request_method} | HTTP istek yöntemini gösterir. | GET |
| HTTP Durum Kodu | %{durum} | Yanıt için HTTP durum kodunu gösterir. | 200 |
| IP Adresi (İstekli) | %{virt_dst_addr} | İsteklinin IP adresini gösterir. | 192.168.1.1 |
| Enlem (İstekli) | %{geo_latitude} | İsteklinin enlemini gösterir. | 34.0995 |
| Boylam (İstekli) | %{geo_longitude} | İsteklinin boylamını gösterir. | -118.4143 |
| Büyükşehir İstatistik Alanı (İstekli) | %{geo_metro_code} | İsteklinin metropolitan alanını gösterir. <br /><br />Bu alan yalnızca ABD kaynaklı istekler için geçerlidir.<br />| 745 |
| Bağlantı Noktası (İstekli) | %{virt_dst_port} | İsteklinin geçici bağlantı noktasını gösterir. | 55885 |
| Posta Kodu (İstekli) | %{geo_postal_code} | İsteklinin posta kodunu gösterir. | 90210 |
| Sorgu Dizesi Bulundu | %{is_args} | Bu değişkenin değeri, isteğin bir sorgu dizesi içerip içermediğine göre değişir.<br /><br />- Sorgu Dizesi Bulundu: ?<br />- Sorgu Dizesi Yok: NULL | ? |
| Sorgu Dize Parametresi Bulundu | %{is_amp} | Bu değişkenin değeri, isteğin en az bir sorgu dize parametresi içerip içermediğine göre değişir.<br /><br />- Parametre Bulundu: &<br />- Parametre yok: NULL | & |
| Sorgu Dize Parametre Değeri | %{arg_&lt;parametresi&gt;} | &lt;Parametre&gt; terimi tarafından tanımlanan sorgu dize parametresine karşılık gelen değeri döndürür. | Örnek Kullanımı: <br />%{arg_language}<br /><br />Örnek Sorgu Dize Parametresi: <br />?language=tr<br /><br />Örnek Değer: tr |
| Sorgu Dize Değeri | %{query_string} | İstek URL'sinde tanımlanan sorgu dize değerinin tamamını gösterir. |key1=val1&key2=val2&key3=val3 |
| Referrer Etki Alanı | %{referring_domain} | Yönlendirici istek üstbilgisinde tanımlanan etki alanını gösterir. | <www.google.com> |
| Bölge (İstekli) | %{geo_region} | Alfasayısal kısaltması aracılığıyla istekte bulunan kişinin bölgesini (örneğin, durum veya bölge) gösterir. | CA |
| İstek Üstbilgi Değeri | %{http_RequestHeader} | RequestHeader terimi tarafından tanımlanan istek üstbilgisine karşılık gelen değeri verir. <br /><br />İstek üstbilgisinin adı bir tire içeriyorsa (örneğin, Kullanıcı Aracısı), alt çizgiyle değiştirin (örneğin, User_Agent).| Örnek Kullanımı: %{http_Connection}<br /><br />Örnek Değer: Keep-Alive | 
| İstek Host | %{ana bilgisayar} | İstek URL'sinde tanımlanan ana bilgisayarını gösterir. | <www.mydomain.com> |
| İstek Protokolü | %{request_protocol} | İstek protokolünü gösterir. | HTTP/1.1 |
| İstek Şeması | %{şema} | İstek düzenini gösterir. |http |
| İstek URI (Göreli) | %{request_uri} | İstek URI'de tanımlanan sorgu dizesi de dahil olmak üzere göreli yolu gösterir. | /marketing/foo.js?loggedin=true |
| İstek URI (Sorgu dizesi olmadan göreli) | %{uri} | İstenen içeriğe göreli yolu gösterir. <br /><br/>Önemli bilgiler:<br />- Bu göreli yol sorgu dizesini dışlar.<br />- Bu göreli yol URL yeniden yazarlarını yansıtır. Bir URL aşağıdaki koşullar altında yeniden yazılacaktır:<br />  &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- URL Yeniden Yazma Özelliği: Bu özellik, URI isteğinde tanımlanan göreli yolu yeniden yazar.<br />    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;- Kenar CNAME URL: Bu istek türü ilgili CDN URL'sine yeniden yazılır. |/800001/corigin/rewrittendir/foo.js |
| İstek URI'si | %{istek} | İsteği açıklar. <br />&lt;Sözdizimi:&gt; &lt;HTTP&gt; &lt;yöntemi nispi yol HTTP protokolü&gt; | GET /marketing/foo.js?loggedin=true HTTP/1.1 |
| Yanıt Üstbilgi Değeri | %{resp_&lt;Yanıt&gt;Başlığı } | YanıtHeader&gt; terimi tarafından tanımlanan yanıt &lt;üstbilgisine karşılık gelen değeri döndürür. <br /><br />Yanıt üstbilgisinin adı bir tire içeriyorsa (örneğin, Kullanıcı Aracısı), alt çizgiyle değiştirin (örneğin, User_Agent). | Örnek Kullanımı: %{resp_Content_Length}<br /><br />Örnek Değeri: 100 |

## <a name="usage"></a>Kullanım
Aşağıdaki tabloda bir HTTP değişkeni belirtmek için uygun sözdizimi açıklanmaktadır.


| Sözdizimi | Örnek | Açıklama |
| ------ | -------- | ---------- |
| %{&lt;HTTPVariable&gt;} | %{ana bilgisayar} | Belirtilen &lt;HTTPVariable&gt;karşılık gelen tüm değeri almak için bu sözdizimini kullanın. |
| %{&lt;HTTPVariableDelimiter&gt;} | %{ana bilgisayar,} | Belirtilen &lt;HTTPVariableDelimiter'a&gt;karşılık gelen tüm değerin karşılığını ayarlamak için bu sözdizimini kullanın. |
| %{&lt;HTTPVariableDelimiterExpression&gt;} | %{host/=^www\.([^ ]+)\.\.\.([^ :]+)/cdn.$2.$3:80} | Bir HTTP değişkeninin değerini değiştirmek, silmek veya işlemek için HTTPVariableDelimiterExpression &lt;&gt; için normal bir ifade kullanın. |

HTTP değişken adları yalnızca alfabetik karakterleri ve alt çizerleri destekler. Desteklenmeyen karakterleri alt çizerlere dönüştürün.

## <a name="delimiter-reference"></a>Delimiter başvurusu
Aşağıdaki etkilerden herhangi birini elde etmek için bir HTTP değişkeninden sonra bir sınırlandırıcı belirtilebilir:

- Değişkenle ilişkili değeri dönüştürün.

     Örnek: Tüm değeri küçük harfe dönüştürün.

- Değişkenle ilişkili değeri silin.

- Değişkenle ilişkili değeri manipüle edin.

     Örnek: HTTP değişkeniyle ilişkili değeri değiştirmek için normal ifadeler kullanın.

Sınır layıcılar aşağıdaki tabloda açıklanmıştır.

| Sınırlayıcı | Açıklama |
| --------- | ----------- |
| := | Varsayılan değerin değişkene atanacağını gösterir: <br />- Eksik <br />- NULL olarak ayarlayın. |
| :+ | Bir değer atandığında değişkene varsayılan değer atanacağını gösterir. |
| : | Değişkene atanan değerin bir alt dizesinin genişletileceğini gösterir. |
| # | Değişkenle ilişkili değerin başında bulunduğunda, bu sınır slayıcısından sonra belirtilen desenin silinmesi gerektiğini belirtir. |
| % | Değişkenle ilişkili değerin sonunda bulunduğunda, bu sınır slayıcısından sonra belirtilen desenin silinmesi gerektiğini belirtir. <br />Bu tanım yalnızca % sembolü sınır layıcı olarak kullanıldığında geçerlidir. |
| / | BIR HTTP değişkenini veya deseni sınırlandırın. |
| // | Belirtilen desenin tüm örneklerini bulun ve değiştirin. |
| /= | Belirtilen desenin tüm oluşumlarını bulun, kopyalayın ve yeniden yazın. |
| , | HTTP değişkeniyle ilişkili değeri küçük harfe dönüştürün. |
| ^ | HTTP değişkeniyle ilişkili değeri büyük harfe dönüştürün. |
| ,, | HTTP değişkeniyle ilişkili değerde belirtilen karakterin tüm örneklerini küçük harfe dönüştürün. |
| ^^ | HTTP değişkeniyle ilişkili değerde belirtilen karakterin tüm örneklerini büyük harfe dönüştürün. |

## <a name="exceptions"></a>Özel durumlar
Aşağıdaki tabloda, belirtilen metnin HTTP değişkeni olarak kabul edilmediği durumlar açıklanmaktadır.

| Koşul | Açıklama | Örnek |
| --------- | ----------- | --------|
| % sembolünden kaçış | Yüzde sembolü bir ters eğik çizgi kullanarak kaçabilir. <br />Sağdaki örnek değeri, http değişkeni olarak değil, gerçek bir değer olarak kabul edilir.| \%{ana bilgisayar} |
| Bilinmeyen değişkenler | Boş bir dize her zaman bilinmeyen değişkenler için döndürülür. | %{unknown_variable} |
| Geçersiz karakterler veya sözdizimi | Geçersiz karakterler veya sözdizimi içeren değişkenler gerçek değerler olarak kabul edilir. <br /><br />Örnek #1: Belirtilen değer geçersiz bir karakter (örneğin, -) içerir. <br /><br />Örnek #2: Belirtilen değer, çift kıvırcık ayraç kümesi içerir. <br /><br />Örnek #3: Belirtilen değerde kapanış kıvırcık ayracı eksik.<br /> | Örnek #1: %{resp_user-agent} <br /><br />Örnek #2: %{{host}} <br /><br />Örnek #3: %{ana bilgisayar |
| Eksik değişken adı | Bir değişken belirtilmediğinde her zaman NULL değeri döndürülür. | %{} |
| İzleyen karakterler | Bir değişkeni takip eden karakterler gerçek değerler olarak kabul edilir. <br />Sağdaki örnek değer, gerçek bir değer olarak kabul edilecek bir iz bırakan kıvırcık ayraç içerir. | %{ana bilgisayar}} |

## <a name="setting-default-header-values"></a>Varsayılan üstbilgi değerlerini ayarlama
Varsayılan değer, aşağıdaki koşullardan herhangi birini karşıladığında üstbilgiye atanabilir:
- Eksik/unset
- NULL olarak ayarlayın.

Aşağıdaki tabloda varsayılan değer nasıl tanımlanılır.

| Koşul | Sözdizimi | Örnek | Açıklama |
| --------- | ------ | --------| ----------- |
| Üstbilgi, aşağıdaki koşullardan herhangi birini karşıladığında varsayılan değere ayarlayın: <br /><br />- Eksik Üstbilgi <br /><br />- Üstbilgi değeri NULL olarak ayarlanır.| %{Değişken:=Değer} | %{http_referrer:=belirtilmemiş} | Yönlendirici üstbilgi yalnızca eksik olduğunda veya NULL olarak ayarlandığında *belirtilmemiş* olarak ayarlanır. Ayarlanmışsa hiçbir işlem yapılmaz. |
| Üstbilgi eksik olduğunda varsayılan değere ayarlayın. | %{Değişken=Değer} | %{http_referrer=belirtilmemiş} | Yönlendirici üstbilgi yalnızca eksik olduğunda *belirtilmemiş* olarak ayarlanır. Ayarlanmışsa hiçbir işlem yapılmaz. |
| Üstbilgi, aşağıdaki koşullardan hiçbirini karşılamadığında varsayılan değere ayarlayın: <br /><br />- Eksik<br /><br /> - NULL olarak ayarlayın. | %{Değişken:+Değer} | %{http_referrer:+belirtilmemiş} | Yönlendirici üstbilgi yalnızca bir değer atandığında *belirtilmemiş* olarak ayarlanır. Eksik veya NULL olarak ayarlanmışsa hiçbir işlem yapılmaz. |

## <a name="manipulating-variables"></a>Değişkenleri manipüle etme
Değişkenler aşağıdaki şekillerde manipüle edilebilir:

- Alt dizeleri genişletme
- Desenleri kaldırma

### <a name="substring-expansion"></a>Substring genişletme
Varsayılan olarak, bir değişken tam değerine genişletir. Yalnızca değişkenin değerinin bir alt dizesini genişletmek için aşağıdaki sözdizimini kullanın.

`%<Variable>:<Offset>:<Length>}`

Önemli bilgiler:

- Ofset terimine atanan değer, alt dizebaşlangıç karakterini belirler:

     - Pozitif: Alt dizenin başlangıç karakteri dizedeki ilk karakterden hesaplanır.
     - Sıfır: Alt dizenin başlangıç karakteri dizedeki ilk karakterdir.
     - Negatif: Alt dizenin başlangıç karakteri dizedeki son karakterden hesaplanır.

- Alt dize uzunluğu *Uzunluk* terimi ile belirlenir:

     - Atlanan: Uzunluk terimini atlayarak substring başlangıç karakteri ve dize sonu arasındaki tüm karakterleri içermesine olanak sağlar.
     - Pozitif: Başlangıç karakterinden sağa substring uzunluğunu belirler.
     - Negatif: Başlangıç karakterinden sola substring uzunluğunu belirler.

#### <a name="example"></a>Örnek:

Aşağıdaki örnek, aşağıdaki örnek istek URL'sine dayanır:

https:\//cdn.mydomain.com/folder/marketing/myconsultant/proposal.html

Aşağıdaki dize değişkenleri işlemek için çeşitli yöntemler gösterir:

https:\//www%{http_host:3}/mobile/%{request_uri:7:10}/%{request_uri:-5:-8}.htm

Örnek istek URL'si temel alınerek, yukarıdaki değişken işleme aşağıdaki değeri üretecektir:

https:\//www.mydomain.com/mobile/marketing/proposal.htm


### <a name="pattern-removal"></a>Desen kaldırma
Belirli bir desenle eşleşen metin, bir değişkenin değerinin başından veya sonundan kaldırılabilir.

| Sözdizimi | Eylem |
| ------ | ------ |
| %{Değişken#Desen} | Bir değişkenin değerinin başında belirtilen desen bulunduğunda metni kaldırın. |
| %{Değişken%Desen} | Bir değişkenin değerinin sonunda belirtilen desen bulunduğunda metni kaldırın. |

#### <a name="example"></a>Örnek:

Bu örnek senaryoda, *request_uri* değişkeni aşağıdaki şekilde ayarlanır:

`/800001/myorigin/marketing/product.html?language=en-US`

Aşağıdaki tablo, bu sözdiziminin nasıl çalıştığını gösterir.

| Örnek sözdizimi | Sonuçlar | |
| ------------- | ------- | --- |
| %{request_uri#/800001}/customerorigin | /customerorigin/myorigin/marketing/product.html?language=tr-US | Değişken desenle başladığı için değiştirildi. |
| %{request_uri%html}htm | /800001/myorigin/marketing/product.html?language=en-US | Değişken desenle bitmediği için değişiklik olmadı.|

### <a name="find-and-replace"></a>Bulma ve değiştirme
Sözdizimini bul ve değiştir aşağıdaki tabloda açıklanmıştır.

| Sözdizimi | Eylem |
| ------ | ------ |
| %{Değişken/Bul/Değiştir} | Belirtilen desenin ilk oluşumunu bulun ve değiştirin. |
| %{Değişken//Bul/Değiştir} | Belirtilen desenin tüm oluşumlarını bulun ve değiştirin. |
| %{Değişken^} |Tüm değeri büyük harfe dönüştürün. |
| %{Değişken^Bul} | Belirtilen desenin ilk oluşumunu büyük harfe dönüştürün. |
| %{Değişken,} | Tüm değeri küçük harfe dönüştürün. |
| %{Değişken,Bul} | Belirtilen desenin ilk oluşumunu küçük harfe dönüştürün. |

### <a name="find-and-rewrite"></a>Bulma ve yeniden yazma
Bul ve değiştirme üzerine bir varyasyon için, yeniden yazarken belirtilen desenle eşleşen metni kullanın. Sözdizimini bulma ve yeniden yazma aşağıdaki tabloda açıklanmıştır.

| Sözdizimi | Eylem |
| ------ | ------ |
| %{Değişken/=Bul/Yeniden Yaz} | Belirtilen desenin tüm oluşumlarını bulun, kopyalayın ve yeniden yazın. |
| %{Değişken/^Bul/Yeniden Yaz} | Değişkenin başında oluştuğunda belirtilen deseni bulun, kopyalayın ve yeniden yazın. |
| %{Değişken/$Find/Yeniden Yazma} | Değişkenin sonunda oluştuğunda belirtilen deseni bulun, kopyalayın ve yeniden yazın. |
| %{Değişken/Bul} | Belirtilen desenin tüm oluşumlarını bulun ve silin. |

Önemli bilgiler:

- Bir dolar işareti ve ardından bir tamsayı (örneğin, 1 TL) belirterek belirtilen desenle eşleşen metni genişletin.

- Birden çok desen belirtilebilir. Desenin belirtildiği sıra, ona atanacak olan tümseği belirler. Aşağıdaki örnekte, ilk desen "www", ikinci desen ikinci düzey etki alanıyla eşleşir ve üçüncü desen üst düzey etki alanıyla eşleşir:

    `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$2.$3:80}`

- Yeniden yazılan değer, herhangi bir metin birleşimi ve bu yer tutuculardan oluşabilir.

    Önceki örnekte, ana bilgisayar adı `cdn.$2.$3:80` (örneğin, cdn.mydomain.com:80) yeniden yazılır.

- Bir desen yer tutucusu (örneğin, 1 $) durumu aşağıdaki bayraklar aracılığıyla değiştirilebilir:
     - U: Genişletilmiş değeri büyük harfe bilebilir.

         Örnek sözdizimi:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$U2.$3:80}`

     - L: Genişletilmiş değeri küçük düşürün.

         Örnek sözdizimi:

         `%{host/=^www\.([^\.]+)\.([^\.:]+)/cdn.$L2.$3:80}`

- Desenden önce bir işleç belirtilmelidir. Belirtilen işleç desen yakalama davranışını belirler:

     - `=`: Belirtilen desenin tüm oluşumlarının yakalanması ve yeniden yazılması gerektiğini gösterir.
     - `^`: Yalnızca belirtilen desenle başlayan metnin yakalanıp alınacağını gösterir.
     - `$`: Yalnızca belirtilen desenle biten metnin yakalanacak olduğunu gösterir.
 
- */Yeniden yazma* değerini atlarsanız, desenle eşleşen metin silinir.


