---
title: TLS-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı için TLS bağlantısının nasıl yapılandırılacağı hakkında yönergeler ve bilgiler-tek sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: d0482e5205b97b5c57c41e0ba98fb9ca819e5d5f
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82141738"
---
# <a name="configure-tls-connectivity-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda TLS bağlantısını yapılandırma-tek sunucu

PostgreSQL için Azure veritabanı, daha önce Güvenli Yuva Katmanı (SSL) olarak bilinen Aktarım Katmanı Güvenliği (TLS) kullanarak istemci uygulamalarınızı PostgreSQL hizmetine bağlamayı tercih eder. Veritabanı sunucunuz ile istemci uygulamalarınız arasında TLS bağlantılarının uygulanması, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur.

Varsayılan olarak, PostgreSQL veritabanı hizmeti TLS bağlantısı gerektirecek şekilde yapılandırılmıştır. İstemci uygulamanız TLS bağlantısını desteklemiyorsa TLS istemeyi devre dışı bırakmayı seçebilirsiniz.

## <a name="enforcing-tls-connections"></a>TLS bağlantılarını zorlama

Azure portal ve CLı aracılığıyla sağlanan PostgreSQL için Azure veritabanı sunucuları için, TLS bağlantılarının uygulanması varsayılan olarak etkindir. 

Benzer şekilde, Azure portal sunucunuz altındaki "bağlantı dizeleri" ayarlarında önceden tanımlanmış olan bağlantı dizeleri, TLS kullanarak veritabanı sunucunuza bağlanmak için gereken ortak diller için gerekli parametreleri içerir. TLS parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

## <a name="configure-enforcement-of-tls"></a>TLS zorlamayı yapılandırma

İsteğe bağlı olarak TLS bağlantısı zorlamayı devre dışı bırakabilirsiniz. Microsoft Azure, gelişmiş güvenlik için her zaman **SSL bağlantısını zorla** ayarını etkinleştirmenizi önerir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

PostgreSQL için Azure veritabanı sunucunuzu ziyaret edin ve **bağlantı güvenliği**' ne tıklayın. **SSL bağlantısını zorla** ayarını etkinleştirmek veya devre dışı bırakmak için iki durumlu düğmeyi kullanın. Ardından **Kaydet**' e tıklayın.

![Bağlantı güvenliği-TLS/SSL uygulanmasını devre dışı bırak](./media/concepts-ssl-connection-security/1-disable-ssl.png)

**SSL zorlama durumu** göstergesini görmek Için **genel bakış** sayfasını görüntüleyerek ayarı doğrulayabilirsiniz.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Azure CLı 'de sırasıyla veya `Enabled` `Disabled` değerlerini kullanarak **SSL zorlama** parametresini etkinleştirebilir veya devre dışı bırakabilirsiniz.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Uygulamanızın veya çerçevesinin TLS bağlantılarını desteklediğinden emin olun

Veritabanı Hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri, yükleme sırasında varsayılan olarak TLS 'yi etkinleştirmez. PostgreSQL sunucunuz TLS bağlantılarını zorluyor, ancak uygulama TLS için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamadan başarısız olabilir. TLS bağlantılarının nasıl etkinleştirileceğini öğrenmek için uygulamanızın belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>TLS bağlantısı için sertifika doğrulaması gerektiren uygulamalar

Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL için Azure veritabanı sunucusuna bağlanma sertifikası konumunda https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pembulunur. Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>Psql kullanarak bağlanma

Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak PostgreSQL sunucunuza nasıl bağlanılacağını gösterir. TLS/ `sslmode=verify-full` SSL sertifika doğrulamasını zorlamak için bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametreye geçirin.

Aşağıdaki komut, psql bağlantı dizesinin bir örneğidir:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Geçirilen değerin, kaydettiğiniz sertifikanın dosya `sslrootcert` yolu ile eşleştiğinden emin olun.

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md)'ndaki çeşitli uygulama bağlantısı seçeneklerini gözden geçirin.
