---
title: 'Öğretici: Dahili yük dengeleyicisi oluşturma - Azure portalı'
titleSuffix: Azure Load Balancer
description: Bu öğretici, Azure portalını kullanarak dahili bir Standart yük dengeleyicisini nasıl oluşturabileceğinizi gösterir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: As an IT administrator, I want to create a load balancer that load balances incoming internal traffic to virtual machines within a specific zone in a region.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 01/08/2020
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 918a7700df6b5be3ebca7949875127e42f8d3a91
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75780384"
---
# <a name="tutorial-balance-internal-traffic-load-with-a-standard-load-balancer-in-the-azure-portal"></a>Öğretici: Azure portalındaki standart yük dengeleyicisiyle dahili trafik yükünü dengeleme

Yük dengeleme, gelen istekleri sanal makinelere (VM) yayatarak daha yüksek bir kullanılabilirlik düzeyi ve ölçek sağlar. Standart yük dengeleyicisi oluşturmak ve VM'ler arasındaki iç trafiği dengelemek için Azure portalını kullanabilirsiniz. Bu öğretici, standart fiyatlandırma katmanında bir dahili yük dengeleyicisi, arka uç sunucular ve ağ kaynaklarını nasıl oluşturacağınızı ve yapılandıracağınızı gösterir.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

İsterseniz, bu adımları portal yerine [Azure CLI](load-balancer-get-started-ilb-arm-cli.md) veya [Azure PowerShell'i](load-balancer-get-started-ilb-arm-ps.md) kullanarak yapabilirsiniz.

Bu öğreticiyi kullanarak adımları yapmak için, 'deki [https://portal.azure.com](https://portal.azure.com)Azure portalında oturum açın.

## <a name="create-a-vnet-back-end-servers-and-a-test-vm"></a>Bir VNet, arka uç sunucuları ve test VM oluşturma

İlk olarak, sanal ağ (VNet) oluşturun. VNet'te, Standart yük dengeleyicinizin arka uç havuzu için kullanılacak iki VM ve yük dengeleyicisini test etmek için kullanılacak üçüncü bir VM oluşturun. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Portalın sol üst tarafında, **Bir kaynak** > **Oluştur** > **Sanal ağ oluştur'u**seçin.
   
1. Sanal **ağ oluştur** bölmesinde şu değerleri yazın veya seçin:
   
   - **Adı**: **MyVNet**yazın .
   - **ResourceGroup**: **Yeni Oluştur'u**seçin, ardından **MyResourceGroupLB'yi**girin ve **Tamam'ı**seçin. 
   - **Alt Ağ** > **Adı**: **MyBackendSubnet**yazın.
   
1. **Oluştur'u**seçin.

   ![Sanal ağ oluşturma](./media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Portalın sol üst tarafında, **kaynak** > **Oluştur Windows** > **Server 2016 Datacenter'ı**seçin. 
   
1. **Sanal bir makine oluştur'da,** **TemelLer** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **Kaynak Grubu**: Açılır ve **MyResourceGroupLB'yi**seçin.
   - **Örnek Ayrıntılar** > **Sanal makine adı**: **MyVM1**yazın.
   - **Örnek Ayrıntılar** > **Bölge**: Doğu **ABD 2**seçin.
  
   
1. **Ağ** sekmesini seçin veya **Sonraki: Diskler,** sonra **Sonraki: Ağ**. 
   
   Aşağıdakilerin seçildiğinden emin olun:
   - **Sanal ağ**: **MyVNet**
   - **Alt ağ**: **MyBackendSubnet**
   - **NIC ağ güvenlik grubu**: **Temel'i**seçin.
   - **Genel IP** > Seçin **Yeni oluştur** ve aşağıdaki değerleri girin ve **Tamam'ı**seçin:
       - **Adı**: **MyVM1-IP**
       - **SKU**: **Standart** Seç
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver'i**seçin.
   - **Gelen bağlantı noktalarını seçin**: Bırak ve **RDP'yi seçin (3389)**

   
   
1. **Yönetim** sekmesini seçin veya **Sonraki** > **Yönetim'i**seçin. **İzleme**altında, **Boot tanılamayı** **Kapalı**olarak ayarlayın.
   
1. **İncele ve oluştur**’u seçin.
   
1. Ayarları gözden geçirin ve ardından **Oluştur'u**seçin. 

1. **MyVM2**adlı ikinci bir VM oluşturmak için adımları izleyin , tüm diğer ayarları MyVM1 ile aynı. 

1. **MyTestVM**adında üçüncü bir VM oluşturmak için adımları yeniden izleyin. 

## <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyicisi oluşturma

Portalı kullanarak standart bir dahili yük dengeleyicioluşturun. Oluşturduğunuz ad ve IP adresi otomatik olarak yük dengeleyicisinin ön ucu olarak yapılandırılır.

1. Portalın sol üst tarafında, kaynak > **Ağ** > **Yük Dengeleyicisi** **Oluştur'u**seçin.
   
2. **Yük bakiyesi oluştur** sayfasının **Temeller** sekmesinde, aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılanları kabul edin ve sonra Gözden Geçir + oluştur seçeneğini **belirleyin:**

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Metin kutusunda **yeni oluştur'u** ve *MyResourceGroupLB* yazın'ı seçin.|
    | Adı                   | *myLoadBalancer*                                   |
    | Bölge         | **Doğu ABD 2**’yi seçin.                                        |
    | Tür          | **Dahili'yi**seçin.                                        |
    | SKU           | **Standart'ı**seçin.                          |
    | Sanal ağ           | *MyVNet'i*seçin.                          |    
    | IP adresi ataması              | **Statik**’i seçin.   |
    | Özel IP adresi|Sanal ağınızın ve alt ağınızın adres alanına bulunan bir adres yazın, örneğin *10.3.0.7*.  |

3. Gözden **Geçir + oluştur** sekmesinde **Oluştur'u**tıklatın. 
   

## <a name="create-standard-load-balancer-resources"></a>Standart yük dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adres havuzu ve sistem durumu sondası için yük dengeleyici ayarlarını yapılandırın ve yük dengeleyici kurallarını belirtin.

### <a name="create-a-back-end-address-pool"></a>Arka uç adres havuzu oluşturma

VM'lere trafik dağıtmak için yük dengeleyicisi arka uç adresi havuzu kullanır. Arka uç adres havuzu, yük dengeleyicisine bağlı sanal ağ arabirimlerinin (NIC) IP adreslerini içerir. 

**VM1 ve VM2 içeren bir arka uç adres havuzu oluşturmak için:**

1. Sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **MyLoadBalancer'ı** seçin.
   
1. **Ayarlar** bölümünde **Arka uç havuzları**’nı ve sonra **Ekle**’yi seçin.
   
1. Arka **uç havuzu** ekle sayfasında aşağıdaki değerleri yazın veya seçin:
   
   - **Adı**: **MyBackendPool**yazın.
   
1. **Sanal makineler**altında . 
   1. **MyVM1** ve **MyVM2'yi** arka uç havuzuna ekleyin.
   2. Her makineyi ekledikten sonra, aşağı bırakın ve **Ağ IP yapılandırmasını**seçin. 
     
1. **Ekle'yi**seçin.
   
   ![Arka uç adresi havuzunu ekleme](./media/tutorial-load-balancer-standard-internal-portal/3-load-balancer-backend-02.png)
   
1. **Backend havuzları** sayfasında **MyBackendPool'u** genişletin ve hem **VM1** hem de **VM2'nin** listelendirilip listelendirilediğinden emin olun.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicisinin VM durumunu izlemesine izin vermek için bir sistem durumu sondası kullanırsınız. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. 

**VM'lerin durumunu izlemek için bir sistem durumu sondası oluşturmak için:**

1. Sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **MyLoadBalancer'ı** seçin.
   
1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
   
1. Sistem **durumu sondası** ekle sayfasında aşağıdaki değerleri yazın veya seçin:
   
   - **Adı**: **MyHealthProbe**yazın.
   - **Protokol**: Aşağı in ve **HTTP**seçin. 
   - **Bağlantı Noktası**: Tip **80**. 
   - **Yol**: **/** Varsayılan URI için kabul edin. Bu değeri başka bir URI ile değiştirebilirsiniz. 
   - **Aralık**: Tip **15**. Aralık, sonda denemeleri arasındaki saniye sayısıdır.
   - **Sağlıksız eşik**: Tip **2**. Bu değer, VM sağlıksız olarak kabul edilmeden önce oluşan ardışık sonda hatalarının sayısıdır.
   
1. **Tamam'ı**seçin.
   
   ![Sonda ekleme](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, trafiğin sanal makinelere nasıl dağıtıldığını belirler. Kural, gelen trafik için ön uç IP yapılandırmasını, trafiği almak için arka uç IP havuzunu ve gerekli kaynak ve hedef bağlantı noktalarını tanımlar. 

**MyLoadBalancerRule** adlı yük dengeleyici kuralı ön uç **LoadBalancerFrontEnd**port 80 dinler. Kural arka uç adres havuzu **MyBackendPool**ağ trafiği gönderir , ayrıca bağlantı noktası 80. 

**Yük dengeleyici kuralını oluşturmak için:**

1. Sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **MyLoadBalancer'ı** seçin.
   
1. **Ayarlar** bölümünde **Yük dengeleme kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. Yük **dengeleme kuralı** ekle sayfasında, zaten mevcut değilse aşağıdaki değerleri yazın veya seçin:
   
   - **Adı**: **MyLoadBalancerRule**yazın.
   - **Frontend IP adresi:** Mevcut değilse **LoadBalancerFrontEnd** yazın.
   - **Protokol**: **TCP'yi**seçin.
   - **Bağlantı Noktası**: Tip **80**.
   - **Arka uç bağlantı noktası**: Tip **80**.
   - **Arka uç havuzu**: **MyBackendPool'u**seçin.
   - **Sağlık sondası**: **MyHealthProbe'u**seçin. 
   
1. **Tamam'ı**seçin.
   
   ![Yük dengeleyici kuralı ekleme](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Arka uç sunuculara Internet Bilgi Hizmetleri (IIS) yükleyin ve ardından yük bakiyesini özel IP adresini kullanarak test etmek için MyTestVM'i kullanın. Her arka uç VM varsayılan IIS web sayfasının farklı bir sürümünü hizmet vermektedir, böylece yük dengeleyicisinin iki VM arasında istekleri dağıttığını görebilirsiniz.

Portalda, **MyLoadBalancer**için **Genel Bakış** sayfasında, **Özel IP Adresi**altında IP adresini bulmak. Adresin üzerine titreyin ve kopyalamak için **Kopyala** simgesini seçin. Bu örnekte, **10.3.0.7'dir.** 

### <a name="connect-to-the-vms-with-rdp"></a>RDP ile VM'lere bağlanma

İlk olarak, Uzak Masaüstü (RDP) ile üç VM'ye de bağlanın. 

>[!NOTE]
>Varsayılan olarak, VM'ler uzak masaüstü erişimine izin vermek için **RDP** (Uzak Masaüstü) bağlantı noktasına zaten açıktır. 

**VM'lere uzak masaüstüne (RDP):**

1. Portalda, sol menüdeki **Tüm kaynakları** seçin. Kaynak listesinden **MyResourceGroupLB** kaynak grubundaki her VM'yi seçin.
   
1. Genel **Bakış** sayfasında **Bağlan'ı**seçin ve ardından **RDP dosyasını indir'i**seçin. 
   
1. İndirdiğiniz RDP dosyasını açın ve **Bağlan'ı**seçin.
   
1. Windows Security **ekranında, daha fazla seçenek** seçin ve sonra farklı bir hesap **kullanın.** 
   
   Kullanıcı adı ve parola girin ve sonra **Tamam'ı**seçin.
   
1. Herhangi bir sertifika istemine **Evet** yanıtı verin. 
   
   VM masaüstü yeni bir pencerede açılır. 

### <a name="install-iis-and-replace-the-default-iis-page-on-the-back-end-vms"></a>IIS'yi yükleyin ve arka uç VM'lerde varsayılan IIS sayfasını değiştirin

Her arka uç sunucusunda, IIS'yi yüklemek ve varsayılan IIS web sayfasını özelleştirilmiş bir sayfayla değiştirmek için PowerShell'i kullanın.

>[!NOTE]
>IIS'yi yüklemek için Server **Manager'daki** **Rol ve Özellikler Ekle Sihirbazı'nı** da kullanabilirsiniz. 

**IIS'yi yüklemek ve varsayılan web sayfasını PowerShell ile güncelleştirmek için:**

1. MyVM1'de ve MyVM2'de **Başlat** menüsünden **Windows PowerShell'i** başlatın. 

2. IIS'yi yüklemek ve varsayılan IIS web sayfasını değiştirmek için aşağıdaki komutları çalıştırın:
   
   ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
      remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add custom htm file
      Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
1. **Bağlantıyı Kesme'yi**seçerek RDP bağlantılarını MyVM1 ve MyVM2 ile kapatın. VM'leri kapatmayın.

### <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. MyTestVM'de **Internet Explorer'ı**açın ve yapılandırma istemlerine **Tamam** yanıt verin. 
   
1. Yük bakiyesinin özel IP adresini *(10.3.0.7)* tarayıcının adres çubuğuna yapıştırın veya yazın. 
   
   Özelleştirilmiş IIS web sunucusu varsayılan sayfası tarayıcıda görünür. Mesaj myVM1 veya Hello World **myVM2**ya Hello **World**okur.
   
1. Yük dengeleyicisinin VM'ler arasında trafik dağıttığını görmek için tarayıcıyı yenileyin. Ayrıca denemeler arasında tarayıcı önbelleği temizlemek gerekebilir.

   Bazen **MyVM1** sayfası görünür, bazen de yük bakiyesi istekleri her arka uç VM'ye dağıttığı için **MyVM2** sayfası görünür. 

   ![Yeni IIS varsayılan sayfası](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png) 
   
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmadığında yük bakiyesini ve ilgili tüm kaynakları silmek için **MyResourceGroupLB** kaynak grubunu açın ve **kaynak grubunu sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, standart bir iç yük dengeleyicisi oluşturdunuz. Ağ kaynaklarını, arka uç sunucularını, sistem durumu sondasını ve yük dengeleyicisi kurallarını oluşturdunuz ve yapılandırıldınız. Arka uç VM'lere IIS yüklediniz ve tarayıcıdaki yük dengeleyicisini test etmek için bir test VM kullandınız. 

Ardından, bakiye VM'lerini kullanılabilirlik bölgeleri arasında nasıl yükleyin.

> [!div class="nextstepaction"]
> [Farklı kullanılabilirlik bölgelerindeki VM’lerin yükünü dengeleme](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
