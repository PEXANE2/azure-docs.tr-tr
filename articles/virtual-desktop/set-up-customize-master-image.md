---
title: Ana VHD görüntüsü hazırlama ve özelleştirme - Azure
description: Windows Sanal Masaüstü ana görüntüsünü Azure'a hazırlama, özelleştirme ve yükleme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 10/14/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 867b327ac25d51cd3955e622da9d8067ae6d9ae9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127731"
---
# <a name="prepare-and-customize-a-master-vhd-image"></a>Ana VHD görüntüsünü hazırlama ve özelleştirme

Bu makalede, azure'a yüklemek için sanal bir sabit disk (VHD) resmi nasıl hazırlayacağınızı ve sanal makinelerin (VM'ler) nasıl oluşturulabileceğinizi ve üzerlerine yazılım yükleyin de dahildir. Bu yönergeler, kuruluşunuzun varolan işlemleriyle kullanılabilecek Windows Sanal Masaüstüne özgü bir yapılandırma içindir.

## <a name="create-a-vm"></a>VM oluşturma

Windows 10 Enterprise çoklu oturumlarını Azure Resim Galerisi'nde kullanılabilir. Bu görüntüyü özelleştirmek için iki seçenek vardır.

İlk seçenek, [yönetilen bir görüntüden VM oluştur'daki](../virtual-machines/windows/create-vm-generalized-managed.md)yönergeleri izleyerek Azure'da sanal bir makine (VM) sağlamak ve ardından [Yazılım hazırlama ve yüklemeye](set-up-customize-master-image.md#software-preparation-and-installation)geçmektir.

İkinci seçenek, görüntüyü indirerek, Hyper-V VM sağlayarak ve aşağıdaki bölümde ele aldığımız ihtiyaçlarınıza göre özelleştirerek görüntüyü yerel olarak oluşturmaktır.

### <a name="local-image-creation"></a>Yerel görüntü oluşturma

Görüntüyü yerel bir konuma indirdikten sonra, kopyaladiğiniz VHD ile bir VM oluşturmak için **Hyper-V Manager'ı** açın. Aşağıdaki talimatlar basit bir sürümüdür, ancak [Hyper-V'de sanal bir makine oluşturun'da](/windows-server/virtualization/hyper-v/get-started/create-a-virtual-machine-in-hyper-v/)daha ayrıntılı yönergeler bulabilirsiniz.

Kopyalanan VHD ile bir VM oluşturmak için:

1. Yeni **Sanal Makine Sihirbazı'nı**açın.

2. Oluşturma yı belirt sayfasında **Nesil 1'i**seçin.

    ![Oluşturmayı Belirt sayfasının ekran görüntüsü. "Nesil 1" seçeneği seçilir.](media/a41174fd41302a181e46385e1e701975.png)

3. Checkpoint Type altında, onay kutusunun denetimini açarak denetim noktalarını devre dışı kaldırın.

    ![Denetim Noktaları sayfasının Checkpoint Türü bölümünün ekran görüntüsü.](media/20c6dda51d7cafef33251188ae1c0c6a.png)

Kontrol noktalarını devre dışı bırakmak için PowerShell'de aşağıdaki cmdlet'i de çalıştırabilirsiniz.

```powershell
Set-VM -Name <VMNAME> -CheckpointType Disabled
```

### <a name="fixed-disk"></a>Sabit disk

Varolan bir VHD'den bir VM oluşturursanız, varsayılan olarak dinamik bir disk oluşturur. Aşağıdaki resimde gösterildiği gibi **Disk edit...** seçilerek sabit bir diske değiştirilebilir. Daha ayrıntılı talimatlar için azure'a [yüklemek için Windows VHD veya VHDX Hazırlama'ya](../virtual-machines/windows/prepare-for-upload-vhd-image.md)bakın.

![Diski Edit seçeneğinin ekran görüntüsü.](media/35772414b5a0f81f06f54065561d1414.png)

Diski sabit bir diske değiştirmek için aşağıdaki PowerShell cmdlet'i de çalıştırabilirsiniz.

```powershell
Convert-VHD –Path c:\\test\\MY-VM.vhdx –DestinationPath c:\\test\\MY-NEW-VM.vhd -VHDType Fixed
```

## <a name="software-preparation-and-installation"></a>Yazılım hazırlama ve yükleme

Bu bölümde FSLogix ve Windows Defender'ın nasıl hazırlanacağı ve yüklenirken, uygulamalar ve resminizin kayıt defteri için bazı temel yapılandırma seçenekleri yer almaktadır. 

VM'nize Office 365 ProPlus ve OneDrive yüklüyorsanız, [Office'i ana VHD görüntüsünde yükleyin](install-office-on-wvd-master-image.md) ve uygulamaları yüklemek için oradaki yönergeleri izleyin. İşin bittikten sonra, bu makaleye geri dön.

Kullanıcılarınızın belirli LOB uygulamalarına erişmeleri gerekiyorsa, bu bölümün yönergelerini tamamladıktan sonra bunları yüklemenizi öneririz.

### <a name="set-up-user-profile-container-fslogix"></a>Kullanıcı profil konteynerini ayarlama (FSLogix)

Görüntünün bir parçası olarak FSLogix kapsayıcısını eklemek [için, dosya paylaşımını kullanarak ana bilgisayar havuzu için profil kapsayıcısı oluşturma'daki](create-host-pools-user-profile.md#configure-the-fslogix-profile-container)yönergeleri izleyin. [Bu quickstart](/fslogix/configure-cloud-cache-tutorial/)ile FSLogix konteyner işlevselliğini test edebilirsiniz.

### <a name="configure-windows-defender"></a>Windows Defender'ı yapılandırma

Windows Defender VM'de yapılandırılırsa, ek sırasında VHD ve VHDX dosyalarının tüm içeriğini tarayıp tamayacağı şekilde yapılandırıldığından emin olun.

Bu yapılandırma yalnızca ek sırasında VHD ve VHDX dosyalarının taramayı kaldırır, ancak gerçek zamanlı tarama etkilemez.

Windows Server'da Windows Defender'ı yapılandırmak için daha ayrıntılı talimatlar için Windows [Server'da Windows Defender Antivirus hariç tutmaişlemlerini yapılandırın'](/windows/security/threat-protection/windows-defender-antivirus/configure-server-exclusions-windows-defender-antivirus/)a bakın.

Belirli dosyaları taramadan çıkarmak için Windows Defender'ı nasıl yapılandıracağı hakkında daha fazla bilgi edinmek için, [dosya uzantısı ve klasör konumuna bağlı olarak dışlamaları yapılandırve doğrulayın.](/windows/security/threat-protection/windows-defender-antivirus/configure-extension-file-exclusions-windows-defender-antivirus/)

### <a name="disable-automatic-updates"></a>Otomatik Güncelleştirmeleri Devre Dışı

Yerel Grup İlkesi aracılığıyla Otomatik Güncelleştirmeleri devre dışı kılabilir:

1. Açık **Yerel Grup\\İlkesi Düzenleyicisi Yönetici Şablonları\\Windows Bileşenleri\\Windows Update**.
2. **Otomatik Güncelleştirmeyi Yapılandır'a** sağ tıklayın ve **Devre Dışı bırakılmış**olarak ayarlayın.

Otomatik Güncelleştirmeleri devre dışı kakmak için komut isteminde aşağıdaki komutu da çalıştırabilirsiniz.

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\WindowsUpdate\AU" /v NoAutoUpdate /t REG_DWORD /d 1 /f
```

### <a name="specify-start-layout-for-windows-10-pcs-optional"></a>Windows 10 bilgisayarların başlangıç düzenini belirtin (isteğe bağlı)

Windows 10 bilgisayarların başlangıç düzeni belirtmek için bu komutu çalıştırın.

```batch
reg add "HKLM\SOFTWARE\Microsoft\Windows\CurrentVersion\Explorer" /v SpecialRoamingOverrideAllowed /t REG_DWORD /d 1 /f
```

### <a name="set-up-time-zone-redirection"></a>Saat dilimi yeniden yönlendirmeyi ayarlama

Ana bilgisayar havuzundaki tüm VM'ler aynı güvenlik grubunun parçası olduğundan, saat dilimi yeniden yönlendirmesi Grup İlkesi düzeyinde zorlanabilir.

Saat dilimlerini yönlendirmek için:

1. Active Directory sunucusunda **Grup İlkesi Yönetim Konsolu'nu**açın.
2. Etki alanınızı ve Grup İlkesi Nesnelerinizi genişletin.
3. Grup ilkesi ayarları için oluşturduğunuz **Grup İlkesi** Nesnesi'ni sağ tıklatın ve **Edit'i**seçin.
4. Grup **İlkesi Yönetimi**Düzenleyicisi, **Bilgisayar Yapılandırma** > **İlkeleri** > **Yönetim Şablonları** > **Windows Bileşenleri** > **Uzak Masaüstü Hizmetleri** > **Uzak Masaüstü Oturum Ana Bilgisayar** > **Aygıt ve Kaynak Yönlendirme**gidin.
5. İzin **saati dilimi yeniden yönlendirme** ayarını etkinleştirin.

Saat dilimlerini yeniden yönlendirmek için bu komutu ana resimde de çalıştırabilirsiniz:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Terminal Services" /v fEnableTimeZoneRedirection /t REG_DWORD /d 1 /f
```

### <a name="disable-storage-sense"></a>Depolama Duyusunu Devre Dışı

Windows 10 Enterprise veya Windows 10 Enterprise çoklu oturumunu kullanan Windows Sanal Masaüstü oturum ana bilgisayarı için Depolama Algılama'yı devre dışı bırakmanızı öneririz. Aşağıdaki ekran görüntüsünde gösterildiği gibi Depolama altındaki Ayarlar menüsünde **Depolama**Duyusunu devre dışı kullanabilirsiniz:

![Ayarlar altında Depolama menüsünün ekran görüntüsü. "Depolama duyusu" seçeneği kapatılır.](media/storagesense.png)

Aşağıdaki komutu çalıştırarak kayıt defteri ile ayarı değiştirebilirsiniz:

```batch
reg add "HKCU\Software\Microsoft\Windows\CurrentVersion\StorageSense\Parameters\StoragePolicy" /v 01 /t REG_DWORD /d 0 /f
```

### <a name="include-additional-language-support"></a>Ek dil desteği ekleme

Bu makalede, dil ve bölgesel destek yapılandırmak için nasıl kapsamaz. Daha fazla bilgi için aşağıdaki makalelere bakın:

- [Windows görüntülerine dil ekleme](/windows-hardware/manufacture/desktop/add-language-packs-to-windows/)
- [İsteğe bağlı özellikler](/windows-hardware/manufacture/desktop/features-on-demand-v2--capabilities/)
- [Talep üzerine dil ve bölge özellikleri (FOD)](/windows-hardware/manufacture/desktop/features-on-demand-language-fod/)

### <a name="other-applications-and-registry-configuration"></a>Diğer uygulamalar ve kayıt defteri yapılandırması

Bu bölüm uygulama ve işletim sistemi yapılandırması kapsar. Bu bölümdeki tüm yapılandırma, komut satırı ve regedit araçları yla yürütülebilen kayıt defteri girişleri aracılığıyla yapılır.

>[!NOTE]
>Grup İlkesi Nesneleri (GpOs) veya kayıt defteri içeri aktarımları ile yapılandırmada en iyi uygulamaları uygulayabilirsiniz. Yönetici, kuruluş gereksinimlerini temel alan her iki seçeneği de seçebilir.

Windows 10 Enterprise çoklu oturumunda telemetri verilerinin geri bildirim merkezi koleksiyonu için şu komutu çalıştırın:

```batch
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows\DataCollection" /v AllowTelemetry /t REG_DWORD /d 3 /f
```

Watson çökmelerini düzeltmek için aşağıdaki komutu çalıştırın:

```batch
remove CorporateWerServer* from Computer\HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\Windows Error Reporting
```

5k çözünürlük desteğini düzeltmek için kayıt defteri düzenleyicisine aşağıdaki komutları girin. Yan yana yığını etkinleştirmek için önce komutları çalıştırmanız gerekir.

```batch
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxMonitors /t REG_DWORD /d 4 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxXResolution /t REG_DWORD /d 5120 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\RDP-Tcp" /v MaxYResolution /t REG_DWORD /d 2880 /f

reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxMonitors /t REG_DWORD /d 4 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxXResolution /t REG_DWORD /d 5120 /f
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" /v MaxYResolution /t REG_DWORD /d 2880 /f
```

## <a name="prepare-the-image-for-upload-to-azure"></a>Görüntüyü Azure'a yüklemek için hazırlama

Yapılandırmayı bitirdikten ve tüm uygulamaları yükledikten sonra, görüntüyü hazırlamak [için Azure'a yüklemek üzere Windows VHD veya VHDX Hazırla'daki](../virtual-machines/windows/prepare-for-upload-vhd-image.md) yönergeleri izleyin.

Görüntüyü yüklemeye hazırladıktan sonra, VM'nin kapalı veya ayrılmış durumda kaldığından emin olun.

## <a name="upload-master-image-to-a-storage-account-in-azure"></a>Azure'daki bir depolama hesabına ana resim yükleme

Bu bölüm yalnızca ana görüntü yerel olarak oluşturulduğunda geçerlidir.

Aşağıdaki talimatlar, ana resminizi bir Azure depolama hesabına nasıl yükleyeceğinizi size söyleyecektir. Zaten bir Azure depolama hesabınız yoksa, bir tane oluşturmak için [bu makaledeki](/azure/javascript/tutorial-vscode-static-website-node-03) yönergeleri izleyin.

1. Henüz yapmadıysanız VM görüntüsünü (VHD) Sabit'e dönüştürün. Görüntüyü Sabit'e dönüştürmezseniz, görüntüyü başarıyla oluşturamazsınız.

2. VHD'yi depolama hesabınızdaki bir blob kapsayıcısına yükleyin. [Depolama Gezgini aracı](https://azure.microsoft.com/features/storage-explorer/)ile hızlı bir şekilde yükleyebilirsiniz. Depolama Gezgini aracı hakkında daha fazla bilgi edinmek için [bu makaleye](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows)bakın.

    ![Microsoft Azure Depolama Gezgini Aracı'nın arama penceresinin ekran görüntüsü. "Upload .vhd veya vhdx dosyaları sayfa lekeleri olarak (önerilir)" onay kutusu seçilir.](media/897aa9a9b6acc0aa775c31e7fd82df02.png)

3. Ardından, tarayıcınızdaki Azure portalına gidin ve "Görüntüler" aramasını yapın. Aramanız, aşağıdaki ekran görüntüsünde gösterildiği gibi **resim oluştur** sayfasına yönlendirmelidir:

    ![Azure portalının resim oluşturma sayfasının görüntü için örnek değerlerle dolu ekran görüntüsü.](media/d3c840fe3e2430c8b9b1f44b27d2bf4f.png)

4. Görüntüyü oluşturduktan sonra, aşağıdaki ekran görüntüsündeki gibi bir bildirim görmeniz gerekir:

    !["Başarıyla oluşturulan resim" bildiriminin ekran görüntüsü.](media/1f41b7192824a2950718a2b7bb9e9d69.png)

## <a name="next-steps"></a>Sonraki adımlar

Artık bir resminiz olduğuna göre, ana bilgisayar havuzları oluşturabilir veya güncelleyebilirsiniz. Ana bilgisayar havuzlarının nasıl oluşturulup güncelleştirilen hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Azure Resource Manager şablonuyla ana bilgisayar havuzu oluşturma](create-host-pools-arm-template.md)
- [Öğretici: Azure Marketi ile ana bilgisayar havuzu oluşturma](create-host-pools-azure-marketplace.md)
- [PowerShell ile ana bilgisayar havuzu oluşturma](create-host-pools-powershell.md)
- [Dosya paylaşımı kullanarak ana bilgisayar havuzu için profil kapsayıcısı oluşturma](create-host-pools-user-profile.md)
- [Windows Sanal Masaüstü yük dengeleme yöntemini yapılandırma](configure-host-pool-load-balancing.md)
