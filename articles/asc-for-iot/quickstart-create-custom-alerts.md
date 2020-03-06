---
title: 'Hızlı başlangıç: IoT için Azure Güvenlik Merkezi için özel uyarılar oluşturma'
description: IoT güvenlik hizmeti için Azure Güvenlik Merkezi için özel cihaz uyarılarını anlayın, oluşturun ve atayın.
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
ms.openlocfilehash: 063e5c9e7d75fd1c07d148c265b1fe64eee3cbc8
ms.sourcegitcommit: f915d8b43a3cefe532062ca7d7dbbf569d2583d8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78303537"
---
# <a name="quickstart-create-custom-alerts"></a>Hızlı başlangıç: özel uyarılar oluşturma


Özel güvenlik grupları ve uyarılar kullanarak, IoT çözümünüz genelinde daha iyi güvenlik sağlamak için uçtan uca güvenlik bilgilerinin ve kategorik cihaz bilgilerinin tam avantajlarından yararlanır. 

## <a name="why-use-custom-alerts"></a>Özel uyarılar neden kullanılmalıdır? 

IoT cihazlarınızı en iyi şekilde anlarsınız.

Beklenen cihaz davranışlarını tam olarak anlayan müşteriler için IoT için Azure Güvenlik Merkezi, bu anlalamayı, beklenen, normal davranıştaki herhangi bir sapmayla ilgili bir cihaz davranışı ilkesi ve uyarısına çevirmenizi sağlar.

## <a name="security-groups"></a>Güvenlik grupları

Güvenlik grupları, mantıksal cihaz gruplarını tanımlamanızı ve güvenlik durumlarını merkezi bir şekilde yönetmenizi sağlar.

Bu gruplar belirli donanımlar olan cihazları, belirli bir konumda dağıtılan cihazları veya özel gereksinimlerinize uygun diğer grupları temsil edebilir.

Güvenlik grupları, **SecurityGroup**adlı bir Device ikizi Tag özelliği tarafından tanımlanır. Varsayılan olarak, IoT Hub her bir IoT çözümünün **varsayılan**adlı bir güvenlik grubu vardır. Bir cihazın güvenlik grubunu değiştirmek için **SecurityGroup** özelliğinin değerini değiştirin.
 
Örnek:

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

Cihazlarınızı mantıksal kategoriler halinde gruplandırmak için güvenlik gruplarını kullanın. Grupları oluşturduktan sonra, en etkili uçtan uca IoT güvenlik çözümü için bunları tercih ettiğiniz özel uyarılara atayın. 

## <a name="customize-an-alert"></a>Bir uyarıyı özelleştirme

1. IoT Hub açın. 
2. **Güvenlik** bölümünde **özel uyarılar** ' a tıklayın. 
3. Özelleştirmeyi uygulamak istediğiniz güvenlik grubunu seçin. 
4. **Özel uyarı Ekle**' ye tıklayın.
5. Açılan listeden özel bir uyarı seçin. 
6. Gerekli özellikleri düzenleyin, **Tamam**' a tıklayın.
7. **Kaydet**' e tıkladığınızdan emin olun. Yeni uyarının kaydedilmeksizin, IoT Hub bir sonraki kapatıyorsunuz uyarı silinir.

 
## <a name="alerts-available-for-customization"></a>Özelleştirmeler için kullanılabilir uyarılar

IoT için Azure Güvenlik Merkezi, özel gereksinimlerinize göre özelleştirilebilen çok sayıda uyarı sunmaktadır. Uyarı önem derecesi, veri kaynağı, açıklama ve her uyarının alındığı zaman ve önerdiğimiz düzeltme adımlarımızın [özelleştirilebilir uyarı tablosunu](concept-customizable-security-alerts.md) gözden geçirin. 


## <a name="next-steps"></a>Sonraki adımlar

Bir güvenlik aracısının nasıl dağıtılacağını öğrenmek için bir sonraki makaleye ilerleyin...

> [!div class="nextstepaction"]
> [Güvenlik aracısı dağıtma](how-to-deploy-agent.md)
