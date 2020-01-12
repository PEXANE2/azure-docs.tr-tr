---
title: SSL-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı için SSL bağlantısının nasıl yapılandırılacağı hakkında yönergeler ve bilgiler-tek sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/27/2019
ms.openlocfilehash: 4f7fd3ea1c83e1bf5183aedf4fe894809884414c
ms.sourcegitcommit: 3eb0cc8091c8e4ae4d537051c3265b92427537fe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75903501"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda SSL bağlantısını yapılandırma-tek sunucu
PostgreSQL için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarınızı PostgreSQL hizmetine bağlamayı tercih eder. Veritabanı sunucunuz ile istemci uygulamalarınız arasında SSL bağlantılarını zorunlu tutma, sunucu ve uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur.

Varsayılan olarak, PostgreSQL veritabanı hizmeti SSL bağlantısı gerektirecek şekilde yapılandırılmıştır. İstemci uygulamanız SSL bağlantısını desteklemiyorsa SSL gerektirmeyi devre dışı bırakabilirsiniz. 

## <a name="enforcing-ssl-connections"></a>SSL bağlantılarını zorlama
Azure portal ve CLı aracılığıyla sağlanan PostgreSQL için tüm Azure veritabanı sunucularına yönelik olarak, SSL bağlantılarının zorlanması varsayılan olarak etkindir. 

Benzer şekilde, Azure portal sunucunuz altındaki "bağlantı dizeleri" ayarlarında önceden tanımlanmış olan bağlantı dizeleri, SSL kullanarak veritabanı sunucunuza bağlanmak için gereken ortak diller için gerekli parametreleri içerir. SSL parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

## <a name="configure-enforcement-of-ssl"></a>SSL zorlamayı yapılandırma
İsteğe bağlı olarak SSL bağlantısı zorlamayı devre dışı bırakabilirsiniz. Microsoft Azure, gelişmiş güvenlik için her zaman **SSL bağlantısını zorla** ayarını etkinleştirmenizi önerir.

> [!NOTE]
> Şu anda PostgreSQL için Azure veritabanı 'nda desteklenen TLS sürümü TLS 1,0, TLS 1,1, TLS 1,2.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma
PostgreSQL için Azure veritabanı sunucunuzu ziyaret edin ve **bağlantı güvenliği**' ne tıklayın. **SSL bağlantısını zorla** ayarını etkinleştirmek veya devre dışı bırakmak için iki durumlu düğmeyi kullanın. Daha sonra **Kaydet**'e tıklayın. 

![Bağlantı güvenliği-SSL zorlamayı devre dışı bırak](./media/concepts-ssl-connection-security/1-disable-ssl.png)

**SSL zorlama durumu** göstergesini görmek Için **genel bakış** sayfasını görüntüleyerek ayarı doğrulayabilirsiniz.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma
Azure CLı 'de sırasıyla `Enabled` veya `Disabled` değerlerini kullanarak **SSL zorlama** parametresini etkinleştirebilir veya devre dışı bırakabilirsiniz.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Uygulamanızın veya çerçevesinin SSL bağlantılarını desteklediğinden emin olun
Veritabanı Hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri, yükleme sırasında varsayılan olarak SSL 'yi etkinleştirmez. PostgreSQL sunucunuz SSL bağlantıları zorluyor, ancak uygulama SSL için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamadan başarısız olabilir. SSL bağlantılarını nasıl etkinleştireceğinizi öğrenmek için uygulamanızın belgelerine başvurun.


## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>SSL bağlantısı için sertifika doğrulaması gerektiren uygulamalar
Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL için Azure veritabanı sunucusuna bağlanma sertifikası https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem konumunda bulunur. Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin. 

### <a name="connect-using-psql"></a>Psql kullanarak bağlanma
Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak PostgreSQL sunucunuza nasıl bağlanılacağını gösterir. SSL sertifikası doğrulamasını zorlamak için `sslmode=verify-full` bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametresine geçirin.

Psql bağlantı dizesinin bir örneği aşağıda verilmiştir:
```
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> `sslrootcert` geçirilen değerin, kaydettiğiniz sertifikanın dosya yoluyla eşleştiğinden emin olun.


## <a name="next-steps"></a>Sonraki adımlar
[PostgreSQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md)'ndaki çeşitli uygulama bağlantısı seçeneklerini gözden geçirin.
