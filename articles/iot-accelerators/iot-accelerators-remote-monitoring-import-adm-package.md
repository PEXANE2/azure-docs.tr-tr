---
title: Uzaktan İzleme çözümü alma paketi - Azure | Microsoft Dokümanlar
description: Bu makalede, otomatik bir cihaz yönetim paketinin Uzaktan İzleme çözüm hızlandırıcısına nasıl aktarılacak
author: dominicbetts
manager: philmea
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 11/29/2018
ms.topic: conceptual
ms.openlocfilehash: 8100914e9a1d1489cb80de55a689e17f6d28a941
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "61443454"
---
# <a name="import-an-automatic-device-management-package-into-your-remote-monitoring-solution-accelerator"></a>Uzaktan İzleme çözüm hızlandırıcınıza otomatik cihaz yönetim paketi alma

Otomatik aygıt yönetimi yapılandırması, bir aygıt grubuna dağıtılan yapılandırma değişikliklerini tanımlar. Bu makalede, kuruluşunuzdaki bir geliştiricinin otomatik aygıt yönetimi yapılandırması oluşturduğunu varsayar. Bir geliştiricinin yapılandırmayı nasıl oluşturduğu hakkında bilgi edinmek için aşağıdaki IoT Hub nasıl yapılsA makalelerine bakın:

- [Azure portalını kullanarak IoT aygıtlarını ölçekte yapılandırma ve izleme](../iot-hub/iot-hub-auto-device-config.md)
- [Azure CLI'yi kullanarak IoT aygıtlarını ölçekte yapılandırma ve izleme](../iot-hub/iot-hub-auto-device-config-cli.md)

Geliştirici, geliştirme ortamında otomatik aygıt yönetimi yapılandırması oluşturur ve sınar. Hazır olduğunuzda, yapılandırmayı Uzaktan İzleme çözüm hızlandırıcınıza aktarabilirsiniz.

## <a name="export-a-configuration"></a>Yapılandırmayı dışa aktarma

Geliştirme ortamınızdan otomatik aygıt yönetimi yapılandırmasını dışa aktarmak için Azure portalını kullanın:

1. Azure portalında, IoT aygıtlarınızı geliştirmek ve test etmek için kullandığınız IoT hub'ına gidin. **IoT cihaz yapılandırması**tıklayın:

    [![IoT cihaz yapılandırması](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/deviceconfiguration-expanded.png#lightbox)

1. Kullanmak istediğiniz yapılandırmayı tıklatın. **Cihaz Yapılandırma Ayrıntıları** sayfası görüntüler:

    [![IoT cihaz yapılandırma detayı](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/configuration-details-expanded.png#lightbox)
1. **Yapılandırma dosyasını İndir'e**tıklayın :

    [![Yapılandırma dosyasını karşıdan yükleme](./media/iot-accelerators-remote-monitoring-import-adm-package/download-inline.png)](./media/iot-accelerators-remote-monitoring-import-adm-package/download-expanded.png#lightbox)

1. JSON dosyasını **configuration.json**adlı yerel bir dosya olarak kaydedin.

Artık otomatik aygıt yönetimi yapılandırmasını içeren bir dosyanız var. Sonraki bölümde, bu yapılandırmayı paket olarak Uzaktan İzleme çözümüne aktarın.

## <a name="import-a-package"></a>Paket alma

Otomatik aygıt yönetimi yapılandırmasını paket olarak çözüme almak için aşağıdaki adımları izleyin:

1. Uzaktan İzleme **Packages** web Kullanıcı Arama ![Hizmeti:Paketler sayfasında paketler sayfasına gidin](media/iot-accelerators-remote-monitoring-import-adm-package/packagepage.png)

1. **+ Yeni Paket'i**tıklatın, paket türü olarak **Yapılandırma'yı** seçin ve önceki bölümde kaydettiğiniz **configuration.json** dosyasını seçmek için **Gözat'ı** tıklatın:

    ![Yapılandırmayı seçin](media/iot-accelerators-remote-monitoring-import-adm-package/uploadpackage.png)

1. Paketi Uzaktan İzleme çözümünüze eklemek için **Yükle'yi** tıklatın:

    ![Yüklenen paket](media/iot-accelerators-remote-monitoring-import-adm-package/uploadedpackage.png)

Şimdi paket olarak otomatik aygıt yönetimi yapılandırması yükledin. **Dağıtımlar** sayfasında, bu paketi bağlı aygıtlarınıza dağıtabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bir yapılandırma paketi oluşturmayı ve uzaktan izleme çözümünden nasıl içe aktarabileceğinizi öğrendiğiniz için, bir sonraki adım, [Uzaktan İzleme'ye bağlı aygıtları toplu olarak](iot-accelerators-remote-monitoring-bulk-configuration-update.md)yönetmeyi öğrenmektir.
