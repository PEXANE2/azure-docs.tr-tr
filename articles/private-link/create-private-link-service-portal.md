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
ms.openlocfilehash: f62adbaea8d6549af0137f49542ee89e7531b9ef
ms.sourcegitcommit: b95983c3735233d2163ef2a81d19a67376bfaf15
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/11/2020
ms.locfileid: "77136162"
---
# <a name="quickstart-create-a-private-link-service-by-using-the-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak özel bir bağlantı hizmeti oluşturma

Bir Azure özel bağlantı hizmeti, özel bağlantı tarafından yönetilen kendi hizmetinize başvurur. Azure Load Balancer arkasında çalışan hizmet veya kaynağa özel bağlantı erişimi verebilirsiniz. Hizmetinizin tüketicileri, kendi sanal ağlarından özel olarak erişebilir. Bu hızlı başlangıçta, Azure portal kullanarak özel bir bağlantı hizmeti oluşturmayı öğreneceksiniz.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-an-internal-load-balancer"></a>İç yük dengeleyici oluşturma

İlk olarak, bir sanal ağ oluşturun. Sonra, özel bağlantı hizmeti ile kullanmak için bir iç yük dengeleyici oluşturun.

### <a name="create-the-virtual-network"></a>Sanal ağı oluşturma

Bu bölümde, bir sanal ağ oluşturursunuz. Ayrıca, özel bağlantı hizmetinize erişen yük dengeleyiciyi barındırmak için alt ağ oluşturursunuz.

1. Portalın sol üst kısmında **bir kaynak oluştur** > **ağ** > **sanal ağ**' ı seçin.

1. **Sanal ağ oluştur** bölmesinde şu değerleri girin veya seçin:

   - **Ad**: **myvnet**girin.
   - **ResourceGroup**: **Yeni oluştur**' u seçin, **Myresourcegrouplb**yazın ve **Tamam**' ı seçin.
   - **Alt ağ** > **adı**: **mybackendsubnet**girin.

1. **Oluştur**'u seçin.

   ![Sanal ağ oluşturma](../load-balancer/media/tutorial-load-balancer-basic-internal-portal/2-load-balancer-virtual-network.png)

### <a name="create-a-standard-load-balancer"></a>Standart yük dengeleyici oluşturma

Standart bir iç yük dengeleyici oluşturmak için portalını kullanın. Belirttiğiniz ad ve IP adresi, yük dengeleyicinin ön ucu olarak otomatik olarak yapılandırılır.

1. Portalın sol üst kısmında **Kaynak oluştur** > **Ağ** > **Yük Dengeleyici** seçeneğini belirleyin.

1. **Yük dengeleyici oluştur** sayfasının **temel bilgiler** sekmesinde, aşağıdaki bilgileri girin veya seçin:

    | Ayar                 | Değer                                              |
    | ---                     | ---                                                |
    | **Abonelik**               | Aboneliğinizi seçin.    |
    | **Kaynak grubu**         | Kutudan **Myresourcegrouplb** ' ı seçin.|
    | **Ad**                   | **Myloadbalancer**girin.                                   |
    | **Bölge**         | **Doğu ABD 2**’yi seçin.                                        |
    | **Tür**          | **Dahili**' ı seçin.                                        |
    | **SKU**           | **Standart**' ı seçin.                          |
    | **Sanal ağ**           | **Myvnet**' i seçin.                          |
    | **IP adresi ataması**              | **Statik**öğesini seçin.   |
    | **Özel IP adresi**|Sanal ağınızın ve alt ağınızın adres alanında bir adres girin. Örnek olarak 10.3.0.7.  |

1. Kalan ayarlar için varsayılan değerleri kabul edin ve ardından **gözden geçir + oluştur** ' u seçin.

1. **Gözden geçir + oluştur** sekmesinde **Oluştur**' u seçin.

### <a name="create-standard-load-balancer-resources"></a>Standart yük dengeleyici kaynakları oluşturma

Bu bölümde, arka uç adres havuzu ve durum araştırması için yük dengeleyici ayarlarını yapılandıracaksınız. Yük dengeleyici kurallarını da belirtirsiniz.

#### <a name="create-a-back-end-pool"></a>Arka uç havuzu oluşturma

Bir arka uç adres havuzu, yük dengeleyiciye bağlı sanal NIC 'lerin IP adreslerini içerir. Bu havuz, kaynaklarınıza trafik dağıtmanızı sağlar. Yük Dengeleme trafiğini içeren kaynakları dahil etmek için **Mybackendpool** adlı arka uç adres havuzunu oluşturun.

1. En soldaki menüden **tüm hizmetler** ' i seçin.
1. **Tüm kaynaklar**' ı seçin ve ardından kaynaklar listesinden **myloadbalancer** ' ı seçin.
1. **Ayarlar** bölümünde **Arka uç havuzları**’nı ve sonra **Ekle**’yi seçin.
1. **Arka uç Havuzu Ekle** sayfasında, arka uç havuzunuzun adı olarak **mybackendpool** girin ve ardından **Ekle**' yi seçin.

#### <a name="create-a-health-probe"></a>Durum araştırması oluşturma

Yük dengeleyici 'nin kaynak durumunu izlemesine izin vermek için bir sistem durumu araştırması kullanın. Sistem durumu denetimlerine yönelik kaynak yanıtına bağlı olarak, sistem durumu araştırması yük dengeleyici rotasyondan kaynakları dinamik olarak ekler veya kaldırır.

Kaynakların sistem durumunu izlemek üzere bir sistem durumu araştırması oluşturmak için:

1. En soldaki menüdeki **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.

1. **Ayarlar** bölümünde **Durum araştırmaları**’nı ve sonra **Ekle**’yi seçin.

1. **Durum araştırması Ekle** sayfasında, aşağıdaki değerleri girin veya seçin:

   - **Ad**: **myhealtharaştırması**girin.
   - **Protokol**: **TCP**' yi seçin.
   - **Bağlantı noktası**: **80**girin.
   - **Aralık**: **15**girin. Bu değer, araştırma denemeleri arasındaki saniye sayısıdır.
   - **Sağlıksız eşik**: **2**girin. Bu değer, bir sanal makinenin sağlıksız olduğu kabul edilmeden önce ortaya çıkan arka arkaya araştırma hatalarının sayısıdır.

1. **Tamam**’ı seçin.

#### <a name="create-a-load-balancer-rule"></a>Yük dengeleyici kuralı oluşturma

Yük dengeleyici kuralı trafiğin kaynaklara nasıl dağıtıldığını tanımlar. Kural şunları tanımlar:

- Gelen trafik için ön uç IP yapılandırması.
- Trafiği almak için arka uç IP Havuzu.
- Gerekli kaynak ve hedef bağlantı noktaları.

**Myloadbalancerrule** adlı yük dengeleyici kuralı **loadbalancerön** uç ön ucundaki 80 numaralı bağlantı noktasını dinler. Kural, aynı bağlantı noktası 80 üzerindeki **Mybackendpool** arka uç adres havuzuna ağ trafiği gönderir.

Yük dengeleyici kuralı oluşturmak için:

1. En soldaki menüdeki **tüm kaynaklar** ' ı seçin ve ardından kaynak listesinden **myloadbalancer** ' yi seçin.

1. **Ayarlar**bölümünde **Yük Dengeleme kuralları**' nı ve ardından **Ekle**' yi seçin.

1. **Yük Dengeleme kuralı ekle** sayfasında, zaten mevcut değilse aşağıdaki değerleri girin veya seçin:

   - **Ad**: **myloadbalancerrule**yazın.
   - **Ön uç IP adresi:** **Loadbalancerön uç**girin.
   - **Protokol**: **TCP**' yi seçin.
   - **Bağlantı noktası**: **80**girin.
   - **Arka uç bağlantı noktası**: **80**girin.
   - **Arka uç havuzu**: **mybackendpool**' u seçin.
   - **Durum araştırması**: **myhealtharaştırması**' ni seçin. 

1. **Tamam**’ı seçin.

## <a name="create-a-private-link-service"></a>Özel Bağlantı hizmeti oluşturma

Bu bölümde, standart yük dengeleyici arkasında bir özel bağlantı hizmeti oluşturursunuz.

1. Azure portal sayfanın sol üst kısmında, **bir kaynak oluştur** > **ağ** > **özel bağlantı merkezi (Önizleme)** seçeneğini belirleyin. Ayrıca, özel bağlantı aramak için portalın arama kutusunu da kullanabilirsiniz.

1. **Özel bağlantı merkezi 'Nde genel bakış** > **başkalarının bağlanabilmesi Için kendi hizmetinizi kullanıma sunabilmeniz için** **Başlat**' ı seçin.

1. **Özel bağlantı hizmeti oluşturma-temel bilgiler**altında bu bilgileri girin veya seçin:

    | Ayar           | Değer                                                                        |
    |-------------------|------------------------------------------------------------------------------|
    | Proje ayrıntıları:  |                                                                              |
    | **Abonelik**      | Aboneliğinizi seçin.                                                     |
    | **Kaynak Grubu**    | **Myresourcegrouplb**öğesini seçin.                                                    |
    | Örnek ayrıntıları: |                                                                              |
    | **Ad**              | **Myprivatelinkservice**yazın. |
    | **Bölge**            | **Doğu ABD 2**’yi seçin.                                                        |

1. **İleri ' yi seçin: giden ayarları**.

1. **Özel bağlantı hizmeti oluştur-giden ayarları**altında bu bilgileri girin veya seçin:

    | Ayar                           | Değer                                                                           |
    |-----------------------------------|---------------------------------------------------------------------------------|
    | **Yük Dengeleyici**                     | **Myloadbalancer**öğesini seçin.                                                           |
    | **Load Balancer ön uç IP adresi** | **Myloadbalancer**ön uç IP adresini seçin.                                |
    | **Kaynak NAT sanal ağı**        | **Myvnet**' i seçin.                                                                   |
    | **Kaynak NAT alt ağı**                 | **Mybackendsubnet**öğesini seçin.                                                          |
    | **TCP proxy v2 'yi etkinleştir**               | Uygulamanızın bir TCP proxy v2 üstbilgisini beklediğine bağlı olarak **Evet** ' i veya **Hayır** ' ı seçin. |
    | **Özel IP adresi ayarları**       | Her NAT IP 'si için ayırma yöntemini ve IP adresini yapılandırın.                  |

1. **İleri ' yi seçin: erişim güvenliği**.

1. **Özel bağlantı hizmeti oluşturma-erişim güvenliği**altında **görünürlük**' i seçin ve ardından **yalnızca rol tabanlı erişim denetimi**' ni seçin.
  
1. Ileri ' **yi seçin: etiketler** > **gözden geçir + oluştur** ' u seçin ya da sayfanın üst kısmındaki **gözden geçir + oluştur** sekmesini seçin.

1. Bilgilerinizi gözden geçirin ve **Oluştur**' u seçin.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Özel bağlantı hizmetini kullanarak işiniz bittiğinde, bu hızlı başlangıçta kullanılan kaynakları temizlemek için kaynak grubunu silin.

1. Portalın üst kısmındaki arama kutusuna **myresourcegrouplb** yazın ve arama sonuçlarından **myresourcegrouplb** ' ı seçin.
1. **Kaynak grubunu sil**'i seçin.
1. **Kaynak grubu adını yazın**alanına **myresourcegroup**girin.
1. **Sil**’i seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir iç Azure yük dengeleyici ve bir özel bağlantı hizmeti oluşturdunuz. Ayrıca [, Azure Portal kullanarak özel uç nokta oluşturmayı](https://docs.microsoft.com/azure/private-link/create-private-endpoint-portal)da öğrenebilirsiniz.
