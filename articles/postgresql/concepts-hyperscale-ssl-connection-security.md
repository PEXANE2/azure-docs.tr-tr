---
title: TLS - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: TLS bağlantılarını düzgün kullanmak için PostgreSQL - Hyperscale (Citus) ve ilişkili uygulamalar için Azure Veritabanı'nı yapılandırmak için yönergeler ve bilgiler.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/30/2020
ms.openlocfilehash: 8a691b6d3f28800ab1cb38a72f124e950d485084
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422330"
---
# <a name="configure-tls-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL - Hyperscale (Citus) için Azure Veritabanında TLS yapılandırma
Hyperscale (Citus) koordinatör düğümüne istemci uygulama bağlantıları, daha önce Güvenli Soket katmanı (SSL) olarak bilinen Taşıma Katmanı Güvenliği (TLS) gerektirir. Veritabanı sunucunuz la istemci uygulamalarınız arasındaki TLS bağlantılarını zorlamak, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korumaya yardımcı olur.

## <a name="enforcing-tls-connections"></a>TLS bağlantılarını uygulama
Azure portalı üzerinden sağlanan PostgreSQL sunucuları için tüm Azure Veritabanı için TLS bağlantılarının uygulanması varsayılan olarak etkinleştirilir. 

Benzer şekilde, Azure portalındaki sunucunuzun altındaki "Bağlantı Dizeleri" ayarlarında önceden tanımlanmış bağlantı dizeleri, TLS kullanarak veritabanı sunucunuza bağlanmak için ortak diller için gerekli parametreleri içerir. TLS parametresi bağlayıcıya göre değişir, örneğin "ssl=true" veya "sslmode=require" veya "sslmode=gerekli" ve diğer varyasyonlar.

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Uygulamanızın veya çerçevenizin TLS bağlantılarını desteklediğinden emin olun
Veritabanı hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri yükleme sırasında varsayılan olarak TLS'yi etkinleştirmez. PostgreSQL sunucunuz TLS bağlantılarını zorlarsa ancak uygulama TLS için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamayabilir. TLS bağlantılarını nasıl etkinleştirin öğrenin diye başvurunuzun belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>TLS bağlantısı için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir Sertifika Yetkilisi (CA) sertifika dosyasından (.cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL - Hyperscale (Citus) https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pemiçin bir Azure Veritabanına bağlanma sertifikası . Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>psql kullanarak bağlan
Aşağıdaki örnekte, psql komut satırı yardımcı programını kullanarak Hyperscale (Citus) koordinatör düğümünüze nasıl bağlanılalıştırılanın olduğu gösterilmektedir. TLS `sslmode=verify-full` sertifika doğrulamasını zorlamak için bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametreye geçirin.

Aşağıda psql bağlantı dizesinin bir örneği verilmiştir:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Kaydettiğiniz sertifikanın `sslrootcert` dosya yolu yla eşleşen değerin geçtiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
[PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'ndaki Güvenlik Duvarı kurallarıyla](concepts-hyperscale-firewall-rules.md)güvenliği daha da artırın.
