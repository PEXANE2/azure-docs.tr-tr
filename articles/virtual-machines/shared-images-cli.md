---
title: Azure CLı ile paylaşılan görüntü galerileri oluşturma
description: Bu makalede Azure CLı 'yı kullanarak Azure 'da bir sanal makinenin paylaşılan görüntüsünü oluşturma hakkında bilgi edineceksiniz.
author: cynthn
ms.service: virtual-machines
ms.subservice: imaging
ms.topic: how-to
ms.workload: infrastructure
ms.date: 05/04/2020
ms.author: cynthn
ms.reviewer: akjosh
ms.openlocfilehash: d64b5f94dae7ad0213fa231b5603064ad3647da1
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82796700"
---
# <a name="create-a-shared-image-gallery-with-the-azure-cli"></a>Azure CLı ile paylaşılan görüntü galerisi oluşturma

[Paylaşılan görüntü Galerisi](./linux/shared-image-galleries.md) , kuruluşunuz genelinde özel görüntü paylaşımını basitleştirir. Özel görüntüler market görüntüleri gibidir, ancak bunları kendiniz oluşturursunuz. Özel görüntüler, uygulamaları, uygulama yapılandırmalarını ve diğer işletim sistemi yapılandırmalarını önceden yükleme gibi yapılandırmaları önyüklemek için kullanılabilir. 

Paylaşılan görüntü Galerisi, özel VM görüntülerinizi başkalarıyla paylaşmanıza olanak sağlar. Hangi görüntüleri paylaşmak istediğinizi, içinde hangi bölgelerin kullanılabilir olmasını istediğinizi ve bunları ile paylaşmak istediğinizi seçin. 

[!INCLUDE [virtual-machines-common-shared-images-cli](../../includes/virtual-machines-common-shared-images-cli.md)]


## <a name="next-steps"></a>Sonraki adımlar

Azure CLı kullanarak bir [VM](image-version-vm-cli.md)'den veya [yönetilen bir görüntüden](image-version-managed-image-cli.md) görüntü sürümü oluşturun.

Paylaşılan görüntü galerileri hakkında daha fazla bilgi için bkz. [genel bakış](./linux/shared-image-galleries.md). Sorunlarla karşılaşırsanız bkz. [paylaşılan görüntü galerilerine sorun giderme](troubleshooting-shared-images.md).
