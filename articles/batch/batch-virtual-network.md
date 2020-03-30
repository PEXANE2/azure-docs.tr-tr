---
title: Sanal ağda havuz sağlama - Azure Toplu İş | Microsoft Dokümanlar
description: Bilgi işlem düğümlerinin dosya sunucusu gibi ağdaki diğer VM'lerle güvenli bir şekilde iletişim kurabilmeleri için Azure sanal ağında Toplu iş havuzu oluşturma.
services: batch
author: LauraBrenner
manager: evansma
ms.service: batch
ms.topic: article
ms.date: 04/10/2019
ms.author: labrenne
ms.custom: seodec18
ms.openlocfilehash: bbe38a9dc7be749b8e138ff3ca9ec4f06255b389
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247754"
---
# <a name="create-an-azure-batch-pool-in-a-virtual-network"></a>Sanal ağda Azure Toplu İşlem havuzu oluşturma

Bir Azure Toplu İş havuzu oluşturduğunuzda, havuzu belirttiğiniz bir [Azure sanal ağının](../virtual-network/virtual-networks-overview.md) (VNet) alt ağında sağlayabilirsiniz. Bu makalede, VNet'te Toplu İşlem havuzu nasıl ayarlanınca açıklanmaktadır. 

## <a name="why-use-a-vnet"></a>Neden VNet kullanıyorsun?

Azure Toplu İşlem havuzu, bilgi işlem düğümlerinin birbirleriyle iletişim kurmasına izin veren ayarlara sahiptir (örneğin, çok örnekli görevleri çalıştırın). Bu ayarlar ayrı bir VNet gerektirmez. Ancak, varsayılan olarak, düğümler bir lisans sunucusu veya dosya sunucusu gibi Toplu İşlem havuzunun parçası olmayan sanal makinelerle iletişim kuramaz. Havuz bilgi işlem düğümlerinin diğer sanal makinelerle veya şirket içi bir ağla güvenli bir şekilde iletişim kurmasına izin vermek için, havuzu bir Azure VNet'in alt ağında sağlayabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar

* **Kimlik doğrulama.** Azure sanal ağı kullanmak için Batch istemci API'sinin Azure Active Directory (AD) kimlik doğrulamasını kullanması gerekir. Azure AD için Azure Batch desteği, [Batch hizmeti çözümlerinin kimliğini Active Directory ile doğrulama](batch-aad-auth.md) makalesinde belirtilmiştir. 

* **Bir Azure VNet**. VNet gereksinimleri ve yapılandırması için aşağıdaki bölüme bakın. Önceden bir veya daha fazla alt ağı olan bir VNet hazırlamak için Azure portalını, Azure PowerShell'i, Azure Komut Satırı Arabirimini (CLI) veya diğer yöntemleri kullanabilirsiniz.  
  * Azure Kaynak Yöneticisi tabanlı bir VNet oluşturmak için [bkz.](../virtual-network/manage-virtual-network.md#create-a-virtual-network) Kaynak Yöneticisi tabanlı VNet yeni dağıtımlar için önerilir ve yalnızca Sanal Makine yapılandırmasındaki havuzlarda desteklenir.
  * Klasik bir VNet oluşturmak için [bkz.](../virtual-network/create-virtual-network-classic.md) Klasik bir VNet yalnızca Bulut Hizmetleri yapılandırmasındaki havuzlarda desteklenir.

## <a name="vnet-requirements"></a>Sanal ağ gereksinimleri

[!INCLUDE [batch-virtual-network-ports](../../includes/batch-virtual-network-ports.md)]

## <a name="create-a-pool-with-a-vnet-in-the-portal"></a>Portalda VNet içeren bir havuz oluşturma

VNet'inizi oluşturduktan ve ona bir alt ağ atadıktan sonra, bu VNet ile bir Toplu Iş havuzu oluşturabilirsiniz. Azure portalından bir havuz oluşturmak için aşağıdaki adımları izleyin: 

1. Azure portalında Batch hesabınıza gidin. Bu hesap, kullanmak istediğiniz VNet'i içeren kaynak grubuyla aynı abonelikte ve bölgede olmalıdır. 
2. Soldaki **Ayarlar** **penceresinde, Havuzlar** menü öğesini seçin.
3. **Havuzlar** penceresinde **Ekle** komutunu seçin.
4. Havuz **Ekle** penceresinde, **Resim Türü** açılır penceresinden kullanmak istediğiniz seçeneği seçin. 
5. Özel resminiz için doğru **Publisher/Offer/Sku'yu** seçin.
6. **Düğüm boyutu,** **Hedef adanmış düğümler**ve **Düşük öncelikli düğümler**ve istenen isteğe bağlı ayarlar da dahil olmak üzere kalan gerekli ayarları belirtin.
7. **Sanal**Ağ'da, kullanmak istediğiniz sanal ağı ve alt ağı seçin.
  
   ![Sanal ağ ile havuz ekleme](./media/batch-virtual-network/add-vnet-pool.png)

## <a name="user-defined-routes-for-forced-tunneling"></a>Zorunlu tünelleme için kullanıcı tanımlı rotalar

Kuruluşunuzdaki Internet'e bağlı trafiği alt ağdan denetim ve günlüğe kaydetme için şirket içi konumunuza yönlendirmek için gereksinimleriniz olabilir. VNet'inizdeki alt ağlar için zorunlu tünel leme özelliğini etkinleştirmiş olabilirsiniz. 

Azure Toplu Iş havuzu hesap düğümlerinizin zorla tünel leme özelliğine sahip bir VNet'te çalışmasını sağlamak için, bu alt ağ için aşağıdaki [kullanıcı tanımlı yolları](../virtual-network/virtual-networks-udr-overview.md) eklemeniz gerekir:

* Toplu İşlem hizmetinin, görevleri zamanlamaiçin havuz bilgi işlem düğümleriyle iletişim kurması gerekir. Bu iletişimi etkinleştirmek için, Toplu İş hesabınızın bulunduğu bölgede Toplu İşlem hizmeti tarafından kullanılan her IP adresi için kullanıcı tanımlı bir rota ekleyin. Toplu İşlem hizmetinin IP adreslerilistesini nasıl elde edebilirsiniz öğrenmek için [şirket içi Hizmet etiketlerine](../virtual-network/service-tags-overview.md) bakın

* Azure Depolama'ya giden trafiğin (özellikle formurlleri `<account>.table.core.windows.net`, `<account>.queue.core.windows.net`ve `<account>.blob.core.windows.net`) şirket içi ağ aygıtınız aracılığıyla engellenmediğinden emin olun.

Kullanıcı tanımlı bir rota eklediğinizde, ilgili her Toplu IP adresi öneki için rotayı tanımlayın ve **Sonraki atlama türünü** **Internet'e**ayarlayın. Aşağıdaki örneğe bakın:

![Kullanıcı tanımlı yol](./media/batch-virtual-network/user-defined-route.png)

## <a name="next-steps"></a>Sonraki adımlar

- Toplu İşleme'ye derinlemesine bir bakış için [bkz.](batch-api-basics.md)
- Kullanıcı tanımlı bir rota oluşturma hakkında daha fazla [şey](../virtual-network/tutorial-create-route-table-portal.md)için bkz.
