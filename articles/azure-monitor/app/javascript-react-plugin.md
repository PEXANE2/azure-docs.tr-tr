---
title: JavaScript SDK 'Sı için Application Insights eklentisi
description: Application Insights JavaScript SDK 'Sı için tepki verme eklentisini yükleyip kullanma.
services: azure-monitor
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 07/28/2020
ms.openlocfilehash: 3a11f77384c520bed9824841269be4ad998adba4
ms.sourcegitcommit: 814778c54b59169c5899199aeaa59158ab67cf44
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/13/2020
ms.locfileid: "90056209"
---
# <a name="react-plugin-for-application-insights-javascript-sdk"></a>JavaScript SDK 'Sı için Application Insights eklentisi

Application Insights JavaScript SDK 'Sı için bir eklentisine yanıt verin, şunları sunar:

- Rota değişikliklerinin izlenmesi
- BT bileşenleri kullanım istatistiklerini tepki verme

## <a name="getting-started"></a>Başlarken

NPM paketini Install:

```bash

npm install @microsoft/applicationinsights-react-js

```

## <a name="basic-usage"></a>Temel kullanım

```javascript
import React from 'react';
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { ReactPlugin, withAITracking } from '@microsoft/applicationinsights-react-js';
import { createBrowserHistory } from "history";

const browserHistory = createBrowserHistory({ basename: '' });
var reactPlugin = new ReactPlugin();
var appInsights = new ApplicationInsights({
    config: {
        instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
        extensions: [reactPlugin],
        extensionConfig: {
          [reactPlugin.identifier]: { history: browserHistory }
        }
    }
});
appInsights.loadAppInsights();

// To instrument various React components usage tracking, apply the `withAITracking` higher-order
// component function.

class MyComponent extends React.Component {
    ...
}

export default withAITracking(reactPlugin,appInsights, MyComponent);

```

## <a name="configuration"></a>Yapılandırma

| Name    | Varsayılan | Description                                                                                                    |
|---------|---------|----------------------------------------------------------------------------------------------------------------|
| geçmiş | null    | Yönlendirici geçmişini tepki verin. Daha fazla bilgi için bkz. [tepki verme yönlendiricisi paketi belgeleri](https://reactrouter.com/web/api/history). Geçmiş nesnesine bileşenler dışında nasıl erişebileceğinizi öğrenmek için bkz. [tepki verme yönlendiricisi hakkında SSS](https://github.com/ReactTraining/react-router/blob/master/FAQ.md#how-do-i-access-the-history-object-outside-of-components)    |

### <a name="react-components-usage-tracking"></a>BT bileşenleri kullanım izlemeyi tepki verme

Çeşitli tepki verme bileşenleri kullanım izlemesini işaretlemek için, `withAITracking` daha yüksek sıralı bileşen işlevini uygulayın.

Olay aracılığıyla olaydan geçen süreyi ölçecek `ComponentDidMount` `ComponentWillUnmount` . Ancak, bu işlemi daha doğru yapmak için kullanıcının boşta olduğu süreyi çıkarır `React Component Engaged Time = ComponentWillUnmount timestamp - ComponentDidMount timestamp - idle time` .

Bu ölçümü Azure portal görmek için Application Insights kaynağına gitmeniz, "ölçümler" sekmesini seçmeniz ve boş grafikleri "özel ölçüm adı ile bir uygulama süresi (saniye)" öğesini görüntüleyecek şekilde yapılandırmak için, ölçümünüzün toplama (Sum, Ort, vb.) öğesini seçin ve bölme Uygula "bileşen adı" ' nı seçin.

![Özel ölçüyü görüntüleyen grafiğin ekran görüntüsü "bileşen ile bağlı süre (saniye)", "bileşen adı" olarak bölünür](./media/javascript-react-plugin/chart.png)

Ayrıca, gereksinimlerinize göre rapor ve görselleştirmeler oluşturmak için Application Insights verileri bölmek üzere özel sorgular çalıştırabilirsiniz. Azure portal Application Insights kaynağına gidin, genel bakış sekmesinin üst menüsünden "analiz" öğesini seçin ve sorgunuzu çalıştırın.

![Özel Ölçüm sorgu sonuçlarının ekran görüntüsü.](./media/javascript-react-plugin/query.png)

> [!NOTE]
> Yeni özel ölçümlerin Azure portalında görünmesi 10 dakikaya kadar sürebilir.

## <a name="sample-app"></a>Örnek uygulama

[Application Insights tepki verme tanıtımı](https://github.com/Azure-Samples/application-insights-react-demo)' ne göz atın.

## <a name="next-steps"></a>Sonraki adımlar

- JavaScript SDK 'Sı hakkında daha fazla bilgi edinmek için [Application Insights JAVASCRIPT SDK belgelerine](javascript.md)bakın.
- Kusto sorgu dili hakkında bilgi edinmek ve Log Analytics verileri sorgulamak için [günlük sorgusuna genel bakış](../../azure-monitor/log-query/log-query-overview.md)bölümüne bakın.
