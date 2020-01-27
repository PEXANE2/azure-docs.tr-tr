---
title: Azure Dosya Eşitleme dağıtımı planlama | Microsoft Docs
description: Azure dosyaları dağıtımı için planlama yaparken göz önünde bulundurmanız gerekenler hakkında bilgi edinin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: d2dbe29c5a348363172f57da86483ccf3fd787f0
ms.sourcegitcommit: 05cdbb71b621c4dcc2ae2d92ca8c20f216ec9bc4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76046091"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Azure Dosya Eşitleme dağıtımı planlama
Şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu koruyarak kuruluşunuzun dosya paylaşımlarını Azure dosyalarında merkezileştirmek için Azure Dosya Eşitleme kullanın. Azure Dosya Eşitleme, Windows Server’ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. SMB, NFS ve FTPS dahil olmak üzere verilerinize yerel olarak erişmek için Windows Server 'da bulunan herhangi bir protokolü kullanabilirsiniz. Dünyanın dört bir yanında ihtiyacınız olan sayıda önbellekler olabilir.

Bu makalede Azure Dosya Eşitleme dağıtımı için önemli noktalar açıklanmaktadır. Ayrıca, [bir Azure dosyaları dağıtımı Için planlamayı](storage-files-planning.md)okumanızı öneririz. 

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

## <a name="azure-file-sync-terminology"></a>Azure Dosya Eşitleme terminolojisi
Azure Dosya Eşitleme dağıtımı için planlama ayrıntılarına geçmeden önce, terminolojiyi anlamak önemlidir.

### <a name="storage-sync-service"></a>Depolama eşitleme hizmeti
Depolama eşitleme hizmeti, Azure Dosya Eşitleme için en üst düzey Azure kaynağıdır. Depolama eşitleme hizmeti kaynağı, depolama hesabı kaynağının bir eşledir ve benzer şekilde Azure Kaynak gruplarına dağıtılabilir. Depolama eşitleme hizmeti birden çok eşitleme grubu aracılığıyla birden çok depolama hesabıyla eşitleme ilişkisi oluşturabileceğinden, depolama hesabı kaynağından ayrı bir en üst düzey kaynak gereklidir. Abonelikte birden fazla depolama eşitleme hizmeti kaynağı dağıtılabilir.

### <a name="sync-group"></a>Eşitleme grubu
Eşitleme grubu, bir dosya kümesi için eşitleme topolojisini tanımlar. Bir eşitleme grubu içindeki uç noktalar, birbiriyle eşitlenmiş durumda tutulur. Örneğin, Azure Dosya Eşitleme ile yönetmek istediğiniz iki farklı dosya kümesine sahipseniz, iki eşitleme grubu oluşturur ve her bir eşitleme grubuna farklı uç noktalar eklersiniz. Depolama eşitleme hizmeti, ihtiyacınız olan sayıda eşitleme grubunu barındırabilir.  

### <a name="registered-server"></a>Kayıtlı sunucu
Kayıtlı sunucu nesnesi, sunucunuz (veya kümeniz) ile depolama eşitleme hizmeti arasındaki bir güven ilişkisini temsil eder. Depolama eşitleme hizmeti örneğine istediğiniz sayıda sunucu kaydedebilirsiniz. Ancak, bir sunucu (veya küme) aynı anda yalnızca bir depolama eşitleme hizmeti ile kaydedilebilir.

### <a name="azure-file-sync-agent"></a>Azure Dosya Eşitleme Aracısı
Azure Dosya Eşitleme aracısı, Windows Server’ın bir Azure dosya paylaşımı ile eşitlenmesini sağlayan indirilebilir bir pakettir. Azure Dosya Eşitleme aracısının üç ana bileşeni vardır: 
- **FileSyncSvc. exe**: sunucu uç noktalarında yapılan değişiklikleri Izlemenin ve Azure 'da eşitleme oturumlarını başlatmaktan sorumlu olan arka plan hizmeti.
- **Storagessync. sys**: Azure dosya eşitleme dosya sistemi filtresi (bulut katmanlama etkinleştirildiğinde).
- **PowerShell yönetim cmdlet 'leri**: Microsoft. Storagessync Azure Kaynak sağlayıcısı ile etkileşim kurmak için kullandığınız PowerShell cmdlet 'leri. Bunları, aşağıdaki (varsayılan) konumlarda bulabilirsiniz:
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.PowerShell.Cmdlets.dll
    - C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll

### <a name="server-endpoint"></a>Sunucu uç noktası
Sunucu uç noktası, bir sunucu birimi üzerindeki klasör gibi kayıtlı bir sunucu üzerindeki belirli bir noktayı temsil eder. Aynı birimde, ad alanları çakışmazsa birden çok sunucu uç noktası bulunabilir (örneğin, `F:\sync1` ve `F:\sync2`). Bulut katmanlama ilkelerini, her sunucu uç noktası için ayrı ayrı yapılandırabilirsiniz. 

Bağlama noktası aracılığıyla sunucu uç noktası oluşturabilirsiniz. Sunucu uç noktası içindeki bağlama noktaları atlanır.  

Sistem biriminde bir sunucu uç noktası oluşturabilirsiniz, ancak bunu yaparsanız iki sınırlama vardır:
* Bulut katmanlaması etkinleştirilemiyor.
* Hızlı ad alanı geri yükleme (sistem ad alanının tamamını hızlı bir şekilde getirir ve içeriği geri çağırmak için başlar) gerçekleştirilmez.


> [!Note]  
> Yalnızca çıkarılabilir olmayan birimler desteklenir.  Uzak paylaşımdan eşlenen sürücüler sunucu uç noktası yolu için desteklenmez.  Ayrıca, bulut katmanlaması sistem biriminde desteklenmediğinden, bir sunucu uç noktası Windows sistem biriminde bulunabilir.

Bir eşitleme grubuna sunucu uç noktası olarak var olan bir dosya kümesine sahip bir sunucu konumu eklerseniz, bu dosyalar eşitleme grubundaki diğer uç noktalarında zaten bulunan diğer dosyalarla birleştirilir.

### <a name="cloud-endpoint"></a>Bulut uç noktası
Bulut uç noktası, bir eşitleme grubunun parçası olan bir Azure dosya paylaşımıdır. Azure dosya paylaşımının tamamı eşitlenir ve bir Azure dosya paylaşımının yalnızca bir bulut uç noktası üyesi olabilir. Bu nedenle, bir Azure dosya paylaşımında yalnızca bir eşitleme grubunun üyesi olabilir. Bir eşitleme grubuna bulut uç noktası olarak var olan bir dosya kümesine sahip bir Azure dosya paylaşma eklerseniz, mevcut dosyalar eşitleme grubundaki diğer uç noktalarında zaten bulunan diğer dosyalarla birleştirilir.

> [!Important]  
> Azure Dosya Eşitleme, doğrudan Azure dosya paylaşımında değişiklik yapmayı destekler. Ancak, ilk olarak Azure dosya paylaşımında yapılan tüm değişikliklerin bir Azure Dosya Eşitleme değişiklik algılama işi tarafından bulunması gerekir. Bir değişiklik algılama işi, her 24 saatte bir bulut uç noktası için başlatılır. Buna ek olarak, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB son değiştirilme zamanını güncelleştirmeyecektir ve eşitleme tarafından değişiklik olarak görünmez. Daha fazla bilgi için bkz. [Azure dosyaları hakkında sık sorulan sorular](storage-files-faq.md#afs-change-detection).

### <a name="cloud-tiering"></a>Bulut katmanlaması 
Bulut katmanlaması, sık erişilen dosyaların sunucu üzerinde yerel olarak önbelleğe alındığı, diğer tüm dosyaların ilke ayarlarına bağlı olarak Azure dosyaları ile katmanlandıkları Azure Dosya Eşitleme isteğe bağlı bir özelliğidir. Daha fazla bilgi için bkz. [bulut katmanlamayı anlama](storage-sync-cloud-tiering.md).

## <a name="azure-file-sync-system-requirements-and-interoperability"></a>Azure Dosya Eşitleme sistem gereksinimleri ve birlikte çalışabilirlik 
Bu bölüm, Windows Server özellikleri ve rolleri ve üçüncü taraf çözümleriyle Azure Dosya Eşitleme Aracı sistem gereksinimlerini ve birlikte çalışabilirliği ele alır.

### <a name="evaluation-cmdlet"></a>Değerlendirme cmdlet 'i
Azure Dosya Eşitleme dağıtılmadan önce, Azure Dosya Eşitleme değerlendirme cmdlet 'ini kullanarak sistem ile uyumlu olup olmadığını değerlendirmelisiniz. Bu cmdlet, dosya sisteminiz ve veri kümeniz ile desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü gibi olası sorunları denetler. Denetimleri, aşağıda bahsedilen özelliklerinin çoğunu kapsar; dağıtımınızın sorunsuz bir şekilde çalışmasını sağlamak için bu bölümün geri kalanını dikkatle okumanız önerilir. 

Değerlendirme cmdlet 'i, buradaki yönergeleri izleyerek yüklenebilen az PowerShell modülünün yüklenerek yüklenebilir: [Azure PowerShell yükleme ve yapılandırma](https://docs.microsoft.com/powershell/azure/install-Az-ps).

#### <a name="usage"></a>Kullanım  
Değerlendirme aracını birkaç farklı şekilde çağırabilirsiniz: sistem denetimleri, veri kümesi denetimleri veya her ikisini de yapabilirsiniz. Hem sistem hem de veri kümesi denetimleri gerçekleştirmek için: 

```powershell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path>
```

Yalnızca veri kümenizi test etmek için:
```powershell
    Invoke-AzStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Yalnızca sistem gereksinimlerini test etmek için:
```powershell
    Invoke-AzStorageSyncCompatibilityCheck -ComputerName <computer name>
```
 
Sonuçları CSV 'de göstermek için:
```powershell
    $errors = Invoke-AzStorageSyncCompatibilityCheck […]
    $errors | Select-Object -Property Type, Path, Level, Description | Export-Csv -Path <csv path>
```

### <a name="system-requirements"></a>Sistem Gereksinimleri
- Aşağıdaki işletim sistemi sürümlerinden birini çalıştıran bir sunucu:

    | Sürüm | Desteklenen SKU 'Lar | Desteklenen Dağıtım seçenekleri |
    |---------|----------------|------------------------------|
    | Windows Server 2019 | Veri merkezi ve standart | Tam ve çekirdek |
    | Windows Server 2016 | Veri merkezi ve standart | Tam ve çekirdek |
    | Windows Server 2012 R2 | Veri merkezi ve standart | Tam ve çekirdek |
    | Depolama için Windows Server IoT 2019| Veri merkezi ve standart | Tam ve çekirdek |
    | Windows Storage Server 2016| Veri merkezi ve standart | Tam ve çekirdek |
    | Windows Storage Server 2012 R2| Veri merkezi ve standart | Tam ve çekirdek |

    Windows Server 'ın gelecek sürümleri, yayımlandıklarında eklenecektir.

    > [!Important]  
    > Azure Dosya Eşitleme ile kullandığınız tüm sunucuları Windows Update en son güncelleştirmelerle güncel tutmanız önerilir. 

- En az 2 GiB belleğe sahip bir sunucu.

    > [!Important]  
    > Sunucu dinamik bellek etkinleştirilmiş bir sanal makinede çalışıyorsa, VM en az 2048 MIB ile yapılandırılmalıdır.
    
- NTFS dosya sistemiyle biçimlendirilen yerel olarak bağlanmış bir birim.

### <a name="file-system-features"></a>Dosya sistemi özellikleri

| Özellik | Destek durumu | Notlar |
|---------|----------------|-------|
| Erişim denetim listeleri (ACL’ler) | Tam olarak destekleniyor | Windows ACL 'Leri Azure Dosya Eşitleme tarafından korunur ve sunucu uç noktalarında Windows Server tarafından zorlanır. Dosyalara doğrudan bulutta erişiliyorsa, Windows ACL 'Leri (henüz) Azure dosyaları tarafından desteklenmez. |
| Sabit bağlantılar | Atlandı | |
| Sembolik bağlantılar | Atlandı | |
| Bağlama noktaları | Kısmen destekleniyor | Bağlama noktaları bir sunucu uç noktasının kökü olabilir, ancak sunucu uç noktasının ad alanında yer alıyorsa atlanır. |
| Merkezleriyle | Atlandı | Örneğin, DfrsrPrivate ve Dfskökleri klasörleri Dağıtılmış Dosya Sistemi. |
| Yeniden ayrıştırma noktaları | Atlandı | |
| NTFS sıkıştırması | Tam olarak destekleniyor | |
| Seyrek dosyalar | Tam olarak destekleniyor | Seyrek dosya eşitleme (engellenmez), ancak buluta tam dosya olarak eşitlenir. Bulutta dosya içerikleri değiştiğinde (veya başka bir sunucuda), değişiklik indirildiyse dosya artık seyrek olmaz. |
| Alternatif veri akışları (ADS) | Korunuyor, ancak eşitlenmedi | Örneğin, Dosya Sınıflandırma Altyapısı tarafından oluşturulan sınıflandırma etiketleri eşitlenmez. Sunucu uç noktalarında bulunan dosyalardaki mevcut sınıflandırma etiketleri dokunmaz. |

> [!Note]  
> Yalnızca NTFS birimleri desteklenir. ReFS, FAT, FAT32 ve diğer dosya sistemleri desteklenmez.

### <a name="files-skipped"></a>Atlanan dosyalar

| Dosya/klasör | Not |
|-|-|
| pagefile.sys | Sisteme özel dosya |
| Desktop. ini | Sisteme özel dosya |
| thumbs. db | Küçük resimler için geçici dosya |
| ehthumbs.db | Medya küçük resimleri için geçici dosya |
| ~$\*.\* | Office geçici dosyası |
| \*. tmp | Geçici dosya |
| \*.laccdb | DB kilitleme dosyasına erişin|
| 635D02A9D91C401B97884B82B3BCDAEA.* | İç eşitleme dosyası|
| Sistem birimi bilgilerini \\ | Birime özgü klasör |
| $RECYCLE. BÖLME| Klasör |
| \\eşitleme | Eşitleme klasörü |

### <a name="failover-clustering"></a>Yük Devretme Kümelemesi
Windows Server Yük Devretme Kümelemesi, "genel kullanım için dosya sunucusu" dağıtım seçeneği için Azure Dosya Eşitleme tarafından desteklenir. Yük Devretme Kümelemesi, "uygulama verileri için Genişleme Dosya Sunucusu" (SOFS) veya kümelenmiş paylaşılan birimlerde (CSV) desteklenmez.

> [!Note]  
> Eşitlemenin doğru çalışması için, bir yük devretme kümesindeki her düğümde Azure Dosya Eşitleme aracısının yüklü olması gerekir.

### <a name="data-deduplication"></a>Yinelenen Verileri Kaldırma
**Windows server 2016 ve Windows server 2019**   
Yinelenen verileri kaldırma, Windows Server 2016 ve Windows Server 2019 üzerinde bulut katmanlaması etkinleştirilmiş birimlerde desteklenir. Bulut katmanlaması etkinleştirilmiş bir birimde yinelenen verileri kaldırma özelliğinin etkinleştirilmesi, daha fazla depolama sağlamaya gerek kalmadan şirket içi daha fazla dosya önbelleğe almanızı sağlar. 

Bulut katmanlaması etkinleştirilmiş bir birimde yinelenen verileri kaldırma etkin olduğunda, sunucu uç noktası konumu içinde en iyi duruma getirilmiş dosyalar bulut katmanlama ilkesi ayarlarına bağlı olarak normal bir dosyaya benzer şekilde katmanlanacaktır. Yinelenenleri kaldırma için iyileştirilmiş dosyalar katmanlandıktan sonra, yinelenen verileri kaldırma atık toplama işi, artık birimdeki diğer dosyalar tarafından başvurulmayan gereksiz öbekleri kaldırarak disk alanını geri kazanmak için otomatik olarak çalışır.

Birim tasarruflarının yalnızca sunucuya uygulanacağını aklınızda yapın; Azure dosya paylaşımındaki verileriniz yinelenenleri kaldırma işlemi geri alınamaz.

> [!Note]  
> Windows Server 2019 ' de etkinleştirilmiş bulut katmanlaması olan birimlerde yinelenen verileri kaldırmayı desteklemek için Windows Update [KB4520062](https://support.microsoft.com/help/4520062) 'in yüklü olması ve Azure dosya eşitleme Aracı sürüm 9.0.0.0 veya daha yeni bir sürümü gereklidir.

**Windows Server 2012 R2**  
Azure Dosya Eşitleme, Windows Server 2012 R2 'deki aynı birimde yinelenen verileri kaldırma ve bulut katmanlaması desteklemez. Yinelenen verileri kaldırma özelliği bir birimde etkinleştirilmişse, bulut katmanlaması devre dışı bırakılmalıdır. 

**Notlar**
- Azure Dosya Eşitleme aracısını yüklemeden önce yinelenen verileri kaldırma yüklüyse, yinelenen verileri kaldırma ve bulut katmanlaması için aynı birimde yeniden başlatma gerekir.
- Bulut katmanlaması etkinleştirildikten sonra bir birimde yinelenen verileri kaldırma etkinleştirilmişse, ilk yinelenenleri kaldırma iyileştirme işi, zaten katmanlı olmayan ve bulut katmanlaması üzerinde aşağıdaki etkiye sahip olan dosyaları en iyi duruma getirir:
    - Boş alan ilkesi, heatmap kullanarak birimdeki boş alana göre dosyaları katmana devam edecektir.
    - Tarih ilkesi, dosyalara erişirken yinelenenleri kaldırma iyileştirme işi nedeniyle katmanlama için uygun olabilecek dosyaların katmanlamasını atlar.
- Devam eden yinelenenleri kaldırma iyileştirme işleri için, dosya zaten katmanlı değilse, tarih ilkesiyle bulut katmanlaması, yinelenen verileri kaldırma [Minimumfileagedays](https://docs.microsoft.com/powershell/module/deduplication/set-dedupvolume?view=win10-ps) ayarı tarafından gecikilir. 
    - Örnek: MinimumFileAgeDays ayarı yedi gün, bulut katmanlama tarihi ilkesi ise 30 gün ise, tarih ilkesi dosyaları 37 gün sonra katman olarak alır.
    - Note: bir dosya Azure Dosya Eşitleme göre katmanlandıktan sonra, yinelenenleri kaldırma iyileştirme işi dosyayı atlar.
- Azure Dosya Eşitleme Aracısı yüklü Windows Server 2012 R2 çalıştıran bir sunucu Windows Server 2016 veya Windows Server 2019 ' ye yükseltilirse, yinelenen verileri kaldırma ve bulut katmanlaması desteği için aşağıdaki adımlar gerçekleştirilmelidir:  
    - Windows Server 2012 R2 için Azure Dosya Eşitleme aracısını kaldırın ve sunucuyu yeniden başlatın.
    - Yeni sunucu işletim sistemi sürümü için Azure Dosya Eşitleme aracısını indirin (Windows Server 2016 veya Windows Server 2019).
    - Azure Dosya Eşitleme aracısını yükleyip sunucuyu yeniden başlatın.  
    
    Note: sunucu üzerindeki Azure Dosya Eşitleme yapılandırma ayarları, aracı kaldırıldığında ve yeniden yüklendiğinde tutulur.

### <a name="distributed-file-system-dfs"></a>Dağıtılmış Dosya Sistemi (DFS)
Azure Dosya Eşitleme, DFS ad alanları (DFS-N) ve DFS Çoğaltma (DFS-R) ile birlikte çalışabilirliği destekler.

**DFS ad alanları (DFS-n)** : Azure dosya EŞITLEME, DFS-N sunucularında tam olarak desteklenir. Sunucu uç noktaları ve bulut uç noktası arasında veri eşitlemek için Azure Dosya Eşitleme aracısını bir veya daha fazla DFS-N üyesine yükleyebilirsiniz. Daha fazla bilgi için bkz. [DFS ad alanlarına genel bakış](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview).
 
**DFS çoğaltma (DFS-r)** : DFS-r ve Azure dosya eşitleme her ikisi de çoğaltma çözümleri olduğundan, çoğu durumda DFS-r 'yi Azure dosya eşitleme değiştirmeniz önerilir. DFS-R ve Azure Dosya Eşitleme kullanmak istediğiniz birkaç senaryo vardır:

- Bir DFS-R dağıtımından Azure Dosya Eşitleme dağıtımına geçiriyoruz. Daha fazla bilgi için, [DFS çoğaltma (DFS-R) dağıtımını Azure dosya eşitleme geçirme](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync)konusuna bakın.
- Dosya verilerinizin bir kopyasına ihtiyaç duymayan her şirket içi sunucu, doğrudan internet 'e bağlanabilir.
- Şube sunucuları, Azure Dosya Eşitleme kullanmak istediğiniz tek bir hub sunucusu üzerinde verileri birleştirir.

Azure Dosya Eşitleme ve DFS-R 'nin yan yana çalışması için:

1. Azure Dosya Eşitleme bulut katmanlaması, DFS-R çoğaltılan klasörlerine sahip birimlerde devre dışı bırakılmalıdır.
2. Sunucu uç noktaları, DFS-R salt okuma çoğaltma klasörlerinde yapılandırılmamalıdır.

Daha fazla bilgi için bkz. [DFS çoğaltma genel bakış](https://technet.microsoft.com/library/jj127250).

### <a name="sysprep"></a>Sysprep
Azure Dosya Eşitleme aracısının yüklü olduğu bir sunucuda Sysprep kullanılması desteklenmez ve beklenmeyen sonuçlara yol açabilir. Aracı yüklemesi ve sunucu kaydı, sunucu görüntüsünü dağıttıktan ve Sysprep Mini Kurulumu tamamlandıktan sonra gerçekleşmelidir.

### <a name="windows-search"></a>Windows Search
Bulut katmanlaması bir sunucu uç noktasında etkinleştirilmişse, katmanlı dosyalar atlanır ve Windows Search tarafından dizine alınmamış olur. Katmanlı olmayan dosyalar doğru şekilde dizine alınır.

### <a name="antivirus-solutions"></a>Virüsten koruma çözümleri
Virüsten koruma, bilinen kötü amaçlı kod için dosyaları tarayarak çalıştığından, bir virüsten koruma ürünü katmanlı dosyaların geri çekmesine neden olabilir. Azure Dosya Eşitleme aracısının 4,0 ve üzeri sürümlerinde katmanlı dosyalar, güvenli Windows özniteliği FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS ayarlanmış olmalıdır. Bu öznitelik kümesi ile dosya okumayı atlamak üzere çözümlerini nasıl yapılandıracağınızı öğrenmek için yazılım satıcınıza danışmanız önerilir (birçok şey otomatik olarak yapılır). 

Microsoft 'un Şirket içi virüsten koruma çözümleri, Windows Defender ve System Center Endpoint Protection (SCEP), her ikisi de bu özniteliği ayarlanmış dosyaları okumayı otomatik olarak atlar. Bunları test ettik ve bir ikincil sorun tanımladı: var olan bir eşitleme grubuna sunucu eklediğinizde, yeni sunucuda 800 bayttan daha küçük dosyalar geri çekilir (indirilir). Bu dosyalar yeni sunucuda kalacak ve katmanlama boyut gereksinimini (> 64 KB) karşılamadığında katmanlanmayacak.

> [!Note]  
> Virüsten koruma satıcıları, Microsoft Indirme Merkezi ' nde indirileceği [Azure dosya eşitleme virüsten koruma Uyumluluk testi paketini](https://www.microsoft.com/download/details.aspx?id=58322)kullanarak ürün ve Azure dosya eşitleme arasındaki uyumluluğun denetlemesini denetleyebilir.

### <a name="backup-solutions"></a>Yedekleme çözümleri
Virüsten koruma çözümleri gibi, yedekleme çözümleri katmanlı dosyaların geri alınmasına neden olabilir. Şirket içi yedekleme ürünü yerine Azure dosya paylaşımının yedeklenmesi için bir bulut yedekleme çözümü kullanmanızı öneririz.

Şirket içi yedekleme çözümü kullanıyorsanız, yedeklemelerin bulut katmanlaması devre dışı olan eşitleme grubundaki bir sunucuda gerçekleştirilmesi gerekir. Geri yükleme gerçekleştirirken birim düzeyinde veya dosya düzeyinde geri yükleme seçeneklerini kullanın. Dosya düzeyi geri yükleme seçeneği kullanılarak geri yüklenen dosyalar, eşitleme grubundaki tüm uç noktalarla eşitlenir ve mevcut dosyalar yedekten geri yüklenen sürümle birlikte değişir.  Birim düzeyinde geri yüklemeler, Azure dosya paylaşımındaki veya diğer sunucu uç noktalarında yeni dosya sürümlerinin yerini olmayacaktır.

> [!Note]  
> Çıplak (BMR) geri yükleme beklenmeyen sonuçlara neden olabilir ve şu anda desteklenmemektedir.

> [!Note]  
> Azure Dosya Eşitleme aracısının 9. sürümünde, VSS anlık görüntüleri (önceki sürümler dahil), artık bulut katmanlaması etkinleştirilmiş birimlerde desteklenmektedir. Ancak, PowerShell aracılığıyla önceki sürüm uyumluluğunu etkinleştirmeniz gerekir. [Nasıl olduğunu öğrenin](storage-files-deployment-guide.md).

### <a name="encryption-solutions"></a>Şifreleme çözümleri
Şifreleme çözümleri için destek, nasıl uygulandığına bağlıdır. Azure Dosya Eşitleme ile birlikte çalışmak bilinmektedir:

- BitLocker şifreleme
- Azure Information Protection, Azure Rights Management Hizmetleri (Azure RMS) ve Active Directory RMS

Azure Dosya Eşitleme ile birlikte çalışmayan bilinen:

- NTFS şifreli dosya sistemi (EFS)

Genellikle, Azure Dosya Eşitleme BitLocker gibi dosya sisteminin altında bulunan şifreleme çözümleriyle birlikte çalışabilirliği ve Azure Information Protection gibi dosya biçiminde uygulanan çözümleri desteklemelidir. Dosya sisteminin üzerine (NTFS EFS gibi) ait çözümler için özel bir birlikte çalışabilirlik yapılmadı.

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Diğer hiyerarşik depolama yönetimi (HSM) çözümleri
Azure Dosya Eşitleme ile başka bir HSM çözümü kullanılmamalıdır.

## <a name="region-availability"></a>Bölgelere göre kullanılabilirlik
Azure Dosya Eşitleme yalnızca aşağıdaki bölgelerde kullanılabilir:

| Bölge | Veri merkezi konumu |
|--------|---------------------|
| Avustralya Doğu | Yeni Güney Galler |
| Avustralya Güneydoğu | Victoria |
| Brezilya Güney | Sao Paulo Eyaleti |
| Kanada Orta | Toronto |
| Kanada Doğu | Quebec City |
| Orta Hindistan | Pune |
| Orta ABD | Iowa |
| Doğu Asya | Hong Kong SAR |
| Doğu ABD | Virginia |
| Doğu ABD 2 | Virginia |
| Fransa Orta | Paris |
| Fransa Güney * | Marseille |
| Kore Orta | Seoul |
| Kore Güney | Busan |
| Japonya Doğu | Tokyo, Saitama |
| Japonya Batı | Osaka |
| Orta Kuzey ABD | Illinois |
| Kuzey Avrupa | İrlanda |
| Güney Afrika Kuzey | Johannesburg |
| Güney Afrika Batı * | Cape Town |
| Orta Güney ABD | Texas |
| Güney Hindistan | Chennai |
| Güneydoğu Asya | Singapur |
| UK Güney | Londra |
| UK, Batı | Cardiff |
| US Gov Arizona | Arizona |
| US Gov Teksas | Texas |
| ABD Hükümeti Virginia | Virginia |
| BAE Kuzey | Dubai |
| BAE Orta * | Abu Dabi |
| Batı Avrupa | Hollanda |
| Orta Batı ABD | Wyoming |
| Batı ABD | Kaliforniya |
| Batı ABD 2 | Washington DC |

Azure Dosya Eşitleme, yalnızca depolama eşitleme hizmeti ile aynı bölgedeki bir Azure dosya paylaşımıyla eşitlemeyi destekler.

Yıldız işaretleri ile işaretlenmiş bölgeler için, bu bölgelerde Azure depolama 'ya erişim istemek üzere Azure desteği 'ne başvurmanız gerekir. İşlem [Bu belgede](https://azure.microsoft.com/global-infrastructure/geographies/)özetlenmiştir.

### <a name="azure-disaster-recovery"></a>Azure olağanüstü durum kurtarma
Azure bölgesinin kaybedilmesine karşı korunmak için, Azure Dosya Eşitleme [coğrafi olarak yedekli depolama artıklığı](../common/storage-redundancy-grs.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json) (GRS) seçeneğiyle tümleştirilir. GRS depolama, birincil bölgedeki depolama alanı arasında zaman uyumsuz blok çoğaltma kullanarak çalışır ve bu da eşleştirilmiş ikincil bölgede depolama alanı. Bir Azure bölgesinin geçici veya kalıcı olarak çevrimdışı olmasına neden olan bir olağanüstü durum durumunda, Microsoft depolama 'yı eşleştirilmiş bölgeye devreder. 

> [!Warning]  
> Azure dosya paylaşımınızı bir GRS depolama hesabında bulut uç noktası olarak kullanıyorsanız, depolama hesabı yük devretmesini başlatmamanız gerekir. Bunun yapılması eşitlemenin çalışmayı durdurmasına neden olur ve yeni katmanlanmış dosyalar söz konusu olduğunda beklenmedik veri kaybına da yol açabilir. Azure bölgesinin kaybedilmesi durumunda, Microsoft, depolama hesabı yük devretmesini Azure Dosya Eşitleme ile uyumlu bir şekilde tetikleyecektir.

Coğrafi olarak yedekli depolama ve Azure Dosya Eşitleme arasında yük devretme tümleştirmesini desteklemek için, tüm Azure Dosya Eşitleme bölgeler, depolama tarafından kullanılan ikincil bölgeyle eşleşen bir ikincil bölgeyle eşleştirilir. Bu çiftler aşağıdaki gibidir:

| Birincil bölge      | Eşleştirilmiş bölge      |
|---------------------|--------------------|
| Avustralya Doğu      | Avustralya Güneydoğu|
| Avustralya Güneydoğu | Avustralya Doğu     |
| Brezilya Güney        | Orta Güney ABD   |
| Kanada Orta      | Kanada Doğu        |
| Kanada Doğu         | Kanada Orta     |
| Orta Hindistan       | Güney Hindistan        |
| Orta ABD          | Doğu ABD 2          |
| Doğu Asya           | Güneydoğu Asya     |
| Doğu ABD             | Batı ABD            |
| Doğu ABD 2           | Orta ABD         |
| Fransa Orta      | Fransa Güney       |
| Fransa Güney        | Fransa Orta     |
| Japonya Doğu          | Japonya Batı         |
| Japonya Batı          | Japonya Doğu         |
| Kore Orta       | Kore Güney        |
| Kore Güney         | Kore Orta      |
| Kuzey Avrupa        | Batı Avrupa        |
| Orta Kuzey ABD    | Orta Güney ABD   |
| Güney Afrika Kuzey  | Güney Afrika Batı  |
| Güney Afrika Batı   | Güney Afrika Kuzey |
| Orta Güney ABD    | Orta Kuzey ABD   |
| Güney Hindistan         | Orta Hindistan      |
| Güneydoğu Asya      | Doğu Asya          |
| UK Güney            | UK, Batı            |
| UK, Batı             | UK Güney           |
| US Gov Arizona      | US Gov Teksas       |
| US Gov Iowa         | ABD Hükümeti Virginia    |
| ABD Hükümeti Virginia      | US Gov Teksas       |
| Batı Avrupa         | Kuzey Avrupa       |
| Orta Batı ABD     | Batı ABD 2          |
| Batı ABD             | Doğu ABD            |
| Batı ABD 2           | Orta Batı ABD    |

## <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="recommended-azure-file-sync-machine-configuration"></a>Azure Dosya Eşitleme makine yapılandırması önerilir

Azure Dosya Eşitleme makine gereksinimleri, ad alanındaki nesne sayısına ve veri kümesindeki dalgalanma göre belirlenir. Tek bir sunucu birden çok eşitleme grubuna ve bir sunucunun eklendiği tam ad alanı için aşağıdaki tablo hesaplarında listelenen nesne sayısına bağlanabilir. Örneğin, 10.000.000 nesneleriyle sunucu uç noktası A ve 10.000.000 Objects = 20.000.000 nesneleriyle sunucu uç noktası B. Bu örnek dağıtım için, ilk geçiş için, sabit durum için 8CPU, 16GiB bellek ve (mümkünse) 48GiB bellek önereceğiz.
 
Ad alanı verileri performans nedenleriyle bellekte depolanır. Bu nedenle, daha büyük ad alanları iyi performans sağlamak için daha fazla bellek gerektirir ve daha fazla dalgalanma daha fazla CPU gerektirir. 
 
Aşağıdaki tabloda, ad alanının boyutunu ve tipik genel amaçlı dosya paylaşımları için kapasiteye dönüştürme işleminin yanı sıra ortalama dosya boyutu 512Kıb ' dir. Dosya boyutlarınız daha küçükse, aynı kapasite miktarı için ek bellek eklemeyi göz önünde bulundurun. Bellek yapılandırmanızı ad alanının boyutuna dayandırın.

| Ad alanı boyutu-dosyalar & dizinler (milyonlarca)  | Tipik kapasite (TiB)  | CPU Çekirdekleri  | Önerilen bellek (GiB) |
|---------|---------|---------|---------|
| 3        | 1.4     | 2        | 8 (ilk eşitleme)/2 (tipik dalgalanma)      |
| 5        | 2.4     | 2        | 16 (ilk eşitleme)/4 (tipik dalgalanma)    |
| 10       | 4.8     | 4        | 32 (ilk eşitleme)/8 (tipik dalgalanma)   |
| 30       | 14,3    | 8        | 48 (ilk eşitleme)/16 (tipik dalgalanma)   |
| 50       | 23,8    | 16       | 64 (ilk eşitleme)/32 (tipik dalgalanma)  |
| 100 *     | 47,7   | 32       | 128 (ilk eşitleme)/32 (tipik dalgalanma)  |

\*100.000.000 'den fazla dosya & Dizin sınanmamıştır. Bu bir hafif sınır.

> [!TIP]
> Bir ad alanının ilk eşitlemesi yoğun bir işlemdir ve ilk eşitleme tamamlanana kadar daha fazla bellek ayırmayı öneririz. Bu gerekli değildir, ancak ilk eşitlemeyi hızlandırabilir. 
> 
> Normal karmaşıklık, her gün değişen ad alanı% 0,5 ' dir. Daha yüksek dalgalanma düzeyleri için daha fazla CPU eklemeyi düşünün. 

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenlik duvarını ve proxy ayarlarını değerlendirin](storage-sync-files-firewall-and-proxy.md)
* [Azure Dosyaları dağıtımını planlama](storage-files-planning.md)
* [Azure dosyalarını dağıtma](storage-files-deployment-guide.md)
* [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
* [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)
