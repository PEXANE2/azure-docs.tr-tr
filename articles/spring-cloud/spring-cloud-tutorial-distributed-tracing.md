---
title: Azure Spring Cloud ile dağıtılmış Izleme kullanma
description: Spring Cloud 'ın dağıtılmış izlemesini Azure 'da nasıl kullanacağınızı öğrenin Application Insights
author: bmitchell287
ms.service: spring-cloud
ms.topic: how-to
ms.date: 10/06/2019
ms.author: brendm
ms.openlocfilehash: ccaf58465c1ade0228daea2b535d06fb6168d64f
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86142119"
---
# <a name="use-distributed-tracing-with-azure-spring-cloud"></a>Azure Spring Cloud ile dağıtılmış izleme kullanma

Azure Spring Cloud 'daki dağıtılmış izleme araçlarıyla, karmaşık sorunları kolayca ayıklayabilir ve izleyebilirsiniz. Azure Spring Cloud, Azure 'ın [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) [yay bulutuna](https://spring.io/projects/spring-cloud-sleuth) karşı tümleşir. Bu tümleştirme Azure portal güçlü dağıtılmış izleme yeteneği sağlar.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Azure portal dağıtılmış izlemeyi etkinleştirin.
> * Uygulamanıza yay bulutu uykuya geçin.
> * Mikro hizmet uygulamalarınız için bağımlılık haritalarını görüntüleyin.
> * Farklı filtrelerle izleme verilerinde arama yapın.

## <a name="prerequisites"></a>Ön koşullar

Bu yordamları izlemek için, zaten sağlanmış ve çalışan bir Azure yay bulut hizmetine ihtiyacınız vardır. Azure yay bulut hizmeti sağlamak ve çalıştırmak için [Azure CLI aracılığıyla uygulama dağıtma hızlı](spring-cloud-quickstart-launch-app-cli.md) başlangıcını doldurun.
    
## <a name="add-dependencies"></a>Bağımlılık Ekle

1. Aşağıdaki satırı Application. Properties dosyasına ekleyin:

   ```xml
   spring.zipkin.sender.type = web
   ```

   Bu değişiklikten sonra, Zipkabağı gönderici Web 'e gönderebilir.

1. [Azure yay bulut uygulaması hazırlama kılavuzumuzu](spring-cloud-tutorial-prepare-app-deployment.md)izlediyseniz bu adımı atlayın. Aksi takdirde, yerel geliştirme ortamınıza gidin ve pom.xml dosyanızı düzenleyerek aşağıdaki yay bulutu uykuya geçme bağımlılığını ekleyin:

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

1. Bu değişiklikleri yansıtacak şekilde Azure Spring Cloud Service için derleyin ve dağıtın.

## <a name="modify-the-sample-rate"></a>Örnek hızını değiştirme

Örnek hızını değiştirerek telemetrinizin toplandığı oranı değiştirebilirsiniz. Örneğin, çoğunlukla yarı bir örnek örneklemek istiyorsanız, Application. Properties dosyanızı açın ve aşağıdaki satırı değiştirin:

```xml
spring.sleuth.sampler.probability=0.5
```

Zaten bir uygulama oluşturup dağıttıysanız, örnek hızı değiştirebilirsiniz. Önceki satırı Azure CLı veya Azure portal bir ortam değişkeni olarak ekleyerek bunu yapın.

## <a name="enable-application-insights"></a>Application Insights'ı etkinleştirme

1. Azure portal Azure Spring Cloud Service sayfanıza gidin.
1. **İzleme** sayfasında, **Dağıtılmış izleme**' yi seçin.
1. Düzenlemek veya yeni bir ayar eklemek için **ayarı Düzenle** ' yi seçin.
1. Yeni bir Application Insights sorgusu oluşturun veya var olan bir sorgu seçin.
1. İzlemek istediğiniz günlüğe kaydetme kategorisini seçin ve bekletme süresini gün olarak belirtin.
1. Yeni izlemeyi uygulamak için **Uygula** ' yı seçin.

## <a name="view-the-application-map"></a>Uygulama haritasını görüntüleme

**Dağıtılmış izleme** sayfasına dönün ve **uygulama haritasını görüntüle**' yi seçin. Uygulamanızın ve izleme ayarlarının görsel temsilini gözden geçirin. Uygulama haritasını nasıl kullanacağınızı öğrenmek için bkz. [uygulama Haritası: dağıtılmış uygulamaları önceliklendirme](https://docs.microsoft.com/azure/azure-monitor/app/app-map).

## <a name="use-search"></a>Aramayı Kullanma

Diğer belirli telemetri öğelerini sorgulamak için Search işlevini kullanın. **Dağıtılmış izleme** sayfasında, **Ara**' yı seçin. Arama işlevini kullanma hakkında daha fazla bilgi için bkz. [Application Insights arama kullanma](https://docs.microsoft.com/azure/azure-monitor/app/diagnostic-search).

## <a name="use-application-insights"></a>Application Insights kullan

Application Insights, uygulama eşlemesi ve arama işlevine ek olarak izleme özellikleri sağlar. Azure portal uygulamanızın adı için arama yapın ve ardından izleme bilgilerini bulmak için bir Application Insights sayfası açın. Bu araçların nasıl kullanılacağına dair daha fazla bilgi için [Azure izleyici günlük sorgularını](https://docs.microsoft.com/azure/azure-monitor/log-query/query-language)inceleyin.

## <a name="disable-application-insights"></a>Application Insights devre dışı bırak

1. Azure portal Azure Spring Cloud Service sayfanıza gidin.
1. **İzleme**sırasında **Dağıtılmış izleme**' yi seçin.
1. Application Insights devre dışı bırakmak için **Disable** ' ı seçin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Azure Spring Cloud 'da dağıtılmış izlemeyi etkinleştirme ve anlama hakkında bilgi edindiniz. Bir uygulamaya hizmet bağlama hakkında bilgi edinmek için bkz. [Azure Spring Cloud uygulamasına Azure Cosmos db veritabanını](spring-cloud-tutorial-bind-cosmos.md)bağlama.
