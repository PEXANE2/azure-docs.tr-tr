---
title: Azure CLı kullanarak bir eşleme hizmeti önizleme bağlantısı kaydetme
description: Azure CLı kullanarak bir eşleme hizmeti bağlantısının nasıl kaydedileceği hakkında bilgi edinin
services: peering-service
author: derekolo
ms.service: peering-service
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: Infrastructure-services
ms.date: 05/2/2020
ms.author: derekol
ms.openlocfilehash: fbb9a6237aafe1c1b7c94d9dd23ad4c3665c0c73
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84872135"
---
# <a name="register-a-peering-service-connection-by-using-the-azure-cli"></a>Azure CLı kullanarak bir eşleme hizmeti bağlantısı kaydetme

Azure eşleme hizmeti, Office 365, Dynamics 365, hizmet olarak yazılım (SaaS) Hizmetleri, Azure veya genel İnternet üzerinden erişilebilen herhangi bir Microsoft hizmeti gibi Microsoft bulut hizmetleriyle müşteri bağlantısını geliştiren bir ağ hizmetidir. Bu makalede, Azure CLı kullanarak bir eşleme hizmeti bağlantısının nasıl kaydedileceği hakkında bilgi edineceksiniz.

Azure aboneliğiniz yoksa, şimdi bir [Hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

CLı 'yi yerel olarak yükleyip kullanmayı tercih ederseniz bu makale, Azure CLı sürüm 2.0.28 veya üstünü gerektirir. Sürümü bulmak için `az --version` komutunu çalıştırın. Yükleme veya yükseltme yapmanız gerekirse bkz. [Azure CLI’yı yükleme](/cli/azure/install-azure-cli).

## <a name="prerequisites"></a>Ön koşullar 

Aşağıdakilere sahip olmanız gerekir:

### <a name="azure-account"></a>Azure hesabı

Geçerli ve etkin bir Microsoft Azure hesabınızın olması gerekir. Eşleme hizmeti bağlantısını kurmak için bu hesap gereklidir. Eşleme hizmeti, Azure aboneliklerindeki bir kaynaktır.

### <a name="connectivity-provider"></a>Bağlantı sağlayıcısı

Ağınızı Microsoft Network 'e bağlamak için eşleme hizmeti edinmek üzere bir Internet hizmeti sağlayıcısı veya Internet Exchange ortağıyla birlikte çalışabilirsiniz.

Bağlantı sağlayıcılarının Microsoft ile iş ortaklığına sahip olduğundan emin olun.

### <a name="1-sign-in-to-your-azure-account-and-select-your-subscription"></a>1. Azure hesabınızda oturum açın ve aboneliğinizi seçin

Yapılandırmanızı başlatmak için Azure hesabınızda oturum açın. Cloud Shell **deneyin** seçeneğini kullanırsanız, otomatik olarak oturum açtınız. Bağlanmanıza yardımcı olması için aşağıdaki örnekleri kullanın.

```azurecli-interactive
az login
```

Hesapla ilişkili abonelikleri kontrol edin.

```azurecli-interactive
az account list
```

Eşleme hizmeti bağlantısını kaydetmek istediğiniz aboneliği seçin.

```azurecli-interactive
az account set --subscription "<subscription ID>"
```

Henüz bir kaynak grubunuz yoksa, eşleme hizmeti bağlantınızı kaydetmeden önce bir tane oluşturmanız gerekir. Aşağıdaki komutu çalıştırarak bir kaynak grubu oluşturabilirsiniz:

```azurecli-interactive
az group create -n MyResourceGroup -l "West US"
```

### <a name="2-register-your-subscription-with-the-resource-provider-and-feature-flag"></a>2. aboneliğinizi kaynak sağlayıcısı ve özellik bayrağıyla kaydedin

Azure CLı kullanarak eşleme hizmeti bağlantısını kaydetme adımlarına devam etmeden önce, Azure CLı kullanarak aboneliğinizi kaynak sağlayıcısı ve özellik bayrağıyla kaydedin. Azure CLı komutları burada belirtilmiştir:

```azurecli-interactive

az feature register --namespace Microsoft.Peering --name AllowPeeringService

```

### <a name="3-register-the-peering-service-connection"></a>3. eşleme hizmeti bağlantısını kaydedin

Azure CLı aracılığıyla aşağıdaki komut kümesini kullanarak eşleme hizmeti bağlantısını kaydedin. Bu örnek, Mtypeınfo Eringservice adlı eşleme hizmeti bağlantısını kaydeder.

```azurecli-interactive
az peering service create : Create peering service\
  --location -l \
  --name myPeeringService\
  --resource-group -g MyResourceGroup\
  --peering-service-location\
  --peering-service-provider\
  --tags
```

### <a name="4-register-the-prefix"></a>4. öneki kaydetme

Azure CLı aracılığıyla aşağıdaki komutları yürüterek bağlantı sağlayıcısı tarafından sunulan ön eki kaydettirin. Bu örnek Myprefıx adlı öneki kaydeder.

```azurecli-interactive
az peering service prefix create \
  --name  myPrefix\
  --peering-service-name myPeeringService\
  --resource-group  -g myResourceGroup\
```

## <a name="next-steps"></a>Sonraki adımlar

- Eşleme hizmeti bağlantısı hakkında daha fazla bilgi edinmek için bkz. [eşleme hizmeti bağlantısı](connection.md).
- Eşleme hizmeti bağlantı telemetrisi hakkında bilgi edinmek için bkz. [eşleme hizmeti bağlantı telemetrisi](connection-telemetry.md).
- Telemetriyi ölçmek için bkz. [ölçüm bağlantı telemetrisi](measure-connection-telemetry.md).
- Azure PowerShell kullanarak bağlantıyı kaydetmek için bkz. [eşleme hizmeti bağlantısını kaydetme-Azure PowerShell](powershell.md).
- Azure portal kullanarak bağlantıyı kaydetmek için bkz. [eşleme hizmeti bağlantısı kaydetme-Azure Portal](azure-portal.md).
