---
title: Azure Bastion'u kullanarak Bir Linux VM'ine bağlanma
description: Bu makalede, Azure Bastion'u kullanarak Linux Virtual Machine'e nasıl bağlanışlarınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: conceptual
ms.date: 02/24/2020
ms.author: cherylmc
ms.openlocfilehash: 381f45a51002188c72174cff83c26b829912a0b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77596836"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion"></a>Azure Bastion'u kullanarak SSH kullanarak Linux sanal makinesine bağlanın

Bu makalede, bir Azure sanal ağında Linux VM'lerinize güvenli ve sorunsuz bir şekilde SSH'yi nasıl ekleyip sağlayamadığınızı gösterir. Doğrudan Azure portalından bir VM'ye bağlanabilirsiniz. Azure Bastion kullanırken, VM'ler istemci, aracı veya ek yazılım gerektirmez. Azure Kalesi hakkında daha fazla bilgi için [Genel Bakış'a](bastion-overview.md)bakın.

Azure Bastion'u Kullanarak Bir Linux sanal makinesine bağlanmak için SSH'yi kullanabilirsiniz. Kimlik doğrulaması için hem kullanıcı adı/parola hem de SSH tuşlarını kullanabilirsiniz. Aşağıdakilerden birini kullanarak SSH tuşları ile VM'nize bağlanabilirsiniz:

* El ile girdiğiniz özel bir anahtar
* Özel anahtar bilgilerini içeren bir dosya

SSH özel anahtarı ile `"-----BEGIN RSA PRIVATE KEY-----"` başlayan ve `"-----END RSA PRIVATE KEY-----"`onunla biten bir biçimde olmalıdır.

## <a name="before-you-begin"></a>Başlamadan önce

VM'nin bulunduğu sanal ağ için bir Azure Bastion ana bilgisayarı ayarladığınızdan emin olun. Daha fazla bilgi için [bkz.](bastion-create-host-portal.md) Bastion hizmeti sağlanıp sanal ağınızda dağıtıldıktan sonra, bu sanal ağdaki herhangi bir VM'ye bağlanmak için kullanabilirsiniz. 

Bağlanmak için Bastion'u kullandığınızda, Bir Windows VM'ye bağlanmak için RDP ve Linux VM'lerinize bağlanmak için SSH kullandığınızı varsayar. Windows VM'ye bağlanma hakkında daha fazla bilgi için [vm- Windows'a bağlan'a](bastion-connect-vm-rdp.md)bakın.

### <a name="required-roles"></a>Gerekli roller

Bağlantı kurmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP'si ile NIC'de okuyucu rolü
* Azure Bastion kaynağında okuyucu rolü

### <a name="ports"></a>Bağlantı Noktaları

Linux VM'ye SSH üzerinden bağlanabilmek için VM'nizde aşağıdaki bağlantı noktalarının açık olması gerekir:

* Gelen bağlantı noktası: SSH (22)

## <a name="connect-using-username-and-password"></a><a name="username"></a>Bağlan: Kullanıcı adı ve parola kullanma

1. Azure [portalını](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan'ı** tıklatın ve açılır menüden **Bastion'u** seçin.

   ![Bağlan](./media/bastion-connect-vm-ssh/connect.png)
1. Bastion'u tıklattıktan sonra, rdp, SSH ve Bastion olmak üzere üç sekmesi olan bir yan çubuk görüntülenir. Bastion sanal ağ için sağlanmışsa, Bastion sekmesi varsayılan olarak etkindir. Sanal ağ için Bastion'u sağlamadıysanız, [Bkz.](bastion-create-host-portal.md)

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Sanal makinenize SSH için kullanıcı adı ve şifre girin.
1. Tuşa girdikten sonra **Bağlan** düğmesini tıklatın.

## <a name="connect-manually-enter-a-private-key"></a><a name="privatekey"></a>Bağlan: Özel bir anahtarı el ile girin

1. Azure [portalını](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan'ı** tıklatın ve açılır menüden **Bastion'u** seçin.

   ![Bağlan](./media/bastion-connect-vm-ssh/connect.png)
1. Bastion'u tıklattıktan sonra, rdp, SSH ve Bastion olmak üzere üç sekmesi olan bir yan çubuk görüntülenir. Bastion sanal ağ için sağlanmışsa, Bastion sekmesi varsayılan olarak etkindir. Sanal ağ için Bastion'u sağlamadıysanız, [Bkz.](bastion-create-host-portal.md)

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Kullanıcı adını girin ve **SSH Private Key'i**seçin.
1. Özel anahtarınızı metin alanına girin **SSH Private Key** (veya doğrudan yapıştırın).
1. Tuşa girdikten sonra **Bağlan** düğmesini tıklatın.

## <a name="connect-using-a-private-key-file"></a><a name="ssh"></a>Bağlanma: Özel anahtar dosyakullanma

1. Azure [portalını](https://portal.azure.com)açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan'ı** tıklatın ve açılır menüden **Bastion'u** seçin.

   ![Bağlan](./media/bastion-connect-vm-ssh/connect.png)
1. Bastion'u tıklattıktan sonra, rdp, SSH ve Bastion olmak üzere üç sekmesi olan bir yan çubuk görüntülenir. Bastion sanal ağ için sağlanmışsa, Bastion sekmesi varsayılan olarak etkindir. Sanal ağ için Bastion'u sağlamadıysanız, [Bkz.](bastion-create-host-portal.md)

   ![Bastion](./media/bastion-connect-vm-ssh/bastion.png)
1. Kullanıcı adını girin ve **Yerel Dosya'dan SSH Private Key'i**seçin.
1. **Gözat** düğmesini (yerel dosyadaki klasör simgesi) tıklatın.
1. Dosyaya göz atın, ardından **Aç'ı**tıklatın.
1. VM'ye bağlanmak için **Bağlan'ı** tıklatın. Bağlan'ı tıklattığınızda, bu sanal makineye SSH doğrudan Azure portalında açılır. Bu bağlantı HTML5 üzerinde sanal makinenin özel IP üzerinden Bastion hizmeti port 443 kullanarak.

## <a name="next-steps"></a>Sonraki adımlar

Burç [SSS'sini](bastion-faq.md) okuyun
