---
title: Azure DevTest Labs Paylaşılan IP adreslerini anlayın | Microsoft Docs
description: Azure DevTest Labs, laboratuvar sanal makinelerinize erişmek için gereken genel IP adreslerini en aza indirmek üzere paylaşılan IP adreslerini nasıl kullandığını öğrenin.
ms.topic: article
ms.date: 06/26/2020
ms.openlocfilehash: 36a5da1b2b6252d0adb480a622c461b33425e675
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85484103"
---
# <a name="understand-shared-ip-addresses-in-azure-devtest-labs"></a>Azure DevTest Labs Paylaşılan IP adreslerini anlayın

Azure DevTest Labs, laboratuar sanal makinelerinize erişim için gereken genel IP adresi sayısını en aza indirmek için laboratuvar VM 'lerinin aynı genel IP adresini paylaşmasına izin verir.  Bu makalede, paylaşılan IP 'Lerin nasıl çalıştığı ve ilgili yapılandırma seçeneklerinin nasıl çalıştığı açıklanır.

## <a name="shared-ip-setting"></a>Paylaşılan IP ayarı

Laboratuvar oluşturduğunuzda bir sanal ağın alt ağında oluşturulur.  Varsayılan olarak, bu alt ağ, **paylaşılan genel IP** kümesini *Evet*olarak etkinleştir seçeneğiyle oluşturulur.  Bu yapılandırma tüm alt ağ için tek bir genel IP adresi oluşturur.  Sanal ağları ve alt ağları yapılandırma hakkında daha fazla bilgi için bkz. [Azure DevTest Labs sanal ağı yapılandırma](devtest-lab-configure-vnet.md).

![Yeni laboratuvar alt ağı](media/devtest-lab-shared-ip/lab-subnet.png)

Mevcut laboratuvarlarda, **sanal ağlar > yapılandırma ve ilkeler**' i seçerek bu seçeneği etkinleştirebilirsiniz. Ardından, listeden bir sanal ağ seçin ve seçili alt ağ için **PAYLAŞıLAN ortak IP 'Yi etkinleştir** ' i seçin. Laboratuvar VM 'Leri genelinde genel IP adresini paylaştırmak istemiyorsanız bu seçeneği herhangi bir laboratuvarda devre dışı bırakabilirsiniz.

Bu laboratuvarda oluşturulan tüm sanal makineler, varsayılan olarak paylaşılan bir IP kullanmaktır.  VM oluştururken, bu ayar, **IP adresi yapılandırması**altındaki **Gelişmiş ayarlar** sayfasında gözlemlenebilir.

![Yeni VM](media/devtest-lab-shared-ip/new-vm.png)

- **Paylaşılan:** **Paylaşılan** olarak oluşturulan tüm VM 'ler bir kaynak grubuna (RG) yerleştirilir. Bu RG için tek bir IP adresi atanır ve RG 'deki tüm sanal makineler bu IP adresini kullanır.
- **Ortak:** Oluşturduğunuz her VM kendi IP adresine sahiptir ve kendi kaynak grubunda oluşturulur.
- **Özel:** Oluşturduğunuz her sanal makine özel bir IP adresi kullanır. Bu VM 'ye uzak masaüstü ile doğrudan internet üzerinden bağlanamazsınız.

Alt ağa Paylaşılan IP etkin bir VM eklendiğinde, DevTest Labs VM 'yi otomatik olarak bir yük dengeleyiciye ekler ve genel IP adresine bir TCP bağlantı noktası numarası atar ve VM 'deki RDP bağlantı noktasına iletim yapar.  

## <a name="using-the-shared-ip"></a>Paylaşılan IP 'yi kullanma

- **Linux kullanıcıları:** IP adresini veya tam etki alanı adını kullanarak VM 'ye SSH, ardından iki nokta üst üste ve ardından bağlantı noktası gelir. Örneğin, aşağıdaki görüntüde, sanal makineye bağlanmak için RDP adresi olur `mydevtestlab597975021002.eastus.cloudapp.azure.com:50661` .

  ![VM örneği](media/devtest-lab-shared-ip/vm-info.png)

- **Windows kullanıcıları:** Önceden yapılandırılmış bir RDP dosyasını indirmek ve sanal makineye erişmek için Azure portal **Bağlan** düğmesini seçin.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure DevTest Labs laboratuvar ilkelerini tanımlama](devtest-lab-set-lab-policy.md)
* [Azure DevTest Labs bir sanal ağ yapılandırma](devtest-lab-configure-vnet.md)





