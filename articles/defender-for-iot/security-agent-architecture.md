---
title: 'Hızlı başlangıç: güvenlik aracılarına genel bakış'
description: Bu hızlı başlangıçta, IoT hizmetinde Azure Defender 'da kullanılan aracıların güvenlik Aracısı mimarisini nasıl anlayacağınızı öğreneceksiniz.
services: defender-for-iot
ms.service: defender-for-iot
documentationcenter: na
author: shhazam-ms
manager: rkarlin
editor: ''
ms.devlang: na
ms.topic: quickstart
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 01/24/2021
ms.author: shhazam
ms.openlocfilehash: de8c52372b2ef92dcf2abe357a40ef8947439a8a
ms.sourcegitcommit: 772eb9c6684dd4864e0ba507945a83e48b8c16f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "103493977"
---
# <a name="quickstart-security-agent-reference-architecture"></a>Hızlı başlangıç: güvenlik Aracısı başvuru mimarisi

IoT için Azure Defender, IoT Hub aracılığıyla güvenlik verilerini günlüğe kaydetmek, işlemek, toplamak ve göndermek için kullanılan güvenlik aracıları için başvuru mimarisi sağlar.

Güvenlik aracıları kısıtlanmış bir IoT ortamında çalışacak şekilde tasarlanmıştır ve kullandıkları kaynaklarla karşılaştırıldığında bunların sağladığı değer bakımından yüksek düzeyde özelleştirilebilir.

Güvenlik aracıları aşağıdaki özellikleri destekler:

- Mevcut cihaz kimliğiyle veya ayrılmış bir modül kimliğiyle kimlik doğrulaması yapın. Daha fazla bilgi için bkz. [Güvenlik Aracısı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md).

- Temel Işletim sisteminden (Linux, Windows) ham güvenlik olayları toplayın. Kullanılabilir güvenlik veri toplayıcıları hakkında daha fazla bilgi edinmek için bkz. [Defender for IoT Aracısı yapılandırması](how-to-agent-configuration.md).

- Ham güvenlik olaylarını IoT Hub aracılığıyla gönderilen iletilere toplayın.

- **Azureiotsecurity** modülünün kullanımı üzerinden uzaktan yapılandırma ikizi. Daha fazla bilgi için bkz. [IoT Aracısı için bir Defender yapılandırma](how-to-agent-configuration.md).

IoT güvenlik aracıları için Defender, açık kaynaklı projeler olarak geliştirilmiştir ve GitHub 'dan kullanılabilir:

- [IoT C tabanlı aracı için Defender](https://github.com/Azure/Azure-IoT-Security-Agent-C)
- [IoT için Defender C# tabanlı aracı](https://github.com/Azure/Azure-IoT-Security-Agent-CS)

## <a name="prerequisites"></a>Önkoşullar

Yok

## <a name="agent-supported-platforms"></a>Aracılı desteklenen platformlar

IoT için Defender, 32 bit ve 64 bit Windows için farklı yükleyici aracıları ve 32 bit ve 64 bit Linux için de aynı şekilde sunulmaktadır. Aşağıdaki tabloya göre cihazlarınızın her biri için doğru aracı yükleyicisine sahip olduğunuzdan emin olun:

| Mimari | Linux | Windows | Ayrıntılar |
|--|--|--|--|
| 32 bit | C | C# |  |
| 64 bit | C# veya C | C# | Daha kısıtlı veya en az cihaz kaynağı olan cihazlar için C Aracısı kullanmanızı öneririz. |


## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, IoT Defender-IoT-mikro aracı mimarisi ve kullanılabilir yükleyiciler için Defender hakkında üst düzey bir genel bakış aldınız.

IoT dağıtımı için Defender 'ı kullanmaya devam etmek için aşağıdaki makaleleri kullanın:

> [!div class="nextstepaction"]
> [Güvenlik Aracısı kimlik doğrulama yöntemleri](concept-security-agent-authentication-methods.md)
