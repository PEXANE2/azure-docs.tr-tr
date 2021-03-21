---
title: Depolama hesabına genel bakış
titleSuffix: Azure Storage
description: Azure depolama 'da farklı depolama hesabı türleri hakkında bilgi edinin. Hesap adlandırma, performans katmanları, erişim katmanları, artıklık, şifreleme, uç noktalar ve daha fazlasını inceleyin.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 03/09/2021
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 24d955b0d1c53f57f5927f9e893b6ecd75fb3ca8
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102561902"
---
# <a name="storage-account-overview"></a>Depolama hesabına genel bakış

Azure depolama hesabı; Bloblar, dosyalar, kuyruklar, tablolar ve diskler içeren tüm Azure depolama veri nesnelerinizi içerir. Depolama hesabı, Azure depolama verileriniz için HTTP veya HTTPS üzerinden dünyanın herhangi bir yerinden erişilebilen benzersiz bir ad alanı sağlar. Azure Depolama hesabınızdaki veriler dayanıklı ve yüksek oranda kullanılabilir, güvenli ve yüksek düzeyde ölçeklenebilir.

Azure depolama hesabı oluşturma hakkında bilgi edinmek için bkz. [Depolama hesabı oluşturma](storage-account-create.md).

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

Azure depolama, birkaç depolama hesabı türü sunar. Her tür farklı özellikleri destekler ve kendi fiyatlandırma modeline sahiptir. Uygulamalarınız için en iyi hesap türünü belirleyebilmek üzere bir depolama hesabı oluşturmadan önce bu farklılıkları göz önünde bulundurun. Depolama hesaplarının türleri şunlardır:

- **Genel amaçlı v2 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için temel depolama hesabı türü. Azure Storage kullanan çoğu senaryo için önerilir.
- **Genel amaçlı v1 hesapları**: Bloblar, dosyalar, kuyruklar ve tablolar için eski hesap türü. Mümkün olduğunda bunun yerine genel amaçlı v2 hesaplarını kullanın.
- **Blockblobstorage hesapları**: blok Blobları ve ekleme Blobları için Premium performans özelliklerine sahip depolama hesapları. Yüksek işlem ücretleri olan senaryolar veya daha küçük nesneler kullanan veya tutarlı depolama gecikme süresi gerektiren senaryolar için önerilir.
- **FileStorage hesapları**: yalnızca dosyalar, Premium performans özelliklerine sahip depolama hesaplarıdır. Kurumsal veya yüksek performanslı ölçekli uygulamalar için önerilir.
- **Blobstorage hesapları**: eski BLOB depolama hesapları. Mümkün olduğunda bunun yerine genel amaçlı v2 hesaplarını kullanın.

Aşağıdaki tabloda depolama hesabı türleri, destekledikleri hizmetler ve her hesap türü için desteklenen dağıtım modelleri açıklanmaktadır:

| Depolama hesabı türü | Desteklenen hizmetler | Artıklık seçenekleri | Dağıtım modeli<sup>1</sup> |
|--|--|--|--|
| Genel amaçlı v2 | Blob, dosya, kuyruk, tablo, disk ve Data Lake Gen2<sup>2</sup> | LRS, GRS, RA-GRS, ZRS, GZRS, RA-GZRS<sup>3</sup> | Resource Manager |
| Genel amaçlı v1 | Blob, dosya, kuyruk, tablo ve disk | LRS, GRS, RA-GRS | Kaynak Yöneticisi, klasik |
| BlockBlobStorage | Blob (yalnızca blok Blobları ve ekleme Blobları) | LRS, ZRS<sup>3</sup> | Resource Manager |
| Dosya depolama | Yalnızca dosya | LRS, ZRS<sup>3</sup> | Resource Manager |
| BlobStorage | Blob (yalnızca blok Blobları ve ekleme Blobları) | LRS, GRS, RA-GRS | Resource Manager |

<sup>1</sup> Azure Resource Manager dağıtım modelinin kullanılması önerilir. Klasik dağıtım modelini kullanan depolama hesapları bazı konumlarda hala oluşturulabilir ve klasik mevcut hesaplar desteklenmeye devam eder. Daha fazla bilgi için bkz [. Azure Resource Manager ile klasik dağıtım: dağıtım modellerini ve kaynaklarınızın durumunu anlayın](../../azure-resource-manager/management/deployment-models.md).

<sup>2</sup> Azure Data Lake Storage 2., Azure Blob depolama üzerinde oluşturulmuş, büyük veri analizi için adanmış bir yetenekler kümesidir. Data Lake Storage 2. yalnızca hiyerarşik ad alanı etkin olan genel amaçlı v2 depolama hesaplarında desteklenir. Data Lake Storage 2. hakkında daha fazla bilgi için bkz. [Azure Data Lake Storage 2. giriş](../blobs/data-lake-storage-introduction.md).

<sup>3</sup> Bölgesel olarak yedekli depolama (ZRS) ve coğrafi bölge yedekli depolama (GZRS/RA-GZRS) yalnızca belirli bölgelerde standart genel amaçlı v2, blok Blobdepolama ve dosya depolama hesapları için kullanılabilir. Azure depolama artıklığı seçenekleri hakkında daha fazla bilgi için bkz. [Azure depolama artıklığı](storage-redundancy.md).

### <a name="storage-account-redundancy"></a>Depolama hesabı artıklığı

Depolama hesabı için artıklık seçenekleri şunlardır:

- **Yerel olarak yedekli depolama (LRS)**: basit, düşük maliyetli bir artıklık stratejisi. Veriler, birincil bölgedeki tek bir fiziksel konum içinde zaman uyumlu olarak üç kez kopyalanır.
- Bölgesel olarak **yedekli depolama (ZRS)**: yüksek kullanılabilirlik gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır.
- **Coğrafi olarak yedekli depolama (GRS)**: bölgesel kesintilere karşı korumak için çapraz bölgesel yedeklilik. Veriler birincil bölgede zaman uyumlu olarak üç kez kopyalanır ve ardından zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için Okuma Erişimli Coğrafi olarak yedekli depolamayı (RA-GRS) etkinleştirin.
- **Coğrafi bölge yedekli depolama (GZRS)**: hem yüksek kullanılabilirlik hem de en yüksek dayanıklılık gerektiren senaryolar için artıklık. Veriler, birincil bölgedeki üç Azure kullanılabilirlik bölgesi arasında zaman uyumlu olarak kopyalanır ve sonra zaman uyumsuz olarak ikincil bölgeye kopyalanır. İkincil bölgedeki verilere yönelik okuma erişimi için, Okuma Erişimli Coğrafi bölge yedekli depolamayı (RA-GZRS) etkinleştirin.

Azure depolama 'daki artıklık seçenekleri hakkında daha fazla bilgi için bkz. [Azure Storage yedekliği](storage-redundancy.md).

### <a name="general-purpose-v2-accounts"></a>Genel amaçlı v2 hesapları

Genel amaçlı v2 depolama hesapları, en son Azure depolama özelliklerini destekler ve genel amaçlı v1 ve BLOB depolama hesaplarının tüm işlevlerini dahil edin. Genel amaçlı v2 hesapları, Azure depolama için en düşük gigabayt başına kapasite fiyatlarını ve sektör rekabetçi işlem fiyatlarını sunar. Genel amaçlı v2 depolama hesapları, bu Azure depolama hizmetlerini destekler:

- Blob 'lar (tüm türler: engelle, Ekle, sayfa)
- Data Lake Gen2
- Dosyalar
- Diskler
- Kuyruklar
- Tablolar

> [!NOTE]
> Microsoft, çoğu senaryo için genel amaçlı v2 depolama hesabı kullanılmasını önerir. Genel amaçlı v1 veya blob depolama hesabını, kapalı kalma süresi olmadan ve verileri kopyalamaya gerek kalmadan, genel amaçlı bir v2 hesabına kolayca yükseltebilirsiniz. Ancak yükseltme işlemi geri alınamaz.
>
> Genel amaçlı v2 hesabına yükseltme hakkında daha fazla bilgi için bkz. [genel amaçlı v2 depolama hesabına yükseltme](storage-account-upgrade.md).

Genel amaçlı v2 depolama hesapları, kullanım modellerinize göre veri depolamak için birden çok erişim katmanı sunar. Daha fazla bilgi için bkz. [Blok Blobu verileri Için erişim katmanları](#access-tiers-for-block-blob-data).

### <a name="general-purpose-v1-accounts"></a>Genel amaçlı v1 hesapları

Genel amaçlı v1 depolama hesapları, tüm Azure depolama hizmetlerine erişim sağlar, ancak en son özelliklere veya gigabayt başına en düşük fiyata sahip olmayabilir. Genel amaçlı v1 depolama hesapları, bu Azure depolama hizmetlerini destekler:

- Bloblar (tüm türler)
- Dosyalar
- Diskler
- Kuyruklar
- Tablolar

Bu senaryolar için genel amaçlı v1 hesaplarını kullanabilirsiniz:

- Uygulamalarınız Azure klasik dağıtım modelini gerektirir. Genel amaçlı v2 hesapları ve BLOB depolama hesapları yalnızca Azure Resource Manager dağıtım modelini destekler.

- Uygulamalarınız işlem açısından yoğun veya önemli coğrafi çoğaltma bant genişliği kullanır, ancak büyük kapasiteye gerek kalmaz. Bu durumda, genel amaçlı v1 en ekonomik seçim olabilir.

- 2014-02-14 'den önceki bir [depolama hizmetleri REST API](/rest/api/storageservices/Versioning-for-the-Azure-Storage-Services) veya 4. x sürümünden daha düşük bir sürüme sahip bir istemci kitaplığı kullanın. Uygulamanızı yükseltemezsiniz.

> [!NOTE]
> Microsoft çoğu senaryo için genel amaçlı v2 hesapları önerse de, Microsoft yeni ve mevcut müşteriler için genel amaçlı v1 hesaplarını desteklemeye devam edecektir. Azure Storage bu bölgelerde kullanılabilir olduğunda, yeni bölgelerde genel amaçlı v1 depolama hesapları oluşturabilirsiniz. Microsoft 'un, genel amaçlı v1 hesapları için desteği kullanımdan kaldırmaya yönelik bir planı yoktur ve herhangi bir Azure depolama özelliğinin kullanım dışı bırakılmadan önce en az bir yıllık ön uyarı sağlar. Microsoft, genel amaçlı v1 hesapları için güvenlik güncelleştirmeleri sağlamaya devam edecek, ancak bu hesap türü için yeni bir özellik geliştirmesi beklenmez.
>
> 1 Ekim 2020 itibariyle, yeni Azure depolama bölgelerinde genel amaçlı v1 hesapları için fiyatlandırma, bu bölgelerdeki genel amaçlı v2 hesapları için fiyatlandırmaya eşittir. Mevcut Azure depolama bölgelerinde fiyatlandırma değişmemiştir. Belirli bir bölgedeki genel amaçlı v1 hesapları için fiyatlandırma ayrıntıları için bkz. Azure Depolama fiyatlandırması sayfası. Bölgenizi seçin ve ardından **fiyatlandırma teklifleri**' nin yanındaki **diğer**' i seçin.

### <a name="blockblobstorage-accounts"></a>BlockBlobStorage hesapları

Bir blok Blobstorage hesabı, yapılandırılmamış nesne verilerini blok blob 'ları veya ekleme Blobları olarak depolamak için Premium performans katmanında özelleştirilmiş bir depolama hesabıdır. Blok Blobstorage hesapları, genel amaçlı v2 ve BlobStorage hesaplarıyla karşılaştırıldığında düşük, tutarlı gecikme süresi ve daha yüksek işlem ücretleri sağlar.

BlockBlobStorage hesapları Şu anda sık erişimli, seyrek erişimli veya arşiv erişim katmanlarına yönelik katmanlamayı desteklememektedir. Bu depolama hesabı türü sayfa bloblarını, tabloları veya kuyrukları desteklemez.

### <a name="filestorage-accounts"></a>Dosya depolama hesapları

Dosya depolama hesabı, Premium dosya paylaşımlarını depolamak ve oluşturmak için kullanılan özel bir depolama hesabıdır. Bu depolama hesabı türü dosyaları destekler ancak blok Blobları, ekleme Blobları, sayfa Blobları, tablolar veya kuyruklar değildir.

FileStorage hesapları, ıOPS patlaması gibi benzersiz performansa özel özellikler sunar. Bu özellikler hakkında daha fazla bilgi için dosyalar planlama kılavuzunun [dosya paylaşma depolama katmanları](../files/storage-files-planning.md#storage-tiers) bölümüne bakın.

## <a name="naming-storage-accounts"></a>Depolama hesaplarını adlandırma

Depolama hesabınızı adlandırırken şu kuralları göz önünde bulundurun:

- Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
- Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. İki depolama hesabı aynı ada sahip olamaz.

## <a name="performance-tiers"></a>Performans katmanları

Oluşturduğunuz depolama hesabının türüne bağlı olarak, standart ve Premium performans katmanları arasından seçim yapabilirsiniz. Aşağıdaki tabloda, hangi depolama hesabı türü için kullanılabilen performans katmanları özetlenmektedir.

| Depolama hesabı türü | Desteklenen performans katmanları |
|--|--|
| Genel amaçlı v2 | Standart, Premium<sup>1</sup> |
| Genel amaçlı v1 | Standart, Premium<sup>1</sup> |
| BlockBlobStorage | Premium |
| Dosya depolama | Premium |
| BlobStorage | Standart |

<sup>1</sup> Genel amaçlı v2 ve genel amaçlı v1 hesapları için Premium performans, yalnızca disk ve Sayfa Blobu için kullanılabilir. Blok veya ekleme Blobları için Premium performans yalnızca blok Blobstorage hesaplarında kullanılabilir. Dosyalar için Premium performans yalnızca FileStorage hesaplarında kullanılabilir.

### <a name="general-purpose-storage-accounts"></a>Genel amaçlı depolama hesapları

Genel amaçlı depolama hesapları aşağıdaki performans katmanlarından biri için yapılandırılabilir:

- Blob, dosya, tablo, kuyruk ve Azure sanal makine disklerini depolamak için standart bir performans katmanı. Standart depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Standart depolama hesapları Için ölçeklenebilirlik hedefleri](scalability-targets-standard-account.md).
- Yönetilmeyen sanal makine disklerini depolamak için Premium performans katmanı. Microsoft, yönetilmeyen diskler yerine Azure sanal makineleri ile yönetilen disklerin kullanılmasını önerir. Premium performans katmanı için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için bkz. [Premium Sayfa Blobu depolama hesapları Için ölçeklenebilirlik hedefleri](../blobs/scalability-targets-premium-page-blobs.md).

### <a name="blockblobstorage-storage-accounts"></a>BlockBlobStorage depolama hesapları

BlockBlobStorage Storage hesapları, blok bloblarını ve ekleme bloblarını depolamak için Premium bir performans katmanı sağlar. Daha fazla bilgi için bkz. [Premium Blok Blobu depolama hesapları Için ölçeklenebilirlik hedefleri](../blobs/scalability-targets-premium-block-blobs.md).

### <a name="filestorage-storage-accounts"></a>FileStorage depolama hesapları

FileStorage depolama hesapları, Azure dosya paylaşımları için Premium bir performans katmanı sağlar. Daha fazla bilgi için bkz. [Azure dosyaları ölçeklenebilirlik ve performans hedefleri](../files/storage-files-scale-targets.md).

## <a name="access-tiers-for-block-blob-data"></a>Blok Blobu verileri için erişim katmanları

Azure depolama, Blok Blobu verilerine erişim için kullanım desenlerine göre farklı seçenekler sağlar. Azure Storage 'daki her erişim katmanı, belirli bir veri kullanımı düzeniyle iyileştirilmiştir. Gereksinimleriniz için doğru erişim katmanını seçerek, Blok Blobu verilerinizi en düşük maliyetli şekilde saklayabilirsiniz.

Kullanılabilir erişim katmanları şunlardır:

- **Sık** erişimli erişim katmanı. Bu katman, depolama hesabındaki nesnelere sık sık erişim için iyileştirilmiştir. Sık erişimli katmanda verilere erişmek, depolama maliyetlerinin daha yüksektir. Yeni depolama hesapları varsayılan olarak sık erişimli katmanda oluşturulur.
- Seyrek **erişimli erişim** katmanı. Bu katman, seyrek erişilen ve en az 30 gün içinde depolanan büyük miktardaki verileri depolamak için iyileştirilmiştir. Seyrek Erişimli katmanda verilerin depolanması daha uygun maliyetli bir işlemdir, ancak bu verilere erişilmesi, sık erişimli katmandaki verilere erişmenin daha pahalı olabilir.
- **Arşiv** katmanı. Bu katman yalnızca bireysel blok Blobları için kullanılabilir. Arşiv katmanı, birkaç saatlik alma gecikmesini kabul eden ve arşiv katmanında en az 180 gün kalacak olan veriler için iyileştirilmiştir. Arşiv katmanı, verileri depolamak için en ekonomik seçenektir. Ancak, verilere erişmek, sık veya seyrek katmanlardaki verilere erişmenin daha pahalıdır.

Verilerinizin kullanım düzeninde bir değişiklik varsa, bu erişim katmanları arasında istediğiniz zaman geçiş yapabilirsiniz. Erişim katmanları hakkında daha fazla bilgi için bkz. [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanları](../blobs/storage-blob-storage-tiers.md).

Aşağıdaki tabloda, her bir depolama hesabı türünde Bloblar için hangi erişim katmanlarının kullanılabildiği gösterilmektedir.

| Depolama hesabı türü | Desteklenen erişim katmanları |
|--|--|
| Genel amaçlı v2 | Sık erişimli, seyrek erişimli arşiv<sup>1</sup> |
| Genel amaçlı v1 | Yok |
| BlockBlobStorage | Yok |
| Dosya depolama | Yok |
| BlobStorage | Sık erişimli, seyrek erişimli arşiv<sup>1</sup> |

<sup>1</sup> arşiv depolama ve BLOB düzeyinde katmanlama yalnızca blok bloblarını destekler. Arşiv katmanı, depolama hesabı düzeyinde değil yalnızca tek bir blob düzeyinde kullanılabilir. Daha fazla bilgi için bkz. [Azure Blob depolama Için erişim katmanları-sık erişimli, seyrek erişimli ve arşiv](../blobs/storage-blob-storage-tiers.md).

> [!IMPORTANT]
> Mevcut bir depolama hesabı veya Blobun erişim katmanını değiştirmek ek ücretler elde edebilir. Daha fazla bilgi için bkz. [depolama hesabı faturalandırması](#storage-account-billing).

## <a name="encryption"></a>Şifreleme

Depolama hesabınızdaki tüm veriler hizmet tarafında şifrelenir. Şifreleme hakkında daha fazla bilgi için bkz. [bekleyen veri Için Azure depolama hizmeti şifrelemesi](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Depolama hesabı uç noktaları

Depolama hesabı, Azure 'da verileriniz için benzersiz bir ad alanı sağlar. Azure Storage 'da depoladığınız her nesnenin benzersiz hesap adınızı içeren bir adresi vardır. Hesap adı ve Azure depolama hizmeti uç noktası birleşimi, depolama hesabınız için uç noktaları oluşturur.

Aşağıdaki tabloda, Azure Depolama hizmetlerinin her biri için uç noktalar listelenmektedir.

| Depolama hizmeti | Uç Nokta |
|--|--|
| Blob depolama | `https://<storage-account>.blob.core.windows.net` |
| Azure Data Lake Storage Gen2 | `https://<storage-account>.dfs.core.windows.net` |
| Azure Dosyaları | `https://<storage-account>.file.core.windows.net` |
| Kuyruk depolama | `https://<storage-account>.queue.core.windows.net` |
| Tablo depolama | `https://<storage-account>.table.core.windows.net` |

> [!NOTE]
> Blok Blobu ve BLOB depolama hesapları yalnızca blob hizmeti uç noktasını kullanıma sunar.

Depolama hesabındaki nesnenin konumunu uç noktaya ekleyerek bir depolama hesabındaki bir nesneye erişmek için URL oluşturun. Örneğin bir blob adresi şu biçimde olabilir: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Ayrıca, depolama hesabınızı Bloblar için özel bir etki alanı kullanacak şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. [Azure depolama hesabınız için özel etki alanı adı yapılandırma](../blobs/storage-custom-domain-name.md).  

## <a name="control-access-to-account-data"></a>Hesap verilerine erişimi denetleme

Varsayılan olarak, hesabınızdaki veriler yalnızca siz, yani hesap sahibi tarafından kullanılabilir. Verilerinize kimlerin erişebileceğini ve sahip oldukları izinleri denetleyin.

Depolama hesabınızda yapılan her isteği yetkilendirmelidir. Hizmetin düzeyinde, istek geçerli bir *Yetkilendirme* üst bilgisi içermelidir. Özellikle, bu üst bilgi, hizmeti yürütmeden önce isteği doğrulamak için gerekli tüm bilgileri içerir.

Aşağıdaki yaklaşımlardan herhangi birini kullanarak Depolama hesabınızdaki verilere erişim izni verebilirsiniz:

- **Azure Active Directory:** Blob ve kuyruk verilerine erişim için bir kullanıcının, grubun veya diğer kimliğin kimliğini doğrulamak üzere Azure Active Directory (Azure AD) kimlik bilgilerini kullanın. Kimlik doğrulaması başarılı olursa Azure AD, Azure Blob depolama veya kuyruk depolama için isteği yetkilendirmede kullanılacak bir belirteç döndürür. Daha fazla bilgi için bkz. [Azure Active Directory kullanarak Azure depolama 'ya erişim kimlik doğrulaması](storage-auth-aad.md).
- **Paylaşılan anahtar yetkilendirmesi:** Azure depolama 'ya erişmek için uygulamanızın çalışma zamanında kullandığı bir bağlantı dizesi oluşturmak için depolama hesabı erişim anahtarınızı kullanın. Bağlantı dizesindeki değerler, Azure depolama 'ya geçirilen *Yetkilendirme* üst bilgisini oluşturmak için kullanılır. Daha fazla bilgi için bkz. [Azure Storage bağlantı dizelerini yapılandırma](storage-configure-connection-string.md).
- **Paylaşılan erişim imzası:** Paylaşılan erişim imzası (SAS) Depolama hesabınızdaki kaynaklara temsilci erişimine izin veren bir belirteçtir. SAS belirteci, URL 'de Azure depolama 'ya bir istek yetkilendirmek için gereken tüm bilgileri kapsüller. SAS oluşturduğunuzda, SAS 'nin bir kaynağa verdiği izinleri ve izinlerin geçerli olduğu aralığı belirtebilirsiniz. Bir SAS belirteci, Azure AD kimlik bilgileriyle veya paylaşılan anahtarla imzalanabilir. Daha fazla bilgi için bkz. [paylaşılan erişim imzaları (SAS) kullanarak Azure depolama kaynaklarına sınırlı erişim verme](storage-sas-overview.md).

> [!NOTE]
> Azure AD kimlik bilgilerini kullanarak kullanıcıların veya uygulamaların kimliğini doğrulamak, diğer yetkilendirme yolları üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Uygulamalarınızla paylaşılan anahtar yetkilendirmesi kullanmaya devam edebilirsiniz, ancak Azure AD atlama 'yi kullanarak hesap erişim anahtarınızı kodunuzla depolama gereksinimini ortadan kaldırabilirsiniz. Ayrıca, Depolama hesabınızdaki kaynaklara ayrıntılı erişim sağlamak için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD SAS belirteçlerini yönetmeye gerek kalmadan veya güvenliği aşılmış bir SAS iptal etme gereksinimi olmadan benzer yetenekler sunmaktadır.
>
> Microsoft, mümkün olduğunda Azure Depolama Blobu ve kuyruk uygulamaları için Azure AD yetkilendirmesi kullanılmasını önerir.

## <a name="copying-data-into-a-storage-account"></a>Bir depolama hesabına veri kopyalama

Microsoft, verilerinizi şirket içi depolama cihazlarından veya üçüncü taraf bulut depolama sağlayıcılardan içeri aktarmaya yönelik yardımcı programlar ve kitaplıklar sağlar. Kullandığınız çözüm, aktardığınız veri miktarına bağlıdır.

Genel amaçlı v1 veya blob depolama hesabından genel amaçlı bir v2 hesabına yükselttiğinizde, verileriniz otomatik olarak geçirilir. Microsoft, hesabınızı yükseltmek için bu patika önerir. Ancak, verileri genel amaçlı v1 hesabından bir BLOB depolama hesabına taşımaya karar verirseniz, aşağıda açıklanan araçları ve kitaplıkları kullanarak verilerinizi el ile geçirebilirsiniz.

### <a name="azcopy"></a>AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy komutunu, mevcut bir genel amaçlı depolama hesabından bir BLOB depolama hesabına kopyalamak veya şirket içi depolama cihazlarından verileri karşıya yüklemek için kullanabilirsiniz. Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](./storage-use-azcopy-v10.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Veri hareketi kitaplığı

.NET için Azure Depolama veri taşıma kitaplığı, AzCopy’yi çalıştıran çekirdek veri taşıma altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. AzCopy özelliklerinden yerel olarak yararlanmak için veri taşıma kitaplığını kullanabilirsiniz. Daha fazla bilgi için bkz. [.net Için Azure Storage veri taşıma kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı

Verilerinizi genel amaçlı v1 depolama hesabından BLOB depolama hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure istemci kitaplıklarından veya Azure Storage Services REST API birini kullanın. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Azure depolama REST API hakkında daha fazla bilgi için bkz. [Azure Storage Services REST API başvurusu](/rest/api/storageservices/).

> [!IMPORTANT]
> Bloblar, blobla birlikte istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. İstemci tarafı şifreleme ile şifrelenmiş bir blobu kopyalarsanız, kopyalama işleminin başta şifreleme ile ilgili meta veriler olmak üzere blob meta verilerini koruduğundan emin olun. Bir blobu şifreleme meta verileri olmadan kopyalarsanız blob içeriği tekrar alınamaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Depolama İstemci Tarafı Şifrelemesi](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="storage-account-billing"></a>Depolama hesabı faturalama

Azure depolama, depolama hesabı kullanımınıza göre faturalandırılır. Depolama hesabındaki tüm nesneler bir grup halinde faturalandırılır. Depolama maliyetleri aşağıdaki etkenlere göre hesaplanır:

- **Bölge** , hesabınızın dayandığı coğrafi bölgeyi ifade eder.
- **Hesap türü** , kullanmakta olduğunuz depolama hesabının türünü belirtir.
- **Erişim katmanı** , genel amaçlı v2 veya blob depolama hesabınız için belirttiğiniz veri kullanımı modelini ifade eder.
- **Kapasite** , veri depolamak için kullandığınız depolama hesabı servis biriminden ne kadarının olduğunu ifade eder.
- **Çoğaltma** , verilerinizin kaç kopyasının aynı anda ve hangi konumlarda tutulmasını belirler.
- **İşlemler** , Azure depolama 'ya yönelik tüm okuma ve yazma işlemlerine başvurur.
- **Veri** çıkışı, bir Azure bölgesinden aktarılan tüm verileri ifade eder. Depolama hesabınızdaki verilere aynı bölgede çalışmayan bir uygulama tarafından erişildiğinde, veri çıkışı için ücretlendirilirsiniz. Kaynak gruplarını, çıkış ücretlerini sınırlandırmak için aynı bölgedeki veri ve hizmetlerinizi gruplandırmak üzere kullanma hakkında bilgi için bkz. [Azure Kaynak grubu nedir?](/azure/cloud-adoption-framework/govern/resource-consistency/resource-access-management#what-is-an-azure-resource-group).

[Azure Storage Fiyatlandırması](https://azure.microsoft.com/pricing/details/storage/) sayfası hesap türüne, depolama kapasitesine, çoğaltmaya ve işlemlere bağlı olarak ayrıntılı fiyatladırma bilgileri sağlar. [Veri Aktarımları Fiyatlandırma Ayrıntıları](https://azure.microsoft.com/pricing/details/data-transfers/), veri çıkışı için ayrıntılı fiyatlandırma bilgileri sağlar. Maliyetlerinizi tahmin etmeye yardımcı olması için [Azure Storage Fiyatlandırması Hesaplayıcısı](https://azure.microsoft.com/pricing/calculator/?scenario=data-management)’nı kullanabilirsiniz.

[!INCLUDE [cost-management-horizontal](../../../includes/cost-management-horizontal.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](storage-account-create.md)
- [Blok blobu depolama hesabı oluşturma](../blobs/storage-blob-create-account-block-blob.md)
- [Genel amaçlı v2 depolama hesabına yükseltme](storage-account-upgrade.md)
- [Silinmiş bir depolama hesabını kurtarma](storage-account-recover.md)