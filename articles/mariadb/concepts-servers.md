---
title: Sunucular-MariaDB için Azure veritabanı
description: Bu konu, MariaDB sunucuları için Azure veritabanı ile çalışmaya yönelik hususlar ve yönergeleri sağlar.
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 12/02/2019
ms.openlocfilehash: 048d9f3089a433dbf8c2647ed86ddab69c78ebaa
ms.sourcegitcommit: 6bb98654e97d213c549b23ebb161bda4468a1997
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74772053"
---
# <a name="server-concepts-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda sunucu kavramları
Bu makalede, MariaDB sunucuları için Azure veritabanı ile çalışmaya yönelik konular ve yönergeler sağlanmaktadır.

## <a name="what-is-an-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure veritabanı nedir?

MariaDB sunucusu için Azure veritabanı, birden çok veritabanı için merkezi bir yönetim noktasıdır. Bu, şirket içi dünyada bildiğiniz aynı MariaDB sunucu yapısıdır. Özellikle, MariaDB hizmeti için Azure veritabanı yönetilir, performans garantisi sağlar ve sunucu düzeyinde erişim ve özellikler sunar.

MariaDB sunucusu için Azure veritabanı:

- Bir Azure aboneliği içinde oluşturulur.
- , Veritabanları için üst kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Güçlü yaşam süresi semantiğinin bulunduğu bir kapsayıcıdır; bir sunucuyu silin ve kapsanan veritabanlarını siler.
- Bir bölgedeki kaynakları birlikte bulur.
- Sunucu ve veritabanı erişimi için bir bağlantı uç noktası sağlar.
- Veritabanlarına uygulanan yönetim ilkeleri için kapsam sağlar: oturum açma, güvenlik duvarı, kullanıcılar, roller, konfigürasyonlar vb.
- MariaDB altyapısı sürüm 10,2 ' de mevcuttur. Daha fazla bilgi için bkz. [MariaDB veritabanı sürümleri Için desteklenen Azure veritabanı](./concepts-supported-versions.md).

MariaDB için Azure Veritabanı sunucusunda bir veya birden fazla veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak veya kaynakları paylaşmak için birden çok veritabanı oluşturmak üzere sunucu başına tek bir veritabanı oluşturmayı tercih edebilirsiniz. Fiyatlandırma Katmanı, sanal çekirdek ve depolama (GB) yapılandırmasına bağlı olarak, fiyatlandırma sunucu başına yapılandırılır. Daha fazla bilgi için bkz. [fiyatlandırma katmanları](./concepts-pricing-tiers.md).

## <a name="how-do-i-secure-an-azure-database-for-mariadb-server"></a>MariaDB sunucusu için Azure veritabanı güvenli Nasıl yaparım? mı?

Aşağıdaki öğeler veritabanınıza güvenli erişimin sağlanmasına yardımcı olur.

|||
| :--| :--|
| **Kimlik doğrulama ve yetkilendirme** | MariaDB sunucusu için Azure veritabanı yerel MySQL kimlik doğrulamasını destekler. Sunucu Yöneticisi oturum açma bilgileri ile sunucuya bağlanabilir ve kimlik doğrulaması yapabilirsiniz. |
| **Protokol** | Hizmet MySQL tarafından kullanılan ileti tabanlı bir protokolü destekler. |
| **TCP/ıP** | Protokol TCP/IP üzerinden ve UNIX-etki alanı Yuvaları üzerinden desteklenir. |
| **Güvenlik duvarı** | Verilerinizin korunmasına yardımcı olmak için, hangi bilgisayarların iznine sahip olduğunu belirtene kadar bir güvenlik duvarı kuralı, veritabanı sunucunuza tüm erişimi engeller. Bkz. [MariaDB sunucusu Için Azure veritabanı güvenlik duvarı kuralları](./concepts-firewall-rules.md). |
| **SSL** | Hizmet, uygulamalarınız ve veritabanı sunucunuz arasında SSL bağlantısı uygulanmasını destekler. [MariaDB Için Azure veritabanı 'na güvenli bir şekilde bağlanmak üzere UYGULAMANıZDA SSL bağlantısını yapılandırma](./howto-configure-ssl.md)konusuna bakın. |

## <a name="how-do-i-manage-a-server"></a>Sunucu Nasıl yaparım? mi?
MariaDB sunucuları için Azure veritabanı 'nı Azure portal veya Azure CLı kullanarak yönetebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar
- Hizmete genel bakış için bkz. [MariaDB Için Azure veritabanı genel bakış](./overview.md)
- **Hizmet katmanınıza**dayalı belirli kaynak kotaları ve sınırlamalar hakkında bilgi için bkz. [hizmet katmanları](./concepts-pricing-tiers.md)

<!-- - For information about connecting to the service, see [Connection libraries for Azure Database for MariaDB](./concepts-connection-libraries.md). -->
