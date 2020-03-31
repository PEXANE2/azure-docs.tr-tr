---
title: Sunucuları kaldırın ve korumayı devre dışı kaldırın | Microsoft Dokümanlar
description: Bu makalede, bir Site Kurtarma kasasından sunucuların kaydını nasıl çıkarılabilmek ve sanal makineler ve fiziksel sunucular için korumayı devre dışı kalımmak için açıklar.
author: rajani-janaki-ram
manager: rochakm
ms.service: site-recovery
ms.topic: conceptual
ms.date: 06/18/2019
ms.author: rajanaki
ms.openlocfilehash: a411fc9a95bef595a8fc49cad77189bb88fb7661
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79257634"
---
# <a name="remove-servers-and-disable-protection"></a>Sunucuları kaldırma ve korumayı devre dışı bırakma

Bu makalede, Kurtarma Hizmetleri kasasından sunucuların nasıl silinebileceği ve Site Kurtarma tarafından korunan makineler için korumanın nasıl devre dışı kısılası yapılacağı açıklanmaktadır.


## <a name="unregister-a--configuration-server"></a>Yapılandırma sunucusunun kaydını kaldırın

VMware VM'leri veya Windows/Linux fiziksel sunucularını Azure'a kopyalarsanız, bağlı olmayan bir yapılandırma sunucusunun kaydını aşağıdaki gibi kasadan çıkarabilirsiniz:

1. [Sanal makinelerin korumadevre dışı.](#disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure)
2. Çoğaltma ilkelerini [ilişkilendirin veya silin.](vmware-azure-set-up-replication.md#disassociate-or-delete-a-replication-policy)
3. [Yapılandırma sunucusunu silme](vmware-azure-manage-configuration-server.md#delete-or-unregister-a-configuration-server)

## <a name="unregister-a-vmm-server"></a>VMM sunucusunun kaydını kaldırın

1. Kaldırmak istediğiniz VMM sunucusunda bulutlarda sanal makineleri çoğaltmayı durdurun.
2. Silmek istediğiniz VMM sunucusundaki bulutlar tarafından kullanılan ağ eşlemelerini silin. **Sistem Merkezi VMM** > **Ağ Eşleme**için **Site Kurtarma Altyapısında,** > Ağ eşlemesini sağ tıklatın > **Sil.**
3. VMM sunucusunun kimliğine dikkat edin.
4. Kaldırmak istediğiniz VMM sunucusundaki bulutlardan çoğaltma ilkelerini ayırın.  **Sistem Merkezi VMM** >  **Çoğaltma İlkeleri**için **Site Kurtarma Altyapısında,** > ilişkili ilkeyi çift tıklatın. Bulut > **Disassociate'e**sağ tıklayın.
5. VMM sunucusunu veya etkin düğümü silin. **Site Kurtarma Altyapısı** > **Sistem Merkezi VMM** > **VMM Sunucular**için , sağ sunucu > **Silin**.
6. VMM sunucunuz Bağlantısız durumdaysa, temizleme komut [dosyasını](https://aka.ms/asr-cleanup-script-vmm) VMM sunucusunda indirin ve çalıştırın. Varsayılan (LocalMachine) kapsamının yürütme ilkesini değiştirmek için PowerShell'i **Yönetici olarak Çalıştır** seçeneğiyle açın. Komut dosyasında, kaldırmak istediğiniz VMM sunucusunun kimliğini belirtin. Komut dosyası, kayıt ve bulut eşleştirme bilgilerini sunucudan kaldırır.
5. Temizleme komut dosyasını herhangi bir ikincil VMM sunucusunda çalıştırın.
6. Sağlayıcıyüklü olan diğer pasif VMM küme düğümlerinde temizleme komut dosyasını çalıştırın.
7. Sağlayıcıyı VMM sunucusunda el ile kaldırın. Bir kümeniz varsa, tüm düğümlerden kaldırın.
8. Sanal makineleriniz Azure'a çoğalıyorsa, silinen bulutlarda Hyper-V ana bilgisayarlarından Microsoft Kurtarma Hizmetleri aracısını kaldırmanız gerekir.

## <a name="unregister-a-hyper-v-host-in-a-hyper-v-site"></a>Hyper-V Sitesinde ki Hyper-V ana bilgisayarkaydını silme

VMM tarafından yönetilmeyen Hyper-V ana bilgisayarları hyper-v sitesinde toplanır. Hyper-V sitesindeki bir ana bilgisayarı aşağıdaki gibi kaldırın:

1. Ana bilgisayarda bulunan Hyper-V VM'ler için çoğaltmayı devre dışı.
2. Hyper-V sitesi için ilkeleri ilişkilendirin. **Hyper-V Siteleri** >  **Çoğaltma İlkeleri**için Site Kurtarma **Altyapısında,** > ilişkili ilkeyi çift tıklatın. **Disassociate**> siteye sağ tıklayın.
3. Hyper-V ana bilgisayarlarını silin. **Hyper-V Siteler** > **Hyper-V Hosts**için Site Kurtarma **Altyapısında,** >  **Silin**> sunucuya sağ tıklayın.
4. Tüm ana bilgisayarlar kaldırıldıktan sonra Hyper-V sitesini silin. **Hyper-V Siteler Hyper-V** > **Siteleri**için Site **Kurtarma Altyapısında,** > Site'yi sağ tıklatın > **Silin.**
5. Hyper-V ana bilgisayarınız **Bağlantısız** durumdaysa, kaldırdığınız her Hyper-V ana bilgisayarda aşağıdaki komut dosyasını çalıştırın. Komut dosyası sunucudaki ayarları temizler ve kasadan kaydeder.


```powershell
        pushd .
        try
        {
            $windowsIdentity=[System.Security.Principal.WindowsIdentity]::GetCurrent()
            $principal=new-object System.Security.Principal.WindowsPrincipal($windowsIdentity)
            $administrators=[System.Security.Principal.WindowsBuiltInRole]::Administrator
            $isAdmin=$principal.IsInRole($administrators)
            if (!$isAdmin)
            {
                "Please run the script as an administrator in elevated mode."
                $choice = Read-Host
                return;
            }

            $error.Clear()
            "This script will remove the old Azure Site Recovery Provider related properties. Do you want to continue (Y/N) ?"
            $choice =  Read-Host

            if (!($choice -eq 'Y' -or $choice -eq 'y'))
            {
            "Stopping cleanup."
            return;
            }

            $serviceName = "dra"
            $service = Get-Service -Name $serviceName
            if ($service.Status -eq "Running")
            {
                "Stopping the Azure Site Recovery service..."
                net stop $serviceName
            }

            $asrHivePath = "HKLM:\SOFTWARE\Microsoft\Azure Site Recovery"
            $registrationPath = $asrHivePath + '\Registration'
            $proxySettingsPath = $asrHivePath + '\ProxySettings'
            $draIdvalue = 'DraID'
            $idMgmtCloudContainerId='IdMgmtCloudContainerId'


            if (Test-Path $asrHivePath)
            {
                if (Test-Path $registrationPath)
                {
                    "Removing registration related registry keys."
                    Remove-Item -Recurse -Path $registrationPath
                }

                if (Test-Path $proxySettingsPath)
                {
                    "Removing proxy settings"
                    Remove-Item -Recurse -Path $proxySettingsPath
                }

                $regNode = Get-ItemProperty -Path $asrHivePath
                if($regNode.DraID -ne $null)
                {
                    "Removing DraId"
                    Remove-ItemProperty -Path $asrHivePath -Name $draIdValue
                }
                if($regNode.IdMgmtCloudContainerId -ne $null)
                {
                    "Removing IdMgmtCloudContainerId"
                    Remove-ItemProperty -Path $asrHivePath -Name $idMgmtCloudContainerId
                }
                "Registry keys removed."
            }

            # First retrieve all the certificates to be deleted
            $ASRcerts = Get-ChildItem -Path cert:\localmachine\my | where-object {$_.friendlyname.startswith('ASR_SRSAUTH_CERT_KEY_CONTAINER') -or $_.friendlyname.startswith('ASR_HYPER_V_HOST_CERT_KEY_CONTAINER')}
            # Open a cert store object
            $store = New-Object System.Security.Cryptography.X509Certificates.X509Store("My","LocalMachine")
            $store.Open('ReadWrite')
            # Delete the certs
            "Removing all related certificates"
            foreach ($cert in $ASRcerts)
            {
                $store.Remove($cert)
            }
        }catch
        {
            [system.exception]
            Write-Host "Error occurred" -ForegroundColor "Red"
            $error[0]
            Write-Host "FAILED" -ForegroundColor "Red"
        }
        popd
```


## <a name="disable-protection-for-a-vmware-vm-or-physical-server-vmware-to-azure"></a>VMware VM veya fiziksel sunucu (VMware to Azure) için korumayı devre dışı

1. **Korumalı Öğeler** > **Çoğaltılan Öğelerde,** **çoğaltmayı devre dışı >** makineyi sağ tıklatın.
2. **Çoğaltmayı devre dışı bırakma** sayfasında, şu seçeneklerden birini seçin:
    - **Çoğaltmayı devre dışı bırak ve kaldır (önerilir)** - Bu seçenek çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır ve makinenin çoğaltması durdurulur. Configuration Server'daki çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Kurtarma faturalandırması durdurulur. Bu seçeneğin yalnızca Configuration Server bağlı durumdayken kullanılabileceğini unutmayın.
    - **Kaldır** - Bu seçeneğin yalnızca kaynak ortam silinmişse veya erişilebilir değilse (bağlı değilse) kullanılması gerekir. Bu, çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır (faturalandırma durdurulur). Configuration Server'daki çoğaltma yapılandırması **temizlenmez.** 

> [!NOTE]
> Her iki seçenekte de mobilite hizmeti korumalı sunuculardan kaldırılamaz, el ile kaldırmanız gerekir. Aynı Yapılandırma sunucusunu kullanarak sunucuyu yeniden korumayı planlıyorsanız, mobilite hizmetini kaldırmayı atlayabilirsiniz.

> [!NOTE]
> Bir VM üzerinde zaten başarısız olduysanız ve Azure'da çalışıyorsa, devre dışı kalma korumasının VM üzerindeki başarısız ları kaldırmadığını/ etkilemediğini unutmayın.
## <a name="disable-protection-for-a-azure-vm-azure-to-azure"></a>Azure VM (Azure'dan Azure'a) için korumayı devre dışı

-  **Korumalı Öğeler** > **Çoğaltılan Öğelerde,** **çoğaltmayı devre dışı >** makineyi sağ tıklatın.
> [!NOTE]
> taşınabilirlik hizmeti korumalı sunuculardan kaldırılamaz, el ile kaldırmanız gerekir. Sunucuyu yeniden korumayı planlıyorsanız, mobilite hizmetini kaldırmayı atlayabilirsiniz.

## <a name="disable-protection-for-a-hyper-v-virtual-machine-hyper-v-to-azure"></a>Hyper-V sanal makine (Hyper-V to Azure) için korumayı devre dışı

> [!NOTE]
> Hyper-V V MM'leri VMM sunucusu olmadan Azure'a kopyalayorsanız bu yordamı kullanın. **Sistem Merkezi VMM'yi** kullanarak sanal makinelerinizi Azure senaryosuna kopyaluyorsanız, Sistem Merkezi VMM'yi Azure senaryosuna kullanarak Hyper-V sanal makine çoğaltma için yönergeleri Devre Dışı Bırak korumasını izleyin

1. **Korumalı Öğeler** > **Çoğaltılan Öğelerde,** **çoğaltmayı devre dışı >** makineyi sağ tıklatın.
2. **Çoğaltmayı Devre Dışı Atla,** aşağıdaki seçenekleri seçebilirsiniz:
   - **Çoğaltmayı devre dışı bırak ve kaldır (önerilir)** - Bu seçenek çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır ve makinenin çoğaltması durdurulur. Şirket içi sanal makinedeki çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Kurtarma faturalandırması durdurulur.
   - **Kaldır** - Bu seçeneğin yalnızca kaynak ortam silinmişse veya erişilebilir değilse (bağlı değilse) kullanılması gerekir. Bu, çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır (faturalandırma durdurulur). Şirket içi sanal makinedeki çoğaltma yapılandırması **temizlenmez.** 

 > [!NOTE]
     > Şirket içi Hyper-V Server çoğaltma ayarlarını temizlemek için aşağıdaki komut dosyalarını kaldırın **seçeneğini** seçtiyseniz.

> [!NOTE]
> Bir VM üzerinde zaten başarısız olduysanız ve Azure'da çalışıyorsa, devre dışı kalma korumasının VM üzerindeki başarısız ları kaldırmadığını/ etkilemediğini unutmayın.

1. Kaynak Hyper-V ana bilgisayar sunucusunda, sanal makine için çoğaltma kaldırmak için. SQLVM1'i sanal makinenizin adıyla değiştirin ve komut dosyasını idari bir PowerShell'den çalıştırın

```powershell
    $vmName = "SQLVM1"
    $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'"
    $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"
    $replicationService.RemoveReplicationRelationship($vm.__PATH)
```

## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-azure-using-the-system-center-vmm-to-azure-scenario"></a>System Center VMM to Azure senaryosunu kullanarak Azure'a çoğaltma özelliğine karşı Hyper-V sanal makine için korumayı devre dışı bırakma

1. **Korumalı Öğeler** > **Çoğaltılan Öğelerde,** **çoğaltmayı devre dışı >** makineyi sağ tıklatın.
2. **Çoğaltmayı devre dışı devre dışı kseçin:**

   - **Çoğaltmayı devre dışı bırak ve kaldır (önerilir)** - Bu seçenek çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır ve makinenin çoğaltması durdurulur. Şirket içi sanal makinedeki çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Kurtarma faturalandırması durdurulur.
   - **Kaldır** - Bu seçeneğin yalnızca kaynak ortam silinmişse veya erişilebilir değilse (bağlı değilse) kullanılması gerekir. Bu, çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır (faturalandırma durdurulur). Şirket içi sanal makinedeki çoğaltma yapılandırması **temizlenmez.** 

     > [!NOTE]
     > **Kaldır** seçeneğini seçtiyseniz, şirket içi VMM Server çoğaltma ayarlarını temizlemek için aşağıdaki komut dosyalarını tun layın.
3. Bu komut dosyasını, VMM konsolundan PowerShell 'i (yönetici ayrıcalıkları gereklidir) kullanarak kaynak VMM sunucusunda çalıştırın. Yer tutucu **SQLVM1'i** sanal makinenizin adı ile değiştirin.

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. Yukarıdaki adımlar VMM sunucusundaki çoğaltma ayarlarını temizler. Hyper-V ana bilgisayar sunucusunda çalışan sanal makinenin çoğaltmasını durdurmak için bu komut dosyasını çalıştırın. SQLVM1'i sanal makinenizin adıyla, host01.contoso.com ise Hyper-V ana bilgisayar sunucusunun adıyla değiştirin.

```powershell
    $vmName = "SQLVM1"
    $hostName  = "host01.contoso.com"
    $vm = Get-WmiObject -Namespace "root\virtualization\v2" -Query "Select * From Msvm_ComputerSystem Where ElementName = '$vmName'" -computername $hostName
    $replicationService = Get-WmiObject -Namespace "root\virtualization\v2"  -Query "Select * From Msvm_ReplicationService"  -computername $hostName
    $replicationService.RemoveReplicationRelationship($vm.__PATH)
```

## <a name="disable-protection-for-a-hyper-v-virtual-machine-replicating-to-secondary-vmm-server-using-the-system-center-vmm-to-vmm-scenario"></a>System Center VMM to VMM senaryosunu kullanarak ikincil VMM Sunucusuna çoğalan hyper-v sanal makine için korumayı devre dışı bırakma

1. **Korumalı Öğeler** > **Çoğaltılan Öğelerde,** **çoğaltmayı devre dışı >** makineyi sağ tıklatın.
2. **Çoğaltmayı devre dışı devre dışı kseçin:**

   - **Çoğaltmayı devre dışı bırak ve kaldır (önerilir)** - Bu seçenek çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır ve makinenin çoğaltması durdurulur. Şirket içi sanal makinedeki çoğaltma yapılandırması temizlenir ve bu korumalı sunucu için Site Kurtarma faturalandırması durdurulur.
   - **Kaldır** - Bu seçeneğin yalnızca kaynak ortam silinmişse veya erişilebilir değilse (bağlı değilse) kullanılması gerekir. Bu, çoğaltılan öğeyi Azure Site Kurtarma'dan kaldırır (faturalandırma durdurulur). Şirket içi sanal makinedeki çoğaltma yapılandırması **temizlenmez.** Şirket içi sanal makineleri çoğaltma ayarlarını temizlemek için aşağıdaki komut dosyalarını çalıştırın.
     > [!NOTE]
     > **Kaldır** seçeneğini seçtiyseniz, şirket içi VMM Server çoğaltma ayarlarını temizlemek için aşağıdaki komut dosyalarını tun layın.

3. Bu komut dosyasını, VMM konsolundan PowerShell 'i (yönetici ayrıcalıkları gereklidir) kullanarak kaynak VMM sunucusunda çalıştırın. Yer tutucu **SQLVM1'i** sanal makinenizin adı ile değiştirin.

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Set-SCVirtualMachine -VM $vm -ClearDRProtection
4. İkincil VMM sunucusunda, ikincil sanal makinenin ayarlarını temizlemek için bu komut dosyasını çalıştırın:

        $vm = get-scvirtualmachine -Name "SQLVM1"
        Remove-SCVirtualMachine -VM $vm -Force
5. İkincil VMM sunucusunda, Hyper-V ana bilgisayar sunucusundaki sanal makineleri yenileyin, böylece ikincil VM VMM konsolunda tekrar algılanır.
6. Yukarıdaki adımlar VMM sunucusundaki çoğaltma ayarlarını temizler. Sanal makine için çoğaltmayı durdurmak istiyorsanız, aşağıdaki komut dosyasını çalıştırın ana ve ikincil VM'ler. SQLVM1'i sanal makinenizin adıyla değiştirin.

        Remove-VMReplication –VMName “SQLVM1”




