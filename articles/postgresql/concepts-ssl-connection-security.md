---
title: SSL/TLS-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı için TLS bağlantısının nasıl yapılandırılacağı hakkında yönergeler ve bilgiler-tek sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 06/02/2020
ms.openlocfilehash: 9422337d30d06f0bb333e4d25ba1e54bd789ffec
ms.sourcegitcommit: 8e5b4e2207daee21a60e6581528401a96bfd3184
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/04/2020
ms.locfileid: "84418169"
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

**ssl-enforcement** `Enabled` `Disabled` Azure CLI 'de sırasıyla veya değerlerini kullanarak SSL zorlama parametresini etkinleştirebilir veya devre dışı bırakabilirsiniz.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-tls-connections"></a>Uygulamanızın veya çerçevesinin TLS bağlantılarını desteklediğinden emin olun

Veritabanı Hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri, yükleme sırasında varsayılan olarak TLS 'yi etkinleştirmez. PostgreSQL sunucunuz TLS bağlantılarını zorluyor, ancak uygulama TLS için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamadan başarısız olabilir. TLS bağlantılarının nasıl etkinleştirileceğini öğrenmek için uygulamanızın belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-tls-connectivity"></a>TLS bağlantısı için sertifika doğrulaması gerektiren uygulamalar

Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL için Azure veritabanı sunucusuna bağlanma sertifikası konumunda bulunur https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pem . Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>Psql kullanarak bağlanma

Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak PostgreSQL sunucunuza nasıl bağlanılacağını gösterir. `sslmode=verify-full`TLS/SSL sertifika doğrulamasını zorlamak için bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametreye geçirin.

Aşağıdaki komut, psql bağlantı dizesinin bir örneğidir:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Geçirilen değerin, `sslrootcert` kaydettiğiniz sertifikanın dosya yolu ile eşleştiğinden emin olun.

## <a name="tls-enforcement-in-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı 'nda tek sunucu için TLS zorlaması

PostgreSQL için Azure veritabanı-tek sunucu, Aktarım Katmanı Güvenliği (TLS) kullanarak veritabanı sunucunuza bağlanan istemciler için şifrelemeyi destekler. TLS, veritabanı sunucunuz ile istemci uygulamalarınız arasında güvenli ağ bağlantıları sağlayan ve uyumluluk gereksinimlerine bağlı olmanızı sağlayan bir endüstri standardı protokolüdür.

### <a name="tls-settings"></a>TLS ayarları

PostgreSQL için Azure veritabanı tek sunucu, istemci bağlantıları için TLS sürümünü zorlama olanağı sağlar. TLS sürümünü zorlamak için **En düşük TLS sürümü** seçenek ayarını kullanın. Bu seçenek ayarı için aşağıdaki değerlere izin verilir:

|  Minimum TLS ayarı             | İstemci TLS sürümü destekleniyor                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (varsayılan) | TLS gerekli değil                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 ve üzeri |
| TLS1_1                           | TLS 1,1, TLS 1,2 ve üzeri          |
| TLS1_2                           | TLS sürüm 1,2 ve üzeri           |


Örneğin, bu en düşük TLS ayarı sürümünü TLS 1,0 olarak ayarlamak, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1.2 + kullanan istemcilerden gelen bağlantılara izin vermek ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedilecektir.

> [!Note] 
> PostgreSQL için Azure veritabanı, tüm yeni sunucular için varsayılan olarak TLS 'yi devre dışı bırakılıyor.
>
> Şu anda PostgreSQL için Azure veritabanı tarafından desteklenen TLS sürümleri TLS 1,0, 1,1 ve 1,2. Belirli bir minimum TLS sürümüne zorlandıktan sonra, devre dışı olarak değiştiremezsiniz.

PostgreSQL için Azure veritabanı tek sunuculu TLS ayarını nasıl ayarlayacağınızı öğrenmek için bkz. [TLS yapılandırma ayarı](howto-tls-configurations.md).

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md)'ndaki çeşitli uygulama bağlantısı seçeneklerini gözden geçirin.

- [TLS Yapılandırma](howto-tls-configurations.md) hakkında bilgi edinin