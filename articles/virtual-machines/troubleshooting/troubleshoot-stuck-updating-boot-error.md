---
title: Azure VM başlatma Windows Update| Microsoft Dokümanlar
description: Bir Azure VM başlatma windows güncelleştirmesine takılıp kaldığında sorunu nasıl gidereceklerini öğrenin.
services: virtual-machines-windows
documentationCenter: ''
author: genlin
manager: dcscontentpm
editor: v-jesits
ms.service: virtual-machines-windows
ms.topic: troubleshooting
ms.tgt_pltfrm: vm-windows
ms.workload: infrastructure
ms.date: 10/09/2018
ms.author: genli
ms.openlocfilehash: 8a47131cb4f19cce1664eafa50c67ab1a1171e67
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77919439"
---
# <a name="azure-vm-startup-is-stuck-at-windows-update"></a>Azure VM başlatma Windows güncellemesi sıkışmış

Bu makale, Sanal Makineniz (VM) başlangıç sırasında Windows Update aşamasında sıkıştığında sorunu çözmeye yardımcı olur. 


## <a name="symptom"></a>Belirti

 Windows VM başlatılamıyor. [Önyükleme tanılama](../troubleshooting/boot-diagnostics.md) penceresindeki ekran görüntülerini kontrol ettiğinizde, başlangıç işleminin güncelleştirme işleminde sıkışıp kaldığını görürsünüz. Alabileceğiniz iletiörnekleri aşağıda verilmiştir:

- Windows 'u yükleme ##% Bilgisayarınızı kapatmayın. Bu işlem, bilgisayarınızın birkaç kez yeniden başlatılmasını biraz zaman alacaktır
- Bu işlem bitene kadar bilgisayarınızı takla tutun. Yükleme güncelleştirme # # 
- Güncellemeleri tamamlayamadık Değişiklikleri Geri Alma Bilgisayarınızı kapatmayın
- Windows güncelleştirmelerini yapılandırma hatası Değişiklikleri Tersine Çevirme Bilgisayarınızı kapatma
- Hata < hata kodu > update işlemleri ###### ##### (\Regist...)
- Hata < hata kodu > güncelleme işlemleri ###### ##### ($$...)


## <a name="solution"></a>Çözüm

Yüklenen veya destek alan güncelleştirme sayısına bağlı olarak, güncelleştirme işlemi biraz zaman alabilir. VM'yi 8 saat boyunca bu durumda bırakın. Bu dönemden sonra VM hala bu durumdaysa, Azure portalından VM'yi yeniden başlatın ve normal şekilde başlayıp başlayıp başlayamayacak olmadığını görün. Bu adım işe yaramazsa, aşağıdaki çözümü deneyin.

### <a name="remove-the-update-that-causes-the-problem"></a>Soruna neden olan güncelleştirmeyi kaldırma

1. Yedek olarak etkilenen VM'nin işletim sistemi diskinin anlık görüntüsünü alın. Daha fazla bilgi için [bir diskanlık anlık görüntüsüne](../windows/snapshot-copy-managed-disk.md)bakın. 
2. [Os diskini kurtarma VM'sine takın.](troubleshoot-recovery-disks-portal-windows.md)
3. Os diski kurtarma VM'sine bağlandıktan sonra Disk Yönetimi'ni açmak için **diskmgmt.msc'yi** çalıştırın ve ekteki diskin **ÇEVRIMIÇI**olduğundan emin olun. \windows klasörünü tutan bağlı işletim sistemi diskine atanan sürücü mektubuna dikkat edin. Disk şifrelenmişse, bu belgedeki sonraki adımlara geçmeden önce diskin şifresini çöz.

4. Yükseltilmiş bir komut istemi örneğini açın (Yönetici olarak çalıştırın). Ekli işletim sistemi diskinde olan güncelleştirme paketlerinin listesini almak için aşağıdaki komutu çalıştırın:

        dism /image:<Attached OS disk>:\ /get-packages > c:\temp\Patch_level.txt

    Örneğin, bağlı işletim sistemi diski F sürücüsüyse, aşağıdaki komutu çalıştırın:

        dism /image:F:\ /get-packages > c:\temp\Patch_level.txt
5. C:\temp\Patch_level.txt dosyasını açın ve alttan yukarıya doğru okuyun. **Bekleyen Yükle** veya Kaldır **Bekleyen** durumundaki güncelleştirmeyi bulun.  Aşağıda güncelleştirme durumunun bir örneği vereme durumu ve

     ```
    Package Identity : Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    State : Install Pending
    Release Type : Security Update
    Install Time :
    ```
6. Soruna neden olan güncelleştirmeyi kaldırın:
    
    ```
    dism /Image:<Attached OS disk>:\ /Remove-Package /PackageName:<PACKAGE NAME TO DELETE>
    ```
    Örnek: 

    ```
    dism /Image:F:\ /Remove-Package /PackageName:Package_for_RollupFix~31bf3856ad364e35~amd64~~17134.345.1.5
    ```

    > [!NOTE] 
    > Paketin boyutuna bağlı olarak, DISM aracının yüklemeyi kaldırma işlemini işlemesi biraz zaman alacaktır. Normalde işlem 16 dakika içinde tamamlanır.

7. [İşletim sistemi diskini ayırın ve VM'yi yeniden oluşturun.](troubleshoot-recovery-disks-portal-windows.md#unmount-and-detach-original-virtual-hard-disk) Ardından sorunun çözülüp çözülmediğini kontrol edin.
