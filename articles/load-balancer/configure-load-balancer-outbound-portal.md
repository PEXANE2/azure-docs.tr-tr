---
title: Azure portalını kullanarak yük dengeleme ve giden kuralları yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Azure portalını kullanarak Standart Yük Dengeleyici'nde yük dengeleme ve giden kurallarınasıl yapılandırılanın gösterilmektedir.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: b75f49155991bfc71f788ad88f166c0bec281841
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77590019"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-the-azure-portal"></a>Azure portalını kullanarak Standart Yük Dengeleyici'nde yük dengeleme ve giden kuralları yapılandırma

Bu makalede, Azure portalını kullanarak Standart Yük Dengeleyici'nde giden kuralları nasıl yapılandırabileceğiniz gösterilmektedir.  

Yük dengeleyici kaynağı iki ön uç ve ilişkili kuralları içerir. Gelen trafik için bir ön uç ve giden trafik için başka bir ön uç var.  

Her ön uç genel bir IP adresine başvurur. Bu senaryoda, gelen trafiğin genel IP adresi giden trafiğin adresinden farklıdır.   Yük dengeleme kuralı yalnızca gelen yük dengelemesağlar. Giden kural VM için giden ağ adresi çevirisini (NAT) denetler.  

Senaryoda iki arka uç havuzu kullanır: biri gelen trafik, diğeri giden trafik için. Bu havuzlar yeteneği gösterir ve senaryo için esneklik sağlar.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

Azure portalında oturum [https://portal.azure.com](https://portal.azure.com)aç.

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Bu bölümde, sanal makineleri dengeleyecek bir yük dengeleyicisi oluşturursunuz. Bir ortak yük dengeleyicisi veya dahili yük dengeleyicioluşturabilirsiniz. Bir genel yük dengeleyicisi oluşturduğunuzda, yük dengeleyicisinin ön ucu olarak yapılandırılan yeni bir Genel IP adresi oluşturursunuz. Ön uç varsayılan olarak **LoadBalancerFrontEnd** olarak adlandırılacaktır.

1. Ekranın sol üst tarafında, **kaynak** > **Oluştur** > **Yük Dengeleyicisini**seçin.
2. **Yük bakiyesi oluştur** sayfasının **Temeller** sekmesine aşağıdaki bilgileri girin veya seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni Oluştur'u** seçin ve metin kutusuna **myResourceGroupSLB** yazın.|
    | Adı                   | **myLoadBalancer**                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Genel'i**seçin.                                        |
    | SKU           | **Standart** veya **Temel'i**seçin. Microsoft, üretim iş yükleri için Standart önerir. |
    | Genel IP adresi | **Yeni oluştur**’u seçin. Kullanmak istediğiniz mevcut bir Genel IP'niz varsa, **varolan kullan'ı**seçin.  Mevcut genel IP **Standart** SKU olmalıdır.  Temel genel IP'ler **Standart** SKU yük dengeleyicisi ile uyumlu değildir.  |
    | Genel IP adresi adı              | Metin kutusuna **myPublicIP** yazın.|
    | Kullanılabilirlik alanı | Esnek bir Yük Dengeleyicisi oluşturmak için **Bölge yedekli'yi** seçin. Bir zonal Yük Dengeleyicisi oluşturmak için, 1, 2 veya 3'ten belirli bir bölge seçin |

3. Yapılandırmanın geri kalanı için varsayılanları kabul edin.
4. **Gözden Geçir ' i** seçin + oluştur

    > [!IMPORTANT]
    > Bu hızlı başlatmanın geri kalanı, yukarıdaki SKU seçim işlemi sırasında **Standart** SKU'nun seçildiğini varsayar.

5. Gözden **Geçir + oluştur** sekmesinde **Oluştur'u**seçin.   

    ![Standart Yük Dengeleyici oluşturma](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracak ve bir yük dengeleyici kuralı belirteceksiniz.

### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Arka uç adres havuzu, arka uç havuzundaki sanal NIC'lerin IP adreslerini içerir. Yük dengeleme internet trafiği için sanal makineleri içerecek şekilde arka uç adresi **myBackendPool** oluşturun.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.
2. **Ayarlar'ın**altında, **Arka uç havuzlarını**seçin, ardından **Ekle'yi**seçin.
3. Ad için **bir arka uç havuzu** ekle sayfasında, **myBackendPool**yazın , arka uç havuzuiçin ad olarak, ve sonra **Ekle'yi**seçin.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Uygulamanızın durumunu izlemek için bir sistem durumu sondası kullanılır. Sistem durumu sondası, sağlık kontrollerine verdikleri yanıta bağlı olarak VM'leri yük dengeleyicisinden ekler veya kaldırır. Sanal makinelerin durumunu izlemek için **myHealthProbe** durum araştırması oluşturun.

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.
2. **Ayarlar**altında, **Sistem Durumu sondalarını**seçin ve ardından **Ekle'yi**seçin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **myHealthProbe**girin. |
    | Protokol | **HTTP'yi**seçin. |
    | Bağlantı noktası | **80**girin.|
    | Interval | Sonda denemeleri arasında saniye cinsinden **Aralık** sayısı için **15** girin. |
    | Sağlıksız durum eşiği | VM sağlıksız olarak kabul edilmeden önce oluşması gereken **Sağlıksız eşik** veya ardışık sonda hataları sayısı için **2'yi** seçin.|
    | | |
4. **Tamam'ı**seçin.

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma
Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. 

Şunları tanımlarsınız:
 - Gelen trafik için frontend IP yapılandırması.
 - Trafiği almak için arka uç IP havuzu.
 - Gerekli kaynak ve hedef bağlantı noktası. 

Aşağıdaki bölümde, bir oluşturmak:
 - Yük dengeleyici kural **myHTTPRule** bağlantı noktası 80 dinlemek için.
 - Frontend **LoadBalancerFrontEnd**.
 - Arka uç adresi havuzu **myBackEndPool** da port 80 kullanarak. 

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.
2. **Ayarlar** **altında, Yük dengeleme kurallarını**seçin ve sonra **Ekle'yi**seçin.
3. Yük dengeleme kuralını yapılandırmak için bu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **MyHTTPRule**girin. |
    | Protokol | **TCP'yi**seçin. |
    | Bağlantı noktası | **80**girin.|
    | Arka uç bağlantı noktası | **80**girin. |
    | Arka uç havuzu | **myBackendPool'u**seçin.|
    | Durum yoklaması | **myHealthProbe'u**seçin. |
    | Örtülü giden kurallar oluşturma | **Hayır**'ı seçin. Daha sonraki bir bölümde özel bir genel IP kullanarak giden kurallar oluştururuz. |
4. Varsayılanların geri kalanını bırakın ve sonra **Tamam'ı**seçin.

## <a name="create-outbound-rule-configuration"></a>Giden kural yapılandırması oluşturma
Yük dengeleyici giden kuralları arka uç havuzunda VM'ler için giden SNAT yapılandırır. 

### <a name="create-an-outbound-public-ip-address-and-frontend"></a>Giden genel IP adresi ve ön uç oluşturma

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.

2. **Ayarlar**altında **Frontend IP yapılandırması'nı**seçin ve sonra **Ekle'yi**seçin.

3. Giden ler için ön uç IP yapılandırmasını yapılandırmak için bu değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **LoadBalancerFrontEndOutbound**girin. |
    | IP sürümü | **IPv4** seçin. |
    | IP türü | **IP adresini**seçin.|
    | Genel IP adresi | **Yeni oluştur**’u seçin. Genel **IP adresi**ekle,myPublicIPOutbound girin. **myPublicIPOutbound**  **Tamam'ı**seçin. |

4. **Ekle'yi**seçin.

### <a name="create-an-outbound-backend-pool"></a>Giden arka uç havuzu oluşturma

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.

2. **Ayarlar'ın**altında, **Arka uç havuzlarını**seçin, ardından **Ekle'yi**seçin.

3. Ad için **bir arka uç havuzu** ekle sayfasında, **myBackendPoolOutbound**yazın , arka uç havuzuiçin ad olarak, ve sonra **Ekle'yi**seçin.

### <a name="create-outbound-rule"></a>Giden kuralı oluşturma

1. Sol menüdeki **Tüm hizmetleri** seçin, **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.

2. **Ayarlar**altında, **Giden kuralları**seçin, sonra **Ekle'yi**seçin.

3. Giden kuralları yapılandırmak için bu değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **myOutboundRule**girin. |
    | Ön uç IP adresi | **LoadBalancerFrontEndOutbound'u**seçin. |
    | Boşta zaman/zaman ası (dakika) | Kaydırıcıyı **15 dakikaya taşıyın.|
    | TCP Sıfırlama | **Etkin**'i seçin.|
    | Arka uç havuzu | **myBackendPoolOutbound'u** seçin |
    | Bağlantı noktası tahsisi -> Bağlantı noktası tahsisi | **Giden bağlantı noktası sayısını El Ile seçin** |
    | Giden bağlantı noktaları -> Tarafından seçin | **Örnek başına Bağlantı Noktalarını** Seçin |
    | Giden bağlantı noktaları -> Bağlantı Noktaları | **10.000**girin. |

4. **Ekle'yi**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Yük bakiyesini içeren kaynak grubu **myResourceGroupSLB'yi** seçin ve sonra **Sil'i**seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede:
 - Standart bir yük dengeleyicisi oluşturdunuz.
 - Hem gelen hem de giden yük dengeleyici trafik kurallarını yapılandırılır.
 - Arka uç havuzundaki VM'ler için bir sistem durumu sondası yapılandırıldı. 

Daha fazla bilgi edinmek [için Azure Yük Dengeleyici'nin öğreticilerine](tutorial-load-balancer-standard-public-zone-redundant-portal.md)devam edin.
