---
title: Azure DevTest Labs bir sanal ağ yapılandırma | Microsoft Docs
description: Var olan bir sanal ağı ve alt ağı yapılandırmayı ve Azure DevTest Labs bir VM 'de kullanmayı öğrenin
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 0819527cfc2a9dfd4e93226542acb7cb0c6d7273
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85482658"
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Azure DevTest Labs bir sanal ağ yapılandırma
[LABORATUVARA VM ekleme](devtest-lab-add-vm.md)makalesinde açıklandığı gibi, laboratuvarda bir VM oluşturduğunuzda, yapılandırılmış bir sanal ağ belirtebilirsiniz. Örneğin, ExpressRoute veya siteden siteye VPN ile yapılandırılmış sanal ağı kullanarak, VM 'lerinize Corpnet kaynaklarınıza erişmeniz gerekebilir.

Bu makalede, var olan sanal ağınızı bir laboratuvarın sanal ağ ayarlarına nasıl ekleyeceğiniz, bu sayede VM 'Ler oluşturulurken seçebileceğiniz açıklanır.

> [!NOTE]
> Azure sanal ağ hizmeti ile ilişkili maliyetler hakkında bilgi edinmek için bkz. [Azure sanal ağ fiyatlandırması](../virtual-network/virtual-networks-overview.md#pricing).

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Azure portal kullanarak laboratuvar için sanal ağ yapılandırma
Aşağıdaki adımlar, aynı laboratuvarda bir VM oluştururken kullanılabilmesi için mevcut bir sanal ağı (ve alt ağ) bir laboratuvara ekleme işleminde size kılavuzluk eder. 

1. [Azure Portal](https://go.microsoft.com/fwlink/p/?LinkID=525040) oturum açın.
1. **Tüm hizmetler**' i seçin ve ardından listeden **DevTest Labs** ' i seçin.
1. Laboratuvarlar listesinden istediğiniz Laboratuvarı seçin. 
1. Laboratuvarın ana bölmesinde **yapılandırma ve ilkeler**' i seçin.

    ![Laboratuvarın yapılandırmasına ve ilkelerine erişin](./media/devtest-lab-configure-vnet/policies-menu.png)
1. **Dış kaynaklar** bölümünde, **sanal ağlar**' ı seçin. Geçerli laboratuvar için yapılandırılmış sanal ağların bir listesi, laboratuvarınız için oluşturulan varsayılan sanal ağ ile birlikte görüntülenir. 
1. **+ Ekle** öğesini seçin.
   
    ![Laboratuvarınızda var olan bir sanal ağ ekleyin](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
1. **Sanal ağ** bölmesinde **[sanal ağ seçin]** öğesini seçin.
   
    ![Var olan bir sanal ağı seçin](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
1. **Sanal ağ seçin** bölmesinde istediğiniz sanal ağı seçin. Abonelikte laboratuvar olarak aynı bölgedeki tüm sanal ağların gösterildiği bir liste görüntülenir.
1. Bir sanal ağ seçildikten sonra, **sanal ağ** bölmesine döndürülürsünüz. Alt kısımdaki listede alt ağı seçin.

    ![Alt ağ listesi](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Laboratuvar alt ağı bölmesi görüntülenir.

    ![Laboratuvar alt ağı bölmesi](./media/devtest-lab-configure-vnet/lab-subnet.png)
     
   - Bir **Laboratuvar alt ağ adı**belirtin.
   - Laboratuvar VM 'si oluşturmada bir alt ağın kullanılmasına izin vermek için, **sanal makine oluştururken kullan**' ı seçin.
   - [Paylaşılan genel IP adresini](devtest-lab-shared-ip.md)etkinleştirmek için **paylaşılan ortak IP 'yi etkinleştir**' i seçin.
   - Bir alt ağda genel IP adreslerine izin vermek için **genel IP oluşturmaya Izin ver**' i seçin.
   - **Kullanıcı başına en fazla sanal makine** alanı alanında her alt ağ için Kullanıcı başına en fazla VM sayısını belirtin. Sınırsız sayıda VM istiyorsanız, bu alanı boş bırakın.
1. Laboratuvar alt ağı bölmesini kapatmak için **Tamam ' ı** seçin.
1. Sanal ağ bölmesini kapatmak için **Kaydet** ' i seçin.

Sanal ağ yapılandırıldığına göre, VM oluşturulurken seçilebilir. VM oluşturma ve sanal ağ belirtme hakkında bilgi için bkz. bir [LABORATUVARA VM ekleme](devtest-lab-add-vm.md). 

Azure 'un [sanal ağ belgeleri](https://docs.microsoft.com/azure/virtual-network) , VNET 'i ayarlama ve yönetme ve şirket içi ağınıza bağlama dahil olmak üzere sanal ağları kullanma hakkında daha fazla bilgi sağlar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
İstediğiniz sanal ağı laboratuvara ekledikten sonra, bir sonraki adım [laboratuvara BIR VM eklemektir](devtest-lab-add-vm.md).

