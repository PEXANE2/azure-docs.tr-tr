---
title: Azure Dosya Eşitleme dağıtımı için planlama | Microsoft Dokümanlar
description: Azure Dosyaları dağıtımı planlarken nelere dikkat etmen gerektiğini öğrenin.
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 01/15/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 0684f626553946619a0db2cd895df39576bd17b9
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79255125"
---
# <a name="planning-for-an-azure-file-sync-deployment"></a>Azure Dosya Eşitleme dağıtımı planlama
[Azure Dosyaları](storage-files-introduction.md) iki ana şekilde dağıtılabilir: sunucusuz Azure dosya paylaşımlarını doğrudan monte ederek veya Azure Dosya Eşitlemeyi'ni kullanarak şirket içinde Azure dosya paylaşımlarını önbelleğe alarak. Hangi dağıtım seçeneğini seçtiğiniz, dağıtımınızı planlarken göz önünde bulundurmanız gereken şeyleri değiştirir. 

- **Azure dosya paylaşımının doğrudan bindirilebilirliği**: Azure Dosyaları Kobİ erişimi sağladığından, Windows, macOS ve Linux'ta kullanılabilen standart SMB istemcisini kullanarak Azure dosya paylaşımlarını şirket içinde veya buluta monte edebilirsiniz. Azure dosya paylaşımları sunucusuz olduğundan, üretim senaryoları için dağıtım için bir dosya sunucusu nun veya NAS aygıtının yönetilmesi gerekmez. Bu, yazılım yamaları uygulamak veya fiziksel diskleri değiştirmek zorunda olmadığınız anlamına gelir. 

- **Azure dosya paylaşımını Azure Dosya Eşitleme**: Azure Dosya Eşitlemi ile şirket içi paylaşma, kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda merkezileştirmenizi sağlarken, şirket içi bir dosya sunucusunun esnekliğini, performansını ve uyumluluğunu korumanızı sağlar. Azure Dosya Eşitlemi, şirket içi (veya bulut) bir Windows Server'ı Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürür. 

Bu makalede, öncelikle Azure Dosya Eşitleme dağıtma için dağıtım hususları giderilmesi. Azure dosya paylaşımlarının şirket içi veya bulut istemcisi tarafından doğrudan monte edilebis olmasını planlamak [için](storage-files-planning.md)bkz.

## <a name="management-concepts"></a>Yönetim kavramları
Azure Dosya Eşitleme dağıtımının üç temel yönetim nesnesi vardır:

- **Azure dosya paylaşımı**: Azure dosya paylaşımı, Azure Dosya Eşitleme ilişkisinin *bulut bitiş noktasını* sağlayan sunucusuz bir bulut dosyası paylaşımıdır. Azure dosya paylaşımındaki dosyalara doğrudan SMB veya FileREST protokolü ile erişilebilir, ancak Azure dosya paylaşımı Azure Dosya Eşitlemi ile kullanılırken öncelikle Windows Server önbelleği üzerinden dosyalara erişmenizi öneririz. Bunun nedeni, Azure Dosyaları'nın günümüzde Windows Server gibi etkin bir değişiklik algılama mekanizmasına sahip olmamasıdır, bu nedenle Azure dosya paylaşımındaki değişikliklerin sunucu uç noktalarına geri yayılması zaman alacaktır.
- **Sunucu bitiş noktası**: Windows Server'da Azure dosya paylaşımıyla eşitlenen yol. Bu, bir birimdeki belirli bir klasör veya birimin kökü olabilir. Ad alanları çakışmazsa, aynı birimde birden çok sunucu uç noktası bulunabilir.
- **Eşitleme grubu**: **Bulut bitiş noktası**ile Azure dosya paylaşımı ile sunucu bitiş noktası arasındaki eşitleme ilişkisini tanımlayan nesnedir. Bir eşitleme grubu içindeki uç noktalar, birbiriyle eşitlenmiş durumda tutulur. Örneğin, Azure Dosya Eşitlemi ile yönetmek istediğiniz iki farklı dosya kümeniz varsa, iki eşitleme grubu oluşturur ve her eşitleme grubuna farklı uç noktalar eklersiniz.

### <a name="azure-file-share-management-concepts"></a>Azure dosya paylaşımı yönetimi kavramları
[!INCLUDE [storage-files-file-share-management-concepts](../../../includes/storage-files-file-share-management-concepts.md)]

### <a name="azure-file-sync-management-concepts"></a>Azure Dosya Eşitleme yönetimi kavramları
Eşitleme grupları, Azure Dosya Eşitlemi ile kullanılmak üzere sunucuları kaydeden ve eşitgrup ilişkilerini içeren üst düzey nesneler olan **Depolama Eşitleme Hizmetleri'ne**dağıtılır. Depolama Eşitleme Hizmeti kaynağı, depolama hesabı kaynağının bir eşidir ve benzer şekilde Azure kaynak gruplarına dağıtılabilir. Depolama Eşitleme Hizmeti, birden çok depolama hesabı ve birden çok kayıtlı Windows Sunucusu arasında Azure dosya paylaşımları içeren eşitleme grupları oluşturabilir.

Depolama Eşitleme Hizmeti'nde eşitleme grubu oluşturmadan önce, önce Depolama Eşitleme Hizmeti'ne bir Windows Sunucusu kaydetmeniz gerekir. Bu, sunucunuz veya kümeniz ile Depolama Eşitleme Hizmeti arasında güven ilişkisini temsil eden kayıtlı bir **sunucu** nesnesi oluşturur. Depolama Eşitleme Hizmeti kaydetmek için öncelikle Sunucuya Azure Dosya Eşitleme aracısını yüklemeniz gerekir. Tek bir sunucu veya küme aynı anda yalnızca bir Depolama Eşitleme Hizmetine kaydedilebilir.

Eşitleme grubu bir bulut bitiş noktası veya Azure dosya paylaşımı ve en az bir sunucu bitiş noktası içerir. Sunucu bitiş noktası nesnesi, Azure Dosya Eşitlemi önbelleğe alma özelliğini sağlayan **bulut katmanlama** özelliğini yapılandıran ayarları içerir. Azure dosya paylaşımıyla eşitlemek için Azure dosya paylaşımını içeren depolama hesabının Depolama Eşitleme Hizmeti ile aynı Azure bölgesinde olması gerekir.

### <a name="management-guidance"></a>Yönetim rehberliği
Azure Dosya Eşitlemeyi'ni dağıtırken şunları öneririz:

- Azure dosyalarını dağıtma, Windows dosya paylaşımlarıyla 1:1 paylaşır. Sunucu bitiş noktası nesnesi, eşitleme ilişkisinin sunucu tarafında eşitleme topolojisini nasıl kurduğunuz konusunda size büyük ölçüde esneklik sağlar. Yönetimi basitleştirmek için, sunucu bitiş noktasıyolunun Windows dosya paylaşımının yolu ile eşleşmesini sağlar. 

- Mümkün olduğunca az Depolama Eşitleme Hizmeti kullanın. Bu, birden çok sunucu bitiş noktası içeren eşitleme gruplarınız olduğunda yönetimi basitleştirir, çünkü bir Windows Server aynı anda yalnızca bir Depolama Eşitleme Hizmetine kaydedilebilir. 

- Azure dosya paylaşımlarını dağıtırken depolama hesabının IOPS sınırlamalarına dikkat edin. İdeal olarak, dosya paylaşımlarını depolama hesaplarıyla 1:1 eşlersiniz, ancak bu, hem kuruluşunuz hem de Azure'dan çeşitli sınırlar ve kısıtlamalar nedeniyle her zaman mümkün olmayabilir. Bir depolama hesabında yalnızca bir dosya paylaşımının dağıtılması mümkün olmadığında, en büyük dosya paylaşımların aynı depolama hesabına birlikte yerleştirilmediğinden emin olmak için hangi hisselerin son derece etkin olacağını ve hangi hisselerin daha az etkin olacağını düşünün.

## <a name="windows-file-server-considerations"></a>Windows dosya sunucusu hususlar
Windows Server'da eşitleme özelliğini etkinleştirmek için Azure Dosya Eşitlenebilir aracısını yüklemeniz gerekir. Azure Dosya Eşitleme aracısı `FileSyncSvc.exe`iki ana bileşen sağlar: sunucu bitiş noktalarındaki değişiklikleri izlemekten ve eşitleme oturumlarını başlatmakdan sorumlu arka plan Windows hizmeti ve `StorageSync.sys`bulut katmanlama ve hızlı olağanüstü durum kurtarma sağlayan bir dosya sistemi filtresi.  

### <a name="operating-system-requirements"></a>İşletim sistemi gereksinimleri
Azure Dosya Eşitlemi, Windows Server'ın aşağıdaki sürümleriyle desteklenir:

| Sürüm | Desteklenen SK'ler | Desteklenen dağıtım seçenekleri |
|---------|----------------|------------------------------|
| Windows Server 2019 | Veri Merkezi, Standart ve IoT | Tam ve Çekirdek |
| Windows Server 2016 | Veri Merkezi, Standart ve Depolama Sunucusu | Tam ve Çekirdek |
| Windows Server 2012 R2 | Veri Merkezi, Standart ve Depolama Sunucusu | Tam ve Çekirdek |

Windows Server'ın gelecekteki sürümleri yayımlandıkça eklenecektir.

> [!Important]  
> Azure Dosya Eşitlemi ile kullandığınız tüm sunucuları Windows Update'teki en son güncelleştirmelerle güncel tutmanızı öneririz. 

### <a name="minimum-system-resources"></a>Minimum sistem kaynakları
Azure Dosya Eşitleme, fiziksel veya sanal, en az bir CPU ve en az 2 GiB bellek içeren bir sunucu gerektirir.

> [!Important]  
> Sunucu dinamik bellek etkin leştirilmiş sanal bir makinede çalışıyorsa, VM en az 2048 MiB bellekle yapılandırılmalıdır.

Çoğu üretim iş yükü için, bir Azure Dosya Eşitleme eşitleme sunucusunu yalnızca minimum gereksinimlerle yapılandırmanızı önermiyoruz. Daha fazla bilgi için [önerilen sistem kaynaklarına](#recommended-system-resources) bakın.

### <a name="recommended-system-resources"></a>Önerilen sistem kaynakları
Herhangi bir sunucu özelliği veya uygulaması gibi, Azure Dosya Eşitlemesine yönelik sistem kaynak gereksinimleri dağıtım ölçeğine göre belirlenir; sunucudaki daha büyük dağıtımlar daha büyük sistem kaynakları gerektirir. Azure Dosya Eşitleme için ölçek, sunucu uç noktalarındaki nesne sayısına ve veri kümesindeki karmaşaya göre belirlenir. Tek bir sunucu, birden çok eşitleme grubunda sunucu uç noktalarına ve bir sunucunun bağlı olduğu tam ad alanı için aşağıdaki tabloda listelenen nesne sayısına sahip olabilir. 

Örneğin, 10 milyon nesne + sunucu bitiş noktası B ile 10 milyon nesne = 20 milyon nesne ile sunucu bitiş noktası A. Bu örnek dağıtım için, sabit durum için 8 CPU, 16 GiB bellek ve (mümkünse) ilk geçiş için bellek 48 GiB öneririz.
 
Ad alanı verileri performans nedenleriyle bellekte depolanır. Bu nedenle, daha büyük ad alanları iyi performansı korumak için daha fazla bellek gerektirir ve daha fazla karmaşa işlemek için daha fazla CPU gerektirir. 
 
Aşağıdaki tabloda, hem ad alanının boyutunu hem de ortalama dosya boyutunun 512 KiB olduğu tipik genel amaçlı dosya paylaşımları için kapasiteye dönüştürme sağladık. Dosya boyutlarınız daha küçükse, aynı kapasite miktarı için ek bellek eklemeyi düşünün. Bellek yapılandırmanızı ad alanının boyutuna dayandırın.

| Namespace boyutu - dosyaları & dizinleri (milyonlarca)  | Tipik kapasite (TiB)  | CPU Çekirdekleri  | Önerilen bellek (GiB) |
|---------|---------|---------|---------|
| 3        | 1.4     | 2        | 8 (ilk eşitleme)/ 2 (tipik churn)      |
| 5        | 2.3     | 2        | 16 (ilk eşitleme)/ 4 (tipik churn)    |
| 10       | 4.7     | 4        | 32 (ilk eşitleme)/ 8 (tipik churn)   |
| 30       | 14.0    | 8        | 48 (ilk eşitleme)/ 16 (tipik churn)   |
| 50       | 23.3    | 16       | 64 (ilk eşitleme)/ 32 (tipik churn)  |
| 100*     | 46.6    | 32       | 128 (ilk eşitleme)/ 32 (tipik churn)  |

\*100 milyondan fazla dosya& dizinleri eşitleme şu anda önerilmez. Bu, test edilmiş eşiklerimize dayalı yumuşak bir sınırdır. Daha fazla bilgi için Azure [Dosyaları ölçeklenebilirliği ve performans hedefleri'ne](storage-files-scale-targets.md#azure-file-sync-scale-targets)bakın.

> [!TIP]
> Ad alanının ilk eşitlemesi yoğun bir işlemdir ve ilk eşitleme tamamlanana kadar daha fazla bellek ayırmanızı öneririz. Bu gerekli değildir, ancak, ilk eşitleme hızlandırabilir. 
> 
> Tipik karmaşa günde değişen ad alanının %0,5'idir. Daha yüksek karmaşa düzeyleri için daha fazla CPU eklemeyi düşünün. 

- NTFS dosya sistemiyle biçimlendirilmiş yerel olarak bağlı bir birim.

### <a name="evaluation-cmdlet"></a>Değerlendirme cmdlet
Azure Dosya Eşitlemesi'ni dağıtmadan önce, Azure Dosya Eşitleme değerlendirme cmdlet'ini kullanarak sisteminizle uyumlu olup olmadığını değerlendirmeniz gerekir. Bu cmdlet, desteklenmeyen karakterler veya desteklenmeyen işletim sistemi sürümü gibi dosya sisteminiz ve veri kümenizle ilgili olası sorunları denetler. Çekleri, aşağıda belirtilen özelliklerin çoğunu kapsamaz, ancak tümünü kapsamaz; dağıtımınızın sorunsuz geçmesini sağlamak için bu bölümün geri kalanını dikkatle okumanızı öneririz. 

Değerlendirme cmdlet burada yönergeleri izleyerek yüklenebilir Az PowerShell modülü yükleyerek yüklenebilir: [Yükleyin ve Azure PowerShell yapılandırmak](https://docs.microsoft.com/powershell/azure/install-Az-ps).

#### <a name="usage"></a>Kullanım  
Değerlendirme aracını birkaç farklı şekilde çağırabilirsiniz: sistem denetimlerini, veri kümesi denetimlerini veya her ikisini birden gerçekleştirebilirsiniz. Hem sistem hem de veri kümesi denetimlerini gerçekleştirmek için: 

```powershell
Invoke-AzStorageSyncCompatibilityCheck -Path <path>
```

Yalnızca veri kümenizi sınamak için:
```powershell
Invoke-AzStorageSyncCompatibilityCheck -Path <path> -SkipSystemChecks
```
 
Yalnızca sistem gereksinimlerini test etmek için:
```powershell
Invoke-AzStorageSyncCompatibilityCheck -ComputerName <computer name>
```
 
Sonuçları CSV'de görüntülemek için:
```powershell
$errors = Invoke-AzStorageSyncCompatibilityCheck […]
$errors | Select-Object -Property Type, Path, Level, Description | Export-Csv -Path <csv path>
```

### <a name="file-system-compatibility"></a>Dosya sistemi uyumluluğu
Azure Dosya Eşitlemi yalnızca doğrudan bağlı NTFS birimlerinde desteklenir. Windows Server'da doğrudan bağlı depolama veya DAS, dosya sisteminin Windows Server işletim sisteminin sahibi olduğu anlamına gelir. DAS, diskleri dosya sunucusuna fiziksel olarak takarak, sanal diskleri bir dosya sunucusu VM'ye (Hyper-V tarafından barındırılan VM gibi) takarak ve hatta ISCSI aracılığıyla sağlanabilir.

Yalnızca NTFS birimleri desteklenir; ReFS, FAT, FAT32 ve diğer dosya sistemleri desteklenmez.

Aşağıdaki tablo, NTFS dosya sistemi özelliklerinin interop durumunu gösterir: 

| Özellik | Destek durumu | Notlar |
|---------|----------------|-------|
| Erişim denetim listeleri (ACL’ler) | Tam destekli | Windows tarzı isteğe bağlı erişim denetim listeleri Azure Dosya Eşitlemi tarafından korunur ve sunucu uç noktalarında Windows Server tarafından zorlanır. Azure dosya paylaşımını doğrudan monte ederken ALAK'lar da uygulanabilir, ancak bu ek yapılandırma gerektirir. Daha fazla bilgi için [Kimlik bölümüne](#identity) bakın. |
| Sabit bağlantılar | Atlandı | |
| Sembolik bağlantılar | Atlandı | |
| Bağlama noktaları | Kısmen desteklenen | Montaj noktaları bir sunucu bitiş noktasının kökü olabilir, ancak bir sunucu bitiş noktasının ad alanında bulunursa atlanır. |
| Kavşak | Atlandı | Örneğin, Dağıtılmış Dosya Sistemi DfrsrPrivate ve DFSRoots klasörleri. |
| Yeniden ayrıştırma noktaları | Atlandı | |
| NTFS sıkıştırma | Tam destekli | |
| Seyrek dosyalar | Tam destekli | Seyrek dosyalar eşitlenir (engellenmez), ancak buluta tam dosya olarak eşitlenirler. Dosya içeriği bulutta (veya başka bir sunucuda) değişirse, değişiklik indirildiğinde dosya artık seyrek değildir. |
| Alternatif Veri Akışları (ADS) | Korunmuş, ancak eşitlenmemiş | Örneğin, Dosya Sınıflandırma Altyapısı tarafından oluşturulan sınıflandırma etiketleri eşitlenmez. Sunucu uç noktalarının her birinde bulunan dosyalardaki varolan sınıflandırma etiketlerine dokunulmaz. |

<a id="files-skipped"></a>Azure Dosya Eşitlemi ayrıca belirli geçici dosyaları ve sistem klasörlerini atlar:

| Dosya/klasör | Not |
|-|-|
| pagefile.sys | Sisteme özgü dosya |
| Desktop.ini | Sisteme özgü dosya |
| Thumbs | Küçük resimler için geçici dosya |
| ehthumbs.db | Ortam küçük resimleri için geçici dosya |
| ~$\*.\* | Ofis geçici dosyası |
| \*Tmp | Geçici dosya |
| \*.laccdb | Erişim DB kilitleme dosyası|
| 635D02A9D91C401B97884B82B3BCDAEA.* | İç Eşitleme dosyası|
| \\Sistem Hacim Bilgileri | Birime özgü klasör |
| $RECYCLE. BIN| Klasör |
| \\SyncShareState | Eşitleme için Klasör |

### <a name="failover-clustering"></a>Yük Devretme Kümelemesi
Windows Server Failover Clustering, "Genel kullanım için Dosya Sunucusu" dağıtım seçeneği için Azure Dosya Eşitlemesi tarafından desteklenir. Failover Clustering, "Uygulama verileri için Ölçeklendir-Out Dosya Sunucusu" (SOFS) veya Kümelenmiş Paylaşılan Birimler (CSV) üzerinde desteklenmez.

> [!Note]  
> Eşitlemenin düzgün çalışması için Azure Dosya Eşitleme aracısının Failover Kümesi'ndeki her düğüme yüklenmesi gerekir.

### <a name="data-deduplication"></a>Yinelenen Verileri Kaldırma
**Windows Server 2016 ve Windows Server 2019**   
Veri Çoğaltma, Windows Server 2016 ve Windows Server 2019'da etkin leştirilmiş bulut katmanlama ile birimlerde desteklenir. Bulut katmanlama etkinleştirilmiş bir birimde Veri Çoğaltma'yı etkinleştirmek, daha fazla depolama alanı sağlamadan şirket içinde daha fazla dosyaönbelleğe sahip yapmanızı sağlar. 

Veri Çoğaltma, bulut katmanlama etkinleştirilmiş bir birimde etkinleştirildiğinde, Dedup optimize edilmiş dosyalar sunucu bitiş noktası konumu ndaki bulut katmanlama ilkesi ayarlarını temel alan normal bir dosyaya benzer şekilde katmanlanır. Dedup en iyi duruma getirilmiş dosyalar katmanlandıktan sonra, Veri Çoğaltma çöp toplama işi, birimdeki diğer dosyalar tarafından artık başvurulamayan gereksiz parçaları kaldırarak disk alanını geri almak için otomatik olarak çalışır.

Ses tasarrufunun yalnızca sunucu için geçerli olduğunu unutmayın; Azure dosya paylaşımındaki verileriniz kopyalanmaz.

> [!Note]  
> Windows Server 2019'da bulut katmanlama etkinleştirilmiş birimlerde Veri Çoğaltma'yı desteklemek için Windows update [KB4520062'nin](https://support.microsoft.com/help/4520062) yüklenmesi ve Azure Dosya Eşitleme aracısı sürümü 9.0.0.0.0 veya daha yeni olması gerekir.

**Windows Server 2012 R2**  
Azure Dosya Eşitlemesi, Windows Server 2012 R2'de aynı ses biriminde Veri Çoğaltma ve bulut katmanlamayı desteklemez. Veri Çoğaltma bir birimde etkinleştirilmişse, bulut katmanlama devre dışı edilmelidir. 

**Notlar**
- Azure Dosya Eşitleme aracısını yüklemeden önce Veri Çoğaltma yüklüyse, Aynı birimde Veri Çoğaltma ve bulut katmanlamayı desteklemek için yeniden başlatma gerekir.
- Bulut katmanlama etkinleştirildikten sonra bir birimde Veri Çoğaltma etkinleştirilirse, ilk Çoğaltma optimizasyonu işi zaten katmanlı olmayan birimdeki dosyaları optimize eder ve bulut katmanlama üzerinde aşağıdaki etkiye sahip olur:
    - Boş alan politikası, ısı haritasını kullanarak hacimdeki boş alana göre dosyaları katmana almaya devam edecektir.
    - Tarih ilkesi, dosyalara erişen Çoğaltma optimizasyonu işi nedeniyle katmanlamaya uygun olabilecek dosyaların katmanlamasını atlar.
- Devam eden Çoğaltma optimizasyonu işleri için, tarih ilkesiyle bulut katmanlama, dosya zaten katmanlı değilse, Veri Çoğaltma [MinimumFileAgeDays](https://docs.microsoft.com/powershell/module/deduplication/set-dedupvolume?view=win10-ps) ayarı tarafından geciktirilir. 
    - Örnek: MinimumFileAgeDays ayarı yedi gün ve bulut katmanlama tarihi ilkesi 30 gün ise, tarih ilkesi dosyaları 37 gün sonra katmanlar.
    - Not: Bir dosya Azure Dosya Eşitlemesi tarafından katmanlandıktan sonra, Çoğaltma en iyi duruma getirilmesi işi dosyayı atlar.
- Windows Server 2012 R2'yi Azure Dosya Eşitleme aracısı yüklü bir sunucu Windows Server 2016 veya Windows Server 2019'a yükseltiyorsa, aynı birimde Veri Çoğaltma ve bulut katmanlamayı desteklemek için aşağıdaki adımların gerçekleştirilmesi gerekir:  
    - Windows Server 2012 R2 için Azure Dosya Eşitleme aracısını kaldırın ve sunucuyu yeniden başlatın.
    - Yeni sunucu işletim sistemi sürümü (Windows Server 2016 veya Windows Server 2019) için Azure Dosya Eşitleme aracısını indirin.
    - Azure Dosya Eşitleme aracısını yükleyin ve sunucuyu yeniden başlatın.  
    
    Not: Aracı kaldırıldığında ve yeniden yüklendiğinde sunucudaki Azure Dosya Eşitleme yapılandırma ayarları korunur.

### <a name="distributed-file-system-dfs"></a>Dağıtılmış Dosya Sistemi (DFS)
Azure Dosya Eşitlemesi, DFS Ad Alanları (DFS-N) ve DFS Çoğaltma (DFS-R) ile interop'u destekler.

**DFS Ad Alanları (DFS-N)**: Azure Dosya Eşitlemi, DFS-N sunucularında tam olarak desteklenir. Sunucu bitiş noktaları ile bulut bitiş noktası arasındaki verileri eşitlemek için Azure Dosya Eşitleme aracısını bir veya daha fazla DFS-N üyesine yükleyebilirsiniz. Daha fazla bilgi için [DFS İsim Alanları genel bakış](https://docs.microsoft.com/windows-server/storage/dfs-namespaces/dfs-overview)bakın.
 
**DFS Çoğaltma (DFS-R)**: DFS-R ve Azure Dosya Eşitlemesi her ikisi de çoğaltma çözümleri olduğundan, çoğu durumda DFS-R'yi Azure Dosya Eşitlemesi ile değiştirmenizi öneririz. Ancak, DFS-R ve Azure Dosya Eşitlemeyi'ni birlikte kullanmak isteyeceğiniz birkaç senaryo vardır:

- DFS-R dağıtımından Azure Dosya Eşitleme dağıtımına geçiş yapıyorsunuz. Daha fazla bilgi için bkz: [Bir DFS Çoğaltma (DFS-R) dağıtımını Azure Dosya Eşitlemesine geçir.](storage-sync-files-deployment-guide.md#migrate-a-dfs-replication-dfs-r-deployment-to-azure-file-sync)
- Dosya verilerinizin bir kopyasına ihtiyaç duyduğu her şirket içi sunucu doğrudan internete bağlanamaz.
- Şube sunucuları verileri Azure Dosya Eşitlemeyi kullanmak istediğiniz tek bir hub sunucusunda birleştirir.

Azure Dosya Eşitlemi ve DFS-R'nin yan yana çalışması için:

1. Azure Dosya Eşitleme bulut katmanlama, DFS-R çoğaltılan klasörlere sahip birimlerde devre dışı bırakılmalı.
2. Sunucu uç noktaları DFS-R salt okunur çoğaltma klasörlerinde yapılandırılmamalıdır.

Daha fazla bilgi için [DFS Çoğaltma genel bakış](https://technet.microsoft.com/library/jj127250)bakın.

### <a name="sysprep"></a>Sysprep
Azure Dosya Eşitleme aracısının yüklü olduğu bir sunucuda sysprep kullanmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Aracı yükleme ve sunucu kaydı sunucu görüntüsünü dağıttıktan ve sysprep mini kurulumu tamamladıktan sonra oluşmalıdır.

### <a name="windows-search"></a>Windows Search
Bir sunucu bitiş noktasında bulut katmanlama etkinleştirilmişse, katmanlı dosyalar atlanır ve Windows Arama tarafından dizine eklenmez. Katmanlı olmayan dosyalar düzgün dizine alınır.

### <a name="other-hierarchical-storage-management-hsm-solutions"></a>Diğer Hiyerarşik Depolama Yönetimi (HSM) çözümleri
Azure Dosya Eşitlemesi ile başka HSM çözümleri kullanılmamalıdır.

## <a name="identity"></a>Kimlik
Azure Dosya Eşitlemi, eşitleme ayarlamanın ötesinde özel bir kurulum olmadan standart AD tabanlı kimliğinizle çalışır. Azure Dosya Eşitlemeyi'ni kullanırken, genel beklenti, erişimlerin çoğunun Azure dosya paylaşımı yerine Azure Dosya Eşitleme önbelleğe alma sunucularından geçmesidir. Sunucu bitiş noktaları Windows Server'da bulunduğundan ve Windows Server AD ve Windows tarzı ALA'ları çok uzun bir süre desteklediğinden, Depolama Eşitleme Hizmetine kayıtlı Windows dosya sunucularının etki alanına katılmasını sağlamak tan başka bir şeye gerek yoktur. Azure Dosya Eşitlemi, Azure dosya paylaşımındaki dosyalarda ALA'ları depolar ve bunları tüm sunucu bitiş noktalarına kopyalar.

Azure dosya paylaşımında doğrudan yapılan değişikliklerin eşitleme grubundaki sunucu uç noktalarıyla eşitlemesi daha uzun sürse de, REKLAM izinlerinizi doğrudan bulutta da dosya paylaşımınızda uygulayabileceğinizden emin olmak isteyebilirsiniz. Bunu yapmak için, windows dosya sunucularınızın etki alanının nasıl birleştiği gibi, depolama hesabınıza şirket içi AD'nize katılmanız gerekir. Alan adının depolama alanı hesabınıza müşteriye ait bir Active Directory'ye katılması hakkında daha fazla bilgi edinmek için [Azure Dosyaları Etkin Dizini'ne genel bakış](storage-files-active-directory-overview.md)'a bakın.

> [!Important]  
> Azure Dosya Eşitlemesi'ni başarıyla dağıtmak için depolama alanı hesabınızı Etkin Dizin'e katılmak gerekmez. Bu, kullanıcılar Azure dosya paylaşımını doğrudan monte ettiğinde Azure dosya paylaşımının şirket içi ALA'ları uygulamasına olanak tanıyan kesinlikle isteğe bağlı bir adımdır.

## <a name="networking"></a>Ağ Oluşturma
Azure Dosya Eşitleme aracısı, Azure Dosya Eşitleme REST protokolü ve FileREST protokolü kullanılarak Depolama Eşitleme Hizmetiniz ve Azure dosya paylaşımınızla iletişim kurar ve her ikisi de her ikisi de 443 bağlantı noktası üzerinden HTTPS kullanır. SMB, Windows Server'ınız ile Azure dosya paylaşımı nız arasında veri yüklemek veya indirmek için asla kullanılmaz. Çoğu kuruluş, çoğu web sitesini ziyaret etmek için bir gereklilik olarak bağlantı noktası 443 üzerinden HTTPS trafiğine izin verdiğinden, Azure Dosya Eşitlemesi dağıtmak için genellikle özel ağ yapılandırması gerekmez.

Kuruluşunuzun ilke veya benzersiz düzenleyici gereksinimlerine bağlı olarak, Azure ile daha kısıtlayıcı iletişim elabilirsiniz ve bu nedenle Azure Dosya Eşitlemesi ağ yapılandırmanız için çeşitli mekanizmalar sağlar. Gereksinimlerinize bağlı olarak şunları yapabilirsiniz:

- ExpressRoute veya Azure VPN'iniz üzerinden tünel eşitleme ve dosya yükleme/indirme trafiği. 
- Azure Dosyaları ve Azure Ağ özelliklerinden yararlanın, hizmet bitiş noktaları ve özel uç noktalar gibi.
- Ortamınızda proxy'nizi destekleyecek şekilde Azure Dosya Eşitlemesini yapılandırın.
- Azure Dosya Eşitle'den ağ etkinliğini daralt.

Azure Dosya Eşitleme'nin ağ işlevselliğini yapılandırma hakkında daha fazla bilgi edinmek için bkz:
- [Azure Dosya Eşitleme proxy’si ve güvenli duvarı ayarları](storage-sync-files-firewall-and-proxy.md)
- [Azure Dosya Eşitlemesini veri merkezinizde iyi bir komşu olduğundan emin olun](storage-sync-files-server-registration.md)

## <a name="encryption"></a>Şifreleme
Azure Dosya Eşitlemeyi kullanırken göz önünde bulundurulması gereken üç farklı şifreleme katmanı vardır: Windows Server'ın istirahat sırasında depolamasında şifreleme, Azure Dosya Eşitleme aracısı ile Azure arasında aktarımda şifreleme ve Azure dosya paylaşımındaki verilerinizin geri kalanında şifreleme. 

### <a name="windows-server-encryption-at-rest"></a>Windows Server şifrelemesi hazır 
Windows Server'daki verileri şifrelemek için genellikle Azure Dosya Eşitlemi ile çalışan iki strateji vardır: dosya sisteminin ve ona yazılan tüm verilerin şifrelenmiş olması ve dosya biçiminin kendisi içinde şifreleme gibi dosya sisteminin altında şifreleme. Bu yöntemler birbirini dışlayan değildir; şifreleme amacı farklı olduğundan istenirse birlikte kullanılabilirler.

Dosya sisteminin altında şifreleme sağlamak için Windows Server BitLocker gelen kutusu sağlar. BitLocker, Azure Dosya Eşitlemi için tamamen saydamdır. BitLocker gibi bir şifreleme mekanizmasıkullanmanın birincil nedeni, diskleri çalan biri tarafından şirket içi veri merkezinizden gelen verilerin fiziksel olarak sızmasını önlemek ve verilerinize yetkisiz okuma/yazma gerçekleştirmek için yetkisiz bir işletim sistemi nin yan yüklenmesinin engellenmesidir. BitLocker hakkında daha fazla bilgi edinmek için [BitLocker'a genel bakış](https://docs.microsoft.com/windows/security/information-protection/bitlocker/bitlocker-overview)bilgisini bulabilirsiniz.

NTFS biriminin altında yer aldıkları için BitLocker'a benzer şekilde çalışan üçüncü taraf ürünlerin de Benzer şekilde Azure Dosya Eşitlemesi ile tamamen saydam olarak çalışması gerekir. 

Verileri şifrelemek için diğer ana yöntem, uygulama dosyayı kaydederken dosyanın veri akışını şifrelemektir. Bazı uygulamalar bunu yerel olarak yapabilir, ancak bu genellikle böyle değildir. Dosyanın veri akışını şifreleme yöntemine örnek olarak Azure Bilgi Koruması (AIP)/Azure Hakları Yönetimi Hizmetleri (Azure RMS)/Active Directory RMS'dir. AIP/RMS gibi bir şifreleme mekanizmasıkullanmanın birincil nedeni, dosya paylaşımınızdan verileri kopyalayan kişilerin, örneğin bir flash sürücüye kopyalamasını veya yetkisiz bir kişiye e-posta yla e-posta yla göndere girmesini önlemektir. Bir dosyanın veri akışı dosya biçiminin bir parçası olarak şifrelendiğinde, bu dosya Azure dosya paylaşımında şifrelenmeye devam eder. 

Azure Dosya Eşitlemesi, dosya sisteminin üzerinde ancak dosyanın veri akışının altında yer alan NTFS Şifreli Dosya Sistemi (NTFS EFS) veya üçüncü taraf şifreleme çözümleriyle birlikte çalışmaz. 

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme
Azure Dosya Eşitleme aracısı, Azure Dosya Eşitleme REST protokolü ve FileREST protokolü kullanılarak Depolama Eşitleme Hizmetiniz ve Azure dosya paylaşımınızla iletişim kurar ve her ikisi de her ikisi de 443 bağlantı noktası üzerinden HTTPS kullanır. Azure Dosya Eşitlemesi HTTP üzerinden şifrelenmemiş istekler göndermez. 

Azure depolama hesapları, aktarım sırasında şifreleme gerektiren bir anahtar içerir ve bu anahtar varsayılan olarak etkinleştirilir. Depolama hesabı düzeyindeki anahtar devre dışı bırakılmış olsa bile, azure dosya paylaşımlarınıza şifrelenmemiş bağlantılar mümkündür, Azure Dosya Eşitlemi yine de yalnızca dosya paylaşımınıza erişmek için şifreli kanallar kullanır.

Depolama hesabı için aktarımda şifrelemeyi devre dışı bilmelidir, windows server 2008 R2 veya daha eski bir Linux dağıtımı gibi eski bir işletim sisteminde çalıştırılması gereken eski bir uygulamayı desteklemek, doğrudan bir Azure dosya paylaşımıyla konuşmaktır. Eski uygulama dosya paylaşımının Windows Server önbelleğine çıkarsa, bu ayarı değiştirin hiçbir etkisi olmaz. 

Aktarım sırasında ki verilerin şifrelemesinin etkin olmasını şiddetle öneririz.

Aktarım sırasında şifreleme hakkında daha fazla bilgi için Azure [depolama alanında güvenli aktarım gerektirmeye](../common/storage-require-secure-transfer.md?toc=%2fazure%2fstorage%2ffiles%2ftoc.json)bakın.

### <a name="azure-file-share-encryption-at-rest"></a>Azure dosya paylaşımı şifrelemesi
[!INCLUDE [storage-files-encryption-at-rest](../../../includes/storage-files-encryption-at-rest.md)]

## <a name="storage-tiers"></a>Depolama katmanları
[!INCLUDE [storage-files-tiers-overview](../../../includes/storage-files-tiers-overview.md)]

### <a name="enable-standard-file-shares-to-span-up-to-100-tib"></a>Standart dosya paylaşımlarını 100 TiB'e kadar yayılmaya olanak sağlama
[!INCLUDE [storage-files-tiers-enable-large-shares](../../../includes/storage-files-tiers-enable-large-shares.md)]

#### <a name="regional-availability"></a>Bölgesel kullanılabilirlik
[!INCLUDE [storage-files-tiers-large-file-share-availability](../../../includes/storage-files-tiers-large-file-share-availability.md)]

## <a name="azure-file-sync-region-availability"></a>Azure dosya eşitleme bölgesi kullanılabilirliği
Azure Dosya Eşitlemi aşağıdaki bölgelerde kullanılabilir:

| Azure bulut | Coğrafi bölge | Azure bölgesi | Bölge kodu |
|-------------|-------------------|--------------|-------------|
| Genel | Asya | Doğu Asya | `eastasia` |
| Genel | Asya | Güneydoğu Asya | `southeastasia` |
| Genel | Avustralya | Doğu Avustralya | `australiaeast` |
| Genel | Avustralya | Güneydoğu Avustralya | `australiasoutheast` |
| Genel | Brezilya | Güney Brezilya | `brazilsouth` |
| Genel | Kanada | Orta Kanada | `canadacentral` |
| Genel | Kanada | Doğu Kanada | `canadaeast` |
| Genel | Avrupa | Kuzey Avrupa | `northeurope` |
| Genel | Avrupa | Batı Avrupa | `westeurope` |
| Genel | Fransa | Orta Fransa | `francecentral` |
| Genel | Fransa | Fransa Güney* | `francesouth` |
| Genel | Hindistan | Orta Hindistan | `centralindia` |
| Genel | Hindistan | Güney Hindistan | `southindia` |
| Genel | Japonya | Doğu Japonya | `japaneast` |
| Genel | Japonya | Batı Japonya | `japanwest` |
| Genel | Güney Kore | Güney Kore - Orta | `koreacentral` |
| Genel | Güney Kore | Güney Kore - Güney | `koreasouth` |
| Genel | Güney Afrika | Güney Afrika Kuzey | `southafricanorth` |
| Genel | Güney Afrika | Güney Afrika Batı* | `southafricawest` |
| Genel | BAE | BAE Merkez* | `uaecentral` |
| Genel | BAE | BAE Kuzey | `uaenorth` |
| Genel | Birleşik Krallık | Güney Birleşik Krallık | `uksouth` |
| Genel | Birleşik Krallık | Batı Birleşik Krallık | `ukwest` |
| Genel | ABD | Orta ABD | `centralus` |
| Genel | ABD | Doğu ABD | `eastus` |
| Genel | ABD | Doğu ABD 2 | `eastus2` |
| Genel | ABD | Orta Kuzey ABD | `northcentralus` |
| Genel | ABD | Orta Güney ABD | `southcentralus` |
| Genel | ABD | Orta Batı ABD | `westcentralus` |
| Genel | ABD | Batı ABD | `westus` |
| Genel | ABD | Batı ABD 2 | `westus2` |
| US Gov | ABD | US Gov Arizona | `usgovarizona` |
| US Gov | ABD | US Gov Texas | `usgovtexas` |
| US Gov | ABD | US Gov Virginia | `usgovvirginia` |

Azure Dosya Eşitlemeyi yalnızca Depolama Eşitleme Hizmeti ile aynı bölgede bulunan bir Azure dosya paylaşımıyla eşitlemeyi destekler.

Yıldız işaretleriyle işaretlenmiş bölgeler için, bu bölgelerdeki Azure Depolama'ya erişim istemek için Azure Desteği'ne başvurmanız gerekir. İşlem [bu belgede](https://azure.microsoft.com/global-infrastructure/geographies/)özetlenmiştir.

## <a name="redundancy"></a>Yedeklilik
[!INCLUDE [storage-files-redundancy-overview](../../../includes/storage-files-redundancy-overview.md)]

> [!Important]  
> Coğrafi yedekli ve Coğrafi bölge yedekli depolama, ikincil bölgeye depolamayı el ile devretme özelliğine sahiptir. Veri kaybı olasılığı nedeniyle Azure Dosya Eşitlemesi'ni kullanırken bunu bir felaket dışında yapmamanızı öneririz. Depolama alanının el ile başarısız olmasını istediğiniz bir felaket durumunda, Azure Dosya Eşitlemesi'nin ikincil bitiş noktasıyla eşitlenmeye devam etmesini sağlamak için Microsoft ile bir destek durumu açmanız gerekir.

## <a name="migration"></a>Geçiş
Varolan bir Windows dosya sunucunuz varsa, verileri yeni bir sunucuya taşımaya gerek kalmadan Azure Dosya Eşitlemi doğrudan yüklenebilir. Azure Dosya Eşitlemesi'ni benimsemenin bir parçası olarak yeni bir Windows dosya sunucusuna geçmeyi planlıyorsanız, verileri taşımak için birkaç olası yaklaşım vardır:

- Eski dosya paylaşımınız ve yeni dosya paylaşımınız için sunucu uç noktaları oluşturun ve Azure Dosyası Eşitle'nin verileri sunucu bitiş noktaları arasında eşitlemasına izin verin. Bu yaklaşımın avantajı, Azure Dosya Eşitlemi bulut katmanlama farkında olduğundan, yeni dosya sunucunuzdaki depolama alanına aşırı abone olmayı çok kolay hale getirerek bu yaklaşımın avantajıdır. Hazır olduğunuzda, son kullanıcıları yeni sunucudaki dosya paylaşımına bölebilir ve eski dosya paylaşımının sunucu bitiş noktasını kaldırabilirsiniz.

- Yalnızca yeni dosya sunucusunda bir sunucu bitiş noktası oluşturun ve verileri `robocopy`kullanarak eski dosya paylaşımından kopyalayın. Yeni sunucunuzdaki dosya paylaşımlarının topolojisine (her birimde kaç paylaşım olduğunu, her birimde kaç paylaşım ınız olduğu, her birimin `robocopy` ne kadar özgür olduğu vb.) bağlı olarak, eski sunucunuzdan şirket içi veri merkezinizdeki yeni sunucunuza kadar Azure Dosya Eşitlemi'nin verileri Azure'a taşımasından daha hızlı tamamlanması beklendiği için geçici olarak ek depolama sağlamanız gerekebilir.

Verileri Azure Dosya Eşitleme dağıtımına geçirmek için Veri Kutusu'yu kullanmak da mümkündür. Çoğu zaman, müşteriler veri almak için Veri Kutusu'nu kullanmak istediklerinde, bunu dağıtım larının hızını artıracağını düşündükleri veya kısıtlı bant genişliği senaryolarına yardımcı olacağını düşündükleri için yaparlar. Azure Dosya Eşitleme dağıtımınıza veri almak için veri kutusu kullanmanın bant genişliği kullanımını azaltacağı doğru olsa da, çoğu senaryonun yukarıda açıklanan yöntemlerden biri aracılığıyla çevrimiçi veri yüklemesini sürdürmesi büyük olasılıkla daha hızlı olacaktır. Azure Dosya Eşitleme dağıtımınıza veri almak için Veri Kutusu'nun nasıl kullanılacağı hakkında daha fazla bilgi edinmek için [bkz.](storage-sync-offline-data-transfer.md)

Müşterilerin verileri yeni Azure Dosya Eşitleme dağıtımlarına aktarırken yaptığı yaygın bir hata, verileri Windows dosya sunucularında değil, doğrudan Azure dosya paylaşımına kopyalamaktır. Azure Dosya Eşitlemi, Azure dosya paylaşımındaki tüm yeni dosyaları tanımlayıp Windows dosya paylaşımlarınıza eşitlese de, bu genellikle Windows dosya sunucusu üzerinden veri yüklemekten çok daha yavaştır. AzCopy gibi birçok Azure kopyalama aracı, zaman damgaları ve ALA'lar gibi bir dosyanın önemli meta verilerinin tümünün kopyalanmaması gibi ek bir dezavantajına sahiptir.

## <a name="antivirus"></a>Virüsten Koruma
Virüsten koruma, bilinen kötü amaçlı kod için dosyaları tarayarak çalıştığından, virüsten koruma ürünü katmanlı dosyaların geri çağrılmasına neden olabilir. Azure Dosya Eşitleme aracısının 4.0 ve üzerindeki sürümlerinde, katmanlı dosyalar güvenli Windows özniteliğine sahiptir FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS kümesine sahiptir. Bu öznitelik kümesiyle dosyaları atlamak için çözümlerini nasıl yapılandıracaklarını öğrenmek için yazılım satıcınıza danışmanızı öneririz (çoğu bunu otomatik olarak yapar). 

Microsoft'un şirket içi virüsten koruma çözümleri, Windows Defender ve System Center Endpoint Protection (SCEP), her ikisi de otomatik olarak bu öznitelik ayarlanmış dosyaları okuma atlamak. Bunları test ettik ve küçük bir sorun belirledik: Varolan bir eşitleme grubuna sunucu eklediğinizde, 800 bayttan küçük dosyalar yeni sunucuda geri çağrılır (karşıdan yüklenir). Bu dosyalar yeni sunucuda kalır ve katmanlama boyutu gereksinimini (>64kb) karşılamadıkları için katmanlandırılamaz.

> [!Note]  
> Virüsten koruma satıcıları, Microsoft İndirme Merkezi'nden indirilebilen [Azure Dosya Eşitleme Virüsten Koruma Uyumluluk Test Paketi'ni](https://www.microsoft.com/download/details.aspx?id=58322)kullanarak ürünleri yle Azure Dosya Eşitlemi arasındaki uyumluluğu denetleyebilir.

## <a name="backup"></a>Backup 
Virüsten koruma çözümleri gibi, yedekleme çözümleri katmanlı dosyaların geri çağrılması na neden olabilir. Şirket içi yedekleme ürünü yerine Azure dosya paylaşımını yedeklemek için bulut yedekleme çözümü kullanmanızı öneririz.

Şirket içi yedekleme çözümü kullanıyorsanız, yedeklemeler bulut katmanlama devre dışı bırakılmış eşitleme grubundaki bir sunucuda gerçekleştirilmelidir. Geri yükleme yaparken, birim düzeyi veya dosya düzeyi geri yükleme seçeneklerini kullanın. Dosya düzeyi geri yükleme seçeneği kullanılarak geri yüklenen dosyalar eşitleme grubundaki tüm uç noktalara eşitlenir ve varolan dosyalar yedekten geri yüklenen sürümle değiştirilir.  Ses düzeyi geri yüklemeleri, Azure dosya paylaşımındaki veya diğer sunucu uç noktalarındaki yeni dosya sürümlerinin yerini alamaz.

> [!Note]  
> Bare-metal (BMR) geri yüklemesi beklenmeyen sonuçlara neden olabilir ve şu anda desteklenmez.

> [!Note]  
> Azure Dosya Eşitleme aracısının Sürüm 9'u ile VSS anlık görüntüleri (Önceki Sürümler sekmesi dahil) artık bulut katmanlama etkinleştirilmiş birimlerde desteklenir. Ancak, PowerShell aracılığıyla önceki sürüm uyumluluğunu etkinleştirmeniz gerekir. [Nasıl olduğunu öğrenin](storage-files-deployment-guide.md).

## <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="next-steps"></a>Sonraki adımlar
* [Güvenlik duvarı ve proxy ayarlarını göz önünde bulundurun](storage-sync-files-firewall-and-proxy.md)
* [Azure Dosyaları dağıtımı planlama](storage-files-planning.md)
* [Azure Dosyaları’nı dağıtma](storage-files-deployment-guide.md)
* [Azure Dosya Eşitleme’yi dağıtma](storage-sync-files-deployment-guide.md)
* [Azure Dosya Eşitleme’yi izleme](storage-sync-files-monitoring.md)