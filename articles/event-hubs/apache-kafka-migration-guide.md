---
title: Apache Kafka için Azure Etkinlik Hub'larına geçiş
description: Bu makalede, farklı protokoller (AMQP, Apache Kafka ve HTTPS) kullanan tüketicilerin ve üreticilerin Azure Etkinlik Hub'larını kullanırken etkinlik alışverişi nasıl yapabildiği gösterilmektedir.
services: event-hubs
documentationcenter: ''
author: ShubhaVijayasarathy
manager: ''
ms.service: event-hubs
ms.devlang: na
ms.topic: article
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 04/01/2020
ms.author: shvija
ms.openlocfilehash: 32b08e565b86af8f6373c9848211646128bb346d
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81677363"
---
# <a name="migrate-to-azure-event-hubs-for-apache-kafka-ecosystems"></a>Apache Kafka Ekosistemleri için Azure Etkinlik Hub'larına geçiş
Azure Etkinlik Hub'ları, Kafka protokolünü kullanarak Olay Hub'larına bağlanmanızı sağlayan bir Apache Kafka bitiş noktasını ortaya çıkarır. Mevcut Kafka uygulamanızda en az değişiklik yaparak Azure Etkinlik Hub'larına bağlanabilir ve Azure ekosisteminden yararlanabilirsiniz. Kafka için Etkinlik Hub'ları [Apache Kafka sürüm 1.0](https://kafka.apache.org/10/documentation.html) ve sonrası sürümlerini destekler.

## <a name="pre-migration"></a>Geçiş öncesi 

### <a name="create-an-azure-account"></a>Azure hesabı oluşturma
Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) bir hesap oluşturun.

### <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma
Olay Hub'ları ad alanı ve olay hub'ı oluşturmak için [olay hub'ı](event-hubs-create.md) oluştur makalesinde adım adım yönergeleri izleyin. 

### <a name="connection-string"></a>Bağlantı dizesi
Portal makalesinden [bağlantı al dizesindeki](event-hubs-get-connection-string.md#get-connection-string-from-the-portal) adımları izleyin. Ve, daha sonra kullanmak için bağlantı dizesini not edin. 

### <a name="fully-qualified-domain-name-fqdn"></a>Tam nitelikli alan adı (FQDN)
Ayrıca, Olay Hub'ınızın ad alanını gösteren FQDN'ye de ihtiyacınız olabilir. FQDN bağlantı dizenizde aşağıdaki gibi bulunabilir:

`Endpoint=sb://`**`mynamespace.servicebus.windows.net`**`/;SharedAccessKeyName=XXXXXX;SharedAccessKey=XXXXXX`

Olay Hub'larınız genel olmayan bir bulutta dağıtılırsa, etki alanı adınız \*farklı \*olabilir (örneğin, .servicebus.chinacloudapi.cn, .servicebus.usgovcloudapi.net veya \*.servicebus.cloudapi.de).

## <a name="migration"></a>Geçiş 

### <a name="update-your-kafka-client-configuration"></a>Kafka istemci yapılandırmanızı güncelleyin

Kafka özellikli bir Etkinlik Hub'ına bağlanmak için Kafka istemci yapılandırmalarını güncelleştirmeniz gerekir. Kendi uygulamanızı bulmakta sorun yaşıyorsanız, `bootstrap.servers` uygulamanızda nerede ayarlandığını aramayı deneyin.

Uygulamanızda anlamlı olan her yere aşağıdaki configs ekleyin. İstemciyi `bootstrap.servers` doğru `sasl.jaas.config` kimlik doğrulamasıyla Olay Hub'larınız Kafka bitiş noktasına yönlendirmek için ve değerleri güncelleştirdiğinizden emin olun. 

```
bootstrap.servers={MYNAMESPACE}.servicebus.windows.net:9093
request.timeout.ms=60000
security.protocol=SASL_SSL
sasl.mechanism=PLAIN
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{CONNECTION STRING TO YOUR NAMESPACE}";
``` 

Çerçevenizde desteklenen bir yapılandırma `sasl.jaas.config` değilse, SASL kullanıcı adı ve parolasını ayarlamak için kullanılan yapılandırmaları bulun ve bunun yerine bunları kullanın. Kullanıcı adını `$ConnectionString` ve parolayı Olay Hub'ları bağlantı dizenize ayarlayın.

## <a name="post-migration"></a>Geçiş sonrası
Olayları etkinlik merkezine gönderen Kafka uygulamanızı çalıştırın. Ardından, etkinlik merkezinin olayları Azure portalını kullanarak aldığını doğrulayın. Olay Hub'larınızın ad alanının **Genel Bakış** sayfasında, **Ölçümler** bölümündeki **İletiler** görünümüne geçin. Grafiği güncellemek için sayfayı yenileyin. İletilerin alındığını göstermesi birkaç saniye sürebilir. 

[![Olay merkezinin iletileri aldığını doğrulama](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png)](./media/getstarted-dotnet-standard-send-v2/verify-messages-portal.png#lightbox)


## <a name="next-steps"></a>Sonraki adımlar
Kafka için Etkinlik Hub'ları ve Etkinlik Hub'ları hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:  

- [Etkinlik Hub'ları için Apache Kafka sorun giderme kılavuzu](apache-kafka-troubleshooting-guide.md)
- [Sık sorulan sorular - Apache Kafka için Etkinlik Hub'ları](apache-kafka-frequently-asked-questions.md)
- [Azure Etkinlik Hub'ları için Apache Kafka geliştirici kılavuzu](apache-kafka-developer-guide.md)
- [Önerilen yapılandırmalar](https://github.com/Azure/azure-event-hubs-for-kafka/blob/master/CONFIGURATION.md)