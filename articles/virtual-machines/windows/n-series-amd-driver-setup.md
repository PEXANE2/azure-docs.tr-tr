---
title: Windows için Azure N serisi AMD GPU sürücü kurulumu
description: Azure 'da Windows Server veya Windows çalıştıran N serisi VM 'Ler için AMD GPU sürücülerini ayarlama
author: vikancha
manager: jkabat
ms.service: virtual-machines-windows
ms.topic: how-to
ms.workload: infrastructure-services
ms.date: 12/4/2019
ms.author: vikancha
ms.openlocfilehash: 745ec7ebf792fe1165022516be4c83fb9e864cc9
ms.sourcegitcommit: cf7caaf1e42f1420e1491e3616cc989d504f0902
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/22/2020
ms.locfileid: "83799873"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Windows çalıştıran N serisi VM 'Lere AMD GPU sürücülerini yükler

Windows çalıştıran yeni Azure NVv4 serisi VM 'lerinin GPU Özellikleri avantajlarından yararlanmak için, AMD GPU sürücüleri yüklenmelidir. [AMD GPU sürücü uzantısı](../extensions/hpccompute-amd-gpu-windows.md) , NVv4 SERISI bir VM 'ye AMD GPU sürücülerini kurar. Azure portal veya Azure PowerShell veya Azure Resource Manager şablonları gibi araçları kullanarak uzantıyı yükler veya yönetir. Desteklenen işletim sistemleri ve dağıtım adımları için [AMD GPU sürücü uzantısı belgelerine](../extensions/hpccompute-amd-gpu-windows.md) bakın.

AMD GPU sürücülerini el ile yüklemeyi seçerseniz, bu makale desteklenen işletim sistemleri, sürücüler ve yükleme ve doğrulama adımları sağlar.

NVv4 VM 'lerinde yalnızca Microsoft tarafından yayımlanan GPU sürücüleri desteklenir. Lütfen GPU sürücülerini başka bir kaynaktan yüklemeyin.

Temel özellikler, depolama kapasiteleri ve disk ayrıntıları için bkz. [GPU WINDOWS VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows 10 EVD-derleme 1903 <br/><br/>Windows 10-derleme 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [20. q 1.1](https://download.microsoft.com/download/3/8/9/3893407b-e8aa-4079-8592-735d7dd1c19a/Radeon-Pro-Software-for-Enterprise-GA.exe) (. exe) |


## <a name="driver-installation"></a>Sürücü yükleme

1. Uzak Masaüstü ile her NVv4 serisi VM 'ye bağlanın.

2. En son sürücüyü indirip yükleyin.

3. VM 'yi yeniden başlatın.

## <a name="verify-driver-installation"></a>Sürücü yüklemeyi doğrulama

Aygıt Yöneticisi ' de sürücü yüklemeyi doğrulayabilirsiniz. Aşağıdaki örnekte, bir Azure NVv4 VM üzerinde Radeon Instinct MI25 kartının başarıyla yapılandırılması gösterilmektedir.
<br />
![GPU sürücüsü özellikleri](./media/n-series-amd-driver-setup/device-manager.png)

Video RAM dahil GPU görüntüleme özelliklerini doğrulamak için dxdiag 'ı kullanabilirsiniz. Aşağıdaki örnekte, bir Azure NVv4 VM üzerinde Radeon Instinct MI25 kartının 1/2 bölümü gösterilmektedir.
<br />
![GPU sürücüsü özellikleri](./media/n-series-amd-driver-setup/dxdiag-output.png)

Windows 10 derleme 1903 veya sonraki bir sürümü çalıştırıyorsanız, dxdiag ' görüntü ' sekmesinde hiçbir bilgi göstermez. Lütfen en alttaki ' tüm bilgileri Kaydet ' seçeneğini kullanın ve çıkış dosyasında AMD MI25 GPU ile ilgili bilgiler gösterilir.

![GPU sürücüsü özellikleri](./media/n-series-amd-driver-setup/dxdiag-details.png)


