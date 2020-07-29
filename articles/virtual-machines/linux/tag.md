---
title: Azure Linux sanal makinesini etiketleme
description: Kaynak Yöneticisi dağıtım modelini kullanarak Azure 'da oluşturulan bir Azure Linux sanal makinesini etiketleme hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: gwallace
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.topic: how-to
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: bddb45be9232ff33629a4df0a5976e7b3c2e30b0
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87369926"
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Azure 'da Linux sanal makinesini etiketleme
Bu makalede, Azure 'daki bir Linux sanal makinesini Kaynak Yöneticisi dağıtım modeliyle etiketlemek için farklı yollar açıklanmaktadır. Etiketler, doğrudan bir kaynağa veya bir kaynak grubuna yerleştirilebilecek Kullanıcı tanımlı anahtar/değer çiftleridir. Azure Şu anda kaynak ve kaynak grubu başına en fazla 50 etiketi desteklemektedir. Etiketler, oluşturma sırasında veya var olan bir kaynağa eklenen bir kaynağa yerleştirilebilir. Lütfen, Etiketler yalnızca Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan kaynaklar için desteklenir.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Azure CLı ile etiketleme

Başlamak için en son [Azure CLI](/cli/azure/install-azure-cli) 'nın yüklü olması ve [az oturum açma](/cli/azure/reference-index#az-login)kullanarak bir Azure hesabında oturum açmış olması gerekir.

Bu komutu kullanarak belirli bir sanal makine için Etiketler dahil olmak üzere tüm özellikleri görüntüleyebilirsiniz:

```azurecli
az vm show --resource-group MyResourceGroup --name MyTestVM
```

Azure CLı aracılığıyla yeni bir VM etiketi eklemek için `azure vm update` komutunu, **--set**etiketi parametresiyle birlikte kullanabilirsiniz:

```azurecli
az vm update \
    --resource-group MyResourceGroup \
    --name MyTestVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

Etiketleri kaldırmak için, komutuyla **--Remove** parametresini kullanabilirsiniz `azure vm update` .

```azurecli
az vm update --resource-group MyResourceGroup --name MyTestVM --remove tags.myNewTagName1
```

Kaynaklarımıza Azure CLı ve Portal için Etiketler uyguladığımıza göre, şimdi de fatura portalındaki etiketleri görmek için kullanım ayrıntılarına göz atalım.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızın etiketlenmesi hakkında daha fazla bilgi edinmek için bkz. [Azure Resource Manager genel bakış][Azure Resource Manager Overview] ve [etiketleri kullanarak Azure kaynaklarınızı düzenleme][Using Tags to organize your Azure Resources].
* Etiketlerin Azure kaynaklarını kullanımınızı yönetmenize nasıl yardımcı olduğunu görmek için bkz. [Azure Faturanızı Anlama][Understanding your Azure Bill] ve [Microsoft Azure Kaynak tüketiminiz hakkında öngörüler elde][Gain insights into your Microsoft Azure resource consumption]etme.

[Azure CLI environment]: ../../azure-resource-manager/management/manage-resources-cli.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/management/overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/management/tag-resources.md
[Understanding your Azure Bill]:../../cost-management-billing/understand/review-individual-bill.md
[Gain insights into your Microsoft Azure resource consumption]:../../cost-management-billing/manage/usage-rate-card-overview.md
