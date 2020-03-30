---
title: Quickstart - Azure portalını kullanarak Özel Bağlantı hizmeti oluşturma
titlesuffix: Azure Private Link
description: Bu hızlı başlangıçta Azure portalını kullanarak Nasıl Özel Bağlantı hizmeti oluşturabilirsiniz öğrenin
services: private-link
author: malopMSFT
ms.service: private-link
ms.topic: quickstart
ms.date: 02/03/2020
ms.author: allensu
ms.openlocfilehash: f21b440ee0e2c53d9824300e85b683629c1575da
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78252540"
---
# <a name="quickstart-create-a-private-link-service-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portalını kullanarak Özel Bağlantı hizmeti oluşturma

Azure Özel Bağlantı hizmeti, Private Link tarafından yönetilen kendi hizmetinizin ifade ettiği anlamına gelir. Azure Standart Yük Dengeleyicisi'nin arkasında çalışan hizmete veya kaynağa Private Link erişimi verebilirsiniz. Hizmetinizin tüketicileri hizmete kendi sanal ağlarından özel olarak erişebilirler. Bu hızlı başlangıçta, Azure portalını kullanarak Bir Özel Bağlantı hizmeti oluşturmayı öğrenirsiniz.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-internal-load-balancer"></a>İç yük dengeleyici oluşturma

İlk olarak, sanal bir ağ oluşturun. Ardından, Özel Bağlantı hizmetinde kullanmak üzere bir dahili yük dengeleyicioluşturun.

## <a name="virtual-network-and-parameters"></a>Sanal ağ ve parametreler

Bu bölümde, bir sanal ağ oluşturursunuz. Ayrıca, Özel Bağlantı hizmetinize erişen yük bakiyesini barındırmak için alt ağı oluşturursunuz.

Bu bölümde aşağıdaki bilgileri ile adımlarda aşağıdaki parametreleri değiştirmeniz gerekir:

| Parametre                   | Değer                |
|-----------------------------|----------------------|
| **\<kaynak-grup adı>**  | myResourceGroupLB |
| **\<sanal ağ adı>** | myVNet          |
| **\<bölge adı>**          | Doğu ABD 2      |
| **\<IPv4 adres-boşluk>**   | 10.3.0.0\16          |
| **\<subnet-name>**          | myBackendSubnet        |
| **\<alt net-adres aralığı>** | 10.3.0.0\24          |

[!INCLUDE [virtual-networks-create-new](../../includes/virtual-networks-create-new.md)]

### <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyici oluşturma

Standart bir dahili yük dengeleyicisi oluşturmak için portalı kullanın. Belirttiğiniz ad ve IP adresi otomatik olarak yük bakiyesinin ön ucu olarak yapılandırılır.

1. Portalın sol üst tarafında, kaynak > **Ağ** > **Yük Dengeleyicisi** **Oluştur'u**seçin.

1. **Yük bakiyesi oluştur** sayfasının **Temeller** sekmesine aşağıdaki bilgileri girin veya seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | **Abonelik**               | Aboneliğinizi seçin.    |
    | **Kaynak grubu**         | Kutudan **myResourceGroupLB'yi** seçin.|
    | **Adı**                   | **myLoadBalancer**girin.                                   |
    | **Bölge**         | **Doğu ABD 2**’yi seçin.                                        |
    | **Tür**          | **Dahili'yi**seçin.                                        |
    | **Sku**           | **Standart'ı**seçin.                          |
    | **Sanal ağ**           | **myVNet'i**seçin.                          |
    | **IP adresi ataması**              | **Statik**’i seçin.   |
    | **Özel IP adresi**|Sanal ağınızın ve alt ağınızın adres alanına bir adres girin. Örnek 10.3.0.7'dir.  |

1. Kalan ayarlar için varsayılanları kabul edin ve ardından **Gözden Geçir + oluştur'u** seçin

1. Gözden **Geçir + oluştur** sekmesinde **Oluştur'u**seçin.

### <a name="create-standard-load-balancer-resources"></a>Standart yük dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracaksınız. Ayrıca yük dengeleyici kurallarını da belirtirsiniz.

#### <a name="create-a-back-end-pool"></a>Arka uç havuzu oluşturma

Arka uç adres havuzu, yük dengeleyicisine bağlı sanal NIC'lerin IP adreslerini içerir. Bu havuz, kaynaklarınıza trafik dağıtmanızı sağlar. Trafiği yükleyen kaynakları içerecek şekilde **myBackendPool** adlı arka uç adres havuzunu oluşturun.

1. En sol menüden **Tüm Hizmetler'i** seçin.
1. **Tüm kaynakları**seçin ve ardından kaynaklar listesinden **myLoadBalancer'ı** seçin.
1. **Ayarlar** bölümünde **Arka uç havuzları**’nı ve sonra **Ekle**’yi seçin.
1. Arka **uç havuzu** ekle sayfasında, arka uç havuzunuzun adı olarak **myBackendPool'u** girin ve sonra **Ekle'yi**seçin.

#### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyicisinin kaynak durumunu izlemesine izin vermek için bir sistem durumu sondası kullanın. Sistem durumu denetimlerine kaynak yanıtına bağlı olarak, sistem durumu sondası kaynakları dinamik olarak yükler dengeleyici döndürmeden ekler veya kaldırır.

Kaynakların durumunu izlemek için bir sistem durumu sondası oluşturmak için:

1. En sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **myLoadBalancer'ı** seçin.

1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.

1. Sistem **durumu sondası** ekle sayfasında aşağıdaki değerleri girin veya seçin:

   - **Adı**: **myHealthProbe**girin .
   - **Protokol**: **TCP'yi**seçin.
   - **Bağlantı Noktası**: **80**girin .
   - **Aralık**: **15**girin . Bu değer, sonda denemeleri arasındaki saniye sayısıdır.
   - **Sağlıksız eşik**: **2**girin . Bu değer, sanal bir makine sağlıksız olarak kabul edilmeden önce oluşan ardışık sonda hatalarının sayısıdır.

1. **Tamam'ı**seçin.

#### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı, trafiğin kaynaklara nasıl dağıtıldığını tanımlar. Kural tanımlar:

- Gelen trafik için ön uç IP yapılandırması.
- Trafiği almak için arka uç IP havuzu.
- Gerekli kaynak ve hedef bağlantı noktaları.

**myLoadBalancerRule** adlı yük dengeleyici kuralı **LoadBalancerFrontEnd** ön ucundaki port 80'i dinler. Kural, ağ trafiğini aynı bağlantı noktası 80'deki **myBackendPool** arka uç adres havuzuna gönderir.

Yük dengeleyici kuralı oluşturmak için:

1. En sol menüdeki **tüm kaynakları** seçin ve kaynak listesinden **myLoadBalancer'ı** seçin.

1. **Ayarlar** **altında, Yük dengeleme kurallarını**seçin ve sonra **Ekle'yi**seçin.

1. Yük **dengeleme kuralı** ekle sayfasında, zaten mevcut değillerse aşağıdaki değerleri girin veya seçin:

   - **Adı**: **myLoadBalancerRule**girin.
   - **Frontend IP adresi:** **LoadBalancerFrontEnd'i**girin.
   - **Protokol**: **TCP'yi**seçin.
   - **Bağlantı Noktası**: **80**girin .
   - **Arka uç bağlantı noktası**: **80**girin .
   - **Arka uç havuzu**: **myBackendPool'u**seçin.
   - **Sağlık sondası**: **myHealthProbe'u**seçin. 

1. **Tamam'ı**seçin.

## <a name="create-a-private-link-service"></a>Özel Bağlantı hizmeti oluşturma

Bu bölümde, standart yük dengeleyicisi arkasında bir Özel Bağlantı hizmeti oluşturursunuz.

1. Azure portalındaki sayfanın sol üst kısmında, **Kaynak** > **Oluştur** > **Özel Bağlantı Merkezi (Önizleme)** seçeneğini belirleyin. Özel Bağlantı'yı aramak için portalın arama kutusunu da kullanabilirsiniz.

1. **Özel Bağlantı Merkezi'** > nde - Genel Bakış**Başkalarının bağlanabilmesi için kendi hizmetinizi ortaya çıkar,** **Başlat'ı**seçin.

1. **Özel bağlantı hizmeti oluştur altında - Temel bilgiler,** bu bilgileri girin veya seçin:

    | Ayar           | Değer                                                                        |
    |-------------------|------------------------------------------------------------------------------|
    | Proje ayrıntıları:  |                                                                              |
    | **Abonelik**      | Aboneliğinizi seçin.                                                     |
    | **Kaynak Grubu**    | **MyResourceGroupLB'umu**seçin.                                                    |
    | Örnek ayrıntılar: |                                                                              |
    | **Adı**              | **myPrivateLinkService**girin. |
    | **Bölge**            | **Doğu ABD 2**’yi seçin.                                                        |

1. **Sonraki'ni seçin: Giden ayarlar.**

1. **Özel bağlantı hizmeti oluşturma altında - Giden ayarlar**, bu bilgileri girin veya seçin:

    | Ayar                           | Değer                                                                           |
    |-----------------------------------|---------------------------------------------------------------------------------|
    | **Load Balancer**                     | **myLoadBalancer'ı**seçin.                                                           |
    | **Yük Dengeleyici frontend IP adresi** | **myLoadBalancer'ın**ön uç IP adresini seçin.                                |
    | **Kaynak NAT Sanal ağ**        | **myVNet'i**seçin.                                                                   |
    | **Kaynak NAT alt ağı**                 | **myBackendSubnet'i**seçin.                                                          |
    | **TCP proxy v2'yi etkinleştirme**               | Uygulamanızın TCP proxy v2 üstbilgisi bekleyip beklemediğine bağlı olarak **EVET** veya **HAYIR'ı** seçin. |
    | **Özel IP adresi ayarları**       | Her NAT IP'si için ayırma yöntemini ve IP adresini yapılandırın.                  |

1. **Sonraki'ni seçin: Güvenliğe erişin.**

1. **Özel bağlantı hizmeti oluşturma altında - Access security**, **Görünürlük**seçin ve sonra **yalnızca Rol Tabanlı erişim denetimi**seçin.
  
1. **İleri: EtiketlerI** > **Gözden Geçir + oluştur** veya sayfanın üst kısmındaki Gözden Geçirme + **oluştur** sekmesini seçin' i seçin.

1. Bilgilerinizi gözden geçirin ve **Oluştur'u**seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel Bağlantı hizmetini kullanmayı bitirdiğinizde, bu hızlı başlatmada kullanılan kaynakları temizlemek için kaynak grubunu silin.

1. Portalın üst kısmındaki arama kutusuna **myResourceGroupLB'yi** girin ve arama sonuçlarından **myResourceGroupLB'yi** seçin.
1. **Kaynak grubunu sil**'i seçin.
1. **KAYNAK GRUBU Ad IN TYPE'da,** **myResourceGroup'a**girin.
1. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, dahili bir Azure yük dengeleyicisi ve Özel Bağlantı hizmeti oluşturdunuz. [Azure portalını kullanarak özel bir bitiş noktası oluşturmayı](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)da öğrenebilirsiniz.
