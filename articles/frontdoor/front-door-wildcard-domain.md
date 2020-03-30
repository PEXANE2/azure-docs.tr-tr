---
title: Azure Ön Kapı - Joker karakter etki alanları için destek
description: Bu makale, Azure Ön Kapı'nın özel etki alanları listesinde joker alan adlarını eşlemeyi ve yönetmeyi nasıl desteklediğini anlamanıza yardımcı olur
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2020
ms.author: sharadag
ms.openlocfilehash: c568c9cc5c57098385cc7399459ec656cdbfc305
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537446"
---
# <a name="wildcard-domains"></a>Joker karakter etki alanları

Apeks etki alanları ve alt etki alanları dışında, bir joker alan adını ön yüz ana bilgisayarları veya Ön Kapı profilinizin özel etki alanları listenize eşleyebilirsiniz. Ön Kapı config'inizde joker karakter etki alanlarının olması, yapılandırmayı her alt etki alanını ayrı ayrı eklemek ve/veya belirtmek için yapılandırmayı değiştirmek zorunda kalmadan, bir API, uygulama veya web sitesi için birden çok alt etki alanı için trafik yönlendirme davranışını basitleştirir. Örnek olarak, joker karakter etki alanı `customer1.contoso.com` `*.contoso.com` `customer2.contoso.com`ekleyerek `customerN.contoso.com` aynı yönlendirme kuralını kullanarak , ve aynı yönlendirme kuralını kullanarak yönlendirmeyi tanımlayabilirsiniz.

Joker karakter etki alanları desteğiyle çözülen önemli senaryolardan bazıları şunlardır:

- Artık Ön Kapınızdaki her alt etki alanında yerleşik ve sonra HTTPS'nin her alt etki alanı için bir sertifika bağlamasını etkinleştirmeniz gerekmez.
- Bir uygulama yeni bir alt etki alanı eklerse, artık üretim Ön Kapı yapılandırmanızı değiştirmeniz gerekmez. Else, daha önce alt etki alanı ekleme, ona bir sertifika bağlama, bir web uygulaması güvenlik duvarı (WAF) ilkesi ekleyerek, farklı yönlendirme kurallarına etki alanı ekleyerek gerekli.

> [!NOTE]
> Şu anda joker karakter etki alanları yalnızca API, PowerShell ve CLI üzerinden desteklenir. Azure portalı üzerinden joker alan adlarını yönetme desteği kullanılamıyor.

## <a name="adding-wildcard-domains"></a>Joker karakter etki alanları ekleme

Frontend Hosts veya Domains bölümü altında bir joker etki alanına binebilirsiniz. Alt etki alanlarında olduğu gibi, Ön Kapı da joker etki alanınız için bir CNAME eşleme olduğunu doğrular. Bu DNS eşleme, joker karakter `*.contoso.com` için CNAME eşlemini doğrulamak `contoso.azurefd.net` için `afdverify.contoso.com` `afdverify.contoso.azurefd.net` eşlenen afdverify geçici eşlemesi gibi doğrudan bir CNAME eşlemesi de olabilir (Azure DNS joker kayıtları destekler).

Ayrıca, en fazla sınayan ana bilgisayarları, ön uç ana bilgisayarlarındaki joker karakter etki alanının tek düzeyalt etki alanlarını da ekleyebilirsiniz. ön uç ana bilgisayarsınırı. Bu işlevsellik, bir alt etki alanı için etki alanlarının geri kalanından (joker karakter etki alanından) farklı bir rota tanımlamak veya belirli bir alt etki alanı için farklı bir WAF ilkesine sahip olmak için gerekli olabilir. Yani, `*.contoso.com` tekrar `foo.contoso.com` etki alanı sahipliği kanıtlamak zorunda `foo.bar.contoso.com` kalmadan ekleyerek izin verecek `*.contoso.com`ama bu tek bir düzey alt etki alanı değildir. Ek `foo.bar.contoso.com` etki alanı sahipliği doğrulaması `*.bar.contosonews.com` olmadan eklemek için eklenmesi gerekir.

### <a name="limitations"></a>Sınırlamalar

1. Belirli bir Ön Kapı profiline joker karakter etki alanı eklenirse, aynı sıcağa başka bir Ön Kapı profiline eklenemez. 
2. Belirli bir Ön Kapı profiline joker karakter etki alanı eklenirse, bu joker karakter etki alanının alt etki alanları Microsoft profilinden diğer Ön Kapı veya Azure CDN'sine eklenemez
3. Joker karakter etki alanının bir alt etki alanı Ön Kapı profiline veya Microsoft profilinden bir Azure CDN'sine eklenirse, joker karakter etki alanı başka bir Ön Kapı profiline eklenemez. 
4. İki profil (Microsoft'tan Ön Kapı veya Azure CDN) bir kök etki alanının çeşitli alt etki alanları varsa, joker karakter etki alanları profillerden hiçbirine eklenmez.

## <a name="certificate-binding-for-wildcard-domains-and-its-subdomains"></a>Joker karakter etki alanları ve alt etki alanları için sertifika bağlama

Joker karakter etki alanınızda HTTPS trafiğini kabul etmek için joker karakter etki alanında HTTPS'yi etkinleştirmeniz gerekir. Joker karakter etki alanı için sertifika bağlama, joker karakter sertifikası gerektirir, diğer bir süre, sertifikanın özne adı da joker karakter etki alanına sahip olmalıdır.

> [!NOTE]
> Şu anda, joker karakter etki alanları için HTTPS'yi etkinleştirmek için yalnızca kendi özel SSL sertifika seçeneğinizi kullanmak kullanılabilir. Ön Kapı yönetilen sertifikalar joker karakter etki alanları için kullanılamaz. 

Alt etki alanları için Key Vault'unuzdan aynı joker karakter sertifikasını kullanmayı seçebilir veya alt etki alanları için Ön Kapı Yönetilen sertifikalarının kullanımı da desteklenir.
Joker karakter etki alanı için bir alt etki alanı eklendiyse ve joker karakter etki alanı zaten ilişkili bir sertifikaya sahipse, bu alt etki alanının HTTPS'si devre dışı tutulamaz. Alt etki alanı, farklı bir Key Vault sertifikası veya Ön Kapı yönetilen sertifikası tarafından geçersiz kılınmadığı sürece, varsayılan olarak joker karakter etki alanının sertifika bağlamasını kullanır.

## <a name="web-application-firewall-for-wildcard-domains-and-its-subdomains"></a>Joker karakter etki alanları ve alt etki alanları için Web uygulaması güvenlik duvarı

WAF ilkeleri, diğer etki adlarına benzer bir joker karakter etki alanına eklenebilir. Joker karakter etki alanının alt etki alanına farklı bir WAF ilkesi uygulanabilir. Alt etki alanları için, joker alan adı ile aynı ilke olsa bile WAF ilkesinin kullanılacağını açıkça belirtmeniz gerekir. Alt etki alanları, WAF ilkesini joker karakter etki alanından otomatik olarak **devralmaz.**

WAF'ın bir alt etki alanı için çalışmasını istemediğiniz bir senaryonuz varsa, yönetilen veya özel kural kümeleri olmayan boş bir WAF ilkesi oluşturabilirsiniz.

## <a name="routing-rules-for-wildcard-domains-and-its-subdomains"></a>Joker karakter etki alanları ve alt etki alanları için yönlendirme kuralları

Yönlendirme kuralını yapılandırırken, ön uç ana bilgisayar olarak joker karakter etki alanı seçebilirsiniz. Joker karakter etki alanı ile alt etki alanları için de farklı yol davranışınız olabilir. [Front Door'un rota eşleştirmesini nasıl yaptığı](front-door-route-matching.md)açıklandığı gibi, çalışma zamanında etki alanı için farklı yönlendirme kuralları arasında en özel eşleşme seçilir.

> [!WARNING]
> **Route 1**gibi iki yönlendirme kuralınız `*.foo.com/*` varsa : Backend Pool A ve **Route 2**ile eşlenen : `bar.foo.com/somePath/*` Backend Pool B'ye eşlenir ve bir istek `bar.foo.com/anotherPath/*`gelirse, ön kapı her iki rotada da eşleşme bulamayacağı için müşterileriniz arızaları görür. Bunun nedeni, [rota eşleştirme algoritmamız uyarınca,](front-door-route-matching.md)Ön Kapı'nın daha spesifik etki alanı eşleşmesine göre Route 2'yi seçmesi, ancak eşleşen yol desenleri olmadığını bulmasıdır. 


## <a name="next-steps"></a>Sonraki adımlar

- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Ön Kapı'ya](front-door-custom-domain.md)nasıl özel bir etki alanı ekleyeceğinizi öğrenin.
- [Özel bir etki alanında HTTPS'yi](front-door-custom-domain-https.md)nasıl etkinleştireceklerini öğrenin.
