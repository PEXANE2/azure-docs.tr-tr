---
title: Azure savunma kullanarak bir Linux VM 'ye bağlanma
description: Bu makalede, Azure savunma kullanarak Linux sanal makinesine nasıl bağlanacağınızı öğrenin.
services: bastion
author: cherylmc
ms.service: bastion
ms.topic: how-to
ms.date: 02/12/2021
ms.author: cherylmc
ms.openlocfilehash: 5d61c2a1a0f5d7b26809621af6dfa88cf5080320
ms.sourcegitcommit: e972837797dbad9dbaa01df93abd745cb357cde1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/14/2021
ms.locfileid: "100518190"
---
# <a name="connect-using-ssh-to-a-linux-virtual-machine-using-azure-bastion"></a>Azure savunma kullanarak bir Linux sanal makinesine SSH kullanarak bağlanma

Bu makalede, bir Azure sanal ağındaki Linux VM 'lerinize güvenli ve sorunsuz bir şekilde SSH ekleyebilirsiniz. Bir VM 'ye doğrudan Azure portal bağlanabilirsiniz. Azure savunma kullanılırken, VM 'Ler bir istemci, aracı veya ek yazılım gerektirmez. Azure savunma hakkında daha fazla bilgi için bkz. [genel bakış](bastion-overview.md).

SSH kullanarak bir Linux sanal makinesine bağlanmak için Azure savunma kullanabilirsiniz. Kimlik doğrulaması için hem Kullanıcı adı/parola hem de SSH anahtarlarını kullanabilirsiniz. VM 'nize şu iki kullanarak SSH anahtarlarıyla bağlanabilirsiniz:

* El ile girdiğiniz özel bir anahtar
* Özel anahtar bilgilerini içeren bir dosya

SSH özel anahtarı ile başlayan ve ile biten bir biçimde olmalıdır  `"-----BEGIN RSA PRIVATE KEY-----"` `"-----END RSA PRIVATE KEY-----"` .

## <a name="prerequisites"></a>Önkoşullar

VM 'nin bulunduğu sanal ağ için bir Azure savunma ana bilgisayarı ayarladığınızdan emin olun. Daha fazla bilgi için bkz. [Azure savunma Konağı oluşturma](./tutorial-create-host-portal.md). Savunma hizmeti, sanal ağınıza sağlandıktan ve dağıtıldıktan sonra, bu sanal ağdaki herhangi bir VM 'ye bağlanmak için kullanabilirsiniz. 

Bağlanmak için savunma kullandığınızda, Windows VM 'ye bağlanmak için RDP kullandığınızı ve Linux sanal makinelerinize bağlanmak için SSH 'yi kullandığınızı varsayar. Bir Windows VM 'ye bağlanma hakkında daha fazla bilgi için bkz. [VM 'ye bağlanma-Windows](bastion-connect-vm-rdp.md).

### <a name="required-roles"></a>Gerekli roller

Bir bağlantı oluşturmak için aşağıdaki roller gereklidir:

* Sanal makinede okuyucu rolü
* Sanal makinenin özel IP’si ile NIC’de okuyucu rolü
* Azure Bastion kaynağında okuyucu rolü

### <a name="ports"></a>Bağlantı noktaları

Linux VM 'sine SSH aracılığıyla bağlanmak için, VM 'niz üzerinde aşağıdaki bağlantı noktalarının açık olması gerekir:

* Gelen bağlantı noktası: SSH (22)

## <a name="connect-using-username-and-password"></a><a name="username"></a>Bağlan: Kullanıcı adı ve parola kullanma

1. [Azure portalını](https://portal.azure.com) açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.

   :::image type="content" source="./media/bastion-connect-vm-ssh/connect.png" alt-text="Ekran görüntüsünde, Connect seçiliyken Azure portal bir sanal makine için genel bakış gösterilmektedir":::
1. Savunma ' yı seçtikten sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız bkz. [yapılandırmayı yapılandırma](./tutorial-create-host-portal.md).

   :::image type="content" source="./media/bastion-connect-vm-ssh/bastion.png" alt-text="Ekran görüntüsü, savunma seçiliyken sanal makineye bağlan iletişim kutusunu gösterir":::
1. Sanal makinenize SSH için Kullanıcı adı ve parola girin.
1. Anahtarı girdikten sonra **Bağlan** düğmesini seçin.

## <a name="connect-manually-enter-a-private-key"></a><a name="privatekey"></a>Bağlan: El Ile özel bir anahtar girin

1. [Azure portalını](https://portal.azure.com) açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.

   :::image type="content" source="./media/bastion-connect-vm-ssh/connect.png" alt-text="Ekran görüntüsünde, Connect seçiliyken Azure portal bir sanal makine için genel bakış gösterilmektedir":::
1. Savunma ' yı seçtikten sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız bkz. [yapılandırmayı yapılandırma](./tutorial-create-host-portal.md).

   :::image type="content" source="./media/bastion-connect-vm-ssh/bastion.png" alt-text="Seçili savunma ile sanal makineye bağlan iletişim kutusu.":::
1. Kullanıcı adını girin ve **SSH özel anahtarı**' nı seçin.
1. Özel anahtarınızı **SSH özel anahtarı** metin alanına girin (veya doğrudan yapıştırın).
1. Anahtarı girdikten sonra **Bağlan** düğmesini seçin.

## <a name="connect-using-a-private-key-file"></a><a name="ssh"></a>Bağlan: özel anahtar dosyası kullanma

1. [Azure portalını](https://portal.azure.com) açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.

   :::image type="content" source="./media/bastion-connect-vm-ssh/connect.png" alt-text="Seçili bağla":::
1. Savunma ' yı seçtikten sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız bkz. [yapılandırmayı yapılandırma](./tutorial-create-host-portal.md).

   :::image type="content" source="./media/bastion-connect-vm-ssh/bastion.png" alt-text="Seçili.":::
1. Kullanıcı adını girin ve **yerel dosyadan SSH özel anahtarı**' nı seçin.
1. **Tarayıcı** düğmesini (yerel dosyadaki klasör simgesi) seçin.
1. Dosyaya gözatıp **Aç**' ı seçin.
1. SANAL makineye bağlanmak için **Bağlan** ' ı seçin. Bağlan ' a tıkladığınızda bu sanal makineye SSH doğrudan Azure portal açılır. Bu bağlantı, sanal makinenizin özel IP 'si üzerinde savunma hizmetinde 443 numaralı bağlantı noktasını kullanan HTML5 'in üstünden fazla.

## <a name="connect-using-a-private-key-stored-in-azure-key-vault"></a><a name="akv"></a>Bağlan: Azure Key Vault depolanan bir özel anahtar kullanma

>[!NOTE]
>Bu özellik için Portal güncelleştirmesi şu anda bölgelere kullanıma alınıyor.
>

1. [Azure portalını](https://portal.azure.com) açın. Bağlanmak istediğiniz sanal makineye gidin, ardından **Bağlan** ' a tıklayın ve **açılan listeden savunma** ' yi seçin.
1. Savunma ' yı seçtikten sonra, üç sekmeye (RDP, SSH ve savunma) sahip bir yan çubuk görüntülenir. Sanal ağ için bir savunma sağlandıysa, savunma sekmesi varsayılan olarak etkindir. Sanal ağ için savunma sağlamadıysanız bkz. [yapılandırmayı yapılandırma](bastion-create-host-portal.md).

   :::image type="content" source="./media/bastion-connect-vm-ssh/bastion.png" alt-text="Savunma sekmesi":::
1. Kullanıcı adını girin ve **Azure Key Vault SSH özel anahtarı**' nı seçin.
1. **Azure Key Vault** açılan listesini SEÇIN ve SSH özel anahtarınızı depoladığınız kaynağı seçin. Bir Azure Key Vault kaynağı ayarlamadıysanız, bkz. [Anahtar Kasası oluşturma](../key-vault/general/quick-create-portal.md) ve SSH özel anahtarınızı yeni bir Key Vault parolasının değeri olarak depolama.

   :::image type="content" source="./media/bastion-connect-vm-ssh/key-vault.png" alt-text="Azure Key Vault":::

Key Vault kaynağında saklanan gizli  dizileri listeleyin **ve erişiminizin** olduğundan emin olun. Key Vault kaynağınız için erişim ilkeleri atamak ve değiştirmek için, bkz. [Key Vault erişim Ilkesi atama](../key-vault/general/assign-access-policy-portal.md).

1. Gizli dizi listesini **Azure Key Vault** SEÇIN ve SSH özel anahtarınızın değerini içeren Key Vault gizli anahtarını seçin.
1. SANAL makineye bağlanmak için **Bağlan** ' ı seçin. Bağlan ' a tıkladığınızda bu sanal makineye SSH doğrudan Azure portal açılır. Bu bağlantı, sanal makinenizin özel IP 'si üzerinde savunma hizmetinde 443 numaralı bağlantı noktasını kullanan HTML5 'in üstünden fazla.

## <a name="next-steps"></a>Sonraki adımlar

Savunma [hakkında SSS](bastion-faq.md) makalesini okuyun
