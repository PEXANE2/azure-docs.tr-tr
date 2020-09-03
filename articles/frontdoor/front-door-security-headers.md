---
title: Kural altyapısına güvenlik üstbilgileri ekleme-Azure ön kapısı
description: Bu makalede, Azure ön kapısından kurallar altyapısı aracılığıyla bir güvenlik üst bilgisi yapılandırma öğretilir
services: frontdoor
documentationcenter: ''
author: duongau
editor: ''
ms.service: frontdoor
ms.devlang: na
ms.topic: overview
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 6/22/2020
ms.author: duau
ms.openlocfilehash: ad1e8a8a2162ece69af9904d76a394d4bad5de23
ms.sourcegitcommit: 5a3b9f35d47355d026ee39d398c614ca4dae51c6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89399149"
---
# <a name="add-security-headers-with-rules-engine"></a>Kural Altyapısı ile Güvenlik üst bilgisi ekleme

HTTP Strict-Transport-Security (HSTS), X-XSS-Protection, Content-Security-Policy veya X-Frame-Options gibi tarayıcı tabanlı güvenlik açıklarını engellemek için güvenlik üst bilgilerini uygulayın. Güvenlik tabanlı öznitelikler, tanımlama bilgileriyle de tanımlanabilir.

Aşağıdaki örnek, kurallarınızın altyapı yapılandırmanızın ilişkilendirildiği yolda tanımlanan yol ile eşleşen tüm gelen isteklere Içerik-güvenlik-Ilke üst bilgisi nasıl ekleneceğini gösterir. Burada, yalnızca güvenilir sitemizdeki betiklerin **https://apiphany.portal.azure-api.net** uygulamamızda çalışmasına izin veririz.

## <a name="add-a-content-security-policy-header-in-azure-portal"></a>Azure portal Içerik-Güvenlik Ilkesi üst bilgisi ekleme

1. Bu özel kuralı oluşturmadan önce, [bir ön kapı oluşturmayı](quickstart-create-front-door.md) veya AFD ya da Rules Engine özelliğini kullanarak ilk kez bir [kural altyapısı oluşturmayı](front-door-tutorial-rules-engine.md) öğrenin.

2. Yeni kural eklemek için **Ekle**'ye tıklayın. Kurala bir ad verin ve ardından **eylem**  >  **yanıt üst bilgisi**Ekle ' ye tıklayın.

3. Bu üst bilgiyi bu rotaya yönelik tüm gelen isteklere yanıt olarak **eklemek için, bu üstbilgiyi Ekle olarak** ayarlayın.

4. Üst bilgi adını ekleyin: **Content-Security-Policy** ve bu üstbilginin kabul etmesi gereken değerleri tanımlayın. Bu senaryoda, *"Script-src ' Self ' öğesini seçtik https://apiphany.portal.azure-api.net .*

5. Yapılandırmanıza istediğiniz kuralların tümünü ekledikten sonra tercih ettiğiniz rotaya gidip kural motoru yapılandırmanızı yol kuralınız ile ilişkilendirmeyin. Kuralın çalışmasını sağlamak için bu adım gereklidir. 

![Portal örneği](./media/front-door-rules-engine/rules-engine-security-header-example.png)

> [!NOTE]
> Bu senaryoda, kurala [eşleşen koşulları](front-door-rules-engine-match-conditions.md) ekleyemedik. Yol kuralında tanımlanan yol ile eşleşen tüm gelen istekler bu kural uygulanmış olacaktır. Bu isteklerin yalnızca bir alt kümesine uygulanmasını istiyorsanız, özel eşleşme koşullarınızı bu kurala eklediğinizden emin olun.


## <a name="next-steps"></a>Sonraki adımlar

- [AFD kuralları altyapısı](front-door-rules-engine.md)hakkında daha fazla bilgi edinin. 
- [Front Door oluşturmayı](quickstart-create-front-door.md) öğrenin.
- [Front Door’un nasıl çalıştığını](front-door-routing-architecture.md) öğrenin.
- Kural altyapısı [eşleştirme koşulları](front-door-rules-engine-match-conditions.md) hakkında daha fazla bilgi edinin
- AFD kural altyapısı [CLI başvurusuna](https://docs.microsoft.com/cli/azure/ext/front-door/network/front-door/rules-engine?view=azure-cli-latest)daha fazla göz atın. 
- AFD kural altyapısı [PowerShell başvurusuna](https://docs.microsoft.com/powershell/module/az.frontdoor/?view=azps-3.8.0)daha fazla göz atın. 
