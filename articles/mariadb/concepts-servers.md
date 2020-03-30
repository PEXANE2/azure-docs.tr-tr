---
title: Sunucular - MariaDB için Azure Veritabanı
description: Bu konu, MariaDB sunucuları için Azure Veritabanı ile çalışmak için göz önünde bulundurulması gereken hususları ve yönergeleri sağlar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 444d7f1574cf1517b01250bcb9d810731030182d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79527801"
---
# <a name="server-concepts-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanı'nda sunucu kavramları
Bu makalede, MariaDB sunucuları için Azure Veritabanı ile çalışmak için göz önünde bulundurulması gereken hususlar ve yönergeler sağlanmaktadır.

## <a name="what-is-an-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure Veritabanı nedir?

MariaDB sunucusu için Azure Veritabanı, birden çok veritabanı için merkezi bir yönetim noktasıdır. Şirket içi dünyada aşina olabileceğiniz aynı MariaDB sunucu yapısıdır. Özellikle, MariaDB için Azure Veritabanı yönetilir, performans garantileri sağlar ve sunucu düzeyinde erişim ve özellikleri ortaya çıkarır.

MariaDB sunucusu için bir Azure Veritabanı:

- Azure aboneliği içinde oluşturulur.
- Veritabanları için ana kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Güçlü ömür boyu semantik ile bir kapsayıcı mı - bir sunucu silmek ve içerdiği veritabanlarını siler.
- Bir bölgedeki kaynakları bir birime sağlar.
- Sunucu ve veritabanı erişimi için bir bağlantı bitiş noktası sağlar.
- Veritabanlarıiçin geçerli olan yönetim ilkelerinin kapsamını sağlar: giriş, güvenlik duvarı, kullanıcılar, roller, yapılandırmalar, vb.
- MariaDB motor versiyonu mevcuttur 10.2. Daha fazla bilgi için [MariaDB veritabanı sürümleri için Desteklenen Azure Veritabanı'na](./concepts-supported-versions.md)bakın.

MariaDB için Azure Veritabanı sunucusunda bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak veya kaynakları paylaşmak için birden çok veritabanı oluşturmak için sunucu başına tek bir veritabanı oluşturmayı tercih edebilirsiniz. Fiyatlandırma, fiyatlandırma katmanı, vCores ve depolama (GB) yapılandırmasına bağlı olarak sunucu başına yapılandırılmıştır. Daha fazla bilgi için [Fiyatlandırma katmanlarına](./concepts-pricing-tiers.md)bakın.

## <a name="how-do-i-secure-an-azure-database-for-mariadb-server"></a>MariaDB sunucusu için azure veritabanını nasıl güvenebilirim?

Aşağıdaki öğeler veritabanınıza güvenli erişim sağlamaya yardımcı olur.

|||
| :--| :--|
| **Kimlik doğrulama ve yetkilendirme** | MariaDB sunucusu için Azure Veritabanı yerel MySQL kimlik doğrulamasını destekler. Sunucunun yönetici girişi olan bir sunucuya bağlanabilir ve kimlik doğrulayabilirsiniz. |
| **Protokolü** | Hizmet, MySQL tarafından kullanılan ileti tabanlı bir protokolü destekler. |
| **TCP/IP** | Protokol TCP/IP ve Unix etki alanı soketleri üzerinden desteklenir. |
| **Güvenlik duvarı** | Verilerinizin korunmasına yardımcı olmak için, güvenlik duvarı kuralı, siz hangi bilgisayarların izni olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. [MariaDB Server güvenlik duvarı kuralları için Azure Veritabanı'na](./concepts-firewall-rules.md)bakın. |
| **Ssl** | Hizmet, uygulamalarınız ve veritabanı sunucunuz arasındaki SSL bağlantılarını zorlamayı destekler. [MariaDB için Azure Veritabanına güvenli bir şekilde bağlanmak için uygulamanızdaki SSL bağlantısını yapılandırın.](./howto-configure-ssl.md) |

## <a name="how-do-i-manage-a-server"></a>Bir sunucuyu nasıl yönetirim?
MariaDB sunucuları için Azure Veritabanı'nı Azure portalını veya Azure CLI'yi kullanarak yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmete genel bir bakış için [MariaDB Genel Bakış için Azure Veritabanı'na](./overview.md) bakın
- **Hizmet katmanınıza**dayalı belirli kaynak kotaları ve sınırlamaları hakkında bilgi için [Bkz.](./concepts-pricing-tiers.md)

<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->
