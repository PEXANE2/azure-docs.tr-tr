---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 06/19/2018
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 202bffb03a73acde67f0d4a03b31aa9a6fbf9824
ms.sourcegitcommit: 8e9a6972196c5a752e9a0d021b715ca3b20a928f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/11/2020
ms.locfileid: "75901626"
---
## <a name="deployment-considerations"></a>Dağıtma konuları

* N serisi VM 'lerin kullanılabilirliği için bkz. [bölgelere göre kullanılabilir ürünler](https://azure.microsoft.com/regions/services/).

* N serisi VM 'Ler yalnızca Kaynak Yöneticisi dağıtım modelinde dağıtılabilir.

* N serisi VM 'Ler, diskleri için destekledikleri Azure Storage türünde farklılık gösterir. NC ve NV VM 'Ler yalnızca standart Disk Depolama (HDD) tarafından desteklenen VM disklerini destekler. NCv2, NCv3, ND, NDv2 ve NVv2 VM 'Ler yalnızca Premium Disk Depolama (SSD) tarafından desteklenen VM disklerini destekler.

* Birkaç tane N serisi VM dağıtmak istiyorsanız, Kullandıkça Öde aboneliğine veya diğer satın alma seçeneklerine göz önünde bulundurun. [Ücretsiz Azure hesabı](https://azure.microsoft.com/free/) kullanıyorsanız, yalnızca sınırlı sayıda Azure işlem çekirdeği kullanabilirsiniz.

* Azure aboneliğinizdeki çekirdek kotasını (bölge başına) artırmanız ve NC, NCv2, NCv3, ND, NDv2, NV veya NVv2 çekirdekleri için ayrı kotayı artırmanız gerekebilir. Kota artışı istemek için, ücretsiz [bir çevrimiçi müşteri destek isteği açın](../articles/azure-portal/supportability/how-to-create-azure-support-request.md) . Varsayılan sınırlar, abonelik kategorime bağlı olarak değişebilir.




