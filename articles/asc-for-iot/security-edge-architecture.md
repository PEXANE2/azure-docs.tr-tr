---
title: IoT Edge için IoT güvenlik modülü için Azure Güvenlik Merkezi'ni anlama| Microsoft Dokümanlar
description: IoT Edge için IoT güvenlik modülü için Azure Güvenlik Merkezi'nin mimarisini ve yeteneklerini anlayın.
services: asc-for-iot
ms.service: asc-for-iot
documentationcenter: na
author: mlottner
manager: rkarlin
editor: ''
ms.assetid: e78523ae-d70a-456a-818d-f8b1b025d7cb
ms.subservice: asc-for-iot
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/23/2019
ms.author: mlottner
ms.openlocfilehash: 148c68234a937efde554ef00a6014cdc1a350f34
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71315900"
---
# <a name="azure-iot-edge-security-module"></a>Azure IoT Edge güvenlik modülü

[Azure IoT Edge,](https://docs.microsoft.com/azure/iot-edge/) iş akışlarını yönetmek ve gerçekleştirmek için güçlü özellikler sağlar.
IoT Edge'in IoT ortamlarında oynadığı en önemli kısım, onu kötü niyetli aktörler için özellikle çekici kılıyor.

Azure Güvenlik Merkezi ioT güvenlik modülü, IoT Edge aygıtlarınız için kapsamlı bir güvenlik çözümü sağlar.
Azure Güvenlik Merkezi IoT modülü, İşletim Sisteminizden ve konteyner sisteminizden gelen ham güvenlik verilerini eyleme geçirilebilir güvenlik önerileri ve uyarılarına toplar, toplar ve analiz eder.

IoT aygıtları için IoT güvenlik aracıları için Azure Güvenlik Merkezi'ne benzer şekilde, IoT Edge için Azure Güvenlik Merkezi modülü ikizi aracılığıyla son derece özelleştirilebilir.
Bkz. Daha fazla bilgi edinmek için [aracınızı yapılandırın.](how-to-agent-configuration.md)

IoT Edge için Azure Güvenlik Merkezi IoT güvenlik modülü aşağıdaki özellikleri sunar:

- Temel İşletim Sistemi (Linux) ve IoT Edge Konteyner sistemlerinden ham güvenlik olayları toplar.
  
  Kullanılabilir güvenlik veri toplayıcıları hakkında daha fazla bilgi edinmek [için IoT aracı yapılandırması için Azure Güvenlik Merkezi'ne](how-to-agent-configuration.md) bakın.

- IoT Edge dağıtım bildirimlerinin analizi.

- Ham güvenlik olaylarını [IoT Edge Hub](https://docs.microsoft.com/azure/iot-edge/iot-edge-runtime#iot-edge-hub)üzerinden gönderilen iletilere toplar.

- Güvenlik modülü ikizi kullanarak yapılandırmayı kaldırın.

  Bkz. Daha fazla bilgi edinmek [için IoT aracısı için bir Azure Güvenlik Merkezi Yapılandırın.](how-to-agent-configuration.md)

IoT Edge için Azure Güvenlik Merkezi IoT Edge için ayrıcalıklı bir modda çalışır.
Modülün İşletim Sistemini ve diğer IoT Edge modüllerini izlemesi için ayrıcalıklı mod gereklidir.

## <a name="module-supported-platforms"></a>Modül destekli platformlar

IoT Edge için Azure Güvenlik Merkezi IoT güvenlik modülü şu anda yalnızca Linux için kullanılabilir. 

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT Edge için IoT güvenlik modülü için Azure Güvenlik Merkezi'nin mimarisi ni ve yeteneklerini öğrendiniz.

IoT dağıtımı için Azure Güvenlik Merkezi'ni kullanmaya devam etmek için aşağıdaki makaleleri kullanın:

- [IoT Edge için güvenlik modüllerini dağıtın](how-to-deploy-edge.md)
- Güvenlik modülünüzü nasıl [yapılandırılatırmayı](how-to-agent-configuration.md) öğrenin
- IoT [Hizmeti ön koşulları](service-prerequisites.md) için Azure Güvenlik Merkezi'ni inceleyin
- [IoT Hub'ınızda Azure Güvenlik Merkezi ioT hizmetini](quickstart-onboard-iot-hub.md) nasıl etkinleştirin öğrenin
- [IoT SSS](resources-frequently-asked-questions.md) için Azure Güvenlik Merkezi'nden hizmet hakkında daha fazla bilgi edinin