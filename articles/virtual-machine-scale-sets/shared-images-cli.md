---
title: Azure 'da ölçek kümesi oluşturmak için paylaşılan VM görüntülerini kullanma
description: Azure 'da sanal makine ölçek kümeleri dağıtmak için kullanılacak paylaşılan VM görüntülerini oluşturmak üzere Azure CLı 'yı nasıl kullanacağınızı öğrenin.
author: axayjo
tags: azure-resource-manager
ms.service: virtual-machine-scale-sets
ms.subservice: imaging
ms.topic: conceptual
ms.date: 05/06/2019
ms.author: akjosh
ms.reviewer: cynthn
ms.custom: ''
ms.openlocfilehash: 8d129ae2c6048e592bfc43f07ee609ce930aa3fd
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84558783"
---
# <a name="create-and-use-shared-images-for-virtual-machine-scale-sets-with-the-azure-cli-20"></a>Azure CLı 2,0 ile sanal makine ölçek kümeleri için paylaşılan görüntüler oluşturma ve kullanma

Ölçek kümesi oluşturduğunuzda, sanal makine örnekleri dağıtılırken kullanılacak bir görüntü belirtirsiniz. [Paylaşılan görüntü Galerisi](shared-image-galleries.md) , kuruluşunuz genelinde özel görüntü paylaşımını basitleştirir. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. 

Paylaşılan görüntü Galerisi, görüntülerinizi başkalarıyla paylaşmanıza olanak sağlar. Hangi görüntüleri paylaşmak istediğinizi, içinde hangi bölgelerin kullanılabilir olmasını istediğinizi ve bunları ile paylaşmak istediğinizi seçin. 


[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]


## <a name="next-steps"></a>Sonraki adımlar

Bir [VM](../virtual-machines/image-version-vm-cli.md)'den veya [yönetilen bir görüntüden](../virtual-machines/image-version-managed-image-cli.md)görüntü sürümü oluşturun.

Paylaşılan görüntü galerileri hakkında daha fazla bilgi için bkz. [genel bakış](shared-image-galleries.md). Sorunlarla karşılaşırsanız bkz. [paylaşılan görüntü galerilerine sorun giderme](troubleshooting-shared-images.md).
