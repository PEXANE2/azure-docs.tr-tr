---
title: VM'ler için özel IP adreslerini yapılandırma - Azure portalı
description: Azure portalını kullanarak sanal makineler için özel IP adreslerini nasıl yapılandırıştırmayı öğrenin.
services: virtual-network
documentationcenter: na
author: KumudD
manager: twooley
editor: tysonn
tags: azure-resource-manager
ms.assetid: 11245645-357d-4358-9a14-dd78e367b494
ms.service: virtual-network
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/07/2020
ms.author: kumud
ms.openlocfilehash: b50875105696dc5c556e2a4a9e756078cf995327
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80060411"
---
# <a name="configure-a-private-ip-address-for-a-vm-using-the-azure-portal"></a>Azure portalını kullanarak bir VM için özel bir IP adresi yapılandırma

[!INCLUDE [virtual-networks-static-private-ip-intro-include](../../includes/virtual-networks-static-private-ip-intro-include.md)]

[!INCLUDE [virtual-networks-static-ip-scenario-include](../../includes/virtual-networks-static-ip-scenario-include.md)]

Aşağıdaki örnek adımlar basit bir ortamın zaten oluşturulmasını bekler. Adımları bu belgede görüntülenen adımları çalıştırmak istiyorsanız, önce [sanal bir ağ oluşturun.](quick-create-portal.md#create-a-virtual-network) Ancak, adım 3'te, bunun yerine şu değerleri kullanın:

| Ayar | Değer |
| ------- | ----- |
| Adı | *TestVNet* |
| Adres alanı | *192.168.0.0/16* |
| Kaynak grubu | **TestRG** (gerekirse oluşturmak için **yeni oluştur'u** seçin) |
| Subnet - Adı | *FrontEnd* |
| Alt Ağ - Adres aralığı | *192.168.1.0/24* |

## <a name="create-a-vm-for-testing-static-private-ip-addresses"></a>Statik özel IP adreslerini test etmek için bir VM oluşturma
Kaynak Yöneticisi dağıtım modunda bir VM oluşturduğunuzda, Azure portalını kullanarak statik bir özel IP adresi ayarlayamazsınız. Bunun yerine, önce VM'yi oluşturursunuz. Sonra statik olarak kendi özel IP ayarlayabilirsiniz.

*TestVNet*adlı sanal bir ağın *FrontEnd* alt ağında *DNS01* adında bir VM oluşturmak için aşağıdaki adımları izleyin:

1. Azure [portalı](https://portal.azure.com) menüsünden **kaynak oluştur'u**seçin.

    ![Kaynak oluşturma, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/create-a-resource.png)
2. **Bilgi İşlem** > **Sanal makinesini**seçin.

    ![VM, Azure portalı oluşturma](./media/virtual-networks-static-ip-arm-pportal/compute-virtual-machine.png)
3. **Temel**Bilgiler'de, aşağıdaki tabloda açıklandığı gibi öğeleriçin değerleri belirtin. Sonra **Next&nbsp;&nbsp;seçin : Diskler** ve sonra **&nbsp;Sonraki :&nbsp;Ağ**.

    | Öğe | Değer |
    | --- | --- |
    | **Abonelik** | Geçerli aboneliğiniz |
    | **Kaynak grubu** | **TestRG** (açılır listeden seçin) |
    | **Sanal makine adı** | *DNS01* |
    | **Bölge** | **(Abd) Doğu ABD** |
    | **Görüntü** | **Windows Server 2019 Datacenter** |
    | **Boyut** | **B1ls** **VM Boyutu** , **Standart** **Sunan** |
    | **Username** | Yönetici hesabınızın kullanıcı adı |
    | **Parola** | Yönetici hesabınızın kullanıcı adının şifresi |
    | **Parolayı onayla** | Parola yine |

    ![Temel bilgiler sekmesi, Sanal makine oluşturma, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-basics.png)
4. **Ağ'da,** aşağıdaki tabloda açıklandığı gibi öğeler için değerleri belirtin ve sonra **İleri'yi**seçin.

    | Öğe | Değer |
    | --- | --- |
    | **Sanal ağ** | **TestVNet** |
    | **Alt ağ** | **FrontEnd** |

    ![Ağ sekmesi, Sanal makine oluşturma, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-networking.png)
5. **Yönetim,** **Tanıdepolama hesabı**altında, **vnetstorage**seçin. Bu depolama hesabı listede görünmüyorsa, **yeni oluştur'u**seçin , *vnetstorage* **adı** belirtin ve **Tamam'ı**seçin. Son olarak, **Gözden&nbsp;+&nbsp;Geçir'i seçin.**

    ![Yönetim sekmesi, Sanal makine oluşturma, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-management.png)
6. **Gözden Geçir + oluştur,** genel bakış bilgilerini gözden geçir ve ardından **Oluştur'u**seçin.

    ![Gözden geçirme + oluşturma sekmesi, Sanal makine oluşturma, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/create-a-virtual-machine-review-create.png)

VM oluşturulduktan sonra aşağıdaki ileti görüntülenir.

![Dağıtım tamamlama iletisi, Sanal makine oluşturma, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/deployment-is-complete.png)

## <a name="retrieve-private-ip-address-information-for-a-vm"></a>VM için özel IP adresi bilgilerini alma
Yeni VM'nizin özel IP adresi bilgilerini görüntülemek için:

1. VM'nizi bulmak için [Azure portalına](https://portal.azure.com) gidin. **Sanal makineleri**arayın ve seçin.

    ![Sanal makineler, Arama kutusu, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/search-box-virtual-machines.png)

2. Yeni VM 'nizin **(DNS01)** adını seçin.

    ![Sanal makine listesi, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/virtual-machine-list.png)

3. **Ağ'ı**seçin ve listelenen tek ağ arabirimini seçin.

    ![Ağ arabirimi, ağ, sanal makine, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/networking-network-interface.png)

4. **IP yapılandırmalarını**seçin ve tabloda listelenen IP yapılandırmasını seçin.

    ![IP yapılandırması, Ağ arabirimi, ağ, sanal makine, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/network-interface-ip-configurations.png)

5. **Özel IP adresi ayarlarında,** **TestVNet/FrontEnd** sanal ağ/alt net altında, **Atama** değerini **(Dinamik** veya **Statik)** ve **IP adresine**not alın.

    ![Dinamik veya statik atama, eski özel IP adresi ayarları, IP yapılandırması, Ağ arabirimi, ağ, sanal makine, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/private-ip-address-settings-old.png)

## <a name="add-a-static-private-ip-address-to-an-existing-vm"></a>Varolan bir VM'ye statik özel IP adresi ekleme
Yeni VM'nize statik özel BIR IP adresi eklemek için:

1. IP yapılandırma sayfasında, özel IP adresinizin atamasını **Statik**olarak ayarlayın.
2. Özel **IP adresinizi** *192.168.1.101*olarak değiştirin ve ardından **Kaydet'i**seçin.
   
    ![Dinamik veya statik atama, yeni özel IP adresi ayarları, IP yapılandırması, Ağ arabirimi, ağ, sanal makine, Azure portalı](./media/virtual-networks-static-ip-arm-pportal/private-ip-address-settings-new.png)

> [!NOTE]
> **Kaydet'i** seçtikten sonra atamanın hala **Dinamik**olarak ayarlandığını fark ederseniz, yazdığınız IP adresi zaten kullanılıyor. Başka bir IP adresi deneyin.

## <a name="remove-a-static-private-ip-address-from-a-vm"></a>Statik özel IP adresini VM'den kaldırma
Statik özel IP adresini VM'nizden kaldırmak için:

IP yapılandırma sayfasında, özel IP adresinizin atamasını **Dinamik**olarak ayarlayın ve ardından **Kaydet'i**seçin.

## <a name="set-ip-addresses-within-the-operating-system"></a>İşletim sistemi içindeki IP adreslerini ayarlama

Bir VM işletim sistemi içinden, Azure VM'ye atanan *özel* IP'yi statik olarak atamamalısınız. VM'lere [çok sayıda IP adresi atamak](virtual-network-multiple-ip-addresses-portal.md)gibi, gerektiğinde özel bir IP'nin statik atamasını yapın. İşletim sistemi içindeki özel IP adresini el ile ayarlarsanız, Azure [ağ arabirimine](virtual-network-network-interface-addresses.md#change-ip-address-settings)atanan özel IP adresiyle eşleştiğinden emin olun. Aksi takdirde, VM'ye bağlantı kaybedebilirsiniz. [Özel IP adresi](virtual-network-network-interface-addresses.md#private) ayarları hakkında daha fazla bilgi edinin.

Ayrıca, sanal makinenin işletim sistemi içinde bir Azure sanal makinesine atanan *genel* IP adresini asla el ile atamamalısınız.

## <a name="next-steps"></a>Sonraki adımlar

[IP adresi ayarlarını](virtual-network-network-interface-addresses.md)yönetme hakkında bilgi edinin.
