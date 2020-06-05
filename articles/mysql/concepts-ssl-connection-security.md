---
title: SSL/TLS bağlantısı-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı 'nı ve ilişkili uygulamaları düzgün şekilde SSL bağlantıları kullanacak şekilde yapılandırma bilgileri
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 2421f8a9396b47d04db35a7cad843f6baa6f6177
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84416112"
---
# <a name="ssltls-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda SSL/TLS bağlantısı

MySQL için Azure veritabanı, veritabanı sunucunuzu Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="ssl-default-settings"></a>SSL varsayılan ayarları

Varsayılan olarak, veritabanı hizmeti MySQL 'e bağlanılırken SSL bağlantıları gerektirecek şekilde yapılandırılmalıdır.  Mümkün olduğunda SSL seçeneğini devre dışı bırakmaktan kaçınmanızı öneririz.

Azure portal ve CLı aracılığıyla yeni bir MySQL için Azure veritabanı sunucusu sağlarken, SSL bağlantılarını zorlama varsayılan olarak etkinleştirilir. 

Çeşitli programlama dilleri için bağlantı dizeleri Azure portal gösterilmektedir. Bu bağlantı dizeleri, veritabanınıza bağlanmak için gerekli SSL parametrelerini içerir. Azure portal, sunucunuzu seçin. **Ayarlar** başlığı altında **bağlantı dizelerini**seçin. SSL parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

Uygulama geliştirirken SSL bağlantısını etkinleştirme veya devre dışı bırakma hakkında bilgi edinmek için bkz. [SSL 'yi yapılandırma](howto-configure-ssl.md).

## <a name="tls-enforcement-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda TLS zorlaması

MySQL için Azure veritabanı, Aktarım Katmanı Güvenliği (TLS) kullanarak veritabanı sunucunuza bağlanan istemciler için şifrelemeyi destekler. TLS, veritabanı sunucunuz ile istemci uygulamalarınız arasında güvenli ağ bağlantıları sağlayan ve uyumluluk gereksinimlerine bağlı olmanızı sağlayan bir endüstri standardı protokolüdür.

### <a name="tls-settings"></a>TLS ayarları

MySQL için Azure veritabanı, istemci bağlantıları için TLS sürümünü zorlayabilme olanağı sağlar. TLS sürümünü zorlamak için **En düşük TLS sürümü** seçenek ayarını kullanın. Bu seçenek ayarı için aşağıdaki değerlere izin verilir:

|  Minimum TLS ayarı             | İstemci TLS sürümü destekleniyor                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (varsayılan) | TLS gerekli değil                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 ve üzeri           |
| TLS1_1                           | TLS 1,1, TLS 1,2 ve üzeri                   |
| TLS1_2                           | TLS sürüm 1,2 ve üzeri                     |


Örneğin, minimum TLS ayarı sürümü değerinin TLS 1,0 olarak ayarlanması, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1.2 + kullanan istemcilerden gelen bağlantılara izin vermek ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedilecektir.

> [!Note] 
> MySQL için Azure veritabanı, tüm yeni sunucular için varsayılan olarak TLS 'yi devre dışı bırakılıyor.
>
> Şu anda MySQL için Azure veritabanı tarafından desteklenen TLS sürümleri TLS 1,0, 1,1 ve 1,2. Belirli bir minimum TLS sürümüne zorlandıktan sonra, devre dışı olarak değiştiremezsiniz.

MySQL için Azure veritabanınız için TLS ayarını ayarlamayı öğrenmek için bkz. [TLS ayarlarını yapılandırma](howto-tls-configurations.md).

## <a name="next-steps"></a>Sonraki adımlar

- [MySQL için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md)
- [SSL 'yi yapılandırmayı](howto-configure-ssl.md) öğrenin
- [TLS Yapılandırma](howto-tls-configurations.md) hakkında bilgi edinin
