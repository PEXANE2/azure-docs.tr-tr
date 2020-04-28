---
title: Uzaktan Izleme çözümü Kullanıcı arabirimine panel ekleme-Azure | Microsoft Docs
description: Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki panoya nasıl yeni bir panel ekleyeceğiniz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 694cc83ffce20a8744d7452a8f6d67c9ce23641c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82187237"
---
# <a name="add-a-custom-panel-to-the-dashboard-in-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki panoya özel panel ekleme

Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki bir pano sayfasına nasıl yeni bir panel ekleyebileceğiniz gösterilmektedir. Makalede şunları açıklanmaktadır:

- Yerel bir geliştirme ortamı hazırlama.
- Web Kullanıcı arabirimindeki bir pano sayfasına yeni panel ekleme.

Bu makaledeki örnek panel, varolan Pano sayfasında görüntülenir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için, yerel geliştirme makinenizde aşağıdaki yazılımların yüklü olması gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce, [Uzaktan izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md) makalesindeki adımları tamamlamalısınız.

## <a name="add-a-panel"></a>Panel ekleme

Web Kullanıcı arabirimine bir panel eklemek için paneli tanımlayan kaynak dosyalarını eklemeniz ve sonra paneli görüntülemesi için panoyu değiştirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-panel"></a>Paneli tanımlayan yeni dosyaları ekleyin

Başlangıç yapmanız için, **src/izlenecek yol/bileşenler/sayfalar/Pano/paneller/examplePanel** klasörü şunları içeren bir paneli tanımlayan dosyaları içerir:

**examplePanel. js**

[!code-javascript[Example panel](~/remote-monitoring-webui/src/walkthrough/components/pages/dashboard/panels/examplePanel/examplePanel.js?name=panel "Example panel")]

Src/ **izlenecek yol/bileşenler/sayfalar/Pano/paneller/examplePanel** klasörünü **src/Components/Pages/Dashboard/paneller** klasörüne kopyalayın.

**Src/izlenecek yol/bileşenler/sayfalar/Dashboard/panel/index. js** dosyasına aşağıdaki dışarı aktarmayı ekleyin:

```js
export * from './examplePanel';
```

### <a name="add-the-panel-to-the-dashboard"></a>Paneli panoya ekleme

Panoyu eklemek için **src/Components/Pages/Dashboard/Dashboard. js** öğesini değiştirin.

Panellerden içeri aktarmalar listesine örnek paneli ekleyin:

```js
import {
  OverviewPanel,
  AlertsPanel,
  TelemetryPanel,
  AnalyticsPanel,
  MapPanel,
  transformTelemetryResponse,
  chartColorObjects,
  ExamplePanel
} from './panels';
```

Aşağıdaki hücre tanımını sayfa içeriğindeki kılavuza ekleyin:

```js
          <Cell className="col-2">
            <ExamplePanel t={t} />
          </Cell>
```

## <a name="test-the-flyout"></a>Açılır menüyü test etme

Web Kullanıcı arabirimi zaten yerel olarak çalışmıyorsa, deponun yerel kopyasının kökünde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komut, Kullanıcı arabirimini ' de `http://localhost:3000/dashboard`yerel olarak çalıştırır. Yeni paneli görüntülemek için **Pano** sayfasına gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uzaktan Izleme çözüm hızlandırıcısında Web Kullanıcı arabiriminde panolar eklemenize veya özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Uzaktan Izleme çözümü Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
