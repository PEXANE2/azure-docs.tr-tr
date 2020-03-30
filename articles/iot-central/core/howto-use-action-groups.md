---
title: Azure IoT Merkezi kuralından birden çok eylem çalıştırma | Microsoft Dokümanlar
description: Tek bir IoT Merkezi kuralından birden çok eylem çalıştırın ve birden çok kuraldan çalıştırabileceğiniz yeniden kullanılabilir eylem grupları oluşturun.
services: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 12/06/2019
ms.topic: how-to
ms.service: iot-central
manager: philmea
ms.openlocfilehash: b447f44d0c95693e560fd5bbfbff8c8daeec964e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157696"
---
# <a name="group-multiple-actions-to-run-from-one-or-more-rules"></a>Bir veya daha fazla kuraldan çalıştırmak için birden çok eylemi gruplandırma

*Bu makale, inşaatçılar ve yöneticiler için geçerlidir.*

Azure IoT Central'da, bir koşul karşılandığında eylemleri çalıştırmak için kurallar oluşturursunuz. Kurallar, aygıt telemetrisini veya olayları temel alınmaktadır. Örneğin, bir aygıtın sıcaklığı bir eşiği aştığında işleç tarafından bilgilendirebilirsiniz. Bu makalede, Bir IoT Merkezi kuralına birden çok eylem eklemek için [Azure Monitor](../../azure-monitor/overview.md) *eylem gruplarının* nasıl kullanılacağı açıklanmaktadır. Bir eylem grubunu birden çok kurala ekleyebilirsiniz. [Eylem grubu,](../../azure-monitor/platform/action-groups.md) Azure aboneliğinin sahibi tarafından tanımlanan bildirim tercihleri topluluğudur.

## <a name="prerequisites"></a>Ön koşullar

- Standart bir fiyatlandırma planı kullanılarak oluşturulan bir uygulama
- Azure Monitöreylem grupları oluşturmak ve yönetmek için bir Azure hesabı ve aboneliği

## <a name="create-action-groups"></a>Eylem grubu oluşturma

Azure [portalında](../../azure-monitor/platform/action-groups.md) veya Azure Kaynak Yöneticisi [şablonuyla](../../azure-monitor/platform/action-groups-create-resource-manager-template.md)eylem grupları oluşturabilir ve yönetebilirsiniz.

Bir eylem grubu şunları yapabilir:

- E-posta, SMS gibi bildirimler gönderin veya sesli arama yapın.
- Webhook arama gibi bir eylem çalıştırın.

Aşağıdaki ekran görüntüsü, e-posta ve SMS bildirimleri gönderen ve webhook çağıran bir eylem grubunu gösterir:

![Eylem grubu](media/howto-use-action-groups/actiongroup.png)

Bir eylem grubunu Bir IoT Merkezi kuralında kullanmak için eylem grubunun IoT Merkezi uygulamasıyla aynı Azure aboneliğinde olması gerekir.

## <a name="use-an-action-group"></a>Eylem grubu kullanma

IoT Merkezi uygulamanızda bir eylem grubu kullanmak için önce bir kural oluşturun. Kurala bir eylem eklediğinizde, **Azure MonitörEylem Grupları'nı**seçin:

![Eylem seçin](media/howto-use-action-groups/chooseaction.png)

Azure aboneliğinizden bir eylem grubu seçin:

![Eylem grubunu seçin](media/howto-use-action-groups/chooseactiongroup.png)

**Kaydet'i**seçin. Eylem grubu şimdi kural tetiklendiğinde çalıştırılan eylemler listesinde görünür:

![Kaydedilen eylem grubu](media/howto-use-action-groups/savedactiongroup.png)

Aşağıdaki tablo, desteklenen eylem türlerine gönderilen bilgileri özetler:

| Eylem türü | Çıktı biçimi |
| ----------- | -------------- |
| Email       | Standart IoT Merkezi e-posta şablonu |
| SMS         | Azure IoT Merkezi uyarısı: ${applicationName} - "${ruleName}" ${triggerDate} ${triggerTime} adresiyle "${deviceName}" üzerinde tetiklenir |
| Ses       | Azure I.O.T Merkezi uyarısı: ${triggerDate} ${triggerTime}, "${deviceName}" cihazında tetiklenen "${ruleName}" kuralı ${applicationName} |
| Web Kancası     | { "schemaId" : "AzureIoTCentralRuleWebhook", "veri": {[normal webhook yükü](howto-create-webhooks.md#payload)}} |

Aşağıdaki metin, bir eylem grubundan örnek bir SMS mesajıdır:

`iotcentral: Azure IoT Central alert: Contoso - "Low pressure alert" triggered on "Motion sensor 2" at March 20, 2019 10:12 UTC`

## <a name="next-steps"></a>Sonraki adımlar

Artık eylem gruplarını kurallarla kullanmayı öğrendiğinize göre, önerilen bir sonraki adım cihazlarınızı nasıl [yöneteceklerinizi](howto-manage-devices.md)öğrenmektir.
