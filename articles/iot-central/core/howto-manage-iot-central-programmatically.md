---
title: Program aracılığıyla IoT Central yönetme | Microsoft Docs
description: Bu makalede, IoT Central programlı bir şekilde oluşturma ve yönetme açıklanır. JavaScript, Python, C#, Ruby ve Go gibi birden çok dil SDK 'Sı kullanarak uygulamayı görüntüleyebilir, değiştirebilir ve kaldırabilirsiniz.
services: iot-central
ms.service: iot-central
author: dominicbetts
ms.author: dobett
ms.date: 05/19/2020
ms.topic: how-to
ms.openlocfilehash: ba0ee0a610299bbe6b7e550f204cd2fd50d6d71a
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "83749210"
---
# <a name="manage-iot-central-programmatically"></a>Program aracılığıyla IoT Central yönetme

[!INCLUDE [iot-central-selector-manage](../../../includes/iot-central-selector-manage.md)]

[Azure IoT Central uygulama Yöneticisi](https://aka.ms/iotcentral) web sitesinde IoT Central uygulamaları oluşturmak ve yönetmek yerine, Azure SDK 'larını kullanarak uygulamalarınızı programlı bir şekilde yönetebilirsiniz. Desteklenen diller arasında JavaScript, Python, C#, Ruby ve go bulunur.

## <a name="install-the-sdk"></a>SDK Yükleme

Aşağıdaki tabloda SDK depoları ve paket yükleme komutları listelenmektedir:

| SDK deposu | Paket yükleme |
| -------------- | ------------ |
| [JavaScript için Azure ıotmerkezileştiri Istemci SDK 'Sı](https://github.com/Azure/azure-sdk-for-js/tree/master/sdk/iotcentral/arm-iotcentral) | `npm install @azure/arm-iotcentral` |
| [Python için SDK Microsoft Azure](https://github.com/Azure/azure-sdk-for-python/tree/master/sdk/iothub/azure-mgmt-iotcentral/azure/mgmt/iotcentral) | `pip install azure-mgmt-iotcentral` |
| [.NET için Azure SDK](https://github.com/Azure/azure-sdk-for-net/tree/master/sdk/iotcentral/Microsoft.Azure.Management.IotCentral) | `dotnet add package Microsoft.Azure.Management.IotCentral` |
| [Ruby-kaynak yönetimi için Microsoft Azure SDK (Önizleme)](https://github.com/Azure/azure-sdk-for-ruby/tree/master/management/azure_mgmt_iot_central/lib/2018-09-01/generated/azure_mgmt_iot_central) | `gem install azure_mgmt_iot_central` |
| [Java için Azure SDK](https://github.com/Azure/azure-sdk-for-java/tree/master/sdk/iotcentral) | [Maven paketi](https://search.maven.org/search?q=a:azure-mgmt-iotcentral) |
| [Go için Azure SDK](https://github.com/Azure/azure-sdk-for-go/tree/master/services/iotcentral/mgmt/2018-09-01/iotcentral) | [Paket sürümleri](https://github.com/Azure/azure-sdk-for-go/releases) |

## <a name="samples"></a>Örnekler

[Azure IoT Central ARM SDK örnekleri](https://docs.microsoft.com/samples/azure-samples/azure-iot-central-arm-sdk-samples/azure-iot-central-arm-sdk-samples/) deposu, Azure IoT Central uygulamalarını oluşturma, güncelleştirme, listeleme ve silme işlemlerinin nasıl yapılacağını gösteren birden fazla programlama dili için kod örneklerine sahiptir.

## <a name="next-steps"></a>Sonraki adımlar

Azure IoT Central uygulamalarını programlı olarak yönetmeyi öğrendiğinize göre, bir sonraki adım [Azure Resource Manager](../../azure-resource-manager/management/overview.md) hizmeti hakkında daha fazla bilgi edinmek için kullanılır.
