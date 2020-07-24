---
title: 'Hızlı başlangıç: ortak yük dengeleyici oluşturma-Azure portal'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta Azure portal kullanarak yük dengeleyici oluşturma gösterilmektedir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a load balancer so that I can load balance internet traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/17/2020
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: f9d736098e42bf5ca07eca0cb952275c5e39c2a9
ms.sourcegitcommit: 0e8a4671aa3f5a9a54231fea48bcfb432a1e528c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/24/2020
ms.locfileid: "87125199"
---
# <a name="quickstart-create-a-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak VM 'Leri dengelemek için yük dengeleyici oluşturma

Ortak yük dengeleyici ve üç sanal makine oluşturmak için Azure portal kullanarak Azure Load Balancer kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

---

# <a name="option-1-default-create-a-load-balancer-standard-sku"></a>[Seçenek 1 (varsayılan): yük dengeleyici oluşturma (Standart SKU)](#tab/option-1-create-load-balancer-standard)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir.  SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

Bu bölümde, sanal makinelerin yükünü dengeleyen bir yük dengeleyici oluşturacaksınız. 

Ortak yük dengeleyici veya iç yük dengeleyici oluşturabilirsiniz. 

Ortak yük dengeleyici oluşturduğunuzda, yük dengeleyici için ön uç (varsayılan olarak **Loadbalancerön uç** olarak adlandırılır) olarak yapılandırılmış yeni BIR genel IP adresi oluşturun.

1. Ekranın sol üst kısmında, **kaynak oluştur**  >  **ağ**  >  **Load Balancer**' ı seçin.

2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni oluştur** ' u seçin ve metin kutusuna **Myresourcegrouplb** yazın.|
    | Name                   | **Myloadbalancer** girin                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Genel**’i seçin.                                        |
    | SKU           | **Standart** seçin |
    | Genel IP adresi | **Yeni oluştur**’u seçin. Kullanmak istediğiniz mevcut bir genel IP varsa, **Varolanı kullan**' ı seçin. |
    | Genel IP adresi adı | Metin kutusuna **Mypublicıp** yazın.|
    | Kullanılabilirlik alanı | Esnek yük dengeleyici oluşturmak için **bölge yedekli** ' ı seçin. Bir ZGen yük dengeleyici oluşturmak için, 1, 2 veya 3 ' ten belirli bir bölge seçin |
    | Genel IPv6 adresi ekle | **Hayır**'ı seçin. </br> IPv6 adresleri ve yük dengeleyici hakkında daha fazla bilgi için bkz. [Azure sanal ağ Için IPv6 nedir?](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)  |

3. Geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin.

4. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.   
    
    :::image type="content" source="./media/quickstart-load-balancer-standard-public-portal/create-standard-load-balancer.png" alt-text="Standart yük dengeleyici oluşturma" border="true":::
 
## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde şunları yapılandırırsınız:

* Arka uç adres havuzu için yük dengeleyici ayarları.
* Bir sistem durumu araştırması.
* Yük dengeleyici kuralı ve otomatik giden kuralı.

### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Arka uç adres havuzu, yük dengeleyiciye bağlı sanal (NIC) IP adreslerini içerir. 

Yük Dengeleme internet trafiği için sanal makineleri dahil etmek üzere **Mybackendpool** arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **arka uç havuzları**' nı seçin ve ardından **Ekle**' yi seçin.

3. **Arka uç Havuzu Ekle** sayfasında, ad için, arka uç havuzunuzun adı olarak **mybackendpool**yazın ve ardından **Ekle**' yi seçin.

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyici, uygulamanızın durumunu bir sistem durumu araştırmasıyla izler. 

Sistem durumu araştırması, sistem durumu denetimlerine yönelik yanıtlarını temel alarak yük dengeleyiciden VM 'Ler ekler veya kaldırır. 

Sanal makinelerin durumunu izlemek için **myHealthProbe** adlı bir durum araştırması oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **sistem durumu araştırmaları**' nı ve ardından **Ekle**' yi seçin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Myhealtharaştırması**girin. |
    | Protokol | **Http**'yi seçin. |
    | Bağlantı noktası | **80**girin.|
    | Aralık | Yoklama denemeleri arasındaki saniye cinsinden **Aralık** sayısı için **15** girin. |
    | İyi durumda olmayan durum eşiği | Bir VM sağlıksız kabul edilmeden önce gerçekleşmesi gereken **sağlıksız eşik** veya arka arkaya araştırma hatası sayısı için **2** ' yi seçin.|
    | | |

3. Rest varsayılanlarını bırakın ve **Tamam**' ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Trafiği almak için gelen trafik için ön uç IP yapılandırmasını ve arka uç IP havuzunu tanımlarsınız. Kaynak ve hedef bağlantı noktası kuralda tanımlanmıştır. 

Bu bölümde, bir yük dengeleyici kuralı oluşturacaksınız:

* Adlandırılmış **Myhttprule**.
* **Loadbalancerön uç**adında.
* **80 numaralı bağlantı noktasını**dinliyor.
* Yük dengeli trafiği, **80 numaralı bağlantı noktasında** **Mybackendpool** adlı arka uca yönlendirir.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**bölümünde **Yük Dengeleme kuralları**' nı ve ardından **Ekle**' yi seçin.

3. Yük Dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Myhttprule**girin. |
    | IP sürümü | **IPv4** seçin |
    | Ön uç IP adresi | **Loadbalancerön uç** seçin |
    | Protokol | **TCP**’yi seçin. |
    | Bağlantı noktası | **80**girin.|
    | Arka uç bağlantı noktası | **80**girin. |
    | Arka uç havuzu | **Mybackendpool**öğesini seçin.|
    | Durum yoklaması | **Myhealtharaştırması**' ni seçin. |
    | Örtük giden kuralları oluşturma | **Hayır**'ı seçin.

4. Kalan varsayılan değerleri bırakın ve **Tamam**' ı seçin.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde şunları yapacaksınız:

* Sanal ağ oluşturun.
* Yük dengeleyicinin arka uç havuzu için üç sanal makine oluşturun.
* Yük dengeleyiciyi test etmek için sanal makinelere IIS yükleyin.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, adımlarda bulunan parametreleri aşağıdaki bilgilerle değiştirirsiniz:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Batı Avrupa      |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Genel IP SKU 'Ları ve yük dengeleyici SKU 'Larının eşleşmesi gerekir. Standart yük dengeleyici için, arka uç havuzunda standart IP adresleriyle VM 'Leri kullanın. 

Bu bölümde, üç farklı bölgede (**bölge 1**, **bölge 2**ve **Bölge 3**) standart bir genel IP adresi ile üç VM (**myVM1**, **myVM2** ve **myVM3**) oluşturacaksınız. 

Bu VM 'Ler, daha önce oluşturulmuş yük dengeleyicinin arka uç havuzuna eklenir.

1. Portalın sol üst kısmında **kaynak oluştur**  >  **işlem**  >  **sanal makinesi**' ni seçin. 
   
2. **Sanal makine oluştur**' da **temel bilgiler** sekmesinde değerleri yazın veya seçin:

    | Ayar | Değer                                          |
    |-----------------------|----------------------------------|
    | **Proje ayrıntıları** |  |
    | Abonelik | Azure aboneliğinizi seçin |
    | Kaynak Grubu | **Myresourcegrouplb** seçin |
    | **Örnek ayrıntıları** |  |
    | Sanal makine adı | **MyVM1** girin |
    | Bölge | **Batı Avrupa** seçin |
    | Kullanılabilirlik seçenekleri | **Kullanılabilirlik bölgelerini** seçin |
    | Kullanılabilirlik alanı | **1** seçin |
    | Görüntü | **Windows Server 2019 Datacenter** seçin |
    | Azure Spot örneği | **Hayır** seçin |
    | Boyut | VM boyutunu seçin veya varsayılan ayarı yapın |
    | **Yönetici hesabı** |  |
    | Kullanıcı adı | Kullanıcı adı girin |
    | Parola | Parola girin |
    | Parolayı onayla | Parolayı yeniden girin |

3. **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin.
  
4. Ağ sekmesinde, şunu seçin veya girin:

    | Ayar | Değer |
    |-|-|
    | **Ağ arabirimi** |  |
    | Sanal ağ | **myVNet** |
    | Alt ağ | **myBackendSubnet** |
    | Genel IP | **Myvm-ip**varsayılanını kabul edin. </br> IP Bölge 1 otomatik olarak standart SKU IP 'si olur. |
    | NIC ağ güvenlik grubu | **Gelişmiş** seçin|
    | Ağ güvenlik grubunu yapılandırma | **Yeni oluştur**’u seçin. </br> **Ağ güvenlik grubu oluştur**' da, **ad**alanına **mynsg** yazın. </br> **Gelen kuralları**altında **+ gelen kuralı ekle**' yi seçin. </br> **Hedef bağlantı noktası aralıkları**altında **80**girin. </br> **Öncelik**altında **100**girin. </br> **Ad**alanına **Myhttprule** yazın </br> **Ekle** 'yi seçin </br> **Tamam 'ı** seçin |
    | **Yük dengeleme**  |
    | Bu sanal makine, var olan bir yük dengeleme çözümünün arkasına mi yerleştirsin? | **Evet** ' i seçin |
    | **Yük Dengeleme ayarları** |
    | Yük dengeleme seçenekleri | **Azure yük dengelemeyi** seçin |
    | Yük dengeleyici seçin | **Myloadbalancer** seçin  |
    | Bir arka uç havuzu seçin | **Mybackendpool** seçin |

5. **Yönetim** sekmesini seçin veya **İleri**  >  **Yönetim**' i seçin.

6. **Yönetim** sekmesinde, şunu seçin veya girin:
    
    | Ayar | Değer |
    |-|-|
    | **İzleme** |  |
    | Önyükleme tanılaması | Seçme **kapalı** |
   
7. **Gözden geçir ve oluştur**’u seçin. 
  
8. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

9. Aşağıdaki değerleri ve **myVM1**ile aynı diğer tüm ayarları içeren Iki ek VM oluşturmak için 1 ile 8 arasındaki adımları uygulayın:

    | Ayar | VM 2| VM 3|
    | ------- | ----- |---|
    | Name |  **myVM2** |**myVM3**|
    | Kullanılabilirlik alanı | **2** |**3**|
    | Ağ güvenlik grubu | Mevcut **Mynsg** 'yi seçin| Mevcut **Mynsg** 'yi seçin|

## <a name="create-outbound-rule-configuration"></a>Giden kuralı yapılandırması oluştur
Yük dengeleyici giden kuralları arka uç havuzundaki VM 'Ler için giden SNAT 'yi yapılandırır. 

Giden bağlantılar hakkında daha fazla bilgi için bkz. [Azure 'Da giden bağlantılar](load-balancer-outbound-connections.md).

### <a name="create-outbound-rule"></a>Giden kuralı oluştur

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **giden kuralları**' nı ve ardından **Ekle**' yi seçin.

3. Giden kuralları yapılandırmak için şu değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Mbir Boundrule**girin. |
    | Ön uç IP adresi | **Yeni oluştur**’u seçin. </br> **Ad**alanına **LoadBalancerFrontEndOutbound**girin. </br> **IP adresi** veya **IP öneki**' ni seçin. </br> **Genel IP adresi** veya **genel IP ön eki**altında **Yeni oluştur** ' u seçin. </br> Ad için **Mypublicıpoıb Utbağlanmadı** veya **myPublicIPPrefixOutbound**girin. </br> **Tamam**’ı seçin. </br> **Ekle**’yi seçin.|
    | Boşta kalma zaman aşımı (dakika) | Kaydırıcıyı **15 dakikaya**taşıyın.|
    | TCP sıfırlaması | **Etkin**'i seçin.|
    | Arka uç havuzu | **Yeni oluştur**’u seçin. </br> **Ad**alanına **Mybackendpooloutbound** yazın. </br> **Ekle**’yi seçin. |
    | Bağlantı noktası ayırma-> bağlantı noktası ayırma | **Giden bağlantı noktası sayısını el ile** seçin |
    | Giden bağlantı noktaları-> seçin | **Örnek başına bağlantı noktası** seçin |
    | Giden bağlantı noktaları-örnek başına > bağlantı noktaları | **10000**girin. |

4. **Ekle**’yi seçin.

### <a name="add-virtual-machines-to-outbound-pool"></a>Sanal makineleri giden havuzuna Ekle

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**’ın altında **Arka Uç Havuzları**’nı seçin.

3. **Mybackendpooloutbound**öğesini seçin.

4. **Sanal ağ**' da, **myvnet**' i seçin.

5. **Sanal makinelerde** **+ Ekle**' yi seçin.

6. **MyVM1**, **myVM2**ve **myVM3**' nin yanındaki kutuları işaretleyin. 

7. **Ekle**’yi seçin.

8. **Kaydet**'i seçin.

# <a name="option-2-create-a-load-balancer-basic-sku"></a>[2. seçenek: yük dengeleyici oluşturma (temel SKU)](#tab/option-1-create-load-balancer-basic)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir.  SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

Bu bölümde, sanal makinelerin yükünü dengeleyen bir yük dengeleyici oluşturacaksınız. 

Ortak yük dengeleyici veya iç yük dengeleyici oluşturabilirsiniz. 

Ortak yük dengeleyici oluşturduğunuzda, yük dengeleyici için ön uç (varsayılan olarak **Loadbalancerön uç** olarak adlandırılır) olarak yapılandırılmış yeni BIR genel IP adresi oluşturun.

1. Ekranın sol üst kısmında, **kaynak oluştur**  >  **ağ**  >  **Load Balancer**' ı seçin.

2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | **Yeni oluştur** ' u seçin ve metin kutusuna **Myresourcegrouplb** yazın.|
    | Name                   | **Myloadbalancer** girin                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Genel**’i seçin.                                        |
    | SKU           | **Temel** seçin |
    | Genel IP adresi | **Yeni oluştur**’u seçin. Kullanmak istediğiniz mevcut bir genel IP varsa, **Varolanı kullan**' ı seçin. |
    | Genel IP adresi adı | Metin kutusuna **Mypublicıp** yazın.|
    | Atama | **Dinamik** seçin |
    | Genel IPv6 adresi ekle | **Hayır**'ı seçin. </br> IPv6 adresleri ve yük dengeleyici hakkında daha fazla bilgi için bkz. [Azure sanal ağ Için IPv6 nedir?](https://docs.microsoft.com/azure/virtual-network/ipv6-overview)  |

3. Geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin.

4. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.   

    :::image type="content" source="./media/quickstart-load-balancer-standard-public-portal/create-basic-load-balancer.png" alt-text="Temel yük dengeleyici oluşturma" border="true":::

## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde şunları yapılandırırsınız:

* Sanal ağ oluşturun.
* Arka uç adres havuzu için yük dengeleyici ayarları.
* Bir sistem durumu araştırması.
* Yük dengeleyici kuralı.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, adımlarda bulunan parametreleri aşağıdaki bilgilerle değiştirirsiniz:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | Batı Avrupa      |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-a-backend-pool"></a>Arka uç havuzu oluşturma

Arka uç adres havuzu, yük dengeleyiciye bağlı sanal (NIC) IP adreslerini içerir. 

Yük Dengeleme internet trafiği için sanal makineleri dahil etmek üzere **Mybackendpool** arka uç adres havuzunu oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **arka uç havuzları**' nı seçin ve ardından **Ekle**' yi seçin.

3. **Arka uç Havuzu Ekle** sayfasında girin veya seçin:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Mybackendpool**girin. |
    | Sanal ağ | **Myvnet**' i seçin. |
    | İlişkili olduğu öğe | **Sanal makineleri** seçin |

### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyici, uygulamanızın durumunu bir sistem durumu araştırmasıyla izler. 

Sistem durumu araştırması, sistem durumu denetimlerine yönelik yanıtlarını temel alarak yük dengeleyiciden VM 'Ler ekler veya kaldırır. 

Sanal makinelerin durumunu izlemek için **myHealthProbe** adlı bir durum araştırması oluşturun.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **sistem durumu araştırmaları**' nı ve ardından **Ekle**' yi seçin.
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Myhealtharaştırması**girin. |
    | Protokol | **Http**'yi seçin. |
    | Bağlantı noktası | **80**girin.|
    | Yol | Girmesini**/** |
    | Aralık | Yoklama denemeleri arasındaki saniye cinsinden **Aralık** sayısı için **15** girin. |
    | İyi durumda olmayan durum eşiği | Bir VM sağlıksız kabul edilmeden önce gerçekleşmesi gereken **sağlıksız eşik** veya arka arkaya araştırma hatası sayısı için **2** ' yi seçin.|

3. **Tamam**’ı seçin.

### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Trafiğin VM’lere dağıtımını tanımlamak için bir yük dengeleyici kuralı kullanılır. Trafiği almak için gelen trafik için ön uç IP yapılandırmasını ve arka uç IP havuzunu tanımlarsınız. Kaynak ve hedef bağlantı noktası kuralda tanımlanmıştır. 

Bu bölümde, bir yük dengeleyici kuralı oluşturacaksınız:

* Adlandırılmış **Myhttprule**.
* **Loadbalancerön uç**adında.
* **80 numaralı bağlantı noktasını**dinliyor.
* Yük dengeli trafiği, **80 numaralı bağlantı noktasında** **Mybackendpool** adlı arka uca yönlendirir.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**bölümünde **Yük Dengeleme kuralları**' nı ve ardından **Ekle**' yi seçin.

3. Yük Dengeleme kuralını yapılandırmak için şu değerleri kullanın:
    
    | Ayar | Değer |
    | ------- | ----- |
    | Ad | **Myhttprule**girin. |
    | IP sürümü | **IPv4** seçin |
    | Ön uç IP adresi | **Loadbalancerön uç** seçin |
    | Protokol | **TCP**’yi seçin. |
    | Bağlantı noktası | **80**girin.|
    | Arka uç bağlantı noktası | **80**girin. |
    | Arka uç havuzu | **Mybackendpool**öğesini seçin.|
    | Durum yoklaması | **Myhealtharaştırması**' ni seçin. |
 
4. Kalan varsayılan değerleri bırakın ve **Tamam**' ı seçin.

## <a name="create-backend-servers"></a>Arka uç sunucular oluşturma

Bu bölümde şunları yapacaksınız:

* Yük dengeleyicinin arka uç havuzu için üç sanal makine oluşturun.
* Sanal makineler için bir kullanılabilirlik kümesi oluşturun.
* Yük dengeleyiciyi test etmek için sanal makinelere IIS yükleyin.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Genel IP SKU 'Ları ve yük dengeleyici SKU 'Larının eşleşmesi gerekir. Temel yük dengeleyici için arka uç havuzundaki temel IP adresleriyle VM 'Leri kullanın. 

Bu bölümde, temel genel IP adresi ile üç VM (**myVM1**, **myVM2**ve **myVM3**) oluşturacaksınız.  

Üç VM, **myAvailabilitySet**adlı bir kullanılabilirlik kümesine eklenecektir.

Bu VM 'Ler, daha önce oluşturulmuş yük dengeleyicinin arka uç havuzuna eklenir.

1. Portalın sol üst kısmında **kaynak oluştur**  >  **işlem**  >  **sanal makinesi**' ni seçin. 
   
2. **Sanal makine oluştur**' da **temel bilgiler** sekmesinde değerleri yazın veya seçin:

    | Ayar | Değer                                          |
    |-----------------------|----------------------------------|
    | **Proje ayrıntıları** |  |
    | Abonelik | Azure aboneliğinizi seçin |
    | Kaynak Grubu | **Myresourcegrouplb** seçin |
    | **Örnek ayrıntıları** |  |
    | Sanal makine adı | **MyVM1** girin |
    | Bölge | **Batı Avrupa** seçin |
    | Kullanılabilirlik seçenekleri | **Kullanılabilirlik kümesi**’ni seçin |
    | Kullanılabilirlik kümesi | **Yeni oluştur**’u seçin. </br> Ada **myAvailabilitySet** girin **Name**. </br> **Tamam 'ı** seçin |
    | Görüntü | **Windows Server 2019 Datacenter** |
    | Azure Spot örneği | **Hayır** seçin |
    | Boyut | VM boyutunu seçin veya varsayılan ayarı yapın |
    | **Yönetici hesabı** |  |
    | Kullanıcı adı | Kullanıcı adı girin |
    | Parola | Parola girin |
    | Parolayı onayla | Parolayı yeniden girin |

3. **Ağ** sekmesini seçin veya **Sonraki: diskler**' i ve sonra **İleri: ağ**' ı seçin.
  
4. Ağ sekmesinde, şunu seçin veya girin:

    | Ayar | Değer |
    |-|-|
    | **Ağ arabirimi** |  |
    | Sanal ağ | **Myvnet** seçin |
    | Alt ağ | **Mybackendsubnet** seçin |
    | Genel IP | **Yeni oluştur** ' u seçin </br> Ad olarak **Myvm-ip** girin. </br> **Tamam 'ı** seçin |
    | NIC ağ güvenlik grubu | **Gelişmiş** seçin|
    | Ağ güvenlik grubunu yapılandırma | **Yeni oluştur**’u seçin. </br> **Ağ güvenlik grubu oluştur**' da, **ad**alanına **mynsg** yazın. </br> **Gelen kuralları**altında **+ gelen kuralı ekle**' yi seçin. </br> **Hedef bağlantı noktası aralıkları**altında **80**girin. </br> **Öncelik**altında **100**girin. </br> **Ad**alanına **Myhttprule** yazın </br> **Ekle** 'yi seçin </br> **Tamam 'ı** seçin |
    | **Yük dengeleme**  |
    | Bu sanal makine, var olan bir yük dengeleme çözümünün arkasına mi yerleştirsin? | **Hayır** seçin |
 
5. **Yönetim** sekmesini seçin veya **İleri**  >  **Yönetim**' i seçin.

6. **Yönetim** sekmesinde, şunu seçin veya girin:
    | Ayar | Değer |
    |-|-|
    | **İzleme** | |
    | Önyükleme tanılaması | Seçme **kapalı** |

7. **Gözden geçir ve oluştur**’u seçin. 
  
8. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

9. Aşağıdaki değerleri ve **myVM1**ile aynı diğer tüm ayarları içeren Iki ek VM oluşturmak için 1 ile 8 arasındaki adımları uygulayın:

    | Ayar | VM 2| VM 3|
    | ------- | ----- |---|
    | Name |  **myVM2** |**myVM3**|
    | Kullanılabilirlik kümesi| **MyAvailabilitySet** seçin | **MyAvailabilitySet** seçin|
    | Ağ güvenlik grubu | Mevcut **Mynsg** 'yi seçin| Mevcut **Mynsg** 'yi seçin|

---

## <a name="install-iis"></a>IIS yükleme

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden, **myresourcegrouplb** kaynak grubunda bulunan **myVM1** ' yi seçin.

2. **Genel bakış** SAYFASıNDA, VM için RDP dosyasını Indirmek üzere **Bağlan** ' ı seçin.

3. RDP dosyasını açın.

4. VM oluşturma işlemleri sırasında belirlediğiniz kimlik bilgilerini kullanarak VM'de oturum açın.

5. Sunucu masaüstünde **Windows Yönetim Araçları** > **Windows PowerShell**' e gidin.

6. PowerShell penceresinde aşağıdaki komutları çalıştırın:

    * IIS sunucusunu yükler
    * Varsayılan iisstart.htm dosyasını Kaldır
    * VM 'nin adını görüntüleyen yeni bir iisstart.htm dosyası ekleyin:

   ```powershell
    
    # install IIS server role
    Install-WindowsFeature -name Web-Server -IncludeManagementTools
    
    # remove default htm file
     remove-item  C:\inetpub\wwwroot\iisstart.htm
    
    # Add a new htm file that displays server name
     Add-Content -Path "C:\inetpub\wwwroot\iisstart.htm" -Value $("Hello World from " + $env:computername)
   ```
7. **myVM1** ile RDP oturumunu kapatın.

8. IIS’yi ve **myVM2** ve **myVM3**’teki güncelleştirilmiş iisstart.htm dosyasını yüklemek için 1 ile 6 arasındaki adımları tekrarlayın.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. **Genel Bakış** ekranında yük dengeleyici için genel IP adresini bulun. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından **mypublicıp**' yi seçin.

2. Genel IP adresini kopyalayıp tarayıcınızın adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

   ![IIS Web sunucusu](./media/tutorial-load-balancer-standard-zonal-portal/load-balancer-test.png)

Yük dengeleyicinin trafiği üç VM 'ye dağıtmasını görmek için, her bir sanal makinenin IIS Web sunucusunun varsayılan sayfasını özelleştirebilir ve sonra Web tarayıcınızı istemci makinesinden yenileyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için kaynakları içeren **Myresourcegrouplb** kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta:

* Azure Standard veya temel Load Balancer oluşturma
* Yük dengeleyicisine 3 VM eklenmiş.
* Yük dengeleyici trafik kuralını, sistem durumu araştırmasını ve sonra yük dengeleyiciyi test edin. 

Azure Load Balancer hakkında daha fazla bilgi edinmek için [Azure Load Balancer nedir?](load-balancer-overview.md) ve [sık sorulan sorular Load Balancer](load-balancer-faqs.md).

[Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
