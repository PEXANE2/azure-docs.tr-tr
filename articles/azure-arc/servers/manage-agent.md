---
title: Sunucular için Azure Arkını yönetme (önizleme) aracısını
description: Bu makalede, Bağlı Makine aracısı sunucular için Azure Arc'ın yaşam döngüsü sırasında genellikle gerçekleştireceğiniz farklı yönetim görevleri açıklanmaktadır.
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-servers
author: mgoedtel
ms.author: magoedte
ms.date: 04/14/2020
ms.topic: conceptual
ms.openlocfilehash: 5ad2127b4cb9da3ca83aa04bd1885908a88dba62
ms.sourcegitcommit: 7e04a51363de29322de08d2c5024d97506937a60
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/14/2020
ms.locfileid: "81308967"
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

> [!NOTE]
> Aracıyı yükseltmek için *kök* erişim izinlerine veya Sudo'yu kullanarak hakları yüksek bir hesaba sahip olmalısınız.

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

## <a name="about-the-azcmagent-tool"></a>Azcmagent aracı hakkında

Azcmagent aracı (Azcmagent.exe), yükleme sırasında sunucular (önizleme) Bağlı Makine aracısı için Azure Arc'ı yapılandırmak veya yüklemeden sonra aracının ilk yapılandırmasını değiştirmek için kullanılır. Azcmagent.exe aracıyı özelleştirmek ve durumunu görüntülemek için komut satırı parametreleri sağlar:

* **Bağlan** - Makineyi Azure Arc'a bağlamak için

* **Bağlantıyı kesmek** - Makineyi Azure Arc'tan ayırmak için

* **Yeniden bağlanma** - Bağlantısı kesilen bir makineyi Azure Arc'a yeniden bağlamak için

* **Göster** - Aracı durumunu ve yapılandırma özelliklerini (Kaynak Grubu adı, Abonelik Kimliği, sürüm, vb.) görüntüleyin ve aracıyla ilgili bir sorunu gidermede yardımcı olabilir.

* **-h veya --help** - Kullanılabilir komut satırı parametrelerini gösterir

    Örneğin, **Yeniden Bağlanma** parametresi için ayrıntılı `azcmagent reconnect -h`yardım görmek için . 

* **-v veya --verbose** - Verbose günlüğe kaydetmeyi etkinleştir

Etkileşimli olarak oturum açtığınızda el ile **Bağlan,** **Bağlantıyı Kes**ve **Yeniden Bağlanma** gerçekleştirebilir veya birden çok aracıda bulunan aynı hizmet ilkesini kullanarak veya Microsoft kimlik platformu [erişim jetonunda](../../active-directory/develop/access-tokens.md)otomatikleştirebilirsiniz. Makineyi sunucular için Azure Arc'a kaydettirmek için bir hizmet ilkesi kullanmadıysanız (önizleme), bir hizmet sorumlusu oluşturmak için aşağıdaki [makaleye](onboard-service-principal.md#create-a-service-principal-for-onboarding-at-scale) bakın.

### <a name="connect"></a>Bağlan

Bu parametre, Azure Kaynak Yöneticisi'nde makineyi temsil eden bir kaynağın Azure'da oluşturulduğunu belirtir. Kaynak belirtilen abonelik ve kaynak grubundadır ve makineyle ilgili veriler `--location` ayar tarafından belirtilen Azure bölgesinde depolanır. Varsayılan kaynak adı belirtilmemişse bu makinenin ana adıdır.

Makinenin sistem tarafından atanan kimliğine karşılık gelen bir sertifika daha sonra karşıdan yüklenir ve yerel olarak depolanır. Bu adım tamamlandıktan sonra, Azure Bağlı Makine Meta veri Hizmeti ve Konuk Yapılandırma Aracısı sunucular için Azure Arc ile eşitleme yapmaya başlar (önizleme).

Bir hizmet sorumlusu kullanarak bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent connect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

Bir erişim belirteci kullanarak bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent connect --access-token <> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

Yüksek oturum açmış kimlik bilgilerinize (etkileşimli) bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent connect --tenant-id <TenantID> --subscription-id <subscriptionID> --resource-group <ResourceGroupName> --location <resourceLocation>`

### <a name="disconnect"></a>Bağlantıyı kes

Bu parametre, Makineyi temsil eden Azure Kaynak Yöneticisi'ndeki bir kaynağın Azure'da silinip silindiğini belirtir. Aracıyı makineden silmez, bu ayrı bir adım olarak yapılmalıdır. Makinenin bağlantısı kesildikten sonra, sunucular için Azure Arc'a yeniden `azcmagent connect` kaydetmek istiyorsanız (önizleme), Azure'da bunun için yeni bir kaynak oluşturulması için kullanın.

Bir hizmet sorumlusu kullanarak bağlantıyı kesmek için aşağıdaki komutu çalıştırın:

`azcmagent disconnect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID>`

Bir erişim jetonu kullanarak bağlantıyı kesmek için aşağıdaki komutu çalıştırın:

`azcmagent disconnect --access-token <accessToken>`

Yükseltilmiş oturum açmış kimlik bilgilerinizle (etkileşimli) bağlantınızı kesmek için aşağıdaki komutu çalıştırın:

`azcmagent disconnect --tenant-id <tenantID>`

### <a name="reconnect"></a>Yeniden bağlan

Bu parametre, zaten kayıtlı olan veya bağlı olan makineyi sunucular için Azure Arc'a yeniden bağlar (önizleme). Makinenin süresinin dolması için makine en az 45 gün kapalıysa bu gerekli olabilir. Bu parametre, bu makineyi temsil eden Azure Kaynak Yöneticisi kaynağına karşılık gelen yeni kimlik bilgilerini almak için sağlanan kimlik doğrulama seçeneklerini kullanır.

Bu komut, [Azure Bağlı Makine Onboarding](overview.md#required-permissions) rolünden daha yüksek ayrıcalıklar gerektirir.

Bir hizmet sorumlusu kullanarak yeniden bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent reconnect --service-principal-id <serviceprincipalAppID> --service-principal-secret <serviceprincipalPassword> --tenant-id <tenantID>`

Erişim jetonu kullanarak yeniden bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent reconnect --access-token <accessToken>`

Yüksek oturum açmış kimlik bilgilerinizle (etkileşimli) yeniden bağlanmak için aşağıdaki komutu çalıştırın:

`azcmagent reconnect --tenant-id <tenantID>`

## <a name="remove-the-agent"></a>Aracıyı kaldırma

Windows veya Linux'a Bağlı Makine aracısını makineden kaldırmak için aşağıdaki yöntemlerden birini gerçekleştirin. Aracıyı kaldırmak, makineyi sunucular için Arc ile birlikte kaldırmaz (önizleme), bu, makineyi Azure'da yönetmeniz gerektiğinde gerçekleştirdiğiniz ayrı bir işlemdir.

### <a name="windows-agent"></a>Windows aracısı

Aşağıdaki yöntemlerin her ikisi de aracıyı kaldırır, ancak makinedeki *C:\Program Files\AzureConnectedMachineAgent* klasörünü kaldırmaz.

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

> [!NOTE]
> Aracıyı kaldırmak için *kök* erişim izinlerine veya Sudo kullanarak hakları yüksek bir hesaba sahip olmalısınız.

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

## <a name="unregister-machine"></a>Kayıt dışı makine

Azure'da destekleyici hizmetlerle makineyi yönetmeyi durdurmayı planlıyorsanız, sunucular için Arc ile makinenin kaydını çıkarmak için aşağıdaki adımları gerçekleştirin (önizleme). Bağlı Makine aracısını makineden çıkarmadan önce veya sonra bu adımları gerçekleştirebilirsiniz.

1. [Azure portalına](https://aka.ms/hybridmachineportal)giderek sunucular için Azure Arc'ı açın (önizleme).

2. Listedeki makineyi seçin, elipsleri seçin (**...**), ve sonra **Sil'i**seçin.
