---
title: Uzaktan İzleme çözümü Kullanıcı Arabirimi'ne bir uçuş ekleme - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'nde bir sayfaya nasıl yeni bir flyout ekleyeceğiniz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/05/2018
ms.topic: conceptual
ms.openlocfilehash: ccb1a7ff6abbc68f42c7632a8ba7a392b2c48794
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447123"
---
# <a name="add-a-custom-flyout-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir uçuş ekleme

Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ndeki bir sayfaya nasıl yeni bir uçuş ekleyeceğiniz gösterilmektedir. Makalede şöyle anlatılmaktadır:

- Yerel bir kalkınma ortamı nasıl hazırlanır.
- Web Web Arama Hizmeti'ndeki bir sayfaya yeni bir uçuş ekleme.

Bu makaledeki örnek uçuş, uzaktan izleme çözüm [hızlandırıcı web Kullanıcı Arabirimi'ne özel bir ızgara eklemenin](iot-accelerators-remote-monitoring-customize-grid.md) nasıl ekleniş yapılacağını gösteren ızgaraile sayfada görüntülenir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için, yerel geliştirme makinenize aşağıdaki yazılımın yüklenmesi gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce aşağıdaki makalelerdeki adımları tamamlamanız gerekir:

- [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir sayfa ekleyin.](iot-accelerators-remote-monitoring-customize-page.md)
- [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)
- [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir ızgara ekleme](iot-accelerators-remote-monitoring-customize-grid.md)

## <a name="add-a-flyout"></a>Açılır öğe ekleme

Web Web Arama Arabirimi'ne bir flyout eklemek için, flyout tanımlayan kaynak dosyaları eklemeniz ve web UI'nin yeni bileşenden haberdar olmasını sağlamak için varolan bazı dosyaları değiştirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-flyout"></a>Flyout tanımlayan yeni dosyaları ekleme

Başlamak **için, src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout** klasörü bir uçuş tanımlayan dosyaları içerir:

**örnekFlyout.container.js**

[!code-javascript[Example flyout container](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.container.js?name=flyoutcontainer "Example flyout container")]

**ÖrnekFlyout.js**

[!code-javascript[Example flyout](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithFlyout/flyouts/exampleFlyout/exampleFlyout.js?name=flyout "Example flyout")]

**src/walkthrough/components/pages/pages/pageWithFlyout/flyouts** klasörünü **src/components/pages/example** klasörüne kopyalayın.

### <a name="add-the-flyout-to-the-page"></a>Sayfaya flyout ekleme

Flyout eklemek için **src/components/pages/example/basicPage.js** adresini değiştirin.

**Bileşenlerden/paylaşılanlardan** yapılan ithalata **Btn** ekleyin ve **svgs** ve **ExampleFlyoutContainer**için ithalat ekleyin:

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

**closedFlyoutState** için bir **const** tanımı ekleyin ve oluşturucu daki duruma ekleyin:

```js
const closedFlyoutState = { openFlyoutName: undefined };

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null, closedFlyoutState };
  }
```

**BasicPage** sınıfına aşağıdaki işlevleri ekleyin:

```js
  closeFlyout = () => this.setState(closedFlyoutState);

  openFlyout = (name) => () => this.setState({ openFlyoutName: name });
```

**Render** işlevine aşağıdaki **const** tanımları ekleyin:

```js
    const { openFlyoutName } = this.state;

    const isExampleFlyoutOpen = openFlyoutName === 'example';
```

Bağlam menüsüne flyout açmak için bir düğme ekleyin:

```js
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
        <Btn svg={svgs.reconfigure} onClick={this.openFlyout('example')}>{t('walkthrough.pageWithFlyout.open')}</Btn>
      </ContextMenu>,
```

Sayfa içeriğine bazı metinler ve çıkış kapsayıcısı ekleyin:

```js
      <PageContent className="basic-page-container" key="page-content">
        {t('walkthrough.pageWithFlyout.pageBody')}
        { isExampleFlyoutOpen && <ExampleFlyoutContainer onClose={this.closeFlyout} /> }
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
```

## <a name="test-the-flyout"></a>Uçuşu test edin

Web Web Arama Ekibi zaten yerel olarak çalışmıyorsa, deponun yerel kopyasının kökünde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komut ui'yi yerel [http://localhost:3000/dashboard](http://localhost:3000/dashboard)olarak çalışır. **Örnek** sayfasına gidin ve **Flyout'u Aç'ı**tıklatın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında web Kullanıcı Arabirimi'ndeki sayfaları eklemenize veya özelleştirmenize yardımcı olacak kaynakları öğrendiniz.

Şimdi bir sayfada bir flyout tanımladık, bir sonraki adım [Uzaktan İzleme çözüm hızlandırıcı web UI panoya bir panel eklemektir.](iot-accelerators-remote-monitoring-customize-panel.md)

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için uzaktan [izleme mimarisine](iot-accelerators-remote-monitoring-sample-walkthrough.md)bakın.
