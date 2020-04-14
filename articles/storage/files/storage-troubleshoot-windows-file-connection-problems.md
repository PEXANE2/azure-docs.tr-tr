---
title: Windows'da Azure Dosyaları sorunlarını giderme | Microsoft Dokümanlar
description: Windows'da Azure Dosyaları sorunlarını giderme
author: jeffpatt24
ms.service: storage
ms.topic: conceptual
ms.date: 01/02/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 3237fe7d87ad058f255d1c77cb6d814bcd1c292e
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81262256"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Windows’ta Azure Dosyalar sorunlarını giderme

Bu makalede, Windows istemcilerinden bağlandığınızda Microsoft Azure Dosyaları ile ilgili sık karşılaşılan sorunlar listelenir. Ayrıca, bu sorunlar için olası nedenleri ve çözümleri sağlar. Bu makaledeki sorun giderme adımlarına ek olarak, Windows istemci ortamının doğru ön koşullara sahip olduğundan emin olmak için [AzFileDiagnostics'i](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) de kullanabilirsiniz. AzFileDiagnostics, bu makalede bahsedilen belirtilerin çoğunun algılanmasını otomatikleştirir ve en iyi performansı elde etmek için ortamınızı ayarlamaya yardımcı olur. Bu bilgileri, Azure Dosyaları paylaşımlarına bağlanma/eşleme/birleştirme sorunlarıyla ilgili olarak size yardımcı olacak adımlar sağlayan [Sorun Giderici paylaşımlarında](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) da bulabilirsiniz.

<a id="error5"></a>
## <a name="error-5-when-you-mount-an-azure-file-share"></a>Azure dosya paylaşımını monte ettiğinizde hata 5

Bir dosya paylaşımını takmaya çalıştığınızda, aşağıdaki hatayı alabilirsiniz:

- Sistem hatası 5 oluştu. Erişim reddedildi.

### <a name="cause-1-unencrypted-communication-channel"></a>Neden 1: Şifresiz iletişim kanalı

Güvenlik nedeniyle, iletişim kanalı şifrelenmemişse ve bağlantı girişimi Azure dosya paylaşımlarının bulunduğu veri merkezinden yapılmıyorsa Azure dosya paylaşımlarına bağlantılar engellenir. Depolama hesabında [Güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı etkinleştirildiyse aynı veri merkezi içinde şifrelenmemiş bağlantılar da engellenebilir. Şifrelenmiş bir iletişim kanalının sağlanabilmesi için kullanıcının istemcisi SMB şifrelemesini desteklemelidir.

Her sistemin Windows 8, Windows Server 2012 ve üstü sürümleri şifrelemeyi destekleyen SMB 3.0 içeren isteklerle anlaşır.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

1. SMB şifrelemesini destekleyen bir istemciden (Windows 8, Windows Server 2012 veya sonrası) bağlanın veya Azure dosya paylaşımı için kullanılan Azure depolama hesabıyla aynı veri merkezindeki sanal bir makineden bağlanın.
2. İstemci Kobİ şifrelemesini desteklemiyorsa, [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı depolama hesabında devre dışı bırakıldığını doğrulayın.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Neden 2: Depolama hesabında sanal ağ veya güvenlik duvarı kuralları etkinleştirilir 

Depolama hesabında sanal ağ (VNET) ve güvenlik duvarı kuralları yapılandırıldıysa, istemci IP adresine veya sanal ağa erişim izni verilmediği sürece ağ trafiğinin erişimi reddedilir.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Depolama hesabında sanal ağ ve güvenlik duvarı kurallarının düzgün yapılandırıldığını doğrulayın. Sanal ağ veya güvenlik duvarı kurallarının soruna neden olup olmadığını test etmek için depolama hesabında **Tüm ağlardan erişime izin ver** ayarını geçici olarak değiştirin. Daha fazla bilgi edinmek için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security).

### <a name="cause-3-share-level-permissions-are-incorrect-when-using-identity-based-authentication"></a>Neden 3: Kimlik tabanlı kimlik doğrulama kullanırken paylaşım düzeyindeizinler yanlış

Kullanıcılar Active Directory (AD) veya Azure Active Directory Domain Services (Azure AD DS) kimlik doğrulaması kullanarak Azure dosya paylaşımına erişiyorsa, paylaşım düzeyindeizinler yanlışsa dosya paylaşımına erişim "Erişim reddedildi" hatasıyla başarısız olur. 

### <a name="solution-for-cause-3"></a>Neden 3 için çözüm

Paylaşım düzeyi izinlerini güncelleştirmek için bir [kimliğe erişim izinleri atay'ı](https://docs.microsoft.com/azure/storage/files/storage-files-identity-auth-active-directory-domain-service-enable#2-assign-access-permissions-to-an-identity)'na bakın.

<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Azure dosya paylaşımını monte ettiğinizde veya monte ettiğinizde Hata 53, Hata 67 veya Hata 87

Şirket içinde veya farklı bir veri merkezinden dosya paylaşımı nı monte etmeye çalıştığınızda aşağıdaki hataları alabilirsiniz:

- Sistem hatası 53 oluştu. Ağ yolu bulunamadı.
- Sistem hatası 67 oluştu. Ağ adı bulunamıyor.
- Sistem hatası 87 oluştu. Parametre yanlış.

### <a name="cause-1-port-445-is-blocked"></a>Neden 1: Bağlantı noktası 445 engellendi

Bir Azure Files veri merkezine giden bağlantı noktası 445 giden iletişim engellenirse sistem hatası 53 veya sistem hatası 67 oluşabilir. 445 numaralı bağlantı noktasından erişime izin veren veya erişimi engelleyen ISP'lerin özetini görmek için [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx)'e gidin.

Güvenlik duvarınızın veya ISS'nizin bağlantı noktası 445'i engelleyip `Test-NetConnection` engellememe mi gerektiğini kontrol etmek için [AzFileDiagnostics](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-a9fa1fe5) aracını veya cmdlet'i kullanın. 

Cmdlet'i `Test-NetConnection` kullanmak için Azure PowerShell modülünün yüklü olması gerekir, daha fazla bilgi için [Azure PowerShell modüllerini yükle'ye](/powershell/azure/install-Az-ps) bakın. `<your-storage-account-name>` ile `<your-resource-group-name>` yerine depolama hesabınızla ilgili bilgileri yazmayı unutmayın.

   
    $resourceGroupName = "<your-resource-group-name>"
    $storageAccountName = "<your-storage-account-name>"

    # This command requires you to be logged into your Azure account, run Login-AzAccount if you haven't
    # already logged in.
    $storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

    # The ComputerName, or host, is <storage-account>.file.core.windows.net for Azure Public Regions.
    # $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign clouds
    # or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
    Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
    
Bağlantı başarılı olursa şu çıktıyı görmeniz gerekir:
    
  
    ComputerName     : <your-storage-account-name>
    RemoteAddress    : <storage-account-ip-address>
    RemotePort       : 445
    InterfaceAlias   : <your-network-interface>
    SourceAddress    : <your-ip-address>
    TcpTestSucceeded : True
 

> [!Note]  
> Yukarıdaki komut, depolama hesabının geçerli IP adresini döndürür. Bu IP adresinin aynı kalacağı garanti edilmez ve bu adres herhangi bir zamanda değişebilir. Bu IP adresini betiklere veya güvenlik duvarına sabit şekilde kodlamayın.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

#### <a name="solution-1---use-azure-file-sync"></a>Çözüm 1 - Azure Dosya Eşitleme'yi kullanın
Azure Dosya Eşitlemi, şirket içi Windows Server'ınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürebilir. SMB, NFS ve FTPS dahil olmak üzere verilerinize yerel olarak erişmek için Windows Server'da kullanılabilen tüm protokolleri kullanabilirsiniz. Azure Dosya Eşitlemi bağlantı noktası 443 üzerinde çalışır ve böylece bağlantı noktası 445 engellenen istemcilerden Azure Dosyalarına erişmek için geçici çözüm olarak kullanılabilir. [Azure Dosya Eşitlemeyi'ni nasıl kuracağız öğrenin.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-extend-servers)

#### <a name="solution-2---use-vpn"></a>Çözüm 2 - VPN kullanın
Belirli Depolama Hesabınıza VPN ayarlayarak, trafik internet üzerinden aksine güvenli bir tünelden geçecektir. Windows'dan Azure Dosyalarına erişmek [için VPN'i kurmak](storage-files-configure-p2s-vpn-windows.md) için yönergeleri izleyin.

#### <a name="solution-3---unblock-port-445-with-help-of-your-ispit-admin"></a>Çözüm 3 - ISP/BT Yöneticinizin yardımıyla 445 numaralı bağlantı noktasının engelini kaldırın
[Azure IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)giden bağlantı noktası 445'i açmak için BT departmanınızla veya ISS ile çalışın.

#### <a name="solution-4---use-rest-api-based-tools-like-storage-explorerpowershell"></a>Çözüm 4 - Depolama Gezgini/Powershell gibi REST API tabanlı araçları kullanın
Azure Files, SMB'ye ek olarak REST'i de destekler. REST erişim port 443 (standart tcp) üzerinde çalışır. Zengin UI deneyimi sağlayan REST API kullanılarak yazılmış çeşitli araçlar vardır. [Depolama Explorer](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) bunlardan biridir. [Depolama Gezgini'ni indirip yükleyin](https://azure.microsoft.com/features/storage-explorer/) ve Azure Dosyaları tarafından desteklenen dosya paylaşımınıza bağlanın. Ayrıca [powershell](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-powershell) kullanabilirsiniz hangi kullanıcı REST API.

### <a name="cause-2-ntlmv1-is-enabled"></a>Neden 2: NTLMv1 etkin

NTLMv1 iletişimistemcide etkin ise sistem hatası 53 veya sistem hatası 87 oluşabilir. Azure Dosyalar yalnızca NTLMv2 kimlik doğrulamasını destekler. NTLMv1'in etkinleştirilmesi daha az güvenli bir istemci oluşturur. Bu nedenle Azure Dosyalar için iletişim engellenir. 

Hatanın nedeninin bu olup olmadığını saptamak için aşağıdaki kayıt defteri alt anahtarının 3 değerine ayarlandığını doğrulayın:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Daha fazla bilgi için TechNet'te [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) konusuna bakın.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Aşağıdaki kayıt defteri alt anahtarında **LmCompatibilityLevel** değerini varsayılan değeri olan 3'e döndürün:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816-not-enough-quota-is-available-to-process-this-command-when-you-copy-to-an-azure-file-share"></a>Bir Azure dosya paylaşımını kopyaladiğinizde hata 1816 "Bu komutu işlemek için yeterli kota yok"

### <a name="cause"></a>Nedeni

Hata 1816, dosya paylaşımının monte edildiği bilgisayarda bir dosya için izin verilen eşzamanlı açık tutamaçların üst sınırına ulaştığınızda gerçekleşir.

### <a name="solution"></a>Çözüm

Bazı tutamaçları kapatarak eşzamanlı açık tutamaçların sayısını azaltın ve sonra yeniden deneyin. Daha fazla bilgi için [Microsoft Azure Depolama performansı ve ölçeklenebilirlik denetim listesine](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)bakın.

Bir dosya paylaşımı, dizin veya dosya için açık tutamaçları görüntülemek için [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet'i kullanın.  

Dosya paylaşımı, dizin veya dosya için açık tutamaçları kapatmak için [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet'i kullanın.

> [!Note]  
> Get-AzStorageFileHandle ve Close-AzStorageFileHandle cmdlets Az PowerShell modül sürüm 2.4 veya daha sonra dahildir. En son Az PowerShell modüllerini yüklemek için Azure [PowerShell modüllerini yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)

<a id="noaaccessfailureportal"></a>
## <a name="error-no-access-when-you-try-to-access-or-delete-an-azure-file-share"></a>Azure Dosya Paylaşımına erişmeye veya silmeye çalıştığınızda "Erişim yok" hatası  
Portaldaki bir Azure dosya paylaşımına erişmeye veya silmeye çalıştığınızda aşağıdaki hatayı alabilirsiniz:

Erişim yok  
Hata kodu: 403 

### <a name="cause-1-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Neden 1: Depolama hesabında sanal ağ veya güvenlik duvarı kuralları etkinleştirilir

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

Depolama hesabında sanal ağ ve güvenlik duvarı kurallarının düzgün yapılandırıldığını doğrulayın. Sanal ağ veya güvenlik duvarı kurallarının soruna neden olup olmadığını test etmek için depolama hesabında **Tüm ağlardan erişime izin ver** ayarını geçici olarak değiştirin. Daha fazla bilgi edinmek için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security).

### <a name="cause-2-your-user-account-does-not-have-access-to-the-storage-account"></a>Neden 2: Kullanıcı hesabınızın depolama hesabına erişimi yok

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Azure dosya paylaşımının bulunduğu depolama hesabına göz atın, **Access denetimine (IAM)** tıklayın ve kullanıcı hesabınızın depolama hesabına erişimi olduğunu doğrulayın. Daha fazla bilgi edinmek [için, Rol Tabanlı Erişim Denetimi (RBAC) ile depolama hesabınızı nasıl güvene](https://docs.microsoft.com/azure/storage/blobs/security-recommendations#data-protection)alabildiğini öğrenin.

<a id="open-handles"></a>
## <a name="unable-to-delete-a-file-or-directory-in-an-azure-file-share"></a>Azure dosya paylaşımında dosya veya dizin silinemiyor
Bir dosyayı silmeye çalıştığınızda aşağıdaki hatayı alabilirsiniz:

Belirtilen kaynak bir SMB istemcisi tarafından silinmesi için işaretlenir.

### <a name="cause"></a>Nedeni
Bu sorun genellikle dosya veya dizin açık bir tanıtıcı varsa oluşur. 

### <a name="solution"></a>Çözüm

Kobİ istemcileri tüm açık tutamaçları kapattıysa ve sorun oluşmaya devam ederse, aşağıdakileri gerçekleştirin:

- Açık tutamaçları görüntülemek için [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet'i kullanın.

- Açık kolları kapatmak için [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet'i kullanın. 

> [!Note]  
> Get-AzStorageFileHandle ve Close-AzStorageFileHandle cmdlets Az PowerShell modül sürüm 2.4 veya daha sonra dahildir. En son Az PowerShell modüllerini yüklemek için Azure [PowerShell modüllerini yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Windows'da Azure Dosyaları ile yavaş dosya alışverişi

Dosyaları Azure Dosyası hizmetine aktarmayı denediğinizde performansın yavaş olduğunu görebilirsiniz.

- Belirli bir minimum G/Ç boyutu gereksiniminiz yoksa, optimum performans için 1 MiB'i G/Ç boyutu olarak kullanmanızı öneririz.
-   Yazmalarla birlikte genişletdiğiniz bir dosyanın son boyutunu biliyorsanız ve dosyadaki yazılmamış kuyruk sıfırlar içeriyorsa yazılımınızın uyumluluk sorunları yoksa, her yazıyı genişletme yazımı yapmak yerine dosya boyutunu önceden ayarlayın.
-   Doğru kopyalama yöntemini kullanın:
    -   İki dosya paylaşımı arasında herhangi bir aktarım için [AzCopy'yi](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) kullanın.
    -   Şirket içi bilgisayarda dosya paylaşımları arasında [Robocopy'yi](/azure/storage/files/storage-files-deployment-guide#robocopy) kullanın.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Windows 8.1 veya Windows Server 2012 R2 için dikkat edilecek noktalar

Windows 8.1 veya Windows Server 2012 R2 çalıştıran istemciler için [KB3114025](https://support.microsoft.com/help/3114025) düzeltmesinin yüklü olduğundan emin olun. Bu düzeltme, oluşturma ve kapatma tutamaçlarının performansını artırır.

Düzeltmenin yüklü olup olmadığını denetlemek için aşağıdaki komut dosyasını çalıştırabilirsiniz:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Hotfix yüklüyse, aşağıdaki çıktı görüntülenir:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Azure Marketi'ndeki Windows Server 2012 R2 görüntülerinin, Aralık 2015'ten itibaren varsayılan olarak kB3114025 ekine sahip olması.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer-or-this-pc"></a>"Bilgisayarım" veya "Bu Bilgisayar" içinde sürücü harfi bulunan klasör yok

Bir Azure dosya paylaşımını net kullanımı kullanarak yönetici olarak eşlerseniz, paylaşım eksik görünüyor.

### <a name="cause"></a>Nedeni

Varsayılan olarak, Windows File Explorer yönetici olarak çalışmaz. Net kullanımı bir yönetim komut isteminden çalıştırın, ağ sürücüsünün eşlerini yönetici olarak eşlersiniz. Eşlenen sürücüler kullanıcı merkezli olduğundan, oturum açan kullanıcı hesabı, farklı bir kullanıcı hesabının altına monte edilmişse sürücüleri görüntülemez.

### <a name="solution"></a>Çözüm
Paylaşımı yönetici olmayan bir komut satırından monte edin. Alternatif olarak, **EnableLinkedConnections** kayıt defteri değerini yapılandırmak için [bu TechNet konusunu](https://technet.microsoft.com/library/ee844140.aspx) takip edebilirsiniz.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Depolama hesabı bir ileri eğik çizgi içeriyorsa net kullanım komutu başarısız olur

### <a name="cause"></a>Nedeni

Net kullanım komutu, ileri eğik çizgi (/) komut satırı seçeneği olarak yorumlar. Kullanıcı hesabı adınız ileri eğik çizgiyle başlarsa, sürücü eşleme başarısız olur.

### <a name="solution"></a>Çözüm

Sorunu aşmak için aşağıdaki adımlardan birini kullanabilirsiniz:

- Aşağıdaki PowerShell komutunu çalıştırın:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc"`

  Toplu iş dosyasından, komutu şu şekilde çalıştırabilirsiniz:

  `Echo new-smbMapping ... | powershell -command –`

- Bu sorunu aşmak için anahtarın etrafına çift tırnak işaretleri koyun - ileri eğik çizgi ilk karakter olmadığı sürece. Eğer varsa, etkileşimli modu kullanın ve parolanızı ayrı ayrı girin veya ileri eğik çizgi ile başlamayan bir anahtar almak için anahtarlarınızı yeniden oluşturun.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Uygulama veya hizmet, monte edilmiş bir Azure Dosyaları sürücüsüne erişemiyor

### <a name="cause"></a>Nedeni

Sürücüler kullanıcı başına monte edilir. Uygulamanız veya hizmetiniz sürücüyü monte eden den farklı bir kullanıcı hesabı altında çalışıyorsa, uygulama sürücüyü görmez.

### <a name="solution"></a>Çözüm

Aşağıdaki çözümlerden birini kullanın:

-   Sürücüyü uygulamayı içeren aynı kullanıcı hesabından monte edin. PsExec gibi bir araç kullanabilirsiniz.
- Net kullanım komutunun kullanıcı adı ve parola parametrelerindeki depolama hesabı adını ve anahtarını geçirin.
- Kimlik bilgilerini Kimlik Bilgisi Yöneticisi'ne eklemek için cmdkey komutunu kullanın. Bunu servis hesabı bağlamı altında bir komut satırından, etkileşimli `runas`bir giriş yoluyla veya kullanarak gerçekleştirin.
  
  `cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>`
- Eşlenmiş bir sürücü mektubu kullanmadan payı doğrudan eşle. Bazı uygulamalar sürücü harfine düzgün bir şekilde yeniden bağlanmayabilir, bu nedenle tüm UNC yolunu kullanmak daha güvenilir olabilir. 

  `net use * \\storage-account-name.file.core.windows.net\share`

Bu yönergeleri takip ettikten sonra, sistem/ağ hizmeti hesabı için net kullanımı çalıştırdığınızda aşağıdaki hata iletisini alabilirsiniz: "Sistem hatası 1312 oluştu. Belirli bir oturum açma oturumu yok. Zaten sonlandırılmış olabilir." Bu durumda, net kullanıma geçirilen kullanıcı adının etki alanı bilgilerini içerdiğinden emin olun (örneğin: "[depolama hesabı adı].dosya.core.windows.net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>Hata "Bir dosyayı şifrelemeyi desteklemeyen bir hedefe kopyalıyorsun"

Bir dosya ağ üzerinden kopyalandığında, dosya kaynak bilgisayarda şifresi çözülür, düz metin olarak aktarılır ve hedefte yeniden şifrelenir. Ancak, şifreli bir dosyayı kopyalamaya çalışırken aşağıdaki hatayı görebilirsiniz: "Dosyayı şifrelemeyi desteklemeyen bir hedefe kopyalıyorsun."

### <a name="cause"></a>Nedeni
Şifreleme Dosya Sistemi (EFS) kullanıyorsanız, bu sorun oluşabilir. BitLocker şifrelenmiş dosyalar Azure Dosyaları'na kopyalanabilir. Ancak Azure Files, NTFS EFS'i desteklemez.

### <a name="workaround"></a>Geçici çözüm
Bir dosyayı ağ üzerinden kopyalamak için önce dosyanın şifresini çözmeniz gerekir. Aşağıdaki yöntemlerden birini kullanın:

- Kopya **/d** komutunu kullanın. Şifrelenmiş dosyaların hedefteki şifresi çözülmüş dosyalar olarak kaydedilmesine olanak tanır.
- Aşağıdaki kayıt defteri anahtarını ayarlayın:
  - Yol = HKLM\Yazılım\İlkeler\Microsoft\Windows\System
  - Değer türü = DWORD
  - Adı = CopyFileAllowDecryptedRemoteDestination
  - Değer = 1

Kayıt defteri anahtarını ayarlamanın ağ paylaşımlarına yapılan tüm kopyalama işlemlerini etkilediğini unutmayın.

## <a name="slow-enumeration-of-files-and-folders"></a>Dosya ve klasörlerin yavaş numaralandırması

### <a name="cause"></a>Nedeni

Büyük dizinler için istemci makinesinde yeterli önbellek yoksa bu sorun oluşabilir.

### <a name="solution"></a>Çözüm

Bu sorunu gidermek için, istemci makinede daha büyük dizin listeleri önbelleğe almak için **DirectoryCacheEntrySizeMax** kayıt defteri değerini ayarlama:

- Konum: HKLM\System\CCS\Services\Lanmanworkstation\Parametreler
- Değer yele: DirectoryCacheEntrySizeMax 
- Değer türü:DWORD
 
 
Örneğin, 0x100000 olarak ayarlayabilir ve performansın daha iyi olup olmadığını görebilirsiniz.

## <a name="error-aaddstenantnotfound-in-enabling-azure-active-directory-domain-service-aad-ds-authentication-for-azure-files-unable-to-locate-active-tenants-with-tenant-id-aad-tenant-id"></a>Azure Dosyaları için Azure Etkin Dizin Etki Alanı Hizmeti (AAD DS) kimlik doğrulaması etkinleştirmede Hata AadDsTenantNotFound "Kiracı Kimliği aad-kiracı-id ile etkin kiracı bulamıyor"

### <a name="cause"></a>Nedeni

Hata AadDsTenantNotFound, [AAD Etki Alanı Hizmetinin (AAD DS)](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) ilişkili aboneliğin AAD kiracısı üzerinde oluşturulmadığı bir depolama hesabında [Azure Dosyaları'nda Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) kimlik doğrulamasını etkinleştirmeye](storage-files-identity-auth-active-directory-domain-service-enable.md) çalıştığınızda gerçekleşir.  

### <a name="solution"></a>Çözüm

Depolama hesabınızın dağıtılan aboneliğin AAD kiracıüzerinde AAD DS'yi etkinleştirin. Yönetilen bir etki alanı oluşturmak için AAD kiracısının yönetici ayrıcalıklarına ihtiyacınız vardır. Azure AD kiracısının yöneticisi değilseniz, Azure [portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetlerini Etkinleştirmek](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started)için yöneticiyle iletişime geçin ve adım adım kılavuzu izleyin.

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

## <a name="error-system-error-1359-has-occurred-an-internal-error-received-over-smb-access-to-file-shares-with-azure-active-directory-domain-service-aad-ds-authentication-enabled"></a>Hata 'Sistem hatası 1359 oluştu. Azure Active Directory Domain Service (AAD DS) kimlik doğrulaması etkinleştirilmiş dosya paylaşımlarına SMB erişimi üzerinden alınan bir iç hata

### <a name="cause"></a>Nedeni

Hata 'Sistem hatası 1359 oluştu. Bir iç hata' sayısal bir karakterle başlayan etki alanı DNS adı olan bir AAD DS karşı etkin AAD DS kimlik doğrulaması ile dosya paylaşımınıza bağlanmaya çalıştığınızda olur. Örneğin, AAD DS Etki Alanı DNS adınız "1domain" ise, AAD kimlik bilgilerini kullanarak dosya paylaşımını bağlamaya çalışırken bu hatayı alırsınız. 

### <a name="solution"></a>Çözüm

Şu anda, aad DS'nizi aşağıdaki kurallarla birlikte geçerli olan yeni bir etki alanı DNS adını kullanarak yeniden dağıtmayı düşünebilirsiniz:
- Adlar sayısal bir karakterle başlayamaz.
- İsimler 3 ila 63 karakter uzunluğunda olmalıdır.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.
Hala yardıma ihtiyacınız varsa, sorunuzun hızla çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
