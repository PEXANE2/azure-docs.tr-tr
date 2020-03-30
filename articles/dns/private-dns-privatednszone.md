---
title: Azure DNS özel bölgesi nedir
description: Özel bir DNS bölgesine genel bakış
services: dns
author: rohinkoul
ms.service: dns
ms.topic: article
ms.date: 9/24/2019
ms.author: rohink
ms.openlocfilehash: a951bc07c4a8ed42b1c116332d13674656bbaafd
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75646804"
---
# <a name="what-is-a-private-azure-dns-zone"></a>Özel Azure DNS bölgesi nedir

Azure Private DNS, özel bir DNS çözümü eklemeye gerek kalmadan sanal ağdaki alan adlarını yönetmek ve çözmek için güvenilir ve güvenli bir DNS hizmeti sağlar. Özel DNS bölgelerini kullanarak, bugün kullanılabilen Azure tarafından sağlanan adlar yerine kendi özel alan adlarınızı kullanabilirsiniz. 

Özel bir DNS bölgesinde bulunan kayıtlar Internet'ten çözülemez. Özel bir DNS bölgesine karşı DNS çözünürlüğü yalnızca ona bağlı sanal ağlardan çalışır.

[Sanal ağ bağlantıları](./private-dns-virtual-network-links.md)oluşturarak özel bir DNS bölgesini bir veya daha fazla sanal ağa bağlayabilirsiniz.
Sanal ağda dağıtılan sanal makineler için DNS kayıtlarının yaşam döngüsünü otomatik olarak yönetmek için [otomatik kayıt](./private-dns-autoregistration.md) özelliğini de etkinleştirebilirsiniz.

## <a name="limits"></a>Sınırlar

Bir abonelikte kaç özel DNS bölgesi oluşturabileceğinizi ve özel bir DNS bölgesinde kaç kayıt kümesinin desteklenebileceğini anlamak için [Azure DNS sınırlarını](https://docs.microsoft.com/azure/azure-resource-manager/management/azure-subscription-service-limits#azure-dns-limits) görün

## <a name="restrictions"></a>Kısıtlamalar

* Tek etiketli özel DNS bölgeleri desteklenmez. Özel DNS bölgenizde iki veya daha fazla etiket olmalıdır. Örneğin contoso.com bir nokta ile ayrılmış iki etiketi vardır. Özel bir DNS bölgesi en fazla 34 etikete sahip olabilir.
* Özel bir DNS bölgesinde bölge delegasyonları (NS kayıtları) oluşturamazsınız. Bir alt etki alanı kullanmak istiyorsanız, etki alanını doğrudan özel bir DNS bölgesi olarak oluşturabilir ve üst bölgeden bir ad sunucusu delegasyonu ayarlamadan sanal ağa bağlayabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure PowerShell](./private-dns-getstarted-powershell.md) veya [Azure CLI'yi](./private-dns-getstarted-cli.md)kullanarak Azure DNS'de nasıl özel bir bölge oluşturabilirsiniz öğrenin.

* Azure DNS'deki özel bölgelerle gerçekleştirilebilecek bazı yaygın [özel bölge senaryoları](./private-dns-scenarios.md) hakkında bilgi edinin.

* Belirli işlem türleri için bekleyebileceğiniz belirli davranışlar da dahil olmak üzere Azure DNS'deki özel bölgelerle ilgili sık sorulan sorular ve yanıtlar için [Bkz.](./dns-faq-private.md)
