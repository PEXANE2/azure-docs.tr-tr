---
title: SSL/TLS bağlantısı-MariaDB için Azure veritabanı
description: MariaDB için Azure veritabanı 'nı ve ilişkili uygulamaları düzgün şekilde SSL bağlantıları kullanacak şekilde yapılandırma bilgileri
author: ajlam
ms.author: andrela
ms.service: mariadb
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 4111b0b01690097535412205b60619172e2c100a
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84416665"
---
# <a name="ssltls-connectivity-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda SSL/TLS bağlantısı
MariaDB için Azure veritabanı, veritabanı sunucunuzu Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="default-settings"></a>Varsayılan ayarlar
Varsayılan olarak, veritabanı hizmeti MariaDB 'ye bağlanırken SSL bağlantıları gerektirecek şekilde yapılandırılmalıdır.  Mümkün olduğunda SSL seçeneğini devre dışı bırakmaktan kaçınmanızı öneririz.

Azure portal ve CLı aracılığıyla MariaDB sunucusu için yeni bir Azure veritabanı sağlarken, SSL bağlantılarını zorlama varsayılan olarak etkinleştirilir.

Çeşitli programlama dilleri için bağlantı dizeleri Azure portal gösterilmektedir. Bu bağlantı dizeleri, veritabanınıza bağlanmak için gerekli SSL parametrelerini içerir. Azure portal, sunucunuzu seçin. **Ayarlar** başlığı altında **bağlantı dizelerini**seçin. SSL parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

Uygulama geliştirirken SSL bağlantısını etkinleştirme veya devre dışı bırakma hakkında bilgi edinmek için bkz. [SSL 'yi yapılandırma](howto-configure-ssl.md).

## <a name="tls-enforcement-in-azure-database-for-mariadb"></a>MariaDB için Azure veritabanı 'nda TLS zorlaması

MariaDB için Azure veritabanı, Aktarım Katmanı Güvenliği (TLS) kullanarak veritabanı sunucunuza bağlanan istemciler için şifrelemeyi destekler. TLS, veritabanı sunucunuz ile istemci uygulamalarınız arasında güvenli ağ bağlantıları sağlayan ve uyumluluk gereksinimlerine bağlı olmanızı sağlayan bir endüstri standardı protokolüdür.

### <a name="tls-settings"></a>TLS ayarları

MariaDB için Azure veritabanı, istemci bağlantıları için TLS sürümünü zorlama olanağı sağlar. TLS sürümünü zorlamak için **En düşük TLS sürümü** seçenek ayarını kullanın. Bu seçenek ayarı için aşağıdaki değerlere izin verilir:

|  Minimum TLS ayarı             | İstemci TLS sürümü destekleniyor                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (varsayılan) | TLS gerekli değil                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 ve üzeri         |
| TLS1_1                           | TLS 1,1, TLS 1,2 ve üzeri              |
| TLS1_2                           | TLS sürüm 1,2 ve üzeri                  |


Örneğin, minimum TLS ayarı sürümü değerinin TLS 1,0 olarak ayarlanması, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1.2 + kullanan istemcilerden gelen bağlantılara izin vermek ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedilecektir.

> [!Note] 
> MariaDB için Azure veritabanı, tüm yeni sunucular için varsayılan olarak TLS 'yi devre dışı bırakılıyor. 
>
> Şu anda, MariaDB için Azure veritabanı tarafından desteklenen TLS sürümleri TLS 1,0, 1,1 ve 1,2. Belirli bir minimum TLS sürümüne zorlandıktan sonra, devre dışı olarak değiştiremezsiniz.

MariaDB için Azure veritabanınız için TLS ayarını ayarlama hakkında bilgi edinmek için bkz. [TLS ayarlarını yapılandırma](howto-tls-configurations.md).

## <a name="next-steps"></a>Sonraki adımlar
- [Sunucu güvenlik duvarı kuralları](concepts-firewall-rules.md) hakkında daha fazla bilgi
- [SSL 'yi yapılandırmayı](howto-configure-ssl.md) öğrenin
- [TLS Yapılandırma](howto-tls-configurations.md) hakkında bilgi edinin
