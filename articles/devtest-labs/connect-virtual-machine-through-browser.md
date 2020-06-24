---
title: Sanal makinelerinize bir tarayıcıdan bağlanın-Azure | Microsoft Docs
description: Sanal makinelerinize bir tarayıcıdan nasıl bağlanacağınızı öğrenin.
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
ms.sourcegitcommit: e3c28affcee2423dc94f3f8daceb7d54f8ac36fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84895615"
---
# <a name="connect-to-your-virtual-machines-through-a-browser"></a>Sanal makinelerinize bir tarayıcı üzerinden bağlanma 

DevTest Labs, sanal makinelerinize bir tarayıcıdan bağlanmanızı sağlayan [Azure](https://docs.microsoft.com/azure/bastion/)savunma ile tümleşir. DevTest Labs 'de bu özelliği etkinleştirme hakkında daha fazla bilgi için bkz. [Laboratuvar sanal makinelerinde tarayıcı bağlantısını etkinleştirme](enable-browser-connection-lab-virtual-machines.md).

*Tarayıcı bağlantısı* etkinleştirildikten sonra, laboratuar kullanıcıları sanal makinelere bir tarayıcıdan erişebilir.  

## <a name="create-a-lab-virtual-machine"></a>Laboratuvar sanal makinesi oluşturma

İlk olarak, üzerinde yapılandırılmış bir sanal ağ içinde laboratuvar sanal makinesini oluşturmanız gerekir. AzureBastionSubnet değil, oluşturduğunuz ikinci **alt ağı** seçin. **Gelişmiş ayarlar** sekmesine giderek, sanal makine oluşturma sırasında bir sanal ağ seçebilirsiniz.

![Sanal makine oluşturma](./media/connect-virtual-machine-through-browser/create-virtual-machine.png)

## <a name="launch-virtual-machine-in-a-browser"></a>Sanal makineyi bir tarayıcıda Başlat

Sanal makine oluşturulduktan sonra *tarayıcı Bağlan* düğmesine tıklayıp makine için Kullanıcı adınızı ve parolanızı girerek bir tarayıcıda başlatabilirsiniz.  

![Tarayıcıda Başlat](./media/connect-virtual-machine-through-browser/browser-connect.png)

## <a name="next-steps"></a>Sonraki Adımlar

[Azure DevTest Labs 'de laboratuvara VM ekleme](devtest-lab-add-vm.md)
