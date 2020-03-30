---
title: Quickstart:Ortak Yük Dengeleyicisi Oluşturma - Azure portalı
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıç, Azure portalını kullanarak Yük Dengeleyicisi'nin nasıl oluşturulacağımı gösterir.
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
ms.openlocfilehash: b424fe315737b84479283eed2d77398c8ce4f148
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78898823"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak bakiye VM'lerini yüklemek için yük bakiyesi oluşturma

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Sanal makinelerin (VM) yük dengelemesini yapmak amacıyla yük dengeleyici oluşturmak için Azure portalını kullanabilirsiniz. Bu hızlı başlangıç, genel yük dengeleyicisini kullanarak bakiye VM'lerini nasıl yükleydiğinizi gösterir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="create-a-load-balancer"></a>Yük Dengeleyici oluşturma

Bu bölümde, sanal makineleri dengelemeye yardımcı olan bir Yük Dengeleyicisi oluşturursunuz. Ortak yük dengeleyicisi veya dahili Yük Dengeleyicisi oluşturabilirsiniz. Bir ortak Yük Dengeleyicisi oluşturduğunuzda, Yük Dengeleyicisi için ön uç (varsayılan olarak *LoadBalancerFrontend* olarak adlandırılır) olarak yapılandırılan yeni bir Genel IP adresi de oluşturmanız gerekir.

1. Ekranın sol üst tarafında, **kaynak** > **Oluştur** > **Yük Dengeleyicisini**seçin.
2. **Yük bakiyesi oluştur** sayfasının **Temeller** sekmesinde, aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılanları kabul edin ve sonra Gözden Geçir + oluştur seçeneğini **belirleyin:**

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni Oluştur'u** seçin ve metin kutusuna *myResourceGroupSLB* yazın.|
    | Adı                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Genel'i**seçin.                                        |
    | SKU           | **Standart** veya **Temel'i**seçin. Microsoft, üretim iş yükleri için Standart önerir. |
    | Genel IP adresi | **Yeni oluştur**’u seçin. Kullanmak istediğiniz mevcut bir Genel IP'niz varsa, **varolan Kullanım'ı** seçin |
    | Genel IP adresi adı              | Metin kutusuna *myPublicIP* yazın.   Temel ```-SKU Basic``` Genel IP oluşturmak için kullanın. Temel Genel IP'ler **Standart** yük dengeleyicisi ile uyumlu değildir. Microsoft, üretim iş yükleri için **Standart'ı** kullanmanızı önerir.|
    | Kullanılabilirlik alanı | Esnek bir Yük Dengeleyicisi oluşturmak için *Bölge yedekli* yazın. Bir zonal Yük Dengeleyicisi oluşturmak için, 1, 2 veya 3'ten belirli bir bölge seçin |

> [!IMPORTANT]
> Bu hızlı başlatmanın geri kalanı, yukarıdaki SKU seçim işlemi sırasında **Standart** SKU'nun seçildiğini varsayar.


3. Gözden **Geçir + oluştur** sekmesinde **Oluştur'u**seçin.   

    ![Standart Yük Dengeleyici oluşturma](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Yük Dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adresi havuzu, bir sistem durumu sondası için Yük Dengeleyici ayarlarını yapılandırın ve bir dengeleyici kuralı belirtin.

### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Trafiği VM'lere dağıtmak için, arka uç adres havuzu Yük Dengeleyicisine bağlı sanal (NIC) IP adreslerini içerir. Yük dengeleme internet trafiği için sanal makineleri içerecek şekilde arka uç adresi *myBackendPool* oluşturun.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.
2. **Ayarlar'ın**altında, **Arka uç havuzlarını**seçin, ardından **Ekle'yi**seçin.
3. Ad için **bir arka uç havuzu** ekle sayfasında, *myBackendPool*yazın , arka uç havuzuiçin ad olarak, ve sonra **Ekle'yi**seçin.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük Dengeleyicisinin uygulamanızın durumunu izlemesine izin vermek için bir sistem durumu sondası kullanırsınız. Sistem durumu sondası, sağlık kontrollerine verdikleri yanıta bağlı olarak VM'leri Yük Dengeleyici döndürmesinden dinamik olarak ekler veya kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.
2. **Ayarlar**altında, **Sistem Durumu sondalarını**seçin ve ardından **Ekle'yi**seçin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *myHealthProbe*girin. |
    | Protokol | **HTTP'yi**seçin. |
    | Bağlantı noktası | *80*girin.|
    | Interval | Sonda denemeleri arasında saniye cinsinden **Aralık** sayısı için *15* girin. |
    | Sağlıksız durum eşiği | VM sağlıksız olarak kabul edilmeden önce oluşması gereken **Sağlıksız eşik** veya ardışık sonda hataları sayısı için **2'yi** seçin.|
    | | |
4. **Tamam'ı**seçin.

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma
Trafiğin sanal makinelere dağıtımını tanımlamak için bir Yük Dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. *Frontend FrontendLoadBalancer* port80 dinlemek ve arka uç adres havuzu *myBackEndPool* da port 80 kullanarak yük dengeli ağ trafiği göndermek için bir Yük Dengeleyici kural *myLoadBalancerRuleWeb* oluşturun. 

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.
2. **Ayarlar** **altında, Yük dengeleme kurallarını**seçin ve sonra **Ekle'yi**seçin.
3. Yük dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *MyHTTPRule*girin. |
    | Protokol | **TCP'yi**seçin. |
    | Bağlantı noktası | *80*girin.|
    | Arka uç bağlantı noktası | *80*girin. |
    | Arka uç havuzu | *myBackendPool'u*seçin.|
    | Durum yoklaması | *myHealthProbe'u*seçin. |
4. Varsayılanların geri kalanını bırakın ve sonra **Tamam'ı**seçin.


## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, sanal bir ağ oluşturur, Yük Dengeleyicisi'nin arka uç havuzu için üç sanal makine oluşturur ve ardından Yük Dengeleyicisini test etmeye yardımcı olmak için Sanal makinelere IIS yüklersiniz.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroupSLB |
| **\<sanal ağ adı>** | myVNet          |
| **\<bölge adı>**          | Batı Avrupa      |
| **\<IPv4 adres-boşluk>**   | 10.1.0.0\16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<alt net-adres aralığı>** | 10.1.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma
Genel IP SK'ler ve Yük Dengeleyici SK'leri eşleşmelidir. Standart Yük Dengeleyicisi için arka uç havuzunda Standart IP adreslerine sahip VM'ler kullanın. Bu bölümde, daha sonra daha önce oluşturulan Yük Dengeleyicisi'nin arka uç havuzuna eklenen üç farklı bölgede *(Bölge 1,* *Bölge 2*ve *Bölge 3)* standart bir genel IP adresine sahip üç VM *(myVM1,* *myVM2* ve *myVM3)* oluşturacaksınız. Basic'i seçtiyseniz, Temel IP adreslerine sahip VM'leri kullanın.

1. Portalın sol üst tarafında, **kaynak** > **Oluştur Windows** > **Server 2019 Datacenter'ı**seçin. 
   
1. **Sanal bir makine oluştur'da,** **TemelLer** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupSLB'yi**seçin.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: *myVM1*yazın.
   - **Örnek Ayrıntılar** > **Bölge** > **Batı Avrupa**seçin.
   - **Örnek Ayrıntılar** > **Kullanılabilirlik Seçenekleri** > **Kullanılabilirlik bölgelerini**seçin. 
   - **Örnek Ayrıntılar** > **Kullanılabilirlik bölgesi** > Seçin **1**.
   - **Yönetici hesabı**> **Kullanıcı Adı**, **Şifre** ve Şifre Bilgilerini **Onayla.**
   - **Ağ** sekmesini seçin veya **Sonraki: Diskler,** sonra **Sonraki: Ağ**.
  
1. **Ağ** sekmesinde aşağıdakilerin seçildiğinden emin olun:
   - **Sanal ağ**: *myVnet*
   - **Alt ağ**: *myBackendSubnet*
   - **Genel IP** > **yeni oluştur'u**seçin ve **SKU**için ortak IP adresi **oluştur** penceresinde, **Standart'ı**seçin ve **Kullanılabilirlik bölgesi**için Bölge **yedekli'yi**seçin ve ardından **Tamam'ı**seçin. Bir Temel Yük Dengeleyicisi oluşturduysanız, Temel'i seçin. Microsoft, üretim iş yükleri için Standart SKU kullanılmasını önerir.
   - **Ağ Güvenlik Grubu**altında yeni bir ağ güvenlik grubu (NSG) oluşturmak için Gelişmiş'i seçin. **Advanced** 
       1. **Yapılandır ağ güvenlik grubu** alanında **yeni oluştur'u**seçin. 
       1. *myNetworkSecurityGroup*yazın ve **Tamam'ı**seçin.
   - VM'yi Yük Dengeleyicisi'nin arka uç havuzunun bir parçası haline getirmek için aşağıdaki adımları tamamlayın:
        - **Yük Dengeleme,** **mevcut bir yük dengeleme çözümü arkasında bu sanal makine yerleştirin için?**, **Evet**seçin .
        - **Yük dengeleme ayarlarında**, **Yük dengeleme seçenekleri**için Azure **yük dengeleyicisi'ni**seçin.
        - **Bir yük dengeleyici seçin**için, *myLoadBalancer*.
        - **Yönetim** sekmesini seçin veya **Sonraki** > **Yönetim'i**seçin.
2. **Yönetim** sekmesinde, **İzleme**altında, **Önyükleme tanılamasını** **Kapalı**olarak ayarlayın. 
1. **İncele ve oluştur**’u seçin.   
1. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.
1. Aşağıdaki değerlere ve *myVM1*ile aynı diğer tüm ayarlara sahip iki ek VM oluşturmak için 2 ila 6 adımlarını izleyin:

    | Ayar | VM 2| VM 3|
    | ------- | ----- |---|
    | Adı |  *myVM2* |*myVM3*|
    | Kullanılabilirlik alanı | 2 |3|
    |Genel IP| **Standart** Sku|**Standart** Sku|
    | Genel IP - Kullanılabilirlik bölgesi| **Bölge yedekli** |**Bölge yedekli**|
    | Ağ güvenlik grubu | Varolan *myNetworkSecurity Grubunu* seçin| Varolan *myNetworkSecurity Grubunu* seçin|

 ### <a name="create-nsg-rule"></a>NSG kuralı oluşturma

Bu bölümde, HTTP kullanarak gelen bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturursunuz.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myResourceGroupSLB** kaynak grubunda bulunan **myNetworkSecurityGroup'u** seçin.
2. **Ayarlar** bölümünde **Gelen güvenlik kuralları**’nı ve sonra **Ekle**’yi seçin.
3. 80 numaralı bağlantı noktasını kullanarak gelen HTTP bağlantılarına izin vermek için *myHTTPRule* adlı gelen güvenlik kuralı için şu değerleri girin:
    - **Kaynak**: *Hizmet Etiketi*
    -  **Kaynak servis etiketi**: *Internet*
    - **Hedef bağlantı noktası aralıkları**: *80*
    - **Protokol**: *TCP*
    - **Action**: *İzin ver*
    - **Öncelik**: *100*
    - **Adı**: *myHTTPRule* 
    - **Açıklama**: "*İzin VER HTTP* 
4. **Ekle'yi**seçin.
5. Gerekirse gelen RDP kuralı için aşağıdaki farklı değerlerle adımları yineleyin:
   - **Hedef bağlantı noktası aralıkları**: Type *3389*.
   - **Öncelik**: Tip *200*. 
   - **Adı**: *MyRDPRule*yazın. 
   - **Açıklama**: Type *Allow RDP*. 
 
### <a name="install-iis"></a>IIS yükleme

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden *myResourceGroupSLB* kaynak grubunda bulunan **myVM1'i** seçin.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’ı seçin.
5. VM oluşturma işlemleri sırasında belirlediğiniz kimlik bilgilerini kullanarak VM'de oturum açın. *myVM1* adlı sanal makinede uzak masaüstü oturumu başlatılır.
6. Sunucu masaüstünde, **Windows Yönetim Araçları**>**Windows PowerShell**gidin.
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

## <a name="test-the-load-balancer"></a>Yük Dengeleyicisini Test Edin
1. **Genel Bakış** ekranında Yük Dengeleyicisi'nin genel IP adresini bulun. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve sonra **myPublicIP'i**seçin.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

   ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük Dengeleyici'nin trafiği üç VM'ye de dağıttığını görmek için, her VM'nin IIS Web sunucusunun varsayılan sayfasını özelleştirebilir ve ardından web tarayıcınızı istemci makinesinden zorla yenileyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekmediğinde, kaynak grubunu, Yük Bakiyesini ve ilgili tüm kaynakları silin. Bunu yapmak için, Yük Bakiyesi'ni içeren kaynak grubunu *(myResourceGroupSLB)* seçin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Standart Yük Dengeleyicisi oluşturdunuz, ona VM'ler iliştirdin, Yük Dengeleyicitrafik kuralını, sistem sondasını yapılandırdın ve ardından Yük Dengeleyicisini test ettiniz. Azure Yük Bakiyesi hakkında daha fazla bilgi edinmek için [Azure Yük Bakiyesi öğreticilerine](tutorial-load-balancer-standard-public-zone-redundant-portal.md)devam edin.

[Yük Dengeleyicisi ve Kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
