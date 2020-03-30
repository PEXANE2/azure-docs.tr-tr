---
title: Sanal makineler için Azure Özel Ana Bilgisayarlarına Genel Bakış
description: Azure Özel Ana Bilgisayarların sanal makineleri dağıtmak için nasıl kullanılabileceğini öğrenin.
services: virtual-machines-windows
author: cynthn
manager: gwallace
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 01/09/2020
ms.author: cynthn
ms.openlocfilehash: 39d1c0ddb4961800e889346ec110ca629ae73546
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78251563"
---
# <a name="azure-dedicated-hosts"></a>Azure Özel Ana Bilgisayarlar

Azure Özel Ana Bilgisayar, bir veya daha fazla sanal makineyi barındırabilen fiziksel sunucular sağlayan bir hizmettir. Özel ana bilgisayarlar, kaynak olarak sağlanan veri merkezlerimizde kullanılan fiziksel sunucuların aynıdır. Bir bölge, kullanılabilirlik bölgesi ve hata etki alanı içinde özel ana bilgisayarları sağlayabilirsiniz. Ardından, gereksinimlerinizi en iyi karşılayan yapılandırmada VM'leri doğrudan sağlanan ana bilgisayarlarınıza yerleştirebilirsiniz.


[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell,](dedicated-hosts-powershell.md) [portal](dedicated-hosts-portal.md)ve [Azure CLI'yi](../linux/dedicated-hosts-cli.md)kullanarak özel bir ana bilgisayar dağıtabilirsiniz.

- Bir bölgede maksimum esneklik için hem bölgeleri hem de fay etki alanlarını kullanan örnek şablon [burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)bulunur.

- [Ayrıca, Ayrılmış](../prepay-dedicated-hosts-reserved-instances.md)Azure Özel Ana Bilgisayar Ları Örneği ile maliyetlerden de tasarruf edebilirsiniz.
