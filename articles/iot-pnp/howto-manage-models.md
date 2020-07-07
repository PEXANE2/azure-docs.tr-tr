---
title: Depodaki IoT Tak ve Kullan önizleme modellerini yönetme | Microsoft Docs '
description: IoT portalı, Azure CLı ve Visual Studio Code için Azure sertifikası 'nı kullanarak depodaki cihaz yeteneği modellerini yönetme.
author: Philmea
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: how-to
ms.date: 12/26/2019
ms.author: philmea
ms.openlocfilehash: 78406175090521af70381f61f4e33dfb6c35ac8c
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80159158"
---
# <a name="manage-models-in-the-repository"></a>Depodaki modelleri yönetme

IoT Tak ve Kullan önizleme modeli deposu cihaz yeteneği modellerini ve arabirimlerini depolar. Depo, model ve arabirimlerin çözüm geliştiricileri tarafından bulunabilir ve tüketilebilir olmasını sağlar.

Depoyu yönetmek için kullanabileceğiniz üç araç vardır:

- IoT için Azure sertifikası portalı
- Azure CLI
- Visual Studio Code

## <a name="model-repositories"></a>Model depoları

Cihaz yeteneği modellerini ve arabirimlerini depolamak için iki tür model deposu vardır:

- [IoT cihaz kataloğunda Azure Sertifikalı](https://aka.ms/iotdevcat)cihazlar için cihaz yeteneği modellerini ve arabirimlerini depolayan tek bir _genel depo_ vardır. Bu depo Ayrıca [ortak arabirimleri](./concepts-common-interfaces.md) ve [DCMS 'Leri ve Microsoft iş ortakları tarafından yayımlanan arabirimleri](./howto-onboard-portal.md)depolar. Bir cihazın nasıl sertifikalandıralınacağını ve cihaz yetenek modelinin ortak depoya nasıl ekleneceğini öğrenmek için bkz. [ıot Tak ve kullan cihazınızı onaylama](./tutorial-certification-test.md)öğreticisi.
- Çoklu _Şirket depoları_vardır. [IoT portalı Için Azure sertifikası 'nı](./howto-onboard-portal.md)eklediğinizde kuruluşunuz için bir şirket deposu otomatik olarak oluşturulur. Geliştirme ve test sırasında cihaz yetenek modellerinizi ve arabirimlerini depolamak için şirket deponuzu kullanabilirsiniz.

## <a name="azure-certified-for-iot-portal"></a>IoT portalı için Azure sertifikası

[IoT Için Azure sertifikası portalı](https://preview.catalog.azureiotsolutions.com)'nda aşağıdaki görevleri tamamlayabilirsiniz:

- [IoT cihazınız için sertifika sürecini doldurun](./tutorial-certification-test.md).
- IoT Tak ve Kullan cihaz yetenek modellerini bulun. Bu modelleri kullanarak IoT 'ye yönelik [kullanıma yönelik cihazları hızla oluşturabilir ve bunları çözümlerle tümleştirebilirsiniz](./quickstart-connect-pnp-device-solution-node.md).

## <a name="azure-cli"></a>Azure CLI’si

Azure CLı, IoT Tak ve Kullan genel ve şirket modeli depolarında cihaz yeteneği modellerini ve arabirimlerini yönetmeye yönelik komutlar sağlar. Daha fazla bilgi için bkz. [Azure CLI Için Azure IoT uzantısı](./howto-install-pnp-cli.md) nasıl yapılır Kılavuzu.

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code ' de **model deposu** görünümünü açmak için.

1. Visual Studio Code açın, **CTRL + SHIFT + P**tuşlarını kullanın, yazın ve **IoT Tak ve kullan: model deposunu aç**' ı seçin.

1. **Ortak model deposunu açmayı** veya **kuruluş modeli deposunu açmayı**seçebilirsiniz. Şirket modeli deposu için model deposu Bağlantı dizenizi girmeniz gerekir. Bu bağlantı dizesini, **Şirket deponuzdaki** **bağlantı dizeleri** sekmesinde [IoT için Azure sertifikası](https://preview.catalog.azureiotsolutions.com) 'nda bulabilirsiniz.

1. Yeni bir sekme, **model deposu** görünümünü açar.

    Cihaz yetenek modellerini ve arabirimlerini eklemek, indirmek ve silmek için bu görünümü kullanın. Listedeki belirli öğeleri bulmak için bir filtre kullanabilirsiniz.

1. Şirket modeli deponuz ve ortak model deposu arasında geçiş yapmak için **CTRL + SHIFT + P**kullanın, yazın ve **IoT Tak ve kullan: oturumu Kapat model deposunu**seçin. Daha sonra **ıot Tak ve kullan: model deposunu aç** komutunu yeniden kullanın.

> [!NOTE]
> VS Code, ortak model deposu salt okunurdur. Microsoft Iş ortakları, [Azure Sertifikalı IoT portalında](https://preview.catalog.azureiotsolutions.com)ortak depoyu güncelleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen sonraki adım, [sertifika Için ıot Tak ve kullan cihazının nasıl gönderileceğini](tutorial-certification-test.md)öğrenirsiniz.
