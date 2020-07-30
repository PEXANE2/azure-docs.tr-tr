---
title: Hizmet kotaları ve bölge kullanılabilirliği
description: Azure Container Instances hizmetinin kotaları, sınırları ve bölge kullanılabilirliği.
ms.topic: article
ms.date: 07/22/2020
ms.openlocfilehash: eaaa8e0b2d72aaea546a1bc351da40932c1deb14
ms.sourcegitcommit: 5b8fb60a5ded05c5b7281094d18cf8ae15cb1d55
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87384846"
---
# <a name="quotas-and-limits-for-azure-container-instances"></a>Azure Container Instances için kotalar ve sınırlar

Tüm Azure hizmetleri, kaynak ve özelliklere yönelik bazı varsayılan limitler ve kotalar içerir. Bu makalede, Azure Container Instances için varsayılan kotalar ve sınırlar ayrıntılı olarak açıklanır.

Azure Container Instances için işlem, bellek ve depolama kaynaklarının kullanılabilirliği bölgeye ve işletim sistemine göre değişir. Ayrıntılar için bkz. [Azure Container Instances Için kaynak kullanılabilirliği](container-instances-region-availability.md).

Abonelik için bir bölgedeki geçerli kota kullanımını gözden geçirmek için [liste kullanımı](/rest/api/container-instances/location/listusage) API 'sini kullanın.

## <a name="service-quotas-and-limits"></a>Hizmet kotaları ve limitleri

[!INCLUDE [container-instances-limits](../../includes/container-instances-limits.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bazı varsayılan limitler ve kotalar artırılabilir. Artırmayı destekleyen bir veya daha fazla kaynak için artış istemek üzere lütfen bir [Azure destek isteği][azure-support] gönderin (**Sorun türü** olarak "Kota" öğesini seçin).

<!-- LINKS - External -->
[azure-support]: https://ms.portal.azure.com/#blade/Microsoft_Azure_Support/HelpAndSupportBlade/newsupportrequest
