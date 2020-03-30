---
title: Azure Depolamaya Giriş - Azure’da bulut depolama | Microsoft Docs
description: Azure Depolama, Microsoft’un bulut depolama çözümüdür. Azure Depolama yüksek oranda kullanılabilir, güvenli, sağlam, yüksek düzeyde ölçeklenebilir ve yedekli veri nesneleri için depolama alanı sağlar.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/16/2020
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 5bab70b6b023a4e6510e32368d407a38388cde2f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79256074"
---
# <a name="introduction-to-azure-storage"></a>Azure Depolama’ya Giriş

Azure Depolama, Microsoft’un modern veri depolama senaryolarına yönelik bulut depolama çözümüdür. Azure Depolama; veri nesneleri için yüksek düzeyde ölçeklenebilir bir nesne deposu, bulut için bir dosya sistemi hizmeti ve güvenilir mesajlaşma için mesajlaşma deposunun yanı sıra bir NoSQL deposu sunar. Azure Depolama şu özelliklere sahiptir:

- **Dayanıklı ve yüksek oranda kullanılabilir.** Yedeklilik, verilerinizin geçici donanım hataları durumunda güvende kalmasını sağlar. Ayrıca, yerel felaketler ya da doğal afetlere karşı ek koruma için verileri meri merkezleri ya da coğrafi bölgeler arasında çoğaltmayı seçebilirsiniz. Bu şekilde çoğaltılan veriler beklenmeyen bir kesinti durumunda yüksek oranda kullanılabilir olmaya devam eder.
- **Güvenli.** Azure Depolama’ya yazılan tüm veriler hizmet tarafından şifrelenir. Azure Depolama, verilerinize erişmesi gereken kişiler üzerinde ayrıntılı denetime sahip olmanızı sağlar.
- **Ölçeklenebilir.** Azure Depolama, günümüzün uygulamalarına ait veri depolama ve performans gereksinimlerini karşılamak için yüksek düzeyde ölçeklenebilir şekilde tasarlanmıştır.
- **Yönetilen.** Microsoft Azure, donanım bakımlarını, güncelleştirmeleri ve kritik sorunları sizin için işler.
- **Erişilebilir.** Azure Depolama’daki verilere dünyanın herhangi bir yerinden HTTP ya da HTTPS aracılığıyla erişilebilir. Microsoft, .NET, Java, Node.js, Python, PHP, Ruby, Go ve diğerleri gibi çeşitli dillerde Azure Depolama istemci kitaplıkları ve olgun bir REST API sağlar. Azure Depolama, Azure PowerShell veya Azure CLI’de betik oluşturmayı destekler. Azure portalı ve Azure Depolama Gezgini ise verilerinizle çalışmaya yönelik kolay görsel çözümler sunar.  

## <a name="azure-storage-services"></a>Azure Depolama hizmetleri

Azure Depolama şu veri hizmetlerini içerir:

- [Azure Blobları](../blobs/storage-blobs-introduction.md): Metin ve ikili veriler için yüksek düzeyde ölçeklenebilir nesne deposu.
- [Azure Dosyaları](../files/storage-files-introduction.md): Bulut veya şirket içi dağıtımlar için yönetilen dosya paylaşımları.
- [Azure Kuyrukları](../queues/storage-queues-introduction.md): Uygulama bileşenleri arasında güvenilir mesajlaşmaya yönelik bir mesajlaşma deposu.
- [Azure Tabloları](../tables/table-storage-overview.md): Yapılandırılmış verilerin şemasız depolanmasına yönelik bir NoSQL deposu.

Her hizmete bir depolama hesabı aracılığıyla erişilir. Başlamak için bkz. [Depolama hesabı oluşturma](storage-account-create.md).

## <a name="blob-storage"></a>Blob depolama

Azure Blob depolama, Microsoft’un buluta yönelik nesne depolama çözümüdür. Blob depolama, metin veya ikili veri gibi çok miktarda yapılandırılmamış veriyi depolamak için iyileştirilmiştir.

Blob depolama aşağıdaki durumlar için idealdir:

- Görüntülerin veya belgelerin doğrudan bir tarayıcıya sunulması.
- Dağıtılan erişim için dosyaların depolanması.
- Video ve ses akışları.
- Yedekleme ve geri yükleme, olağanüstü durum kurtarma ve arşivleme için verilerin depolanması.
- Şirket içi veya Azure’da barındırılan bir hizmetle analiz için verilerin depolanması.

Blob depolamadaki nesnelere, dünyanın her yerinden HTTP veya HTTPS üzerinden erişilebilir. Kullanıcılar veya istemci uygulamalar, bloblara URL’ler, [Azure Depolama REST API’si](https://docs.microsoft.com/rest/api/storageservices/blob-service-rest-api), [Azure PowerShell](https://docs.microsoft.com/powershell/module/azure.storage), [Azure CLI](https://docs.microsoft.com/cli/azure/storage) veya bir Azure Depolama istemci kitaplığı aracılığıyla erişebilir. Depolama istemci kitaplıkları [.NET](/dotnet/api/overview/azure/storage?view=azure-dotnet), [Java](https://docs.microsoft.com/java/api/overview/azure/storage), [Node.js](https://azure.github.io/azure-storage-node), [Python](https://azure-storage.readthedocs.io/), [PHP](https://azure.github.io/azure-storage-php/) ve [Ruby](https://azure.github.io/azure-storage-ruby) dahil olmak üzere birden çok dil ile kullanılabilir.

Blob depolama hakkında daha fazla bilgi için, [Blob depolamagiriş](../blobs/storage-blobs-introduction.md)bakın.

## <a name="azure-files"></a>Azure Dosyaları

[Azure Dosyaları](../files/storage-files-introduction.md), standart Sunucu İleti Bloğu (SMB) protokolü kullanılarak erişilebilen yüksek oranda kullanılabilir ağ dosya paylaşımları oluşturmanıza olanak tanır. Bu durum, birden fazla VM’nin hem okuma hem de yazma erişimi ile aynı dosyaları paylaşabildiği anlamına gelir. Dosyaları REST arabirimi veya depolama istemci kitaplıkları kullanarak da okuyabilirsiniz.

Dosyalara dünyanın herhangi bir yerinden dosyayı işaret eden ve paylaşılan erişim imzası (SAS) belirteci içeren bir URL kullanarak erişebilme olanağı, Azure Dosyaları'nı kurumsal bir dosya paylaşımındaki dosyalardan ayıran özelliklerden biridir. SAS belirteçleri oluşturabilirsiniz; bunlar, belirli bir süre için özel bir varlığa belirli bir erişim izni verir.

Dosya paylaşımları için birçok yaygın senaryoda kullanılabilir:

- Birçok şirket içi uygulama, dosya paylaşımlarını kullanır. Bu özellik, veri paylaşan uygulamaları Azure’a geçirmeyi kolaylaştırır. Dosya paylaşımını şirket içi uygulamanın kullandığı sürücü harfine bağlarsanız, uygulamanızın dosya paylaşımına erişen kısmı, varsa minimum değişikliklerle çalışır.

- Yapılandırma dosyaları bir dosya paylaşımında depolanabilir ve birden çok VM tarafından bu dosyalara erişilebilir. Bir gruptaki birden fazla geliştirici tarafından kullanılan araç ve yardımcı programlar, herkesin bulabilmesi ve aynı sürümü kullandıklarından emin olunması için bir dosya paylaşımında depolanabilir.

- Tanılama günlükleri, ölçümler ve kilitlenme bilgi dökümleri, bir dosya paylaşımına yazılıp daha sonra işlenebilen veya çözümlenebilen verilerin yalnızca üç örneğidir.

Şu anda Active Directory tabanlı kimlik doğrulaması ve erişim denetimi listeleri (ACL) desteklenmemektedir, ancak gelecekte desteklenecektir. Depolama hesabı kimlik bilgileri, dosya paylaşımına erişim için kimlik doğrulaması sağlamak üzere kullanılır. Bu durum, paylaşımın bağlı olduğu herkesin paylaşıma okuma/yazma erişimi elde edeceği anlamına gelir.

Azure Dosyaları hakkında daha fazla bilgi için bkz. [Azure Dosyaları'na Giriş](../files/storage-files-introduction.md).

## <a name="queue-storage"></a>Kuyruk depolama

Azure Kuyruk hizmeti, iletileri depolamak ve almak için kullanılır. Kuyruk iletilerinin boyutu 64 KB'ye kadar olabilir ve bir kuyruk, milyonlarca ileti içerebilir. Kuyruklar, genellikle zaman uyumsuz olarak işlenecek ileti listelerini depolamak için kullanılır.

Örneğin, müşterilerinizin resimleri karşıya yükleyebilmesini ve her resmin küçük resimlerini oluşturabilmesini istediğinizi düşünelim. Müşterinizin resimleri karşıya yüklerken küçük resimleri oluşturmanızı beklemesini sağlayabilirsiniz. Alternatif olarak bir kuyruk kullanabilirsiniz. Müşteri yüklemesini bitirdiğinde, kuyruğa bir ileti yazın. Ardından Azure İşlevinin iletiyi kuyruktan alıp ve küçük resimleri oluşturmasını sağlayın. Bu işlemin tüm bölümleri ayrıca ölçeklendirilebileceğinden kullanımınız için ayarlarken daha fazla kontrol sunar.

Azure Kuyrukları hakkında daha fazla bilgi için bkz. [Kuyruklara Giriş](../queues/storage-queues-introduction.md).

## <a name="table-storage"></a>Table Storage

Azure Tablo depolama artık Azure Cosmos DB’nin bir parçasıdır. Azure Tablo depolama belgelerini görmek için bkz. [Azure Tablo Depolamaya Genel Bakış](../tables/table-storage-overview.md). Mevcut Azure Tablo depolama hizmetine ek olarak, aktarım hızı için iyileştirilmiş tablolar, genel dağıtım ve otomatik ikincil dizinler sağlayan yeni bir Azure Cosmos DB Tablo API'si teklifi vardır. Daha fazla bilgi edinmek ve yeni premium deneyimini denemek için lütfen [Azure Cosmos DB Tablo API’si](https://aka.ms/premiumtables) konusunu inceleyin.

Tablo depolama hakkında daha fazla bilgi için bkz. [Azure Tablo depolamaya genel bakış](../tables/table-storage-overview.md).

## <a name="disk-storage"></a>Disk depolama

Azure yönetilen disk, sanal bir sabit disktir (VHD). Bunu şirket içi sunucudaki fiziksel bir disk gibi düşünebilirsiniz ama sanallaştırılmıştır. Azure yönetilen diskler, Azure'da rasgele bir IO depolama nesnesi olan sayfa blob'ları olarak depolanır. Yönetilen diske 'yönetilen' diyoruz, çünkü bu sayfa lekeleri, blob kapsayıcıları ve Azure depolama hesapları üzerinde bir soyutlamadır. Yönetilen disklerde tek yapmanız gereken diski sağlamaktır ve Gerisini Azure halleder.

Yönetilen diskler hakkında daha fazla bilgi için azure [yönetilen disklere Giriş'e](../../virtual-machines/windows/managed-disks-overview.md)bakın.

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

Depolama hesabı türleri hakkında daha fazla bilgi için [Azure depolama hesabına genel bakış](storage-account-overview.md)bilgisine bakın.

## <a name="securing-access-to-storage-accounts"></a>Depolama hesaplarına erişimi güvence altına alma

Azure Depolama'ya her istek yetkilendirilmelidir. Azure Depolama aşağıdaki yetkilendirme yöntemlerini destekler:

- **Blob ve sıra verileri için Azure Active Directory (Azure AD) tümleştirmesi.** Azure Depolama, rol tabanlı erişim denetimi (RBAC) aracılığıyla Blob ve Queue hizmetleri için Azure AD ile kimlik doğrulama yı ve yetkilendirmeyi destekler. Üstün güvenlik ve kullanım kolaylığı için Azure AD ile istekleri yetkilendirmeniz önerilir. Daha fazla bilgi için bkz. Azure [Etkin Dizin'i kullanarak Azure blob'larına ve kuyruklarına erişimi yetkilendirme.](storage-auth-aad.md)
- **Azure Dosyaları için SMB üzerinden Azure AD yetkilendirmesi (önizleme).** Azure Dosyaları, Azure Active Directory Domain Services aracılığıyla Kobİ (Sunucu İleti Bloğu) üzerinden kimlik tabanlı yetkilendirmeyi destekler. Etki alanına katılan Windows sanal makineleriniz (VM'ler), Azure AD kimlik bilgilerini kullanarak Azure dosya paylaşımlarına erişebilir. Daha fazla bilgi için, Azure Dosyaları için Kobİ üzerinden [Azure Etkin Dizin yetkisine Genel Bakış (önizleme) bölümüne](../files/storage-files-active-directory-overview.md)bakın.
- **Paylaşılan Anahtar ile Yetkilendirme.** Azure Depolama Blob, Kuyruk ve Tablo hizmetleri ve Azure Dosyaları, Paylaşılan Anahtar yetkilendirmesini kullanarak Paylaşılan Anahtar istemcisi ile yetkilendirmeyi destekler, depolama hesabı erişim anahtarı kullanılarak imzalanan her istekle birlikte üstbilgigeçer. Daha fazla bilgi için Bkz. [Paylaşılan Anahtarla Yetkilendirme](https://docs.microsoft.com/rest/api/storageservices/authorize-with-shared-key).
- **Paylaşılan erişim imzalarını (SAS) kullanarak yetkilendirme.** Paylaşılan erişim imzası (SAS), depolama kaynağı için URI'ye eklenebilen bir güvenlik belirteci içeren dizedir. Güvenlik belirteci, izinler ve erişim aralığı gibi kısıtlamaları kapsüller. Daha fazla bilgi için [Paylaşılan Erişim İmzalarını (SAS) kullanma'ya](storage-sas-overview.md)bakın.
- **Konteynerlere ve lekelere anonim erişim.** Bir konteyner ve lekeleri herkese açık olabilir. Bir kapsayıcının veya blobun herkese açık olduğunu belirttiğiniz zaman, herkes onu anonim olarak okuyabilir; kimlik doğrulaması gerekmez. Daha fazla bilgi için bkz: [Kapsayıcılara ve bloblara anonim okuma erişimini yönet](../blobs/storage-manage-access-to-resources.md)

## <a name="encryption"></a>Şifreleme

Depolama hizmetleri için iki temel şifreleme seçeneği vardır. Güvenlik ve şifreleme hakkında daha fazla bilgi için bkz. [Azure Depolama güvenlik kılavuzu](../blobs/security-recommendations.md).

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

Azure Depolama şifrelemesi, kuruluş güvenliği ve uyumluluk taahhütlerinizi yerine getirmek için verilerinizi korur ve korur. Azure Depolama, depolama hesabında kalıcı olarak devam edilmeden önce tüm verileri otomatik olarak şifreler ve geri almadan önce şifresini çözer. Şifreleme, şifre çözme ve anahtar yönetimi işlemleri kullanıcılar için tamamen saydamdır. Müşteriler azure key vault'u kullanarak kendi anahtarlarını yönetmeyi de seçebilirler. Daha fazla bilgi [için, veriler için Azure Depolama şifrelemesi'ne](storage-service-encryption.md)bakın.

### <a name="client-side-encryption"></a>İstemci Tarafında Şifreleme

Azure Depolama istemci kitaplıkları, istemci kitaplığından gelen verileri kabloüzerinden göndermeden ve yanıtın şifresini çözmeden önce şifreleme yöntemleri sağlar. İstemci tarafı şifreleme yoluyla şifrelenen veriler de Azure Depolama tarafından istirahatte şifrelenir. İstemci tarafı şifreleme hakkında daha fazla bilgi için, [Azure Depolama için .NET ile İstemci tarafı şifrelemesi'ne](storage-client-side-encryption.md)bakın.

## <a name="redundancy"></a>Yedeklilik

Azure Depolama, verilerinizin dayanıklı olduğundan emin olmak için verilerinizin birden çok kopyasını depolar. Depolama hesabınızı ayarlarken, artıklık seçeneğini seçersiniz.

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

## <a name="transferring-data-to-and-from-azure-storage"></a>Azure Depolama’da veri alışverişi

Verileri Azure Depolama'ya veya Azure Depolama'ya taşımak için çeşitli seçenekleriniz var. Seçtiğiniz seçenek, veri kümenizin boyutuna ve ağ bant genişliğinize bağlıdır. Daha fazla bilgi için [bkz.](storage-choose-data-transfer-solution.md)

## <a name="pricing"></a>Fiyatlandırma

Azure Depolama için fiyatlandırma hakkında ayrıntılı bilgi için bkz. [Fiyatlandırma sayfası](https://azure.microsoft.com/pricing/details/storage/blobs/).

## <a name="storage-apis-libraries-and-tools"></a>Depolama API'leri, kitaplıklar ve araçlar

Azure Storage kaynakları HTTP/HTTPS isteği yapabilen her dil ile erişilebilir. Ayrıca, Azure Storage birkaç popüler dilde programlama kitaplıkları sunar. Bu kitaplıklar zaman uyumlu ve zaman uyumsuz çağrılar, işlemleri gruplama, özel durum yönetimi, otomatik yeniden denemeler, işlemsel davranışlar vb. ayrıntıları ele alarak Azure Storage ile çalışmanın çoğu yönünü basitleştirir. Kitaplıklar şu anda aşağıdaki diller için mevcuttur ve yeni kitaplıklar geliştirme aşamasındadır:

### <a name="azure-storage-data-api-and-library-references"></a>Azure Depolama veri API’si ve kitaplık başvuruları

- [Azure Depolama REST API](https://docs.microsoft.com/rest/api/storageservices/)
- [.NET için Azure Depolama istemci kitaplığı](https://docs.microsoft.com/dotnet/api/overview/azure/storage)
- [Java/Android için Azure Depolama istemci kitaplığı](https://docs.microsoft.com/java/api/overview/azure/storage)
- [Düğüm.js için Azure Depolama istemci kitaplığı](https://docs.microsoft.com/javascript/api/overview/azure/storage-overview)
- [Python için Azure Depolama istemci kitaplığı](https://github.com/Azure/azure-storage-python)
- [PHP için Azure Depolama istemci kitaplığı](https://github.com/Azure/azure-storage-php)
- [Ruby için Azure Depolama istemci kitaplığı](https://github.com/Azure/azure-storage-ruby)
- [C++ için Azure Depolama istemci kitaplığı](https://github.com/Azure/azure-storage-cpp)

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
- [AzCopy Command-Line Utility](https://aka.ms/downloadazcopy)
- [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/), Microsoft’un Windows, macOS ve Linux üzerinde Azure Depolama verileriyle görsel olarak çalışmanızı sağlayan ücretsiz ve tek başına uygulamasıdır.
- [Azure Storage İstemci Araçları](../storage-explorers.md)
- [Azure Geliştirici Araçları](https://azure.microsoft.com/tools/)

## <a name="next-steps"></a>Sonraki adımlar

Çalışır durumda bir Azure Depolama edinmek için bkz. [Depolama hesabı oluşturma](storage-account-create.md).
