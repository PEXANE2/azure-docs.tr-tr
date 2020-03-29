---
title: Uzaktan İzleme çözümüne hizmet ekleme Kullanıcı Arabirimi - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne nasıl yeni bir hizmet ekleyeceğiniz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/02/2018
ms.topic: conceptual
ms.openlocfilehash: e44aa8ade512a6005959e795cb1d4ad861da1338
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447055"
---
# <a name="add-a-custom-service-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir hizmet ekleme

Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne nasıl yeni bir hizmet ekleyeceğiniz gösterilmektedir. Makalede şöyle anlatılmaktadır:

- Yerel bir kalkınma ortamı nasıl hazırlanır.
- Web Web Arama Hizmeti'ne yeni bir hizmet ekleme.

Bu makaledeki örnek hizmet, Uzaktan İzleme [çözüm hızlandırıcı web Ara](iot-accelerators-remote-monitoring-customize-grid.md) Birimi'ne özel bir ızgara eklemenin nasıl ekleniş yapılacağını gösterdiği bir ızgara için veri sağlar.

Tepki uygulamasında, bir hizmet genellikle arka uç hizmetiyle etkileşime girer. Uzaktan İzleme çözüm hızlandırıcısındaki örnekler arasında IoT hub yöneticisi ve yapılandırma mikro hizmetleriyle etkileşim edebilen hizmetler yer almaktadır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için, yerel geliştirme makinenize aşağıdaki yazılımın yüklenmesi gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi makalesine özel bir sayfa ekle](iot-accelerators-remote-monitoring-customize-page.md) adımlarını tamamlamanız gerekir.

## <a name="add-a-service"></a>Hizmet ekleme

Web Ara Birimi'ne bir hizmet eklemek için, hizmeti tanımlayan kaynak dosyaları eklemeniz ve web UI'nin yeni hizmetten haberdar olmasını sağlamak için varolan bazı dosyaları değiştirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-service"></a>Hizmeti tanımlayan yeni dosyaları ekleme

Başlamak **için, src/walkthrough/services** klasörü basit bir hizmeti tanımlayan dosyaları içerir:

**örnekService.js**

[!code-javascript[Example service](~/remote-monitoring-webui/src/walkthrough/services/exampleService.js?name=service "Example service")]

Hizmetlerin nasıl uygulandığı hakkında daha fazla bilgi edinmek [için, kaçırdığınız Reaktif Programlamaya giriş](https://gist.github.com/staltz/868e7e9bc2a7b8c1f754)etüt'e bakın.

**model/örnekModels.js**

[!code-javascript[Example model](~/remote-monitoring-webui/src/walkthrough/services/models/exampleModels.js?name=models "Example model")]

**ÖrnekService.js'yi** **src/services** klasörüne kopyalayın ve **örnekModels.js'i** **src/services/models** klasörüne kopyalayın.

Yeni hizmeti dışa aktarmak için **src/services** klasöründeki **index.js** dosyasını güncelleştirin:

```js
export * from './exampleService';
```

Yeni modeli dışa aktarmak için **src/services/models** klasöründeki **index.js** dosyasını güncelleştirin:

```js
export * from './exampleModels';
```

### <a name="set-up-the-calls-to-the-service-from-the-store"></a>Mağazadan servise yapılan aramaları ayarlama

Başlamak için **src/walkthrough/store/reducers** klasöründe örnek bir azaltıcı bulunur:

**örnekReducer.js**

[!code-javascript[Example reducer](~/remote-monitoring-webui/src/walkthrough/store/reducers/exampleReducer.js?name=reducer "Example reducer")]

Kopya **örneğiReducer.js** **src/store/reducers** klasörüne.

Redüktör ve **Destanlar**hakkında daha fazla bilgi edinmek için [redux-gözlemlenebilir](https://redux-observable.js.org/)bakın.

### <a name="configure-the-middleware"></a>Ara yazılımı yapılandırma

Middleware yapılandırmak için, **src/store** klasöründeki **rootReducer.js** dosyasına azaltıcıyı ekleyin:

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

**src/store** klasöründeki **rootEpics.js** dosyasına destanları ekleyin:

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

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında web Kullanıcı Arabirimi'nde hizmet eklemenize veya özelleştirmenize yardımcı olacak kaynakları öğrendiniz.

Şimdi bir hizmet tanımlamış, bir sonraki adım, hizmet tarafından döndürülen verileri görüntüleyen [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir ızgara eklemektir.](iot-accelerators-remote-monitoring-customize-grid.md)

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için uzaktan [izleme mimarisine](iot-accelerators-remote-monitoring-sample-walkthrough.md)bakın.
