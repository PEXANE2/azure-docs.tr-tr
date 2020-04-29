---
title: Azure savunma kullanarak bir Linux VM 'ye bağlanma
description: Bu makalede, Azure savunma kullanarak Linux sanal makinesine nasıl bağlanacağınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: 381f45a51002188c72174cff83c26b829912a0b9
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77596836"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion"></a>Azure savunma kullanarak bir Linux sanal makinesine SSH kullanarak bağlanma

Bu makalede, bir Azure sanal ağındaki Linux VM 'lerinize güvenli ve sorunsuz bir şekilde SSH ekleyebilirsiniz. Bir VM 'ye doğrudan Azure portal bağlanabilirsiniz. Azure savunma kullanılırken, VM 'Ler bir istemci, aracı veya ek yazılım gerektirmez. Azure savunma hakkında daha fazla bilgi için bkz. [genel bakış](bastion-overview.md).

SSH kullanarak bir Linux sanal makinesine bağlanmak için Azure savunma kullanabilirsiniz. Kimlik doğrulaması için hem Kullanıcı adı/parola hem de SSH anahtarlarını kullanabilirsiniz. VM 'nize şu iki kullanarak SSH anahtarlarıyla bağlanabilirsiniz:

* El ile girdiğiniz özel bir anahtar
* Özel anahtar bilgilerini içeren bir dosya

SSH özel anahtarı ile başlayan `"-----BEGIN RSA PRIVATE KEY-----"` ve ile biten bir biçimde olmalıdır. `"-----END RSA PRIVATE KEY-----"`

## <a name="before-you-begin"></a>Başlamadan önce

VM 'nin bulunduğu sanal ağ için bir Azure savunma ana bilgisayarı ayarladığınızdan emin olun. Daha fazla bilgi için bkz. [Azure savunma Konağı oluşturma](bastion-create-host-portal.md). Savunma hizmeti, sanal ağınıza sağlandıktan ve dağıtıldıktan sonra, bu sanal ağdaki herhangi bir VM 'ye bağlanmak için kullanabilirsiniz. 

Bağlanmak için savunma kullandığınızda, Windows VM 'ye bağlanmak için RDP kullandığınızı ve Linux sanal makinelerinize bağlanmak için SSH 'yi kullandığınızı varsayar. Bir Windows VM 'ye bağlanma hakkında daha fazla bilgi için bkz. [VM 'ye bağlanma-Windows](bastion-connect-vm-rdp.md).

### <a name="required-roles"></a>Gerekli roller

Bir bağlantı oluşturmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP 'si ile NIC 'de okuyucu rolü
* Azure savunma kaynağında okuyucu rolü

### <a name="ports"></a>Bağlantı noktaları

Linux VM 'sine SSH aracılığıyla bağlanmak için, VM 'niz üzerinde aşağıdaki bağlantı noktalarının açık olması gerekir:

* Gelen bağlantı noktası: SSH (22)

## <a name="connect-using-username-and-password"></a><a name="username"></a>Bağlan: Kullanıcı adı ve parola kullanma

1. [Azure Portal](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.

   ![Bağlan](./media/bastion-connect-vm-ssh/connect.png)
1. Atlama ' ye tıkladıktan sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız bkz. [yapılandırmayı yapılandırma](bastion-create-host-portal.md).

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Sanal makinenize SSH için Kullanıcı adı ve parola girin.
1. Anahtarı girdikten sonra **Bağlan** düğmesine tıklayın.

## <a name="connect-manually-enter-a-private-key"></a><a name="privatekey"></a>Bağlan: El Ile özel bir anahtar girin

1. [Azure Portal](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.

   ![Bağlan](./media/bastion-connect-vm-ssh/connect.png)
1. Atlama ' ye tıkladıktan sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız bkz. [yapılandırmayı yapılandırma](bastion-create-host-portal.md).

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Kullanıcı adını girin ve **SSH özel anahtarı**' nı seçin.
1. Özel anahtarınızı **SSH özel anahtarı** metin alanına girin (veya doğrudan yapıştırın).
1. Anahtarı girdikten sonra **Bağlan** düğmesine tıklayın.

## <a name="connect-using-a-private-key-file"></a><a name="ssh"></a>Bağlan: özel anahtar dosyası kullanma

1. [Azure Portal](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.

   ![Bağlan](./media/bastion-connect-vm-ssh/connect.png)
1. Atlama ' ye tıkladıktan sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız bkz. [yapılandırmayı yapılandırma](bastion-create-host-portal.md).

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Kullanıcı adını girin ve **yerel dosyadan SSH özel anahtarı**' nı seçin.
1. (Yerel dosyadaki klasör simgesi **) düğmesine tıklayın** .
1. Dosyaya gözatıp açın, sonra **Aç**' a tıklayın.
1. SANAL makineye bağlanmak için **Bağlan** ' a tıklayın. Bağlan ' a tıkladığınızda bu sanal makineye SSH doğrudan Azure portal açılır. Bu bağlantı, sanal makinenizin özel IP 'si üzerinde savunma hizmetinde 443 numaralı bağlantı noktasını kullanan HTML5 'in üstünden fazla.

## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md) makalesini okuyun
