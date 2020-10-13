---
title: 'Öğretici: Azure ön kapısı ve Azure Web uygulaması güvenlik duvarı (WAF) kullanarak bir Web uygulamasını ölçeklendirme ve koruma'
description: Bu öğretici, Azure Web uygulaması güvenlik duvarı 'nı Azure ön kapılı hizmeti ile nasıl kullanacağınızı gösterecektir.
services: frontdoor
documentationcenter: ''
author: duongau
ms.service: frontdoor
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 10/01/2020
ms.author: duau
ms.openlocfilehash: 7c5e938f985296e0534ca6e2438cf3acedb0fb65
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91626488"
---
# <a name="tutorial-quickly-scale-and-protect-a-web-application-by-using-azure-front-door-and-azure-web-application-firewall-waf"></a>Öğretici: Azure ön kapısı ve Azure Web uygulaması güvenlik duvarı (WAF) kullanarak bir Web uygulamasını hızla ölçeklendirme ve koruma

Birçok Web uygulaması, COVıD-19 nedeniyle son haftalarda trafik hızlı bir şekilde artışını yaşadı. Bu Web uygulamaları, hizmet reddi saldırıları da dahil olmak üzere kötü amaçlı trafikte bir aşırı gerilim yaşıyor. Hem uygulama hem de uygulamalarınızı saldırılara karşı korumak için etkili bir yöntem vardır: Web uygulamanızın önünde hızlandırma, önbelleğe alma ve güvenlik katmanı olarak Azure WAF ile Azure ön kapısını yapılandırma. Bu makalede, Azure 'un içinde veya dışında çalışan herhangi bir Web uygulaması için yapılandırılmış Azure WAF ile Azure ön kapısının nasıl alınacağı hakkında rehberlik sunulmaktadır. 

Bu öğreticide WAF 'yi yapılandırmak için Azure CLı kullanacağız. Azure portal, Azure PowerShell, Azure Resource Manager veya Azure REST API 'Lerini kullanarak aynı şeyi gerçekleştirebilirsiniz. 

Bu öğreticide aşağıdakilerin nasıl yapılacağını öğreneceksiniz:
> [!div class="checklist"]
> - Ön kapı oluşturun.
> - Azure WAF ilkesi oluşturun.
> - Bir WAF ilkesi için kural kümelerini yapılandırın.
> - Bir WAF ilkesini ön kapıya ilişkilendirin.
> - Özel bir etki alanı yapılandırın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prerequisites"></a>Ön koşullar

- Bu öğreticideki yönergeler, Azure CLı 'yi kullanır. Azure CLı 'yı kullanmaya başlamak için [Bu Kılavuzu görüntüleyin](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli?view=azure-cli-latest&preserve-view=true) .

  > [!TIP] 
  > Azure CLı 'yı kullanmaya başlamak için kolay ve hızlı bir yol [Azure Cloud Shell](https://docs.microsoft.com/azure/cloud-shell/quickstart).

- `front-door`Uzantının Azure CLI 'ye eklendiğinden emin olun:

   ```azurecli-interactive 
   az extension add --name front-door
   ```

> [!NOTE] 
> Bu öğreticide kullanılan komutlar hakkında daha fazla bilgi için bkz. [ön kapı Için Azure CLI başvurusu](https://docs.microsoft.com/cli/azure/ext/front-door/?view=azure-cli-latest&preserve-view=true).

## <a name="create-an-azure-front-door-resource"></a>Azure ön kapı kaynağı oluşturma

```azurecli-interactive 
az network front-door create --backend-address <>  --accepted-protocols <> --name <> --resource-group <>
```

`--backend-address`: Korumak istediğiniz uygulamanın tam etki alanı adı (FQDN). Örneğin, `myapplication.contoso.com`.

`--accepted-protocols`: Azure ön kapısının Web uygulamanız için desteklemesini istediğiniz protokolleri belirtir. Örneğin, `--accepted-protocols Http Https`.

`--name`: Azure ön kapısının adı.

`--resource-group`: Bu Azure ön kapısı kaynağını yerleştirmek istediğiniz kaynak grubu. Kaynak grupları hakkında daha fazla bilgi edinmek için bkz. [Azure 'da kaynak gruplarını yönetme](https://docs.microsoft.com/azure/azure-resource-manager/management/manage-resource-groups-portal).

Bu komutu çalıştırdığınızda aldığınız yanıtta, anahtarı bulun `hostName` . Daha sonraki bir adımda bu değere ihtiyacınız olacaktır. , `hostName` Oluşturduğunuz Azure ön kapısının DNS adıdır.

## <a name="create-an-azure-waf-profile-to-use-with-azure-front-door-resources"></a>Azure ön kapı kaynakları ile kullanmak için bir Azure WAF profili oluşturma

```azurecli-interactive 
az network front-door waf-policy create --name <>  --resource-group <>  --disabled false --mode Prevention
```

`--name`: Yeni Azure WAF ilkesinin adı.

`--resource-group`: Bu WAF kaynağını yerleştirmek istediğiniz kaynak grubu. 

Önceki CLı kodu, etkin olan ve önleme modundaki bir WAF ilkesi oluşturacak. 

> [!NOTE] 
> Algılama modunda WAF ilkesini oluşturmak ve koruma modunu kullanmaya karar vermeden önce kötü amaçlı istekleri nasıl algılayıp günlüğe kaydettiğini (bunları engellemeden) gözlemlemek isteyebilirsiniz.

Bu komutu çalıştırdığınızda aldığınız yanıtta, anahtarı bulun `ID` . Daha sonraki bir adımda bu değere ihtiyacınız olacaktır. 

`ID`Alan şu biçimde olmalıdır:

/Subscriptions/**abonelik kimliği**/ResourceGroups/**kaynak grubu adı**/Providers/Microsoft.Network/frontdoorwebapplicationfirewallpolicies/**WAF ilke adı**

## <a name="add-managed-rule-sets-to-the-waf-policy"></a>WAF ilkesine yönetilen kural kümeleri ekleme

Bir WAF ilkesine yönetilen kural kümeleri ekleyebilirsiniz. Yönetilen bir kural kümesi, Microsoft tarafından oluşturulan ve yönetilen ve bir tehdit sınıfına karşı korunmanıza yardımcı olan kuralların bir kümesidir. Bu örnekte, iki kural kümesi ekliyoruz:
- Genel Web tehditlerine karşı korunmanıza yardımcı olan varsayılan kural kümesi. 
- Kötü amaçlı botlara karşı korunmanıza yardımcı olan bot koruma kuralı kümesi.

Varsayılan kural kümesini ekleyin:

   ```azurecli-interactive 
   az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type DefaultRuleSet --version 1.0
   ```

Bot koruma kuralı kümesini ekleyin:

   ```azurecli-interactive 
   az network front-door waf-policy managed-rules add --policy-name <> --resource-group <> --type Microsoft_BotManagerRuleSet --version 1.0
   ```

`--policy-name`: Azure WAF kaynağınız için belirttiğiniz ad.

`--resource-group`: WAF kaynağını yerleştirdiğiniz kaynak grubu.

## <a name="associate-the-waf-policy-with-the-azure-front-door-resource"></a>WAF ilkesini Azure ön kapı kaynağıyla ilişkilendir

Bu adımda, oluşturduğumuz WAF ilkesini, Web uygulamanızın önünde bulunan Azure ön kapı kaynağıyla ilişkilendireceğiz:

```azurecli-interactive 
az network front-door update --name <> --resource-group <> --set frontendEndpoints[0].webApplicationFirewallPolicyLink='{"id":"<>"}'
```

`--name`: Azure ön kapısının kaynağı için belirttiğiniz addır.

`--resource-group`: Azure ön kapısı kaynağını yerleştirdiğiniz kaynak grubu.

`--set`: `WebApplicationFirewallPolicyLink` `frontendEndpoint` Yeni WAF Ilkesiyle Azure ön kapısının bulunduğu ilişkili için özniteliği güncelleştireceğiniz yerdir. Bu öğreticide daha önce WAF profilini oluştururken aldığınız yanıttan WAF ilkesinin KIMLIĞINE sahip olmanız gerekir.

 > [!NOTE] 
> Önceki örnek, özel bir etki alanı kullanmadığınız durumlarda geçerlidir. Web uygulamalarınıza erişmek için özel etki alanı kullanmıyorsanız, sonraki bölümü atlayabilirsiniz. Bu durumda, müşterilerinize `hostName` Azure ön kapısının kaynağını oluştururken elde edersiniz. `hostName`Web uygulamanıza gitmek için bunu kullanacağız.

## <a name="configure-the-custom-domain-for-your-web-application"></a>Web uygulamanız için özel etki alanını yapılandırma

Web uygulamanızın özel etki alanı adı, müşterilerin uygulamanıza başvurmak için kullandığı bir addır. Örneğin, www.contoso.com. Başlangıçta, bu özel etki alanı adı, Azure ön kapısına sunulmadan önce çalıştığı konuma işaret ediyor. Uygulamanın önüne Azure ön kapısı ve WAF ekledikten sonra, bu özel etki alanına karşılık gelen DNS girişi, Azure ön kapısına işaret etmelidir. Bu değişikliği, DNS sunucunuzdaki girişi, `hostName` Azure ön kapısının oluşturulduğu sırada not ettiğiniz Azure ön kapısına yeniden tanımlayarak yapabilirsiniz.

DNS kayıtlarınızı güncelleştirmek için özel adımlar, DNS hizmet sağlayıcınıza göre değişir. DNS adınızı barındırmak için Azure DNS kullanırsanız, [DNS kaydını güncelleştirme](https://docs.microsoft.com/azure/dns/dns-operations-recordsets-cli) ve Azure ön kapısına işaret eden adımlar için belgelere başvurabilirsiniz `hostName` . 

Müşterilerinizin bölge tepesinde kullanarak Web sitenize (örneğin, contoso.com) sahip olup olmadığına dikkat etmeniz önemli bir şeydir. Bu durumda, DNS adınızı barındırmak için Azure DNS ve [diğer ad kayıt türünü](https://docs.microsoft.com/azure/dns/dns-alias) kullanmanız gerekir. 

Ayrıca, bu eşlemenin farkında olması için [özel etki alanını eklemek](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain) üzere Azure ön kapı yapılandırmanızı güncelleştirmeniz gerekir.

Son olarak, Web uygulamanıza erişmek için özel bir etki alanı kullanıyorsanız ve HTTPS protokolünü etkinleştirmek istiyorsanız. [Özel etki alanınız için sertifikaları Azure ön kapısına](https://docs.microsoft.com/azure/frontdoor/front-door-custom-domain-https)göre belirlemeniz gerekir. 

## <a name="lock-down-your-web-application"></a>Web uygulamanızı kilitleme

Yalnızca Azure ön kapısının Web uygulamanızla iletişim kurabildiğinden emin olmanızı öneririz. Bunun yapılması, hiçbir birinin Azure ön kapı korumasını atlayamayacağını ve uygulamanıza doğrudan erişmesini sağlamaya devam edebilir. Bu kilidi başarmak için, [arka ucumun erişimini yalnızca Azure ön kapısına nasıl yaparım?](https://docs.microsoft.com/azure/frontdoor/front-door-faq#how-do-i-lock-down-the-access-to-my-backend-to-only-azure-front-door).

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide kullanılan kaynaklara artık ihtiyacınız kalmadığında, [az Group Delete](https://docs.microsoft.com/cli/azure/group?view=azure-cli-latest#az-group-delete&preserve-view=true) komutunu kullanarak kaynak grubunu, ön kapıyı ve WAF ilkesini kaldırın:

```azurecli-interactive
  az group delete \
    --name <>
```
`--name`: Bu öğreticide kullanılan tüm kaynaklar için kaynak grubunun adı.

## <a name="next-steps"></a>Sonraki adımlar

Ön kapılarınızın sorunlarını giderme hakkında bilgi edinmek için bkz. sorun giderme kılavuzu:

> [!div class="nextstepaction"]
> [Yaygın yönlendirme sorunlarını giderme](front-door-troubleshoot-routing.md)
