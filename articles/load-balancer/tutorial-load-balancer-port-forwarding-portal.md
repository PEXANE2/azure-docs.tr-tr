---
title: 'Öğretici: Bağlantı noktası iletme yapılandırma - Azure portalı'
titleSuffix: Azure Load Balancer
description: Bu öğretici, bir Azure sanal ağında VM'lere bağlantı oluşturmak için Azure Yük Bakiyesi kullanarak bağlantı noktası iletmenin nasıl yapılandırılabildiğini gösterir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: twooley
Customer intent: As an IT administrator, I want to configure port forwarding in Azure Load Balancer to remotely connect to VMs in an Azure virtual network.
ms.service: load-balancer
ms.devlang: na
ms.topic: tutorial
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 02/26/2019
ms.author: allensu
ms.custom: seodec18
ms.openlocfilehash: e740a65d453a69a987e938a5170ae8e04c7bfe40
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78249874"
---
# <a name="tutorial-configure-port-forwarding-in-azure-load-balancer-using-the-portal"></a>Öğretici: Portalı kullanarak Azure Yük Dengeleyici'nde bağlantı noktası yönlendirmesini yapılandırın

Bağlantı noktası yönlendirme, Azure Yük Bakiyesi genel IP adresi ve bağlantı noktası numarasını kullanarak Azure sanal ağındaki sanal makinelere (VM) bağlanmanızı sağlar. 

Bu eğitimde, bir Azure Yük Dengeleyicisi üzerinde bağlantı noktası yönlendirmesi ayarlayın. Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * VM'ler üzerindeki ağ trafiğini dengelemek için genel bir Standart yük dengeleyicioluşturun. 
> * Ağ güvenlik grubu (NSG) kuralına sahip sanal ağ ve Sanal M'ler oluşturun. 
> * VM'leri yük bakiyesi arka uç adres havuzuna ekleyin.
> * Bir yük dengeleyici sağlık sondası ve trafik kuralları oluşturun.
> * Yük dengeleyici gelen NAT bağlantı noktası iletme kuralları oluşturun.
> * Yük dengelemeve bağlantı noktası yönlendirmeyi iş başında görüntülemek için IIS'yi VM'lere yükleyin ve yapılandırın.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

Bu öğreticideki tüm adımlar için Azure portalında oturum aç. [https://portal.azure.com](https://portal.azure.com)

## <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyicisi oluşturma

İlk olarak, VM'ler üzerindeki trafik yükünü dengeleyebilen genel bir Standart yük dengeleyicioluşturun. Standart yük dengeleyicisi yalnızca standart bir genel IP adresini destekler. Standart yük dengeleyicisi oluşturduğunuzda, yük dengeleyiciön ucu olarak yapılandırılan ve varsayılan olarak **LoadBalancerFrontEnd** olarak adlandırılan yeni bir Standart genel IP adresi de oluşturursunuz. 

1. Ekranın sol üst tarafında, **kaynak** > **Oluşturma** > **Yük Dengeleyicisi'ni**tıklatın.
2. **Yük bakiyesi oluştur** sayfasının **Temeller** sekmesinde, aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılanları kabul edin ve sonra Gözden Geçir + oluştur seçeneğini **belirleyin:**

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Metin kutusunda **yeni oluştur'u** ve *MyResourceGroupLB* yazın'ı seçin.|
    | Adı                   | *myLoadBalancer*                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Genel'i**seçin.                                        |
    | SKU           | **Standart'ı**seçin.                          |
    | Genel IP adresi | **Yeni oluştur**’u seçin. |
    | Genel IP adresi adı              | Metin kutusuna *myPublicIP* yazın.   |
    |Kullanılabilirlik alanı| **Bölge yedekli**seçin.    |
     
    >[!NOTE]
     >Doluluk Bölgelerini destekleyen bir konumda Yük Dengeleyicinizi ve bunun için tüm kaynakları oluşturduğunuzdan emin olun. Daha fazla bilgi için [bkz.](../availability-zones/az-overview.md#services-support-by-region) 

3. Gözden **Geçir + oluştur** sekmesinde **Oluştur'u**tıklatın.  
  
## <a name="create-and-configure-back-end-servers"></a>Arka uç sunucuları oluşturma ve yapılandırma

İki sanal makineden oluşan bir sanal ağ oluşturun ve VM'leri yük bakiyenizin arka uç havuzuna ekleyin. 

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroupLB (Varolan kaynak grubunu seçin) |
| **\<sanal ağ adı>** | myVNet          |
| **\<bölge adı>**          | Batı Avrupa      |
| **\<IPv4 adres-boşluk>**   | 10.3.0.0\16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<alt net-adres aralığı>** | 10.3.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-vms-and-add-them-to-the-load-balancer-back-end-pool"></a>VM'ler oluşturun ve bunları yük dengeleyici arka uç havuzuna ekleyin

1. Portalın sol üst tarafında, **kaynak** > **Oluştur Windows** > **Server 2016 Datacenter'ı**seçin. 
   
1. **Sanal bir makine oluştur'da,** **TemelLer** sekmesinde aşağıdaki değerleri yazın veya seçin:
   - **Abonelik** > **Kaynak Grubu**: Açılır ve **MyResourceGroupLB'yi**seçin.
   - **Sanal makine adı**: *MyVM1*yazın.
   - **Bölge**: **Batı Avrupa'yı**seçin. 
   - **Kullanıcı Adı**: *Azureuser*yazın.
   - **Şifre**: *Azure1234567*yazın. 
     **Parolayı Onayla** alanına yeniden yazın.
   
1. **Ağ** sekmesini seçin veya **Sonraki: Diskler,** sonra **Sonraki: Ağ**. 
   
   Aşağıdakilerin seçildiğinden emin olun:
   - **Sanal ağ**: **MyVNet**
   - **Alt ağ**: **MyBackendSubnet**
   
1. **Genel IP**altında, **yeni oluştur'u**seçin, Ortak IP adresi oluştur sayfasında **Standart'ı** seçin ve ardından **Tamam'ı**seçin. **Create public IP address** 
   
1. **Ağ Güvenliği Grubu**altında, bir güvenlik duvarı türü olan yeni bir ağ güvenlik grubu (NSG) oluşturmak için **Gelişmiş'i** seçin. 
   1. **Yapılandır ağ güvenlik grubu** alanında **yeni oluştur'u**seçin. 
   1. *MyNetworkSecurityGroup*yazın ve **Tamam'ı**seçin. 
   
   >[!NOTE]
   >Varsayılan olarak, NSG'nin uzak masaüstü (RDP) bağlantı noktası olan 3389 bağlantı noktasını açmak için gelen bir kuralı olduğunu gözlemleyin.
   
1. Oluşturduğunuz yük dengeleyici arka uç havuzuna VM'yi ekleyin:
   
   1. **YÜK DENGELEME** > si altında**bu sanal makineyi mevcut bir yük dengeleme çözümünü arkasına yerleştirin?** **Yes** 
   1. **Yük dengeleme seçenekleri**için, aşağı bırakın ve Azure yük **dengeleyiciseçin.** 
   1. **Bir yük dengeleyiciseçin,** aşağı bırakın ve **MyLoadBalancer**seçin. 
   1. **Bir arka uç havuzu seçin,** yeni **oluştur'u**seçin, ardından *MyBackendPool*yazın ve **Oluştur'u**seçin. 
   
   ![Sanal ağ oluşturma](./media/tutorial-load-balancer-port-forwarding-portal/create-vm-networking.png)
   
1. **Yönetim** sekmesini seçin veya **Sonraki** > **Yönetim'i**seçin. **İzleme**altında, **Boot tanılamayı** **Kapalı**olarak ayarlayın.
   
1. **İncele ve oluştur**’u seçin.
   
1. Ayarları gözden geçirin ve doğrulama başarılı olduğunda **Oluştur'u**seçin. 

1. *MyVM2*adlı ikinci bir VM oluşturmak için adımları izleyin , tüm diğer ayarları MyVM1 ile aynı. 
   
   **Ağ Güvenliği Grubu**için Gelişmiş'i seçtikten sonra aşağı bırakın ve zaten oluşturduğunuz **MyNetworkSecurityGroup'u** seçin. **Advanced** 
   
   **Bir arka uç havuzu seçin**altında, **MyBackendPool'un** seçildiğinden emin olun. 

### <a name="create-an-nsg-rule-for-the-vms"></a>VM'ler için NSG kuralı oluşturma

VM'lerin gelen internet (HTTP) bağlantılarına izin vermesi için bir ağ güvenlik grubu (NSG) kuralı oluşturun.

>[!NOTE]
>Varsayılan olarak, NSG zaten port 3389, uzak masaüstü (RDP) bağlantı noktası açan bir kural vardır.

1. Soldaki menüden **Tüm kaynaklar**’ı seçin. Kaynak listesinden **MyResourceGroupLB** kaynak grubunda **MyNetworkSecurityGroup'u** seçin.
   
1. **Ayarlar** bölümünde **Gelen güvenlik kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. Gelen **güvenlik kuralı ekle** iletişim kutusunda aşağıdakileri yazın veya seçin:
   
   - **Kaynak**: **Servis Etiketini**Seçin.  
   - **Kaynak servis etiketi**: **Internet'i**seçin. 
   - **Hedef bağlantı noktası aralıkları**: Type *80*.
   - **Protokol**: **TCP'yi**seçin. 
   - **Eylem**: **İzin Ver'i**seçin.  
   - **Öncelik**: Tip *100*. 
   - **Adı**: *MyHTTPRule*yazın. 
   - **Açıklama**: Type *Allow HTTP*. 
   
1. **Ekle'yi**seçin. 
   
   ![NSG kuralı oluşturma](./media/tutorial-load-balancer-port-forwarding-portal/8-load-balancer-nsg-rules.png)
   
## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde, yük dengeleyici arka uç havuzunu inceler ve bir yük dengeleyici sağlık sondası ve trafik kurallarını yapılandırın.

### <a name="view-the-back-end-address-pool"></a>Arka uç adres havuzunu görüntüleme

VM'lere trafik dağıtmak için yük dengeleyicisi, yük bakiyesine bağlı sanal ağ arabirimlerinin (NIC) IP adreslerini içeren bir arka uç adres havuzu kullanır. 

Yük dengeleyici arka uç havuzunuzu oluşturdunuz ve VM'leri oluşturduğunuzda ona VM eklediniz. Ayrıca arka uç havuzları oluşturabilir ve yük bakiyesi Arka **uç havuzları** sayfasından VM'ler ekleyebilir veya kaldırabilirsiniz. 

1. Sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **MyLoadBalancer'ı** seçin.
   
1. **Ayarlar**’ın altında **Arka Uç Havuzları**’nı seçin.
   
1. **Backend havuzları** sayfasında **MyBackendPool'u** genişletin ve hem **VM1** hem de **VM2'nin** listelendirilip listelendirilediğinden emin olun.

1. **MyBackendPool'u**seçin. 
   
   **MyBackendPool** sayfasında, **VIRTUAL MACHINE** ve **IP ADRESİ**altında, havuza kullanılabilir VM'leri kaldırabilir veya ekleyebilirsiniz.

**Arka uç havuzları** sayfasında **Ekle'yi** seçerek yeni arka uç havuzları oluşturabilirsiniz.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicisinin VM durumunu izlemesine izin vermek için bir sistem durumu sondası kullanırsınız. Durum yoklaması, durum denetimlerine verdikleri yanıtlara göre VM’leri dinamik olarak yük dengeleyici rotasyonuna ekler ve kaldırır. 

1. Sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **MyLoadBalancer'ı** seçin.
   
1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
   
1. Sistem **durumu sondası ekle** sayfasında aşağıdaki değerleri yazın veya seçin:
   
   - **Adı**: *MyHealthProbe*yazın.
   - **Protokol**: Aşağı in ve **HTTP**seçin. 
   - **Bağlantı Noktası**: Tip *80*. 
   - **Yol**: */* Varsayılan URI için kabul edin. Bu değeri başka bir URI ile değiştirebilirsiniz. 
   - **Aralık**: Tip *15*. Aralık, sonda denemeleri arasındaki saniye sayısıdır.
   - **Sağlıksız eşik**: Tip *2*. Bu değer, VM sağlıksız olarak kabul edilmeden önce oluşan ardışık sonda hatalarının sayısıdır.
   
1. **Tamam'ı**seçin.
   
   ![Sonda ekleme](./media/tutorial-load-balancer-port-forwarding-portal/4-load-balancer-probes.png)

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, trafiğin sanal makinelere nasıl dağıtıldığını belirler. Kural, gelen trafik için ön uç IP yapılandırmasını, trafiği almak için arka uç IP havuzunu ve gerekli kaynak ve hedef bağlantı noktalarını tanımlar. 

**MyLoadBalancerRule** adlı yük dengeleyici kuralı ön uç **LoadBalancerFrontEnd**port 80 dinler. Kural arka uç adres havuzu **MyBackendPool**ağ trafiği gönderir , ayrıca bağlantı noktası 80. 

1. Sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **MyLoadBalancer'ı** seçin.
   
1. **Ayarlar** bölümünde **Yük dengeleme kuralları**’nı ve sonra **Ekle**’yi seçin.
   
1. Yük **dengeleme kuralı** ekle sayfasında aşağıdaki değerleri yazın veya seçin:
   
   - **Adı**: *MyLoadBalancerRule*yazın.
   - **Protokol**: **TCP'yi**seçin.
   - **Bağlantı Noktası**: Tip *80*.
   - **Arka uç bağlantı noktası**: Tip *80*.
   - **Arka uç havuzu**: **MyBackendPool'u**seçin.
   - **Sağlık sondası**: **MyHealthProbe'u**seçin. 
   
1. **Tamam'ı**seçin.
   
   ![Yük dengeleyici kuralı ekleme](./media/tutorial-load-balancer-port-forwarding-portal/5-load-balancing-rules.png)

## <a name="create-an-inbound-nat-port-forwarding-rule"></a>Gelen NAT bağlantı noktası iletme kuralı oluşturma

Trafiği ön uç IP adresinin belirli bir bağlantı noktasından belirli bir arka uç VM'nin belirli bir bağlantı noktasına iletmek için bir yük dengeleyici gelen ağ adresi çevirisi (NAT) kuralı oluşturun.

1. Sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **MyLoadBalancer'ı** seçin.
   
1. **Ayarlar**altında **Gelen NAT kurallarını**seçin ve sonra **Ekle'yi**seçin. 
   
1. Gelen **NAT kuralı** ekle sayfasında aşağıdaki değerleri yazın veya seçin:
   
   - **Adı**: *MynATRuleVM1*yazın.
   - **Bağlantı Noktası**: Tip *4221*.
   - **Hedef sanal makine**: Açılan yerden **MyVM1'i** seçin.
   - **Ağ IP Yapılandırması**: Açılan dan **ipconfig1'i** seçin.
   - **Bağlantı noktası eşleme**: **Özel'i**seçin.
   - **Hedef bağlantı noktası**: Tip *3389*.
   
1. **Tamam'ı**seçin.
   
1. **Port:** *4222* ve **Hedef sanal makine**kullanarak *MyNATRuleVM2*adlı gelen NAT kuralı eklemek için adımları tekrarlayın : **MyVM2**.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

Bu bölümde, arka uç sunucularına Internet Bilgi Hizmetleri (IIS) yükler ve makine adını göstermek için varsayılan web sayfasını özelleştirin. Ardından, yük bakiyesini test etmek için yük bakiyesinin genel IP adresini kullanırsınız. 

Her arka uç VM varsayılan IIS web sayfasının farklı bir sürümünü hizmet vermektedir, böylece yük dengeleyicisinin iki VM arasında istekleri dağıttığını görebilirsiniz.

### <a name="connect-to-the-vms-with-rdp"></a>RDP ile VM'lere bağlanma

Uzak Masaüstü (RDP) ile her VM'ye bağlanın. 

1. Portalda, sol menüdeki **Tüm kaynakları** seçin. Kaynak listesinden **MyResourceGroupLB** kaynak grubundaki her VM'yi seçin.
   
1. Genel **Bakış** sayfasında **Bağlan'ı**seçin ve ardından **RDP dosyasını indir'i**seçin. 
   
1. İndirdiğiniz RDP dosyasını açın ve **Bağlan'ı**seçin.
   
1. Windows Security **ekranında, daha fazla seçenek** seçin ve sonra farklı bir hesap **kullanın.** 
   
   Kullanıcı adı *azureuser* ve şifre *Azure1234567*girin ve **Tamam'ı**seçin.
   
1. Herhangi bir sertifika istemine **Evet** yanıtı verin. 
   
   VM masaüstü yeni bir pencerede açılır. 

### <a name="install-iis-and-replace-the-default-iis-web-page"></a>IIS'yi yükleyin ve varsayılan IIS web sayfasını değiştirin 

IIS'yi yüklemek ve varsayılan IIS web sayfasını VM adını görüntüleyen bir sayfayla değiştirmek için PowerShell'i kullanın.

1. MyVM1'de ve MyVM2'de **Başlat** menüsünden **Windows PowerShell'i** başlatın. 

2. IIS'yi yüklemek ve varsayılan IIS web sayfasını değiştirmek için aşağıdaki komutları çalıştırın:
   
   ```powershell-interactive
    # Install IIS
      Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # Remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    #Add custom htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
    
   ```
   
1. **Bağlantıyı Kesme'yi**seçerek RDP bağlantılarını MyVM1 ve MyVM2 ile kapatın. VM'leri kapatma.

### <a name="test-load-balancing"></a>Test yük dengeleme

1. Portalda, **MyLoadBalancer**için **Genel Bakış** sayfasında, Genel **IP adresi**altında genel IP adresini kopyalayın. Adresin üzerine titreyin ve kopyalamak için **Kopyala** simgesini seçin. Bu örnekte, **40.67.218.235'** tir. 
   
1. Yük bakiyesi ortak IP adresini *(40.67.218.235)* internet tarayıcınızın adres çubuğuna yapıştırın veya yazın. 
   
   Özelleştirilmiş IIS web sunucusu varsayılan sayfası tarayıcıda görünür. Mesaj myVM1 veya Hello World **myVM2**ya Hello **World**okur.
   
   ![Yeni IIS varsayılan sayfası](./media/tutorial-load-balancer-port-forwarding-portal/9-load-balancer-test.png) 
   
1. Yük dengeleyicisinin VM'ler arasında trafik dağıttığını görmek için tarayıcıyı yenileyin. Bazen **MyVM1** sayfası görünür, bazen de yük bakiyesi istekleri her arka uç VM'ye dağıttığı için **MyVM2** sayfası görünür.
   
   >[!NOTE]
   >Denemeler arasında tarayıcı önbelleğinizi temizlemeniz veya yeni bir tarayıcı penceresi açmanız gerekebilir.

## <a name="test-port-forwarding"></a>Bağlantı noktası iletmeyi test etme

Bağlantı noktası yönlendirmeile, yük bakiyesinin IP adresini ve NAT kuralında tanımlanan ön uç bağlantı noktası değerini kullanarak masaüstünü arka uç VM'ye uzak tutabilirsiniz. 

1. Portalda, **MyLoadBalancer**için **Genel Bakış** sayfasında, genel IP adresini kopyalayın. Adresin üzerine titreyin ve kopyalamak için **Kopyala** simgesini seçin. Bu örnekte, **40.67.218.235'** tir. 
   
1. Bir komut istemi açın ve yük bakiyesi ortak IP adresini ve VM'nin NAT kuralında tanımladığınız ön uç bağlantı noktasını kullanarak MyVM2 ile uzak bir masaüstü oturumu oluşturmak için aşağıdaki komutu kullanın. 
   
   ```
   mstsc /v:40.67.218.235:4222
   ```
  
1. İndirilen RDP dosyasını açın ve **Bağlan'ı**seçin.
   
1. Windows Security **ekranında, daha fazla seçenek** seçin ve sonra farklı bir hesap **kullanın.** 
   
   Kullanıcı adı *azureuser* ve şifre *Azure1234567*girin ve **Tamam'ı**seçin.
   
1. Herhangi bir sertifika istemine **Evet** yanıtı verin. 
   
   MyVM2 masaüstü yeni bir pencerede açılır. 

Gelen NAT kuralı **MyNATRuleVM2** yük dengeleyicisinin ön uç bağlantı noktası 4222'den MyVM2'nin 3389 (RDP bağlantı noktası) bağlantı noktasına yönlendirdiği için RDP bağlantısı başarılı dır.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız olmadığında yük bakiyesini ve ilgili tüm kaynakları silmek için **MyResourceGroupLB** kaynak grubunu açın ve **kaynak grubunu sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, standart bir genel yük dengeleyicisi oluşturdunuz. Ağ kaynaklarını, arka uç sunucularını, sistem durumu sondasını ve yük dengeleyicisi kurallarını oluşturdunuz ve yapılandırıldınız. Arka uç VM'lerine IIS yükledin ve yük dengeleyicisini test etmek için yük bakiyesinin genel IP adresini kullandınız. Yük dengeleyicisindeki belirli bir bağlantı noktasından arka uç VM'deki bir bağlantı noktasına iletme bağlantı noktasını ayarlar ve test edeyim. 

Azure Yük Dengeleyicisi hakkında daha fazla bilgi edinmek için daha fazla yük bakiyesi öğreticisine devam edin.

> [!div class="nextstepaction"]
> [Azure Load Balancer öğreticileri](tutorial-load-balancer-standard-public-zone-redundant-portal.md)
