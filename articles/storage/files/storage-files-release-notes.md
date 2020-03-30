---
title: Azure Dosya Eşitleme aracısı için sürüm notları | Microsoft Dokümanlar
description: Azure Dosya Eşitleme aracısı için notlar bırakın.
services: storage
author: wmgries
ms.service: storage
ms.topic: conceptual
ms.date: 3/16/2020
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 5b247e8f4a611ace227f5adcef7e0e27de341c96
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454644"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Azure Dosya Eşitleme aracısı sürüm notları
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Windows Server yüklemeleriniz, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürülür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları verilmektedir.

## <a name="supported-versions"></a>Desteklenen sürümler
Azure Dosya Eşitleme aracısı aşağıdaki sürümleri destekler:

| Kilometre taşı | Aracı sürüm numarası | Sürüm tarihi | Durum |
|----|----------------------|--------------|------------------|
| Aralık 2019 güncelleme toplama - [KB4522360](https://support.microsoft.com/help/4522360)| 9.1.0.0 | 12 Aralık 2019 | Destekleniyor |
| V9 Yayın - [KB4522359](https://support.microsoft.com/help/4522359)| 9.0.0.0 | 2 Aralık 2019 | Destekleniyor |
| V8 Yayın - [KB4511224](https://support.microsoft.com/help/4511224)| 8.0.0.0 | 8 Ekim 2019 | Destekleniyor |
| Temmuz 2019 güncelleme toplama - [KB4490497](https://support.microsoft.com/help/4490497)| 7.2.0.0 | 24 Temmuz 2019 | Destekleniyor |
| Temmuz 2019 güncelleme toplama - [KB4490496](https://support.microsoft.com/help/4490496)| 7.1.0.0 | 12 Temmuz 2019 | Destekleniyor |
| V7 Yayın - [KB4490495](https://support.microsoft.com/help/4490495)| 7.0.0.0 | Haziran 19, 2019 | Destekleniyor |
| Haziran 2019 güncelleme toplama - [KB4489739](https://support.microsoft.com/help/4489739)| 6.3.0.0 | Haziran 27, 2019 | Desteklenen - Aracı sürümü 21 Nisan 2020 tarihinde sona erecek |
| Haziran 2019 güncelleme toplama - [KB4489738](https://support.microsoft.com/help/4489738)| 6.2.0.0 | Haziran 13, 2019 | Desteklenen - Aracı sürümü 21 Nisan 2020 tarihinde sona erecek |
| Mayıs 2019 güncelleme toplama - [KB4489737](https://support.microsoft.com/help/4489737)| 6.1.0.0 | Mayıs 7, 2019 | Desteklenen - Aracı sürümü 21 Nisan 2020 tarihinde sona erecek |
| V6 Yayın - [KB4489736](https://support.microsoft.com/help/4489736)| 6.0.0.0 | 21 Nisan 2019 | Desteklenen - Aracı sürümü 21 Nisan 2020 tarihinde sona erecek |
| Nisan 2019 güncelleme toplama - [KB4481061](https://support.microsoft.com/help/4481061)| 5.2.0.0 | 4 Nisan 2019 | Desteklenen - Agent sürümü 18 Mart 2020 tarihinde sona erecek |
| Mart 2019 güncelleme toplama - [KB4481060](https://support.microsoft.com/help/4481060)| 5.1.0.0 | 7 Mart 2019 | Desteklenen - Agent sürümü 18 Mart 2020 tarihinde sona erecek |
| V5 Yayın - [KB4459989](https://support.microsoft.com/help/4459989)| 5.0.2.0 | 12 Şubat 2019 | Desteklenen - Agent sürümü 18 Mart 2020 tarihinde sona erecek |
| V4 Yayın | 4.0.1.0 - 4.3.0.0 | Yok | Desteklenmiyor - Aracı sürümlerinin süresi 6 Kasım 2019'da doldu |
| V3 Yayın | 3.1.0.0 - 3.4.0.0 | Yok | Desteklenmiyor - Aracı sürümlerinin süresi 19 Ağustos 2019'da sona erdi |
| Ga öncesi ajanlar | 1.1.0.0 - 3.0.13.0 | Yok | Desteklenmiyor - Aracı sürümleri1 Ekim 2018 tarihinde sona erdi |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-9100"></a>Aracı sürümü 9.1.0.0
Aşağıdaki sürüm notları, 12 Aralık 2019'da yayımlanan Azure Dosya Eşitleme aracısının 9.1.0.0 sürümü için hazırdır. Bu notlar sürüm 9.0.0.0 için listelenen sürüm notlarına ek olarak verilir.

Sorun bu sürümde sabit:  
- Eşitleme, Azure Dosya Eşitleme aracısı sürüm 9.0'a yükselttikten sonra aşağıdaki hatalardan biriyle başarısız olur:
    - 0x8e5e044e (JET_errWriteConflict)
    - 0x8e5e0450 (JET_errInvalidSesid)
    - 0x8e5e0442 (JET_errInstanceUnavailable)

## <a name="agent-version-9000"></a>Aracı sürümü 9.0.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 9.0.0.0 sürümü içindir (2 Aralık 2019'da yayımlanır).

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Self servis geri yükleme desteği
    - Kullanıcılar artık önceki sürüm özelliğini kullanarak dosyalarını geri yükleyebilirler. v9 sürümünden önce, önceki sürüm özelliği bulut katmanlama özelliği etkin olan birimlerde desteklenmedi. Bu özellik, bulut katmanlama etkin bir bitiş noktasının bulunduğu her birim için ayrı ayrı etkinleştirilmelidir. Daha fazla bilgi için bkz.  
[Önceki Sürümler ve VSS (Cilt Gölge Kopyalama Hizmeti) ile self servis geri yükleme.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#self-service-restore-through-previous-versions-and-vss-volume-shadow-copy-service) 
 
- Daha büyük dosya paylaşımı boyutları için destek 
    - Azure Dosya Eşitlemi artık tek bir ad alanında 64TiB ve 100 milyona kadar dosyayı destekler.  
 
- Server 2019'da Veri Çoğaltma desteği 
    - Veri Çoğaltma artık Windows Server 2019'da etkin leştirilmiş bulut katmanlama ile desteklendi. Bulut katmanlama lı birimlerde Veri Çoğaltma'yı desteklemek için Windows [güncelleştirmek KB4520062'nin](https://support.microsoft.com/help/4520062) yüklenmesi gerekir. 
 
- Bir dosyanın katmana yükseltilmesi için geliştirilmiş minimum dosya boyutu 
    - Bir dosyanın katmana en küçük dosya boyutu artık dosya sistemi küme boyutunu temel alıyor (dosya sistemi küme boyutunu iki katına çıkar). Örneğin, varsayılan olarak, NTFS dosya sistemi küme boyutu 4KB, bir dosya için ortaya çıkan minimum dosya boyutu 8KB olduğunu. 
 
- Ağ bağlantısı testi cmdlet 
    - Azure Dosya Eşitleme yapılandırmasının bir parçası olarak, birden çok hizmet uç noktasıyla bağlantı yapılmalıdır. Her birinin sunucu için erişilebilir olması gereken kendi DNS adı vardır. Bu URL'ler, sunucunun kayıtlı olduğu bölgeye de özeldir. Bir sunucu kaydedildikten sonra, bu sunucuya özgü tüm URL'lerle iletişimi test etmek için bağlantı testi cmdlet (PowerShell ve Server Registration Utility) kullanılabilir. Bu cmdlet, eksik iletişim sunucunun Azure Dosya Eşitlemesi ile tam olarak çalışmasını engellediğinde sorun gidermeye yardımcı olabilir ve proxy ve güvenlik duvarı yapılandırmalarında ince ayar yapmak için kullanılabilir.  
 
        Ağ bağlantısı testini çalıştırmak için aşağıdaki PowerShell komutlarını çalıştırın: 
 
        Alma Modülü "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"  
        Test-DepolamaSyncNetworkConnectivity
 
- Bulut katmanlama etkinleştirildiğinde sunucu uç noktası geliştirmeyi kaldırma 
    - Daha önce olduğu gibi, bir sunucu bitiş noktasının kaldırılması Azure dosya paylaşımındaki dosyaların kaldırılmasıyla sonuçlanmaz. Ancak, yerel sunucuda telafi noktaları için davranış değişti. Bir sunucu bitiş noktası kaldırıldığında düzeltme noktaları (sunucuda yerel olmayan dosyalara işaretçiler) şimdi silinir. Tam önbelleğe alınan dosyalar sunucuda kalır. Bu geliştirme, bir sunucu bitiş noktasını kaldırırken [alt katmanlı dosyaları](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint) önlemek için yapıldı. Sunucu bitiş noktası yeniden oluşturulursa, katmanlı dosyaların düzeltme noktaları sunucuda yeniden oluşturulur.  
 
- Performans ve güvenilirlik iyileştirmeleri 
    - Azaltılmış geri çağırma hataları. Geri çağırma boyutu artık ağ bant genişliğine göre otomatik olarak ayarlanır. 
    - Eşitleme grubuna yeni bir sunucu eklerken daha iyi indirme performansı. 
    - Kısıtlama çakışmaları nedeniyle eşitlenen dosyaların küçültülmesi. 
    - Sunucu bitiş noktası yolu bir birim montaj noktası ise dosyalar katmanbaşarısız veya beklenmedik bir şekilde belirli senaryolarda geri çağrılır.
    
### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure Dosya Eşitlemesi'ni dağıtmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmeniz gerekir. Bu araç, dosya sisteminiz ve veri kümenizle ilgili desteklenmeyen karakterler veya desteklenmeyen işletim sistemi sürümü gibi olası sorunları kontrol eden bir Azure PowerShell cmdlet'tir. Yükleme ve kullanım talimatları için planlama kılavuzundaki [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server ile nasıl yükleyip yapılandırılabilenler hakkında daha fazla bilgi için Azure [Dosya Eşitleme dağıtımı için planlama](storage-sync-files-planning.md) ve Azure Dosya [Eşitleme dağıtımı nın nasıl dağıtılanakadar dağıtılabildiğini](storage-sync-files-deployment-guide.md)öğrenin.

- Aracı yükleme paketi yükseltilmiş (yönetici) izinlerle yüklenmelidir.
- Aracı Nano Server dağıtım seçeneğinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2'de desteklenir.
- Aracı en az 2 GiB bellek gerektirir. Sunucu dinamik bellek etkin leştirilmiş sanal bir makinede çalışıyorsa, VM en az 2048 MiB bellekle yapılandırılmalıdır.
- Depolama Eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birim bilgileri (SVI) dizininin sıkıştırılmış olduğu bir birimüzerinde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara yol açar.

### <a name="interoperability"></a>Birlikte Çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için Bkz. [Sorun Giderme Azure Dosya Eşitlemi.](storage-sync-files-troubleshoot.md)
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları, dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Azure Dosya Eşitleme aracısını yüklü olan bir sunucuda sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Azure Dosya Eşitleme aracısı, sunucu görüntüsünü dağıttıktan ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için [Sorun Giderme kılavuzuna](storage-sync-files-troubleshoot.md#handling-unsupported-characters) bakın.
- Bir dönemle biten dosyalar veya dizinler.
- 2.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu bitiş noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosya, sunucu bitiş noktasını silmeden önce geri çağrılmazsa katmanlı dosyalara erişilemez hale gelir. Dosyalara erişimi geri yüklemek için sunucu bitiş noktasını yeniden oluşturun. Sunucu bitiş noktasının silinmesinin üzerinden 30 gün geçtiyse veya bulut bitiş noktası silinmişse, geri çağrılmayan katmanlı dosyalar kullanılamaz hale gelir. Daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint)
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama sayfalama dosyalarını sunucu bitiş noktası konumu içinde saklamayın.
- Sunucu nun adı yeniden adlandırıldıysa, portaldaki sunucu adı güncelleştirilmeyecektir.

### <a name="cloud-endpoint"></a>Bulut bitiş noktası
- Azure Dosya Eşitlemi, Azure dosya paylaşımında doğrudan değişiklik yapmayı destekler. Ancak, Azure dosya paylaşımında yapılan değişikliklerin öncelikle bir Azure Dosya Eşitlemi algılama işi tarafından keşfedilmesi gerekir. Bir bulut bitiş noktası için her 24 saatte bir değişiklik algılama işi başlatılır. Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) PowerShell cmdlet, Azure dosya paylaşımındaki değişikliklerin algılanmasını el ile başlatmak için kullanılabilir. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB'nin son değiştirilen saatini güncelleştirmez ve eşitleme yle yapılan bir değişiklik olarak görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, varolan Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneye taşınabilir. Depolama hesabı taşınırsa, Karma Dosya Eşitleme Hizmeti'ne depolama hesabına erişim sağlamanız gerekir (bkz. [Azure Dosya Eşitlemesi'nin depolama hesabına erişmesini sağlayın).](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)

    > [!Note]  
    > Azure Dosya Eşitlemesi, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken, dosya zaman damgalarını korumak için /MIR seçeneğini kullanın. Bu, eski dosyaların yakın zamanda erişilen dosyalardan daha kısa bir sürede katmanlandırılmasını sağlar.
- Pagefile.sys bulut katmanlama etkin leştirilmiş bir birimde bulunuyorsa dosyalar katmanlamada başarısız olabilir. Pagefile.sys, bulut katmanlama devre dışı bırakılmış bir birimde bulunmalıdır.

## <a name="agent-version-8000"></a>Aracı sürümü 8.0.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 8.0.0.0 sürümü içindir (8 Ekim 2019'da yayımlanır).

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Performans Geliştirmelerini Geri Yükleme
    - Azure Yedekleme aracılığıyla yapılan kurtarma için daha hızlı kurtarma süreleri. Geri yüklenen dosyalar Azure Dosya Eşitleme sunucularına çok daha hızlı eşitlenir. 
- Geliştirilmiş bulut katmanlama portalı deneyimi  
    - Geri çağırmayı başaramayan katmanlı dosyalarınız varsa, artık sunucu bitiş noktası özelliklerindeki geri çağırma hatalarını görüntüleyebilirsiniz. Ayrıca, bulut katmanlama filtresi sürücüsü sunucuya yüklenmezse, sunucu uç noktası durumu artık bir hata ve azaltma adımları gösterir.
- Daha basit aracı yükleme
    - Az\AzureRM PowerShell modülü artık sunucunun yüklemeyi daha basit ve hızlı hale getirdiğini kaydetmek için gerekli değildir.
- Çeşitli performans ve güvenilirlik iyileştirmeleri

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure Dosya Eşitlemesi'ni dağıtmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmeniz gerekir. Bu araç, dosya sisteminiz ve veri kümenizle ilgili desteklenmeyen karakterler veya desteklenmeyen işletim sistemi sürümü gibi olası sorunları kontrol eden bir Azure PowerShell cmdlet'tir. Yükleme ve kullanım talimatları için planlama kılavuzundaki [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server ile nasıl yükleyip yapılandırılabilenler hakkında daha fazla bilgi için Azure [Dosya Eşitleme dağıtımı için planlama](storage-sync-files-planning.md) ve Azure Dosya [Eşitleme dağıtımı nın nasıl dağıtılanakadar dağıtılabildiğini](storage-sync-files-deployment-guide.md)öğrenin.

- Aracı yükleme paketi yükseltilmiş (yönetici) izinlerle yüklenmelidir.
- Aracı Nano Server dağıtım seçeneğinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2'de desteklenir.
- Aracı en az 2 GiB bellek gerektirir. Sunucu dinamik bellek etkin leştirilmiş sanal bir makinede çalışıyorsa, VM en az 2048 MiB bellekle yapılandırılmalıdır.
- Depolama Eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birim bilgileri (SVI) dizininin sıkıştırılmış olduğu bir birimüzerinde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara yol açar.

### <a name="interoperability"></a>Birlikte Çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için Bkz. [Sorun Giderme Azure Dosya Eşitlemi.](storage-sync-files-troubleshoot.md)
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları, dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Azure Dosya Eşitleme aracısını yüklü olan bir sunucuda sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Azure Dosya Eşitleme aracısı, sunucu görüntüsünü dağıttıktan ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için [Sorun Giderme kılavuzuna](storage-sync-files-troubleshoot.md#handling-unsupported-characters) bakın.
- Bir dönemle biten dosyalar veya dizinler.
- 2.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu bitiş noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosya, sunucu bitiş noktasını silmeden önce geri çağrılmazsa katmanlı dosyalara erişilemez hale gelir. Dosyalara erişimi geri yüklemek için sunucu bitiş noktasını yeniden oluşturun. Sunucu bitiş noktasının silinmesinin üzerinden 30 gün geçtiyse veya bulut bitiş noktası silinmişse, geri çağrılmayan katmanlı dosyalar kullanılamaz hale gelir. Daha fazla bilgi edinmek için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint)
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama sayfalama dosyalarını sunucu bitiş noktası konumu içinde saklamayın.
- Sunucu nun adı yeniden adlandırıldıysa, portaldaki sunucu adı güncelleştirilmeyecektir.

### <a name="cloud-endpoint"></a>Bulut bitiş noktası
- Azure Dosya Eşitlemi, Azure dosya paylaşımında doğrudan değişiklik yapmayı destekler. Ancak, Azure dosya paylaşımında yapılan değişikliklerin öncelikle bir Azure Dosya Eşitlemi algılama işi tarafından keşfedilmesi gerekir. Bir bulut bitiş noktası için her 24 saatte bir değişiklik algılama işi başlatılır. Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) PowerShell cmdlet, Azure dosya paylaşımındaki değişikliklerin algılanmasını el ile başlatmak için kullanılabilir. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB'nin son değiştirilen saatini güncelleştirmez ve eşitleme yle yapılan bir değişiklik olarak görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, varolan Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneye taşınabilir. Depolama hesabı taşınırsa, Karma Dosya Eşitleme Hizmeti'ne depolama hesabına erişim sağlamanız gerekir (bkz. [Azure Dosya Eşitlemesi'nin depolama hesabına erişmesini sağlayın).](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)

    > [!Note]  
    > Azure Dosya Eşitlemesi, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken, dosya zaman damgalarını korumak için /MIR seçeneğini kullanın. Bu, eski dosyaların yakın zamanda erişilen dosyalardan daha kısa bir sürede katmanlandırılmasını sağlar.

## <a name="agent-version-7200"></a>Aracı sürümü 7.2.0.0
Aşağıdaki sürüm notları, 24 Temmuz 2019'da yayımlanan Azure Dosya Eşitleme aracısının 7.2.0.0 sürümü için hazırdır. Bu notlar sürüm 7.0.0.0 için listelenen sürüm notlarına ek olarak verilir.

Bu sürümde düzeltilen sorunların listesi:  
- Proxy yapılandırması null ise Depolama Eşitleme Aracısı (FileSyncSvc) çöker.
- Sunucudaki birden çok uç nokta aynı ada sahipse sunucu bitiş noktası BCDR (hata 0x80c80257 - ECS_E_BCDR_IN_PROGRESS) başlatacaktır.
- Bulut katmanlama güvenilirlik iyileştirmeleri.

## <a name="agent-version-7100"></a>Aracı sürümü 7.1.0.0
Aşağıdaki sürüm notları, 12 Temmuz 2019'da yayımlanan Azure Dosya Eşitleme aracısının 7.1.0.0 sürümü için hazırdır. Bu notlar sürüm 7.0.0.0 için listelenen sürüm notlarına ek olarak verilir.

Bu sürümde düzeltilen sorunların listesi:  
- Windows Server 2012 R2'de sunucu bitiş noktası konumuna SMB üzerinden erişmek veya göz atmak yavaştır. 
- Azure Dosya Eşitleme v6 aracısını yükledikten sonra CPU kullanımı artırıldı.
- Bulut katmanlama telemetri iyileştirmeleri.
- Bulut katmanlama ve eşitleme için çeşitli güvenilirlik iyileştirmeleri.

## <a name="agent-version-7000"></a>Aracı sürümü 7.0.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 7.0.0.0 sürümü içindir (19 Haziran 2019'da yayımlanır).

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Daha büyük dosya paylaşımı boyutları için destek
    - Daha büyük Azure dosya paylaşımlarının önizlemesiyle, dosya eşitlemi için destek sınırlarımızı da artırıyoruz. Bu ilk adımda, Azure Dosya Eşitlemi artık ad alanında en fazla 25 TB ve 50 milyon dosyayı destekler. Büyük dosya paylaşımı önizlemesine başvurmak için https://aka.ms/azurefilesatscalesurveybu formu doldurun. 
- Depolama hesaplarında güvenlik duvarı ve sanal ağ ayarı desteği
    - Azure Dosya Eşitlemi artık depolama hesaplarındaki güvenlik duvarını ve sanal ağ ayarını destekler. Dağıtımınızı güvenlik duvarı ve sanal ağ ayarı yla çalışacak şekilde yapılandırmak için [bkz.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide?tabs=azure-portal#configure-firewall-and-virtual-network-settings)
- Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için PowerShell cmdlet
    - Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için Invoke-AzStorageSyncChangeDetection PowerShell cmdlet, Azure dosya paylaşımındaki değişikliklerin algılanmasını el ile başlatmak için kullanılabilir. Bu cmdlet, azure dosya paylaşımında bir tür otomatik işlem yapıldığı veya değişikliklerin bir yönetici tarafından yapıldığı senaryolar için tasarlanmıştır (örneğin, dosya ve dizinleri paylaşıma taşımak gibi). Son kullanıcı değişiklikleri için öneri, Azure Dosya Eşitleme aracısını bir IaaS VM'ye yüklemek ve son kullanıcıların Dosya paylaşımına IaaS VM üzerinden erişmelerini sağlamaktır. Bu şekilde tüm değişiklikler Invoke-AzStorageSyncChangeDetection cmdlet kullanmaya gerek kalmadan diğer aracılar ile hızlı bir şekilde eşitlenecektir. Daha fazla bilgi için [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) belgelerine bakın.
- Eşitleme olmayan dosyalarla karşılaşırsanız geliştirilmiş portal deneyimi
    - Eşitleme de başarısız dosyaları varsa, şimdi portalda geçici ve kalıcı hatalar arasında ayrım. Geçici hatalar genellikle yönetici eylemi gerek kalmadan kendilerini çözmek. Örneğin, şu anda kullanılmakta olan bir dosya işlamacı kapatılana kadar eşitleme yapmaz. Kalıcı hatalar için artık her hatadan etkilenen dosya sayısını gösteriyoruz. Kalıcı hata sayısı, eşitleme grubundaki tüm sunucu uç noktalarının eşitleme sütunundaki dosyalarda da görüntülenir.
- Geliştirilmiş Azure Yedekleme dosya düzeyi geri yüklemesi
    - Azure Yedekleme kullanılarak geri yüklenen tek tek dosyalar artık algılanır ve sunucu bitiş noktasına daha hızlı eşitlenir.
- Geliştirilmiş bulut katmanlama geri çağırma cmdlet güvenilirliği 
    - Invoke-StorageSyncFileRecall cmdlet artık müşterilerin dosya yeniden deneme sayısı ve dosya yeniden deneme gecikmesi başına robocopy benzer belirtmenize olanak sağlar. Daha önce, bu cmdlet rasgele sırayla belirli bir yol altında tüm katmanlı dosyaları geri çağırmak istiyorsunuz. Yeni -Sipariş parametresi ile, bu cmdlet önce en sıcak verileri hatırlayacak ve bulut katmanlama ilkesini onurlandıracaktır (tarih ilkesi karşılanırsa veya birim boş alan karşılanırsa geri çağırmayı bırakın; hangisi önce gerçekleşirse).
- Yalnızca TLS 1,2 desteği (TLS 1.0 ve 1.1 devre dışı)
    - Azure Dosya Eşitlemi artık TLS 1.2'yi yalnızca TLS 1.0 ve 1.1 devre dışı bırakılmış sunucularda kullanmayı destekliyor. Bu iyileştirmeden önce, TLS 1.0 ve 1.1 sunucuda devre dışı bırakılırsa sunucu kaydı başarısız olur.
- Senkronizasyon ve bulut katmanlama için çeşitli performans ve güvenilirlik iyileştirmeleri
    - Bu sürümde çeşitli güvenilirlik ve performans iyileştirmeleri vardır. Bunlardan bazıları, bant genişliği azaltma zamanlamanız olduğunda bulut katmanlamayı daha verimli hale getirmek ve azure dosya eşitlemeyi bu gibi durumlarda daha iyi çalışır hale getirmek için hedeflenir.

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure Dosya Eşitlemesi'ni dağıtmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmeniz gerekir. Bu araç, dosya sisteminiz ve veri kümenizle ilgili desteklenmeyen karakterler veya desteklenmeyen işletim sistemi sürümü gibi olası sorunları kontrol eden bir Azure PowerShell cmdlet'tir. Yükleme ve kullanım talimatları için planlama kılavuzundaki [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server ile nasıl yükleyip yapılandırılabilenler hakkında daha fazla bilgi için Azure [Dosya Eşitleme dağıtımı için planlama](storage-sync-files-planning.md) ve Azure Dosya [Eşitleme dağıtımı nın nasıl dağıtılanakadar dağıtılabildiğini](storage-sync-files-deployment-guide.md)öğrenin.

- Aracı yükleme paketi yükseltilmiş (yönetici) izinlerle yüklenmelidir.
- Aracı Nano Server dağıtım seçeneğinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2'de desteklenir.
- Aracı en az 2 GiB bellek gerektirir. Sunucu dinamik bellek etkin leştirilmiş sanal bir makinede çalışıyorsa, VM en az 2048 MiB bellekle yapılandırılmalıdır.
- Depolama Eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birim bilgileri (SVI) dizininin sıkıştırılmış olduğu bir birimüzerinde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara yol açar.

### <a name="interoperability"></a>Birlikte Çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için Bkz. [Sorun Giderme Azure Dosya Eşitlemi.](storage-sync-files-troubleshoot.md)
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları, dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Azure Dosya Eşitleme aracısını yüklü olan bir sunucuda sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Azure Dosya Eşitleme aracısı, sunucu görüntüsünü dağıttıktan ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için [Sorun Giderme kılavuzuna](storage-sync-files-troubleshoot.md#handling-unsupported-characters) bakın.
- Bir dönemle biten dosyalar veya dizinler.
- 2.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu bitiş noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosya, sunucu bitiş noktasını silmeden önce geri çağrılmazsa katmanlı dosyalara erişilemez hale gelir. Dosyalara erişimi geri yüklemek için sunucu bitiş noktasını yeniden oluşturun. Sunucu bitiş noktasının silinmesinin üzerinden 30 gün geçtiyse veya bulut bitiş noktası silinmişse, geri çağrılmayan katmanlı dosyalar kullanılamaz hale gelir.
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama sayfalama dosyalarını sunucu bitiş noktası konumu içinde saklamayın.
- Sunucu nun adı yeniden adlandırıldıysa, portaldaki sunucu adı güncelleştirilmeyecektir.

### <a name="cloud-endpoint"></a>Bulut bitiş noktası
- Azure Dosya Eşitlemi, Azure dosya paylaşımında doğrudan değişiklik yapmayı destekler. Ancak, Azure dosya paylaşımında yapılan değişikliklerin öncelikle bir Azure Dosya Eşitlemi algılama işi tarafından keşfedilmesi gerekir. Bir bulut bitiş noktası için her 24 saatte bir değişiklik algılama işi başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB'nin son değiştirilen saatini güncelleştirmez ve eşitleme yle yapılan bir değişiklik olarak görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, varolan Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneye taşınabilir. Depolama hesabı taşınırsa, Karma Dosya Eşitleme Hizmeti'ne depolama hesabına erişim sağlamanız gerekir (bkz. [Azure Dosya Eşitlemesi'nin depolama hesabına erişmesini sağlayın).](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)

    > [!Note]  
    > Azure Dosya Eşitlemesi, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken, dosya zaman damgalarını korumak için /MIR seçeneğini kullanın. Bu, eski dosyaların yakın zamanda erişilen dosyalardan daha kısa bir sürede katmanlandırılmasını sağlar.

## <a name="agent-version-6300"></a>Aracı sürümü 6.3.0.0
Aşağıdaki sürüm notları, 27 Haziran 2019'da yayımlanan Azure Dosya Eşitleme aracısının 6.3.0.0 sürümü için hazırdır. Bu notlar sürüm 6.0.0.0 için listelenen sürüm notlarına ek olarak verilir.

Bu sürümde düzeltilen sorunların listesi:  
- Windows Server 2012 R2'de sunucu bitiş noktası konumuna SMB üzerinden erişmek veya göz atmak yavaştır 
- Azure Dosya Eşitleme v6 aracısını yükledikten sonra artan CPU kullanımı
- Bulut katmanlama telemetri iyileştirmeleri

## <a name="agent-version-6200"></a>Aracı sürümü 6.2.0.0
Aşağıdaki sürüm notları, 13 Haziran 2019'da yayımlanan Azure Dosya Eşitleme aracısının 6.2.0.0 sürümü için hazırdır. Bu notlar sürüm 6.0.0.0 için listelenen sürüm notlarına ek olarak verilir.

Bu sürümde düzeltilen sorunların listesi:  
- Bir sunucu bitiş noktası oluşturduktan sonra, arka plan hatırlama sunucuya dosya indirirken Yüksek CPU kullanımı oluşabilir
- Eşitleme ve bulut katmanlama işlemleri, belirteç sona erme nedeniyle ECS_E_SERVER_CREDENTIAL_NEEDED hatayla başarısız olabilir
- Dosyayı indirmek için URL ayrılmış karakterler içeriyorsa, dosyayı geri çağırmak başarısız olabilir 

## <a name="agent-version-6100"></a>Aracı sürümü 6.1.0.0
Aşağıdaki sürüm notları, 6 Mayıs 2019'da yayımlanan Azure Dosya Eşitleme aracısının 6.1.0.0 sürümü için hazırdır. Bu notlar sürüm 6.0.0.0 için listelenen sürüm notlarına ek olarak verilir.

Bu sürümde düzeltilen sorunların listesi:  
- Windows Yönetici Merkezi, Azure Dosya Eşitleme aracısı sürüm 6.0 yüklü olan sunucularda aracı sürümünü ve sunucu bitiş noktası yapılandırmasını görüntülemeyi başarısız olur.

## <a name="agent-version-6000"></a>Aracı sürümü 6.0.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 6.0.0.0 sürümü içindir (22 Nisan 2019'da yayımlanır).

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Aracı otomatik güncelleme desteği
  - Geri bildiriminizi duyduk ve Azure Dosya Eşitleme sunucu aracısına bir otomatik güncelleştirme özelliği ekledik. Daha fazla bilgi için [Azure Dosya Eşitleme aracısı güncelleştirme ilkesine](https://docs.microsoft.com/azure/storage/files/storage-files-release-notes#azure-file-sync-agent-update-policy)bakın.
- Azure dosya paylaşımı Aç'ları için destek
  - Azure Dosya Eşitlemesi, aLA'ların sunucu bitiş noktaları arasında eşitlemeyi her zaman desteklemiştir, ancak ALA'lar bulut bitiş noktasıyla eşitlenmemiştir (Azure dosya paylaşımı). Bu sürüm, ALA'ları sunucu ve bulut uç noktaları arasında eşitleme desteği ekler.
- Sunucu bitiş noktası için paralel yükleme ve indirme eşitleme oturumları 
  - Sunucu uç noktaları artık dosyaların aynı anda yüklenmesini ve indirilmesine destek oluyor. Dosyaların Azure dosya paylaşımına yüklenebilmeleri için yüklemenin tamamlanmasını beklemek yok artık. 
- Hacim ve katmanlama durumu elde etmek için yeni Bulut Katmanlama cmdlets
  - Bulut katmanlama ve dosya hatırlama bilgilerini elde etmek için artık iki yeni, sunucu yerel PowerShell cmdlet kullanılabilir. Sunucudaki iki olay kanalından günlük bilgilerini kullanılabilir hale getirirler:
    - Get-StorageSyncFileTieringResult, katmanlı olmayan tüm dosyaları ve yollarını listeleyecek ve bunun nedenini bildirir.
    - Get-StorageSyncFileRecallResult tüm dosya hatırlama olaylarını raporlar. Geri çağrılan her dosyayı ve yolunu ve bu geri çağırma için başarı veya hatayı listeler.
  - Varsayılan olarak, her iki olay kanalının her biri 1 MB'a kadar depolanabilir – olay kanalı boyutunu artırarak bildirilen dosya miktarını artırabilirsiniz.
- FIPS modu için destek
  - Azure Dosya Eşitlemi artık Azure Dosya Eşitleme aracısını yüklü olan sunucularda FIPS modunu etkinleştirmeyi destekler.
    - Sunucunuzda FIPS modunu etkinleştirmeden önce, sunucunuzda Azure Dosya Eşitleme aracısı ve [PackageManagement modüllerini](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) yükleyin. SUNUCUda FIPS zaten etkinse, [PackageManagement modüllerini](https://www.powershellgallery.com/packages/PackageManagement/1.1.7.2) [sunucunuza el ile indirin.](/powershell/scripting/gallery/how-to/working-with-packages/manual-download)
- Bulut katmanlama ve senkronizasyon için çeşitli güvenilirlik iyileştirmeleri

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure Dosya Eşitlemesi'ni dağıtmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmeniz gerekir. Bu araç, dosya sisteminiz ve veri kümenizle ilgili desteklenmeyen karakterler veya desteklenmeyen işletim sistemi sürümü gibi olası sorunları kontrol eden bir Azure PowerShell cmdlet'tir. Yükleme ve kullanım talimatları için planlama kılavuzundaki [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server ile nasıl yükleyip yapılandırılabilenler hakkında daha fazla bilgi için Azure [Dosya Eşitleme dağıtımı için planlama](storage-sync-files-planning.md) ve Azure Dosya [Eşitleme dağıtımı nın nasıl dağıtılanakadar dağıtılabildiğini](storage-sync-files-deployment-guide.md)öğrenin.

- Aracı yükleme paketi yükseltilmiş (yönetici) izinlerle yüklenmelidir.
- Aracı Nano Server dağıtım seçeneğinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2'de desteklenir.
- Aracı en az 2 GiB bellek gerektirir. Sunucu dinamik bellek etkin leştirilmiş sanal bir makinede çalışıyorsa, VM en az 2048 MiB bellekle yapılandırılmalıdır.
- Depolama Eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birim bilgileri (SVI) dizininin sıkıştırılmış olduğu bir birimüzerinde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara yol açar.

### <a name="interoperability"></a>Birlikte Çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için Bkz. [Sorun Giderme Azure Dosya Eşitlemi.](storage-sync-files-troubleshoot.md)
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları, dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Azure Dosya Eşitleme aracısını yüklü olan bir sunucuda sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Azure Dosya Eşitleme aracısı, sunucu görüntüsünü dağıttıktan ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için [Sorun Giderme kılavuzuna](storage-sync-files-troubleshoot.md#handling-unsupported-characters) bakın.
- Bir dönemle biten dosyalar veya dizinler.
- 2.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu bitiş noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosya, sunucu bitiş noktasını silmeden önce geri çağrılmazsa katmanlı dosyalara erişilemez hale gelir. Dosyalara erişimi geri yüklemek için sunucu bitiş noktasını yeniden oluşturun. Sunucu bitiş noktasının silinmesinin üzerinden 30 gün geçtiyse veya bulut bitiş noktası silinmişse, geri çağrılmayan katmanlı dosyalar kullanılamaz hale gelir.
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama sayfalama dosyalarını sunucu bitiş noktası konumu içinde saklamayın.
- Sunucu nun adı yeniden adlandırıldıysa, portaldaki sunucu adı güncelleştirilmeyecektir.

### <a name="cloud-endpoint"></a>Bulut bitiş noktası
- Azure Dosya Eşitlemi, Azure dosya paylaşımında doğrudan değişiklik yapmayı destekler. Ancak, Azure dosya paylaşımında yapılan değişikliklerin öncelikle bir Azure Dosya Eşitlemi algılama işi tarafından keşfedilmesi gerekir. Bir bulut bitiş noktası için her 24 saatte bir değişiklik algılama işi başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB'nin son değiştirilen saatini güncelleştirmez ve eşitleme yle yapılan bir değişiklik olarak görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, varolan Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneye taşınabilir. Depolama hesabı taşınırsa, Karma Dosya Eşitleme Hizmeti'ne depolama hesabına erişim sağlamanız gerekir (bkz. [Azure Dosya Eşitlemesi'nin depolama hesabına erişmesini sağlayın).](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)

    > [!Note]  
    > Azure Dosya Eşitlemesi, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken, dosya zaman damgalarını korumak için /MIR seçeneğini kullanın. Bu, eski dosyaların yakın zamanda erişilen dosyalardan daha kısa bir sürede katmanlandırılmasını sağlar.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.

## <a name="agent-version-5200"></a>Aracı sürümü 5.2.0.0
Aşağıdaki sürüm notları, 4 Nisan 2019'da yayımlanan Azure Dosya Eşitleme aracısının 5.2.0.0 sürümü için hazırdır. Bu notlar sürüm 5.0.2.0 için listelenen sürüm notlarına ek olarak verilir.

Bu sürümde düzeltilen sorunların listesi:  
- Çevrimdışı veri aktarımı ve veri aktarımı özgeçmiş özellikleri için güvenilirlik iyileştirmeleri
- Eşitleme telemetri iyileştirmeleri

## <a name="agent-version-5100"></a>Aracı sürümü 5.1.0.0
Aşağıdaki sürüm notları, 7 Mart 2019'da yayımlanan Azure Dosya Eşitleme aracısının 5.1.0.0 sürümü için hazırdır. Bu notlar sürüm 5.0.2.0 için listelenen sürüm notlarına ek olarak verilir.

Bu sürümde düzeltilen sorunların listesi:  
- Sunucuda değişiklik numaralandırmabaşarısızsa dosyalar 0x80c8031d (ECS_E_CONCURRENCY_CHECK_FAILED) hatası ile eşitlemeyebilir
- Eşitleme oturumu veya dosya 0x80072f78 (WININET_E_INVALID_SERVER_RESPONSE) hatası alırsa, eşitleme işlemi yeniden dener
- Dosyalar hata 0x80c80203 (ECS_E_SYNC_INVALID_STAGED_FILE) ile eşitlemek için başarısız olabilir
- Dosyaları geri çağırırken yüksek bellek kullanımı oluşabilir
- Bulut katmanlama telemetri iyileştirmeleri 

## <a name="agent-version-5020"></a>Aracı sürümü 5.0.2.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 5.0.2.0 sürümü içindir (12 Şubat 2019'da yayımlanır).

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Azure Devlet bulutu desteği
  - Azure Devlet bulutu için önizleme desteği ekledik. Bu, beyaz listedeki bir abonelik ve Microsoft'tan özel bir aracı indirme gerektirir. Önizlemeye erişmek için lütfen doğrudan bize [AzureFiles@microsoft.com](mailto:AzureFiles@microsoft.com)e-posta gönderin.
- Veri Çoğaltma Desteği
    - Veri Çoğaltma artık Windows Server 2016 ve Windows Server 2019'da etkin leştirilmiş bulut katmanlama ile tam olarak desteklendi. Bulut katmanlama özelliğine sahip bir birimde kopyayı etkinleştirmek, daha fazla depolama alanı sağlamadan şirket içinde daha fazla dosyaönbelleğe sahip yapmanızı sağlar.
- Çevrimdışı veri aktarımı desteği (örn. Veri Kutusu üzerinden)
    - Seçtiğiniz herhangi bir yolla büyük miktarda veriyi Azure Dosya Eşitle'ye kolayca geçirin. Azure Veri Kutusu, AzCopy ve hatta üçüncü taraf geçiş hizmetlerini seçebilirsiniz. Veri Kutusu durumunda, verilerinizi Azure'a sokmak için büyük miktarda bant genişliği kullanmanıza gerek yok – sadece oraya postalayın! Daha fazla bilgi için [Çevrimdışı Veri Aktarım Dokümanları'na](https://aka.ms/AFS/OfflineDataTransfer)bakın.
- Geliştirilmiş eşitleme performansı
    - Aynı birimde birden çok sunucu bitiş noktası olan müşteriler, bu sürümden önce yavaş eşitleme performansı yaşamış olabilir. Azure Dosya Eşitlemi, açık tutamaçları olan dosyaları eşitlemek için sunucuda günde bir kez geçici bir VSS anlık görüntüsü oluşturur. Eşitleme şimdi, VSS eşitleme oturumu etkin olduğunda birim üzerinde eşitleme sağlayan birden çok sunucu uç noktasını destekler. Eşitlemenin birimdeki diğer sunucu uç noktalarında devam edilebilmeleri için VSS eşitleme oturumunun tamamlanmasını beklemek yok artık.
- Portalda geliştirilmiş izleme
    - Grafikler görüntülemek için Depolama Eşitleme Hizmeti portalına eklendi:
        - Eşitlenen dosya sayısı
        - Aktarılan verilerin boyutu
        - Eşitlenen dosya sayısı
        - Geri çağrılan verilerin boyutu
        - Sunucu bağlantı durumu
    - Daha fazla bilgi için Bkz. [Azure Dosya Eşitlemeyi'ni izleyin.](https://docs.microsoft.com/azure/storage/files/storage-sync-files-monitoring)
- Geliştirilmiş ölçeklenebilirlik ve güvenilirlik
    - Bir dizindeki maksimum dosya sistemi nesnesi (dizin ve dosya) sayısı 1.000.000'e yükselmiştir. Önceki limit 200.000'di.
    - Eşitleme, büyük dosyalar için aktarım kesintiye uğradığında yeniden aktarmak yerine veri aktarımına devam etmeye çalışır 

### <a name="evaluation-tool"></a>Değerlendirme Aracı
Azure Dosya Eşitlemesi'ni dağıtmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmeniz gerekir. Bu araç, dosya sisteminiz ve veri kümenizle ilgili desteklenmeyen karakterler veya desteklenmeyen işletim sistemi sürümü gibi olası sorunları kontrol eden bir Azure PowerShell cmdlet'tir. Yükleme ve kullanım talimatları için planlama kılavuzundaki [Değerlendirme Aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısını Windows Server ile nasıl yükleyip yapılandırılabilenler hakkında daha fazla bilgi için Azure [Dosya Eşitleme dağıtımı için planlama](storage-sync-files-planning.md) ve Azure Dosya [Eşitleme dağıtımı nın nasıl dağıtılanakadar dağıtılabildiğini](storage-sync-files-deployment-guide.md)öğrenin.

- Aracı yükleme paketi yükseltilmiş (yönetici) izinlerle yüklenmelidir.
- Aracı, Windows Server Core veya Nano Server dağıtım seçeneklerinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2'de desteklenir.
- Aracı en az 2 GiB bellek gerektirir. Sunucu dinamik bellek etkin leştirilmiş sanal bir makinede çalışıyorsa, VM en az 2048 MiB bellekle yapılandırılmalıdır.
- Depolama Eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birim bilgileri (SVI) dizininin sıkıştırılmış olduğu bir birimüzerinde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara yol açar.
- FIPS modu desteklenmez ve devre dışı bırakılmalı. 

### <a name="interoperability"></a>Birlikte Çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için Bkz. [Sorun Giderme Azure Dosya Eşitlemi.](storage-sync-files-troubleshoot.md)
- Dosya Sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları, dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarına neden olabilir.
- Azure Dosya Eşitleme aracısını yüklü olan bir sunucuda sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Azure Dosya Eşitleme aracısı, sunucu görüntüsünü dağıttıktan ve sysprep mini kurulumu tamamladıktan sonra yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için [Sorun Giderme kılavuzuna](storage-sync-files-troubleshoot.md#handling-unsupported-characters) bakın.
- Bir dönemle biten dosyalar veya dizinler.
- 2.048 karakterden uzun yollar.
- 2 KB2den büyük olması durumunda bir güvenlik tanımlayıcısının isteğe bağlı erişim denetim listesi (DACL) kısmı. (Bu sorun yalnızca tek bir öğe üzerinde yaklaşık 40'tan fazla erişim denetimi girişi (ACE) olduğunda geçerlidir.)
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu bitiş noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Dosya, sunucu bitiş noktasını silmeden önce geri çağrılmazsa katmanlı dosyalara erişilemez hale gelir. Dosyalara erişimi geri yüklemek için sunucu bitiş noktasını yeniden oluşturun. Sunucu bitiş noktasının silinmesinin üzerinden 30 gün geçtiyse veya bulut bitiş noktası silinmişse, geri çağrılmayan katmanlı dosyalar kullanılamaz hale gelir.
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama sayfalama dosyalarını sunucu bitiş noktası konumu içinde saklamayın.
- Sunucu nun adı yeniden adlandırıldıysa, portaldaki sunucu adı güncelleştirilmeyecektir.

### <a name="cloud-endpoint"></a>Bulut bitiş noktası
- Azure Dosya Eşitlemi, Azure dosya paylaşımında doğrudan değişiklik yapmayı destekler. Ancak, Azure dosya paylaşımında yapılan değişikliklerin öncelikle bir Azure Dosya Eşitlemi algılama işi tarafından keşfedilmesi gerekir. Bir bulut bitiş noktası için her 24 saatte bir değişiklik algılama işi başlatılır. Ayrıca, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB'nin son değiştirilen saatini güncelleştirmez ve eşitleme yle yapılan bir değişiklik olarak görülmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, varolan Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneye taşınabilir. Depolama hesabı taşınırsa, Karma Dosya Eşitleme Hizmeti'ne depolama hesabına erişim sağlamanız gerekir (bkz. [Azure Dosya Eşitlemesi'nin depolama hesabına erişmesini sağlayın).](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)

    > [!Note]  
    > Azure Dosya Eşitlemesi, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken, dosya zaman damgalarını korumak için /MIR seçeneğini kullanın. Bu, eski dosyaların yakın zamanda erişilen dosyalardan daha kısa bir sürede katmanlandırılmasını sağlar.
- SMB'nin dosya meta verilerini önbelleğe hatalı bir şekilde alması nedeniyle dosya özellikleri bir SMB istemcisinden görüntülenirken çevrimdışı özniteliği hatalı şekilde ayarlanmış görünebilir.
