---
title: Sanal makineler için Azure Özel Ana Bilgisayarlarına Genel Bakış
description: Azure Özel Ana Bilgisayarların sanal makineleri dağıtmak için nasıl kullanılabileceğini öğrenin.
author: cynthn
ms.service: virtual-machines-windows
ms.topic: article
ms.workload: infrastructure
ms.date: 01/09/2020
ms.author: cynthn
ms.reviewer: zivr
ms.openlocfilehash: 9709408f006cbcc5786a4e5a854abcd20affd693
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82082841"
---
# <a name="azure-dedicated-hosts"></a>Azure Özel Ana Bilgisayarlar

Azure Özel Ana Bilgisayar, bir veya daha fazla sanal makineyi barındırabilen fiziksel sunucular sağlayan bir hizmettir. Özel ana bilgisayarlar, kaynak olarak sağlanan veri merkezlerimizde kullanılan fiziksel sunucuların aynıdır. Bir bölge, kullanılabilirlik bölgesi ve hata etki alanı içinde özel ana bilgisayarları sağlayabilirsiniz. Ardından, gereksinimlerinizi en iyi karşılayan yapılandırmada VM'leri doğrudan sağlanan ana bilgisayarlarınıza yerleştirebilirsiniz.


[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell,](dedicated-hosts-powershell.md) [portal](dedicated-hosts-portal.md)ve [Azure CLI'yi](../linux/dedicated-hosts-cli.md)kullanarak özel bir ana bilgisayar dağıtabilirsiniz.

- Bir bölgede maksimum esneklik için hem bölgeleri hem de fay etki alanlarını kullanan örnek şablon [burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)bulunur.

- [Ayrıca, Ayrılmış](../prepay-dedicated-hosts-reserved-instances.md)Azure Özel Ana Bilgisayar Ları Örneği ile maliyetlerden de tasarruf edebilirsiniz.
