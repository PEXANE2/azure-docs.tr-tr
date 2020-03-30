---
title: SSL bağlantısı - MySQL için Azure Veritabanı
description: MySQL ve ilişkili uygulamalar için Azure Veritabanını SSL bağlantılarını düzgün kullanacak şekilde yapılandırmak için bilgiler
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 6a12ef851823ab5eff2b11905d05be1950c82ef0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79474280"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure Veritabanında SSL bağlantısı

MySQL için Azure Veritabanı, Güvenli Soketkatmanı (SSL) kullanarak veritabanı sunucunuzu istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="ssl-default-settings"></a>SSL Varsayılan ayarları

Varsayılan olarak, veritabanı hizmeti MySQL'e bağlanırken SSL bağlantılarını gerektirecek şekilde yapılandırılmalıdır.  Mümkün olduğunca SSL seçeneğini devre dışı bırakmanızı öneririz.

Azure portalı ve CLI üzerinden MySQL sunucusu için yeni bir Azure Veritabanı sağlarken, Varsayılan olarak SSL bağlantılarının uygulanması etkinleştirilir. 

Azure portalında çeşitli programlama dilleri için bağlantı dizeleri gösterilir. Bu bağlantı dizeleri veritabanınıza bağlanmak için gerekli SSL parametrelerini içerir. Azure portalında sunucunuzu seçin. **Ayarlar** başlığı altında **Bağlantı dizeleri'ni**seçin. SSL parametresi bağlayıcıya göre değişir, örneğin "ssl=true" veya "sslmode=require" veya "sslmode=gerekli" ve diğer varyasyonlar.

Uygulama geliştirirken SSL bağlantısını nasıl etkinleştirecek veya devre dışı kalımkonusunda öğrenmek için [SSL'yi nasıl yapılandırılatırınız.](howto-configure-ssl.md)

## <a name="next-steps"></a>Sonraki adımlar

[MySQL için Azure Veritabanı için bağlantı kitaplıkları](concepts-connection-libraries.md)
