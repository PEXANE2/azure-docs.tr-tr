---
title: Windows sanal masaüstü 'nde uzak masaüstü istemci bağlantıları-Azure
description: Windows sanal masaüstü kiracı ortamında istemci bağlantılarını ayarlarken oluşan sorunları çözme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 04/08/2019
ms.author: helohr
ms.openlocfilehash: 4f325d9fc512fd9f6776fcd799b720aaf60ce472
ms.sourcegitcommit: b3bad696c2b776d018d9f06b6e27bffaa3c0d9c3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2019
ms.locfileid: "69876757"
---
# <a name="remote-desktop-client-connections"></a>Uzak Masaüstü istemcisi bağlantıları

Windows sanal masaüstü istemci bağlantılarıyla ilgili sorunları gidermek için bu makaleyi kullanın.

## <a name="provide-feedback"></a>Geri bildirimde bulunma

Şu anda Windows sanal masaüstü önizlemedeyken destek talebi sunulmamaktadır. Windows Sanal Masaüstü hizmetini ürün ekibi ve etkin topluluk üyeleriyle tartışmak için [Windows sanal masaüstü teknoloji Community](https://techcommunity.microsoft.com/t5/Windows-Virtual-Desktop/bd-p/WindowsVirtualDesktop) 'yi ziyaret edin.

## <a name="you-cant-open-a-web-client"></a>Bir Web istemcisi açamazsınız

Başka bir Web sitesi açarak internet bağlantısı olduğunu onaylayın; Örneğin, [www.Bing.com](https://www.bing.com).

DNS 'in FQDN 'yi çözediğini onaylamak için **nslookup** kullanın:

```cmd
nslookup rdweb.wvd.microsoft.com
```

Windows 7 veya Windows 10 için Uzak Masaüstü istemcisi gibi başka bir istemciyle bağlanmayı deneyin ve Web istemcisini açıp açmediğinizi denetleyin.

### <a name="error-opening-another-site-fails"></a>Hata: Başka bir site açma başarısız

**Sağlamak** Ağ sorunları ve/veya kesintiler.

**Onar** Ağ desteğiyle iletişim kurun.

### <a name="error-nslookup-cannot-resolve-the-name"></a>Hata: Nslookup adı çözümleyemiyor

**Sağlamak** Ağ sorunları ve/veya kesintiler.

**Onar** Ağ desteğiyle iletişim kurun

### <a name="error-you-cant-connect-but-other-clients-can-connect"></a>Hata: Bağlanamazsınız, ancak diğer istemciler bağlanabilir

**Sağlamak** Tarayıcı beklenildiği gibi çalışmıyor ve çalışmayı durdurdu.

**Onar** Tarayıcıda sorun gidermek için bu yönergeleri izleyin.

1. Tarayıcıyı yeniden başlatın.
2. Tarayıcı tanımlama bilgilerini temizleyin. Bkz. [Internet Explorer 'da tanımlama bilgisi dosyalarını silme](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
3. Tarayıcı önbelleğini temizleyin. [Tarayıcınızın tarayıcı önbelleğini temizleme](https://binged.it/2RKyfdU)bölümüne bakın.
4. Tarayıcıyı özel modda açın.

## <a name="web-client-stops-responding-or-disconnects"></a>Web istemcisi yanıt vermeyi durduruyor veya bağlantıyı keser

Başka bir tarayıcı veya istemci kullanarak bağlanmayı deneyin.

### <a name="error-other-browsers-and-clients-also-malfunction-or-fail-to-open"></a>Hata: Diğer tarayıcılar ve istemciler de hatalı veya açılmayabilir

**Sağlamak** Ağ ve/veya işlem sistemi sorunları veya kesintiler

**Onar** Destek ekipleriyle iletişim kurun.

## <a name="web-client-keeps-prompting-for-credentials"></a>Web istemcisi kimlik bilgileri istemini tutar

Web istemcisi kimlik bilgilerini sormaya devam ederse, bu yönergeleri izleyin.

1. Web istemcisi URL 'sinin doğru olduğundan emin olun.
2. Kimlik bilgilerinin, URL 'ye bağlı Windows sanal masaüstü ortamı için olduğunu doğrulayın.
3. Tarayıcı tanımlama bilgilerini temizleyin. Bkz. [Internet Explorer 'da tanımlama bilgisi dosyalarını silme](https://support.microsoft.com/help/278835/how-to-delete-cookie-files-in-internet-explorer).
4. Tarayıcı önbelleğini temizleyin. [Tarayıcınızın tarayıcı önbelleğini temizleme](https://binged.it/2RKyfdU)bölümüne bakın.
5. Tarayıcıyı özel modda açın.

## <a name="remote-desktop-client-for-windows-7-or-windows-10-stops-responding-or-cannot-be-opened"></a>Windows 7 veya Windows 10 için Uzak Masaüstü istemcisi yanıt vermeyi durduruyor veya açılamıyor

Bant dışı (OOB) istemci kayıt defterlerini temizlemek için aşağıdaki PowerShell cmdlet 'lerini kullanın.

```PowerShell
Remove-ItemProperty 'HKCU:\Software\Microsoft\Terminal Server Client\Default' - Name FeedURLs

#Remove RdClientRadc registry key
Remove-Item 'HKCU:\Software\Microsoft\RdClientRadc' -Recurse

#Remove all files under %appdata%\RdClientRadc
Remove-Item C:\Users\pavithir\AppData\Roaming\RdClientRadc\* -Recurse
```

**%AppData%\RdClientRadc** adresine gidin ve tüm içeriği silin.

Windows 7 ve Windows 10 için uzak masaüstü istemcisini kaldırın ve yeniden yükleyin.

## <a name="troubleshooting-end-user-connectivity"></a>Son Kullanıcı bağlantısı sorunlarını giderme

Bazen kullanıcılar kendi akışına ve yerel kaynaklarına erişebilir, ancak yine de uzak kaynaklara erişmesini önleyen yapılandırma, kullanılabilirlik veya performans sorunlarına sahip olabilir. Bu durumlarda, Kullanıcı şuna benzer iletileri alır:

![Uzak Masaüstü Bağlantısı hata iletisi.](media/eb76b666808bddb611448dfb621152ce.png)

![Ağ Geçidi hata iletisine bağlanılamıyor.](media/a8fbb9910d4672147335550affe58481.png)

İstemci bağlantı hata kodları için bu genel sorun giderme yönergelerini izleyin.

1. Sorun yaşandığında Kullanıcı adını ve saatini onaylayın.
2. **PowerShell** 'i açın ve sorunun bildirildiği Windows sanal masaüstü kiracısıyla bağlantı kurun.
3. **Get-RdsTenant** ile doğru kiracıya bağlantıyı onaylayın.
4. **Get-RdsHostPool** ve **Get-RdsSessionHost** cmdlet 'lerini kullanarak, sorun gidermenin doğru konak havuzunda yapıldığını onaylayın.
5. Belirtilen zaman penceresi için bağlantı türü başarısız olan tüm etkinliklerin listesini almak için aşağıdaki komutu yürütün:

    ```PowerShell
     Get-RdsDiagnosticActivities -TenantName <TenantName> -username <UPN> -StartTime
     "11/21/2018 1:07:03 PM" -EndTime "11/21/2018 1:27:03 PM" -Outcome Failure -ActivityType Connection
    ```

6. Önceki cmdlet çıktısından **ActivityId** kullanarak aşağıdaki komutu çalıştırın:

    ```PowerShell
    (Get-RdsDiagnosticActivities -TenantName $tenant -ActivityId <ActivityId> -Detailed).Errors
    ```

7. Komut aşağıda gösterilen çıktıya benzer bir çıktı üretir. Kök nedenin sorunlarını gidermek için **Errorcodesembolik** ve **ErrorMessage** kullanın.

    ```PowerShell
    ErrorSource       : <Source>
    ErrorOperation    : <Operation>
    ErrorCode         : <Error code>
    ErrorCodeSymbolic : <Error code string>
    ErrorMessage      : <Error code message>
    ErrorInternal     : <Internal for the OS>
    ReportedBy        : <Reported by component>
    Time              : <Timestampt>
    ```

### <a name="error-o_add_user_to_group_failed--failed-to-add-user--username-to-group--remote-desktop-users-reason-win32error_no_such_member"></a>Hata: O_ADD_USER_TO_GROUP_FAILED/Kullanıcı = ≤ Kullanıcı adı ≥, Grup = uzak masaüstü kullanıcıları 'na eklenemedi. Yüzden Win32. ERROR_NO_SUCH_MEMBER

**Sağlamak** VM, Kullanıcı nesnesinin bulunduğu etki alanına katılmadı.

**Onar** Sanal makineyi doğru etki alanına ekleyin. Bkz. [Windows Server sanal makinesini yönetilen bir etki alanına katma](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-admin-guide-join-windows-vm-portal).

### <a name="error-nslookup-cannot-resolve-the-name"></a>Hata: Nslookup adı çözümleyemiyor

**Sağlamak** Ağ sorunları veya kesintiler.

**Onar** Ağ desteğiyle iletişim kurun

### <a name="error-connectionfailedclientprotocolerror"></a>Hata: ConnectionFailedClientProtocolError

**Sağlamak** Kullanıcının bağlanmaya çalışan sanal makineler, etki alanına katılmış değildir.

**Onar** Bir konak havuzunun parçası olan tüm VM 'Leri etki alanı denetleyicisine ekleyin.

## <a name="user-connects-but-nothing-is-displayed-no-feed"></a>Kullanıcı bağlanıyor, ancak hiçbir şey görüntülenmiyor (akış yok)

Kullanıcı, uzak masaüstü istemcileri başlatabilir ve kimlik doğrulaması yapabilir, ancak kullanıcı Web bulma akışında herhangi bir simge görmez.

Bu komut satırını kullanarak, sorunları raporlayan kullanıcının uygulama gruplarına atandığını doğrulayın:

```PowerShell
Get-RdsAppGroupUser <tenantname> <hostpoolname> <appgroupname>
```

Kullanıcının doğru kimlik bilgileriyle oturum açmasını onaylayın.

Web istemcisi kullanılıyorsa, önbelleğe alınmış kimlik bilgileri sorunları olduğunu doğrulayın.

## <a name="next-steps"></a>Sonraki adımlar

- Windows sanal masaüstü ve yükseltme izlemelerinin sorunlarını giderme hakkında genel bilgi için bkz. [sorun giderme genel bakış, geri bildirim ve destek](troubleshoot-set-up-overview.md).
- Bir Windows sanal masaüstü ortamında kiracı ve konak havuzu oluştururken oluşan sorunları gidermek için bkz. [kiracı ve konak havuzu oluşturma](troubleshoot-set-up-issues.md).
- Windows sanal masaüstündeki bir sanal makineyi (VM) yapılandırırken oluşan sorunları gidermek için bkz. [oturum ana bilgisayarı sanal makine yapılandırması](troubleshoot-vm-configuration.md).
- Windows sanal masaüstü ile PowerShell kullanırken karşılaşılan sorunları gidermek için bkz. [Windows sanal masaüstü PowerShell](troubleshoot-powershell.md).
- Önizleme hizmeti hakkında daha fazla bilgi edinmek için bkz. [Windows sanal masaüstü önizleme ortamı](https://docs.microsoft.com/azure/virtual-desktop/environment-setup?).
- Sorun giderme öğreticisine geçmek için bkz [. Öğretici: Kaynak Yöneticisi şablonu dağıtımlarıyla](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-tutorial-troubleshoot)ilgili sorunları giderin.
- Denetim eylemleri hakkında bilgi edinmek için bkz. [Kaynak Yöneticisi Ile denetim işlemleri](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-audit).
- Dağıtım sırasında hataları belirleme eylemleri hakkında bilgi edinmek için bkz. [dağıtım Işlemlerini görüntüleme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-deployment-operations).
