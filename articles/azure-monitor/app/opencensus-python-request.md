---
title: OpenCensus Python ile Azure Uygulama Öngörülerinde Gelen İstek İzleme | Microsoft Dokümanlar
description: OpenCensus Python aracılığıyla Python uygulamalarınız için istek çağrılarını izleyin.
ms.topic: conceptual
author: lzchen
ms.author: lechen
ms.date: 10/15/2019
ms.openlocfilehash: 0396bd8d150c6145a39f36e7be9e6e2dcacef2c4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77669956"
---
# <a name="track-incoming-requests-with-opencensus-python"></a>OpenCensus Python ile gelen istekleri izleme

Gelen istek verileri OpenCensus Python ve çeşitli tümleştirmeleri kullanılarak toplanır. Popüler web çerçevelerinin `django`üzerine inşa edilmiş web uygulamalarınız için gönderilen `flask` `pyramid`gelen istek verilerini izleyin ve . Veriler daha sonra Azure Monitor altında uygulama `requests` öngörüleri için telemetri olarak gönderilir.

İlk olarak, en son [OpenCensus Python SDK](../../azure-monitor/app/opencensus-python.md)ile Python uygulamanızı enstrüman.

## <a name="tracking-django-applications"></a>Django uygulamalarını izleme

1. [PyPI'den](https://pypi.org/project/opencensus-ext-django/) indirin ve kurun `django` `opencensus-ext-django` ve aracı uygulamanızı ara yazılımla birlikte enstrüman. Başvurunuza `django` gönderilen gelen istekler takip edilecektir.

2. Dosyanıza `settings.py` ' `MIDDLEWARE`ın altında ekle. `opencensus.ext.django.middleware.OpencensusMiddleware`

    ```python
    MIDDLEWARE = (
        ...
        'opencensus.ext.django.middleware.OpencensusMiddleware',
        ...
    )
    ```

3. AzureExporter'nın altında `OPENCENSUS`düzgün şekilde `settings.py` yapılandırıldığından emin olun.

    ```python
    OPENCENSUS = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=1)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>"
            )''',
        }
    }
    ```

4. İzlemek `settings.py` istemediğiniz istekler `BLACKLIST_PATHS` için altına urller de ekleyebilirsiniz.

    ```python
    OPENCENSUS = {
        'TRACE': {
            'SAMPLER': 'opencensus.trace.samplers.ProbabilitySampler(rate=0.5)',
            'EXPORTER': '''opencensus.ext.azure.trace_exporter.AzureExporter(
                connection_string="InstrumentationKey=<your-ikey-here>",
            )''',
            'BLACKLIST_PATHS': ['https://example.com'],  <--- These sites will not be traced if a request is sent from it.
        }
    }
    ```

## <a name="tracking-flask-applications"></a>Flask uygulamalarını izleme

1. [PyPI'den](https://pypi.org/project/opencensus-ext-flask/) indirin ve kurun `flask` `opencensus-ext-flask` ve aracı uygulamanızı ara yazılımla birlikte enstrüman. Başvurunuza `flask` gönderilen gelen istekler takip edilecektir.

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

2. Aracınızı `flask` doğrudan kodda yapılandırabilirsiniz. İzlemek istemediğiniz url'lerden gelen istekler için, `BLACKLIST_PATHS`bunları ekleyin.

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

1. [PyPI'den](https://pypi.org/project/opencensus-ext-pyramid/) indirin ve kurun `pyramid` `opencensus-ext-django` ve uygulamanızı tween ile enstrüman. Başvurunuza `pyramid` gönderilen gelen istekler takip edilecektir.

    ```python
    def main(global_config, **settings):
        config = Configurator(settings=settings)
    
        config.add_tween('opencensus.ext.pyramid'
                         '.pyramid_middleware.OpenCensusTweenFactory')
    ```

2. Yeninizi doğrudan `pyramid` kodda yapılandırabilirsiniz. İzlemek istemediğiniz url'lerden gelen istekler için, `BLACKLIST_PATHS`bunları ekleyin.

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
* [Kullanılabilir -lik](../../azure-monitor/app/monitor-web-app-availability.md)
* [Arama](../../azure-monitor/app/diagnostic-search.md)
* [Günlük (Analytics) sorgusu](../../azure-monitor/log-query/log-query-overview.md)
* [İşlem tanılama](../../azure-monitor/app/transaction-diagnostics.md)
