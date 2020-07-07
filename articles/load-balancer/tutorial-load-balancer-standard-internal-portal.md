---
title: 'Öğretici: iç yük dengeleyici oluşturma-Azure portal'
titleSuffix: Azure Load Balancer
description: Bu öğreticide, Azure portal kullanarak bir iç standart yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir.
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
ms.openlocfilehash: b8fcef13fbe41ac26b2a31d6871896428649eaa1
ms.sourcegitcommit: dee7b84104741ddf74b660c3c0a291adf11ed349
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85920860"
---
# <a name="tutorial-balance-internal-traffic-load-with-a-standard-load-balancer-in-the-azure-portal"></a>Öğretici: Azure portal standart yük dengeleyici ile iç trafik yükünü dengeleme

Yük Dengeleme, sanal makinelerde (VM) gelen istekleri yayarak daha yüksek düzeyde kullanılabilirlik ve ölçek sağlar. Standart yük dengeleyici oluşturmak ve VM 'Ler arasında iç trafiği dengelemek için Azure portal kullanabilirsiniz. Bu öğreticide, standart fiyatlandırma katmanında iç yük dengeleyici, arka uç sunucuları ve ağ kaynakları oluşturma ve yapılandırma işlemlerinin nasıl yapılacağı gösterilir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

İsterseniz, bu adımları Portal yerine [Azure CLI](load-balancer-get-started-ilb-arm-cli.md) veya [Azure PowerShell](load-balancer-get-started-ilb-arm-ps.md) kullanarak yapabilirsiniz.

Bu öğreticiyi kullanarak adımları uygulamak için Azure portal ' de oturum açın [https://portal.azure.com](https://portal.azure.com) .

## <a name="create-a-vnet-back-end-servers-and-a-test-vm"></a>VNet, arka uç sunucuları ve test VM 'si oluşturma

İlk olarak, bir sanal ağ (VNet) oluşturun. VNet 'te standart yük dengeleyicinizin arka uç havuzu için kullanılacak iki VM oluşturun ve yük dengeleyiciyi test etmek için kullanılacak üçüncü bir VM oluşturun. 

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

1. Portalın sol üst kısmında **kaynak oluştur**  >  **ağ**  >  **sanal ağ**' ı seçin.
   
1. **Sanal ağ oluştur** bölmesinde şu değerleri yazın veya seçin:
   
   - **Ad**: **myvnet**yazın.
   - **ResourceGroup**: **Yeni oluştur**' u seçin ve **Myresourcegrouplb**girin ve **Tamam**' ı seçin. 
   - **Alt ağ**  >  **Ad**: **Mybackendsubnet**yazın.
   
1. **Oluştur**'u seçin.

   ![Sanal ağ oluşturma](./media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

1. Portalın sol üst tarafında, **kaynak oluştur**  >  **işlem**  >  **Windows Server 2016 Datacenter**' u seçin. 
   
1. **Sanal makine oluştur**' da, **temel bilgiler** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik**  >  **Kaynak grubu**: açılır ve **Myresourcegrouplb**' ı seçin.
   - **Örnek ayrıntıları**  >  **Sanal makine adı**: **MyVM1**yazın.
   - **Örnek ayrıntıları**  >  **Bölge**: **Doğu ABD 2**seçin.
  
   
1. **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin. 
   
   Aşağıdakilerin seçili olduğundan emin olun:
   - **Sanal ağ**: **myvnet**
   - **Alt ağ**: **mybackendsubnet**
   - **NIC ağ güvenlik grubu**: **temel**öğesini seçin.
   - **Ortak ıp** > **Yeni oluştur** ' u seçin ve aşağıdaki değerleri girip **Tamam**' ı seçin:
       - **Ad**: **MyVM1-IP**
       - **SKU**: **Standart** seçin
   - **Ortak gelen bağlantı noktaları**: **Seçili bağlantı noktalarına izin ver**öğesini seçin.
   - **Gelen bağlantı noktalarını seçin**: açılır ve RDP 'yi seçin **(3389)**

   
   
1. **Yönetim** sekmesini seçin veya **İleri**  >  **Yönetim**' i seçin. **İzleme**altında, **önyükleme tanılamayı** **kapalı**olarak ayarlayın.
   
1. **İncele ve oluştur**’u seçin.
   
1. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin. 

1. **MyVM2**adlı ikinci bir sanal makine oluşturmak için adımları izleyin, diğer tüm ayarlar MyVM1 ile aynıdır. 

1. **Mytestvm**adlı üçüncü bir VM oluşturmak için adımları yeniden izleyin. 

## <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyici oluşturma

Portalı kullanarak standart bir iç yük dengeleyici oluşturun. Oluşturduğunuz ad ve IP adresi, yük dengeleyicinin ön ucu olarak otomatik olarak yapılandırılır.

1. Portalın sol üst tarafında, **kaynak oluştur**  >  **ağ**  >  **Load Balancer**' ı seçin.
   
2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni oluştur** ' u seçin ve metin kutusuna *Myresourcegrouplb* yazın.|
    | Name                   | *myLoadBalancer*                                   |
    | Bölge         | **Doğu ABD 2**’yi seçin.                                        |
    | Tür          | **Dahili**' ı seçin.                                        |
    | SKU           | **Standart**' ı seçin.                          |
    | Sanal ağ           | *Myvnet*' i seçin.                          |    
    | IP adresi ataması              | **Statik**’i seçin.   |
    | Özel IP adresi|Sanal ağınızın ve alt ağınızın adres alanında olan bir adres yazın, örneğin *10.3.0.7*.  |

3. **Gözden geçir + oluştur** sekmesinde **Oluştur**' a tıklayın. 
   

## <a name="create-standard-load-balancer-resources"></a>Standart yük dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adres havuzu ve bir sistem durumu araştırması için yük dengeleyici ayarlarını yapılandırır ve yük dengeleyici kurallarını belirtirsiniz.

### <a name="create-a-back-end-address-pool"></a>Arka uç adres havuzu oluşturma

Trafiği VM 'lere dağıtmak için, yük dengeleyici bir arka uç adres havuzu kullanır. Arka uç adres havuzu, yük dengeleyiciye bağlı sanal ağ arabirimlerinin (NIC 'ler) IP adreslerini içerir. 

**VM1 ve VM2 içeren bir arka uç adres havuzu oluşturmak için:**

1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
   
1. **Ayarlar** bölümünde **Arka uç havuzları**’nı ve sonra **Ekle**’yi seçin.
   
1. **Arka uç Havuzu Ekle** sayfasında, aşağıdaki değerleri yazın veya seçin:
   
   - **Ad**: **Mybackendpool**yazın.
   
1. **Sanal makineler**altında. 
   1. Arka uç havuzuna **MyVM1** ve **MyVM2** ekleyin.
   2. Her makineyi ekledikten sonra, açılır ve **ağ IP yapılandırması**' nı seçin. 
     
1. **Add (Ekle)** seçeneğini belirleyin.
   
   ![Arka uç adres havuzunu ekleme](./media/tutorial-load-balancer-standard-internal-portal/3-load-balancer-backend-02.png)
   
1. **Arka uç havuzları** sayfasında, **Mybackendpool** ' u genişletin ve hem **VM1** hem de **VM2** öğelerinin listelendiğinden emin olun.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicinin VM durumunu izlemesine izin vermek için, bir sistem durumu araştırması kullanırsınız. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. 

**VM 'lerin sistem durumunu izlemek üzere bir sistem durumu araştırması oluşturmak için:**

1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
   
1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
   
1. **Durum araştırması Ekle** sayfasında, aşağıdaki değerleri yazın veya seçin:
   
   - **Ad**: **myhealtharaştırması**yazın.
   - **Protokol**: açılır ve **http**' yi seçin. 
   - **Bağlantı noktası**: **80**yazın. 
   - **Yol**: **/** varsayılan URI için kabul edin. Bu değeri başka bir URI ile değiştirebilirsiniz. 
   - **Aralık**: **15**yazın. Aralık, yoklama denemeleri arasındaki saniye sayısıdır.
   - **Sağlıksız eşik**: tür **2**. Bu değer, bir VM 'nin sağlıksız olduğu kabul edilmeden önce oluşan ardışık yoklama hatalarının sayısıdır.
   
1. **Tamam**’ı seçin.
   
   ![Araştırma ekleyin](./media/tutorial-load-balancer-basic-internal-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, trafiğin sanal makinelere nasıl dağıtıldığını belirler. Kural, gelen trafik için ön uç IP yapılandırmasını, trafiği almak için arka uç IP havuzunu ve gerekli kaynak ve hedef bağlantı noktalarını tanımlar. 

**Myloadbalancerrule** adlı yük dengeleyici kuralı ön uç **loadbalancerön**ucunda 80 numaralı bağlantı noktasını dinler. Kural, bağlantı noktası 80 ' de, **Mybackendpool**arka uç adres havuzuna ağ trafiği gönderir. 

**Yük dengeleyici kuralı oluşturmak için:**

1. Sol taraftaki menüden **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.
   
1. **Ayarlar** bölümünde **Yük dengeleme kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. **Yük Dengeleme kuralı ekle** sayfasında, zaten mevcut değilse, aşağıdaki değerleri yazın veya seçin:
   
   - **Ad**: **myloadbalancerrule**yazın.
   - **Ön uç IP adresi:** Mevcut değilse **Loadbalancerön uç** yazın.
   - **Protokol**: **TCP**' yi seçin.
   - **Bağlantı noktası**: **80**yazın.
   - **Arka uç bağlantı noktası**: **80**yazın.
   - **Arka uç havuzu**: **mybackendpool**' u seçin.
   - **Durum araştırması**: **myhealtharaştırması**' ni seçin. 
   
Azure portal kullanarak [yüksek kullanılabilirlik bağlantı noktalarını](load-balancer-ha-ports-overview.md) yapılandırmak Için, **ha bağlantı noktaları** onay kutusunu seçin. Seçildiğinde, ilişkili bağlantı noktası ve protokol yapılandırması otomatik olarak doldurulur. 

1. **Tamam**’ı seçin.
   
   ![Yük dengeleyici kuralı ekleme](./media/tutorial-load-balancer-basic-internal-portal/5-load-balancing-rules.png)

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Arka uç sunucularına Internet Information Services (IIS) yükleyin, ardından özel IP adresini kullanarak yük dengeleyiciyi sınamak için MyTestVM ' yi kullanın. Her arka uç sanal makinesi varsayılan IIS Web sayfasının farklı bir sürümüne hizmet eder, böylece yük dengeleyici istekleri iki VM arasında dağıtır.

Portalda, **Myloadbalancer** **genel bakış** sayfasında, **özel IP adresi**altındaki IP adresini bulun. Adresin üzerine gelin ve kopyalamak için **Kopyala** simgesini seçin. Bu örnekte, **10.3.0.7**' dir. 

### <a name="connect-to-the-vms-with-rdp"></a>RDP ile VM 'lere bağlanma

İlk olarak, Uzak Masaüstü (RDP) ile üç VM 'ye bağlanın. 

>[!NOTE]
>Varsayılan olarak, VM 'Lerde Uzak Masaüstü erişimine izin vermek için **RDP** (Uzak Masaüstü) bağlantı noktası açık durumdadır. 

**Sanal makinelere uzak masaüstü (RDP) için:**

1. Portalda, sol taraftaki menüden **tüm kaynaklar** ' ı seçin. Kaynak listesinden, **Myresourcegrouplb** kaynak grubundaki her bir VM 'yi seçin.
   
1. **Genel bakış** sayfasında **Bağlan**' ı seçin ve ardından **RDP dosyasını indir**' i seçin. 
   
1. İndirdiğiniz RDP dosyasını açın ve **Bağlan**' ı seçin.
   
1. Windows güvenliği ekranında, **diğer seçenekler** ' i seçin ve **farklı bir hesap kullanın**. 
   
   Kullanıcı adı ve parola girin ve **Tamam**' ı seçin.
   
1. Herhangi bir sertifika istemine **Evet** yanıtı verin. 
   
   VM masaüstü yeni bir pencerede açılır. 

### <a name="install-iis-and-replace-the-default-iis-page-on-the-back-end-vms"></a>IIS 'yi yükler ve arka uç VM 'lerinde varsayılan IIS sayfasını değiştirme

Her arka uç sunucusunda, IIS yüklemek ve varsayılan IIS Web sayfasını özelleştirilmiş bir sayfayla değiştirmek için PowerShell 'i kullanın.

>[!NOTE]
>IIS yüklemek için **Sunucu Yöneticisi** içindeki **rol ve Özellik Ekleme Sihirbazı** ' nı da kullanabilirsiniz. 

**IIS yüklemek ve varsayılan Web sayfasını PowerShell ile güncelleştirmek için:**

1. MyVM1 ve MyVM2 üzerinde, **Başlat** menüsünden **Windows PowerShell** 'i başlatın. 

2. IIS yüklemek için aşağıdaki komutları çalıştırın ve varsayılan IIS Web sayfasını değiştirin:
   
   ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
      remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add custom htm file
      Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    ```
1. **Bağlantıyı kes**' i seçerek RDP bağlantılarını MyVM1 ve MyVM2 ile kapatın. VM 'Leri kapatmayın.

### <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. MyTestVM 'de **Internet Explorer**'ı açın ve herhangi bir yapılandırma isteminde **Tamam** ' ı yanıtlayın. 
   
1. Yük dengeleyicinin özel IP adresini (*10.3.0.7*) tarayıcının adres çubuğuna yapıştırın veya yazın. 
   
   Özelleştirilmiş IIS Web sunucusu varsayılan sayfası tarayıcıda görüntülenir. İleti, MyVM1 ' **dan Merhaba Dünya**veya MyVM2 ' **den Merhaba Dünya**okur.
   
1. Yük dengeleyicinin trafiği VM 'Ler arasında dağıtmalarını görmek için tarayıcıyı yenileyin. Ayrıca, denemeler arasında tarayıcı önbelleğinizi temizlemeniz gerekebilir.

   Bazı durumlarda, yük dengeleyici istekleri her bir arka uç sanal makinesine dağıttığı için, **MyVM1** sayfası görünür ve **MyVM2** sayfasının diğer zamanları görüntülenir. 

   ![Yeni IIS varsayılan sayfası](./media/tutorial-load-balancer-basic-internal-portal/9-load-balancer-test.png) 
   
## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyaç kalmadığında yük dengeleyiciyi ve tüm ilgili kaynakları silmek için **Myresourcegrouplb** kaynak grubunu açın ve **kaynak grubunu sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, standart bir iç yük dengeleyici oluşturdunuz. Ağ kaynakları, arka uç sunucuları, bir sistem durumu araştırması ve yük dengeleyici için kurallar oluşturdunuz ve yapılandırdınız. IIS 'yi arka uç VM 'lerine yüklediniz ve yük dengeleyiciyi tarayıcıda test etmek için bir test sanal makinesi kullandınız. 

Daha sonra, kullanılabilirlik alanları arasında VM 'Lerin yük dengelenmesi hakkında bilgi edinin.

> [!div class="nextstepaction"]
> [Farklı kullanılabilirlik bölgelerindeki VM’lerin yükünü dengeleme](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
