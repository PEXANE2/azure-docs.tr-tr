---
title: Hızlı başlangıç-Azure portal kullanarak özel bir bağlantı hizmeti oluşturma
titlesuffix: Azure Private Link
description: Bu hızlı başlangıçta Azure portal kullanarak özel bir bağlantı hizmeti oluşturmayı öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 02/03/2020
ms.author: allensu
ms.openlocfilehash: 4dd27d3fb4321c2eecf222136376bfd4b49790bb
ms.sourcegitcommit: 21e33a0f3fda25c91e7670666c601ae3d422fb9c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/05/2020
ms.locfileid: "77027779"
---
# <a name="quickstart-create-a-private-link-service-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak özel bir bağlantı hizmeti oluşturma

Azure özel bağlantı hizmeti, Azure özel bağlantısı tarafından desteklenen kendi hizmetinize yapılan başvurudur. Azure Load Balancer arkasında çalışan hizmet veya kaynak, özel bağlantı erişimi için etkinleştirilebilir. Hizmetinizin tüketicileri, hizmete kendi sanal ağlarından özel olarak erişebilir. Bu hızlı başlangıçta, Azure portal kullanarak özel bir bağlantı hizmeti oluşturmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.


## <a name="sign-in-to-azure"></a>Azure'da oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-internal-load-balancer"></a>Iç yük dengeleyici oluşturma

İlk olarak, bir sanal ağ oluşturun ve Azure özel bağlantı hizmeti ile kullanmak üzere bir iç yük dengeleyici oluşturun.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, özel bağlantı hizmetinize erişmek için kullanılan yük dengeleyiciyi barındırmak için bir sanal ağ ve alt ağ oluşturacaksınız.


1. Portalda sol tarafta seçin **kaynak Oluştur** > **ağ** > **sanal ağ**.
   
1. İçinde **sanal ağ oluştur** bölmesinde yazın veya bu değerleri seçin:
   
   - **Adı**: türü **MyVNet**.
   - **ResourceGroup**: seçin **Yeni Oluştur**, enter **MyResourceGroupLB**seçip **Tamam**. 
   - **Alt ağ** > **adı**: türü **MyBackendSubnet**.
   
1. **Oluştur**'u seçin.

   ![Sanal ağ oluşturun](../load-balancer/media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyici oluşturma

Portalı kullanarak standart bir iç yük dengeleyici oluşturun. Oluşturduğunuz IP adresi ve adını, load balancer'ın ön ucu olarak otomatik olarak yapılandırılır.

1. Portalın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici** seçeneğini belirleyin.
   
2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde aşağıdaki bilgileri girin veya seçin, kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Açılır kutuda *Myresourcegrouplb* ' ı seçin.|
    | Ad                   | *myLoadBalancer*                                   |
    | Bölge         | **Doğu ABD 2**’yi seçin.                                        |
    | Tür          | **Dahili**' ı seçin.                                        |
    | SKU           | **Standart**' ı seçin.                          |
    | Sanal ağ           | *Myvnet*' i seçin.                          |    
    | IP adresi ataması              | **Statik**öğesini seçin.   |
    | Özel IP adresi|Sanal ağınızın ve alt ağınızın adres alanında olan bir adres yazın, örneğin *10.3.0.7*.  |

3. **Gözden geçir + oluştur** sekmesinde **Oluştur**' a tıklayın. 
   

### <a name="create-standard-load-balancer-resources"></a>Standart yük dengeleyici kaynakları oluşturma

Bu bölümde, bir arka uç adres havuzu ve bir sistem durumu araştırması için yük dengeleyici ayarlarını yapılandırır ve yük dengeleyici kurallarını belirtirsiniz.

#### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Kaynaklarınıza trafik dağıtmak için bir arka uç adres havuzu, Load Balancer bağlı sanal (NIC) IP adreslerini içerir. Yük Dengeleme trafiğine yönelik kaynakları dahil etmek için *Mybackendpool* arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
2. **Ayarlar**altında **arka uç havuzları**' nı seçin ve ardından **Ekle**' yi seçin.
3. **Arka uç Havuzu Ekle** sayfasında, ad için, arka uç havuzunuzun adı olarak *mybackendpool*yazın ve ardından **Ekle**' yi seçin.



#### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicinin kaynak durumunu izlemesine izin vermek için, bir sistem durumu araştırması kullanırsınız. Sistem durumu araştırması, yük dengeleyici rotasyonlarından, sistem durumu denetimlerine olan yanıtına göre kaynak dinamik olarak ekler veya kaldırır. 

**Kaynakların sistem durumunu izlemek üzere bir sistem durumu araştırması oluşturmak için:**

1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.
   
1. Üzerinde **bir durum araştırması Ekle** sayfasında yazın veya aşağıdaki değerleri seçin:
   
   - **Adı**: türü **MyHealthProbe**.
   - **Protokol**: açılır ve **TCP**' yi seçin. 
   - **Bağlantı noktası**: türü **80**. 
   - **Aralığı**: türü **15**. Araştırma denemeleri arasındaki saniye sayısını aralığıdır.
   - **Sağlıksız durum eşiği**: türü **2**. Bu değer bir VM kötü olarak kabul edilmeden önce gerçekleşmesi ardışık araştırma hatası sayısıdır.
   
1. **Tamam**’ı seçin.

#### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı trafiğin kaynaklara nasıl dağıtıldığını tanımlar. Ön uç IP yapılandırmasını gelen trafiğe, trafik ve gerekli kaynak ve hedef bağlantı noktalarını almak için arka uç IP havuzu için kuralı tanımlar. 

Adlı yük dengeleyici kuralı **MyLoadBalancerRule** ön 80 numaralı bağlantı noktasını dinler **LoadBalancerFrontEnd**. Kural ağ trafiği arka uç adres havuzuna gönderen **MyBackendPool**, bağlantı noktası 80'de. 

**Yük Dengeleyici kuralı oluşturmak için:**

1. Seçin **tüm kaynakları** sol menüsünü ve ardından **MyLoadBalancer** kaynak listesinde.
   
1. **Ayarlar**bölümünde **Yük Dengeleme kuralları**' nı ve ardından **Ekle**' yi seçin.
   
1. **Yük Dengeleme kuralı ekle** sayfasında, zaten mevcut değilse, aşağıdaki değerleri yazın veya seçin:
   
   - **Adı**: türü **MyLoadBalancerRule**.
   - **Ön uç IP adresi:** türü **LoadBalancerFrontEnd** yoksa.
   - **Protokol**: seçin **TCP**.
   - **Bağlantı noktası**: türü **80**.
   - **Arka uç bağlantı noktası**: türü **80**.
   - **Arka uç havuzu**: seçin **MyBackendPool**.
   - **Durum araştırması**: seçin **MyHealthProbe**. 
   
1. **Tamam**’ı seçin.
   
## <a name="create-a-private-link-service"></a>Özel bağlantı hizmeti oluşturma

Bu bölümde, standart yük dengeleyici arkasında bir özel bağlantı hizmeti oluşturacaksınız.

1. Ekranın sol üst kısmında Azure portal **kaynak oluştur** > **ağ** > **özel bağlantı merkezi (Önizleme)** seçeneğini belirleyin. Ayrıca, Portal arama aracılığıyla özel bağlantı araması yapabilirsiniz.

2. **Özel bağlantı merkezi 'Ne genel bakış**' da, **"başkalarının bağlanabilmesi Için kendi hizmetinizi kullanıma sunma"** seçeneğinde Başlat ' ı seçin.

3. **Özel bağlantı hizmeti oluşturma-temel bilgiler**bölümünde, bu bilgileri girin veya seçin:

    | Ayar           | Değer                                                                        |
    |-------------------|------------------------------------------------------------------------------|
    | **Proje ayrıntıları:**  |                                                                              |
    | Abonelik      | Aboneliğinizi seçme                                                     |
    | Kaynak Grubu    | *Myresourcegrouplb* seçin                                                     |
    | **ÖRNEK AYRıNTıLARı:** |                                                                              |
    | Ad              | *Myprivatelinkservice* girin |
    | Bölge            | *Doğu ABD 2* seçin                                                         |

4. **İleri ' yi seçin: giden ayarları**.

5. **Özel bağlantı hizmeti oluşturma-giden ayarları**' nda, bu bilgileri girin veya seçin:


    | Ayar                           | Değer                                                                           |
    |-----------------------------------|---------------------------------------------------------------------------------|
    | Load Balancer                     | *Myloadbalancer* seçin                                                           |
    | Load Balancer ön uç IP adresi | MyLoadBalancer ön uç IP adresini seçin                                |
    | Kaynak NAT sanal ağı        | *Myvnet* seçin                                                                   |
    | Kaynak NAT alt ağı                 | *Mybackendsubnet* seçin                                                          |
    | TCP proxy v2 'yi etkinleştir               | Uygulamanızın TCP proxy v2 üst bilgisini beklediği takdirde Evet/Hayır ' ı seçin |
    | Özel IP adresi ayarları       | Her NAT IP 'si için ayırma yöntemini ve IP adresini yapılandırın                  |

6. **İleri ' yi seçin: erişim güvenliği**.

7. **Özel bağlantı hizmeti oluşturma-erişim güvenliği**' nde bu bilgileri girin veya seçin:

    | Ayar                                     | Değer                                                             |
    |---------------------------------------------|-------------------------------------------------------------------|
    | Görünürlük                                  | *Yalnızca rol tabanlı erişim denetimi* seçin         |
  
8. **İleri: Etiketler** **' i seçin, sonra da** sayfanın üst kısmındaki **Gözden** geçir + Oluştur sekmesini seçin.

9. Bilgilerinizi gözden geçirin ve **Oluştur**' u seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme 
Özel bağlantı hizmetini kullanarak işiniz bittiğinde, bu hızlı başlangıçta kullanılan kaynakları temizlemek için kaynak grubunu silin:

1. Portalın üst kısmındaki **arama** kutusuna *myresourcegrouplb* yazın ve arama sonuçlarından *myresourcegrouplb* ' ı seçin. 
2. **Kaynak grubunu sil**'i seçin. 
3. **Kaynak grubu adını yazın** ve **Sil**' i seçmek için myresourcegroup girin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir iç Azure yük dengeleyici ve bir özel bağlantı hizmeti oluşturdunuz. Özel uç noktalar oluşturma hakkında daha fazla bilgi için, bkz. [Azure Portal kullanarak özel uç noktalar oluşturma](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal).
