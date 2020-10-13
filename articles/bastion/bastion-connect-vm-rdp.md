---
title: Azure savunma kullanarak bir Windows VM 'ye bağlanma
description: Bu makalede, Azure savunma kullanarak Windows çalıştıran bir Azure sanal makinesine nasıl bağlanacağınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 10/12/2020
ms.author: cherylmc
ms.openlocfilehash: 9c9bac20beb415f8bc29ca63d530e5cd8492d2d3
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91997008"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Azure savunma kullanarak bir Windows sanal makinesine bağlanma

Azure savunma 'yı kullanarak, sanal makinelerinize doğrudan Azure portal SSL üzerinden güvenli bir şekilde bağlanabilir ve sorunsuz bir şekilde bağlanabilirsiniz. Azure savunma kullandığınızda, sanal makinelerinize bir istemci, aracı veya ek yazılım gerekmez. Bu makalede, Windows sanal makinelerinize nasıl bağlanabilmeniz gösterilmektedir. Linux VM 'ye bağlanma hakkında daha fazla bilgi için bkz. [LINUX VM 'ye bağlanma](bastion-connect-vm-ssh.md).

Azure savunma, sağlandığı sanal ağdaki tüm VM 'lere güvenli bağlantı sağlar. Azure savunma 'nın kullanılması, sanal makinelerinizin, RDP/SSH bağlantı noktalarını dış dünyaya sunulmasını sağlarken RDP/SSH kullanarak güvenli erişim sağlamaya devam eder. Daha fazla bilgi için bkz. [Azure](bastion-overview.md)savunma nedir?.

## <a name="before-you-begin"></a>Başlamadan önce

Başlamadan önce aşağıdaki ölçütleri karşıladığınızı doğrulayın:

* Savunma ana bilgisayarına sahip bir VNet zaten yüklü.

   VM 'nin bulunduğu sanal ağ için bir Azure savunma ana bilgisayarı ayarladığınızdan emin olun. Savunma hizmeti, sanal ağınıza sağlandıktan ve dağıtıldıktan sonra, sanal ağdaki herhangi bir VM 'ye bağlanmak için kullanabilirsiniz. Bir Azure savunma ana bilgisayarı kurmak için bkz. [bir savunma ana bilgisayarı oluşturma](tutorial-create-host-portal.md#createhost).
* Sanal ağdaki bir Windows sanal makinesi.
* Aşağıdaki gerekli roller:
  * Sanal makinede okuyucu rolü.
  * Sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü.
  * Azure savunma kaynağında okuyucu rolü.
* Bağlantı noktaları: Windows VM 'ye bağlanmak Için aşağıdaki bağlantı noktalarının Windows VM 'niz üzerinde açık olması gerekir:
  * Gelen bağlantı noktaları: RDP (3389)

## <a name="connect"></a><a name="rdp"></a>Bağlan

[!INCLUDE [Connect to a Windows VM](../../includes/bastion-vm-rdp.md)]
 
## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.