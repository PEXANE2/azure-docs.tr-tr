---
title: Dosya yüklemesini yapılandırmak için Azure portalını kullanın | Microsoft Dokümanlar
description: IoT hub'ınızı bağlı aygıtlardan dosya yüklemelerini etkinleştirmek için yapılandırmak için Azure portalını kullanma. Hedef Azure depolama hesabını yapılandırma yla ilgili bilgileri içerir.
author: robinsh
manager: philmea
ms.service: iot-hub
services: iot-hub
ms.topic: conceptual
ms.date: 07/03/2017
ms.author: robinsh
ms.openlocfilehash: bd7cc37b8fc81fc9d4109826743f2243913d0604
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60735052"
---
# <a name="configure-iot-hub-file-uploads-using-the-azure-portal"></a>Azure portalını kullanarak IoT Hub dosya karşıya yükleme işlemlerini yapılandırma

[!INCLUDE [iot-hub-file-upload-selector](../../includes/iot-hub-file-upload-selector.md)]

## <a name="file-upload"></a>Dosya yükleme

[IoT Hub'da dosya yükleme işlevini](iot-hub-devguide-file-upload.md)kullanmak için öncelikle bir Azure Depolama hesabını hub'ınızla ilişkilendirmeniz gerekir. Değiştirilen IoT hub'ı için dosya yükleme özelliklerinin listesini görüntülemek için **Dosya yükleme'yi** seçin.

![Portaldaki IoT Hub dosya yükleme ayarlarını görüntüleyin](./media/iot-hub-configure-file-upload/file-upload-settings.png)

* **Depolama kapsayıcısı**: IoT Hub'ınızla ilişkilendirmek için geçerli Azure aboneliğinizdeki bir Azure Depolama hesabında bir blob kapsayıcısı seçmek için Azure portalını kullanın. Gerekirse, **Kapsayıcılar** bıçağında **Depolama hesapları** bıçak ve blob kapsayıcısı üzerinde bir Azure Depolama hesabı oluşturabilirsiniz. IoT Hub, aygıtların dosya yüklerken kullanmaları için bu blob kapsayıcısına yazma izinleri ile otomatik olarak SAS URI'leri oluşturur.

   ![Portalda dosya yükleme için depolama kaplarını görüntüleme](./media/iot-hub-configure-file-upload/file-upload-container-selection.png)

* **Yüklenen dosyalar için bildirimler alın**: Geçiş yoluyla dosya yükleme bildirimlerini etkinleştirin veya devre dışı edin.

* **SAS TTL**: Bu ayar, IoT Hub tarafından cihaza döndürülen SAS URI'lerinin canlı olarak zaman dilimidir. Varsayılan olarak bir saatolarak ayarlayın, ancak kaydırıcıyı kullanarak diğer değerlere göre özelleştirilebilir.

* **Dosya bildirim ayarları varsayılan TTL**: Süresi dolmadan önce dosya yükleme bildiriminin zaman aşımısüresi. Varsayılan olarak bir güne ayarlayın, ancak kaydırıcıyı kullanarak diğer değerlere göre özelleştirilebilir.

* **Dosya bildirimi maksimum teslimat sayısı**: IoT Hub'ının dosya yükleme bildirimini kaç kez sunmaya çalıştığı. Varsayılan olarak 10 olarak ayarlayın, ancak kaydırıcıyı kullanarak diğer değerlere göre özelleştirilebilir.

   ![Portalda IoT Hub dosya yüklemesini yapılandırma](./media/iot-hub-configure-file-upload/file-upload-selected-container.png)

## <a name="next-steps"></a>Sonraki adımlar

IoT Hub'ın dosya yükleme özellikleri hakkında daha fazla bilgi için, IoT Hub geliştirici kılavuzundaki [bir aygıttan dosya yükleme](iot-hub-devguide-file-upload.md) bölümüne bakın.

Azure IoT Hub'ı yönetme hakkında daha fazla bilgi edinmek için aşağıdaki bağlantıları izleyin:

* [IoT cihazlarını toplu yönetme](iot-hub-bulk-identity-mgmt.md)
* [IoT Hub ölçümleri](iot-hub-metrics.md)
* [İşlemleri izleme](iot-hub-operations-monitoring.md)

IoT Hub'ın yeteneklerini daha fazla keşfetmek için bkz:

* [IoT Hub geliştirici kılavuzu](iot-hub-devguide.md)
* [Azure IOT Edge ile sınır cihazlarına Al dağıtma](../iot-edge/tutorial-simulate-device-linux.md)
* [IoT çözümünüzü sıfırdan emniyete](../iot-fundamentals/iot-security-ground-up.md)
