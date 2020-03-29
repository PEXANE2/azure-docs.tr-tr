---
title: Uzaktan İzleme çözümü alma Edge paketi - Azure | Microsoft Dokümanlar
description: Bu makalede, Uzaktan İzleme çözüm hızlandırıcısına bir IoT Edge paketinin nasıl aktarılacak
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/10/2018
ms.topic: conceptual
ms.openlocfilehash: 34222f396ed3c43932371aa9f64a459bb2a5dd0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61443020"
---
# <a name="import-an-iot-edge-package-into-your-remote-monitoring-solution-accelerator"></a>Uzaktan İzleme çözüm hızlandırıcınıza bir IoT Edge paketi alma

Dağıtım bildirimi, bir IoT Edge aygıtına dağıtılan modülleri tanımlar. Bu makalede, kuruluşunuzdaki bir geliştiricinin dağıtım bildirimini zaten oluşturduğunu varsayar. Bir geliştiricinin nasıl bir bildirim oluşturduğunu öğrenmek için aşağıdaki IoT Edge nasıl yapılsın makalelerine bakın:

- [Azure portalından Azure IoT Edge modüllerini dağıtma](../iot-edge/how-to-deploy-modules-portal.md)
- [Azure CLI ile Azure IoT Edge modüllerini dağıtma](../iot-edge/how-to-deploy-modules-cli.md)
- [Azure IoT Edge modüllerini Visual Studio Kodundan dağıtma](../iot-edge/how-to-deploy-modules-vscode.md)

Geliştirici, geliştirme ortamında bir dağıtım bildirimi oluşturur ve sınar. Hazır olduğunuzda, bildirimi Uzaktan İzleme çözüm hızlandırıcınıza aktarabilirsiniz.

## <a name="export-a-manifest"></a>Bir manifesto verme

Geliştirme ortamınızdan dağıtım bildirimini dışa aktarmak için Azure portalını kullanın:

1. Azure portalında, IoT Edge aygıtlarınızı geliştirmek ve test etmek için kullandığınız IoT hub'ına gidin. **IoT Edge'i** ve ardından **IoT Edge dağıtımlarını**tıklatın: ![IoT Edge](media/iot-accelerators-remote-monitoring-import-edge-package/iotedge.png)

1. Kullanmak istediğiniz dağıtım yapılandırmasına sahip dağıtımı tıklatın. **Dağıtım Ayrıntıları** sayfası ![görüntüler: IoT Edge dağıtım ayrıntıları](media/iot-accelerators-remote-monitoring-import-edge-package/deploymentdetails.png)

1. **IoT Edge manifestosu İndir'e**tıklayın : ![Dağıtım bildirimini indirin](media/iot-accelerators-remote-monitoring-import-edge-package/download.png)

1. JSON dosyasını **deploymentmanifest.json**adlı yerel bir dosya olarak kaydedin.

Şimdi dağıtım bildirimini içeren bir dosyanız var. Sonraki bölümde, bu bildirimi paket olarak Uzaktan İzleme çözümüne aktarın.

## <a name="import-a-package"></a>Paket alma

Bir Kenar dağıtım bildirimini paketi olarak çözüme almak için aşağıdaki adımları izleyin:

1. Uzaktan İzleme **Packages** web Kullanıcı Arama ![Hizmeti:Paketler sayfasında paketler sayfasına gidin](media/iot-accelerators-remote-monitoring-import-edge-package/packagespage.png)

1. **+ Yeni Paket'i**tıklatın, paket türü olarak **Kenar Bildirimi'ni** seçin ve önceki bölümde kaydettiğiniz ![ **deploymentmanifest.json** dosyasını seçmek için **Gözat'ı** tıklatın: Manifesto'yu seçin](media/iot-accelerators-remote-monitoring-import-edge-package/selectmanifest.png)

1. Paketi Uzaktan İzleme çözümünüze eklemek için ![ **Yükle'yi** tıklatın: Yüklenen paket](media/iot-accelerators-remote-monitoring-import-edge-package/uploadedpackage.png)

Şimdi paket olarak bir IoT Edge dağıtım bildirimi yükledin. **Dağıtımlar** sayfasında, bu paketi bağlı IoT Edge aygıtlarınıza dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Uzaktan İzleme çözümünden modülleri Bir IoT Edge cihazına dağıtmayı öğrendiğiniz için, bir sonraki adım [IoT Edge](../iot-edge/about-iot-edge.md)hakkında daha fazla bilgi edinmektir.
