---
title: Azure IoT Central uygulamanızı Dynamics 365 alan hizmetleriyle bağlama | Microsoft Docs
description: Azure IoT Central ve Dynamics 365 alan hizmeti ile uçtan uca bir çözüm oluşturmayı öğrenin
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 7c708268eaeade1cfb625c73ff8758a1941ff5b8
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "80157458"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Azure IoT Central ve Dynamics 365 alan hizmeti ile uçtan uca çözüm oluşturma 



Bir Oluşturucu olarak, Azure IoT Central uygulamanızın diğer iş sistemleriyle tümleştirilmesine olanak sağlayabilirsiniz. 


Örneğin, bağlı bir çöp yönetimi çözümünde çöp koleksiyonları kamyonları gönderimi iyileştirebilirsiniz. İyileştirme, bağlı çöp kutularının IoT algılayıcı verilerine göre yapılabilir. [IoT Central bağlı çöp yönetimi uygulamanızda](./tutorial-connected-waste-management.md) , kuralları ve eylemleri yapılandırabilir ve bu ayarı Dynamics alan hizmeti 'nde uyarı oluşturmayı tetikleyebilirsiniz. Bu senaryo, uygulamalar ve hizmetler genelinde iş akışlarını otomatikleştirmek için IoT Central doğrudan yapılandırılacak olan Microsoft Flow kullanılarak gerçekleştirilir. Ayrıca, alan hizmetindeki hizmet etkinliklerine bağlı olarak, bilgiler Azure IoT Central 'ye gönderilebilir. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Azure IoT Central uygulamanızı Dynamics 365 alan hizmetleriyle bağlama 

Aşağıdaki tümleştirme işlemlerine, saf bir yapılandırma deneyimine göre kolayca uygulanabilirler:
* Azure IoT Central, Tanılama için bağlı alan hizmeti (IoT uyarısı olarak) ile cihaz bozuklukları hakkında bilgi gönderebilir.
* Bağlı alan hizmeti, cihaz bozuklularından tetiklenen servis talepleri veya iş emirleri oluşturabilir.
* Bağlı alan hizmeti, kapalı kalma süresinin oluşmasını önlemeye yönelik teknisyenleri İnceleme için zamanlayabilir.
* Azure IoT Central cihaz panosu, ilgili hizmet ve zamanlama bilgileriyle güncelleştirilir.


## <a name="next-steps"></a>Sonraki adımlar
* [IoT Central kamu şablonları](./overview-iot-central-government.md) hakkında daha fazla bilgi edinin
* [IoT Central](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central) hakkında daha fazla bilgi edinin
* [Dynamics 365 alan Hizmetleri](https://docs.microsoft.com/dynamics365/field-service/cfs-iot-overview) hakkında daha fazla bilgi edinin
