---
title: Öğretici - Azure Yay Bulutu ile Dağıtılmış İzleme'yi Kullanma
description: Bu öğretici, Bahar Bulutu'nun Azure Uygulama Öngörüleri aracılığıyla Dağıtılmış İzleme'sini nasıl kullanacağımı gösterir
author: bmitchell287
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 10/06/2019
ms.author: brendm
ms.openlocfilehash: 0815aa084462d1b829d64cd7c5d6fa7cebf534fc
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78273205"
---
# <a name="use-distributed-tracing-with-azure-spring-cloud"></a>Azure İlkbahar Bulutu ile dağıtılmış izleme yi kullanma

Azure Bahar Bulutu'ndaki dağıtılmış izleme araçlarıyla karmaşık sorunları kolayca hata ayıklayabilir ve izleyebilirsiniz. Azure Bulut [Bulutu, Spring Cloud Sleuth'u](https://spring.io/projects/spring-cloud-sleuth) Azure'un [Uygulama Öngörüleri](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview)ile bütünleştirir. Bu tümleştirme, Azure portalından güçlü dağıtılmış izleme özelliği sağlar.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure portalında dağıtılmış izlemeyi etkinleştirin.
> * Uygulamanıza Spring Cloud Sleuth'u ekleyin.
> * Mikro hizmet uygulamalarınız için bağımlılık haritalarını görüntüleyin.
> * Farklı filtrelerle verileri izleme de.

## <a name="prerequisites"></a>Ön koşullar

Bu öğreticiyi tamamlamak için, zaten hazır lanmış ve çalışan bir Azure İlkbahar Bulut hizmetine ihtiyacınız var. Azure İlkbahar Bulut u hizmetini sağlamak ve çalıştırmak için [Azure CLI üzerinden bir uygulamayı dağıtmaya hızlı bir şekilde başlayın.](spring-cloud-quickstart-launch-app-cli.md)
    
## <a name="add-dependencies"></a>Bağımlılıkekleme

1. application.properties dosyasına aşağıdaki satırı ekleyin:

   ```xml
   spring.zipkin.sender.type = web
   ```

   Bu değişiklikten sonra, Zipkin gönderen web'e gönderebilirsiniz.

1. [Azure İlkbahar Bulutu uygulaması hazırlama kılavuzumuzu](spring-cloud-tutorial-prepare-app-deployment.md)takip ettiyseniz bu adımı atlayın. Aksi takdirde, yerel geliştirme ortamınıza gidin ve aşağıdaki Spring Cloud Sleuth bağımlılığını içerecek şekilde pom.xml dosyanızı edin:

    ```xml
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-sleuth</artifactId>
                <version>${spring-cloud-sleuth.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <dependencies>
        <dependency>
            <groupId>org.springframework.cloud</groupId>
            <artifactId>spring-cloud-starter-sleuth</artifactId>
        </dependency>
    </dependencies>
    ```

1. Bu değişiklikleri yansıtmak için Azure İlkbahar Bulutu hizmetiniçin yeniden oluşturun ve dağıtın.

## <a name="modify-the-sample-rate"></a>Örnek oranını değiştirme

Örnek oranını değiştirerek telemetrinizin toplanma hızını değiştirebilirsiniz. Örneğin, yarı sıklıkta örnekleme yapmak istiyorsanız, application.properties dosyanızı açın ve aşağıdaki satırı değiştirin:

```xml
spring.sleuth.sampler.probability=0.5
```

Bir uygulama zaten oluşturup dağıttıysanız, örnek oranını değiştirebilirsiniz. Bunu, Azure CLI veya Azure portalında önceki satırı ortam değişkeni olarak ekleyerek yapın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

1. Azure portalındaki Azure İlkbahar Buluthizmet sayfanıza gidin.
1. **İzleme** sayfasında **Dağıtılmış İzleme'yi**seçin.
1. Yeni **Edit setting** bir ayar ayarlamak veya eklemek için Ayarla'yı seçin.
1. Yeni bir Uygulama Öngörüleri sorgusu oluşturun veya varolan bir sorguyu seçin.
1. İzlemek istediğiniz günlük kategorisini seçin ve gün içinde bekletme süresini belirtin.
1. Yeni izlemeyi uygulamak için **Uygula'yı** seçin.

## <a name="view-the-application-map"></a>Uygulama haritasını görüntüleme

**Dağıtılmış İzleme** sayfasına dönün ve **uygulama haritasını görüntüle'yi**seçin. Uygulamanızın ve izleme ayarlarınızın görsel temsilini gözden geçirin. Uygulama haritasını nasıl kullanacağınızı öğrenmek için Uygulama [Haritası: Triage dağıtılmış uygulamalara](https://docs.microsoft.com/azure/azure-monitor/app/app-map)bakın.

## <a name="use-search"></a>Aramayı Kullanma

Diğer belirli telemetri öğelerini sorgulamak için arama işlevini kullanın. **Dağıtılmış İzleme** sayfasında **Arama'yı**seçin. Arama işlevinin nasıl kullanılacağı hakkında daha fazla bilgi için [bkz.](https://docs.microsoft.com/azure/azure-monitor/app/diagnostic-search)

## <a name="use-application-insights"></a>Uygulama Öngörülerini Kullanma

Application Insights, uygulama haritası ve arama işlevine ek olarak izleme özellikleri sağlar. Uygulamanızın adını bulmak için Azure portalında arama yapın ve izleme bilgilerini bulmak için bir Application Insights sayfasını açın. Bu araçların nasıl kullanılacağı hakkında daha fazla bilgi için [Azure Monitor günlük sorgularına](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language)göz atın.

## <a name="disable-application-insights"></a>Uygulama Öngörülerini Devre Dışı

1. Azure portalındaki Azure İlkbahar Buluthizmet sayfanıza gidin.
1. **İzleme**de, **Dağıtılmış İzleme'yi**seçin.
1. Uygulama Öngörülerini devre **dışı kalım'ı** seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Azure Bahar Bulutu'nda dağıtılmış izlemeyi nasıl etkinleştirdiğinizi ve anlayacağınızı öğrendiniz. Uygulamanızı Azure Cosmos DB veritabanına nasıl bağlatışacağımıöğrenmek için bir sonraki eğitime devam edin.

> [!div class="nextstepaction"]
> [Azure Cosmos DB veritabanına nasıl bağlanıp bağlanılamayı öğrenin](spring-cloud-tutorial-bind-cosmos.md)
