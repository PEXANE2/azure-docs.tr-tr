---
title: SSL bağlantısı-MariaDB için Azure veritabanı
description: MariaDB için Azure veritabanı 'nı ve ilişkili uygulamaları düzgün şekilde SSL bağlantıları kullanacak şekilde yapılandırma bilgileri
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: c03a56176a6e2cc995e74017b60747541fc843bb
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371639"
---
# <a name="ssl-connectivity-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda SSL bağlantısı
MariaDB için Azure veritabanı, veritabanı sunucunuzu Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="default-settings"></a>Varsayılan ayarları
Varsayılan olarak, veritabanı hizmeti MariaDB 'ye bağlanırken SSL bağlantıları gerektirecek şekilde yapılandırılmalıdır.  Mümkün olduğunda SSL seçeneğini devre dışı bırakmaktan kaçınmanızı öneririz.

Azure portal ve CLı aracılığıyla MariaDB sunucusu için yeni bir Azure veritabanı sağlarken, SSL bağlantılarını zorlama varsayılan olarak etkinleştirilir.

Çeşitli programlama dilleri için bağlantı dizeleri Azure portal gösterilmektedir. Bu bağlantı dizeleri, veritabanınıza bağlanmak için gerekli SSL parametrelerini içerir. Azure portal, sunucunuzu seçin. **Ayarlar** başlığı altında **bağlantı dizelerini**seçin. SSL parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

Uygulama geliştirirken SSL bağlantısını etkinleştirme veya devre dışı bırakma hakkında bilgi edinmek için bkz. [SSL 'yi yapılandırma](howto-configure-ssl.md).

## <a name="tls-connectivity-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda TLS bağlantısı

MariaDB için Azure veritabanı, Aktarım Katmanı Güvenliği (TLS) kullanarak veritabanı sunucunuza bağlanan istemciler için şifrelemeyi destekler. TLS, veritabanı sunucunuz ile istemci uygulamalarınız arasında güvenli ağ bağlantıları sağlayan ve uyumluluk gereksinimlerine bağlı olmanızı sağlayan bir endüstri standardı protokolüdür.

### <a name="tls-settings"></a>TLS ayarları

MariaDB için Azure veritabanı, istemci bağlantıları için TLS sürümünü zorlama olanağı sağlar. TLS seçeneğini kullanmak için **En düşük TLS sürümü** seçenek ayarını kullanın. Bu seçenek ayarı için aşağıdaki değerlere izin verilir:

|  Minimum TLS ayarı             | Desteklenen TLS sürümü                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (varsayılan) | TLS gerekli değil                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 ve üzeri |
| TLS1_1                           | TLS 1,1, TLS 1,2 ve üzeri          |
| TLS1_2                           | TLS sürüm 1,2 ve üzeri           |


Örneğin, bu en düşük TLS ayarı sürümünü TLS 1,0 olarak ayarlamak, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1,2 kullanarak istemcilerden gelen bağlantılara izin vertiğinizi ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedildiğini belirtir.

> [!Note] 
> MariaDB için Azure veritabanı, tüm yeni sunucular için varsayılan olarak TLS 'yi devre dışı bırakılıyor.
>
> Şu anda, MariaDB için TLS sürümleri desteklenen byAzure veritabanı, TLS 1,0, 1,1 ve 1,2.

MariaDB için Azure veritabanınız için TLS ayarını ayarlama hakkında bilgi edinmek için bkz. [TLS ayarlarını yapılandırma](howto-tls-configurations.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucu güvenlik duvarı kuralları](concepts-firewall-rules.md) hakkında daha fazla bilgi
- [SSL 'yi yapılandırmayı](howto-configure-ssl.md)öğrenin.
- [TLS Yapılandırma](howto-tls-configurations.md)hakkında bilgi edinin.