---
title: Bölge yedekli yüksek kullanılabilirliği yönetme-Azure portal-MySQL için Azure veritabanı esnek sunucu
description: Bu makalede, Azure portal aracılığıyla MySQL için Azure veritabanı esnek sunucusu 'nda bölge yedekli yüksek kullanılabilirliğinin nasıl etkinleştirileceği veya devre dışı bırakılacağı açıklanır.
author: mksuni
ms.author: sumuth
ms.service: mysql
ms.topic: how-to
ms.date: 09/21/2020
ms.custom: references_regions
ms.openlocfilehash: d65b074385311e74444929ef74901e402e29ec03
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "93241744"
---
# <a name="manage-zone-redundant-high-availability-in-azure-database-for-mysql-flexible-server-preview"></a>MySQL için Azure veritabanı esnek sunucusu 'nda bölge yedekli yüksek kullanılabilirliği yönetme (Önizleme)

Bu makalede, esnek sunucunuzda bölge yedekli yüksek kullanılabilirlik yapılandırmasını nasıl etkinleştirebileceğinizi veya devre dışı bırakabileceğinizi açıklanmaktadır.

Yüksek kullanılabilirlik özelliği, farklı bölgelerde fiziksel ve bekleme çoğaltmasını fiziksel olarak ayırır. Daha fazla ayrıntı için bkz. [yüksek kullanılabilirlik kavramları belgeleri](./concepts/../concepts-high-availability.md). 

> [!IMPORTANT]
> Esnek sunucu oluşturma sırasında bölge yedekli yüksek kullanılabilirliği etkinleştirebilirsiniz.

Bu sayfa, yüksek kullanılabilirliği nasıl etkinleştirebilirim veya devre dışı bırakabilmeniz için yönergeler sağlar. Bu işlem VNET yapılandırması, güvenlik duvarı ayarları ve yedekleme saklama dahil diğer ayarlarınızı değiştirmez. Benzer şekilde, yüksek kullanılabilirliği devre dışı bırakmak çevrimiçi bir işlemdir ve uygulama bağlantınızı ve işlemlerinizi etkilemez.

> [!IMPORTANT]
> Bölgesel olarak yedekli yüksek kullanılabilirlik, sınırlı sayıda bölgede kullanılabilir: Güneydoğu Asya, WestUS 2, Batı Avrupa ve Doğu ABD.  

## <a name="enable-high-availability-during-server-creation"></a>Sunucu oluşturma sırasında yüksek kullanılabilirliği etkinleştir

Bu bölüm, özellikle de HA ile ilgili alanlar için ayrıntılar sağlar. Esnek sunucunuzu oluştururken yüksek kullanılabilirlik dağıtmak için bu adımları izleyebilirsiniz.

1.  [Azure Portal](https://portal.azure.com/)esnek sunucu ' yı seçin ve **Oluştur**' a tıklayın.  **Abonelik**, **kaynak grubu**, **sunucu adı**, **bölge** ve diğer alanlar gibi ayrıntıların nasıl doldurulması gerektiği hakkında ayrıntılı bilgi için bkz. sunucu oluşturma için nasıl yapılır belgeleri.

2.  Kullanılabilirlik seçeneğinde **bölge yedekli yüksek kullanılabilirlik** onay kutusuna tıklayın.

3.  Varsayılan işlem ve depolamayı değiştirmek istiyorsanız,  **sunucuyu Yapılandır**' a tıklayın.

4.  Yüksek kullanılabilirlik seçeneği işaretliyse, Burstable katman seçim için kullanılamaz. **Genel amaçlı** ya da **bellek için iyileştirilmiş** işlem katmanları seçebilirsiniz.

    > [!IMPORTANT]
    > ***Genel amaçlı** _ ve _ *_bellek için iyileştirilmiş_** fiyatlandırma katmanı için yalnızca bölge yedekli yüksek kullanılabilirliğini destekliyoruz.

5.  Açılır menüden istediğiniz **işlem boyutunu** seçin.

6.  Kayan çubuğu kullanarak GiB 'de **depolama boyutu** ' nu seçin ve **yedekleme saklama süresini** 7 gün ile 35 gün arasında seçin.   

## <a name="disable-high-availability"></a>Yüksek kullanılabilirliği devre dışı bırak

Zaten bölge yedekliği ile yapılandırılmış esnek sunucunuz için yüksek kullanılabilirliği devre dışı bırakmak üzere bu adımları izleyin.

1.  [Azure Portal](https://portal.azure.com/), MySQL Için mevcut Azure veritabanı esnek sunucusunu seçin.

2.  Esnek sunucu sayfasında, yüksek kullanılabilirlik sayfasını açmak için ön panelinden **yüksek kullanılabilirlik** ' e tıklayın.

3.  Seçeneği devre dışı bırakmak için **bölge yedekli yüksek kullanılabilirlik** onay kutusuna tıklayın ve değişikliği kaydetmek için **Kaydet** ' e tıklayın.

4.  HA 'nın devre dışı bırakılmasını doğrulayabileceğiniz bir onay iletişim kutusu gösterilir.

5.  Yüksek kullanılabilirliği devre dışı bırakmak için **ha düğmesini devre dışı bırak** seçeneğine tıklayın.

6.  Bir bildirim, yüksek kullanılabilirlik dağıtımının açığa sürme sürecini gösterir.

## <a name="next-steps"></a>Sonraki adımlar

-   [İş sürekliliği](./concepts-business-continuity.md) hakkında bilgi edinin
-   [Bölge yedekli yüksek kullanılabilirlik](./concepts-high-availability.md) hakkında bilgi edinin
