---
title: Azure ön kapısı ve Azure Web uygulaması güvenlik duvarı (WAF) kullanarak bir Web uygulamasını hızlıca ölçeklendirin ve koruyun | Microsoft Docs
description: Bu öğreticide, Azure ön kapılı hizmetinizdeki Web uygulaması güvenlik duvarı 'nı nasıl kullanacağınız anlatılmaktadır
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 09/14/2020
ms.author: duau
ms.openlocfilehash: 1958481193b66c8cec2cb6a1ac6648a6900d70ac
ms.sourcegitcommit: 03662d76a816e98cfc85462cbe9705f6890ed638
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/15/2020
ms.locfileid: "90531211"
---
# <a name="tutorial-quickly-scale-and-protect-a-web-application-using-azure-front-door-and-azure-web-application-firewall-waf"></a>Öğretici: Azure ön kapısı ve Azure Web uygulaması güvenlik duvarı (WAF) kullanarak bir Web uygulamasını hızla ölçeklendirme ve koruma

Birçok Web uygulaması, COVıD-19 ile ilgili son haftalarda hızlı bir şekilde trafik artışı yaşadı. Ayrıca, bu Web uygulamaları, hizmet reddi saldırıları da dahil olmak üzere kötü amaçlı trafikte bir aşırı gerilim gözlemleyerek. Bu ihtiyaçları her ikisi de işlemek için etkili bir yöntem olan trafik dalgalanmaları ve saldırılardan korunmak için, Web uygulamanızın önünde bir hızlandırma, önbelleğe alma ve güvenlik katmanı olarak Azure WAF ile Azure ön kapısı ayarlamaya yöneliktir. Bu makalede, Azure 'da veya Azure dışında çalışan Web uygulamaları için Azure WAF kurulumu ile bu Azure ön kapısının nasıl hızlı bir şekilde alınacağı hakkında rehberlik sunulmaktadır. 

Bu öğreticide WAF 'yi ayarlamak için Azure CLı kullanacağız, ancak tüm bu adımlar Azure portal, Azure PowerShell, Azure ARM ve Azure REST API 'Lerinde de tam olarak desteklenmektedir. 

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - Ön kapı oluşturun.
> - Azure WAF ilkesi oluşturun.
> - WAF ilkesi için RuleSets 'i yapılandırın.
> - WAF ilkesini ön kapıya ilişkilendir
> - Özel etki alanı yapılandırma

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Önkoşullar

Bu blogdaki yönergeler Azure komut satırı arabirimini (CLı) kullanır. [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest)kullanmaya başlamak için bu Kılavuzu görüntüleyin.

*İpucu: Azure CLı 'yı kullanmaya başlamak için kolay bir & hızlı yolu, [Bash ile Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart)*

Ön kapı uzantısının Azure CLı 'nize eklendiğinden emin olun

```azurecli-interactive 
az extension add --name front-door
```

Note: aşağıda listelenen komutlara ilişkin daha fazla ayrıntı Için, [ön kapıda Azure CLI başvurusuna](https://docs.microsoft.com/cli/azure/ext/front-door/?view=azure-cli-latest)bakın.

## <a name="create-an-azure-front-door-afd-resource"></a>Azure ön kapısı (AFD) kaynağı oluşturma

```azurecli-interactive 
az network front-door create --backend-address <>  --accepted-protocols <> --name <> --resource-group <>
```

**--arka uç-adres**: arka uç adresi, korumak Istediğiniz uygulamanın tam etki alanı adı (FQDN) adıdır. Örneğin, myapplication.contoso.com

**--kabul edilen protokoller**: kabul edilen protokoller, AFD 'ın Web uygulamanız için hangi protokollerin desteklemesini istediğinizi belirtir. Örnek,--kabul edilen protokoller HTTP HTTPS olur.

**--Name**: AFD kaynağınız için bir ad belirtin

**--Resource-Group**: Bu AFD kaynağını yerleştirmek istediğiniz kaynak grubu.  Kaynak grupları hakkında daha fazla bilgi edinmek için bkz. Azure 'da kaynak gruplarını yönetme

Bu komutu başarılı bir şekilde yürüttükten sonra, "hostName" anahtarını arayın ve daha sonraki bir adımda kullanılacak değerini aklınızda bulun. Ana bilgisayar adı, oluşturduğunuz AFD kaynağının DNS adıdır

## <a name="create-an-azure-waf-profile-to-use-with-azure-front-door-resources"></a>Azure ön kapı kaynakları ile kullanmak için bir Azure WAF profili oluşturma

```azurecli-interactive 
az network front-door waf-policy create --name <>  --resource-group <>  --disabled false --mode Prevention
```

--AD Azure WAF ilkeniz için bir ad belirtin

--Bu WAF kaynağını yerleştirmek istediğiniz kaynak grubunu kaynak grubuna gruplayın. 

Yukarıdaki CLı kodu, etkin olan ve önleme modunda olan bir WAF ilkesi oluşturacak. 

Not: WAF 'yi algılama modunda oluşturmak ve koruma moduna değiştirmeye karar vermeden önce kötü amaçlı istekleri (engellenmeyen) & nasıl algılayacağını gözlemlemek isteyebilirsiniz.

Bu komutu başarılı bir şekilde yürüttükten sonra, "KIMLIK" anahtarını arayın ve daha sonraki bir adımda kullanılacak değerini aklınızda bulun. KIMLIK alanı şu biçimde olmalıdır

/Subscriptions/**abonelik kimliği**/ResourceGroups/**kaynak grubu adı**/Providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/**WAF ilke adı**

## <a name="add-managed-rulesets-to-this-waf-policy"></a>Bu WAF ilkesine yönetilen RuleSets ekleme

Bir WAF ilkesinde, Microsoft tarafından oluşturulan ve yönetilen bir kural kümesi olan yönetilen RuleSets 'ler ekleyebilirsiniz ve tüm tehdit sınıflarının tamamında kullanıma hazır bir koruma sunar. Bu örnekte, yaygın web tehditleri ve (2) botları koruma kuralı kümesi ile korunan ve kötü amaçlı botlara karşı koruyan iki tür RuleSets (1) varsayılan RuleSet kümesi ekliyoruz

(1) varsayılan RuleSet 'i ekleme

```azurecli-interactive 
az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type DefaultRuleSet --version 1.0
```

(2) bot Manager RuleSet 'i ekleme

```azurecli-interactive 
az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type Microsoft_BotManagerRuleSet --version 1.0
```

--Policy-Azure WAF kaynağınız için verdiğiniz adı adlandırın

--kaynak grubu bu WAF kaynağını yerleştirdiğiniz kaynak grubunu gruplayın.

## <a name="associate-the-waf-policy-with-the-afd-resource"></a>WAF ilkesini AFD kaynağıyla ilişkilendir

Bu adımda, geliştirdiğimiz WAF ilkesini Web uygulamanızın önünde bulunan AFD kaynağıyla ilişkilendireceğiz.

```azurecli-interactive 
az network front-door update --name <> --resource-group <> --set frontendEndpoints[0].webApplicationFirewallPolicyLink='{"id":"<>"}'
```

--AFD kaynağınız için belirttiğiniz adı adlandırın

--kaynak grubu Azure ön kapısının kaynağını yerleştirdiğiniz kaynak grubunu gruplayın.

--Bunu, yeni oluşturulan WAF ilkesi ile AFD kaynağınız ile ilişkilendirilen ön ek için WebApplicationFirewallPolicyLink özniteliğini güncelleştireceğiniz yerdir. WAF ilkesinin KIMLIĞI, yukarıdaki adım #2 aldığınız yanıttan bulunabilir

Not: Yukarıdaki örnek, bir özel etki alanı kullanmadığınız durumlar için

Web uygulamalarınıza erişmek için özel etki alanı kullanmıyorsanız, #5 adımı atlayabilirsiniz. Bu durumda, son kullanıcılarınıza Web uygulamanıza gitmek için #1 adımında elde ettiğiniz ana bilgisayar adını sağlayacaksınız.

## <a name="configure-custom-domain-for-your-web-application"></a>Web uygulamanız için özel etki alanı yapılandırma

Başlangıçta Web uygulamanızın özel etki alanı adı (örneğin, www.contoso.com), AFD tanıtılmadan önce çalıştırmak istediğiniz yere doğru işaret ediyor. Bu mimarinin uygulamanın önüne AFD + WAF ekleme mimarisi değiştirildikten sonra, bu özel etki alanına karşılık gelen DNS girişi artık bu AFD kaynağını işaret etmelidir. Bu işlem, DNS sunucunuzdaki bu girişin #1 adımında not ettiğiniz AFD ana bilgisayar adıyla yeniden eşleştirerek yapılabilir.

DNS kayıtlarınızı güncelleştirmek için özel adımlar, DNS hizmet sağlayıcınıza bağlı olacaktır, ancak DNS adınızı barındırmak için Azure DNS kullanıyorsanız, [adımlar için BIR DNS kaydını güncelleştirme](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-cli) ve AFD ana bilgisayar adına işaret eden yönergeler için belgelere başvurabilirsiniz. 

Burada, kullanıcılarınızın bölge tepesinde kullanarak Web sitenize gitmesini istiyorsanız (örneğin, contoso.com), DNS adınızı barındırmak için Azure DNS ve [diğer ad kayıt türünü](https://docs.microsoft.com/azure/dns/dns-alias) kullanmanız gerekir. 

Ayrıca, AFD bu eşlemeyi anlaması için [Bu özel etki alanını eklemek](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain) üzere AFD yapılandırmanızı de güncelleştirmeniz gerekir.

Son olarak, Web uygulamanıza erişmek için özel bir etki alanı kullanıyorsanız ve HTTPS protokolünü etkinleştirmek istiyorsanız, [AFD içindeki özel etki alanı kurulumunuzu sertifikalara](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain-https)sahip olmanız gerekir. 

## <a name="lock-down-your-web-application"></a>Web uygulamanızı kilitleme

En isteğe bağlı bir en iyi uygulama, Web uygulamanızla yalnızca AFD kenarlarının iletişim kurabildiğinden emin sağlamaktır. Bu eylem, hiçbir birinin AFD korumalarını atlayamayacak ve uygulamalarınıza doğrudan erişemeyeceğini sağlayacaktır. Bu kilidi, [AFD 'ın SSS bölümünü](https://docs.microsoft.com/azure/frontdoor/front-door-faq) ziyaret ederek ve arka uçlarını yalnızca AFD tarafından erişim için kilitleme hakkındaki soruya başvurarak gerçekleştirebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticideki kaynaklara artık ihtiyacınız kalmadığında, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete) komutunu kullanarak kaynak grubunu, ön kapıyı ve WAF ilkesini kaldırın.

```azurecli-interactive
  az group delete \
    --name <>
```
--Bu öğreticide dağıtılan tüm kaynaklar için kaynak grubu adını adlandırın.

## <a name="next-steps"></a>Sonraki Adımlar

Ön kapılarınızın sorunlarını nasıl giderebileceğinizi öğrenmek için nasıl yapılır kılavuzlarıyla devam edin.

> [!div class="nextstepaction"]
> [Yaygın yönlendirme sorunlarını giderme](front-door-troubleshoot-routing.md)
