---
title: Azure CDN varlıklarını belirteç kimlik doğrulamasıyla güvence altına alma| Microsoft Dokümanlar
description: Azure CDN varlıklarınıza erişimi güvence altına almak için belirteç kimlik doğrulamasını nasıl kullanacağınızı öğrenin.
services: cdn
documentationcenter: .net
author: zhangmanling
manager: zhangmanling
editor: ''
ms.assetid: 837018e3-03e6-4f9c-a23e-4b63d5707a64
ms.service: azure-cdn
ms.devlang: multiple
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: integration
ms.date: 11/17/2017
ms.author: mezha
ms.openlocfilehash: fa71f472294b91baebc2a6075ddb2b50123e545d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67593386"
---
# <a name="securing-azure-cdn-assets-with-token-authentication"></a>Azure CDN varlıklarını belirteç kimlik doğrulamasıyla güvence altına alma

[!INCLUDE [cdn-premium-feature](../../includes/cdn-premium-feature.md)]

## <a name="overview"></a>Genel Bakış

Belirteç kimlik doğrulaması, Azure İçerik Teslim Ağı'nın (CDN) varlıkları yetkisiz istemcilere sunmasını engellemenize olanak tanıyan bir mekanizmadır. Belirteç kimlik doğrulaması genellikle, ileti panosu gibi farklı bir web sitesinin varlıklarınızı izinsiz olarak kullandığı içeriğin *hotlinking'ini* önlemek için yapılır. Hotlinking'in içerik dağıtım maliyetleriniz üzerinde bir etkisi olabilir. CDN'de belirteç kimlik doğrulaması etkinleştirilerek, CDN içeriği teslim etmeden önce isteklerin CDN kenar sunucusu tarafından doğrulanması. 

## <a name="how-it-works"></a>Nasıl çalışır?

Belirteç kimlik doğrulaması, isteklerin isteğe bağlı bir site tarafından oluşturulduğunu doğrulayarak, istekte bulunan hakkında kodlanmış bilgiler tutan bir belirteç değeri içermesini gerektirir. İçerik, yalnızca kodlanmış bilgiler gereksinimleri karşılıyorsa, istekte bulunana sunulur; aksi takdirde, istekler reddedilir. Aşağıdaki parametrelerden birini veya birkaçını kullanarak gereksinimleri ayarlayabilirsiniz:

- Ülke: [Ülke kodlarında](/previous-versions/azure/mt761717(v=azure.100))belirtilen ülkelerden/bölgelerden kaynaklanan isteklere izin verin veya bunları reddedin.
- URL: Yalnızca belirtilen varlık veya yol ile eşleşen isteklere izin verin.
- Ana Bilgisayar: İstek üstbilgisinde belirtilen ana bilgisayarları kullanan isteklere izin verin veya reddedin.
- Başvurucu: Belirtilen başvurucun isteğine izin verin veya reddedin.
- IP adresi: Yalnızca belirli IP adresinden veya IP alt ağlarından kaynaklanan isteklere izin verin.
- Protokol: İçeriği istemek için kullanılan protokole dayalı isteklere izin verin veya reddedin.
- Son kullanma süresi: Bir bağlantının yalnızca sınırlı bir süre için geçerli kalmasını sağlamak için bir tarih ve saat atayın.

Daha fazla bilgi için, [belirteç kimlik doğrulaması ayarlama'daki](#setting-up-token-authentication)her parametre için ayrıntılı yapılandırma örneklerine bakın.

>[!IMPORTANT] 
> Bu hesaptaki herhangi bir yol için belirteç yetkilendirmesi etkinleştirilmişse, sorgu dizesi önbelleğe alınması için kullanılabilecek tek mod standart önbellek modudur. Daha fazla bilgi için bkz. [Sorgu dizeleri içeren Azure CDN önbelleğe alma davranışını kontrol etme](cdn-query-string-premium.md).

## <a name="reference-architecture"></a>Başvuru mimarisi

Aşağıdaki iş akışı diyagramı, CDN'nin web uygulamanızla çalışmak için belirteç kimlik doğrulamasını nasıl kullandığını açıklar.

![CDN belirteç kimlik doğrulama iş akışı](./media/cdn-token-auth/cdn-token-auth-workflow2.png)

## <a name="token-validation-logic-on-cdn-endpoint"></a>CDN bitiş noktasında belirteç doğrulama mantığı
    
Aşağıdaki akış şeması, belirteç kimlik doğrulaması CDN bitiş noktasında yapılandırıldığında Azure CDN'nin istemci isteğini nasıl doğruladığını açıklar.

![CDN belirteç doğrulama mantığı](./media/cdn-token-auth/cdn-token-auth-validation-logic.png)

## <a name="setting-up-token-authentication"></a>Belirteç kimlik doğrulaması ayarlama

1. Azure [portalından](https://portal.azure.com)CDN profilinize göz atın ve ek portalı başlatmak için **Yönet'i** seçin.

    ![CDN profili Yönet düğmesi](./media/cdn-token-auth/cdn-manage-btn.png)

2. **HTTP Büyük**üzerinde hover , sonra flyout **Token Auth** seçin. Daha sonra şifreleme anahtarı nı ve şifreleme parametrelerini aşağıdaki gibi ayarlayabilirsiniz:

   1. Bir veya daha fazla şifreleme anahtarı oluşturun. Şifreleme anahtarı büyük/küçük harf duyarlıdır ve alfasayısal karakterlerin herhangi bir birleşimini içerebilir. Boşluklar da dahil olmak üzere diğer karakter türlerine izin verilmez. Maksimum uzunluk 250 karakterdir. Şifreleme anahtarlarınızın rasgele olduğundan emin olmak için [OpenSSL aracını](https://www.openssl.org/)kullanarak bunları oluşturmanız önerilir. 

      OpenSSL aracı aşağıdaki sözdizimine sahiptir:

      ```rand -hex <key length>```

      Örnek:

      ```OpenSSL> rand -hex 32``` 

      Kapalı kalma süresini önlemek için hem birincil hem de yedek anahtar oluşturun. Yedekleme anahtarı, birincil anahtarınız güncelleştirilirken içeriğinize kesintisiz erişim sağlar.
    
   2. **Birincil Anahtar** kutusuna benzersiz bir şifreleme anahtarı girin ve isteğe bağlı olarak **Yedek Anahtar** kutusuna bir yedek anahtar girin.

   3. **Minimum Şifreleme Sürüm** listesinden her anahtar için minimum şifreleme sürümünü seçin ve ardından **Güncelleştir'i**seçin:
      - **V2**: 2.0 ve 3.0 belirteçleri oluşturmak için anahtarın kullanılabileceğini gösterir. Bu seçeneği yalnızca eski sürüm 2.0 şifreleme anahtarından sürüm 3.0 anahtarına geçiş yapıyorsunuz.
      - **V3**: (Önerilen) Anahtarın yalnızca sürüm 3.0 belirteçlerini oluşturmak için kullanılabileceğini belirtir.

      ![CDN belirteç auth kurulum anahtarı](./media/cdn-token-auth/cdn-token-auth-setupkey.png)
    
   4. Şifreleme parametrelerini ayarlamak ve bir belirteç oluşturmak için şifreleme aracını kullanın. Şifreleme aracıyla, son kullanma süresi, ülke/bölge, referrer, protokol ve istemci IP'ye (herhangi bir kombinasyonda) dayalı isteklere izin verebilir veya reddedebilirsiniz. Belirteç oluşturmak için biraraya getirilebilen parametrelerin sayısı ve birleşimi için bir sınır olmamasına rağmen, bir belirteç toplam uzunluğu 512 karakterle sınırlıdır. 

      ![CDN şifreleme aracı](./media/cdn-token-auth/cdn-token-auth-encrypttool.png)

      **Aracı Şifreleme** bölümünde aşağıdaki şifreleme parametrelerinden biri veya birkaçı için değerleri girin: 

      > [!div class="mx-tdCol2BreakAll"] 
      > <table>
      > <tr>
      >   <th>Parametre adı</th> 
      >   <th>Açıklama</th>
      > </tr>
      > <tr>
      >    <td><b>ec_expire</b></td>
      >    <td>Belirteci için bir son kullanma süresi atar ve sonra belirteci sona erer. Son kullanma tarihinden sonra gönderilen istekler reddedilir. Bu parametre, standart Unix döneminden bu yana saniye sayısına dayalı bir `1/1/1970 00:00:00 GMT`Unix zaman damgası kullanır. (Standart saat ile Unix saati arasında dönüştürme yapmak için çevrimiçi araçları kullanabilirsiniz.)> 
      >    Örneğin, belirteci süresinin dolmasını `12/31/2016 12:00:00 GMT`istiyorsanız, Unix zaman `1483185600`damgası değerini girin. 
      > </tr>
      > <tr>
      >    <td><b>ec_url_allow</b></td> 
      >    <td>Belirteçleri belirli bir varlığa veya yola uyarlamanızı sağlar. URL'si belirli bir göreli yol ile başlayan isteklere erişimi kısıtlar. URL'ler büyük/küçük harf duyarlıdır. Her yolu virgülle ayırarak birden çok yol gir; boşluk eklemeyin. Gereksinimlerinize bağlı olarak, farklı erişim düzeyi sağlamak için farklı değerler ayarlayabilirsiniz.> 
      >    Örneğin, URL `http://www.mydomain.com/pictures/city/strasbourg.png`için, bu isteklere aşağıdaki giriş değerleri için izin verilir: 
      >    <ul>
      >       <li>Giriş değeri `/`: Tüm isteklere izin verilir.</li>
      >       <li>Giriş değeri, `/pictures`aşağıdaki isteklere izin verilir: <ul>
      >          <li>`http://www.mydomain.com/pictures.png`</li>
      >          <li>`http://www.mydomain.com/pictures/city/strasbourg.png`</li>
      >          <li>`http://www.mydomain.com/picturesnew/city/strasbourgh.png`</li>
      >       </ul></li>
      >       <li>Giriş değeri `/pictures/`: Yalnızca `/pictures/` yolu içeren isteklere izin verilir. Örneğin, `http://www.mydomain.com/pictures/city/strasbourg.png`.</li>
      >       <li>Giriş değeri `/pictures/city/strasbourg.png`: Yalnızca bu belirli yol ve varlık için isteklere izin verilir.</li>
      >    </ul>
      > </tr>
      > <tr>
      >    <td><b>ec_country_allow</b></td> 
      >    <td>Yalnızca belirli bir veya daha fazla ülkeden/bölgeden gelen isteklere izin verir. Diğer tüm ülkelerden/bölgelerden gelen talepler reddedilir. Her ülke için iki harfli [ISO 3166 ülke kodu](/previous-versions/azure/mt761717(v=azure.100)) kullanın ve her birini virgülle ayırın; boşluk eklemeyin. Örneğin, yalnızca ABD ve Fransa'dan erişime izin `US,FR`vermek istiyorsanız, girin.</td>
      > </tr>
      > <tr>
      >    <td><b>ec_country_deny</b></td> 
      >    <td>Belirli bir ülkeden/bölgeden kaynaklanan istekleri reddeder. Diğer tüm ülkelerden/bölgelerden gelen isteklere izin verilir. Uygulama <b>ec_country_allow</b> parametresi ile aynıdır. <b>Hem ec_country_allow</b> hem de <b>ec_country_deny</b> parametrelerinde bir ülke kodu varsa, <b>ec_country_allow</b> parametresi önceliklidir.</td>
      > </tr>
      > <tr>
      >    <td><b>ec_ref_allow</b></td>
      >    <td>Yalnızca belirtilen başvurucun isteklerine izin verir. Bir başvuru, istenen kaynağa bağlı web sayfasının URL'sini tanımlar. Protokolü parametre değerine dahil etmeyin.> 
      >    Aşağıdaki giriş türlerine izin verilir:
      >    <ul>
      >       <li>Bir ana bilgisayar adı veya bir ana bilgisayar adı ve bir yol.</li>
      >       <li>Birden fazla referrer. Birden çok başvuru eklemek için, her başvurucuzun virgülle ayrılması; boşluk eklemeyin. Bir yönlendirici değeri belirtirseniz, ancak tarayıcı yapılandırması nedeniyle istekte yönlendirici bilgileri gönderilmezse, istek varsayılan olarak reddedilir.</li> 
      >       <li>Eksik veya boş yönlendirme bilgileri içeren istekler. Varsayılan olarak, <b>ec_ref_allow</b> parametresi bu tür istekleri engeller. Bu isteklere izin vermek için, "eksik" metni girin veya boş bir değer girin (sondaki virgül kullanarak).</li> 
      >       <li>Alt. Alt etki adlarına izin vermek için\*yıldız işareti girin ( ). Örneğin, tüm alt etki alanlarını `contoso.com`, `*.contoso.com`girin izin vermek için .</li>
      >    </ul> 
      >    Örneğin, tüm alt alan `www.contoso.com`adlarından `contoso2.com`gelen isteklere ve boş veya eksik yönlendiren lere erişimsağlamak için girin. `www.contoso.com,*.contoso.com,missing`</td>
      > </tr>
      > <tr> 
      >    <td><b>ec_ref_deny</b></td>
      >    <td>Belirtilen başvurucun isteklerini reddeder. Uygulama <b>ec_ref_allow</b> parametresi ile aynıdır. <b>Hem ec_ref_allow</b> hem de <b>ec_ref_deny</b> parametrelerinde bir başvuru varsa, <b>ec_ref_allow</b> parametresi önceliklidir.</td>
      > </tr>
      > <tr> 
      >    <td><b>ec_proto_allow</b></td> 
      >    <td>Yalnızca belirtilen protokolden gelen isteklere izin verir. Geçerli değerler `http` `https`, `http,https`veya .</td>
      > </tr>
      > <tr>
      >    <td><b>ec_proto_deny</b></td>
      >    <td>Belirtilen protokolden gelen istekleri reddeder. Uygulama <b>ec_proto_allow</b> parametresi ile aynıdır. <b>Hem ec_proto_allow</b> hem de <b>ec_proto_deny</b> parametrelerinde bir protokol varsa, <b>ec_proto_allow</b> parametresi önceliklidir.</td>
      > </tr>
      > <tr>
      >    <td><b>ec_clientip</b></td>
      >    <td>Belirtilen isteğin IP adresine erişimi kısıtlar. Hem IPV4 hem de IPV6 desteklenir. Belirli bir alt ağla ilişkili tek bir istek IP adresi veya IP adresi belirtebilirsiniz. Örneğin, `11.22.33.0/22` IP adreslerinden 11.22.32.1 ile 11.22.35.254 arasında isteklere izin verir.</td>
      > </tr>
      > </table>

   5. Şifreleme parametre değerlerini girmeyi bitirdikten sonra, **Şifrelemek Için Anahtar** listesinden şifrelemek için bir anahtar (hem birincil hem de yedek anahtar oluşturduysanız) seçin.
    
   6. **Şifreleme Sürüm** listesinden bir şifreleme sürümü seçin: Sürüm 2 için **V2** veya sürüm 3 için **V3** (önerilir). 

   7. Belirteci oluşturmak için **Şifrele'yi** seçin.

      Belirteç oluşturulduktan sonra **Oluşturulan Belirteç** kutusunda görüntülenir. Belirteci kullanmak için, URL yolunuzdaki dosyanın sonuna sorgu dizesi olarak ekleyin. Örneğin, `http://www.domain.com/content.mov?a4fbc3710fd3449a7c99986b`.
        
   8. İsteğe bağlı olarak, belirteç parametrelerini görebilmeniz için belirtecinizi şifre çözme aracıyla test edin. Token'deki belirteç değerini **Decrypt kutusuna** yapıştırın. **Şifreyi Çöz'e Anahtar** listesinden kullanılacak şifreleme anahtarını seçin ve ardından **Şifreyi Çöz'ü**seçin.

      Belirteç şifresi çözüldükten sonra parametreleri **Özgün Parametreler** kutusunda görüntülenir.

   9. İsteğe bağlı olarak, bir istek reddedildiğinde döndürülen yanıt kodu türünü özelleştirin. **Etkin'i**seçin, ardından **Yanıt Kodu** listesinden yanıt kodunu seçin. **Üstbilgi Adı** otomatik olarak **Konum**olarak ayarlanır. Yeni yanıt kodunu uygulamak için **Kaydet'i** seçin. Belirli yanıt kodları için, hata sayfanızın URL'sini **üstbilgi değeri** kutusuna da girmeniz gerekir. **403** yanıt kodu (Yasak) varsayılan olarak seçilir. 

3. **HTTP Large**altında, **Kurallar Motoru**seçin. Özelliği uygulamak, belirteç kimlik doğrulama özelliğini etkinleştirmek ve belirteç kimlik doğrulamayla ilgili ek özellikleri etkinleştirmek için yolları tanımlamak için kurallar altyapısını kullanırsınız. Daha fazla bilgi için [Kurallar motoru başvurusuna](cdn-rules-engine-reference.md)bakın.

   1. Varolan bir kural seçin veya belirteç kimlik doğrulaması uygulamak istediğiniz varlığı veya yolu tanımlamak için yeni bir kural oluşturun. 
   2. Bir kuralda belirteç kimlik doğrulamasını etkinleştirmek için **Özellikler** listesinden **[Token Auth'u](cdn-verizon-premium-rules-engine-reference-features.md#token-auth)** seçin ve ardından **Etkin'i**seçin. Bir kuralı güncelliyorsanız **Güncelleştir'i** veya kural oluşturuyorsanız **Ekle'yi** seçin.
        
      ![CDN kuralları motor belirteç kimlik doğrulaması örnek etkinleştirme](./media/cdn-token-auth/cdn-rules-engine-enable2.png)

4. Kurallar altyapısında, belirteç kimlik doğrulamayla ilgili ek özellikleri de etkinleştirebilirsiniz. Aşağıdaki özelliklerden herhangi birini etkinleştirmek için **Özellikler** listesinden seçin ve ardından **Etkin'i**seçin.
    
   - **[Token Auth Denial Code](cdn-verizon-premium-rules-engine-reference-features.md#token-auth-denial-code)**: İstek reddedildiğinde kullanıcıya döndürülen yanıt türünü belirler. Burada ayarlanan kurallar, belirteç tabanlı kimlik doğrulama sayfasındaki **Özel Reddi İşlemi** bölümünde ayarlanan yanıt kodunu geçersiz kılar.

   - **[Token Auth Ignore URL Case](cdn-verizon-premium-rules-engine-reference-features.md#token-auth-ignore-url-case)**: Belirteci doğrulamak için kullanılan URL'nin büyük/küçük harf duyarlı olup olmadığını belirler.

   - **[Belirteç Auth Parametresi](cdn-verizon-premium-rules-engine-reference-features.md#token-auth-parameter)**: İstenen URL'de görünen belirteç auth sorgu dize parametresini yeniden adlandırır. 
        
     ![CDN kuralları motor belirteç kimlik doğrulama ayarları örneği](./media/cdn-token-auth/cdn-rules-engine2.png)

5. [GitHub'da](https://github.com/VerizonDigital/ectoken)kaynak koduna erişerek belirtecinizi özelleştirebilirsiniz.
   Kullanılabilir diller şunlardır:
    
   - C
   - C#
   - PHP
   - Perl
   - Java
   - Python 

## <a name="azure-cdn-features-and-provider-pricing"></a>Azure CDN özellikleri ve sağlayıcı fiyatlandırması

Özellikler hakkında daha fazla bilgi için [Azure CDN ürün özelliklerine](cdn-features.md)bakın. Fiyatlandırma hakkında daha fazla bilgi için [İçerik Dağıtım Ağı fiyatlandırması'na](https://azure.microsoft.com/pricing/details/cdn/)bakın.
