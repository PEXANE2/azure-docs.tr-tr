---
title: Azure Bastion kullanarak Windows sanal makine ölçeğine bağlanma | Microsoft Dokümanlar
description: Bu makalede, Azure Bastion kullanarak bir Azure sanal makine ölçeği kümesine nasıl bağlanışsüreceğinizi öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: cherylmc
ms.openlocfilehash: 4f513aaf113ef4bd6e75e5c4b31e0f0252d45f10
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76988099"
---
# <a name="connect-to-a-virtual-machine-scale-set-using-azure-bastion"></a>Azure Bastion'u kullanarak sanal makine ölçeği kümesine bağlanma

Bu makalede, Azure Bastion'u kullanarak bir Azure sanal ağındaki Windows sanal makine ölçeği setinize güvenli ve sorunsuz bir şekilde RDP'yi nasıl belirleyebilirsiniz. Doğrudan Azure portalından sanal makine ölçeği seti örneğine bağlanabilirsiniz. Azure Bastion kullanırken, VM'ler istemci, aracı veya ek yazılım gerektirmez. Azure Kalesi hakkında daha fazla bilgi için [Genel Bakış'a](bastion-overview.md)bakın.

## <a name="before-you-begin"></a>Başlamadan önce

Sanal makine ölçeğinin bulunduğu sanal ağ için bir Azure Bastion ana bilgisayarı ayarladığınızdan emin olun. Daha fazla bilgi için [bkz.](bastion-create-host-portal.md) Bastion hizmeti sanal ağınızda sağlanıp dağıtıldıktan sonra, bu sanal ağdaki sanal makine ölçeği kümesi örneğine bağlanmak için kullanabilirsiniz. Bastion, Windows sanal makine ölçeği setine bağlanmak için RDP ve Linux sanal makine ölçek setinize bağlanmak için SSH kullandığınızı varsayar. Linux VM bağlantısı hakkında bilgi [için](bastion-connect-vm-ssh.md)bkz.

## <a name="connect-using-rdp"></a><a name="rdp"></a>RDP kullanarak bağlanma

1. Azure [portalını](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makine ölçeği kümesine gidin.

   ![navigate](./media/bastion-connect-vm-scale-set/1.png)
2. Bağlanmak istediğiniz sanal makine ölçeği seti örneğine gidin ve ardından **Bağlan'ı**seçin. RDP bağlantısı kullanırken, sanal makine ölçeği kümesi bir Windows sanal makine ölçeği kümesi olmalıdır.

   ![sanal makine ölçek seti](./media/bastion-connect-vm-scale-set/2.png)
3. **Connect'i**seçtikten sonra, rdp, SSH ve Bastion olmak üzere üç sekmesi olan bir yan çubuk görünür. Yan çubuktan **Bastion** sekmesini seçin. Sanal ağ için Bastion'u sağlamadıysanız, Bastion'u yapılandırmak için bağlantıyı seçebilirsiniz. Yapılandırma yönergeleri için [bkz.](bastion-create-host-portal.md)

   ![Burç sekmesi](./media/bastion-connect-vm-scale-set/3.png)
4. Bastion sekmesine, sanal makine ölçek kümenizin kullanıcı adını ve parolasını girin ve ardından **Bağlan'ı**seçin.

   ![bağlanmaya](./media/bastion-connect-vm-scale-set/4.png)
5. Bastion üzerinden bu sanal makineye RDP bağlantısı doğrudan Azure portalında (HTML5 üzerinden) bağlantı noktası 443 ve Bastion hizmetini kullanarak açılır.

## <a name="next-steps"></a>Sonraki adımlar

[Bastion SSS'yi](bastion-faq.md)okuyun.