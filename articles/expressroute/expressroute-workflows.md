---
title: 'Azure ExpressRoute: devre yapılandırma iş akışları'
description: Bu sayfada, ExpressRoute bağlantı hattı ve eşlemeleri yapılandırmak için iş akışlarını gösterilir.
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: cherylmc
ms.openlocfilehash: e833e20085d7cfd8f727acb394851e96e7e19368
ms.sourcegitcommit: 12a26f6682bfd1e264268b5d866547358728cd9a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/10/2020
ms.locfileid: "75864375"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Devre hazırlama ve devre durumları için ExpressRoute iş akışları
Bu sayfa sağlama ve yüksek düzeyde yapılandırma iş akışlarını yönlendirme hizmeti size kılavuzluk eder.

![bağlantı hattı iş akışı](./media/expressroute-workflows/expressroute-circuit-workflow.png)

Aşağıdaki şekil ve ilgili adımlar, bir ExpressRoute bağlantı hattını uçtan uca sağlamak için görevleri özetler. 

1. Bir ExpressRoute bağlantı hattı yapılandırmak için PowerShell kullanın. Bölümündeki yönergeleri [oluşturma ExpressRoute bağlantı hatları](expressroute-howto-circuit-classic.md) makale daha fazla ayrıntı için.
2. Bağlantı, hizmet sağlayıcısından sipariş. Bu işlem değişir. Bağlantı sipariş hakkında daha fazla ayrıntı için bağlantı sağlayıcınıza başvurun.
3. Devre başarıyla sağlama durumu PowerShell aracılığıyla bir ExpressRoute bağlantı hattı doğrulayarak sağlandığından emin olun. 
4. Yönlendirme etki alanları yapılandırın. Bağlantı sağlayıcınız katman 3 yapılandırmasını yönetirse, bu, devreniz için yönlendirmeyi yapılandıracaktır. Bağlantı sağlayıcınız yalnızca Katman 2 Hizmetleri sunuyorsa, yönlendirme [gereksinimleri](expressroute-routing.md) ve [Yönlendirme yapılandırması](expressroute-howto-routing-classic.md) sayfalarında açıklanan yönergeler temelinde yönlendirmeyi yapılandırmanız gerekir.
   
   * Azure özel eşlemeyi etkinleştirmesini - bu eşlemeyi bulut Hizmetleri içindeki sanal ağlar dağıtılan Vm'lere bağlanmak etkinleştirin.

   * Microsoft eşlemesini etkinleştir-bunu, Office 365 gibi Microsoft çevrimiçi hizmetler erişmek için etkinleştirin. Tüm Azure PaaS hizmetlerine Microsoft eşlemesi üzerinden erişilebilir.
     
     > [!IMPORTANT]
     > Ayrı bir ara sunucu kullan / hesaptan Microsoft'a bağlanmak için kenar kullanmak için Internet sağlamalısınız. ExpressRoute ve Internet için aynı edge kullanarak asimetrik yönlendirme neden ve ağınızın bağlantı kesintilerine neden olur.
     > 
     > 
     
     ![Yönlendirme iş akışları](./media/expressroute-workflows/routing-workflow.png)
5. Sanal ağları bağlama ExpressRoute devresine - sanal ağları ExpressRoute bağlantı hattına bağlayabilirsiniz. Yönergeleri izleyerek [sanal ağları bağlamak için](expressroute-howto-linkvnet-arm.md) bağlantı hattınız için. Bu sanal ağlar aynı abonelikte Azure ExpressRoute bağlantı hattı olabilir veya farklı bir abonelikte olabilir.

## <a name="expressroute-circuit-provisioning-states"></a>ExpressRoute bağlantı hattı sağlama durumları
Her bir ExpressRoute bağlantı hattında iki durum vardır:

* Hizmet Sağlayıcısı sağlama durumu
* Durum

Durum Microsoft'un sağlama durumu temsil eder. Bir Expressroute bağlantı hattı oluşturduğunuzda, bu özellik etkin olarak ayarlanır

Bağlantı Sağlayıcısı sağlama durumu bağlantı sağlayıcısı tarafı durumunu temsil eder. Ya da olabilir *NotProvisioned*, *sağlama*, veya *sağlanan*. ExpressRoute bağlantı hattı, eşlemeyi yapılandırmak için sağlanan bir durumda olmalıdır.

### <a name="possible-states-of-an-expressroute-circuit"></a>Olası bir ExpressRoute bağlantı hattı durumları
Bu bölümde, bir ExpressRoute devresine ait olası durumlar listelenmiştir.

**Oluşturma zamanında**

ExpressRoute bağlantı hattı, kaynak oluşturma sırasında aşağıdaki durumları rapor eder.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Bağlantı sağlayıcısı devre sağlama sürecinde olduğunda**

ExpressRoute bağlantı hattı, bağlantı sağlayıcı devreyi sağlamak için çalışırken aşağıdaki durumları bildirir.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Bağlantı Sağlayıcısı sağlama işlemi tamamlandığında**

Bağlantı sağlayıcısı devresini başarıyla sağladıktan sonra ExpressRoute bağlantı hattı aşağıdaki durumları rapor eder.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


**Ne zaman bağlantı sağlayıcısı devreyi sağlamayı kaldırma**

ExpressRoute bağlantı hattının sağlanması gerekiyorsa, hizmet sağlayıcısı sağlamayı kaldırma işlemini tamamladıktan sonra devre aşağıdaki durumları rapor eder.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Gerekli ya da bağlantı hattını silmek için PowerShell cmdlet'lerini çalıştırmak yeniden etkinleştirmeyi seçebilirsiniz.  

> [!IMPORTANT]
> ServiceProviderProvisioningState sağlama veya sağlama olduğunda bir devre silinemez. Bağlantı sağlayıcısı silinmeden önce devre sağlamasını kaldırmalıdır. Microsoft, ExpressRoute bağlantı hattı kaynağı Azure 'da silinene kadar devreyi faturalandıralmaya devam edecektir.
> 

## <a name="routing-session-configuration-state"></a>Yönlendirme oturum yapılandırma durumu
BGP sağlama durumu, BGP oturumunun Microsoft Edge üzerinde etkin olup olmadığını bildirir. Özel veya Microsoft eşlemesi kullanmak için durumun etkin olması gerekir.

Özellikle de Microsoft eşlemesi için BGP oturumu durumunu denetlemek önemlidir. Sağlama durumu BGP ek olarak, adlı başka bir durum yoktur *tanıtılan genel önekler durumu*. Tanıtılan genel ön eklerin durumu, hem BGP oturumunun açık olması hem de yönlendirmenin uçtan uca çalışması için *yapılandırılmış* durumda olmalıdır. 

Tanıtılan genel ön eki durumu ayarlanırsa bir *gerekli doğrulama* durumunda, BGP oturumu etkin değil, tanıtılan önekler herhangi bir kayıt defterlerini yönlendirme AS numarasını eşleşmediğinden. 

> [!IMPORTANT]
> Tanıtılan genel ön ekler *el ile doğrulama* durumundaysa, [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ile bir destek bileti açmanız ve ilgili otonom SISTEM numarasıyla birlikte tanıtılan IP adreslerinden sahip olduğunuz konusunda kanıt sağlamanız gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute bağlantınızı yapılandırın.
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)

