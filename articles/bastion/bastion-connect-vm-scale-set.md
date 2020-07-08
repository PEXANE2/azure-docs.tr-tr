---
title: Azure savunma kullanarak bir Windows sanal makine ölçek kümesine bağlanma | Microsoft Docs
description: Bu makalede, Azure savunma kullanarak bir Azure sanal makine ölçek kümesine bağlanmayı öğrenin.
services: bastion
author: charwen
ms.service: bastion
ms.topic: how-to
ms.date: 02/03/2020
ms.author: charwen
ms.openlocfilehash: e3dc7ce36e773b5a615b1abf4f50406fcb07826b
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84744315"
---
# <a name="connect-to-a-virtual-machine-scale-set-using-azure-bastion"></a>Azure savunma kullanarak bir sanal makine ölçek kümesine bağlanma

Bu makalede, Azure savunma kullanarak bir Azure sanal ağındaki Windows sanal makine ölçek kümesi örneğinizi güvenli ve sorunsuz bir şekilde RDP ile nasıl kullanabileceğiniz gösterilmektedir. Bir sanal makine ölçek kümesi örneğine doğrudan Azure portal bağlanabilirsiniz. Azure savunma kullanılırken, VM 'Ler bir istemci, aracı veya ek yazılım gerektirmez. Azure savunma hakkında daha fazla bilgi için bkz. [genel bakış](bastion-overview.md).

## <a name="before-you-begin"></a>Başlamadan önce

Sanal makine ölçek kümesinin bulunduğu sanal ağ için bir Azure savunma ana bilgisayarı ayarladığınızdan emin olun. Daha fazla bilgi için bkz. [Azure savunma Konağı oluşturma](bastion-create-host-portal.md). Savunma hizmeti, sanal ağınıza sağlandıktan ve dağıtıldıktan sonra, bu sanal ağdaki bir sanal makine ölçek kümesi örneğine bağlanmak için kullanabilirsiniz. Savunma, bir Windows sanal makine ölçek kümesine bağlanmak için RDP kullandığınızı ve Linux sanal makine ölçek kümesine bağlanmak için SSH kullandığınızı varsayar. Bir Linux VM bağlantısı hakkında daha fazla bilgi için bkz. [VM 'ye bağlanma-Linux](bastion-connect-vm-ssh.md).

## <a name="connect-using-rdp"></a><a name="rdp"></a>RDP kullanarak bağlanma

1. [Azure Portal](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makine ölçek kümesine gidin.

   ![navigate](./media/bastion-connect-vm-scale-set/1.png)
2. Bağlanmak istediğiniz sanal makine ölçek kümesi örneğine gidin, ardından **Bağlan**' ı seçin. RDP bağlantısı kullanılırken, sanal makine ölçek kümesi bir Windows sanal makine ölçek kümesi olmalıdır.

   ![sanal makine ölçek kümesi](./media/bastion-connect-vm-scale-set/2.png)
3. **Bağlan**' ı seçtikten sonra, üç SEKMEYE (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Yan çubukta **savunma sekmesini seçin** . Sanal ağ için bir savunma sağlamadıysanız, bağlantıyı yapılandırmak için bağlantıyı seçebilirsiniz. Yapılandırma yönergeleri için bkz. yapılandırmayı [yapılandırma](bastion-create-host-portal.md).

   ![Savunma sekmesi](./media/bastion-connect-vm-scale-set/3.png)
4. Savunma sekmesinde, sanal makine ölçek kümesi için Kullanıcı adı ve parola girin ve sonra **Bağlan**' ı seçin.

   ![bağlanmaya](./media/bastion-connect-vm-scale-set/4.png)
5. Bu sanal makineyle savunma aracılığıyla RDP bağlantısı, bağlantı noktası 443 ve savunma hizmeti kullanılarak doğrudan Azure portal (HTML5 üzerinden) açılır.

## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md)makalesini okuyun.