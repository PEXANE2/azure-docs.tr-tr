---
title: TLS-Hyperscale (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı-hiper ölçek (Citus) ve ilişkili uygulamaları, TLS bağlantılarını düzgün şekilde kullanacak şekilde yapılandırmak için yönergeler ve bilgiler.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: 8a691b6d3f28800ab1cb38a72f124e950d485084
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80422330"
---
# <a name="configure-tls-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda TLS 'yi yapılandırma-hiper ölçek (Citus)
Hiper ölçek (Citus) düzenleyici düğümüne yönelik istemci uygulaması bağlantıları, daha önce Güvenli Yuva Katmanı (SSL) olarak bilinen Aktarım Katmanı Güvenliği (TLS) gerektirir. Veritabanı sunucunuz ile istemci uygulamalarınız arasında TLS bağlantılarının uygulanması, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur.

## <a name="enforcing-tls-connections"></a>TLS bağlantılarını zorlama
Azure portal aracılığıyla sağlanan PostgreSQL için Azure veritabanı sunucuları için, TLS bağlantılarının uygulanması varsayılan olarak etkindir. 

Benzer şekilde, Azure portal sunucunuz altındaki "bağlantı dizeleri" ayarlarında önceden tanımlanmış olan bağlantı dizeleri, TLS kullanarak veritabanı sunucunuza bağlanmak için gereken ortak diller için gerekli parametreleri içerir. TLS parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Uygulamanızın veya çerçevesinin TLS bağlantılarını desteklediğinden emin olun
Veritabanı Hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri, yükleme sırasında varsayılan olarak TLS 'yi etkinleştirmez. PostgreSQL sunucunuz TLS bağlantılarını zorluyor, ancak uygulama TLS için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamadan başarısız olabilir. TLS bağlantılarının nasıl etkinleştirileceğini öğrenmek için uygulamanızın belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>TLS bağlantısı için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL için Azure veritabanı 'na bağlanma sertifikası-hiper ölçek (Citus) konumunda https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pembulunur. Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>Psql kullanarak bağlanma
Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak Hyperscale (Citus) Düzenleyicisi düğümüne nasıl bağlanılacağını gösterir. TLS sertifikası `sslmode=verify-full` doğrulamasını zorlamak için bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametreye geçirin.

Psql bağlantı dizesinin bir örneği aşağıda verilmiştir:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Geçirilen değerin, kaydettiğiniz sertifikanın dosya `sslrootcert` yolu ile eşleştiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[PostgreSQL Için Azure veritabanı-Hyperscale (Citus) güvenlik duvarı kurallarıyla](concepts-hyperscale-firewall-rules.md)güvenliği artırın.
