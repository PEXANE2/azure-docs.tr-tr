---
title: SSL bağlantısı - MariaDB için Azure Veritabanı
description: SSL bağlantılarını düzgün kullanacak şekilde MariaDB ve ilişkili uygulamalar için Azure Veritabanı'nı yapılandırmak için bilgiler
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 36532575645d135a7abe7239798b6f2abc4246f2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477077"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>MariaDB için Azure Veritabanında SSL bağlantısı
MariaDB için Azure Veritabanı, Güvenli Soketkatmanı (SSL) kullanarak veritabanı sunucunuzu istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="default-settings"></a>Varsayılan ayarlar
Varsayılan olarak, veritabanı hizmeti MariaDB'ye bağlanırken SSL bağlantılarını gerektirecek şekilde yapılandırılmalıdır.  Mümkün olduğunca SSL seçeneğini devre dışı bırakmanızı öneririz.

Azure portalı ve CLI üzerinden MariaDB sunucusu için yeni bir Azure Veritabanı sağlarken, Varsayılan olarak SSL bağlantılarının uygulanması etkinleştirilir.

Azure portalında çeşitli programlama dilleri için bağlantı dizeleri gösterilir. Bu bağlantı dizeleri veritabanınıza bağlanmak için gerekli SSL parametrelerini içerir. Azure portalında sunucunuzu seçin. **Ayarlar** başlığı altında **Bağlantı dizeleri'ni**seçin. SSL parametresi bağlayıcıya göre değişir, örneğin "ssl=true" veya "sslmode=require" veya "sslmode=gerekli" ve diğer varyasyonlar.

Uygulama geliştirirken SSL bağlantısını nasıl etkinleştirecek veya devre dışı kalımkonusunda öğrenmek için [SSL'yi nasıl yapılandırılatırınız.](howto-configure-ssl.md)

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucu güvenlik duvarı kuralları](concepts-firewall-rules.md) hakkında daha fazla bilgi edinin
- [SSL'yi nasıl yapılandırılatırın](howto-configure-ssl.md)öğrenin.
