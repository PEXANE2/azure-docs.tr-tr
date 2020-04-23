---
title: 'Öğretici: NAT Ağ Geçidi oluşturma ve test edin - Azure portalı'
titlesuffix: Azure Virtual Network NAT
description: Bu öğretici, Azure portalını kullanarak bir NAT Ağ Geçidi oluşturmanın ve NAT hizmetini nasıl test edilebildiğini gösterir
services: virtual-network
documentationcenter: na
author: asudbring
manager: KumundD
Customer intent: I want to test a NAT Gateway for outbound connectivity for my virtual network.
ms.service: virtual-network
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/24/2020
ms.author: allensu
ms.openlocfilehash: ceadbb4297ad0c5ce28470dd75b3f3496c9c5152
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82084752"
---
# <a name="tutorial-create-a-nat-gateway-using-the-azure-portal-and-test-the-nat-service"></a>Öğretici: Azure portalını kullanarak bir NAT Ağ Geçidi oluşturun ve NAT hizmetini test edin

Bu eğitimde, Azure'daki sanal makineler için giden bağlantı sağlamak için bir NAT ağ geçidi oluşturursunuz. NAT ağ geçidini sınamak için bir kaynak ve hedef sanal makine dağıtabilirsiniz. Kaynaktan hedef sanal makineye genel bir IP adresine giden bağlantılar yaparak NAT ağ geçidini sınayacaksınız.  Bu öğretici, yalnızca basitlik için aynı kaynak grubunda ki iki farklı sanal ağda kaynak ve hedef dağıtır.

İsterseniz, bu adımları portal yerine [Azure CLI](tutorial-create-validate-nat-gateway-cli.md) veya [Azure PowerShell'i](tutorial-create-validate-nat-gateway-powershell.md) kullanarak yapabilirsiniz.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="prepare-the-source-for-outbound-traffic"></a>Kaynağı giden trafik için hazırlayın

Tam bir test ortamının yapılandırması ve sonraki adımlarda testlerin kendisinin yürütülmesi nde size rehberlik edeceğiz. Daha sonraki adımlarda oluşturduğumuz NAT ağ geçidi kaynağını kullanacak kaynakla başlayacağız.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bir VM dağıtmadan ve NAT ağ ağınızı kullanmadan önce kaynak grubu ve sanal ağ oluşturmamız gerekir.

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroupNAT |
| **\<sanal ağ adı>** | myVNetsource          |
| **\<bölge adı>**          | Doğu ABD 2      |
| **\<IPv4 adres-boşluk>**   | 192.168.0.0/16          |
| **\<subnet-name>**          | mySubnetsource        |
| **\<alt net-adres aralığı>** | 192.168.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-source-virtual-machine"></a>Kaynak sanal makine oluşturma

Şimdi NAT hizmetini kullanmak için bir VM oluşturacağız. Bu VM, VM'ye erişmenizi sağlamak için örnek düzeyinde public IP olarak kullanabileceği ortak bir IP'ye sahiptir. NAT hizmeti akış yönü farkındadır ve alt anızdaki varsayılan Internet hedefinin yerini alır. VM'nin genel IP adresi giden bağlantılar için kullanılmaz.

NAT ağ geçidini test etmek için, bu VM'ye dışarıdan erişmek için örnek düzeyinde bir Genel IP olarak ortak bir IP adresi kaynağı atayacağız. Bu adres yalnızca test için erişmek için kullanılır.  NAT hizmetinin diğer giden seçeneklerden nasıl öncelikli olduğunu göstereceğiz.

Ayrıca, bu VM'yi genel bir IP olmadan oluşturabilir ve bir egzersiz olarak genel IP'si olmadan atlama kutusu olarak kullanmak üzere başka bir VM oluşturabilirsiniz.

1. Portalın sol üst tarafında, **bir kaynak** > **Oluştur Compute** > **Ubuntu Server 18.04 LTS'yi**seçin veya Market aramasında **Ubuntu Server 18.04 LTS'yi** arayın.

2. **Sanal makine Oluştur'da** **Temeller** sekmesine aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupNAT'ı**seçin.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: **myVMsource**girin.
   - **Örnek Ayrıntılar** > **Bölge** > Doğu **ABD 2**seçin.
   - **Yönetici hesabı** > **Kimlik Doğrulaması girin**: **Parola'yı**seçin.
   - **Yönetici hesabı** > **Kullanıcı Adı,** **Şifre**ve Şifre Bilgilerini **Onayla'** yı girin.
   - **Gelen bağlantı noktası kuralları** > **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen bağlantı noktası kuralları** > **Gelen bağlantı noktalarını seçin**: **SSH'yi seçin (22)**
   - **Ağ** sekmesini seçin veya **Sonraki: Diskler,** sonra **Sonraki: Ağ**.

3. **Ağ** sekmesinde aşağıdakilerin seçildiğinden emin olun:
   - **Sanal ağ**: **myVnetsource**
   - **Alt ağ**: **mySubnetsource**
   - **Genel IP** > Seçin **yeni oluştur.**  Ortak **IP adresi oluşturma** penceresinde, **Ad** alanına **myPublicIPsourceVM** girin. **SKU**için **Standart'ı** seçin. Geri kalanını varsayılanlarda bırakın ve **Tamam'ı**tıklatın.
   - **NIC ağ güvenlik grubu**: **Temel'i**seçin.
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen bağlantı noktalarını seçin**: **SSH'yi** onaylayın seçilir.

4. **Yönetim** sekmesinde, **İzleme**altında, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve **Oluştur'a**tıklayın.

## <a name="create-the-nat-gateway"></a>NAT Ağ Geçidi oluşturma

Bir veya daha fazla genel IP adresi kaynağı, genel IP önekleri veya her ikisini de NAT ağ geçidiyle kullanabilirsiniz. Genel IP kaynağı, genel IP öneki ve NAT ağ geçidi kaynağı ekleriz.

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
    | Adı | **MyPublicIPsource**girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **MyResourceGroupNAT'ı**seçin. |
    | Konum | **Doğu ABD 2**’yi seçin.|

3. Varsayılanların geri kalanını bırakın ve **Oluştur'u**seçin.

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

1. Portalın sol üst tarafında, **bir kaynak** > **Networking** > Oluştur**Genel IP öneki**ni seçin veya Market aramasında **Genel IP önekini** arayın.

2. **Ortak IP öneki oluştur'da** **Temeller** sekmesinde aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupNAT'ı** seçin>
   - **Örnek ayrıntıları** > **Adı**: **myPublicIPprefixsource**girin .
   - **Örnek ayrıntıları** > **Bölge**: Doğu **ABD 2'yi**seçin.
   - **Örnek ayrıntıları** > **Önek boyutu**: Seç **/31 (2 adres)**

3. Geri sini varsayılanları bırakın ve **Gözden Geçir + oluştur'u**seçin.

4. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.


### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

1. Portalın sol üst tarafında, Bir kaynak > **Ağı** > **NAT ağ geçidi** **oluştur'u**veya Market aramasında **NAT ağ geçidini** arayın'ı seçin.

2. **Ağ adresi çevirisi oluştur (NAT) ağ geçidinde**Temel **Ler** sekmesine aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupNAT'ı**seçin.
   - **Örnek ayrıntıları** > **NAT ağ geçidi adı**: **myNATgateway**girin.
   - **Örnek ayrıntıları** > **Bölge**: Doğu **ABD 2'yi**seçin.
   - **Örnek ayrıntıları** > **Boşta zaman ası (dakika)**: **10**girin .
   - Genel **IP** sekmesini seçin veya **Sonraki: Genel IP'yi**seçin.

3. Genel **IP** sekmesine aşağıdaki değerleri girin veya seçin:
   - **Genel IP adresleri**: **myPublicIPsource'u**seçin.
   - **Genel IP Önekleri**: **MyPublicIPprefixsource'u**seçin.
   - Alt **net** sekmesini seçin veya **Sonraki: Subnet'i**seçin.

4. Alt **net** sekmesine aşağıdaki değerleri girin veya seçin:
   - **Sanal Ağ**: **myResourceGroupNAT** > **myVnetsource'u**seçin.
   - **Subnet adı**: **mySubnetsource**yanındaki kutuyu seçin.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

İnternet hedeflerine giden tüm giden trafik artık NAT hizmetini kullanıyor.  Bir UDR yapılandırmak için gerekli değildir.


## <a name="prepare-destination-for-outbound-traffic"></a>Giden trafik için hedef hazırlama

Şimdi, NAT hizmeti tarafından çevrilen giden trafik için bir hedef oluşturarak bunu test edebilirsiniz.


## <a name="virtual-network-and-parameters-for-destination"></a>Hedef için sanal ağ ve parametreler

Hedef için bir VM dağıtmadan önce, hedef sanal makinenin bulunduğu bir sanal ağ oluşturmamız gerekir. Hedef bitiş noktasını ortaya çıkarmak için bazı küçük değişikliklerle kaynak VM ile aynı adımlar aşağıda verilmiştir.

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroupNAT |
| **\<sanal ağ adı>** | myVNetdestination          |
| **\<bölge adı>**          | Doğu ABD 2      |
| **\<IPv4 adres-boşluk>**   | 192.168.0.0/16          |
| **\<subnet-name>**          | mySubnetdestination        |
| **\<alt net-adres aralığı>** | 192.168.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-destination-virtual-machine"></a>Hedef sanal makine oluşturma

1. Portalın sol üst tarafında, **bir kaynak** > **Oluştur Compute** > **Ubuntu Server 18.04 LTS'yi**seçin veya Market aramasında **Ubuntu Server 18.04 LTS'yi** arayın.

2. **Sanal makine Oluştur'da** **Temeller** sekmesine aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupNAT'ı**seçin.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: **myVMdestination**girin .
   - **Örnek Ayrıntılar** > **Bölge** > Doğu **ABD 2**seçin.
   - **Yönetici hesabı** > **Kimlik Doğrulaması girin**: **Parola'yı**seçin.
   - **Yönetici hesabı** > **Kullanıcı Adı,** **Şifre**ve Şifre Bilgilerini **Onayla'** yı girin.
   - **Gelen bağlantı noktası kuralları** > **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen bağlantı noktası kuralları** > **Gelen bağlantı noktalarını seçin**: **SSH (22)** ve **HTTP (80)** seçeneğini seçin.
   - **Ağ** sekmesini seçin veya **Sonraki: Diskler,** sonra **Sonraki: Ağ**.

3. **Ağ** sekmesinde aşağıdakilerin seçildiğinden emin olun:
   - **Sanal ağ**: **myVnetdestination**
   - **Alt ağ**: **mySubnetdestination**
   - **Genel IP** > Seçin **yeni oluştur.**  Ortak **IP adresi oluşturma** penceresinde, **Ad** alanına **myPublicIPdestinationVM** girin. **SKU**için **Standart'ı** seçin. Geri kalanını varsayılanlarda bırakın ve **Tamam'ı**tıklatın.
   - **NIC ağ güvenlik grubu**: **Temel'i**seçin.
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen bağlantı noktalarını seçin**: **SSH** ve **HTTP'yi** onaylayın seçilir.

4. **Yönetim** sekmesinde, **İzleme**altında, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Hedef VM'de bir web sunucusu hazırlayın ve yükü test edin

Öncelikle hedef VM IP adresini keşfetmek gerekir. 

1. Portalın sol tarafında Kaynak **grupları**seçin.
2. **MyResourceGroupNAT'ı**seçin.
3. **myVMdestination'ı**seçin.
4. **Genel**Bakış'ta, **Genel IP adresi** değerini kopyalayın ve VM'ye erişmek için kullanabilmeniz için not defterine yapıştırın.

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve sonraki adımlarda kullanabilmeniz için not defterine yapıştırın. Bunun hedef sanal makine olduğunu belirtin.

### <a name="sign-in-to-destination-vm"></a>Hedef VM'de oturum açın

Tarayıcınızda bir [Azure Bulut Kabuğu](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

```azurecli-interactive
ssh <username>@<ip-address-destination>
```

Oturum açtıktan sonra aşağıdaki komutları kopyalayıp yapıştırın.  

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get -y install nginx && \
sudo ln -sf /dev/null /var/log/nginx/access.log && \
sudo touch /var/www/html/index.html && \
sudo rm /var/www/html/index.nginx-debian.html && \
sudo dd if=/dev/zero of=/var/www/html/100k bs=1024 count=100
```

Bu komutlar sanal makinenizi günceller, nginx yükler ve 100 KBytes dosyası oluşturur. Bu dosya NAT hizmeti kullanılarak kaynak VM'den alınacaktır.

Hedef VM ile SSH oturumunu kapatın.

## <a name="prepare-test-on-source-vm"></a>Kaynak VM üzerinde test hazırlama

Öncelikle kaynak VM IP adresini keşfetmek gerekir.

1. Portalın sol tarafında Kaynak **grupları**seçin.
2. **MyResourceGroupNAT'ı**seçin.
3. **myVMsource'u**seçin.
4. **Genel**Bakış'ta, **Genel IP adresi** değerini kopyalayın ve VM'ye erişmek için kullanabilmeniz için not defterine yapıştırın.

>[!IMPORTANT]
>Ortak IP adresini kopyalayın ve sonraki adımlarda kullanabilmeniz için not defterine yapıştırın. Bunun kaynak sanal makine olduğunu belirtin.

### <a name="log-into-source-vm"></a>Kaynak VM'de oturum aç

Tarayıcınızda Azure [Bulut Su şutu](https://shell.azure.com) için yeni bir sekme açın.  Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```azurecli-interactive
ssh <username>@<ip-address-source>
```

NAT hizmetini sınamaya hazırlanmak için aşağıdaki komutları kopyalayın ve yapıştırın.

```bash
sudo apt-get -y update && \
sudo apt-get -y upgrade && \
sudo apt-get -y dist-upgrade && \
sudo apt-get -y autoremove && \
sudo apt-get -y autoclean && \
sudo apt-get install -y nload golang && \
echo 'export GOPATH=${HOME}/go' >> .bashrc && \
echo 'export PATH=${PATH}:${GOPATH}/bin' >> .bashrc && \
. ~/.bashrc &&
go get -u github.com/rakyll/hey

```

Bu komut sanal makinenizi günceller, git'i yükler, GitHub'dan [yüklenir](https://github.com/rakyll/hey) ve kabuk ortamınızı günceller.

Artık NAT hizmetini test etmeye hazırsınız.

## <a name="validate-nat-service"></a>NAT hizmetini doğrulama

Kaynak VM'de oturum açtığınızda, hedef IP adresine istek oluşturmak için **curl** ve **hey'i** kullanabilirsiniz.

100-KBytes dosyasını almak için kıvırma kullanın.  Aşağıdaki örnekte ** \<ip adresi hedef>** daha önce kopyalamış olduğunuz hedef IP adresiyle değiştirin.  **--çıkış** parametresi, alınan dosyanın atılacağını gösterir.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

Ayrıca **hey**kullanarak istekleri bir dizi oluşturabilirsiniz. Yine, ** \<ip adresi hedef>** daha önce kopyalamış olduğunuz hedef IP adresiyle değiştirin.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Bu komut, TCP bağlantısını yeniden kullanmadan 30 saniyelik bir zaman aşımıyla 10 aynı anda 100 istek oluşturur.  Her istek 100 Kbyte alır.  Çalışma sonunda, **hey** NAT hizmeti ne kadar iyi yaptığı hakkında bazı istatistikleri rapor edecektir.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, NAT ağ geçidini ve ilgili tüm kaynakları silin. NAT ağ geçidini içeren **myResourceGroupNAT** kaynak grubunu seçin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir NAT ağ geçidi oluşturdunuz, bir kaynak ve hedef VM oluşturdunuz ve sonra NAT ağ geçidini test ettiniz.

NAT hizmetinizin çalıştığını görmek için Azure Monitor'da ölçümleri inceleyin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanı.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi de eklenerek kolayca giderilir.

- Sanal [Ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI'yi kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmak için hızlı başlatın.
- [Azure PowerShell'i kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmak için hızlı başlatın.
- [Azure portalını kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmak için hızlı başlatın.

> [!div class="nextstepaction"]

