---
title: "ExpressRoute: devreleri klasik 'dan Azure Resource Manager taşıyın"
description: Klasik bir Azure ExpressRoute devresini Azure Resource Manager dağıtım modeline taşıdığınızda ne olacağı hakkında bilgi edinin.
services: expressroute
author: duongau
ms.service: expressroute
ms.topic: how-to
ms.date: 12/15/2020
ms.author: duau
ms.openlocfilehash: dcba2e9de2b37e8c432f94781b3c4c369ad52395
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "97807950"
---
# <a name="moving-expressroute-circuits-from-the-classic-to-the-resource-manager-deployment-model"></a>ExpressRoute bağlantı hatlarını klasikten Resource Manager dağıtım modeline taşıma
Bu makalede bir Azure ExpressRoute devresini Azure Resource Manager dağıtım modeline taşırken ne olacağı hakkında genel bir bakış sunulmaktadır.

Hem klasik hem de Kaynak Yöneticisi dağıtım modellerinde dağıtılan sanal ağlara bağlanmak için tek bir ExpressRoute bağlantı hattı kullanabilirsiniz.

![Her iki dağıtım modeli üzerinden sanal ağlara bağlanan bir ExpressRoute bağlantı hattı](./media/expressroute-move/expressroute-move-1.png)

## <a name="expressroute-circuits-that-are-created-in-the-classic-deployment-model"></a>Klasik dağıtım modelinde oluşturulmuş ExpressRoute bağlantı hatları
Klasik dağıtım modelinde oluşturulan ExpressRoute devreleri öncelikle Kaynak Yöneticisi dağıtım modeline geçirilmesi gerekir. Yalnızca, hem klasik hem de Kaynak Yöneticisi dağıtım modellerine bağlantı sağlayabilirsiniz. Bağlantı taşınırken bağlantı kaybı veya aksaklık olmaz. Aynı abonelik ve çapraz abonelik içindeki klasik dağıtım modelindeki tüm devre dışı sanal ağ bağlantıları korunur.

Taşıma başarıyla tamamlandıktan sonra, ExpressRoute bağlantı hattı, Kaynak Yöneticisi dağıtım modelinde oluşturulan bir ExpressRoute devresi gibi davranır. Resource Manager dağıtım modelinde artık sanal ağlara bağlantılar oluşturabilirsiniz.

ExpressRoute bağlantı hattını Kaynak Yöneticisi dağıtım modeline taşındıktan sonra yalnızca Kaynak Yöneticisi dağıtım modelinde yönetebilirsiniz. Peerusing 'leri yönetmeye yönelik işlemler, devre özelliklerini güncelleştirme ve silme devreleri yalnızca Kaynak Yöneticisi dağıtım modeliyle kullanılabilir. Her iki dağıtım modeline erişimi nasıl yönetebileceğinizi öğrenmek için aşağıdaki bölüme bakın.

Devrenizi Kaynak Yöneticisi dağıtım modeline taşımak için bağlantı sağlayıcınızı dahil etmeniz gerekmez.

## <a name="expressroute-circuits-that-are-created-in-the-resource-manager-deployment-model"></a>Resource Manager dağıtım modelinde oluşturulmuş ExpressRoute bağlantı hatları
Resource Manager dağıtım modelinde oluşturulmuş ExpressRoute bağlantı hatlarının her iki dağıtım modelinden erişilebilir olmasını etkinleştirebilirsiniz. Aboneliğinizdeki herhangi bir ExpressRoute bağlantı hattı, her iki dağıtım modelinden erişime sahip olacak şekilde yapılandırılabilir.

* Kaynak Yöneticisi dağıtım modelinde oluşturulan ExpressRoute devreleri, varsayılan olarak klasik dağıtım modeline erişemez.
* Klasik dağıtım modelinden Kaynak Yöneticisi dağıtım modeline taşınan ExpressRoute devreleri, her iki dağıtım modelinden de varsayılan olarak erişilebilir.
* ExpressRoute bağlantı hattı, Kaynak Yöneticisi veya klasik dağıtım modelinde oluşturulup oluşturulmayacağı Kaynak Yöneticisi dağıtım modeline her zaman erişebilir. [Sanal ağları bağlama](expressroute-howto-linkvnet-arm.md)yönergelerini izleyerek sanal ağlara bağlantılar oluşturabilirsiniz.
* ExpressRoute bağlantı hattında klasik dağıtım modeline erişim **allowClassicOperations** parametresi tarafından denetlenir.

> [!IMPORTANT]
> [Hizmet sınırlamaları](../azure-resource-manager/management/azure-subscription-service-limits.md) sayfasında belgelenen tüm kotalar uygulanır. Örnek olarak, standart bir bağlantı hattı her iki klasik ve Resource Manager dağıtım modeli üzerinden en fazla 10 sanal ağ bağlantısına sahip olabilir.
> 
> 

## <a name="controlling-access-to-the-classic-deployment-model"></a>Klasik dağıtım modeline erişimi denetleme
Her iki dağıtım modelinde sanal ağlara bağlantı sağlamak için bir ExpressRoute devresine izin verebilirsiniz. Bunu yapmak için ExpressRoute devresinde **Allowclassicoperations** parametresini ayarlayın.

**allowClassicOperations** parametresinin TRUE olarak ayarlanması her iki dağıtım modelinden ExpressRoute bağlantı hattına bağlanmanızı etkinleştirir. 
* Klasik dağıtım modelindeki sanal ağları bağlamak için bkz. [sanal ağları klasik dağıtım modeli için bağlama](expressroute-howto-linkvnet-classic.md).
* Kaynak Yöneticisi dağıtım modelindeki sanal ağları bağlamak için, bkz. [sanal ağları Kaynak Yöneticisi dağıtım modelinde bağlama](expressroute-howto-linkvnet-arm.md).

**allowClassicOperations** parametresini FALSE olarak ayarlayarak klasik dağıtım modelinden bağlantı hattına erişimi engeller. Ancak, klasik dağıtım modelinde bağlantılı tüm sanal ağlar hala korunur. ExpressRoute bağlantı hattı, klasik dağıtım modelinde görünmez.

## <a name="supported-operations-in-the-classic-deployment-model"></a>Klasik dağıtım modelinde desteklenen işlemler
**allowClassicOperations** parametresi TRUE olarak ayarlandığında bir ExpressRoute bağlantı hattı üzerinde şu klasik işlemler desteklenir:

* ExpressRoute bağlantı hattı bilgilerini alma
* Klasik sanal ağlara sanal ağ bağlantıları oluşturma/güncelleştirme/alma/silme
* çapraz abonelik bağlantısı için sanal ağ bağlantı yetkilerini oluşturma/güncelleştirme/alma/silme

Ancak, **Allowclassicoperations** değeri true olarak ayarlandığında, aşağıdaki klasik işlemleri yürütemezsiniz:

* Azure özel, Azure genel ve Microsoft eşlemeleri için Sınır Ağ Geçidi Protokolü (BGP) eşlemeleri oluşturma/güncelleştirme/alma/silme
* ExpressRoute bağlantı hatlarını silme

## <a name="communication-between-the-classic-and-the-resource-manager-deployment-models"></a>Klasik ve Resource Manager dağıtım modelleri arasında iletişim
ExpressRoute bağlantı hattı klasik ve Resource Manager dağıtım modelleri arasında bir köprü gibi davranır. Her iki sanal ağ da aynı bağlantı hattına bağlanmışsa, her iki dağıtım modeli için sanal ağlar arasındaki trafik ExpressRoute bağlantı hattı üzerinden geçirebilir.

Toplu işleme, sanal ağa ait ağ geçidi işleme kapasitesi tarafından sınırlandırılmıştır. Trafik, bu gibi durumlarda bağlantı sağlayıcısının ağlarını veya ağlarınızı girmez. Sanal ağlar arasındaki trafik akışı tam olarak Microsoft ağı içerisinde yer alır.

## <a name="access-to-azure-public-and-microsoft-peering-resources"></a>Azure genel ve Microsoft eşleme kaynaklarına erişim
Azure genel eşleme ve Microsoft eşleme aracılığıyla normalde erişilebilen kaynaklara erişmeye kesintisiz devam edebilirsiniz.  

## <a name="whats-supported"></a>Desteklenen durumlar
Bu bölümde ExpressRoute bağlantı hatları için desteklenen durumlar açıklanmaktadır:

* Klasik ve Resource Manager dağıtım modellerinde dağıtılan sanal ağlara erişim için tek bir ExpressRoute bağlantı hattı kullanabilirsiniz.
* Bir ExpressRoute bağlantı hattını klasikten Resource Manager dağıtım modeline taşıyabilirsiniz. Taşındıktan sonra ExpressRoute bağlantı hattı Kaynak Yöneticisi dağıtım modelinde oluşturulan diğer ExpressRoute bağlantı hattı gibi çalışmaya devam edecektir.
* Yalnızca ExpressRoute bağlantı hattını taşıyabilirsiniz. Bağlantı hattı bağlantıları, ağ geçitleri ve VPN ağ geçitleri bu işlem aracılığıyla taşınamaz.
* Bir ExpressRoute bağlantı hattı Resource Manager dağıtım modeline taşındıktan sonra ExpressRoute bağlantı hattı yaşam döngüsünü yalnızca Resource Manager dağıtım modeli kullanarak yönetebilirsiniz. Bu, eşlemeleri ekleme/güncelleştirme/silme, devre özelliklerini güncelleştirme (bant genişliği, SKU ve faturalandırma türü gibi) ve yalnızca Kaynak Yöneticisi dağıtım modelinde devreleri silme gibi işlemleri çalıştırabilmeniz anlamına gelir.
* ExpressRoute bağlantı hattı klasik ve Resource Manager dağıtım modelleri arasında bir köprü gibi davranır. Klasik sanal ağlardaki sanal makineler ve Kaynak Yöneticisi sanal ağlardaki sanal makineler arasındaki trafik, her iki sanal ağ aynı ExpressRoute devresine bağlanmışsa ExpressRoute üzerinden iletişim kurabilir.
* Çapraz abonelik bağlantısı her iki klasik ve Resource Manager dağıtım modellerinde desteklenir.
* Bir ExpressRoute bağlantı hattını klasik modelden Azure Resource Manager modeline taşıdıktan sonra [ExpressRoute işlem hattına bağlı sanal ağları geçirebilirsiniz](expressroute-migration-classic-resource-manager.md).

## <a name="whats-not-supported"></a>Desteklenmeyen durumlar
Bu bölümde ExpressRoute bağlantı hatları için desteklenmeyen durumlar açıklanmaktadır:

* Klasik dağıtım modelinden bir ExpressRoute bağlantı hattının yaşam döngüsünü yönetme.
* Klasik dağıtım modeli için Azure rol tabanlı erişim denetimi (Azure RBAC) desteği. Azure RBAC denetimlerini klasik dağıtım modelindeki bir devreye göre çalıştıramazsınız. Abonelikteki tüm yöneticiler/yardımcı yöneticiler bağlantı hattına sanal ağları bağlayabilir veya bağlantılarını kaldırabilir.

## <a name="configuration"></a>Yapılandırma
[Bir ExpressRoute bağlantı hattını klasikten Resource Manager dağıtım modeline taşıma](expressroute-howto-move-arm.md) konusunda açıklanan yönergeleri izleyin.

## <a name="next-steps"></a>Sonraki adımlar
* [ExpressRoute işlem hattına bağlı sanal ağları klasik modelden Azure Resource Manager modeline geçirme](expressroute-migration-classic-resource-manager.md)
* İş akışı bilgileri için bkz. [ExpressRoute bağlantı hattı sağlama iş akışları ve devre durumları](expressroute-workflows.md).
* ExpressRoute’unuzu yapılandırmak için:
  
  * [ExpressRoute bağlantı hattı oluşturma](expressroute-howto-circuit-arm.md)
  * [Yönlendirmeyi yapılandırma](expressroute-howto-routing-arm.md)
  * [ExpressRoute bağlantı hattına bir sanal ağı bağlama](expressroute-howto-linkvnet-arm.md)

