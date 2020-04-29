---
title: Azure portal kullanarak yük dengelemeyi ve giden kuralları yapılandırma
titleSuffix: Azure Load Balancer
description: Bu makalede, Standart Load Balancer Azure portal kullanarak yük dengelemenin ve giden kuralların nasıl yapılandırılacağı gösterilmektedir.
services: load-balancer
author: asudbring
ms.service: load-balancer
ms.topic: article
ms.date: 09/24/2019
ms.author: allensu
ms.openlocfilehash: b75f49155991bfc71f788ad88f166c0bec281841
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77590019"
---
# <a name="configure-load-balancing-and-outbound-rules-in-standard-load-balancer-by-using-the-azure-portal"></a>Standart Load Balancer Azure portal kullanarak yük dengelemeyi ve giden kuralları yapılandırma

Bu makalede, Azure portal kullanarak Standart Load Balancer giden kuralların nasıl yapılandırılacağı gösterilmektedir.  

Yük dengeleyici kaynağı iki ön uç ve bunlarla ilişkili kuralları içerir. Gelen trafik için bir ön uç ve giden trafik için başka bir ön uç vardır.  

Her ön uç, bir genel IP adresine başvurur. Bu senaryoda, gelen trafik için genel IP adresi giden trafik adresinden farklıdır.   Yük Dengeleme kuralı yalnızca gelen yük dengelemeyi sağlar. Giden kuralı, VM için giden ağ adresi çevirisini (NAT) denetler.  

Senaryo iki arka uç havuzu kullanır: biri gelen trafik ve diğeri giden trafik için. Bu havuzlar, özelliği gösterir ve senaryo için esneklik sağlar.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

## <a name="create-a-load-balancer"></a>Yük dengeleyici oluşturma

Bu bölümde, sanal makinelerin yükünü dengelemeye yönelik bir yük dengeleyici oluşturacaksınız. Ortak yük dengeleyici veya iç yük dengeleyici oluşturabilirsiniz. Ortak yük dengeleyici oluşturduğunuzda, yük dengeleyici için ön uç olarak yapılandırılmış yeni bir genel IP adresi oluşturun. Ön uç, varsayılan olarak **Loadbalancerön uç** olarak adlandırılır.

1. Ekranın sol üst kısmında, **kaynak** > oluştur**ağ** > **Load Balancer**' ı seçin.
2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni oluştur** ' u seçin ve metin kutusuna **Myresourcegroupslb** yazın.|
    | Adı                   | **myLoadBalancer**                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Ortak**seçeneğini belirleyin.                                        |
    | SKU           | **Standart** veya **temel**seçeneğini belirleyin. Microsoft, üretim iş yükleri için standart önerir. |
    | Genel IP adresi | **Yeni oluştur**’u seçin. Kullanmak istediğiniz mevcut bir genel IP varsa, **Varolanı kullan**' ı seçin.  Mevcut genel IP **Standart** SKU olmalıdır.  Temel genel IP 'Ler **Standart** SKU yük dengeleyici ile uyumlu değildir.  |
    | Genel IP adresi adı              | Metin kutusuna **Mypublicıp** yazın.|
    | Kullanılabilirlik alanı | Esnek Load Balancer oluşturmak için **bölge yedekli** ' ı seçin. Bir bölgesel Load Balancer oluşturmak için, 1, 2 veya 3 ' ten belirli bir bölge seçin |

3. Yapılandırmanın geri kalanı için varsayılanları kabul edin.
4. **Gözden geçir + oluştur** ' u seçin

    > [!IMPORTANT]
    > Bu hızlı başlangıçtaki geri kalanı, yukarıdaki SKU seçim sürecinde **Standart** SKU 'nun seçili olduğunu varsayar.

5. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.   

    ![Standart Yük Dengeleyici oluşturma](./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png)

## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracak ve bir yük dengeleyici kuralı belirteceksiniz.

### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Arka uç adres havuzu, arka uç havuzundaki sanal NIC 'lerin IP adreslerini içerir. Yük Dengeleme internet trafiği için sanal makineleri dahil etmek üzere **Mybackendpool** arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
2. **Ayarlar**altında **arka uç havuzları**' nı seçin ve ardından **Ekle**' yi seçin.
3. **Arka uç Havuzu Ekle** sayfasında, ad için, arka uç havuzunuzun adı olarak **mybackendpool**yazın ve ardından **Ekle**' yi seçin.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Uygulamanızın durumunu izlemek için bir sistem durumu araştırması kullanılır. Sistem durumu araştırması, sistem durumu denetimlerine yönelik yanıtlarını temel alarak yük dengeleyiciden VM 'Ler ekler veya kaldırır. Sanal makinelerin durumunu izlemek için **myHealthProbe** durum araştırması oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
2. **Ayarlar**altında **sistem durumu araştırmaları**' nı ve ardından **Ekle**' yi seçin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **Myhealtharaştırması**girin. |
    | Protokol | **Http**'yi seçin. |
    | Bağlantı noktası | **80**girin.|
    | Interval | Yoklama denemeleri arasındaki saniye cinsinden **Aralık** sayısı için **15** girin. |
    | Sağlıksız durum eşiği | Bir VM sağlıksız kabul edilmeden önce gerçekleşmesi gereken **sağlıksız eşik** veya arka arkaya araştırma hatası sayısı için **2** ' yi seçin.|
    | | |
4. **Tamam**’ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma
Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. 

Şunu tanımlarsınız:
 - Gelen trafik için ön uç IP yapılandırması.
 - Trafiği almak için arka uç IP Havuzu.
 - Gerekli kaynak ve hedef bağlantı noktası. 

Aşağıdaki bölümde, şunu oluşturursunuz:
 - 80 numaralı bağlantı noktasını dinlemek için **Myhttprule** yük dengeleyici kuralı.
 - Ön uç **Loadbalancerön**ucu.
 - Arka uç adres havuzu **Mybackendpool** 80 numaralı bağlantı noktasını de kullanıyor. 

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
2. **Ayarlar**bölümünde **Yük Dengeleme kuralları**' nı ve ardından **Ekle**' yi seçin.
3. Yük Dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **Myhttprule**girin. |
    | Protokol | **TCP**' yi seçin. |
    | Bağlantı noktası | **80**girin.|
    | Arka uç bağlantı noktası | **80**girin. |
    | Arka uç havuzu | **Mybackendpool**öğesini seçin.|
    | Durum yoklaması | **Myhealtharaştırması**' ni seçin. |
    | Örtük giden kuralları oluşturma | **Hayır**'ı seçin. Giden kuralları, ayrılmış bir genel IP kullanarak sonraki bir bölümde oluşturacağız. |
4. Kalan varsayılan değerleri bırakın ve **Tamam**' ı seçin.

## <a name="create-outbound-rule-configuration"></a>Giden kuralı yapılandırması oluştur
Yük dengeleyici giden kuralları arka uç havuzundaki VM 'Ler için giden SNAT 'yi yapılandırır. 

### <a name="create-an-outbound-public-ip-address-and-frontend"></a>Giden genel IP adresi ve ön uç oluşturma

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **ön uç IP yapılandırması**' nı ve ardından **Ekle**' yi seçin.

3. Giden için ön uç IP yapılandırmasını yapılandırmak üzere bu değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **LoadBalancerFrontEndOutbound**girin. |
    | IP sürümü | **IPv4** seçin. |
    | IP türü | **IP adresi**seçin.|
    | Genel IP adresi | **Yeni oluştur**’u seçin. **Genel IP adresi ekle**' de, **Mypublicıpoıb utbağlanacak**' yı girin.  **Tamam**’ı seçin. |

4. **Add (Ekle)** seçeneğini belirleyin.

### <a name="create-an-outbound-backend-pool"></a>Giden arka uç havuzu oluşturma

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **arka uç havuzları**' nı seçin ve ardından **Ekle**' yi seçin.

3. **Arka uç Havuzu Ekle** sayfasında, ad için, arka uç havuzunuzun adı olarak **Mybackendpooloutbound**yazın ve ardından **Ekle**' yi seçin.

### <a name="create-outbound-rule"></a>Giden kuralı oluştur

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **giden kuralları**' nı ve ardından **Ekle**' yi seçin.

3. Giden kuralları yapılandırmak için şu değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Adı | **Mbir Boundrule**girin. |
    | Ön uç IP adresi | **LoadBalancerFrontEndOutbound**öğesini seçin. |
    | Boşta kalma zaman aşımı (dakika) | Kaydırıcıyı * * 15 dakikaya taşıyın.|
    | TCP sıfırlaması | **Etkin**'i seçin.|
    | Arka uç havuzu | **Mybackendpooloutbound** seçin |
    | Bağlantı noktası ayırma-> bağlantı noktası ayırma | **Giden bağlantı noktası sayısını el ile** seçin |
    | Giden bağlantı noktaları-> seçin | **Örnek başına bağlantı noktası** seçin |
    | Giden bağlantı noktaları-örnek başına > bağlantı noktaları | **10.000**girin. |

4. **Add (Ekle)** seçeneğini belirleyin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Yük dengeleyiciyi içeren **Myresourcegroupslb** kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede:
 - Standart yük dengeleyici oluşturdunuz.
 - Hem gelen hem de giden yük dengeleyici trafik kuralları yapılandırıldı.
 - Arka uç havuzundaki VM 'Ler için bir sistem durumu araştırması yapılandırıldı. 

Daha fazla bilgi edinmek için [Azure Load Balancer öğreticilerine](tutorial-load-balancer-standard-public-zone-redundant-portal.md)geçin.
