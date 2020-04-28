---
title: include dosyası
description: include dosyası
services: iot-suite
author: dominicbetts
ms.service: iot-suite
ms.topic: include
ms.date: 09/17/2018
ms.author: dobett
ms.custom: include file
ms.openlocfilehash: c79b6f854dc78670a7eb8a1275c3e2fc46fcdd99
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67188806"
---
### <a name="code-walkthrough"></a>Kod kılavuzu

Bu bölümde, örnek kodun bazı önemli bölümleri açıklanmakta ve bunların uzaktan Izleme çözümü hızlandırıcılarıyla ilgisi açıklanmaktadır.

Aşağıdaki kod parçacığında, cihazın yeteneklerini tanımlayan raporlanan özelliklerin nasıl tanımlandığı gösterilmektedir. Bu özellikler şunları içerir:

- Cihazı haritaya eklemek için çözüm hızlandırıcıyı etkinleştirmek üzere cihazın konumu.
- Geçerli bellenim sürümü.
- Cihazın desteklediği yöntemlerin listesi.
- Cihaz tarafından gönderilen telemetri iletilerinin şeması.

[!code-cpp[Define data structures for Chiller](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=datadefinition "Define data structures for Chiller")]

Örnek, Parson kitaplığı kullanılarak bu veri yapısını serileştiren bir **Serializetojson** işlevi içerir.

Örnek, istemci çözüm hızlandırıcıyla etkileşerek konsola bilgi yazdıracak birkaç geri çağırma işlevi içerir:

- **connection_status_callback**
- **send_confirm_callback**
- **reported_state_callback**
- **device_method_callback**

Aşağıdaki kod parçacığında **device_method_callback** işlevi gösterilmektedir. Bu işlev, çözüm hızlandırıcısında bir yöntem çağrısı alındığında gerçekleştirilecek eylemi belirler. İşlevi, **Usercontextcallback** parametresindeki **Chiller** veri yapısına bir başvuru alır. **Usercontextcallback** değeri, **Main** işlevinde geri çağırma işlevi yapılandırıldığında ayarlanır:

[!code-cpp[Device method callback](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=devicemethodcallback "Device method callback")]

Çözüm Hızlandırıcısı bellenim güncelleştirme yöntemini çağırdığında, örnek JSON yükünü serileştirir ve güncelleştirme işlemini tamamlaması için bir arka plan iş parçacığı başlatır. Aşağıdaki kod parçacığında iş parçacığı üzerinde çalışan **do_firmware_update** gösterilmektedir:

[!code-cpp[Firmware update thread](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=firmwareupdate "Firmware update thread")]

Aşağıdaki kod parçacığında, istemcinin çözüm hızlandırıcısına nasıl telemetri iletisi gönderdiği gösterilmektedir. İleti özellikleri, çözüm hızlandırıcısının panoda telemetri görüntülemesine yardımcı olacak ileti şemasını içerir:

[!code-cpp[Send telemetry](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=sendmessage "Send telemetry")]

Örnekteki **Main** işlevi:

- SDK alt sistemini başlatır ve kapatır.
- **Chilveri** yapısını başlatır.
- Bildirilen özellikleri çözüm hızlandırıcısına gönderir.
- Cihaz yöntemi geri çağırma işlevini yapılandırır.
- Çözüm hızlandırıcısına sanal telemetri değerleri gönderir.

[!code-cpp[Main](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=main "Main")]
