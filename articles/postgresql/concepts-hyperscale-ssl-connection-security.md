---
title: SSL - Hyperscale (Citus) - PostgreSQL için Azure Veritabanı
description: SSL bağlantılarını düzgün kullanmak için PostgreSQL - Hyperscale (Citus) ve ilişkili uygulamalar için Azure Veritabanı'nı yapılandırma kullanabilirsiniz.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.topic: conceptual
ms.date: 09/17/2019
ms.openlocfilehash: 3e4ef5d2d6db3a3d4f8923f47079f2484639a751
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74973994"
---
# <a name="configure-ssl-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL - Hyperscale (Citus) için Azure Veritabanında SSL'yi yapılandırma
Hyperscale (Citus) koordinatör düğümüne istemci uygulama bağlantıları güvenli soketkatmanı (SSL) gerektirir. Veritabanı sunucunuz la istemci uygulamalarınız arasındaki SSL bağlantılarını zorlamak, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korumaya yardımcı olur.

## <a name="enforcing-ssl-connections"></a>SSL bağlantılarını zorlama
Azure portalı üzerinden sağlanan PostgreSQL sunucuları için tüm Azure Veritabanı için, SSL bağlantılarının uygulanması varsayılan olarak etkinleştirilir. 

Benzer şekilde, Azure portalındaki sunucunuzun altındaki "Bağlantı Dizeleri" ayarlarında önceden tanımlanmış bağlantı dizeleri, ortak dillerin SSL kullanarak veritabanı sunucunuza bağlanması için gerekli parametreleri içerir. SSL parametresi bağlayıcıya göre değişir, örneğin "ssl=true" veya "sslmode=require" veya "sslmode=gerekli" ve diğer varyasyonlar.

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Uygulamanızın veya çerçevenizin SSL bağlantılarını desteklediğinden emin olun
Veritabanı hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri yükleme sırasında varsayılan olarak SSL'yi etkinleştirmez. PostgreSQL sunucunuz SSL bağlantılarını zorlarsa ancak uygulama SSL için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamayabilir. SSL bağlantılarını nasıl etkinleştireceklerini öğrenmek için başvurunuzun belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>SSL bağlantısı için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir Sertifika Yetkilisi (CA) sertifika dosyasından (.cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL - Hyperscale (Citus) https://dl.cacerts.digicert.com/DigiCertGlobalRootCA.crt.pemiçin bir Azure Veritabanına bağlanma sertifikası . Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>psql kullanarak bağlan
Aşağıdaki örnekte, psql komut satırı yardımcı programını kullanarak Hyperscale (Citus) koordinatör düğümünüze nasıl bağlanılalıştırılanın olduğu gösterilmektedir. SSL sertifika doğrulamasını zorlamak için `sslmode=verify-full` bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametreye geçirin.

Aşağıda psql bağlantı dizesinin bir örneği verilmiştir:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Kaydettiğiniz sertifikanın `sslrootcert` dosya yolu yla eşleşen değerin geçtiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar
[PostgreSQL - Hyperscale (Citus) için Azure Veritabanı'ndaki Güvenlik Duvarı kurallarıyla](concepts-hyperscale-firewall-rules.md)güvenliği daha da artırın.
