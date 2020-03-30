---
title: Sunucular için Azure Arkını yönetme (önizleme) aracısını
description: Bu makalede, Bağlı Makine aracısı sunucular için Azure Arc'ın yaşam döngüsü sırasında genellikle gerçekleştireceğiniz farklı yönetim görevleri açıklanmaktadır.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 03/24/2020
ms.topic: conceptual
ms.openlocfilehash: 758e6123fd09df1e3f8b2e883a729b9fec4328d1
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80367297"
---
# <a name="managing-and-maintaining-the-connected-machine-agent"></a>Bağlı Makine aracısını yönetme ve sürdürme

Windows veya Linux için sunucular için Azure Arc (önizleme) Bağlı Makine aracısını ilk kez dağıtımından sonra, aracıyı yeniden yapılandırmanız, yükseltmeniz veya kullanım ömrü içinde emeklilik aşamasına ulaşmışsa bilgisayardan kaldırmanız gerekebilir. Bu rutin bakım görevlerini el ile veya otomasyon yoluyla kolayca yönetebilirsiniz, bu da hem operasyonel hatayı hem de giderleri azaltır.

## <a name="upgrading-agent"></a>Aracıyı yükseltme

Windows ve Linux için Azure Bağlantılı Makine aracısı, gereksinimlerinize bağlı olarak el ile veya otomatik olarak en son sürüme yükseltilebilir. Aşağıdaki tabloda aracı yükseltme gerçekleştirmek için desteklenen yöntemler açıklanmaktadır.

| İşletim sistemi | Yükseltme yöntemi |
|------------------|----------------|
| Windows | El ile<br> Windows Update |
| Ubuntu | [Apt](https://help.ubuntu.com/lts/serverguide/apt.html) |
| SUSE Linux Enterprise Server | [zypper](https://en.opensuse.org/SDB:Zypper_usage_11.3) |
| RedHat Enterprise, Amazon, CentOS Linux | [Yum](https://wiki.centos.org/PackageManagement/Yum) | 

### <a name="windows-agent"></a>Windows aracısı

Aracı, Windows makinesindeki aracıyı en son sürüme güncellemek için Microsoft Update'ten edinilebilir ve varolan yazılım güncelleştirme yönetimi işleminizi kullanarak dağıtılabilir. Komut İstemi'nden, komut dosyasından veya başka bir otomasyon çözümünden veya ui `AzureConnectedMachine.msi`sihirbazından yürütülerek el ile çalıştırılabilir. 

> [!NOTE]
> * Aracıyı yükseltmek için *Yönetici* izinlerine sahip olmalısınız.
> * El ile yükseltmek için önce Yükleyici paketini hedef sunucudaki bir klasöre veya paylaşılan bir ağ klasöründen indirmeniz ve kopyalamanız gerekir. 

Windows Installer paketleri için komut satırı seçeneklerini bilmiyorsanız, [Msiexec standart komut satırı seçeneklerini](https://docs.microsoft.com/windows/win32/msi/standard-installer-command-line-options) ve [Msiexec komut satırı seçeneklerini gözden geçirin.](https://docs.microsoft.com/windows/win32/msi/command-line-options)

#### <a name="to-upgrade-using-the-setup-wizard"></a>Kurulum Sihirbazı'nı kullanarak yükseltme yapmak için

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Kurulum Sihirbazı'nı başlatmak için **AzureConnectedMachineAgent.msi'yi** çalıştırın.

Kurulum Sihirbazı, önceki bir sürümün var olup olmadığını keşfeder ve aracının yükseltmesini otomatik olarak gerçekleştirir. Yükseltme tamamlandığında, Kurulum Sihirbazı otomatik olarak kapanır.

#### <a name="to-upgrade-from-the-command-line"></a>Komut satırından yükseltmek için

1. Yönetim haklarına sahip bir hesapla bilgisayarda oturum açın.

2. Aracıyı sessizce yükseltmek ve `C:\Support\Logs` klasörde bir kurulum günlüğü dosyası oluşturmak için aşağıdaki komutu çalıştırın.

    ```dos
    msiexec.exe /i AzureConnectedMachineAgent.msi /qn /l*v "C:\Support\Logs\Azcmagentupgradesetup.log"
    ```

### <a name="linux-agent"></a>Linux aracısı

Bir Linux makinesindeki aracıyı en son sürüme güncellemek için iki komut içerir. Yerel paket dizini depolardan en son kullanılabilir paketlerin listesiyle güncelleştirmek için bir komut ve yerel paketi yükseltmek için bir komut. 

#### <a name="upgrade-ubuntu"></a>Yükseltme Ubuntu

1. Yerel paket dizini depolarda yapılan en son değişikliklerle güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    apt update
    ```

2. Sisteminizi yükseltmek için aşağıdaki komutu çalıştırın:

    ```bash
    apt upgrade
    ```

Paketlerin yüklenmesi ve kaldırılması gibi [apt](https://help.ubuntu.com/lts/serverguide/apt.html) komutunun eylemleri `/var/log/dpkg.log` günlük dosyasında günlüğe kaydedilir.

#### <a name="upgrade-red-hatcentosamazon-linux"></a>Kırmızı Şapka/CentOS/Amazon Linux'u Yükselt

1. Yerel paket dizini depolarda yapılan en son değişikliklerle güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    yum check-update
    ```

2. Sisteminizi yükseltmek için aşağıdaki komutu çalıştırın:

    ```bash
    yum update
    ```

Paketlerin yüklenmesi ve kaldırılması gibi [yum](https://access.redhat.com/articles/yum-cheat-sheet) komutunun eylemleri `/var/log/yum.log` günlük dosyasında günlüğe kaydedilir. 

#### <a name="upgrade-suse-linux-enterprise"></a>SUSE Linux Kurumsal yükseltme

1. Yerel paket dizini depolarda yapılan en son değişikliklerle güncelleştirmek için aşağıdaki komutu çalıştırın:

    ```bash
    zypper refresh
    ```

2. Sisteminizi yükseltmek için aşağıdaki komutu çalıştırın:

    ```bash
    zypper update
    ```

[Zypper](https://en.opensuse.org/Portal:Zypper) komutunun paketlerin yüklenmesi ve kaldırılması gibi eylemleri `/var/log/zypper.log` günlük dosyasında günlüğe kaydedilir. 

## <a name="remove-the-agent"></a>Aracıyı kaldırma

Bu bölümde açıklanan komut satırını veya kurulum sihirbazını kullanarak Windows veya Linux aracısını kaldırmak için aşağıdaki yordamlardan birini kullanın. Aracıyı yüklemeden önce, aşağıdaki adımları tamamlayarak sunucular için makineyi Azure Arc'tan (önizleme) çıkarın: 

1. [Azure portalına](https://aka.ms/hybridmachineportal)giderek sunucular için Azure Arc'ı açın (önizleme).

2. Listedeki makineyi seçin, elipsleri seçin (**...**), ve sonra **Sil'i**seçin.

### <a name="windows-agent"></a>Windows aracısı

#### <a name="uninstall-from-control-panel"></a>Denetim Panelinden Kaldır

1. Windows aracısını makineden kaldırmak için aşağıdakileri yapın:

    a. Yönetici izinleri olan bir hesapla bilgisayarda oturum açın.  
    b. **Denetim**Masası'nda, Programlar **ve Özellikler'i**seçin.  
    c. **Programlar ve Özellikler'de**Azure Bağlı **Makine Aracısı'nı**seçin , **Kaldır**'ı seçin ve ardından **Evet'i**seçin.  

    >[!NOTE]
    > Ayrıca **AzureConnectedMachineAgent.msi** yükleyici paketini çift tıklatarak aracı kurulum sihirbazını çalıştırabilirsiniz.

#### <a name="uninstall-from-the-command-line"></a>Komut satırından kaldırma

Aracıyı Komut İstemi'nden el ile kaldırmak veya komut dosyası gibi otomatik bir yöntem kullanmak için aşağıdaki örneği kullanabilirsiniz. Öncelikle, uygulama paketinin temel tanımlayıcısı olan bir GUID olan ürün kodunu işletim sisteminden almanız gerekir. Kaldırma Msiexec.exe komut satırı kullanılarak gerçekleştirilir `msiexec /x {Product Code}`- .
    
1. Kayıt Defteri Düzenleyicisi'ni açın.

2. Kayıt defteri `HKEY_LOCAL_MACHINE\Software\Microsoft\Windows\CurrentVersion\Uninstall`anahtarı altında, ürün kodunu arayın ve kopyalayın GUID.

3. Daha sonra aşağıdaki örnekleri kullanarak Msiexec kullanarak aracıyı kaldırabilirsiniz:

   * Komut satırı türünden:

       ```dos
       msiexec.exe /x {product code GUID} /qn
       ```

   * PowerShell'i kullanarak aynı adımları gerçekleştirebilirsiniz:

       ```powershell
       Get-ChildItem -Path HKLM:\Software\Microsoft\Windows\CurrentVersion\Uninstall | `
       Get-ItemProperty | `
       Where-Object {$_.DisplayName -eq "Azure Connected Machine Agent"} | `
       ForEach-Object {MsiExec.exe /x "$($_.PsChildName)" /qn}
       ```

### <a name="linux-agent"></a>Linux aracısı

Linux aracısını kaldırmak için kullanılacak komut Linux işletim sistemine bağlıdır.

- Ubuntu için aşağıdaki komutu çalıştırın:

    ```bash
    sudo apt purge azcmagent
    ```

- RHEL, CentOS ve Amazon Linux için aşağıdaki komutu çalıştırın:

    ```bash
    sudo yum remove azcmagent
    ```

- SLES için aşağıdaki komutu çalıştırın:

    ```bash
    sudo zypper remove azcmagent
    ```
