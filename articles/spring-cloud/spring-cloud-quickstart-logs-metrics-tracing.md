---
title: Hızlı başlangıç-Günlükler, ölçümler ve izleme ile Azure yay bulut uygulamalarını Izleme
description: Günlük akışını, Log Analytics, ölçümleri ve izlemeyi kullanarak Azure Spring Cloud 'daki Palametrik örnek uygulamalarını izleyin.
author: MikeDodaro
ms.author: brendm
ms.service: spring-cloud
ms.topic: quickstart
ms.date: 08/04/2020
ms.custom: devx-track-java
ms.openlocfilehash: 3737c7ec9018bfb7f884eab89a987bcada724483
ms.sourcegitcommit: e69bb334ea7e81d49530ebd6c2d3a3a8fa9775c9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/27/2020
ms.locfileid: "88951747"
---
# <a name="quickstart-monitoring-azure-spring-cloud-apps-with-logs-metrics-and-tracing"></a>Hızlı başlangıç: Günlükler, ölçümler ve izleme ile Azure yay bulut uygulamalarını Izleme

Azure Spring Cloud 'daki yerleşik izleme özelliği sayesinde, karmaşık sorunları kolayca ayıklayabilir ve izleyebilirsiniz. Azure Spring Cloud, Azure 'ın [Application Insights](https://docs.microsoft.com/azure/azure-monitor/app/app-insights-overview) [yay bulutuna](https://spring.io/projects/spring-cloud-sleuth) karşı tümleşir. Bu tümleştirme, Azure portal güçlü Günlükler, ölçümler ve dağıtılmış izleme yeteneği sağlar. Günlük akışını, Log Analytics, ölçümleri ve dağıtılmış Plımetrik uygulamalarıyla dağıtılmış izlemeyi nasıl kullanacağınızı adım adım göstereceğiz.

## <a name="prerequisites"></a>Önkoşullar
Önceki adımları doldurun: 
* [Azure yay bulutu örneği sağlama](spring-cloud-quickstart-provision-service-instance.md)
* [Yapılandırma sunucusunu ayarlama](spring-cloud-quickstart-setup-config-server.md)
* [Uygulamalar oluşturun ve dağıtın](spring-cloud-quickstart-deploy-apps.md).

## <a name="logs"></a>Günlükler
Azure Spring Cloud 'da günlükleri görmenin iki yolu vardır: uygulama örneği başına gerçek zamanlı günlüklerin **günlüğünü** veya Gelişmiş sorgu özelliğine sahip toplanmış günlükler için **Log Analytics** .

### <a name="log-streaming"></a>Günlük akışı

#### <a name="cli"></a>[CLI](#tab/Azure-CLI)

Azure CLı 'de günlük akışını aşağıdaki komutla birlikte kullanabilirsiniz.

```azurecli
az spring-cloud app logs -s <service instance name> -g <resource group name> -n gateway -f
```

Şu şekilde Günlükler görürsünüz:

   [![Azure CLI ](media/spring-cloud-quickstart-logs-metrics-tracing/logs-streaming-cli.png) 'Dan günlük akışı](media/spring-cloud-quickstart-logs-metrics-tracing/logs-streaming-cli.png#lightbox)

>[!TIP]
> `az spring-cloud app logs -h`Daha fazla parametre ve günlük akışı işlevlerini araştırmak için kullanın.

#### <a name="intellij"></a>[IntelliJ](#tab/IntelliJ)

Azure Toolkit for IntelliJ kullanarak günlükleri almak için:
1. **Azure Gezgini**' ni ve ardından **yay bulutu**' nı seçin.
1. Çalışan uygulamaya sağ tıklayın.
1. Açılan listeden **akış günlükleri** ' ni seçin.
    ![Akış günlüklerini seçme](media/spring-cloud-intellij-howto/streaming-logs.png)
1. **Örnek**seçin.

    ![Örnek seçin](media/spring-cloud-intellij-howto/select-instance.png)
1. Akış günlüğü, çıkış penceresinde görünür olur.
    ![Akış günlüğü çıkışı](media/spring-cloud-intellij-howto/streaming-log-output.png)

---
### <a name="log-analytics"></a>Log Analytics

1. **Hizmete git | Genel Bakış** sayfası ve **Izleme** bölümünde **Günlükler** ' i seçin. Azure yay bulutu için örnek sorgulardan birinde **Çalıştır** ' a tıklayın. 

    [![Günlük Analizi girişi ](media/spring-cloud-quickstart-logs-metrics-tracing/logs-entry.png)](media/spring-cloud-quickstart-logs-metrics-tracing/logs-entry.png#lightbox)
    
1. Ardından filtrelenmiş günlükleri görürsünüz. Sorgu yazma hakkında daha fazla bilgi için bkz. [Azure Log Analytics docs](https://docs.microsoft.com/azure/azure-monitor/log-query/get-started-queries) .

    [![Günlük Analizi sorgusu ](media/spring-cloud-quickstart-logs-metrics-tracing/logs-query.png)](media/spring-cloud-quickstart-logs-metrics-tracing/logs-query.png#lightbox)

## <a name="metrics"></a>Ölçümler
1. **Hizmete git | Genel Bakış** sayfası ve **Izleme** bölümünde **ölçümler** ' i seçin. `system.cpu.usage` **Ölçüm** için ' i seçerek ve `Avg` toplam CPU kullanımı için zaman çizelgesini görmek üzere **toplama** için ' i seçerek ilk ölçüsünü ekleyin.

    [![Ölçüm girişi ](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-basic-cpu.png)](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-basic-cpu.png#lightbox)
    
1. Yukarıdaki araç çubuğundan **Filtre Ekle** ' ye tıklayın, `App=Gateway` yalnızca **ağ geçidi** uygulamasının CPU kullanımını görmek için seçin.

    [![Ölçümlerde ](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-filter.png) filtre kullanma](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-filter.png#lightbox)

1. Yukarıda oluşturulan filtreyi kapatın, **bölmeyi Uygula** ' ya tıklayın ve `App` farklı uygulamalar tarafından CPU kullanımını görmek için **değerler** ' i seçin.

    [![Ölçümlere ](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-split.png) bölme Uygula](media/spring-cloud-quickstart-logs-metrics-tracing/metrics-split.png#lightbox)

## <a name="distributed-tracing"></a>Dağıtılmış Izleme
1. **Hizmete git | Genel Bakış** sayfası ve **Izleme** bölümünde **Dağıtılmış izleme** ' yi seçin. Ardından sağdaki **uygulama haritasını görüntüle** sekmesine tıklayın.

    [![Dağıtılmış izleme girdisi ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-entry.png)](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-entry.png#lightbox)

1. Artık, Palametrik uygulamalar arasındaki çağrıların durumunu görebilirsiniz. 

    [![Dağıtılmış izlemeye genel bakış ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-overview.png)](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-overview.png#lightbox)
    
1. HTTP yöntemlerine göre en yavaş çağrılar gibi daha fazla ayrıntı görmek için **ağ geçidi** ve **Hesap-hizmet** arasındaki bağlantıya tıklayın.

    [![Dağıtılmış izleme ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-call.png)](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-call.png#lightbox)
    
1. Son olarak, daha güçlü yerleşik performans analizini araştırmak için **performansı araştır** ' a tıklayın.

    [![Dağıtılmış izleme performansı ](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-performance.png)](media/spring-cloud-quickstart-logs-metrics-tracing/tracing-performance.png#lightbox)

## <a name="clean-up-resources"></a>Kaynakları temizleme
Önceki adımlarda, bir kaynak grubunda Azure kaynakları oluşturdunuz. Gelecekte bu kaynaklara ihtiyaç duymazsanız, kaynak grubunu portaldan silin veya Cloud Shell aşağıdaki komutu çalıştırın:
```azurecli
az group delete --name <your resource group name; for example: hellospring-1558400876966-rg> --yes
```
Yukarıdaki adımlarda, varsayılan kaynak grubu adını da ayarlarsınız. Bu varsayılan seçimini kaldırmak için Cloud Shell aşağıdaki komutu çalıştırın:
```azurecli
az configure --defaults group=
```
## <a name="next-steps"></a>Sonraki adımlar

Azure Spring Cloud için kullanıma hazır izleme özelliği hakkında daha fazla bilgi edinmek için bkz.:
> [!div class="nextstepaction"]
> [Tanılama Hizmetleri](diagnostic-services.md) 
>  [Dağıtılmış izleme](spring-cloud-tutorial-distributed-tracing.md) 
>  [Gerçek zamanlı akış günlükleri](spring-cloud-howto-log-streaming.md)
