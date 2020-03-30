---
title: 'Azure ExpressRoute: Devre yapılandırma iş akışları'
description: Bu sayfa, ExpressRoute devresini ve eşlemeleri yapılandırmak için iş akışlarını gösterir
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: cherylmc
ms.openlocfilehash: e833e20085d7cfd8f727acb394851e96e7e19368
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75864375"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Devre sağlama ve devre durumları için ExpressRoute iş akışları
Bu sayfa, yüksek düzeyde hizmet sağlama ve yönlendirme yapılandırma iş akışları ile size yol gösterir.

![devre iş akışı](./media/expressroute-workflows/expressroute-circuit-workflow.png)

Aşağıdaki şekil ve ilgili adımlar, expressroute devresinin uçtan uca sağlanmasına ilişkin görevleri özetler. 

1. ExpressRoute devresini yapılandırmak için PowerShell'i kullanın. Daha fazla ayrıntı için [ExpressRoute devreleri Oluştur](expressroute-howto-circuit-classic.md) makalesindeki yönergeleri izleyin.
2. Servis sağlayıcıdan sipariş bağlantısı. Bu işlem değişir. Bağlantı siparişi verme hakkında daha fazla bilgi için bağlantı sağlayıcınıza başvurun.
3. PowerShell üzerinden ExpressRoute devre saðlama durumunu doğrulayarak devrenin baarýlý bir şekilde saðlanýrýldýðýndan emin olun. 
4. Yönlendirme etki alanlarını yapılandırın. Bağlantı sağlayıcınız Katman 3 yapılandırmayı yönetiyorsa, devreniz için yönlendirmeyi yapılandıracaktır. Bağlantı sağlayıcınız yalnızca Katman 2 hizmetleri sunuyorsa, [yönlendirme gereksinimlerinde](expressroute-routing.md) ve [yönlendirme yapılandırma](expressroute-howto-routing-classic.md) sayfalarında açıklanan yönergelere göre yönlendirmeyi yapılandırmanız gerekir.
   
   * Azure özel eşlemesini etkinleştirin - Sanal ağlarda dağıtılan VM/bulut hizmetlerine bağlanmak için bu eşlemeyi etkinleştirin.

   * Microsoft'un eşlemesini etkinleştirin - Office 365 gibi Microsoft çevrimiçi hizmetlerine erişmek için bunu etkinleştirin. Tüm Azure PaaS hizmetlerine Microsoft'un bakışları aracılığıyla erişilebilir.
     
     > [!IMPORTANT]
     > Microsoft'a bağlanmak için Internet için kullandığınız proxy'den daha ayrı bir proxy / kenar kullandığınızdan emin olmalısınız. Hem ExpressRoute hem de Internet için aynı kenarı kullanmak asimetrik yönlendirmeye neden olur ve ağınız için bağlantı kesintilerine neden olur.
     > 
     > 
     
     ![yönlendirme iş akışları](./media/expressroute-workflows/routing-workflow.png)
5. Sanal ağları ExpressRoute devrelerine bağlama - Sanal ağları ExpressRoute devrenize bağlayabilirsiniz. [VNets'i](expressroute-howto-linkvnet-arm.md) devrenize bağlamak için yönergeleri izleyin. Bu VNet'ler ExpressRoute devresi ile aynı Azure aboneliğinde olabilir veya farklı bir abonelikte olabilir.

## <a name="expressroute-circuit-provisioning-states"></a>ExpressRoute devresi sağlama durumları
Her ExpressRoute devresi iki özellenesahiptir:

* Hizmet sağlayıcı sağlama durumu
* Durum

Durum, Microsoft'un sağlama durumunu temsil eder. Bir Expressroute devresi oluşturduğunuzda bu özellik Etkin olarak ayarlanır

Bağlantı sağlayıcı sağlama durumu, bağlantı sağlayıcısının tarafındaki durumu temsil eder. Ya *NotProvisioned*olabilir , *Provizyon*, ya da *Hüküm .* ExpressRoute devresi, eşlemi yapılandırmak için Hükmedilen durumda olmalıdır.

### <a name="possible-states-of-an-expressroute-circuit"></a>Bir ExpressRoute devresinin olası durumları
Bu bölümde, Bir ExpressRoute devresinin olası durumları listelemektedir.

**Yaratılış zamanında**

ExpressRoute devresi kaynak oluşturma sırasında aşağıdaki durumları bildirir.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Bağlantı sağlayıcısı devreyi sağlama sürecindeyken**

Bağlantı sağlayıcısı devreyi sağlamak için çalışırken ExpressRoute devresi aşağıdaki durumları bildirir.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Bağlantı sağlayıcısı sağlama işlemini tamamladığında**

ExpressRoute devresi, bağlantı sağlayıcısı devreyi başarıyla sağladıktan sonra aşağıdaki durumları raporedecektir.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


**Bağlantı sağlayıcısı devreyi devreyi devreyi devreyi devre yitiriyorken**

ExpressRoute devresinin iptal edilmesi gerekiyorsa, servis sağlayıcı sağlama işlemini tamamladıktan sonra devre aşağıdaki durumları bildirir.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Gerekirse yeniden etkinleştirmeyi seçebilir veya devreyi silmek için PowerShell cmdlets çalıştırabilirsiniz.  

> [!IMPORTANT]
> ServiceProviderProvisioningState Hüküm veya Provizyon alırken bir devre silinemez. Bağlantı sağlayıcısının silinebilmesi için devreyi devreyi devreyi devreyi çözmesi gerekir. Microsoft, ExpressRoute devre kaynağı Azure'da silinene kadar devreyi faturalandırmaya devam edecektir.
> 

## <a name="routing-session-configuration-state"></a>Yönlendirme oturumu yapılandırma durumu
BGP oturumu Microsoft kenarında etkinleştirilmişse BGP sağlama durumu raporları. Özel veya Microsoft eşleme kullanmak için durum etkinleştirilmelidir.

Özellikle Microsoft'un bakışları için BGP oturum durumunu denetlemek önemlidir. BGP sağlama durumuna ek olarak, *reklamı yapılan genel önekleri durumu*olarak adlandırılan başka bir durum daha vardır. Reklamı yapılan genel öneekler durumu, hem BGP oturumunun açık olması hem de yönlendirmenizin uçtan uca çalışması için *yapılandırılan* durumda olmalıdır. 

Reklamı yapılan genel önek durumu gerekli bir *doğrulama* durumuna ayarlanmışsa, reklamı yapılan öneekler yönlendirme kayıt larından herhangi birinde AS numarasıyla eşleşmediği için BGP oturumu etkinleştirilmez. 

> [!IMPORTANT]
> Reklamı yapılan genel öneekler durumu *el ile doğrulama* durumundaysa, [Microsoft desteğiyle](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) bir destek bileti açmanız ve ilişkili Özerk Sistem numarasıyla birlikte reklamı yapılan IP adreslerine sahip olduğunuza dair kanıt sağlamanız gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute bağlantınızı yapılandırın.
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)

