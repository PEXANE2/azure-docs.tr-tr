---
title: 'Öğretici: iç yük dengeleyici oluşturma-Azure portal'
titleSuffix: Azure Load Balancer
description: Bu öğretici, Azure portalını kullanarak iç temel yük dengeleyici oluşturma işlemini göstermektedir.
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
ms.date: 02/27/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: 6f62771d707d1aebccbfaf809dee7d0dedf5fefa
ms.sourcegitcommit: be53e74cd24bbabfd34597d0dcb5b31d5e7659de
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/11/2020
ms.locfileid: "79096122"
---
# <a name="tutorial-balance-internal-traffic-load-with-a-basic-load-balancer-in-the-azure-portal"></a>Öğretici: Bakiye iç trafik yükü ile Azure portalında bir temel yük dengeleyici

Yük Dengeleme, sanal makineye (VM) gelen istekleri yayarak daha yüksek düzeyde kullanılabilirlik ve ölçek sağlar. Basic load balancer oluşturma ve VM'ler arasında iç trafiği dengelemek için Azure portalını kullanabilirsiniz. Bu öğreticide oluşturma ve iç yük dengeleyici, arka uç sunucularının ve ağ kaynakları temel fiyatlandırma katmanında yapılandırma gösterilmektedir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

İsterseniz, bu adımları Portal yerine [Azure CLI](load-balancer-get-started-ilb-arm-cli.md) veya [Azure PowerShell](load-balancer-get-started-ilb-arm-ps.md) kullanarak yapabilirsiniz.

Bu öğreticiyi kullanarak adımları uygulamak için [https://portal.azure.com](https://portal.azure.com)Azure Portal oturum açın.

## <a name="create-a-vnet-back-end-servers-and-a-test-vm"></a>Bir sanal ağ, arka uç sunucularının ve bir test sanal makinesi oluşturma

İlk olarak bir sanal ağın (VNet) oluşturun. Sanal ağda Yük Dengeleyiciyi test etmek için kullanılacak temel yük dengeleyici ve üçüncü bir VM arka uç havuzu için kullanılacak iki VM oluşturun. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Portalın sol üst kısmında **bir kaynak oluştur** > **ağ** > **sanal ağ**' ı seçin.
   
1. **Sanal ağ oluştur** bölmesinde şu değerleri yazın veya seçin:
   
   - **Ad**: *myvnet*yazın.
   - **ResourceGroup**: **Yeni oluştur**' u seçin ve *Myresourcegrouplb*girin ve **Tamam**' ı seçin. 
   - **Alt ağ** > **adı**: *mybackendsubnet*yazın.
   
1. **Oluştur**’u seçin.

   ![Sanal ağ oluşturma](./media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Portalın sol üst kısmında **Windows Server 2016 Datacenter** > **Işlem** > **kaynak oluştur** ' u seçin. 
   
1. **Sanal makine oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **kaynak grubu**: açılır ve **myresourcegrouplb**' ı seçin.
   - **Örnek ayrıntıları** > **sanal makine adı**: tür *MyVM1*.
   - **Örnek ayrıntıları** > **kullanılabilirlik seçenekleri**: 
     1. Açılır ve **kullanılabilirlik kümesi**' ni seçin. 
     2. **Yeni oluştur**' u seçin, *MyAvailabilitySet*yazın ve **Tamam**' ı seçin.
   
1. **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin. 
   
   Aşağıdaki seçili olduğundan emin olun:
   - **Sanal ağ**: **myvnet**
   - **Alt ağ**: **mybackendsubnet**
   
   **Ağ güvenlik grubu**altında:
   1. **Gelişmiş**'i seçin. 
   1. Açılan **ağ güvenlik grubunu yapılandırın** ve **yok**' u seçin. 
   
1. **Yönetim** sekmesini seçin veya **İleri** > **Yönetim**' i seçin. **İzleme**altında, **önyükleme tanılamayı** **kapalı**olarak ayarlayın.
   
1. **İncele ve oluştur**’u seçin.
   
1. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin. 

1. *MyVM2*adlı ikinci bir sanal makine oluşturmak için adımları izleyin, diğer tüm ayarlar MyVM1 ile aynıdır. 

1. *Mytestvm*adlı üçüncü bir VM oluşturmak için adımları yeniden izleyin. 

## <a name="create-a-basic-load-balancer"></a>Temel yük dengeleyici oluşturma

Portalı kullanarak bir iç temel yük dengeleyici oluşturun. Oluşturduğunuz IP adresi ve adını, load balancer'ın ön ucu olarak otomatik olarak yapılandırılır.

1. Portalın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici** seçeneğini belirleyin.
   
2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni oluştur** ' u seçin ve metin kutusuna *Myresourcegrouplb* yazın.|
    | Adı                   | *myLoadBalancer*                                   |
    | Bölge         | **Doğu ABD 2**’yi seçin.                                        |
    | Tür          | **Dahili**' ı seçin.                                        |
    | SKU           | **Temel**'i seçin.                          |
    | Sanal ağ           | *Myvnet*' i seçin.                          |    
    | IP adresi ataması              | **Statik**öğesini seçin.   |
    | Özel IP adresi|Sanal ağınızın ve alt ağınızın adres alanında olan bir adres yazın, örneğin *10.3.0.7*.  |

3. **Gözden geçir + oluştur** sekmesinde **Oluştur**' a tıklayın. 
   

## <a name="create-basic-load-balancer-resources"></a>Temel yük dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adres havuzu ve bir sistem durumu araştırması için yük dengeleyici ayarlarını yapılandırır ve yük dengeleyici kurallarını belirtirsiniz.

### <a name="create-a-back-end-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği Vm'lere dağıtmak için yük dengeleyici arka uç adres havuzu kullanır. Arka uç adres havuzundaki IP adreslerini yük dengeleyiciye bağlı sanal ağ arabirimlerini (NIC'ler) içerir. 

**VM1 ve VM2 içeren bir arka uç adres havuzu oluşturmak için:**

1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
   
1. **Ayarlar** bölümünde **Arka uç havuzları**’nı ve sonra **Ekle**’yi seçin.
   
1. **Arka uç Havuzu Ekle** sayfasında, aşağıdaki değerleri yazın veya seçin:
   
   - **Ad**: *Mybackendpool*yazın.
   - **İlişkili**: aşağı açılan ve **sanal makine**seçin.
   
   
1. **Sanal makineyi**seçin. 
   1. Arka uç havuzuna **MyVM1** ve **MyVM2** ekleyin.
   2. Her makineyi ekledikten sonra, açılır ve **ağ IP yapılandırması**' nı seçin. 
   
   >[!NOTE]
   >**Mytestvm** 'yi havuza eklemeyin. 
   
1. **Tamam**’ı seçin.
   
   ![Arka uç adres havuzu ekleme](./media/tutorial-load-balancer-basic-internal-portal/3-load-balancer-backend-02.png)
   
1. **Arka uç havuzları** sayfasında, **Mybackendpool** ' u genişletin ve hem **VM1** hem de **VM2** öğelerinin listelendiğinden emin olun.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

VM durumunu izlemek için yük dengeleyicisine izin vermek için durum araştırması kullanabilirsiniz. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. 

**VM 'lerin sistem durumunu izlemek üzere bir sistem durumu araştırması oluşturmak için:**

1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
   
1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
   
1. **Durum araştırması Ekle** sayfasında, aşağıdaki değerleri yazın veya seçin:
   
   - **Ad**: *myhealtharaştırması*yazın.
   - **Protokol**: açılır ve **http**' yi seçin. 
   - **Bağlantı noktası**: *80*yazın. 
   - **Yol**: varsayılan urı için */* kabul edin. Diğer bir URI ile bu değeri değiştirebilirsiniz. 
   - **Aralık**: *15*yazın. Araştırma denemeleri arasındaki saniye sayısını aralığıdır.
   - **Sağlıksız eşik**: tür *2*. Bu değer bir VM kötü olarak kabul edilmeden önce gerçekleşmesi ardışık araştırma hatası sayısıdır.
   
1. **Tamam**’ı seçin.
   
   ![Bir araştırma eklemek](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, trafiğin sanal makinelere nasıl dağıtıldığını belirler. Ön uç IP yapılandırmasını gelen trafiğe, trafik ve gerekli kaynak ve hedef bağlantı noktalarını almak için arka uç IP havuzu için kuralı tanımlar. 

**Myloadbalancerrule** adlı yük dengeleyici kuralı ön uç **loadbalancerön**ucunda 80 numaralı bağlantı noktasını dinler. Kural, bağlantı noktası 80 ' de, **Mybackendpool**arka uç adres havuzuna ağ trafiği gönderir. 

**Yük dengeleyici kuralı oluşturmak için:**

1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
   
1. **Ayarlar** bölümünde **Yük dengeleme kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. **Yük Dengeleme kuralı ekle** sayfasında, zaten mevcut değilse, aşağıdaki değerleri yazın veya seçin:
   
   - **Ad**: *myloadbalancerrule*yazın.
   - **Ön uç IP adresi:** Mevcut değilse *Loadbalancerön uç* yazın.
   - **Protokol**: **TCP**' yi seçin.
   - **Bağlantı noktası**: *80*yazın.
   - **Arka uç bağlantı noktası**: *80*yazın.
   - **Arka uç havuzu**: **mybackendpool**' u seçin.
   - **Durum araştırması**: **myhealtharaştırması**' ni seçin. 
   
1. **Tamam**’ı seçin.
   
   ![Yük Dengeleyici Kuralı Ekle](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Internet Information Services (IIS) arka uç sunucularına yükleyin ve ardından MyTestVM özel IP adresini kullanarak Yük Dengeleyiciyi test etmek için kullanın. Her arka uç VM, yük dengeleyicinin istekler iki VM arasında dağıtmasını görebilmeniz için varsayılan IIS web sayfasına, farklı bir sürümü işlevi görür.

Portalda, **Myloadbalancer** **genel bakış** sayfasında, **özel IP adresi**altındaki IP adresini bulun. Adresin üzerine gelin ve kopyalamak için **Kopyala** simgesini seçin. Bu örnekte, **10.3.0.7**' dir. 

### <a name="connect-to-the-vms-with-rdp"></a>Vm'lere RDP ile bağlanma

İlk olarak, tüm üç VM ile Uzak Masaüstü (RDP) bağlanın. 

>[!NOTE]
>Varsayılan olarak, VM 'Lerde Uzak Masaüstü erişimine izin vermek için **RDP** (Uzak Masaüstü) bağlantı noktası açık durumdadır. 

**Sanal makinelere uzak masaüstü (RDP) için:**

1. Portalda, sol taraftaki menüden **tüm kaynaklar** ' ı seçin. Kaynak listesinden, **Myresourcegrouplb** kaynak grubundaki her bir VM 'yi seçin.
   
1. **Genel bakış** sayfasında **Bağlan**' ı seçin ve ardından **RDP dosyasını indir**' i seçin. 
   
1. İndirdiğiniz RDP dosyasını açın ve **Bağlan**' ı seçin.
   
1. Windows güvenliği ekranında, **diğer seçenekler** ' i seçin ve **farklı bir hesap kullanın**. 
   
   Kullanıcı adı ve parola girin ve **Tamam**' ı seçin.
   
1. Herhangi bir sertifika istemine **Evet** yanıtı verin. 
   
   VM masaüstüne yeni bir pencerede açılır. 

### <a name="install-iis-and-replace-the-default-iis-page-on-the-back-end-vms"></a>IIS yüklemek ve arka uç sanal makinelerin varsayılan IIS sayfasında değiştirin

Her arka uç sunucusunda, IIS yüklemek ve varsayılan IIS web sayfasına içeren özelleştirilmiş bir sayfa değiştirmek için PowerShell kullanın.

>[!NOTE]
>IIS yüklemek için **Sunucu Yöneticisi** içindeki **rol ve Özellik Ekleme Sihirbazı** ' nı da kullanabilirsiniz. 

**IIS yüklemek ve varsayılan Web sayfasını PowerShell ile güncelleştirmek için:**

1. MyVM1 ve MyVM2 üzerinde, **Başlat** menüsünden **Windows PowerShell** 'i başlatın. 

2. IIS yüklemek ve varsayılan IIS web sayfasına değiştirmek için aşağıdaki komutları çalıştırın:
   
   ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
1. **Bağlantıyı kes**' i seçerek RDP bağlantılarını MyVM1 ve MyVM2 ile kapatın. Vm'lerini kapatmayın.

### <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. MyTestVM 'de **Internet Explorer**'ı açın ve herhangi bir yapılandırma isteminde **Tamam** ' ı yanıtlayın. 
   
1. Yük dengeleyicinin özel IP adresini (*10.3.0.7*) tarayıcının adres çubuğuna yapıştırın veya yazın. 
   
   Özelleştirilmiş bir IIS web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir. İleti, MyVM1 ' **dan Merhaba Dünya**veya MyVM2 ' **den Merhaba Dünya**okur.
   
1. Yük dengeleyicinin trafiği VM'ye dağıtmasını görmek için tarayıcıyı yenileyin. Denemeler arasındaki tarayıcı önbelleğini temizlemeniz gerekebilir.

   Bazı durumlarda, yük dengeleyici istekleri her bir arka uç sanal makinesine dağıttığı için, **MyVM1** sayfası görünür ve **MyVM2** sayfasının diğer zamanları görüntülenir. 

   ![Yeni IIS varsayılan sayfası](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png) 
   
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç kalmadığında yük dengeleyiciyi ve tüm ilgili kaynakları silmek için **Myresourcegrouplb** kaynak grubunu açın ve **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, temel katmanı iç yük dengeleyici oluşturdunuz. Oluşturduğunuz ve ağ kaynaklarına, arka uç sunucuları, bir durum araştırması ve yük dengeleyici kuralları yapılandırılmış. Arka uç sanal makinelere IIS yüklemesini ve bir test sanal makinesi yük dengeleyici, tarayıcıda test etmek için kullanılan. 

Ardından, sanal makinelerin kullanılabilirlik alanları genelinde Yük Dengelemesi konusunda bilgi edinin.

> [!div class="nextstepaction"]
> [Kullanılabilirlik bölgelerindeki VM’lerde yük dengeleme](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
