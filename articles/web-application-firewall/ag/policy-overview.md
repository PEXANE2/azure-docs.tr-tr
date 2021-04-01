---
title: Azure Web uygulaması güvenlik duvarı (WAF) ilkesine genel bakış
description: Bu makale, Web uygulaması güvenlik duvarı (WAF) genel, site başına ve URI başına ilkelere yönelik bir genel bakıştır.
services: web-application-firewall
ms.topic: article
author: winthrop28
ms.service: web-application-firewall
ms.date: 11/20/2020
ms.author: victorh
ms.openlocfilehash: 59ca0b85ba2aff29bdb2ad3379c1054041d2b4cb
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "96518745"
---
# <a name="azure-web-application-firewall-waf-policy-overview"></a>Azure Web uygulaması güvenlik duvarı (WAF) ilkesine genel bakış

Web uygulaması güvenlik duvarı Ilkeleri tüm WAF ayarlarını ve yapılandırmasını içerir. Bu, dışlamaları, özel kuralları, yönetilen kuralları vb. içerir. Bu ilkeler daha sonra bir uygulama ağ geçidi (genel), bir dinleyici (site başına) veya yol tabanlı bir kural (URI başına), etkili olması için ilişkilendirilir.

Oluşturabileceğiniz ilke sayısı için bir sınır yoktur. Bir ilke oluşturduğunuzda, etkili olması için bir uygulama ağ geçidi ile ilişkilendirilmesi gerekir. Uygulama ağ geçitleri, dinleyicileri ve yol tabanlı kuralların herhangi bir birleşimiyle ilişkilendirilebilir.

## <a name="global-waf-policy"></a>Global WAF ilkesi

Bir WAF ilkesini küresel olarak ilişkilendirdiğinizde, WAF Application Gateway arkasındaki her site aynı yönetilen kurallarla, Özel kurallarla, dışlamalarla ve diğer yapılandırılmış ayarlarla korunur.

Tek bir ilkenin tüm sitelere uygulanmasını istiyorsanız, ilkeyi uygulama ağ geçidiyle ilişkilendirebilirsiniz. Daha fazla bilgi için, Azure portal kullanarak bir WAF ilkesi oluşturmak ve uygulamak üzere [Application Gateway Için Web uygulaması güvenlik duvarı Ilkeleri oluşturma](create-waf-policy-ag.md) konusuna bakın. 

## <a name="per-site-waf-policy"></a>Site başına WAF ilkesi

Site başına uygulanan WAF ilkeleri sayesinde site başına uygulanan ilkeleri kullanarak farklı güvenlik gereksinimlerine sahip olan siteleri tek bir WAF arkasında koruyabilirsiniz. Örneğin WAF örneğinizin arkasında beş site varsa her bir site için dışlamaları, özel kuralları, yönetilen kural kümelerini ve diğer tüm WAF ayarlarını özelleştirmek için beş ayrı WAF ilkesi (her dinleyici için bir tane) oluşturabilirsiniz.

Uygulama ağ geçidinize genel ilke uyguladığınızı düşünelim. Sonrasında bu uygulama ağ geçidindeki bir dinleyiciye farklı bir ilke uygulayabilirsiniz. Dinleyiciye uyguladığınız ilke sadece o dinleyiciyi etkiler. Uygulama ağ geçidinin genel ilkesi, özel ilke atanmamış olan diğer tüm dinleyiciler ve yol tabanlı kurallar için geçerli olmaya devam eder.

## <a name="per-uri-policy"></a>URI başına ilke

URI düzeyine daha fazla özelleştirme için, bir WAF ilkesini yol tabanlı bir kuralla ilişkilendirebilirsiniz. Farklı ilkeler gerektiren tek bir sitede belirli sayfalar varsa, WAF ilkesinde yalnızca belirli bir URI 'yi etkileyen değişiklikler yapabilirsiniz. Bu, bir ödeme veya oturum açma sayfası veya WAF 'nizin arkasındaki diğer sitelerden daha fazla belirli bir WAF ilkesi gerektiren başka bir URI için de geçerlidir.

Site başına WAF ilkelerinde olduğu gibi, daha belirgin ilkeler, daha az özel ilke geçersiz kılar. Bu, bir URL yolu eşlemesindeki URI başına bir ilkenin üzerinde her site veya genel WAF ilkesini geçersiz kılacak anlamına gelir.

### <a name="example"></a>Örnek

Üç siteniz olduğunu varsayalım: contoso.com, fabrikam.com ve adatum.com tümü aynı uygulama ağ geçidi arkasında. Tüm üç siteye de bir WAF uygulanmasını istiyorsunuz, ancak müşterilerin ürünleri ziyaret ettiği, gözatdığı ve satın satın aldığı yer olduğu için adatum.com ile güvenlik eklemeniz gerekir.

Bir genel ilkeyi, bazı temel ayarlar, dışlamaları veya özel kurallar ile birlikte kullanarak, trafiği engelleyen bazı yanlış pozitif durumları durdurmanız gerekir. Bu durumda, fabrikam.com ve contoso.com, SQL arka ucu olmayan statik sayfalar olduğundan, genel SQL ekleme kurallarının çalışıyor olması gerekmez. Bu nedenle, genel ilkede bu kuralları devre dışı bırakabilirsiniz.

Bu genel ilke contoso.com ve fabrikam.com için uygundur, ancak oturum açma bilgilerinin ve ödemelerin işlendiği adatum.com konusunda daha dikkatli olmanız gerekir. Adatum dinleyicisine site başına ilke uygulayabilir ve SQL kurallarını çalışır durumda bırakabilirsiniz. Ayrıca, bazı trafiği engelleyen bir tanımlama bilgisinin olduğunu varsayalım. bu nedenle, yanlış pozitif değeri durdurmak için bu tanımlama bilgisi için bir dışlama oluşturabilirsiniz. 

Adatum.com/payments URI 'SI, dikkatli olmanız gereken yerdir. Bu nedenle, bu URI üzerinde başka bir ilke uygulayın ve tüm kuralları etkin bırakın ve ayrıca tüm dışlamaları kaldırın.

Bu örnekte, iki site için geçerli olan genel bir ilkeniz vardır. Tek bir site için geçerli olan bir site başına ilkeniz ve ardından belirli bir yol tabanlı kural için geçerli olan URI başına bir ilke vardır. Bu örnek için ilgili PowerShell için [Azure PowerShell kullanarak site başına WAF Ilkelerini yapılandırma](per-site-policies.md) konusuna bakın.

## <a name="existing-waf-configurations"></a>Var olan WAF yapılandırması

Tüm yeni Web uygulaması güvenlik duvarı WAF ayarları (özel kurallar, yönetilen kural kümesi yapılandırması, dışlamaları vb.) bir WAF ilkesinde bulunur. Var olan bir WAF 'niz varsa, bu ayarlar WAF yapılandırmanızda yine de mevcut olabilir. Yeni WAF ilkesine taşıma hakkında daha fazla bilgi için [WAF config dosyasını BIR WAF Ilkesine geçirin](./migrate-policy.md). 


## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell kullanarak site başına ve URI başına Ilkeler oluşturun](per-site-policies.md).
