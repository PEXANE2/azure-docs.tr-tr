---
title: 'Azure ExpressRoute: devre yapılandırma iş akışları'
description: Bu sayfada ExpressRoute devresine ve eşayarlarını yapılandırma iş akışları gösterilmektedir
services: expressroute
author: cherylmc
ms.service: expressroute
ms.topic: conceptual
ms.date: 09/18/2018
ms.author: cherylmc
ms.openlocfilehash: e833e20085d7cfd8f727acb394851e96e7e19368
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "75864375"
---
# <a name="expressroute-workflows-for-circuit-provisioning-and-circuit-states"></a>Devre sağlama ve devre durumları için ExpressRoute iş akışları
Bu sayfada, hizmet sağlama ve yapılandırma iş akışlarının yüksek düzeyde nasıl bulunduğu anlatılmaktadır.

![devre iş akışı](./media/expressroute-workflows/expressroute-circuit-workflow.png)

Aşağıdaki şekil ve ilgili adımlar, bir ExpressRoute bağlantı hattını uçtan uca sağlamak için görevleri özetler. 

1. ExpressRoute bağlantı hattını yapılandırmak için PowerShell 'i kullanın. Daha fazla ayrıntı için [ExpressRoute devreleri oluşturma](expressroute-howto-circuit-classic.md) makalesindeki yönergeleri izleyin.
2. Hizmet sağlayıcısından bağlantı siparişi. Bu işlem farklılık gösterir. Bağlantı siparişi alma hakkında daha fazla ayrıntı için bağlantı sağlayıcınızla iletişim kurun.
3. PowerShell aracılığıyla ExpressRoute bağlantı hattı sağlama durumunu doğrulayarak devre 'nın başarıyla sağlandığından emin olun. 
4. Yönlendirme etki alanlarını yapılandırın. Bağlantı sağlayıcınız katman 3 yapılandırmasını yönetirse, bu, devreniz için yönlendirmeyi yapılandıracaktır. Bağlantı sağlayıcınız yalnızca Katman 2 Hizmetleri sunuyorsa, yönlendirme [gereksinimleri](expressroute-routing.md) ve [Yönlendirme yapılandırması](expressroute-howto-routing-classic.md) sayfalarında açıklanan yönergeler temelinde yönlendirmeyi yapılandırmanız gerekir.
   
   * Azure özel eşlemesini etkinleştir-sanal ağlarda dağıtılan VM 'lere/bulut hizmetlerine bağlanmak için bu eşlemeyi etkinleştirin.

   * Microsoft eşlemesini etkinleştir-bunu, Office 365 gibi Microsoft çevrimiçi hizmetler erişmek için etkinleştirin. Tüm Azure PaaS hizmetlerine Microsoft eşlemesi üzerinden erişilebilir.
     
     > [!IMPORTANT]
     > Internet için kullandığınızdan Microsoft 'a bağlanmak için ayrı bir proxy/kenar kullandığınızdan emin olmanız gerekir. Hem ExpressRoute hem de Internet için aynı kenarı kullanmak asimetrik yönlendirmeye yol açabilir ve ağınız için bağlantı kesintilerine neden olur.
     > 
     > 
     
     ![Yönlendirme iş akışları](./media/expressroute-workflows/routing-workflow.png)
5. Sanal ağları ExpressRoute devresine bağlama-sanal ağları ExpressRoute bağlantı hattına bağlayabilirsiniz. [Devnetlerinizi bağlantı hattına bağlamak için](expressroute-howto-linkvnet-arm.md) yönergeleri izleyin. Bu sanal ağlar ExpressRoute devresi ile aynı Azure aboneliğinde olabilir veya farklı bir abonelikte olabilir.

## <a name="expressroute-circuit-provisioning-states"></a>ExpressRoute devre sağlama durumları
Her ExpressRoute devresi iki duruma sahiptir:

* Hizmet sağlayıcı sağlama durumu
* Durum

Durum Microsoft 'un sağlama durumunu temsil eder. Bu özellik, bir ExpressRoute bağlantı hattı oluşturduğunuzda etkin olarak ayarlanır

Bağlantı sağlayıcısı sağlama durumu, bağlantı sağlayıcısının tarafındaki durumu temsil eder. *Nottemin*edilebilir, *sağlanıyor*ya da *sağlanabilir*. ExpressRoute bağlantı hattı, eşlemeyi yapılandırmak için sağlanan bir durumda olmalıdır.

### <a name="possible-states-of-an-expressroute-circuit"></a>Bir ExpressRoute devresine ait olası durumlar
Bu bölümde, bir ExpressRoute devresine ait olası durumlar listelenmiştir.

**Oluşturulma zamanında**

ExpressRoute bağlantı hattı, kaynak oluşturma sırasında aşağıdaki durumları rapor eder.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


**Bağlantı sağlayıcısı, devre sağlama sürecinde olduğunda**

ExpressRoute bağlantı hattı, bağlantı sağlayıcı devreyi sağlamak için çalışırken aşağıdaki durumları bildirir.

    ServiceProviderProvisioningState : Provisioning
    Status                           : Enabled


**Bağlantı sağlayıcısı sağlama işlemini tamamladığında**

Bağlantı sağlayıcısı devresini başarıyla sağladıktan sonra ExpressRoute bağlantı hattı aşağıdaki durumları rapor eder.

    ServiceProviderProvisioningState : Provisioned
    Status                           : Enabled


**Bağlantı sağlayıcısı devre dışı bırakıldığında**

ExpressRoute bağlantı hattının sağlanması gerekiyorsa, hizmet sağlayıcısı sağlamayı kaldırma işlemini tamamladıktan sonra devre aşağıdaki durumları rapor eder.

    ServiceProviderProvisioningState : NotProvisioned
    Status                           : Enabled


Gerekirse yeniden etkinleştirmeyi seçebilir veya devre dışı bırakmak için PowerShell cmdlet 'lerini çalıştırabilirsiniz.  

> [!IMPORTANT]
> ServiceProviderProvisioningState sağlama veya sağlama olduğunda bir devre silinemez. Bağlantı sağlayıcısı silinmeden önce devre sağlamasını kaldırmalıdır. Microsoft, ExpressRoute bağlantı hattı kaynağı Azure 'da silinene kadar devreyi faturalandıralmaya devam edecektir.
> 

## <a name="routing-session-configuration-state"></a>Yönlendirme oturumu yapılandırma durumu
BGP sağlama durumu, BGP oturumunun Microsoft Edge üzerinde etkin olup olmadığını bildirir. Özel veya Microsoft eşlemesi kullanmak için durumun etkin olması gerekir.

BGP oturum durumunu özellikle Microsoft eşlemesi için denetlemek önemlidir. BGP sağlama durumuna ek olarak *tanıtılan genel ön ekler durumu*adlı başka bir durum da vardır. Tanıtılan genel ön eklerin durumu, hem BGP oturumunun açık olması hem de yönlendirmenin uçtan uca çalışması için *yapılandırılmış* durumda olmalıdır. 

Tanıtılan genel ön ek durumu *doğrulama gerekli* durumuna ayarlandıysa, tanıtılan ön ekler yönlendirme kayıt DEFTERLERININ hiçbirinde as NUMARASıYLA eşleşmediğinden BGP oturumu etkinleştirilmez. 

> [!IMPORTANT]
> Tanıtılan genel ön ekler *el ile doğrulama* durumundaysa, [Microsoft desteği](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) ile bir destek bileti açmanız ve ilgili otonom SISTEM numarasıyla birlikte tanıtılan IP adreslerinden sahip olduğunuz konusunda kanıt sağlamanız gerekir.
> 
> 

## <a name="next-steps"></a>Sonraki adımlar
* ExpressRoute bağlantınızı yapılandırın.
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir Sanal Ağ bağlama](expressroute-howto-linkvnet-arm.md)

