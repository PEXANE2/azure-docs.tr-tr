---
title: Uzaktan Izleme çözümü içeri aktarma paketi-Azure | Microsoft Docs
description: Bu makalede otomatik bir cihaz yönetim paketinin uzaktan Izleme çözüm hızlandırıcısına nasıl aktarılacağı açıklanır
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 8100914e9a1d1489cb80de55a689e17f6d28a941
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "61443454"
---
# <a name="import-an-automatic-device-management-package-into-your-remote-monitoring-solution-accelerator"></a>Otomatik cihaz yönetim paketini uzaktan Izleme çözüm hızlandırıcısına aktarma

Otomatik Cihaz Yönetimi yapılandırması, bir cihaz grubuna dağıtılacak yapılandırma değişikliklerini tanımlar. Bu makalede, kuruluşunuzdaki bir geliştiricinin zaten bir otomatik cihaz yönetimi yapılandırması oluşturduğunu varsaymaktadır. Bir geliştiricinin yapılandırma oluşturma hakkında bilgi edinmek için aşağıdaki IoT Hub nasıl yapılır makalelerinden birine bakın:

- [Azure portal kullanarak IoT cihazlarını ölçeklendirerek yapılandırma ve izleme](../iot-hub/iot-hub-auto-device-config.md)
- [Azure CLı kullanarak IoT cihazlarını ölçeklendirerek yapılandırma ve izleme](../iot-hub/iot-hub-auto-device-config-cli.md)

Geliştirici bir geliştirme ortamında otomatik cihaz yönetimi yapılandırması oluşturur ve test eder. Hazırsanız, yapılandırmayı uzaktan Izleme çözüm hızlandırıcısına aktarabilirsiniz.

## <a name="export-a-configuration"></a>Yapılandırmayı dışarı aktar

Otomatik cihaz yönetimi yapılandırmasını geliştirme ortamınızdan dışarı aktarmak için Azure portal kullanın:

1. Azure portal IoT cihazlarınızı geliştirmek ve test etmek için kullandığınız IoT Hub 'ına gidin. **IoT cihaz yapılandırması**' na tıklayın:

    [![IoT cihaz yapılandırması](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-expanded.png#lightbox)

1. Kullanmak istediğiniz yapılandırmaya tıklayın. **Cihaz yapılandırma ayrıntıları** sayfası şunu görüntüler:

    [![IoT cihaz yapılandırması ayrıntısı](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-expanded.png#lightbox)
1. **Yapılandırma dosyasını indir**' e tıklayın:

    [![Yapılandırma dosyasını indir](./media/iot-accelerators-remote-monitoring-import-adm-package/download-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/download-expanded.png#lightbox)

1. JSON dosyasını **configuration.js**adlı yerel bir dosya olarak kaydedin.

Artık otomatik cihaz yönetimi yapılandırmasını içeren bir dosyanız vardır. Sonraki bölümde, bu yapılandırmayı uzaktan Izleme çözümüne bir paket olarak içeri aktarırsınız.

## <a name="import-a-package"></a>Bir paketi içeri aktar

Otomatik cihaz yönetimi yapılandırmasını çözümünüze bir paket olarak içeri aktarmak için aşağıdaki adımları izleyin:

1. Uzaktan Izleme Web Kullanıcı arabirimindeki **paketler** sayfasına gidin: ![ paketler sayfası](media/iot-accelerators-remote-monitoring-import-adm-package/packagepage.png)

1. **+ Yeni paket**' e tıklayın, paket türü olarak **yapılandırma** ' yı seçin ve önceki bölümde kaydettiğiniz dosya **configuration.js** seçmek için, **Araştır** ' a tıklayın:

    ![Yapılandırma Seç](media/iot-accelerators-remote-monitoring-import-adm-package/uploadpackage.png)

1. Paketi uzaktan Izleme çözümünüze eklemek için **karşıya yükle** ' ye tıklayın:

    ![Paket karşıya yüklendi](media/iot-accelerators-remote-monitoring-import-adm-package/uploadedpackage.png)

Artık paket olarak bir otomatik cihaz yönetimi yapılandırması yüklediniz. **Dağıtımlar** sayfasında, bu paketi bağlı cihazlarınıza dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bir yapılandırma paketi oluşturmayı ve uzaktan Izleme çözümünden içine aktarmayı öğrendiğinize göre, bir sonraki adım, [uzaktan izlemeye bağlı cihazların toplu olarak nasıl yönetileceğini](iot-accelerators-remote-monitoring-bulk-configuration-update.md)öğrendiğinize bağlıdır.
