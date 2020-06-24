---
title: Azure DevTest Labs sanal makinelerde tarayıcı bağlantısını etkinleştir | Microsoft Docs
description: DevTest Labs artık Azure savunma ile tümleşir, bu da laboratuvarın sahibi olarak tüm laboratuar sanal makinelerine erişimi bir tarayıcıdan sağlayabilir.
services: devtest-lab,virtual-machines,lab-services
documentationcenter: na
author: tanmayeekamath
manager: femila
editor: ''
ms.service: lab-services
ms.workload: na
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 12/09/2019
ms.author: takamath
ms.openlocfilehash: 074d512767769fe434cd4a6d4f4e8e6a88f7ed2a
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84896056"
---
# <a name="enable-browser-connection-on-lab-virtual-machines"></a>Laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştir 
DevTest Labs, sanal makinelerinize bir tarayıcıdan bağlanmanızı sağlayan [Azure](https://docs.microsoft.com/azure/bastion/)savunma ile tümleşir. Önce laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştirmeniz gerekir.

Bir laboratuvarın sahibi olarak, tüm Laboratuvar sanal makinelerine bir tarayıcıdan erişmeyi etkinleştirebilirsiniz. Ek bir istemci, aracı veya yazılım parçası gerekmez. Azure savunma, doğrudan TLS üzerinden Azure portal sanal makinelerinize güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure savunma aracılığıyla bağlandığınızda, sanal makinelerinizin ortak bir IP adresi olması gerekmez. Daha fazla bilgi için bkz. Azure savunma nedir [?](../bastion/bastion-overview.md)


Bu makalede, laboratuvar sanal makinelerinde tarayıcı bağlantısının nasıl etkinleştirileceği gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar 
Mevcut laboratuvarınızın sanal ağında bir savunma ana bilgisayarı dağıtın **(veya)** laboratuvarınızı yapılandırılmış bir sanal ağla bağlayın. 

Bir savunma konağını sanal bir ağa dağıtmayı öğrenmek için bkz. [Azure savunma Konağı oluşturma](../bastion/bastion-create-host-portal.md). Savunma konağını oluştururken, laboratuvarın sanal ağını seçin. 

İlk olarak, AzureBastionSubnet 'de savunma kaynakları oluşturulmasına izin vermediğinden, savunma sanal ağında ikinci bir alt ağ oluşturmanız gerekir. 

## <a name="create-a-second-sub-net-in-the-bastion-virtual-network"></a>Savunma sanal ağında ikinci bir alt ağ oluşturun
Azure savunma alt ağında laboratuvar VM 'Leri oluşturamazsınız. Aşağıdaki görüntüde gösterildiği gibi, savunma sanal ağı içinde başka bir alt ağ oluşturun:

![Azure savunma sanal ağı 'nda ikinci alt ağ](./media/connect-virtual-machine-through-browser/second-subnet.png)

## <a name="enable-vm-creation-in-the-subnet"></a>Alt ağda VM oluşturmayı etkinleştirme
Şimdi, aşağıdaki adımları izleyerek bu alt ağdaki VM 'Leri oluşturmayı etkinleştirin: 

1. [Azure Portal](https://portal.azure.com) oturum açın.
1. Sol gezinti menüsünde **tüm hizmetler** ' i seçin. 
1. Listeden **DevTest Labs**'i seçin. 
1. Laboratuvarlar listesinden *laboratuvarınızı*seçin. 

    > [!NOTE]
    > Azure savunma artık şu bölgelerde genel kullanıma sunulmuştur: Batı ABD, Doğu ABD, Batı Avrupa, Orta Güney ABD, Avustralya Doğu ve Japonya Doğu. Bu nedenle, laboratuvarınız bunlardan birinde değilse, bu bölgelerden birinde bir laboratuvar oluşturun. 
    
1. Sol menüdeki **Ayarlar** bölümünde **yapılandırma ve ilkeler** ' i seçin. 
1. **Sanal ağları**seçin.
1. Araç çubuğundan **Ekle** ' yi seçin. 
1. Savunma konağının dağıtıldığı **sanal ağı** seçin. 
1. Daha önce oluşturduğunuz diğeri olan **AzureBastionSubnet**değil, VM 'ler için alt ağı seçin. En alttaki listede alt ağı görmüyorsanız sayfayı kapatın ve yeniden açın. 

    ![Alt ağda VM oluşturmayı etkinleştirme](./media/connect-virtual-machine-through-browser/enable-vm-creation-subnet.png)
1. **Sanal makine oluşturma seçeneğinde kullan '** ı seçin. 
1. Araç çubuğunda **Kaydet**’i seçin. 
1. Laboratuvar için eski bir sanal ağınız varsa, **...* seçeneğini belirleyerek kaldırın.  ve **kaldırın**. 

## <a name="enable-browser-connection"></a>Tarayıcı bağlantısını etkinleştir 

Laboratuvarda bir laboratuar sahibi olarak yapılandırılmış bir sanal ağa sahip olduktan sonra, laboratuvar sanal makinelerinde tarayıcı bağlantısı sağlayabilirsiniz.

Laboratuvar sanal makinelerinde tarayıcı bağlantısı sağlamak için şu adımları izleyin:

1. Azure portal *laboratuvara*gidin.
1. **Yapılandırma ve ilkeler '** i seçin.
1. **Ayarlar**' da, **tarayıcı bağlantısı**' nı seçin. Bu seçeneği görmüyorsanız, **yapılandırma ilkeleri** sayfasını kapatın ve yeniden açın. 

    ![Tarayıcı bağlantısını etkinleştir](./media/enable-browser-connection-lab-virtual-machines/browser-connect.png)

## <a name="next-steps"></a>Sonraki Adımlar
Bir tarayıcı kullanarak VM 'lerinize nasıl bağlanacağınızı öğrenmek için aşağıdaki makaleye bakın: [bir tarayıcı aracılığıyla sanal makinelerinize bağlanma](connect-virtual-machine-through-browser.md)
