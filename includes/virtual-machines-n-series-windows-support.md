---
title: dosya dahil etme
description: dosya dahil etme
services: virtual-machines-windows
author: cynthn
ms.service: virtual-machines-windows
ms.topic: include
ms.date: 02/11/2019
ms.author: cynthn
ms.custom: include file
ms.openlocfilehash: bfb7d1d52549d7fda9547b65a259fe2ce73f8839
ms.sourcegitcommit: 83610f637914f09d2a87b98ae7a6ae92122a02f1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2020
ms.locfileid: "91997883"
---
## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

### <a name="nvidia-tesla-cuda-drivers"></a>NVıDıA Tesla (CUDA) sürücüleri

NC, NCv2, NCv3, NCasT4_v3, ND ve NDv2 serisi VM 'Ler için NVıDıA Tesla (CUDA) sürücüleri (NV serisi için isteğe bağlı) yalnızca aşağıdaki tabloda listelenen işletim sistemlerinde desteklenir. Sürücü indirme bağlantıları yayın sırasında geçerli. En son sürücüler için [NVIDIA](https://www.nvidia.com/) web sitesini ziyaret edin.

> [!TIP]
> Bir Windows Server sanal makinesinde el ile CUDA sürücüsü yüklemeye alternatif olarak Azure [veri bilimi sanal makinesi](../articles/machine-learning/data-science-virtual-machine/overview.md) görüntüsünü dağıtabilirsiniz. Windows Server 2016 için DSVM sürümleri, NVıDıA CUDA sürücülerini, CUDA derin sinir ağ kitaplığını ve diğer araçları önceden yükler.


| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows Server 2019 | [451,82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (. exe) |
| Windows Server 2016 | [451,82](http://us.download.nvidia.com/tesla/451.82/451.82-tesla-desktop-winserver-2019-2016-international.exe) (. exe) |

### <a name="nvidia-grid-drivers"></a>NVıDıA KıLAVUZ sürücüleri

Microsoft, sanal iş istasyonları veya sanal uygulamalar olarak kullanılan NV ve NVv3 serisi VM 'Ler için NVıDıA GRID sürücü yükleyicilerini yeniden dağıtır. Yalnızca aşağıdaki tabloda listelenen işletim sistemlerinde Azure NV serisi VM 'lerine yalnızca bu KıLAVUZ sürücülerini yükler. Bu sürücüler Azure 'da GRID sanal GPU yazılımı için lisanslama içerir. NVıDıA vGPU yazılım lisans sunucusu ayarlamanız gerekmez.

Azure tarafından yeniden dağıtılan KıLAVUZ sürücüleri, NC, NCv2, NCv3, ND ve NDv2 serisi VM 'Ler gibi NV serisi olmayan VM 'lerde çalışmaz.

NVIDIA uzantısının en son sürücüyü her zaman yükleyeceğini lütfen unutmayın. Daha eski bir sürüme bağımlılığı olan müşteriler için buradaki önceki sürüme bağlantılar sağlıyoruz.

Windows Server 2019, Windows Server 2016 ve Windows 10 için (derleme 2004 ' ye kadar):
- [Kılavuz 11 (452,39)](https://go.microsoft.com/fwlink/?linkid=874181) (. exe)
- [Kılavuz 11,0 (451,48)](https://download.microsoft.com/download/C/1/4/c147a482-1364-4d12-b9e3-0beda0f00a13/451.48_grid_win10_server2016_server2019_64bit_international.exe) (. exe) 

Windows Server 2012 R2 için: 
- [Kılavuz 11,0 (451,48)](https://download.microsoft.com/download/C/1/4/c147a482-1364-4d12-b9e3-0beda0f00a13/451.48_grid_win10_server2016_server2019_64bit_international.exe) (. exe) 
- [Kılavuz 10,1 (442,66)](https://download.microsoft.com/download/4/3/3/4330fd5c-c685-4ca1-abca-3b2fb3c11d2e/442.06_grid_win8_win7_64bit_international_whql.exe) (. exe)  
