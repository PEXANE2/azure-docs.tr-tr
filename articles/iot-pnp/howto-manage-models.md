---
title: Depodaki IoT Tak ve Kullan önizleme modellerini yönetme | Microsoft Docs '
description: IoT portalı, Azure CLı ve Visual Studio Code için Azure sertifikası 'nı kullanarak depodaki cihaz yeteneği modellerini yönetme.
author: YasinMSFT
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: conceptual
ms.date: 06/12/2019
ms.author: yahajiza
ms.openlocfilehash: ec36bef9c2b65a5c88a52621bbf3a1db708c235c
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69880507"
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
- IoT Tak ve Kullan cihaz yetenek modellerini bulun. Bu modelleri kullanarak IoT 'ye yönelik [kullanıma yönelik cihazları hızla oluşturabilir ve bunları çözümlerle tümleştirebilirsiniz](./quickstart-connect-pnp-device-solution.md).

## <a name="azure-cli"></a>Azure CLI

Azure CLı, IoT Tak ve Kullan genel ve şirket modeli depolarında cihaz yeteneği modellerini ve arabirimlerini yönetmeye yönelik komutlar sağlar.

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code ' de **model deposu** görünümünü açmak için.

1. Visual Studio Code açın, **CTRL + SHIFT + P**tuşlarını kullanın, yazın ve **IoT Tak ve Kullan seçin: Model deposunu**açın.

1. **Ortak model deposunu açmayı** veya **kuruluş modeli deposunu açmayı**seçebilirsiniz. Şirket modeli deposu için model deposu Bağlantı dizenizi girmeniz gerekir. Bu bağlantı dizesini, **Şirket deponuzdaki** **bağlantı dizeleri** sekmesinde [IoT için Azure sertifikası](https://preview.catalog.azureiotsolutions.com) 'nda bulabilirsiniz.

1. Yeni bir sekme, **model deposu** görünümünü açar.

    Cihaz yetenek modellerini ve arabirimlerini eklemek, indirmek ve silmek için bu görünümü kullanın. Listedeki belirli öğeleri bulmak için bir filtre kullanabilirsiniz.

1. Şirket modeli deponuz ve ortak model deposu arasında geçiş yapmak için **CTRL + SHIFT + P**kullanın, yazın ve IoT Tak ve kullan **seçin: Model deposunu**kapatın. Daha sonra **IoT Tak ve kullan kullanın: Model deposu** komutunu yeniden açın.

> [!NOTE]
> VS Code, ortak model deposu salt okunurdur. Microsoft Iş ortakları, [Azure Sertifikalı IoT portalında](https://preview.catalog.azureiotsolutions.com)ortak depoyu güncelleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen sonraki adım, [sertifika Için ıot Tak ve kullan cihazının](tutorial-certification-test.md)nasıl gönderileceğini öğrenirsiniz.
