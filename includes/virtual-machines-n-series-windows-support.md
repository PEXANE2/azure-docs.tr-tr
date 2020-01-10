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
ms.openlocfilehash: ab68fc6533be5e3241de2e49652251fea5fe2f7d
ms.sourcegitcommit: 5b073caafebaf80dc1774b66483136ac342f7808
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75780669"
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nvidia-tesla-cuda-drivers"></a>NVıDıA Tesla (CUDA) sürücüleri

NC, NCv2, NCv3, ND ve NDv2 serisi VM 'Ler için NVıDıA Tesla (CUDA) sürücüleri (NV serisi için isteğe bağlı) yalnızca aşağıdaki tabloda listelenen işletim sistemlerinde desteklenir. Sürücü indirme bağlantıları yayın sırasında geçerli. En son sürücüler için [NVIDIA](https://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Bir Windows Server sanal makinesinde el ile CUDA sürücüsü yüklemeye alternatif olarak Azure [veri bilimi sanal makinesi](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntüsünü dağıtabilirsiniz. Windows Server 2016 için DSVM sürümleri, NVıDıA CUDA sürücülerini, CUDA derin sinir ağ kitaplığını ve diğer araçları önceden yükler.


| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2016 | [398,75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2016-international.exe) (. exe) |
| Windows Server 2012 R2 | [398,75](https://us.download.nvidia.com/Windows/Quadro_Certified/398.75/398.75-tesla-desktop-winserver2008-2012r2-64bit-international.exe) (. exe) |

### <a name="nvidia-grid-drivers"></a>NVıDıA KıLAVUZ sürücüleri

Microsoft, sanal iş istasyonları veya sanal uygulamalar olarak kullanılan NV ve NVv3 serisi VM 'Ler için NVıDıA GRID sürücü yükleyicilerini yeniden dağıtır. Yalnızca aşağıdaki tabloda listelenen işletim sistemlerinde Azure NV serisi VM 'lerine yalnızca bu KıLAVUZ sürücülerini yükler. Bu sürücüler Azure 'da GRID sanal GPU yazılımı için lisanslama içerir. NVıDıA vGPU yazılım lisans sunucusu ayarlamanız gerekmez.

NVIDIA uzantısının en son sürücüyü her zaman yükleyeceğini lütfen unutmayın. Daha eski bir sürüme bağımlılığı olan müşteriler için buradaki önceki sürüme bağlantılar sağlıyoruz.

Windows Server 2019, Windows Server 2016 ve Windows 10 için (derleme 1909 ' ye kadar):
- [Kılavuz 10,0 (441,66)](https://go.microsoft.com/fwlink/?linkid=874181) (. exe)
- [Kılavuz 9,0 (431,02)](https://download.microsoft.com/download/8/C/C/8CC88D54-EB07-44D3-8FA9-B797B173ED04/431.02_grid_win10_server2016_server2019_64bit_international.exe) (. exe) 

Windows Server 2012 R2, Windows Server 2008 R2, Windows 8 ve Windows 7 için: 
- [Kılavuz 10,0 (441,66)](https://go.microsoft.com/fwlink/?linkid=874184) (. exe)
- [Kılavuz 9,1 (431,79)](https://download.microsoft.com/download/8/6/e/86ef2daa-b31e-43ad-90f2-bd795384b71e/431.79_grid_win7_win8_server2008R2_server2012R2_64bit_international.exe) (. exe)  
