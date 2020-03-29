---
title: Azure VM Aracısını çevrimdışı modda yükleme | Microsoft Dokümanlar
description: Azure VM Aracısını çevrimdışı modda nasıl yükleyeceğimiz öğrenin.
services: virtual-machines-windows
documentationcenter: ''
author: genlin
manager: dcscontentpm
editor: ''
tags: azure-resource-manager
ms.service: virtual-machines-windows
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-windows
ms.topic: article
ms.date: 10/31/2018
ms.author: genli
ms.openlocfilehash: 8ea85b560f35c79b3d5066d794f587345810b5d0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77920867"
---
# <a name="install-the-azure-virtual-machine-agent-in-offline-mode"></a>Azure Sanal Makine Aracısını çevrimdışı modda yükleme 

Azure Sanal Makine Aracısı (VM Agent), yerel yönetici parola sıfırlama ve komut dosyası zorlama gibi yararlı özellikler sağlar. Bu makalede, çevrimdışı Bir Windows sanal makine (VM) için VM Aracısı nasıl yüklenir gösterir. 

## <a name="when-to-use-the-vm-agent-in-offline-mode"></a>VM Aracısı çevrimdışı modda ne zaman kullanılır?

Aşağıdaki senaryolarda VM Aracısını çevrimdışı modda yükleyin:

- Dağıtılan Azure VM'de VM Aracısı yüklü değildir veya aracı çalışmıyor.
- VM'nin yönetici parolasını unuttunuz veya VM'ye erişemedin.

## <a name="how-to-install-the-vm-agent-in-offline-mode"></a>VM Aracısı çevrimdışı modda nasıl yüklenir?

VM Aracısını çevrimdışı modda yüklemek için aşağıdaki adımları kullanın.

### <a name="step-1-attach-the-os-disk-of-the-vm-to-another-vm-as-a-data-disk"></a>Adım 1: VM'nin işletim sistemi diskini veri diski olarak başka bir VM'ye takın

1. Etkilenen VM işletim sistemi diski için anlık görüntü alın, anlık görüntüden bir disk oluşturun ve ardından diski sorun giderme VM'sine takın. Daha fazla bilgi için, [Azure portalını kullanarak işletim sistemi diskini kurtarma VM'ine ekleyerek Windows VM Sorun Giderme'ye](troubleshoot-recovery-disks-portal-windows.md)bakın. Klasik VM için VM'yi silin ve işletim sistemi diskini tutun ve ardından işletim sistemi diskini sorun giderme VM'sine takın.

2.  Sorun giderici VM'ye bağlanın. Açık **Bilgisayar yönetimi** > **Disk yönetimi**. İşletim sistemi diskinin çevrimiçi olduğunu ve sürücü harflerinin disk bölümlerine atandığını doğrulayın.

### <a name="step-2-modify-the-os-disk-to-install-the-azure-vm-agent"></a>Adım 2: Azure VM Aracısını yüklemek için işletim sistemi diskini değiştirme

1.  Sorun giderici VM'ye uzak bir masaüstü bağlantısı kurun.

2.  Sorun giderici VM'de, iliştirdiğiniz işletim sistemi diskine göz atın, \windows\system32\config klasörünü açın. Geri alma gerekirse, bu klasördeki tüm dosyaları yedek olarak kopyalayın.

3.  Kayıt **Defteri Düzenleyicisini** Başlat (regedit.exe).

4.  **HKEY_LOCAL_MACHINE** tuşunu seçin. Menüde **Dosya** > **Yük Kovanı'nı**seçin:

    ![Kovanı yükleyin](./media/install-vm-agent-offline/load-hive.png)

5.  Eklediğiniz işletim sistemi diskindeki \windows\system32\config\SYSTEM klasörüne göz atın. Kovanın adı için **BROKENSYSTEM'i**girin. Yeni kayıt kovanı **HKEY_LOCAL_MACHINE** anahtarının altında görüntülenir.

6.  Eklediğiniz işletim sistemi diskindeki \windows\system32\config\SOFTWARE klasörüne göz atın. Kovan yazılımının adı için **BROKENSOFTWARE'i**girin.

7. Bağlı Işletim Sistemi diski VM aracısını yüklüyorsa, geçerli yapılandırmanın yedeğini gerçekleştirin. VM aracısı yüklü değilse, bir sonraki adıma geçin.
      
    1. \windowsazure klasörünü \windowsazure.old olarak yeniden adlandırın.

    2. Aşağıdaki kayıtları dışa aktarın:
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE\BROKENSYSTEM\ControlSet001\Services\RdAgent

8.  Sorun giderici VM'deki varolan dosyaları VM Aracısı yüklemesi için bir depo olarak kullanın. Aşağıdaki adımları tamamlayın:

    1. Sorun giderici VM'den, aşağıdaki alt anahtarları kayıt defteri biçiminde (.reg) dışa aktarın: 
        - HKEY_LOCAL_MACHINE \System\ControlSet001\Services\WindowsAzureGuestAgent
        - HKEY_LOCAL_MACHINE \System\ControlSet001\Services\WindowsAzureTelemetryService
        - HKEY_LOCAL_MACHINE \System\ControlSet001\Services\RdAgent

          ![Kayıt defteri alt anahtarlarını dışa aktarma](./media/install-vm-agent-offline/backup-reg.png)

    2. Kayıt defteri dosyalarını edin. Her dosyada, giriş değeri **System'i** **BROKENSYSTEM** (aşağıdaki resimlerde gösterildiği gibi) olarak değiştirin ve dosyayı kaydedin. Geçerli VM aracısının **ImagePath'ini** hatırlayın. İlgili klasörü bağlı işletim sistemi diskine kopyalamamız gerekir. 

        ![Kayıt defteri alt anahtar değerlerini değiştirme](./media/install-vm-agent-offline/change-reg.png)

    3. Her kayıt defteri dosyasını çift tıklatarak kayıt defteri dosyalarını depoya aktarın.

    4. Aşağıdaki üç alt tuşun **BROKENSYSTEM** kovanına başarıyla aktarıldığını doğrulayın:
        - WindowsAzureGuestAgent
        - WindowsAzureTelemetryService
        - Rdagent

    5. Geçerli VM Aracısı'nın yükleme klasörünü bağlı işletim sistemi diskine kopyalayın: 

        1.  Eklediğiniz işletim sistemi diskinde kök yolunda WindowsAzure adında bir klasör oluşturun.

        2.  Sorun giderici VM'deki C:\WindowsAzure'a gidin, C:\WindowsAzure\GuestAgent_X.X.XXXX.XXX adındaki klasörü arayın. C:\WindowsAzure'dan en son sürüm numarasına sahip GuestAgent klasörünü ekli işletim sistemi diskindeki WindowsAzure klasörüne kopyalayın. Hangi klasörün kopyalanması gerektiğinden emin değilseniz, tüm GuestAgent klasörlerini kopyalayın. Aşağıdaki resimde, bağlı işletim sistemi diskine kopyalanan GuestAgent klasörünün bir örneği gösterilmektedir.

             ![GuestAgent klasörünü kopyala](./media/install-vm-agent-offline/copy-files.png)

9.  **BROKENSYSTEM'i**seçin. Menüden **Dosya** > **Boşalt Kovanı'nı**seçin.

10.  **BROKENSOFTWARE'i**seçin. Menüden **Dosya** > **Boşalt Kovanı'nı**seçin.

11.  Os diskini ayırın ve [etkilenen VM için işletim sistemi diskini değiştirin.](troubleshoot-recovery-disks-portal-windows.md#swap-the-os-disk-for-the-vm) Klasik VM için, onarılmış işletim sistemi diskini kullanarak yeni bir VM oluşturun.

12.  VM'ye erişin. RdAgent'ın çalıştığını ve günlüklerin oluşturulduğuna dikkat edin.

Kaynak Yöneticisi dağıtım modelini kullanarak VM'yi oluşturduysanız, bittiniz.

### <a name="use-the-provisionguestagent-property-for-classic-vms"></a>Klasik VM'ler için ProvisionGuestAgent özelliğini kullanın

[!INCLUDE [classic-vm-deprecation](../../../includes/classic-vm-deprecation.md)]

VM'yi klasik modeli kullanarak oluşturduysanız, **ProvisionGuestAgent** özelliğini güncelleştirmek için Azure PowerShell modüllerini kullanın. Özellik, Azure'a VM Aracısı'nın yüklü olduğunu bildirir.

**ProvisionGuestAgent** özelliğini ayarlamak için Azure PowerShell'de aşağıdaki komutları çalıştırın:

   ```powershell
   $vm = Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   $vm.VM.ProvisionGuestAgent = $true
   Update-AzureVM –Name <VM name> –VM $vm.VM –ServiceName <cloud service name>
   ```

O zaman `Get-AzureVM` komutu çalıştırın. **GuestAgentStatus** özelliğinin artık verilerle dolu olduğuna dikkat edin:

   ```powershell
   Get-AzureVM –ServiceName <cloud service name> –Name <VM name>
   GuestAgentStatus:Microsoft.WindowsAzure.Commands.ServiceManagement.Model.PersistentVMModel.GuestAgentStatus
   ```

## <a name="next-steps"></a>Sonraki adımlar

- [Azure Sanal Makine Aracısı'na genel bakış](../extensions/agent-windows.md)
- [Windows için sanal makine uzantıları ve özellikleri](../extensions/features-windows.md)
