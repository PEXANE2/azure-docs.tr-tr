---
title: SSL-PostgreSQL için Azure veritabanı-tek sunucu
description: PostgreSQL için Azure veritabanı için SSL bağlantısının nasıl yapılandırılacağı hakkında yönergeler ve bilgiler-tek sunucu.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: c235562834ae78a12b690fcd1b96d6a3640e0c66
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371673"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure veritabanı 'nda SSL bağlantısını yapılandırma-tek sunucu

PostgreSQL için Azure veritabanı, Güvenli Yuva Katmanı (SSL) kullanarak istemci uygulamalarınızı PostgreSQL hizmetine bağlamayı tercih eder. Veritabanı sunucunuz ile istemci uygulamalarınız arasında SSL bağlantılarını zorunlu tutma, sunucu ve uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korunmaya yardımcı olur.

Varsayılan olarak, PostgreSQL veritabanı hizmeti SSL bağlantısı gerektirecek şekilde yapılandırılmıştır. İstemci uygulamanız SSL bağlantısını desteklemiyorsa SSL gerektirmeyi devre dışı bırakabilirsiniz.

## <a name="enforcing-ssl-connections"></a>SSL bağlantılarını zorlama

Azure portal ve CLı aracılığıyla sağlanan PostgreSQL için tüm Azure veritabanı sunucularına yönelik olarak, SSL bağlantılarının zorlanması varsayılan olarak etkindir. 

Benzer şekilde, Azure portal sunucunuz altındaki "bağlantı dizeleri" ayarlarında önceden tanımlanmış olan bağlantı dizeleri, SSL kullanarak veritabanı sunucunuza bağlanmak için gereken ortak diller için gerekli parametreleri içerir. SSL parametresi bağlayıcıya göre değişir; Örneğin, "SSL = true" veya "sslmode = gerektir" veya "sslmode = Required" ve diğer Çeşitlemeler.

## <a name="configure-enforcement-of-ssl"></a>SSL zorlamayı yapılandırma

İsteğe bağlı olarak SSL bağlantısı zorlamayı devre dışı bırakabilirsiniz. Microsoft Azure, gelişmiş güvenlik için her zaman **SSL bağlantısını zorla** ayarını etkinleştirmenizi önerir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

PostgreSQL için Azure veritabanı sunucunuzu ziyaret edin ve **bağlantı güvenliği**' ne tıklayın. **SSL bağlantısını zorla** ayarını etkinleştirmek veya devre dışı bırakmak için iki durumlu düğmeyi kullanın. Ardından **Kaydet**' e tıklayın.

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

Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir sertifika yetkilisi (CA) sertifika dosyasından (. cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL için Azure veritabanı sunucusuna bağlanma sertifikası https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pemkonumunda bulunur. Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>Psql kullanarak bağlanma

Aşağıdaki örnek, psql komut satırı yardımcı programını kullanarak PostgreSQL sunucunuza nasıl bağlanılacağını gösterir. SSL sertifikası doğrulamasını zorlamak için `sslmode=verify-full` bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametresine geçirin.

Aşağıdaki komut, psql bağlantı dizesinin bir örneğidir:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> `sslrootcert` geçirilen değerin, kaydettiğiniz sertifikanın dosya yoluyla eşleştiğinden emin olun.

## <a name="tls-connectivity-in-azure-database-for-postgresql-single-server"></a>PostgreSQL için Azure veritabanı 'nda TLS bağlantısı tek sunucu

PostgreSQL için Azure veritabanı-tek sunucu, Aktarım Katmanı Güvenliği (TLS) kullanarak veritabanı sunucunuza bağlanan istemciler için şifrelemeyi destekler. TLS, veritabanı sunucunuz ile istemci uygulamalarınız arasında güvenli ağ bağlantıları sağlayan ve uyumluluk gereksinimlerine bağlı olmanızı sağlayan bir endüstri standardı protokolüdür.

### <a name="tls-settings"></a>TLS ayarları

Müşteriler artık, PostgreSQL için Azure veritabanı 'na bağlanan istemci için TLS sürümünü zorunlu kılabilir. TLS seçeneğini kullanmak için **En düşük TLS sürümü** seçenek ayarını kullanın. Bu seçenek ayarı için aşağıdaki değerlere izin verilir:

|  Minimum TLS ayarı             | Desteklenen TLS sürümü                |
|:---------------------------------|-------------------------------------:|
| TLSEnforcementDisabled (varsayılan) | TLS gerekli değil                      |
| TLS1_0                           | TLS 1,0, TLS 1,1, TLS 1,2 ve üzeri |
| TLS1_1                           | TLS 1,1, TLS 1,2 ve üzeri          |
| TLS1_2                           | TLS sürüm 1,2 ve üzeri           |


Örneğin, bu en düşük TLS ayarı sürümünü TLS 1,0 olarak ayarlamak, sunucunuzun TLS 1,0, 1,1 ve 1.2 + kullanarak istemcilerden gelen bağlantılara izin vermeyeceği anlamına gelir. Alternatif olarak, bunu 1,2 olarak ayarlamak yalnızca TLS 1,2 kullanarak istemcilerden gelen bağlantılara izin vertiğinizi ve TLS 1,0 ve TLS 1,1 ile tüm bağlantıların reddedildiğini belirtir.

> [!Note] 
> PostgreSQL için Azure veritabanı tek sunuculu varsayılan olarak tüm yeni sunucular için TLS devre dışı bırakılıyor.
>
> Şu anda TLS sürümleri PostgreSQL için desteklenen byAzure veritabanı, TLS 1,0, 1,1 ve 1,2.

PostgreSQL için Azure veritabanı tek sunuculu TLS ayarını nasıl ayarlayacağınızı öğrenmek için bkz. [TLS yapılandırma ayarı](howto-tls-configurations.md).

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL Için Azure veritabanı bağlantı kitaplıkları](concepts-connection-libraries.md)'ndaki çeşitli uygulama bağlantısı seçeneklerini gözden geçirin.
