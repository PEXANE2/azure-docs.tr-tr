---
title: include dosyası
description: include dosyası
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: bab282fded7e4b30d2eca6ed51ceaecf22206869
ms.sourcegitcommit: 19a821fc95da830437873d9d8e6626ffc5e0e9d6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/29/2019
ms.locfileid: "70166799"
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nvidia-tesla-cuda-drivers"></a>NVıDıA Tesla (CUDA) sürücüleri

NC, NCv2, NCv3, ND ve NDv2 serisi VM 'Ler için NVıDıA Tesla (CUDA) sürücüleri (NV serisi için isteğe bağlı) yalnızca aşağıdaki tabloda listelenen işletim sistemlerinde desteklenir. Sürücü indirme bağlantıları yayın sırasında geçerli. En son sürücüler için [NVIDIA](https://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Bir Windows Server sanal makinesinde el ile CUDA sürücüsü yüklemeye alternatif olarak Azure [veri bilimi sanal makinesi](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntüsünü dağıtabilirsiniz. Windows Server 2016 için DSVM sürümleri, NVıDıA CUDA sürücülerini, CUDA derin sinir ağ kitaplığını ve diğer araçları önceden yükler.


| OS | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [398,75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (. exe) |
| Windows Server 2012 R2 | [398,75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (. exe) |

### <a name="nvidia-grid-drivers"></a>NVıDıA KıLAVUZ sürücüleri

Microsoft, sanal iş istasyonları veya sanal uygulamalar olarak kullanılan NV ve NVv3 serisi VM 'Ler için NVıDıA GRID sürücü yükleyicilerini yeniden dağıtır. Yalnızca aşağıdaki tabloda listelenen işletim sistemlerinde Azure NV serisi VM 'lerine yalnızca bu KıLAVUZ sürücülerini yükler. Bu sürücüler Azure 'da GRID sanal GPU yazılımı için lisanslama içerir. NVıDıA vGPU yazılım lisans sunucusu ayarlamanız gerekmez.

NVIDIA uzantısının her zaman latst sürücüsünü yükleyeceğini lütfen unutmayın. Daha eski bir sürüme bağımlılığı olan müşteriler için buradaki önceki sürüme bağlantılar sağlıyoruz.

| OS | Sürücü |
| -------- |------------- |
| Windows Server 2019<br/><br/>Windows Server 2016<br/><br/>Windows 10 | [Kılavuz 9,0 (431,02)](https://go.microsoft.com/fwlink/?linkid=874181) (. exe) <br/><br/> [Kılavuz 8,0 (425,31)](https://download.microsoft.com/download/4/8/C/48C2D46E-EB64-460E-A8D9-0F55737D0D68/425.31_grid_win10_server2016_64bit_international.exe) (. exe)  |
| Windows Server 2012 R2<br/><br/>Windows Server 2008 R2<br/><br/>Windows 8<br/><br/>Windows 7 | [Kılavuz 9,0 (431,02)](https://go.microsoft.com/fwlink/?linkid=874184) (. exe)<br/><br/> [Kılavuz 8,0 (425,31)](https://download.microsoft.com/download/6/D/7/6D73C628-B5FB-4243-9520-DAEF363223CB/425.31_grid_win8_win7_server2012R2_server2008R2_64bit_international.exe) (. exe)  |
