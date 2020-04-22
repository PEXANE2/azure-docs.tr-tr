---
title: Azure Ön Kapı - Joker karakter etki alanları için destek
description: Bu makale, Azure Ön Kapı'nın özel etki alanları listesinde joker alan adlarını eşlemeyi ve yönetmeyi nasıl desteklediğini anlamanıza yardımcı olur.
services: frontdoor
author: sharad4u
ms.service: frontdoor
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/10/2020
ms.author: sharadag
ms.openlocfilehash: 6d8a6d6f0b05b9b7fd0144959c82b6a2c9e659a3
ms.sourcegitcommit: d57d2be09e67d7afed4b7565f9e3effdcc4a55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/22/2020
ms.locfileid: "81768310"
---
# <a name="wildcard-domains"></a>Joker karakter etki alanları

Tepe etki alanları ve alt etki alanları dışında, Azure Ön Kapı profilinizde bir joker alan adını ön uç ana bilgisayarları veya özel etki alanları listenize eşleyebilirsiniz. Azure Ön Kapı yapılandırmanızda joker karakter etki alanlarının olması, aynı yönlendirme kuralından bir API, uygulama veya web sitesi için birden çok alt etki alanı için trafik yönlendirme davranışını basitleştirir. Her alt etki alanını ayrı ayrı eklemek veya belirtmek için yapılandırmayı değiştirmeniz gerekmez. Örnek olarak, aynı yönlendirme kuralını `customer1.contoso.com`kullanarak `customer2.contoso.com`ve `customerN.contoso.com` joker karakter etki alanını `*.contoso.com`ekleyerek yönlendirmeyi tanımlayabilirsiniz.

Joker karakter etki alanları desteğiyle geliştirilmiş önemli senaryolar şunlardır:

- Azure Ön Kapı profilinizdeki her alt etki alanında yerleşik olmanız ve ardından HTTPS'nin her alt etki alanı için bir sertifika bağlamasını sağlamanız gerekmez.
- Bir uygulama yeni bir alt etki alanı eklerse, artık üretimAzure Ön Kapı yapılandırmanızı değiştirmeniz gerekmez. Daha önce, alt etki alanı eklemek, bir sertifika bağlamak, bir web uygulaması güvenlik duvarı (WAF) ilkesi eklemek ve sonra farklı yönlendirme kurallarına etki alanı eklemek zorunda kaldı.

> [!NOTE]
> Şu anda joker karakter etki alanları yalnızca API, PowerShell ve Azure CLI üzerinden desteklenir. Azure portalında joker alan adları ekleme ve yönetme desteği kullanılamaz.

## <a name="adding-wildcard-domains"></a>Joker karakter etki alanları ekleme

Ön uç ana bilgisayarlar veya etki alanları için bölümün altına bir joker karakter etki alanı ekleyebilirsiniz. Azure Ön Kapı, alt etki alanlarında olduğu gibi joker alan adınız için CNAME kayıt eşlemesi olduğunu doğrular. Bu DNS eşleme, eşlenen gibi `*.contoso.com` doğrudan cname kayıt eşleme `contoso.azurefd.net`olabilir. Veya afdverify geçici eşleme kullanabilirsiniz. Örneğin, `afdverify.contoso.com` joker karakter `afdverify.contoso.azurefd.net` için CNAME kayıt eşlemi doğrulanır.

> [!NOTE]
> Azure DNS joker kayıtlarını destekler.

Ön uç ana bilgisayarlarına, joker karakter etki alanının tek düzeyli alt etki alanlarını ön uç ana bilgisayarlarının sınırına kadar ekleyebilirsiniz. Bu işlevsellik için gerekli olabilir:

- Bir alt etki alanı için etki alanlarının geri kalanından (joker karakter etki alanından) farklı bir rota tanımlama.

- Belirli bir alt etki alanı için farklı bir WAF ilkesine sahip olmak. Örneğin, `*.contoso.com` etki `foo.contoso.com` alanı sahipliğini yeniden kanıtlamak zorunda kalmadan eklemeye izin verir. Ancak tek bir `foo.bar.contoso.com` düzey alt etki alanı olmadığı için `*.contoso.com`izin vermez. Ek `foo.bar.contoso.com` etki alanı sahipliği doğrulaması olmadan eklemek için eklenmesi `*.bar.contosonews.com` gerekir.

Joker karakter etki alanlarını ve bunların alt etki alanlarını belirli sınırlamalarla ekleyebilirsiniz:

- Azure Ön Kapı profiline joker karakter etki alanı eklenirse:
  - Joker karakter etki alanı başka hiçbir Azure Ön Kapı profiline eklenmez.
  - Joker karakter etki alanının birinci düzey alt etki alanları başka bir Azure Ön Kapı profiline veya Azure İçerik Dağıtım Ağı profiline eklenmez.
- Bir Joker karakter etki alanının alt etki alanı Azure Ön Kapı profiline veya Azure İçerik Teslim Ağı profiline eklenirse, joker karakter etki alanı diğer Azure Ön Kapı profillerine eklenmez.
- İki profil (Azure Ön Kapı veya Azure İçerik Dağıtım Ağı) bir kök etki alanının çeşitli alt etki alanına sahipse, joker karakter etki alanları profillerden hiçbirine eklenmez.

## <a name="certificate-binding"></a>Sertifika bağlama

Joker karakter etki alanınızda HTTPS trafiğini kabul etmek için joker karakter etki alanında HTTPS'yi etkinleştirmeniz gerekir. Joker karakter etki alanı için sertifika bağlama, joker karakter sertifikası gerektirir. Diğer bir zamanda, sertifikanın özne adı da joker karakter etki alanına sahip olmalıdır.

> [!NOTE]
> Şu anda, joker karakter etki alanları için HTTPS'yi etkinleştirmek için yalnızca kendi özel SSL sertifika seçeneğinizi kullanmak kullanılabilir. Azure Ön Kapı yönetilen sertifikalar joker karakter etki alanları için kullanılamaz.

Aynı joker karakter sertifikasını Azure Key Vault'tan veya alt etki alanları için Azure Ön Kapı yönetilen sertifikalarından kullanmayı seçebilirsiniz.

Zaten onunla ilişkili bir sertifikaya sahip bir joker alan adı için bir alt etki alanı eklenirse, alt etki alanı için HTTPS devre dışı tutulamaz. Farklı bir Key Vault veya Azure Ön Kapı yönetilen sertifika geçersiz kılmadığı sürece alt etki alanı joker karakter etki alanı için sertifika bağlama kullanır.

## <a name="waf-policies"></a>WAF politikaları

WAF ilkeleri, diğer etki adlarına benzer şekilde joker karakter etki adlarına eklenebilir. Joker karakter etki alanının alt etki alanına farklı bir WAF ilkesi uygulanabilir. Alt etki alanları için, joker karakter etki alanıyla aynı ilke olsa bile WAF ilkesinin kullanılacağını belirtmeniz gerekir. Alt etki alanları, WAF ilkesini joker karakter etki alanından otomatik olarak *devralmaz.*

Bir WAF ilkesinin bir alt etki alanı için çalışmasını istemiyorsanız, yönetilen veya özel kural kümeleri olmayan boş bir WAF ilkesi oluşturabilirsiniz.

## <a name="routing-rules"></a>Yönlendirme kuralları

Yönlendirme kuralını yapılandırırken, ön uç ana bilgisayar olarak joker karakter etki alanı seçebilirsiniz. Joker karakter etki alanları ve alt etki alanları için de farklı rota davranışınız olabilir. [Azure Ön Kapı'nın rota eşleştirmesini nasıl yaptığı](front-door-route-matching.md)konusunda açıklandığı gibi, çalışma zamanında etki alanı için farklı yönlendirme kuralları arasında en özel eşleşme seçilir.

> [!IMPORTANT]
> Yönlendirme kurallarınızda eşleşen yol desenleri olması gerekir, aksi takdirde istemcileriniz hataları görür. Örneğin, Route 1 (arka`*.foo.com/*` uç havuzu A'ya eşlenmiş) ve Route 2`bar.foo.com/somePath/*` (arka uç bpool B'ye eşlenmiş) gibi iki yönlendirme kuralınız vardır. Sonra, bir istek `bar.foo.com/anotherPath/*`için geldi. Azure Ön Kapı, daha belirli bir etki alanı eşleşmesine göre Route 2'yi seçer, ancak rotalar arasında eşleşen yol desenleri bulamaz.

## <a name="next-steps"></a>Sonraki adımlar

- Azure Ön Kapı profilini nasıl [oluşturabilirsiniz](quickstart-create-front-door.md)öğrenin.
- [Azure Ön Kapı'ya nasıl özel bir etki alanı ekleyeceğinizi](front-door-custom-domain.md)öğrenin.
- [Özel bir etki alanında HTTPS'yi](front-door-custom-domain-https.md)nasıl etkinleştireceklerini öğrenin.
