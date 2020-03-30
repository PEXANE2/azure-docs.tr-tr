---
title: include dosyası
description: include dosyası
services: virtual-machines-linux
author: cynthn
ms.service: virtual-machines-linux
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: 5dccdb6c357635e78b076b1560bf6c0c62c03753
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77135077"
---
## <a name="supported-distributions-and-drivers"></a>Desteklenen dağıtımlar ve sürücüler

### <a name="nvidia-cuda-drivers"></a>NVIDIA CUDA sürücüleri

NC, NCv2, NCv3, ND ve NDv2 serisi VM'ler (NV serisi için isteğe bağlı) için NVIDIA CUDA sürücüleri yalnızca aşağıdaki tabloda listelenen Linux dağıtımlarında desteklenir. CUDA sürücü bilgileri yayın sırasında geçerlidir. En son CUDA sürücüleri için [NVIDIA](https://developer.nvidia.com/cuda-zone) web sitesini ziyaret edin. Dağıtımınız için en son CUDA sürücülerini yüklediğinizden veya yükselttiğinizi zedebilirsiniz. 

> [!TIP]
> Linux VM'de manuel CUDA sürücü yüklemesine alternatif olarak, Bir Azure [Veri Bilimi Sanal Makine](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntüsünü dağıtabilirsiniz. Ubuntu 16.04 LTS veya CentOS 7.4 ön yükleme NVIDIA CUDA sürücüleri, CUDA Derin Sinir Ağı Kütüphanesi ve diğer araçlar için DSVM sürümleri.

| Dağıtım | Sürücü |
| --- | -- | 
| Ubuntu 16.04 LTS, 18.04 LTS<br/><br/> Red Hat Enterprise Linux 7.3, 7.4, 7.5, 7.6<br/><br/> CentOS tabanlı 7.3, 7.4, 7.5, 7.6, CentOS tabanlı 7.4 HPC | NVIDIA CUDA 10.1, sürücü şubesi R418 |

### <a name="nvidia-grid-drivers"></a>NVIDIA GRID sürücüleri

Microsoft, sanal iş istasyonları olarak kullanılan NV ve NVv3 serisi VM'ler veya sanal uygulamalar için NVIDIA GRID sürücü yükleyicilerini yeniden dağıtır. Yalnızca aşağıdaki tabloda listelenen işletim sistemlerine yalnızca bu GRID sürücülerini Azure NV VM'lerine yükleyin. Bu sürücüler, Azure'daki GRID Virtual GPU Yazılımı için lisanslamayı içerir. Bir NVIDIA vGPU yazılım lisans sunucusu kurmanız gerekmez.

| Dağıtım | Sürücü |
| --- | -- |
|Ubuntu 18.04 LTS<br/><br/>Ubuntu 16.04 LTS<br/><br/>Red Hat Enterprise Linux 7.0 ile 7.6<br/><br/>CentOS tabanlı 7.0 ile 7.6 arası<br/><br/>SUSE Linux Kurumsal Sunucu 12 SP2 | NVIDIA GRID 10.1, sürücü kolu R440|

> [!WARNING] 
> Red Hat ürünlerine üçüncü taraf yazılım yüklenmesi Red Hat destek koşullarını etkileyebilir. Bkz. [Red Hat Bilgi Bankası makalesi](https://access.redhat.com/articles/1067).
>
