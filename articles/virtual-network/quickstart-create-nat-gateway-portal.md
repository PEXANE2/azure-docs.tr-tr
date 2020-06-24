---
title: 'Öğretici: NAT Gateway oluşturma-Azure portal'
titlesuffix: Azure Virtual Network NAT
description: Bu hızlı başlangıçta Azure portal kullanarak NAT ağ geçidi oluşturma gösterilmektedir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a NAT gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.subservice: nat
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: 5010dc08b695a0376ace5dde935c63caf0c39633
ms.sourcegitcommit: 537c539344ee44b07862f317d453267f2b7b2ca6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/11/2020
ms.locfileid: "84711417"
---
# <a name="tutorial-create-a-nat-gateway-using-the-azure-portal"></a>Öğretici: Azure portal kullanarak NAT ağ geçidi oluşturma

Bu öğreticide, Azure sanal ağ NAT hizmetini nasıl kullanacağınız gösterilmektedir. Azure 'da bir sanal makine için giden bağlantı sağlamak üzere bir NAT ağ geçidi oluşturacaksınız. 

Tercih ederseniz, [Azure CLI](quickstart-create-nat-gateway-cli.md)'yi kullanarak bu adımları uygulayabilirsiniz, [Azure PowerShell](quickstart-create-nat-gateway-powershell.md)veya Portal yerine bir [ARM şablonu](quickstart-create-nat-gateway-powershell.md) dağıtabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bir VM 'yi dağıtmadan ve NAT ağ geçidinizi kullanabilmeniz için, kaynak grubunu ve sanal ağı oluşturmanız gerekir.

Bu bölümde, adımlarda aşağıdaki parametreleri aşağıdaki bilgilerle değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupNAT |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Doğu ABD 2      |
| **\<IPv4-address-space>**   | 192.168.0.0 \ 16          |
| **\<subnet-name>**          | mySubnet        |
| **\<subnet-address-range>** | 192.168.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-a-vm-to-use-the-nat-gateway"></a>NAT ağ geçidini kullanmak için bir VM oluşturma

Artık NAT hizmetini kullanmak için bir VM oluşturacağız. Bu VM 'nin VM 'ye erişiminizi sağlamak için örnek düzeyi genel IP olarak kullanılacak genel bir IP 'si vardır. NAT hizmeti akış yönü farkındadır ve alt ağınızdaki varsayılan Internet hedefini değiştirecek. VM 'nin genel IP adresi giden bağlantılar için kullanılmayacak.

1. Portalın sol üst tarafında, **kaynak oluştur**  >  **işlem**  >  **Ubuntu Server 18,04 LTS**' yi seçin veya Market aramasında **Ubuntu Server 18,04 LTS** ' yi arayın.

2. **Sanal makine oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik**  >  **Kaynak grubu**: **Myresourcegroupnat**' ı seçin.
   - **Örnek ayrıntıları**  >  **Sanal makine adı**: **myvm**yazın.
   - **Örnek ayrıntıları**  >  **Bölge** > **Doğu ABD 2**seçin.
   - **Yönetici hesabı**  >  **Kimlik doğrulama türü**: **parola**seçin.
   - **Yönetici hesabı** > **Kullanıcı adı**, **parola**ve **parola onaylama** bilgilerini girin.
   - **Gelen bağlantı noktası kuralları**  >  **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver**öğesini seçin.
   - **Gelen bağlantı noktası kuralları**  >  **Gelen bağlantı noktalarını seçin**: **SSH seçin (22)**
   - **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin.

3. **Ağ** sekmesinde aşağıdakilerin seçili olduğundan emin olun:
   - **Sanal ağ**: **myvnet**
   - **Alt ağ**: **mysubnet**
   - **Ortak ıp** > **Yeni oluştur**' u seçin.  **Genel IP adresi oluştur** penceresinde **ad** alanına **Mypublicipvm** yazın ve **SKU**için **Standart** ' ı seçin.  **Tamam**'a tıklayın.
   - **NIC ağ güvenlik grubu**: **temel**öğesini seçin.
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver**öğesini seçin.
   - **Gelen bağlantı noktalarını seçin**: **SSH** 'yi Onayla seçili.

4. **Yönetim** sekmesinde, **izleme**' nin altında, **önyükleme tanılamayı** **kapalı**olarak ayarlayın.

5. **İncele ve oluştur**’u seçin. 

6. Ayarları gözden geçirin ve **Oluştur**' a tıklayın.

## <a name="create-the-nat-gateway"></a>NAT ağ geçidini oluşturma

Bir veya daha fazla genel IP adresi kaynağı, genel IP ön ekleri veya her ikisini birden kullanabilirsiniz. Genel IP kaynağı, genel IP öneki ve NAT ağ geçidi kaynağı ekleyeceğiz.

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini oluşturma ve yapılandırma işlemlerinin ayrıntıları verilmiştir:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel IP havuzu ve genel IP öneki.
  - Boşta kalma zaman aşımını varsayılan olarak 4 dakika ile 10 dakika arasında değiştirin.

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

1. Portalın sol üst tarafında, **kaynak**  >  **ağı**  >  **genel IP adresi**oluştur ' u seçin veya Market aramasında **genel IP adresini** arayın.

2. **Genel IP adresi oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | IP sürümü | **IPv4** seçin.
    | SKU | **Standart**' ı seçin.
    | Name | **Mypublicıp**girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Myresourcegroupnat**öğesini seçin. |
    | Konum | **Doğu ABD 2**’yi seçin.|

3. Varsayılan değerleri bırakın ve **Oluştur**' u seçin.

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

1. Portalın sol üst kısmında **kaynak**  >  **ağı**  >  **genel IP öneki**oluştur ' u seçin veya Market aramasında **genel IP öneki** ' ni arayın. 

2. **Genel IP öneki oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik**  >  **Kaynak grubu**: **Myresourcegroupnat** seçin>
   - **Örnek ayrıntıları**  >  **Ad**: **Mypublicipprefix**yazın.
   - **Örnek ayrıntıları**  >  **Bölge**: **Doğu ABD 2**seçin.
   - **Örnek ayrıntıları**  >  **Ön ek boyutu**: Select **/31 (2 adres)**

3. Rest varsayılanlarını bırakın ve **gözden geçir + oluştur**' u seçin.

4. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.
   

### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

1. Portalın sol üst kısmında, **kaynak**  >  **ağı**  >  **NAT ağ geçidi**oluştur ' u seçin veya Market aramasında **NAT ağ geçidini** arayın.

2. **Ağ adresi çevirisi (NAT) ağ geçidi oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik**  >  **Kaynak grubu**: **Myresourcegroupnat**' ı seçin.
   - **Örnek ayrıntıları**  >  **NAT ağ geçidi adı**: **Mynatgateway**yazın.
   - **Örnek ayrıntıları**  >  **Bölge**: **Doğu ABD 2**seçin.
   - **Örnek ayrıntıları**  >  **Boşta kalma zaman aşımı (dakika)**: tür **10**.
   - **Genel IP** sekmesini seçin veya ileri ' yi seçin **: genel IP**.

3. **Genel IP** sekmesinde, aşağıdaki değerleri yazın veya seçin:
   - **Genel IP adresleri**: **Mypublicıp**öğesini seçin.
   - **Genel IP önekleri**: **Mypublicipprefix**' i seçin.
   - **Alt ağ** sekmesini seçin veya **Ileri: alt ağ**' ı seçin.

4. **Alt ağ** sekmesinde, aşağıdaki değerleri yazın veya seçin:
   - **Sanal ağ**: **myresourcegroupnat**  >  **myvnet**öğesini seçin.
   - **Alt ağ adı**: **mysubnet öğesinin**yanındaki kutuyu seçin.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

## <a name="discover-the-ip-address-of-the-vm"></a>VM 'nin IP adresini bulma

1. Portalın sol tarafında **kaynak grupları**' nı seçin.
2. **Myresourcegroupnat**öğesini seçin.
3. **Myvm**' yi seçin.
4. **Genel**olarak, **genel IP adresi** değerini kopyalayın ve sanal makineye erişmek için kullanabilmek üzere not defteri 'ne yapıştırın.

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve ardından sanal makineye erişmek için kullanabilmek üzere bir not defteri 'ne yapıştırın.

## <a name="sign-in-to-vm"></a>VM 'de oturum açma

Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

```azurecli-interactive
ssh <username>@<ip-address-destination>
```

Artık NAT hizmetini kullanmaya hazırsınız.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, NAT ağ geçidini ve tüm ilgili kaynakları silin. NAT ağ geçidini içeren **Myresourcegroupnat** kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir NAT ağ geçidi ve onu kullanmak için bir VM oluşturdunuz. 

NAT hizmeti 'nin çalışıyor olduğunu görmek için Azure Izleyici 'de ölçümleri gözden geçirin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanılayın.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi eklenerek adreslenir.


- [Azure sanal ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI kullanarak NAT Gateway kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure PowerShell kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure Portal kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmaya yönelik hızlı başlangıç.
> [!div class="nextstepaction"]

