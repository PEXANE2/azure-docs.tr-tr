---
title: Azure DevTest Labs sanal makinelerinde tarayıcı bağlantısını etkinleştirin | Microsoft Dokümanlar
description: DevTest Labs artık Azure Bastion ile entegre olarak, bir tarayıcı üzerinden tüm laboratuvar sanal makinelerine erişmesini etkinleştirebileceğiniz laboratuvarın sahibi olarak.
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
ms.date: 08/19/2019
ms.author: takamath
ms.openlocfilehash: 86304fc7776f49c999924b8609f2d26120cee372
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549093"
---
# <a name="enable-browser-connection-on-lab-virtual-machines"></a>Laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştirme 

DevTest Labs, sanal makinelerinize bir tarayıcı üzerinden bağlanmanızı sağlayan [Azure Bastion](https://docs.microsoft.com/azure/bastion/)ile tümleşir. Öncelikle laboratuvar sanal makinelerde tarayıcı bağlantısını etkinleştirmeniz gerekir.

Bir laboratuvarın sahibi olarak, tüm laboratuvar sanal makinelerine bir tarayıcı üzerinden erişmesini sağlayabilirsiniz. Ek bir istemciye, aracıya veya yazılım parçasına ihtiyacınız yoktur. Azure Bastion, TLS üzerinden sanal makinelerinize doğrudan Azure portalında güvenli ve sorunsuz RDP/SSH bağlantısı sağlar. Azure Bastion üzerinden bağlandığınızda, sanal makinelerinizin herkese açık bir IP adresine ihtiyacı yoktur. Daha fazla bilgi için azure [kalesi nedir?](../bastion/bastion-overview.md)

> [!NOTE]
> Laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştirmek önizlemededir.

Bu makalede, laboratuvar sanal makinelerde tarayıcı bağlantısı etkinleştirmek için nasıl gösterir.

## <a name="prerequisites"></a>Ön koşullar 
Mevcut laboratuvarınızın sanal ağına **(OR)** bir Bastion ana bilgisayar dağıtın, laboratuvarınızı yapılandırılmış bir VNet ile bağlayın. 

Bir VNet'te Bir Bastion ana bilgisayarınnasıl dağıtılangerektiğini öğrenmek için [bkz.](../bastion/bastion-create-host-portal.md) Bastion ana bilgisayarını oluştururken, laboratuvarın sanal ağını seçin. 

Laboratuvarınızı Bastion yapılandırılmış bir VNet'e nasıl bağlayacağınızı öğrenmek için Azure [DevTest Labs'da sanal ağı Yapılandırma'ya](devtest-lab-configure-vnet.md)bakın. Bastion ana bilgisayarını dağıtılan VNet'i ve içinde **AzureBastionSubnet'i** seçin. Aşağıda ayrıntılı adımlar veörnekler yer alıyor: 

1. [Azure portalında](https://portal.azure.com)oturum açın.
1. Sol navigasyon menüsünden **Tüm Hizmetler'i** seçin. 
1. Listeden **DevTest Labs**'i seçin. 
1. Laboratuvarlar listesinden, *laboratuvarınızı*seçin. 

    > [!NOTE]
    > Azure Bastion şu anda önizlemede. Aşağıdaki bölgelerle sınırlıdır: Batı ABD, Doğu ABD, Batı Avrupa, Güney Orta ABD, Avustralya Doğu ve Japonya Doğu. Eğer laboratuvarınız bunlardan birinde değilse, bu bölgelerden birinde bir laboratuvar oluşturun. 
1. Sol menüdeki **Ayarlar** bölümünde **Yapılandırma ve ilkeler'i** seçin. 
1. **Sanal ağları**seçin.
1. Araç çubuğundan **Ekle'yi** seçin. 
1. Bastion ana bilgisayarını dağıtan **VNet'i** seçin. 
1. Alt ağı seçin: **AzureBastionSubnet**. 

    ![Alt ağ](./media/enable-browser-connection-lab-virtual-machines/subnet.png)
1. **Sanal makine oluşturma seçeneğinde Kullan'ı** seçin. 
1. Araç çubuğunda **Kaydet**’i seçin. 
1. Eğer laboratuvar için eski bir VNet varsa, * seçerek kaldırın *...*  ve **kaldırın.** 

## <a name="enable-browser-connection"></a>Tarayıcı bağlantısını etkinleştirme 

Laboratuvar içinde bir Bastion yapılandırılmış VNet sonra, bir laboratuvar sahibi olarak, tarayıcı laboratuvar sanal makinelerde bağlanmak etkinleştirebilirsiniz.

Laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştirmek için aşağıdaki adımları izleyin:

1. Azure portalında *laboratuvarınıza*gidin.
1. **Yapılandırma ve ilkeleri**seçin.
1. **Ayarlar'da** **Tarayıcı bağlantısı (Önizleme)** seçeneğini belirleyin.

![Tarayıcı bağlantısını etkinleştirme](./media/enable-browser-connection-lab-virtual-machines/browser-connect.png)

## <a name="next-steps"></a>Sonraki Adımlar
Tarayıcı kullanarak SANAL M'lerinize nasıl bağlanışsüreceğinizi öğrenmek için aşağıdaki makaleye bakın: [Tarayıcı üzerinden sanal makinelerinize bağlanın](connect-virtual-machine-through-browser.md)