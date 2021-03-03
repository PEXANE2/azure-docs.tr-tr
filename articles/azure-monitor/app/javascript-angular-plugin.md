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
ms.openlocfilehash: d45d8bed328dc91dfeeabd6ce878074fa1218623
ms.sourcegitcommit: c27a20b278f2ac758447418ea4c8c61e27927d6a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/03/2021
ms.locfileid: "101737027"
---
# <a name="angular-plugin-for-application-insights-javascript-sdk"></a>JavaScript SDK Application Insights için angular eklentisi

Application Insights JavaScript SDK 'Sı için angular eklentisi şunları sunar:

- Yönlendirici değişikliklerinin izlenmesi

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
import { Component } from '@angular/core';
import { ApplicationInsights } from '@microsoft/applicationinsights-web';
import { AngularPlugin } from '@microsoft/applicationinsights-angularplugin-js';
import { Router } from '@angular/router';

@Component({
  selector: 'app-root',
  templateUrl: './app.component.html',
  styleUrls: ['./app.component.css']
})
export class AppComponent {
    constructor(
        private router: Router
    ){
        var angularPlugin = new AngularPlugin();
        const appInsights = new ApplicationInsights({ config: {
        instrumentationKey: 'YOUR_INSTRUMENTATION_KEY_GOES_HERE',
        extensions: [angularPlugin],
        extensionConfig: {
            [angularPlugin.identifier]: { router: this.router }
        }
        } });
        appInsights.loadAppInsights();
    }
}
```

## <a name="next-steps"></a>Sonraki adımlar

- JavaScript SDK 'Sı hakkında daha fazla bilgi edinmek için [Application Insights JAVASCRIPT SDK belgelerine](javascript.md) bakın.
- [GitHub 'da angular eklentisi](https://github.com/microsoft/ApplicationInsights-JS/tree/master/extensions/applicationinsights-angularplugin-js)
