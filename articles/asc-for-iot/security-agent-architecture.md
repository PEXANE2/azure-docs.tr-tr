---
title: IoT güvenlik aracımimarisi için Azure Güvenlik Merkezi'ni anlama| Microsoft Dokümanlar
description: IoT için Azure Güvenlik Merkezi'nde kullanılan aracılar için güvenlik aracımimarisini anlayın.
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
ms.date: 07/26/2019
ms.author: mlottner
ms.openlocfilehash: 998aeab197931a75579fc39b28e3a248b85fc57b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68596915"
---
# <a name="security-agent-reference-architecture"></a>Güvenlik aracısı başvuru mimarisi

Azure Güvenlik Merkezi ioT hub üzerinden güvenlik verilerini kaydeden, işleyen, toplayan ve gönderen güvenlik aracıları için başvuru mimarisi sağlar.

Güvenlik aracıları kısıtlı bir IoT ortamında çalışacak şekilde tasarlanmıştır ve tükettikleri kaynaklarla karşılaştırıldığında sağladıkları değerler açısından son derece özelleştirilebilir.

Güvenlik aracıları aşağıdaki özellikleri destekler:

- Temel İşletim Sisteminden (Linux, Windows) ham güvenlik olaylarını toplayın. Kullanılabilir güvenlik veri toplayıcıları hakkında daha fazla bilgi edinmek [için IoT aracı yapılandırması için Azure Güvenlik Merkezi'ne](how-to-agent-configuration.md)bakın.

- Ham güvenlik olaylarını IoT Hub üzerinden gönderilen iletilere toplar.

- Varolan aygıt kimliğiyle veya özel bir modül kimliğiyle kimlik doğrulaması. Daha fazla bilgi edinmek için [Güvenlik aracısı kimlik doğrulama yöntemlerine](concept-security-agent-authentication-methods.md) bakın.

- **Azureiotsecurity** modülü ikizi kullanarak uzaktan yapılandırın. Daha fazla bilgi için [bkz.](how-to-agent-configuration.md)

Azure Güvenlik Merkezi IoT Güvenlik aracıları açık kaynak proje olarak geliştirilmiştir ve GitHub'dan edinilebilir: 

- [IoT C tabanlı aracı için Azure Güvenlik Merkezi](https://github.com/Azure/Azure-IoT-Security-Agent-C) 
- [IoT C# tabanlı aracı için Azure Güvenlik Merkezi](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="agent-supported-platforms"></a>Aracı destekli platformlar

Azure Security Center for IoT, 32bit ve 64bit Windows için farklı yükleyici aracıları sunarken, 32bit ve 64bit Linux için de aynı sıyrık. Aşağıdaki tabloya göre her aygıtınız için doğru aracı yükleyiciye sahip olduğundan emin olun:

| Mimari | Linux | Windows |    Ayrıntılar|
|----------|----------------------------------------------|-------------|-------------------------------------------|
| 32 bit  | C  | C#  ||
| 64bit  | C# veya C           | C#      | C aracısını daha kısıtlı veya minimum aygıt kaynaklarına sahip aygıtlar için kullanmanızı öneririz.|
|

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT güvenlik aracısı mimarisi için Azure Güvenlik Merkezi'ni ve kullanılabilir yükleyicileri öğrendiniz.

IoT dağıtımı için Azure Güvenlik Merkezi'ni kullanmaya devam etmek için aşağıdaki makaleleri kullanın:

- [Güvenlik aracısı kimlik doğrulama yöntemlerini](concept-security-agent-authentication-methods.md) anlama
- Bir güvenlik [aracısı](how-to-deploy-agent.md) seçme ve dağıtma
- IoT [hizmeti ön koşulları](service-prerequisites.md) için Azure Güvenlik Merkezi'ni inceleyin
- [IoT Hub'ınızda Azure Güvenlik Merkezi IoT hizmetini nasıl etkinleştirmenizi](quickstart-onboard-iot-hub.md) öğrenin
- [IoT SSS](resources-frequently-asked-questions.md) için Azure Güvenlik Merkezi'nden hizmet hakkında daha fazla bilgi edinin
