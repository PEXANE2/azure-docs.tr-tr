---
title: Uzaktan İzleme çözümü Kullanıcı Arabirimi'ne panel ekleme - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'nde panoya nasıl yeni bir panel ekleyeceğiniz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: 3b855c3bed75945f44b55463bdacd049b7930aa7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447072"
---
# <a name="add-a-custom-panel-to-the-dashboard-in-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'nde panoya özel bir panel ekleme

Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'nde bir pano sayfasına yeni bir panel nasıl ekleyeceğiniz gösterilmektedir. Makalede şöyle anlatılmaktadır:

- Yerel bir kalkınma ortamı nasıl hazırlanır.
- Web Web Arama Hizmeti'ndeki bir pano sayfasına yeni bir panel ekleme.

Bu makaledeki örnek panel varolan pano sayfasında görüntülenir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için, yerel geliştirme makinenize aşağıdaki yazılımın yüklenmesi gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi makalesine özel bir sayfa ekle](iot-accelerators-remote-monitoring-customize-page.md) adımlarını tamamlamanız gerekir.

## <a name="add-a-panel"></a>Panel ekleme

Web Ara Birimi'ne bir panel eklemek için, paneli tanımlayan kaynak dosyaları eklemeniz ve ardından paneli görüntülemek için panoyu değiştirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-panel"></a>Paneli tanımlayan yeni dosyaları ekleme

Başlamak **için, src/walkthrough/components/pages/dashboard/panels/examplePanel** klasörü, aşağıdakiler dahil olmak üzere bir paneli tanımlayan dosyaları içerir:

**örnekPanel.js**

[!code-javascript[Example panel](~/remote-monitoring-webui/src/walkthrough/components/pages/dashboard/panels/examplePanel/examplePanel.js?name=panel "Example panel")]

**src/walkthrough/components/pages/dashboard/panels/examplePanel** klasörünü **src/components/pages/dashboard/panels** klasörüne kopyalayın.

**src/walkthrough/components/pages/dashboard/panels/index.js** dosyasına aşağıdaki dışa aktarımları ekleyin:

```js
export * from './examplePanel';
```

### <a name="add-the-panel-to-the-dashboard"></a>Paneli panoya ekleme

Paneli eklemek için **src/components/pages/dashboard/dashboard.js'yi** değiştirin.

Panellerden gelen içeri aktarımlar listesine örnek paneli ekleyin:

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

Sayfa içeriğindeki ızgaraya aşağıdaki hücre tanımını ekleyin:

```js
          <Cell className="col-2">
            <ExamplePanel t={t} />
          </Cell>
```

## <a name="test-the-flyout"></a>Uçuşu test edin

Web Web Arama Ekibi zaten yerel olarak çalışmıyorsa, deponun yerel kopyasının kökünde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komut ui'yi yerel [http://localhost:3000/dashboard](http://localhost:3000/dashboard)olarak çalışır. Yeni paneli görüntülemek için **Pano** sayfasına gidin.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında web Kullanıcı Arabirimi'ndeki panolar eklemenize veya özelleştirmenize yardımcı olacak kaynakları öğrendiniz.

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için uzaktan [izleme mimarisine](iot-accelerators-remote-monitoring-sample-walkthrough.md)bakın.
