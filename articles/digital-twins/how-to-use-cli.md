---
title: Azure Digital Twins CLI’sini kullanma
titleSuffix: Azure Digital Twins
description: Bkz. kullanmaya başlama ve Azure dijital TWINS CLı 'yi kullanma.
author: baanders
ms.author: baanders
ms.date: 05/25/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: a24b8b18dd109f1d8ed5acaa7de55ce5a3cc1eb9
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102201116"
---
# <a name="use-the-azure-digital-twins-cli"></a>Azure Digital Twins CLI’sini kullanma

Azure dijital TWINS örneğinizi, Azure portal Azure dijital TWINS örneğinizin yönetimine ek olarak, Azure **[CLI](/cli/azure/what-is-azure-cli) için** , hizmetle birlikte en önemli işlemleri gerçekleştirmek için kullanabileceğiniz bir komut kümesi vardır; örneğin:
* Azure dijital TWINS örneğini yönetme
* Modelleri yönetme
* Dijital TWINS 'yi yönetme
* İkizi ilişkilerini yönetme
* Uç noktaları yapılandırma
* [Yolları](concepts-route-events.md) yönetme
* Azure rol tabanlı erişim denetimi (Azure RBAC) aracılığıyla [güvenliği](concepts-security.md) yapılandırma

Komut kümesi **az DT** olarak adlandırılır ve [Azure CLI için Azure IoT uzantısının](https://github.com/Azure/azure-iot-cli-extension)bir parçasıdır. Komut kümesi için başvuru belgelerinin bir parçası olarak komutların ve kullanımlarınızın tam listesini görebilirsiniz `az iot` : [ *az DT* komut Reference](/cli/azure/ext/azure-iot/dt).

## <a name="uses-deploy-and-validate"></a>Kullanır (Dağıt ve Doğrula)

Genellikle örneğinizi yönetmeye ek olarak, CLı de dağıtım ve doğrulama için yararlı bir araçtır.
* Denetim düzlemi komutları, yeni bir örneğin dağıtımını tekrarlanabilir veya otomatik hale getirmek için kullanılabilir.
* Veri düzlemi komutları, örneğinizin değerlerini hızlıca denetlemek ve işlemlerin beklendiği gibi tamamlandığını doğrulamak için kullanılabilir.

## <a name="get-the-command-set"></a>Komut kümesini al

Azure dijital TWINS komutları Azure [CLI (Azure-IoT) Için Azure IoT uzantısının](https://github.com/Azure/azure-iot-cli-extension)bir parçasıdır. bu nedenle, `azure-iot` **az DT** komutlarıyla en son uzantıya sahip olduğunuzdan emin olmak için aşağıdaki adımları izleyin.

### <a name="cli-version-requirements"></a>CLı sürüm gereksinimleri

Azure CLı 'yı PowerShell ile kullanıyorsanız, Uzantı paketi Azure CLı sürümünüzün **2.3.1** veya üzeri olmasını gerektirir.

Bu CLı komutuyla Azure CLı 'nizin sürümünü denetleyebilirsiniz:
```azurecli
az --version
```

Azure CLı 'yı daha yeni bir sürüme yüklemeye veya güncelleştirmeye ilişkin yönergeler için bkz. [*Azure CLI 'Yı yüklemek*](/cli/azure/install-azure-cli).

### <a name="get-the-extension"></a>Uzantıyı al

Bu adımlarla uzantının en son sürümüne sahip olduğunuzdan emin olabilirsiniz `azure-iot` . Bu komutları [Azure Cloud Shell](../cloud-shell/overview.md) veya [yerel bir Azure CLI](/cli/azure/install-azure-cli)' de çalıştırabilirsiniz.

[!INCLUDE [digital-twins-cloud-shell-extensions.md](../../includes/digital-twins-cloud-shell-extensions.md)]

## <a name="next-steps"></a>Sonraki adımlar

Başvuru belgeleri aracılığıyla CLı ve tam komut kümesini gezin:
* [*az DT* komut başvurusu](/cli/azure/ext/azure-iot/dt)