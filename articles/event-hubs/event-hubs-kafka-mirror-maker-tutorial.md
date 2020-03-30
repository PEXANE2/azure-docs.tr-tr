---
title: Apache Kafka MirrorMaker kullanın - Azure Etkinlik Hub'ları | Microsoft Dokümanlar
description: Bu makalede, AzureEvent Hub'larında Kafka kümesini yansıtmak için Kafka MirrorMaker'ın nasıl kullanılacağı hakkında bilgi verilmektedir.
services: event-hubs
documentationcenter: .net
author: ShubhaVijayasarathy
manager: timlt
ms.service: event-hubs
ms.topic: conceptual
ms.custom: seodec18
ms.date: 12/06/2018
ms.author: shvija
ms.openlocfilehash: 6dc902b6a26c175713381b4fce88934dca3f409e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80283591"
---
# <a name="use-kafka-mirrormaker-with-event-hubs-for-apache-kafka"></a>Apache Kafka için Etkinlik Hub'ları ile Kafka MirrorMaker kullanın

Bu öğretici, Kafka MirrorMaker kullanarak Kafka etkin bir etkinlik hub'ında bir Kafka aracının nasıl yansıtılabildiğini gösterir.

   ![Etkinlik Göbekli Kafka MirrorMaker](./media/event-hubs-kafka-mirror-maker-tutorial/evnent-hubs-mirror-maker1.png)

> [!NOTE]
> Bu örnek [GitHub'da](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker) mevcuttur


Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Kafka kümesi kurma
> * Yapılandırılan Kafka MirrorMaker
> * Koş Kafka Aynacı

## <a name="introduction"></a>Giriş
Modern bulut ölçekli uygulamalar için göz önünde bulundurulması gereken en önemli noktalardan biri, hizmeti kesintiye uğratmadan altyapıyı güncelleştirme, geliştirme ve değiştirme olanağıdır. Bu öğretici, bir etkinlik merkezinin ve Kafka MirrorMaker'ın, Etkinlik Hub'ları hizmetindeki Kafka giriş akışını "yansıtarak" mevcut kafka boru hattını Azure'a nasıl entegre edebileceğini gösterir. 

Azure Etkinlik Hub'ları Kafka bitiş noktası, Kafka protokolünü (kafka istemcileri) kullanarak Azure Etkinlik Hub'larına bağlanmanızı sağlar. Kafka uygulamasında en az değişiklik yaparak Azure Etkinlik Hub'larına bağlanabilir ve Azure ekosisteminin avantajlarından yararlanabilirsiniz. Kafka etkin Event Hubs şu anda Kafka sürümleri 1.0 ve sonrası destekler.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için şunlar sahip olduğunuzdan emin olun:

* [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md) makalesini okuyun. 
* Azure aboneliği. Aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio) oluşturun.
* [Java Development Kit (JDK) 1.7+](https://aka.ms/azure-jdks)
    * Ubuntu’da JDK’yi yüklemek için `apt-get install default-jdk` komutunu çalıştırın.
    * JAVA_HOME ortam değişkenini JDK’nin yüklü olduğu klasöre işaret edecek şekilde ayarladığınızdan emin olun.
* Maven ikili [arşivini indirin](https://maven.apache.org/download.cgi) ve [kurun](https://maven.apache.org/install.html)
    * Ubuntu’da Maven’i yüklemek için `apt-get install maven` komutunu çalıştırabilirsiniz.
* [Git](https://www.git-scm.com/downloads)
    * Ubuntu’da Git’i yüklemek için `sudo apt-get install git` komutunu çalıştırabilirsiniz.

## <a name="create-an-event-hubs-namespace"></a>Event Hubs ad alanı oluşturma

Herhangi bir Event Hubs hizmetinden göndermek ve almak için Event Hubs ad alanı gereklidir. Event Hubs Kafka uç noktası alma yönergeleri için bkz. [Kafka özellikli Event Hubs oluşturma](event-hubs-create.md). Daha sonra kullanmak üzere Olay Hub'ları bağlantı dizesini kopyaladığından emin olun.

## <a name="clone-the-example-project"></a>Örnek projeyi kopyalama

Artık Kafka özellikli Olay Hub'ları bağlantı dizeniz olduğuna göre, Kafka deposu için Azure `mirror-maker` Etkinlik Hub'larını klonla ve alt klasöre gidin:

```shell
git clone https://github.com/Azure/azure-event-hubs-for-kafka.git
cd azure-event-hubs-for-kafka/tutorials/mirror-maker
```

## <a name="set-up-a-kafka-cluster"></a>Kafka kümesi kurma

İstenilen ayarlara (veya varolan kafka kümesini kullanarak) bir küme ayarlamak için [Kafka hızlı başlat kılavuzunu](https://kafka.apache.org/quickstart) kullanın.

## <a name="configure-kafka-mirrormaker"></a>Yapılandırılan Kafka MirrorMaker

Kafka MirrorMaker bir derenin "yansıtmasını" sağlar. Kaynak ve hedef Kafka kümeleri göz önüne alındığında, MirrorMaker kaynak kümeye gönderilen tüm iletilerin hem kaynak hem de hedef kümeler tarafından alınmasını sağlar. Bu örnek, hedef olay hub'ı olan bir kaynak Kafka kümesinin nasıl yansıtılabildiğini gösterir. Bu senaryo, varolan kafka ardışık bir boru hattından olay hub'larına veri akışını kesintiye uğratmadan veri göndermek için kullanılabilir. 

Kafka MirrorMaker hakkında daha ayrıntılı bilgi için [Kafka Yansıtma/MirrorMaker kılavuzuna](https://cwiki.apache.org/confluence/pages/viewpage.action?pageId=27846330)bakın.

Kafka MirrorMaker'ı yapılandırmak için, tüketici/kaynak olarak Kafka kümesi ni ve üretici/hedef olarak bir etkinlik merkezi verin.

#### <a name="consumer-configuration"></a>Tüketici yapılandırması

MirrorMaker'a `source-kafka.config`kaynak Kafka kümesinin özelliklerini söyleyen tüketici yapılandırma dosyasını güncelleştirin.

##### <a name="source-kafkaconfig"></a>kaynak-kafka.config

```
bootstrap.servers={SOURCE.KAFKA.IP.ADDRESS1}:{SOURCE.KAFKA.PORT1},{SOURCE.KAFKA.IP.ADDRESS2}:{SOURCE.KAFKA.PORT2},etc
group.id=example-mirrormaker-group
exclude.internal.topics=true
client.id=mirror_maker_consumer
```

#### <a name="producer-configuration"></a>Üretici yapılandırması

Şimdi, MirrorMaker'a yinelenen (veya "yansıtılmış") verileri Event Hubs hizmetine göndermesini söyleyen üretici yapılandırma dosyasını `mirror-eventhub.config`güncelleştirin. Özellikle, `bootstrap.servers` değiştirin ve `sasl.jaas.config` Olay Hub'ları Kafka bitiş noktasına işaret etmek. Olay Hub'ları hizmeti, aşağıdaki yapılandırmada son üç özelliği ayarlayarak elde edilen güvenli (SASL) iletişim gerektirir: 

##### <a name="mirror-eventhubconfig"></a>ayna-eventhub.config

```
bootstrap.servers={YOUR.EVENTHUBS.FQDN}:9093
client.id=mirror_maker_producer

#Required for Event Hubs
sasl.mechanism=PLAIN
security.protocol=SASL_SSL
sasl.jaas.config=org.apache.kafka.common.security.plain.PlainLoginModule required username="$ConnectionString" password="{YOUR.EVENTHUBS.CONNECTION.STRING}";
```

## <a name="run-kafka-mirrormaker"></a>Koş Kafka Aynacı

Kafka MirrorMaker komut dosyalarını yeni güncellenen yapılandırma dosyalarını kullanarak Kafka dizininden çalıştırın. Config dosyalarını kafka dizininin köküne kopyaladığından veya aşağıdaki komutla yollarını güncelleştirdiğinden emin olun.

```shell
bin/kafka-mirror-maker.sh --consumer.config source-kafka.config --num.streams 1 --producer.config mirror-eventhub.config --whitelist=".*"
```

Olayların etkinlik merkezine ulaştığını doğrulamak için [Azure portalındaki](https://azure.microsoft.com/features/azure-portal/)giriş istatistiklerine bakın veya bir tüketiciyi etkinlik merkezine karşı çalıştırın.

MirrorMaker'ın çalıştırılmasıyla, kaynak Kafka kümesine gönderilen tüm olaylar hem Kafka kümesi hem de aynalı Kafka etkin etkinlik merkezi hizmeti tarafından alınır. MirrorMaker ve Event Hubs Kafka bitiş noktasını kullanarak, varolan kafka ardışık hattını, varolan kümeyi değiştirmeden veya devam eden veri akışını kesintiye uğratmadan yönetilen Azure Etkinlik Hub'ları hizmetine geçirebilirsiniz.

## <a name="samples"></a>Örnekler
GitHub'da aşağıdaki örneklere bakın:

- [GitHub bu öğretici için örnek kod](https://github.com/Azure/azure-event-hubs-for-kafka/tree/master/tutorials/mirror-maker)
- [Azure Konteyner Örneği'nde Çalışan Azure Etkinlik Hub'ları Kafka MirrorMaker](https://github.com/djrosanova/EventHubsMirrorMaker)

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:
> [!div class="checklist"]
> * Event Hubs ad alanı oluşturma
> * Örnek projeyi kopyalama
> * Kafka kümesi kurma
> * Yapılandırılan Kafka MirrorMaker
> * Koş Kafka Aynacı

Event Hubs ve Kafka için Event Hubs hakkında daha fazla bilgi edinmek için aşağıdaki konuya bakın:  

- [Event Hubs hakkında bilgi edinin](event-hubs-what-is-event-hubs.md)
- [Apache Kafka için Event Hubs](event-hubs-for-kafka-ecosystem-overview.md)
- [Olay hub'ı oluşturma](event-hubs-create.md)
- [Kafka uygulamalarınızdan Event Hubs'a akış yapma](event-hubs-quickstart-kafka-enabled-event-hubs.md)
- [Apache Spark'ı bir olay hub'ına bağlama](event-hubs-kafka-spark-tutorial.md)
- [Apache Flink'i bir olay hub'ına bağlama](event-hubs-kafka-flink-tutorial.md)
- [Kafka Connect'i bir etkinlik merkeziyle bütünleştirin](event-hubs-kafka-connect-tutorial.md)
- [Akka Akışlarını bir etkinlik merkezine bağlayın](event-hubs-kafka-akka-streams-tutorial.md)
- [GitHub'ımızdaki örnekleri inceleme](https://github.com/Azure/azure-event-hubs-for-kafka)
