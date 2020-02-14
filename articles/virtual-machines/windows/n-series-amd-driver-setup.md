---
title: Windows için Azure N serisi AMD GPU sürücü kurulumu
description: Azure 'da Windows Server veya Windows çalıştıran N serisi VM 'Ler için AMD GPU sürücülerini ayarlama
services: virtual-machines-windows
author: vikancha
manager: jkabat
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.topic: article
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: fdc6834f3fb5ee97f27a6397645b965863e90a6b
ms.sourcegitcommit: b07964632879a077b10f988aa33fa3907cbaaf0e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/13/2020
ms.locfileid: "77190531"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Windows çalıştıran N serisi VM 'Lere AMD GPU sürücülerini yükler

Windows çalıştıran yeni Azure NVv4 serisi VM 'lerinin GPU Özellikleri avantajlarından yararlanmak için, AMD GPU sürücüleri yüklenmelidir. AMD Sürücü Uzantısı önümüzdeki haftalarda kullanılabilir olacaktır. Bu makalede desteklenen işletim sistemleri, sürücüler ve el ile yükleme ve doğrulama adımları sağlanmaktadır.

Temel özellikler, depolama kapasiteleri ve disk ayrıntıları için bkz. [GPU WINDOWS VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows 10 EVD-derleme 1903 <br/><br/>Windows 10-derleme 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [19. q 4.1](https://download.microsoft.com/download/7/e/5/7e558ac0-3fff-413d-af62-800285a2fc53/Radeon-Pro-Software-for-Enterprise-19.Q4.1-Technical-Preview.exe) (. exe) |

## <a name="driver-installation"></a>Sürücü yükleme

1. Uzak Masaüstü ile her NVv4 serisi VM 'ye bağlanın.

1. Sürücü kurulum dosyalarını indirin ve ayıklayın. Klasöre gidin ve Windows işletim sisteminiz için desteklenen sürücüyü yüklemek üzere ' Setup. exe ' yi çalıştırın.

## <a name="verify-driver-installation"></a>Sürücü yüklemeyi doğrulama

Aygıt Yöneticisi ' de sürücü yüklemeyi doğrulayabilirsiniz. Aşağıdaki örnekte, bir Azure NVv4 VM üzerinde Radeon Instinct MI25 kartının başarıyla yapılandırılması gösterilmektedir.
<br />
GPU sürücü özelliklerini ![](./media/n-series-amd-driver-setup/device-manager.png)

Video RAM dahil GPU görüntüleme özelliklerini doğrulamak için dxdiag 'ı kullanabilirsiniz. Aşağıdaki örnek, bir Azure NVv4 VM üzerinde Radeon Instinct MI25 kartının 1/8th bölümünü gösterir.
<br />
GPU sürücü özelliklerini ![](./media/n-series-amd-driver-setup/dxdiag.png)
