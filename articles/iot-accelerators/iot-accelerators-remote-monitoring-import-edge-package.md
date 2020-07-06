---
title: Uzaktan Izleme çözümü içeri aktarma Edge paketi-Azure | Microsoft Docs
description: Bu makalede, bir IoT Edge paketini uzaktan Izleme çözüm hızlandırıcısına aktarma işlemi açıklanır
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/10/2018
ms.topic: conceptual
ms.openlocfilehash: 34222f396ed3c43932371aa9f64a459bb2a5dd0e
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "61443020"
---
# <a name="import-an-iot-edge-package-into-your-remote-monitoring-solution-accelerator"></a>Uzaktan Izleme çözüm hızlandırıcısına IoT Edge paketini içeri aktarın

Dağıtım bildirimi, IoT Edge cihaza dağıtılacak modülleri tanımlar. Bu makalede, kuruluşunuzdaki bir geliştiricinin zaten dağıtım bildirimi oluşturduğu varsayılmaktadır. Bir geliştiricinin bildirim oluşturma hakkında bilgi edinmek için aşağıdaki IoT Edge nasıl yapılır makalelerinden birine bakın:

- [Azure portal Azure IoT Edge modüllerini dağıtma](../iot-edge/how-to-deploy-modules-portal.md)
- [Azure CLı ile Azure IoT Edge modülleri dağıtma](../iot-edge/how-to-deploy-modules-cli.md)
- [Visual Studio Code Azure IoT Edge modüllerini dağıtma](../iot-edge/how-to-deploy-modules-vscode.md)

Geliştirici bir geliştirme ortamında dağıtım bildirimi oluşturur ve test eder. Hazırsanız, bildirimi uzaktan Izleme çözüm hızlandırıcısına aktarabilirsiniz.

## <a name="export-a-manifest"></a>Bildirimi dışarı aktarma

Dağıtım bildirimini geliştirme ortamınızdan dışarı aktarmak için Azure portal kullanın:

1. Azure portal, IoT Edge cihazlarınızı geliştirmek ve test etmek için kullandığınız IoT Hub 'ına gidin. **IoT Edge** ' ye tıklayın ve ardından **dağıtımlar IoT Edge**: ![ IoT Edge](media/iot-accelerators-remote-monitoring-import-edge-package/iotedge.png)

1. Kullanmak istediğiniz dağıtım yapılandırmasını içeren dağıtıma tıklayın. **Dağıtım ayrıntıları** sayfası şunu görüntüler: ![ IoT Edge dağıtım ayrıntıları](media/iot-accelerators-remote-monitoring-import-edge-package/deploymentdetails.png)

1. **IoT Edge bildirim yükle**' ye tıklayın: ![ dağıtım bildirimini indir](media/iot-accelerators-remote-monitoring-import-edge-package/download.png)

1. JSON dosyasını **deploymentmanifest.js**adlı yerel bir dosya olarak kaydedin.

Artık dağıtım bildirimini içeren bir dosyanız vardır. Sonraki bölümde, bu bildirimi uzak Izleme çözümüne bir paket olarak içeri aktarırsınız.

## <a name="import-a-package"></a>Bir paketi içeri aktar

Bir uç dağıtım bildirimini çözümünüze paket olarak içeri aktarmak için aşağıdaki adımları izleyin:

1. Uzaktan Izleme Web Kullanıcı arabirimindeki **paketler** sayfasına gidin: ![ paketler sayfası](media/iot-accelerators-remote-monitoring-import-edge-package/packagespage.png)

1. **+ Yeni paket**' e tıklayın, paket türü olarak **Edge bildirimi** ' ni seçin ve önceki bölümde kaydettiğiniz dosya **deploymentmanifest.js** seçmek için, **görüntüle** ' ye tıklayın: ![ bildirim Seç](media/iot-accelerators-remote-monitoring-import-edge-package/selectmanifest.png)

1. Paketi uzaktan Izleme çözümünüze eklemek için **karşıya yükle** ' ye tıklayın: ![ paket yüklendi](media/iot-accelerators-remote-monitoring-import-edge-package/uploadedpackage.png)

Artık bir IoT Edge dağıtım bildirimini paket olarak yüklediniz. **Dağıtımlar** sayfasında, bu paketi bağlı IoT Edge cihazlarınıza dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık modülleri uzaktan Izleme çözümünün bir IoT Edge cihazına dağıtmayı öğrendiğinize göre, bir sonraki adım [IoT Edge](../iot-edge/about-iot-edge.md)hakkında daha fazla bilgi edineceksiniz.
