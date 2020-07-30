---
title: Azure özel bağlantısı ile Redsıs için Azure önbelleği (Önizleme)
description: Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen, özel olarak ve güvenli bir şekilde Azure önbelleğine bağlayan bir ağ arabirimidir. Bu makalede, Azure portal kullanarak bir Azure önbelleği, Azure sanal ağı ve özel bir uç nokta oluşturmayı öğreneceksiniz.
author: curib
ms.author: cauribeg
ms.service: cache
ms.topic: conceptual
ms.date: 07/21/2020
ms.openlocfilehash: 5db756b60330cdac4e43e13bfe29d9397f87af50
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87421663"
---
# <a name="azure-cache-for-redis-with-azure-private-link-preview"></a>Azure özel bağlantısı ile Redsıs için Azure önbelleği (Önizleme)
Azure özel uç noktası, Azure özel bağlantısı tarafından desteklenen, özel olarak ve güvenli bir şekilde Azure önbelleğine bağlayan bir ağ arabirimidir. 

Bu makalede, Azure portal kullanarak bir Azure önbelleği, Azure sanal ağı ve özel bir uç nokta oluşturmayı öğreneceksiniz.  

> [!IMPORTANT]
> Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz [. Microsoft Azure önizlemeleri Için ek kullanım koşulları.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 
> 

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)

> [!NOTE]
> Bu özellik şu anda önizleme aşamasındadır. ilgilendiğiniz durumlarda [bizimle iletişim kurun](mailto:azurecache@microsoft.com) .
>

## <a name="create-a-cache"></a>Bir önbellek oluşturma
1. Önbellek oluşturmak için [Azure Portal](https://portal.azure.com) oturum açın ve **kaynak oluştur**' u seçin. 

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="Kaynak oluştur ' u seçin.":::
   
1. **Yeni** sayfada **veritabanları** ' nı seçin ve ardından **redsıs için Azure önbelleği**' ni seçin.

    :::image type="content" source="media/cache-private-link/2-select-cache.png" alt-text="Redsıs için Azure önbelleği ' ni seçin.":::
   
1. **Yeni Redis Cache** sayfasında, yeni önbelleğiniz için ayarları yapılandırın.
   
   | Ayar      | Önerilen değer  | Açıklama |
   | ------------ |  ------- | -------------------------------------------------- |
   | **DNS adı** | Genel olarak benzersiz bir ad girin. | Önbellek adı, yalnızca rakam, harf veya kısa çizgi içeren 1 ile 63 karakter arasında bir dize olmalıdır. Ad bir sayı veya harfle başlamalı ve bitmeli ve ardışık kısa çizgi içeremez. Önbellek örneğinizin *ana bilgisayar adı* * \<DNS name> . Redis.cache.Windows.net*olacaktır. | 
   | **Abonelik** | Açılır ve aboneliğinizi seçin. | Redsıs örneği için bu yeni Azure önbelleğinin oluşturulacağı abonelik. | 
   | **Kaynak grubu** | Açılır ve bir kaynak grubu seçin veya **Yeni oluştur** ' u seçin ve yeni bir kaynak grubu adı girin. | Önbelleğinizin ve diğer kaynaklarınızın oluşturulacağı kaynak grubunun adı. Tüm uygulama kaynaklarınızı tek bir kaynak grubuna yerleştirerek, bunları birlikte kolayca yönetebilir veya silebilirsiniz. | 
   | **Konum** | Açılır ve bir konum seçin. | Önbelleğinizi kullanacak diğer hizmetlerin yakınında bir [bölge](https://azure.microsoft.com/regions/) seçin. |
   | **Fiyatlandırma katmanı** | Açılır ve bir [fiyatlandırma katmanı](https://azure.microsoft.com/pricing/details/cache/)seçin. |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi için bkz. [redsıs Için Azure önbelleği 'Ne genel bakış](cache-overview.md). |
   
1. **Oluştur**’u seçin. 
   
    :::image type="content" source="media/cache-private-link/3-new-cache.png" alt-text="Redsıs için Azure önbelleği oluşturun.":::
   
   Önbelleğin oluşturulması biraz zaman alır. Redsıs **genel bakış** sayfasında ilerlemeyi izleyebilirsiniz. **Durum** **çalışıyor**olarak görüntülendiğinde, önbellek kullanıma hazırdır.
    
    :::image type="content" source="media/cache-private-link/4-status.png" alt-text="Redfor Redfor için Azure önbelleği oluşturulur.":::

## <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bu bölümde, bir sanal ağ ve alt ağ oluşturacaksınız.

1. **Kaynak oluştur**’u seçin.

    :::image type="content" source="media/cache-private-link/1-create-resource.png" alt-text="Kaynak oluştur ' u seçin.":::

2. **Yeni** sayfasında **ağ** ' ı seçin ve ardından **sanal ağ**' ı seçin.

    :::image type="content" source="media/cache-private-link/5-select-vnet.png" alt-text="Sanal bir ağ oluşturun.":::

3. **Sanal ağ oluştur**' da, **temel** bilgiler sekmesinde bu bilgileri girin veya seçin:

    | **Ayar**          | **Değer**                                                           |
    |------------------|-----------------------------------------------------------------|
    | **Proje ayrıntıları**  |                                                                 |
    | Abonelik     | Açılır ve aboneliğinizi seçin.                                  |
    | Kaynak Grubu   | Açılır ve bir kaynak grubu seçin. |
    | **Örnek ayrıntıları** |                                                                 |
    | Name             | Girmesini**\<virtual-network-name>**                                    |
    | Bölge           | Seçin**\<region-name>** |

4. **IP adresleri** sekmesini seçin veya sayfanın altındaki **Sonraki: IP adresleri** düğmesini seçin.

5. **IP adresleri** sekmesinde şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | IPv4 adres alanı | Girmesini**\<IPv4-address-space>** |

6. **Alt ağ adı**altında, **varsayılan**sözcük ' ı seçin.

7. **Alt ağı Düzenle**' de şu bilgileri girin:

    | Ayar            | Değer                      |
    |--------------------|----------------------------|
    | Alt ağ adı | Girmesini**\<subnet-name>** |
    | Alt ağ adres aralığı | Girmesini**\<subnet-address-range>**

8. **Kaydet**'i seçin.

9. **Gözden geçir + oluştur** sekmesini seçin ya da **gözden geçir + oluştur** düğmesini seçin.

10. **Oluştur**’u seçin.


## <a name="create-a-private-endpoint"></a>Özel uç nokta oluşturma 

Bu bölümde, özel bir uç nokta oluşturacak ve daha önce oluşturduğunuz önbelleğe bağlayacaksınız.

1. **Özel bağlantı** araması yapın ve ENTER tuşuna basın veya arama önerilerinde bunu seçin.

    :::image type="content" source="media/cache-private-link/7-create-private-link.png" alt-text="Özel bir bağlantı arayın.":::

2. Ekranın sol tarafında **Özel uç noktalar**' ı seçin.

    :::image type="content" source="media/cache-private-link/8-select-private-endpoint.png" alt-text="Özel bağlantıyı seçin.":::

3. Özel uç noktanızı oluşturmak için **+ Ekle** düğmesini seçin. 

    :::image type="content" source="media/cache-private-link/9-add-private-endpoint.png" alt-text="Özel bir bağlantı ekleyin.":::

4. **Özel uç nokta Oluştur sayfasında**, Özel uç noktanız için ayarları yapılandırın.

    | Ayar | Değer |
    | ------- | ----- |
    | **PROJE AYRıNTıLARı** | |
    | Abonelik | Açılır ve aboneliğinizi seçin. |
    | Kaynak grubu | Açılır ve bir kaynak grubu seçin. |
    | **ÖRNEK AYRıNTıLARı** |  |
    | Name |Özel uç noktanız için bir ad girin.  |
    | Bölge |Açılır ve bir konum seçin. |
    |||

5. Sayfanın alt kısmındaki **Sonraki: kaynak** düğmesini seçin.

6. **Kaynak** sekmesinde aboneliğinizi seçin, Microsoft. Cache/reddir olarak kaynak türünü seçin ve ardından önceki bölümde yaptığınız önbelleği seçin.

    :::image type="content" source="media/cache-private-link/10-resource-private-endpoint.png" alt-text="Özel bağlantı için kaynaklar.":::

7. Sayfanın alt kısmındaki **Sonraki: yapılandırma** düğmesini seçin.

8. **Yapılandırma** sekmesinde, önceki bölümde oluşturduğunuz sanal ağı ve alt ağı seçin.

    :::image type="content" source="media/cache-private-link/11-configuration-private-endpoint.png" alt-text="Özel bağlantı yapılandırması.":::

9. Sayfanın alt kısmındaki **Sonraki: Etiketler** düğmesini seçin.

10. Kaynağı sınıflandırmak istiyorsanız, **Etiketler** sekmesinde adı ve değeri girin. Bu adım isteğe bağlıdır.

    :::image type="content" source="media/cache-private-link/12-tags-private-endpoint.png" alt-text="Özel bağlantı için Etiketler.":::

11.  **Gözden geçir + oluştur**' u seçin. Azure 'un yapılandırmanızı doğruladığı, **gözden geçir + oluştur**sekmesine götürülürsünüz   .

12. Yeşil **doğrulama başarılı** Iletisi göründüğünde **Oluştur**' u seçin.


## <a name="next-steps"></a>Sonraki Adımlar

Özel bağlantı hakkında daha fazla bilgi edinmek için bkz. [Azure özel bağlantı belgeleri](https://docs.microsoft.com/azure/private-link/private-link-overview). 

