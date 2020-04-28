---
title: Uzaktan Izleme çözümü Kullanıcı arabirimine hizmet ekleme-Azure | Microsoft Docs
description: Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine nasıl yeni bir hizmet ekleyeceğiniz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: e44aa8ade512a6005959e795cb1d4ad861da1338
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "61447055"
---
# <a name="add-a-custom-service-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel bir hizmet ekleme

Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine nasıl yeni bir hizmet ekleyeceğiniz gösterilmektedir. Makalede şunları açıklanmaktadır:

- Yerel bir geliştirme ortamı hazırlama.
- Web Kullanıcı arabirimine yeni bir hizmet nasıl eklenir.

Bu makaledeki örnek hizmet, [Uzaktan izleme çözümü Hızlandırıcısı Web UI nasıl yapılır makalesine özel kılavuz ekleme](iot-accelerators-remote-monitoring-customize-grid.md) konusunda size nasıl ekleneceğini gösteren bir kılavuza yönelik verileri sağlar.

Tepki verme bir uygulamada, bir hizmet genellikle arka uç hizmetiyle etkileşime girer. Uzaktan Izleme çözüm hızlandırıcısının örnekleri, IoT Hub Manager ve yapılandırma mikro hizmetleriyle etkileşime geçen hizmetleri içerir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için, yerel geliştirme makinenizde aşağıdaki yazılımların yüklü olması gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce, [Uzaktan izleme çözümü Hızlandırıcısı Web UI nasıl yapılır makalesine özel sayfa ekle](iot-accelerators-remote-monitoring-customize-page.md) ' deki adımları tamamlamalısınız.

## <a name="add-a-service"></a>Hizmet ekleme

Web Kullanıcı arabirimine bir hizmet eklemek için, hizmeti tanımlayan kaynak dosyaları eklemeniz ve mevcut bazı dosyaları değiştirerek Web Kullanıcı arabirimini yeni hizmetten haberdar hale getirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-service"></a>Hizmeti tanımlayan yeni dosyaları ekleyin

Başlamak için, **src/Walkthrough/Services** klasörü, basit bir hizmeti tanımlayan dosyaları içerir:

**exampleService. js**

[!code-javascript[Example service](~/remote-monitoring-webui/src/walkthrough/services/exampleService.js?name=service "Example service")]

Hizmetlerin nasıl uygulandığı hakkında daha fazla bilgi edinmek için bkz. [reaktif programlamaya giriş eksik](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754).

**model/örnek modeller. js**

[!code-javascript[Example model](~/remote-monitoring-webui/src/walkthrough/services/models/exampleModels.js?name=models "Example model")]

**Exampleservice. js** ' i **src/Services** klasörüne kopyalayın ve **ExampleModel. js** öğesini **src/Services/modeller** klasörüne kopyalayın.

Yeni hizmeti dışarı aktarmak için **src/Services** klasöründeki **index. js** dosyasını güncelleştirin:

```js
export * from './exampleService';
```

Yeni modeli dışarı aktarmak için **src/Services/modeller** klasöründeki **index. js** dosyasını güncelleştirin:

```js
export * from './exampleModels';
```

### <a name="set-up-the-calls-to-the-service-from-the-store"></a>Mağazadan hizmete yönelik çağrıları ayarlama

Başlamak için **src/Walkthrough/Store/azaltıcının** klasörü bir örnek Reducer içerir:

**exampleReducer. js**

[!code-javascript[Example reducer](~/remote-monitoring-webui/src/walkthrough/store/reducers/exampleReducer.js?name=reducer "Example reducer")]

**ExampleReducer. js** ' i **src/Store/azaltıcının** klasörüne kopyalayın.

Reducer ve **Estanlar**hakkında daha fazla bilgi edinmek için bkz. [Redux-observable](https://redux-observable.js.org/).

### <a name="configure-the-middleware"></a>Ara yazılımı yapılandırma

Ara yazılımı yapılandırmak için Reducer, **src/Store** klasöründeki **rootReducer. js** dosyasına ekleyin:

```js
import { reducer as exampleReducer } from './reducers/exampleReducer';

const rootReducer = combineReducers({
  ...appReducer,
  ...devicesReducer,
  ...rulesReducer,
  ...simulationReducer,
  ...exampleReducer
});
```

**Kaynak/mağaza** klasöründeki **rootepıcs. js** dosyasına EPICS ekleyin:

```js
import { epics as exampleEpics } from './reducers/exampleReducer';

// Extract the epic function from each property object
const epics = [
  ...appEpics.getEpics(),
  ...devicesEpics.getEpics(),
  ...rulesEpics.getEpics(),
  ...simulationEpics.getEpics(),
  ...exampleEpics.getEpics()
];
```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uzaktan Izleme çözüm hızlandırıcısında Web Kullanıcı arabirimindeki Hizmetleri eklemenize veya özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Bir hizmeti tanımladınız, bir sonraki adım, hizmet tarafından döndürülen verileri görüntüleyen [Uzaktan izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel bir kılavuz eklemektir](iot-accelerators-remote-monitoring-customize-grid.md) .

Uzaktan Izleme çözümü Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
