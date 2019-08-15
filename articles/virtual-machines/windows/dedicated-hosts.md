---
title: Sanal makineler için Azure ayrılmış konaklarına genel bakış | Microsoft Docs
description: Azure adanmış ana bilgisayarlarının sanal makineleri dağıtmak için nasıl kullanılabileceği hakkında daha fazla bilgi edinin.
services: virtual-machines-windows
author: cynthn
manager: gwallace
editor: tysonn
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 07/25/2019
ms.author: cynthn
ms.openlocfilehash: 011bfeb337f3c04b2d9041abedac50affe1f86b0
ms.sourcegitcommit: 62bd5acd62418518d5991b73a16dca61d7430634
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2019
ms.locfileid: "68977255"
---
# <a name="preview-azure-dedicated-hosts"></a>Önizleme: Azure ayrılmış Konakları

Azure adanmış ana bilgisayar, bir veya daha fazla sanal makineyi bir Azure aboneliğine ayrılmış olarak barındırabilecek fiziksel sunucular sağlayan bir hizmettir. Adanmış konaklar, kaynak olarak sağlanmış olan veri merkezlerimizde kullanılan fiziksel sunuculardır. Bir bölge, kullanılabilirlik alanı ve hata etki alanı içinde adanmış konaklar sağlayabilirsiniz. Daha sonra, gereksinimlerinizi en iyi şekilde karşılayan her yapılandırma için VM 'Leri doğrudan sağlanan konaklarınıza yerleştirebilirsiniz.

[!INCLUDE [virtual-machines-common-dedicated-hosts-preview](../../../includes/virtual-machines-common-dedicated-hosts-preview.md)]

[!INCLUDE [virtual-machines-common-dedicated-hosts](../../../includes/virtual-machines-common-dedicated-hosts.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Azure PowerShell](dedicated-hosts-powershell.md), [Portal](dedicated-hosts-portal.md)ve [Azure CLI](../linux/dedicated-hosts-cli.md)kullanarak adanmış bir konak dağıtabilirsiniz.

- [Burada](https://github.com/Azure/azure-quickstart-templates/blob/master/201-vm-dedicated-hosts/README.md), bir bölgedeki maksimum dayanıklılık için hem bölge hem de hata etki alanı kullanan örnek şablon vardır.
