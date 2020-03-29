---
title: Azure DevTest Labs'da sanal ağı yapılandırma | Microsoft Dokümanlar
description: Varolan bir sanal ağı ve alt ağı nasıl yapılandırılayacağınızı ve Azure DevTest Labs ile bunları VM'de nasıl kullanacağınızı öğrenin
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: spelluru
manager: femila
editor: ''
ms.assetid: 6cda99c2-b87e-4047-90a0-5df10d8e9e14
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 09/05/2019
ms.author: spelluru
ms.openlocfilehash: 6cf3d2f82c98a3caab47ff48a600316747932b72
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "70390042"
---
# <a name="configure-a-virtual-network-in-azure-devtest-labs"></a>Azure DevTest Labs'da sanal ağı yapılandırma
Makalede açıklandığı gibi [bir laboratuvara VM ekle](devtest-lab-add-vm.md), laboratuarda bir VM oluşturduğunuzda, yapılandırılmış bir sanal ağ belirtebilirsiniz. Örneğin, ExpressRoute veya siteden siteye VPN ile yapılandırılan sanal ağı kullanarak Sanal M'lerinizden corpnet kaynaklarınıza erişmeniz gerekebilir.

Bu makalede, vm oluştururken seçim yapabilmek için mevcut sanal ağınızın bir laboratuvarın Sanal Ağ ayarlarına nasıl eklenebileceği açıklanmaktadır.

> [!NOTE]
> Azure Sanal Ağ hizmetiyle ilişkili maliyetler hakkında bilgi edinmek için Azure [Sanal Ağ Fiyatlandırması'na](../virtual-network/virtual-networks-overview.md#pricing)bakın.

## <a name="configure-a-virtual-network-for-a-lab-using-the-azure-portal"></a>Azure portalını kullanarak bir laboratuvar için sanal ağı yapılandırma
Aynı laboratuvarda VM oluştururken kullanılabilen bir laboratuvara varolan sanal ağ (ve alt ağ) eklemede aşağıdaki adımlar size yol kat eder. 

1. [Azure portalında](https://go.microsoft.com/fwlink/p/?LinkID=525040)oturum açın.
1. **Tüm Hizmetler'i**seçin ve ardından listeden **DevTest Labs'ı** seçin.
1. Laboratuvarlar listesinden istediğiniz laboratuarı seçin. 
1. Laboratuvarın ana bölmesine Yapılandırma **ve ilkeler'i**seçin.

    ![Laboratuvarın yapılandırmave ilkelerine erişin](./media/devtest-lab-configure-vnet/policies-menu.png)
1. DıŞ **KAYNAKLAR** bölümünde **Sanal ağları**seçin. Geçerli laboratuvar için yapılandırılan sanal ağların listesi ve laboratuvarınız için oluşturulan varsayılan sanal ağ görüntülenir. 
1. **+ Ekle** öğesini seçin.
   
    ![Laboratuvarınıza varolan bir sanal ağ ekleme](./media/devtest-lab-configure-vnet/lab-settings-vnet-add.png)
1. Sanal **ağ** bölmesinde **[Sanal ağı seç]** seçeneğini belirleyin.
   
    ![Varolan bir sanal ağı seçin](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet1.png)
1. Sanal **ağ** bölmesini seç'te, istediğiniz sanal ağı seçin. Abonelikte aynı bölge altında bulunan tüm sanal ağları gösteren bir liste görüntülenir.
1. Sanal ağ seçtikten sonra, Sanal **ağ** bölmesine döndürülür. En alttaki listedeki alt ağı seçin.

    ![Alt ağ listesi](./media/devtest-lab-configure-vnet/lab-settings-vnets-vnet2.png)
    
    Lab Subnet bölmesi görüntülenir.

    ![Laboratuvar alt ağ bölmesi](./media/devtest-lab-configure-vnet/lab-subnet.png)
     
   - Bir **Laboratuar alt net adı**belirtin.
   - Bir alt netin laboratuvar VM oluşturmada kullanılmasına izin vermek **için, sanal makine oluşturmada kullan'ı**seçin.
   - Paylaşılan genel IP adresini etkinleştirmek için paylaşılan genel [IP'yi](devtest-lab-shared-ip.md) **etkinleştir'i**seçin.
   - Bir alt ağda genel IP adreslerine izin vermek **için, ortak IP oluşturmayı İzin ver'i'ni**seçin.
   - Kullanıcı **başına maksimum sanal makinelerde,** her alt ağ için kullanıcı başına maksimum VM'leri belirtin. Sınırsız sayıda VM istiyorsanız, bu alanı boş bırakın.
1. Lab Subnet bölmesini kapatmak için **Tamam'ı** seçin.
1. Sanal ağ bölmesini kapatmak için **Kaydet'i** seçin.

Artık sanal ağ yapılandırıldığına göre, VM oluşturulurken seçilebilir. VM'nin nasıl oluşturulup sanal ağ belirtin, makaleye bakın, [laboratuvara VM ekleyin.](devtest-lab-add-vm.md) 

Azure'un [Sanal Ağ Dokümantasyonu,](https://docs.microsoft.com/azure/virtual-network) bir VNet'in nasıl kurulup yönetilen ve şirket içi ağınıza nasıl bağlanılabilen dahil olmak üzere VNet'lerin nasıl kullanılacağı hakkında daha fazla bilgi sağlar.

[!INCLUDE [devtest-lab-try-it-out](../../includes/devtest-lab-try-it-out.md)]

## <a name="next-steps"></a>Sonraki adımlar
İstediğiniz sanal ağı laboratuvarınıza ekledikten sonra, bir sonraki adım [laboratuvarınıza bir VM eklemektir.](devtest-lab-add-vm.md)

