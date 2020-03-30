---
title: Depodaki IoT Tak ve Çalıştır Önizleme modellerini yönetin| Microsoft Dokümanlar'
description: IoT portalı, Azure CLI ve Visual Studio kodunu kullanarak depodaki aygıt yeteneği modellerini yönetme.
author: Philmea
manager: philmea
ms.service: iot-pnp
services: iot-pnp
ms.topic: how-to
ms.date: 12/26/2019
ms.author: philmea
ms.openlocfilehash: 78406175090521af70381f61f4e33dfb6c35ac8c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159158"
---
# <a name="manage-models-in-the-repository"></a>Depodaki modelleri yönetme

IoT Tak ve Çalıştır Önizleme modeli deposu aygıt özelliği modellerini ve arayüzlerini saklar. Depo, modelleri ve arayüzleri çözüm geliştiricileri tarafından keşfedilebilir ve sarf edilebilir hale getirir.

Depoyu yönetmek için kullanabileceğiniz üç araç vardır:

- IoT portalı için Azure Sertifikalı
- Azure CLI
- Visual Studio Code

## <a name="model-repositories"></a>Model depoları

Aygıt özelliği modellerini ve arabirimlerini depolamak için iki tür model deposu vardır:

- Aygıt özelliği _modellerini_ ve arabirimlerini [IoT aygıt kataloğunda Bulunan Azure Sertifikalı'da](https://aka.ms/iotdevcat)depolayan tek bir Genel depo vardır. Bu depo aynı zamanda Microsoft Partners tarafından yayınlanan [ortak arabirimleri](./concepts-common-interfaces.md) ve [DCM'leri ve arabirimleri](./howto-onboard-portal.md)de saklar. Bir aygıtı nasıl onayleyeceğinizi ve aygıt yetenek modelini genel depoya nasıl ekleyeceğinizi öğrenmek için, öğretici ye bakın [IoT Tak ve Çalıştır aygıtınızı sertifikalayın.](./tutorial-certification-test.md)
- Birden fazla _Şirket deposu_vardır. IoT portalı için [Azure Sertifikalı portalına bindiğinizde](./howto-onboard-portal.md)kuruluşunuz için otomatik olarak bir şirket deposu oluşturulur. Geliştirme ve test sırasında cihaz yetenek modellerinizi ve arayüzlerinizi depolamak için şirket deponuzu kullanabilirsiniz.

## <a name="azure-certified-for-iot-portal"></a>IoT portalı için Azure Sertifikalı

Azure [Sertifikalı IoT portalında](https://preview.catalog.azureiotsolutions.com)aşağıdaki görevleri tamamlayabilirsiniz:

- [IoT cihazınız için sertifika işlemini tamamlayın.](./tutorial-certification-test.md)
- IoT Tak ve Çalıştır aygıtı özelliği modellerini bulun. Bu modelleri Hızlı [bir şekilde IoT hazır cihazlar oluşturmak ve çözümler ile entegre](./quickstart-connect-pnp-device-solution-node.md)etmek için kullanabilirsiniz.

## <a name="azure-cli"></a>Azure CLI

Azure CLI, IoT Tak ve Ortak ve şirket model depolarında aygıt yeteneği modellerini ve arabirimlerini yönetmek için komutlar sağlar. Daha fazla bilgi için Azure CLI nasıl yapılsın kılavuzuna bakın [ve Azure CLI için Azure IoT uzantısını kullanın.](./howto-install-pnp-cli.md)

## <a name="visual-studio-code"></a>Visual Studio Code

Visual Studio Code'da **Model Deposu** görünümünü açmak için.

1. Görsel Stüdyo Kodunu Açın, **Ctrl+Shift+P**kullanın, **IoT Tak ve Çalıştır'ı yazın ve seçin: Model Deposu'nu açın.**

1. Ortak Model **Deposu'nu** açmayı veya **Kuruluş Modeli Deposu'nu açmayı**seçebilirsiniz. Şirket modeli deposu için model deposu bağlantı dizenizi girmeniz gerekir. Bu bağlantı dizesini, **Şirket deponuz**için Bağlantı **dizeleri** sekmesinde [IoT için Azure Sertifikalı portalında](https://preview.catalog.azureiotsolutions.com) bulabilirsiniz.

1. Yeni bir sekme **Model Deposu** görünümünü açar.

    Aygıt özelliği modelleri ve arabirimleri eklemek, indirmek ve silmek için bu görünümü kullanın. Listedeki belirli öğeleri bulmak için bir filtre kullanabilirsiniz.

1. Şirket model deponuz ile genel model deposu arasında geçiş yapmak için **Ctrl+Shift+P**, Type ve Select **IoT Tak ve Çalıştır'ı kullanın: Model Deposunu kapatın.** Ardından **IoT Tak ve Çalıştır: Model Deposu** komutunu tekrar açın.

> [!NOTE]
> VS Code'da, ortak model deposu salt okunur. Microsoft İş Ortakları, [IoT için Azure Sertifikalı portalındaki](https://preview.catalog.azureiotsolutions.com)genel depoyu güncelleştirebilir.

## <a name="next-steps"></a>Sonraki adımlar

Önerilen bir sonraki adım, sertifika için [bir IoT Tak ve Çalıştır cihazını](tutorial-certification-test.md)nasıl göndereceğini öğrenmektir.
