---
title: Uzaktan İzleme çözümüne ızgara ekleme Kullanıcı Arabirimi - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'nde bir sayfaya nasıl yeni bir gid ekleyeceğiniz gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/04/2018
ms.topic: conceptual
ms.openlocfilehash: a24cb7f39ccb8ea07d4dde2869dc7c924b91983a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61447106"
---
# <a name="add-a-custom-grid-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir ızgara ekleme

Bu makalede, Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ndeki bir sayfaya nasıl yeni bir ızgara ekleyeceğiniz gösterilmektedir. Makalede şöyle anlatılmaktadır:

- Yerel bir kalkınma ortamı nasıl hazırlanır.
- Web Web Arama Hizmeti'ndeki bir sayfaya yeni bir ızgara ekleme.

Bu makaledeki örnek Kılavuz, Uzaktan İzleme [çözüm hızlandırıcı web Ara Birimi'ne özel bir hizmet ekle'nin](iot-accelerators-remote-monitoring-customize-service.md) nasıl ekleyeceğimize gösterdiği hizmetteki verileri görüntüler.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yap'la ilgili kılavuzdaki adımları tamamlamak için, yerel geliştirme makinenize aşağıdaki yazılımın yüklenmesi gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce aşağıdaki makalelerdeki adımları tamamlamanız gerekir:

- [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir sayfa ekleyin.](iot-accelerators-remote-monitoring-customize-page.md)
- [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)

## <a name="add-a-grid"></a>Kılavuz ekleme

Web Ara Birimi'ne ızgara eklemek için, ızgarayı tanımlayan kaynak dosyaları eklemeniz ve web UI'nin yeni bileşenden haberdar olmasını sağlamak için varolan bazı dosyaları değiştirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-grid"></a>Izgarayı tanımlayan yeni dosyaları ekleme

Başlamak için **src/walkthrough/components/pages/pagesWithGrid/exampleGrid** klasörü ızgara tanımlayan dosyaları içerir:

**örnekGrid.js**

[!code-javascript[Example grid](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithGrid/exampleGrid/exampleGrid.js?name=grid "Example grid")]

**ÖrnekGridConfig.js**

[!code-javascript[Example grid configuration](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithGrid/exampleGrid/exampleGridConfig.js?name=gridconfig "Example grid configuration")]

**src/walkthrough/components/pages/pages/pageWithGrid/exampleGrid** klasörünü **src/components/pages/example** klasörüne kopyalayın.

### <a name="add-the-grid-to-the-page"></a>Sayfaya ızgara ekleme

Hizmet tanımlarını almak için **src/components/pages/example/basicPage.container.js** adresini aşağıdaki gibi değiştirin:

```js
import { connect } from 'react-redux';
import { translate } from 'react-i18next';

import {
  epics as exampleEpics,
  getExamples,
  getExamplesError,
  getExamplesLastUpdated,
  getExamplesPendingStatus
} from 'store/reducers/exampleReducer';
import { BasicPage } from './basicPage';

// Pass the data
const mapStateToProps = state => ({
  data: getExamples(state),
  error: getExamplesError(state),
  isPending: getExamplesPendingStatus(state),
  lastUpdated: getExamplesLastUpdated(state)
});

// Wrap the dispatch method
const mapDispatchToProps = dispatch => ({
  fetchData: () => dispatch(exampleEpics.actions.fetchExamples())
});

export const BasicPageContainer = translate()(connect(mapStateToProps, mapDispatchToProps)(BasicPage));
```

Izgarayı eklemek için **src/components/pages/example/basicPage.js'i** aşağıdaki gibi değiştirin:

```js
// Copyright (c) Microsoft. All rights reserved.

import React, { Component } from 'react';

import {
  AjaxError,
  ContextMenu,
  PageContent,
  RefreshBar
} from 'components/shared';
import { ExampleGrid } from './exampleGrid';

import './basicPage.css';

export class BasicPage extends Component {
  constructor(props) {
    super(props);
    this.state = { contextBtns: null };
  }

  componentDidMount() {
    const { isPending, lastUpdated, fetchData } = this.props;
    if (!lastUpdated && !isPending) fetchData();
  }

  onGridReady = gridReadyEvent => this.gridApi = gridReadyEvent.api;

  onContextMenuChange = contextBtns => this.setState({ contextBtns });

  render() {
    const { t, data, error, isPending, lastUpdated, fetchData } = this.props;
    const gridProps = {
      onGridReady: this.onGridReady,
      rowData: isPending ? undefined : data || [],
      onContextMenuChange: this.onContextMenuChange,
      t: this.props.t
    };

    return [
      <ContextMenu key="context-menu">
        {this.state.contextBtns}
      </ContextMenu>,
      <PageContent className="basic-page-container" key="page-content">
        <RefreshBar refresh={fetchData} time={lastUpdated} isPending={isPending} t={t} />
        {!!error && <AjaxError t={t} error={error} />}
        {!error && <ExampleGrid {...gridProps} />}
      </PageContent>
    ];
  }
}
```

Testleri güncellemek için **src/components/pages/example/basicPage.test.js** adresini aşağıdaki gibi değiştirin:

```js
// Copyright (c) Microsoft. All rights reserved.

import React from 'react';
import { shallow } from 'enzyme';
import 'polyfills';

import { BasicPage } from './basicPage';

describe('BasicPage Component', () => {
  it('Renders without crashing', () => {

    const fakeProps = {
      data: undefined,
      error: undefined,
      isPending: false,
      lastUpdated: undefined,
      fetchData: () => { },
      t: () => { },
    };

    const wrapper = shallow(
      <BasicPage {...fakeProps} />
    );
  });
});
```

## <a name="test-the-grid"></a>Izgarayı test edin

Web Web Arama Ekibi zaten yerel olarak çalışmıyorsa, deponun yerel kopyasının kökünde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komut ui'yi yerel [http://localhost:3000/dashboard](http://localhost:3000/dashboard)olarak çalışır. Hizmetteki ızgara görüntü verilerini görmek için **Örnek** sayfasına gidin.

## <a name="select-rows"></a>Satırları seçin

Bir kullanıcının ızgaradaki satırları seçmesini etkinleştirmek için iki seçenek vardır:

### <a name="hard-select-rows"></a>Zor seçili satırlar

Bir kullanıcının aynı anda birden çok satırda işlem yapması gerekiyorsa, satırlar üzerindeki onay kutularını kullanın:

1. Kılavuza sağlanan **sütuna** bir **onay kutusuSütun** ekleyerek satırların zor seçimini etkinleştirin. **checkboxSütun** tanımlanır **/src/components/shared/pcsGrid/pcsGrid.js**:

    ```js
    this.columnDefs = [
      checkboxColumn,
      exampleColumnDefs.id,
      exampleColumnDefs.description
    ];
    ```

1. Seçili öğelere erişmek için iç ızgara API'sine başvuru alırsınız:

    ```js
    onGridReady = gridReadyEvent => {
      this.gridApi = gridReadyEvent.api;
      // Call the onReady props if it exists
      if (isFunc(this.props.onGridReady)) {
        this.props.onGridReady(gridReadyEvent);
      }
    };
    ```

1. Kılavuzdaki bir satır zor seçildiğinde sayfaya bağlam düğmeleri sağlayın:

    ```js
    this.contextBtns = [
      <Btn key="context-btn-1" svg={svgs.reconfigure} onClick={this.doSomething()}>Button 1</Btn>,
      <Btn key="context-btn-2" svg={svgs.trash} onClick={this.doSomethingElse()}>Button 2</Btn>
    ];
    ```

    ```js
    onHardSelectChange = (selectedObjs) => {
      const { onContextMenuChange, onHardSelectChange } = this.props;
      // Show the context buttons when there are rows checked.
      if (isFunc(onContextMenuChange)) {
        onContextMenuChange(selectedObjs.length > 0 ? this.contextBtns : null);
      }
      //...
    }
    ```

1. Bir bağlam düğmesi tıklandığında, aşağıdaki leri yapmak için zor seçilen öğeleri alın:

    ```js
    doSomething = () => {
      //Just for demo purposes. Don't console log in a real grid.
      console.log('Hard selected rows', this.gridApi.getSelectedRows());
    };
    ```

### <a name="soft-select-rows"></a>Yumuşak seçili satırlar

Kullanıcının yalnızca tek bir satırda hareket etmesi gerekiyorsa, **Satırlar sütunundaki**bir veya daha fazla sütun için yumuşak seçimli bir bağlantı yapılandırır.

1. **ÖrnekgridConfig.js,** bir sütuniçin **cellRendererFramework** olarak **SoftSelectLinkRenderer** **ekleyinDef**.

    ```js
    export const exampleColumnDefs = {
      id: {
        headerName: 'examples.grid.name',
        field: 'id',
        sort: 'asc',
        cellRendererFramework: SoftSelectLinkRenderer
      }
    };
    ```

1. Yumuşak seçimli bir bağlantı tıklandığında, **onSoftSelectChange** olayını tetikler. Ayrıntıları açma gibi, bu satır için istenilen eylemi gerçekleştirin. Bu örnek sadece konsola yazıyor:

    ```js
    onSoftSelectChange = (rowId, rowData) => {
      //Note: only the Id is reliable, rowData may be out of date
      const { onSoftSelectChange } = this.props;
      if (rowId) {
        //Just for demo purposes. Don't console log a real grid.
        console.log('Soft selected', rowId);
        this.setState({ softSelectedId: rowId });
      }
      if (isFunc(onSoftSelectChange)) {
        onSoftSelectChange(rowId, rowData);
      }
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında web Kullanıcı Arabirimi'ndeki sayfaları eklemenize veya özelleştirmenize yardımcı olacak kaynakları öğrendiniz.

Şimdi bir ızgara tanımlamış, bir sonraki adım örnek sayfada görüntüler [Uzaktan İzleme çözüm hızlandırıcı web UI özel bir flyout eklemektir.](iot-accelerators-remote-monitoring-customize-flyout.md)

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için uzaktan [izleme mimarisine](iot-accelerators-remote-monitoring-sample-walkthrough.md)bakın.
