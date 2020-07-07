---
title: Azure Depolamaya Giriş - Azure’da bulut depolama | Microsoft Docs
description: Temel Azure depolama platformu, Microsoft 'un bulut depolama çözümüdür. Azure Depolama yüksek oranda kullanılabilir, güvenli, sağlam, yüksek düzeyde ölçeklenebilir ve yedekli veri nesneleri için depolama alanı sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 04/08/2020
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 81ee07eb41df6d8d663510913572b829feffd995
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "82133789"
---
# <a name="introduction-to-the-core-azure-storage-services"></a>Temel Azure depolama hizmetlerine giriş

Azure depolama platformu, Microsoft 'un modern veri depolama senaryolarına yönelik bulut depolama çözümüdür. Çekirdek Depolama Hizmetleri veri nesneleri için yüksek düzeyde ölçeklenebilir bir nesne deposu, Azure sanal makineler için disk depolama alanı (VM 'Ler), bulut için bir dosya sistemi hizmeti, güvenilir mesajlaşma için bir mesajlaşma deposu ve bir NoSQL deposu sunar. Hizmetler şunlardır:

- **Dayanıklı ve yüksek oranda kullanılabilir.** Yedeklilik, verilerinizin geçici donanım hataları durumunda güvende kalmasını sağlar. Ayrıca, yerel felaketler ya da doğal afetlere karşı ek koruma için verileri meri merkezleri ya da coğrafi bölgeler arasında çoğaltmayı seçebilirsiniz. Bu şekilde çoğaltılan veriler beklenmeyen bir kesinti durumunda yüksek oranda kullanılabilir olmaya devam eder.
- **Güvende.** Bir Azure depolama hesabına yazılan tüm veriler hizmet tarafından şifrelenir. Azure Depolama, verilerinize erişmesi gereken kişiler üzerinde ayrıntılı denetime sahip olmanızı sağlar.
- **Ölçeklenebilir.** Azure Depolama, günümüzün uygulamalarına ait veri depolama ve performans gereksinimlerini karşılamak için yüksek düzeyde ölçeklenebilir şekilde tasarlanmıştır.
- **Lebilmesi.** Azure, donanım bakımını, güncelleştirmeleri ve kritik sorunları sizin için işler.
- **Erişilebilir.** Azure Depolama’daki verilere dünyanın herhangi bir yerinden HTTP ya da HTTPS aracılığıyla erişilebilir. Microsoft, Azure depolama için, .NET, Java, Node.js, Python, PHP, Ruby, Go ve diğerleri gibi çeşitli dillerde istemci kitaplıkları sağlar ve bu da yetişkinlere yönelik REST API. Azure Depolama, Azure PowerShell veya Azure CLI’de betik oluşturmayı destekler. Azure portalı ve Azure Depolama Gezgini ise verilerinizle çalışmaya yönelik kolay görsel çözümler sunar.  

## <a name="core-storage-services"></a>Çekirdek Depolama Hizmetleri

Azure Storage platformu aşağıdaki veri hizmetlerini içerir:

- [Azure Blobları](../blobs/storage-blobs-introduction.md): Metin ve ikili veriler için yüksek düzeyde ölçeklenebilir nesne deposu. Ayrıca Data Lake Storage 2. aracılığıyla büyük veri analizi desteğini de içerir.
- [Azure Dosyaları](../files/storage-files-introduction.md): Bulut veya şirket içi dağıtımlar için yönetilen dosya paylaşımları.
- [Azure Kuyrukları](../queues/storage-queues-introduction.md): Uygulama bileşenleri arasında güvenilir mesajlaşmaya yönelik bir mesajlaşma deposu.
- [Azure Tabloları](../tables/table-storage-overview.md): Yapılandırılmış verilerin şemasız depolanmasına yönelik bir NoSQL deposu.
- [Azure diskleri](../../virtual-machines/windows/managed-disks-overview.md): Azure VM 'ler için blok düzeyinde depolama birimleri.

Her hizmete bir depolama hesabı aracılığıyla erişilir. Başlamak için bkz. [Depolama hesabı oluşturma](storage-account-create.md).

## <a name="example-scenarios"></a>Örnek senaryolar

Aşağıdaki tabloda dosyalar, Bloblar, diskler, kuyruklar ve tablolar karşılaştırılmaktadır ve her biri için örnek senaryolar gösterilmiştir.

| Özellik | Description | Kullanılması gereken durumlar |
|--------------|-------------|-------------|
| **Azure Dosyaları** |, Sektör standart sunucu Ileti bloğu (SMB) protokolü aracılığıyla herhangi bir yerden erişebileceğiniz tam olarak yönetilen bulut dosya paylaşımları sunar.<br><br>Azure dosya paylaşımlarını buluttan veya Windows, Linux ve macOS 'ın şirket içi dağıtımlarından bağlayabilirsiniz. | Azure 'da çalışan ve diğer uygulamalar arasında veri paylaşmak için yerel dosya sistemi API 'Lerini zaten kullanan bir uygulamayı buluta "kaldırmak ve kaydırmak" istiyorsunuz.<br/><br/>Şirket içi dosya sunucularını veya NAS cihazlarını değiştirmek veya eklemek istiyorsunuz.<br><br> Birçok sanal makineden erişilmesi gereken geliştirme ve hata ayıklama araçlarını depolamak istiyorsunuz. |
| **Azure Blobları** | Yapılandırılmamış verilerin, Blok Bloblarında çok büyük ölçekte depolanmasına ve erişilmesine izin verir.<br/><br/>Ayrıca kurumsal büyük veri analizi çözümleri için [Azure Data Lake Storage 2.](../blobs/data-lake-storage-introduction.md) destekler. | Uygulamanızın akış ve rastgele erişim senaryolarını desteklemesini istiyorsunuz.<br/><br/>Uygulama verilerine dilediğiniz yerden erişebilmek istiyorsunuz.<br/><br/>Azure 'da bir kurumsal veri Gölü oluşturmak ve büyük veri analizi gerçekleştirmek istiyorsunuz. |
| **Azure Diskleri** | Verilerin kalıcı olarak depolanmasına ve bağlı bir sanal sabit diskten erişilmesine izin verir. | Kalıcı disklere veri okumak ve yazmak için yerel dosya sistemi API 'Leri kullanan uygulamaları "kaldırmak ve kaydırmak" istiyorsunuz.<br/><br/>Diskin eklendiği sanal makinenin dışından erişilmesi gerekmeyen verileri depolamak istiyorsunuz.. |
| **Azure Kuyrukları** | Uygulama bileşenleri arasında zaman uyumsuz ileti sıraya alma olanağı sağlar. | Uygulama bileşenlerini ayırmak ve aralarında iletişim kurmak için zaman uyumsuz mesajlaşma kullanmak istiyorsunuz.<br><br>Kuyruk depolama alanını Service Bus kuyruklardan ne zaman kullanacağınızı öğrenmek için bkz. [depolama kuyrukları ve Service Bus kuyrukları-karşılaştırılan ve](https://docs.microsoft.com/azure/service-bus-messaging/service-bus-azure-and-service-bus-queues-compared-contrasted)benzerlikler. |
| **Azure Tabloları** | Yapılandırılmış NoSQL verilerini bulutta depolamanıza izin vererek, şemaya daha az tasarıma sahip bir anahtar/öznitelik deposu sağlayabilirsiniz. | Web uygulamaları için Kullanıcı verileri, adres defterleri, cihaz bilgileri veya hizmetinizin gerektirdiği diğer meta veri türleri gibi esnek veri kümelerini depolamak istiyorsunuz. <br/><br/>Azure Cosmos DB Tablo API'si tablo depolamanın ne zaman kullanılacağı konusunda rehberlik için, bkz. [Azure Cosmos DB tablo API'si ve Azure Tablo depolama Ile geliştirme](../../cosmos-db/table-support.md). |

## <a name="blob-storage"></a>Blob depolama

Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir.

Blob depolama aşağıdaki durumlar için idealdir:

- Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
- Dağıtılan erişim için dosyaların depolanması.
- Video ve ses akışları.
- Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
- Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Blob depolamadaki nesnelere, dünyanın her yerinden HTTP veya HTTPS üzerinden erişilebilir. Kullanıcılar veya istemci uygulamalar, bloblara URL’ler, [Azure Depolama REST API’si](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) veya bir Azure Depolama istemci kitaplığı aracılığıyla erişebilir. Depolama istemci kitaplıkları [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), [Java](https://docs.microsoft.com/java/api/overview/azure/storage), [Node.js](https://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/), [PHP](https://azure.github.io/azure-storage-php/) ve [Ruby](https://azure.github.io/azure-storage-ruby) dahil olmak üzere birden çok dil ile kullanılabilir.

BLOB depolama hakkında daha fazla bilgi için bkz. [BLOB depolamaya giriş](../blobs/storage-blobs-introduction.md).

## <a name="azure-files"></a>Azure Dosyaları

[Azure Dosyaları](../files/storage-files-introduction.md), standart Sunucu İleti Bloğu (SMB) protokolü kullanılarak erişilebilen yüksek oranda kullanılabilir ağ dosya paylaşımları oluşturmanıza olanak tanır. Bu durum, birden fazla VM’nin hem okuma hem de yazma erişimi ile aynı dosyaları paylaşabildiği anlamına gelir. Dosyaları REST arabirimi veya depolama istemci kitaplıkları kullanarak da okuyabilirsiniz.

Dosyalara dünyanın herhangi bir yerinden dosyayı işaret eden ve paylaşılan erişim imzası (SAS) belirteci içeren bir URL kullanarak erişebilme olanağı, Azure Dosyaları'nı kurumsal bir dosya paylaşımındaki dosyalardan ayıran özelliklerden biridir. SAS belirteçleri oluşturabilirsiniz; bunlar, belirli bir süre için özel bir varlığa belirli bir erişim izni verir.

Dosya paylaşımları için birçok yaygın senaryoda kullanılabilir:

- Birçok şirket içi uygulama, dosya paylaşımlarını kullanır. Bu özellik, veri paylaşan uygulamaları Azure’a geçirmeyi kolaylaştırır. Dosya paylaşımını şirket içi uygulamanın kullandığı sürücü harfine bağlarsanız, uygulamanızın dosya paylaşımına erişen kısmı, varsa minimum değişikliklerle çalışır.

- Yapılandırma dosyaları bir dosya paylaşımında depolanabilir ve birden çok VM tarafından bu dosyalara erişilebilir. Bir gruptaki birden fazla geliştirici tarafından kullanılan araç ve yardımcı programlar, herkesin bulabilmesi ve aynı sürümü kullandıklarından emin olunması için bir dosya paylaşımında depolanabilir.

- Kaynak günlükleri, ölçümler ve kilitlenme dökümleri, bir dosya paylaşımında yazılabilen ve daha sonra işlenen ya da çözümlenen verilerin yalnızca üç örneğidir.

Azure Dosyaları hakkında daha fazla bilgi için bkz. [Azure Dosyaları'na Giriş](../files/storage-files-introduction.md).

Bazı SMB özellikleri bulut için geçerli değildir. Daha fazla bilgi için bkz. [Azure dosya hizmeti tarafından desteklenmeyen özellikler](/rest/api/storageservices/features-not-supported-by-the-azure-file-service).

## <a name="queue-storage"></a>Kuyruk depolama

Azure Kuyruk hizmeti, iletileri depolamak ve almak için kullanılır. Kuyruk iletilerinin boyutu 64 KB'ye kadar olabilir ve bir kuyruk, milyonlarca ileti içerebilir. Kuyruklar, genellikle zaman uyumsuz olarak işlenecek ileti listelerini depolamak için kullanılır.

Örneğin, müşterilerinizin resimleri karşıya yükleyebilmesini ve her resmin küçük resimlerini oluşturabilmesini istediğinizi düşünelim. Müşterinizin resimleri karşıya yüklerken küçük resimleri oluşturmanızı beklemesini sağlayabilirsiniz. Alternatif olarak bir kuyruk kullanabilirsiniz. Müşteri karşıya yüklemeyi tamamladığında, kuyruğa bir ileti yazın. Ardından Azure İşlevinin iletiyi kuyruktan alıp ve küçük resimleri oluşturmasını sağlayın. Bu işlemin tüm bölümleri ayrıca ölçeklendirilebileceğinden kullanımınız için ayarlarken daha fazla kontrol sunar.

Azure Kuyrukları hakkında daha fazla bilgi için bkz. [Kuyruklara Giriş](../queues/storage-queues-introduction.md).

## <a name="table-storage"></a>Tablo depolama

Azure Tablo depolama artık Azure Cosmos DB’nin bir parçasıdır. Azure Tablo depolama belgelerini görmek için bkz. [Azure Tablo Depolamaya Genel Bakış](../tables/table-storage-overview.md). Mevcut Azure Tablo depolama hizmetine ek olarak, aktarım hızı için iyileştirilmiş tablolar, genel dağıtım ve otomatik ikincil dizinler sağlayan yeni bir Azure Cosmos DB Tablo API'si teklifi vardır. Daha fazla bilgi edinmek ve yeni Premium deneyimi denemek için bkz. [Azure Cosmos DB tablo API'si](https://aka.ms/premiumtables).

Tablo depolama hakkında daha fazla bilgi için bkz. [Azure Tablo depolamaya genel bakış](../tables/table-storage-overview.md).

## <a name="disk-storage"></a>Disk depolama

Azure yönetilen disk, bir sanal sabit disktir (VHD). Bunu, şirket içi sunucuda bir fiziksel disk gibi düşünebilirsiniz, ancak sanallaştırılabilir. Azure tarafından yönetilen diskler, Azure 'da rastgele bir GÇ depolama nesnesi olan sayfa Blobları olarak depolanır. Sayfa Blobları, blob kapsayıcıları ve Azure depolama hesapları üzerinde bir soyutlama olduğundan, yönetilen bir disk ' Managed ' çağrısı yaptık. Yönetilen disklerle, tüm yapmanız gereken diski temin etmek ve Azure 'un geri kalanını yapması gerekir.

Yönetilen diskler hakkında daha fazla bilgi için bkz. [Azure yönetilen disklere giriş](../../virtual-machines/windows/managed-disks-overview.md).

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

Azure depolama, birkaç depolama hesabı türü sunar. Her tür farklı özellikleri destekler ve kendi fiyatlandırma modeline sahiptir. Depolama hesabı türleri hakkında daha fazla bilgi için bkz. [Azure depolama hesabına genel bakış](storage-account-overview.md).

## <a name="secure-access-to-storage-accounts"></a>Depolama hesaplarına güvenli erişim

Azure depolama 'ya yönelik her isteğin yetkilendirilmiş olması gerekir. Azure depolama aşağıdaki yetkilendirme yöntemlerini destekler:

- **Blob ve kuyruk verileri için Azure Active Directory (Azure AD) tümleştirmesi.** Azure depolama, rol tabanlı erişim denetimi (RBAC) aracılığıyla blob ve sıra Hizmetleri için Azure AD ile kimlik doğrulaması ve yetkilendirmeyi destekler. Azure AD ile istekleri yetkilendirmek, üstün güvenlik ve kullanım kolaylığı için önerilir. Daha fazla bilgi için bkz. [Azure Active Directory kullanarak Azure bloblarına ve kuyruklara erişim yetkisi verme](storage-auth-aad.md).
- **Azure dosyaları için SMB üzerinden Azure AD yetkilendirmesi.** Azure dosyaları, Azure Active Directory Domain Services (Azure AD DS) veya şirket içi Active Directory Domain Services (Önizleme) aracılığıyla SMB (sunucu Ileti bloğu) üzerinden kimlik tabanlı yetkilendirmeyi destekler. Etki alanına katılmış Windows VM 'leriniz, Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir. Daha fazla bilgi için bkz. [Azure dosyalarına genel bakış SMB erişimi için kimlik tabanlı kimlik doğrulama desteği](../files/storage-files-active-directory-overview.md) ve [bir Azure dosyaları dağıtımı için planlama](../files/storage-files-planning.md#identity).
- **Paylaşılan anahtarla yetkilendirme.** Azure Depolama Blobu, dosyalar, kuyruk ve tablo Hizmetleri, paylaşılan anahtarla yetkilendirmeyi destekler. Paylaşılan anahtar yetkilendirmesi kullanan bir istemci, depolama hesabı erişim anahtarı kullanılarak imzalanan her isteği içeren bir üst bilgi geçirir. Daha fazla bilgi için bkz. [paylaşılan anahtarla yetkilendirme](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key).
- **Paylaşılan erişim imzalarını (SAS) kullanarak yetkilendirme.** Paylaşılan erişim imzası (SAS), bir depolama kaynağının URI 'sine eklenebileceği bir güvenlik belirteci içeren bir dizedir. Güvenlik belirteci, izinler ve erişim aralığı gibi kısıtlamaları kapsüller. Daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma (SAS)](storage-sas-overview.md).
- **Kapsayıcılara ve bloblara anonim erişim.** Bir kapsayıcı ve Blobları herkese açık olabilir. Bir kapsayıcının veya Blobun genel olduğunu belirttiğinizde, herkes onu anonim olarak okuyabilir; kimlik doğrulaması gerekmiyor. Daha fazla bilgi için bkz. [Kapsayıcılara ve bloblara anonim okuma erişimini yönetme](../blobs/storage-manage-access-to-resources.md).

## <a name="encryption"></a>Şifreleme

Çekirdek Depolama Hizmetleri için kullanılabilen iki temel şifreleme türü vardır. Güvenlik ve şifreleme hakkında daha fazla bilgi için bkz. [Azure Depolama güvenlik kılavuzu](../blobs/security-recommendations.md).

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Azure depolama şifrelemesi, kurumsal güvenlik ve uyumluluk taahhütlerinizi karşılamak için verilerinizi korur ve korur. Azure depolama, depolama hesabına kalıcı yapmadan önce tüm verileri otomatik olarak şifreler ve alma işleminden önce şifresini çözer. Şifreleme, şifre çözme ve anahtar yönetim işlemleri kullanıcılara saydamdır. Müşteriler, Azure Key Vault kullanarak kendi anahtarlarını yönetmeyi de seçebilir. Daha fazla bilgi için bkz. [bekleyen veriler Için Azure depolama şifrelemesi](storage-service-encryption.md).

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

Azure depolama istemci kitaplıkları, istemci kitaplığından, verileri kablo üzerinden göndermeden ve yanıtın şifresini çözmeden önce şifrelemek için yöntemler sağlar. İstemci tarafı şifreleme aracılığıyla şifrelenen veriler de Azure Storage tarafından REST 'de şifrelenir. İstemci tarafı şifreleme hakkında daha fazla bilgi için bkz. [Azure depolama için .net Ile istemci tarafı şifreleme](storage-client-side-encryption.md).

## <a name="redundancy"></a>Yedeklilik

Verilerinizin dayanıklı olduğundan emin olmak için, Azure depolama verilerinizin birden çok kopyasını depolar. Depolama hesabınızı ayarlarken bir artıklık seçeneği belirleyin. Daha fazla bilgi için bkz. [Azure Depolama yedekliliği](/azure/storage/common/storage-redundancy?toc=/azure/storage/blobs/toc.json).

## <a name="transfer-data-to-and-from-azure-storage"></a>Azure Storage 'a veri aktarma

Verileri Azure depolama alanına veya dışına taşımaya yönelik çeşitli seçenekleriniz vardır. Seçtiğiniz seçenek, veri kümenizin boyutuna ve ağ bant genişliğine bağlıdır. Daha fazla bilgi için bkz. [veri aktarımı için bir Azure çözümü seçme](storage-choose-data-transfer-solution.md).

## <a name="pricing"></a>Fiyatlandırma

Verilerinizin nasıl depolandığı ve erişildiği hakkında kararlar verirken, dahil edilen maliyetleri de göz önünde bulundurmanız gerekir. Daha fazla bilgi için bkz. [Azure Depolama fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/).

## <a name="storage-apis-libraries-and-tools"></a>Depolama API'leri, kitaplıklar ve araçlar

Bir depolama hesabındaki kaynaklara, HTTP/HTTPS istekleri yapan herhangi bir dilde erişebilirsiniz. Ayrıca, çekirdek Azure Storage Hizmetleri, çeşitli popüler diller için programlama kitaplıkları sunar. Bu kitaplıklar zaman uyumlu ve zaman uyumsuz çağrılar, işlemleri gruplama, özel durum yönetimi, otomatik yeniden denemeler, işlemsel davranışlar vb. ayrıntıları ele alarak Azure Storage ile çalışmanın çoğu yönünü basitleştirir. Kitaplıklar şu anda aşağıdaki diller için mevcuttur ve yeni kitaplıklar geliştirme aşamasındadır:

### <a name="azure-storage-data-api-and-library-references"></a>Azure Depolama veri API’si ve kitaplık başvuruları

- [Azure Depolama REST API](https://docs.microsoft.com/rest/api/storageservices/)
- [.NET için Azure depolama istemci kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
- [Java/Android için Azure depolama istemci kitaplığı](https://docs.microsoft.com/java/api/overview/azure/storage)
- [Node.jsiçin Azure depolama istemci kitaplığı](https://docs.microsoft.com/javascript/api/overview/azure/storage-overview)
- [Python için Azure depolama istemci kitaplığı](https://github.com/Azure/azure-storage-python)
- [PHP için Azure depolama istemci kitaplığı](https://github.com/Azure/azure-storage-php)
- [Ruby için Azure depolama istemci kitaplığı](https://github.com/Azure/azure-storage-ruby)
- [C++ için Azure depolama istemci kitaplığı](https://github.com/Azure/azure-storage-cpp)

### <a name="azure-storage-management-api-and-library-references"></a>Azure Depolama yönetimi API’si ve kitaplık başvuruları

- [Depolama Kaynak Sağlayıcısı REST API’si](https://docs.microsoft.com/rest/api/storagerp/)
- [.NET için Depolama Kaynak Sağlayıcısı İstemci Kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/storage/management)
- [Depolama Hizmet Yönetimi REST API'si (Klasik)](https://msdn.microsoft.com/library/azure/ee460790.aspx)

### <a name="azure-storage-data-movement-api-and-library-references"></a>Azure Depolama veri taşıma API’si ve kitaplık başvuruları

- [Depolama İçeri/Dışarı Aktarma Hizmeti REST API’si](https://docs.microsoft.com/rest/api/storageimportexport/)
- [.NET için Depolama Veri Taşıma İstemci Kitaplığı](/dotnet/api/microsoft.azure.storage.datamovement)

### <a name="tools-and-utilities"></a>Araçlar ve Yardımcı Programlar

- [Depolama için Azure PowerShell Cmdlet'leri](https://docs.microsoft.com/powershell/module/az.storage)
- [Depolama için Azure CLI Cmdlet'leri](https://docs.microsoft.com/cli/azure/storage)
- [AzCopy komut satırı yardımcı programı](https://aka.ms/downloadazcopy)
- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
- [Azure depolama Istemci araçları](../storage-explorers.md)
- [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Sonraki adımlar

Temel Azure depolama hizmetlerini kullanmaya başlayın, bkz. [depolama hesabı oluşturma](storage-account-create.md).
