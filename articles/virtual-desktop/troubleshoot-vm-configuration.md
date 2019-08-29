---
title: Windows sanal masaüstü-Azure 'da kiracı ve konak havuzu oluşturma
description: Bir Windows sanal masaüstü ortamında kiracı ve oturum ana bilgisayar sanal makinesini (VM) yapılandırırken oluşan sorunları çözme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 07/10/2019
ms.author: helohr
ms.openlocfilehash: f797d3ee525806d8002b19edb1378d0376508b08
ms.sourcegitcommit: 82499878a3d2a33a02a751d6e6e3800adbfa8c13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70073925"
---
# <a name="tenant-and-host-pool-creation"></a>Kiracı ve ana bilgisayar havuzu oluşturma

Windows sanal masaüstü oturumu ana bilgisayarı sanal makinelerini (VM 'Ler) yapılandırırken karşılaştığınız sorunları gidermek için bu makaleyi kullanın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Şu anda Windows sanal masaüstü önizlemedeyken destek talebi sunulmamaktadır. Windows Sanal Masaüstü hizmetini ürün ekibi ve etkin topluluk üyeleriyle tartışmak için [Windows sanal masaüstü teknoloji Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) 'yi ziyaret edin.

## <a name="vms-are-not-joined-to-the-domain"></a>VM 'Ler etki alanına katılmamış

VM 'Leri etki alanına eklerken sorun yaşıyorsanız bu yönergeleri izleyin.

- [Windows Server sanal makinesini yönetilen bir etki alanına ekleme](https://docs.microsoft.com/azure/active-directory-domain-services/Active-directory-ds-admin-guide-join-windows-vm-portal) veya [etki alanına ekleme şablonunu](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)kullanma içindeki işlemi kullanarak VM 'yi el ile birleştirin.
- VM 'de komut satırından etki alanı adının ping komutunu deneyin.
- Etki alanına ekleme [hata Iletileriyle Ilgili sorunları gidermek](https://social.technet.microsoft.com/wiki/contents/articles/1935.troubleshooting-domain-join-error-messages.aspx)için etki alanına katılması hata iletileri listesini gözden geçirin.

### <a name="error-incorrect-credentials"></a>Hata: Yanlış kimlik bilgileri

**Sağlamak** Azure Resource Manager şablon arabirimi düzeltmeleriyle kimlik bilgileri girildiğinde oluşturulan bir yazım hatası vardı.

**Onar** Çözümlemek için aşağıdaki eylemlerden birini gerçekleştirin.

- VM 'Leri bir etki alanına el ile ekleyin.
- Kimlik bilgileri onaylandıktan sonra şablonu yeniden dağıtın. Bkz. [PowerShell ile konak havuzu oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell).
- [Mevcut bir WINDOWS sanal MAKINESINI ad etki alanına birleştiren](https://azure.microsoft.com/resources/templates/201-vm-domain-join-existing/)bir şablon kullanarak VM 'leri etki alanına katın.

### <a name="error-timeout-waiting-for-user-input"></a>Hata: Kullanıcı girişi beklenirken zaman aşımı

**Sağlamak** Etki alanı katılımı tamamlamaya yönelik hesap Multi-Factor Authentication 'a (MFA) sahip olabilir.

**Onar** Çözümlemek için aşağıdaki eylemlerden birini gerçekleştirin.

- Hesap için MFA 'yı geçici olarak kaldırın.
- Bir hizmet hesabı kullanın.

### <a name="error-the-account-used-during-provisioning-doesnt-have-permissions-to-complete-the-operation"></a>Hata: Sağlama sırasında kullanılan hesabın işlemi tamamlamaya yönelik izinleri yok

**Sağlamak** Kullanılmakta olan hesabın, uyumluluk ve yönetmelikler nedeniyle VM 'Leri etki alanına katma izni yok.

**Onar** Çözümlemek için aşağıdaki eylemlerden birini gerçekleştirin.

- Yönetici grubunun üyesi olan bir hesap kullanın.
- Kullanılan hesaba gerekli izinleri verin.

### <a name="error-domain-name-doesnt-resolve"></a>Hata: Etki alanı adı çözümlenmiyor

**Neden 1:** VM 'Ler, etki alanının bulunduğu sanal ağ (VNET) ile ilişkilendirilmemiş bir sanal ağ üzerinde bulunuyor.

**1. Çözüm:** VM 'Lerin sağlandığı VNET ile etki alanı denetleyicisinin (DC) çalıştığı VNET arasında VNET eşlemesi oluşturun. Bkz. [sanal ağ eşlemesi oluşturma-kaynak yöneticisi, farklı abonelikler](https://docs.microsoft.com/azure/virtual-network/create-peering-different-subscriptions).

**Neden 2:** Azure Active Directory Domain Services (Azure AD DS) kullanırken, sanal ağın DNS sunucusu ayarları, yönetilen etki alanı denetleyicilerini işaret etmek üzere güncellenir.

**2. Çözüm:** Azure AD DS içeren sanal ağın DNS ayarlarını güncelleştirmek için bkz. [Azure sanal ağı IÇIN DNS ayarlarını güncelleştirme](https://docs.microsoft.com/azure/active-directory-domain-services/tutorial-create-instance#update-dns-settings-for-the-azure-virtual-network).

**Neden 3:** Ağ arabiriminin DNS sunucusu ayarları, sanal ağda uygun DNS sunucusunu göstermiyor.

**3. Çözüm:** Çözümlemek için, [DNS sunucularını değiştirme] bölümündeki adımları izleyerek aşağıdaki eylemlerden birini gerçekleştirin.
- Ağ arabiriminin DNS sunucusu ayarlarını, [DNS sunucularını değiştirme](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#change-dns-servers) adımları ile **özel** olarak DEĞIŞTIRIN ve sanal ağda DNS sunucularının özel IP adreslerini belirtin.
- Ağ arabiriminin DNS sunucusu ayarlarını, [DNS sunucularını değiştirme](https://docs.microsoft.com/azure/virtual-network/virtual-network-network-interface#change-dns-servers)adımları ile **sanal ağdan devralacak** şekilde değiştirin, ardından sanal ağın DNS sunucusu ayarlarını [değiştirme DNS sunucuları](https://docs.microsoft.com/azure/virtual-network/manage-virtual-network#change-dns-servers)adımlarından adımları değiştirin.

## <a name="windows-virtual-desktop-agent-and-windows-virtual-desktop-boot-loader-are-not-installed"></a>Windows sanal masaüstü Aracısı ve Windows sanal masaüstü önyükleme yükleyicisi yüklü değil

VM 'Leri sağlamak için önerilen yol, **Windows sanal masaüstü konak havuzu şablonu oluşturma ve sağlama** Azure Resource Manager kullanmaktır. Şablon, Windows sanal masaüstü Aracısı ve Windows sanal masaüstü Aracısı önyükleme yükleyicisini otomatik olarak yükler.

Bileşenlerin yüklendiğini doğrulamak ve hata iletilerini denetlemek için bu yönergeleri izleyin.

1. **Denetim Masası** > programlarProgramlar > **ve Özellikler ' i**denetleyerek iki bileşenin yüklendiğini onaylayın. **Windows sanal masaüstü Aracısı** ve **Windows sanal masaüstü Aracısı önyükleme yükleyicisi** görünür değilse, VM 'de yüklü değildir.
2. **Dosya Gezgini** 'ni açın ve **C:\windows\temp\scriptlogs.log**konumuna gidin. Dosya eksikse, iki bileşeni yükleyen PowerShell DSC 'nin belirtilen güvenlik bağlamında çalıştırılmadığını gösterir.
3. **C:\windows\temp\scriptlogs.log** dosyası varsa, açın ve hata iletilerini denetleyin.

### <a name="error-windows-virtual-desktop-agent-and-windows-virtual-desktop-agent-boot-loader-are-missing-cwindowstempscriptlogslog-is-also-missing"></a>Hata: Windows sanal masaüstü Aracısı ve Windows sanal masaüstü Aracısı önyükleme yükleyicisi eksik. C:\Windows\Temp\scriptlogs.log de eksik

**Neden 1:** Azure Resource Manager şablonu için giriş sırasında belirtilen kimlik bilgileri yanlış veya izinler yetersiz.

**1. Çözüm:** [PowerShell ile konak havuzu oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell)' yı kullanarak eksik bileşenleri VM 'lere el ile ekleyin.

**Neden 2:** PowerShell DSC, Windows sanal masaüstü 'nde oturum açıp gerekli bilgileri alabilmesi için başlatma ve yürütme işlemi başarısız oldu, ancak tamamlanamayacak.

**2. Çözüm:** Aşağıdaki listedeki öğeleri onaylayın.

- Hesabın MFA içermediğinden emin olun.
- Kiracı adının doğru olduğunu ve kiracının Windows sanal masaüstü 'nde mevcut olduğunu doğrulayın.
- Hesabın en az RDS katkıda bulunan izinleri olduğunu onaylayın.

### <a name="error-authentication-failed-error-in-cwindowstempscriptlogslog"></a>Hata: Kimlik doğrulama başarısız, C:\Windows\Temp\scriptlogs.log hatası

**Sağlamak** PowerShell DSC 'nin yürütülmesi, ancak Windows sanal masaüstüne bağlanamadık.

**Onar** Aşağıdaki listedeki öğeleri onaylayın.

- VM 'Leri Windows sanal masaüstü hizmeti ile el ile kaydedin.
- Windows sanal masaüstüne bağlanmak için kullanılan hesabın, kiracı üzerinde konak havuzları oluşturma izinlerine sahip olduğunu doğrulayın.
- Onaylama hesabında MFA yok.

## <a name="windows-virtual-desktop-agent-is-not-registering-with-the-windows-virtual-desktop-service"></a>Windows sanal masaüstü Aracısı Windows sanal masaüstü hizmeti 'ne kaydolmadı

Windows sanal masaüstü Aracısı, oturum ana bilgisayar VM 'lerine ilk kez yüklendiğinde (el ile veya Azure Resource Manager şablonu ve PowerShell DSC aracılığıyla), bir kayıt belirteci sağlar. Aşağıdaki bölümde, Windows sanal masaüstü Aracısı ve belirteci için geçerli olan sorun giderme sorunları ele alınmaktadır.

### <a name="error-the-status-filed-in-get-rdssessionhost-cmdlet-shows-status-as-unavailable"></a>Hata: Get-RdsSessionHost cmdlet 'inde dosyalanmış durum durumu kullanılamıyor olarak gösterir

![Get-RdsSessionHost cmdlet 'i, durumu kullanım dışı olarak gösterir.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Sağlamak** Aracı kendisini yeni bir sürüme güncelleştiremeyebilir.

**Onar** Aracıyı el ile güncelleştirmek için bu yönergeleri izleyin.

1. Oturum Ana bilgisayar VM 'sinde aracının yeni bir sürümünü indirin.
2. Görev Yöneticisi 'Ni başlatın ve hizmet sekmesinde Rdadgentönyükleme Yükleyicisi hizmetini durdurun.
3. Windows sanal masaüstü aracısının yeni sürümü için yükleyiciyi çalıştırın.
4. Kayıt belirteci sorulduğunda, INVALID_TOKEN girdisini kaldırın ve ileri 'ye basın (yeni bir belirteç gerekli değildir).
5. Yükleme sihirbazını doldurun.
6. Görev Yöneticisi 'Ni açın ve Rdadgentönyükleme Yükleyicisi hizmetini başlatın.

## <a name="error--windows-virtual-desktop-agent-registry-entry-isregistered-shows-a-value-of-0"></a>Hata:  Windows sanal masaüstü Aracısı kayıt defteri girdisi ıregistered 0 değerini gösterir

**Sağlamak** Kayıt belirtecinin süresi doldu veya süre sonu 999999 ile üretildi.

**Onar** Aracı kayıt defteri hatasını onarmak için bu yönergeleri izleyin.

1. Zaten bir kayıt belirteci varsa Remove-RDSRegistrationInfo ile kaldırın.
2. RDS-Newregistrationınfo ile yeni belirteç oluştur.
3. -ExpriationHours parametresinin 72 olarak ayarlandığını onaylayın (en büyük değer 99999 ' dir).

### <a name="error-windows-virtual-desktop-agent-isnt-reporting-a-heartbeat-when-running-get-rdssessionhost"></a>Hata: Windows sanal masaüstü Aracısı, Get-RdsSessionHost çalıştırılırken bir sinyal bildirmiyor

**Neden 1:** Rdavgentönyükleme Yükleyicisi hizmeti durduruldu.

**1. Çözüm:** Görev Yöneticisi 'Ni başlatın ve hizmet sekmesi Rdadgentönyükleme Yükleyicisi hizmeti için durdurulmuş bir durum bildirirse hizmeti başlatın.

**Neden 2:** 443 numaralı bağlantı noktası kapatılabilir.

**2. Çözüm:** 443 numaralı bağlantı noktasını açmak için bu yönergeleri izleyin.

1. PSPing aracını [SysInternal araçlarından](https://docs.microsoft.com/sysinternals/downloads/psping)indirerek bağlantı noktası 443 ' nin açık olduğunu doğrulayın.
2. Aracının çalıştığı oturum ana bilgisayar VM 'sine PSPing 'yi yükler.
3. Komut istemi 'ni yönetici olarak açın ve aşağıdaki komutu verin:

    ```cmd
    psping rdbroker.wvdselfhost.microsoft.com:443
    ```

4. Rdsping 'nin bilgileri RDBroker 'dan geri aldığını onaylayın:

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

## <a name="troubleshooting-issues-with-the-windows-virtual-desktop-side-by-side-stack"></a>Windows sanal masaüstü yan yana yığınıyla ilgili sorunları giderme

Windows sanal masaüstü yan yana yığın Windows Server 2019 ile otomatik olarak yüklenir. Microsoft Windows Server 2016 veya Windows Server 2012 R2 'ye yan yana yığın yüklemek için Microsoft yükleyicisi 'ni (MSI) kullanın. Microsoft Windows 10 ' da, Windows sanal masaüstü yan yana yığın, **enablesxstackrons. ps1**ile etkinleştirilir.

Yan yana yığının, oturum ana bilgisayar havuzu VM 'lerinde yüklü veya etkin olduğu başlıca üç yol vardır:

- Azure Resource Manager **Yeni Windows sanal masaüstü konak havuzu şablonu oluşturma ve sağlama**
- Ana görüntüde dahil edilip etkin olarak
- Her VM 'de el ile yüklenmiş veya etkinleştirilmiş (veya uzantılar/PowerShell ile)

Windows sanal masaüstü 'Nün yan yana Stack ile ilgili sorun yaşıyorsanız, yan yana yığının yüklendiğini veya etkinleştirildiğini doğrulamak için komut isteminden **qwinsta** komutunu yazın.

Yan yana yığın yüklenip etkinleştirilirse, **qwinsta** çıktısı çıktıda **RDP-sxs** ' i listeler.

![Yan yana yığın, çıktıda RDP-sxs olarak listelenen qwinsta yüklü veya etkin.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

Aşağıda listelenen kayıt defteri girişlerini inceleyin ve değerlerinin eşleştiğini doğrulayın. Kayıt defteri anahtarları eksikse veya değerler uyuştubulunursa, yan yana yığının nasıl yeniden yükleneceğini öğrenmek için [PowerShell ile bir konak havuzu oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) ' daki yönergeleri izleyin.

```registry
    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\WinStations\rds-sxs\"fEnableWinstation":DWORD=1

    HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Control\Terminal
    Server\ClusterSettings\"SessionDirectoryListener":rdp-sxs
```

### <a name="error-o_reverse_connect_stack_failure"></a>Hata: O_REVERSE_CONNECT_STACK_FAILURE

![O_REVERSE_CONNECT_STACK_FAILURE hata kodu.](media/23b8e5f525bb4e24494ab7f159fa6b62.png)

**Sağlamak** Yan yana yığın, oturum ana bilgisayar VM 'sinde yüklü değil.

**Onar** Oturum Ana bilgisayar VM 'sine yan yana yığın yüklemek için bu yönergeleri izleyin.

1. Doğrudan oturum ana bilgisayar VM 'sine yerel yönetici olarak almak için Uzak Masaüstü Protokolü (RDP) kullanın.
2. Henüz yapmadıysanız PowerShell oturumunuzda kullanmak üzere [Windows sanal masaüstü PowerShell modülünü](https://docs.microsoft.com/powershell/windows-virtual-desktop/overview) indirin ve içeri aktarın.
3. [PowerShell ile bir konak havuzu oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell)' yı kullanarak yan yana yığını yükler.

## <a name="how-to-fix-a-windows-virtual-desktop-side-by-side-stack-that-malfunctions"></a>Düzgün bir şekilde bir Windows sanal masaüstü yan yana yığınını çözme

Yan yana yığının hatalı çalışmasına neden olabilecek bilinen koşullar vardır:

- Yan yana yığını etkinleştirmek için adımların doğru sırasını takip etmez
- Windows 10 geliştirilmiş çok yönlü disk (EVD) için otomatik güncelleştirme
- Uzak Masaüstü Oturumu Ana Bilgisayarı (RDSH) rolü eksik
- Enablesxsstackrc. ps1 birden çok kez çalıştırılıyor
- Yerel yönetici ayrıcalıklarına sahip olmayan bir hesapta enablesxsstackronc. ps1 çalıştırma

Bu bölümdeki yönergeler, Windows sanal masaüstü 'Nün yan yana yığınını kaldırmanızı sağlamanıza yardımcı olabilir. Yan yana yığını kaldırdıktan sonra yan yana yığını yeniden yüklemek için [PowerShell ile bir konak havuzu oluşturma](https://docs.microsoft.com/azure/virtual-desktop/create-host-pools-powershell) bölümünde "VM 'Yi Windows sanal masaüstü ana bilgisayar havuzuna Kaydet" bölümüne gidin.

Düzeltmeyi çalıştırmak için kullanılan VM 'nin, hatalı çalışan yan yana Stack ile aynı alt ağda ve etki alanında olması gerekir.

Aynı alt ağ ve etki alanından düzeltmeyi çalıştırmak için aşağıdaki yönergeleri izleyin:

1. Standart Uzak Masaüstü Protokolü (RDP) ile, bu, düzeltmesinin uygulanacağı VM 'ye bağlanın.
2. PsExec 'yi şuradan https://docs.microsoft.com/sysinternals/downloads/psexec indirin.
3. İndirilen dosyayı sıkıştırmayı açın.
4. Komut istemi 'ni yerel yönetici olarak başlatın.
5. PsExec 'nin sıkıştırısaklandığı klasöre gidin.
6. Komut isteminden aşağıdaki komutu kullanın:

    ```cmd
            psexec.exe \\<VMname> cmd
    ```

    >[!Note]
    >VMname, hatalı çalışan yan yana yığın ile VM 'nin makine adıdır.

7. Kabul et 'e tıklayarak PsExec lisans sözleşmesini kabul edin.

    ![Yazılım Lisans Sözleşmesi ekran görüntüsü.](media/SoftwareLicenseTerms.png)

    >[!Note]
    >Bu iletişim kutusu yalnızca PsExec ilk kez çalıştırıldığında görünür.

8. Komut istemi oturumu, hatalı çalışan yan yana Stack ile sanal makinede açıldıktan sonra qwinsta komutunu çalıştırın ve RDP-sxs adlı bir girdinin kullanılabilir olduğunu onaylayın. Aksi halde, sorun yan yana yığına bağlı olmadığından, sanal makinede yan yana bir yığın yok.

    ![Yönetici komut istemi](media/AdministratorCommandPrompt.png)

9. Aşağıdaki komutu çalıştırarak, sanal makinede yüklü olan Microsoft bileşenlerini hatalı yan yana yığın ile listeleyin.

    ```cmd
        wmic product get name
    ```

10. Yukarıdaki adımdaki ürün adlarıyla aşağıdaki komutu çalıştırın.

    ```cmd
        wmic product where name="<Remote Desktop Services Infrastructure Agent>" call uninstall
    ```

11. "Uzak Masaüstü" ile başlayan tüm ürünleri kaldırın.

12. Tüm Windows Sanal Masaüstü bileşenleri kaldırıldıktan sonra, işletim sisteminiz için yönergeleri izleyin:

13. İşletim sisteminiz Windows Server ise, hatalı çalışan yan yana yığına sahip olan VM 'yi (Azure portal veya PsExec aracından) yeniden başlatın.

İşletim sisteminiz Microsoft Windows 10 ise aşağıdaki yönergelerle devam edin:

14. PsExec çalıştıran VM 'den, dosya Gezgini 'ni açın ve disablesxsstackronc. ps1 dosyasını VM 'nin sistem sürücüsüne kopyalayın ve bu arada bir yan yana yığın.

    ```cmd
        \\<VMname>\c$\
    ```

    >[!NOTE]
    >VMname, hatalı çalışan yan yana yığın ile VM 'nin makine adıdır.

15. Önerilen işlem: PsExec aracından PowerShell 'i başlatın ve önceki adımda bulunan klasöre gidin ve disablesxsstackronc. ps1 ' yi çalıştırın. Alternatif olarak, aşağıdaki cmdlet 'leri çalıştırabilirsiniz:

    ```PowerShell
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\ClusterSettings" -Name "SessionDirectoryListener" -Force
    Remove-Item -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations\rdp-sxs" -Recurse -Force
    Remove-ItemProperty -Path "HKLM:\SYSTEM\CurrentControlSet\Control\Terminal Server\WinStations" -Name "ReverseConnectionListener" -Force
    ```

16. Cmdlet 'ler çalışmayı tamamladıktan sonra, arızalı yan yana yığın ile sanal makineyi yeniden başlatın.

## <a name="remote-licensing-model-is-not-configured"></a>Uzaktan lisanslama modeli yapılandırılmadı

Bir yönetici hesabı kullanarak Windows 10 Enterprise çoklu oturumunda oturum açarsanız, "Uzak Masaüstü lisans modu yapılandırılmadı, Uzak Masaüstü Hizmetleri X gün içinde çalışmayı durduracaktır. Bağlantı Aracısı sunucusunda, Uzak Masaüstü lisans modunu belirtmek için Sunucu Yöneticisi kullanın. " Bu iletiyi görürseniz, bu, lisans modunu **Kullanıcı başına**el ile yapılandırmanız gerektiği anlamına gelir.

Lisanslama modunu el ile yapılandırmak için:  

1. **Başlangıç menüsü** arama kutusuna gidip yerel Grup İlkesi düzenleyicisine erişmek için **gpedit. msc** dosyasını bulup açın. 
2.  > **Windows bileşenleri** ****  > Uzak Masaüstü Hizmetleri Yönetim ŞablonlarıBilgisayarYapılandırması'nagidinUzakMasaüstüoturumuanabilgisayarı >  >  >  **Lisanslama**. 
3. **Uzak Masaüstü lisans modunu ayarla** ' yı seçin ve **Kullanıcı başına**olarak değiştirin.

Şu anda bildirim ve yetkisiz kullanım süresi zaman aşımı sorunlarını bakıyoruz ve gelecekteki bir güncelleştirmede ele almak için plan yapıyoruz. 

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme izlemelerinin sorunlarını giderme hakkında genel bilgi için bkz. [sorun giderme genel bakış, geri bildirim ve destek](troubleshoot-set-up-overview.md).
- Bir Windows sanal masaüstü ortamında kiracı ve konak havuzu oluştururken oluşan sorunları gidermek için bkz. [kiracı ve konak havuzu oluşturma](troubleshoot-set-up-issues.md).
- Windows sanal masaüstündeki bir sanal makineyi (VM) yapılandırırken oluşan sorunları gidermek için bkz. [oturum ana bilgisayarı sanal makine yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için bkz. [Uzak Masaüstü istemci bağlantıları](troubleshoot-client-connection.md).
- Windows sanal masaüstü ile PowerShell kullanırken karşılaşılan sorunları gidermek için bkz. [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Önizleme hizmeti hakkında daha fazla bilgi edinmek için bkz. [Windows sanal masaüstü önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup).
- Sorun giderme öğreticisine geçmek için bkz [. Öğretici: Kaynak Yöneticisi şablonu dağıtımlarıyla](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot)ilgili sorunları giderin.
- Denetim eylemleri hakkında bilgi edinmek için bkz. [Kaynak Yöneticisi Ile denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Dağıtım sırasında hataları belirleme eylemleri hakkında bilgi edinmek için bkz. [dağıtım Işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
