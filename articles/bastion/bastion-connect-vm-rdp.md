---
title: Azure savunma kullanarak bir Windows VM 'ye bağlanma
description: Bu makalede, Azure savunma kullanarak Windows çalıştıran bir Azure sanal makinesine nasıl bağlanacağınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: 6ec60dc313c8a4374637adf38ea0e5a7d4ed964b
ms.sourcegitcommit: 99ac4a0150898ce9d3c6905cbd8b3a5537dd097e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/25/2020
ms.locfileid: "77597362"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Azure savunma kullanarak bir Windows sanal makinesine bağlanma

Azure savunma 'yı kullanarak, sanal makinelerinize doğrudan Azure portal SSL üzerinden güvenli bir şekilde bağlanabilir ve sorunsuz bir şekilde bağlanabilirsiniz. Azure savunma kullandığınızda, sanal makinelerinize bir istemci, aracı veya ek yazılım gerekmez. Bu makalede, Windows sanal makinelerinize nasıl bağlanabilmeniz gösterilmektedir. Linux VM 'ye bağlanma hakkında daha fazla bilgi için bkz. [Azure savunma-Linux kullanarak VM 'ye bağlanma](bastion-connect-vm-ssh.md).

Azure savunma, sağlandığı sanal ağdaki tüm VM 'lere güvenli bağlantı sağlar. Azure savunma 'nın kullanılması, sanal makinelerinizin, RDP/SSH bağlantı noktalarını dış dünyaya sunulmasını sağlarken RDP/SSH kullanarak güvenli erişim sağlamaya devam eder. Daha fazla bilgi için bkz. [Genel Bakış](bastion-overview.md).

## <a name="before-you-begin"></a>Başlamadan önce

VM 'nin bulunduğu sanal ağ için bir Azure savunma ana bilgisayarı ayarladığınızdan emin olun. Savunma hizmeti, sanal ağınıza sağlandıktan ve dağıtıldıktan sonra, sanal ağdaki herhangi bir VM 'ye bağlanmak için kullanabilirsiniz. Bir Azure savunma ana bilgisayarı kurmak için bkz. [Azure savunma Konağı oluşturma](bastion-create-host-portal.md).

### <a name="required-roles"></a>Gerekli roller

Bir bağlantı oluşturmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü
* Azure savunma kaynağında okuyucu rolü

### <a name="ports"></a>Bağlantı Noktaları

Windows VM 'ye bağlanmak için aşağıdaki bağlantı noktalarının Windows VM 'niz üzerinde açık olması gerekir:

* Gelen bağlantı noktaları: RDP (3389)

## <a name="rdp"></a>Bağlanabilmeniz

1. [Azure portalını](https://portal.azure.com) açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.

   ![VM bağlantısı](./media/bastion-connect-vm-rdp/connect.png)
1. Atlama ' ye tıkladıktan sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için bir savunma sağlamadıysanız, bağlantıyı yapılandırmak için bağlantıya tıklayabilirsiniz. Yapılandırma yönergeleri için bkz. yapılandırmayı [yapılandırma](bastion-create-host-portal.md).

   ![Savunma sekmesi](./media/bastion-connect-vm-rdp/bastion.png)
1. Savunma sekmesinde, sanal makinenizin Kullanıcı adını ve parolasını girin ve ardından **Bağlan**' a tıklayın. Bu sanal makineyle savunma aracılığıyla RDP bağlantısı, bağlantı noktası 443 ve savunma hizmeti kullanılarak doğrudan Azure portal (HTML5 üzerinden) açılır.

   ![RDP bağlantısı](./media/bastion-connect-vm-rdp/443rdp.png)
 
## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md) makalesini okuyun
