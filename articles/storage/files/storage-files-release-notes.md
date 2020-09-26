---
title: Azure Dosya Eşitleme Aracısı için sürüm notları | Microsoft Docs
description: Azure Dosya Eşitleme aracısına ait sürüm notlarını okuyun ve bu, kuruluşunuzun dosya paylaşımlarını Azure dosyalarında merkezileştirmenizi sağlar.
services: storage
author: wmgries
ms.service: storage
ms.topic: conceptual
ms.date: 9/24/2020
ms.author: wgries
ms.subservice: files
ms.openlocfilehash: 0b99ce2afcdb5fd7462827fb9893e34577fc6c02
ms.sourcegitcommit: 5dbea4631b46d9dde345f14a9b601d980df84897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91371210"
---
# <a name="release-notes-for-the-azure-file-sync-agent"></a>Azure Dosya Eşitleme aracısı sürüm notları
Azure Dosya Eşitleme aracısı şirket içi dosya sunucularının sağladığı esneklik, performans ve uyumluluk özelliklerinden vazgeçmeden kuruluşunuzun dosya paylaşımlarını Azure Dosyaları'nda toplamanızı sağlar. Windows Server yüklemeleriniz, Azure dosya paylaşımınızın hızlı bir önbelleğine dönüştürülür. Verilere yerel olarak erişmek için Windows Server üzerinde kullanılabilen tüm protokolleri (SMB, NFS ve FTPS gibi) kullanabilirsiniz. Dünya çapında istediğiniz sayıda önbellek oluşturabilirsiniz.

Bu makalede Azure Dosya Eşitleme aracısının desteklenen sürümleri için sürüm notları verilmektedir.

## <a name="supported-versions"></a>Desteklenen sürümler
Aşağıdaki Azure Dosya Eşitleme Aracısı sürümleri desteklenir:

| Ina | Aracı sürüm numarası | Sürüm tarihi | Durum |
|----|----------------------|--------------|------------------|
| V 10.1 sürüm- [KB4522411](https://support.microsoft.com/en-us/help/4522411)| 10.1.0.0 | 5 Haziran 2020 | Desteklenir |
| 2020 Mayıs güncelleştirme paketi- [KB4522412](https://support.microsoft.com/help/4522412)| 10.0.2.0 | 19 Mayıs 2020 | Desteklenir |
| İle v10 arasındaki Release- [KB4522409](https://support.microsoft.com/en-us/help/4522409)| 10.0.0.0 | 9 Nisan 2020 | Desteklenir |
| Aralık 2019 güncelleştirme paketi- [KB4522360](https://support.microsoft.com/help/4522360)| 9.1.0.0 | 12 Aralık 2019 | Desteklenen-aracı sürümü, 16 Şubat 2021 ' de sona erecektir |
| V9 Release- [KB4522359](https://support.microsoft.com/help/4522359)| 9.0.0.0 | 2 Aralık 2019 | Desteklenen-aracı sürümü, 16 Şubat 2021 ' de sona erecektir |
| V8 Release- [KB4511224](https://support.microsoft.com/help/4511224)| 8.0.0.0 | 8 Ekim 2019 | Desteklenen-aracı sürümü 12 Ocak 2021 tarihinde sona erecektir |

## <a name="unsupported-versions"></a>Desteklenmeyen sürümler
Aşağıdaki Azure Dosya Eşitleme Aracısı sürümlerinin zaman aşımına uğradı ve artık desteklenmiyor:

| Ina | Aracı sürüm numarası | Sürüm tarihi | Durum |
|----|----------------------|--------------|------------------|
| V7 sürümü | 7.0.0.0 - 7.2.0.0 | Yok | Desteklenmez-1 Eylül 2020 tarihinde aracılı sürümler zaman aşımına uğradı |
| V6 sürümü | 6.0.0.0 - 6.3.0.0 | Yok | Desteklenmez-, 21 Nisan 2020 tarihinde Aracı sürümleri zaman aşımına uğradı |
| V5 sürümü | 5.0.2.0 - 5.2.0.0 | Yok | Desteklenmez-18 Mart 2020 tarihinde Aracı sürümleri zaman aşımına uğradı |
| V4 sürümü | 4.0.1.0 - 4.3.0.0 | Yok | Desteklenmiyor-aracı sürümlerinin tarihi 6 Kasım 2019 ' de |
| V3 sürümü | 3.1.0.0 - 3.4.0.0 | Yok | Desteklenmiyor-Aracı sürümleri 19 Ağustos 2019 tarihinde zaman aşımına uğradı |
| Ön GA aracıları | 1.1.0.0-3.0.13.0 | Yok | Desteklenmez-1 Ekim 2018 ' de Aracı sürümleri zaman aşımına uğradı |

### <a name="azure-file-sync-agent-update-policy"></a>Azure Dosya Eşitleme aracısı güncelleştirme ilkesi
[!INCLUDE [storage-sync-files-agent-update-policy](../../../includes/storage-sync-files-agent-update-policy.md)]

## <a name="agent-version-10100"></a>Aracı sürümü 10.1.0.0
Aşağıdaki sürüm notları, 5 Haziran 2020 ' de yayınlanan Azure Dosya Eşitleme aracısının sürüm 10.1.0.0 içindir. Bu notlar, 10.0.0.0 ve 10.0.2.0 sürümü için listelenen sürüm notlarına ek niteliğindedir.

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Azure özel uç nokta desteği
    - Depolama eşitleme hizmeti ile eşitleme trafiği artık özel bir uç noktaya gönderilebilir. Bu, bir ExpressRoute veya VPN bağlantısı üzerinden tünelleme olanağı sunar. Daha fazla bilgi için bkz. [Azure dosya eşitleme ağ uç noktalarını yapılandırma](https://docs.microsoft.com/azure/storage/files/storage-sync-files-networking-endpoints).
- Eşitlenmiş dosya ölçümü, son olarak değil büyük bir eşitleme çalıştırılırken ilerleme durumunu görüntüleyecektir.
- Aracı yükleme, bulut katmanlama, eşitleme ve telemetri için çeşitli güvenilirlik geliştirmeleri

## <a name="agent-version-10020"></a>Aracı sürümü 10.0.2.0
Aşağıdaki sürüm notları, 19 Mayıs 2020 ' de yayınlanan Azure Dosya Eşitleme aracısının sürüm 10.0.2.0 içindir. Bu notlar, 10.0.0.0 sürümü için listelenen sürüm notlarına ek niteliğindedir.

Bu sürümde düzeltilen sorun:  
- Depolama eşitleme Aracısı (FileSyncSvc), Azure Dosya Eşitleme ile v10 arasındaki aracısını yükledikten sonra sıkça çöker.

> [!Note]  
>Bu sürüm, yeni bir sürüm kullanılabilir olduğunda otomatik olarak güncelleştirilecek şekilde yapılandırılmış sunuculara eklenmedi. Bu güncelleştirmeyi yüklemek için Microsoft Update veya Microsoft Update kataloğunu kullanın (bkz. yükleme yönergeleri için bkz. [KB4522412](https://support.microsoft.com/help/4522412) ).

## <a name="agent-version-10000"></a>Aracı sürümü 10.0.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının 10.0.0.0 sürümü (9 Nisan 2020 ' de yayımlanmıştır) içindir.

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Portalda geliştirilmiş eşitleme ilerlemesi
    - Ile v10 arasındaki aracı sürümü ile Azure portal yakında çalışan eşitleme oturumunun türünü göstermeye başlayacaktır. Örneğin ilk indirme, düzenli indirme, arka plan geri çekme (hızlı olağanüstü durum kurtarma durumları) ve benzeri. 

- Geliştirilmiş bulut katmanlama portalı deneyimi
    - Katman veya geri çağırma başarısız olan dosyalarınız varsa, artık sunucu uç noktası özelliklerindeki katmanlama hatalarını görüntüleyebilirsiniz.
    - Sunucu uç noktası için ek bulut katmanlama bilgileri mevcuttur:
        - Yerel önbellek boyutu
        - Önbellek kullanım verimliliği
        - Bulut katmanlama ilkesi ayrıntıları: birim boyutu, geçerli boş alan veya yerel önbellekteki en eski dosyanın en son erişildiği zaman.
    - Bu değişiklikler ilk Ile v10 arasındaki aracı sürümünden kısa bir süre sonra Azure portal gönderilir.

- Depolama eşitleme hizmeti ve/veya depolama hesabını farklı bir Azure Active Directory kiracısına taşıma desteği
    - Azure Dosya Eşitleme artık depolama eşitleme hizmeti ve/veya depolama hesabı farklı bir kaynak grubuna, aboneliğe veya Azure AD kiracısına taşınmayı destekler.
    
- Çeşitli performans ve güvenilirlik iyileştirmeleri
    - Depolama hesabında sanal ağ (VNET) ve güvenlik duvarı kuralları yapılandırılmışsa Azure dosya paylaşımında değişiklik algılama başarısız olabilir.
    - Geri çağırma ile ilişkili azaltılan bellek tüketimi. 
    - [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) cmdlet 'i kullanılırken gelişmiş performans.
    - Diğer çeşitli güvenilirlik geliştirmeleri. 
    
### <a name="evaluation-tool"></a>Değerlendirme aracı
Azure Dosya Eşitleme dağıtılmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmelisiniz. Bu araç, desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü gibi dosya sisteminizle ve veri kümesiyle ilgili olası sorunları denetleyen bir Azure PowerShell cmdlet 'i. Yükleme ve kullanım yönergeleri için, planlama kılavuzundaki [değerlendirme aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısının Windows Server ile nasıl yükleneceği ve yapılandırılacağı hakkında daha fazla bilgi için bkz. [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Aracı nano sunucu dağıtım seçeneğinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 'de desteklenir.
- Aracı için en az 2 GiB bellek gerekir. Sunucu dinamik bellek etkinleştirilmiş bir sanal makinede çalışıyorsa, VM en az 2048 MIB ile yapılandırılmalıdır.
- Depolama eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birimi bilgileri (SVı) dizini sıkıştırılmış bir birimde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara neden olacak.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için bkz. [sorun giderme Azure dosya eşitleme](storage-sync-files-troubleshoot.md).
- Dosya sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarıyla neden olabilir.
- Azure Dosya Eşitleme aracısının yüklü olduğu bir sunucuda Sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Sunucu görüntüsünü dağıttıktan ve Sysprep Mini Kurulumu tamamlandıktan sonra Azure Dosya Eşitleme Aracısı yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için bkz. [sorun giderme kılavuzu](storage-sync-files-troubleshoot.md#handling-unsupported-characters) .
- Bir noktayla biten dosyalar veya dizinler.
- 2.048 karakterden uzun yollar.
- Denetim için kullanılan bir güvenlik tanımlayıcısının sistem erişim denetim listesi (SACL) kısmı.
- Genişletilmiş öznitelikler.
- Alternatif veri akışları.
- Yeniden ayrıştırma noktaları.
- Sabit bağlantılar.
- Bir dosyaya diğer uç noktalardan gelen değişiklikler eşitlendiğinde sıkıştırma ayarları (sunucu dosyasında mevcutsa) korunmaz.
- Hizmetin verileri okumasını engelleyen EFS (veya diğer kullanıcı şifrelemesi modları) şifrelemeli dosyalar.

    > [!Note]  
    > Azure Dosya Eşitleme her zaman aktarımdaki verileri şifreler. Veriler her zaman Azure’da bekleyen durumda şifrelenir.
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama disk belleği dosyasını sunucu uç noktası konumu içinde depolamayın.
- Sunucu yeniden adlandırılırsa portaldaki sunucu adı güncellenmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure Dosya Eşitleme, doğrudan Azure dosya paylaşımında değişiklik yapmayı destekler. Ancak, ilk olarak Azure dosya paylaşımında yapılan tüm değişikliklerin bir Azure Dosya Eşitleme değişiklik algılama işi tarafından bulunması gerekir. Her 24 saatte bir bulut uç noktası için değişiklik algılama işi başlatılır. Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) PowerShell cmdlet 'i Azure dosya paylaşımındaki değişikliklerin algılanmasını el ile başlatmak için kullanılabilir. Buna ek olarak, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB son değiştirilme zamanını güncelleştirmeyecektir ve eşitleme tarafından değişiklik olarak görünmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı farklı bir kaynak grubuna, aboneliğe veya Azure AD kiracısına taşınabilir. Depolama eşitleme hizmeti veya depolama hesabı taşındıktan sonra, Microsoft. Storagessync uygulamasına depolama hesabına erişim sağlamanız gerekir (bkz. [Azure dosya eşitleme depolama hesabına erişimi olduğundan emin olun](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Bulut uç noktası oluşturulurken, depolama eşitleme hizmeti ve depolama hesabı aynı Azure AD kiracısında olmalıdır. Bulut uç noktası oluşturulduktan sonra, depolama eşitleme hizmeti ve depolama hesabı farklı Azure AD kiracılarına taşınabilir.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken dosya zaman damgalarını korumak için/MıR seçeneğini kullanın. Bu, eski dosyaların son erişilen dosyalardan daha önce katmanlı olmasını sağlayacaktır.

## <a name="agent-version-9100"></a>Aracı sürümü 9.1.0.0
Aşağıdaki sürüm notları, 12 Aralık 2019 ' de yayınlanan Azure Dosya Eşitleme aracısının sürüm 9.1.0.0 içindir. Bu notlar, sürüm 9.0.0.0 için listelenen sürüm notlarına ek niteliğindedir.

Bu sürümde düzeltilen sorun:  
- Azure Dosya Eşitleme Aracı sürüm 9,0 ' e yükselttikten sonra eşitleme aşağıdaki hatalardan biriyle başarısız olur:
    - 0x8e5e044e (JET_errWriteConflict)
    - 0x8e5e0450 (JET_errInvalidSesid)
    - 0x8e5e0442 (JET_errInstanceUnavailable)

## <a name="agent-version-9000"></a>Aracı sürümü 9.0.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısına ait sürüm 9.0.0.0 yöneliktir (2 Aralık 2019 ' de yayımlanmıştır).

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Self Servis geri yükleme desteği
    - Kullanıcılar artık önceki sürüm özelliğini kullanarak dosyalarını geri yükleyebilir. V9 sürümünden önce, bulut katmanlaması etkinleştirilmiş birimlerde önceki sürüm özelliği destekleniyordu. Bu özellik, bulut katmanlaması etkin olan bir uç noktanın bulunduğu her birim için ayrı olarak etkinleştirilmelidir. Daha fazla bilgi için bkz.  
[Önceki sürümler ve VSS (birim gölge kopyası hizmeti) üzerinden self servis geri yükleme](https://docs.microsoft.com/azure/storage/files/storage-sync-files-deployment-guide#self-service-restore-through-previous-versions-and-vss-volume-shadow-copy-service). 
 
- Daha büyük dosya paylaşma boyutları için destek 
    - Azure Dosya Eşitleme artık tek bir eşitleme ad alanında 64TiB ve 100.000.000 dosyalarını desteklemektedir.  
 
- Sunucu 2019 üzerinde yinelenen verileri kaldırma desteği 
    - Yinelenen verileri kaldırma, Windows Server 2019 ' de etkinleştirilen bulut katmanlaması ile desteklenmektedir. Bulut katmanlaması olan birimlerde yinelenen verileri kaldırmayı desteklemek için Windows Update [KB4520062](https://support.microsoft.com/help/4520062) yüklenmelidir. 
 
- Bir dosya katmanında en düşük dosya boyutu geliştirildi 
    - Bir dosya için en küçük dosya boyutu, artık dosya sistemi kümesi boyutuna (çift dosya sistemi kümesi boyutu) göre belirlenir. Örneğin, varsayılan olarak, NTFS dosya sistemi küme boyutu 4KB 'tır, bir dosya için en düşük dosya boyutu 8 KB 'tır. 
 
- Ağ bağlantısı test cmdlet 'i 
    - Azure Dosya Eşitleme yapılandırmanın bir parçası olarak, birden fazla hizmet uç noktasına başvurulmalıdır. Bunların her biri, sunucu tarafından erişilebilmesi gereken kendi DNS adına sahiptir. Bu URL 'Ler, bir sunucunun kaydedildiği bölgeye de özeldir. Bir sunucu kaydedildikten sonra, bu sunucuya özgü tüm URL 'lerle iletişimleri test etmek için bağlantı testi cmdlet 'i (PowerShell ve sunucu kayıt yardımcı programı) kullanılabilir. Bu cmdlet, tamamlanmamış iletişim, sunucunun Azure Dosya Eşitleme ile tamamen çalışmasını engellediğinde ve proxy ve güvenlik duvarı yapılandırmalarının ince ayar yapmak için kullanılabilir olduğunda sorun gidermeye yardımcı olabilir.  
 
        Ağ bağlantısı testini çalıştırmak için aşağıdaki PowerShell komutlarını çalıştırın: 
 
        Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"  
        Test-StorageSyncNetworkConnectivity
 
- Bulut katmanlaması etkinleştirildiğinde sunucu uç noktası geliştirmesini kaldır 
    - Daha önce olduğu gibi, sunucu uç noktasının kaldırılması Azure dosya paylaşımındaki dosyaların kaldırılmasına neden olmaz. Ancak, yerel sunucudaki yeniden ayrıştırma noktalarının davranışı değişmiştir. Yeniden ayrıştırma noktaları (sunucuda yerel olmayan dosyalara işaretçiler) artık sunucu uç noktası kaldırılırken silinir. Tamamen önbelleğe alınan dosyalar sunucuda kalır. Sunucu uç noktası kaldırılırken [yalnız bırakılmış katmanlı dosyaları](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint) engellemek için bu geliştirme yapılmıştır. Sunucu uç noktası yeniden oluşturulduğunda katmanlı dosyalar için yeniden ayrıştırma noktaları sunucuda yeniden oluşturulur.  
 
- Performans ve güvenilirlik iyileştirmeleri 
    - Daha az geri çekme başarısızlığı. Geri çağırma boyutu artık ağ bant genişliğine göre otomatik olarak ayarlanır. 
    - Yeni bir sunucu bir eşitleme grubuna eklenirken geliştirilmiş indirme performansı. 
    - Kısıtlama çakışmaları nedeniyle, azaltılmış dosyalar eşitlenmiyor. 
    - Sunucu uç noktası yolu bir birim bağlama noktası ise, dosyalar katmana veya belirli senaryolarda beklenmedik şekilde geri çekilir.
    
### <a name="evaluation-tool"></a>Değerlendirme aracı
Azure Dosya Eşitleme dağıtılmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmelisiniz. Bu araç, desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü gibi dosya sisteminizle ve veri kümesiyle ilgili olası sorunları denetleyen bir Azure PowerShell cmdlet 'i. Yükleme ve kullanım yönergeleri için, planlama kılavuzundaki [değerlendirme aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısının Windows Server ile nasıl yükleneceği ve yapılandırılacağı hakkında daha fazla bilgi için bkz. [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Aracı nano sunucu dağıtım seçeneğinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 'de desteklenir.
- Aracı için en az 2 GiB bellek gerekir. Sunucu dinamik bellek etkinleştirilmiş bir sanal makinede çalışıyorsa, VM en az 2048 MIB ile yapılandırılmalıdır.
- Depolama eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birimi bilgileri (SVı) dizini sıkıştırılmış bir birimde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara neden olacak.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için bkz. [sorun giderme Azure dosya eşitleme](storage-sync-files-troubleshoot.md).
- Dosya sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarıyla neden olabilir.
- Azure Dosya Eşitleme aracısının yüklü olduğu bir sunucuda Sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Sunucu görüntüsünü dağıttıktan ve Sysprep Mini Kurulumu tamamlandıktan sonra Azure Dosya Eşitleme Aracısı yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için bkz. [sorun giderme kılavuzu](storage-sync-files-troubleshoot.md#handling-unsupported-characters) .
- Bir noktayla biten dosyalar veya dizinler.
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
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Sunucu uç noktası silinmeden önce dosyalar geri çağrılmıyorsa katmanlı dosyalar erişilemez duruma gelir. Dosyalara erişimi geri yüklemek için sunucu uç noktasını yeniden oluşturun. Sunucu uç noktası silindikten sonra 30 gün geçmişse veya bulut uç noktası silinmişse, geri çağrılmayan katmanlı dosyalar kullanılamaz olur. Daha fazla bilgi edinmek için bkz. [sunucu uç noktası silindikten sonra katmanlı dosyalara sunucuda erişilemiyor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint).
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama disk belleği dosyasını sunucu uç noktası konumu içinde depolamayın.
- Sunucu yeniden adlandırılırsa portaldaki sunucu adı güncellenmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure Dosya Eşitleme, doğrudan Azure dosya paylaşımında değişiklik yapmayı destekler. Ancak, ilk olarak Azure dosya paylaşımında yapılan tüm değişikliklerin bir Azure Dosya Eşitleme değişiklik algılama işi tarafından bulunması gerekir. Her 24 saatte bir bulut uç noktası için değişiklik algılama işi başlatılır. Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) PowerShell cmdlet 'i Azure dosya paylaşımındaki değişikliklerin algılanmasını el ile başlatmak için kullanılabilir. Buna ek olarak, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB son değiştirilme zamanını güncelleştirmeyecektir ve eşitleme tarafından değişiklik olarak görünmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, mevcut Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneliğe taşınabilir. Depolama hesabı taşınmışsa, karma Dosya Eşitleme hizmetine depolama hesabına erişim sağlamanız gerekir (bkz. [Azure dosya eşitleme depolama hesabına erişimi olduğundan emin olun](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure Dosya Eşitleme, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken dosya zaman damgalarını korumak için/MıR seçeneğini kullanın. Bu, eski dosyaların son erişilen dosyalardan daha önce katmanlı olmasını sağlayacaktır.
- pagefile.sys, bulut katmanlaması etkinleştirilmiş bir birimde bulunuyorsa dosyalar katman üzerinde başarısız olabilir. pagefile.sys, bulut katmanlaması devre dışı bırakılmış bir birimde bulunmalıdır.

## <a name="agent-version-8000"></a>Aracı sürümü 8.0.0.0
Aşağıdaki sürüm notları Azure Dosya Eşitleme aracısının sürüm 8.0.0.0 (8 Ekim 2019 ' de yayımlanmıştır) içindir.

### <a name="improvements-and-issues-that-are-fixed"></a>Düzeltilen geliştirmeler ve sorunlar

- Performans geliştirmelerini geri yükleme
    - Azure Backup aracılığıyla gerçekleştirilen kurtarma için daha hızlı kurtarma süreleri. Geri yüklenen dosyalar, Azure Dosya Eşitleme sunuculara daha hızlı eşitlenir. 
- Geliştirilmiş bulut katmanlama portalı deneyimi  
    - Geri çağıramayan katmanlı dosyalarınız varsa, artık sunucu uç noktası özelliklerindeki geri çağırma hatalarını görüntüleyebilirsiniz. Ayrıca, bulut katmanlama filtresi sürücüsü sunucuda yüklü değilse sunucu uç noktası durumu artık bir hata ve risk azaltma adımları gösterir.
- Daha basit aracı yüklemesi
    - Daha basit ve hızlı bir şekilde sunucu kaydetmek için Az\azurerd PowerShell modülünün artık gerekli değildir.
- Çeşitli performans ve güvenilirlik iyileştirmeleri

### <a name="evaluation-tool"></a>Değerlendirme aracı
Azure Dosya Eşitleme dağıtılmadan önce, Azure Dosya Eşitleme değerlendirme aracını kullanarak sisteminizle uyumlu olup olmadığını değerlendirmelisiniz. Bu araç, desteklenmeyen karakterler veya desteklenmeyen bir işletim sistemi sürümü gibi dosya sisteminizle ve veri kümesiyle ilgili olası sorunları denetleyen bir Azure PowerShell cmdlet 'i. Yükleme ve kullanım yönergeleri için, planlama kılavuzundaki [değerlendirme aracı](https://docs.microsoft.com/azure/storage/files/storage-sync-files-planning#evaluation-cmdlet) bölümüne bakın. 

### <a name="agent-installation-and-server-configuration"></a>Aracı yükleme ve sunucu yapılandırması
Azure Dosya Eşitleme aracısının Windows Server ile nasıl yükleneceği ve yapılandırılacağı hakkında daha fazla bilgi için bkz. [Azure dosya eşitleme dağıtımı planlama](storage-sync-files-planning.md) ve [Azure dosya eşitleme dağıtma](storage-sync-files-deployment-guide.md).

- Aracı yükleme paketinin yükseltilmiş (yönetici) izinlerle yüklenmesi gerekir.
- Aracı nano sunucu dağıtım seçeneğinde desteklenmez.
- Aracı yalnızca Windows Server 2019, Windows Server 2016 ve Windows Server 2012 R2 'de desteklenir.
- Aracı için en az 2 GiB bellek gerekir. Sunucu dinamik bellek etkinleştirilmiş bir sanal makinede çalışıyorsa, VM en az 2048 MIB ile yapılandırılmalıdır.
- Depolama eşitleme Aracısı (FileSyncSvc) hizmeti, sistem birimi bilgileri (SVı) dizini sıkıştırılmış bir birimde bulunan sunucu uç noktalarını desteklemez. Bu yapılandırma beklenmeyen sonuçlara neden olacak.

### <a name="interoperability"></a>Birlikte çalışabilirlik
- Çevrimdışı özniteliğe dikkat edip ilgili dosyaların içeriğini okumayı atlamadıkları sürece katmanlı dosyalara erişen virüsten koruma, yedekleme ve diğer amaçlı uygulamalar istenmeyen geri çekme durumlarına neden olabilir. Daha fazla bilgi için bkz. [sorun giderme Azure dosya eşitleme](storage-sync-files-troubleshoot.md).
- Dosya sunucusu Kaynak Yöneticisi (FSRM) dosya ekranları dosya ekranı nedeniyle dosyalar engellendiğinde sonsuz eşitleme hatalarıyla neden olabilir.
- Azure Dosya Eşitleme aracısının yüklü olduğu bir sunucuda Sysprep çalıştırmak desteklenmez ve beklenmeyen sonuçlara yol açabilir. Sunucu görüntüsünü dağıttıktan ve Sysprep Mini Kurulumu tamamlandıktan sonra Azure Dosya Eşitleme Aracısı yüklenmelidir.

### <a name="sync-limitations"></a>Eşitleme sınırlamaları
Aşağıdaki öğeler eşitlenmez ancak sistem normal şekilde çalışmaya devam eder:
- Desteklenmeyen karakterleri olan dosyalar. Desteklenmeyen karakterlerin listesi için bkz. [sorun giderme kılavuzu](storage-sync-files-troubleshoot.md#handling-unsupported-characters) .
- Bir noktayla biten dosyalar veya dizinler.
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
 
### <a name="server-endpoint"></a>Sunucu uç noktası
- Sunucu uç noktası yalnızca bir NTFS biriminde oluşturulabilir. ReFS, FAT, FAT32 ve diğer dosya sistemleri şu an için Azure Dosya Eşitleme tarafından desteklenmez.
- Sunucu uç noktası silinmeden önce dosyalar geri çağrılmıyorsa katmanlı dosyalar erişilemez duruma gelir. Dosyalara erişimi geri yüklemek için sunucu uç noktasını yeniden oluşturun. Sunucu uç noktası silindikten sonra 30 gün geçmişse veya bulut uç noktası silinmişse, geri çağrılmayan katmanlı dosyalar kullanılamaz olur. Daha fazla bilgi edinmek için bkz. [sunucu uç noktası silindikten sonra katmanlı dosyalara sunucuda erişilemiyor](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cazure-portal#tiered-files-are-not-accessible-on-the-server-after-deleting-a-server-endpoint).
- Sistem biriminde bulut katmanlama desteklenmez. Sistem biriminde sunucu uç noktası oluşturmak için, sunucu uç noktasını oluştururken bulutta katmanlamayı devre dışı bırakın.
- Yük Devretme Kümelemesi yalnızca kümelenmiş disklerle desteklenir, Küme Paylaşılan Birimleri (CSV) ile desteklenmez.
- Sunucu uç noktası iç içe olamaz. Aynı birim üzerinde başka bir uç noktaya paralel olarak birlikte bulunabilir.
- Bir işletim sistemi veya uygulama disk belleği dosyasını sunucu uç noktası konumu içinde depolamayın.
- Sunucu yeniden adlandırılırsa portaldaki sunucu adı güncellenmez.

### <a name="cloud-endpoint"></a>Bulut uç noktası
- Azure Dosya Eşitleme, doğrudan Azure dosya paylaşımında değişiklik yapmayı destekler. Ancak, ilk olarak Azure dosya paylaşımında yapılan tüm değişikliklerin bir Azure Dosya Eşitleme değişiklik algılama işi tarafından bulunması gerekir. Her 24 saatte bir bulut uç noktası için değişiklik algılama işi başlatılır. Azure dosya paylaşımında değiştirilen dosyaları hemen eşitlemek için [Invoke-AzStorageSyncChangeDetection](https://docs.microsoft.com/powershell/module/az.storagesync/invoke-azstoragesyncchangedetection) PowerShell cmdlet 'i Azure dosya paylaşımındaki değişikliklerin algılanmasını el ile başlatmak için kullanılabilir. Buna ek olarak, REST protokolü üzerinden bir Azure dosya paylaşımında yapılan değişiklikler, SMB son değiştirilme zamanını güncelleştirmeyecektir ve eşitleme tarafından değişiklik olarak görünmez.
- Depolama eşitleme hizmeti ve/veya depolama hesabı, mevcut Azure AD kiracısı içinde farklı bir kaynak grubuna veya aboneliğe taşınabilir. Depolama hesabı taşınmışsa, karma Dosya Eşitleme hizmetine depolama hesabına erişim sağlamanız gerekir (bkz. [Azure dosya eşitleme depolama hesabına erişimi olduğundan emin olun](https://docs.microsoft.com/azure/storage/files/storage-sync-files-troubleshoot?tabs=portal1%2Cportal#troubleshoot-rbac)).

    > [!Note]  
    > Azure Dosya Eşitleme, aboneliğin farklı bir Azure AD kiracısına taşınmasını desteklemez.

### <a name="cloud-tiering"></a>Bulut katmanlaması
- Katmanlanmış bir dosya Robocopy kullanılarak başka bir konuma kopyalanırsa, elde edilen dosya katmanlanmaz. Robocopy bu özniteliği kopyalama işlemlerine yanlışlıkla dahil ettiği için çevrimdışı özniteliği ayarlanmış olabilir.
- Robocopy kullanarak dosyaları kopyalarken dosya zaman damgalarını korumak için/MıR seçeneğini kullanın. Bu, eski dosyaların son erişilen dosyalardan daha önce katmanlı olmasını sağlayacaktır.
