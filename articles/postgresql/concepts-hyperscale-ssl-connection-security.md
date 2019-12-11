---
title: SSL-Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı-hiper ölçek (Citus) ve ilişkili uygulamaları, SSL bağlantılarını düzgün şekilde kullanacak şekilde yapılandırmaya yönelik yönergeler ve bilgiler.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: 3e4ef5d2d6db3a3d4f8923f47079f2484639a751
ms.sourcegitcommit: 5ab4f7a81d04a58f235071240718dfae3f1b370b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/10/2019
ms.locfileid: "74973994"
---
# <a name="configure-ssl-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda SSL 'yi yapılandırma-hiper ölçek (Citus)
Hiper ölçek (Citus) düzenleyici düğümüne yönelik istemci uygulaması bağlantıları Güvenli Yuva Katmanı (SSL) gerektirir. Veritabanı sunucunuz ile istemci uygulamalarınız arasında SSL bağlantılarını zorunlu tutma, sunucu ve uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur.

## <a name="enforcing-ssl-connections"></a>SSL bağlantılarını zorlama
Azure portal aracılığıyla sağlanan PostgreSQL için Azure veritabanı sunucuları için, SSL bağlantılarının zorlanması varsayılan olarak etkindir. 

Benzer şekilde, Azure portal sunucunuz altındaki "bağlantı dizeleri" ayarlarında önceden tanımlanmış olan bağlantı dizeleri, SSL kullanarak veritabanı sunucunuza bağlanmak için gereken ortak diller için gerekli parametreleri içerir. SSL parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Uygulamanızın veya çerçevesinin SSL bağlantılarını desteklediğinden emin olun
Veritabanı Hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri, yükleme sırasında varsayılan olarak SSL 'yi etkinleştirmez. PostgreSQL sunucunuz SSL bağlantıları zorluyor, ancak uygulama SSL için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamadan başarısız olabilir. SSL bağlantılarını nasıl etkinleştireceğinizi öğrenmek için uygulamanızın belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>SSL bağlantısı için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL için Azure veritabanı 'na bağlanma sertifikası-hiper ölçek (Citus) https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem konumunda bulunur. Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>Psql kullanarak bağlanma
Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak Hyperscale (Citus) Düzenleyicisi düğümüne nasıl bağlanılacağını gösterir. SSL sertifikası doğrulamasını zorlamak için `sslmode=verify-full` bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametresine geçirin.

Psql bağlantı dizesinin bir örneği aşağıda verilmiştir:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> `sslrootcert` geçirilen değerin, kaydettiğiniz sertifikanın dosya yoluyla eşleştiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[PostgreSQL Için Azure veritabanı-Hyperscale (Citus) güvenlik duvarı kurallarıyla](concepts-hyperscale-firewall-rules.md)güvenliği artırın.
