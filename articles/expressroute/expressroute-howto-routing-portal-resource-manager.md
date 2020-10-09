---
title: 'Öğretici: ExpressRoute bağlantı hattı için eşlemeyi Yapılandırma-Azure portal'
description: Bu öğreticide, Azure portal kullanarak ExpressRoute özel ve Microsoft eşlemesi oluşturma ve sağlama işlemlerinin nasıl yapılacağı gösterilir.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: tutorial
ms.date: 10/08/2020
ms.author: duau
ms.openlocfilehash: cad098ed2dedc7abba57394ef1e26b9b7c87cd9c
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91855480"
---
# <a name="tutorial-create-and-modify-peering-for-an-expressroute-circuit-using-the-azure-portal"></a>Öğretici: Azure portal kullanarak bir ExpressRoute bağlantı hattı için eşleme oluşturma ve değiştirme

Bu öğreticide, Azure portal kullanarak Azure Resource Manager ExpressRoute bağlantı hattı için Yönlendirme yapılandırması oluşturma ve yönetme işlemlerinin nasıl yapılacağı gösterilir. Ayrıca, bir ExpressRoute bağlantı hattı için durum, güncelleştirme veya silme ve yinelenenleri kaldırma ve sağlama ayarlarını da denetleyebilirsiniz. Devrenize çalışmak için farklı bir yöntem kullanmak istiyorsanız, aşağıdaki listeden bir makale seçin:

> [!div class="op_single_selector"]
> * [Azure portalındaki](expressroute-howto-routing-portal-resource-manager.md)
> * [PowerShell](expressroute-howto-routing-arm.md)
> * [Azure CLI](howto-routing-cli.md)
> * [Ortak eşleme](about-public-peering.md)
> * [Video-özel eşleme](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-azure-private-peering-for-your-expressroute-circuit)
> * [Video-Microsoft eşlemesi](https://azure.microsoft.com/documentation/videos/azure-expressroute-how-to-set-up-microsoft-peering-for-your-expressroute-circuit)
> * [PowerShell (klasik)](expressroute-howto-routing-classic.md)
> 

ExpressRoute bağlantı hattı için özel eşleme ve Microsoft eşlemesi yapılandırabilirsiniz (Azure genel eşleme yeni devreler için kullanım dışıdır). Eşlemeler, seçtiğiniz herhangi bir sırada yapılandırılabilir. Ancak, her eşlemenin yapılandırmasını birer birer tamamladığınızdan emin olmanız gerekir. Yönlendirme etki alanları ve eşlemeler hakkında daha fazla bilgi için bkz. [ExpressRoute yönlendirme etki alanları](expressroute-circuit-peerings.md). Ortak eşleme hakkında daha fazla bilgi için bkz. [ExpressRoute genel eşleme](about-public-peering.md).

Bu öğreticide şunların nasıl yapıldığını öğreneceksiniz:
> [!div class="checklist"]
> - Devre için Microsoft eşlemesini yapılandırma, güncelleştirme ve silme
> - Devre için Azure özel eşlemesini yapılandırma, güncelleştirme ve silme

## <a name="prerequisites"></a>Ön koşullar

* Yapılandırmaya başlamadan önce aşağıdaki sayfaları gözden geçirdiğinizden emin olun:
    * [Önkoşullar](expressroute-prerequisites.md) 
    * [Yönlendirme gereksinimleri](expressroute-routing.md)
    * [İş akışları](expressroute-workflows.md)
* Etkin bir ExpressRoute bağlantı hattınızın olması gerekir. Devam etmeden önce [ExpressRoute](expressroute-howto-circuit-portal-resource-manager.md) bağlantı hattı oluşturma ve bağlantı sağlayıcınız için devre dışı bırakma yönergelerini izleyin. Eşlemeyi yapılandırmak için ExpressRoute bağlantı hattının sağlanmış ve etkin durumda olması gerekir. 
* Paylaşılan anahtar/MD5 karması kullanmayı planlıyorsanız, tünelin her iki tarafında da anahtarı kullandığınızdan emin olun. Sınır en fazla 25 alfasayısal karakterdir. Özel karakterler desteklenmez. 

Bu yönergeler yalnızca Katman 2 bağlantı hizmetleri sunan hizmet sağlayıcıları ile oluşturulan bağlantı hatları için geçerlidir. Yönetilen katman 3 Hizmetleri (genellikle MPLS gibi bir ıPVPN) sunan bir hizmet sağlayıcısı kullanıyorsanız, bağlantı sağlayıcınız yönlendirmeyi yapılandırır ve yönetir. 

> [!IMPORTANT]
> Şu anda hizmet yönetim portalı aracılığıyla hizmet sağlayıcılar tarafından yapılandırılan eşlemeleri tanıtmıyoruz. Bu özelliği yakında etkinleştirmek için çalışıyoruz. BGP eşayarlarını yapılandırmadan önce hizmet sağlayıcınıza danışın.
> 

## <a name="microsoft-peering"></a><a name="msft"></a>Microsoft eşlemesi

Bu bölüm, bir ExpressRoute bağlantı hattı için Microsoft eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmesine yardımcı olur.

> [!IMPORTANT]
> 1 Ağustos 2017 ' den önce yapılandırılmış ExpressRoute bağlantı hattı Microsoft eşlemesi, yönlendirme filtreleri tanımlanmasa bile Microsoft eşlemesi aracılığıyla tanıtılan tüm hizmet öneklerini de alacak. 1 Ağustos 2017 ' de veya sonrasında yapılandırılan ExpressRoute devrelerinin Microsoft eşlemesi, bir yol filtresi devresine iliştirilene kadar tanıtılan öneklere sahip olmayacaktır. Daha fazla bilgi için bkz. [Microsoft eşlemesi için rota filtresi yapılandırma](how-to-routefilter-powershell.md).
> 
> 

### <a name="to-create-microsoft-peering"></a>Microsoft eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devreye devam etmeden önce bağlantı sağlayıcı tarafından tam olarak sağlandığından emin olmak için **sağlayıcı durumunu** kontrol edin.

   Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan Microsoft eşlemesini sizin için etkinleştirmesini isteyebilirsiniz. Sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmezse, devrenizi oluşturduktan sonra bu adımlarla devam edin.

   **Devre dışı sağlayıcı durumu: sağlanmadı**

   [![ExpressRoute tanıtım devresi için genel bakış sayfasını, sağlayıcı durumunun "sağlanmadı" olarak ayarlandığını belirten kırmızı bir kutu ile gösteren ekran görüntüsü.](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m.png)](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-m-lightbox.png#lightbox)


   **Devre dışı sağlayıcı durumu: sağlandı**

   [![ExpressRoute tanıtım devresi için genel bakış sayfasını, sağlayıcı durumunun "sağlandı" olarak ayarlandığını belirten kırmızı bir kutu ile gösteren ekran görüntüsü.](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m.png)](./media/expressroute-howto-routing-portal-resource-manager/provisioned-m-lightbox.png#lightbox)

2. Bağlantı hattı için Microsoft eşlemesini yapılandırın. Devam etmeden önce aşağıdaki bilgilere sahip olduğunuzdan emin olun.

   * Birincil bağlantı için bir /30 alt ağı. Adres bloğunun sizin tarafınızdan sahip olduğu ve bir RıR/IÇ_VERIM_ORANı 'ye kayıtlı geçerli bir genel IPv4 öneki olması gerekir. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * İkincil bağlantı için bir /30 alt ağı. Adres bloğunun sizin tarafınızdan sahip olduğu ve bir RıR/IÇ_VERIM_ORANı 'ye kayıtlı geçerli bir genel IPv4 öneki olması gerekir. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Birincil ve Ikincil bağlantılar aynı VLAN KIMLIĞINI kullanmanız gerekir.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz.
   * Tanıtılan ön ekler: BGP oturumunda tanıtmayı planladığınız tüm ön eklerin bir listesini sağlarsınız. Yalnızca ortak IP adresi ön ekleri kabul edilir. Bir önek kümesi gönderilmesini planlıyorsanız, virgülle ayrılmış bir liste gönderebilirsiniz. Bu önekler size bir RIR / IRR içinde kaydedilmiş olmalıdır.
   * **Isteğe bağlı-** Müşteri ASN: eşdeğer olarak eşleme kayıtlı olmayan ön ekler varsa, birlikte kaydettirildikleri AS numarasını belirtebilirsiniz.
   * Yönlendirme Kayıt Defteri Adı: AS numarası ve öneklerinin kaydedildiği RIR / IRR’yi belirtebilirsiniz.
   * **Isteğe bağlı-** Kullanmayı seçerseniz bir MD5 karma değeri.
3. Yapılandırmak istediğiniz eşlemeyi aşağıdaki örnekte gösterildiği gibi seçebilirsiniz. Microsoft eşleme satırını seçin.

   [![Microsoft eşleme satırını seçin](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m.png "Microsoft eşleme satırını seçin")](./media/expressroute-howto-routing-portal-resource-manager/select-peering-m-lightbox.png#lightbox)
4. Microsoft eşlemesini yapılandırın. Tüm parametreleri belirttikten sonra yapılandırmayı **kaydedin** . Aşağıdaki görüntüde örnek bir yapılandırma gösterilmektedir:

   ![Microsoft eşlemesini yapılandırma](./media/expressroute-howto-routing-portal-resource-manager/configuration-m.png)

> [!IMPORTANT]
> Microsoft, belirtilen ' tanıtılan genel ön eklerin ' ve ' eşdüzey ASN ' (veya ' müşteri ASN ') tarafından Internet yönlendirme kayıt defterinde size atanıp atanmadığını doğrular. Diğer bir varlıktan ortak ön ekleri alıyorsanız ve atama, yönlendirme kayıt defteriyle birlikte kaydedilmetiyse, otomatik doğrulama tamamlanmaz ve el ile doğrulama gerektirir. Otomatik doğrulama başarısız olursa, ' doğrulama gerekiyor ' iletisini görürsünüz. 
>
> ' Doğrulama gerekli ' iletisini görürseniz, genel önekleri gösteren belge (ler) i, yönlendirme kayıt defterindeki ön eklerin sahibi olarak listelenen varlık tarafından kuruluşunuza atanır ve bir destek bileti açarak bu belgeleri el ile doğrulama için gönderir. 
>

   Devre dışı bir ' doğrulama gerekli ' durumuna alırsa, ön eklerin sahipliğinin destek ekibimize gösterilmesi için bir destek bileti açmanız gerekir. Aşağıdaki örnekte gösterildiği gibi, doğrudan portaldan bir destek bileti açabilirsiniz:

   ![Doğrulama gerekiyor-destek bileti](./media/expressroute-howto-routing-portal-resource-manager/ticket-portal-m.png)

5. Yapılandırma başarıyla kabul edildikten sonra aşağıdaki görüntüye benzer bir şey görürsünüz:

   ![Eşleme durumu: yapılandırıldı](./media/expressroute-howto-routing-portal-resource-manager/configured-m.png "Eşleme durumu: yapılandırıldı")

### <a name="to-view-microsoft-peering-details"></a><a name="getmsft"></a>Microsoft eşleme ayrıntılarını görüntülemek için

Eşleme satırını seçerek Microsoft eşlemesi özelliklerini görüntüleyebilirsiniz.

[![Microsoft eşleme özelliklerini görüntüle](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m.png "Özellikleri görüntüle")](./media/expressroute-howto-routing-portal-resource-manager/view-peering-m-lightbox.png#lightbox)
### <a name="to-update-microsoft-peering-configuration"></a><a name="updatemsft"></a>Microsoft eşlemesi yapılandırmasını güncelleştirmek için

Değiştirmek istediğiniz eşlemenin satırını seçip eşleme özelliklerini değiştirebilir ve değişikliklerinizi kaydedebilirsiniz.

![Eşleme satırı seç](./media/expressroute-howto-routing-portal-resource-manager/update-peering-m.png)

## <a name="azure-private-peering"></a><a name="private"></a>Azure özel eşlemesi

Bu bölüm, bir ExpressRoute bağlantı hattı için Azure özel eşleme yapılandırmasını oluşturmanıza, almanıza, güncelleştirmenize ve silmenize yardımcı olur.

### <a name="to-create-azure-private-peering"></a>Azure özel eşlemesi oluşturmak için

1. ExpressRoute bağlantı hattını yapılandırın. Devam etmeden önce bağlantı sağlayıcı tarafından bağlantı hattının tam olarak sağlandığından emin olun. 

   Bağlantı sağlayıcınız yönetilen katman 3 hizmetleri sunuyorsa, bağlantı sağlayıcınızdan sizin için Azure özel eşlemeyi etkinleştirmesini isteyebilirsiniz. Sonraki bölümlerde listelenen yönergeleri izlemeniz gerekmez. Ancak, bağlantı sağlayıcınız yönlendirmeyi sizin için yönetmezse, devrenizi oluşturduktan sonra sonraki adımlarla devam edin.

   **Devre dışı sağlayıcı durumu: sağlanmadı**

   [![ExpressRoute tanıtım devresi için genel bakış sayfasını gösteren ekran görüntüsü, "sağlanmadı" olarak ayarlanan sağlayıcı durumunu vurgulamaya yönelik kırmızı bir kutu.](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p.png)](./media/expressroute-howto-routing-portal-resource-manager/not-provisioned-p-lightbox.png#lightbox)

   **Devre dışı sağlayıcı durumu: sağlandı**

   [!["Sağlandı" olarak ayarlanan sağlayıcı durumunu vurgulamakta olan ExpressRoute tanıtım devresi için genel bakış sayfasını gösteren ekran görüntüsü.](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p.png)](./media/expressroute-howto-routing-portal-resource-manager/provisioned-p-lightbox.png#lightbox)

2. Bağlantı hattı için Azure özel eşlemesini yapılandırın. Sonraki adımlarla devam etmeden önce aşağıdaki öğelere sahip olduğunuzdan emin olun:

   * Birincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * İkincil bağlantı için bir /30 alt ağı. Alt ağ, sanal ağlar için ayrılmış herhangi bir adres alanının parçası olmamalıdır. Bu alt ağdan, Microsoft 'un yönlendiricisi için kullanılan ikinci IP adresini kullandığından, ilk yapılandırılmış IP adresini yönlendiricinize atayacaksınız.
   * Bu eşlemenin kurulacağı geçerli bir VLAN kimliği. Bağlantı hattındaki başka bir eşlemenin aynı VLAN kimliğini kullanmadığından emin olun. Birincil ve Ikincil bağlantılar aynı VLAN KIMLIĞINI kullanmanız gerekir.
   * Eşleme için AS numarası. 2 bayt ve 4 bayt AS numaralarını kullanabilirsiniz. 65515 ile 65520 arasında (ikisi de dahil olmak üzere), bu eşleme için özel bir AS numarası kullanabilirsiniz.
   * Özel eşlemeyi yapılandırırken şirket içi kenar yönlendiricinizin yollarını BGP aracılığıyla Azure 'a duyurmanız gerekir.
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

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="to-delete-microsoft-peering"></a><a name="deletemsft"></a>Microsoft eşlemesini silmek için

Aşağıdaki görüntüde gösterildiği gibi, silme simgesine tıklayarak eşleme yapılandırmanızı kaldırabilirsiniz:

![Eşlemeyi Sil](./media/expressroute-howto-routing-portal-resource-manager/delete-peering-m.png)

### <a name="to-delete-azure-private-peering"></a><a name="deleteprivate"></a>Azure özel eşlemesini silmek için

Aşağıdaki görüntüde gösterildiği gibi, Sil simgesini seçerek eşleme yapılandırmanızı kaldırabilirsiniz:

> [!WARNING]
> Bu örneği çalıştırmadan önce tüm sanal ağların ve ExpressRoute Global Reach bağlantılarının kaldırıldığından emin olmanız gerekir. 
> 
> 

![Özel eşlemeyi Sil](./media/expressroute-howto-routing-portal-resource-manager/delete-p.png)

## <a name="next-steps"></a>Sonraki adımlar

Azure özel eşlemesini yapılandırdıktan sonra, sanal bir ağı devresine bağlamak için bir ExpressRoute ağ geçidi oluşturabilirsiniz. 

> [!div class="nextstepaction"]
> [ExpressRoute için sanal ağ geçidi yapılandırma](expressroute-howto-add-gateway-resource-manager.md)
