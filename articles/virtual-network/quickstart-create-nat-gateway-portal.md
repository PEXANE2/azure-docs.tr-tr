---
title: 'Hızlı başlangıç: NAT ağ geçidi oluşturma - Azure portalı'
titlesuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıç, Azure portalını kullanarak bir NAT ağ geçidinin nasıl oluşturulacak olduğunu gösterir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: 1ff13d8ef0ca4c6cf499c3245d3ef14370283075
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "80066397"
---
# <a name="quickstart-create-a-nat-gateway-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak bir NAT ağ geçidi oluşturma

Bu hızlı başlangıç, Azure Sanal Ağ NAT hizmetini nasıl kullanacağınızı gösterir. Azure'daki sanal bir makine için giden bağlantı sağlamak için bir NAT ağ geçidi oluşturursunuz. 

İsterseniz, bu adımları [Azure CLI](quickstart-create-nat-gateway-cli.md), [Azure PowerShell](quickstart-create-nat-gateway-powershell.md)kullanarak yapabilir veya portal yerine bir [ARM Şablonu](quickstart-create-nat-gateway-powershell.md) dağıtabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure portalında](https://portal.azure.com)oturum açın.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bir VM dağıtmadan ve NAT ağ ağınızı kullanmadan önce kaynak grubu ve sanal ağ oluşturmamız gerekir.

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroupNAT |
| **\<sanal ağ adı>** | myVNet          |
| **\<bölge adı>**          | Doğu ABD 2      |
| **\<IPv4 adres-boşluk>**   | 192.168.0.0\16          |
| **\<subnet-name>**          | mySubnet        |
| **\<alt net-adres aralığı>** | 192.168.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-a-vm-to-use-the-nat-gateway"></a>NAT ağ geçidini kullanmak için bir VM oluşturma

Şimdi NAT hizmetini kullanmak için bir VM oluşturacağız. Bu VM, VM'ye erişmenizi sağlamak için örnek düzeyinde public IP olarak kullanabileceği ortak bir IP'ye sahiptir. NAT hizmeti akış yönü farkındadır ve alt anızdaki varsayılan Internet hedefinin yerini alır. VM'nin genel IP adresi giden bağlantılar için kullanılmaz.

1. Portalın sol üst tarafında, **bir kaynak** > **Oluştur Compute** > **Ubuntu Server 18.04 LTS'yi**seçin veya Market aramasında **Ubuntu Server 18.04 LTS'yi** arayın.

2. **Sanal bir makine oluştur'da,** **TemelLer** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupNAT'ı**seçin.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: **myVM**yazın.
   - **Örnek Ayrıntılar** > **Bölge** > Doğu **ABD 2**seçin.
   - **Yönetici hesabı** > **Kimlik Doğrulama türü**: **Parola'yı**seçin.
   - **Yönetici hesabı** > **Kullanıcı Adı,** **Şifre**ve Şifre Bilgilerini **Onayla'** yı girin.
   - **Gelen bağlantı noktası kuralları** > **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen bağlantı noktası kuralları** > **Gelen bağlantı noktalarını seçin**: **SSH'yi seçin (22)**
   - **Ağ** sekmesini seçin veya **Sonraki: Diskler,** sonra **Sonraki: Ağ**.

3. **Ağ** sekmesinde aşağıdakilerin seçildiğinden emin olun:
   - **Sanal ağ**: **myVnet**
   - **Alt ağ**: **mySubnet**
   - **Genel IP** > Seçin **yeni oluştur.**  Ortak **IP adresi oluşturma** penceresinde, **Ad** alanına **myPublicIPVM** yazın ve **SKU**için **Standart'ı** seçin.  **Tamam**'a tıklayın.
   - **NIC ağ güvenlik grubu**: **Temel'i**seçin.
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen bağlantı noktalarını seçin**: **SSH'yi** onaylayın seçilir.

4. **Yönetim** sekmesinde, **İzleme**altında, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın.

5. **İncele ve oluştur**’u seçin. 

6. Ayarları gözden geçirin ve **Oluştur'a**tıklayın.

## <a name="create-the-nat-gateway"></a>NAT ağ geçidioluşturma

Bir veya daha fazla genel IP adresi kaynağı, genel IP önekleri veya her ikisini de kullanabilirsiniz. Genel IP kaynağı, genel IP öneki ve NAT ağ geçidi kaynağı ekleriz.

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini nasıl oluşturabileceğiniz ve yapılandırabileceğiniz ayrıntılı olarak açıklanmaktadır:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel BIR IP havuzu ve genel IP öneki.
  - Boşta kalma süresini varsayılan 4 dakikadan 10 dakikaya değiştirin.

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

1. **Networking** > Portalın sol üst tarafında, bir kaynak > Oluşturma**Ortak IP adresi** **oluştur'u**veya Market aramasında Ortak **IP adresini** arayın'ı seçin.

2. **Ortak IP adresi oluştur'da**bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | IP Sürümü | **IPv4** seçin.
    | SKU | **Standart'ı**seçin.
    | Adı | **MyPublicIP**girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **MyResourceGroupNAT'ı**seçin. |
    | Konum | **Doğu ABD 2**’yi seçin.|

3. Varsayılanların geri kalanını bırakın ve **Oluştur'u**seçin.

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

1. Portalın sol üst tarafında, **bir kaynak** > **Networking** > Oluştur**Genel IP öneki**ni seçin veya Market aramasında **Genel IP önekini** arayın. 

2. **Ortak IP öneki**oluştur'da, **Temeller** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupNAT'ı** seçin>
   - **Örnek ayrıntıları** > **Adı**: **myPublicIPprefix**yazın.
   - **Örnek ayrıntıları** > **Bölge**: Doğu **ABD 2'yi**seçin.
   - **Örnek ayrıntıları** > **Önek boyutu**: Seç **/31 (2 adres)**

3. Geri sini varsayılanları bırakın ve **Gözden Geçir + oluştur'u**seçin.

4. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.
   

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

1. Portalın sol üst tarafında, Bir kaynak > **Ağı** > **NAT ağ geçidi** **oluştur'u**veya Market aramasında **NAT ağ geçidini** arayın'ı seçin.

2. **Ağ adresi çevirisi (NAT) ağ geçidi oluştur'da,** Temel **Ler** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupNAT'ı**seçin.
   - **Örnek ayrıntıları** > **NAT ağ geçidi adı**: **myNATgateway**yazın.
   - **Örnek ayrıntıları** > **Bölge**: Doğu **ABD 2'yi**seçin.
   - **Örnek ayrıntıları** > **Boşta zaman ası (dakika)**: Tip **10**.
   - Genel **IP** sekmesini seçin veya **Sonraki: Genel IP'yi**seçin.

3. Genel **IP** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Genel IP adresleri**: **myPublicIP'i**seçin.
   - **Genel IP Önekleri**: **MyPublicIPprefix'i**seçin.
   - Alt **net** sekmesini seçin veya **Sonraki: Subnet'i**seçin.

4. Alt **net** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Sanal Ağ**: **myResourceGroupNAT** > **myVnet'i**seçin.
   - **Subnet adı**: **mySubnet'in**yanındaki kutuyu seçin.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

## <a name="discover-the-ip-address-of-the-vm"></a>VM'nin IP adresini keşfedin

1. Portalın sol tarafında Kaynak **grupları**seçin.
2. **MyResourceGroupNAT'ı**seçin.
3. **myVM'i**seçin.
4. **Genel**Bakış'ta, **Genel IP adresi** değerini kopyalayın ve VM'ye erişmek için kullanabilmeniz için not defterine yapıştırın.

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve VM'ye erişmek için kullanabilmeniz için not defterine yapıştırın.

## <a name="sign-in-to-vm"></a>VM'de oturum açın

Tarayıcınızda bir [Azure Bulut Kabuğu](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

```azurecli-interactive
ssh <username>@<ip-address-destination>
```

Artık NAT hizmetini kullanmaya hazırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, NAT ağ geçidini ve ilgili tüm kaynakları silin. NAT ağ geçidini içeren **myResourceGroupNAT** kaynak grubunu seçin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, kullanmak için bir NAT ağ geçidi ve bir VM oluşturdunuz. 

NAT hizmetinizin çalıştığını görmek için Azure Monitor'da ölçümleri inceleyin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanı.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi de eklenerek giderilir.


- Azure [Sanal Ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI'yi kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmak için hızlı başlatın.
- [Azure PowerShell'i kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmak için hızlı başlatın.
- [Azure portalını kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmak için hızlı başlatın.
> [!div class="nextstepaction"]

