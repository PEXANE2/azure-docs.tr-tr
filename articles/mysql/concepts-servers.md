---
title: Sunucu kavramları - MySQL için Azure Veritabanı
description: Bu konu, MySQL sunucuları için Azure Veritabanı ile çalışmak için göz önünde bulundurulması gereken hususları ve yönergeleri sağlar.
author: ajlam
ms.author: andrela
ms.service: mysql
ms.topic: conceptual
ms.date: 3/18/2020
ms.openlocfilehash: 300470b2e8fb10fda7cfc59517cef00c07bf0632
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79537015"
---
# <a name="server-concepts-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanı'ndaki sunucu kavramları

Bu makalede, MySQL sunucuları için Azure Veritabanı ile çalışmak için göz önünde bulundurulması gereken hususlar ve yönergeler sağlanmaktadır.

## <a name="what-is-an-azure-database-for-mysql-server"></a>MySQL sunucusu için Azure Veritabanı nedir?

MySQL sunucusu için Azure Veritabanı, birden çok veritabanı için merkezi bir yönetim noktasıdır. Şirket içi dünyada aşina olabileceğiniz aynı MySQL sunucu yapısıdır. Özellikle, MySQL için Azure Veritabanı hizmeti yönetilir, performans garantileri sağlar ve sunucu düzeyinde erişim ve özellikleri ortaya çıkarır.

MySQL sunucusu için bir Azure Veritabanı:

- Azure aboneliği içinde oluşturulur.
- Veritabanları için ana kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Güçlü ömür boyu semantik ile bir kapsayıcı mı - bir sunucu silmek ve içerdiği veritabanlarını siler.
- Bir bölgedeki kaynakları bir birime sağlar.
- Sunucu ve veritabanı erişimi için bir bağlantı bitiş noktası sağlar.
- Veritabanlarıiçin geçerli olan yönetim ilkelerinin kapsamını sağlar: giriş, güvenlik duvarı, kullanıcılar, roller, yapılandırmalar, vb.
- Birden çok sürümde kullanılabilir. Daha fazla bilgi için [MySQL veritabanı sürümleri için Desteklenen Azure Veritabanı'na](./concepts-supported-versions.md)bakın.

MySQL sunucusu için Azure Veritabanı içinde bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak veya kaynakları paylaşmak için birden çok veritabanı oluşturmak için sunucu başına tek bir veritabanı oluşturmayı tercih edebilirsiniz. Fiyatlandırma, fiyatlandırma katmanı, vCores ve depolama (GB) yapılandırmasına bağlı olarak sunucu başına yapılandırılmıştır. Daha fazla bilgi için [Fiyatlandırma katmanlarına](./concepts-service-tiers.md)bakın.

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-mysql-server"></a>MySQL sunucusu için bir Azure Veritabanına nasıl bağlanır ve kimliğimi doğrulayabilirim?

Aşağıdaki öğeler veritabanınıza güvenli erişim sağlamaya yardımcı olur.

|     |     |
| :-- | :-- |
| **Kimlik doğrulama ve yetkilendirme** | MySQL sunucusu için Azure Veritabanı, yerel MySQL kimlik doğrulamasını destekler. Sunucunun yönetici girişi olan bir sunucuya bağlanabilir ve kimlik doğrulayabilirsiniz. |
| **Protokolü** | Hizmet, MySQL tarafından kullanılan ileti tabanlı bir protokolü destekler. |
| **TCP/IP** | Protokol TCP/IP ve Unix etki alanı soketleri üzerinden desteklenir. |
| **Güvenlik duvarı** | Verilerinizin korunmasına yardımcı olmak için, güvenlik duvarı kuralı, siz hangi bilgisayarların izni olduğunu belirtene kadar veritabanı sunucunuza tüm erişimi engeller. [MySQL Server güvenlik duvarı kuralları için Azure Veritabanı'na](./concepts-firewall-rules.md)bakın. |
| **Ssl** | Hizmet, uygulamalarınız ve veritabanı sunucunuz arasındaki SSL bağlantılarını zorlamayı destekler.  Bkz. [MySQL için Azure Veritabanına güvenli bir şekilde bağlanmak üzere uygulamanızda SSL bağlantısı yapılandırma](./howto-configure-ssl.md). |

## <a name="how-do-i-manage-a-server"></a>Bir sunucuyu nasıl yönetirim?

MySQL sunucuları için Azure Veritabanı'nı Azure portalını veya Azure CLI'yi kullanarak yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

- Hizmete genel bir bakış için [MySQL Genel Bakışı için Azure Veritabanı'na](./overview.md) bakın
- **Hizmet katmanınıza**dayalı belirli kaynak kotaları ve sınırlamaları hakkında bilgi için [Bkz.](./concepts-service-tiers.md)
- Hizmete bağlanma hakkında daha fazla bilgi için [MySQL için Azure Veritabanı bağlantı kitaplıklarına](./concepts-connection-libraries.md)bakın.
