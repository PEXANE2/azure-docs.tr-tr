---
title: 'Hızlı başlangıç: iç yük dengeleyici oluşturma-Azure portal'
titleSuffix: Azure Load Balancer
description: Bu hızlı başlangıçta, Azure portal kullanarak iç yük dengeleyicinin nasıl oluşturulacağı gösterilmektedir.
services: load-balancer
documentationcenter: na
author: asudbring
manager: KumudD
Customer intent: I want to create a internal load balancer so that I can load balance internal traffic to VMs.
ms.service: load-balancer
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: infrastructure-services
ms.date: 07/30/2020
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: fb0a5c06c8b7bdbb62e937e865e6bb2fd4000f2d
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87462488"
---
# <a name="quickstart-create-an-internal-load-balancer-to-load-balance-vms-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak VM 'Lerin yükünü dengelemek için iç yük dengeleyici oluşturma

İç yük dengeleyici ve iki sanal makine oluşturmak için Azure portal kullanarak Azure Load Balancer kullanmaya başlayın.

## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F).

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

[https://portal.azure.com](https://portal.azure.com) adresinden Azure portalında oturum açın.

---

# <a name="option-1-default-create-a-internal-load-balancer-standard-sku"></a>[Seçenek 1 (varsayılan): iç yük dengeleyici oluşturma (Standart SKU)](#tab/option-1-create-internal-load-balancer-standard)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir.  SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

Bu bölümde, sanal makinelerin yükünü dengeleyen bir yük dengeleyici oluşturacaksınız. 

Ortak yük dengeleyici veya iç yük dengeleyici oluşturabilirsiniz. 

Bir iç yük dengeleyici oluşturduğunuzda, yük dengeleyici için ağ olarak bir sanal ağ yapılandırılır. 

Sanal ağdaki özel bir IP adresi, yük dengeleyici için ön uç (varsayılan olarak **Loadbalancerön uç** olarak adlandırılır) olarak yapılandırılır. 

Ön uç IP adresi **statik** veya **dinamik**olabilir.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, adımlarda bulunan parametreleri aşağıdaki bilgilerle değiştirirsiniz:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | West Europe      |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

1. Ekranın sol üst kısmında, **kaynak oluştur**  >  **ağ**  >  **Load Balancer**' ı seçin.

2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Önceki adımda oluşturulan **Myresourcegrouplb** öğesini seçin.|
    | Name                   | **Myloadbalancer** girin                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Dahili**' ı seçin.                                        |
    | SKU           | **Standart** seçin |
    | Sanal ağ | Önceki adımda oluşturulan **Myvnet** öğesini seçin. |
    | Alt ağ  | Önceki adımda oluşturulan **Mybackendsubnet** öğesini seçin. |
    | IP adresi ataması | **Dinamik**' i seçin. |
    | Kullanılabilirlik alanı | **Bölge yedekli** seçeneğini belirleyin |

3. Geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin.

4. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.   
    
    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/create-standard-internal-load-balancer.png" alt-text="Standart iç yük dengeleyici oluşturma" border="true":::
 
## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde şunları yapılandırırsınız:

* Arka uç adres havuzu için yük dengeleyici ayarları.
* Bir sistem durumu araştırması.
* Yük dengeleyici kuralı.

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

* Yük dengeleyicinin arka uç havuzu için iki sanal makine oluşturun.
* Yük dengeleyiciyi test etmek için sanal makinelere IIS yükleyin.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Bu bölümde, iki bölgede standart bir genel IP adresi (**bölge 1** ve **bölge 2**) Ile iki VM (**myVM1** ve **myVM2**) oluşturacaksınız. 

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
    | Ağ güvenlik grubunu yapılandırma | **Yeni oluştur**’u seçin. </br> **Ağ güvenlik grubu oluştur**' da, **ad**alanına **mynsg** yazın. </br> **Tamam 'ı** seçin |
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

9. Aşağıdaki değerleri ve **myVM1**ile aynı diğer tüm ayarları içeren BIR ek VM oluşturmak için 1 ile 8 arasındaki adımları uygulayın:

    | Ayar | VM 2|
    | ------- | ----- |
    | Name |  **myVM2** |
    | Kullanılabilirlik alanı | **2** |
    | Ağ güvenlik grubu | Mevcut **Mynsg** 'yi seçin|


# <a name="option-2-create-a-internal-load-balancer-basic-sku"></a>[2. seçenek: iç yük dengeleyici oluşturma (temel SKU)](#tab/option-1-create-internal-load-balancer-basic)

>[!NOTE]
>Standart SKU yük dengeleyici, üretim iş yükleri için önerilir.  SKU 'lar hakkında daha fazla bilgi için bkz. **[Azure Load Balancer SKU 'lar](skus.md)**.

Bu bölümde, sanal makinelerin yükünü dengeleyen bir yük dengeleyici oluşturacaksınız. 

Ortak yük dengeleyici veya iç yük dengeleyici oluşturabilirsiniz. 

Bir iç yük dengeleyici oluşturduğunuzda, yük dengeleyici için ağ olarak bir sanal ağ yapılandırılır. 

Sanal ağdaki özel bir IP adresi, yük dengeleyici için ön uç (varsayılan olarak **Loadbalancerön uç** olarak adlandırılır) olarak yapılandırılır. 

Ön uç IP adresi **statik** veya **dinamik**olabilir.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, adımlarda bulunan parametreleri aşağıdaki bilgilerle değiştirirsiniz:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<resource-group-name>**  | myResourceGroupLB |
| **\<virtual-network-name>** | myVNet          |
| **\<region-name>**          | West Europe      |
| **\<IPv4-address-space>**   | 10.1.0.0 \ 16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<subnet-address-range>** | 10.1.0.0 \ 24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

## <a name="create-load-balancer"></a>Yük dengeleyici oluşturma

1. Ekranın sol üst kısmında, **kaynak oluştur**  >  **ağ**  >  **Load Balancer**' ı seçin.

2. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin: 

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | Abonelik               | Aboneliğinizi seçin.    |    
    | Kaynak grubu         | Önceki adımda oluşturulan **Myresourcegrouplb** öğesini seçin.|
    | Name                   | **Myloadbalancer** girin                                   |
    | Bölge         | **Batı Avrupa**'yı seçin.                                        |
    | Tür          | **Dahili**' ı seçin.                                        |
    | SKU           | **Temel** seçin |
    | Sanal ağ | Önceki adımda oluşturulan **Myvnet** öğesini seçin. |
    | Alt ağ  | Önceki adımda oluşturulan **Mybackendsubnet** öğesini seçin. |
    | IP adresi ataması | **Dinamik**' i seçin. |

3. Geri kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur**' u seçin.

4. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.   

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/create-basic-internal-load-balancer.png" alt-text="Standart iç yük dengeleyici oluşturma" border="true":::

## <a name="create-load-balancer-resources"></a>Yük dengeleyici kaynakları oluşturma

Bu bölümde şunları yapılandırırsınız:

* Arka uç adres havuzu için yük dengeleyici ayarları.
* Bir sistem durumu araştırması.
* Yük dengeleyici kuralı.

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

4. **Ekle**'yi seçin.

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

* Yük dengeleyicinin arka uç havuzu için iki sanal makine oluşturun.
* Sanal makineler için bir kullanılabilirlik kümesi oluşturun.
* Yük dengeleyiciyi test etmek için sanal makinelere IIS yükleyin.

### <a name="create-virtual-machines"></a>Sanal makineler oluşturma

Genel IP SKU 'Ları ve yük dengeleyici SKU 'Larının eşleşmesi gerekir. Temel yük dengeleyici için arka uç havuzundaki temel IP adresleriyle VM 'Leri kullanın. 

Bu bölümde, temel bir genel IP adresi ile iki VM (**myVM1**ve **myVM2**) oluşturacaksınız.  

İki VM, **myAvailabilitySet**adlı bir kullanılabilirlik kümesine eklenecektir.

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
    | Ağ güvenlik grubunu yapılandırma | **Yeni oluştur**’u seçin. </br> **Ağ güvenlik grubu oluştur**' da, **ad**alanına **mynsg** yazın. </br> **Tamam 'ı** seçin |
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

9. Aşağıdaki değerleri ve **myVM1**ile aynı diğer tüm ayarları içeren BIR ek VM oluşturmak için 1 ile 8 arasındaki adımları uygulayın:

    | Ayar | VM 2 |
    | ------- | ----- |
    | Name |  **myVM2** |
    | Kullanılabilirlik kümesi| **MyAvailabilitySet** seçin |
    | Ağ güvenlik grubu | Mevcut **Mynsg** 'yi seçin|

### <a name="add-virtual-machines-to-the-backend-pool"></a>Arka uç havuzuna sanal makineler ekleme

Önceki adımlarda oluşturulan VM 'Lerin **Myloadbalancer**arka uç havuzuna eklenmesi gerekir.

1. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.

2. **Ayarlar**altında **arka uç havuzları**' nı seçin ve ardından **mybackendpool**' u seçin.

3. Ilişkili **sanal makineleri** seçin **Associated to**.

4. **Sanal makineler** bölümünde **+ Ekle**' yi seçin.

5. **MyVM1** ve **myVM2**' nin yanındaki kutuları seçin.

6. **Ekle**'yi seçin.

7. **Kaydet**'i seçin.
---

## <a name="create-test-virtual-machine"></a>Test sanal makinesi oluştur

Bu bölümde, **Mytestvm**adlı bir sanal makine oluşturacaksınız.  Bu VM, yük dengeleyici yapılandırmasını test etmek için kullanılacaktır.

1. Portalın sol üst kısmında **kaynak oluştur**  >  **işlem**  >  **sanal makinesi**' ni seçin. 
   
2. **Sanal makine oluştur**' da **temel bilgiler** sekmesinde değerleri yazın veya seçin:

    | Ayar | Değer                                          |
    |-----------------------|----------------------------------|
    | **Proje ayrıntıları** |  |
    | Abonelik | Azure aboneliğinizi seçin |
    | Kaynak Grubu | **Myresourcegrouplb** seçin |
    | **Örnek ayrıntıları** |  |
    | Sanal makine adı | **Mytestvm** girin |
    | Bölge | **Batı Avrupa** seçin |
    | Kullanılabilirlik seçenekleri | **Altyapı yedekliliği gerekli değil** ' i seçin |
    | Kullanılabilirlik alanı | **Bölge yedekli** seçeneğini belirleyin |
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
    | Genel IP | **Mytestvm-IP**varsayılan değerini kabul edin. |
    | NIC ağ güvenlik grubu | **Gelişmiş** seçin|
    | Ağ güvenlik grubunu yapılandırma | Önceki adımda oluşturulan **Mynsg** öğesini seçin.|
    
5. **Yönetim** sekmesini seçin veya **İleri**  >  **Yönetim**' i seçin.

6. **Yönetim** sekmesinde, şunu seçin veya girin:
    
    | Ayar | Değer |
    |-|-|
    | **İzleme** |  |
    | Önyükleme tanılaması | Seçme **kapalı** |
   
7. **Gözden geçir ve oluştur**’u seçin. 
  
8. Ayarları gözden geçirin ve ardından **Oluştur**' u seçin.

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

8. IIS’yi ve **myVM2**’deki güncelleştirilmiş iisstart.htm dosyasını yüklemek için 1 ile 6 arasındaki adımları tekrarlayın.

## <a name="test-the-load-balancer"></a>Yük dengeleyiciyi test etme

1. **Genel bakış** ekranında yük DENGELEYICININ özel IP adresini bulun. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından **myloadbalancer**' ı seçin.

2. **Myloadbalancer** **genel** görünümünde, **özel IP adresi** ' nin yanındaki bir yere göz atın veya adresi kopyalayın.

3. Sol taraftaki menüden **tüm hizmetler** ' i seçin, **tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **Myresourcegrouplb** kaynak grubunda bulunan **mytestvm** ' yi seçin.

4. **Genel bakış** SAYFASıNDA, VM için RDP dosyasını Indirmek üzere **Bağlan** ' ı seçin.

5. RDP dosyasını açın.

6. VM oluşturma işlemleri sırasında belirlediğiniz kimlik bilgilerini kullanarak VM'de oturum açın.

7. **Mytestvm**'de **Internet Explorer** 'ı açın.

4. Özel IP adresini kopyalayın ve tarayıcının adres çubuğuna yapıştırın. IIS Web sunucusunun varsayılan sayfası, tarayıcıda görüntülenir.

    :::image type="content" source="./media/quickstart-load-balancer-standard-internal-portal/load-balancer-test.png" alt-text="Standart iç yük dengeleyici oluşturma" border="true":::
   
Yük dengeleyicinin trafiği üç VM 'ye dağıtmasını görmek için, her bir sanal makinenin IIS Web sunucusunun varsayılan sayfasını özelleştirebilir ve sonra Web tarayıcınızı istemci makinesinden yenileyebilirsiniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında kaynak grubunu, yük dengeleyiciyi ve tüm ilgili kaynakları silin. Bunu yapmak için kaynakları içeren **Myresourcegrouplb** kaynak grubunu seçin ve **Sil**' i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta:

* Azure Standard veya temel dahili Load Balancer oluşturma
* Yük dengeleyicisine 2 sanal makine eklendi.
* Yük dengeleyici trafik kuralını, sistem durumu araştırmasını ve sonra yük dengeleyiciyi test edin. 

Azure Load Balancer hakkında daha fazla bilgi edinmek için [Azure Load Balancer nedir?](load-balancer-overview.md) ve [sık sorulan sorular Load Balancer](load-balancer-faqs.md).

[Load Balancer ve kullanılabilirlik bölgeleri](load-balancer-standard-availability-zones.md)hakkında daha fazla bilgi edinin.
