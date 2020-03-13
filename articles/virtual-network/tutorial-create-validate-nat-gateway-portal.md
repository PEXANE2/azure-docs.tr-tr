---
title: 'Öğretici: NAT Gateway oluşturma ve test etme-Azure portal'
titlesuffix: Azure Virtual Network NAT
description: Bu öğreticide, Azure portal kullanarak NAT ağ geçidinin oluşturulması ve NAT hizmetinin test olması gösterilmektedir
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
ms.openlocfilehash: 3468dd72ff47c895ee4df6ed4308aabab92c33f1
ms.sourcegitcommit: 7b25c9981b52c385af77feb022825c1be6ff55bf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79216941"
---
# <a name="tutorial-create-a-nat-gateway-using-the-azure-portal-and-test-the-nat-service"></a>Öğretici: Azure portal kullanarak NAT ağ geçidi oluşturma ve NAT hizmetini test etme

Bu öğreticide, Azure 'da sanal makineler için giden bağlantı sağlamak üzere bir NAT ağ geçidi oluşturacaksınız. NAT ağ geçidini test etmek için bir kaynak ve hedef sanal makine dağıtırsınız. Kaynaktan hedef sanal makineye bir genel IP adresine giden bağlantı yaparak NAT ağ geçidini test edersiniz.  Bu öğretici, kaynak ve hedefi yalnızca kolaylık sağlaması için aynı kaynak grubundaki iki farklı sanal ağda dağıtır.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[Azure Portal](https://portal.azure.com) oturum açın.

## <a name="prepare-the-source-for-outbound-traffic"></a>Kaynağı giden trafik için hazırlama

Bir tam test ortamının yapılandırması ve bir sonraki adımlarda testlerin yürütülmesi konusunda size kılavuzluk edeceğiz. Sonraki adımlarda oluşturduğumuz NAT ağ geçidi kaynağını kullanacak kaynak ile başlayacağız.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bir VM 'yi dağıtmadan ve NAT ağ geçidinizi kullanabilmeniz için, kaynak grubunu ve sanal ağı oluşturmanız gerekir.

Bu bölümde, adımlarda aşağıdaki parametreleri aşağıdaki bilgilerle değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak grubu-adı >**  | myResourceGroupNAT |
| **\<sanal ağ-adı >** | myVNetsource          |
| **\<bölge adı >**          | Doğu ABD 2      |
| **\<IPv4-adres-alanı >**   | 192.168.0.0 \ 16          |
| **\<alt ağ-adı >**          | mySubnetsource        |
| **\<alt ağ-adres aralığı >** | 192.168.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-source-virtual-machine"></a>Kaynak sanal makine oluştur

Artık NAT hizmetini kullanmak için bir VM oluşturacağız. Bu VM 'nin VM 'ye erişiminizi sağlamak için örnek düzeyi genel IP olarak kullanılacak genel bir IP 'si vardır. NAT hizmeti akış yönü farkındadır ve alt ağınızdaki varsayılan Internet hedefini değiştirecek. VM 'nin genel IP adresi giden bağlantılar için kullanılmayacak.

NAT ağ geçidini test etmek için, bu sanal makineye dışarıdan erişmek üzere örnek düzeyi genel IP olarak bir genel IP adresi kaynağı atayacağız. Bu adres yalnızca test için erişim için kullanılır.  NAT hizmetinin diğer giden seçeneklere göre nasıl öncelikli olduğunu göstereceğiz.

Ayrıca, bu VM 'yi ortak IP olmadan oluşturabilir ve bir alıştırma olarak genel IP 'si olmadan bir sıçrama kutusu olarak kullanmak için başka bir VM oluşturabilirsiniz.

1. Portalın sol üst **kısmında > ** **kaynak oluştur** ' u > **Ubuntu Server 18,04 LTS**' i seçin veya market aramasında **Ubuntu Server 18,04 LTS** ' yi arayın.

2. **Sanal makine oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **kaynak grubu**: **myresourcegroupnat**öğesini seçin.
   - **Örnek ayrıntıları** > **sanal makine adı**: **myvmsource**girin.
   - **Örnek ayrıntıları** **bölge** > > **Doğu ABD 2**seçin.
   - **Yönetici hesabı** > **kimlik doğrulaması girin**: **parola**seçin.
   - **Yönetici hesabı** > **Kullanıcı adı**, **parola**ve **parola onaylama** bilgilerini girin.
   - **Gelen bağlantı noktası kuralları** > **ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver**öğesini seçin
   - **Gelen bağlantı noktası kuralları** > **gelen bağlantı noktalarını seçin**: **SSH seçin (22)**
   - **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin.

3. **Ağ** sekmesinde aşağıdakilerin seçili olduğundan emin olun:
   - **Sanal ağ**: **myvnetsource**
   - **Alt ağ**: **mysubnetsource**
   - **Ortak ıp** > **Yeni oluştur**' u seçin.  **Genel IP adresi oluştur** penceresinde **ad** alanına **Mypublicipsourcevm** girin. **SKU**için **Standart** ' ı seçin. Rest 'i varsayılan konumda bırakın ve **Tamam 'a**tıklayın.
   - **NIC ağ güvenlik grubu**: **temel**öğesini seçin.
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver**öğesini seçin.
   - **Gelen bağlantı noktalarını seçin**: **SSH** 'yi Onayla seçili.

4. **Yönetim** sekmesinde, **izleme**' nin altında, **önyükleme tanılamayı** **kapalı**olarak ayarlayın.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve **Oluştur**' a tıklayın.

## <a name="create-the-nat-gateway"></a>NAT ağ geçidini oluşturma

NAT ağ geçidiyle bir veya daha fazla genel IP adresi kaynağı, genel IP ön ekleri veya her ikisini birden kullanabilirsiniz. Genel IP kaynağı, genel IP öneki ve NAT ağ geçidi kaynağı ekleyeceğiz.

Bu bölümde, NAT ağ geçidi kaynağını kullanarak NAT hizmetinin aşağıdaki bileşenlerini oluşturma ve yapılandırma işlemlerinin ayrıntıları verilmiştir:
  - NAT ağ geçidi kaynağı tarafından çevrilen giden akışlar için kullanılacak genel IP havuzu ve genel IP öneki.
  - Boşta kalma zaman aşımını varsayılan olarak 4 dakika ile 10 dakika arasında değiştirin.

### <a name="create-a-public-ip-address"></a>Genel IP adresi oluşturma

1. Portalın sol üst kısmında, **kaynak oluştur** > **ağ** > **genel IP adresi**' nı seçin veya Market aramasında **genel IP adresini** arayın. 

2. **Genel IP adresi oluştur**' da bu bilgileri girin veya seçin:

    | Ayar | Değer |
    | ------- | ----- |
    | IP sürümü | **IPv4**' ü seçin.
    | SKU | **Standart**' ı seçin.
    | Adı | **Mypublicipsource**girin. |
    | Abonelik | Aboneliğinizi seçin.|
    | Kaynak grubu | **Myresourcegroupnat**öğesini seçin. |
    | Konum | **Doğu ABD 2**’yi seçin.|

3. Varsayılan değerleri bırakın ve **Oluştur**' u seçin.

### <a name="create-a-public-ip-prefix"></a>Genel IP öneki oluşturma

1. Portalın sol üst kısmında, **kaynak oluştur** > **ağ** > **genel IP öneki**' nı seçin veya Market aramasında **genel IP öneki** ' ni arayın.

2. **Genel IP öneki oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **kaynak grubu**: **myresourcegroupnat** seçeneğini belirleyin>
   - **Örnek ayrıntıları** > **adı**: **myPublicIPprefixsource**girin.
   - **Örnek ayrıntıları** > **bölgesi**: **Doğu ABD 2**seçin.
   - **Örnek ayrıntıları** > **ön ek boyutu**: Select **/31 (2 adres)**

3. Rest varsayılanlarını bırakın ve **gözden geçir + oluştur**' u seçin.

4. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.


### <a name="create-a-nat-gateway-resource"></a>NAT ağ geçidi kaynağı oluşturma

1. Portalın sol üst kısmında, **kaynak oluştur** > **ağ** > **NAT ağ geçidi**' ni seçin veya Market aramasında **NAT ağ geçidini** arayın.

2. **Ağ adresi çevirisi (NAT) ağ geçidi oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **kaynak grubu**: **myresourcegroupnat**öğesini seçin.
   - **Örnek ayrıntıları** > **NAT ağ geçidi adı**: **mynatgateway**girin.
   - **Örnek ayrıntıları** > **bölgesi**: **Doğu ABD 2**seçin.
   - **Örnek ayrıntıları** > **boşta zaman aşımı (dakika)** : **10**girin.
   - **Genel IP** sekmesini seçin veya ileri ' yi seçin **: genel IP**.

3. **Genel IP** sekmesinde, aşağıdaki değerleri girin veya seçin:
   - **Genel IP adresleri**: **Mypublicipsource**' u seçin.
   - **Genel IP ön ekleri**: **myPublicIPprefixsource**seçin.
   - **Alt ağ** sekmesini seçin veya **Ileri: alt ağ**' ı seçin.

4. **Alt ağ** sekmesinde, aşağıdaki değerleri girin veya seçin:
   - **Sanal ağ**: **Myresourcegroupnat** > **myvnetsource**' u seçin.
   - **Alt ağ adı**: **mysubnetsource**seçeneğinin yanındaki kutuyu seçin.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

Internet hedeflerine giden tüm trafik artık NAT hizmetini kullanıyor.  UDR 'yi yapılandırmak gerekli değildir.


## <a name="prepare-destination-for-outbound-traffic"></a>Giden trafik için hedefi hazırlama

Artık, NAT hizmeti tarafından, test yapmanıza olanak tanımak için çevrilmiş giden trafik için bir hedef oluşturacağız.


## <a name="virtual-network-and-parameters-for-destination"></a>Hedef için sanal ağ ve parametreler

Hedef için bir VM dağıtmadan önce, hedef sanal makinenin bulunabileceği bir sanal ağ oluşturmanız gerekir. Kaynak VM 'de, hedef uç noktayı açığa çıkarmak için bazı küçük değişikliklerle aynı adımlar aşağıda verilmiştir.

Bu bölümde, adımlarda aşağıdaki parametreleri aşağıdaki bilgilerle değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak grubu-adı >**  | myResourceGroupNAT |
| **\<sanal ağ-adı >** | myVNetdestination          |
| **\<bölge adı >**          | Doğu ABD 2      |
| **\<IPv4-adres-alanı >**   | 192.168.0.0 \ 16          |
| **\<alt ağ-adı >**          | mySubnetdestination        |
| **\<alt ağ-adres aralığı >** | 192.168.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-destination-virtual-machine"></a>Hedef sanal makine oluştur

1. Portalın sol üst **kısmında > ** **kaynak oluştur** ' u > **Ubuntu Server 18,04 LTS**' i seçin veya market aramasında **Ubuntu Server 18,04 LTS** ' yi arayın.

2. **Sanal makine oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri girin veya seçin:
   - **Abonelik** > **kaynak grubu**: **myresourcegroupnat**öğesini seçin.
   - **Örnek ayrıntıları** > **sanal makine adı**: **myvmdestination**girin.
   - **Örnek ayrıntıları** **bölge** > > **Doğu ABD 2**seçin.
   - **Yönetici hesabı** > **kimlik doğrulaması girin**: **parola**seçin.
   - **Yönetici hesabı** > **Kullanıcı adı**, **parola**ve **parola onaylama** bilgilerini girin.
   - **Gelen bağlantı noktası kuralları** > **ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver**öğesini seçin
   - **Gelen bağlantı noktası kuralları** > **gelen bağlantı noktalarını seçin**: **SSH (22)** ve **http (80)** seçeneğini belirleyin.
   - **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin.

3. **Ağ** sekmesinde aşağıdakilerin seçili olduğundan emin olun:
   - **Sanal ağ**: **myvnetdestination**
   - **Alt ağ**: **mysubnetdestination**
   - **Ortak ıp** > **Yeni oluştur**' u seçin.  **Genel IP adresi oluştur** penceresinde **ad** alanına **Mypublicipdestinationvm** girin. **SKU**için **Standart** ' ı seçin. Rest 'i varsayılan konumda bırakın ve **Tamam 'a**tıklayın.
   - **NIC ağ güvenlik grubu**: **temel**öğesini seçin.
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver**öğesini seçin.
   - **Gelen bağlantı noktalarını seçin**: **SSH** 'yi onaylayın ve **http** seçilidir.

4. **Yönetim** sekmesinde, **izleme**' nin altında, **önyükleme tanılamayı** **kapalı**olarak ayarlayın.

5. **İncele ve oluştur**’u seçin.

6. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

## <a name="prepare-a-web-server-and-test-payload-on-destination-vm"></a>Hedef VM 'de bir Web sunucusunu ve test yükünü hazırlama

İlk olarak hedef sanal makinenin IP adresini bulduk. 

1. Portalın sol tarafında **kaynak grupları**' nı seçin.
2. **Myresourcegroupnat**öğesini seçin.
3. **Myvmdestination**öğesini seçin.
4. **Genel**olarak, **genel IP adresi** değerini kopyalayın ve sanal makineye erişmek için kullanabilmek üzere not defteri 'ne yapıştırın.

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve sonra sonraki adımlarda kullanabilmeniz için bir not defteri 'ne yapıştırın. Bu, hedef sanal makine olduğunu gösterir.

### <a name="sign-in-to-destination-vm"></a>Hedef VM 'de oturum açma

Tarayıcınızda bir [Azure Cloud Shell](https://shell.azure.com) açın. Sanal makineye SSH için önceki adımda alınan IP adresini kullanın.

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

Bu komutlar, sanal makinenizi güncelleştirir, NGINX 'i yükler ve bir 100 KB dosya oluşturur. Bu dosya, NAT hizmeti kullanılarak kaynak VM 'den alınacaktır.

Hedef VM ile SSH oturumunu kapatın.

## <a name="prepare-test-on-source-vm"></a>Kaynak VM 'de testi hazırlama

İlk olarak, kaynak VM 'nin IP adresini keşfetmemiz gerekir.

1. Portalın sol tarafında **kaynak grupları**' nı seçin.
2. **Myresourcegroupnat**öğesini seçin.
3. **Myvmsource**öğesini seçin.
4. **Genel**olarak, **genel IP adresi** değerini kopyalayın ve sanal makineye erişmek için kullanabilmek üzere not defteri 'ne yapıştırın.

>[!IMPORTANT]
>Genel IP adresini kopyalayın ve sonra sonraki adımlarda kullanabilmeniz için bir not defteri 'ne yapıştırın. Bunun kaynak sanal makine olduğunu gösterir.

### <a name="log-into-source-vm"></a>Kaynak VM 'de oturum açma

Tarayıcınızda [Azure Cloud Shell](https://shell.azure.com) için yeni bir sekme açın.  Sanal makineye SSH için önceki adımda alınan IP adresini kullanın. 

```azurecli-interactive
ssh <username>@<ip-address-source>
```

NAT hizmetinin test edilmesine hazırlanmak için aşağıdaki komutları kopyalayıp yapıştırın.

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

Bu komut, sanal makinenizi güncelleştirir, git ' i yükler, GitHub 'dan [Hey](https://github.com/rakyll/hey) yükler ve kabuk ortamınızı güncelleştirmeyecektir.

Artık NAT hizmetini test etmeye hazırsınız.

## <a name="validate-nat-service"></a>NAT hizmetini doğrula

Kaynak VM 'de oturum açarken, hedef IP adresine istek oluşturmak için **kıvrımlı** ve **Hey** kullanabilirsiniz.

100-Kbayt dosyasını almak için kıvrımlı kullanın.  Aşağıdaki örnekteki **\<IP adresi-hedef >** , daha önce KOPYALADıĞıNıZ hedef IP adresiyle değiştirin.  **--Output** parametresi, alınan dosyanın atılacağını gösterir.

```bash
curl http://<ip-address-destination>/100k --output /dev/null
```

Ayrıca, **Hey**kullanarak bir dizi istek da oluşturabilirsiniz. Yeniden **\<IP adresi-hedef >** , daha önce KOPYALADıĞıNıZ hedef IP adresiyle değiştirin.

```bash
hey -n 100 -c 10 -t 30 --disable-keepalive http://<ip-address-destination>/100k
```

Bu komut, zaman aşımı süresi 30 saniye olan ve TCP bağlantısını yeniden kullanmadan 100 istek, 10 eşzamanlı bir işlem oluşturur.  Her istek 100 Kbayt alır.  Çalıştırmanın sonunda **, bu,** NAT hizmetinin ne kadar iyi olduğunu gösteren bir istatistik rapor eder.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, NAT ağ geçidini ve tüm ilgili kaynakları silin. NAT ağ geçidini içeren **Myresourcegroupnat** kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar
Bu öğreticide, bir NAT ağ geçidi oluşturdunuz, kaynak ve hedef VM oluşturdunuz ve sonra NAT ağ geçidini test ediyor.

NAT hizmeti 'nin çalışıyor olduğunu görmek için Azure Izleyici 'de ölçümleri gözden geçirin. Kullanılabilir SNAT bağlantı noktalarının kaynak tükenmesi gibi sorunları tanılayın.  SNAT bağlantı noktalarının kaynak tükenmesi, ek genel IP adresi kaynakları veya genel IP öneki kaynakları veya her ikisi eklenerek kolayca adreslenir.

- [Sanal ağ NAT](./nat-overview.md) hakkında bilgi edinin
- [NAT ağ geçidi kaynağı](./nat-gateway-resource.md)hakkında bilgi edinin.
- [Azure CLI kullanarak NAT Gateway kaynağını](./quickstart-create-nat-gateway-cli.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure PowerShell kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-powershell.md)dağıtmaya yönelik hızlı başlangıç.
- [Azure Portal kullanarak NAT ağ geçidi kaynağını](./quickstart-create-nat-gateway-portal.md)dağıtmaya yönelik hızlı başlangıç.

> [!div class="nextstepaction"]

