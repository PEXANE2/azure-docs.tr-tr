---
title: Azure IoT Hub ile bağlantı bağlantılarını izleme ve sorun giderme
description: Azure IoT Hub için aygıt bağlantısıyla sık karşılaşılan hataları izlemeyi ve sorun gidermeyi öğrenin
author: jlian
manager: briz
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 01/30/2020
ms.author: jlian
ms.openlocfilehash: bed6736fda0c1815964f9017adb1e6fffa9335d9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77110678"
---
# <a name="monitor-diagnose-and-troubleshoot-disconnects-with-azure-iot-hub"></a>Azure IoT Hub ile bağlantı bağlantılarını izleme, tanılama ve sorun giderme

Birçok olası hata noktası olduğundan, IoT aygıtları için bağlantı sorunlarını gidermek zor olabilir. Uygulama mantığı, fiziksel ağlar, protokoller, donanım, IoT Hub ve diğer bulut hizmetlerinin tümü sorunlara neden olabilir. Bir sorunun kaynağını algılama ve tam olarak belirleme yeteneği çok önemlidir. Ancak, ölçekte bir IoT çözümü binlerce cihaza sahip olabilir, bu nedenle tek tek aygıtları el ile kontrol etmek pratik değildir. Bu sorunları ölçekte algılamanıza, tanılamanıza ve sorun gidermenize yardımcı olmak için IoT Hub'ın Azure Monitor aracılığıyla sağladığı izleme özelliklerini kullanın. Bu özellikler IoT Hub'ın gözlemleyebildikleriyle sınırlıdır, bu nedenle cihazlarınız ve diğer Azure hizmetlerini izleme en iyi uygulamaları izlemenizi de öneririz.

## <a name="get-alerts-and-error-logs"></a>Uyarıları ve hata günlüklerini alın

Aygıtlar kesildiğinde uyarı almak ve günlükler yazmak için Azure Monitor'u kullanın.

### <a name="turn-on-diagnostic-logs"></a>Tanılama günlüklerini açma

Aygıt bağlantı olaylarını ve hatalarını günlüğe kaydetmek için IoT Hub için tanılamayı açın. Bu günlükleri mümkün olduğunca erken açmanızı öneririz, çünkü tanılama günlükleri etkin değilse, aygıt bağlantısı kesildiğinde, sorunu gidermek için herhangi bir bilginiz olmaz.

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. IoT hub'ınıza göz atın.

3. **Tanılama ayarlarını**seçin.

4. **Tanılamayı Aç'ı**seçin.

5. **Bağlantılar** günlüklerinin toplanmasını etkinleştirin.

6. Daha kolay analiz **için, Günlük Analytics'e Gönder'i** açın[(fiyatlandırmaya bakın).](https://azure.microsoft.com/pricing/details/log-analytics/) [Bağlantı hatalarını çözümle](#resolve-connectivity-errors)altındaki örneğe bakın.

   ![Önerilen ayarlar](./media/iot-hub-troubleshoot-connectivity/diagnostic-settings-recommendation.png)

Daha fazla bilgi edinmek için [Azure IoT Hub'ının sistem durumunu izleyin ve sorunları hızlı bir şekilde tanıla.](iot-hub-monitor-resource-health.md)

### <a name="set-up-alerts-for-device-disconnect-at-scale"></a>Ölçekte aygıt bağlantısını kesmek için uyarılar ayarlama

Aygıtlar bağlantı kesildiğinde uyarı almak için, **Bağlı aygıtlar (önizleme)** ölçümündeki uyarıları yapılandırın.

1. [Azure portalında](https://portal.azure.com)oturum açın.

2. IoT hub'ınıza göz atın.

3. **Uyarıları**seçin.

4. **Yeni uyarı kuralı**'nı seçin.

5. **Koşul Ekle'yi**seçin ve ardından "Bağlı aygıtlar (önizleme)" seçeneğini belirleyin.

6. İstemleri izleyerek eşiği ve uyarıyı ayarlayın.

Daha fazla bilgi edinmek için Microsoft [Azure'daki uyarıların nelerdir?](../azure-monitor/platform/alerts-overview.md)

#### <a name="detecting-individual-device-disconnects"></a>Tek tek aygıtın bağlantısını algılama

Bir fabrikanın çevrimdışı olduğunu bilmeniz gerektiğinde olduğu gibi *cihaz başına* bağlantı bağlantı larını algılamak için, Aygıt Bağlantısını [Olay Izgarasıyla aygıt bağlantısını kesme olayını yapılandırın.](iot-hub-event-grid.md)

## <a name="resolve-connectivity-errors"></a>Bağlantı hatalarını çözümle

Bağlı aygıtlar için tanı lama günlüklerini ve uyarılarını açtığınızda, hatalar oluştuğunda uyarılar alırsınız. Bu bölümde, bir uyarı aldığınızda sık karşılaşılan sorunlar nasıl araştırılabildiğiniz açıklanmaktadır. Aşağıdaki adımlar, tanılama günlükleriniz için Azure Monitor günlükleri ayarladığınızı varsayar.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. IoT hub'ınıza göz atın.

1. **Günlükleri**seçin.

1. IoT Hub için bağlantı hata günlüklerini yalıtmak için aşağıdaki sorguyu girin ve ardından **Çalıştır'ı**seçin:

    ```kusto
    AzureDiagnostics
    | where ( ResourceType == "IOTHUBS" and Category == "Connections" and Level == "Error")
    ```

1. Sonuç varsa, hata `OperationName`hakkında `ResultType` daha fazla `ResultDescription` ayrıntı almak için , (hata kodu) ve (hata iletisi) arayın.

   ![Hata günlüğü örneği](./media/iot-hub-troubleshoot-connectivity/diag-logs.png)

1. En yaygın hatalar için sorun giderme kılavuzlarını izleyin:

    - **[404104 DeviceConnectionClosedRemotely](iot-hub-troubleshoot-error-404104-deviceconnectionclosedremotely.md)**
    - **[401003 IoTHubUnauthorized](iot-hub-troubleshoot-error-401003-iothubunauthorized.md)**
    - **[409002 LinkCreationConflict](iot-hub-troubleshoot-error-409002-linkcreationconflict.md)**
    - **[500001 ServerError](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**
    - **[500008 Genel Zaman Çıkış](iot-hub-troubleshoot-error-500xxx-internal-errors.md)**

## <a name="i-tried-the-steps-but-they-didnt-work"></a>Adımları denedim ama işe yaramadı.

Önceki adımlar yardımcı olmadıysa, aşağıdakileri deneyin:

* Sorunlu aygıtlara fiziksel veya uzaktan (SSH gibi) erişiminiz varsa, sorun gidermeye devam etmek için [aygıt tarafındaki sorun giderme kılavuzunu](https://github.com/Azure/azure-iot-sdk-node/wiki/Troubleshooting-Guide-Devices) izleyin.

* IoT hub'ınız > > IoT aygıtlarınızda aygıtlarınızın Azure portalında **Etkinleştirildiğinden.**

* Aygıtınız MQTT protokolü kullanıyorsa, 8883 bağlantı noktasının açık olduğundan doğrulayın. Daha fazla bilgi için Bkz. [IoT Hub'ına Bağlanma (MQTT).](iot-hub-mqtt-support.md#connecting-to-iot-hub)

* [Azure IoT Hub forumu,](https://social.msdn.microsoft.com/Forums/azure/home?forum=azureiothub) [Yığın Taşma](https://stackoverflow.com/questions/tagged/azure-iot-hub)veya [Azure desteğinden](https://azure.microsoft.com/support/options/)yardım alın.

Herkes için belgeleri geliştirmeye yardımcı olmak için, bu kılavuz size yardımcı olmadıysa aşağıdaki geri bildirim bölümüne bir yorum bırakın.

## <a name="next-steps"></a>Sonraki adımlar

* Geçici sorunları çözme hakkında daha fazla bilgi edinmek için Geçici [hata işleme](/azure/architecture/best-practices/transient-faults)bölümüne bakın.

* Azure IoT SDK ve yeniden denemeyönetme hakkında daha fazla bilgi edinmek için [Azure IoT Hub aygıt SDK'larını kullanarak bağlantı ve güvenilir iletileri nasıl yönetebilirsiniz'e](iot-hub-reliability-features-in-sdks.md#connection-and-retry)bakın.
