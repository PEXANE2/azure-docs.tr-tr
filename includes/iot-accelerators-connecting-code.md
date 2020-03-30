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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "67188806"
---
### <a name="code-walkthrough"></a>Kod kılavuzu

Bu bölümde örnek kodun bazı önemli bölümleri açıklanır ve uzaktan izleme çözüm hızlandırıcısı ile nasıl ilişkili olduklarını açıklar.

Aşağıdaki snippet, aygıtın yeteneklerini açıklayan bildirilen özelliklerin nasıl tanımlandığını gösterir. Bu özellikler şunlardır:

- Çözüm hızlandırıcısının cihazı eşe eklemesini sağlamak için aygıtın konumu.
- Geçerli firmware sürümü.
- Aygıtın desteklediği yöntemlerin listesi.
- Cihaz tarafından gönderilen telemetri iletilerinin şeması.

[!code-cpp[Define data structures for Chiller](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=datadefinition "Define data structures for Chiller")]

Örnek, Parson kitaplığını kullanarak bu veri yapısını seri hale getiren bir **serializeToJson** işlevi içerir.

Örnek, istemci çözüm hızlandırıcısıyla etkileşimde bulunduğunda konsola bilgi yazdıran birkaç geri arama işlevi içerir:

- **connection_status_callback**
- **send_confirm_callback**
- **reported_state_callback**
- **device_method_callback**

Aşağıdaki parçacık **device_method_callback** işlevini gösterir. Bu işlev, çözüm hızlandırıcıdan bir yöntem çağrısı aldığında yapılacak eylemi belirler. **İşlev, userContextCallback** parametresinde **Chiller** veri yapısına bir başvuru alır. Geri arama işlevi **ana** işlevde yapılandırıldığında **userContextCallback'in** değeri ayarlanır:

[!code-cpp[Device method callback](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=devicemethodcallback "Device method callback")]

Çözüm hızlandırıcı firmware güncelleştirme yöntemini aradığında, örnek JSON yükünü deserialize eder ve güncelleştirme işlemini tamamlamak için bir arka plan iş parçacığı başlatır. Aşağıdaki parçacık iş parçacığı üzerinde çalışan **do_firmware_update** gösterir:

[!code-cpp[Firmware update thread](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=firmwareupdate "Firmware update thread")]

Aşağıdaki parçacık, istemcinin çözüm hızlandırıcısına nasıl bir telemetri iletisi gönderdiğini gösterir. İleti özellikleri, çözüm hızlandırıcısının gösterge panelinde telemetriyi görüntülemesine yardımcı olacak ileti şemasını içerir:

[!code-cpp[Send telemetry](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=sendmessage "Send telemetry")]

Örnekteki **ana** işlev:

- SDK alt sistemini başlayıp kapatır.
- **Chiller** veri yapısını başharfe aktarıyor.
- Bildirilen özellikleri çözüm hızlandırıcısına gönderir.
- Aygıt yöntemi geri arama işlevini yapılandırır.
- Çözüm hızlandırıcısına simüle telemetri değerleri gönderir.

[!code-cpp[Main](~/iot-samples-c/samples/solutions/remote_monitoring_client/remote_monitoring.c?name=main "Main")]
