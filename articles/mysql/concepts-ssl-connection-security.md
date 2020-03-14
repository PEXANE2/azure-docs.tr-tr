---
title: SSL bağlantısı-MySQL için Azure veritabanı
description: MySQL için Azure veritabanı 'nı ve ilişkili uygulamaları düzgün şekilde SSL bağlantıları kullanacak şekilde yapılandırma bilgileri
author: kummanish
ms.author: manishku
ms.service: mysql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 7bc3a238ca2ff2861ec6fc65033738e741574321
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79370857"
---
# <a name="ssl-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda SSL bağlantısı

MySQL için Azure veritabanı, veritabanı sunucunuzu Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarına bağlamayı destekler. Veritabanı sunucunuzla istemci uygulamalarınız arasında SSL bağlantılarının zorunlu tutulması, sunucuya uygulamanız arasındaki veri akışını şifreleyerek "bağlantıyı izinsiz izleme" saldırılarına karşı korumaya yardımcı olur.

## <a name="ssl-default-settings"></a>SSL varsayılan ayarları

Varsayılan olarak, veritabanı hizmeti MySQL 'e bağlanılırken SSL bağlantıları gerektirecek şekilde yapılandırılmalıdır.  Mümkün olduğunda SSL seçeneğini devre dışı bırakmaktan kaçınmanızı öneririz.

Azure portal ve CLı aracılığıyla yeni bir MySQL için Azure veritabanı sunucusu sağlarken, SSL bağlantılarını zorlama varsayılan olarak etkinleştirilir. 

Çeşitli programlama dilleri için bağlantı dizeleri Azure portal gösterilmektedir. Bu bağlantı dizeleri, veritabanınıza bağlanmak için gerekli SSL parametrelerini içerir. Azure portal, sunucunuzu seçin. **Ayarlar** başlığı altında **bağlantı dizelerini**seçin. SSL parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

Uygulama geliştirirken SSL bağlantısını etkinleştirme veya devre dışı bırakma hakkında bilgi edinmek için bkz. [SSL 'yi yapılandırma](howto-configure-ssl.md).

## <a name="tls-connectivity-in-azure-database-for-mysql"></a>MySQL için Azure veritabanı 'nda TLS bağlantısı

MySQL için Azure veritabanı, Aktarım Katmanı Güvenliği (TLS) kullanarak veritabanı sunucunuza bağlanan istemciler için şifrelemeyi destekler. TLS, veritabanı sunucunuz ile istemci uygulamalarınız arasında güvenli ağ bağlantıları sağlayan ve uyumluluk gereksinimlerine bağlı olmanızı sağlayan bir endüstri standardı protokolüdür.

### <a name="tls-settings"></a>TLS ayarları

Müşteriler artık, MySQL için Azure veritabanı 'na bağlanan istemci için TLS sürümünü zorunlu kılabilir. TLS seçeneğini kullanmak için **En düşük TLS sürümü** seçenek ayarını kullanın. Bu seçenek ayarı için aşağıdaki değerlere izin verilir:

|  Minimum TLS ayarı             | Desteklenen TLS sürümü                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (varsayılan) | TLS gerekli değil                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 ve üzeri |
| TLS1_1                           | TLS 1,1, TLS 1,2 ve üzeri          |
| TLS1_2                           | TLS sürüm 1,2 ve üzeri           |


Örneğin, bu en düşük TLS ayarı sürümünü TLS 1,0 olarak ayarlamak, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1,2 kullanarak istemcilerden gelen bağlantılara izin vertiğinizi ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedildiğini belirtir.

> [!Note] 
> MySQL için Azure veritabanı, tüm yeni sunucular için varsayılan olarak TLS 'yi devre dışı bırakılıyor.
>
> Şu anda MySQL için Azure veritabanı tarafından desteklenen TLS sürümleri TLS 1,0, 1,1 ve 1,2.

MySQL için Azure veritabanınız için TLS ayarını ayarlamayı öğrenmek için bkz. [TLS ayarlarını yapılandırma](howto-tls-configurations.md).

## <a name="next-steps"></a>Sonraki adımlar

[MySQL için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md)
