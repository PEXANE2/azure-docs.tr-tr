---
title: Sanal makinelerinize tarayıcı üzerinden bağlanma - Azure | Microsoft Dokümanlar
description: Sanal makinelerinize tarayıcı üzerinden nasıl bağlanıştırılanın öğrenin.
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
ms.openlocfilehash: 4549ee0f50c06040b70307c96b5ca96cd4162214
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974316"
---
# <a name="connect-to-your-virtual-machines-through-a-browser"></a>Sanal makinelerinize tarayıcı üzerinden bağlanma 

DevTest Labs, sanal makinelerinize bir tarayıcı üzerinden bağlanmanızı sağlayan [Azure Bastion](https://docs.microsoft.com/azure/bastion/)ile tümleşir. DevTest Labs'da bu özelliği nasıl etkinleştirin hakkında bilgi için laboratuvar [sanal makinelerinde tarayıcı bağlantısını etkinleştir mesuliyonu'na](enable-browser-connection-lab-virtual-machines.md)bakın.

Tarayıcı *bağlantısı* etkinleştirildikten sonra, laboratuvar kullanıcıları bir tarayıcı üzerinden sanal makinelere erişebilir.  

## <a name="create-a-lab-virtual-machine"></a>Laboratuvar sanal makinesi oluşturma

Öncelikle bastion üzerinde yapılandırılmış olan bir sanal ağ içinde laboratuvar sanal makine oluşturmanız gerekir. Oluşturduğunuz ikinci **alt ağı** seçin, AzureBastionSubnet'i değil. **Gelişmiş ayarlar** sekmesine giderek sanal makine oluşturma sırasında sanal bir ağ seçebilirsiniz.

![Sanal makine oluşturma](./media/connect-virtual-machine-through-browser/create-virtual-machine.png)

## <a name="launch-virtual-machine-in-a-browser"></a>Sanal makineyi tarayıcıda başlatın

Sanal makine oluşturulduktan sonra Tarayıcı *bağlantı* düğmesine tıklayarak ve makine için kullanıcı adınızı ve şifrenizi girerek tarayıcıda başlatabilirsiniz.  

![Tarayıcıda başlatma](./media/connect-virtual-machine-through-browser/browser-connect.png)

## <a name="next-steps"></a>Sonraki Adımlar

[Azure DevTest Labs'daki laboratuvara VM ekleme](devtest-lab-add-vm.md)
