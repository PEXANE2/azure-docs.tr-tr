---
title: Sorun Giderme Windows Sanal Masaüstü oturum ana bilgisayar - Azure
description: Windows Sanal Masaüstü oturumunu sanal makineleri yapılandırırken sorunları nasıl gidereceğiniz.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 12/03/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: c7d9a5d576ceec301eba7436c1e0af34412ae854
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127594"
---
# <a name="session-host-virtual-machine-configuration"></a>Oturum ana bilgisayarı sanal makine yapılandırması

Windows Sanal Masaüstü oturumunu yapılandırırken yaşadığınız sorunları gidermek için bu makaleyi kullanın sanal makineler (VM'ler).

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Windows Sanal Masaüstü Hizmeti'ni ürün ekibi ve etkin topluluk üyeleriyle tartışmak için [Windows Sanal Masaüstü Teknik Topluluğu'nu](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) ziyaret edin.

## <a name="vms-are-not-joined-to-the-domain"></a>VM'ler etki alanına katılmaz

VM'leri etki alanına birleştirme sorunları yaşıyorsanız, bu yönergeleri izleyin.

- [Windows Server sanal makinesini yönetilen bir etki alanına katılma](../active-directory-domain-services/join-windows-vm.md) işlemini veya etki alanına katılma [şablonunu](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)kullanarak VM'ye el ile katılın.
- VM'deki komut satırından alan adını pingle'ı deneyin.
- [Sorun giderme Etki Alanı Birleştirme Hata İletileri'nde](https://social.technet.microsoft.com/wiki/contents/articles/1935.troubleshooting-domain-join-error-messages.aspx)etki alanına katılma hata iletilerinin listesini gözden geçirin.

### <a name="error-incorrect-credentials"></a>Hata: Yanlış kimlik bilgileri

**Sebep:** Kimlik bilgileri Azure Kaynak Yöneticisi şablon arabirimi düzeltmeleri girildiğinde bir yazım hatası yapıldı.

**Düzeltme:** Çözmek için aşağıdaki eylemlerden birini alın.

- VM'leri bir etki alanına el ile ekleyin.
- Kimlik bilgileri onaylandıktan sonra şablonu yeniden dağıtın. Bkz. [PowerShell ile ana bilgisayar havuzu oluşturun.](create-host-pools-powershell.md)
- [Varolan windows vm'den AD Etki Alanına Katılma'ya](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)sahip bir şablonkullanarak bir etki alanına VM'lere katılın.

### <a name="error-timeout-waiting-for-user-input"></a>Hata: Kullanıcı girişi için zaman ekini bekliyor

**Sebep:** Etki alanı birleştirmesini tamamlamak için kullanılan hesabın çok faktörlü kimlik doğrulaması (MFA) olabilir.

**Düzeltme:** Çözmek için aşağıdaki eylemlerden birini alın.

- Hesap için Geçici Olarak MFA'yı kaldırın.
- Bir hizmet hesabı kullanın.

### <a name="error-the-account-used-during-provisioning-doesnt-have-permissions-to-complete-the-operation"></a>Hata: Sağlama sırasında kullanılan hesabın işlemi tamamlama izni yok

**Sebep:** Kullanılan hesabın, uyumluluk ve düzenlemeler nedeniyle etki alanına VM'lere katılma izni yoktur.

**Düzeltme:** Çözmek için aşağıdaki eylemlerden birini alın.

- Yönetici grubunun üyesi olan bir hesap kullanın.
- Kullanılan hesaba gerekli izinleri ver.

### <a name="error-domain-name-doesnt-resolve"></a>Hata: Etki alanı adı çözülmüyor

**Neden 1:** Sanal M'ler, etki alanının bulunduğu sanal ağla (VNET) ilişkili olmayan bir sanal ağda dır.

**Düzeltme 1:** VM'lerin sağlandığı VNET ile etki alanı denetleyicisinin (DC) çalıştığı VNET arasında VNET eşlemi oluşturun. Bkz. [Sanal ağ eşleme oluşturma - Kaynak Yöneticisi, farklı abonelikler.](../virtual-network/create-peering-different-subscriptions.md)

**Neden 2:** Azure Active Directory Domain Services (Azure AD DS) kullanırken, sanal ağ dns sunucu ayarlarını yönetilen etki alanı denetleyicilerini işaret etmek üzere güncelleştirmiyor.

**Düzeltme 2:** Azure AD DS içeren sanal ağın DNS ayarlarını güncelleştirmek [için Azure sanal ağı için DNS ayarlarını güncelleştir'e](../active-directory-domain-services/tutorial-create-instance.md#update-dns-settings-for-the-azure-virtual-network)bakın.

**Neden 3:** Ağ arabiriminin DNS sunucu ayarları, sanal ağdaki uygun DNS sunucusuna işaret etmez.

**Düzeltme 3:** [DNS sunucularını değiştir]'deki adımları izleyerek çözmek için aşağıdaki eylemlerden birini alın.
- [DNS sunucularını değiştir](../virtual-network/virtual-network-network-interface.md#change-dns-servers) adımları ile ağ arabiriminin DNS sunucu ayarlarını **Özel** olarak değiştirin ve sanal ağdaki DNS sunucularının özel IP adreslerini belirtin.
- Ağ arabiriminin DNS sunucu [ayarlarını, DNS sunucularını değiştir](../virtual-network/virtual-network-network-interface.md#change-dns-servers)adımlarla **sanal ağdan devralmak** için değiştirin, ardından [DNS sunucularını değiştir'in](../virtual-network/manage-virtual-network.md#change-dns-servers)adımları ile sanal ağın DNS sunucu ayarlarını değiştirin.

## <a name="windows-virtual-desktop-agent-and-windows-virtual-desktop-boot-loader-are-not-installed"></a>Windows Sanal Masaüstü Aracısı ve Windows Sanal Masaüstü Önyükleme Yükleyici yüklü değil

VM'leri sağlamanın önerilen yolu, Azure Kaynak Yöneticisi **Oluştur ve Windows Sanal Masaüstü ana bilgisayar havuzu** şablonu sağlamaktır. Şablon otomatik olarak Windows Sanal Masaüstü Aracısı ve Windows Sanal Masaüstü Aracısı Boot Loader yükler.

Bileşenlerin yüklü olduğunu onaylamak ve hata iletilerini denetlemek için bu yönergeleri izleyin.

1. **Kontrol Paneli** > **Programları Programları** > **ve Özellikleri'ni**denetleyerek iki bileşenin yüklü olduğunu doğrulayın. **Windows Sanal Masaüstü Aracısı** ve Windows Sanal Masaüstü **Aracısı Boot Loader** görünmezse, VM'de yüklü değildir.
2. **Dosya Gezgini'ni** açın ve **C:\Windows\Temp\ScriptLog.log'a**gidin. Dosya eksikse, iki bileşeni yükleyen PowerShell DSC'nin sağlanan güvenlik bağlamında çalışmadığını gösterir.
3. **C:\Windows\Temp\ScriptLog.log** dosyası varsa, açın ve hata iletilerini denetleyin.

### <a name="error-windows-virtual-desktop-agent-and-windows-virtual-desktop-agent-boot-loader-are-missing-cwindowstempscriptloglog-is-also-missing"></a>Hata: Windows Sanal Masaüstü Aracısı ve Windows Sanal Masaüstü Aracısı Boot Loader eksik. C:\Windows\Temp\ScriptLog.log da eksik

**Neden 1:** Azure Kaynak Yöneticisi şablonu için giriş sırasında sağlanan kimlik bilgileri yanlış veya izinler yetersizdi.

**Düzeltme 1:** PowerShell ile ana bilgisayar havuzu [oluştur'u](create-host-pools-powershell.md)kullanarak eksik bileşenleri VM'lere el ile ekleyin.

**Neden 2:** PowerShell DSC başlatıp yürütebildi ancak Windows Sanal Masaüstü'nde oturum açamadığı ve gerekli bilgileri elde edemediği için tamamlayamadı.

**Düzeltme 2:** Aşağıdaki listedeki öğeleri onaylayın.

- Hesabın MFA olmadığından emin olun.
- Kiracı adının doğru olduğunu ve kiracının Windows Sanal Masaüstü'nde olduğunu doğrulayın.
- Hesabın en az RDS Katılımcısı izni olduğunu onaylayın.

### <a name="error-authentication-failed-error-in-cwindowstempscriptloglog"></a>Hata: Kimlik doğrulama başarısız oldu, C:\Windows\Temp\ScriptLog.log hatası

**Sebep:** PowerShell DSC yürütmeyi başardı ancak Windows Sanal Masaüstü'ne bağlanamadı.

**Düzeltme:** Aşağıdaki listedeki öğeleri onaylayın.

- VM'leri Windows Sanal Masaüstü hizmetine el ile kaydedin.
- Windows Sanal Masaüstüne bağlanmak için kullanılan hesabı onaylayın, ana bilgisayar havuzları oluşturmak için kiracıda izinler vardır.
- HesapMFA'sı olmadığını onaylayın.

## <a name="windows-virtual-desktop-agent-is-not-registering-with-the-windows-virtual-desktop-service"></a>Windows Sanal Masaüstü Aracısı Windows Sanal Masaüstü hizmetine kaydetmiyor

Windows Sanal Masaüstü Aracısı ilk oturum ana bilgisayar VM'lerine (el ile veya Azure Kaynak Yöneticisi şablonu ve PowerShell DSC aracılığıyla) yüklendiğinde, bir kayıt belirteci sağlar. Aşağıdaki bölümde, Windows Sanal Masaüstü Aracısı ve belirteç için geçerli olan sorun giderme sorunları ele aluyulmaktadır.

### <a name="error-the-status-filed-in-get-rdssessionhost-cmdlet-shows-status-as-unavailable"></a>Hata: Get-RdsSessionHost cmdlet dosyasında dosyalanan durum kullanılamayan olarak durumu gösterir

![Get-RdsSessionHost cmdlet kullanılamayan olarak durumunu gösterir.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Sebep:** Aracı kendisini yeni bir sürüme güncelleştiremez.

**Düzeltme:** Aracıyı el ile güncelleştirmek için bu yönergeleri izleyin.

1. Oturum ana bilgisayar VM'deki aracının yeni bir sürümünü indirin.
2. Görev Yöneticisi'ni başlatın ve Hizmet Sekmesinde RDAgentBootLoader hizmetini durdurun.
3. Windows Sanal Masaüstü Aracısı'nın yeni sürümü için yükleyiciyi çalıştırın.
4. Kayıt belirteci için istendiğinde, giriş INVALID_TOKEN kaldırın ve sonrakine basın (yeni bir belirteç gerekmez).
5. Yükleme Sihirbazı'nı tamamlayın.
6. Görev Yöneticisi'ni açın ve RDAgentBootLoader hizmetini başlatın.

## <a name="error--windows-virtual-desktop-agent-registry-entry-isregistered-shows-a-value-of-0"></a>Hata: Windows Virtual Desktop Agent kayıt defteri giriş Kayıtlı 0 değerini gösterir

**Sebep:** Kayıt belirteci süresi doldu veya 999999 son kullanma değeri ile oluşturuldu.

**Düzeltme:** Aracı kayıt defteri hatasını düzeltmek için aşağıdaki yönergeleri izleyin.

1. Zaten bir kayıt belirteci varsa, remove-RDSRegistrationInfo ile kaldırın.
2. Rds-NewRegistrationInfo ile yeni belirteç oluşturun.
3. -ExpriationHours parametresinin 72 olarak ayarlendiğini doğrulayın (maksimum değer 99999'dur).

### <a name="error-windows-virtual-desktop-agent-isnt-reporting-a-heartbeat-when-running-get-rdssessionhost"></a>Hata: Windows Sanal Masaüstü aracısı Get-RdsSessionHost çalıştırırken bir sinyal bildiriyor değil

**Neden 1:** RDAgentBootLoader hizmeti durduruldu.

**Düzeltme 1:** Görev Yöneticisi başlatın ve Hizmet Sekmesi RDAgentBootLoader hizmeti için durdurulan bir durum bildiriyorsa, hizmeti başlatın.

**Neden 2:** Port 443 kapalı olabilir.

**Düzeltme 2:** Bağlantı noktası 443'ün açmak için bu yönergeleri izleyin.

1. Onay bağlantı noktası 443 [Sysinternal araçlardan](/sysinternals/downloads/psping/)PSPing aracı indirerek açık.
2. Aracının çalıştığı oturum ana bilgisayarı VM'ye PSPing'i yükleyin.
3. Komut istemini yönetici olarak açın ve aşağıdaki komutu sorun:

    ```cmd
    psping rdbroker.wvdselfhost.microsoft.com:443
    ```

4. PSPing'in RDBroker'dan bilgi aldığını doğrulayın:

    ```
    PsPing v2.10 - PsPing - ping, latency, bandwidth measurement utility
    Copyright (C) 2012-2016 Mark Russinovich
    Sysinternals - www.sysinternals.com
    TCP connect to 13.77.160.237:443:
    5 iterations (warmup 1) ping test:
    Connecting to 13.77.160.237:443 (warmup): from 172.20.17.140:60649: 2.00ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60650: 3.83ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60652: 2.21ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60653: 2.14ms
    Connecting to 13.77.160.237:443: from 172.20.17.140:60654: 2.12ms
    TCP connect statistics for 13.77.160.237:443:
    Sent = 4, Received = 4, Lost = 0 (0% loss),
    Minimum = 2.12ms, Maximum = 3.83ms, Average = 2.58ms
    ```

## <a name="troubleshooting-issues-with-the-windows-virtual-desktop-side-by-side-stack"></a>Windows Sanal Masaüstü yan yana yığınla ilgili sorun giderme sorunları

Windows Virtual Desktop yan yana yığın Windows Server 2019 ile otomatik olarak yüklenir. Microsoft Windows Server 2016 veya Windows Server 2012 R2'ye yan yana yığını yüklemek için Microsoft Installer'ı (MSI) kullanın. Microsoft Windows 10 için, Windows Sanal Masaüstü yan yana yığını **enablesxstackrs.ps1**ile etkinleştirilir.

Oturum ana bilgisayar havuzu VM'lerde yan yana yığının yüklenmesi veya etkinleştirilmesinin üç ana yolu vardır:

- Azure Kaynak Yöneticisi **Oluşturma ve yeni Windows Sanal Masaüstü ana bilgisayar havuzu** şablonu sağlama ile
- Ana görüntüye dahil edilip etkinleştirilerek
- Her VM'ye el ile yüklenmiş veya etkinleştirilmiş (veya uzantılar/PowerShell ile)

Windows Sanal Masaüstü yan yana yığınla ilgili sorunlar yaşıyorsanız, yan yana yığının yüklü veya etkin olduğunu doğrulamak için komut isteminden **qwinsta** komutunu yazın.

Yan yana yığın kurulup etkinleştirilmişse **qwinsta** çıktısı çıkışta **rdp-sx'leri** listeler.

![Çıkışta rdp-sxs olarak listelenen qwinsta ile yan yana yığın yüklenir veya etkinleştirilmiş.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

Aşağıda listelenen kayıt defteri girişlerini inceleyin ve değerlerinin eşleşip eşleşmediğini onaylayın. Kayıt defteri anahtarları eksikse veya değerler uyumsuzsa, [PowerShell'e sahip ana bilgisayar havuzu oluştur'da](create-host-pools-powershell.md) yan yana yığının nasıl yeniden yüklenacağına ilişkin yönergeleri izleyin.

```registry
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\WinStations\rds-sxs\"fEnableWinstation":DWORD=1

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\ClusterSettings\"SessionDirectoryListener":rdp-sxs
```

### <a name="error-o_reverse_connect_stack_failure"></a>Hata: O_REVERSE_CONNECT_STACK_FAILURE

![O_REVERSE_CONNECT_STACK_FAILURE hata kodu.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Sebep:** Oturum ana bilgisayar VM'de yan yana yığın yüklenmiyor.

**Düzeltme:** Oturum ana bilgisayar VM'ye yan yana yığın yüklemek için aşağıdaki yönergeleri izleyin.

1. Yerel yönetici olarak oturum ana bilgisayar VM doğrudan almak için Uzak Masaüstü Protokolü (RDP) kullanın.
2. PowerShell oturumunuzda kullanmak üzere [Windows Virtual Desktop PowerShell modülünü](/powershell/windows-virtual-desktop/overview/) indirin ve alın, ardından hesabınızda oturum açmanız için bu cmdlet'i çalıştırın:

    ```powershell
    Add-RdsAccount -DeploymentUrl "https://rdbroker.wvd.microsoft.com"
    ```

3. PowerShell ile ana bilgisayar havuzu oluştur'u kullanarak yan yana yığını [yükleyin.](create-host-pools-powershell.md)

## <a name="how-to-fix-a-windows-virtual-desktop-side-by-side-stack-that-malfunctions"></a>Windows Sanal Masaüstü arızaları yan yana yığın nasıl düzeltilir?

Yan yana yığının arızaya neden olabilecek bilinen durumlar vardır:

- Yan yana yığını etkinleştirmek için adımların doğru sırasını takip etme
- Windows 10 Gelişmiş Çok Yönlü Disk (EVD) için otomatik güncelleme
- Uzak Masaüstü Oturum Ana Bilgisayar (RDSH) rolünü eksik
- Çalışan enablesxsstackrc.ps1 birden çok kez
- Yerel yönetici ayrıcalıkları olmayan bir hesapta enablesxsstackrc.ps1 çalıştırma

Bu bölümdeki yönergeler, Windows Sanal Masaüstü'nü yan yana kaldırmanıza yardımcı olabilir. Yan yana yığını kaldırdıktan sonra, yan yana yığını yeniden yüklemek için [PowerShell ile ana bilgisayar havuzu oluştur'da](create-host-pools-powershell.md) "VM'yi Windows Sanal Masaüstü ana bilgisayar havuzuyla kaydet" adlı ana bilgisayara gidin.

Düzeltmeyi çalıştırmak için kullanılan VM, arızalı yan yana yığınla VM ile aynı alt ağ ve etki alanında olmalıdır.

Aynı alt ağ ve etki alanından düzeltme çalıştırmak için aşağıdaki yönergeleri izleyin:

1. Düzeltmenin uygulanacağı yerden VM'ye standart Uzak Masaüstü Protokolü (RDP) ile bağlanın.
2. PsExec'i https://docs.microsoft.com/sysinternals/downloads/psexecburadan indirin.
3. İndirilen dosyanın zip'ini açın.
4. Yerel yönetici olarak komut istemini başlatın.
5. PsExec'in fermuarını açtığı klasöre gidin.
6. Komut istemi'nden aşağıdaki komutu kullanın:

    ```cmd
            psexec.exe \\<VMname> cmd
    ```

    >[!Note]
    >VMname, arızalı yan yana yığına sahip VM'nin makine adıdır.

7. Kabul et'i tıklayarak PsExec Lisans Sözleşmesini kabul edin.

    ![Yazılım lisans sözleşmesi ekran görüntüsü.](media/SoftwareLicenseTerms.png)

    >[!Note]
    >Bu iletişim yalnızca PsExec ilk kez çalıştırılında gösterecektir.

8. VM'de arızalı yan yana yığınla komut istemi oturumu açıldıktan sonra qwinsta çalıştırın ve rdp-sxs adlı bir girişin kullanılabilir olduğunu onaylayın. Değilse, vm'de yan yana yığın bulunmadığından, sorun yan yana yığına bağlı değildir.

    ![Yönetici komut istemi](media/AdministratorCommandPrompt.png)

9. VM'de yüklü Olan Microsoft bileşenlerini yan yana yığınla listeleyen aşağıdaki komutu çalıştırın.

    ```cmd
        wmic product get name
    ```

10. Yukarıdaki adımdan ürün adlarıyla aşağıdaki komutu çalıştırın.

    ```cmd
        wmic product where name="<Remote Desktop Services Infrastructure Agent>" call uninstall
    ```

11. "Uzak Masaüstü" ile başlayan tüm ürünleri kaldırın.

12. Tüm Windows Sanal Masaüstü bileşenleri kaldırıldıktan sonra, işletim sisteminizin yönergelerini izleyin:

13. İşletim sisteminiz Windows Server ise, arızalı yan yana yığına sahip VM'yi yeniden başlatın (Azure portalı yla veya PsExec aracından).

İşletim sisteminiz Microsoft Windows 10 ise, aşağıdaki yönergeleri uygulayın:

14. PsExec çalıştıran VM'den Dosya Gezgini'ni açın ve arızalı yan yana yığınla VM'nin sistem sürücüsüne disablesxsstackrc.ps1 kopyalayın.

    ```cmd
        \\<VMname>\c$\
    ```

    >[!NOTE]
    >VMname, arızalı yan yana yığına sahip VM'nin makine adıdır.

15. Önerilen işlem: PsExec aracından PowerShell'i başlatın ve önceki adımdan klasöre gidin ve devre dışı bırakıp devre dışı bırakıp devre dışı bırakıp devre dışı bırakıp.ps1 çalıştırın. Alternatif olarak, aşağıdaki cmdlets çalıştırabilirsiniz:

    ```PowerShell
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\ClusterSettings" -Name "SessionDirectoryListener" -Force
    Remove-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" -Recurse -Force
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations" -Name "ReverseConnectionListener" -Force
    ```

16. Cmdlets çalışan bittiğinde, arızalı yan yana yığını ile VM yeniden başlatın.

## <a name="remote-desktop-licensing-mode-isnt-configured"></a>Uzak Masaüstü lisans modu yapılandırılmamıştır

Windows 10 Enterprise'da bir yönetim hesabı kullanarak oturum açtığınızda, "Uzak Masaüstü lisanslama modu yapılandırılmamış, Uzak Masaüstü Hizmetleri X gün içinde çalışmayı durdurur. Connection Broker sunucusunda, Uzak Masaüstü lisanslama modunu belirtmek için Server Manager'ı kullanın."

Zaman sınırı nın süresi dolduğunda, "Bu bilgisayar için kullanılabilir Uzak Masaüstü istemci erişim lisansları olmadığı için uzak oturum kesildi" yazan bir hata iletisi görüntülenir.

Bu iletilerden birini görürseniz, bu, görüntünün en son Windows güncelleştirmelerinin yüklü olmadığı veya grup ilkesi aracılığıyla Uzak Masaüstü lisanslama modunu ayarladığınız anlamına gelir. Grup ilkesi ayarını denetlemek, Windows 10 Enterprise çok oturumlu sürümünü tanımlamak ve ilgili güncelleştirmeyi yüklemek için sonraki bölümlerdeki adımları izleyin.  

>[!NOTE]
>Windows Sanal Masaüstü yalnızca ana bilgisayar havuzunuz Windows Server oturum ana bilgisayarları içeriyorsa RDS istemci erişim lisansı (CAL) gerektirir. RDS CAL'ı nasıl yapılandırıştırılamayı öğrenmek için [RDS dağıtımınızı istemci erişim lisanslarıyla lisansla](/windows-server/remote/remote-desktop-services/rds-client-access-license/)license'a bakın.

### <a name="disable-the-remote-desktop-licensing-mode-group-policy-setting"></a>Uzak Masaüstü lisanslama modu grup ilkesi ayarını devre dışı bırakın

VM'de Grup İlkesi Düzenleyicisi'ni açarak ve **Yönetim Şablonları** > **Windows Components** > **Remote Desktop Services** > **Remote Desktop Session Host** > **Licensing** > **Set Remote Desktop licensing moduna**geçerek grup ilkesi ayarını kontrol edin. Grup ilkesi ayarı **Enabled**Etkinse, **devre dışı bırakılmış**olarak değiştirin. Zaten devre dışı bırakılmışsa, olduğu gibi bırakın.

>[!NOTE]
>Etki alanınız aracılığıyla grup ilkesi ayarlarsanız, bu Windows 10 Enterprise çok oturumlu VM'leri hedefleyen ilkeler deki bu ayarı devre dışı bırakın.

### <a name="identify-which-version-of-windows-10-enterprise-multi-session-youre-using"></a>Windows 10 Enterprise çoklu oturumunun hangi sürümünü kullandığınızı belirleyin

Windows 10 Enterprise çoklu oturumunun hangi sürümüne sahip olduğunuzu denetlemek için:

1. Yönetici hesabınızla oturum açın.
2. Başlat menüsünün yanındaki arama çubuğuna "Hakkında" girin.
3. **Pc'niz Hakkında'yı**seçin.
4. "Sürüm"un yanındaki numarayı kontrol edin. Sayı aşağıdaki resimde gösterildiği gibi "1809" veya "1903" olmalıdır.

    ![Windows belirtimleri penceresinin ekran görüntüsü. Sürüm numarası mavi ile vurgulanır.](media/windows-specifications.png)

Artık sürüm numaranızı bildiğinize göre, ilgili bölüme geçin.

### <a name="version-1809"></a>Sürüm 1809

Sürüm numaranızda "1809" yazıyorsa, [KB4516077 güncelleştirmesini yükleyin.](https://support.microsoft.com/help/4516077)

### <a name="version-1903"></a>Sürüm 1903

Ana bilgisayar işletim sistemini, Azure Galerisi'nden Windows 10'un en son sürümü olan sürüm 1903 görüntüsüyle yeniden dağıtın.

## <a name="next-steps"></a>Sonraki adımlar

- Windows Sanal Masaüstü sorun giderme ve yükseltme parçalarına genel bakış için [Sorun Giderme genel bakışı, geri bildirim ve desteğe](troubleshoot-set-up-overview.md)bakın.
- Windows Sanal Masaüstü ortamında kiracı ve ana bilgisayar havuzu oluştururken sorunları gidermek için [Bkz. Kiracı ve ana bilgisayar havuzu oluşturma.](troubleshoot-set-up-issues.md)
- Windows Sanal Masaüstü'nde sanal makine (VM) yapılandırırken sorunları gidermek için [Oturum ana bilgisayar sanal makine yapılandırmasına](troubleshoot-vm-configuration.md)bakın.
- Windows Sanal Masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için [Windows Sanal Masaüstü hizmet bağlantılarına](troubleshoot-service-connection.md)bakın.
- Uzak Masaüstü istemcileriyle ilgili sorunları gidermek için Bkz. [Uzak Masaüstü istemcisi sorun giderme](troubleshoot-client.md)
- Windows Virtual Desktop ile PowerShell kullanırken sorunları gidermek için [Windows Virtual Desktop PowerShell'e](troubleshoot-powershell.md)bakın.
- Hizmet hakkında daha fazla bilgi edinmek için [Windows Sanal Masaüstü ortamına](environment-setup.md)bakın.
- Bir sorun giderme öğreticisine geçmek için [Bkz. Öğretici: Kaynak Yöneticisi şablonu dağıtımları.](../azure-resource-manager/templates/template-tutorial-troubleshoot.md)
- Denetim eylemleri hakkında bilgi edinmek için [Kaynak Yöneticisi ile Denetim işlemlerine](../azure-resource-manager/management/view-activity-logs.md)bakın.
- Dağıtım sırasında hataları belirlemek için eylemler hakkında bilgi edinmek için [bkz.](../azure-resource-manager/templates/deployment-history.md)
