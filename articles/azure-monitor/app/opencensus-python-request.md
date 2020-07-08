---
title: OpenCensus Python ile Azure Application Insights gelen Istek Izleme | Microsoft Docs
description: OpenCensus Python aracılığıyla Python uygulamalarınızın istek çağrılarını izleyin.
ms.topic: conceptual
author: lzchen
ms.author: lechen
ms.date: 10/15/2019
ms.custom: tracking-python
ms.openlocfilehash: c9d69c0f39d9cad52dc86c3ab33d202c88131ab0
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84753202"
---
# <a name="track-incoming-requests-with-opencensus-python"></a>OpenCensus Python ile gelen istekleri izleme

Gelen istek verileri, OpenCensus Python ve çeşitli tümleştirmeler kullanılarak toplanır. Popüler Web çerçeveleri ve üzerinde oluşturulmuş Web uygulamalarınıza gönderilen gelen istek verilerini izleyin `django` `flask` `pyramid` . Veriler daha sonra Azure Izleyici altında telemetri olarak Application Insights gönderilir `requests` .

İlk olarak, Python uygulamanızı en son [Opencensus Python SDK 'sı](../../azure-monitor/app/opencensus-python.md)ile işaretleyin.

## <a name="tracking-django-applications"></a>Docgo uygulamalarını izleme

1. `opencensus-ext-django` [Pypı](https://pypi.org/project/opencensus-ext-django/) 'den indirip yükleyin ve uygulamayı `django` Ara yazılım ile işaretleyin. Uygulamanıza gönderilen gelen istekler `django` izlenir.

2. `opencensus.ext.django.middleware.OpencensusMiddleware` `settings.py` Altına dosyanıza ekleyin `MIDDLEWARE` .

    ```python
    MIDDLEWARE = (
        ...
        'opencensus.ext.django.middleware.OpencensusMiddleware',
        ...
    )
    ```

3. AzureExporter 'in altında düzgün şekilde yapılandırıldığından emin olun `settings.py` `OPENCENSUS` . İzlemek istemediğiniz URL 'lerden gelen istekler için, onları öğesine ekleyin `BLACKLIST_PATHS` .

    ```python
    OPENCENSUS = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>"
            )''',
            'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent to it.
        }
    }
    ```

## <a name="tracking-flask-applications"></a>Flask uygulamalarını izleme

1. `opencensus-ext-flask` [Pypı](https://pypi.org/project/opencensus-ext-flask/) 'den indirip yükleyin ve uygulamayı `flask` Ara yazılım ile işaretleyin. Uygulamanıza gönderilen gelen istekler `flask` izlenir.

    ```python
    
    from flask import Flask
    from opencensus.ext.azure.trace_exporter import AzureExporter
    from opencensus.ext.flask.flask_middleware import FlaskMiddleware
    from opencensus.trace.samplers import ProbabilitySampler
    
    app = Flask(__name__)
    middleware = FlaskMiddleware(
        app,
        exporter=AzureExporter(connection_string="InstrumentationKey=<your-ikey-here>"),
        sampler=ProbabilitySampler(rate=1.0),
    )
    
    @app.route('/')
    def hello():
        return 'Hello World!'
    
    if __name__ == '__main__':
        app.run(host='localhost', port=8080, threaded=True)
    
    ```

2. `flask`Uygulamanızı aracılığıyla da yapılandırabilirsiniz `app.config` . İzlemek istemediğiniz URL 'lerden gelen istekler için, onları öğesine ekleyin `BLACKLIST_PATHS` .

    ```python
    app.config['OPENCENSUS'] = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1.0)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>",
            )''',
            'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent to it.
        }
    }
    ```

## <a name="tracking-pyramid-applications"></a>Piramit uygulamalarını izleme

1. `opencensus-ext-django` [Pypı](https://pypi.org/project/opencensus-ext-pyramid/) 'den indirip yükleyin ve uygulamanızı ara ile işaretleyin `pyramid` . Uygulamanıza gönderilen gelen istekler `pyramid` izlenir.

    ```python
    def main(global_config, **settings):
        config = Configurator(settings=settings)
    
        config.add_tween('opencensus.ext.pyramid'
                         '.pyramid_middleware.OpenCensusTweenFactory')
    ```

2. `pyramid`Arayı doğrudan kodda yapılandırabilirsiniz. İzlemek istemediğiniz URL 'lerden gelen istekler için, onları öğesine ekleyin `BLACKLIST_PATHS` .

    ```python
    settings = {
        'OPENCENSUS': {
            'TRACE': {
                'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1.0)',
                'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                    connection_string="InstrumentationKey=<your-ikey-here>",
                )''',
                'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent to it.
            }
        }
    }
    config = Configurator(settings=settings)
    ```

## <a name="next-steps"></a>Sonraki adımlar

* [Uygulama Eşlemesi](../../azure-monitor/app/app-map.md)
* [Kullanılabilirlik](../../azure-monitor/app/monitor-web-app-availability.md)
* [Search](../../azure-monitor/app/diagnostic-search.md)
* [Log (Analytics) sorgusu](../../azure-monitor/log-query/log-query-overview.md)
* [İşlem tanılamaları](../../azure-monitor/app/transaction-diagnostics.md)
