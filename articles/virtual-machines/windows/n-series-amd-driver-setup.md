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
ms.openlocfilehash: 6dc8c54b9d138ab62e086cca59cd5b4801fa6130
ms.sourcegitcommit: 390cfe85629171241e9e81869c926fc6768940a4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/02/2020
ms.locfileid: "78228336"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Windows çalıştıran N serisi VM 'Lere AMD GPU sürücülerini yükler

Windows çalıştıran yeni Azure NVv4 serisi VM 'lerinin GPU Özellikleri avantajlarından yararlanmak için, AMD GPU sürücüleri yüklenmelidir. AMD Sürücü Uzantısı önümüzdeki haftalarda kullanılabilir olacaktır. Bu makalede desteklenen işletim sistemleri, sürücüler ve el ile yükleme ve doğrulama adımları sağlanmaktadır.

Temel özellikler, depolama kapasiteleri ve disk ayrıntıları için bkz. [GPU WINDOWS VM boyutları](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json).



## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows 10 EVD-derleme 1903 <br/><br/>Windows 10-derleme 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [20. q 1.1](https://download.microsoft.com/download/3/8/9/3893407b-e8aa-4079-8592-735d7dd1c19a/Radeon-Pro-Software-for-Enterprise-GA.exe) (. exe) |


## <a name="driver-installation"></a>Sürücü yükleme

1. Uzak Masaüstü ile her NVv4 serisi VM 'ye bağlanın.

2. Bir NVv4 Preview müşterisiyseniz lütfen VM 'yi durdurun ve bu işlemin durdurulmuş (serbest bırakılmış) duruma gelmesini bekleyin.

3. Lütfen VM 'yi başlatın ve ardından ". ..\Amdcleanunınstallusystemutility" klasöründe bulunan "amdcleanuputility-x64. exe" komutunu çalıştırarak önizleme sürücüsünü kaldırın. Tam yol, önceki sürücü yükleme dosyalarının bulunduğu yere göre farklılık gösterecektir.  

4. En son sürücüyü indirip yükleyin.

## <a name="verify-driver-installation"></a>Sürücü yüklemeyi doğrulama

Aygıt Yöneticisi ' de sürücü yüklemeyi doğrulayabilirsiniz. Aşağıdaki örnekte, bir Azure NVv4 VM üzerinde Radeon Instinct MI25 kartının başarıyla yapılandırılması gösterilmektedir.
<br />
GPU sürücü özelliklerini ![](./media/n-series-amd-driver-setup/device-manager.png)

Video RAM dahil GPU görüntüleme özelliklerini doğrulamak için dxdiag 'ı kullanabilirsiniz. Aşağıdaki örnek, bir Azure NVv4 VM üzerinde Radeon Instinct MI25 kartının 1/8th bölümünü gösterir.
<br />
GPU sürücü özelliklerini ![](./media/n-series-amd-driver-setup/dxdiag.png)
