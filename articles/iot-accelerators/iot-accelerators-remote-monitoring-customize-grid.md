---
title: Uzaktan Izleme çözümü Kullanıcı arabirimine kılavuz ekleme | Microsoft Docs
description: Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki bir sayfaya yeni bir GID ekleme konusu gösterilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/04/2018
ms.topic: conceptual
ms.custom: devx-track-javascript
ms.openlocfilehash: 0eab7d743823109ed138ce19f35cea0ffb143422
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87422734"
---
# <a name="add-a-custom-grid-to-the-remote-monitoring-solution-accelerator-web-ui"></a>Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel kılavuz ekleme

Bu makalede, uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimindeki bir sayfaya nasıl yeni bir kılavuz ekleyeceğiniz gösterilmektedir. Makalede şunları açıklanmaktadır:

- Yerel bir geliştirme ortamı hazırlama.
- Web Kullanıcı arabirimindeki bir sayfaya yeni kılavuz ekleme.

Bu makaledeki örnek kılavuz, hizmetin [Uzaktan izleme çözümü Hızlandırıcısı Web UI nasıl yapılır makalesine özel bir hizmet eklemesini](iot-accelerators-remote-monitoring-customize-service.md) sağlayan hizmetten alınan verileri gösterir.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılır kılavuzundaki adımları tamamlayabilmeniz için, yerel geliştirme makinenizde aşağıdaki yazılımların yüklü olması gerekir:

- [Git](https://git-scm.com/downloads)
- [Node.js](https://nodejs.org/download/)

## <a name="before-you-start"></a>Başlamadan önce

Devam etmeden önce aşağıdaki makalelerdeki adımları tamamlamalısınız:

- [Uzaktan izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel bir sayfa ekleyin](iot-accelerators-remote-monitoring-customize-page.md).
- [Uzaktan Izleme çözümü Hızlandırıcısı Web Kullanıcı arabirimine özel bir hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)

## <a name="add-a-grid"></a>Kılavuz ekleme

Web Kullanıcı arabirimine kılavuz eklemek için, Kılavuzu tanımlayan kaynak dosyaları eklemeniz ve mevcut bazı dosyaları değiştirerek Web Kullanıcı arabirimini yeni bileşenden haberdar hale getirmeniz gerekir.

### <a name="add-the-new-files-that-define-the-grid"></a>Kılavuzu tanımlayan yeni dosyaları ekleyin

Başlangıç yapmanız için, **src/izlenecek yol/bileşenler/sayfalar/pageWithGrid/examplegrid** klasörü bir kılavuz tanımlayan dosyaları içerir:

**exampleGrid.js**

[!code-javascript[Example grid](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithGrid/exampleGrid/exampleGrid.js?name=grid "Example grid")]

**exampleGridConfig.js**

[!code-javascript[Example grid configuration](~/remote-monitoring-webui/src/walkthrough/components/pages/pageWithGrid/exampleGrid/exampleGridConfig.js?name=gridconfig "Example grid configuration")]

Src/ **izlenecek yol/bileşenler/sayfalar/pageWithGrid/exampleGrid** klasörünü **src/Components/Pages/example** klasörüne kopyalayın.

### <a name="add-the-grid-to-the-page"></a>Kılavuza kılavuza ekleme

**Kaynak/bileşenler/sayfalar/örnek/basicPage.container.js** hizmet tanımlarını içeri aktarmak için aşağıdaki gibi değiştirin:

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

Kılavuza eklemek için **src/Components/Pages/example/basicPage.js** aşağıdaki gibi değiştirin:

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

Testleri güncelleştirmek için **src/Components/Pages/example/basicPage.test.js** aşağıdaki gibi değiştirin:

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

## <a name="test-the-grid"></a>Kılavuzu test etme

Web Kullanıcı arabirimi zaten yerel olarak çalışmıyorsa, deponun yerel kopyasının kökünde aşağıdaki komutu çalıştırın:

```cmd/sh
npm start
```

Önceki komut, Kullanıcı arabirimini ' de yerel olarak çalıştırır `http://localhost:3000/dashboard` . Hizmetten gelen ızgara görüntüleme verilerini görmek için **örnek** sayfasına gidin.

## <a name="select-rows"></a>Satırları Seç

Kullanıcının kılavuzda satırları seçmesini sağlamak için iki seçenek vardır:

### <a name="hard-select-rows"></a>Sabit seçme satırları

Bir kullanıcının aynı anda birden çok satır üzerinde işlem yapması gerekiyorsa, satırlarda onay kutularını kullanın:

1. Kılavuza sunulan **Columndefs** öğesine bir **checkboxcolumn** ekleyerek satırların sabit seçimini etkinleştirin. **Checkboxcolumn** , **/src/Components/Shared/pcsGrid/pcsGrid.js**tanımlanmıştır:

    ```js
    this.columnDefs = [
      checkboxColumn,
      exampleColumnDefs.id,
      exampleColumnDefs.description
    ];
    ```

1. Seçili öğelere erişmek için iç kılavuz API 'sine bir başvuru alırsınız:

    ```js
    onGridReady = gridReadyEvent => {
      this.gridApi = gridReadyEvent.api;
      // Call the onReady props if it exists
      if (isFunc(this.props.onGridReady)) {
        this.props.onGridReady(gridReadyEvent);
      }
    };
    ```

1. Kılavuzdaki bir satır sabit seçildiğinde sayfaya bağlam düğmeleri sağlar:

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

1. Bağlam düğmesine tıklandığında, çalışmanız için sabit seçili öğeleri alın:

    ```js
    doSomething = () => {
      //Just for demo purposes. Don't console log in a real grid.
      console.log('Hard selected rows', this.gridApi.getSelectedRows());
    };
    ```

### <a name="soft-select-rows"></a>Yumuşak-seçme satırları

Kullanıcının yalnızca tek bir satırda işlem yapması gerekiyorsa, **Columndefs**içindeki bir veya daha fazla sütun için bir geçici seçme bağlantısı yapılandırın.

1. **exampleGridConfig.js**, bir **Columndef**Için **Cellrendererframework** olarak **softselectlinkrenderer** ekleyin.

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

1. Yumuşak seçim bağlantısına tıklandığında **Onsoftselectchange** olayını tetikler. Ayrıntılar açılan menüsü açma gibi bu satır için istediğiniz eylemi gerçekleştirin. Bu örnek yalnızca konsola yazar:

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

Bu makalede, uzaktan Izleme çözüm hızlandırıcısında Web Kullanıcı arabirimindeki sayfaları eklemenize veya özelleştirmenize yardımcı olacak kaynaklar hakkında bilgi edindiniz.

Artık bir kılavuz tanımladınız, bir sonraki adım, örnek sayfada görüntülenen [Uzaktan izleme çözümü Hızlandırıcı Web Kullanıcı arabirimine özel bir açılır pencere eklemektir](iot-accelerators-remote-monitoring-customize-flyout.md) .

Uzaktan Izleme çözümü Hızlandırıcısı hakkında daha fazla kavramsal bilgi için bkz. [Uzaktan izleme mimarisi](iot-accelerators-remote-monitoring-sample-walkthrough.md).
