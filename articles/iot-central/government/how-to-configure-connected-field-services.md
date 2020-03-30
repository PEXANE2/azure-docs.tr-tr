---
title: Azure IoT Central uygulamanızı Dynamics 365 Saha Hizmetleri ile bağlayın | Microsoft Dokümanlar
description: Azure IoT Merkezi ve Dynamics 365 Saha Hizmeti ile uçtan uca çözüm oluşturmayı öğrenin
author: miriambrus
ms.author: miriamb
ms.date: 10/23/2019
ms.topic: how-to
ms.service: iot-central
services: iot-central
ms.openlocfilehash: 7c708268eaeade1cfb625c73ff8758a1941ff5b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80157458"
---
# <a name="build-end-to-end-solution-with-azure-iot-central-and-dynamics-365-field-service"></a>Azure IoT Central ve Dynamics 365 Saha Hizmeti ile uçtan uca çözüm oluşturun 



Bir oluşturucu olarak, Azure IoT Central uygulamanızın diğer iş sistemlerine entegrasyonunu etkinleştirebilirsiniz. 


Örneğin, bağlı bir atık yönetimi çözümünde çöp toplama kamyonlarının sevkiyatını optimize edebilirsiniz. Optimizasyon, bağlı atık kutularından alınan IoT sensörleri verilerine dayalı olarak yapılabilir. [IoT Central'a bağlı atık yönetimi uygulamanızda](./tutorial-connected-waste-management.md) kuralları ve eylemleri yapılandırabilir ve Dynamics Field Service'de uyarı oluşturmayı tetiklemek üzere ayarlayabilirsiniz. Bu senaryo, uygulamalar ve hizmetler arasında iş akışlarını otomatikleştirmek için doğrudan IoT Central'da yapılandırılacak olan Microsoft Akışı kullanılarak gerçekleştirilir. Ayrıca, Saha Hizmeti'ndeki hizmet etkinliklerine bağlı olarak, bilgiler Azure IoT Central'a geri gönderilebilir. 

## <a name="how-to-connect-your-azure-iot-central-application-with-dynamics-365-field-services"></a>Azure IoT Central uygulamanızı Dynamics 365 Saha Hizmetleri ile bağlama 

Aşağıdaki tümleştirme süreçleri, saf bir yapılandırma deneyimine dayalı olarak kolayca uygulanabilir:
* Azure IoT Central, cihaz anomalileri hakkında bilgi tanı için Bağlı Saha Hizmetine (IoT Uyarısı olarak) gönderebilir.
* Bağlı Saha Hizmeti, aygıt anomalilerinden tetiklenen servis ler veya iş emirleri oluşturabilir.
* Bağlı Saha Hizmeti, kapalı kalma süresi olaylarını önlemek için teknisyenleri denetime programlayabilir.
* Azure IoT Merkezi aygıt panosu ilgili hizmet ve zamanlama bilgileriyle güncellenebilir.


## <a name="next-steps"></a>Sonraki adımlar
* [IoT Merkezi yönetim şablonları](./overview-iot-central-government.md) hakkında daha fazla bilgi edinin
* [IoT Central](https://docs.microsoft.com/azure/iot-central/core/overview-iot-central) hakkında daha fazla bilgi edinin
* [Dynamics 365 Saha Hizmetleri](https://docs.microsoft.com/dynamics365/field-service/cfs-iot-overview) hakkında daha fazla bilgi edinin
