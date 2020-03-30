---
title: Sunucular - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'nı yapılandırmak ve yönetmek için göz önünde bulundurulması gereken hususlar ve yönergeler sağlanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: d86170a53b4bfbe712bbca12db4d6063214aba21
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768172"
---
# <a name="azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanı - Tek Sunucu
Bu makalede, PostgreSQL - Single Server için Azure Veritabanı ile çalışmak için göz önünde bulundurulması gereken hususlar ve yönergeler yer almaktadır.

## <a name="what-is-an-azure-database-for-postgresql-server"></a>PostgreSQL için Azure Veritabanı sunucusu nedir?
PostgreSQL için Azure Veritabanı'ndaki bir sunucu - Tek Sunucu dağıtım seçeneği, birden çok veritabanı için merkezi bir yönetim noktasıdır. Şirket içi dünyada aşina olabileceğiniz aynı PostgreSQL sunucu yapısıdır. Özellikle, PostgreSQL hizmeti yönetilir, performans garantileri sağlar, sunucu düzeyinde erişim ve özellikleri ortaya çıkarır.

PostgreSQL sunucusu için bir Azure Veritabanı:

- Azure aboneliği içinde oluşturulur.
- Veritabanları için ana kaynaktır.
- Veritabanları için bir ad alanı sağlar.
- Güçlü ömür boyu semantik ile bir kapsayıcı mı - bir sunucu silmek ve içerdiği veritabanlarını siler.
- Bir bölgedeki kaynakları bir birime sağlar.
- Sunucu ve veritabanı erişimi için bağlantı bitiş noktası sağlar 
- Veritabanlarıiçin geçerli olan yönetim ilkelerinin kapsamını sağlar: giriş, güvenlik duvarı, kullanıcılar, roller, yapılandırmalar, vb.
- Birden çok sürümde kullanılabilir. Daha fazla bilgi için [desteklenen PostgreSQL veritabanı sürümlerine](concepts-supported-versions.md)bakın.
- Kullanıcılar tarafından genişletilebilir. Daha fazla bilgi için [PostgreSQL uzantılarına](concepts-extensions.md)bakın.

PostgreSQL sunucusu için bir Azure Veritabanı içinde bir veya birden çok veritabanı oluşturabilirsiniz. Tüm kaynakları kullanmak için sunucu başına tek bir veritabanı oluşturmayı veya kaynakları paylaşmak için birden çok veritabanı oluşturmayı seçebilirsiniz. Fiyatlandırma, fiyatlandırma katmanı, vCores ve depolama (GB) yapılandırmasına bağlı olarak sunucu başına yapılandırılmıştır. Daha fazla bilgi için [Fiyatlandırma katmanlarına](./concepts-pricing-tiers.md)bakın.

## <a name="how-do-i-connect-and-authenticate-to-an-azure-database-for-postgresql-server"></a>PostgreSQL sunucusu için bir Azure Veritabanına nasıl bağlanır ve kimliğimi doğrulayabilirim?
Aşağıdaki öğeler veritabanınıza güvenli erişim sağlamaya yardımcı olur:

|||
|:--|:--|
| **Kimlik doğrulama ve yetkilendirme** | PostgreSQL için Azure Veritabanı sunucusu, yerel PostgreSQL kimlik doğrulamasını destekler. Sunucunun yönetici hesabını kullanarak sunucuya bağlanabilir ve kimlik doğrulaması gerçekleştirebilirsiniz. |
| **Protokolü** | Hizmet, PostgreSQL tarafından kullanılan ileti tabanlı bir protokolü destekler. |
| **TCP/IP** | Protokol TCP/IP ve Unix etki alanı soketleri üzerinden desteklenir. |
| **Güvenlik duvarı** | Verilerinizin korunmasına yardımcı olmak için, güvenlik duvarı kuralı, siz hangi bilgisayarların izni olduğunu belirtene kadar sunucunuza ve veritabanlarına tüm erişimi engeller.  [PostgreSQL Server güvenlik duvarı kuralları için Azure Veritabanı'na](concepts-firewall-rules.md)bakın. |

## <a name="managing-your-server"></a>Sunucunuzu yönetme
PostgreSQL sunucuları için Azure Veritabanı'nı [Azure portalını](https://portal.azure.com) veya [Azure CLI'yi](/cli/azure/postgres)kullanarak yönetebilirsiniz.

Sunucu oluştururken, yönetici kullanıcınızın kimlik bilgilerini ayarlarsınız. Yönetici kullanıcı, sunucudaki en yüksek ayrıcalık kullanıcısıdır. azure_pg_admin rolüne ait. Bu rolün tam süper kullanıcı izinleri yoktur. 

PostgreSQL süperkullanıcı özniteliği, yönetilen hizmete ait olan azure_superuser atanır. Bu role erişiminiz yok.

PostgreSQL sunucusu için bir Azure Veritabanı varsayılan veritabanlarına sahiptir: 
- **postgres** - Sunucunuz oluşturulduktan sonra bağlanabileceğiniz varsayılan bir veritabanı.
- **azure_maintenance** - Bu veritabanı, yönetilen hizmeti sağlayan işlemleri kullanıcı eylemlerinden ayırmak için kullanılır. Bu veritabanına erişiminiz yok.
- **azure_sys** - Sorgu Deposu için bir veritabanı. Bu veritabanı, Sorgu Deposu kapalıyken veri birikir; bu varsayılan ayardır. Daha fazla bilgi için [Sorgu Mağazası'na genel bakış'a](concepts-query-store.md)bakın.


## <a name="server-parameters"></a>Sunucu parametreleri
PostgreSQL sunucu parametreleri sunucunun yapılandırmasını belirler. PostgreSQL için Azure Veritabanı'nda, parametre listesi Azure portalı veya Azure CLI kullanılarak görüntülenebilir ve düzenlenebilir. 

Postgres için yönetilen bir hizmet olarak, PostgreSQL için Azure Veritabanı'ndaki yapılandırılabilir parametreler yerel bir Postgres örneğindeki parametrelerin bir alt kümesidir (Postgres parametreleri hakkında daha fazla bilgi için [PostgreSQL belgelerine](https://www.postgresql.org/docs/9.6/static/runtime-config.html)bakın). PostgreSQL sunucusu için Azure Veritabanınız, oluşturmadaki her parametre için varsayılan değerlerle etkinleştirilmiştir. Değişikliklerin etkili olması için sunucu yeniden başlatması veya süper kullanıcı erişimi gerektiren bazı parametreler kullanıcı tarafından yapılandırılamaz.


## <a name="next-steps"></a>Sonraki adımlar
- Hizmete genel bir bakış için [PostgreSQL Genel Bakışı için Azure Veritabanı'na](overview.md)bakın.
- **Hizmet katmanınızı**temel alan belirli kaynak kotaları ve sınırlamaları hakkında bilgi için [Bkz. Hizmet katmanları.](concepts-pricing-tiers.md)
- Hizmete bağlanma hakkında daha fazla bilgi için [PostgreSQL için Azure Veritabanı bağlantı kitaplıklarına](concepts-connection-libraries.md)bakın.
- Sunucu parametrelerini [Azure portalı](howto-configure-server-parameters-using-portal.md) veya [Azure CLI](howto-configure-server-parameters-using-cli.md)üzerinden görüntüleyin ve düzenleme.
