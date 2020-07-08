---
title: Azure Application Gateway 'de Web uygulaması güvenlik duvarı istek boyutu sınırları ve dışlama listeleri Azure portal
description: Bu makalede, Azure portal Application Gateway Web uygulaması güvenlik duvarı istek boyutu sınırları ve dışlama listeleri yapılandırması hakkında bilgi sağlanır.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 02/20/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: ddf631601510e725d77cc391ad41192a47ab0cf1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84752482"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Web uygulaması güvenlik duvarı istek boyutu sınırları ve dışlama listeleri

Azure Application Gateway Web uygulaması güvenlik duvarı (WAF), Web uygulamaları için koruma sağlar. Bu makalede WAF istek boyutu sınırları ve dışlama listeleri yapılandırması açıklanır. Bu ayarlar, Application Gateway ile ilişkili WAF Ilkesinde bulunur. WAF Ilkeleri hakkında daha fazla bilgi edinmek için bkz. Azure ['Da Azure Web uygulaması güvenlik duvarı Application Gateway](ag-overview.md) ve [Application Gateway Için Web uygulaması güvenlik duvarı ilkeleri oluşturma](create-waf-policy-ag.md)

## <a name="waf-exclusion-lists"></a>WAF dışlama listeleri

![İstek boyutu sınırları](../media/application-gateway-waf-configuration/waf-policy.png)

WAF dışlama listeleri, bir WAF değerlendirmesinden belirli istek özniteliklerini atlamanızı sağlar. Ortak bir örnek, kimlik doğrulama veya parola alanları için kullanılan Active Directory eklenen belirteçlerdir. Bu tür öznitelikler, WAF kurallarından yanlış bir pozitif değeri tetikleyebilecek özel karakterler içeriyor olabilir. WAF dışlama listesine bir öznitelik eklendikten sonra, yapılandırılmış ve etkin bir WAF kuralı tarafından değerlendirilmez. Dışlama listeleri, kapsamda geneldir.

Aşağıdaki öznitelikler ada göre dışlama listelerine eklenebilir. Seçilen alanın değerleri WAF kurallarına göre değerlendirilmez, ancak adları yine de olur (Aşağıdaki örnek 1 ' i inceleyin, Kullanıcı Aracısı üstbilgisinin değeri WAF değerlendirmesinden hariç tutulur). Dışlama Listeleri alanın değerinin incelemesini kaldırır.

* İstek Üst Bilgileri
* İstek tanımlama bilgileri
* İstek özniteliği adı (args) bir dışlama öğesi olarak eklenebilir, örneğin:

   * Form alanı adı
   * JSON varlığı
   * URL sorgu dizesi bağımsız değişkenleri

Tam bir istek üst bilgisi, gövde, tanımlama bilgisi veya sorgu dizesi özniteliği eşleşmesi belirtebilirsiniz.  Ya da isteğe bağlı olarak kısmi eşleşmeler belirtebilirsiniz. Dışlama kuralları kapsamda geneldir ve tüm sayfalar ve tüm kurallar için geçerlidir.

Aşağıdakiler, desteklenen eşleşme ölçütü işleçleridir:

- **Eşittir**: Bu işleç tam eşleşme için kullanılır. Örnek olarak, **yataertoken**adlı bir üst bilgi seçmek için, Selector ile WITH, **yataya**kümesi olarak ayarlanmış Equals işlecini kullanın.
- **Ile başlar**: Bu işleç, belirtilen Seçici değeriyle başlayan tüm alanlarla eşleşir.
- **Şununla biter**: Bu işleç, belirtilen Seçici değeriyle biten tüm istek alanlarıyla eşleşir.
- **Contains**: Bu işleç, belirtilen Seçici değerini içeren tüm istek alanlarıyla eşleşir.
- **Eşittir any**: Bu işleç tüm istek alanlarıyla eşleşir. * Seçici değeri olacaktır.

Tüm durumlarda eşleşen büyük/küçük harfe duyarlı değildir ve normal ifadeye seçici olarak izin verilmez.

> [!NOTE]
> Daha fazla bilgi ve sorun giderme yardımı için bkz. [WAF sorun giderme](web-application-firewall-troubleshoot.md).

### <a name="examples"></a>Örnekler

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Aşağıdaki örneklerde, dışlamaları kullanımı gösterilmektedir.

#### <a name="example-1"></a>Örnek 1

Bu örnekte, Kullanıcı Aracısı üst bilgisini dışlamak istiyorsunuz. Kullanıcı Aracısı istek üst bilgisi, ağ protokolü eşlerinin uygulama türü, işletim sistemi, yazılım satıcısı veya istekte bulunan yazılım Kullanıcı aracısının yazılım sürümünü belirlemesine izin veren bir özellik dizesi içerir. Daha fazla bilgi için bkz. [Kullanıcı Aracısı](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent).

Bu üst bilgiyi değerlendirmeyi devre dışı bırakmak için herhangi bir sayıda neden olabilir. WAF 'nin gördüğü bir dize olabilir ve kötü amaçlı olduğunu varsaymaktadır. Örneğin, bir dizedeki klasik SQL saldırısı "x = x". Bazı durumlarda bu yasal trafik olabilir. Bu nedenle, bu üstbilgiyi WAF değerlendirmesinden çıkarmanız gerekebilir.

Aşağıdaki Azure PowerShell cmdlet 'i, Kullanıcı Aracısı üst bilgisini değerlendirmeden dışlar:

```azurepowershell
$exclusion1 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestHeaderNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "User-Agent"
```
#### <a name="example-2"></a>Örnek 2

Bu örnek, istekte URL aracılığıyla geçirilen *Kullanıcı* parametresindeki değeri dışlar. Örneğin, Kullanıcı alanı için ortamınızda, WAF 'in kötü amaçlı içerik olarak görünümlerinin olduğu bir dize içermesi için ortak olduğunu varsayalım.  Bu durumda Kullanıcı parametresini dışarıda bırakabilirsiniz, böylece WAF alandaki herhangi bir şeyi değerlendirmez.

Aşağıdaki Azure PowerShell cmdlet 'i, Kullanıcı parametresini değerlendirmeden dışlar:

```azurepowershell
$exclusion2 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestArgNames" `
   -SelectorMatchOperator "StartsWith" `
   -Selector "user"
```
Bu nedenle, URL `http://www.contoso.com/?user%281%29=fdafdasfda` WAF 'ye geçirilirse **fdadfdasfda**dizesini değerlendirmez, ancak yine de **%281 %29**parametre adı kullanıcı olarak değerlendirilir. 

## <a name="waf-request-size-limits"></a>WAF istek boyutu sınırları



Web uygulaması güvenlik duvarı, istek boyutu sınırlarını alt ve üst sınırlar içinde yapılandırmanıza olanak tanır. Aşağıdaki iki boyut sınırı yapılandırması kullanılabilir:

- En büyük istek gövdesi boyutu alanı kilobayt cinsinden belirtilir ve dosya yükleme işlemleri hariç olmak üzere genel istek boyutu sınırını denetler. Bu alan, 1 KB en az 128-KB maksimum değer arasında değişebilir. İstek gövdesi boyutu için varsayılan değer 128 KB 'tır.
- Dosya karşıya yükleme sınırı alanı MB olarak belirtilir ve izin verilen en büyük dosya yükleme boyutunu yönetir. Bu alan en az 1 MB ve aşağıdaki en yüksek değer olabilir:

   - V1 orta WAF ağ geçitleri için 100 MB
   - V1 büyük WAF ağ geçitleri için 500 MB
   - v2 WAF ağ geçitleri için 750 MB 

 Karşıya dosya yükleme sınırı için varsayılan değer 100 MB 'tır.

WAF, istek gövdesi incelemesini açmak veya kapatmak için yapılandırılabilir bir düğme de sunmaktadır. Varsayılan olarak, istek gövdesi incelemesi etkindir. İstek gövdesi incelemesi kapalıysa, WAF, HTTP ileti gövdesinin içeriğini değerlendirmez. Bu gibi durumlarda WAF, üst bilgiler, tanımlama bilgileri ve URI üzerinde WAF kurallarını uygulamaya devam eder. İstek gövdesi incelemesi kapalıysa, en fazla istek gövdesi boyutu alanı uygulanamaz ve ayarlanamaz. İstek gövdesi incelemesini kapatmak, 128 KB 'den büyük mesajların WAF 'ye gönderilmesini sağlar, ancak ileti gövdesi güvenlik açıkları için incelenemiyor.

## <a name="next-steps"></a>Sonraki adımlar

WAF ayarlarınızı yapılandırdıktan sonra, WAF günlüklerinizi görüntülemeyi öğrenebilirsiniz. Daha fazla bilgi için bkz. [tanılama Application Gateway](../../application-gateway/application-gateway-diagnostics.md#diagnostic-logging).
