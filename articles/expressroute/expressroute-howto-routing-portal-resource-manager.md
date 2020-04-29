---
title: 'Azure ExpressRoute: eşlemeyi yapılandırma'
description: Bu makalede, ExpressRoute özel ve Microsoft eşlemesi oluşturma ve sağlama adımları belgelemekte. Bu makalede ayrıca, bir devre için durumun durumunu denetleme, güncelleştirme veya silme işlemlerinin nasıl yapılacağı gösterilir.
services: expressroute
author: mialdrid
ms.service: expressroute
ms.topic: conceptual
ms.date: 02/13/2019
ms.author: mialdrid
ms.openlocfilehash: 18d2db18e9880028c60b4b545c3628f4a9cb4703
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79264797"
---
# <a name="create-and-modify-peering-for-an-expressroute-circuit"></a>Bir ExpressRoute bağlantı hattı için eşlemeyi oluşturma ve değiştirme

Bu makale, Azure portal kullanarak bir Azure Resource Manager (ARM) ExpressRoute devresi için Yönlendirme yapılandırması oluşturmanıza ve yönetmenize yardımcı olur. Ayrıca, bir ExpressRoute bağlantı hattı için durum, güncelleştirme veya silme ve yinelenenleri kaldırma ve sağlama ayarlarını da denetleyebilirsiniz. Devrenize çalışmak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden bir makale seçin:

> [!div class="op_single_selector"]
> * [Azure portal](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Ortak eşleme](about-public-peering.md)
> * [Video-özel eşleme](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video-Microsoft eşlemesi](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

ExpressRoute bağlantı hattı için özel eşleme ve Microsoft eşlemesi yapılandırabilirsiniz (Azure genel eşleme yeni devreler için kullanım dışıdır). Eşlemeler, seçtiğiniz herhangi bir sırada yapılandırılabilir. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. Yönlendirme etki alanları ve eşlemeler hakkında daha fazla bilgi için bkz. [ExpressRoute yönlendirme etki alanları](expressroute-circuit-peerings.md). Ortak eşleme hakkında daha fazla bilgi için bkz. [ExpressRoute genel eşleme](about-public-peering.md).

## <a name="configuration-prerequisites"></a>Yapılandırma önkoşulları

* Yapılandırmaya başlamadan önce [önkoşullar](expressroute-prerequisites.md) sayfasını, [yönlendirme gereksinimleri](expressroute-routing.md) sayfasını ve [iş akışları](expressroute-workflows.md) sayfasını gözden geçirdiğinizden emin olun.
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-portal-resource-manager.md) yönergelerini izleyin ve bağlantı sağlayıcınızın bağlantı hattını etkinleştirmesini isteyin. Eşlemeyi yapılandırmak için ExpressRoute bağlantı hattının sağlanmış ve etkin durumda olması gerekir. 
* Paylaşılan bir anahtar/MD5 karması kullanmayı planlıyorsanız, bunu tünelin her iki tarafında da kullandığınızdan emin olun ve alfasayısal karakter sayısını en fazla 25 olarak sınırlandırın. Özel karakterler desteklenmez. 

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen katman 3 Hizmetleri (genellikle MPLS gibi bir ıPVPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi yapılandırır ve yönetir. 

> [!IMPORTANT]
> Şu anda hizmet yönetim portalı aracılığıyla hizmet sağlayıcılar tarafından yapılandırılan eşlemeleri tanıtmıyoruz. Bu özelliği yakında etkinleştirmek için çalışıyoruz. BGP eşayarlarını yapılandırmadan önce hizmet sağlayıcınıza danışın.
> 
> 

## <a name="microsoft-peering"></a><a name="msft"></a>Microsoft eşlemesi

Bu bölüm, bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmesine yardımcı olur.

> [!IMPORTANT]
> 1 Ağustos 2017 ' den önce yapılandırılmış ExpressRoute bağlantı hattı Microsoft eşlemesi, yönlendirme filtreleri tanımlanmasa bile Microsoft eşlemesi aracılığıyla tanıtılan tüm hizmet öneklerini de alacak. 1 Ağustos 2017 ' de veya sonrasında yapılandırılan ExpressRoute devrelerinin Microsoft eşlemesi, bir yol filtresi devresine iliştirilene kadar tanıtılan öneklere sahip olmayacaktır. Daha fazla bilgi için bkz. [Microsoft eşlemesi için rota filtresi yapılandırma](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devreye devam etmeden önce bağlantı sağlayıcı tarafından tam olarak sağlandığından emin olmak için **sağlayıcı durumunu** kontrol edin.

   Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Microsoft eşlemesini sizin için etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmezse, devrenizi oluşturduktan sonra bu adımlarla devam edin.

   **Devre dışı sağlayıcı durumu: sağlanmadı**

    [![](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m.png "Provider status: Not provisioned")](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m-lightbox.png#lightbox)

   **Devre dışı sağlayıcı durumu: sağlandı**

   [![](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m.png "Provider status = Provisioned")](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m-lightbox.png#lightbox)
2. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * İkincil bağlantı için bir /30 alt ağı. Bu size ait ve bir RIR / IRR içinde kayıtlı bir geçerli ortak IPv4 ön eki olmalıdır. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Birincil ve Ikincil bağlantılar için aynı VLAN KIMLIĞINI kullanmanız gerekir.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan önekler: BGP oturumunda tanıtmayı planladığınız tüm öneklerin bir listesini sağlamanız gerekir. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir önek kümesi gönderilmesini planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu önekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * **Isteğe bağlı-** Müşteri ASN: eşleme numarasına kayıtlı olmayan ön ekler varsa, bunların kaydedildiği AS numarasını belirtebilirsiniz.
   * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
   * **Isteğe bağlı-** Kullanmayı seçerseniz bir MD5 karma değeri.
3. Yapılandırmak istediğiniz eşlemeyi aşağıdaki örnekte gösterildiği gibi seçebilirsiniz. Microsoft eşleme satırını seçin.

   [![Microsoft eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m.png "Microsoft eşleme satırını seçin")](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m-lightbox.png#lightbox)
4. Microsoft eşlemesini yapılandırın. Tüm parametreleri belirttikten sonra yapılandırmayı **kaydedin** . Aşağıdaki görüntüde örnek bir yapılandırma gösterilmektedir:

   ![Microsoft eşlemesini yapılandırma](./media/expressroute-howto-routing-portal-resource-manager/configuration-m.png)

> [!IMPORTANT]
> Microsoft, belirtilen ' tanıtılan genel ön eklerin ' ve ' eşdüzey ASN ' (veya ' müşteri ASN ') tarafından Internet yönlendirme kayıt defterinde size atanıp atanmadığını doğrular. Diğer bir varlıktan ortak ön ekleri alıyorsanız ve atama, yönlendirme kayıt defteriyle birlikte kaydedilmetiyse, otomatik doğrulama tamamlanmaz ve el ile doğrulama gerektirir. Otomatik doğrulama başarısız olursa, ' doğrulama gerekiyor ' iletisini görürsünüz. 
>
> ' Doğrulama gerekli ' iletisini görürseniz, genel önekleri gösteren belge (ler) i, yönlendirme kayıt defterindeki ön eklerin sahibi olarak listelenen varlık tarafından kuruluşunuza atanır ve aşağıda gösterildiği gibi bir destek bileti açarak bu belgeleri el ile doğrulama için gönderir. 
>

   Devre dışı bir ' doğrulama gerekli ' durumuna alırsa, ön eklerin sahipliğinin destek ekibimize gösterilmesi için bir destek bileti açmanız gerekir. Aşağıdaki örnekte gösterildiği gibi, doğrudan portaldan bir destek bileti açabilirsiniz:

   ![Doğrulama gerekiyor-destek bileti](./media/expressroute-howto-routing-portal-resource-manager/ticket-portal-m.png)

5. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki görüntüye benzer bir şey görürsünüz:

   ![Eşleme durumu: yapılandırıldı](./media/expressroute-howto-routing-portal-resource-manager/configured-m.png "Eşleme durumu: yapılandırıldı")]

### <a name="to-view-microsoft-peering-details"></a><a name="getmsft"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Eşleme satırını seçerek Microsoft eşlemesi özelliklerini görüntüleyebilirsiniz.

[![Microsoft eşleme özelliklerini görüntüle](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m.png "Özellikleri görüntüle")](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m-lightbox.png#lightbox)
### <a name="to-update-microsoft-peering-configuration"></a><a name="updatemsft"></a>Microsoft eşlemesi yapılandırmasını güncelleştirmek için

Değiştirmek istediğiniz eşlemenin satırını seçip eşleme özelliklerini değiştirebilir ve değişikliklerinizi kaydedebilirsiniz.

![Eşleme satırı seç](./media/expressroute-howto-routing-portal-resource-manager/update-peering-m.png)

### <a name="to-delete-microsoft-peering"></a><a name="deletemsft"></a>Microsoft eşlemesini silmek için

Aşağıdaki görüntüde gösterildiği gibi, silme simgesine tıklayarak eşleme yapılandırmanızı kaldırabilirsiniz:

![Eşlemeyi Sil](./media/expressroute-howto-routing-portal-resource-manager/delete-peering-m.png)

## <a name="azure-private-peering"></a><a name="private"></a>Azure özel eşlemesi

Bu bölüm, bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmenize yardımcı olur.

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun. 

   Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Bu durumda, sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmezse, devrenizi oluşturduktan sonra bir sonraki adımla devam edin.

   **Devre dışı sağlayıcı durumu: sağlanmadı**

   [![](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p.png "Provider status = Not Provisioned")](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p-lightbox.png#lightbox)

   **Devre dışı sağlayıcı durumu: sağlandı**

   [![](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p.png "Provider Status = Provisioned")](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p-lightbox.png#lightbox)

2. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlara devam etmeden önce aşağıdaki öğelerin bulunduğundan emin olun:

   * Birincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * İkincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Birincil ve Ikincil bağlantılar için aynı VLAN KIMLIĞINI kullanmanız gerekir.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. 65515 ile 65520 arasında (ikisi de dahil olmak üzere), bu eşleme için özel bir AS numarası kullanabilirsiniz.
   * Özel eşlemeyi ayarlarken, şirket içi kenar yönlendiricinizin yollarını BGP aracılığıyla Azure 'a duyurmanız gerekir.
   * **Isteğe bağlı-** Kullanmayı seçerseniz bir MD5 karma değeri.
3. Aşağıdaki örnekte gösterildiği gibi Azure özel eşleme satırını seçin:

   [![Özel eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/select-peering-p.png "Özel eşleme satırını seçin")](./media/expressroute-howto-routing-portal-resource-manager/select-peering-p-lightbox.png#lightbox)
4. Özel eşleme oluşturun. Tüm parametreleri belirttikten sonra yapılandırmayı **kaydedin** .

   ![Özel eşlemeyi yapılandırma](./media/expressroute-howto-routing-portal-resource-manager/configuration-p.png)
5. Yapılandırma başarıyla kabul edildikten sonra, aşağıdaki örneğe benzer bir şey görürsünüz:

   ![özel eşleme kaydedildi](./media/expressroute-howto-routing-portal-resource-manager/save-p.png)

### <a name="to-view-azure-private-peering-details"></a><a name="getprivate"></a>Azure özel eşleme ayrıntılarını görüntülemek için

Eşlemeyi seçerek Azure özel eşleme özelliklerini görüntüleyebilirsiniz.

[![Özel eşleme özelliklerini görüntüle](./media/expressroute-howto-routing-portal-resource-manager/view-p.png "Özel eşleme özelliklerini görüntüle")](./media/expressroute-howto-routing-portal-resource-manager/view-p-lightbox.png#lightbox)

### <a name="to-update-azure-private-peering-configuration"></a><a name="updateprivate"></a>Azure özel eşleme yapılandırmasını güncelleştirmek için

Eşleme için satırı seçebilir ve eşleme özelliklerini değiştirebilirsiniz. Güncelleştirme sonrasında yaptığınız değişiklikleri kaydedin.

![Özel eşlemeyi Güncelleştir](./media/expressroute-howto-routing-portal-resource-manager/update-peering-p.png)

### <a name="to-delete-azure-private-peering"></a><a name="deleteprivate"></a>Azure özel eşlemesini silmek için

Aşağıdaki görüntüde gösterildiği gibi, Sil simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz:

> [!WARNING]
> Bu örneği çalıştırmadan önce tüm sanal ağların ve ExpressRoute Global Reach bağlantılarının kaldırıldığından emin olmanız gerekir. 
> 
> 

![Özel eşlemeyi Sil](./media/expressroute-howto-routing-portal-resource-manager/delete-p.png)


## <a name="next-steps"></a>Sonraki adımlar

Sonraki adım, [bir sanal ağı ExpressRoute devresine bağlama](expressroute-howto-linkvnet-portal-resource-manager.md)
* ExpressRoute iş akışları hakkında daha fazla bilgi için bkz. [ExpressRoute iş akışları](expressroute-workflows.md).
* Bağlantı hattı eşlemesi hakkında daha fazla bilgi için bkz. [ExpressRoute bağlantı hattı ve yönlendirme etki alanları](expressroute-circuit-peerings.md).
* Sanal ağlarla çalışma hakkında daha fazla bilgi için bkz. [Sanal ağa genel bakış](../virtual-network/virtual-networks-overview.md).
