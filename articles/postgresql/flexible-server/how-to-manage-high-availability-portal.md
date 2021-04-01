---
title: Bölge yedekli yüksek kullanılabilirliği yönetme-Azure portal-PostgreSQL için Azure veritabanı-esnek sunucu
description: Bu makalede, Azure portal aracılığıyla PostgreSQL için Azure veritabanı-esnek sunucu 'da bölge yedekli yüksek kullanılabilirliğinin nasıl etkinleştirileceği veya devre dışı bırakılacağı açıklanır.
author: sr-msft
ms.author: srranga
ms.service: postgresql
ms.topic: how-to
ms.date: 09/22/2020
ms.openlocfilehash: fc1bca1265139a438fad86bfce770026866d9a2f
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "90941019"
---
# <a name="manage-zone-redundant-high-availability-in-flexible-server"></a>Esnek sunucuda bölge yedekli yüksek kullanılabilirliği yönetme

> [!IMPORTANT]
> PostgreSQL için Azure veritabanı-esnek sunucu önizlemededir

Bu makalede, esnek sunucunuzda bölge yedekli yüksek kullanılabilirlik yapılandırmasını nasıl etkinleştirebileceğinizi veya devre dışı bırakabileceğinizi açıklanmaktadır.

Yüksek kullanılabilirlik özelliği, farklı bölgelerde fiziksel ve bekleme çoğaltmasını fiziksel olarak ayırır. Daha fazla ayrıntı için bkz. [yüksek kullanılabilirlik kavramları belgeleri](./concepts-high-availability.md). Esnek sunucu oluşturma sırasında veya oluşturulduktan sonra yüksek kullanılabilirliği etkinleştirmeyi seçebilirsiniz. Bu sayfa, yüksek kullanılabilirliği nasıl etkinleştirebilirim veya devre dışı bırakabilmeniz için yönergeler sağlar. Bu işlem VNET yapılandırması, güvenlik duvarı ayarları ve yedekleme saklama dahil diğer ayarlarınızı değiştirmez. Benzer şekilde, yüksek kullanılabilirliği etkinleştirmek ve devre dışı bırakmak, çevrimiçi bir işlemdir ve uygulamanızın bağlantı ve işlemlerini etkilemez.

## <a name="pre-requisites"></a>Ön koşullar

Bölgesel olarak yedekli yüksek kullanılabilirlik yalnızca birden fazla bölgenin desteklendiği bölgelerde kullanılabilir. 

## <a name="enable-high-availability-during-server-creation"></a>Sunucu oluşturma sırasında yüksek kullanılabilirliği etkinleştir

Bu bölüm, özellikle de HA ile ilgili alanlar için ayrıntılar sağlar. Esnek sunucunuzu oluştururken yüksek kullanılabilirlik dağıtmak için bu adımları izleyebilirsiniz.

1.  [Azure Portal](https://portal.azure.com/)esnek sunucu ' yı seçin ve Oluştur ' a tıklayın.  **Abonelik**, **kaynak grubu**, **sunucu adı**, **bölge** ve diğer alanlar gibi ayrıntıların nasıl doldurulması gerektiği hakkında ayrıntılı bilgi için bkz. sunucu oluşturma için nasıl yapılır belgeleri.
   
    :::image type="content" source="./media/how-to-manage-high-availability-portal/subscription-region.png" alt-text="Aboneliği ve bölgeyi görüntüle":::

2.  **Kullanılabilirlik** bölgenizi seçin. Bu, gecikme süresini azaltmak için uygulamanızı veritabanı ile aynı Kullanılabilirlik bölgesinde birlikte konumlandırmak istiyorsanız yararlıdır. Esnek sunucunun herhangi bir kullanılabilirlik alanına dağıtılmasını istiyorsanız **tercih yok** ' ı seçin.
    ![AZ seçim ]() :::image type="content" source="./media/how-to-manage-high-availability-portal/zone-selection.png" alt-text="kullanılabilirlik alanı seçimi":::  

3.  Kullanılabilirlik seçeneğinde **bölge yedekli yüksek kullanılabilirlik** onay kutusuna tıklayın.

    :::image type="content" source="./media/how-to-manage-high-availability-portal/high-availability-checkbox.png" alt-text="Yüksek kullanılabilirlik onay kutusu":::

4.  Varsayılan işlem ve depolamayı değiştirmek istiyorsanız,  **sunucuyu Yapılandır**' a tıklayın.
 
    :::image type="content" source="./media/how-to-manage-high-availability-portal/configure-server.png" alt-text="sunucu-işlem + depolamayı yapılandırma":::  

5.  Yüksek kullanılabilirlik seçeneği işaretliyse, Burstable katman seçim için kullanılamaz. **Genel amaçlı** ya da **bellek için iyileştirilmiş** işlem katmanları seçebilirsiniz. Ardından, açılır menüden istediğiniz **işlem boyutunu** seçebilirsiniz.

    :::image type="content" source="./media/how-to-manage-high-availability-portal/select-compute.png" alt-text="İşlem Katmanı seçimi":::  


6.  Kayan çubuğu kullanarak GiB 'de **depolama boyutu** ' nu seçin ve **yedekleme saklama süresini** 7 gün ile 35 gün arasında seçin.
   
    :::image type="content" source="./media/how-to-manage-high-availability-portal/storage-backup.png" alt-text="Depolama yedekleme"::: 

7. **Kaydet**’e tıklayın. 

## <a name="enable-high-availability-post-server-creation"></a>Yüksek kullanılabilirlik sonrası sunucu oluşturmayı etkinleştir

Mevcut esnek sunucunuz için yüksek kullanılabilirliği etkinleştirmek üzere aşağıdaki adımları izleyin.

1.  [Azure Portal](https://portal.azure.com/), mevcut PostgreSQL esnek sunucunuzu seçin.

2.  Esnek sunucu sayfasında, yüksek kullanılabilirlik sayfasını açmak için sol panelde **yüksek kullanılabilirlik** ' e tıklayın.
   
     :::image type="content" source="./media/how-to-manage-high-availability-portal/high-availability-left-panel.png" alt-text="Sol panel seçimi"::: 

3.  Seçeneği **etkinleştirmek** için **bölge yedekli yüksek kullanılabilirlik** onay kutusuna tıklayın ve değişikliği kaydetmek için **Kaydet** ' e tıklayın.

     :::image type="content" source="./media/how-to-manage-high-availability-portal/enable-high-availability.png" alt-text="Yüksek kullanılabilirliği etkinleştir"::: 

4.  Bir onay iletişim kutusu, yüksek kullanılabilirliği etkinleştirerek, ek sunucu ve depolama dağıtımı nedeniyle maliyetlerinizin arttırabileceğini gösterir.

5.  Yüksek kullanılabilirliği etkinleştirmek için **ha düğmesini etkinleştir** ' e tıklayın.

6.  Yüksek kullanılabilirlik dağıtımının devam ettiğini belirten bir bildirim gösterilir.

## <a name="disable-high-availability"></a>Yüksek kullanılabilirliği devre dışı bırak

Zaten bölge yedekliği ile yapılandırılmış esnek sunucunuz için yüksek kullanılabilirliği devre dışı bırakmak üzere bu adımları izleyin.

1.  [Azure Portal](https://portal.azure.com/), mevcut PostgreSQL Için Azure veritabanı-esnek sunucu ' yı seçin.

2.  Esnek sunucu sayfasında, yüksek kullanılabilirlik sayfasını açmak için ön panelinden **yüksek kullanılabilirlik** ' e tıklayın.
   
    :::image type="content" source="./media/how-to-manage-high-availability-portal/high-availability-left-panel.png" alt-text="Sol panel seçimi"::: 

3.  Seçeneği **devre dışı bırakmak** için **bölge yedekli yüksek kullanılabilirlik** onay kutusuna tıklayın. Sonra değişikliği kaydetmek için **Kaydet** ' e tıklayın.

     :::image type="content" source="./media/how-to-manage-high-availability-portal/disable-high-availability.png" alt-text="Yüksek kullanılabilirliği devre dışı bırak"::: 

4.  Yüksek kullanılabilirliği devre dışı bırakmayı doğrulayabileceğiniz bir onay iletişim kutusu gösterilir.

5.  Yüksek kullanılabilirliği devre dışı bırakmak için **ha düğmesini devre dışı bırak** seçeneğine tıklayın.

6.  Bir bildirim, yüksek kullanılabilirlik dağıtımının açığa sürme sürecini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

-   [İş sürekliliği](./concepts-business-continuity.md) hakkında bilgi edinin
-   [Bölge yedekli yüksek kullanılabilirlik](./concepts-high-availability.md) hakkında bilgi edinin
