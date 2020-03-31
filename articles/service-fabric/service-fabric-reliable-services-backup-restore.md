---
title: Service Fabric Yedekleme ve Geri Yükleme
description: Güvenilir Stateful hizmetlerinin ve Güvenilir Aktörlerin yedeklemesini yapılandırmak için bir hizmet olan Service Fabric Backup and Restore için kavramsal dokümantasyon.
author: mcoskun
ms.topic: conceptual
ms.date: 10/29/2018
ms.author: mcoskun
ms.openlocfilehash: ac6bb14517b67a4b308460583e8c9fb99a2df9f0
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75922787"
---
# <a name="backup-and-restore-reliable-services-and-reliable-actors"></a>Güvenilir Hizmetleri ve Güvenilir Aktörleri Yedekleme ve Geri Yükleme
Azure Hizmet Kumaşı, bu yüksek kullanılabilirliği korumak için durumu birden çok düğüm üzerinde çoğaltır anımsatan yüksek kullanılabilirlik platformudur.  Böylece, kümedeki bir düğüm başarısız olsa bile, hizmetler kullanılabilir olmaya devam eder. Platform tarafından sağlanan bu dahili fazlalık bazıları için yeterli olsa da, bazı durumlarda hizmetin verileri yedeklemesi (harici bir depoya) istenir.

> [!NOTE]
> Veri kaybı senaryolarından kurtarabilmeniz için verilerinizi yedeklemek ve geri yüklemek (ve beklendiği gibi çalıştığını test etmek) çok önemlidir.
> 

> [!NOTE]
> Microsoft, Güvenilir Durum hizmetleri ve Güvenilir Aktörler'in veri yedeklemesini yapılandırmak için Periyodik yedekleme yi kullanmanızı [ve geri yüklemenizi](service-fabric-backuprestoreservice-quickstart-azurecluster.md) önerir. 
> 


Örneğin, bir hizmet aşağıdaki senaryolardan korumak için verileri yedeklemek isteyebilir:

- Tüm Service Fabric kümesinin kalıcı kaybı durumunda.
- Bir hizmet bölümünün yinelemelerinin çoğunluğunun kalıcı kaybı
- Durum yanlışlıkla silinir veya bozuk alır yönetim hataları. Örneğin, yeterli ayrıcalığa sahip bir yönetici hizmeti hatalı bir şekilde silerse, bu durum oluşabilir.
- Hizmette veri bozulmasına neden olan hatalar. Örneğin, bir hizmet kodu yükseltmesi güvenilir bir koleksiyona hatalı veri yazmaya başladığında bu durum oluşabilir. Böyle bir durumda, hem kod hem de veri önceki bir duruma döndürülebilir.
- Çevrimdışı veri işleme. Verileri oluşturan hizmetten ayrı olarak gerçekleşen iş zekası için verilerin çevrimdışı işlenmesi uygun olabilir.

Yedekleme/Geri Yükleme özelliği, Güvenilir Hizmetler API'si üzerine inşa edilmiş hizmetlerin yedekleme oluşturmasına ve geri yüklemesine olanak tanır. Platform tarafından sağlanan yedekleme API'leri, okuma veya yazma işlemlerini engellemeden bir hizmet bölümü durumunun yedek(ler) izin verir. Geri yükleme API'leri, bir hizmet bölümü durumunun seçilen bir yedeklemeden geri yüklenmesini sağlar.

## <a name="types-of-backup"></a>Yedekleme Türleri
İki yedekleme seçeneği vardır: Tam ve Artımlı.
Tam yedekleme, yinelemenin durumunu yeniden oluşturmak için gereken tüm verileri içeren bir yedeklemedir: denetim noktaları ve tüm günlük kayıtları.
Denetim noktaları ve günlük olduğundan, tam bir yedekleme kendisi tarafından geri yüklenebilir.

Tam yedeklemelerle ilgili sorun, denetim noktaları büyük olduğunda ortaya çıkar.
Örneğin, 16 GB durumu olan bir yineleme, yaklaşık 16 GB'a kadar ekleyen denetim noktalarına sahip olacaktır.
Eğer beş dakikalık bir Kurtarma Noktası Hedefimiz varsa, kopyanın her beş dakikada bir yedeklenmeleri gerekir.
Her yedeklenince, 50 MB 'a ek olarak 16 GB kontrol `CheckpointThresholdInMB`noktasını kopyalaması gerekir (kullanılarak yapılandırılabilir) günlükler.

![Tam Yedekleme Örneği.](media/service-fabric-reliable-services-backup-restore/FullBackupExample.PNG)

Bu sorunun çözümü, yedeklemenin yalnızca son yedeklemeden bu yana değiştirilen günlük kayıtlarını içerdiği artımlı yedeklemelerdir.

![Artımlı Yedekleme Örneği.](media/service-fabric-reliable-services-backup-restore/IncrementalBackupExample.PNG)

Son yedekleme (denetim noktalarını içermez) olduğundan, artımlı yedeklemeler yalnızca değişiklikler olduğundan, daha hızlı olma eğilimindedirler, ancak kendi başlarına geri yüklenemezler.
Artımlı yedeklemeyi geri yüklemek için tüm yedekleme zincirinin olması gerekir.
Yedekleme zinciri, tam yedeklemeyle başlayan ve ardından bitişik artımlı yedeklemeler içeren bir yedekleme zinciridir.

## <a name="backup-reliable-services"></a>Yedekleme Güvenilir Hizmetler
Hizmet yazarı, yedeklemelerin ne zaman yapılacağı ve yedeklemelerin nerede depolanacağı hakkında tam denetime sahiptir.

Yedekleme başlatmak için, hizmetin devralınan üye `BackupAsync`işlevi çağırması gerekir.  
Yedeklemeler yalnızca birincil yinelemelerden yapılabilir ve yazma durumunun verilmesini gerektirir.

Aşağıda gösterildiği `BackupAsync` gibi, `BackupDescription` yedekleme klasörü yerel olarak oluşturulduğunda çağrılan ve bazı dış depolama `Func<< BackupInfo, CancellationToken, Task<bool>>>` alanına taşınmaya hazır olan tam veya artımlı yedeklemenin yanı sıra bir geri arama işlevi belirtilebilen bir nesneyi alır.

```csharp

BackupDescription myBackupDescription = new BackupDescription(BackupOption.Incremental,this.BackupCallbackAsync);

await this.BackupAsync(myBackupDescription);

```

Artımlı yedekleme alma isteği `FabricMissingFullBackupException`ile başarısız olabilir. Bu özel durum, aşağıdaki şeylerden birinin gerçekleştiğini gösterir:

- çoğaltma birincil haline gelmiştir bu yana tam bir yedekleme almamış,
- son yedekleme kesildiberi bazı günlük kayıtları veya
- çoğaltma `MaxAccumulatedBackupLogSizeInMB` sınırı geçti.

Kullanıcılar, yapılandırarak `MinLogSizeInMB` artımlı yedekleme yapabilme olasılığını artırabilir `TruncationThresholdFactor`veya .
Bu değerleri artırmak çoğaltma diski başına kullanımını artırır.
Daha fazla bilgi için Bkz. [Güvenilir Hizmetler Yapılandırması](service-fabric-reliable-services-configuration.md)

`BackupInfo`çalışma zamanı nın yedeklemeyi kaydettiği klasörün konumu da dahil olmak`BackupInfo.Directory`üzere yedeklemeyle ilgili bilgiler sağlar ( ). Geri arama işlevi harici `BackupInfo.Directory` bir mağazaya veya başka bir konuma taşıyabilir.  Bu işlev, yedekleme klasörünü hedef konumuna başarıyla taşıyıp taşıyamayacağını gösteren bir bool da döndürür.

Aşağıdaki kod, yedeklemeyi `BackupCallbackAsync` Azure Depolama'ya yüklemek için yöntemin nasıl kullanılabileceğini gösterir:

```csharp
private async Task<bool> BackupCallbackAsync(BackupInfo backupInfo, CancellationToken cancellationToken)
{
    var backupId = Guid.NewGuid();

    await externalBackupStore.UploadBackupFolderAsync(backupInfo.Directory, backupId, cancellationToken);

    return true;
}
```

Önceki örnekte, `ExternalBackupStore` Azure Blob depolama ile arabirim için kullanılan `UploadBackupFolderAsync` örnek sınıfdır ve klasörü sıkıştırAn ve Azure Blob deposuna yerleştiren yöntemdir.

Şunlara dikkat edin:

  - Herhangi bir zamanda çoğaltma başına yalnızca bir yedekleme işlemi olabilir. Aynı anda birden `BackupAsync` fazla `FabricBackupInProgressException` arama, uçak içi yedeklemeleri bir ile sınırlandırmak için atar.
  - Yedekleme devam ederken yineleme üzerinde başarısız olursa, yedekleme tamamlanmamış olabilir. Bu nedenle, başarısızlık sona erdiğinde, gerektiğinde çağırarak `BackupAsync` yedeklemeyi yeniden başlatmak hizmetin sorumluluğundadır.

## <a name="restore-reliable-services"></a>Güvenilir Hizmetleri Geri Yükleme
Genel olarak, geri yükleme işlemi gerçekleştirmeniz gerekebilecek durumlar aşağıdaki kategorilerden birine girer:

  - Hizmet bölümü veri kaybetti. Örneğin, bir bölüm için üç yinelemeden ikisinin diski (birincil yineleme dahil) bozuk veya silinir. Yeni birincil bir yedekleme verileri geri yüklemek gerekebilir.
  - Tüm hizmet kayboldu. Örneğin, bir yönetici tüm hizmeti kaldırır ve böylece hizmet ve verilerin geri yüklenmesi gerekir.
  - Hizmet bozuk uygulama verilerini çoğaltmış (örneğin, bir uygulama hatası nedeniyle). Bu durumda, hizmetin bozulma nedenini kaldırmak için yükseltilmesi veya geri alınması ve bozulmayan verilerin geri yüklenmesi gerekir.

Birçok yaklaşım mümkün olsa da, yukarıdaki `RestoreAsync` senaryolardan kurtarmak için kullanma hakkında bazı örnekler sunuyoruz.

## <a name="partition-data-loss-in-reliable-services"></a>Güvenilir Hizmetlerde bölüm veri kaybı
Bu durumda, çalışma zamanı veri kaybını otomatik olarak `OnDataLossAsync` algılar ve API'yi çağırır.

Hizmet yazarıkurtarmak için aşağıdakileri gerçekleştirmesi gerekir:

  - Sanal taban sınıf yöntemini `OnDataLossAsync`geçersiz kıl.
  - Hizmetin yedeklerini içeren dış konumda en son yedeklemeyi bulun.
  - En son yedeklemeyi indirin (ve sıkıştırılmışsa yedeklemeyi yedekleme klasörüne geri çekin).
  - Yöntem `OnDataLossAsync` bir `RestoreContext`. Sağlanan `RestoreContext` `RestoreAsync` API'yi arayın.
  - Restorasyon başarılı olsaydı doğru dön.

Aşağıda yöntemin bir `OnDataLossAsync` örnek uygulaması verilmiştir:

```csharp
protected override async Task<bool> OnDataLossAsync(RestoreContext restoreCtx, CancellationToken cancellationToken)
{
    var backupFolder = await this.externalBackupStore.DownloadLastBackupAsync(cancellationToken);

    var restoreDescription = new RestoreDescription(backupFolder);

    await restoreCtx.RestoreAsync(restoreDescription);

    return true;
}
```

`RestoreDescription``RestoreContext.RestoreAsync` çağrıya geçirilen bir üye `BackupFolderPath`denir.
Tek bir tam yedeklemegeri, `BackupFolderPath` bu tam yedekleme içeren klasörün yerel yolu için ayarlanmalıdır.
Tam yedekleme ve bir dizi artımlı yedekleme geri `BackupFolderPath` alırken, yalnızca tam yedekleme yi değil, aynı zamanda tüm artımlı yedeklemeleri de içeren klasörün yerel yoluna ayarlanmalıdır.
`RestoreAsync`sağlanan tam `FabricMissingFullBackupException` bir `BackupFolderPath` yedekleme içermiyorsa arama atılabilir.
Ayrıca, artan `ArgumentException` `BackupFolderPath` yedeklemelerden oluşan kırık bir zincir varsa da atabilir.
Örneğin, tam yedekleme, ilk artımlı ve üçüncü artımlı yedekleme içeriyorsa, ancak ikinci artımlı yedekleme yok.

> [!NOTE]
> Restore İlke'si varsayılan olarak Güvenli olarak ayarlanır.  Bu, yedekleme `RestoreAsync` klasöründe bu yinelemede bulunan durumdan daha eski veya eşit bir durum içerdiğini algılarsa, ApI'nin Bağımsız Durum ile başarısız olacağı anlamına gelir.  `RestorePolicy.Force`bu güvenlik kontrolünü atlamak için kullanılabilir. Bu, `RestoreDescription`bir parçası olarak belirtilir.
> 

## <a name="deleted-or-lost-service"></a>Silinmiş veya kaybolan hizmet
Bir hizmet kaldırılırsa, verilerin geri yüklenmeden önce hizmeti yeniden oluşturmanız gerekir.  Verilerin sorunsuz bir şekilde geri yüklenabilmesi için hizmeti aynı yapılandırmaya (örneğin, bölümleme düzeniyle) oluşturmak önemlidir.  Hizmet dolduktan sonra, verileri geri yüklemek`OnDataLossAsync` için API (üstte) bu hizmetin her bölümüne çağrılmalıdır. Bunu başarmanın bir yolu her bölümde [FabricClient.TestManagementClient.StartPartitionDataLossAsync](https://msdn.microsoft.com/library/mt693569.aspx) kullanarak.  

Bu noktadan itibaren, uygulama yukarıdaki senaryo ile aynıdır. Her bölümün dış mağazadan en son ilgili yedeklemeyi geri yüklemesi gerekir. Bir uyarı, çalışma zamanı dinamik olarak bölüm kimlikleri oluşturduğundan, bölüm kimliği artık değişmiş olabilir. Bu nedenle, hizmetin her bölüm için geri yüklemek için doğru en son yedeklemeyi belirlemek için uygun bölüm bilgilerini ve hizmet adını depolaması gerekir.

> [!NOTE]
> Küme durumunuz bozulabileceğinden, tüm hizmeti geri yüklemek için her bölümde kullanılması `FabricClient.ServiceManager.InvokeDataLossAsync` önerilmez.
> 

## <a name="replication-of-corrupt-application-data"></a>Bozuk uygulama verilerinin çoğaltılması
Yeni dağıtılan uygulama yükseltmesi bir hata varsa, bu veri bozulmasına neden olabilir. Örneğin, bir uygulama yükseltmesi, güvenilir bir sözlükteki her telefon numarası kaydını geçersiz bir alan koduyla güncelleştirmeye başlayabilir.  Bu durumda, Hizmet Kumaşı depolanan verilerin doğasından haberdar olmadığından geçersiz telefon numaraları çoğaltılır.

Veri bozulmasına neden olan böyle korkunç bir hatayı algıladıktan sonra yapmanız gereken ilk şey, hizmeti uygulama düzeyinde dondurmak ve mümkünse hata olmayan uygulama kodu sürümüne yükseltmektir.  Ancak, hizmet kodu düzeltildikten sonra bile, veriler yine de bozuk olabilir ve bu nedenle verilerin geri yüklenmesi gerekebilir.  Bu gibi durumlarda, en son yedeklemeler de bozuk olabileceğinden, en son yedeklemeyi geri yüklemek yeterli olmayabilir.  Bu nedenle, veriler bozulmadan önce yapılan son yedeklemeyi bulmanız gerekir.

Hangi yedeklemelerin bozuk olduğundan emin değilseniz, yeni bir Hizmet Kumaşı kümesi ni dağıtabilir ve yukarıdaki "Silinmiş veya kaybolan hizmet" senaryosunda olduğu gibi etkilenen bölümlerin yedeklerini geri yükleyebilirsiniz.  Her bölüm için yedekleri en sondan en azına geri vermeye başlayın. Bozulmaya sahip olmayan bir yedekleme bulduğunuzda, bu bölümün daha yeni olan tüm yedeklemelerini (yedeklemeden daha fazla) taşıyın/silin. Her bölüm için bu işlemi yineleyin. Şimdi, `OnDataLossAsync` üretim kümesindeki bölüm çağrıldığında, dış depoda bulunan son yedekleme yukarıdaki işlem tarafından seçilen yedekleme olacaktır.

Şimdi, "Silinmiş veya kaybolan hizmet" bölümündeki adımlar, buggy kodu devleti bozmadan önce hizmetin durumunu devlete geri yüklemek için kullanılabilir.

Şunlara dikkat edin:

  - Geri yüklediğinizde, geri yüklenen yedeklemenin veriler kaybolmadan önce bölümün durumundan daha eski olma olasılığı vardır. Bu nedenle, mümkün olduğunca çok veri kurtarmak için yalnızca son çare olarak geri yüklemeniz gerekir.
  - Yedekleme klasörü yolunu ve yedek klasörün içindeki dosyaların yollarını temsil eden dize, FabricDataRoot yoluna ve Uygulama Türü adının uzunluğuna bağlı olarak 255 karakterden büyük olabilir. Bu, bazı .NET yöntemlerinin `Directory.Move` `PathTooLongException` , örneğin, özel durum atamasını sağlayabilir. Bir geçici çözüm doğrudan kernel32 API'ler, gibi `CopyFile`aramaktır.

## <a name="back-up-and-restore-reliable-actors"></a>Güvenilir Aktörleri yedekle ve geri yükleyin


Güvenilir Aktörler Çerçevesi, Güvenilir Hizmetler üzerine kurulmuştur. Aktör(ler) barındıran ActorService, devlete güvenilir bir hizmettir. Bu nedenle, Güvenilir Hizmetler'de bulunan tüm yedekleme ve geri yükleme işlevleri Güvenilir Aktörler tarafından da kullanılabilir (devlet sağlayıcıya özgü davranışlar hariç). Yedeklemeler bölüm başına alınacağı için, bu bölümdeki tüm aktörlerin durumları yedeklenir (ve geri yükleme benzerdir ve bölüm başına olarak gerçekleşir). Yedekleme/geri yükleme gerçekleştirmek için, hizmet sahibinin ActorService sınıfından türetilen özel bir aktör hizmet sınıfı oluşturması ve daha sonra önceki bölümlerde açıklandığı gibi Güvenilir Hizmetlere benzer yedekleme/geri yükleme yapması gerekir.

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo)
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

Özel bir aktör hizmet sınıfı oluşturduğunuzda, aktörü kaydederken bunu da kaydetmeniz gerekir.

```csharp
ActorRuntime.RegisterActorAsync<MyActor>(
    (context, typeInfo) => new MyCustomActorService(context, typeInfo)).GetAwaiter().GetResult();
```

Güvenilir Aktörler için varsayılan `KvsActorStateProvider`durum sağlayıcısı. Artımlı yedekleme için `KvsActorStateProvider`varsayılan olarak etkin değildir. Oluşturucusundaki uygun ayarla `KvsActorStateProvider` oluşturarak ve ardından aşağıdaki kod snippet'inde gösterildiği gibi ActorService oluşturucuya geçirerek artımlı yedeklemeyi etkinleştirebilirsiniz:

```csharp
class MyCustomActorService : ActorService
{
    public MyCustomActorService(StatefulServiceContext context, ActorTypeInformation actorTypeInfo)
          : base(context, actorTypeInfo, null, null, new KvsActorStateProvider(true)) // Enable incremental backup
    {
    }
    
    //
    // Method overrides and other code.
    //
}
```

Artımlı yedekleme etkinleştirildikten sonra, artımlı yedekleme alarak aşağıdaki nedenlerden biri için FabricMissingFullBackupException ile başarısız olabilir ve artımlı yedekleme(ler almadan önce tam bir yedekleme almak gerekir):

  - Çoğaltma birincil olduğundan beri tam bir yedekleme almamış.
  - Son yedekleme alındığından beri bazı günlük kayıtları kesildi.

Artımlı yedekleme etkinleştirildiğinde, `KvsActorStateProvider` günlük kayıtlarını yönetmek için dairesel arabellek kullanmaz ve düzenli aralıklarla kırpılır. Kullanıcı tarafından 45 dakikalık bir süre için yedekleme yapılmazsa, sistem günlük kayıtlarını otomatik olarak kırdar. Bu aralık, oluşturucuolarak `logTruncationIntervalInMinutes` `KvsActorStateProvider` belirtilerek yapılandırılabilir (artımlı yedeklemeyi etkinleştirirken benzer). Birincil yinelemetüm verilerini göndererek başka bir yineleme oluşturmak gerekiyorsa günlük kayıtları da kesildi alabilirsiniz.

Güvenilir Hizmetlere benzer bir yedekleme zincirinden geri yükleme yaparken, BackupFolderPath tam yedekleme içeren bir alt dizin içeren alt dizinler ve artımlı yedekleme(ler) içeren diğer alt dizinler içermelidir. Geri yükleme API' si, yedekleme zinciri doğrulaması başarısız olursa, uygun hata iletisiyle FabricException'ı atar. 

> [!NOTE]
> `KvsActorStateProvider`şu anda Geri Yükleme Politikası.Güvenli seçeneğini yok sayar. Bu özellik için destek yaklaşan bir sürümde planlanmaktadır.
> 

## <a name="testing-back-up-and-restore"></a>Yedekleme ve Geri Yükleme Test
Kritik verilerin yedeklendiğini ve geri yüklenebileceğinden emin olmak önemlidir. Bu, PowerShell'de veri `Start-ServiceFabricPartitionDataLoss` yedekleme ve hizmetinizin işlevselliğini geri yüklemenin beklendiği gibi çalışıp çalışmadığını test etmek için belirli bir bölümde veri kaybına neden olabilecek cmdlet'i çağırarak yapılabilir.  Ayrıca programlı olarak veri kaybı çağırmak ve bu olaydan geri yüklemek de mümkündür.

> [!NOTE]
> GitHub'daki Web Başvuru Uygulaması'nda yedekleme ve yeniden yükleme işlevlerinin örnek bir uygulamasını bulabilirsiniz. Daha fazla `Inventory.Service` bilgi için lütfen servise bakın.
> 
> 

## <a name="under-the-hood-more-details-on-backup-and-restore"></a>Kaputun altında: yedekleme ve geri yükleme hakkında daha fazla bilgi
Yedekleme ve geri yükleme yle ilgili daha fazla ayrıntı yıkın.

### <a name="backup"></a>Backup
Güvenilir Durum Yöneticisi, okuma veya yazma işlemlerini engellemeden tutarlı yedeklemeler oluşturma olanağı sağlar. Bunu yapmak için, bir denetim noktası ve günlük kalıcılık mekanizması kullanır.  Güvenilir Durum Yöneticisi, işlem günlüğündeki baskıyı azaltmak ve kurtarma sürelerini iyileştirmek için belirli noktalarda bulanık (hafif) denetim noktaları alır.  Çağrıldığında, `BackupAsync` Güvenilir Durum Yöneticisi tüm Güvenilir nesnelere en son denetim noktası dosyalarını yerel bir yedekleme klasörüne kopyalamalarını bildirir.  Daha sonra, Güvenilir Durum Yöneticisi tüm günlük kayıtlarını kopyalar, "başlat işaretçisi"nden yedekleme klasörüne en son günlük kaydına kadar.  En son günlük kaydına kadar tüm günlük kayıtları yedeklemeye dahil olduğundan ve Güvenilir Durum Yöneticisi yazma işlemini koruduğundan, Güvenilir`CommitAsync` Durum Yöneticisi taahhüt edilen tüm işlemlerin (başarıyla döndürüldüğünü) yedeklemeye dahil edildiğini garanti eder.

Çağrıldıktan sonra `BackupAsync` yapılan herhangi bir hareket yedeklemede olabilir veya olmayabilir.  Yerel yedekleme klasörü platform tarafından doldurulur sonra (diğer bir süre, yerel yedekleme çalışma süresine göre tamamlanır), hizmetin yedek geri arama çağrılır.  Bu geri arama, yedekleme klasörünü Azure Depolama gibi harici bir konuma taşımaktan sorumludur.

### <a name="restore"></a>Geri Yükleme
Güvenilir Durum `RestoreAsync` Yöneticisi, API'yi kullanarak yedeklemeden geri yükleme olanağı sağlar.  
Üzerindeki `RestoreAsync` `RestoreContext` yöntem yalnızca `OnDataLossAsync` yöntem içinde çağrılabilir.
Döndürülen `OnDataLossAsync` bool, hizmetin durumunu harici bir kaynaktan geri yükleyip geri yüklemediğini gösterir.
`OnDataLossAsync` İadeler doğruysa, Hizmet Kumaşı bu birincildeki diğer tüm kopyaları yeniden bir arada alacaktır. Service Fabric, birincil role ilk `OnDataLossAsync` çağrı geçişi alacak ancak okuma veya yazma durumu verilmeyen yinelemelerin olmasını sağlar.
Bu, StatefulService uygulayıcıları `RunAsync` için, başarıyla `OnDataLossAsync` bitene kadar çağrılmayacak anlamına gelir.
Daha `OnDataLossAsync` sonra, yeni birincil çağrılacak.
Bir hizmet bu API'yi başarıyla tamamlayana (doğru veya yanlış döndürerek) ve ilgili yeniden yapılandırmayı bitirene kadar, API teker teker çağrılmadan devam eder.

`RestoreAsync`ilk olarak çağrıldığı birincil yinelemedeki varolan tüm durumu düşürür. Ardından Güvenilir Durum Yöneticisi, yedekleme klasöründe bulunan tüm Güvenilir nesneleri oluşturur. Daha sonra, Güvenilir nesnelerin yedekleme klasöründeki denetim noktalarından geri yüklemesi talimatı verilir. Son olarak, Güvenilir Durum Yöneticisi yedekleme klasöründeki günlük kayıtlarından kendi durumunu kurtarır ve kurtarma gerçekleştirir. Kurtarma işleminin bir parçası olarak, yedekleme klasöründe günlük kayıtlarını kaydeden "başlangıç noktasından" başlayan işlemler Güvenilir nesnelere yeniden oynandı. Bu adım, kurtarılan durum tutarlı olmasını sağlar.

## <a name="next-steps"></a>Sonraki adımlar
  - [Güvenilir Koleksiyonlar](service-fabric-work-with-reliable-collections.md)
  - [Güvenilir Hizmetler hızlı başlat](service-fabric-reliable-services-quick-start.md)
  - [Güvenilir Hizmetler bildirimleri](service-fabric-reliable-services-notifications.md)
  - [Güvenilir Hizmetler yapılandırması](service-fabric-reliable-services-configuration.md)
  - [Güvenilir Koleksiyonlar için geliştirici başvurusu](https://msdn.microsoft.com/library/azure/microsoft.servicefabric.data.collections.aspx)
  - [Azure Service Fabric’te düzenli yedekleme ve geri yükleme](service-fabric-backuprestoreservice-quickstart-azurecluster.md)

