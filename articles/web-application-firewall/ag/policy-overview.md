---
title: Azure Web Uygulama Güvenlik Duvarı (WAF) ilkesine genel bakış
description: Bu makale, Web Uygulaması Güvenlik Duvarı (WAF) genel, site başına ve URI başına ilkelerine genel bir bakıştır.
services: web-application-firewall
ms.topic: article
author: winthrop28
ms.service: web-application-firewall
ms.date: 02/01/2020
ms.author: victorh
ms.openlocfilehash: fff4fb5707c07098fd7fac261a36909224365cdf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060263"
---
# <a name="azure-web-application-firewall-waf-policy-overview"></a>Azure Web Uygulama Güvenlik Duvarı (WAF) ilkesine genel bakış

Web Uygulaması Güvenlik Duvarı İlkeleri tüm WAF ayarlarını ve yapılandırmalarını içerir. Buna dışlamalar, özel kurallar, yönetilen kurallar ve benzeri kurallar dahildir. Bu ilkeler daha sonra bir uygulama ağ geçidi (genel), dinleyici (site başına) veya bunların etkili olması için yol tabanlı bir kuralla (URI başına) ilişkilendirilir.

> [!NOTE]
> Site başına ve URI başına Azure Web Uygulaması Güvenlik Duvarı (WAF) genel önizlemededir.
> 
> Bu genel önizleme bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılmamalıdır. Belirli özellikler desteklenmiyor olabilir, kısıtlı yeteneklere sahip olabilir veya tüm Azure konumlarında mevcut olmayabilir. Daha fazla bilgi için Microsoft [Azure Önizlemeleri için Ek Kullanım Koşulları'na](https://azure.microsoft.com/support/legal/preview-supplemental-terms/)bakın.

Oluşturabileceğiniz ilke sayısında bir sınır yoktur. Bir ilke oluşturduğunuzda, etkili olmak için bir uygulama ağ geçidi ile ilişkili olmalıdır. Uygulama ağ geçitleri, dinleyiciler ve yol tabanlı kuralların herhangi bir birleşimi ile ilişkili olabilir.

## <a name="global-waf-policy"></a>Küresel WAF politikası

Bir WAF ilkesini genel olarak ilişkilendirdiğinizde, Application Gateway WAF'ınızın arkasındaki her site aynı yönetilen kurallar, özel kurallar, dışlamalar ve diğer yapılandırılmış ayarlarla korunur.

Tüm sitelere tek bir ilke uygulamak istiyorsanız, ilkeyi uygulama ağ geçidiyle ilişkilendirebilirsiniz. Daha fazla bilgi için, Azure portalını kullanarak bir WAF ilkesi oluşturmak ve uygulamak [için Uygulama Ağ Geçidi için Web Uygulaması Güvenlik Duvarı ilkeleri oluştur'a](create-waf-policy-ag.md) bakın. 

## <a name="per-site-waf-policy"></a>Site başına WAF ilkesi

Site başına WAF ilkeleriyle, site başına ilkeler kullanarak tek bir WAF'ın arkasında farklı güvenlik gereksinimleri olan birden çok siteyi koruyabilirsiniz. Örneğin, WAF'Nizin arkasında beş site varsa, dışlamaları, özel kuralları, yönetilen kural kümelerini ve her site için diğer tüm WAF ayarlarını özelleştirmek için beş ayrı WAF politikasına (her dinleyici için bir tane) sahip olabilirsiniz.

Uygulama ağ geçidinizin genel bir ilkesi olduğunu varsan. Ardından, bu uygulama ağ geçidindeki bir dinleyiciye farklı bir ilke uygularsınız. Dinleyicinin politikası artık sadece o dinleyici için geçerli. Uygulama ağ geçidinin genel ilkesi, kendilerine atanmış belirli bir ilke olmayan diğer tüm dinleyiciler ve yol tabanlı kurallar için hala geçerlidir.

## <a name="per-uri-policy"></a>URI başına politikası

URI düzeyine kadar daha fazla özelleştirme için, bir WAF ilkesini yol tabanlı bir kuralla ilişkilendirebilirsiniz. Tek bir siteiçinde farklı ilkeler gerektiren belirli sayfalar varsa, WAF ilkesinde yalnızca belirli bir URI'yi etkileyen değişiklikler yapabilirsiniz. Bu, bir ödeme veya oturum açma sayfası veya WAF'ınızın arkasındaki diğer sitelere göre daha spesifik bir WAF ilkesine ihtiyaç duyan diğer URL'ler için geçerli olabilir.

Site başına WAF ilkelerinde olduğu gibi, daha spesifik ilkeler daha az özel ilkeleri geçersiz kılar. Bu, BIR URL yol haritasındaki URI başına ilkenin, site başına veya küresel WAF ilkesini geçersiz kdığı anlamına gelir.

## <a name="example"></a>Örnek

Üç siteniz olduğunu varsa, contoso.com, fabrikam.com ve aynı uygulama ağ geçidinin arkasında adatum.com. Her üç siteye de bir WAF uygulanmasını istiyorsunuz, ancak müşterilerin ürünleri ziyaret ettiği, göz attığı ve satın aldığı yer olduğu için adatum.com ile daha fazla güvenlik gerekiyor.

Bazı yanlış pozitif lerin trafiği engellemesini durdurmak için gerekirse bazı temel ayarlar, dışlamalar veya özel kurallarla BIRLIKTE WAF'a küresel bir ilke uygulayabilirsiniz. Bu durumda, fabrikam.com ve contoso.com SQL arka ucu olmayan statik sayfalar olduğundan, genel SQL enjeksiyon kurallarının çalıştırılması gerekmez. Böylece küresel politikada bu kuralları devre dışı kullanabilirsiniz.

Bu küresel politika contoso.com ve fabrikam.com için uygundur, ancak oturum açma bilgilerinin ve ödemelerin işlendiği adatum.com konusunda daha dikkatli olmanız gerekir. Adatum dinleyicisine site başına bir ilke uygulayabilir ve SQL kurallarını çalışır durumda bırakabilirsiniz. Ayrıca bazı trafiği engelleyen bir çerez olduğunu varsayalım, böylece yanlış pozitif durdurmak için bu çerez için bir dışlama oluşturabilirsiniz. 

Uri'adatum.com/payments dikkatli olmanız gereken yerdir. Bu nedenle, uri'ye başka bir ilke uygulayın ve tüm kuralları etkin bırakın ve ayrıca tüm dışlamaları kaldırın.

Bu örnekte, iki site için geçerli olan genel bir ilkeniz vardır. Bir site için geçerli olan site başına bir politikanız ve ardından belirli bir yol tabanlı kural için geçerli olan URI başına bir ilkeniz vardır. Bu örnek için ilgili PowerShell için site başına ve URI başına politikaların nasıl oluşturulup oluşturulabildiğini (var olduğunda buraya bağlantı ekle) bakın.

## <a name="existing-waf-configurations"></a>Mevcut WAF yapılandırmaları

Tüm yeni Web Uygulaması Güvenlik Duvarı'nın WAF ayarları (özel kurallar, yönetilen kural kümesi yapılandırmaları, dışlamalar vb.) bir WAF ilkesinde bulunur. Varolan bir WAF'ınız varsa, bu ayarlar WAF yapılandırmanızda hala bulunabilir. Yeni WAF ilkesine geçme hakkında daha fazla bilgi için [WAF Config'i bir WAF Politikasına geçirin.](https://docs.microsoft.com/azure/web-application-firewall/ag/migrate-policy) 


## <a name="next-steps"></a>Sonraki adımlar

Azure PowerShell'i kullanarak site başına ve URI başına ilkeler oluşturun.
