---
title: Windows için Azure N serisi AMD GPU sürücü kurulumu
description: Azure'da Windows Server veya Windows çalıştıran N serisi VM'ler için AMD GPU sürücüleri nasıl ayarlanır?
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
ms.openlocfilehash: 02213feb507e9a032a50241fddf31714b9dfd7ee
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81011096"
---
# <a name="install-amd-gpu-drivers-on-n-series-vms-running-windows"></a>Windows çalıştıran N serisi VM'lere AMD GPU sürücülerini yükleme

Windows çalıştıran yeni Azure NVv4 serisi VM'lerin GPU özelliklerinden yararlanmak için AMD GPU sürücülerinin yüklenmesi gerekir. AMD sürücü uzantısı önümüzdeki haftalarda kullanıma sunulacaktır. Bu makalede desteklenen işletim sistemleri, sürücüler ve el ile yükleme ve doğrulama adımları sağlar.

Temel özellikler, depolama kapasiteleri ve disk ayrıntıları için [GPU Windows VM boyutlarına](sizes-gpu.md?toc=%2fazure%2fvirtual-machines%2fwindows%2ftoc.json)bakın.



## <a name="supported-operating-systems-and-drivers"></a>Desteklenen işletim sistemleri ve sürücüler

| İşletim Sistemi | Sürücü |
| -------- |------------- |
| Windows 10 EVD - Yapı 1903 <br/><br/>Windows 10 - Yapı 1809<br/><br/>Windows Server 2016<br/><br/>Windows Server 2019 | [20.Q1.1](https://download.microsoft.com/download/3/8/9/3893407b-e8aa-4079-8592-735d7dd1c19a/Radeon-Pro-Software-for-Enterprise-GA.exe) (.exe) |


## <a name="driver-installation"></a>Sürücü yükleme

1. Uzak Masaüstü'ne bağlanın ve her NVv4 serisi VM'ye bağlanın.

2. Bir NVv4 önizleme müşterisiyseniz, lütfen VM'yi durdurun ve Durduruldu(Deallocated) durumuna geçmesini bekleyin.

3. Lütfen VM'yi başlatın ve en son [AMD Temizleme Yardımcı Programını](https://download.microsoft.com/download/4/f/1/4f19b714-9304-410f-9c64-826404e07857/AMDCleanupUtilityni.exe)indirin. "amdcleanuputility-x64.exe" çalıştırarak varolan sürücüyü kaldırın. Lütfen önceki sürücü ile yüklü herhangi bir varolan temizleme yardımcı programı kullanmayın.  

4. En son sürücüyü indirin ve kurun.

5. VM'yi yeniden başlatın.

## <a name="verify-driver-installation"></a>Sürücü yüklemeyi doğrulama

Aygıt Yöneticisi'nde sürücü yüklemeyi doğrulayabilirsiniz. Aşağıdaki örnek, Azure NVv4 VM'deki Radeon Instinct MI25 kartının başarılı bir şekilde yapılandırışını gösterir.
<br />
![GPU sürücü özellikleri](./media/n-series-amd-driver-setup/device-manager.png)

Video RAM de dahil olmak üzere GPU ekran özelliklerini doğrulamak için dxdiag'ı kullanabilirsiniz. Aşağıdaki örnek, Azure NVv4 VM'deki Radeon Instinct MI25 kartının 1/2'lik bir bölümüdür.
<br />
![GPU sürücü özellikleri](./media/n-series-amd-driver-setup/dxdiag-output.png)

Windows 10 build 1903 veya daha yüksek çalıştırıyorsanız, dxdiag 'Görüntüle' sekmesinde hiçbir bilgi göstermez. Lütfen en alttaki 'Tüm Bilgileri Kaydet' seçeneğini kullanın ve çıkış dosyası AMD MI25 GPU ile ilgili bilgileri gösterir.

![GPU sürücü özellikleri](./media/n-series-amd-driver-setup/dxdiag-details.png)


