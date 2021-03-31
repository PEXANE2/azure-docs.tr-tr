---
title: Redin için Azure önbelleğine çoğaltmalar ekleme (Önizleme)
description: Redsıs örnekleri için Premium katman Azure önbelleğinize daha fazla çoğaltma eklemeyi öğrenin
author: yegu-ms
ms.author: yegu
ms.service: cache
ms.topic: conceptual
ms.date: 08/11/2020
ms.openlocfilehash: 87b5ec5eb13f2bc53bdf993547ce3da1c74404bf
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "91566797"
---
# <a name="add-replicas-to-azure-cache-for-redis-preview"></a>Redin için Azure önbelleğine çoğaltmalar ekleme (Önizleme)
Bu makalede, Azure portal kullanarak ek yinelemelerle bir Azure önbellek örneği ayarlamayı öğreneceksiniz.

Redsıs standart ve Premium katmanları için Azure önbelleği, her bir önbelleği iki adanmış sanal makinede (VM) barındırarak artıklık sağlar. Bu VM 'Ler birincil ve çoğaltma olarak yapılandırılır. Birincil VM kullanılamaz hale geldiğinde, çoğaltma bunu algılar ve otomatik olarak yeni birincil olarak devralır. Artık bir Premium önbellekteki çoğaltmaların sayısını en fazla üçten artırabilirsiniz, böylece bir önbelleği destekleyen toplam dört sanal makine sağlarsınız. Birden çok çoğaltmanın, tek bir çoğaltmanın sağlayabilmesinden daha yüksek esnekliği.

> [!IMPORTANT]
> Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz [. Microsoft Azure önizlemeleri Için ek kullanım koşulları.](https://azure.microsoft.com/support/legal/preview-supplemental-terms/) 
> 

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/)

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
    | **DNS adı** | Genel olarak benzersiz bir ad girin. | Önbellek adı, yalnızca rakam, harf veya kısa çizgi içeren 1 ile 63 karakter arasında bir dize olmalıdır. Ad bir sayı veya harfle başlamalı ve bitmeli ve ardışık kısa çizgi içeremez. Önbellek örneğinizin *ana bilgisayar adı* *\<DNS name> . Redis.cache.Windows.net* olacaktır. | 
    | **Konum** | Bir konum seçin. | Önbelleğinizi kullanacak diğer hizmetlerin yakınında bir [bölge](https://azure.microsoft.com/regions/) seçin. |
    | **Önbellek türü** | [Premium katman](https://azure.microsoft.com/pricing/details/cache/) önbelleği seçin. |  Fiyatlandırma katmanı önbellek için kullanılabilen boyut, performans ve özellikleri belirler. Daha fazla bilgi için bkz. [redsıs Için Azure önbelleği 'Ne genel bakış](cache-overview.md). |
   
1. **Gelişmiş** sayfasında, **çoğaltma sayısı**' nı seçin.
   
    :::image type="content" source="media/cache-how-to-multi-replicas/create-multi-replicas.png" alt-text="Çoğaltma sayısı.":::

1. Diğer seçenekleri varsayılan ayarlarında bırakın. 

    > [!NOTE]
    > Çoklu çoğaltma desteği şu anda yalnızca kümelenmemiş önbelleklerle çalışır.
    >

1. **Oluştur**’a tıklayın.
   
    Önbelleğin oluşturulması biraz zaman alır. Redsıs **genel bakış** sayfasında ilerlemeyi izleyebilirsiniz. **Durum** **çalışıyor** olarak görüntülendiğinde, önbellek kullanıma hazırdır.

    > [!NOTE]
    > Bir önbellekteki çoğaltmaların sayısı, oluşturulduktan sonra değiştirilemez.
    >

## <a name="next-steps"></a>Sonraki Adımlar
Redsıs özellikleri için Azure önbelleği hakkında daha fazla bilgi edinin.

> [!div class="nextstepaction"]
> [Redsıs Premium hizmet katmanları için Azure önbelleği](cache-overview.md#service-tiers)
