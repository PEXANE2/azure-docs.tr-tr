---
title: Azure IoT Hub ile kesilen bağlantıları izleme ve sorun giderme
description: Azure IoT Hub cihaz bağlantısıyla sık karşılaşılan hataları izlemeyi ve sorun gidermeyi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: jlian
ms.custom: mqtt
ms.openlocfilehash: abce9387d4c23cd6cb5ecf73e3d5c8428d83d459
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83833668"
---
# <a name="monitor-diagnose-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Azure IoT Hub bağlantısını izleme, tanılama ve sorun giderme

Birçok olası hata noktası olduğundan, IoT cihazlarına yönelik bağlantı sorunlarının giderilmesi zor olabilir. Uygulama mantığı, fiziksel ağlar, protokoller, donanım, IoT Hub ve diğer bulut Hizmetleri, sorun oluşmasına neden olabilir. Bir sorunun kaynağını tespit etme ve oluşturma özelliği kritik öneme sahiptir. Ancak, ölçekteki bir IoT çözümünde binlerce cihaz olabilir, bu nedenle tek tek cihazları el ile denetlemek pratik değildir. Bu sorunları ölçeklendirmek, tanılamak ve sorunlarını gidermenize yardımcı olmak için Azure Izleyici aracılığıyla IoT Hub izleme yeteneklerini kullanın. Bu yetenekler IoT Hub ile sınırlıdır, bu nedenle cihazlarınız ve diğer Azure hizmetleri için en iyi uygulamaları izlemeyi izlemeniz önerilir.

## <a name="get-alerts-and-error-logs"></a>Uyarıları ve hata günlüklerini al

Cihazların bağlantısı kesildiğinde uyarıları almak ve günlükleri yazmak için Azure Izleyici 'yi kullanın.

### <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini aç

Cihaz bağlantısı olaylarını ve hatalarını günlüğe kaydetmek için IoT Hub tanılamayı açın. Tanılama günlükleri etkinleştirilmemişse, cihazın bağlantısı kesildiğinde sorun gidermek için herhangi bir bilgi olmayacak ve bu günlükleri mümkün olduğunca erken açmanızı öneririz.

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. IoT Hub 'ınıza gidin.

3. **Tanılama ayarları**' nı seçin.

4. **Tanılamayı aç '** ı seçin.

5. Toplanabilecek **bağlantı** günlüklerini etkinleştirin.

6. Daha kolay analiz için **Log Analytics gönder** ' i açın ([bkz. fiyatlandırma](https://azure.microsoft.com/pricing/details/log-analytics/)). [Bağlantı hatalarını çözme](#resolve-connectivity-errors)bölümündeki örneğe bakın.

   ![Önerilen ayarlar](./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png)

Daha fazla bilgi edinmek için bkz. [Azure IoT Hub sistem durumunu izleme ve sorunları hızlı bir şekilde tanılama](iot-hub-monitor-resource-health.md).

### <a name="set-up-alerts-for-device-disconnect-at-scale"></a>Ölçekte cihaz bağlantısı kesilmeye yönelik uyarıları ayarlama

Cihazların bağlantısı kesildiğinde uyarı almak için, **bağlı cihazlar (Önizleme)** ölçümünde uyarıları yapılandırın.

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. IoT Hub 'ınıza gidin.

3. **Uyarıları**seçin.

4. **Yeni uyarı kuralı**'nı seçin.

5. **Koşul Ekle**' yi seçin ve ardından "bağlı cihazlar (Önizleme)" öğesini seçin.

6. Aşağıdaki istemlere göre eşik ve uyarı ayarlayın.

Daha fazla bilgi edinmek için bkz. [Microsoft Azure uyarılar nelerdir?](../azure-monitor/platform/alerts-overview.md).

#### <a name="detecting-individual-device-disconnects"></a>Ayrı ayrı cihaz bağlantısı kesiliyor

*Cihaz başına* kesilen bağlantıları algılamak için (örneğin, bir fabrikanın çevrimdışı olduğunu bilmeniz gerektiğinde, [Event Grid ile cihaz bağlantısı kesme olaylarını yapılandırın](iot-hub-event-grid.md).

## <a name="resolve-connectivity-errors"></a>Bağlantı hatalarını çözme

Bağlı cihazlar için tanılama günlüklerini ve uyarılarını açtığınızda, hata oluştuğunda uyarılar alırsınız. Bu bölümde, bir uyarı aldığınızda yaygın sorunların nasıl aranacağı açıklanmaktadır. Aşağıdaki adımlarda, tanılama günlüklerinizin Azure Izleyici günlüklerini ayarlamış olduğunuz varsayılmaktadır.

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. IoT Hub 'ınıza gidin.

1. **Günlükleri**seçin.

1. IoT Hub için bağlantı hata günlüklerini yalıtmak için aşağıdaki sorguyu girin ve **Çalıştır**' ı seçin:

    ```kusto
    AzureDiagnostics
    | where ( ResourceType == "IOTHUBS" and Category == "Connections" and Level == "Error")
    ```

1. Sonuçlar varsa, `OperationName` `ResultType` `ResultDescription` hata hakkında daha fazla ayrıntı almak için, (hata kodu) ve (hata iletisi) bölümüne bakın.

   ![Hata günlüğü örneği](./media/iot-hub-troubleshoot-connectivity/diag-logs.png)

1. En yaygın hatalar için sorun çözümleme kılavuzlarını izleyin:

    - **[404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)**
    - **[401003 IoTHubUnauthorized](iot-hub-troubleshoot-error-401003-iothubunauthorized.md)**
    - **[409002 LinkCreationConflict](iot-hub-troubleshoot-error-409002-linkcreationconflict.md)**
    - **[500001 ServerError](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**
    - **[500008 GenericTimeout](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**

## <a name="i-tried-the-steps-but-they-didnt-work"></a>Adımları denedim, ancak bunlar çalışmadı

Önceki adımlarda yardım olmadıysa, şunları deneyin:

* Fiziksel veya uzaktan (SSH gibi) sorunlu cihazlara erişiminiz varsa, sorun gidermeye devam etmek için [cihaz tarafı sorun giderme kılavuzunu](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) izleyin.

* IoT Hub 'ınızın > IoT cihazlarınızın Azure portal >, cihazlarınızın **etkinleştirildiğini** doğrulayın.

* Cihazınız MQTT protokolünü kullanıyorsa, 8883 numaralı bağlantı noktasının açık olduğunu doğrulayın. Daha fazla bilgi için bkz. [IoT Hub bağlanma (MQTT)](iot-hub-mqtt-support.md#connecting-to-iot-hub).

* Azure IoT Hub, [Stack Overflow](https://stackoverflow.com/questions/tagged/azure-iot-hub)veya [Azure desteği](https://azure.microsoft.com/support/options/) [için Microsoft Q&soru sayfasından](https://docs.microsoft.com/answers/topics/azure-iot-hub.html)yardım alın.

Herkesin belgelerinin artırılmasına yardımcı olmak için, bu kılavuz size yardımcı olmadıysa aşağıdaki geri bildirim bölümünde bir yorum bırakın.

## <a name="next-steps"></a>Sonraki adımlar

* Geçici sorunları çözme hakkında daha fazla bilgi için bkz. [geçici hata işleme](/azure/architecture/best-practices/transient-faults).

* Azure IoT SDK ve yeniden denemeleri yönetme hakkında daha fazla bilgi edinmek için bkz. [azure IoT Hub cihaz SDK 'larını kullanarak bağlantı ve güvenilir mesajlaşma yönetimi](iot-hub-reliability-features-in-sdks.md#connection-and-retry).
