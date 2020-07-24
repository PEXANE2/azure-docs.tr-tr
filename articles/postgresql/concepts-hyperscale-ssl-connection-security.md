---
title: Aktarım Katmanı Güvenliği (TLS)-hiper ölçek (Citus)-PostgreSQL için Azure veritabanı
description: PostgreSQL için Azure veritabanı-hiper ölçek (Citus) ve ilişkili uygulamaları, TLS bağlantılarını düzgün şekilde kullanacak şekilde yapılandırmak için yönergeler ve bilgiler.
author: jonels-msft
ms.author: jonels
ms.service: postgresql
ms.subservice: hyperscale-citus
ms.topic: conceptual
ms.date: 07/16/2020
ms.openlocfilehash: 659f86a107e4b08db4ec5195635ea32d2260d677
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87071467"
---
# <a name="configure-tls-in-azure-database-for-postgresql---hyperscale-citus"></a>PostgreSQL için Azure veritabanı 'nda TLS 'yi yapılandırma-hiper ölçek (Citus)
Hiper ölçek (Citus) düzenleyici düğümü, istemci uygulamalarının Aktarım Katmanı Güvenliği (TLS) ile bağlanmasını gerektirir. Veritabanı sunucusu ile istemci uygulamaları arasında TLS kullanılması, verilerin aktarım sırasında gizli kalmasına yardımcı olur. Aşağıda açıklanan ek doğrulama ayarları Ayrıca "ortadaki adam" saldırılarına karşı koruma sunar.

## <a name="enforcing-tls-connections"></a>TLS bağlantılarını zorlama
Uygulamalar, bir bağlantının hedef veritabanını ve ayarlarını belirlemek için bir "bağlantı dizesi" kullanır. Farklı istemciler farklı ayarlar gerektirir. Ortak istemciler tarafından kullanılan bağlantı dizelerinin listesini görmek için, Azure portal sunucu grubunuzun **bağlantı dizeleri** bölümüne bakın.

TLS parametreleri `ssl` ve `sslmode` bağlayıcının özelliklerine göre farklılık gösterir, örneğin `ssl=true` veya `sslmode=require` veya `sslmode=required` .

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Uygulamanızın veya çerçevesinin TLS bağlantılarını desteklediğinden emin olun
Bazı uygulama çerçeveleri, PostgreSQL bağlantıları için varsayılan olarak TLS 'yi etkinleştirmez. Bununla birlikte, güvenli bağlantı olmadan bir uygulama bir Hyperscale (Citus) düzenleyici düğümüne bağlanamaz. TLS bağlantılarının nasıl etkinleştirileceğini öğrenmek için uygulamanızın belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>TLS bağlantısı için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL için Azure veritabanı 'na bağlanma sertifikası-hiper ölçek (Citus) konumunda bulunur https://cacerts.digicert.com/DigiCertGlobalRootCA.crt.pem . Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

> [!NOTE]
>
> Sertifikanın özgünlüğünü denetlemek için, OpenSSL komut satırı aracını kullanarak SHA-256 parmak izini doğrulayabilirsiniz:
>
> ```sh
> openssl x509 -in DigiCertGlobalRootCA.crt.pem -noout -sha256 -fingerprint
>
> # should output:
> # 43:48:A0:E9:44:4C:78:CB:26:5E:05:8D:5E:89:44:B4:D8:4F:96:62:BD:26:DB:25:7F:89:34:A4:43:C7:01:61
> ```

### <a name="connect-using-psql"></a>Psql kullanarak bağlanma
Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak Hyperscale (Citus) Düzenleyicisi düğümüne nasıl bağlanılacağını gösterir. `sslmode=verify-full`TLS sertifikası doğrulamasını zorlamak için bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametreye geçirin.

Psql bağlantı dizesinin bir örneği aşağıda verilmiştir:
```
psql "sslmode=verify-full sslrootcert=DigiCertGlobalRootCA.crt.pem host=mydemoserver.postgres.database.azure.com dbname=citus user=citus password=your_pass"
```
> [!TIP]
> Geçirilen değerin, `sslrootcert` kaydettiğiniz sertifikanın dosya yolu ile eşleştiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar
[PostgreSQL Için Azure veritabanı-Hyperscale (Citus) güvenlik duvarı kurallarıyla](concepts-hyperscale-firewall-rules.md)güvenliği artırın.
