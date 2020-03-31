---
title: Geliştirme ve test için Azure depolama emülatörü kullanın | Microsoft Dokümanlar
description: Azure depolama emülatörü, Azure Depolama uygulamalarınızı geliştirmek ve test etmek için ücretsiz bir yerel geliştirme ortamı sağlar.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 08/21/2019
ms.service: storage
ms.subservice: common
ms.topic: conceptual
ms.openlocfilehash: 1f13efeb5c2ebcb8b23dea6f9ae997039972f089
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72428307"
---
# <a name="use-the-azure-storage-emulator-for-development-and-testing"></a>Geliştirme ve sınama için Azure depolama emülatörü'ni kullanma

Microsoft Azure depolama emülatörü, Azure Blob, Kuyruk ve Tablo hizmetlerini yerel geliştirme amacıyla taklit eden bir araçtır. Bir Azure aboneliği oluşturmadan veya herhangi bir ücret ödemeden uygulamanızı yerel olarak depolama hizmetlerine karşı test edebilirsiniz. Uygulamanızın emülatörde nasıl çalıştığından memnun olduğunuzda, bulutta bir Azure depolama hesabı kullanmaya geçin.

## <a name="get-the-storage-emulator"></a>Depolama emülatörlerini alın

Depolama emülatörü Microsoft Azure [SDK'nın](https://azure.microsoft.com/downloads/)bir parçası olarak kullanılabilir. Ayrıca [bağımsız yükleyici](https://go.microsoft.com/fwlink/?linkid=717179&clcid=0x409) (doğrudan indirme) kullanarak depolama emülatörü yükleyebilirsiniz. Depolama emülatörü yüklemek için bilgisayarınızda yönetim ayrıcalıkları olması gerekir.

Depolama emülatörü şu anda yalnızca Windows'da çalışır. Linux için bir depolama emülatörü gerekiyorsa, bir seçenek topluluk muhafaza, açık kaynak depolama emülatörü [Azurite](https://github.com/azure/azurite)olduğunu.

> [!NOTE]
> Depolama emülatörünün bir sürümünde oluşturulan verilerin farklı bir sürüm kullanırken erişilebilir olacağı garanti edilmez. Verilerinizi uzun süre bekletmeniz gerekiyorsa, bu verileri depolama emülatörü yerine bir Azure depolama hesabında depolamanızı tavsiye ettik.
> 
> Depolama emülatörü, OData kitaplıklarının belirli sürümlerine bağlıdır. Depolama emülatörü tarafından kullanılan OData DL'lerinin diğer sürümlerle değiştirilmesi desteklenmez ve beklenmeyen davranışlara neden olabilir. Ancak, depolama hizmeti tarafından desteklenen OData'nın herhangi bir sürümü emülatöre istek göndermek için kullanılabilir.

## <a name="how-the-storage-emulator-works"></a>Depolama emülatörü nasıl çalışır?

Depolama emülatörü, Azure depolama hizmetlerini taklit etmek için yerel bir Microsoft SQL Server 2012 Express LocalDB örneğini kullanır. Depolama emülatörü, LocalDB örneği yerine sql server'ın yerel bir örneğine erişmek için yapılandırmayı seçebilirsiniz. Daha fazla bilgi edinmek için bu makalenin ilerleyen bölümlerinde [Başlangıç'a bakın ve depolama emülatörü](#start-and-initialize-the-storage-emulator) bölümünü başlatın.

Depolama emülatörü, Windows kimlik doğrulamasını kullanarak SQL Server veya LocalDB'ye bağlanır.

Depolama emülatörü ve Azure depolama hizmetleri arasında bazı işlevsellik farklılıkları vardır. Bu farklar hakkında daha fazla bilgi için, bu makalenin ilerleyen bölümlerinde [depolama emülatörü ve Azure Depolama](#differences-between-the-storage-emulator-and-azure-storage) bölümü arasındaki farklara bakın.

## <a name="start-and-initialize-the-storage-emulator"></a>Depolama emülatörlerini başlatın ve başlatın

Azure depolama emülatörü başlatmak için:

1. **Başlat** düğmesini seçin veya **Windows** tuşuna basın.
2. Yazmaya `Azure Storage Emulator`başlayın.
3. Görüntülenen uygulamalar listesinden emülatör seçin.

Depolama emülatörü başladığında, komut istemi penceresi görüntülenir. Depolama emülatörü başlatmak ve durdurmak için bu konsol penceresini kullanabilirsiniz. Ayrıca verileri temizleyebilir, durum alabilir ve komut isteminden emülatörün başlatılmasını da sağlayabilirsiniz. Daha fazla bilgi için bu makalenin ilerleyen bölümlerinde [Depolama emülatör komut satırı araç başvuru](#storage-emulator-command-line-tool-reference) bölümüne bakın.

> [!NOTE]
> Azurite gibi başka bir depolama emülatörü sistemde çalışıyorsa, Azure depolama emülatörü doğru şekilde çalışmayabilir.

Öykünücü çalıştırıldığında, Windows görev çubuğu bildirim alanında bir simge görürsünüz.

Depolama emülatör Komut Ustem penceresini kapattığınızda, depolama emülatörü çalışmaya devam edecektir. Depolama Emülatör konsolu penceresini yeniden açmak için, önceki adımları depolama emülatörü başlatıyormuş gibi izleyin.

Depolama emülatörü ilk çalıştırdığınızda, yerel depolama ortamı sizin için başharflere verilir. Başlatma işlemi LocalDB'de bir veritabanı oluşturur ve her yerel depolama hizmeti için HTTP bağlantı noktalarını ayırır.

Depolama emülatörü varsayılan olarak `C:\Program Files (x86)\Microsoft SDKs\Azure\Storage Emulator`.

> [!TIP]
> Yerel depolama emülatörü kaynaklarıyla çalışmak için [Microsoft Azure Depolama Gezgini'ni](https://storageexplorer.com) kullanabilirsiniz. Depolama Gezgini kaynakları ağacında & depolama gezgini kaynakları ağacında "(Emulator - Varsayılan Bağlantı Noktaları) (Anahtar)" (Anahtar)"yı yükleyip depo emülatörü kurduktan sonra arayın.
>

### <a name="initialize-the-storage-emulator-to-use-a-different-sql-database"></a>Farklı bir SQL veritabanı kullanmak için depolama emülatörü başlatma

Depolama emülatörü başlatmayı kullanarak varsayılan LocalDB örneği dışındaki bir SQL veritabanı örneğini işaret etmek için depolama emülatör komut satırı aracını kullanabilirsiniz:

1. Başlangıç'ta açıklandığı gibi Depolama Emülatör konsolu penceresini açın [ve depolama emülatörü bölümünü başlatın.](#start-and-initialize-the-storage-emulator)
1. Konsol penceresinde, SQL Server örneğinin `<SQLServerInstance>` adı aşağıdaki komutu yazın. LocalDB'yi kullanmak `(localdb)\MSSQLLocalDb` için SQL Server örneği olarak belirtin.

   `AzureStorageEmulator.exe init /server <SQLServerInstance>`

   Emülatöre varsayılan SQL Server örneğini kullanmaya yönlendiren aşağıdaki komutu da kullanabilirsiniz:

   `AzureStorageEmulator.exe init /server .`

   Veya veritabanını varsayılan LocalDB örneğine yeniden başlayan aşağıdaki komutu kullanabilirsiniz:

   `AzureStorageEmulator.exe init /forceCreate`

Bu komutlar hakkında daha fazla bilgi için Bkz. [Depolama emülatör komut satırı araç başvurusu.](#storage-emulator-command-line-tool-reference)

> [!TIP]
> LocalDB yüklemesi de dahil olmak üzere SQL Server örneklerinizi yönetmek için [Microsoft SQL Server Management Studio'yu](/sql/ssms/download-sql-server-management-studio-ssms) (SSMS) kullanabilirsiniz. SMSS **Connect to Server** iletişim `(localdb)\MSSQLLocalDb` kutusunda, Sunucu adını **belirtin:** LocalDB örneğine bağlanmak için alan.

## <a name="authenticating-requests-against-the-storage-emulator"></a>Depolama emülatöre karşı istekleri doğrulama

Depolama emülatörunu yükledikten ve başlattıktan sonra, kodunuzu buna karşı sınayabilirsiniz. Anonim bir istek olmadığı sürece, depolama emülatörüne karşı yaptığınız her istek onaylanmalıdır. Paylaşılan Anahtar kimlik doğrulaması kullanarak veya paylaşılan erişim imzası (SAS) kullanarak depolama emülatörüne karşı istekleri yetkilendirebilirsiniz.

### <a name="authorize-with-shared-key-credentials"></a>Paylaşılan Anahtar kimlik bilgileriyle yetkilendirme

[!INCLUDE [storage-emulator-connection-string-include](../../../includes/storage-emulator-connection-string-include.md)]

Bağlantı dizeleri hakkında daha fazla bilgi için Azure [Depolama bağlantı dizelerini yapılandırma'ya](../storage-configure-connection-string.md)bakın.

### <a name="authorize-with-a-shared-access-signature"></a>Paylaşılan erişim imzasıyla yetkilendirme

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Xamarin kitaplığı gibi bazı Azure depolama istemci kitaplıkları, yalnızca paylaşılan erişim imzası (SAS) belirteciyle kimlik doğrulamasını destekler. [Depolama](https://storageexplorer.com/) Gezgini'ni veya Paylaşılan Anahtar kimlik doğrulamasını destekleyen başka bir uygulamayı kullanarak SAS belirteci oluşturabilirsiniz.

Azure PowerShell'i kullanarak bir SAS jetonu da oluşturabilirsiniz. Aşağıdaki örnek, bir blob kapsayıcısına tam izinleri olan bir SAS belirteci oluşturur:

1. Azure PowerShell'i henüz yüklememişseniz (Azure PowerShell cmdlets'in en son sürümünü kullanmanız önerilir). Yükleme yönergeleri için Azure [PowerShell'i Yükle ve yapılandırma'ya](/powershell/azure/install-Az-ps)bakın.
2. Azure PowerShell'i açın ve seçtiğiniz `CONTAINER_NAME` bir adla değiştirerek aşağıdaki komutları çalıştırın:

```powershell
$context = New-AzStorageContext -Local

New-AzStorageContainer CONTAINER_NAME -Permission Off -Context $context

$now = Get-Date

New-AzStorageContainerSASToken -Name CONTAINER_NAME -Permission rwdl -ExpiryTime $now.AddDays(1.0) -Context $context -FullUri
```

Yeni kapsayıcı için elde edilen paylaşılan erişim imzası URI benzer olmalıdır:

```
http://127.0.0.1:10000/devstoreaccount1/sascontainer?sv=2012-02-12&se=2015-07-08T00%3A12%3A08Z&sr=c&sp=wl&sig=t%2BbzU9%2B7ry4okULN9S0wst%2F8MCUhTjrHyV9rDNLSe8g%3Dsss
```

Bu örnekle oluşturulan paylaşılan erişim imzası bir gün için geçerlidir. İmza, kapsayıcıiçindeki lekelere tam erişim (okuma, yazma, silme, liste) verir.

Paylaşılan erişim imzaları hakkında daha fazla bilgi için bkz: [Paylaşılan erişim imzalarını (SAS) kullanarak Azure Depolama kaynaklarına sınırlı erişim izni](storage-sas-overview.md)ver.

## <a name="addressing-resources-in-the-storage-emulator"></a>Depolama emülatördeki kaynakların ele alınması

Depolama emülatörü için hizmet bitiş noktaları, bir Azure depolama hesabının bitiş noktalarından farklıdır. Yerel bilgisayar, depolama emülatörü uç noktalarının yerel adresler olmasını gerektiren etki alanı adı çözümlemesi yapmaz.

Bir Azure depolama hesabında bir kaynağa hitap ettiğinizde, aşağıdaki düzeni kullanırsınız. Hesap adı URI ana bilgisayar adının bir parçasıdır ve ele alınmakta olan kaynak URI yolunun bir parçasıdır:

`<http|https>://<account-name>.<service-name>.core.windows.net/<resource-path>`

Örneğin, aşağıdaki URI, Azure depolama hesabındaki bir blob için geçerli bir adrestir:

`https://myaccount.blob.core.windows.net/mycontainer/myblob.txt`

Yerel bilgisayar etki alanı adı çözümlemesi yapmadığından, hesap adı ana bilgisayar adı yerine URI yolunun bir parçasıdır. Depolama emülatöründeki bir kaynak için işleyen aşağıdaki URI biçimini kullanım:

`http://<local-machine-address>:<port>/<account-name>/<resource-path>`

Örneğin, depolama emülatöründeki bir blob'a erişmek için aşağıdaki adres kullanılabilir:

`http://127.0.0.1:10000/myaccount/mycontainer/myblob.txt`

Depolama emülatörü için hizmet bitiş noktaları şunlardır:

* Blob hizmeti:`http://127.0.0.1:10000/<account-name>/<resource-path>`
* Sıra hizmeti:`http://127.0.0.1:10001/<account-name>/<resource-path>`
* Tablo servisi:`http://127.0.0.1:10002/<account-name>/<resource-path>`

### <a name="addressing-the-account-secondary-with-ra-grs"></a>RA-GRS ile ikincil hesabın ele alınması

Sürüm 3.1 ile başlayarak, depolama emülatörü okuma erişimi coğrafi çoğaltmayı (RA-GRS) destekler. İkincil konuma, hesap adına ikincil ekleyerek erişebilirsiniz. Örneğin, aşağıdaki adres, depolama emülatöründeki salt okunur ikincil bilgileri kullanarak bir blob'a erişmek için kullanılabilir:

`http://127.0.0.1:10000/myaccount-secondary/mycontainer/myblob.txt`

> [!NOTE]
> Depolama emülatörü ile ikincil programatik erişim için ,.NET sürüm 3.2 veya sonraki için Depolama İstemci Kitaplığını kullanın. Ayrıntılar [için .NET için Microsoft Azure Depolama İstemci Kitaplığı'na](https://msdn.microsoft.com/library/azure/dn261237.aspx) bakın.
>
>

## <a name="storage-emulator-command-line-tool-reference"></a>Depolama emülatör komut satırı araç başvurusu

Sürüm 3.0'dan başlayarak, Depolama Emülatörü'nü başlattığınızda bir konsol penceresi görüntülenir. Emülatörbaşlatmak ve durdurmak için konsol penceresindekomut satırı kullanın. Ayrıca durum sorgusu yapabilir ve komut satırından diğer işlemleri yapabilirsiniz.

> [!NOTE]
> Microsoft Azure işlem emülatörü yüklüyse, Depolama Emülatörü'ni başlattığınızda bir sistem tepsisi simgesi görüntülenir. Depolama Emülatörü başlatmak ve durdurmak için grafik bir yol sağlayan bir menü ortaya çıkarmak için simgeye sağ tıklayın.
>
>

### <a name="command-line-syntax"></a>Komut satırı sözdizimi

`AzureStorageEmulator.exe [start] [stop] [status] [clear] [init] [help]`

### <a name="options"></a>Seçenekler

Seçenek listesini görüntülemek için komut satırına `/help` yazın.

| Seçenek | Açıklama | Komut | Bağımsız Değişkenler |
| --- | --- | --- | --- |
| **Başlangıç** |Depolama emülatörü başlatıyor. |`AzureStorageEmulator.exe start [-inprocess]` |*-Yeniden İşlem*: Yeni bir işlem oluşturmak yerine emülatörü geçerli işlemde başlatın. |
| **Durdur** |Depolama emülatörü durdurur. |`AzureStorageEmulator.exe stop` | |
| **Durum** |Depolama emülatörü durumunu yazdırır. |`AzureStorageEmulator.exe status` | |
| **Temizle** |Komut satırında belirtilen tüm hizmetlerdeki verileri temizler. |`AzureStorageEmulator.exe clear [blob] [table] [queue] [all]` |*blob*: Blob verilerini temizler. <br/>*sıra*: Sıra verilerini temizler. <br/>*tablo*: Tablo verilerini temizler. <br/>*all*: Tüm hizmetlerdeki tüm verileri temizler. |
| **ınit** |Emülatör kurmak için bir kerelik başlatma yapar. |<code>AzureStorageEmulator.exe init [-server serverName] [-sqlinstance instanceName] [-forcecreate&#124;-skipcreate] [-reserveports&#124;-unreserveports] [-inprocess]</code> |*-server serverName\instanceName*: SQL örneğini barındıran sunucuyu belirtir. <br/>*-sqlinstance instanceName*: Varsayılan sunucu örneğinde kullanılacak SQL örneğinin adını belirtir. <br/>*-forcecreate*: SQL veritabanının oluşturulmasını zorlar, zaten mevcut olsa bile. <br/>*-skipcreate*: SQL veritabanının oluşturulmasını atlar. Bu -forcecreate üzerinde önceliklidir.<br/>*-reserveports*: Hizmetlerle ilişkili HTTP bağlantı noktalarını ayırmaya çalışır.<br/>*-unreserveports*: Hizmetlerle ilişkili HTTP bağlantı noktaları için rezervasyonları kaldırmaya çalışır. Bu, -reserveports'a göre önceliklidir.<br/>*-inprocess*: Yeni bir işlem yumurtlamak yerine geçerli süreçte başlatma gerçekleştirir. Bağlantı noktası rezervasyonları değişiyorsa, geçerli işlem yüksek izinlerle başlatılmalıdır. |

## <a name="differences-between-the-storage-emulator-and-azure-storage"></a>Depolama emülatörü ve Azure Depolama arasındaki farklar

Depolama emülatörü yerel bir taklit ortam olduğundan, emülatör ile buluttaki bir Azure depolama hesabının kullanımı arasında farklar vardır:

* Depolama emülatörü yalnızca tek bir sabit hesabı ve tanınmış bir kimlik doğrulama anahtarını destekler.
* Depolama emülatörü ölçeklenebilir bir depolama hizmeti değildir ve çok sayıda eşzamanlı istemciyi desteklemez.
* [Depolama emülatöründeki kaynakların Adresleme'sinde](#addressing-resources-in-the-storage-emulator)açıklandığı gibi, kaynaklar depolama emülatöründe Azure depolama hesabına göre farklı şekilde ele alınır. Aradaki fark, etki alanı adı çözümlemesi bulutta kullanılabilir, ancak yerel bilgisayarda bulunmaz.
* Sürüm 3.1 ile başlayarak, depolama emülatörü hesabı okuma-erişim coğrafi çoğaltma (RA-GRS) destekler. Emülatörde, tüm hesaplar RA-GRS etkinleştirilmiş ve birincil ve ikincil yinelemeler arasında hiçbir gecikme yoktur. Blob Hizmet İstatistikleri Al, Sıra Hizmeti İstatistikleri Al ve Tablo Hizmeti İstatistikleri Al işlemleri ikincil hesapta desteklenir ve yanıt öğesinin `LastSyncTime` değerini her zaman temel SQL veritabanına göre geçerli saat olarak döndürür.
* Dosya hizmeti ve SMB iletişim kuralı bitiş noktaları şu anda depolama emülatöründe desteklenmez.
* Emülatör tarafından desteklenmeyen depolama hizmetlerinin bir sürümünü kullanırsanız, emülatör bir VersionNotSupportedByEmulator hatasını döndürür (HTTP durum kodu 400 - Kötü İstek).

### <a name="differences-for-blob-storage"></a>Blob depolama için farklılıklar

Aşağıdaki farklar emülatördeki Blob depolamaiçin geçerlidir:

* Depolama emülatörü yalnızca 2 GB'a kadar olan blob boyutlarını destekler.
* Depolama emülatöründeki blob adının maksimum uzunluğu 256 karakter, Azure Depolama'daki bir blob adının maksimum uzunluğu ise 1024 karakterdir.
* Artımlı kopya, üzerinde yazılan lekelerden anlık görüntünün kopyalanmasını sağlar ve bu da hizmette bir hata yıkarşılar.
* Sayfa Aralıkları Diff alın, Artımlı Kopya Blob kullanılarak kopyalanan anlık görüntüler arasında çalışmaz.
* Bir Put Blob işlemi, istekte kira kimliği belirtilmemiş olsa bile, depolama emülatöründe etkin bir kira ile bulunan bir blob'a karşı başarılı olabilir.
* Ek Blob işlemleri emülatör tarafından desteklenmez. Bir ek blob üzerinde bir işlem girişimi bir FeatureNotSupportedByEmulator hata (HTTP durum kodu 400 - Kötü İstek) döndürür.

### <a name="differences-for-table-storage"></a>Tablo depolama için farklılıklar

Aşağıdaki farklar emülatördeki Tablo depolamaiçin geçerlidir:

* Depolama emülatöründeki Tablo hizmetindeki tarih özellikleri yalnızca SQL Server 2005 tarafından desteklenen aralığı destekler (1 Ocak 1753'ten sonra olması gerekir). 1 Ocak 1753'den önceki tüm tarihler bu değere değiştirilir. Tarihlerin kesinliği SQL Server 2005'in hassasiyeti ile sınırlıdır, bu da tarihlerin saniyenin 1/300'ü kadar hassas olduğu anlamına gelir.
* Depolama emülatörü, her biri 512 bayttan az bölüm anahtarı ve satır anahtar özellik değerlerini destekler. Hesap adının, tablo adının ve anahtar özellik adlarının toplam boyutu birlikte 900 baytı geçemez.
* Depolama emülatöründeki bir tablodaki bir satırın toplam boyutu 1 MB'dan az dır.
* Depolama emülatöründe, veri `Edm.Guid` türü `Edm.Binary` veya `Equal (eq)` yalnızca `NotEqual (ne)` sorgu filtresi dizelerinde bulunan ve karşılaştırma işleçlerini destekleyen özellikler.

### <a name="differences-for-queue-storage"></a>Sıra depolama için farklılıklar

Emülatörde Sıra depolamasına özgü bir fark yoktur.

## <a name="storage-emulator-release-notes"></a>Depolama emülatör sürüm notları

### <a name="version-510"></a>Sürüm 5.10

* Depolama emülatörü, Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2019-07-07 sürümünü reddetmez.

### <a name="version-59"></a>Sürüm 5.9

* Depolama emülatörü, Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2019-02-02 sürümünü reddetmez.

### <a name="version-58"></a>Sürüm 5.8

* Depolama emülatörü, Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2018-11-09 sürümünü reddetmez.

### <a name="version-57"></a>Sürüm 5.7

* Günlüğe kaydetme etkinse kilitlenmeye neden olacak bir hata düzeltildi.

### <a name="version-56"></a>Sürüm 5.6

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2018-03-28 sürümünü destekler.

### <a name="version-55"></a>Sürüm 5.5

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2017-11-09 sürümünü destekler.
* Blob'un oluşturma süresini döndüren blob **Oluşturulan** özelliği için destek eklendi.

### <a name="version-54"></a>Sürüm 5.4

* Yükleme kararlılığını iyileştirmek için emülatör artık yükleme zamanında bağlantı noktalarını ayırmaya çalışmaz. Bağlantı noktası rezervasyonlarını istiyorsanız, bunları belirtmek için **init** komutunun *-reserveports* seçeneğini kullanın.

### <a name="version-53"></a>Sürüm 5.3

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2017-07-29 sürümünü destekler.

### <a name="version-52"></a>Sürüm 5.2

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2017-04-17 sürümünü destekler.
* Tablo özelliği değerlerinin düzgün kodlanmadığı bir hata düzeltildi.

### <a name="version-51"></a>Sürüm 5.1

* Depolama emülatörü, hizmetin olmadığı `DataServiceVersion` bazı yanıtlarda üstbilginin döndürüldüğü bir hata düzeltildi.

### <a name="version-50"></a>Sürüm 5.0

* Depolama emülatörü yükleyici artık varolan MSSQL ve .NET Framework yüklemelerini denetlemez.
* Depolama emülatör yükleyici artık yükleme nin bir parçası olarak veritabanı oluşturur. Başlatmanın bir parçası olarak gerekirse veritabanı yine de oluşturulur.
* Veritabanı oluşturma artık yükseklik gerektirir.
* Başlangıç için artık bağlantı noktası rezervasyonlarına gerek yoktur.
* Aşağıdaki seçenekleri `init`ekler: `-reserveports` (yükseklik `-unreserveports` gerektirir), (yükseklik gerektirir), `-skipcreate`.
* Sistem tepsisi simgesindeki Depolama Emülatör UI seçeneği artık komut satırı arabirimini başlattı. Eski GUI artık kullanılamıyor.
* Bazı DL'ler kaldırıldı veya yeniden adlandırıldı.

### <a name="version-46"></a>Sürüm 4.6

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2016-05-31 sürümünü destekler.

### <a name="version-45"></a>Sürüm 4.5

* Destek veritabanı yeniden adlandırıldığında yükleme ve başlatmanın başarısız olmasıyla ilgili bir hata düzeltildi.

### <a name="version-44"></a>Sürüm 4.4

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2015-12-11 sürümünü destekler.
* Depolama emülatörü'nün blob verisinin çöp toplaması, çok sayıda blob ile uğraşırken artık daha verimli hale geliyor.
* ACL XML kapsayıcısının depolama hizmetinin yaptığı ndan biraz farklı olarak doğrulanmasına neden olan bir hata düzeltildi.
* Bazen hatalı saat diliminde bildirilen max ve min DateTime değerlerine neden olan bir hata düzeltildi.

### <a name="version-43"></a>Sürüm 4.3

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2015-07-08 sürümünü destekler.

### <a name="version-42"></a>Sürüm 4.2

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2015-04-05 sürümünü destekler.

### <a name="version-41"></a>Sürüm 4.1

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2015-02-21 sürümünü destekler. Yeni Append Blob özelliklerini desteklemez.
* Emülatör şimdi depolama hizmetlerinin desteklenmeyen sürümleri için anlamlı bir hata iletisi döndürür. Emülatörün en son sürümünü kullanmanızı öneririz. Bir VersionNotSupportedByEmulator hatası alırsanız (HTTP durum kodu 400 - Kötü İstek), emülatörün en son sürümünü indirin.
* Bir yarış koşulunun, eşzamanlı birleştirme işlemleri sırasında tablo varlık verilerinin yanlış kalmasına neden olduğu bir hata düzeltildi.

### <a name="version-40"></a>Sürüm 4.0

* Yürütülebilir depolama emülatörü *AzureStorageEmulator.exe*olarak yeniden adlandırılmıştır.

### <a name="version-32"></a>Sürüm 3.2

* Depolama emülatörü artık Blob, Queue ve Tablo hizmeti uç noktalarındaki depolama hizmetlerinin 2014-02-14 sürümünü destekler. Dosya hizmeti bitiş noktaları şu anda depolama emülatöründe desteklenmez. Sürüm 2014-02-14 hakkında ayrıntılar için [Azure Depolama Hizmetleri](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) sürümüne bakın.

### <a name="version-31"></a>Sürüm 3.1

* Okuma erişimi coğrafi yedekli depolama (RA-GRS) artık depolama emülatörde desteklenir. , `Get Blob Service Stats` `Get Queue Service Stats`, `Get Table Service Stats` ve API'ler ikincil hesap için desteklenir ve her zaman altta yatan SQL veritabanına göre geçerli saat olarak LastSyncTime yanıt öğesinin değerini döndürür. Depolama emülatörü ile ikincil programatik erişim için ,.NET sürüm 3.2 veya sonraki için Depolama İstemci Kitaplığını kullanın. Ayrıntılar için .NET Başvurusu için Microsoft Azure Depolama İstemci Kitaplığı'na bakın.

### <a name="version-30"></a>Sürüm 3.0

* Azure depolama emülatörü artık işlem emülatörüyle aynı pakette gönderilmez.
* Depolama emülatörü grafik kullanıcı arabirimi amortismana hazırdır. Komut dosyası komut satırı arabirimi ile değiştirildi. Komut satırı arabirimi yle ilgili ayrıntılar için Storage Emülatör Komut-Line Araç Başvurusu'na bakın. Grafik arabirimi sürüm 3.0'da mevcut olmaya devam eder, ancak yalnızca Sistem tepsisi simgesine sağ tıklayarak ve Depolama Emülatör UI'sini seçerek Bilgi İşlem Emülatör'ü yüklendiğinde erişilebilir.
* Azure depolama hizmetlerinin 2013/08-15 sürümü artık tamamen desteklendi. (Daha önce bu sürüm yalnızca Depolama Emülatörü sürümü 2.2.1 Önizleme tarafından desteklenmiştir.)

## <a name="next-steps"></a>Sonraki adımlar

* Çapraz platform, toplum tarafından korunan açık kaynak depolama emülatörü [Azurite'yi](https://github.com/arafato/azurite)değerlendirin. 
* [.NET'i kullanan Azure Depolama örnekleri,](../storage-samples-dotnet.md) uygulamanızı geliştirirken kullanabileceğiniz çeşitli kod örneklerine bağlantılar içerir.
* Microsoft Azure [Depolama](https://storageexplorer.com) Gezgini'ni bulut Depolama hesabınızdaki ve depolama emülatöründeki kaynaklarla çalışmak için kullanabilirsiniz.
