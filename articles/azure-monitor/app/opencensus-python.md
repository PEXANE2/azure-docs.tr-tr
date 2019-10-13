---
title: Azure Izleyici ile Python uygulamalarını izleme | Microsoft Docs
description: Azure Izleyici ile OpenCensus Python 'ı bağlamak için yönergeler sağlar
services: application-insights
keywords: ''
author: reyang
ms.author: reyang
ms.date: 10/11/2019
ms.service: application-insights
ms.topic: conceptual
ms.reviewer: mbullwin
manager: carmonm
ms.openlocfilehash: 1316cf6808f6ccfc4165ad162c51421638b130be
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72293987"
---
# <a name="set-up-azure-monitor-for-your-python-application"></a>Python uygulamanız için Azure Izleyicisini ayarlama

Azure Izleyici, [Opencensus](https://opencensus.io)ile tümleştirme yoluyla Python uygulamalarının dağıtılmış izlemeyi, ölçüm toplamayı ve günlüğe kaydedilmesini destekler. Bu makale, Python için OpenCensus ayarlama ve izleme verilerinizi Azure Izleyici 'ye alma sürecinde adım adım kılavuzluk eder.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure Aboneliğine sahip olmanız gerekir.
- Python yüklü olmalıdır; Bu makalede, önceki sürümler büyük olasılıkla küçük ayarlamalarla çalışamayabilse de [Python 3.7.0](https://www.python.org/downloads/)kullanılmaktadır.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz](https://azure.microsoft.com/free/) bir hesap oluşturun.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

[Azure Portal](https://portal.azure.com/)’ında oturum açın.

## <a name="create-application-insights-resource-in-azure-monitor"></a>Azure Izleyici 'de Application Insights kaynağı oluşturma

İlk olarak Azure Izleyici 'de bir Application Insights kaynak oluşturmanız gerekir ve bu işlem bir izleme anahtarı (Ikey) oluşturur. Ikey daha sonra, Azure Izleyici 'ye telemetri verileri gönderecek şekilde OpenCensus SDK 'sını yapılandırmak için kullanılır.

1. @No__t **kaynak oluştur**' u seçin-1**Geliştirici Araçları** > **Application Insights**.

   ![Application Insights Kaynağı ekleme](./media/opencensus-python/0001-create-resource.png)

   Bir yapılandırma kutusu görünür. Giriş alanlarını doldurmak için aşağıdaki tabloyu kullanın.

    | Ayarlar        | Değer           | Açıklama  |
   | ------------- |:-------------|:-----|
   | **Adı**      | Genel Olarak Benzersiz Değer | İzlemekte olduğunuz uygulamayı tanımlayan ad |
   | **Kaynak Grubu**     | myResourceGroup      | App Insights verilerini barındıran yeni kaynak grubunun adı |
   | **Konum** | Doğu ABD | Yakınınızda bulunan veya uygulamanızın barındırıldığı konumun yakınında olan bir konum seçin |

2. **Oluştur**’a tıklayın.

## <a name="instrumenting-with-opencensus-python-sdk-for-azure-monitor"></a>Azure Izleyici için OpenCensus Python SDK 'Sı ile ayarlama

1. OpenCensus Azure Izleyici exporler 'i yükler:

    ```console
    python -m pip install opencensus-ext-azure
    ```

    > [!NOTE]
    > `python -m pip install opencensus-ext-azure`, Python yüklemeniz için ayarlanmış bir yol ortam değişkenine sahip olduğunuzu varsayar. Bunu yapılandırmadıysanız, Python yürütülebilirin bulunduğu konuma tam dizin yolunu vermeniz gerekir; bu, şunun gibi bir komutla sonuçlanır: `C:\Users\Administrator\AppData\Local\Programs\Python\Python37-32\python.exe -m pip install opencensus-ext-azure`.

2. SDK, Azure Izleyici 'ye farklı türlerde telemetri göndermek için üç Azure Izleyici dışarı aktaramasıyla yararlanır: Trace, ölçümler ve Günlükler. Bu farklı türler hakkında daha fazla bilgi için [veri platformuna genel bakışa](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform) göz atın. Bu farklı türlerin üç dışarı aktarma yöntemiyle nasıl gönderileceğini görmek için aşağıdaki yönergeleri izleyin.

### <a name="trace"></a>İzleme

1. İlk olarak, bazı izleme verilerini yerel olarak oluşturalım. Python boş veya istediğiniz Düzenleyicinizde aşağıdaki kodu girin.

    ```python
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer

    tracer = Tracer(sampler=ProbabilitySampler(1.0))

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2. Kodu çalıştırmak, sürekli olarak bir değer girmenizi ister. Her giriş ile, değer kabuğa yazdırılır ve karşılık gelen bir **Spandata** parçası, OpenCensus Python modülü tarafından oluşturulur. OpenCensus projesi bir izlemeyi bir [_yayılma ağacı olarak_](https://opencensus.io/core-concepts/tracing/)tanımlar.
    
    ```
    Enter a value: 4
    4
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='15ac5123ac1f6847', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:22.805429Z', end_time='2019-06-27T18:21:44.933405Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 25
    25
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='2e512f846ba342de', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:44.933405Z', end_time='2019-06-27T18:21:46.156787Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    Enter a value: 100
    100
    [SpanData(name='test', context=SpanContext(trace_id=8aa41bc469f1a705aed1bdb20c342603, span_id=None, trace_options=TraceOptions(enabled=True), tracestate=None), span_id='f3f9f9ee6db4740a', parent_span_id=None, attributes=BoundedDict({}, maxlen=32), start_time='2019-06-27T18:21:46.157732Z', end_time='2019-06-27T18:21:47.269583Z', child_span_count=0, stack_trace=None, annotations=BoundedList([], maxlen=32), message_events=BoundedList([], maxlen=128), links=BoundedList([], maxlen=32), status=None, same_process_as_parent_span=None, span_kind=0)]
    ```

3. Tanıtım amacıyla faydalı olsa da, son olarak `SpanData` ' ı Azure Izleyici 'ye yayma istiyoruz. Aşağıdaki kod örneğine göre kodunuzu önceki adımdan değiştirin:

    ```python
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.trace.samplers import ProbabilitySampler
    from opencensus.trace.tracer import Tracer
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    tracer = Tracer(
        exporter=AzureExporter(
            connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000'),
        ),
        sampler=ProbabilitySampler(1.0),
    )

    def valuePrompt():
        with tracer.span(name="test") as span:
            line = input("Enter a value: ")
            print(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

4. Artık Python betiğini çalıştırdığınızda, yine de değer girmeniz istenir, ancak artık yalnızca değeri kabukta yazdırılıyor. Oluşturulan @no__t 0 Azure Izleyici 'ye gönderilir. Yayılan yayılmış verileri `dependencies` altında bulabilirsiniz.

### <a name="metrics"></a>Ölçümler

1. İlk olarak, bazı yerel ölçüm verileri oluşturalım. Kullanıcının Enter tuşuna bastığı süreyi izlemek için basit bir ölçüm oluşturacağız.

    ```python
    from datetime import datetime
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```
2. Kodu çalıştırmak, sürekli olarak ENTER tuşuna basmanız istenir. Basılan değer sayısını izlemek için bir ölçüm oluşturulur. Her giriş ile, değer artırılır ve ölçüm bilgileri konsolda, geçerli değerle ve geçerli zaman damgasıyla birlikte görüntülenir.

    ```
    Press enter.
    Point(value=ValueLong(5), timestamp=2019-10-09 20:58:04.930426)
    Press enter.
    Point(value=ValueLong(6), timestamp=2019-10-09 20:58:06.570167)
    Press enter.
    Point(value=ValueLong(7), timestamp=2019-10-09 20:58:07.138614)
    ```

3. Tanıtım amacıyla yararlı olsa da, sonuçta ölçüm verilerini Azure Izleyici 'ye yaymak istiyoruz. Aşağıdaki kod örneğine göre kodunuzu önceki adımdan değiştirin:

    ```python
    from datetime import datetime
    from opencensus.ext.azure import metrics_exporter
    from opencensus.stats import aggregation as aggregation_module
    from opencensus.stats import measure as measure_module
    from opencensus.stats import stats as stats_module
    from opencensus.stats import view as view_module
    from opencensus.tags import tag_map as tag_map_module

    stats = stats_module.stats
    view_manager = stats.view_manager
    stats_recorder = stats.stats_recorder
    
    prompt_measure = measure_module.MeasureInt("prompts",
                                               "number of prompts",
                                               "prompts")
    prompt_view = view_module.View("prompt view",
                                   "number of prompts",
                                   [],
                                   prompt_measure,
                                   aggregation_module.CountAggregation())
    view_manager.register_view(prompt_view)
    mmap = stats_recorder.new_measurement_map()
    tmap = tag_map_module.TagMap()

    # TODO: replace the all-zero GUID with your instrumentation key.
    exporter = metrics_exporter.new_metrics_exporter(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    view_manager.register_exporter(exporter)

    def prompt():
        input("Press enter.")
        mmap.measure_int_put(prompt_measure, 1)
        mmap.record(tmap)
        metrics = list(mmap.measure_to_view_map.get_metrics(datetime.utcnow()))
        print(metrics[0].time_series[0].points[0])

    def main():
        while True:
            prompt()

    if __name__ == "__main__":
        main()
    ```

4. Dışarı Aktarıcı, ölçüm verilerini Azure Izleyici 'ye sabit bir aralıkta, varsayılan olarak her 15 saniyede bir gönderir. Bu ölçüm verilerinin, içerdiği değer ve zaman damgasıyla birlikte her aralığa gönderilmesi için tek bir ölçümü izliyoruz. @No__t-0 ' ın altında verileri bulabilirsiniz.

### <a name="logs"></a>Günlükler

1. İlk olarak, bazı yerel günlük verileri oluşturalım.

    ```python
    import logging

    logger = logging.getLogger(__name__)

    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)

    def main():
        while True:
            valuePrompt()

    if __name__ == "__main__":
        main()
    ```

2.  Kod, sürekli olarak bir değer girilmesini ister. Değer olarak girilen değeri içeren her değer için bir günlük girişi yayılır.

    ```
    Enter a value: 24
    24
    Enter a value: 55
    55
    Enter a value: 123
    123
    Enter a value: 90
    90
    ```

3. Tanıtım amacıyla yararlı olsa da, sonuçta ölçüm verilerini Azure Izleyici 'ye yaymak istiyoruz. Aşağıdaki kod örneğine göre kodunuzu önceki adımdan değiştirin:

    ```python
    import logging
    from opencensus.ext.azure.log_exporter import AzureLogHandler
    
    logger = logging.getLogger(__name__)
    
    # TODO: replace the all-zero GUID with your instrumentation key.
    logger.addHandler(AzureLogHandler(
        connection_string='InstrumentationKey=00000000-0000-0000-0000-000000000000')
    )
    
    def valuePrompt():
        line = input("Enter a value: ")
        logger.warning(line)
    
    def main():
        while True:
            valuePrompt()
    
    if __name__ == "__main__":
        main()
    ```

4. Dışarı aktarma programı günlük verilerini Azure Izleyici 'ye gönderir. @No__t-0 ' ın altında verileri bulabilirsiniz.

## <a name="start-monitoring-in-the-azure-portal"></a>Azure portalında izlemeyi başlatma

1. Artık çalışmakta olan uygulamanızla ilgili ayrıntıları görüntülemek için Azure portal Application Insights **genel bakış** sayfasını yeniden açabilirsiniz. **Canlı ölçüm akışı**' nı seçin.

   ![Kırmızı kutuda seçili canlı ölçüm akışının bulunduğu genel bakış bölmesinin ekran görüntüsü](./media/opencensus-python/0005-overview-live-metrics-stream.png)

2. **Genel bakış** sayfasına geri dönün ve bağımlılık ilişkilerinin görsel düzeni ve uygulama bileşenleriniz arasındaki çağrı zamanlaması Için **uygulama Haritası** ' nı seçin.

    ![Temel uygulama eşlemesinin ekran görüntüsü](./media/opencensus-python/0007-application-map.png)

    Yalnızca bir yöntem çağrısını izliyor olduğundan, uygulama haritamız ilginç değildir. Ancak uygulama Haritası, daha fazla Dağıtılmış uygulamayı görselleştirmek için ölçeklendirebilir:

   ![Uygulama Eşlemesi](media/opencensus-python/application-map.png)

3. Ayrıntılı performans analizi gerçekleştirmek ve performansının yavaşlamasına neden olan performansı belirlemek için **performansı araştır** ' ı seçin.

    ![Performans bölmesinin ekran görüntüsü](./media/opencensus-python/0008-performance.png)

4. **Örnekler** ' i seçip sağ bölmede görüntülenen örneklerden birine tıkladığınızda, uçtan uca işlem ayrıntıları deneyimi başlatılır. Örnek uygulamamız yalnızca tek bir olay gösterirken, daha karmaşık bir uygulama, tek bir olayın çağrı yığınının düzeyine kadar uçtan uca işlemleri araştırmanıza olanak sağlar.

     ![Uçtan uca işlem arabiriminin ekran görüntüsü](./media/opencensus-python/0009-end-to-end-transaction.png)

## <a name="view-your-data-with-queries"></a>Sorgular ile verilerinizi görüntüleme

1. Uygulamalarınızdan gönderilen telemetri verilerini Günlükler (Analiz) sekmesinden görüntüleyebilirsiniz.

    ![Kırmızı kutuda seçili Günlükler (Analiz) ile genel bakış bölmesinin ekran görüntüsü](./media/opencensus-python/0010-logs-query.png)

2. Azure Izleyici izleme programı ile gönderilen telemetri için, gelen istekler `requests` ' ın altında görünür ve çıkış istekleri `dependencies` altında görünür.

3. Azure Izleyici ölçümleri dışarı aktarıcı ile gönderilen telemetri için, gönderilen ölçümler `customMetrics` altında görünür.

4. Azure Izleyici günlükleri dışarı aktarıcı ile gönderilen telemetri için Günlükler `traces` altında görünür ve özel durumlar `exceptions` altında görünür.

5. Sorguları ve günlükleri kullanma hakkında daha ayrıntılı bilgi için [Azure izleyici 'de günlüklere](https://docs.microsoft.com/azure/azure-monitor/platform/data-platform-logs) göz atın.

## <a name="opencensus-for-python"></a>Python için OpenCensus

* [GitHub 'da OpenCensus Python](https://github.com/census-instrumentation/opencensus-python)
* [Liş](https://github.com/census-instrumentation/opencensus-python/blob/master/README.rst#customization)
* [Flask tümleştirmesi](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-flask)
* [Docgo tümleştirmesi](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-django)
* [MySQL tümleştirmesi](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-mysql)
* [PostgreSQL](https://github.com/census-instrumentation/opencensus-python/tree/master/contrib/opencensus-ext-postgresql)

## <a name="next-steps"></a>Sonraki adımlar

* [API Özeti](./../../azure-monitor/app/api-custom-events-metrics.md)
* [Uygulama eşlemesi](./../../azure-monitor/app/app-map.md)
* [Uçtan uca performans izleme](./../../azure-monitor/learn/tutorial-performance.md)

### <a name="alerts"></a>Uyarılar

* [Kullanılabilirlik testleri](../../azure-monitor/app/monitor-web-app-availability.md): Sitenizin web’de görünür olduğundan emin olmaya yönelik testler oluşturun.
* [Akıllı tanılama](../../azure-monitor/app/proactive-diagnostics.md): Bu testler otomatik olarak çalıştığından, bunları ayarlamak için herhangi bir şey yapmanız gerekmez. Uygulamanızda olağan dışı oranda başarısız istek olup olmadığını bildirirler.
* [Ölçüm uyarıları](../../azure-monitor/app/alerts.md): bir ölçüm bir eşiği kesiştyolursa sizi uyarmak için Uyarılar ayarlayın. Bunları, uygulamanıza kodladığınız özel ölçümlerde ayarlayabilirsiniz.