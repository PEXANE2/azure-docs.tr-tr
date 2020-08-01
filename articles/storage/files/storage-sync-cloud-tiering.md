---
title: Azure Dosya Eşitleme bulut katmanlamayı anlama | Microsoft Docs
description: Azure Dosya Eşitleme Özellik bulutu katmanlama hakkında bilgi edinin
author: roygara
ms.service: storage
ms.topic: conceptual
ms.date: 06/15/2020
ms.author: rogarana
ms.subservice: files
ms.openlocfilehash: 74887e6ee4656091aa647b481bc406dcc23b9c12
ms.sourcegitcommit: f988fc0f13266cea6e86ce618f2b511ce69bbb96
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/31/2020
ms.locfileid: "87460091"
---
# <a name="cloud-tiering-overview"></a>Bulut katmanlaması genel bakış
Bulut katmanlaması, sık erişilen dosyaların sunucu üzerinde yerel olarak önbelleğe alındığı, diğer tüm dosyaların ilke ayarlarına bağlı olarak Azure dosyaları ile katmanlandıkları Azure Dosya Eşitleme isteğe bağlı bir özelliğidir. Bir dosya katmanlı olduğunda, Azure Dosya Eşitleme dosya sistemi filtresi (StorageSync.sys) dosyayı bir işaretçi veya yeniden ayrıştırma noktasıyla yerel olarak değiştirir. Yeniden ayrıştırma noktası, Azure dosyalarındaki dosyanın bir URL 'sini temsil eder. Katmanlı bir dosyanın hem "çevrimdışı" özniteliği hem de FILE_ATTRIBUTE_RECALL_ON_DATA_ACCESS özniteliği bulunur. böylece, üçüncü taraf uygulamaların katmanlı dosyaları güvenle belirleyebilmesini sağlayabilirsiniz.
 
Bir Kullanıcı katmanlı bir dosya açtığında, kullanıcının dosyanın Azure 'da depolandığını bilmeleri gerekmeden Azure dosyalarından dosya verilerini sorunsuzca geri çeker Azure Dosya Eşitleme. 
 
 > [!Important]  
 > Bulut katmanlaması, Windows sistem biriminde desteklenmez.
    
 > [!Important]  
 > Katmanlı dosyaları geri çağırmak için ağ bant genişliği en az 1 MB/sn olmalıdır. Ağ bant genişliği 1 MB/sn 'den küçükse, dosyalar bir zaman aşımı hatasıyla geri çekemeyebilir.

## <a name="cloud-tiering-faq"></a>Bulut katmanlama hakkında SSS

<a id="afs-cloud-tiering"></a>
### <a name="how-does-cloud-tiering-work"></a>Bulut katmanlaması nasıl çalışır?
Azure Dosya Eşitleme sistem filtresi, her sunucu uç noktasında ad alanınız için bir "heatmap" oluşturur. Zaman içinde erişimleri (okuma ve yazma işlemleri) izler, sonra da erişim sıklığı ve, her ikisine de göre her dosyaya bir ısı puanı atar. En son açılan bir dosya sık erişilen olarak kabul edilir, ancak bir süre önce dokunulmayan ve erişilmeyen bir dosya seyrek olarak değerlendirilir. Bir sunucudaki dosya birimi, ayarladığınız birimde boş alan eşiğini aşarsa, boş alan yüzdesi karşılanana kadar, en fazla dosyaları Azure dosyalarına katman olarak alır.

Ayrıca, kullanılabilir yerel depolama kapasitesine bakılmaksızın, belirtilen gün sayısı içinde erişilmeyen tüm dosyaları barındıracak her sunucu uç noktasında bir tarih ilkesi belirtebilirsiniz. Bu sunucu uç noktasındaki dosyaların belirli bir yaşın ötesinde yerel olarak korunması gerekmediğini biliyorsanız, yerel disk alanını öngörülebilir bir şekilde serbest bırakmak iyi bir seçimdir. Bu, dosyalarını daha fazla önbelleğe almak için aynı birimdeki diğer uç noktalar için değerli yerel disk kapasitesini serbest bırakır.

Bulut katmanlama ısı haritasını, aslında eşitlenen ve bulut katmanlaması etkinleştirilmiş bir konumda olan tüm dosyaların sıralı bir listesidir. Bu heatmap içindeki tek bir dosyanın göreli konumunu öğrenmek için, sistem şu zaman damgalarının her birini (Bu sırada) en fazla (son erişim saati, son değiştirilme saati, oluşturma saati) kullanır. Genellikle, son erişim zamanı izlenir ve kullanılabilir. Ancak, yeni bir sunucu uç noktası oluşturulduğunda, bulut katmanlaması etkinken, bu durumda başlangıçta dosya erişimini gözlemlemek için yeterli süre geçti. Son erişim zamanı yokluğunda, heatmap 'teki göreli konumu değerlendirmek için son değiştirilme zamanı kullanılır. Aynı geri dönüş, tarih ilkesi için geçerlidir. Son erişim zamanı olmadan tarih ilkesi, son değiştirilme zamanına göre hareket eder. Bu durumda, bir dosyanın oluşturma saatine geri dönecektir. Zaman içinde, sistem, ağırlıklı 'e daha fazla ve daha fazla dosya erişim isteği gözlemleyecek ve özete otomatik olarak izlenen son erişim zamanını kullanacaktır.

Bulut katmanlaması, son erişim süresini izlemek için NTFS özelliğine bağlı değildir. Bu NTFS özelliği varsayılan olarak kapalıdır ve performans konuları nedeniyle bu özelliği el ile etkinleştirmenizi önermiyoruz. Bulut katmanlaması, son erişim süresini ayrı ve çok verimli bir şekilde izler.

<a id="tiering-minimum-file-size"></a>
### <a name="what-is-the-minimum-file-size-for-a-file-to-tier"></a>Katmana bir dosya için en küçük dosya boyutu nedir?

Aracı sürümleri 9 ve daha yeni sürümlerde, bir dosyanın katmana en küçük dosya boyutu dosya sistemi kümesi boyutunu temel alır. Bulut katmanlaması için uygun olan minimum dosya boyutu, 2x küme boyutu ve en az 8 KB ile hesaplanır. Aşağıdaki tabloda, birim kümesi boyutuna bağlı olarak katmanlanılabilecek minimum dosya boyutları gösterilmektedir:

|Birim kümesi boyutu (bayt) |Bu boyut veya daha büyük dosyalar katmanlı olabilir  |
|----------------------------|---------|
|4 KB veya daha küçük (4096)      | 8 KB    |
|8 KB (8192)                 | 16 KB   |
|16 KB (16384)               | 32 KB   |
|32 KB (32768)               | 64 KB   |
|64 KB (65536)               | 128 KB  |

Windows Server 2019 ve Azure Dosya Eşitleme Agent sürümü 12 ve daha yeni sürümlerde, 2 MB 'a kadar olan küme boyutları da desteklenir ve bu büyük küme boyutları üzerinde katmanlama aynı şekilde çalışır. Daha eski işletim sistemi veya Aracı sürümleri, 64 KB 'a kadar küme boyutlarını destekler.

Windows tarafından kullanılan tüm dosya sistemleri, sabit diskinizi küme boyutuna (ayırma birimi boyutu olarak da bilinir) göre düzenleyin. Küme boyutu bir dosyayı tutmak için kullanılabilecek en küçük disk alanı miktarını temsil eder. Dosya boyutları küme boyutunun hatta daha fazla olmadığında, dosyayı küme boyutunun bir sonraki katı olarak tutmak için ek alan kullanılmalıdır.

Azure Dosya Eşitleme, Windows Server 2012 R2 ve daha yeni bir sürümü olan NTFS birimlerinde desteklenir. Aşağıdaki tabloda yeni bir NTFS birimi oluşturduğunuzda varsayılan küme boyutları açıklanmaktadır. 

|Birim boyutu    |Windows Server 2012R2 ve üzeri |
|---------------|---------------|
|7 MB – 16 TB   | 4 KB          |
|16 TB – 32 TB   | 8 KB          |
|32TB – 64 TB   | 16 KB         |
|64TB – 128 TB  | 32 KB         |
|128TB – 256 TB | 64 KB         |
|> 256 TB       | Desteklenmez |

Birim oluşturulduktan sonra, birimi farklı bir küme boyutuyla el ile biçimlendirmiş olursunuz. Biriminiz Windows 'un eski bir sürümünden farklıysa, varsayılan küme boyutları da farklı olabilir. [Bu makalede varsayılan küme boyutları hakkında daha ayrıntılı bilgi bulunur.](https://support.microsoft.com/help/140365/default-cluster-size-for-ntfs-fat-and-exfat) 4 KB 'tan daha küçük bir küme boyutu seçseniz bile, katmanlı olabilecek en küçük dosya boyutu olarak 8 KB 'lik bir sınır de geçerlidir. (Teknik açıdan 2x küme boyutu 8 KB 'tan daha az bir değer de olabilir.)

Mutlak minimumın nedeni, NTFS 'nin son derece küçük dosyaları-1 KB-4 KB boyutlu dosyaları depoladığı şekilde bulunur. Birimin diğer parametrelerine bağlı olarak, küçük dosyaların diskte bulunan bir kümede depolanması mümkündür. Bu tür dosyaları doğrudan birimin ana dosya tablosunda veya "MFT kaydında" depolamak daha verimli olabilir. Bulut katmanlama yeniden ayrıştırma noktası her zaman diskte depolanır ve tam olarak bir küme alır. Bu küçük dosyaların katmanlaması, hiçbir boşluk tasarrufları olmadan sona geçirebilir. Olağanüstü durumlar, bulut katmanlaması etkinken daha fazla alan kullanmaya da daha fazla zaman açamaz. Bu şekilde korumak için, bulut katmanlaması katman olacak bir dosyanın en küçük boyutu 4 KB veya daha küçük bir küme boyutunda 8 KB 'tır.

<a id="afs-volume-free-space"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work"></a>Birim boş alan katmanlama ilkesi nasıl çalışır?
Birim boş alanı, bir sunucu uç noktasının bulunduğu birimde ayırmak istediğiniz boş alan miktarıdır. Örneğin, birim boş alanı bir sunucu uç noktası olan bir birimde %20 ' ye ayarlandıysa, birim alanının %80 ' e kadar, en son erişilen dosyalar tarafından Azure 'a kadar katmanlı olan bu alana sığmayan diğer dosyalar ile birlikte yer alır. Birim boş alanı, tek tek dizinlerin veya eşitleme gruplarının düzeyinde değil, birim düzeyinde geçerlidir. 

<a id="volume-free-space-fastdr"></a>
### <a name="how-does-the-volume-free-space-tiering-policy-work-with-regards-to-new-server-endpoints"></a>Birim boş alanı katmanlama ilkesi yeni sunucu uç noktaları bakımından nasıl çalışır?
Sunucu uç noktası yeni sağlandığında ve bir Azure dosya paylaşımında bağlandığında, sunucu önce ad alanını çeker ve ardından birim boş alanı eşiğini bulana kadar gerçek dosyaları çeker. Bu işlem, hızlı olağanüstü durum kurtarma veya hızlı ad alanı geri yükleme olarak da bilinir.

<a id="afs-effective-vfs"></a>
### <a name="how-is-volume-free-space-interpreted-when-i-have-multiple-server-endpoints-on-a-volume"></a>Bir birim üzerinde birden fazla sunucu uç noktası bulunduğunda birimin boş alanı nasıl belirleniyor?
Bir birimde birden fazla sunucu uç noktası olduğunda, etkin birimde boş alan eşiği, söz konusu birimdeki herhangi bir sunucu uç noktasında belirtilen en büyük birim boş alanıdır. Dosyalar, ait oldukları sunucu uç noktasından bağımsız olarak kullanım düzenlerine göre katmanlanacaktır. Örneğin, bir birimde iki sunucu uç noktası varsa, Endpoint1 ve Endpoint2 ' de bir birimde boş alan eşiğinin %25 ' i ve Endpoint2% 50 ' lik bir birim boş alan eşiğine sahip olduğunda, her iki sunucu uç noktası için birim boş alan eşiği %50 olacaktır. 

<a id="date-tiering-policy"></a>
### <a name="how-does-the-date-tiering-policy-work-in-conjunction-with-the-volume-free-space-tiering-policy"></a>Tarih katmanlama ilkesi birim boş alanı katmanlama ilkesi ile birlikte nasıl çalışır? 
Sunucu uç noktasında bulut katmanlamayı etkinleştirirken bir birimde boş alan ilkesi ayarlarsınız. Tarih ilkesi de dahil olmak üzere her zaman diğer ilkelere göre önceliklidir. İsteğe bağlı olarak, söz konusu birimdeki her bir sunucu uç noktası için bir tarih ilkesi etkinleştirebilirsiniz. Bu ilke, bu ilkenin açıkladığı gün aralığı içinde yalnızca erişilen (yani, okunan veya yazılan) dosyaların yerel olarak saklanacağını yönetir. Belirtilen gün sayısıyla erişilmeyen dosyalar katmanlı olacaktır. 

Bulut katmanlaması, hangi dosyaların katmanlı olacağını belirleyen son erişim süresini kullanır. Bulut katmanlama filtresi sürücüsü (storagesync.sys) son erişim süresini izler ve bulut katmanlama ısı deposundaki bilgileri günlüğe kaydeder. Yerel bir PowerShell cmdlet 'i kullanarak ısı mağazasını görebilirsiniz.

```powershell
Import-Module '<SyncAgentInstallPath>\StorageSync.Management.ServerCmdlets.dll'
Get-StorageSyncHeatStoreInformation '<LocalServerEndpointPath>'
```

> [!IMPORTANT]
> Son erişilen zaman damgası NTFS tarafından izlenen bir özellik değildir ve bu nedenle dosya Gezgini 'nde varsayılan olarak görünmez. Tarih-ilkenin beklendiği gibi çalışıp çalışmadığını denetlemek için bir dosyada son değiştirme zaman damgasını kullanmayın. Bu zaman damgası yalnızca yazmaları izler, okuma değil. Bu değerlendirme için son erişilen zaman damgasını almak üzere gösterilen cmdlet 'i kullanın.

> [!WARNING]
> Dosya ve klasörler için son erişilen zaman damgasına yönelik NTFS özelliğini açık duruma getirin. Büyük bir performans etkisi olduğundan bu özellik varsayılan olarak kapalıdır. Azure Dosya Eşitleme, son erişilen zamanları otomatik olarak ve çok verimli bir şekilde izler ve bu NTFS özelliğini kullanmaz.

Birim boş alanı ilkesinin her zaman öncelikli olduğunu ve birimde, tarih ilkesi tarafından açıklandığı şekilde çok sayıda gün olması için yeterli boş alan olmadığında Azure Dosya Eşitleme, birim boş alan yüzdesi karşılanana kadar en fazla gün sayısı ile devam eder.

Örneğin, 60 günlük bir tarih tabanlı katmanlama ilkeniz ve %20 ' lik bir birim boş alan ilkesi olduğunu varsayalım. Tarih ilkesini uyguladıktan sonra birimde boş alan %20 ' den daha az yer alır, birim boş alan ilkesi başlatılır ve Tarih ilkesini geçersiz kılar. Bu, daha fazla dosyanın katmanlı olmasını sağlar. bu şekilde, sunucuda tutulan verilerin miktarı 60 günlük veriler 45 güne azalabilir. Buna karşılık, bu ilke boş alan eşiğine ulaşamasanız bile zaman aralığınızı aşacak olan dosyaların katmanlamasını zorlar. bu nedenle, biriminiz boş olsa bile 61 gün öncesine sahip bir dosya katmanlanacaktır.

<a id="volume-free-space-guidelines"></a>
### <a name="how-do-i-determine-the-appropriate-amount-of-volume-free-space"></a>Uygun birim boş alanı miktarını nasıl belirleyebilirim?
Yerel tutmanız gereken veri miktarı bazı etkenlere göre belirlenir: bant genişliğiniz, veri kümenizin erişim deseninin ve bütçeniz. Düşük bant genişliğine sahip bir bağlantınız varsa, kullanıcılarınız için en az gecikme olduğundan emin olmak için verilerinizin yerel kalmasını sağlamak isteyebilirsiniz. Aksi takdirde, belirli bir süre boyunca dalgalanma oranını temel alabilirsiniz. Örneğin, 1 TB 'lık veri kümenizin yaklaşık %10 ' u değişiklik olduğunu veya her ay etkin bir şekilde erişildiğini biliyorsanız, dosyaları sık geri çekmediğinizde 100 GB yerel tutmak isteyebilirsiniz. Biriminiz 2TB ise, %5 ' i (veya 100 GB) yerel tutmak isteyeceksiniz, yani kalan %95, birim boş alanınız yüzdesidir. Ancak, daha yüksek değişim dönemleri için hesaba bir arabellek eklemenizi öneririz. diğer bir deyişle, daha düşük bir birimde boş alan yüzdesi ile başlayıp daha sonra gerekirse bu değeri ayarlayabilirsiniz. 

Verilerin yerel olarak tutulması, Azure 'dan daha az sayıda dosya geri çekeceği ve aynı zamanda kendi maliyetiyle birlikte gelen daha büyük miktarda şirket içi depolama alanı tutmanızı gerektiren çıkış maliyetlerinin düşük olmasını sağlar. Azure Dosya Eşitleme dağıtılan bir örneğe sahip olduktan sonra, birim boş alan ayarlarınızın kullanımınız için uygun olup olmadığını kabaca ölçmek üzere depolama hesabınızın çıkış bölümüne bakabilirsiniz. Depolama hesabının yalnızca Azure Dosya Eşitleme bulut uç noktasını (yani, eşitleme paylaşımınızı) içerdiğini varsayarak, yüksek çıkış, buluttan çok sayıda dosyanın geri çağrılmakta olduğu ve yerel önbelleğinizi artırmayı göz önünde bulundurmanız gereken anlamına gelir.

<a id="how-long-until-my-files-tier"></a>
### <a name="ive-added-a-new-server-endpoint-how-long-until-my-files-on-this-server-tier"></a>Yeni bir sunucu uç noktası ekledim. Bu sunucu katmanında dosyalarımı ne kadar süreyle kullanıyorsunuz?

Dosyaların Küme başına katmanlı olması gerekip gerekmediğini belirtir, saat bir kez değerlendirilir. Yeni bir sunucu uç noktası oluşturulduğunda iki durum oluşabilir:

1. Yeni bir sunucu uç noktası eklediğinizde, bu sunucu konumunda genellikle dosyalar bulunur. Bulut katmanlaması başlamadan önce önce bunların karşıya yüklenmesi gerekir. Tüm dosyaların ilk karşıya yüklemesi tamamlanana kadar birim boş alan ilkesi çalışmaya başlamacaktır. Ancak, bir dosya karşıya yüklendikten hemen sonra isteğe bağlı tarih ilkesi tek bir dosya temelinde çalışmaya başlar. Bir saatlik Aralık burada da geçerlidir. 
2. Yeni bir sunucu uç noktası eklediğinizde, içindeki verilerle birlikte boş bir sunucu konumunu bir Azure dosya paylaşımında bağlamanız mümkündür. Bunun ister ikinci bir sunucu için ister bir olağanüstü durum kurtarma durumunda olsun. Sunucunuza ilk indirme sırasında ad alanını indirmeyi ve içeriği geri çekmeyi seçerseniz, ad alanı kapatıldıktan sonra dosyalar son değiştirme zaman damgasına göre geri alınacaktır. Birim boş alan ilkesi ve isteğe bağlı tarih ilkesi içine sığacak şekilde, yalnızca birçok dosya geri çekilir.

<a id="is-my-file-tiered"></a>
### <a name="how-can-i-tell-whether-a-file-has-been-tiered"></a>Bir dosyanın katmanlı olup olmadığını nasıl anlayabilirim?
Bir dosyanın Azure dosya paylaşımınıza katmanlı olup olmadığını denetlemek için çeşitli yollar vardır:
    
   *  **Dosyadaki dosya özniteliklerini denetleyin.**
     Dosyaya sağ tıklayın, **Ayrıntılar**' a gidin ve ardından **öznitelikler** özelliğine kaydırın. Katmanlı bir dosya aşağıdaki öznitelikler kümesine sahiptir:     
        
        | Öznitelik harfi | Öznitelik | Tanım |
        |:----------------:|-----------|------------|
        | A | Arşiv | Dosyanın yedekleme yazılımı tarafından yedeklenmesi gerektiğini gösterir. Bu öznitelik, dosyanın katmanlı veya tam disk üzerinde depolanmadığına bakılmaksızın her zaman ayarlanır. |
        | P | Seyrek dosya | Dosyanın seyrek bir dosya olduğunu gösterir. Seyrek dosya, NTFS 'nin disk akışındaki dosya genellikle boş olduğunda verimli kullanım için sunduğu özelleşmiş bir dosya türüdür. Azure Dosya Eşitleme, bir dosya tamamen katmanlı veya kısmen geri çekilmiş olduğundan seyrek dosyalar kullanır. Tam katmanlı bir dosyada, dosya akışı bulutta depolanır. Kısmen geri çekilmiş bir dosyada, dosyanın o bölümü zaten disk üzerinde. Bir dosya diske tamamen geri çekilir Azure Dosya Eşitleme, dosyayı seyrek bir dosyadan normal bir dosyaya dönüştürür. Bu öznitelik yalnızca Windows Server 2016 ve üzeri sürümlerde ayarlanır.|
        | M | Veri erişimi üzerinde geri çek | Dosyanın verilerinin yerel depolamada tam olarak mevcut olmadığını gösterir. Dosyayı okumak, dosya içeriğinin en az bir kısmının sunucu uç noktasının bağlı olduğu bir Azure dosya paylaşımından alınmasına neden olur. Bu öznitelik yalnızca Windows Server 2019 ' de ayarlanır. |
        | L | Yeniden ayrıştırma noktası | Dosyanın bir yeniden ayrıştırma noktası olduğunu gösterir. Yeniden ayrıştırma noktası, bir dosya sistemi filtresi tarafından kullanılmak üzere özel bir işaretçisidir. Azure Dosya Eşitleme, dosyanın depolandığı bulut konumunu Azure Dosya Eşitleme dosya sistemi filtresine (StorageSync.sys) tanımlamak için yeniden ayrıştırma noktaları kullanır. Bu, sorunsuz erişimi destekler. Kullanıcıların Azure Dosya Eşitleme kullanıldığını veya Azure dosya paylaşımınızda dosyaya nasıl erişim alınacağını bilmeleri gerekmez. Bir dosya tamamen geri çekilir Azure Dosya Eşitleme, yeniden ayrıştırma noktasını dosyadan kaldırır. |
        | O | Çevrimdışı | Dosyanın içeriğinin bir kısmının veya tümünün diskte depolanmadığını belirtir. Bir dosya tamamen geri çekilir Azure Dosya Eşitleme, bu özniteliği kaldırır. |

        ![Ayrıntılar sekmesi seçiliyken bir dosyanın özellikler iletişim kutusu](media/storage-files-faq/azure-file-sync-file-attributes.png)
        
        Dosya Gezgini 'nin tablo görüntüsüne **öznitelikler** alanını ekleyerek bir klasördeki tüm dosyaların özniteliklerini görebilirsiniz. Bunu yapmak için var olan bir sütuna (örneğin, **Boyut**) sağ tıklayın, **daha fazla**' yı seçin ve ardından açılan listeden **öznitelikler** ' i seçin.
        
   * **`fsutil`Bir dosyadaki yeniden ayrıştırma noktalarını denetlemek için kullanın.**
       Önceki seçenekte açıklandığı gibi, katmanlı bir dosya her zaman bir yeniden ayrıştırma noktası kümesine sahiptir. Yeniden ayrıştırma işaretçisi, Azure Dosya Eşitleme dosya sistemi filtresi (StorageSync.sys) için özel bir işaretçisidir. Bir dosyanın yeniden ayrıştırma noktasına sahip olup olmadığını denetlemek için, yükseltilmiş bir komut Istemi veya PowerShell penceresinde, `fsutil` yardımcı programını çalıştırın:
    
        ```powershell
        fsutil reparsepoint query <your-file-name>
        ```

        Dosyada bir yeniden ayrıştırma noktası varsa, **yeniden ayrıştırma etiketi değerini görmeyi bekleyebilir: 0x8000001E**. Bu onaltılık değer Azure Dosya Eşitleme sahip olan yeniden ayrıştırma noktası değeridir. Çıktı Ayrıca, Azure dosya paylaşımınızda dosyanızın yolunu temsil eden yeniden ayrıştırma verilerini içerir.

        > [!WARNING]  
        > `fsutil reparsepoint`Yardımcı program komutunun aynı zamanda bir yeniden ayrıştırma noktasını silme yeteneği de vardır. Azure Dosya Eşitleme mühendislik ekibi sizi sormadığı müddetçe bu komutu yürütün. Bu komutun çalıştırılması, veri kaybına neden olabilir. 

<a id="afs-recall-file"></a>
### <a name="a-file-i-want-to-use-has-been-tiered-how-can-i-recall-the-file-to-disk-to-use-it-locally"></a>Kullanmak istediğim bir dosya katmanlı. Dosyayı yerel olarak kullanmak için diske nasıl geri çekirim?
Dosyayı diske geri çekmenin en kolay yolu dosyayı açmak. Azure Dosya Eşitleme dosya sistemi filtresi (StorageSync.sys), sizin bölüminizden herhangi bir iş yapmadan dosyayı Azure dosya paylaşımınızdan sorunsuz bir şekilde indirir. Multimedya veya. zip dosyaları gibi kısmen okunabilecek dosya türleri için bir dosyayı açmak dosyanın tamamını indirmez.

Ayrıca, bir dosyanın geri çağrılamasını zorlamak için PowerShell kullanabilirsiniz. Bu seçenek, bir klasördeki tüm dosyalar gibi birden çok dosyayı aynı anda geri çağırmak istiyorsanız yararlı olabilir. Azure Dosya Eşitleme yüklendiği sunucu düğümünde bir PowerShell oturumu açın ve ardından aşağıdaki PowerShell komutlarını çalıştırın:
    
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint>
```
İsteğe bağlı parametreler:
* `-Order CloudTieringPolicy`en son değiştirilen veya erişilen dosyaları ilk olarak çağırır ve geçerli katmanlama ilkesi tarafından izin verilir. 
    * Birim boş alan ilkesi yapılandırılmışsa, birimde boş alan ilkesi ayarına ulaşılana kadar dosyalar geri alınacaktır. Örneğin, birim ücretsiz ilke ayarı %20 ise, birim boş alanı %20 ' ye ulaştıktan sonra geri çekme durdurulur.  
    * Birim boş alanı ve tarih ilkesi yapılandırılmışsa, birimde boş alan veya tarih ilkesi ayarına ulaşılana kadar dosyalar geri alınacaktır. Örneğin, birim ücretsiz ilke ayarı %20 ise ve tarih ilkesi 7 gün ise, birim boş alanı %20 ' ye ulaşırsa veya 7 gün içinde erişilen veya değiştirilen tüm dosyalar yereldir.
* `-ThreadCount`paralel olarak kaç dosya geri çağrılabileceğini belirler.
* `-PerFileRetryCount`Şu anda engellenen bir dosya için bir geri çekmenin ne sıklıkta denenmeyeceğini belirler.
* `-PerFileRetryDelaySeconds`yeniden çağırma denemeleri arasındaki saniye cinsinden süreyi belirler ve bir önceki parametreyle birlikte her zaman kullanılmalıdır.

Örnek:
```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncFileRecall -Path <path-to-to-your-server-endpoint> -ThreadCount 8 -Order CloudTieringPolicy -PerFileRetryCount 3 -PerFileRetryDelaySeconds 10
``` 

> [!Note]  
> - Invoke-StorageSyncFileRecall cmdlet 'i, var olan bir eşitleme grubuna yeni bir sunucu uç noktası eklerken dosya indirme performansını artırmak için de kullanılabilir.  
>- Sunucuyu barındıran yerel birimde, tüm katmanlı verileri yeniden çağırmak için yeterli boş alan yoksa, `Invoke-StorageSyncFileRecall` cmdlet başarısız olur.  

<a id="sizeondisk-versus-size"></a>
### <a name="why-doesnt-the-size-on-disk-property-for-a-file-match-the-size-property-after-using-azure-file-sync"></a>Azure Dosya Eşitleme, bir dosyanın *disk özelliği üzerindeki boyutu* neden bir dosya için *Boyut* özelliği ile eşleşmiyor? 
Windows Dosya Gezgini, bir dosyanın boyutunu temsil etmek için iki özellik **sunar: Diskteki boyut ve** **Boyut**. Bu özellikler, anlamı daha da farklıdır. **Boyut** , dosyanın tamamının boyutunu temsil eder. **Disk üzerindeki boyut** , diskte depolanan dosya akışının boyutunu temsil eder. Bu özelliklerin değerleri, sıkıştırma, yinelenen verileri kaldırma veya Azure Dosya Eşitleme ile bulut katmanlama gibi çeşitli nedenlerle farklılık gösterebilir. Bir dosya Azure dosya paylaşımında katmanlanırsa, dosya akışı diskte değil, Azure dosya paylaşımınızda depolandığından, diskteki boyut sıfırdır. Ayrıca, bir dosyanın kısmen katmanlı (veya kısmen geri çekilmiş) olması da mümkündür. Kısmen katmanlı bir dosyada, dosyanın bir kısmı diskte bulunur. Bu durum, dosyalar, multimedya oynatıcılar veya ZIP yardımcı programları gibi uygulamalar tarafından kısmen okunarak ortaya çıkabilir. 

<a id="afs-force-tiering"></a>
### <a name="how-do-i-force-a-file-or-directory-to-be-tiered"></a>Nasıl yaparım? bir dosyanın veya dizinin katmanlanmasını zorlıyor musunuz?

> [!NOTE]
> Katmanlı olacak bir dizin seçtiğinizde, yalnızca şu anda dizindeki dosyalar katmanlı olur. Bu saatten sonra oluşturulan dosyalar otomatik olarak katmanlanmazlar.

Bulut katmanlama özelliği etkinleştirildiğinde, bulut katmanlaması, dosyaları son erişime göre otomatik olarak katmanlarını ve bulut uç noktasında belirtilen birim boş alan yüzdesine ulaşmak için zaman sayısını değiştirir. Bazen bir dosyayı katmana el ile zorlamak isteyebilirsiniz. Bu, uzun bir süre için yeniden kullanmayı düşünmediğiniz büyük bir dosyayı kaydedip daha sonra biriminizdeki boş alanın diğer dosya ve klasörler için kullanmasını istediğiniz durumlarda yararlı olabilir. Aşağıdaki PowerShell komutlarını kullanarak, katmanlamayı zorlayabilirsiniz:

```powershell
Import-Module "C:\Program Files\Azure\StorageSyncAgent\StorageSync.Management.ServerCmdlets.dll"
Invoke-StorageSyncCloudTiering -Path <file-or-directory-to-be-tiered>
```

<a id="afs-image-thumbnail"></a>
### <a name="why-are-my-tiered-files-not-showing-thumbnails-or-previews-in-windows-explorer"></a>Katmanlı dosyalar neden Windows Gezgini 'nde küçük resimleri veya önizlemeleri gösterilmiyor?
Katmanlı dosyalar için, küçük resimler ve önizlemeler sunucu uç noktanıza görünmez. Bu davranış, Windows 'daki küçük resim önbelleği özelliği, çevrimdışı özniteliği olan dosyaların okunmasına göre atlanmasından bu yana beklenmektedir. Bulut katmanlama özelliği etkinken katmanlı dosyalar aracılığıyla okuma, bunların indirilememesine (geri çekilir) neden olur.

Bu davranış Azure Dosya Eşitleme özgü değildir, Windows Gezgini, çevrimdışı özniteliği ayarlanmış herhangi bir dosya için "gri X" görüntüler. SMB üzerinden dosyalara erişirken X simgesini görürsünüz. Bu davranışın ayrıntılı bir açıklaması için bkz.[https://blogs.msdn.microsoft.com/oldnewthing/20170503-00/?p=96105](https://blogs.msdn.microsoft.com/oldnewthing/20170503-00/?p=96105)

<a id="afs-tiering-disabled"></a>
### <a name="i-have-cloud-tiering-disabled-why-are-there-tiered-files-in-the-server-endpoint-location"></a>Bulut katmanlama devre dışı, neden sunucu uç noktası konumunda katmanlı dosyalar var?

Katmanlı dosyaların sunucu uç noktası konumunda bulunabilmesinin iki nedeni vardır:

- Var olan bir eşitleme grubuna yeni bir sunucu uç noktası eklerken, meta veriler öncelikle sunucuya eşitlenir ve dosyalar daha sonra arka planda sunucuya indirilir. Dosyalar, yerel olarak indirilene kadar katmanlı olarak görünür. Bir eşitleme grubuna yeni bir sunucu eklerken dosya indirme performansını artırmak için [Invoke-StorageSyncFileRecall](storage-sync-cloud-tiering.md#afs-recall-file) cmdlet 'ini kullanın.

- Bulut katmanlaması sunucu uç noktasında etkinleştirildiyse ve devre dışı bırakılmışsa, dosyalar erişilene kadar katmanlı olarak kalır.


## <a name="next-steps"></a>Sonraki Adımlar
* [Azure Dosya Eşitleme dağıtımı için planlama yapma](storage-sync-files-planning.md)
