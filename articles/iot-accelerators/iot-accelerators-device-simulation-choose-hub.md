---
title: Aygıt Simülasyonu çözümüyle mevcut IoT hub'ını kullanma - Azure | Microsoft Dokümanlar
description: Bu makalede, aygıt simülasyonçözüm hızlandırıcısının varolan bir IoT Hub'ını kullanacak şekilde nasıl yapılandırılabildiğini açıklanmaktadır.
author: dominicbetts
manager: timlt
ms.author: dobett
ms.service: iot-accelerators
services: iot-accelerators
ms.date: 10/25/2018
ms.topic: conceptual
ms.openlocfilehash: 1f89e23d7bb279e7cce5c104060cc7898517f8b7
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "73889189"
---
# <a name="use-an-existing-iot-hub-with-the-device-simulation-solution-accelerator"></a>Cihaz Benzetimi çözüm hızlandırıcısında, var olan bir IoT Hub kullanma

Aygıt Simülasyonu'ni dağıttığınızda, simülasyonunuzda kullanmak üzere bir IoT hub'ı dağıtmayı da seçebilirsiniz. Bu seçenek, tek bir ölçek birimi ile bir [S2 tier IoT hub](../iot-hub/iot-hub-scaling.md)dağıtılır. Bu isteğe bağlı IoT hub'ı dağıtıyorsanız, simülasyon çalışması için başka bir IoT Hub'ı hedeflemeyi seçebilirsiniz.

İsteğe bağlı IoT Hub'ını dağıtmamayı seçerseniz, çalıştırdığınız simülasyonlar için kendi hub'ınızı kullanmanız gerekir.

IoT hub'ıniz yoksa, [Azure portalından](https://portal.azure.com)her zaman yeni bir hub oluşturabilirsiniz.

Önceden varolan bir IoT hub'ını kullanmak **için, iothubowner** paylaşılan erişim ilkesi için bağlantı dizesi gerekir. Bu bağlantı dizesini [Azure portalından](https://portal.azure.com)alabilirsiniz:

1. Portaldaki hub'ın yapılandırma sayfasında **Paylaşılan erişim ilkelerini**tıklatın.

1. **iothubowner'ı**tıklatın.

1. Birincil veya ikincil bağlantı dizesini kopyalayın.

[![Bağlantı dizesi alın](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-inline.png)](./media/iot-accelerators-device-simulation-choose-hub/connectionstring-expanded.png#lightbox)

Simülasyonu yapılandırırken kopyaladığınız bağlantı dizesini kullanın:

![Simülasyonu yapılandırma](./media/iot-accelerators-device-simulation-choose-hub/configuresimulation.png)

### <a name="next-steps"></a>Sonraki adımlar

Bu nasıl yapılmalı kılavuzunda, bir simülasyonda varolan bir IoT hub'ı nasıl kullanacağınızı öğrendiniz. Ardından, simülasyon için gelişmiş [bir aygıt modeli oluşturmayı](iot-accelerators-device-simulation-advanced-device.md) öğrenmek isteyebilirsiniz.
