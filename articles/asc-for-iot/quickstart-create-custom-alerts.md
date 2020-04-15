---
title: 'Quickstart: Özel uyarılar oluşturma'
description: IoT güvenlik hizmeti için Azure Güvenlik Merkezi için özel aygıt uyarılarını anlayın, oluşturun ve atayın.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: d1757868-da3d-4453-803a-7e3a309c8ce8
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 1/27/2020
ms.author: mlottner
ms.openlocfilehash: 8ba81b669c25d91efa8ad1f07b1a7835b42c5cf4
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81310842"
---
# <a name="quickstart-create-custom-alerts"></a>Quickstart: Özel uyarılar oluşturma

Özel güvenlik gruplarını ve uyarıları kullanarak, IoT çözümünüzde daha iyi güvenlik sağlamak için uçtan uca güvenlik bilgilerinden ve kategorik aygıt bilgilerinden tam olarak yararlanır.

## <a name="why-use-custom-alerts"></a>Neden özel uyarılar kullanıyorsun?

IoT cihazlarınızı en iyi siz biliyorsunuz.

Beklenen aygıt davranışLarını tam olarak anlayan müşteriler için, Azure Güvenlik Merkezi IoT için bu anlayışı bir aygıt davranış ilkesine dönüştürmenize ve beklenen normal davranıştan sapma konusunda uyarıda beklemenize olanak tanır.

## <a name="security-groups"></a>Güvenlik grupları

Güvenlik grupları, mantıksal aygıt gruplarını tanımlamanızı ve güvenlik durumlarını merkezi bir şekilde yönetmenize olanak tanır.

Bu gruplar, belirli donanıma sahip aygıtları, belirli bir konumda dağıtılan aygıtları veya özel gereksinimlerinize uygun başka bir grubu temsil edebilir.

Güvenlik grupları **SecurityGroup**adlı bir aygıt çift etiket özelliği tarafından tanımlanır. Varsayılan olarak, IoT Hub'daki her IoT çözümcünün **varsayılan**adlı bir güvenlik grubu bulunuyor. Bir aygıtın güvenlik grubunu değiştirmek için **SecurityGroup** özelliğinin değerini değiştirin.

Örneğin:

```
{
  "deviceId": "VM-Contoso12",
  "etag": "AAAAAAAAAAM=",
  "deviceEtag": "ODA1BzA5QjM2",
  "status": "enabled",
  "statusUpdateTime": "0001-01-01T00:00:00",
  "connectionState": "Disconnected",
  "lastActivityTime": "0001-01-01T00:00:00",
  "cloudToDeviceMessageCount": 0,
  "authenticationType": "sas",
  "x509Thumbprint": {
    "primaryThumbprint": null,
    "secondaryThumbprint": null
  },
  "version": 4,
  "tags": {
    "SecurityGroup": "default"
  },
```

Aygıtlarınızı mantıksal kategorilere gruplandırmak için güvenlik gruplarını kullanın. Grupları oluşturduktan sonra, en etkili uçtan uca IoT güvenlik çözümü için onları seçtiğiniz özel uyarılara atayın.

## <a name="customize-an-alert"></a>Bir uyarıyı özelleştirme

1. IoT Hub'ınızı açın.
1. **Güvenlik** bölümünde **Özel uyarıları** tıklatın.
1. Özelleştirmeyi uygulamak istediğiniz bir güvenlik grubu seçin.
1. **Özel uyarı ekle'yi**tıklatın.
1. Açılır listeden özel bir uyarı seçin.
1. Gerekli özellikleri edin, **Tamam'ı**tıklatın.
1. **KAYDET'i**tıklattığınızdan emin olun. Yeni uyarıyı kaydetmeden, IoT Hub'ı bir sonraki kapattığınızda uyarı silinir.

## <a name="alerts-available-for-customization"></a>Özelleştirme için kullanılabilir uyarılar

Azure Güvenlik Merkezi IoT, özel gereksinimlerinize göre özelleştirilebilen çok sayıda uyarı sunar. Uyarı nın önem derecesi, veri kaynağı, açıklama ve önerilen düzeltme adımlarımız için [özelleştirilebilir uyarı tablosunu](concept-customizable-security-alerts.md) gözden geçirin.

## <a name="next-steps"></a>Sonraki adımlar

Bir güvenlik aracısını nasıl dağıtılayın öğrenmek için bir sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Bir güvenlik aracısı dağıtma](how-to-deploy-agent.md)
