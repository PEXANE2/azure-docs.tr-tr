---
title: Azure Dosya Eşitleme Bulut Katmanlama'yı Anlama | Microsoft Dokümanlar
description: Azure Dosya Eşitleme özelliği Bulut Katmanlama hakkında bilgi edinin
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 03/17/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: e8a8502b40410df221886cde2fa5f3db15bf3eed
ms.sourcegitcommit: 980c3d827cc0f25b94b1eb93fd3d9041f3593036
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80549171"
---
# <a name="cloud-tiering-overview"></a>Bulut Katmanlama Genel Bakış
Bulut katmanlama, sık erişilen dosyaların sunucuda yerel olarak önbelleğe alınıp diğer tüm dosyaların ilke ayarlarına göre Azure Dosyalarına katmanlandığı Azure Dosya Eşitlemi'nin isteğe bağlı bir özelliğidir. Bir dosya katmanlı olduğunda, Azure Dosya Eşitleme dosya sistemi filtresi (StorageSync.sys) dosyayı yerel olarak bir işaretçiyle veya telafi noktasıyla değiştirir. Reparse noktası, Azure Dosyaları'ndaki dosyanın URL'sini temsil eder. Katmanlı bir dosya, üçüncü taraf uygulamaların katmanlı dosyaları güvenli bir şekilde tanımlayabilmesi için NTFS'de ayarlanan hem "çevrimdışı" özniteliğine hem de FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS özniteliğine sahiptir.
 
Bir kullanıcı katmanlı bir dosya açtığında, Azure Dosya Eşitlemi, kullanıcının dosyanın Azure'da depolandığından bilmesine gerek kalmadan Azure Dosyaları'ndaki dosya verilerini sorunsuz bir şekilde çağırır. 
 
 > [!Important]  
 > Bulut katmanlama, Windows sistem hacminde desteklenmez.
    
 > [!Important]  
 > Katmanlı dosyaları geri çağırmak için ağ bant genişliği en az 1 Mbps olmalıdır. Ağ bant genişliği 1 Mbps'den azsa, dosyalar zaman aralığı hatasıyla geri çağırmada başarısız olabilir.

## <a name="cloud-tiering-faq"></a>Bulut Katmanlama SSS

<a id="afs-cloud-tiering"></a>
### <a name="how-does-cloud-tiering-work"></a>Bulut katmanlama nasıl çalışır?
Azure Dosya Eşitleme sistemi filtresi, her sunucu bitiş noktasında ad alanınızın bir "ısı haritasını" oluşturur. Zaman içinde erişilenleri (okuma ve yazma işlemlerini) izler ve erişimin hem sıklığına hem de recency'sine bağlı olarak her dosyaya bir ısı puanı atar. Yakın zamanda açılan sık erişilen bir dosya sıcak olarak kabul edilirken, zar zor dokunulan ve bir süredir erişilmeyen bir dosya serin olarak kabul edilecektir. Bir sunucudaki dosya hacmi belirlediğiniz birim boş alan eşiğini aştığında, boş alan yüzdeniz karşılanana kadar en havalı dosyaları Azure Dosyaları'na aktaracaktır.

Azure Dosya Eşitleme aracısının 4.0 ve üzerindeki sürümlerinde, her sunucu bitiş noktasında, erişilmeyen veya değiştirilmeyen dosyaları belirli bir gün içinde katmanlayacak bir tarih ilkesi de belirtebilirsiniz.

<a id="tiering-minimum-file-size"></a>
### <a name="what-is-the-minimum-file-size-for-a-file-to-tier"></a>Bir dosyanın katmana en küçük dosya boyutu nedir?
Aracı sürümleri 9.x ve daha yeni için, bir dosyanın katmana en küçük dosya boyutu dosya sistemi küme boyutunu temel a (dosya sistemi küme boyutunu iki katına) temel aşar. Örneğin, NTFS dosya sistemi küme boyutu 4KB ise, bir dosyanın katmana gelen minimum dosya boyutu 8KB'dir. Aracı sürümleri 8.x ve daha büyük için, bir dosyanın katmana en küçük dosya boyutu 64KB'dir.

<a id="afs-volume-free-space"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work"></a>Birim boş alan katmanlama ilkesi nasıl çalışır?
Birim boş alan, sunucu bitiş noktasının bulunduğu birimde ayırmak istediğiniz boş alan miktarıdır. Örneğin, bir sunucu bitiş noktası olan bir birimde birim boş alan %20'ye ayarlanmışsa, birim alanının %80'e kadarı en son erişilen dosyalar tarafından işgal edilir ve kalan dosyalar Azure'a katmanlanır. Birim boş alan, tek tek dizinler veya eşitleme grupları düzeyinde değil, birim düzeyinde geçerlidir. 

<a id="volume-free-space-fastdr"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work-with-regards-to-new-server-endpoints"></a>Birim boş alanı katmanlama ilkesi yeni sunucu uç noktaları bakımından nasıl çalışır?
Bir sunucu bitiş noktası yeni olarak sağlandığında ve bir Azure dosya paylaşımına bağlandığında, sunucu önce ad alanını aşağı çeker ve ardından ses serbest alanı eşiğine gelene kadar gerçek dosyaları aşağı çeker. Bu işlem, hızlı olağanüstü durum kurtarma veya hızlı ad alanı geri yükleme olarak da bilinir.

<a id="afs-effective-vfs"></a>
### <a name="how-is-volume-free-space-interpreted-when-i-have-multiple-server-endpoints-on-a-volume"></a>Bir birim üzerinde birden fazla sunucu uç noktası bulunduğunda birimin boş alanı nasıl belirleniyor?
Bir birimde birden fazla sunucu bitiş noktası olduğunda, etkin hacimli boş alan eşiği, o birimdeki herhangi bir sunucu bitiş noktasında belirtilen en büyük birim boş alandır. Dosyalar, ait oldukları sunucu bitiş noktasına bakılmaksızın kullanım şekillerine göre katmanlanır. Örneğin, bir birimde iki sunucu bitiş noktası varsa, Endpoint1 ve Endpoint2'de %25'lik bir birim boş alan eşiği varsa ve Endpoint2 'de %50'lik bir birim boş alan eşiği varsa, her iki sunucu bitiş noktası için birim boş alan eşiği %50 olacaktır. 

<a id="date-tiering-policy"></a>
### <a name="how-does-the-date-tiering-policy-work-in-conjunction-with-the-volume-free-space-tiering-policy"></a>Tarih katmanlama ilkesi birim boş alanı katmanlama ilkesi ile birlikte nasıl çalışır? 
Bir sunucu bitiş noktasında bulut katmanlamayı etkinleştirirken, bir birim boş alan ilkesi ayarlarsınız. Tarih ilkesi de dahil olmak üzere diğer ilkelerden her zaman önceliklidir. İsteğe bağlı olarak, o birimdeki her sunucu bitiş noktası için bir tarih ilkesi etkinleştirebilirsiniz. Bu ilke, yalnızca bu iipolitikasın açıkladığı gün aralığında erişilen (yani okunan veya yazılan) dosyaların yerel tutulacağını yönetir. Belirtilen gün sayısıyla erişilmeyen dosyalar katmanlanır. 

Bulut Katmanlama, hangi dosyaların katmanlandırılmak gerektiğini belirlemek için son erişim süresini kullanır. Bulut katmanlama filtresi sürücüsü (storagesync.sys) son erişim süresini izler ve bilgileri bulut katmanlama ısı deposunda kaydeder. Yerel bir PowerShell cmdlet kullanarak ısı deposu görebilirsiniz.

```powershell
Import-Module '<SyncAgentInstallPath>\StorageSync.Management.ServerCmdlets.dll'
Get-StorageSyncHeatStoreInformation '<LocalServerEndpointPath>'
```

> [!IMPORTANT]
> Son erişilen zaman damgası NTFS tarafından izlenen bir özellik değildir ve bu nedenle Dosya Gezgini'nde varsayılan olarak görünmez. Tarih ilkesinin beklendiği gibi çalışıp çalışmadığını denetlemek için dosyadaki son değiştirilmiş zaman damgasını kullanmayın. Bu zaman damgası sadece izler yazıyor, okumaz. Bu değerlendirme için son erişilen zaman damgasını almak için gösterilen cmdlet'i kullanın.

> [!WARNING]
> Dosya ve klasörler için son erişilen zaman damgasını izlemenin NTFS özelliğini açmayın. Büyük bir performans etkisi olduğundan, bu özellik varsayılan olarak kapalıdır. Azure Dosya Eşitlemesi, son erişilen süreleri otomatik olarak ve çok verimli bir şekilde izler ve bu NTFS özelliğinden yararlanmaz.

Birim boş alan ilkesinin her zaman öncelikli olduğunu ve birimde tarih ilkesinde açıklandığı kadar çok gün değerinde dosya tutacak yeterli boş alan olmadığında, Azure Dosya Eşitlemesi'nin en soğuk dosyaları, birim boş alan yüzdesi karşılanana kadar katmanlandırmaya devam edeceğini unutmayın.

Örneğin, 60 günlük bir tarih tabanlı katmanlama politikanız ve %20'lik bir hacim boş alan politikanız olduğunu varsa. Tarih ilkesini uyguladıktan sonra, birimde boş alanın %20'sinden azı varsa, birim boş alan ilkesi devreye girecek ve tarih ilkesini geçersiz kılar. Bu, sunucuda tutulan veri miktarının 60 günden 45 güne düşürülebileceği gibi daha fazla dosyanın katmanlanmasına neden olur. Tam tersine, bu ilke, boş alan eşiğinize çarpmamış olsanız bile zaman aralığınızın dışına çıkan dosyaların katmanlamasını zorlar , böylece 61 günlük bir dosya, ses düzeyiniz boş olsa bile katmanlanır.

<a id="volume-free-space-guidelines"></a>
### <a name="how-do-i-determine-the-appropriate-amount-of-volume-free-space"></a>Uygun birim boş alanı miktarını nasıl belirleyebilirim?
Yerel tutmanız gereken veri miktarı birkaç etkene göre belirlenir: bant genişliğiniz, veri kümenizin erişim deseni ve bütçeniz. Düşük bant genişliğine sahip bir bağlantınız varsa, kullanıcılarınız için en az gecikme olduğundan emin olmak için verilerinizin daha fazlasını yerel tutmak isteyebilirsiniz. Aksi takdirde, belirli bir dönemde çalkalama oranına dayandırabilirsiniz. Örneğin, 1 TB veri setinizin yaklaşık %10'unun değiştiğini veya her ay etkin olarak erişilebildiğini biliyorsanız, dosyaları sık sık geri çağırmamak için 100 GB'ı yerel tutmak isteyebilirsiniz. Hacminiz 2 TB ise, %5'ini (veya 100 GB) yerel kalmasını istersiniz, yani kalan %95'lik kısım ses serbest alan yüzdenizdir. Ancak, daha yüksek karmaşa dönemleri için hesaba bir arabellek eklemenizi öneririz – başka bir deyişle, daha düşük hacimli boş alan yüzdesi ile başlayan ve daha sonra gerekirse bunu ayarlama. 

Daha fazla verinin yerel tutulması, Azure'dan daha az dosya geri çağrılacağı için çıkış maliyetlerinin düşmesi anlamına gelir, ancak aynı zamanda kendi maliyetiyle gelen daha büyük miktarda şirket içi depolama alanı korumanızı gerektirir. Azure Dosya Eşitleme'nin bir örneğini dağıttıktan sonra, ses serbest alan ayarlarınızın kullanımınız için uygun olup olmadığını kabaca ölçmek için depolama hesabınızın çıkışına bakabilirsiniz. Depolama hesabının yalnızca Azure Dosya Eşitleme Bulut Bitiş Noktanızı (diğer bir şekilde eşitleme payınız) içerdiğini varsayarsak, yüksek çıkış, buluttan birçok dosyanın geri çağrıldığı anlamına gelir ve yerel önbelleğinizi artırmayı düşünmelisiniz.

<a id="how-long-until-my-files-tier"></a>
### <a name="ive-added-a-new-server-endpoint-how-long-until-my-files-on-this-server-tier"></a>Yeni bir sunucu bitiş noktası ekledim. Bu sunucu katmanındaki dosyalarıma ne kadar kaldı?
Azure Dosya Eşitleme aracısının 4.0 ve üzerindeki sürümlerinde, dosyalarınız Azure dosya paylaşımına yüklendikten sonra, bir sonraki katmanlama oturumu çalışır yayınlanmaz ve saatte bir gerçekleşen ilkelerinize göre katmanlanır. Eski ajanlarda katmanlamanın gerçekleşmesi 24 saat kadar sürebilir.

<a id="is-my-file-tiered"></a>
### <a name="how-can-i-tell-whether-a-file-has-been-tiered"></a>Bir dosyanın katmanlı olup olmadığını nasıl anlayabilirim?
Bir dosyanın Azure dosya paylaşımınıza katmanlandırılıp katmanlandırılmadığını denetlemenin birkaç yolu vardır:
    
   *  **Dosyadaki dosya özniteliklerini denetleyin.**
     Bir dosyaya sağ tıklayın, **Ayrıntılar'a**gidin ve ardından **Öznitelikler** özelliğine gidin. Katmanlı bir dosyada aşağıdaki öznitelikler kümesi vardır:     
        
        | Öznitelik harfi | Öznitelik | Tanım |
        |:----------------:|-----------|------------|
        | A | Arşiv | Dosyanın yedekleme yazılımı tarafından yedeklemesi gerektiğini gösterir. Bu öznitelik, dosyanın katmanlı olup olmadığına veya diskte tam olarak depolanıp depolanmadığına bakılmaksızın her zaman ayarlanır. |
        | P | Seyrek dosya | Dosyanın seyrek bir dosya olduğunu gösterir. Seyrek dosya, NTFS'nin disk akışındaki dosya çoğunlukla boşolduğunda verimli kullanım için sunduğu özel bir dosya türüdür. Azure Dosya Eşitlemi, bir dosya tamamen katmanlı veya kısmen geri çağrıldığı için seyrek dosyalar kullanır. Tam katmanlı bir dosyada, dosya akışı bulutta depolanır. Kısmen geri çağrılan bir dosyada, dosyanın bu bölümü zaten disktedir. Bir dosya diske tamamen geri çağrıldıysa, Azure Dosya Eşitlemi dosyayı seyrek bir dosyadan normal bir dosyaya dönüştürür. Bu öznitelik yalnızca Windows Server 2016 ve daha eski olarak ayarlanır.|
        | M | Veri erişimini geri çağırma | Dosyaverilerinin yerel depolama da tam olarak bulunmadığını gösterir. Dosyayı okumak, dosya içeriğinin en azından bir kısmının sunucu bitiş noktasının bağlı olduğu bir Azure dosya paylaşımından alınmasına neden olur. Bu öznitelik yalnızca Windows Server 2019'da ayarlanır. |
        | L | Yeniden ayrıştırma noktası | Dosyanın bir telafi noktası olduğunu gösterir. Reparse noktası, dosya sistemi filtresi tarafından kullanılmak üzere özel bir işaretçidir. Azure Dosya Eşitlemi, dosyanın depolandığı bulut konumunu Azure Dosya Eşitleme dosya sistemi filtresine (StorageSync.sys) tanımlamak için telafi noktaları kullanır. Bu, kesintisiz erişimi destekler. Kullanıcıların Azure Dosya Eşitlemi'nin kullanıldığını veya Azure dosya paylaşımınızdaki dosyaya nasıl erişeceklerini bilmeleri gerekmez. Bir dosya tamamen geri çağrıldığında, Azure Dosya Eşitlemi dosyadan telafi noktasını kaldırır. |
        | O | Çevrimdışı | Dosyanın içeriğinin bir kısmının veya tamamının diskte depolanmadığını gösterir. Bir dosya tamamen geri çağrıldığında, Azure Dosya Eşitlemi bu özelliği kaldırır. |

        ![Ayrıntılar sekmesi seçili bir dosya için Özellikler iletişim kutusu](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Dosya Gezgini'nin tablo ekranına **Öznitelikler** alanını ekleyerek bir klasördeki tüm dosyaların özniteliklerini görebilirsiniz. Bunu yapmak için, varolan bir sütuna (örneğin, **Boyut),** **Daha fazla**seçeneğini ve ardından açılan listeden **Öznitelikler'i** sağ tıklatın.
        
   * **Dosyadaki reparse puanlarını denetlemek için kullanın. `fsutil`**
       Önceki seçenekte açıklandığı gibi, katmanlı bir dosyanın her zaman bir reparse puan kümesi vardır. Reparse işaretçisi, Azure Dosya Eşitleme dosya sistemi filtresi (StorageSync.sys) için özel bir işaretçidir. Bir dosyanın bir reparse noktası olup olmadığını denetlemek için, yükseltilmiş komut `fsutil` istemi veya PowerShell penceresinde, yardımcı programı çalıştırın:
    
        ```powershell
        fsutil reparsepoint query <your-file-name>
        ```

        Dosyanın bir telafi noktası varsa, **Reparse Etiket Değeri: 0x8000001e**görmek bekleyebilirsiniz. Bu hexadecimal değer, Azure Dosya Eşitlemi'ne ait olan telafi noktası değeridir. Çıktı, Azure dosya paylaşımınızda dosyanıza giden yolu temsil eden telafi verilerini de içerir.

        > [!WARNING]  
        > Yardımcı `fsutil reparsepoint` program komutu aynı zamanda bir telafi noktasını silme yeteneğine de sahiptir. Azure Dosya Eşitleme mühendislik ekibi sizden istediği sürece bu komutu çalıştırmayın. Bu komutun çalıştırılarak veri kaybına neden olabilir. 

<a id="afs-recall-file"></a>

### <a name="a-file-i-want-to-use-has-been-tiered-how-can-i-recall-the-file-to-disk-to-use-it-locally"></a>Kullanmak istediğim bir dosya katmanlı hale alındı. Yerel olarak kullanmak için dosyayı diske nasıl geri çağırabilirim?
Bir dosyayı diske çağırmanın en kolay yolu dosyayı açmaktır. Azure Dosya Eşitleme dosya sistemi filtresi (StorageSync.sys), dosyayı azure dosya paylaşımınızdan sorunsuz bir şekilde indirir. Multimedya veya .zip dosyaları gibi kısmen okunabilen dosya türleri için, dosya açmak dosyanın tamamını karşıdan yüklemez.

PowerShell'i bir dosyanın geri çağrılması için de kullanabilirsiniz. Bu seçenek, bir klasördeki tüm dosyalar gibi aynı anda birden çok dosyayı geri çağırmak istiyorsanız yararlı olabilir. Azure Dosya Eşitlemi'nin yüklü olduğu sunucu düğümüne bir PowerShell oturumu açın ve ardından aşağıdaki PowerShell komutlarını çalıştırın:
    
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```
İsteğe bağlı parametreler:
* `-Order CloudTieringPolicy`önce en son değiştirilen dosyaları hatırlayacaktır.  
* `-ThreadCount`paralel olarak kaç dosyanın geri çağrılabileceğini belirler.
* `-PerFileRetryCount`şu anda engellenen bir dosyanın ne sıklıkta geri çağırma girişiminde bulunulacağını belirler.
* `-PerFileRetryDelaySeconds`denemelerini geri çağırmak için yeniden deneme arasındaki saniye cinsinden süreyi belirler ve her zaman önceki parametre ile birlikte kullanılmalıdır.

> [!Note]  
> Sunucuyu barındıran yerel birim, katmanlı tüm verileri geri çağırmak için `Invoke-StorageSyncFileRecall` yeterli boş alana sahip değilse, cmdlet başarısız olur.  

<a id="sizeondisk-versus-size"></a>
### <a name="why-doesnt-the-size-on-disk-property-for-a-file-match-the-size-property-after-using-azure-file-sync"></a>Bir dosyanın *disk özelliğindeki Boyut,* Azure Dosya Eşitlemesi'ni kullandıktan sonra neden *Boyut* özelliğiyle eşleşmiyor? 
Windows Dosya Gezgini, bir dosyanın boyutunu temsil edecek iki özelliği ortaya çıkarır: Diskteki **Boyut** ve **Boyut.** Bu özellikler anlam olarak kurnazca farklılık gösterir. **Boyut,** dosyanın tam boyutunu temsil eder. **Diskteki boyut,** diskte depolanan dosya akışının boyutunu temsil eder. Bu özelliklerin değerleri sıkıştırma, Veri Çoğaltma kullanımı veya Azure Dosya Eşitlemi ile bulut katmanlama gibi çeşitli nedenlerle farklılık görebilir. Dosya akışı diskte değil, Azure dosya paylaşımınızda depolandığı için, dosya bir Azure dosya paylaşımına katmanlanmışsa, diskteki boyut sıfırdır. Bir dosyanın kısmen katmanlandırılabilir (veya kısmen geri çağrılması) da mümkündür. Kısmen katmanlı bir dosyada, dosyanın bir bölümü disktedir. Bu, dosyalar multimedya oynatıcılar veya zip yardımcı programları gibi uygulamalar tarafından kısmen okunduğunda oluşabilir. 

<a id="afs-force-tiering"></a>
### <a name="how-do-i-force-a-file-or-directory-to-be-tiered"></a>Bir dosyayı veya dizini katmanlı olmaya nasıl zorlayabilirim?
Bulut katmanlama özelliği etkinleştirildiğinde, bulut katmanlama dosyaları son erişime göre otomatik olarak katmanlar ve bulut bitiş noktasında belirtilen birim boş alan yüzdesini elde etmek için süreleri değiştirir. Ancak bazen, bir dosyayı el ile katmana zorlamak isteyebilirsiniz. Bu, uzun süre tekrar kullanmak istemediğiniz büyük bir dosyayı kaydederseniz ve artık biriminizdeki boş alanın diğer dosya ve klasörler için kullanılmasını istiyorsanız yararlı olabilir. Aşağıdaki PowerShell komutlarını kullanarak katmanlamayı zorlayabilirsiniz:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
```

<a id="afs-image-thumbnail"></a>
### <a name="why-are-my-tiered-files-not-showing-thumbnails-or-previews-in-windows-explorer"></a>Katmanlı dosyalarım neden Windows Gezgini'nde küçük resimleri veya önizlemeleri göstermiyor?
Katmanlı dosyalar için küçük resimler ve önizlemeler sunucu bitiş noktanızda görünmez. Windows'daki küçük resim önbelleği özelliği çevrimdışı öznitelik ile okuma dosyalarını kasıtlı olarak atladığından, bu davranış bekleniyor. Bulut Katmanlama etkinleştirildiğinde, katmanlı dosyalar üzerinden okumak bu dosyaların karşıdan yüklenmelerine (geri çağrılmasını) neden olur.

Bu davranış Azure Dosya Eşitlemesi için özel değildir, Windows Gezgini çevrimdışı öznitelik ayarlanmış olan tüm dosyalar için bir "gri X" görüntüler. SMB üzerinden dosyalara erişirken X simgesini görürsünüz. Bu davranışın ayrıntılı bir açıklaması için,[https://blogs.msdn.microsoft.com/oldnewthing/20170503-00/?p=96105](https://blogs.msdn.microsoft.com/oldnewthing/20170503-00/?p=96105)


## <a name="next-steps"></a>Sonraki Adımlar
* [Azure Dosya Eşitleme Dağıtımı Planlama](storage-sync-files-planning.md)
