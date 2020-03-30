---
title: Azure Akış Analitiği'nden çıktıları anlama
description: Bu makalede, analiz sonuçları için Power BI de dahil olmak üzere Azure Akış Analizi'nde bulunan veri çıktısı seçenekleri açıklanmaktadır.
author: mamccrea
ms.author: mamccrea
ms.reviewer: mamccrea
ms.service: stream-analytics
ms.topic: conceptual
ms.date: 02/14/2020
ms.openlocfilehash: e0b4bcac8494f136dde21b03422e12b72cecb8f3
ms.sourcegitcommit: 07d62796de0d1f9c0fa14bfcc425f852fdb08fb1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "80366438"
---
# <a name="understand-outputs-from-azure-stream-analytics"></a>Azure Akış Analitiği'nden çıktıları anlama

Bu makalede, bir Azure Akış Analizi işi için kullanılabilir çıktı türleri açıklanmaktadır. Çıktılar, Stream Analytics işinin sonuçlarını depolamanızı ve kaydetmenizi sağlar. Çıktı verilerini kullanarak, verilerinizin daha fazla iş analitiği ve veri depolamasını yapabilirsiniz.

Akış Analizi sorgunuzu tasarlarken, [INTO yan tümcesini](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics)kullanarak çıktının adına bakın. Sorguda birden çok INTO yan tümcesi sağlayarak iş başına tek bir çıktı veya akış işi başına birden çok çıktı (gerekirse) kullanabilirsiniz.

Akış Analizi iş çıktılarını oluşturmak, düzenlemek ve test etmek için [Azure portalı, Azure](stream-analytics-quick-create-portal.md#configure-job-output) [PowerShell](stream-analytics-quick-create-powershell.md#configure-output-to-the-job), [.NET API](https://docs.microsoft.com/dotnet/api/microsoft.azure.management.streamanalytics.ioutputsoperations?view=azure-dotnet), [REST API](https://docs.microsoft.com/rest/api/streamanalytics/stream-analytics-output)ve [Visual Studio'yu](stream-analytics-quick-create-vs.md)kullanabilirsiniz.

Bazı çıktı türleri [bölümleme](#partitioning)desteği. [Çıktı toplu iş boyutları,](#output-batch-size) verimi optimize etmek için değişir.


## <a name="azure-data-lake-storage-gen-1"></a>Azure Veri Gölü Depolama Gen 1

Akış [Analizi, Azure Veri Gölü Depolama Gen 1'i](../data-lake-store/data-lake-store-overview.md)destekler. Azure Veri Gölü Depolama, büyük veri analitik iş yükleri için kurumsal çapında, hiper ölçekli bir depodur. Veri Gölü Depolama'yı, operasyonel ve araştırmacı analizler için herhangi bir boyut, tür ve yutma hızıverilerini depolamak için kullanabilirsiniz. Veri Gölü Depolama'ya erişmek için Akış Analizi'nin yetkilendirilmesi gerekir.

Stream Analytics'ten Azure Veri Gölü Depolama çıktısı şu anda Azure China 21Vianet ve Azure Almanya (T-Systems International) bölgelerinde kullanılamıyor.

Aşağıdaki tabloda, Veri Gölü Depolama Gen 1 çıktınızı yapılandırmak için özellik adları ve açıklamaları listelenilir.   

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Sorgu çıktısını Data Lake Store'a yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Abonelik | Azure Veri Gölü Depolama hesabınızı içeren abonelik. |
| Hesap adı | Çıktınızı gönderdiğiniz Veri Gölü Deposu hesabının adı. Aboneliğinizde kullanılabilen Veri Gölü Deposu hesaplarının açılır bir listesi size sunulur. |
| Yol öneki deseni | Dosyalarınızı belirtilen Data Lake Store hesabına yazmak için kullanılan dosya yolu. {date} ve {time} değişkenlerinden bir veya daha fazla örnek belirtebilirsiniz:<br /><ul><li>Örnek 1: folder1/logs/{date}/{time}</li><li>Örnek 2: klasör1/logs/{tarih}</li></ul><br />Oluşturulan klasör yapısının zaman damgası, yerel saati değil, UTC'yi izler.<br /><br />Dosya yolu deseni bir çizgi (/) içermiyorsa, dosya yolundaki son desen dosya adı öneki olarak kabul edilir. <br /><br />Bu koşullarda yeni dosyalar oluşturulur:<ul><li>Çıktı şemasındaki değişim</li><li>Bir işin harici veya dahili yeniden başlatma</li></ul> |
| Tarih biçimi | İsteğe bağlı. Tarih belirteci önek yolunda kullanılıyorsa, dosyalarınızın düzenlendiği tarih biçimini seçebilirsiniz. Örnek: YYYY/MM/DD |
|Zaman biçimi | İsteğe bağlı. Önek yolunda saat belirteci kullanılıyorsa, dosyalarınızın düzenlendiği saat biçimini belirtin. Şu anda desteklenen tek değer HH'dir. |
| Olay serileştirme biçimi | Çıktı verileri için serileştirme biçimi. JSON, CSV ve Avro desteklenir.|
| Encoding | CSV veya JSON biçimini kullanıyorsanız, bir kodlama belirtilmelidir. UTF-8 şu anda desteklenen tek kodlama biçimidir.|
| Sınırlayıcı | Yalnızca CSV serileştirme için geçerlidir. Akış Analizi, CSV verilerini seri hale getirmek için bir dizi ortak sınır dışı layıcıyı destekler. Desteklenen değerler virgül, yarı sütun, boşluk, sekme ve dikey çubuk'tür.|
| Biçimlendir | Yalnızca JSON serileştirme için geçerlidir. **Satır ayrılmış** çıktısı her JSON nesnesi yeni bir satır ile ayrılmış alarak biçimlendirilir belirtir. **Satır ayrılmış**seçerseniz, JSON bir seferde bir nesne okunur. Tek başına tüm içerik geçerli bir JSON olmaz.  **Dizi,** çıktının json nesnesi dizisi olarak biçimlendirilmiş olduğunu belirtir. Bu dizi yalnızca iş durduğunda veya Akış Analizi bir sonraki zaman penceresine geçtiğinde kapatılır. Genel olarak, çıktı dosyası hala yazılmakta yken herhangi bir özel işleme gerektirmediği için satır ayrılmış JSON kullanmak tercih edilir.|
| Kimlik doğrulaması modu | [Yönetilen Kimlik](stream-analytics-managed-identities-adls.md) veya Kullanıcı belirteci kullanarak Veri Gölü Depolama hesabınıza erişim yetkisi verebilirsiniz. Erişim izni verdikten sonra, kullanıcı hesabı parolasını değiştirerek, bu iş için Veri Gölü Depolama çıktısını silerek veya Stream Analytics işini silerek erişimi iptal edebilirsiniz. |

## <a name="sql-database"></a>SQL Database

[Azure SQL Veritabanı'nı,](https://azure.microsoft.com/services/sql-database/) doğası gereği ilişkisel veriler veya ilişkisel bir veritabanında barındırılan içeriğe bağlı uygulamalar için çıktı olarak kullanabilirsiniz. Akış Analizi işleri SQL Veritabanı'ndaki varolan bir tabloya yazar. Tablo şeması, işinizin çıktısındaki alanları ve bunların türlerini tam olarak eşleştirmelidir. Ayrıca, SQL Veritabanı çıktısı seçeneği aracılığıyla [Azure SQL Veri Ambarı'nı](https://azure.microsoft.com/documentation/services/sql-data-warehouse/) çıktı olarak da belirtebilirsiniz. Yazma verime sini geliştirmenin yolları hakkında bilgi edinmek için, Çıktı makalesi [olarak Azure SQL Veritabanı ile Akış Analizi'ni](stream-analytics-sql-output-perf.md) görün.

Azure SQL [Veritabanı Yönetilen Örneği'ni](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance) çıktı olarak da kullanabilirsiniz. [Azure SQL Veritabanı Yönetilen Örneği'nde ortak bitiş noktasını yapılandırmanız](https://docs.microsoft.com/azure/sql-database/sql-database-managed-instance-public-endpoint-configure) ve ardından Azure Akış Analizi'nde aşağıdaki ayarları el ile yapılandırmanız gerekir. SQL Server'ı bir veritabanı ekli çalıştıran Azure sanal makine, aşağıdaki ayarları el ile yapılandırarak da desteklenir.

Aşağıdaki tabloda, SQL Veritabanı çıktısı oluşturmak için özellik adları ve açıklamaları listeleneb.)

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıktısını bu veritabanına yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Database | Çıktınızı gönderdiğiniz veritabanının adı. |
| Sunucu adı | SQL Veritabanı sunucu adı. Azure SQL Veritabanı Yönetilen Örnek için bağlantı noktası 3342'yi belirtmeniz gerekir. Örneğin, *sampleserver.public.database.windows.net,3342* |
| Kullanıcı adı | Veritabanına yazma erişimi olan kullanıcı adı. Akış Analizi yalnızca SQL kimlik doğrulamasını destekler. |
| Parola | Veritabanına bağlanmak için parola. |
| Tablo | Çıktının yazıldığı tablo adı. Tablo adı büyük/küçük harf duyarlıdır. Bu tablonun şeması, iş çıktınızın oluşturduğu alan sayısı ve bunların türleri ile tam olarak eşleşmelidir. |
|Devralma bölüm şemasını| Tabloya birden çok yazarla tam paralel topoloji sağlamak için önceki sorgu adımınızın bölümleme düzenini devralma seçeneği. Daha fazla bilgi için Azure [Akış Analizi çıktısı için Azure SQL Veritabanı'na](stream-analytics-sql-output-perf.md)bakın.|
|Maksimum toplu iş sayısı| Her toplu ekleme hareketiyle gönderilen kayıt sayısında önerilen üst sınır.|

## <a name="blob-storage-and-azure-data-lake-gen2"></a>Blob depolama ve Azure Veri Gölü Gen2

Data Lake Storage Gen2, Azure Depolama'yı Azure'da kurumsal veri gölleri oluşturmak için temel haline getirir. Veri Gölü Depolama Gen2, yüzlerce gigabayt lık bir iş parçasını sürdürürken, birden fazla petabayt bilgi nin hizmet vermek üzere başlangıçtan itibaren tasarlanan Veri Gölü Depolama Gen2, büyük miktarda veriyi kolayca yönetmenize olanak tanır. Veri Gölü Depolama Gen2'nin temel bir parçası, Blob depolamasına hiyerarşik bir ad alanı eklenmesidir.

Azure Blob depolama, büyük miktarda yapılandırılmamış veriyi bulutta depolamak için uygun maliyetli ve ölçeklenebilir bir çözüm sunar. Blob depolama alanı ve kullanımı hakkında bir giriş için Azure [portalıyla yükleme, indirme ve liste bloblarına](../storage/blobs/storage-quickstart-blobs-portal.md)bakın.

Aşağıdaki tabloda, bir blob veya ADLS Gen2 çıktısı oluşturmak için özellik adları ve açıklamaları listeleneb.)

| Özellik adı       | Açıklama                                                                      |
| ------------------- | ---------------------------------------------------------------------------------|
| Çıktı diğer adı        | Sorgu çıktısını bu blob depolama alanına yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Depolama hesabı     | Çıktınızı gönderdiğiniz depolama hesabının adı.               |
| Depolama hesabı anahtarı | Depolama hesabıyla ilişkili gizli anahtar.                              |
| Depolama konteyneri   | Azure Blob hizmetinde depolanan lekeler için mantıksal bir gruplama. Blob hizmetine bir blob yüklediğinizde, bu blob için bir kapsayıcı belirtmeniz gerekir. |
| Yol deseni | İsteğe bağlı. Lekelerinizi belirtilen kapsayıcının içine yazmak için kullanılan dosya yolu deseni. <br /><br /> Yol deseninde, blobs'un yazıldığı sıklığını belirtmek için tarih ve saat değişkenlerinin bir veya daha fazla örneğini kullanmayı seçebilirsiniz: <br /> {tarih}, {zaman} <br /><br />Olay verilerinizden bölüm bloblarına özel {alan} adını belirtmek için özel blob bölümleme kullanabilirsiniz. Alan adı alfanümeriktir ve boşluklar, tireler ve alt çizgi içerir. Özel alanlardaki kısıtlamalar şunlardır: <ul><li>Alan adları büyük/küçük harf duyarlı değildir. Örneğin, hizmet "Id" sütunu ile "id" sütunu arasında ayrım yapamaz.</li><li>İç içe alanlara izin verilmez. Bunun yerine, alanı "düzleştirmek" için iş sorgusunda bir takma ad kullanın.</li><li>İfadeler alan adı olarak kullanılamaz.</li></ul> <br />Bu özellik, yoldaki özel tarih/saat biçimi belirtim yapılandırmalarının kullanılmasını sağlar. Özel tarih ve saat biçimleri, {datetime:\<>} anahtar sözcüğüyle birlikte teker teker belirtilmelidir. \<> için izin verilebilir girişler yyyy, MM, M, dd, d, HH, H, mm, m, ss veya s'dir. {datetime:\<belirtici>} anahtar kelimesi, özel tarih/saat yapılandırmaları oluşturma yolunda birden çok kez kullanılabilir. <br /><br />Örnekler: <ul><li>Örnek 1: cluster1/logs/{date}/{time}</li><li>Örnek 2: cluster1/logs/{date}</li><li>Örnek 3: cluster1/{client_id}/{date}/{time}</li><li>Örnek 4: cluster1/{datetime:ss}/{myField} sorgunun olduğu yer: SELECT data.myField AS myField FROM Input;</li><li>Örnek 5: cluster1/year={datetime:yyyy}/month={datetime:MM}/day={datetime:dd}</ul><br />Oluşturulan klasör yapısının zaman damgası, yerel saati değil, UTC'yi izler.<br /><br />Dosya adlandırma aşağıdaki kuralı kullanır: <br /><br />{Yol Önek Deseni}/schemaHashcode_Guid_Number.uzantı<br /><br />Örnek çıktı dosyaları:<ul><li>Myoutput/20170901/00/45434_gguid_1.csv</li>  <li>Myoutput/20170901/01/45434_gguid_1.csv</li></ul> <br />Bu özellik hakkında daha fazla bilgi için [Azure Akış Analizi özel blob çıktı bölümleme](stream-analytics-custom-path-patterns-blob-storage-output.md)bölümüne bakın. |
| Tarih biçimi | İsteğe bağlı. Tarih belirteci önek yolunda kullanılıyorsa, dosyalarınızın düzenlendiği tarih biçimini seçebilirsiniz. Örnek: YYYY/MM/DD |
| Zaman biçimi | İsteğe bağlı. Önek yolunda saat belirteci kullanılıyorsa, dosyalarınızın düzenlendiği saat biçimini belirtin. Şu anda desteklenen tek değer HH'dir. |
| Olay serileştirme biçimi | Çıktı verileri için serileştirme biçimi. JSON, CSV, Avro ve Parke desteklenir. |
|Minimum satırlar (sadece Parke)|Toplu iş başına en az satır sayısı. Parke için, her toplu yeni bir dosya oluşturur. Geçerli varsayılan değer 2.000 satırdır ve izin verilen maksimum değer 10.000 satırdır.|
|Maksimum süre (sadece parke)|Toplu iş başına maksimum bekleme süresi. Bu süre den sonra, en az satır gereksinimi karşılanmasa bile toplu iş çıktısına yazılır. Geçerli varsayılan değer 1 dakikadır ve izin verilen maksimum değer 2 saattir. Blob çıkışınızda yol deseni sıklığı varsa, bekleme süresi bölüm süresi aralığından daha yüksek olamaz.|
| Encoding    | CSV veya JSON biçimini kullanıyorsanız, bir kodlama belirtilmelidir. UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| Sınırlayıcı   | Yalnızca CSV serileştirme için geçerlidir. Akış Analizi, CSV verilerini seri hale getirmek için bir dizi ortak sınır dışı layıcıyı destekler. Desteklenen değerler virgül, yarı sütun, boşluk, sekme ve dikey çubuk'tür. |
| Biçimlendir      | Yalnızca JSON serileştirme için geçerlidir. **Satır ayrılmış** çıktısı her JSON nesnesi yeni bir satır ile ayrılmış alarak biçimlendirilir belirtir. **Satır ayrılmış**seçerseniz, JSON bir seferde bir nesne okunur. Tek başına tüm içerik geçerli bir JSON olmaz. **Dizi,** çıktının json nesnesi dizisi olarak biçimlendirilmiş olduğunu belirtir. Bu dizi yalnızca iş durduğunda veya Akış Analizi bir sonraki zaman penceresine geçtiğinde kapatılır. Genel olarak, çıktı dosyası hala yazılmakta yken herhangi bir özel işleme gerektirmediği için satır ayrılmış JSON kullanmak tercih edilir. |

Blob depolama alanını çıktı olarak kullandığınızda, aşağıdaki durumlarda blob'da yeni bir dosya oluşturulur:

* Dosya izin verilen maksimum blok sayısını aşıyorsa (şu anda 50.000). İzin verilen maksimum blob boyutuna ulaşmadan izin verilen maksimum blok sayısına ulaşabilirsiniz. Örneğin, çıktı oranı yüksekse, blok başına daha fazla bayt görebilirsiniz ve dosya boyutu daha büyüktür. Çıktı oranı düşükse, her bloğun daha az verisi vardır ve dosya boyutu daha küçüktür.
* Çıktıda şema değişikliği varsa ve çıktı biçimi sabit şema (CSV ve Avro) gerektiriyorsa.
* Bir iş, harici olarak bir kullanıcı tarafından durdurularak ve başlatarak veya sistem bakımı veya hata kurtarma için dahili olarak yeniden başlatılırsa.
* Sorgu tamamen bölümlenmişse ve her çıktı bölümü için yeni bir dosya oluşturulursa.
* Kullanıcı bir dosyayı veya depolama hesabının bir kapsayıcısını silerse.
* Çıktı yol öneki deseni kullanılarak zaman bölümlenirse ve sorgu bir sonraki saate geçtiğinde yeni bir blob kullanılır.
* Çıktı özel bir alantarafından bölümlenmişse ve yoksa bölüm anahtarı başına yeni bir blob oluşturulur.
* Çıktı, bölüm anahtarının 8.000'i aştığı özel bir alanla bölümlenirse ve bölüm anahtarı başına yeni bir blob oluşturulursa.

## <a name="event-hubs"></a>Event Hubs

[Azure Etkinlik Hub'ları](https://azure.microsoft.com/services/event-hubs/) hizmeti, son derece ölçeklenebilir bir yayımlama-abone olay yutulur. Saniyede milyonlarca olay toplayabilir. Bir olay hub'ının çıktı olarak kullanılması, bir Akış Analizi işinin çıktısının başka bir akış işinin girişi haline gelmesidir. Maksimum ileti boyutu ve toplu iş boyutu optimizasyonu hakkında bilgi için [çıktı toplu iş boyutu](#output-batch-size) bölümüne bakın.

Olay merkezlerinden gelen veri akışlarını çıktı olarak yapılandırmak için birkaç parametreye ihtiyacınız vardır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Sorgu çıktısını bu olay hub'ına yönlendirmek için sorgularda kullanılan dost adı. |
| Olay hub’ı ad alanı | Bir dizi ileti varlıkları için bir kapsayıcı. Yeni bir olay hub'ı oluşturduğunuzda, bir olay merkezi ad alanı da oluşturabilirsiniz. |
| Olay hub'ı adı | Olay merkezi çıktınızın adı. |
| Olay merkezi ilkesi adı | Olay merkezinin **Yapılandırma** sekmesinde oluşturabileceğiniz paylaşılan erişim ilkesi. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. |
| Olay merkezi ilkesi anahtarı | Olay hub'ına erişimin kimliğini doğrulamak için kullanılan paylaşılan erişim anahtarı ad alanı. |
| Bölme anahtar sütunu | İsteğe bağlı. Olay hub çıktısı için bölüm anahtarı nı içeren bir sütun. |
| Olay serileştirme biçimi | Çıktı verileri için serileştirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding | CSV ve JSON için UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| Sınırlayıcı | Yalnızca CSV serileştirme için geçerlidir. Stream Analytics, csv formatında verileri seri hale getirmek için bir dizi ortak sınır layıcıyı destekler. Desteklenen değerler virgül, yarı sütun, boşluk, sekme ve dikey çubuk'tür. |
| Biçimlendir | Yalnızca JSON serileştirme için geçerlidir. **Satır ayrılmış** çıktısı her JSON nesnesi yeni bir satır ile ayrılmış alarak biçimlendirilir belirtir. **Satır ayrılmış**seçerseniz, JSON bir seferde bir nesne okunur. Tek başına tüm içerik geçerli bir JSON olmaz. **Dizi,** çıktının json nesnesi dizisi olarak biçimlendirilmiş olduğunu belirtir.  |
| Özellik sütunları | İsteğe bağlı. Yük yerine giden iletinin kullanıcı özellikleri olarak eklenmesi gereken virgülle ayrılmış sütunlar. Bu özellik hakkında daha fazla bilgi çıkış için özel meta veri özellikleri bölümünde yer [almaktadır.](#custom-metadata-properties-for-output) |

## <a name="power-bi"></a>Power BI

[Power](https://powerbi.microsoft.com/) BI'yi, analiz sonuçlarının zengin bir görselleştirme deneyimi sağlamak için Stream Analytics işi için bir çıktı olarak kullanabilirsiniz. Bu özelliği operasyonel panolar, rapor oluşturma ve metrik tabanlı raporlama için kullanabilirsiniz.

Stream Analytics'in Güç BI çıktısı şu anda Azure China 21Vianet ve Azure Almanya (T-Systems International) bölgelerinde kullanılamıyor.

Aşağıdaki tabloda, Power BI çıktınızı yapılandırmak için özellik adları ve açıklamaları listelenebiler.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıktısını bu Power BI çıkışına yönlendirmek için sorgularda kullanılan kolay bir ad sağlayın. |
| Grup çalışma alanı |Diğer Power BI kullanıcılarıyla veri paylaşımını etkinleştirmek için, Power BI hesabınızdaki grupları seçebilir veya bir gruba yazmak istemiyorsanız **Çalışma Alanım'ı** seçebilirsiniz. Varolan bir grubu güncelleştirmek için Power BI kimlik doğrulaması nın yenilenmesi gerekiyor. |
| Veri kümesi adı |Power BI çıkışının kullanılmasını istediğiniz bir veri kümesi adı sağlayın. |
| Tablo adı |Power BI çıkışının veri kümesi altında bir tablo adı sağlayın. Şu anda, Stream Analytics işlerinden gelen Power BI çıktısında veri kümesinde yalnızca bir tablo olabilir. |
| Bağlantı yetkilendirme | Çıktı ayarlarınızı yapılandırmak için Power BI ile yetkilendirmeniz gerekir. Bu çıktıyı Power BI panonuza verdikten sonra, kullanıcı hesabı parolasını değiştirerek, iş çıktısını silerek veya Stream Analytics işini silerek erişimi iptal edebilirsiniz. | 

Power BI çıktısını ve panosunu yapılandırma kinleri için [Azure Akışı Analizi ve Power BI](stream-analytics-power-bi-dashboard.md) öğreticisini görün.

> [!NOTE]
> Power BI panosunda veri kümesini ve tabloyu açıkça oluşturmayın. İş başlatıldığında ve iş Power BI'ye çıktı pompalamaya başladığında veri kümesi ve tablo otomatik olarak doldurulur. İş sorgusu herhangi bir sonuç oluşturmuyorsa, veri kümesi ve tablo oluşturulmaz. Power BI'de bu Akış Analizi işinde sağlananla aynı ada sahip bir veri kümesi ve tablo varsa, varolan veriler üzerine yazılır.
>

### <a name="create-a-schema"></a>Şema oluşturma
Azure Akış Analizi, zaten yoksa kullanıcı için bir Power BI veri kümesi ve tablo şeması oluşturur. Diğer tüm durumlarda, tablo yeni değerlerle güncelleştirilir. Şu anda, bir veri kümesi içinde yalnızca bir tablo olabilir. 

Power BI, ilk-in, ilk çıkan (FIFO) bekletme ilkesini kullanır. Veriler, 200.000 satıra çıkana kadar bir tabloda toplanır.

### <a name="convert-a-data-type-from-stream-analytics-to-power-bi"></a>Akış Analizi'nden Power BI'ye veri türünü dönüştürme
Azure Akış Analizi, çıktı şeması değişirse veri modelini çalışma zamanında dinamik olarak güncelleştirir. Sütun adı değişiklikleri, sütun türü değişiklikleri ve sütunekleme veya kaldırma tüm izlenir.

Bu tablo, Power BI veri kümesi ve tablo [yoksa, Stream Analytics veri türlerinden](https://docs.microsoft.com/stream-analytics-query/data-types-azure-stream-analytics) Power BI [Entity Data Model (EDM) türlerine](https://docs.microsoft.com/dotnet/framework/data/adonet/entity-data-model)veri türü dönüşümlerini kapsar.

Akış Analizi'nden | Güç BI için
-----|-----
bigint | Int64
nvarchar(maks) | Dize
datetime | Tarih saat
float | Çift
Kayıt dizisi | Dize türü, sabit değer "IRecord" veya "IArray"

### <a name="update-the-schema"></a>Şeayı güncelleştir
Akış Analizi, çıktıdaki ilk olay kümesine göre veri modeli şeasını ortaya çıkar. Daha sonra, gerekirse, veri modeli şeması özgün şemaya sığmayabilecek gelen olayları barındıracak şekilde güncelleştirilir.

Satırlar `SELECT *` arasında dinamik şema güncelleştirmesini önlemek için sorgudan kaçının. Olası performans etkilerine ek olarak, sonuçlar için alınan sürenin belirsizliğine neden olabilir. Power BI panosunda gösterilmesi gereken tam alanları seçin. Ayrıca, veri değerleri seçilen veri türüyle uyumlu olmalıdır.


Önceki/geçerli | Int64 | Dize | Tarih saat | Çift
-----------------|-------|--------|----------|-------
Int64 | Int64 | Dize | Dize | Çift
Çift | Çift | Dize | Dize | Çift
Dize | Dize | Dize | Dize | Dize 
Tarih saat | Dize | Dize |  Tarih saat | Dize

## <a name="table-storage"></a>Table Storage

[Azure Tablo depolama](../storage/common/storage-introduction.md) alanı, bir uygulamanın kullanıcı talebini karşılamak için otomatik olarak ölçeklendirebilmeleri için yüksek oranda kullanılabilir, büyük ölçüde ölçeklenebilir depolama alanı sunar. Tablo depolama, şemaüzerinde daha az kısıtlama içeren yapılandırılmış veriler için kullanabileceğiniz Microsoft'un NoSQL anahtar/öznitelik deposudur. Azure Tablo depolama kalıcılık ve verimli alma için veri depolamak için kullanılabilir.

Aşağıdaki tabloda, tablo çıktısı oluşturmak için özellik adları ve açıklamaları listelenistır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıktısını bu tablo depolama alanına yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Depolama hesabı |Çıktınızı gönderdiğiniz depolama hesabının adı. |
| Depolama hesabı anahtarı |Depolama hesabıyla ilişkili erişim anahtarı. |
| Tablo adı |Masanın adı. Tablo yoksa oluşturulur. |
| Bölüm anahtarı |Bölüm anahtarını içeren çıkış sütununun adı. Bölüm anahtarı, varlığın birincil anahtarının ilk bölümünü oluşturan bir tabloiçindeki bölüm için benzersiz bir tanımlayıcıdır. 1 KB boyutuna kadar olabilecek bir dize değeridir. |
| Satır tuşu |Satır anahtarını içeren çıktı sütununun adı. Satır anahtarı, bölüm içindeki bir varlık için benzersiz bir tanımlayıcıdır. Bir varlığın birincil anahtarının ikinci bölümünü oluşturur. Satır tuşu, 1 KB boyutuna kadar olabilecek bir dize değeridir. |
| Toplu iş boyutu |Toplu işleyiş için kayıt sayısı. Varsayılan (100) çoğu iş için yeterlidir. Bu ayarı değiştirme hakkında daha fazla bilgi için [Tablo Toplu İşlemi spec](https://docs.microsoft.com/java/api/com.microsoft.azure.storage.table._table_batch_operation) bakın. |

## <a name="service-bus-queues"></a>Service Bus kuyrukları

[Servis Veri Hizmetleri kuyrukları,](../service-bus-messaging/service-bus-queues-topics-subscriptions.md) bir veya daha fazla rakip tüketiciye BIR FIFO ileti teslimi sunar. Genellikle, iletiler alıcılar tarafından sıraya eklendikleri zamansal sırada alınır ve işlenir. Her ileti alınır ve yalnızca bir ileti tüketicisi tarafından işlenir.

[Uyumluluk düzeyi 1.2'de](stream-analytics-compatibility-level.md)Azure Akış Analizi, Servis Veri Hizmetleri Kuyrukları ve Konuları'na yazmak için [Gelişmiş İleti Kuyruk Protokolü (AMQP)](../service-bus-messaging/service-bus-amqp-overview.md) ileti protokolünü kullanır. AMQP, açık standart bir protokol kullanarak çapraz platform, karma uygulamalar oluşturmanıza olanak tanır.

Aşağıdaki tabloda, sıra çıktısı oluşturmak için özellik adları ve açıklamaları listelenilir.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıktısını bu Hizmet Veri Servisi kuyruğuna yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Servis Veri Günü ad alanı |Bir dizi ileti varlıkları için bir kapsayıcı. |
| Kuyruk adı |Servis Veri Günü kuyruğunun adı. |
| Sıra ilkesi adı |Bir kuyruk oluşturduğunuzda, sıranın **Yapılandırma** sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. |
| Sıra ilkesi anahtarı |Hizmet Veri Servisi ad alanına erişimi doğrulamak için kullanılan paylaşılan erişim anahtarı. |
| Olay serileştirme biçimi |Çıktı verileri için serileştirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding |CSV ve JSON için UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| Sınırlayıcı |Yalnızca CSV serileştirme için geçerlidir. Stream Analytics, csv formatında verileri seri hale getirmek için bir dizi ortak sınır layıcıyı destekler. Desteklenen değerler virgül, yarı sütun, boşluk, sekme ve dikey çubuk'tür. |
| Biçimlendir |Yalnızca JSON tipi için geçerlidir. **Satır ayrılmış** çıktısı her JSON nesnesi yeni bir satır ile ayrılmış alarak biçimlendirilir belirtir. **Satır ayrılmış**seçerseniz, JSON bir seferde bir nesne okunur. Tek başına tüm içerik geçerli bir JSON olmaz. **Dizi,** çıktının json nesnesi dizisi olarak biçimlendirilmiş olduğunu belirtir. |
| Özellik sütunları | İsteğe bağlı. Yük yerine giden iletinin kullanıcı özellikleri olarak eklenmesi gereken virgülle ayrılmış sütunlar. Bu özellik hakkında daha fazla bilgi çıkış için özel meta veri özellikleri bölümünde yer [almaktadır.](#custom-metadata-properties-for-output) |
| Sistem Özelliği sütunları | İsteğe bağlı. Yük yerine giden iletiye eklenmesi gereken Sistem Özellikleri ve ilgili sütun adlarının anahtar değer çiftleri. Bu özellik hakkında daha fazla bilgi [Hizmet Veri Servisi Sırası ve Konu çıktıları için](#system-properties-for-service-bus-queue-and-topic-outputs) bölüm Sistemi özellikleri bulunmaktadır  |

Bölüm sayısı Servis [Veri Servisi SKU ve boyutuna dayanmaktadır.](../service-bus-messaging/service-bus-partitioning.md) Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değeridir.

## <a name="service-bus-topics"></a>Service Bus Konuları
Servis Veri Servisi kuyrukları gönderenden alıcıya bire bir iletişim yöntemi sağlar. [Service Bus konuları](https://msdn.microsoft.com/library/azure/hh367516.aspx) bire bir iletişim biçimi sağlar.

Aşağıdaki tabloda, Hizmet Veri Servisi konu çıktısı oluşturmak için özellik adları ve açıklamaları listelenistır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı |Sorgu çıktısını bu Hizmet Veri Servisi konusuna yönlendirmek için sorgularda kullanılan kolay bir ad. |
| Servis Veri Günü ad alanı |Bir dizi ileti varlıkları için bir kapsayıcı. Yeni bir olay hub'ı oluşturduğunuzda, bir Hizmet Veri Merkezi ad alanı da oluşturdunuz. |
| Konu adı |Konular, olay hub'larına ve kuyruklara benzer ileti varlıklarıdır. Cihazlardan ve hizmetlerden etkinlik akışları toplamak için tasarlanmıştır. Bir konu oluşturulduğunda, belirli bir ad da verilir. Bir konuya gönderilen iletiler, abonelik oluşturulmadığı sürece kullanılamaz, bu nedenle konunun altında bir veya daha fazla abonelik olduğundan emin olun. |
| Konu ilkesi adı |Bir Hizmet Veri Servisi konusu oluşturduğunuzda, konunun **Yapılandırma** sekmesinde paylaşılan erişim ilkeleri de oluşturabilirsiniz. Paylaşılan her erişim ilkesinin bir adı, belirlediğiniz izinler ve erişim anahtarları vardır. |
| Konu ilkesi anahtarı |Hizmet Veri Servisi ad alanına erişimi doğrulamak için kullanılan paylaşılan erişim anahtarı. |
| Olay serileştirme biçimi |Çıktı verileri için serileştirme biçimi. JSON, CSV ve Avro desteklenir. |
| Encoding |CSV veya JSON biçimini kullanıyorsanız, bir kodlama belirtilmelidir. UTF-8 şu anda desteklenen tek kodlama biçimidir. |
| Sınırlayıcı |Yalnızca CSV serileştirme için geçerlidir. Stream Analytics, csv formatında verileri seri hale getirmek için bir dizi ortak sınır layıcıyı destekler. Desteklenen değerler virgül, yarı sütun, boşluk, sekme ve dikey çubuk'tür. |
| Özellik sütunları | İsteğe bağlı. Yük yerine giden iletinin kullanıcı özellikleri olarak eklenmesi gereken virgülle ayrılmış sütunlar. Bu özellik hakkında daha fazla bilgi çıkış için özel meta veri özellikleri bölümünde yer [almaktadır.](#custom-metadata-properties-for-output) |
| Sistem Özelliği sütunları | İsteğe bağlı. Yük yerine giden iletiye eklenmesi gereken Sistem Özellikleri ve ilgili sütun adlarının anahtar değer çiftleri. Bu özellik hakkında daha fazla bilgi [Hizmet Veri Servisi Sırası ve Konu çıktıları için](#system-properties-for-service-bus-queue-and-topic-outputs) bölüm Sistemi özellikleri bulunmaktadır |

Bölüm sayısı Servis [Veri Servisi SKU ve boyutuna dayanmaktadır.](../service-bus-messaging/service-bus-partitioning.md) Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değeridir.

## <a name="azure-cosmos-db"></a>Azure Cosmos DB
[Azure Cosmos DB,](https://azure.microsoft.com/services/documentdb/) dünya çapında sınırsız elastik ölçek, zengin sorgu ve şema-agnostik veri modelleri üzerinden otomatik dizin oluşturma sunan, küresel olarak dağıtılan bir veritabanı hizmetidir. Akış Analizi için Azure Cosmos DB kapsayıcı seçenekleri hakkında bilgi edinmek için, çıktı makalesi [olarak Azure Cosmos DB ile Akış Analizi'ne](stream-analytics-documentdb-output.md) bakın.

Stream Analytics'in Azure Cosmos DB çıktısı şu anda Azure China 21Vianet ve Azure Almanya (T-Systems International) bölgelerinde kullanılamıyor.

> [!Note]
> Şu anda Azure Akış Analizi yalnızca SQL API'sini kullanarak Azure Cosmos DB'ye bağlantıyı destekler.
> Diğer Azure Cosmos DB API'leri henüz desteklenmez. Azure Akış Analitiği'ni diğer API'lerle oluşturulan Azure Cosmos DB hesaplarına yönlendirerseniz, veriler düzgün şekilde depolanamayabilir.

Aşağıdaki tabloda, Azure Cosmos DB çıktısı oluşturma özellikleri açıklanmaktadır.

| Özellik adı | Açıklama |
| --- | --- |
| Çıktı diğer adı | Akış Analizi sorgunuzda bu çıktıyı ifade eden bir diğer ad. |
| Havuz | Azure Cosmos DB. |
| İçeri aktarma seçeneği | **Aboneliğinizden Cosmos DB'yi seçin** veya **Cosmos DB ayarlarını el ile sağlayın.**
| Hesap Kimliği | Azure Cosmos DB hesabının adı veya bitiş noktası URI. |
| Hesap anahtarı | Azure Cosmos DB hesabının paylaşılan erişim anahtarı. |
| Database | Azure Cosmos DB veritabanı adı. |
| Kapsayıcı adı | Kullanılacak konteyner adı, Cosmos DB'de bulunması gerekir. Örnek:  <br /><ul><li> _MyContainer_: "MyContainer" adlı bir kapsayıcı bulunmalıdır.</li>|
| Belge Kimliği |İsteğe bağlı. Ekleme veya güncelleştirme işlemlerinin temel aldığı birincil anahtarı belirtmek için kullanılan çıktı olaylarında alanın adı.

## <a name="azure-functions"></a>Azure İşlevleri
Azure İşlevler, altyapıyı açıkça sağlamanız veya yönetmek zorunda kalmadan isteğe bağlı kodu çalıştırmak için kullanabileceğiniz sunucusuz bir bilgi işlem hizmetidir. Azure veya iş ortağı hizmetlerinde meydana gelen olaylar tarafından tetiklenen kodu uygulamanıza olanak tanır. Azure Fonksiyonlarının tetikleyicilere yanıt verebilme özelliği, azure akış analizi için doğal bir çıktı dır. Bu çıktı bağdaştırıcısı, kullanıcıların Akış Analizi'ni Azure İşlevleri'ne bağlamasına ve çeşitli olaylara yanıt olarak bir komut dosyası veya kod parçası çalıştırmasına olanak tanır.

Stream Analytics'in Azure İşlevler çıktısı şu anda Azure China 21Vianet ve Azure Almanya (T-Systems International) bölgelerinde kullanılamıyor.

Azure Akış Analizi, HTTP tetikleyicileri aracılığıyla Azure Fonksiyonlarını çağırır. Azure İşlevler çıktı bağdaştırıcısı aşağıdaki yapılandırılabilir özelliklere sahiptir:

| Özellik adı | Açıklama |
| --- | --- |
| İşlev uygulaması |Azure İşlevler uygulamanızın adı. |
| İşlev |Azure İşlevler uygulamanızdaki işlevin adı. |
| Anahtar |Başka bir abonelikten bir Azure İşlevi kullanmak istiyorsanız, bunu işlevinize erişmek için gereken anahtarı sağlayarak yapabilirsiniz. |
| Maksimum toplu iş boyutu |Azure işlevinize gönderilen her çıktı toplu işlemi için maksimum boyutu ayarlamanızı sağlayan bir özellik. Giriş birimi baytlarda. Varsayılan olarak, bu değer 262.144 bayt (256 KB) olur. |
| Maksimum toplu iş sayısı  |Azure İşlevlerine gönderilen her toplu iş partisinde en fazla olay sayısını belirtmenize olanak tanıyan bir özellik. Varsayılan değer 100’dür. |

Azure Akış Analizi, başarıyla işlenen toplu işler için Işlevler uygulamasından HTTP durumu 200 bekler.

Azure Akış Analizi, bir Azure işlevinden 413 ("http İstek Varlığı Çok Büyük") özel durumu aldığında, Azure İşlevlerine gönderdiği toplu iş sayısının boyutunu azaltır. Azure işlev kodunuzda, Azure Akış Analizi'nin büyük toplu iş göndermediğinden emin olmak için bu özel durumu kullanın. Ayrıca, işlevde kullanılan maksimum toplu iş sayısı ve boyut değerlerinin Akış Analizi portalında girilen değerlerle tutarlı olduğundan emin olun.

> [!NOTE]
> Test bağlantısı sırasında, Akış Analizi, ikisi arasındaki bağlantının çalışıp çalışmamasını test etmek için Azure İşlevleri'ne boş bir toplu iş gönderir. Test bağlantısının geçtiğinden emin olmak için Fonksiyonlar uygulamanızın boş toplu iş isteklerini işlediğinden emin olun.

Ayrıca, bir zaman penceresinde olay iniş olmayan bir durumda, hiçbir çıkış oluşturulur. Sonuç olarak, **computeResult** işlevi çağrılmaz. Bu davranış, yerleşik pencereli toplu işlevlerle tutarlıdır.

## <a name="custom-metadata-properties-for-output"></a>Çıktı için özel meta veri özellikleri 

Giden iletilerinize kullanıcı özellikleri olarak sorgu sütunları ekleyebilirsiniz. Bu sütunlar yüke girmiyor. Özellikler, çıktı iletisinde sözlük biçiminde bulunur. *Anahtar* sütun adıdır ve *değer* özellikler sözlüğündeki sütun değeridir. Kayıt ve Dizi dışında tüm Akış Analizi veri türleri desteklenir.  

Desteklenen çıktılar: 
* Service Bus kuyruğu 
* Service Bus konusu 
* Olay hub'ı 

Aşağıdaki örnekte, iki alanı `DeviceId` ve `DeviceStatus` meta verilere ekliyoruz. 
* Sorgu:`select *, DeviceId, DeviceStatus from iotHubInput`
* Çıktı yapılandırması:`DeviceId,DeviceStatus`

![Özellik sütunları](./media/stream-analytics-define-outputs/10-stream-analytics-property-columns.png)

Aşağıdaki ekran görüntüsü, Service Bus Explorer aracılığıyla EventHub'da denetlenen çıktı [iletisi](https://github.com/paolosalvatori/ServiceBusExplorer)özelliklerini gösterir.

![Olay özel özellikleri](./media/stream-analytics-define-outputs/09-stream-analytics-custom-properties.png)

## <a name="system-properties-for-service-bus-queue-and-topic-outputs"></a>Servis Veri Servisi Sırası ve Konu çıktıları için sistem özellikleri 
Giden servis veri servisi Sıranız veya Konu iletilerinize [sistem özellikleri](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties) olarak sorgu sütunları ekleyebilirsiniz. Bu sütunlar taşıma yerine ilgili BrokeredMessage [sistem özelliği](https://docs.microsoft.com/dotnet/api/microsoft.servicebus.messaging.brokeredmessage?view=azure-dotnet#properties) sorgu sütun değerleri ile doldurulur yük içine gitmez.
Bu sistem özellikleri desteklenir - `MessageId, ContentType, Label, PartitionKey, ReplyTo, SessionId, CorrelationId, To, ForcePersistence, TimeToLive, ScheduledEnqueueTimeUtc`.
Bu sütunların dize değerleri karşılık gelen sistem özellik değeri türü olarak ayrıştırılır ve herhangi bir ayrıştırma hataları veri hatası olarak kabul edilir.
Bu alan JSON nesne biçimi olarak sağlanır. Bu format la ilgili ayrıntılar aşağıdaki gibidir -
* Kıvırcık parantezile {}çevrili.
* Anahtar/değer çiftleri ile yazılır.
* Anahtarlar ve değerler dizeleri olmalıdır.
* Anahtar sistem özellik adı ve değer sorgu sütun adıdır.
* Anahtarlar ve değerler bir iki nokta üst üste ayrılır.
* Her anahtar/değer çifti virgülle ayrılır.

Bu özellik nasıl kullanılacağını gösterir -

* Sorgu:`select *, column1, column2 INTO queueOutput FROM iotHubInput`
* Sistem Özellik Sütunları:`{ "MessageId": "column1", "PartitionKey": "column2"}`

Bu, `MessageId` servis veri yolundaki sıra iletilerini ''nin değerleri `column2`yle ve PartitionKey 'in değerleri yle ayarlanır. `column1`

## <a name="partitioning"></a>Bölümleme

Aşağıdaki tabloda bölüm desteği ve her çıkış türü için çıktı yazarlarının sayısı özetlenebilir:

| Çıkış türü | Bölümleme desteği | Bölüm anahtarı  | Çıktı yazar sayısı |
| --- | --- | --- | --- |
| Azure Data Lake Store | Evet | Yol öneki deseninde {date} ve {time} belirteçlerini kullanın. YYYY/MM/DD, DD/MM/YYYY veya MM-DD-YYYY gibi tarih biçimini seçin. HH zaman biçimi için kullanılır. | [Tamamen paralelleştirilebilir sorgular](stream-analytics-scale-jobs.md)için giriş bölümleme izler. |
| Azure SQL Veritabanı | Evet, etkinleştirilmesi gerekiyor. | Sorgudaki PARTITION BY yan tümcesini temel alın. | Devralma Bölümleme seçeneği etkinleştirildiğinde, [tam paralelleştirilebilir sorgular](stream-analytics-scale-jobs.md)için giriş bölümleme izler. Verileri Azure SQL Veritabanı'na yüklerken daha iyi yazma verim performansı elde etme hakkında daha fazla bilgi edinmek için Azure Akış Analizi çıktısı için Azure [SQL Veritabanı'na](stream-analytics-sql-output-perf.md)bakın. |
| Azure Blob depolama | Evet | Yol desenindeki olay alanlarınızdan {date} ve {time} belirteçlerini kullanın. YYYY/MM/DD, DD/MM/YYYY veya MM-DD-YYYY gibi tarih biçimini seçin. HH zaman biçimi için kullanılır. Blob çıktısı tek bir özel olay özniteliği {fieldname} veya\<{datetime:> belirtici ile bölümlenebilir. | [Tamamen paralelleştirilebilir sorgular](stream-analytics-scale-jobs.md)için giriş bölümleme izler. |
| Azure Event Hubs | Evet | Evet | Bölüm hizalanmasına bağlı olarak değişir.<br /> Olay hub çıktısı için bölüm anahtarı yukarı (önceki) sorgu adımıyla eşit olarak hizalandığında, yazar sayısı olay merkezi çıkışındaki bölüm sayısıyla aynıdır. Her yazar olayları belirli bölüme göndermek için [EventHubSender sınıfını](/dotnet/api/microsoft.servicebus.messaging.eventhubsender?view=azure-dotnet) kullanır. <br /> Olay hub çıktısı için bölüm anahtarı yukarı (önceki) sorgu adımı yla hizalanmadığında, yazar sayısı o önceki adımdaki bölüm sayısıyla aynıdır. Her yazar, tüm çıktı bölümlerine olay göndermek için **EventHubClient'daki** [SendBatchAsync sınıfını](/dotnet/api/microsoft.servicebus.messaging.eventhubclient.sendasync?view=azure-dotnet) kullanır. |
| Power BI | Hayır | None | Geçerli değildir. |
| Azure Tablo depolama | Evet | Herhangi bir çıkış sütunu.  | [Tam paralelleştirilmiş sorgular](stream-analytics-scale-jobs.md)için giriş bölümleme izler. |
| Azure Service Bus konusu | Evet | Otomatik olarak seçilir. Bölüm sayısı [Servis Veri Servisi SKU ve boyutuna](../service-bus-messaging/service-bus-partitioning.md)dayanmaktadır. Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değeridir.| Çıktı konusubölüm sayısıyla aynıdır.  |
| Azure Service Bus kuyruğu | Evet | Otomatik olarak seçilir. Bölüm sayısı [Servis Veri Servisi SKU ve boyutuna](../service-bus-messaging/service-bus-partitioning.md)dayanmaktadır. Bölüm anahtarı, her bölüm için benzersiz bir tamsayı değeridir.| Çıkış kuyruğundaki bölüm sayısıyla aynı. |
| Azure Cosmos DB | Evet | Sorgudaki PARTITION BY yan tümcesini temel alın. | [Tam paralelleştirilmiş sorgular](stream-analytics-scale-jobs.md)için giriş bölümleme izler. |
| Azure İşlevleri | Evet | Sorgudaki PARTITION BY yan tümcesini temel alın. | [Tam paralelleştirilmiş sorgular](stream-analytics-scale-jobs.md)için giriş bölümleme izler. |

Çıktı yazarlarının sayısı da sorgunuzdaki `INTO <partition count>` [(INTO)](https://docs.microsoft.com/stream-analytics-query/into-azure-stream-analytics#into-shard-count)yan tümcesi kullanılarak denetlenebilir ve bu da istenen bir iş topolojisine ulaşmada yararlı olabilir. Çıktı bağdaştırıcınız bölümlenmiyorsa, tek bir giriş bölmesinde veri olmaması geç varış süresine kadar gecikmeye neden olur. Bu gibi durumlarda, çıktı tek bir yazarla birleştirilir ve bu da ardışık ardınızda darboğazlara neden olabilir. Geç varış politikası hakkında daha fazla bilgi edinmek için [Azure Akışı Analytics etkinlik siparişi hususlarına](stream-analytics-out-of-order-and-late-events.md)bakın.

## <a name="output-batch-size"></a>Çıktı toplu iş boyutu
Azure Akış Analizi, olayları işlemek ve çıktılara yazmak için değişken boyutlu toplu iş serileri kullanır. Genellikle Stream Analytics motoru aynı anda tek bir ileti yazmaz ve verimlilik için toplu iş kullanır. Hem gelen hem de giden olayların oranı yüksekolduğunda, Stream Analytics daha büyük toplu iş ler kullanır. Çıkış oranı düşük olduğunda, gecikmeyi düşük tutmak için daha küçük toplu iş ler kullanır.

Aşağıdaki tabloda çıktı toplu işlemesi için dikkat edilmesi gereken noktalardan bazıları açıklanmaktadır:

| Çıkış türü | Maksimum ileti boyutu | Toplu boyut optimizasyonu |
| :--- | :--- | :--- |
| Azure Data Lake Store | [Bkz. Veri Gölü Depolama sınırları.](../azure-resource-manager/management/azure-subscription-service-limits.md#data-lake-store-limits) | Yazma işlemi başına 4 MB'a kadar kullanın. |
| Azure SQL Veritabanı | Max toplu iş sayısı kullanılarak yapılandırılabilir. Varsayılan olarak tek toplu uç başına 10.000 maksimum ve 100 minimum satır.<br />Bkz. [Azure SQL sınırları.](../sql-database/sql-database-resource-limits.md) |  Her toplu iş başlangıçta en fazla toplu madde sayısı ile eklenir. Toplu iş, SQL'den yeniden denilebilir hatalara göre ikiye bölünür (minimum toplu iş sayısına kadar). |
| Azure Blob depolama | Bkz. [Azure Depolama sınırları.](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits) | Maksimum blob blok boyutu 4 MB'dır.<br />Maksimum damla bock sayısı 50.000'dir. |
| Azure Event Hubs  | İleti başına 256 KB veya 1 MB. <br />Bkz. [Olay Hub'ları sınırları.](../event-hubs/event-hubs-quotas.md) |  Giriş/çıktı bölümleme hizalı olmadığında, her olay ayrı ayrı `EventData` paketlenir ve maksimum ileti boyutuna kadar bir toplu iş halinde gönderilir. Özel meta [veri özellikleri](#custom-metadata-properties-for-output) kullanılırsa da bu olur. <br /><br />  Giriş/çıktı bölümleme hizalandığında, birden çok olay en `EventData` yüksek ileti boyutuna kadar tek bir örnek halinde paketlenir ve gönderilir. |
| Power BI | Bkz. [Güç BI Rest API sınırları.](https://msdn.microsoft.com/library/dn950053.aspx) |
| Azure Tablo depolama | Bkz. [Azure Depolama sınırları.](../azure-resource-manager/management/azure-subscription-service-limits.md#storage-limits) | Varsayılan değer, tek bir hareket başına 100 varlıktır. Gerektiğinde daha küçük bir değere göre yapılandırabilirsiniz. |
| Azure Service Bus kuyruğu   | Standart katman için mesaj başına 256 KB, Premium katman için 1MB.<br /> [Bkz. Servis Veri Günü sınırları.](../service-bus-messaging/service-bus-quotas.md) | İleti başına tek bir olay kullanın. |
| Azure Service Bus konusu | Standart katman için mesaj başına 256 KB, Premium katman için 1MB.<br /> [Bkz. Servis Veri Günü sınırları.](../service-bus-messaging/service-bus-quotas.md) | İleti başına tek bir olay kullanın. |
| Azure Cosmos DB   | [Azure Cosmos DB sınırlarını](../azure-resource-manager/management/azure-subscription-service-limits.md#azure-cosmos-db-limits)görün. | Toplu iş boyutu ve yazma sıklığı, Azure Cosmos DB yanıtları temel alınca dinamik olarak ayarlanır. <br /> Stream Analytics'in önceden belirlenmiş sınırlamaları yoktur. |
| Azure İşlevleri   | | Varsayılan toplu iş boyutu 262.144 bayt (256 KB) 'dir. <br /> Toplu iş başına varsayılan olay sayısı 100'dür. <br /> Toplu iş boyutu yapılandırılabilir ve Stream Analytics [çıkış seçeneklerinde](#azure-functions)artırılabilir veya azaltılabilir.

## <a name="next-steps"></a>Sonraki adımlar
> [!div class="nextstepaction"]
> 
> [Hızlı başlangıç: Azure portalını kullanarak Stream Analytics işi oluşturma](stream-analytics-quick-create-portal.md)

<!--Link references-->
[stream.analytics.developer.guide]: ../stream-analytics-developer-guide.md
[stream.analytics.scale.jobs]: stream-analytics-scale-jobs.md
[stream.analytics.introduction]: stream-analytics-introduction.md
[stream.analytics.get.started]: stream-analytics-real-time-fraud-detection.md
[stream.analytics.query.language.reference]: https://go.microsoft.com/fwlink/?LinkID=513299
[stream.analytics.rest.api.reference]: https://go.microsoft.com/fwlink/?LinkId=517301
