---
title: Azure Uygulama Ağ Geçidi ile HTTP üstbilgilerini yeniden yazın | Microsoft Dokümanlar
description: Bu makale, Azure Uygulama Ağ Geçidi'ndeki HTTP üstbilgilerini yeniden yazmaya genel bir bakış sağlar
services: application-gateway
author: vhorne
ms.service: application-gateway
ms.topic: article
ms.date: 08/08/2019
ms.author: absha
ms.openlocfilehash: d0b28770940f0e1adeec16aa89cd087299bd4abc
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80132990"
---
# <a name="rewrite-http-headers-with-application-gateway"></a>Uygulama Ağ Geçidi ile HTTP üstbilgilerini yeniden yazın

[!INCLUDE [updated-for-az](../../includes/updated-for-az.md)]

HTTP üstbilgileri, istemci nin ve sunucunun istek veya yanıtla ek bilgileri geçirmesine izin verir. Bu üstbilgileri yeniden yazarak, HSTS/ X-XSS-Protection gibi güvenlikle ilgili üstbilgi alanları ekleme, hassas bilgileri ortaya çıkarabilecek yanıt üstbilgisi alanlarını kaldırma ve bağlantı noktası bilgilerini X-Forwarded-For üstbilgilerinden kaldırma gibi önemli görevleri gerçekleştirebilirsiniz.

Application Gateway, istek ve yanıt paketleri istemci ile arka uç havuzları arasında gidip gelirken HTTP istek ve yanıt üst bilgileri eklemenize ya da mevcut bilgileri kaldırmanıza veya güncelleştirmenize olanak tanır. Ayrıca, ancak bazı koşulların yerine getirilmesi durumunda belirtilen üst bilgilerin yeniden yazılmasını sağlamak için koşullar eklemenize de olanak tanır.

Uygulama Ağ Geçidi, [istekler](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers#server-variables) ve yanıtlar hakkında ek bilgi depolamanıza yardımcı olan birkaç sunucu değişkenini de destekler. Bu, güçlü yeniden yazma kuralları oluşturmanızı kolaylaştırır.

> [!NOTE]
>
> HTTP üstbilgi yeniden yazma desteği yalnızca [Standard_V2 ve sku WAF_v2](application-gateway-autoscaling-zone-redundant.md)için kullanılabilir.

![Üstbilgiyeniden yazma](media/rewrite-http-headers/rewrite-headers.png)

## <a name="supported-headers"></a>Desteklenen üstbilgi

Ana Bilgisayar, Bağlantı ve Yükseltme üstbilgisi dışında tüm üstbilgi istek ve yanıtlardaki tüm üstbilgilerini yeniden yazabilirsiniz. Ayrıca, özel üstbilgi oluşturmak ve bunları üzerinden yönlendirilen istek ve yanıtlara eklemek için uygulama ağ geçidini de kullanabilirsiniz.

## <a name="rewrite-conditions"></a>Koşulları yeniden yazma

HTTP(S) isteklerinin ve yanıtlarının içeriğini değerlendirmek ve yalnızca bir veya daha fazla koşul yerine getirildiğinde üstbilginin yeniden yazılması için yeniden yazma koşullarını kullanabilirsiniz. Uygulama ağ geçidi, HTTP(S) isteklerinin ve yanıtlarının içeriğini değerlendirmek için bu tür değişkenleri kullanır:

- Istekte HTTP başlıkları.
- Yanıtta HTTP başlıkları.
- Uygulama Ağ Geçidi sunucu değişkenleri.

Belirli bir değişkenin var olup olmadığını, belirli bir değişkenin belirli bir değerle eşleşip eşleşmediğini veya belirli bir değişkenin belirli bir desenle eşleşip eşleşmediğini değerlendirmek için bir koşul kullanabilirsiniz. Koşullarda düzenli ifade deseni eşleştirmesi ayarlamak için [Perl Uyumlu Düzenli İfadeler (PCRE) kitaplığını](https://www.pcre.org/) kullanırsınız. Normal ifade sözdizimi hakkında bilgi edinmek için [Perl normal ifadeler ana sayfasına](https://perldoc.perl.org/perlre.html)bakın.

## <a name="rewrite-actions"></a>Eylemleri yeniden yazma

Yeniden yazmak istediğiniz istek ve yanıt üstbilgisini ve üstbilgi için yeni değeri belirtmek için eylemleri yeniden yazma kullanırsınız. Yeni bir üstbilgi oluşturabilir, varolan bir üstbilginin değerini değiştirebilir veya varolan bir üstbilgiyi silebilirsiniz. Yeni bir üstbilginin veya varolan bir üstbilginin değeri bu tür değerlere ayarlanabilir:

- Metin.
- Üstbilgi isteyin. İstek üstbilgisini belirtmek için {http_req_*üstbilgi*}sözdizimini kullanmanız gerekir.
- Yanıt üst bilgisi. Yanıt üstbilgisini belirtmek için {http_resp_*üstbilgi*}.'yi kullanmanız gerekir.
- Sunucu değişkeni. Bir sunucu değişkeni belirtmek için {var_*serverVariable*} sözdizimini kullanmanız gerekir.
- Metin, istek üstbilgi, yanıt üstbilgi ve sunucu değişkeninin birleşimi.

## <a name="server-variables"></a>Sunucu değişkenleri

Uygulama Ağ Geçidi, sunucu, istemciyle bağlantı ve bağlantıdaki geçerli istek hakkında yararlı bilgiler depolamak için sunucu değişkenlerini kullanır. Depolanan bilgilere örnek olarak istemcinin IP adresi ve web tarayıcısı türü verilebilir. Sunucu değişkenleri, örneğin yeni bir sayfa yüklendiğinde veya form yayınlandığında dinamik olarak değişir. Bu değişkenleri yeniden yazma koşullarını değerlendirmek ve üstbilgi yeniden yazmak için kullanabilirsiniz.

Uygulama ağ geçidi bu sunucu değişkenlerini destekler:

| Değişken adı | Açıklama                                                  |
| -------------------------- | :----------------------------------------------------------- |
| add_x_forwarded_for_proxy  | Ip1, IP2, IP3 vb. `client_ip` biçiminde eklenen değişkenin yer ettiği X-Forwarded-For istemci istek üstbilgi alanı (bu tabloda daha sonra açıklamaya bakınız). X-Forwarded-For alanı istemci istek üstbilgisinde yoksa, `add_x_forwarded_for_proxy` değişken `$client_ip` değişkene eşittir. Bu değişken, özellikle Uygulama Ağ Geçidi tarafından belirlenen X-Forwarded-For üstbilgisini yeniden yazmak istediğinizde, üstbilginin bağlantı noktası bilgisi olmadan yalnızca IP adresini içermesi için kullanışlıdır. |
| ciphers_supported          | İstemci tarafından desteklenen şifrelerin listesi.          |
| ciphers_used               | Kurulan bir TLS bağlantısı için kullanılan şifreler dizesi. |
| client_ip                  | Başvuru ağ geçidinin isteği aldığı istemcinin IP adresi. Uygulama ağ geçidinden ve kaynak istemciden önce ters proxy varsa, *client_ip* ters proxy'nin IP adresini döndürecektir. |
| client_port                | İstemci bağlantı noktası.                                                  |
| client_tcp_rtt             | İstemci TCP bağlantısı hakkında bilgi. TCP_INFO soketi seçeneğini destekleyen sistemlerde kullanılabilir. |
| client_user                | HTTP kimlik doğrulaması kullanıldığında, kimlik doğrulaması için kullanıcı adı verilir. |
| konak                       | Bu öncelik sırasına göre: istek satırındaki ana bilgisayar adı, Ana Bilgisayar istek üstbilgialanından ana bilgisayar adı veya istekle eşleşen sunucu adı. |
| cookie_*adı*              | *Cookie adı.*                                            |
| http_method                | URL isteğinde bulunmak için kullanılan yöntem. Örneğin, GET veya POST. |
| http_status                | Oturum durumu. Örneğin, 200, 400 veya 403.                       |
| http_version               | İstek protokolü. Genellikle HTTP/1.0, HTTP/1.1 veya HTTP/2.0. |
| query_string               | İstenen URL'de "?" aşağıdaki değişken/değer çiftleri listesi. |
| received_bytes             | İstek uzunluğu (istek satırı, üstbilgi ve istek gövdesi dahil). |
| request_query              | İstek satırındaki bağımsız değişkenler.                                |
| request_scheme             | İstek şeması: http veya https.                            |
| request_uri                | Tam orijinal istek URI (bağımsız değişkenler ile).                   |
| sent_bytes                 | İstemciye gönderilen bayt sayısı.                             |
| server_port                | İsteği kabul eden sunucunun bağlantı noktası.                 |
| ssl_connection_protocol    | Kurulan BIR TLS bağlantısının protokolü.        |
| ssl_enabled                | Bağlantı TLS modunda çalışıyorsa "A)'. Aksi takdirde, boş bir dize. |

## <a name="rewrite-configuration"></a>Yapılandırmayı yeniden yazma

HTTP üstbilgi yeniden yapılandırmak için, bu adımları tamamlamanız gerekir.

1. HTTP üstbilgi yeniden yazmak için gerekli olan nesneleri oluşturun:

   - **Yeniden Yazma Eylemi**: Yeniden yazmak istediğiniz istek ve istek üstbilgi alanlarını ve üstbilgi için yeni değeri belirtmek için kullanılır. Bir veya daha fazla yeniden yazma koşullarını yeniden yazma eylemiyle ilişkilendirebilirsiniz.

   - **Yeniden yazma koşulu**: İsteğe bağlı yapılandırma. Yeniden yazma koşulları, HTTP(S) istek ve yanıtlarının içeriğini değerlendirir. HTTP(S) isteği veya yanıtı yeniden yazma koşuluyla eşleşirse yeniden yazma eylemi gerçekleşir.

     Birden fazla koşulu bir eylemle ilişkilendirirseniz, eylem yalnızca tüm koşullar karşılandığında oluşur. Başka bir deyişle, işlem mantıksal ve işlemdir.

   - **Yeniden yazma kuralı**: Birden çok yeniden yazma eylemi / yeniden yazma koşul birleşimlerini içerir.

   - **Kural sırası**: Yeniden yazma kurallarının yürütülme sırasını belirlemeye yardımcı olur. Bu yapılandırma, yeniden yazma kümesinde birden çok yeniden yazma kuralınız olduğunda yararlıdır. Daha düşük kural sırası değeriolan bir yeniden yazma kuralı önce çalışır. Aynı kural sırasını iki yeniden yazma kuralına atarsanız, yürütme sırası belirleyici değildir.

   - **Yeniden Yazma Kümesi**: İstek yönlendirme kuralıyla ilişkilendirilecek birden çok yeniden yazma kuralı içerir.

2. Yeniden yazma kümesini *(rewriteRuleSet)* yönlendirme kuralına takın. Yeniden yazma yapılandırması yönlendirme kuralı ile kaynak dinleyiciye eklenir. Temel bir yönlendirme kuralı kullandığınızda, üstbilgi yeniden yazma yapılandırması bir kaynak dinleyiciyle ilişkilidir ve genel bir üstbilgi yeniden yazmaktır. Yol tabanlı yönlendirme kuralı kullandığınızda, üstbilgi yeniden yazma yapılandırması URL yol haritasında tanımlanır. Bu durumda, yalnızca bir sitenin belirli yol alanı için geçerlidir.
   > [!NOTE]
   > URL Yeniden Yazma üstbilgi değiştirin; yolun URL'sini değiştirmez.

Birden çok HTTP üstbilgi yeniden yazma kümeleri oluşturabilir ve her yeniden yazma kümesini birden çok dinleyiciye uygulayabilirsiniz. Ancak belirli bir dinleyiciye yalnızca bir yeniden yazma kümesi uygulayabilirsiniz.

## <a name="common-scenarios"></a>Genel senaryolar

Üstbilgi yeniden kullanmak için bazı yaygın senaryolar aşağıda verilmiştir.

### <a name="remove-port-information-from-the-x-forwarded-for-header"></a>Bağlantı noktası bilgilerini X-Forwarded-For üstbilgisinden kaldırma

Uygulama Ağ Geçidi, istekleri arka uca iletmeden önce tüm isteklere X-Forwarded-For başlığı ekler. Bu üstbilgi, IP bağlantı noktalarının virgülden ayrılmış bir listesidir. Arka uç sunucularının yalnızca IP adreslerini içermek için üstbilgiye ihtiyaç duyduğu senaryolar olabilir. Bağlantı noktası bilgilerini X-Forwarded-For üstbilgisinden kaldırmak için üstbilgiyi yeniden yazabilirsiniz. Bunu yapmanın bir yolu, üstbilgiadd_x_forwarded_for_proxy sunucu değişkenine ayarlamaktır:

![Bağlantı noktasını kaldırma](media/rewrite-http-headers/remove-port.png)

### <a name="modify-a-redirection-url"></a>Yeniden yönlendirme URL'sini değiştirme

Bir arka uç uygulaması yeniden yönlendirme yanıtı gönderdiğinde, istemciyi arka uç uygulamasında belirtilenden farklı bir URL'ye yönlendirmek isteyebilirsiniz. Örneğin, bir uygulama hizmeti bir uygulama ağ geçidinin arkasında barındırıldığında ve istemcinin göreli yoluna yeniden yönlendirme yapmasını gerektirdiğinde bunu yapmak isteyebilirsiniz. (Örneğin, contoso.azurewebsites.net/path1'dan contoso.azurewebsites.net/path2'a yönlendirme.)

Uygulama Hizmeti çok kiracılı bir hizmet olduğundan, isteği doğru bitiş noktasına yönlendirmek için istekteki ana bilgisayar üstbilgisini kullanır. Uygulama hizmetlerinin varsayılan etki alanı adı *.azurewebsites.net (contoso.azurewebsites.net deyin) ve uygulama ağ geçidinin etki alanı adından farklıdır (contoso.com deyin). İstemciden gelen özgün istek, ana bilgisayar adı olarak uygulama ağ geçidinin etki alanı adı (contoso.com) olduğundan, uygulama ağ geçidi ana bilgisayar adını contoso.azurewebsites.net olarak değiştirir. Uygulama hizmetinin isteği doğru bitiş noktasına yönlendirebilmeleri için bu değişikliği yapar.

Uygulama hizmeti bir yeniden yönlendirme yanıtı gönderdiğinde, yanıtının konum üstbilgisinde, uygulama ağ geçidinden aldığı istekteki yle aynı ana bilgisayar adını kullanır. Bu nedenle istemci, başvuru ağ geçidinden (contoso.com/path2) geçmek yerine isteği doğrudan contoso.azurewebsites.net/path2 yapacaktır. Uygulama ağ geçidini atlayarak istenmez.

Bu sorunu, yer üstbilgisindeki ana bilgisayar adını uygulama ağ geçidinin etki alanı adına ayarlayarak çözebilirsiniz.

Ana bilgisayar adını değiştirme adımları şunlardır:

1. Yanıttaki konum üstbilgisinin azurewebsites.net içerip içermediğini değerlendiren bir durumla yeniden yazma kuralı oluşturun. Deseni `(https?):\/\/.*azurewebsites\.net(.*)$`girin.
1. Uygulama ağ geçidinin ana bilgisayarının adını görebilmek için konum üstbilgisini yeniden yazmak için bir eylem gerçekleştirin. Bunu üstbilgi `{http_resp_Location_1}://contoso.com{http_resp_Location_2}` değeri olarak girerek yapın.

![Konum üstbilgisini değiştirme](media/rewrite-http-headers/app-service-redirection.png)

### <a name="implement-security-http-headers-to-prevent-vulnerabilities"></a>Güvenlik açıklarını önlemek için güvenlik http üstbilgilerini uygulayın

Uygulama yanıtında gerekli üstbilgi uygulayarak çeşitli güvenlik açıklarını giderebilirsiniz. Bu güvenlik başlıkları X-XSS-Protection, Strict-Transport-Security ve Content-Security-Policy'yi içerir. Tüm yanıtlar için bu üstbilgi ayarlamak için Uygulama Ağ Geçidi'ni kullanabilirsiniz.

![Güvenlik üstbilgi](media/rewrite-http-headers/security-header.png)

### <a name="delete-unwanted-headers"></a>İstenmeyen üstbilgileri silme

Bir HTTP yanıtından hassas bilgileri gösteren üstbilgileri kaldırmak isteyebilirsiniz. Örneğin, arka uç sunucu adı, işletim sistemi veya kitaplık ayrıntıları gibi bilgileri kaldırmak isteyebilirsiniz. Bu üstbilgi kaldırmak için uygulama ağ geçidini kullanabilirsiniz:

![Üstbilgi silme](media/rewrite-http-headers/remove-headers.png)

### <a name="check-for-the-presence-of-a-header"></a>Üstbilginin varlığını denetle

Bir ÜSTbilgi veya sunucu değişkeninin varlığı için bir HTTP isteği veya yanıt üstbilgisini değerlendirebilirsiniz. Bu değerlendirme, yalnızca belirli bir üstbilgi olduğunda bir üstbilgiyeniden gerçekleştirmek istediğinizde yararlıdır.

![Üstbilginin varlığını denetleme](media/rewrite-http-headers/check-presence.png)

## <a name="limitations"></a>Sınırlamalar

- Bir yanıtın aynı ada sahip birden fazla üstbilgisi varsa, bu üstbilgiden birinin değerini yeniden yazmak yanıttaki diğer üstbilginin düşmesine neden olur. Bu genellikle Set-Cookie üstbilgisi ile gerçekleşebilir, çünkü yanıtta birden fazla Set-Cookie üstbilgisi olabilir. Bu senaryolardan biri, uygulama ağ geçidi olan bir uygulama hizmetini kullandığınız ve uygulama ağ geçidinde çerez tabanlı oturum afinitesini yapılandırdığınız da dır. Bu durumda yanıt iki Set-Cookie üstbilgileri içerecektir: biri uygulama hizmeti `Set-Cookie: ARRAffinity=ba127f1caf6ac822b2347cc18bba0364d699ca1ad44d20e0ec01ea80cda2a735;Path=/;HttpOnly;Domain=sitename.azurewebsites.net` tarafından kullanılan, örneğin: ve başka `Set-Cookie: ApplicationGatewayAffinity=c1a2bd51lfd396387f96bl9cc3d2c516; Path=/`bir uygulama ağ geçidi yakınlık için, örneğin. Bu senaryoda Ayar-Çerez üstbilgilerinden birinin yeniden yazılması, diğer Ayar-Çerez üstbilgisinin yanıttan kaldırılmasına neden olabilir.

- Bağlantı, Yükseltme ve Ana Bilgisayar üstbilgilerini yeniden yazmak şu anda desteklenmiyor.

- Üstbilgi [adları, RFC 7230'da](https://tools.ietf.org/html/rfc7230#page-27)tanımlandığı gibi alfasayısal karakterler ve belirli semboller içerebilir. Şu anda Başlık adlarında\_alt karakter ( ) özel karakter desteklemiyorum.

## <a name="next-steps"></a>Sonraki adımlar

HTTP üstbilgilerini yeniden yazmayı öğrenmek için bkz:

- [Azure portalLarını kullanarak HTTP üstbilgilerini yeniden yazma](https://docs.microsoft.com/azure/application-gateway/rewrite-http-headers-portal)
- [Azure PowerShell'i kullanarak HTTP üstbilgilerini yeniden yazın](add-http-header-rewrite-rule-powershell.md)
