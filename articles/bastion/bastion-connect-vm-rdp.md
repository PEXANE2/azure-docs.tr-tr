---
title: Azure savunma kullanarak bir Windows VM 'ye bağlanma
description: Bu makalede, Azure savunma kullanarak Windows çalıştıran bir Azure sanal makinesine nasıl bağlanacağınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 10/12/2020
ms.author: cherylmc
ms.openlocfilehash: 8b9653daf945b6a189bc528cd00de832ae97c03b
ms.sourcegitcommit: d103a93e7ef2dde1298f04e307920378a87e982a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91978161"
---
# <a name="connect-to-a-windows-virtual-machine-using-azure-bastion"></a>Azure savunma kullanarak bir Windows sanal makinesine bağlanma

Azure savunma 'yı kullanarak, sanal makinelerinize doğrudan Azure portal SSL üzerinden güvenli bir şekilde bağlanabilir ve sorunsuz bir şekilde bağlanabilirsiniz. Azure savunma kullandığınızda, sanal makinelerinize bir istemci, aracı veya ek yazılım gerekmez. Bu makalede, Windows sanal makinelerinize nasıl bağlanabilmeniz gösterilmektedir. Linux VM 'ye bağlanma hakkında daha fazla bilgi için bkz. [Azure savunma-Linux kullanarak VM 'ye bağlanma](bastion-connect-vm-ssh.md).

Azure savunma, sağlandığı sanal ağdaki tüm VM 'lere güvenli bağlantı sağlar. Azure savunma 'nın kullanılması, sanal makinelerinizin, RDP/SSH bağlantı noktalarını dış dünyaya sunulmasını sağlarken RDP/SSH kullanarak güvenli erişim sağlamaya devam eder. Daha fazla bilgi için bkz. [Genel Bakış](bastion-overview.md).

## <a name="before-you-begin"></a>Başlamadan önce

### <a name="install-the-bastion-host"></a>Savunma konağını yükler

VM 'nin bulunduğu sanal ağ için bir Azure savunma ana bilgisayarı ayarladığınızdan emin olun. Savunma hizmeti, sanal ağınıza sağlandıktan ve dağıtıldıktan sonra, sanal ağdaki herhangi bir VM 'ye bağlanmak için kullanabilirsiniz. Bir Azure savunma ana bilgisayarı kurmak için bkz. [Azure savunma Konağı oluşturma](bastion-create-host-portal.md).

### <a name="required-roles"></a>Gerekli roller

Bir bağlantı oluşturmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü
* Azure savunma kaynağında okuyucu rolü

### <a name="ports"></a>Bağlantı noktaları

Windows VM 'ye bağlanmak için aşağıdaki bağlantı noktalarının Windows VM 'niz üzerinde açık olması gerekir:

* Gelen bağlantı noktaları: RDP (3389)

## <a name="connect"></a><a name="rdp"></a>Bağlan

[!INCLUDE [Connect to a Windows VM](../../includes/bastion-vm-rdp.md)]
 
## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.
