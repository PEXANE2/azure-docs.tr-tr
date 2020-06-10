---
title: Azure dijital TWINS CLı 'sını kullanma
titleSuffix: Azure Digital Twins
description: Bkz. kullanmaya başlama ve Azure dijital TWINS CLı 'yi kullanma.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 53b20ded8e4b4a003beff1ef8489ecd9ff3451ac
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84612806"
---
# <a name="use-the-azure-digital-twins-cli"></a>Azure dijital TWINS CLı 'sını kullanma

Azure Digital TWINS örneğinizi, Azure portal Azure dijital TWINS örneğinizin yönetimine ek olarak, hizmetle birlikte en önemli işlemleri gerçekleştirmek için kullanabileceğiniz bir **komut satırı arabirimi (CLI)** vardır:
* Azure dijital TWINS örneğini yönetme
* Modelleri yönetme
* Dijital TWINS 'yi yönetme
* İkizi ilişkilerini yönetme
* Uç noktaları yapılandırma
* [Yolları](concepts-route-events.md) yönetme
* Rol tabanlı erişim denetimi (RBAC) aracılığıyla [güvenliği](concepts-security.md) yapılandırma

Azure dijital TWINS komutları Azure [CLI Için Azure IoT uzantısının](https://github.com/Azure/azure-iot-cli-extension)bir parçasıdır. Bu komutlar için başvuru belgelerini komut kümesinin bir parçası olarak görüntüleyebilirsiniz `az iot` : [az DT](https://docs.microsoft.com/cli/azure/ext/azure-iot/dt?view=azure-cli-latest).

## <a name="deploy-and-validate"></a>Dağıtma ve doğrulama

Genellikle örneğinizi yönetmeye ek olarak, CLı de dağıtım ve doğrulama için yararlı bir araçtır.
* Denetim düzlemi komutları, yeni bir örneğin dağıtımını tekrarlanabilir veya otomatik hale getirmek için kullanılabilir.
* Veri düzlemi komutları, örneğinizin değerlerini hızlıca denetlemek ve işlemlerin beklendiği gibi tamamlandığını doğrulamak için kullanılabilir.

## <a name="next-steps"></a>Sonraki adımlar

CLı komutlarına bir alternatif için, bkz. API 'Leri ve SDK 'Ları kullanarak Azure dijital TWINS örneğini yönetme:
* [Nasıl yapılır: Azure dijital TWINS API 'Leri ve SDK 'Larını kullanma](how-to-use-apis-sdks.md)
