---
title: Azure Uygulama Ağ Geçidi -Azure portalında Web uygulaması güvenlik duvarı isteği boyutu sınırları ve hariç tutma listeleri
description: Bu makalede, Azure portalı ile Uygulama Ağ Geçidi'nde Web Uygulaması Güvenlik Duvarı istek boyutu sınırları ve dışlama listeleri yapılandırması hakkında bilgi verilmektedir.
services: web-application-firewall
author: vhorne
ms.service: web-application-firewall
ms.date: 02/20/2020
ms.author: victorh
ms.topic: conceptual
ms.openlocfilehash: 7244788bbc7431c7f26363b2852babb72d5697e9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77526799"
---
# <a name="web-application-firewall-request-size-limits-and-exclusion-lists"></a>Web Uygulama Güvenlik Duvarı istek boyut sınırları ve dışlama listeleri

Azure Uygulama Ağ Geçidi Web Uygulaması Güvenlik Duvarı (WAF), web uygulamaları için koruma sağlar. Bu makalede, WAF istek boyutu sınırları ve dışlama listeleri yapılandırma açıklanmaktadır. Bu ayarlar, Uygulama Ağ Geçidinizle ilişkili WAF İlkesi'nde yer alır. WAF İlkeleri hakkında daha fazla bilgi edinmek için [Azure Uygulama Ağ Geçidi'nde Azure Web Uygulaması Güvenlik Duvarı'na](ag-overview.md) bakın ve Uygulama Ağ Geçidi için Web Uygulaması Güvenlik Duvarı ilkeleri [oluşturun](create-waf-policy-ag.md)

## <a name="waf-exclusion-lists"></a>WAF dışlama listeleri

![İstek boyutu sınırları](../media/application-gateway-waf-configuration/waf-policy.png)

WAF dışlama listeleri, waf değerlendirmesinden belirli istek özniteliklerini atlayasınız. Yaygın bir örnek, kimlik doğrulama veya parola alanları için kullanılan Etkin Dizin eklenmiş belirteçleridir. Bu tür öznitelikler, WAF kurallarından yanlış pozitif tetikleyebilecek özel karakterler içermeye eğilimlidir. BIR öznitelik WAF dışlama listesine eklendikten sonra, herhangi bir yapılandırılmış ve etkin WAF kuralı tarafından kabul etmez. Dışlama listeleri kapsamı geneldir.

Dışlama listelerine ada göre aşağıdaki öznitelikler eklenebilir. Seçilen alanın değerleri WAF kurallarına göre değerlendirilmez, ancak adları hala (aşağıdaki Örnek 1'e bakın, Kullanıcı Temsilcisi üstbilginin değeri WAF değerlendirmesinin dışında tutulur). Dışlama listeleri, alanın değerinin denetimini kaldırır.

* İstek Üst Bilgileri
* Çerezler İsteyin
* İstek özniteliği adı (args) gibi bir dışlama öğesi olarak eklenebilir:

   * Form alan adı
   * XML varlığı
   * JSON varlığı
   * URL sorgu dize args

Tam istek üstbilgisi, gövde, çerez veya sorgu dize özniteliği eşleşmesi belirtebilirsiniz.  Veya isteğe bağlı olarak kısmi eşleşmeleri belirtebilirsiniz. Dışlama kuralları kapsamı geneldir ve tüm sayfalar ve tüm kurallar için geçerlidir.

Desteklenen maç ölçütleri işleçleri şunlardır:

- **Equals**: Bu işleç tam bir eşleşme için kullanılır. Örnek olarak, **taşıyıcıToken**adlı bir üstbilgi seçmek için, **taşıyıcıToken**olarak seçici kümesi ile eşitler işleci kullanın.
- **Başlangıç**: Bu işleç, belirtilen seçici değeriyle başlayan tüm alanları eşler.
- **Biter**: Bu işleç belirtilen seçici değeri ile biten tüm istek alanları eşleşir.
- **Contains**: Bu işleç, belirtilen seçici değeri içeren tüm istek alanlarıyla eşleşir.
- **Eşittir any**: Bu işleç tüm istek alanlarıyla eşleşir. * seçici değeri olacaktır.

Tüm durumlarda eşleşen durumda duyarsız ve düzenli ifade seçici olarak izin verilmez.

> [!NOTE]
> Daha fazla bilgi ve sorun giderme yardımı için [WAF sorun giderme](web-application-firewall-troubleshoot.md)bilgisine bakın.

### <a name="examples"></a>Örnekler

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Aşağıdaki örnekler dışlamaların kullanımını göstermektedir.

#### <a name="example-1"></a>Örnek 1

Bu örnekte, kullanıcı aracısı üstbilgisini hariç tutmak istiyorsunuz. Kullanıcı aracısı istek üstbilgisi, ağ protokolü eşlerinin isteyen yazılım kullanıcı aracısının uygulama türünü, işletim sistemini, yazılım satıcısını veya yazılım sürümünü tanımlamasına olanak tanıyan karakteristik bir dize içerir. Daha fazla bilgi için [User-Agent'a](https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/User-Agent)bakın.

Bu üstbilginin değerlendirilmesini devre dışı bırakmanın birçok nedeni olabilir. WAF görür ve kötü niyetli olduğunu varsayar bir dize olabilir. Örneğin, klasik SQL bir dizedeki "x=x" atağı. Bazı durumlarda, bu yasal trafik olabilir. Bu nedenle, bu üstbilginin WAF değerlendirmesinden hariç tutmanız gerekebilir.

Aşağıdaki Azure PowerShell cmdlet kullanıcı aracı üstbilgisini değerlendirmedışı olarak tanımlar:

```azurepowershell
$exclusion1 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestHeaderNames" `
   -SelectorMatchOperator "Equals" `
   -Selector "User-Agent"
```
#### <a name="example-2"></a>Örnek 2

Bu örnek, *istekte* URL üzerinden geçirilen kullanıcı parametresindeki değeri hariç tutar. Örneğin, kullanıcı alanının WAF'ın kötü amaçlı içerik olarak görüntülenebildiği bir dize içermesinin ortamınızda yaygın olduğunu ve bu nedenle bunu engellediğini varsayıl.  WAF'ın alandaki hiçbir şeyi değerlendirmemesi için bu durumda kullanıcı parametresini hariç tutabilirsiniz.

Aşağıdaki Azure PowerShell cmdlet kullanıcı parametresini değerlendirmedışında hariç tutar:

```azurepowershell
$exclusion2 = New-AzApplicationGatewayFirewallExclusionConfig `
   -MatchVariable "RequestArgNames" `
   -SelectorMatchOperator "StartsWith" `
   -Selector "user"
```
Yani URL `http://www.contoso.com/?user%281%29=fdafdasfda` WAF geçirilirse, bu dize **fdafda**değerlendirmek olmaz , ama yine de parametre adı **kullanıcı% 281% 29**değerlendirecektir. 

## <a name="waf-request-size-limits"></a>WAF istek boyutu sınırları



Web Uygulaması Güvenlik Duvarı, istek boyutu sınırlarını alt ve üst sınırlar içinde yapılandırmanıza olanak tanır. Aşağıdaki iki boyut sınırı yapılandırmaları kullanılabilir:

- Maksimum istek gövde boyutu alanı kilobayt cinsinden belirtilir ve dosya yüklemeleri hariç genel istek boyutu sınırını denetler. Bu alan en az 1-KB ile 128-KB maksimum değer arasında olabilir. İstek gövde boyutu için varsayılan değer 128 KB'dir.
- Dosya yükleme sınır alanı MB'de belirtilir ve izin verilen maksimum dosya yükleme boyutunu yönetir. Bu alan en az 1 MB değere ve aşağıdaki maksimum değerlere sahip olabilir:

   - v1 Orta WAF ağ geçitleri için 100 MB
   - v1 Büyük WAF ağ geçitleri için 500 MB
   - v2 WAF ağ geçitleri için 750 MB 

 Dosya yükleme sınırı için varsayılan değer 100 MB'dır.

WAF ayrıca istek vücut denetimini açmak veya kapatmak için yapılandırılabilir bir düğüm sunar. Varsayılan olarak, istek gövde denetimi etkinleştirilir. İstek vücut denetimi kapatılırsa, WAF HTTP ileti gövdesinin içeriğini değerlendirmez. Bu gibi durumlarda WAF, üstbilgi, tanımlama bilgileri ve URI üzerindeki WAF kurallarını uygulamaya devam eder. İstek vücut denetimi kapatılırsa, maksimum istek gövde boyutu alanı geçerli değildir ve ayarlanamaz. İstek gövde denetiminin kapatılması, 128 KB'den büyük iletilerin WAF'a gönderilmesine izin verir, ancak ileti gövdesi güvenlik açıkları için denetlenmez.

## <a name="next-steps"></a>Sonraki adımlar

WAF ayarlarınızı yapılandırdıktan sonra, WAF günlüklerinizi nasıl görüntülediğinizi öğrenebilirsiniz. Daha fazla bilgi için [Uygulama Ağ Geçidi tanılama](../../application-gateway/application-gateway-diagnostics.md#diagnostic-logging)bakın.
