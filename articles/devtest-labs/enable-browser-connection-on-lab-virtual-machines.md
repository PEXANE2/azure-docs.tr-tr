---
title: Azure DevTest Labs sanal makinelerde tarayıcı bağlantısını etkinleştir | Microsoft Docs
description: DevTest Labs artık Azure savunma ile tümleşir, bu da laboratuvarın sahibi olarak tüm laboratuar sanal makinelerine erişimi bir tarayıcıdan sağlayabilir.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 9b901d36907c8d2bbae866c38bbf1c2554e0f1da
ms.sourcegitcommit: 1d9f7368fa3dadedcc133e175e5a4ede003a8413
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/27/2020
ms.locfileid: "85482947"
---
# <a name="enable-browser-connection-on-lab-virtual-machines"></a>Laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştir 

DevTest Labs, sanal makinelerinize bir tarayıcıdan bağlanmanızı sağlayan [Azure](https://docs.microsoft.com/azure/bastion/)savunma ile tümleşir. Önce laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştirmeniz gerekir.

Bir laboratuvarın sahibi olarak, tüm Laboratuvar sanal makinelerine bir tarayıcıdan erişmeyi etkinleştirebilirsiniz. Ek bir istemci, aracı veya yazılım parçası gerekmez. Azure savunma, doğrudan TLS üzerinden Azure portal sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure savunma aracılığıyla bağlandığınızda, sanal makinelerinizin ortak bir IP adresi olması gerekmez. Daha fazla bilgi için bkz. Azure savunma nedir [?](../bastion/bastion-overview.md)

> [!NOTE]
> Laboratuvar sanal makinelerinde tarayıcı bağlantısının etkinleştirilmesi önizleme aşamasındadır.

Bu makalede, laboratuvar sanal makinelerinde tarayıcı bağlantısının nasıl etkinleştirileceği gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar 
Mevcut laboratuvarın sanal ağında bir savunma ana bilgisayarı dağıtın **(veya)** laboratuvarınızı yapılandırılmış bir VNET ile bağlayın. 

Bir savunma konağını VNet 'te dağıtmayı öğrenmek için bkz. [Azure savunma Konağı (Önizleme) oluşturma](../bastion/bastion-create-host-portal.md). Savunma konağını oluştururken, laboratuvarın sanal ağını seçin. 

Laboratuvarınızı yapılandırılmış bir sanal ağ ile nasıl bağlayacağınızı öğrenmek için bkz. [Azure DevTest Labs sanal ağı yapılandırma](devtest-lab-configure-vnet.md). Savunma konağının dağıtıldığı VNet 'i ve içinde **AzureBastionSubnet** seçin. Ayrıntılı adımlar aşağıda verilmiştir: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezinti menüsünde **tüm hizmetler** ' i seçin. 
1. Listeden **DevTest Labs**'i seçin. 
1. Laboratuvarlar listesinden *laboratuvarınızı*seçin. 

    > [!NOTE]
    > Azure savunma Şu anda önizleme aşamasındadır. Şu bölgelerle sınırlıdır: Batı ABD, Doğu ABD, Batı Avrupa, Orta Güney ABD, Avustralya Doğu ve Japonya Doğu. Bu nedenle, laboratuvarınız bunlardan birinde değilse, bu bölgelerden birinde bir laboratuvar oluşturun. 
1. Sol menüdeki **Ayarlar** bölümünde **yapılandırma ve ilkeler** ' i seçin. 
1. **Sanal ağları**seçin.
1. Araç çubuğundan **Ekle** ' yi seçin. 
1. Savunma konağının dağıtıldığı **VNET** 'i seçin. 
1. Alt ağı seçin: **AzureBastionSubnet**. 

    ![Alt ağ](./media/enable-browser-connection-lab-virtual-machines/subnet.png)
1. **Sanal makine oluşturma seçeneğinde kullan '** ı seçin. 
1. Araç çubuğunda **Kaydet**’i seçin. 
1. Laboratuvar için eski bir VNet 'iniz varsa, **...* seçeneğini belirleyerek kaldırın.  ve **kaldırın**. 

## <a name="enable-browser-connection"></a>Tarayıcı bağlantısını etkinleştir 

Laboratuvarda laboratuvar sahibi olarak yapılandırılmış VNet 'e sahip olduktan sonra, laboratuvar sanal makinelerinde tarayıcı bağlantısı sağlayabilirsiniz.

Laboratuvar sanal makinelerinde tarayıcı bağlantısı sağlamak için şu adımları izleyin:

1. Azure portal *laboratuvara*gidin.
1. **Yapılandırma ve ilkeler '** i seçin.
1. **Ayarlar**' da **tarayıcı bağlantısı (Önizleme)** öğesini seçin.

![Tarayıcı bağlantısını etkinleştir](./media/enable-browser-connection-lab-virtual-machines/browser-connect.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bir tarayıcı kullanarak VM 'lerinize nasıl bağlanacağınızı öğrenmek için aşağıdaki makaleye bakın: [bir tarayıcı aracılığıyla sanal makinelerinize bağlanma](connect-virtual-machine-through-browser.md)