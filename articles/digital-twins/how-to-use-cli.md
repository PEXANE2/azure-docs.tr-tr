---
title: Azure Digital Twins CLI’sini kullanma
titleSuffix: Azure Digital Twins
description: Bkz. kullanmaya başlama ve Azure dijital TWINS CLı 'yi kullanma.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 34afc732d64a7aa29261989cc790c8a113208d96
ms.sourcegitcommit: 03713bf705301e7f567010714beb236e7c8cee6f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/21/2020
ms.locfileid: "92331779"
---
# <a name="use-the-azure-digital-twins-cli"></a>Azure Digital Twins CLI’sini kullanma

Azure Digital TWINS örneğinizi, Azure portal Azure dijital TWINS örneğinizin yönetimine ek olarak, hizmetle birlikte en önemli işlemleri gerçekleştirmek için kullanabileceğiniz bir **komut satırı arabirimi (CLI)** vardır:
* Azure dijital TWINS örneğini yönetme
* Modelleri yönetme
* Dijital TWINS 'yi yönetme
* İkizi ilişkilerini yönetme
* Uç noktaları yapılandırma
* [Yolları](concepts-route-events.md) yönetme
* Azure rol tabanlı erişim denetimi (Azure RBAC) aracılığıyla [güvenliği](concepts-security.md) yapılandırma

[!INCLUDE [digital-twins-known-issue-cloud-shell](../../includes/digital-twins-known-issue-cloud-shell.md)]

## <a name="uses-deploy-and-validate"></a>Kullanır (Dağıt ve Doğrula)

Genellikle örneğinizi yönetmeye ek olarak, CLı de dağıtım ve doğrulama için yararlı bir araçtır.
* Denetim düzlemi komutları, yeni bir örneğin dağıtımını tekrarlanabilir veya otomatik hale getirmek için kullanılabilir.
* Veri düzlemi komutları, örneğinizin değerlerini hızlıca denetlemek ve işlemlerin beklendiği gibi tamamlandığını doğrulamak için kullanılabilir.

## <a name="get-the-extension"></a>Uzantıyı al

Azure dijital TWINS komutları Azure [CLI Için Azure IoT uzantısının](https://github.com/Azure/azure-iot-cli-extension)bir parçasıdır. Komut kümesi için başvuru belgelerinin bir parçası olarak komutların ve kullanımlarınızın tam listesini görebilirsiniz `az iot` : [ *az DT* komut Reference](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest).

Bu adımlarla uzantının en son sürümüne sahip olduğunuzdan emin olabilirsiniz. Bu komutları [Azure Cloud Shell](../cloud-shell/overview.md) veya [yerel bir Azure CLI](/cli/azure/install-azure-cli?preserve-view=true&view=azure-cli-latest)' de çalıştırabilirsiniz.

[!INCLUDE [digital-twins-cloud-shell-extensions.md](../../includes/digital-twins-cloud-shell-extensions.md)]

## <a name="next-steps"></a>Sonraki adımlar

Başvuru belgeleri aracılığıyla CLı ve tam komut kümesini gezin:
* [*az DT* komut başvurusu](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest)