---
title: Redsıs için Azure önbelleği için bölge yedekliliği etkinleştirme (Önizleme)
description: Redsıs örnekleri için Premium katmanınız için Azure önbelleğiniz için bölge yedekliliği ayarlamayı öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 33c346fa2e4572799ad6341bd5115cdd6e3b9ec9
ms.sourcegitcommit: 829d951d5c90442a38012daaf77e86046018e5b9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/09/2020
ms.locfileid: "91569987"
---
# <a name="enable-zone-redundancy-for-azure-cache-for-redis-preview"></a>Redsıs için Azure önbelleği için bölge yedekliliği etkinleştirme (Önizleme)
Bu makalede, Azure portal kullanarak bölgesel olarak yedekli bir Azure önbellek örneğini yapılandırmayı öğreneceksiniz.

Redsıs Standard ve Premium katmanları için Azure önbelleği, her bir önbelleği iki adanmış sanal makinede (VM) barındırarak yerleşik artıklık sağlar. Bu VM 'Ler ayrı [Azure hata ve güncelleştirme etki alanları](/azure/virtual-machines/windows/manage-availability) ve yüksek oranda kullanılabilir olmasına rağmen, veri merkezi düzeyindeki hatalara açıktır. Redde için Azure önbelleği, kendi Premium katmanında bölge yedekliliği destekler. Bölgesel olarak yedekli bir önbellek, birden fazla [kullanılabilirlik](/azure/virtual-machines/windows/manage-availability#use-availability-zones-to-protect-from-datacenter-level-failures)alanına yayılan sanal makinelerde çalışır. Daha yüksek esnekliği ve kullanılabilirlik sağlar.

> [!IMPORTANT]
> Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz [. Microsoft Azure önizlemeleri Için ek kullanım koşulları.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 
> 

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)

> [!NOTE]
> Bu özellik şu anda önizleme aşamasındadır. ilgilendiğiniz durumlarda [bizimle iletişim kurun](mailto:azurecache@microsoft.com) .
>

## <a name="create-a-cache"></a>Bir önbellek oluşturma
Önbellek oluşturmak için aşağıdaki adımları izleyin:

1. [Azure Portal](https://portal.azure.com) oturum açın ve **kaynak oluştur**' u seçin.
  
1. **Yeni** sayfada **veritabanları** ' nı seçin ve ardından **redsıs için Azure önbelleği**' ni seçin.

    :::image type="content" source="media/cache-create/new-cache-menu.png" alt-text="Redsıs için Azure önbelleği ' ni seçin.":::
   
1. **Temel bilgiler** sayfasında, yeni önbelleğiniz için ayarları yapılandırın.
   
    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Abonelik** | Aboneliğinizi seçin. | Redsıs örneği için bu yeni Azure önbelleğinin oluşturulacağı abonelik. | 
    | **Kaynak grubu** | Bir kaynak grubu seçin veya **Yeni oluştur** ' u seçin ve yeni bir kaynak grubu adı girin. | Önbelleğinizin ve diğer kaynaklarınızın oluşturulacağı kaynak grubunun adı. Tüm uygulama kaynaklarınızı tek bir kaynak grubuna yerleştirerek, bunları birlikte kolayca yönetebilir veya silebilirsiniz. | 
    | **DNS adı** | Genel olarak benzersiz bir ad girin. | Önbellek adı, yalnızca rakam, harf veya kısa çizgi içeren 1 ile 63 karakter arasında bir dize olmalıdır. Ad bir sayı veya harfle başlamalı ve bitmeli ve ardışık kısa çizgi içeremez. Önbellek örneğinizin *ana bilgisayar adı* * \<DNS name> . Redis.cache.Windows.net*olacaktır. | 
    | **Konum** | Bir konum seçin. | Önbelleğinizi kullanacak diğer hizmetlerin yakınında bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **Önbellek türü** | [Premium katman](https://azure.microsoft.com/pricing/details/cache/) önbelleği seçin. |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi için bkz. [redsıs Için Azure önbelleği 'Ne genel bakış](cache-overview.md). |
   
1. **Gelişmiş** sayfasında, **çoğaltma sayısı**' nı seçin.
   
    :::image type="content" source="media/cache-how-to-multi-replicas/create-multi-replicas.png" alt-text="Redsıs için Azure önbelleği ' ni seçin.":::

1. **Kullanılabilirlik alanları**' nı seçin. 
   
    :::image type="content" source="media/cache-how-to-zone-redundancy/create-zones.png" alt-text="Redsıs için Azure önbelleği ' ni seçin.":::

1. Diğer seçenekleri varsayılan ayarlarında bırakın. 

    > [!NOTE]
    > Bölge artıklığı desteği, şu anda kümelenmemiş ve coğrafi olmayan çoğaltmasız önbelleklerle çalışır. Ayrıca, özel bağlantıyı, ölçeklendirmeyi, veri kalıcılığını veya içeri/dışarı aktarmayı desteklemez.
    >

1. **Oluştur**’a tıklayın. 
   
    Önbelleğin oluşturulması biraz zaman alır. Redsıs **genel bakış** sayfasında ilerlemeyi izleyebilirsiniz. **Durum** **çalışıyor**olarak görüntülendiğinde, önbellek kullanıma hazırdır.
   
    > [!NOTE]
    > Kullanılabilirlik alanları, bir önbellek oluşturulduktan sonra değiştirilemez.
    >

## <a name="next-steps"></a>Sonraki Adımlar
Redsıs özellikleri için Azure önbelleği hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Redsıs Premium hizmet katmanları için Azure önbelleği](cache-overview.md#service-tiers)
