---
title: Azure Digital Twins CLI’sini kullanma
titleSuffix: Azure Digital Twins
description: Bkz. kullanmaya başlama ve Azure dijital TWINS CLı 'yi kullanma.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 7f13dc3e86b21a3f4113a7a7c6f477f239315a27
ms.sourcegitcommit: 11e2521679415f05d3d2c4c49858940677c57900
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87499101"
---
# <a name="use-the-azure-digital-twins-cli"></a>Azure Digital Twins CLI’sini kullanma

Azure Digital TWINS örneğinizi, Azure portal Azure dijital TWINS örneğinizin yönetimine ek olarak, hizmetle birlikte en önemli işlemleri gerçekleştirmek için kullanabileceğiniz bir **komut satırı arabirimi (CLI)** vardır:
* Azure dijital TWINS örneğini yönetme
* Modelleri yönetme
* Dijital TWINS 'yi yönetme
* İkizi ilişkilerini yönetme
* Uç noktaları yapılandırma
* [Yolları](concepts-route-events.md) yönetme
* Rol tabanlı erişim denetimi (RBAC) aracılığıyla [güvenliği](concepts-security.md) yapılandırma

## <a name="uses-deploy-and-validate"></a>Kullanır (Dağıt ve Doğrula)

Genellikle örneğinizi yönetmeye ek olarak, CLı de dağıtım ve doğrulama için yararlı bir araçtır.
* Denetim düzlemi komutları, yeni bir örneğin dağıtımını tekrarlanabilir veya otomatik hale getirmek için kullanılabilir.
* Veri düzlemi komutları, örneğinizin değerlerini hızlıca denetlemek ve işlemlerin beklendiği gibi tamamlandığını doğrulamak için kullanılabilir.

## <a name="get-the-extension"></a>Uzantıyı al

Azure dijital TWINS komutları Azure [CLI Için Azure IoT uzantısının](https://github.com/Azure/azure-iot-cli-extension)bir parçasıdır. Bu komutlar için başvuru belgelerini komut kümesinin bir parçası olarak görüntüleyebilirsiniz `az iot` : [az DT](https://docs.microsoft.com/cli/azure/ext/azure-iot/dt?view=azure-cli-latest).

Bu adımlarla uzantının en son sürümüne sahip olduğunuzdan emin olabilirsiniz. Bu komutları [Azure Cloud Shell](../cloud-shell/overview.md) veya [yerel bir Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)' de çalıştırabilirsiniz.

[!INCLUDE [digital-twins-cloud-shell-extensions.md](../../includes/digital-twins-cloud-shell-extensions.md)]

## <a name="next-steps"></a>Sonraki adımlar

CLı komutlarına bir alternatif için, bkz. API 'Leri ve SDK 'Ları kullanarak Azure dijital TWINS örneğini yönetme:
* [*Nasıl yapılır: Azure dijital TWINS API 'Leri ve SDK 'Larını kullanma*](how-to-use-apis-sdks.md)
