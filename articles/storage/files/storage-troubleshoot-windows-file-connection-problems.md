---
title: Windows 'da Azure dosyaları sorunlarını giderme | Microsoft Docs
description: Windows 'da Azure dosyaları sorunlarını giderme
author: jeffpatt24
ms.service: storage
ms.topic: troubleshooting
ms.date: 05/31/2019
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: e855ed169a0c4eca7dda696c03deedb9e519e9bf
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259991"
---
# <a name="troubleshoot-azure-files-problems-in-windows"></a>Windows’ta Azure Dosyalar sorunlarını giderme

Bu makalede, Windows istemcilerinden bağlandığınızda Microsoft Azure dosyalarla ilgili yaygın sorunlar listelenmektedir. Ayrıca, bu sorunlar için olası nedenler ve çözümler de sağlar. Bu makaledeki sorun giderme adımlarına ek olarak, [AzFileDiagnostics](https://github.com/Azure-Samples/azure-files-samples/tree/master/AzFileDiagnostics/Windows)   Windows istemci ortamının doğru önkoşullara sahip olduğundan emin olmak Için azfilediagnostics 'i de kullanabilirsiniz. AzFileDiagnostics, bu makalede bahsedilen belirtilerin çoğunu algılamayı otomatikleştirir ve en iyi performansı elde etmek için ortamınızı ayarlamanıza yardımcı olur. Bu bilgileri Azure dosya [paylaşımları sorun gidericisinde](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares) de bulabilirsiniz. Bu bilgiler, Azure dosya paylaşımlarını bağlama/eşleme/bağlama sorunları konusunda size yardımcı olacak adımlar sağlar.

<a id="error5"></a>
## <a name="error-5-when-you-mount-an-azure-file-share"></a>Bir Azure dosya paylaşımından bağlama yaparken hata 5

Bir dosya payını bağlamaya çalıştığınızda, şu hatayı alabilirsiniz:

- Sistem hatası 5 oluştu. Erişim reddedildi.

### <a name="cause-1-unencrypted-communication-channel"></a>Neden 1: şifrelenmemiş iletişim kanalı

Güvenlik nedeniyle, iletişim kanalı şifrelenmemişse ve bağlantı girişimi Azure dosya paylaşımlarının bulunduğu veri merkezinden yapılmıyorsa Azure dosya paylaşımlarına bağlantılar engellenir. Depolama hesabında [Güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı etkinleştirildiyse aynı veri merkezi içinde şifrelenmemiş bağlantılar da engellenebilir. Şifrelenmiş bir iletişim kanalının sağlanabilmesi için kullanıcının istemcisi SMB şifrelemesini desteklemelidir.

Her sistemin Windows 8, Windows Server 2012 ve üstü sürümleri şifrelemeyi destekleyen SMB 3.0 içeren isteklerle anlaşır.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

1. SMB şifrelemesini (Windows 8, Windows Server 2012 veya üzeri) destekleyen bir istemciden bağlanın veya aynı veri merkezindeki bir sanal makineden Azure dosya paylaşımında kullanılan Azure depolama hesabıyla bağlanın.
2. İstemci SMB şifrelemesini desteklemiyorsa, depolama hesabında [Güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarının devre dışı olduğunu doğrulayın.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Neden 2: depolama hesabında sanal ağ veya güvenlik duvarı kuralları etkin 

Depolama hesabında sanal ağ (VNET) ve güvenlik duvarı kuralları yapılandırıldıysa, istemci IP adresine veya sanal ağa erişim izni verilmediği sürece ağ trafiğinin erişimi reddedilir.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Depolama hesabında sanal ağ ve güvenlik duvarı kurallarının düzgün yapılandırıldığını doğrulayın. Sanal ağ veya güvenlik duvarı kurallarının soruna neden olup olmadığını test etmek için depolama hesabında **Tüm ağlardan erişime izin ver** ayarını geçici olarak değiştirin. Daha fazla bilgi edinmek için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security).

### <a name="cause-3-share-level-permissions-are-incorrect-when-using-identity-based-authentication"></a>Neden 3: kimlik tabanlı kimlik doğrulaması kullanılırken, paylaşma düzeyi izinleri yanlış

Kullanıcılar Active Directory (AD) veya Azure Active Directory Domain Services (Azure AD DS) kimlik doğrulaması kullanarak Azure dosya paylaşımıyla erişiyorsa, paylaşma düzeyi izinleri yanlışsa dosya paylaşımının erişimi "erişim reddedildi" hatasıyla başarısız olur. 

### <a name="solution-for-cause-3"></a>Neden 3 için çözüm

Paylaşma düzeyi izinlerini güncelleştirmek için bkz. [bir kimliğe erişim Izinleri atama](https://docs.microsoft.com/azure/storage/files/storage-files-identity-auth-active-directory-domain-service-enable#2-assign-access-permissions-to-an-identity).

<a id="error53-67-87"></a>
## <a name="error-53-error-67-or-error-87-when-you-mount-or-unmount-an-azure-file-share"></a>Bir Azure dosya paylaşımını bağladığınızda veya kaldırdığınızda hata 53, hata 67 veya hata 87

Şirket içinden veya farklı bir veri merkezinden bir dosya paylaşımının bağlamaya çalıştığınızda aşağıdaki hataları alabilirsiniz:

- Sistem hatası 53 oluştu. Ağ yolu bulunamadı.
- Sistem hatası 67 oluştu. Ağ adı bulunamıyor.
- Sistem hatası 87 oluştu. Parametre yanlış.

### <a name="cause-1-port-445-is-blocked"></a>Neden 1: bağlantı noktası 445 engellendi

Bir Azure dosyaları veri merkezi ile bağlantı noktası 445 giden iletişim engellendiğinde sistem hatası 53 veya sistem hatası 67 oluşur. 445 numaralı bağlantı noktasından erişime izin veren veya erişimi engelleyen ISP'lerin özetini görmek için [TechNet](https://social.technet.microsoft.com/wiki/contents/articles/32346.azure-summary-of-isps-that-allow-disallow-access-from-port-445.aspx)'e gidin.

Güvenlik duvarınızın veya ISS 'nizin bağlantı noktası 445 ' i engelleyip engellemediğini denetlemek için [Azfilediagnostics](https://github.com/Azure-Samples/azure-files-samples/tree/master/AzFileDiagnostics/Windows) aracını veya `Test-NetConnection` cmdlet 'ini kullanın. 

`Test-NetConnection`Cmdlet 'ini kullanmak için Azure PowerShell modülünün yüklenmesi gerekir. daha fazla bilgi için bkz. [Azure PowerShell modülünü yükleme](/powershell/azure/install-Az-ps) . `<your-storage-account-name>` ile `<your-resource-group-name>` yerine depolama hesabınızla ilgili bilgileri yazmayı unutmayın.

   
```azurepowershell
$resourceGroupName = "<your-resource-group-name>"
$storageAccountName = "<your-storage-account-name>"

# This command requires you to be logged into your Azure account, run Login-AzAccount if you haven't
# already logged in.
$storageAccount = Get-AzStorageAccount -ResourceGroupName $resourceGroupName -Name $storageAccountName

# The ComputerName, or host, is <storage-account>.file.core.windows.net for Azure Public Regions.
# $storageAccount.Context.FileEndpoint is used because non-Public Azure regions, such as sovereign clouds
# or Azure Stack deployments, will have different hosts for Azure file shares (and other storage resources).
Test-NetConnection -ComputerName ([System.Uri]::new($storageAccount.Context.FileEndPoint).Host) -Port 445
```
    
Bağlantı başarılı olursa şu çıktıyı görmeniz gerekir:
    
  
```azurepowershell
ComputerName     : <your-storage-account-name>
RemoteAddress    : <storage-account-ip-address>
RemotePort       : 445
InterfaceAlias   : <your-network-interface>
SourceAddress    : <your-ip-address>
TcpTestSucceeded : True
```
 

> [!Note]  
> Yukarıdaki komut, depolama hesabının geçerli IP adresini döndürür. Bu IP adresinin aynı kalacağı garanti edilmez ve bu adres herhangi bir zamanda değişebilir. Bu IP adresini betiklere veya güvenlik duvarına sabit şekilde kodlamayın.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

#### <a name="solution-1---use-azure-file-sync"></a>Çözüm 1 - Azure Dosya Eşitleme'yi kullanın
Azure Dosya Eşitleme, şirket içi Windows Server 'larınızı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürebilir. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Azure Dosya Eşitleme, 443 numaralı bağlantı noktası üzerinden çalıştığından 445 numaralı bağlantı noktası engellenmiş olan istemcilerden Azure Dosyalar'a erişmek için geçici çözüm olarak kullanılabilir. [Azure dosya eşitleme ayarlamayı öğrenin](https://docs.microsoft.com/azure/storage/files/storage-sync-files-extend-servers).

#### <a name="solution-2---use-vpn"></a>Çözüm 2 - VPN kullanın
Belirli depolama hesabınıza bir VPN ayarlayarak trafik, internet üzerinden değil, güvenli bir tünelden geçer. Azure Dosyalar'a Windows'dan erişmek için [VPN kurulum yönergelerini](storage-files-configure-p2s-vpn-windows.md) izleyin.

#### <a name="solution-3---unblock-port-445-with-help-of-your-ispit-admin"></a>Çözüm 3 - ISP/BT Yöneticinizin yardımıyla 445 numaralı bağlantı noktasının engelini kaldırın
[Azure IP aralıklarına](https://www.microsoft.com/download/details.aspx?id=41653)giden 445 numaralı bağlantı noktasını açmak için BT departmanınızla veya ISS 'niz ile çalışın.

#### <a name="solution-4---use-rest-api-based-tools-like-storage-explorerpowershell"></a>Çözüm 4 - Depolama Gezgini/Powershell gibi REST API tabanlı araçları kullanın
Azure dosyaları da SMB 'ye ek olarak REST 'i destekler. REST erişimi, 443 numaralı bağlantı noktası üzerinden (standart TCP) çalışır. REST API kullanarak yazılmış olan ve zengin bir kullanıcı arabirimi deneyimi sunan birçok araç vardır. [Depolama Gezgini](https://docs.microsoft.com/azure/vs-azure-tools-storage-manage-with-storage-explorer?tabs=windows) bunlardan biridir. [Depolama Gezgini'ni indirip yükleyerek](https://azure.microsoft.com/features/storage-explorer/) Azure Dosyalar destekli dosya paylaşımınıza bağlanın. Ayrıca, Kullanıcı REST API de [PowerShell](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-powershell) kullanabilirsiniz.

### <a name="cause-2-ntlmv1-is-enabled"></a>Neden 2: NTLMv1 etkin

İstemcide NTLMv1 iletişimi etkinse sistem hatası 53 veya sistem hatası 87 oluşur. Azure Dosyalar yalnızca NTLMv2 kimlik doğrulamasını destekler. NTLMv1'in etkinleştirilmesi daha az güvenli bir istemci oluşturur. Bu nedenle Azure Dosyalar için iletişim engellenir. 

Hatanın nedeninin bu olup olmadığını saptamak için aşağıdaki kayıt defteri alt anahtarının 3 değerine ayarlandığını doğrulayın:

**HKLM\SYSTEM\CurrentControlSet\Control\Lsa > LmCompatibilityLevel**

Daha fazla bilgi için TechNet'te [LmCompatibilityLevel](https://technet.microsoft.com/library/cc960646.aspx) konusuna bakın.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Aşağıdaki kayıt defteri alt anahtarında **LmCompatibilityLevel** değerini varsayılan değeri olan 3'e döndürün:

  **HKLM\SYSTEM\CurrentControlSet\Control\Lsa**

<a id="error1816"></a>
## <a name="error-1816---not-enough-quota-is-available-to-process-this-command"></a>Hata 1816-bu komutu işlemek için yeterli kota yok

### <a name="cause"></a>Nedeni

Azure dosya paylaşımında bir dosya veya dizin için izin verilen eş zamanlı açık tanıtıcıların üst sınırına ulaştığınızda hata 1816 olur. Daha fazla bilgi için bkz. [Azure Dosyaları ölçeklendirme hedefleri](https://docs.microsoft.com/azure/storage/files/storage-files-scale-targets#azure-files-scale-targets).

### <a name="solution"></a>Çözüm

Bazı tutamaçları kapatarak eşzamanlı açık tanıtıcı sayısını azaltın ve yeniden deneyin. Daha fazla bilgi için bkz. [performans ve ölçeklenebilirlik denetim listesi Microsoft Azure depolama](../common/storage-performance-checklist.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json).

Bir dosya paylaşımının, dizinin veya dosyanın açık tanıtıcılarını görüntülemek için [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet 'ini kullanın.  

Bir dosya paylaşımının, dizinin veya dosyanın açık tanıtıcılarını kapatmak için, [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet 'ini kullanın.

> [!Note]  
> Get-AzStorageFileHandle ve Close-AzStorageFileHandle cmdlet 'leri az PowerShell Module sürüm 2,4 veya üzeri sürümlerde bulunur. En son az PowerShell modülünü yüklemek için bkz. [Azure PowerShell modülünü yüklemek](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="noaaccessfailureportal"></a>
## <a name="error-no-access-when-you-try-to-access-or-delete-an-azure-file-share"></a>Bir Azure dosya paylaşımından erişmeye veya silmeye çalıştığınızda "erişim yok" hatası  
Portalda bir Azure dosya paylaşımından erişmeye veya silmeye çalıştığınızda şu hatayı alabilirsiniz:

Erişim yok  
Hata kodu: 403 

### <a name="cause-1-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Neden 1: depolama hesabında sanal ağ veya güvenlik duvarı kuralları etkin

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

Depolama hesabında sanal ağ ve güvenlik duvarı kurallarının düzgün yapılandırıldığını doğrulayın. Sanal ağ veya güvenlik duvarı kurallarının soruna neden olup olmadığını test etmek için depolama hesabında **Tüm ağlardan erişime izin ver** ayarını geçici olarak değiştirin. Daha fazla bilgi edinmek için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security).

### <a name="cause-2-your-user-account-does-not-have-access-to-the-storage-account"></a>Neden 2: Kullanıcı hesabınızın depolama hesabına erişimi yok

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Azure dosya paylaşımının bulunduğu depolama hesabına gidin, **erişim denetimi (IAM)** öğesine tıklayın ve Kullanıcı hesabınızın depolama hesabına erişimi olduğunu doğrulayın. Daha fazla bilgi edinmek için bkz. [rol tabanlı Access Control (RBAC) ile depolama hesabınızın güvenliğini sağlama](https://docs.microsoft.com/azure/storage/blobs/security-recommendations#data-protection).

<a id="open-handles"></a>
## <a name="unable-to-delete-a-file-or-directory-in-an-azure-file-share"></a>Azure dosya paylaşımında dosya veya dizin silinemiyor
Bir dosyayı silmeye çalıştığınızda şu hatayı alabilirsiniz:

Belirtilen kaynak SMB istemcisi tarafından silinmek üzere işaretlendi.

### <a name="cause"></a>Nedeni
Bu sorun genellikle dosya veya dizinde açık bir tanıtıcı varsa oluşur. 

### <a name="solution"></a>Çözüm

SMB istemcileri tüm açık tutamaçları kapatmışsa ve sorun oluşmaya devam ederse, şunları yapın:

- Açık tutamaçları görüntülemek için [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet 'ini kullanın.

- Açık tutamaçları kapatmak için [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet 'ini kullanın. 

> [!Note]  
> Get-AzStorageFileHandle ve Close-AzStorageFileHandle cmdlet 'leri az PowerShell Module sürüm 2,4 veya üzeri sürümlerde bulunur. En son az PowerShell modülünü yüklemek için bkz. [Azure PowerShell modülünü yüklemek](https://docs.microsoft.com/powershell/azure/install-az-ps).

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-windows"></a>Windows'da Azure Dosyaları ile yavaş dosya alışverişi

Azure dosya hizmetine dosya aktarmaya çalıştığınızda yavaş performans görebilirsiniz.

- Belirli bir en düşük g/ç boyutu gereksiniminize sahip değilseniz en iyi performans için g/ç boyutu olarak 1 MIB kullanmanızı öneririz.
-   Yazmalar ile genişletilen bir dosyanın son boyutunu biliyorsanız ve bu dosyada, yazılı olmayan kuyruk sıfır içerdiğinde yazılım uyumluluk sorunlarıyla karşılaşırsanız, her yazma için bir genişletme yazma yapmak yerine dosya boyutunu önceden ayarlayın.
-   Doğru kopyalama yöntemini kullanın:
    -   İki dosya paylaşımı arasındaki herhangi bir aktarım için [AzCopy](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) kullanın.
    -   Şirket içi bilgisayardaki dosya paylaşımları arasında [Robocopy](/azure/storage/files/storage-files-deployment-guide#robocopy) kullanın.

### <a name="considerations-for-windows-81-or-windows-server-2012-r2"></a>Windows 8.1 veya Windows Server 2012 R2 ile ilgili konular

Windows 8.1 veya Windows Server 2012 R2 çalıştıran istemciler için [KB3114025](https://support.microsoft.com/help/3114025) düzeltmesinin yüklü olduğundan emin olun. Bu düzeltme, oluşturma ve kapatma tanıtıcılarının performansını geliştirir.

Düzeltmenin yüklenip yüklenmediğini denetlemek için aşağıdaki betiği çalıştırabilirsiniz:

`reg query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies`

Düzeltme yüklüyse, aşağıdaki çıktı görüntülenir:

`HKEY_Local_MACHINE\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters\Policies {96c345ef-3cac-477b-8fcd-bea1a564241c} REG_DWORD 0x1`

> [!Note]
> Azure Marketi 'ndeki Windows Server 2012 R2 görüntüleri, varsayılan olarak güncelleştirme 2015 ' den itibaren KB3114025 yüklüdür.

<a id="shareismissing"></a>
## <a name="no-folder-with-a-drive-letter-in-my-computer-or-this-pc"></a>"Bilgisayarımda" veya "Bu bılgısayar" içinde sürücü harfine sahip bir klasör yok

Bir Azure dosya paylaşımından, net use kullanarak yönetici olarak eşleme yaptıysanız, paylaşımın eksik olduğu görülüyor.

### <a name="cause"></a>Nedeni

Varsayılan olarak, Windows Dosya Gezgini yönetici olarak çalışmaz. Net use komutunu bir yönetim komut isteminden çalıştırırsanız, ağ sürücüsünü yönetici olarak eşlersiniz. Eşlenen sürücüler Kullanıcı merkezli olduğundan, oturum açmış olan kullanıcı hesabı, farklı bir kullanıcı hesabı altına bağlandıklarında sürücüleri görüntülemez.

### <a name="solution"></a>Çözüm
Paylaşımdan yönetici olmayan bir komut satırından bağlayın. Alternatif olarak, **EnableLinkedConnections** kayıt defteri değerini yapılandırmak Için [Bu TechNet konusunu](https://technet.microsoft.com/library/ee844140.aspx) da izleyebilirsiniz.

<a id="netuse"></a>
## <a name="net-use-command-fails-if-the-storage-account-contains-a-forward-slash"></a>Depolama hesabı eğik çizgi içeriyorsa net use komutu başarısız olur

### <a name="cause"></a>Nedeni

Net use komutu bir eğik çizgi (/) bir komut satırı seçeneği olarak yorumlar. Kullanıcı hesabınızın adı eğik çizgiyle başlıyorsa sürücü eşleştirmesi başarısız olur.

### <a name="solution"></a>Çözüm

Sorunu geçici olarak çözmek için aşağıdaki adımlardan birini kullanabilirsiniz:

- Aşağıdaki PowerShell komutunu çalıştırın:

  `New-SmbMapping -LocalPath y: -RemotePath \\server\share -UserName accountName -Password "password can contain / and \ etc"`

  Bir toplu iş dosyasından komutunu şu şekilde çalıştırabilirsiniz:

  `Echo new-smbMapping ... | powershell -command –`

- Bu sorunu geçici olarak çözmek için anahtarın etrafına çift tırnak işareti koyun; eğik çizgi ilk karakter değilse. Bu durumda, etkileşimli modu kullanın ve parolanızı ayrı olarak girin ya da eğik çizgiyle başlamaymeyen bir anahtar almak için anahtarlarınızı yeniden oluşturun.

<a id="cannotaccess"></a>
## <a name="application-or-service-cannot-access-a-mounted-azure-files-drive"></a>Uygulama veya hizmet bağlı bir Azure dosyaları sürücüsüne erişemiyor

### <a name="cause"></a>Nedeni

Sürücüler Kullanıcı başına bağlanır. Uygulamanız veya hizmetiniz sürücüyü takenden farklı bir kullanıcı hesabı altında çalışıyorsa, uygulama sürücüyü görmez.

### <a name="solution"></a>Çözüm

Aşağıdaki çözümlerden birini kullanın:

-   Sürücüyü uygulamayı içeren kullanıcı hesabından bağlayın. PsExec gibi bir araç kullanabilirsiniz.
- Depolama hesabı adını ve anahtarını net use komutunun Kullanıcı adı ve parola parametrelerine geçirin.
- Kimlik bilgilerini kimlik bilgileri Yöneticisi 'ne eklemek için cmdkey komutunu kullanın. Bunu, hizmet hesabı bağlamı altındaki bir komut satırından, etkileşimli oturum açma yoluyla veya kullanarak gerçekleştirin `runas` .
  
  `cmdkey /add:<storage-account-name>.file.core.windows.net /user:AZURE\<storage-account-name> /pass:<storage-account-key>`
- Eşlenen bir sürücü harfi kullanmadan doğrudan paylaşma eşleyin. Bazı uygulamalar sürücü harfine düzgün şekilde yeniden bağlanmayabilir, bu nedenle tam UNC yolunun kullanılması daha güvenilir olabilir. 

  `net use * \\storage-account-name.file.core.windows.net\share`

Bu yönergeleri izledikten sonra, sistem/ağ hizmeti hesabı için net use çalıştırdığınızda aşağıdaki hata iletisini alabilirsiniz: "sistem hatası 1312 oluştu. Belirtilen bir oturum açma oturumu yok. Zaten sonlandırılmış olabilir. " Bu durumda, ağ kullanımına geçirilen kullanıcı adının etki alanı bilgilerini içerdiğinden emin olun (örneğin: "[depolama hesabı adı]. File. Core. Windows. net").

<a id="doesnotsupportencryption"></a>
## <a name="error-you-are-copying-a-file-to-a-destination-that-does-not-support-encryption"></a>"Şifrelemeyi desteklemeyen bir hedefe dosya kopyalıyorsunuz" hatası

Ağ üzerinden bir dosya kopyalandığında, dosya, düz metin olarak iletilir ve hedefte yeniden şifrelendiğinde kaynak bilgisayarda şifresi çözülür. Ancak, şifrelenmiş bir dosyayı kopyalamaya çalışırken şu hatayı görebilirsiniz: "dosyayı şifrelemeyi desteklemeyen bir hedefe kopyalıyorsunuz."

### <a name="cause"></a>Nedeni
Şifreleme dosya sistemi (EFS) kullanıyorsanız, bu sorun oluşabilir. BitLocker ile şifrelenen dosyalar Azure dosyalarına kopyalanabilir. Ancak, Azure dosyaları NTFS EFS 'yi desteklemez.

### <a name="workaround"></a>Geçici çözüm
Ağ üzerinden bir dosyayı kopyalamak için, önce onu çözmelisiniz. Aşağıdaki yöntemlerden birini kullanın:

- **Copy/d** komutunu kullanın. Şifrelenmiş dosyaların hedefte şifresi çözülmüş dosyalar olarak kaydedilmesine izin verir.
- Aşağıdaki kayıt defteri anahtarını ayarlayın:
  - Yol = HKLM\Software\Policies\Microsoft\Windows\System
  - Değer türü = DWORD
  - Ad = CopyFileAllowDecryptedRemoteDestination
  - Değer = 1

Kayıt defteri anahtarını ayarlamanın ağ paylaşımlarına yapılan tüm kopyalama işlemlerini etkilediğini unutmayın.

## <a name="slow-enumeration-of-files-and-folders"></a>Dosya ve klasörlerin yavaş numaralandırılması

### <a name="cause"></a>Nedeni

Bu sorun, büyük dizinler için istemci makinesinde yeterli önbellek olmadığında ortaya çıkabilir.

### <a name="solution"></a>Çözüm

Bu sorunu çözmek için **DirectoryCacheEntrySizeMax** kayıt defteri değerini, istemci makinedeki daha büyük dizin listelerinin önbelleğe alınmasına izin verecek şekilde ayarlama:

- Konum: HKLM\System\CCS\Services\Lanmanworkstation\Parameters
- Değer Mane: DirectoryCacheEntrySizeMax 
- Değer türü: DWORD
 
 
Örneğin, bunu 0x100000 olarak ayarlayabilir ve performansın daha iyi olup olmadığını görebilirsiniz.

## <a name="error-aaddstenantnotfound-in-enabling-azure-active-directory-domain-service-aad-ds-authentication-for-azure-files-unable-to-locate-active-tenants-with-tenant-id-aad-tenant-id"></a>Hata AadDsTenantNotFound, Azure dosyaları için Azure Active Directory etki alanı hizmeti (AAD DS) kimlik doğrulamasını etkinleştirme "Kiracı kimliği AAD-Tenant-ID" olan etkin kiracılar bulunamıyor

### <a name="cause"></a>Nedeni

Azure dosyalarında, ilişkili aboneliğin AAD kiracısında [AAD etki alanı hizmeti 'nin (AAD DS)](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-overview) oluşturulabileceği bir depolama hesabındaki [Azure Active Directory Domain Services (Azure AD DS) kimlik doğrulamasını etkinleştirmeye](storage-files-identity-auth-active-directory-domain-service-enable.md) çalıştığınızda hata AadDsTenantNotFound olur.  

### <a name="solution"></a>Çözüm

Depolama hesabınızın dağıtıldığı aboneliğin AAD kiracısında AAD DS 'yi etkinleştirin. Yönetilen bir etki alanı oluşturmak için AAD kiracısının yönetici ayrıcalıklarına sahip olmanız gerekir. Azure AD kiracısı yöneticisi değilseniz, yöneticiye başvurun ve [Azure Portal kullanarak Azure Active Directory Domain Services etkinleştirmek](https://docs.microsoft.com/azure/active-directory-domain-services/active-directory-ds-getting-started)için adım adım yönergeleri izleyin.

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

## <a name="error-system-error-1359-has-occurred-an-internal-error-received-over-smb-access-to-file-shares-with-azure-active-directory-domain-service-aad-ds-authentication-enabled"></a>Hata ' sistem hatası 1359 oluştu. Azure Active Directory etki alanı hizmeti (AAD DS) kimlik doğrulaması etkinken dosya paylaşımlarına SMB erişimi üzerinden alınan bir iç hata '

### <a name="cause"></a>Nedeni

Hata ' sistem hatası 1359 oluştu. Bir iç hata ', bir sayısal karakterle başlayan etki alanı DNS adına sahip bir AAD DS kimlik doğrulaması ile dosya paylaşımınıza bağlanmaya çalıştığınızda oluşur. Örneğin, AAD DS etki alanı DNS adınız "1domain" ise, AAD kimlik bilgilerini kullanarak dosya paylaşımının bağlamaya çalışırken bu hatayı alırsınız. 

### <a name="solution"></a>Çözüm

Şu anda, aşağıdaki kurallarla geçerli olan yeni bir etki alanı DNS adı kullanarak AAD DS 'nizi yeniden dağıtmaya göz önüne alabilirsiniz:
- Adlar sayısal bir karakterle başlayamaz.
- Adların uzunluğu 3 ile 63 karakter arasında olmalıdır.

## <a name="unable-to-mount-azure-files-with-ad-credentials"></a>Azure dosyaları AD kimlik bilgileriyle bağlanamıyor 

### <a name="self-diagnostics-steps"></a>Kendi kendine Tanılama adımları
İlk olarak, [Azure dosyaları ad kimlik doğrulamasını etkinleştirmek](https://docs.microsoft.com/azure/storage/files/storage-files-identity-auth-active-directory-enable)için tüm dört adımı izlediğinizden emin olun.

İkinci olarak, [depolama hesabı anahtarıyla Azure dosya paylaşımının bağlanmasını](https://docs.microsoft.com/azure/storage/files/storage-how-to-use-files-windows)deneyin. Bağlama işlemi başarısız olursa, istemciyi çalıştıran istemciyi doğrulamanıza yardımcı olmak üzere [AzFileDiagnostics.ps1](https://github.com/Azure-Samples/azure-files-samples/tree/master/AzFileDiagnostics/Windows) Indirin, Azure dosyaları için erişim hatasına neden olacak uyumsuz istemci yapılandırmasını tespit edin, kendi kendine düzeltmeyle ilgili yönergeler verir ve tanılama izlemelerini toplayın.

Üçüncü olarak, oturum açmış AD kullanıcısı ile AD yapılandırmanızda basit denetimler kümesi yürütmek için Debug-AzStorageAccountAuth cmdlet 'ini çalıştırabilirsiniz. Bu cmdlet, [AzFilesHybrid v0.1.2+ sürümünde](https://github.com/Azure-Samples/azure-files-samples/releases) desteklenir. Bu cmdlet'i hedef depolama hesabında sahip izinlerine sahip bir AD kullanıcısıyla çalıştırmanız gerekir.  
```PowerShell
$ResourceGroupName = "<resource-group-name-here>"
$StorageAccountName = "<storage-account-name-here>"

Debug-AzStorageAccountAuth -StorageAccountName $StorageAccountName -ResourceGroupName $ResourceGroupName -Verbose
```
Cmdlet bu denetimleri sırayla gerçekleştirir ve hatalara yönelik rehberlik sağlar:
1. CheckPort445Connectivity: bağlantı noktası 445 ' nin SMB bağlantısı için açıldığını denetleyin
2. Checkdomainkatılmış: istemci makinenin AD 'ye katılmış olduğunu doğrulama
3. CheckADObject: depolama hesabını temsil eden ve doğru SPN 'ye (hizmet asıl adı) sahip Active Directory bir nesne olduğunu onaylayın.
4. CheckGetKerberosTicket: depolama hesabına bağlanmak için bir Kerberos bileti almayı deneyin 
5. Checkadobjectpassworno: depolama hesabını temsil eden AD kimliği üzerinde yapılandırılan parolanın, depolama hesabı kerb1 veya kerb2 anahtarı ile aynı olduğundan emin olun
6. Checksıdhasaaduser: oturum açan AD kullanıcısının Azure AD ile eşitlendiğinden emin olun. Belirli bir AD kullanıcısının Azure AD ile eşitlenip eşitlenmediğini aramak isterseniz, giriş parametrelerinde-UserName ve-Domain öğesini belirtebilirsiniz.
7. CheckAadUserHasSid: bir Azure AD kullanıcısının AD 'de SID 'ye sahip olup olmadığını denetleyin. Bu denetim, kullanıcının Azure AD kullanıcısının nesne kimliğini parametre-ObjectID ile girmesini gerektirir. 
8. Checkstorageaccountdomainkatılmış: AD kimlik doğrulamasının etkinleştirildiğini ve hesabın AD özelliklerinin doldurulduğunu görmek için depolama hesabının özelliklerini denetleyin.

## <a name="unable-to-configure-directoryfile-level-permissions-windows-acls-with-windows-file-explorer"></a>Windows Dosya Gezgini ile dizin/dosya düzeyi izinleri (Windows ACL 'Leri) yapılandırılamıyor

### <a name="symptom"></a>Belirti

Windows ACL 'Leri bağlı bir dosya paylaşımında dosya Gezgini ile yapılandırmaya çalışırken aşağıda açıklanan belirtilerden biriyle karşılaşabilirsiniz:
- Güvenlik sekmesi altındaki Düzenle iznine tıkladıktan sonra, Izin Sihirbazı yüklenmez. 
- Yeni bir kullanıcı veya grup seçmeyi denediğinizde, etki alanı konumu doğru AD DS etki alanını görüntülemez. 

### <a name="solution"></a>Çözüm

Geçici bir çözüm olarak dizin/dosya düzeyi izinlerini yapılandırmak için [ıacl 'ler aracını](https://docs.microsoft.com/windows-server/administration/windows-commands/icacls) kullanmanızı öneririz. 

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.
Hala yardıma ihtiyacınız varsa, sorununuzun hızla çözülmesini sağlamak için [desteğe başvurun](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade) .
