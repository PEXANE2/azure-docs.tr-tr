---
title: Apache Kafka için Azure Event Hubs 'e geçiş
description: Bu makalede, Azure Event Hubs kullanılırken farklı protokoller (AMQP, Apache Kafka ve HTTPS) kullanan tüketiciler ve üreticileri olayları nasıl değiş tokuş edebilir gösterilmektedir.
ms.topic: article
ms.date: 06/23/2020
ms.openlocfilehash: 8f6c4cbdcbbc1d589b0803f36305f9a9fe6eebfa
ms.sourcegitcommit: 01cd19edb099d654198a6930cebd61cae9cb685b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/24/2020
ms.locfileid: "85322734"
---
# <a name="migrate-to-azure-event-hubs-for-apache-kafka-ecosystems"></a>Apache Kafka ekosistemleri için Azure Event Hubs 'e geçiş
Azure Event Hubs, Kafka protokolünü kullanarak Event Hubs bağlanmanızı sağlayan bir Apache Kafka uç noktası sunar. Mevcut Kafka uygulamanızda en az değişiklik yaparak Azure Event Hubs bağlanabilir ve Azure ekosisteminin avantajlarından yararlanabilirsiniz. Kafka Apache Kafka desteği için Event Hubs [sürüm 1,0](https://kafka.apache.org/10/documentation.html) ve üzeri.

## <a name="pre-migration"></a>Geçiş öncesi 

### <a name="create-an-azure-account"></a>Azure hesabı oluşturma
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.

### <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma
Event Hubs ad alanı ve Olay Hub 'ı oluşturmak için [Olay Hub 'ı oluşturma](event-hubs-create.md) makalesindeki adım adım yönergeleri izleyin. 

### <a name="connection-string"></a>Bağlantı dizesi
[Portaldan bağlantı dizesini al](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) makalesindeki adımları izleyin. Ve daha sonra kullanmak için bağlantı dizesini aklınızda edin. 

### <a name="fully-qualified-domain-name-fqdn"></a>Tam etki alanı adı (FQDN)
Ayrıca, Olay Hub 'ı ad alanınızı işaret eden FQDN 'ye de ihtiyacınız olabilir. FQDN, bağlantı dizeniz içinde aşağıdaki gibi bulunabilir:

`Endpoint=sb://`**`mynamespace.servicebus.windows.net`**`/;SharedAccessKeyName=XXXXXX;SharedAccessKey=XXXXXX`

Event Hubs ad alanınız genel olmayan bir buluta dağıtılmışsa, etki alanı adınız farklı olabilir (örneğin, \* . ServiceBus.chinacloudapi.cn, \* . ServiceBus.usgovcloudapi.NET veya \* . ServiceBus.cloudapi.de).

## <a name="migration"></a>Geçiş 

### <a name="update-your-kafka-client-configuration"></a>Kafka istemci yapılandırmanızı güncelleştirme

Kafka etkin bir olay hub 'ına bağlanmak için, Kafka istemci yapılandırmasını güncelleştirmeniz gerekir. Sizinki bulmakta sorun yaşıyorsanız, uygulamanızda nerede ayarlandığını aramayı deneyin `bootstrap.servers` .

Uygulamanızda anlatacakları her yerde aşağıdaki yapılandırmaları ekleyin. `bootstrap.servers` `sasl.jaas.config` İstemcisini, doğru kimlik doğrulamasıyla Event Hubs Kafka uç noktasına yönlendirmek için ve değerlerini güncelleştirdiğinizden emin olun. 

```
bootstrap.servers={MYNAMESPACE}.servicebus.windows.net:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING TO YOUR NAMESPACE}";
``` 

`sasl.jaas.config`Ortamınızda desteklenen bir yapılandırma değilse, SASL Kullanıcı adı ve parolasını ayarlamak için kullanılan yapılandırmaları bulun ve bunun yerine bunları kullanın. Kullanıcı adını `$ConnectionString` ve parolayı Event Hubs bağlantı dizeniz olarak ayarlayın.

## <a name="post-migration"></a>Geçiş sonrası
Olayları Olay Hub 'ına gönderen Kafka uygulamanızı çalıştırın. Daha sonra, Olay Hub 'ının Azure portal kullanarak olayları aldığını doğrulayın. Event Hubs ad alanının **genel bakış** sayfasında, **ölçümler** bölümünde **iletiler** görünümüne geçin. Grafiği güncelleştirmek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir. 

[![Olay Hub 'ının iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)


## <a name="next-steps"></a>Sonraki adımlar
Kafka için Event Hubs ve Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

- [Event Hubs için sorun giderme kılavuzu Apache Kafka](apache-kafka-troubleshooting-guide.md)
- [Sık Sorulan Sorular-Apache Kafka için Event Hubs](apache-kafka-frequently-asked-questions.md)
- [Azure Event Hubs için Apache Kafka Geliştirici Kılavuzu](apache-kafka-developer-guide.md)
- [Önerilen yapılandırma](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md)