---
title: Uzaktan İzleme çözümünü özelleştirin Kullanıcı Arabirimi - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcı kullanıcı arabirimi için kaynak koduna nasıl erişebileceğiniz ve bazı özelleştirmeler nasıl yapabileceğiniz hakkında bilgi verilmektedir.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/09/2018
ms.topic: conceptual
ms.openlocfilehash: eb3d5fea68b5b1b6e648943cb3dbaab5857e9e07
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68608013"
---
# <a name="customize-the-remote-monitoring-solution-accelerator"></a>Uzaktan İzleme çözüm hızlandırıcısını özelleştirin

Bu makalede, kaynak koduna nasıl erişebileceğiniz ve Uzaktan İzleme çözüm hızlandırıcı kullanıcı arabirimi'ni nasıl özelleştirebileceğiniz hakkında bilgi verilmektedir.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="prepare-a-local-development-environment-for-the-ui"></a>UI için yerel bir geliştirme ortamı hazırlama

Uzaktan İzleme çözüm hızlandırıcı ui kodu React.js çerçevesi kullanılarak uygulanır. Kaynak kodunu [azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) GitHub deposunda bulabilirsiniz.

UI'de değişiklik yapmak için, bir kopyasını yerel olarak çalıştırabilirsiniz. Telemetri alma gibi eylemleri tamamlamak için, yerel kopya çözümün dağıtılmış bir örneğine bağlanır.

Aşağıdaki adımlar, UI geliştirme için yerel bir ortam ayarlama işlemini ana hatlarını verebilmektedir:

1. **Pccs** CLI'yi kullanarak çözüm hızlandırıcının **temel** bir örneğini dağıtın. Dağıtımınızın adını ve sanal makine için sağladığınız kimlik bilgilerini not edin. Daha fazla bilgi için [CLI'yi kullanarak Dağıt'a](iot-accelerators-remote-monitoring-deploy-cli.md)bakın.

1. SSH'nin çözümünüzdeki mikro hizmetleri barındıran sanal makineye erişimini sağlamak için Azure portalını veya Azure Bulut Kabuğu'nu kullanın. Örnek:

    ```azurecli-interactive
    az network nsg rule update --name SSH --nsg-name {your solution name}-nsg --resource-group {your solution name} --access Allow
    ```

    Yalnızca test ve geliştirme sırasında SSH erişimine olanak sağlar. SSH'yi etkinleştiriseniz, [kullanmayı bitirir bitirmez devre dışı bırakmanız gerekir.](../security/fundamentals/network-best-practices.md#disable-rdpssh-access-to-virtual-machines)

1. Sanal makinenizin adını ve genel IP adresini bulmak için Azure portalını veya Azure Bulut Kabuğu'nu kullanın. Örnek:

    ```azurecli-interactive
    az resource list --resource-group {your solution name} -o table
    az vm list-ip-addresses --name {your vm name from previous command} --resource-group {your solution name} -o table
    ```

1. Sanal makinenize bağlanmak için SSH'yi kullanın. Önceki adımdaki IP adresini ve çözümü dağıtmak için **bilgisayarları** çalıştırdığınızda sağladığınız kimlik bilgilerini kullanın. Komut, `ssh` Azure Bulut Kabuğu'nda kullanılabilir.

1. Yerel UX'nin bağlanmasına izin vermek için, sanal makinedeki bash kabuğunda aşağıdaki komutları çalıştırın:

    ```sh
    cd /app
    sudo ./start.sh --unsafe
    ```

1. Komutun tamamlanıp web sitesinin başladığını göredikten sonra, sanal makineyle bağlantınızı kesebilirsiniz.

1. [Azure-iot-pcs-remote-monitoring-webui](https://github.com/Azure/azure-iot-pcs-remote-monitoring-webui) deposunun yerel kopyanızda, dağıtılan çözümünuzun URL'sini eklemek için **.env** dosyasını edin:

    ```config
    NODE_PATH = src/
    REACT_APP_BASE_SERVICE_URL=https://{your solution name}.azurewebsites.net/
    ```

1. Komut isteminde, klasörün `azure-iot-pcs-remote-monitoring-webui` yerel kopyasına gidin.

1. Gerekli kitaplıkları yüklemek ve ui'yi yerel olarak çalıştırmak için aşağıdaki komutları çalıştırın:

    ```cmd/sh
    npm install
    npm start
    ```

1. Önceki komut ui'yi yerel olarak\/http: /localhost:3000/dashboard adresinde çalıştırın. Site çalışırken kodu güncelleyebilir ve dinamik olarak güncelleştirebilirsiniz.

## <a name="customize-the-layout"></a>Düzeni özelleştirme

Uzaktan İzleme çözümündeki her sayfa, kaynak kodundaki *paneller* olarak adlandırılan bir dizi denetimden oluşur. **Pano** sayfası beş panelden oluşur: Genel Bakış, Harita, Uyarılar, Telemetri ve Analitik. Her sayfayı ve panellerini tanımlayan kaynak kodunu [pcs-remote-monitoring-webui](https://github.com/Azure/pcs-remote-monitoring-webui) GitHub deposunda bulabilirsiniz. Örneğin, **Pano** sayfasını, düzenini ve sayfadaki panelleri tanımlayan kod [src/components/pages/dashboard](https://github.com/Azure/pcs-remote-monitoring-webui/tree/master/src/components/pages/dashboard) klasöründe bulunur.

Paneller kendi düzenini ve boyutlandırmalarını yönettiğinden, sayfanın düzenini kolayca değiştirebilirsiniz. Dosyadaki **PageContent** öğesinde `src/components/pages/dashboard/dashboard.js` aşağıdaki değişiklikleri yapın:

* Harita ve telemetri panellerinin konumlarını değiştirin.
* Harita ve analiz panellerinin göreli genişliklerini değiştirin.

```javascript
<PageContent className="dashboard-container">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        activeDeviceGroup={activeDeviceGroup}
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={analyticsIsPending || devicesIsPending}
        error={deviceGroupError || devicesError || analyticsError}
        t={t} />
    </Cell>
    <Cell className="col-6">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <AlertsPanel
        alerts={currentActiveAlertsWithName}
        isPending={analyticsIsPending || rulesIsPending}
        error={rulesError || analyticsError}
        t={t}
        deviceGroups={deviceGroups} />
    </Cell>
    <Cell className="col-4">
      <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          analyticsVersion={analyticsVersion}
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlert={devicesInAlert}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || analyticsIsPending}
          error={azureMapsKeyError || devicesError || analyticsError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <AnalyticsPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        topAlerts={topAlertsWithName}
        alertsPerDeviceId={alertsPerDeviceType}
        criticalAlertsChange={criticalAlertsChange}
        isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || analyticsError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    {
      Config.showWalkthroughExamples &&
      <Cell className="col-4">
        <ExamplePanel t={t} />
      </Cell>
    }
  </Grid>
</PageContent>
```

![Panel düzenini değiştirme](./media/iot-accelerators-remote-monitoring-customize/layout.png)

Ayrıca, [bir paneli çoğaltıp özelleştirdiğinizde](#duplicate-and-customize-an-existing-control)aynı panelin birkaç örneğini veya birkaç sürümü ekleyebilirsiniz. Aşağıdaki örnek, telemetri panelinin iki örneğini nasıl ekleyeceğinizi gösterir. Bu değişiklikleri yapmak için `src/components/pages/dashboard/dashboard.js` dosyayı değiştirin:

```javascript
<PageContent className="dashboard-container">
  <Grid>
    <Cell className="col-1 devices-overview-cell">
      <OverviewPanel
        activeDeviceGroup={activeDeviceGroup}
        openWarningCount={openWarningCount}
        openCriticalCount={openCriticalCount}
        onlineDeviceCount={onlineDeviceCount}
        offlineDeviceCount={offlineDeviceCount}
        isPending={analyticsIsPending || devicesIsPending}
        error={deviceGroupError || devicesError || analyticsError}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <TelemetryPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        telemetry={telemetry}
        isPending={telemetryIsPending}
        lastRefreshed={lastRefreshed}
        error={deviceGroupError || telemetryError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    <Cell className="col-3">
      <AlertsPanel
        alerts={currentActiveAlertsWithName}
        isPending={analyticsIsPending || rulesIsPending}
        error={rulesError || analyticsError}
        t={t}
        deviceGroups={deviceGroups} />
    </Cell>
    <Cell className="col-4">
      <PanelErrorBoundary msg={t('dashboard.panels.map.runtimeError')}>
        <MapPanel
          analyticsVersion={analyticsVersion}
          azureMapsKey={azureMapsKey}
          devices={devices}
          devicesInAlert={devicesInAlert}
          mapKeyIsPending={azureMapsKeyIsPending}
          isPending={devicesIsPending || analyticsIsPending}
          error={azureMapsKeyError || devicesError || analyticsError}
          t={t} />
      </PanelErrorBoundary>
    </Cell>
    <Cell className="col-6">
      <AnalyticsPanel
        timeSeriesExplorerUrl={timeSeriesParamUrl}
        topAlerts={topAlertsWithName}
        alertsPerDeviceId={alertsPerDeviceType}
        criticalAlertsChange={criticalAlertsChange}
        isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
        error={devicesError || rulesError || analyticsError}
        theme={theme}
        colors={chartColorObjects}
        t={t} />
    </Cell>
    {
      Config.showWalkthroughExamples &&
      <Cell className="col-4">
        <ExamplePanel t={t} />
      </Cell>
    }
  </Grid>
</PageContent>
```

Daha sonra her panelde farklı telemetri görüntüleyebilirsiniz:

![Çoklu telemetri panelleri](./media/iot-accelerators-remote-monitoring-customize/multiple-telemetry.png)

## <a name="duplicate-and-customize-an-existing-control"></a>Varolan denetimi çoğaltma ve özelleştirme

Aşağıdaki adımlar, varolan bir paneli nasıl yineleyip değiştirip sonra değiştirilen sürümü nasıl kullanacağımı ana hatlarını verebilmiştir. Adımlar, **uyarılar** panelini örnek olarak kullanır:

1. Deponun yerel kopyasında, `src/components/pages/dashboard/panels` klasördeki **uyarılar** klasörünün bir kopyasını yapın. Yeni kopyayı **cust_alerts.**

1. **cust_alerts** klasöründe **alertsPanel.js** dosyasında, **CustAlertsPanel**olmak üzere sınıfın adını edin:

    ```javascript
    export class CustAlertsPanel extends Component {
    ```

1. `src/components/pages/dashboard/panels/index.js` Dosyaya aşağıdaki satırı ekleyin:

    ```javascript
    export * from './cust_alerts';
    ```

1. Dosyadaki `CustAlertsPanel` yle `src/components/pages/dashboard/dashboard.js` değiştir: `alertsPanel`

    ```javascript
    import {
      OverviewPanel,
      CustAlertsPanel,
      TelemetryPanel,
      KpisPanel,
      MapPanel,
      transformTelemetryResponse,
      chartColors
    } from './panels';

    ...

    <Cell className="col-3">
      <CustAlertsPanel
        alerts={currentActivealertsWithName}
        isPending={kpisIsPending || rulesIsPending}
        error={rulesError || kpisError}
        t={t} />
    </Cell>
    ```

Şimdi **custAlerts**adlı bir kopyası ile orijinal **uyarılar** paneli değiştirdiniz. Bu kopya orijinalle aynıdır. Artık kopyayı değiştirebilirsiniz. Örneğin, **uyarılar** panelinde sütun sırasını değiştirmek için:

1. `src/components/pages/dashboard/panels/cust_alerts/alertsPanel.js` dosyasını açın.

1. Aşağıdaki kod snippet gösterildiği gibi sütun tanımları değiştirin:

    ```javascript
    this.columnDefs = [
      rulesColumnDefs.severity,
      {
        headerName: 'rules.grid.count',
        field: 'count'
      },
      {
        ...rulesColumnDefs.ruleName,
        minWidth: 200
      },
      rulesColumnDefs.explore
    ];
    ```

Aşağıdaki ekran görüntüsü **uyarılar** panelinin yeni sürümünü gösterir:

![uyarıları paneli güncellendi](./media/iot-accelerators-remote-monitoring-customize/reorder-columns.png)

## <a name="customize-the-telemetry-chart"></a>Telemetri grafiğini özelleştirme

Klasördeki `src/components/pages/dashboard/panels/telemtry` **dosyalar, Pano** sayfasındaki telemetri grafiğini tanımlar. UI, dosyadaki çözüm arka ucundan telemetriyi `src/services/telemetryService.js` alır. Aşağıdaki adımlar, telemetri grafiğinde görüntülenen süreyi 15 dakikadan 5 dakikaya nasıl değiştireceğinizi gösterir:

1. Dosyada `src/services/telemetryService.js` **getTelemetryByDeviceIdP15M**adlı işlevi bulun. Bu işlevin bir kopyasını yapın ve kopyayı aşağıdaki gibi değiştirin:

    ```javascript
    static getTelemetryByDeviceIdP5M(devices = []) {
      return TelemetryService.getTelemetryByMessages({
        from: 'NOW-PT5M',
        to: 'NOW',
        order: 'desc',
        devices
      });
    }
    ```

1. Telemetri grafiğini doldurmak için bu yeni işlevi `src/components/pages/dashboard/dashboard.js` kullanmak için dosyayı açın. Telemetri akışını niçin başlatını getiren satırı bulun ve aşağıdaki gibi değiştirin:

    ```javascript
    const getTelemetryStream = ({ deviceIds = [] }) => TelemetryService.getTelemetryByDeviceIdP5M(deviceIds)
    ```

Telemetri grafiği artık beş dakikalık telemetri verilerini gösterir:

![Bir günü gösteren telemetri grafiği](./media/iot-accelerators-remote-monitoring-customize/telemetry-period.png)

## <a name="add-a-new-kpi"></a>Yeni bir KPI ekleme

**Pano** sayfası **Analytics** panelinde KP'leri görüntüler. Bu KP'ler `src/components/pages/dashboard/dashboard.js` dosyada hesaplanır. KP'ler `src/components/pages/dashboard/panels/analytics/analyticsPanel.js` dosya tarafından işlenir. Aşağıdaki adımlar, **Pano** sayfasında yeni bir KPI değerinin nasıl hesaplanacağını ve nasıl oluşturultuğa açıklanmıştır. Gösterilen örnek uyarı uyarıları KPI yeni bir yüzde değişikliği eklemektir:

1. `src/components/pages/dashboard/dashboard.js` dosyasını açın. Bir **warningAlertsChange** özelliğini içerecek şekilde **ilkDurum** nesnesini değiştirin:

    ```javascript
    const initialState = {
      ...

      // Analytics data
      analyticsVersion: 0,
      currentActiveAlerts: [],
      topAlerts: [],
      alertsPerDeviceId: {},
      criticalAlertsChange: 0,
      warningAlertsChange: 0,
      analyticsIsPending: true,
      analyticsError: null

      ...
    };
    ```

1. **GeçerliAlertsStats** nesnesini, **toplam WarningCount'ı** özellik olarak içerecek şekilde değiştirin:

    ```javascript
    return {
      openWarningCount: (acc.openWarningCount || 0) + (isWarning && isOpen ? 1 : 0),
      openCriticalCount: (acc.openCriticalCount || 0) + (isCritical && isOpen ? 1 : 0),
      totalWarningCount: (acc.totalWarningCount || 0) + (isWarning ? 1 : 0),
      totalCriticalCount: (acc.totalCriticalCount || 0) + (isCritical ? 1 : 0),
      alertsPerDeviceId: updatedAlertsPerDeviceId
    };
    ```

1. Yeni KPI'yi hesaplayın. Kritik uyarılar için hesaplama sayısı bulun. Kodu çoğaltmave kopyayı aşağıdaki gibi değiştirin:

    ```javascript
    // ================== Warning Alerts Count - START
    const currentWarningAlerts = currentAlertsStats.totalWarningCount;
    const previousWarningAlerts = previousAlerts.reduce(
      (cnt, { severity }) => severity === Config.ruleSeverity.warning ? cnt + 1 : cnt,
      0
    );
    const warningAlertsChange = ((currentWarningAlerts - previousWarningAlerts) / currentWarningAlerts * 100).toFixed(2);
    // ================== Warning Alerts Count - END
    ```

1. KPI akışına yeni **warningAlertsChange** KPI ekleyin:

    ```javascript
    return ({
      analyticsIsPending: false,
      analyticsVersion: this.state.analyticsVersion + 1,

      // Analytics data
      currentActiveAlerts,
      topAlerts,
      criticalAlertsChange,
      warningAlertsChange,
      alertsPerDeviceId: currentAlertsStats.alertsPerDeviceId,

      ...
    });
    ```

1. UI işlemek için kullanılan durum verilerine yeni **warningAlertsChange** KPI ekleyin:

    ```javascript
    const {
      ...

      analyticsVersion,
      currentActiveAlerts,
      topAlerts,
      alertsPerDeviceId,
      criticalAlertsChange,
      warningAlertsChange,
      analyticsIsPending,
      analyticsError,

      ...
    } = this.state;
    ```

1. KP'ler paneline aktarılan verileri güncelleştirin:

    ```javascript
    <AnalyticsPanel
      timeSeriesExplorerUrl={timeSeriesParamUrl}
      topAlerts={topAlertsWithName}
      alertsPerDeviceId={alertsPerDeviceType}
      criticalAlertsChange={criticalAlertsChange}
      warningAlertsChange={warningAlertsChange}
      isPending={analyticsIsPending || rulesIsPending || devicesIsPending}
      error={devicesError || rulesError || analyticsError}
      theme={theme}
      colors={chartColorObjects}
      t={t} />
    ```

Dosyadaki değişiklikleri tamamladınız. `src/components/pages/dashboard/dashboard.js` Aşağıdaki adımlar, yeni KPI'yi görüntülemek için `src/components/pages/dashboard/panels/analytics/analyticsPanel.js` dosyada yapacak değişiklikleri açıklar:

1. Yeni KPI değerini almak için aşağıdaki kod satırını aşağıdaki gibi değiştirin:

    ```javascript
    const { t, isPending, criticalAlertsChange, warningAlertsChange, alertsPerDeviceId, topAlerts, timeSeriesExplorerUrl, error } = this.props;
    ```

1. Yeni KPI değerini aşağıdaki gibi görüntülemek için biçimlendirmeyi değiştirin:

    ```javascript
    <div className="analytics-cell">
      <div className="analytics-header">{t('dashboard.panels.analytics.criticalAlerts')}</div>
      <div className="critical-alerts">
        {
          !showOverlay &&
            <div className="analytics-percentage-container">
              <div className="analytics-value">{ !isNaN(criticalAlertsChange) ? criticalAlertsChange : 0 }</div>
              <div className="analytics-percentage-sign">%</div>
            </div>
        }
      </div>
      <div className="critical-alerts">
        {
          !showOverlay &&
            <div className="analytics-percentage-container">
              <div className="analytics-value">{ !isNaN(warningAlertsChange) ? warningAlertsChange : 0 }</div>
              <div className="analytics-percentage-sign">%</div>
            </div>
        }
      </div>
    </div>
    ```

**Pano** sayfası artık yeni KPI değerini görüntüler:

![Uyarı KPI](./media/iot-accelerators-remote-monitoring-customize/new-kpi.png)

## <a name="customize-the-map"></a>Haritayı özelleştirin

Çözümdeki harita bileşenlerinin ayrıntıları için GitHub'daki [haritayı Özelleştir](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/wiki/Developer-Reference-Guide#upgrade-map-key-to-see-devices-on-a-dynamic-map) sayfasına bakın.

<!--
### Connect an external visualization tool

See the [Connect an external visualization tool](https://github.com/Azure/azure-iot-pcs-remote-monitoring-dotnet/) page in GitHub for details of how to connect an external visualization tool.

-->

## <a name="other-customization-options"></a>Diğer özelleştirme seçenekleri

Uzaktan İzleme çözümündeki sunu ve görselleştirme katmanını daha fazla değiştirmek için kodu değiştirebilirsiniz. İlgili GitHub depoları şunlardır:

* [Azure IoT Çözümleri (.NET) için yapılandırma mikro hizmeti](https://github.com/Azure/remote-monitoring-services-dotnet/tree/master/config)
* [Azure IoT Çözümleri (Java) için yapılandırma mikro hizmeti](https://github.com/Azure/remote-monitoring-services-java/tree/master/config)
* [Azure IoT PCS Uzaktan İzleme Web Kullanıcı II](https://github.com/Azure/pcs-remote-monitoring-webui)

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, Uzaktan İzleme çözüm hızlandırıcısında web Kullanıcı Arabirimi'ni özelleştirmenize yardımcı olacak kaynakları öğrendiniz. UI'yi özelleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

* [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir sayfa ekleme](iot-accelerators-remote-monitoring-customize-page.md)
* [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir hizmet ekleme](iot-accelerators-remote-monitoring-customize-service.md)
* [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir ızgara ekleme](iot-accelerators-remote-monitoring-customize-grid.md)
* [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'ne özel bir uçuş ekleme](iot-accelerators-remote-monitoring-customize-flyout.md)
* [Uzaktan İzleme çözüm hızlandırıcı web Kullanıcı Arabirimi'nde panoya özel bir panel ekleme](iot-accelerators-remote-monitoring-customize-panel.md)

Uzaktan İzleme çözüm hızlandırıcısı hakkında daha fazla kavramsal bilgi için [bkz.](iot-accelerators-remote-monitoring-sample-walkthrough.md)

Uzaktan İzleme çözümü mikro hizmetlerini özelleştirme hakkında daha fazla bilgi için, [bir microservice'i Özelleştir ve yeniden dağıtma](iot-accelerators-microservices-example.md)yı görün.
<!-- Next tutorials in the sequence -->
