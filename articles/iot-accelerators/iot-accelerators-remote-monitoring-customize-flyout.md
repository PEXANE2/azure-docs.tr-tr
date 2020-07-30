---
title: Uzaktan Izleme çözümü Kullanıcı arabirimine bir açılır pencere ekleme | Microsoft Docs
description: Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki bir sayfaya yeni bir açılır pencere ekleme konusu gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.custom: devx-track-javascript
ms.openlocfilehash: f10be59501368012b0e4269f402fe258da552e25
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87422751"
---
# <a name="add-a-custom-flyout-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel bir açılır pencere ekleyin

Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki bir sayfanın üzerine yeni bir açılır pencere ekleme konusu gösterilmektedir. Makalede şunları açıklanmaktadır:

- Yerel bir geliştirme ortamı hazırlama.
- Web Kullanıcı arabirimindeki bir sayfaya yeni bir açılır pencere ekleme.

Bu makaledeki örnek açılır pencere, kılavuza, [Uzaktan izleme çözümü Hızlandırıcısı Web UI](iot-accelerators-remote-monitoring-customize-grid.md) ile ilgili nasıl ekleneceğini gösteren özel kılavuz ekleme kılavuzunu içeren sayfada görüntülenir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için, yerel geliştirme makinenizde aşağıdaki yazılımların yüklü olması gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce aşağıdaki makalelerdeki adımları tamamlamalısınız:

- [Uzaktan izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel bir sayfa ekleyin](iot-accelerators-remote-monitoring-customize-page.md).
- [Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel bir hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)
- [Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel kılavuz ekleme](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-flyout"></a>Açılır öğe ekleme

Web Kullanıcı arabirimine bir açılır pencere eklemek için, açılır menüyü tanımlayan kaynak dosyaları eklemeniz ve var olan bazı dosyaları değiştirerek Web Kullanıcı arabirimini yeni bileşenden haberdar hale getirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-flyout"></a>Açılır menüyü tanımlayan yeni dosyaları ekleyin

Başlangıç yapmanız için, **src/Walkthrough/bileşenler/Pages/Pagewithaçılır/Flyout/exampleFlyout** klasörü, bir açılır pencere tanımlayan dosyaları içerir:

**exampleFlyout.container.js**

[!code-javascript[Example flyout container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example flyout container")]

**exampleFlyout.js**

[!code-javascript[Example flyout](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example flyout")]

Src/ **Walkthrough/Components/Pages/Pagewithaçılır/Flyout** klasörünü **src/Components/Pages/example** klasörüne kopyalayın.

### <a name="add-the-flyout-to-the-page"></a>Açılır menüyü sayfaya ekleyin

' **İ/bileşenleri/sayfaları/örnek/basicPage.js** açılır menüyü eklemek için değiştirin.

**Bileşenler/paylaşılan** içeri aktarmaları Için **btn** ekleyin ve **svgs** ve **ExampleFlyoutContainer**için içeri aktarmaları ekleyin:

```js
import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar,
  Btn
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';
import { svgs } from 'utilities';
import { ExampleFlyoutContainer } from './flyouts/exampleFlyout';
```

**Closedflsavstate** için bir **const** tanımı ekleyin ve oluşturucuyu oluşturucuya ekleyin:

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

Aşağıdaki işlevleri **Basicpage** sınıfına ekleyin:

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

**Render** işlevine aşağıdaki **const** tanımlarını ekleyin:

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

Bağlam menüsüne açılan menüyü açmak için bir düğme ekleyin:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Sayfa içeriğine metin ve açılır pencere kapsayıcısını ekleyin:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-flyout"></a>Açılır menüyü test etme

Web Kullanıcı arabirimi zaten yerel olarak çalışmıyorsa, deponun yerel kopyasının kökünde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komut, Kullanıcı arabirimini ' de yerel olarak çalıştırır `http://localhost:3000/dashboard` . **Örnek** sayfasına gidin ve **açılan menüyü aç**' a tıklayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, uzaktan Izleme çözüm hızlandırıcısında Web Kullanıcı arabirimindeki sayfaları eklemenize veya özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Bir sayfada açılır pencere tanımladınız bir sonraki adım, [Uzaktan izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki panoya bir panel eklemektir](iot-accelerators-remote-monitoring-customize-panel.md).

Uzaktan Izleme çözümü Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
