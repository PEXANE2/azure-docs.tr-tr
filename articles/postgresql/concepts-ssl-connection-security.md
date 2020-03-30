---
title: SSL - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: PostgreSQL - Single Server için Azure Veritabanı için SSL bağlantısının nasıl yapılandırılabildiğini anlatan talimatlar ve bilgiler.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 03/10/2020
ms.openlocfilehash: 303da4dcb68a79e69254f6610afc0003bf0aa22c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79477009"
---
# <a name="configure-ssl-connectivity-in-azure-database-for-postgresql---single-server"></a>PostgreSQL için Azure Veritabanında SSL bağlantısını yapılandırma - Tek Sunucu

PostgreSQL için Azure Veritabanı, Güvenli Soketkatmanı (SSL) kullanarak istemci uygulamalarınızı PostgreSQL hizmetine bağlamayı tercih eder. Veritabanı sunucunuz la istemci uygulamalarınız arasındaki SSL bağlantılarını zorlamak, sunucu ile uygulamanız arasındaki veri akışını şifreleyerek "ortadaki adam" saldırılarına karşı korumaya yardımcı olur.

Varsayılan olarak, PostgreSQL veritabanı hizmeti SSL bağlantısı gerektirecek şekilde yapılandırılır. İstemci uygulamanız SSL bağlantısını desteklemiyorsa, SSL gerektiren istemciyi devre dışı bırakmayı seçebilirsiniz.

## <a name="enforcing-ssl-connections"></a>SSL bağlantılarını zorlama

Azure portalı ve CLI aracılığıyla sağlanan PostgreSQL sonrası sunucular için tüm Azure Veritabanı için, Varsayılan olarak SSL bağlantılarının uygulanması etkinleştirilir. 

Benzer şekilde, Azure portalındaki sunucunuzun altındaki "Bağlantı Dizeleri" ayarlarında önceden tanımlanmış bağlantı dizeleri, ortak dillerin SSL kullanarak veritabanı sunucunuza bağlanması için gerekli parametreleri içerir. SSL parametresi bağlayıcıya göre değişir, örneğin "ssl=true" veya "sslmode=require" veya "sslmode=gerekli" ve diğer varyasyonlar.

## <a name="configure-enforcement-of-ssl"></a>SSL'nin Yapılandırma Uygulaması

SSL bağlantısını zorlamayı isteğe bağlı olarak devre dışı kullanabilirsiniz. Microsoft Azure, gelişmiş güvenlik için **SSL bağlantı** ayarını her zaman etkinleştirmenizi önerir.

### <a name="using-the-azure-portal"></a>Azure portalını kullanma

PostgreSQL sunucusu için Azure Veritabanınızı ziyaret edin ve **Bağlantı güvenliğini**tıklatın. **SSL bağlantı** ayarını etkinleştirmek veya devre dışı katmak için geçiş düğmesini kullanın. Ardından **Kaydet'i**tıklatın.

![Bağlantı Güvenliği - Enforce SSL'yi devre dışı](./media/concepts-ssl-connection-security/1-disable-ssl.png)

**SSL zorunlu durum** göstergesini görmek için **Genel Bakış** sayfasını görüntüleyerek ayarı onaylayabilirsiniz.

### <a name="using-azure-cli"></a>Azure CLI’yı kullanma

Azure CLI'de sırasıyla **ssl zorlama** `Enabled` parametresini veya değerleri kullanarak etkinleştirebilir veya `Disabled` devre dışı bırakabilirsiniz.

```azurecli
az postgres server update --resource-group myresourcegroup --name mydemoserver --ssl-enforcement Enabled
```

## <a name="ensure-your-application-or-framework-supports-ssl-connections"></a>Uygulamanızın veya çerçevenizin SSL bağlantılarını desteklediğinden emin olun

Veritabanı hizmetleri için PostgreSQL kullanan bazı uygulama çerçeveleri yükleme sırasında varsayılan olarak SSL'yi etkinleştirmez. PostgreSQL sunucunuz SSL bağlantılarını zorlarsa ancak uygulama SSL için yapılandırılmamışsa, uygulama veritabanı sunucunuza bağlanamayabilir. SSL bağlantılarını nasıl etkinleştireceklerini öğrenmek için başvurunuzun belgelerine başvurun.

## <a name="applications-that-require-certificate-verification-for-ssl-connectivity"></a>SSL bağlantısı için sertifika doğrulaması gerektiren uygulamalar

Bazı durumlarda, uygulamalar güvenli bir şekilde bağlanmak için güvenilir bir Sertifika Yetkilisi (CA) sertifika dosyasından (.cer) oluşturulan yerel bir sertifika dosyası gerektirir. PostgreSQL sunucusu için bir Azure Veritabanına bağlanma https://www.digicert.com/CACerts/BaltimoreCyberTrustRoot.crt.pemsertifikası . Sertifika dosyasını indirin ve tercih ettiğiniz konuma kaydedin.

### <a name="connect-using-psql"></a>psql kullanarak bağlan

Aşağıdaki örnekte, psql command-line yardımcı programını kullanarak PostgreSQL sunucunuza nasıl bağlanılalıyorum gösterilmektedir. SSL sertifika doğrulamasını zorlamak için `sslmode=verify-full` bağlantı dizesi ayarını kullanın. Yerel sertifika dosya yolunu `sslrootcert` parametreye geçirin.

Aşağıdaki komut psql bağlantı dizesinin bir örneğidir:

```shell
psql "sslmode=verify-full sslrootcert=BaltimoreCyberTrustRoot.crt host=mydemoserver.postgres.database.azure.com dbname=postgres user=myusern@mydemoserver"
```

> [!TIP]
> Kaydettiğiniz sertifikanın `sslrootcert` dosya yolu yla eşleşen değerin geçtiğini doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

[PostgreSQL için Azure Veritabanı için Bağlantı kitaplıklarında](concepts-connection-libraries.md)çeşitli uygulama bağlantısı seçeneklerini gözden geçirin.
