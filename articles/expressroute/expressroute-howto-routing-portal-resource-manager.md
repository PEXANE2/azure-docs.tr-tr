---
title: 'Azure ExpressRoute: Eşlemeyi yapılandırma'
description: Bu makalede, ExpressRoute özel ve Microsoft eşleme oluşturma ve sağlama adımları belgeledir. Bu makalede, bir devreiçin durum denetimi, güncelleştirme veya silmek için nasıl gösterir.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: mialdrid
ms.openlocfilehash: 18d2db18e9880028c60b4b545c3628f4a9cb4703
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79264797"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Bir ExpressRoute bağlantı hattı için eşlemeyi oluşturma ve değiştirme

Bu makale, Azure portalını kullanarak bir Azure Kaynak Yöneticisi (ARM) ExpressRoute devresi için yönlendirme yapılandırması oluşturmanıza ve yönetmenize yardımcı olur. Ayrıca, expressroute devresi için durum durumunu, güncelleştirmeyi veya silme ve deprovision eşlemelerini de denetleyebilirsiniz. Devrenizle çalışmak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden bir makale seçin:

> [!div class="op_single_selector"]
> * [Azure portalında](expressroute-howto-routing-portal-resource-manager.md)
> * [Powershell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Ortak eşleme](about-public-peering.md)
> * [Video - Özel akran](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video - Microsoft'a bakış](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

ExpressRoute devresi için özel eşleme ve Microsoft eşleme yapılandırabilirsiniz (Azure genel eşleme yeni devreler için amortismana hazırdır). Eşlemeler seçtiğiniz herhangi bir sırada yapılandırılabilir. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. Yönlendirme etki alanları ve eşlemeler hakkında daha fazla bilgi için [ExpressRoute yönlendirme etki alanları](expressroute-circuit-peerings.md)bakın. Genel bakış hakkında bilgi için [ExpressRoute genel bakış'](about-public-peering.md)a bakın.

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Peering(ler) yapılandırması için, ExpressRoute devresinin etkin ve etkin bir durumda olması gerekir. 
* Paylaşılan bir anahtar/MD5 karmakullanmayı planlıyorsanız, bunu tünelin her iki tarafında da kullandığınızdan emin olun ve alfasayısal karakter sayısını en fazla 25 ile sınırlayın. Özel karakterler desteklenmez. 

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen Katman 3 hizmetleri (genellikle MPLS gibi bir IPVPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi sizin için yapılandırır ve yönetir. 

> [!IMPORTANT]
> Şu anda hizmet yönetim portalı aracılığıyla hizmet sağlayıcılar tarafından yapılandırılan eşlemeleri tanıtmıyoruz. Bu özelliği yakında etkinleştirmek için çalışıyoruz. BGP eşlemelerini yapılandırmadan önce servis sağlayıcınıza danışın.
> 
> 

## <a name="microsoft-peering"></a><a name="msft"></a>Microsoft'a bakış

Bu bölüm, ExpressRoute devresi için Microsoft eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmenize yardımcı olur.

> [!IMPORTANT]
> 1 Ağustos 2017 tarihinden önce yapılandırılan ExpressRoute devrelerinin Microsoft'a bakması, rota filtreleri tanımlanmamış olsa bile, Microsoft'un bakışları aracılığıyla duyurulan tüm hizmet önekleri olacaktır. Microsoft' un 1 Ağustos 2017 tarihinde veya sonrasında yapılandırılan ExpressRoute devrelerine bakması, devreye bir rota filtresi iliştirilene kadar herhangi bir önek reklamı yapmayacaktır. Daha fazla bilgi için bkz: [Microsoft eşlenetrasyon için bir rota filtresi yapılandırma.](how-to-routefilter-powershell.md)
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Daha fazla devam etmeden önce devrenin bağlantı sağlayıcısı tarafından tam olarak sağlandığından emin olmak için **Sağlayıcı durumunu** kontrol edin.

   Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Microsoft'un sizin için bakmasını etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız sizin için yönlendirmeyi yönetmezse, devrenizi oluşturduktan sonra aşağıdaki adımları ile devam edin.

   **Devre - Sağlayıcı durumu: Sağlanmadı**

    [![](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m.png "Provider status: Not provisioned")](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m-lightbox.png#lightbox)

   **Devre - Sağlayıcı durumu: Sağlanan**

   [![](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m.png "Provider status = Provisioned")](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m-lightbox.png#lightbox)
2. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft yönlendiricisi için ikinci kullanılabilir IP'yi kullandığından, bu alt ağdan yönlendiricinize ilk kullanılabilir IP adresini atarsınız.
   * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Microsoft yönlendiricisi için ikinci kullanılabilir IP'yi kullandığından, bu alt ağdan yönlendiricinize ilk kullanılabilir IP adresini atarsınız.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Hem Birincil hem de İkincil bağlantılar için aynı VLAN Kimliğini kullanmanız gerekir.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir önek kümesi göndermeyi planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu önekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * **İsteğe bağlı -** Müşteri ASN: Emsal AS numarasına kayıtlı olmayan önekleri reklamlıyorsanız, kayıtlı oldukları AS numarasını belirtebilirsiniz.
   * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
   * **İsteğe bağlı -** Bir kullanmayı seçerseniz bir MD5 karma.
3. Aşağıdaki örnekte gösterildiği gibi, yapılandırmak istediğiniz eşlemi seçebilirsiniz. Microsoft eşleme satırını seçin.

   [![Microsoft eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m.png "Microsoft eşleme satırını seçin")](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m-lightbox.png#lightbox)
4. Microsoft eşlemesini yapılandırın. Tüm parametreleri belirttikten sonra yapılandırmayı **kaydedin.** Aşağıdaki resim örnek bir yapılandırma yı gösterir:

   ![Microsoft eşlemesini yapılandırma](./media/expressroute-howto-routing-portal-resource-manager/configuration-m.png)

> [!IMPORTANT]
> Microsoft, Internet Yönlendirme Kayıt Defteri'nde belirtilen 'Reklamı yapılan genel önekleri' ve 'Eş ASN' (veya 'Müşteri ASN') atanmışsa bunu doğrular. Genel önekleri başka bir varlıktan alıyorsanız ve atama yönlendirme kayıt defterine kaydedilmezse, otomatik doğrulama tamamlanmaz ve el ile doğrulama gerektirir. Otomatik doğrulama başarısız olursa, 'Doğrulama gerekli' iletisini görürsünüz. 
>
> 'Doğrulama gerekli' iletisini görüyorsanız, genel öneklerin kuruluşunuza yönlendirme kayıt defterindeki öneklerin sahibi olarak listelenen kuruluş tarafından atandığını gösteren belge(ler) toplayın ve bu belgeleri el ile doğrulama için gönderin. aşağıda gösterildiği gibi bir destek bileti açma. 
>

   Devreniz bir 'Doğrulama gerekli' durumuna ulaşıyorsa, destek ekibimize öneklerin sahipliğinin kanıtını göstermek için bir destek bileti açmanız gerekir. Aşağıdaki örnekte gösterildiği gibi, doğrudan portaldan bir destek bileti açabilirsiniz:

   ![Doğrulama Gerekli - destek bileti](./media/expressroute-howto-routing-portal-resource-manager/ticket-portal-m.png)

5. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki resme benzer bir şey görürsünüz:

   ![Eşleme durumu: Yapılandırılan](./media/expressroute-howto-routing-portal-resource-manager/configured-m.png "Eşleme durumu: Yapılandırıldı")]

### <a name="to-view-microsoft-peering-details"></a><a name="getmsft"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Eşleme için satırı seçerek Microsoft'un görünüm özelliklerini görüntüleyebilirsiniz.

[![Microsoft bakan özelliklerini görüntüleme](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m.png "Özellikleri görüntüle")](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m-lightbox.png#lightbox)
### <a name="to-update-microsoft-peering-configuration"></a><a name="updatemsft"></a>Microsoft eşlemesi yapılandırmasını güncelleştirmek için

Değiştirmek istediğiniz eşleme için satırı seçebilir, ardından eşleme özelliklerini değiştirebilir ve değişikliklerinizi kaydedebilirsiniz.

![Eşleme satırLarını seçme](./media/expressroute-howto-routing-portal-resource-manager/update-peering-m.png)

### <a name="to-delete-microsoft-peering"></a><a name="deletemsft"></a>Microsoft eşlemesini silmek için

Aşağıdaki resimde gösterildiği gibi sil simgesine tıklayarak eşleme yapılandırmanızı kaldırabilirsiniz:

![Eşlemi silme](./media/expressroute-howto-routing-portal-resource-manager/delete-peering-m.png)

## <a name="azure-private-peering"></a><a name="private"></a>Azure özel eşlemesi

Bu bölüm, ExpressRoute devresi için Azure özel eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmenize yardımcı olur.

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun. 

   Bağlantı sağlayıcınız yönetilen Katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Azure özel eşlemesini sizin için etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız sizin için yönlendirmeyi yönetmezse, devrenizi oluşturduktan sonra sonraki adımlara devam edin.

   **Devre - Sağlayıcı durumu: Sağlanmadı**

   [![](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p.png "Provider status = Not Provisioned")](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p-lightbox.png#lightbox)

   **Devre - Sağlayıcı durumu: Sağlanan**

   [![](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p.png "Provider Status = Provisioned")](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p-lightbox.png#lightbox)

2. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

   * Birincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Microsoft yönlendiricisi için ikinci kullanılabilir IP'yi kullandığından, bu alt ağdan yönlendiricinize ilk kullanılabilir IP adresini atarsınız.
   * İkincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Microsoft yönlendiricisi için ikinci kullanılabilir IP'yi kullandığından, bu alt ağdan yönlendiricinize ilk kullanılabilir IP adresini atarsınız.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Hem Birincil hem de İkincil bağlantılar için aynı VLAN Kimliğini kullanmanız gerekir.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. 65515 ile 65520 arasında değişen sayılar dışında bu eşleme için özel bir AS numarası kullanabilirsiniz.
   * Özel bir bakış oluştururken, şirket içi Edge yönlendiricinizden Azure'a giden rotaların reklamını BGP üzerinden vermelisiniz.
   * **İsteğe bağlı -** Bir kullanmayı seçerseniz bir MD5 karma.
3. Aşağıdaki örnekte gösterildiği gibi Azure özel eşleme satırını seçin:

   [![Özel eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/select-peering-p.png "Özel eşleme satırını seçin")](./media/expressroute-howto-routing-portal-resource-manager/select-peering-p-lightbox.png#lightbox)
4. Özel eşleme oluşturun. Tüm parametreleri belirttikten sonra yapılandırmayı **kaydedin.**

   ![özel eşleme yapılandırma](./media/expressroute-howto-routing-portal-resource-manager/configuration-p.png)
5. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki örneğe benzer bir şey görürsünüz:

   ![kaydedilmiş özel akran](./media/expressroute-howto-routing-portal-resource-manager/save-p.png)

### <a name="to-view-azure-private-peering-details"></a><a name="getprivate"></a>Azure özel eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure özel eşleme özelliklerini görüntüleyebilirsiniz.

[![Özel eşleme özelliklerini görüntüleme](./media/expressroute-howto-routing-portal-resource-manager/view-p.png "Özel eşleme özelliklerini görüntüleme")](./media/expressroute-howto-routing-portal-resource-manager/view-p-lightbox.png#lightbox)

### <a name="to-update-azure-private-peering-configuration"></a><a name="updateprivate"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz. Güncellemeden sonra değişikliklerinizi kaydedin.

![özel eşlemi güncelle](./media/expressroute-howto-routing-portal-resource-manager/update-peering-p.png)

### <a name="to-delete-azure-private-peering"></a><a name="deleteprivate"></a>Azure özel eşlemesini silmek için

Aşağıdaki resimde gösterildiği gibi silme simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz:

> [!WARNING]
> Bu örneği çalıştırmadan önce tüm sanal ağların ve ExpressRoute Global Reach bağlantılarının kaldırıldığından emin olmalısınız. 
> 
> 

![özel eşlemi silme](./media/expressroute-howto-routing-portal-resource-manager/delete-p.png)


## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, [Bir VNet'i ExpressRoute devresine bağla](expressroute-howto-linkvnet-portal-resource-manager.md)
* ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
* Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).
