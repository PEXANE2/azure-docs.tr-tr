---
title: "Öğretici: VM'lere trafik bakiyesi yük - Azure portalı"
titleSuffix: Azure Load Balancer
description: Bu öğreticide, Azure portalını kullanarak Standard Load Balancer'ın nasıl oluşturulacağı ve yönetileceği gösterilir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: I want to create and Standard Load Balancer so that I can load balance internet traffic to VMs and add and remove VMs from the load-balanced set.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 03/11/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 8961a50490bdbf8b456e87e1c00577c2c8afd050
ms.sourcegitcommit: 253d4c7ab41e4eb11cd9995190cd5536fcec5a3c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/25/2020
ms.locfileid: "80240372"
---
# <a name="tutorial-load-balance-internet-traffic-to-vms-using-the-azure-portal"></a>Öğretici: Azure portalını kullanarak VM'lere bakiye li internet trafiğini yükleyin

Yük dengeleme, gelen istekleri birden fazla sanal makineye yayarak daha yüksek bir kullanılabilirlik ve ölçek düzeyi sağlar. Bu eğitimde, Internet trafiğini Sanal Taşıtlara dağıtan ve yüksek kullanılabilirlik sağlayan Azure Standart Yük Dengeleyicisi'nin farklı bileşenleri hakkında bilgi edinebilirsiniz. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:


> [!div class="checklist"]
> * Azure Yük Bakiyesi Oluşturma
> * Yük Dengeleyici kaynakları oluşturma
> * Sanal makineler oluşturma ve IIS sunucusunu yükleme
> * Yük Dengeleyici'ni iş başında görüntüleme
> * Yük Dengeleyicisinden VM ekleme ve çıkarma

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="create-a-standard-load-balancer"></a>Standart Yük Dengeleyici oluşturma

Bu bölümde, sanal makinelerin yük dengesini sağlayan bir Standart Yük Dengeleyicisi oluşturursunuz. Standart Yük Dengeleyici yalnızca Standart Genel IP adresini destekler. Standard Load Balancer oluşturduğunuzda, Standard Load Balancer için ön uç (varsayılan olarak *LoadBalancerFrontend* adını alır) olarak yapılandırılmış yeni bir Standart Genel IP adresi de oluşturmanız gerekir. 

1. Ekranın sol üst tarafında, **kaynak** > **Oluşturma** > **Yük Dengeleyicisi'ni**tıklatın.
2. **Yük bakiyesi oluştur** sayfasının **Temeller** sekmesinde, aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılanları kabul edin ve sonra Gözden Geçir + oluştur seçeneğini **belirleyin:**

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni Oluştur'u** seçin ve metin kutusuna *myResourceGroupSLB* yazın.|
    | Adı                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Genel'i**seçin.                                        |
    | SKU           | **Standart'ı**seçin.                          |
    | Genel IP adresi | **Yeni oluştur**’u seçin. |
    | Genel IP adresi adı              | Metin kutusuna *myPublicIP* yazın.   |
    |Kullanılabilirlik alanı| **Bölge yedekli**seçin.    |

3. Gözden **Geçir + oluştur** sekmesinde **Oluştur'u**tıklatın.

   ![Standart Yük Dengeleyici oluşturma](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Yük Dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adresi havuzu, bir sistem durumu sondası için Yük Dengeleyici ayarlarını yapılandırın ve bir dengeleyici kuralı belirtin.

### <a name="create-a-backend-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği VM'lere dağıtmak için, arka uç adres havuzu Yük Dengeleyicisine bağlı sanal (NIC) IP adreslerini içerir. Yük dengeleme internet trafiği için sanal makineleri içerecek şekilde arka uç adresi *myBackendPool* oluşturun.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** tıklatın.
2. **Ayarlar** bölümünde **Arka uç havuzları**’na ve sonra **Ekle**’ye tıklayın.
3. Ad için **bir arka uç havuzu** ekle sayfasında, *myBackendPool*yazın , arka uç havuzuiçin ad olarak, ve sonra **Ekle'yi**seçin.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük Dengeleyicisinin uygulamanızın durumunu izlemesine izin vermek için bir sistem durumu sondası kullanırsınız. Sistem durumu sondası, sağlık kontrollerine verdikleri yanıta bağlı olarak VM'leri Yük Dengeleyici döndürmesinden dinamik olarak ekler veya kaldırır. Sanal makinelerin durumunu izlemek için *myHealthProbe* durum araştırması oluşturun.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** tıklatın.
2. **Ayarlar** bölümünde **Durum araştırmaları**’na ve sonra **Ekle**’ye tıklayın.
3. Durum araştırması oluşturmak için şu değerleri kullanın:
     
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *myHealthProbe*girin. |
    | Protokol | **HTTP'yi**seçin. |
    | Bağlantı noktası | *80*girin.|
    | Interval | Sonda denemeleri arasında saniye cinsinden **Aralık** sayısı için *15* girin. |
    | Sağlıksız durum eşiği | VM sağlıksız olarak kabul edilmeden önce oluşması gereken **Sağlıksız eşik** veya ardışık sonda hataları sayısı için *2'yi* seçin.|
    
4. **Tamam'ı**seçin.

### <a name="create-a-load-balancer-rule"></a>Yük Dengeleyici kuralı oluşturma

Trafiğin sanal makinelere dağıtımını tanımlamak için bir Yük Dengeleyici kuralı kullanılır. Gerekli kaynak ve hedef bağlantı noktalarının yanı sıra gelen trafik için ön uç IP yapılandırması ve trafiği almak için arka uç IP havuzu tanımlamanız gerekir. *Frontend FrontendLoadBalancer* port80 dinlemek ve arka uç adres havuzu *myBackEndPool* da port 80 kullanarak yük dengeli ağ trafiği göndermek için bir Yük Dengeleyici kural *myLoadBalancerRuleWeb* oluşturun.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** tıklatın.
2. **Ayarlar** bölümünde **Yük dengeleme kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. Yük dengeleme kuralını yapılandırmak için bu değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | *MyHTTPRule*girin. |
    | Protokol | **TCP'yi**seçin. |
    | Bağlantı noktası | *80*girin.|
    | Arka uç bağlantı noktası | *80*girin. |
    | Arka uç havuzu | *myBackendPool'u*seçin.|
    | Durum yoklaması | *myHealthProbe'u*seçin. |
    
4. Diğer varsayılan ayarları olduğu gibi bırakın ve **Tamam**’ı seçin.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde, sanal bir ağ oluşturur, Yük Dengeleyicisi'nin arka uç havuzu için üç sanal makine oluşturur ve ardından Yük Dengeleyicisini test etmeye yardımcı olmak için Sanal makinelere IIS yüklersiniz.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroupSLB (Varolan kaynak grubunu seçin) |
| **\<sanal ağ adı>** | myVNet          |
| **\<bölge adı>**          | Batı Avrupa      |
| **\<IPv4 adres-boşluk>**   | 10.1.0.0/16          |
| **\<subnet-name>**          | mySubnet        |
| **\<alt net-adres aralığı>** | 10.1.0.0/24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Standart Yük Dengeleyicisi, vm'leri yalnızca arka uç havuzunda Standart IP adresleriyle destekler. Bu bölümde, daha önce oluşturulan Standart Yük Dengeleyicisinin arka uç havuzuna eklenen üç farklı bölgede *(Bölge 1,* *Bölge 2*ve *Bölge 3)* standart genel IP adresine sahip üç VM *(myVM1,* *myVM2*ve *myVM3)* oluşturacaksınız.

1. Portalın sol üst tarafında, **kaynak** > **Oluştur Windows** > **Server 2016 Datacenter'ı**seçin. 
   
1. **Sanal bir makine oluştur'da,** **TemelLer** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **Kaynak Grubu**: **myResourceGroupSLB'yi**seçin.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: *myVM1*yazın.
   - **Örnek Ayrıntılar** > **Bölge** > **Batı Avrupa**seçin.
   - **Örnek Ayrıntılar** > **Kullanılabilirlik Seçenekleri** > **Kullanılabilirlik bölgelerini**seçin. 
   - **Örnek Ayrıntılar** > **Kullanılabilirlik bölgesi** > Seçin **1**.
  
1. **Ağ** sekmesini seçin veya **Sonraki: Diskler,** sonra **Sonraki: Ağ**. 
   
   - Aşağıdakilerin seçildiğinden emin olun:
       - **Sanal ağ**: **myVnet**
       - **Alt ağ**: **myBackendSubnet**
       - **Ortak IP** > seçin **yeni oluştur**ve Ortak IP adresi **oluştur** penceresinde, **SKU**için , **Standart**seçin ve **Kullanılabilirlik bölgesi**için, **Bölge yedekli** seçin
      
   - **Ağ Güvenlik Grubu**altında yeni bir ağ güvenlik grubu (NSG) oluşturmak için Gelişmiş'i seçin. **Advanced** 
       1. **Yapılandır ağ güvenlik grubu** alanında **yeni oluştur'u**seçin. 
       1. *myNetworkSecurityGroup*yazın ve **Tamam'ı**seçin.

   - VM'yi Yük Dengeleyicisi'nin arka uç havuzunun bir parçası haline getirmek için aşağıdaki adımları tamamlayın:
        - **Yük Dengeleme,** **mevcut bir yük dengeleme çözümü arkasında bu sanal makine yerleştirin için?**, **Evet**seçin .
        - **Yük dengeleme ayarlarında**, **Yük dengeleme seçenekleri**için Azure **yük dengeleyicisi'ni**seçin.
        - **Bir yük dengeleyici seçin**için, *myLoadBalancer*. 
1. **Yönetim** sekmesini seçin veya **Sonraki** > **Yönetim'i**seçin. **İzleme**altında, **Boot tanılamayı** **Kapalı**olarak ayarlayın. 
1. **İncele ve oluştur**’u seçin.   
1. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin.
1. *MyVM2* ve *myVM3*olmak üzere iki ek VM oluşturmak için aşağıdaki adımları izleyin, **sırasıyla Kullanılabilirlik bölgesi** **2** ve **3'te** standart SKU genel IP adresi ve *myVM1*ile aynı diğer tüm ayarları.  

### <a name="create-network-security-group-rule"></a>Ağ güvenlik grubu kuralı oluşturma

Bu bölümde, HTTP kullanarak gelen bağlantılara izin vermek için bir ağ güvenlik grubu kuralı oluşturursunuz.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myResourceGroupSLB** kaynak grubunda bulunan **myNetworkSecurityGroup'u** tıklatın.
2. **Ayarlar**’ın altında **Gelen güvenlik kuralları**’na ve sonra **Ekle**’ye tıklayın.
3. 80 numaralı bağlantı noktasını kullanarak gelen HTTP bağlantılarına izin vermek için *myHTTPRule* adlı gelen güvenlik kuralı için şu değerleri girin:
    - **Kaynak** için *Hizmet Etiketi*.
    - **Kaynak hizmet etiketi** için *İnternet*
    - **Hedef bağlantı noktası aralıkları** için *80*
    - **Protokol** için *TCP*
    - **Eylem** için *İzin Ver*
    - **Öncelik** için *100*
    - isim için *myHTTPRule*
    - Açıklama için *HTTP’ye İzin Ver*
4. **Ekle'yi**seçin.

### <a name="install-iis-on-vms"></a>VM’lere IIS yükleme

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden *myResourceGroupSLB* kaynak grubunda bulunan **myVM1'i** tıklatın.
2. Sanal makineye yönelik RDP için **Genel Bakış** sayfasında **Bağlan**’a tıklayın.
3. **Sanal makineye bağlanın** açılır penceresinde **RDP Dosyasını İndir**'i seçin ve indirilen RDP dosyasını açın.
4. **Uzak Masaüstü Bağlantısı** penceresinde **Bağlan**'a tıklayın.
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
1. **Genel Bakış** ekranında Yük Dengeleyicisi'nin genel IP adresini bulun. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve sonra **myPublicIP'i**tıklatın.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

      ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük Dengeleyicisinin uygulamanızı çalıştıran üç VM'ye trafik dağıttığını görmek için web tarayıcınızı zorla yenileyebilirsiniz.

## <a name="remove-or-add-vms-from-the-backend-pool"></a>Arka uç havuzunda VM'leri kaldırma veya ekleme
Uygulamanızı çalıştıran VM’lerde işletim sistemi güncelleştirmelerini yükleme gibi bakım işlemleri gerçekleştirmeniz gerekebilir. Uygulamanıza gelen trafiği fazla trafiği yönetmek için ek VM’lere ihtiyaç duyabilirsiniz. Bu bölümde Yük Dengeleyicisinden vm *(myVM1)* nasıl kaldırılabileceğinizi veya ekleyeceğiniz gösterilmektedir.

### <a name="remove-vm-from-a-backend-pool"></a>VM'yi arka uç havuzundan kaldırma
*MyVM1'i* arka uç havuzundan kaldırmak için aşağıdaki adımları tamamlayın:

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** tıklatın.
2. **Ayarlar**'ın altında **Arka uç havuzları**'na tıklayın, ardından arka uç havuzunun listesinde **myBackendPool**'a tıklayın.
3. **myBackendPool** sayfasında, *VM1* kaldırmak için *myVM1*görüntüler satırın sonunda silme simgesini seçin ve sonra **Kaydet'i**tıklatın.

Artık *myVM1* arka uç adres havuzunda yer almadığından, *myVM1* üzerinde yazılım güncelleştirmelerini yükleme gibi tüm bakım görevlerini gerçekleştirebilirsiniz. *VM1*yokluğunda, yük şimdi *myVM2 ve myVM3* arasında dengelenir. *myVM3* 

### <a name="add-vm-to-a-backend-pool"></a>Arka uç havuzuna VM ekleme
*MyVM1'i* arka uç havuzuna geri eklemek için aşağıdaki adımları tamamlayın:

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynaklar'ı**seçin ve ardından kaynaklar listesinden **myVM1'i** seçin.
2. **VM1** sayfasında, **Ayarlar**altında **Ağ'ı**seçin.
3. **Ağ** **sayfasında, Yük dengeleme** sekmesini seçin ve ardından yük **dengeleme ekle'yi**seçin.
4. Yük **dengeleme ekle** sayfasında aşağıdakileri yapın:
   1. **Yük dengeleme seçenekleri**için Azure yük **dengeleyicisi'ni**seçin.
   2. **Bir yük dengeleyiciseçin**için *myLoadBalancer'ı*seçin.
   3. **Bir arka uç havuzu seçin,** *myBackendPool'u*seçin. 

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç duyulmadığında, kaynak grubunu, Yük Bakiyesini ve ilgili tüm kaynakları silin. Bunu yapmak için, Yük Dengeleyicisi'ni içeren *myResouceGroupSLB* kaynak grubunu seçin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir Standart Yük Dengeleyicisi oluşturdunuz, ona VM'ler iliştirdin, Yük Dengeleyicitrafik kuralını, sağlık sondasını yapılandırıldınız ve ardından Yük Dengeleyicisini test ettiniz. Ayrıca, yük dengeli kümeden bir VM kaldırdınız ve VM'yi arka uç adres havuzuna geri eklediniz. Azure Load Balancer hakkında daha fazla bilgi almak için Azure Load Balancer öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
