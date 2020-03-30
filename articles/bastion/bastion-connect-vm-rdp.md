---
title: Azure Bastion'u kullanarak Windows VM'ye bağlanma
description: Bu makalede, Azure Bastion'u kullanarak Windows çalıştıran bir Azure Sanal Makine'ye nasıl bağlanışlarınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: 6ec60dc313c8a4374637adf38ea0e5a7d4ed964b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77597362"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Azure Bastion'u kullanarak Windows sanal makinesine bağlanma

Azure Bastion'u kullanarak sanal makinelerinize SSL üzerinden doğrudan Azure portalıüzerinden güvenli ve sorunsuz bir şekilde bağlanabilirsiniz. Azure Bastion'u kullandığınızda, VM'lerinizin istemci, aracı veya ek yazılım gerektirmemesi gerekir. Bu makalede, Windows VM'lerinize nasıl bağlanabileceğiniz gösterilmektedir. Bir Linux VM'ye bağlanma hakkında daha fazla bilgi için Azure [Bastion - Linux kullanarak Bir VM'ye Bağlan'a](bastion-connect-vm-ssh.md)bakın.

Azure Bastion, sağlandığı sanal ağdaki tüm VM'lere güvenli bağlantı sağlar. Azure Bastion'u kullanmak, sanal makinelerinizi RDP/SSH bağlantı noktalarını dış dünyaya maruz bırakmaktan korurken, RDP/SSH kullanarak güvenli erişim sağlar. Daha fazla bilgi için bkz. [Genel Bakış](bastion-overview.md).

## <a name="before-you-begin"></a>Başlamadan önce

VM'nin bulunduğu sanal ağ için bir Azure Bastion ana bilgisayarı ayarladığınızdan emin olun. Bastion hizmeti sağlanıp sanal ağınızda dağıtıldıktan sonra, sanal ağdaki herhangi bir VM'ye bağlanmak için kullanabilirsiniz. Azure Bastion ana bilgisayarı ayarlamak için [bkz.](bastion-create-host-portal.md)

### <a name="required-roles"></a>Gerekli roller

Bağlantı kurabilmek için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP'si ile NIC'de okuyucu rolü
* Azure Bastion kaynağında okuyucu rolü

### <a name="ports"></a>Bağlantı Noktaları

Windows VM'ye bağlanmak için Windows VM'nizde aşağıdaki bağlantı noktalarının açık olması gerekir:

* Gelen bağlantı noktaları: RDP (3389)

## <a name="connect"></a><a name="rdp"></a>Bağlanma

1. Azure [portalını](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan'ı** tıklatın ve açılır menüden **Bastion'u** seçin.

   ![VM bağlantısı](./media/bastion-connect-vm-rdp/connect.png)
1. Bastion'u tıklattıktan sonra, rdp, SSH ve Bastion olmak üzere üç sekmesi olan bir yan çubuk görüntülenir. Bastion sanal ağ için sağlanmışsa, Bastion sekmesi varsayılan olarak etkindir. Sanal ağ için Bastion'u sağlamadıysanız, Bastion'u yapılandırmak için bağlantıyı tıklatabilirsiniz. Yapılandırma yönergeleri için [bkz.](bastion-create-host-portal.md)

   ![burç sekmesi](./media/bastion-connect-vm-rdp/bastion.png)
1. Bastion sekmesine, sanal makinenizin kullanıcı adını ve parolasını girin ve ardından **Bağlan'ı**tıklatın. Bastion üzerinden bu sanal makineye RDP bağlantısı doğrudan Azure portalında (HTML5 üzerinden) bağlantı noktası 443 ve Bastion hizmetini kullanarak açılır.

   ![RDP bağlantısı](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Sonraki adımlar

Burç [SSS'sini](bastion-faq.md) okuyun
