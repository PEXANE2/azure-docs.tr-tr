---
title: Linux'taki Azure Dosyaları sorunlarını giderme | Microsoft Dokümanlar
description: Linux'ta Azure Dosyaları sorunlarını giderme
author: jeffpatt24
ms.service: storage
ms.topic: conceptual
ms.date: 10/16/2018
ms.author: jeffpatt
ms.subservice: files
ms.openlocfilehash: 95e220102cba290664a32cb6bbebef881ae4ffde
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80159498"
---
# <a name="troubleshoot-azure-files-problems-in-linux"></a>Linux'taki Azure Dosyaları sorunlarını giderme

Bu makalede, Linux istemcilerinden bağlandığınızda Azure Dosyaları ile ilgili sık karşılaşılan sorunlar listelenir. Ayrıca, bu sorunlar için olası nedenleri ve çözümleri sağlar. 

Bu makaledeki sorun giderme adımlarına ek olarak, Linux istemcisinin doğru ön koşullara sahip olduğundan emin olmak için [AzFileDiagnostics'i](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089) kullanabilirsiniz. AzFileDiagnostics, bu makalede belirtilen belirtilerin çoğunun algılanmasını otomatikleştirir. En iyi performansı elde etmek için ortamınızı ayarlamanıza yardımcı olur. Bu bilgileri Azure Files [paylaşımlarında sorun gidericide](https://support.microsoft.com/help/4022301/troubleshooter-for-azure-files-shares)de bulabilirsiniz. Sorun giderici, Azure Dosyaları paylaşımlarına bağlanma, eşleme ve montaj sorunları konusunda size yardımcı olacak adımlar sağlar.

## <a name="cannot-connect-to-or-mount-an-azure-file-share"></a>Azure dosyası paylaşımına bağlanamıyor veya bağlayamıyor

### <a name="cause"></a>Nedeni

Bu sorunun yaygın nedenleri şunlardır:

- Uyumsuz bir Linux dağıtım istemcisi kullanıyorsunuz. Azure dosya paylaşımına bağlanmak için aşağıdaki Linux dağıtımlarını kullanmanızı öneririz:

|   | SMB 2.1 <br>(Aynı Azure bölgesindeki VM'lere bağlanır) | SMB 3.0 <br>(Tesis içi ve çapraz bölgeden gelen bağlar) |
| --- | :---: | :---: |
| Ubuntu Server | 14,04+ | 16,04+ |
| RHEL | 7+ | 7.5+ |
| CentOS | 7+ |  7.5+ |
| Debian | 8+ |   |
| openSUSE | 13.2+ | 42.3+ |
| SUSE Linux Enterprise Server | 12 | 12 SP3+ |

- CIFS yardımcı programları (cifs-utils) istemciye yüklenmez.
- İstemcide minimum SMB/CIFS sürümü, 2.1 yüklü değildir.
- SMB 3.0 şifreleme istemciüzerinde desteklenmez. Önceki tablo, şifreleme kullanarak şirket içi ve bölgeler arası montajı destekleyen Linux dağıtımlarının bir listesini sağlar. Diğer dağıtımlar için çekirdek 4.11 ve üstü sürümler gerekir.
- TCP bağlantı noktası 445 üzerinden desteklenmeyen bir depolama hesabına bağlanmaya çalışıyorsunuz.
- Bir Azure VM'den bir Azure dosya paylaşımına bağlanmaya çalışıyorsunuz ve VM depolama hesabıyla aynı bölgede değil.
- Depolama hesabında [Güvenli aktarım gerekli]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı etkinleştirilirse, Azure Files yalnızca Şifreleme ile SMB 3.0 kullanan bağlantılara izin verir.

### <a name="solution"></a>Çözüm

Sorunu çözmek [için, Azure Dosyaları'nın Linux'ta hata lar eklemesi için sorun giderme aracını](https://gallery.technet.microsoft.com/Troubleshooting-tool-for-02184089)kullanın. Bu araç:

* İstemci çalışma ortamını doğrulamanıza yardımcı olur.
* Azure Dosyaları için erişim hatasına neden olacak uyumsuz istemci yapılandırmasını algılar.
* Kendi kendini sabitleme konusunda ön yazı rehberlik sağlar.
* Teşhis izlerini toplar.

<a id="mounterror13"></a>
## <a name="mount-error13-permission-denied-when-you-mount-an-azure-file-share"></a>"Montaj hatası(13): Azure dosya paylaşımı nı monte ettiğinizde izin reddedildi"

### <a name="cause-1-unencrypted-communication-channel"></a>Neden 1: Şifresiz iletişim kanalı

Güvenlik nedeniyle, iletişim kanalı şifrelenmemişse ve bağlantı girişimi Azure dosya paylaşımlarının bulunduğu veri merkezinden yapılmıyorsa Azure dosya paylaşımlarına bağlantılar engellenir. Depolama hesabında [Güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı etkinleştirildiyse aynı veri merkezi içinde şifrelenmemiş bağlantılar da engellenebilir. Şifrelenmiş bir iletişim kanalının sağlanabilmesi için kullanıcının istemcisi SMB şifrelemesini desteklemelidir.

Daha fazla bilgi edinmek için bkz. [Linux ve cifs-utils paketiyle Azure dosya paylaşımını bağlamak için önkoşullar](storage-how-to-use-files-linux.md#prerequisites). 

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

1. Kobİ şifrelemesini destekleyen bir istemciden bağlanın veya Azure dosya paylaşımı için kullanılan Azure depolama hesabıyla aynı veri merkezindeki sanal makineden bağlanın.
2. İstemci Kobİ şifrelemesini desteklemiyorsa, [güvenli aktarım gerekli](https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarı depolama hesabında devre dışı bırakıldığını doğrulayın.

### <a name="cause-2-virtual-network-or-firewall-rules-are-enabled-on-the-storage-account"></a>Neden 2: Depolama hesabında sanal ağ veya güvenlik duvarı kuralları etkinleştirilir 

Depolama hesabında sanal ağ (VNET) ve güvenlik duvarı kuralları yapılandırıldıysa, istemci IP adresine veya sanal ağa erişim izni verilmediği sürece ağ trafiğinin erişimi reddedilir.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Depolama hesabında sanal ağ ve güvenlik duvarı kurallarının düzgün yapılandırıldığını doğrulayın. Sanal ağ veya güvenlik duvarı kurallarının soruna neden olup olmadığını test etmek için depolama hesabında **Tüm ağlardan erişime izin ver** ayarını geçici olarak değiştirin. Daha fazla bilgi edinmek için bkz. [Azure Depolama güvenlik duvarlarını ve sanal ağları yapılandırma](https://docs.microsoft.com/azure/storage/common/storage-network-security).

<a id="permissiondenied"></a>
## <a name="permission-denied-disk-quota-exceeded-when-you-try-to-open-a-file"></a>"[izin reddedildi] Disk kotası aşıldı" bir dosyayı açmaya çalıştığınızda

Linux'ta, aşağıdakilere benzeyen bir hata iletisi alırsınız:

**\<dosya adı> [izin reddedildi] Disk kotası aşıldı**

### <a name="cause"></a>Nedeni

Bir dosya için izin verilen eşzamanlı açık tutamaçların üst sınırına ulaştınız.

Tek bir dosyada 2.000 açık tutamaç kotası vardır. 2.000 açık tanıtıcınız olduğunda, kotaya ulaşıldığını belirten bir hata iletisi görüntülenir.

### <a name="solution"></a>Çözüm

Bazı tutamaçları kapatarak eşzamanlı açık tutamaçların sayısını azaltın ve işlemi yeniden deneyin.

Bir dosya paylaşımı, dizin veya dosya için açık tutamaçları görüntülemek için [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet'i kullanın.  

Dosya paylaşımı, dizin veya dosya için açık tutamaçları kapatmak için [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet'i kullanın.

> [!Note]  
> Get-AzStorageFileHandle ve Close-AzStorageFileHandle cmdlets Az PowerShell modül sürüm 2.4 veya daha sonra dahildir. En son Az PowerShell modüllerini yüklemek için Azure [PowerShell modüllerini yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)

<a id="slowfilecopying"></a>
## <a name="slow-file-copying-to-and-from-azure-files-in-linux"></a>Linux'taki Azure Dosyalarına ve Azure Dosyalarından yavaş dosya kopyalama

- Belirli bir minimum G/Ç boyutu gereksiniminiz yoksa, optimum performans için 1 MiB'i G/Ç boyutu olarak kullanmanızı öneririz.
- Doğru kopyalama yöntemini kullanın:
    - İki dosya paylaşımı arasında herhangi bir aktarım için [AzCopy'yi](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) kullanın.
    - Paralel cp veya dd kullanmak kopyalama hızını artırabilir, iş parçacığı sayısı kullanım durumunuza ve iş yükünüze bağlıdır. Aşağıdaki örneklerde altı tane kullanılır: 
    - cp örneği (cp, dosya sisteminin varsayılan blok boyutunu yığın `find * -type f | parallel --will-cite -j 6 cp {} /mntpremium/ &`boyutu olarak kullanır): .
    - dd örnek (bu komut açıkça 1 MiB için yığın boyutu ayarlar):`find * -type f | parallel --will-cite-j 6 dd if={} of=/mnt/share/{} bs=1M`
    - Açık kaynak üçüncü taraf araçları gibi:
        - [GNU Paralel](https://www.gnu.org/software/parallel/).
        - [Fpart](https://github.com/martymac/fpart) - Dosyaları sıralar ve bölümlere paketler.
        - [Fpsync](https://github.com/martymac/fpart/blob/master/tools/fpsync) - src_dir'den dst_url'a veri geçirmek için birden çok örnek ortaya çıkarmak için Fpart ve bir kopyalama aracı kullanır.
        - [Çok](https://github.com/pkolano/mutil) - GNU coreutils dayalı çok dişli cp ve md5sum.
- Dosya boyutunu önceden ayarlamak, her yazıyı genişletme yazmayapmak yerine, dosya boyutunun bilindiği senaryolarda kopyalama hızını artırmaya yardımcı olur. Yazma genişletme kaçınılması gerekiyorsa, komutile `truncate - size <size><file>` bir hedef dosya boyutu ayarlayabilirsiniz. Bundan sonra, `dd if=<source> of=<target> bs=1M conv=notrunc`komut, hedef dosyanın boyutunu tekrar tekrar güncelleştirmek zorunda kalmadan bir kaynak dosyayı kopyalar. Örneğin, kopyalamak istediğiniz her dosya için hedef dosya boyutunu ayarlayabilirsiniz (/mnt/share altında bir paylaşım monte edilmiş varsayalım):
    - `$ for i in `` find * -type f``; do truncate --size ``stat -c%s $i`` /mnt/share/$i; done`
    - ve sonra - paralel olarak yazıyor genişletmeden dosyaları kopyalamak:`$find * -type f | parallel -j6 dd if={} of =/mnt/share/{} bs=1M conv=notrunc`

<a id="error115"></a>
## <a name="mount-error115-operation-now-in-progress-when-you-mount-azure-files-by-using-smb-30"></a>SMB 3.0 kullanarak Azure Dosyalarını monte ettiğinizde "Montaj hatası(115): İşlem şimdi devam ediyor"

### <a name="cause"></a>Nedeni

Bazı Linux dağıtımları henüz SMB 3.0'daki şifreleme özelliklerini desteklemiyor. Azure Dosyalar'ı SMB 3.0 kullanarak bağlamaya çalışan kullanıcılar eksik özellik nedeniyle "115" hata iletisini alabilir. Tam şifreleme ile SMB 3.0 yalnızca Ubuntu 16.04 veya üstünü kullandığınızda desteklenir.

### <a name="solution"></a>Çözüm

Linux için SMB 3.0'ın şifreleme özelliği 4.11 çekirdeğinde kullanıma sunuldu. Bu özellik Azure dosya paylaşımını şirket içinden veya farklı bir Azure bölgesinden bağlamaya olanak tanır. Bazı Linux dağıtımları, 4.11 çekirdeğinden linux çekirdeğinin eski sürümlerine kadar olan ve korudukları değişiklikleri desteklenmiş olabilir. Linux sürümünüzün SMB 3.0'ı şifrelemeyle destekleyip desteklememenize yardımcı olmak için [Linux ile Azure Dosyalarını Kullan'a](storage-how-to-use-files-linux.md)başvurun. 

Linux SMB istemciniz şifrelemeyi desteklemiyorsa, Azure Dosyalar'ı bağlamak için dosya paylaşımıyla aynı veri merkezinde bulunan bir Azure Linux VM'sinden SMB 2.1'i kullanın. Depolama hesabında [Güvenli aktarım gerekli]( https://docs.microsoft.com/azure/storage/common/storage-require-secure-transfer) ayarının devre dışı bırakıldığını doğrulayın. 

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

### <a name="cause"></a>Nedeni
Bu sorun genellikle dosya veya dizin açık bir tanıtıcı varsa oluşur. 

### <a name="solution"></a>Çözüm

Kobİ istemcileri tüm açık tutamaçları kapattıysa ve sorun oluşmaya devam ederse, aşağıdakileri gerçekleştirin:

- Açık tutamaçları görüntülemek için [Get-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/get-azstoragefilehandle) PowerShell cmdlet'i kullanın.

- Açık kolları kapatmak için [Close-AzStorageFileHandle](https://docs.microsoft.com/powershell/module/az.storage/close-azstoragefilehandle) PowerShell cmdlet'i kullanın. 

> [!Note]  
> Get-AzStorageFileHandle ve Close-AzStorageFileHandle cmdlets Az PowerShell modül sürüm 2.4 veya daha sonra dahildir. En son Az PowerShell modüllerini yüklemek için Azure [PowerShell modüllerini yükleyin.](https://docs.microsoft.com/powershell/azure/install-az-ps)

<a id="slowperformance"></a>
## <a name="slow-performance-on-an-azure-file-share-mounted-on-a-linux-vm"></a>Linux sanal makinesine bağlanmış Azure dosya paylaşımında yavaş performans

### <a name="cause-1-caching"></a>Neden 1: Önbelleğe alma

Yavaş performansın olası bir nedeni devre dışı bırakmadır. Önbelleğe alma, bir dosyaya tekrar tekrar erişiyorsanız yararlı olabilir, aksi takdirde ek bir ek olabilir. Devre dışı bırakmadan önce önbelleği kullanıp kullanmadığınızı kontrol edin.

### <a name="solution-for-cause-1"></a>Neden 1 için çözüm

Önbelleğe alma devre dışı olup olmadığını kontrol etmek için **önbellek=** girişini arayın.

**Önbellek=hiçbiri** önbelleğe almanın devre dışı bırakıldığını gösterir. Varsayılan önbelleğe alma veya "katı" önbelleğe alma modunun etkin olduğundan emin olmak için varsayılan montaj komutunu kullanarak veya **önbelleği açıkça** takma komutuna ekleyerek payı yeniden bağla.

Bazı senaryolarda, **serverino** montaj seçeneği **lS** komutu her dizin girişine karşı stat çalıştırmak için neden olabilir. Bu davranış, büyük bir dizin listelediğinizde performans düşüşüne neden olabilir. **/etc/fstab** girişinizdeki montaj seçeneklerini kontrol edebilirsiniz:

`//azureuser.file.core.windows.net/cifs /cifs cifs vers=2.1,serverino,username=xxx,password=xxx,dir_mode=0777,file_mode=0777`

Ayrıca **sudo montaj | grep cifs** komutu çalıştırarak ve çıktısını kontrol ederek doğru seçeneklerin kullanılıp kullanılmadığını da kontrol edebilirsiniz. Aşağıdaki örnek çıktı:

```
//azureuser.file.core.windows.net/cifs on /cifs type cifs (rw,relatime,vers=2.1,sec=ntlmssp,cache=strict,username=xxx,domain=X,uid=0,noforceuid,gid=0,noforcegid,addr=192.168.10.1,file_mode=0777, dir_mode=0777,persistenthandles,nounix,serverino,mapposix,rsize=1048576,wsize=1048576,actimeo=1)
```

**Önbellek=katı** veya **serverino** seçeneği yoksa, montaj komutunu [belgelerden](../storage-how-to-use-files-linux.md)çalıştırarak Azure Dosyalarını yeniden kaldırın ve monte edin. Daha sonra/ **etc/fstab** girişinin doğru seçeneklere sahip olup olmadığını yeniden kontrol edin.

### <a name="cause-2-throttling"></a>Neden 2: Azaltma

Azaltma yaşıyor olabilirsiniz ve istekleriniz bir kuyruğa gönderiliyor. [Azure Depolama ölçümlerinden](../common/storage-metrics-in-azure-monitor.md)yararlanarak bunu doğrulayabilirsiniz.

### <a name="solution-for-cause-2"></a>Neden 2 için çözüm

Uygulamanızın [Azure Dosyaları ölçeği hedefleri](storage-files-scale-targets.md#azure-files-scale-targets)içinde olduğundan emin olun.

<a id="timestampslost"></a>
## <a name="time-stamps-were-lost-in-copying-files-from-windows-to-linux"></a>Windows'dan Linux'a dosya kopyalamada zaman damgaları kayboldu

Linux/Unix platformlarında, farklı kullanıcıların kendi dosya 1 ve dosya 2'si varsa **cp -p** komutu başarısız olur.

### <a name="cause"></a>Nedeni

COPYFILE'daki kuvvet bayrağı **f,** Unix'te **cp -p -f'nin** yürütülmesiyle sonuçlanır. Bu komut, sahibi olmadığınız dosyanın zaman damgasını da koruyamaz.

### <a name="workaround"></a>Geçici çözüm

Dosyaları kopyalamak için depolama hesabı kullanıcısını kullanın:

- `Useadd : [storage account name]`
- `Passwd [storage account name]`
- `Su [storage account name]`
- `Cp -p filename.txt /share`

## <a name="ls-cannot-access-ltpathgt-inputoutput-error"></a>ls: '&lt;yol&gt;': Giriş/çıkış hatası erişemiyorum

LS komutunu kullanarak bir Azure dosya paylaşımındaki dosyaları listelemeye çalıştığınızda, dosyaları listelerken komut askıda kalır. Aşağıdaki hatayı alırsınız:

**ls:&lt;erişemiyorum'&gt;yolu ': Giriş/çıkış hatası**


### <a name="solution"></a>Çözüm
Linux çekirdeğini, bu soruna yönelik bir çözümü olan aşağıdaki sürümlere yükseltin:

- 4.4.87+
- 4.9.48+
- 4.12.11+
- 4,13'ten büyük veya eşit olan tüm sürümler

## <a name="cannot-create-symbolic-links---ln-failed-to-create-symbolic-link-t-operation-not-supported"></a>Sembolik bağlantılar oluşturamazsınız - ln: sembolik bağlantı 't' oluşturmak için başarısız: İşlem desteklenmiyor

### <a name="cause"></a>Nedeni
Varsayılan olarak, CIFS kullanarak Linux'ta Azure dosya paylaşımları oluşturmak sembolik bağlantılar (symlinks) desteğine olanak sağlamaz. Böyle bir hata görürsünüz:
```
ln -s linked -n t
ln: failed to create symbolic link 't': Operation not supported
```
### <a name="solution"></a>Çözüm
Linux CIFS istemcisi, Kobİ 2 veya 3 protokolü üzerinde Windows tarzı sembolik bağlantıların oluşturulmasını desteklemez. Şu anda, Linux istemcisi hem oluşturmak ve işlemleri takip etmek için [Minshall + Fransızca symlinks](https://wiki.samba.org/index.php/UNIX_Extensions#Minshall.2BFrench_symlinks) denilen sembolik bağlantıların başka bir stil destekler. Sembolik bağlantılara ihtiyaç duyan müşteriler "mfsymlinks" montaj seçeneğini kullanabilirler. Mac'lerin kullandığı biçim olduğu için "mfsymlinks" öneririz.

Symlinks kullanmak için, CIFS montaj komutunun sonuna aşağıdakileri ekleyin:

```
,mfsymlinks
```

Yani komut gibi bir şey görünüyor:

```
sudo mount -t cifs //<storage-account-name>.file.core.windows.net/<share-name> <mount-point> -o vers=<smb-version>,username=<storage-account-name>,password=<storage-account-key>,dir_mode=0777,file_mode=0777,serverino,mfsymlinks
```

Daha sonra [wiki'de](https://wiki.samba.org/index.php/UNIX_Extensions#Storing_symlinks_on_Windows_servers)önerildiği gibi symlinks oluşturabilirsiniz.

[!INCLUDE [storage-files-condition-headers](../../../includes/storage-files-condition-headers.md)]

<a id="error112"></a>
## <a name="mount-error112-host-is-down-because-of-a-reconnection-time-out"></a>"Montaj hatası(112): Ana bilgisayar düştü" çünkü yeniden bağlantı zaman aşımı

Linux istemcisi uzun süre boşta kaldığında istemcide "112" bağlama hatası oluşur. Uzun bir boşta kalma süresinden sonra istemcinin bağlantısı kesilir ve bağlantı zaman aşımına uğrar.  

### <a name="cause"></a>Nedeni

Bağlantı aşağıdaki nedenlerle boşta kalabilir:

-   Varsayılan "yazılım yoluyla" bağlama seçeneği kullanıldığında sunucuyla yeniden TCP bağlantısı kurulmasını engelleyen ağ iletişim hataları
-   Eski çekirdeklerde bulunmayan son yeniden bağlantı düzeltmeleri

### <a name="solution"></a>Çözüm

Linux çekirdeğindeki bu yeniden bağlantı sorunu artık aşağıdaki değişikliklerin bir parçası olarak düzeltildi:

- [Yuva yeniden bağlantısından sonra smb3 oturumunun yeniden bağlantısını geciktirmemek için yeniden bağlantı düzeltildi](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/fs/cifs?id=4fcd1813e6404dd4420c7d12fb483f9320f0bf93)
- [Yuva yeniden bağlantısından hemen sonra yankı hizmeti çağrısı](https://git.kernel.org/pub/scm/linux/kernel/git/torvalds/linux.git/commit/?id=b8c600120fc87d53642476f48c8055b38d6e14c7)
- [CIFS: Yeniden bağlanma sırasında olası bir bellek bozulmasını düzeltme](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=53e0e11efe9289535b060a51d4cf37c25e0d0f2b)
- [CIFS: Yeniden bağlanma sırasında mutex olası bir çift kilitleme düzeltmek (çekirdek v4.9 ve sonrası için)](https://git.kernel.org/cgit/linux/kernel/git/torvalds/linux.git/commit/?id=96a988ffeb90dba33a71c3826086fe67c897a183)

Öte yandan bu değişiklikler henüz tüm Linux dağıtımlarına taşınmamış olabilir. Popüler bir Linux dağıtımı kullanıyorsanız, dağıtımınızın hangi sürümünde gerekli çekirdek değişikliklerine sahip olduğunu görmek için [Linux ile Azure Dosyalarını Kullan'ı](storage-how-to-use-files-linux.md) denetleyebilirsiniz.

### <a name="workaround"></a>Geçici çözüm

Bu soruna geçici bir çözüm olarak donanım yoluyla bağlamayı belirtebilirsiniz. Donanım yoluyla bağlama istemciyi bağlantı kurulana kadar veya açıkça kesilene kadar beklemeye zorlar. Ağ zaman aşımlarından kaynaklanan hataları önlemek için bunu kullanabilirsiniz. Öte yandan bu geçici çözüm belirsiz bekleme sürelerine neden olabilir. Gerektiğinde bağlantıları durdurmaya hazırlıklı olun.

En son çekirdek sürümlerine yükseltemiyorsanız Azure dosya paylaşımında bir dosya tutup her 30 saniyede bir (veya daha sık) bu dosyaya yazarak bu soruna geçici bir çözüm getirebilirsiniz. Bunun, dosyaya oluşturma veya değiştirme tarihini yeniden yazma gibi bir yazma işlemi olması gerekir. Aksi takdirde önbelleğe alınan sonuçlar elde edebilirsiniz ve işleminiz yeniden bağlantıyı tetiklemeyebilir.

## <a name="cifs-vfs-error--22-on-ioctl-to-get-interface-list-when-you-mount-an-azure-file-share-by-using-smb-30"></a>SMB 3.0 kullanarak Bir Azure dosya paylaşımını monte ettiğinizde "CIFS VFS: arabirim listesini almak için ioctl'de hata -22"

### <a name="cause"></a>Nedeni
Azure Dosyaları [şu anda Kobİ çok kanallı'yı desteklemediği](https://docs.microsoft.com/rest/api/storageservices/features-not-supported-by-the-azure-file-service)için bu hata günlüğe kaydedilir.

### <a name="solution"></a>Çözüm
Bu hata yoksayılabilir.

## <a name="need-help-contact-support"></a>Yardıma mı ihtiyacınız var? Desteğe başvurun.

Hala yardıma ihtiyacınız varsa, sorunuzun hızla çözülmesi için [desteğe başvurun.](https://portal.azure.com/?#blade/Microsoft_Azure_Support/HelpAndSupportBlade)
