---
title: Azure Linux sanal makine nasıl etiketilir?
description: Kaynak Yöneticisi dağıtım modelini kullanarak Azure'da oluşturulan bir Azure Linux sanal makinesini etiketleme hakkında bilgi edinin.
services: virtual-machines-linux
documentationcenter: ''
author: mmccrory
manager: gwallace
tags: azure-resource-manager
ms.assetid: ca0e17e5-d78e-42e6-9dad-c1e8f1c58027
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure-services
ms.date: 02/28/2017
ms.author: memccror
ms.openlocfilehash: fd4a93f4c0b2f052fe5c9890bee01e5da0dcead2
ms.sourcegitcommit: b55d7c87dc645d8e5eb1e8f05f5afa38d7574846
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81460962"
---
# <a name="how-to-tag-a-linux-virtual-machine-in-azure"></a>Azure'da Bir Linux sanal makine nasıl etiketilir?
Bu makalede, Kaynak Yöneticisi dağıtım modeli aracılığıyla Azure'daki bir Linux sanal makinesini etiketlemenin farklı yolları açıklanmaktadır. Etiketler, doğrudan bir kaynağa veya kaynak grubuna yerleştirilebilen kullanıcı tanımlı anahtar/değer çiftleridir. Azure şu anda kaynak ve kaynak grubu başına en fazla 50 etiketi destekler. Etiketler, oluşturulduğu sırada bir kaynağa yerleştirilebilir veya varolan bir kaynağa eklenebilir. Etiketlerin yalnızca Kaynak Yöneticisi dağıtım modeli aracılığıyla oluşturulan kaynaklar için desteklenir.

[!INCLUDE [virtual-machines-common-tag](../../../includes/virtual-machines-common-tag.md)]

## <a name="tagging-with-azure-cli"></a>Azure CLI ile etiketleme

Başlamak için, en son [Azure CLI'nin](/cli/azure/install-azure-cli) [az girişi](/cli/azure/reference-index#az-login)kullanarak bir Azure hesabına yüklenmesi ve oturum açması gerekir.

Bu komutu kullanarak etiketler de dahil olmak üzere belirli bir Sanal Makinenin tüm özelliklerini görüntüleyebilirsiniz:

```azurecli
az vm show --resource-group MyResourceGroup --name MyTestVM
```

Azure CLI üzerinden yeni bir VM etiketi eklemek `azure vm update` için, komutu etiket parametresi **-set:**

```azurecli
az vm update \
    --resource-group MyResourceGroup \
    --name MyTestVM \
    --set tags.myNewTagName1=myNewTagValue1 tags.myNewTagName2=myNewTagValue2
```

Etiketleri kaldırmak için, komuttaki **--kaldır** `azure vm update` parametresini kullanabilirsiniz.

```azurecli
az vm update --resource-group MyResourceGroup --name MyTestVM --remove tags.myNewTagName1
```

Artık kaynaklarımıza etiketler uyguladığımıza göre Azure CLI ve Portal, fatura portalındaki etiketleri görmek için kullanım ayrıntılarına bir göz atalım.

[!INCLUDE [virtual-machines-common-tag-usage](../../../includes/virtual-machines-common-tag-usage.md)]

## <a name="next-steps"></a>Sonraki adımlar
* Azure kaynaklarınızı etiketleme hakkında daha fazla bilgi edinmek için Azure Kaynakları Yöneticinize [Genel Bakış][Azure Resource Manager Overview] ve [Azure Kaynaklarınızı düzenlemek için Etiketleri Kullanma'ya][Using Tags to organize your Azure Resources]bakın.
* Etiketlerin Azure kaynaklarını kullanımınızı yönetmenize nasıl yardımcı olabileceğini görmek için Azure [Faturanızı Anlama][Understanding your Azure Bill] ve [Microsoft Azure kaynak tüketiminize ilişkin öngörüler elde][Gain insights into your Microsoft Azure resource consumption]etme bölümüne bakın.

[Azure CLI environment]: ../../azure-resource-manager/xplat-cli-azure-resource-manager.md
[Azure Resource Manager Overview]: ../../azure-resource-manager/management/overview.md
[Using Tags to organize your Azure Resources]: ../../azure-resource-manager/management/tag-resources.md
[Understanding your Azure Bill]:../../cost-management-billing/understand/review-individual-bill.md
[Gain insights into your Microsoft Azure resource consumption]:../../cost-management-billing/manage/usage-rate-card-overview.md
