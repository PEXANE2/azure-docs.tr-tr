---
title: JavaScript SDK Application Insights için angular eklentisi
description: Application Insights JavaScript SDK 'Sı için angular eklentisini yükleyip kullanma.
services: azure-monitor
author: lgayhardt
ms.workload: tbd
ms.tgt_pltfrm: ibiza
ms.topic: conceptual
ms.date: 10/07/2020
ms.author: lagayhar
ms.openlocfilehash: 152ba4b1c8a4e09db0bce759f5b67f577ec5d584
ms.sourcegitcommit: b87c7796c66ded500df42f707bdccf468519943c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2020
ms.locfileid: "91844033"
---
# <a name="angular-plugin-for-application-insights-javascript-sdk"></a>JavaScript SDK Application Insights için angular eklentisi

Application Insights JavaScript SDK 'Sı için angular eklentisi şunları sunar:

- Yönlendirici değişikliklerinin izlenmesi
- Angular bileşenleri kullanım istatistikleri

> [!WARNING]
> Angular eklentisi ECMAScript 3 (ES3) uyumlu DEĞILDIR.

## <a name="getting-started"></a>Başlarken

NPM paketini Install:

```bash
npm install @microsoft/applicationinsights-angularplugin-js
```

## <a name="basic-usage"></a>Temel kullanım

Uygulamanızdaki giriş bileşeninde bir Application Insights örneği ayarlayın:

```js
import { Component, OnInit } from '@angular/core';
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { AngularPlugin, AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent implements OnInit {
    private appInsights;
    constructor(
        private router: Router,
        private angularPluginService: AngularPluginService 
    ){
        var angularPlugin = new AngularPlugin();
        this.angularPluginService.init(angularPlugin, this.router);
        this.appInsights = new ApplicationInsights({ config: {
        instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
        extensions: [angularPlugin],
        extensionConfig: {
            [angularPlugin.identifier]: { router: this.router }
        }
        } });
    }

    ngOnInit() {
        this.appInsights.loadAppInsights();
    }
}

```

`trackMetric`Angular bileşen kullanımını izlemek için yöntemini kullanmak için, `AngularPluginService` dosyadaki sağlayıcılar listesinde sağlayıcı olarak ekleyin `app.module.ts` .

```js
import { AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';

@NgModule({
    ...
  providers: [ AngularPluginService ],
})
export class AppModule { }
```

Bir bileşenin ömrünü izlemek için, `trackMetric` `ngOnDestroy` Bu bileşenin yöntemini çağırın. Bileşen yok edildiğinde, `trackMetric` kullanıcının sayfada ve bileşen adının bulunduğu zamanı gönderen bir olayı tetikler.

```js
import { Component, OnDestroy, HostListener } from '@angular/core';
import { AngularPluginService } from '@microsoft/applicationinsights-angularplugin-js';

@Component({
  selector: 'app-test',
  templateUrl: './test.component.html',
  styleUrls: ['./test.component.css']
})
export class TestComponent implements OnDestroy {

  constructor(private angularPluginService: AngularPluginService) {}

  @HostListener('window:beforeunload')
  ngOnDestroy() {
    this.angularPluginService.trackMetric();
  }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- JavaScript SDK 'Sı hakkında daha fazla bilgi edinmek için [Application Insights JAVASCRIPT SDK belgelerine](javascript.md) bakın.
- [GitHub 'da angular eklentisi](https://github.com/microsoft/ApplicationInsights-JS/tree/master/extensions/applicationinsights-angularplugin-js)