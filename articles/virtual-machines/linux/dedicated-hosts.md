---
title: Sanal makineler için Azure Özel Ana Bilgisayarlarına Genel Bakış
description: Azure Özel Ana Bilgisayarların sanal makineleri dağıtmak için nasıl kullanılabileceğini öğrenin.
author: cynthn
ms.service: virtual-machines
ms.topic: article
ms.date: 01/09/2020
ms.author: cynthn
ms.openlocfilehash: c19b3410e106aaf5fed53aba45d06eac6dd9d601
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78970737"
---
# <a name="azure-dedicated-hosts"></a>Azure Özel Ana Bilgisayarlar

Azure Özel Ana Bilgisayar, bir veya daha fazla sanal makineyi barındırabilen fiziksel sunucular sağlayan bir hizmettir. Özel ana bilgisayarlar, kaynak olarak sağlanan veri merkezlerimizde kullanılan fiziksel sunucuların aynıdır. Bir bölge, kullanılabilirlik bölgesi ve hata etki alanı içinde özel ana bilgisayarları sağlayabilirsiniz. Ardından, gereksinimlerinizi en iyi karşılayan yapılandırmada VM'leri doğrudan sağlanan ana bilgisayarlarınıza yerleştirebilirsiniz.



[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]


## <a name="next-steps"></a>Sonraki adımlar

- [Azure CLI](dedicated-hosts-cli.md), [portal](dedicated-hosts-portal.md)ve [PowerShell'i](../windows/dedicated-hosts-powershell.md)kullanarak özel bir ana bilgisayar dağıtabilirsiniz.

- Daha fazla bilgi [için, Özel ana bilgisayarlarına](dedicated-hosts.md) genel bakış alabakın.

- Bir bölgede maksimum esneklik için hem bölgeleri hem de fay etki alanlarını kullanan örnek şablon [burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md)bulunur.

- [Ayrıca, Ayrılmış](../prepay-dedicated-hosts-reserved-instances.md)Azure Özel Ana Bilgisayar Ları Örneği ile maliyetlerden de tasarruf edebilirsiniz.
