---
title: 'Hızlı başlangıç: ortak Load Balancer oluşturma-Azure portal'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure portal kullanılarak Load Balancer oluşturma gösterilmektedir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create a Load Balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2020
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: 99a2de4cd8a19d3f05b9dc37f3bcd08cd84b2e68
ms.sourcegitcommit: ff19f4ecaff33a414c0fa2d4c92542d6e91332f8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/18/2020
ms.locfileid: "85052752"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak VM 'Lerin yükünü dengelemek için Load Balancer oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Sanal makinelerin (VM) yük dengelemesini yapmak amacıyla yük dengeleyici oluşturmak için Azure portalını kullanabilirsiniz. Bu hızlı başlangıçta, ortak bir Load Balancer kullanarak VM 'Lerin yükünü dengelemek gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-load-balancer"></a>Yük Dengeleyici oluşturma

Bu bölümde, sanal makinelerin yük dengelemeye yardımcı olan bir Load Balancer oluşturursunuz. Ortak bir Load Balancer veya iç Load Balancer oluşturabilirsiniz. Ortak bir Load Balancer oluşturduğunuzda, Load Balancer için ön uç (varsayılan olarak *Loadbalancerön uç* olarak adlandırılır) olarak yapılandırılmış yeni BIR genel IP adresi de oluşturmanız gerekir.

1. Ekranın sol üst kısmında, **kaynak oluştur**  >  **ağ**  >  **Load Balancer**' ı seçin.
2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni oluştur** ' u seçin ve metin kutusuna *Myresourcegroupslb* yazın.|
    | Name                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Ortak**seçeneğini belirleyin.                                        |
    | SKU           | **Standart** veya **temel**seçeneğini belirleyin. Microsoft, üretim iş yükleri için standart önerir. |
    | Genel IP adresi | **Yeni oluştur**’u seçin. Kullanmak istediğiniz var olan bir genel IP varsa, **var olanı kullan** ' ı seçin. |
    | Genel IP adresi adı              | Metin kutusuna *Mypublicıp* yazın.   ```-SKU Basic```Temel genel IP oluşturmak için kullanın. Temel genel IP 'Ler **Standart** yük dengeleyici ile uyumlu değildir. Microsoft, üretim iş yükleri için **Standart** kullanılmasını önerir.|
    | Kullanılabilirlik alanı | Esnek bir Load Balancer oluşturmak için *bölge yedekli* yazın. Bir bölgesel Load Balancer oluşturmak için, 1, 2 veya 3 ' ten belirli bir bölge seçin |

> [!IMPORTANT]
> Bu hızlı başlangıçtaki geri kalanı, yukarıdaki SKU seçim sürecinde **Standart** SKU 'nun seçili olduğunu varsayar.


3. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.   

    ![Standart Yük Dengeleyici oluşturma](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Load Balancer kaynakları oluşturma

Bu bölümde, bir arka uç adres havuzu ve bir sistem durumu araştırması için Load Balancer ayarlarını yapılandırır ve bir dengeleyici kuralı belirlersiniz.

### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Trafiği VM 'lere dağıtmak için bir arka uç adres havuzu, Load Balancer bağlı sanal (NIC) IP adreslerini içerir. Yük Dengeleme internet trafiği için sanal makineleri dahil etmek üzere *Mybackendpool* arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
2. **Ayarlar**altında **arka uç havuzları**' nı seçin ve ardından **Ekle**' yi seçin.
3. **Arka uç Havuzu Ekle** sayfasında, ad için, arka uç havuzunuzun adı olarak *mybackendpool*yazın ve ardından **Ekle**' yi seçin.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Load Balancer uygulamanızın durumunu izlemesine izin vermek için, bir sistem durumu araştırması kullanırsınız. Sistem durumu araştırması, sistem durumu denetimlerine olan yanıtına göre Load Balancer dönüşten VM 'Leri dinamik olarak ekler veya kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
2. **Ayarlar**altında **sistem durumu araştırmaları**' nı ve ardından **Ekle**' yi seçin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *Myhealtharaştırması*girin. |
    | Protokol | **Http**'yi seçin. |
    | Bağlantı noktası | *80*girin.|
    | Interval | Yoklama denemeleri arasındaki saniye cinsinden **Aralık** sayısı için *15* girin. |
    | Sağlıksız durum eşiği | Bir VM sağlıksız kabul edilmeden önce gerçekleşmesi gereken **sağlıksız eşik** veya arka arkaya araştırma hatası sayısı için **2** ' yi seçin.|
    | | |
4. **Tamam**’ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma
Trafiğin sanal makinelere dağıtımını tanımlamak için bir Yük Dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. Ön uç *Frontendloadbalancer* 80 numaralı bağlantı noktasını dinlemek ve yük dengeli ağ trafiğini *mybackendpool* bağlantı noktası 80 ' i kullanarak arka uç adres havuzuna göndermek Için *myloadbalancerruleweb* Load Balancer kuralını oluşturun. 

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
2. **Ayarlar**bölümünde **Yük Dengeleme kuralları**' nı ve ardından **Ekle**' yi seçin.
3. Yük dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *Myhttprule*girin. |
    | Protokol | **TCP**' yi seçin. |
    | Bağlantı noktası | *80*girin.|
    | Arka uç bağlantı noktası | *80*girin. |
    | Arka uç havuzu | *Mybackendpool*öğesini seçin.|
    | Durum yoklaması | *Myhealtharaştırması*' ni seçin. |
4. Kalan varsayılan değerleri bırakın ve **Tamam**' ı seçin.


## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, bir sanal ağ oluşturur, Load Balancer arka uç havuzu için üç sanal makine oluşturur ve sonra Load Balancer test etmenize yardımcı olması için sanal makinelere IIS yüklersiniz.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, adımlarda aşağıdaki parametreleri aşağıdaki bilgilerle değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupSLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Batı Avrupa      |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma
Genel IP SKU 'Ları ve Load Balancer SKU 'Ları eşleşmelidir. Standart Load Balancer için, arka uç havuzundaki standart IP adresleriyle VM 'Leri kullanın. Bu bölümde, daha sonra daha önce oluşturulmuş Load Balancer arka uç havuzuna eklenen üç farklı bölgede (*bölge 1*, *bölge 2*ve *Bölge 3*) standart bir genel IP adresi ile üç VM (*myVM1*, *myVM2* ve *myVM3*) oluşturacaksınız. Temel ' yı seçtiyseniz, temel IP adresleriyle VM 'Leri kullanın.

1. Portalın sol üst tarafında, **kaynak oluştur**  >  **işlem**  >  **Windows Server 2019 Datacenter**' u seçin. 
   
1. **Sanal makine oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik**  >  **Kaynak grubu**: **Myresourcegroupslb**öğesini seçin.
   - **Örnek ayrıntıları**  >  **Sanal makine adı**: *myVM1*yazın.
   - **Örnek ayrıntıları**  >  **Bölge** > **Batı Avrupa**seçin.
   - **Örnek ayrıntıları**  >  Kullanılabilirlik **seçenekleri** > **kullanılabilirlik alanları**' nı seçin. 
   - **Örnek ayrıntıları**  >  **Kullanılabilirlik bölgesi** **1**> seçin.
   - **Yönetici hesabı**> **Kullanıcı adı**, **parola** ve **parola onaylama** bilgilerini girin.
   - **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin.
  
1. **Ağ** sekmesinde aşağıdakilerin seçili olduğundan emin olun:
   - **Sanal ağ**: *myvnet*
   - **Alt ağ**: *mybackendsubnet*
   - **Genel ıp** > **Yeni oluştur**' u seçin ve **genel IP adresi oluştur** penceresinde, **SKU**için **Standart**' ı seçin ve **kullanılabilirlik bölgesi**için, bölgesel olarak **yedekli**' i seçin ve ardından **Tamam**' ı seçin. Temel bir Load Balancer oluşturduysanız temel ' yı seçin. Microsoft, üretim iş yükleri için standart SKU kullanmayı önerir.
   - Yeni bir ağ güvenlik grubu (NSG) oluşturmak için, **ağ güvenlik grubu**altında bir güvenlik duvarı türü **Gelişmiş**' i seçin. 
       1. **Ağ güvenlik grubunu yapılandır** alanında **Yeni oluştur**' u seçin. 
       1. *Mynetworksecuritygroup*yazın ve **Tamam**' ı seçin.
   - VM 'yi Load Balancer arka uç havuzunun bir parçası yapmak için aşağıdaki adımları izleyin:
        - **Yük dengelemesinde**, **Bu sanal makineyi var olan bir yük dengeleme çözümünün arkasına koymak**için **Evet**' i seçin.
        - **Yük Dengeleme ayarları**' nda, **Yük dengeleme seçenekleri**için **Azure yük dengeleyici**' ni seçin.
        - **Yük dengeleyici seçin**için *myloadbalancer*.
        - **Yönetim** sekmesini seçin veya **İleri**  >  **Yönetim**' i seçin.
2. **Yönetim** sekmesinde, **izleme**' nin altında, **önyükleme tanılamayı** **kapalı**olarak ayarlayın. 
1. **İncele ve oluştur**’u seçin.   
1. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.
1. Aşağıdaki değerleri ve *myVM1*ile aynı diğer tüm ayarları içeren Iki ek VM oluşturmak için 2 ile 6 arasındaki adımları uygulayın:

    | Ayar | VM 2| VM 3|
    | ------- | ----- |---|
    | Name |  *myVM2* |*myVM3*|
    | Kullanılabilirlik alanı | 2 |3|
    |Genel IP| **Standart** ISTEYIN|**Standart** ISTEYIN|
    | Genel IP kullanılabilirlik alanı| **Bölge yedekli** |**Bölge yedekli**|
    | Ağ güvenlik grubu | Mevcut *Mynetworksecuritygroup* 'u seçin| Mevcut *Mynetworksecuritygroup* 'u seçin|

 ### <a name="create-nsg-rule"></a>NSG kuralı oluşturma

Bu bölümde, HTTP kullanarak gelen bağlantılara izin veren bir ağ güvenlik grubu kuralı oluşturacaksınız.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myresourcegroupslb** kaynak grubunda bulunan **mynetworksecuritygroup** ' u seçin.
2. **Ayarlar** bölümünde **Gelen güvenlik kuralları**’nı ve sonra **Ekle**’yi seçin.
3. 80 numaralı bağlantı noktasını kullanarak gelen HTTP bağlantılarına izin vermek için *myHTTPRule* adlı gelen güvenlik kuralı için şu değerleri girin:
    - **Kaynak**: *hizmet etiketi*
    -  **Kaynak hizmet etiketi**: *Internet*
    - **Hedef bağlantı noktası aralıkları**: *80*
    - **Protokol**: *TCP*
    - **Eylem**: *izin ver*
    - **Öncelik**: *100*
    - **Ad**: *myhttprule* 
    - **Açıklama**: "*http 'ye izin ver* 
4. **Ekle**'yi seçin.
5. Gerekirse, gelen RDP kuralı için aşağıdaki farklı değerlerle adımları yineleyin:
   - **Hedef bağlantı noktası aralıkları**: *3389*yazın.
   - **Öncelik**: *200*yazın. 
   - **Ad**: *Myrdprule*yazın. 
   - **Açıklama**: *RDP 'ye izin ver*yazın. 
 
### <a name="install-iis"></a>IIS yükleme

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden, *Myresourcegroupslb* kaynak grubunda bulunan **myVM1** ' yi seçin.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’ı seçin.
5. VM oluşturma işlemleri sırasında belirlediğiniz kimlik bilgilerini kullanarak VM'de oturum açın. *myVM1* adlı sanal makinede uzak masaüstü oturumu başlatılır.
6. Sunucu masaüstünde **Windows Yönetim Araçları** > **Windows PowerShell**' e gidin.
7. PowerShell Penceresinde aşağıdaki komutları çalıştırarak IIS sunucusunu yükleyin, varsayılan iisstart.htm dosyasını kaldırın ve ardından VM’nin adını gösteren yeni bir iisstart.htm dosyasını ekleyin:

   ```azurepowershell-interactive
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
6. *myVM1* ile RDP oturumunu kapatın.
7. IIS’yi ve *myVM2* ve *myVM3*’teki güncelleştirilmiş iisstart.htm dosyasını yüklemek için 1 ile 6 arasındaki adımları tekrarlayın.

## <a name="test-the-load-balancer"></a>Load Balancer test etme
1. **Genel bakış** ekranında Load Balancer genel IP adresini bulun. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından **mypublicıp**' yi seçin.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

   ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Trafiği üç VM 'ye dağıtmak Load Balancer görmek için, her bir sanal makinenin IIS Web sunucusunun varsayılan sayfasını özelleştirebilir ve sonra Web tarayıcınızı istemci makinesinden yenileyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, kaynak grubunu, Load Balancer ve tüm ilgili kaynakları silin. Bunu yapmak için, Load Balancer içeren kaynak grubunu (*Myresourcegroupslb*) seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Standart Load Balancer oluşturdunuz, bu sanal makineye bağlı VM 'Ler Load Balancer trafik kuralını ve sistem durumu araştırmasını yapılandırdınız ve ardından Load Balancer test edilmiştir. Azure Load Balancer hakkında daha fazla bilgi edinmek için [öğreticilere Azure Load Balancer](tutorial-load-balancer-standard-public-zone-redundant-portal.md)devam edin.

[Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
