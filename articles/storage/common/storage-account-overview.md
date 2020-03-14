---
title: Depolama hesabına genel bakış
titleSuffix: Azure Storage
description: Azure depolama hesabı oluşturma ve kullanma seçeneklerini anlayın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/17/2020
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 3694a1d04ec25f44cfcf9395bdd148e2fd3c0d9f
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79371571"
---
# <a name="storage-account-overview"></a>Depolama hesabına genel bakış

Azure depolama hesabı; Bloblar, dosyalar, kuyruklar, tablolar ve diskler içeren tüm Azure depolama veri nesnelerinizi içerir. Depolama hesabı, Azure depolama verileriniz için HTTP veya HTTPS üzerinden dünyanın herhangi bir yerinden erişilebilen benzersiz bir ad alanı sağlar. Azure Depolama hesabınızdaki veriler dayanıklı ve yüksek oranda kullanılabilir, güvenli ve yüksek düzeyde ölçeklenebilir.

Azure depolama hesabı oluşturma hakkında bilgi edinmek için bkz. [depolama hesabı oluşturma](storage-account-create.md).

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

### <a name="general-purpose-v2-accounts"></a>Genel amaçlı v2 hesapları

Genel amaçlı v2 depolama hesapları, en son Azure depolama özelliklerini destekler ve genel amaçlı v1 ve BLOB depolama hesaplarının tüm işlevlerini dahil edin. Genel amaçlı v2 hesapları, Azure depolama için en düşük gigabayt başına kapasite fiyatlarını ve sektör rekabetçi işlem fiyatlarını sunar. Genel amaçlı v2 depolama hesapları, bu Azure depolama hizmetlerini destekler:

- Blob 'lar (tüm türler: engelle, Ekle, sayfa)
- Data Lake Gen2
- Dosyalar
- Diskler
- Kuyruklar
- Tablolar

> [!NOTE]
> Microsoft, çoğu senaryo için genel amaçlı v2 depolama hesabı kullanılmasını önerir. Genel amaçlı v1 veya blob depolama hesabını, kapalı kalma süresi olmadan ve verileri kopyalamaya gerek kalmadan, genel amaçlı bir v2 hesabına kolayca yükseltebilirsiniz.
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

Çoğu durumda genel amaçlı v2 hesapları kullanmanız gerekir. Bu senaryolar için genel amaçlı v1 hesaplarını kullanabilirsiniz:

- Uygulamalarınız Azure klasik dağıtım modelini gerektirir. Genel amaçlı v2 hesapları ve BLOB depolama hesapları yalnızca Azure Resource Manager dağıtım modelini destekler.

- Uygulamalarınız işlem açısından yoğun veya önemli coğrafi çoğaltma bant genişliği kullanır, ancak büyük kapasiteye gerek kalmaz. Bu durumda, genel amaçlı v1 en ekonomik seçim olabilir.

- 2014-02-14 'den önceki bir [depolama hizmetleri REST API](https://msdn.microsoft.com/library/azure/dd894041.aspx) veya 4. x sürümünden daha düşük bir sürüme sahip bir istemci kitaplığı kullanın. Uygulamanızı yükseltemezsiniz.

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

Oluşturduğunuz depolama hesabının türüne bağlı olarak, standart ve Premium performans katmanları arasından seçim yapabilirsiniz.

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

> [!IMPORTANT]
> Mevcut bir depolama hesabı veya Blobun erişim katmanını değiştirmek ek ücretler elde edebilir. Daha fazla bilgi için [depolama hesabı faturalama bölümüne](#storage-account-billing)bakın.

## <a name="redundancy"></a>Yedeklilik

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

## <a name="encryption"></a>Şifreleme

Depolama hesabınızdaki tüm veriler hizmet tarafında şifrelenir. Şifreleme hakkında daha fazla bilgi için bkz. [bekleyen veri Için Azure depolama hizmeti şifrelemesi](storage-service-encryption.md).

## <a name="storage-account-endpoints"></a>Depolama hesabı uç noktaları

Depolama hesabı, Azure 'da verileriniz için benzersiz bir ad alanı sağlar. Azure Storage 'da depoladığınız her nesnenin benzersiz hesap adınızı içeren bir adresi vardır. Hesap adı ve Azure depolama hizmeti uç noktası birleşimi, depolama hesabınız için uç noktaları oluşturur.

Örneğin, genel amaçlı depolama hesabınız *mystorageaccount*olarak adlandırılmışsa, bu hesaba ait varsayılan uç noktalar şunlardır:

- BLOB depolama: `https://*mystorageaccount*.blob.core.windows.net`
- Tablo Depolama: `https://*mystorageaccount*.table.core.windows.net`
- Kuyruk depolama: `https://*mystorageaccount*.queue.core.windows.net`
- Azure dosyaları: `https://*mystorageaccount*.file.core.windows.net`

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
- **Paylaşılan erişim imzası:** Azure AD yetkilendirmesi kullanmıyorsanız, Depolama hesabınızdaki kaynaklara erişim yetkisi vermek için paylaşılan erişim imzası kullanın. Paylaşılan erişim imzası, URL 'de Azure depolama 'ya bir istek yetkilendirmek için gereken tüm bilgileri kapsülleyen bir belirteçtir. Depolama kaynağını, verilen izinleri ve izinlerin, paylaşılan erişim imzasının bir parçası olarak geçerli olduğu zaman aralığını belirtebilirsiniz. Daha fazla bilgi için bkz. [paylaşılan erişim Imzalarını kullanma (SAS)](storage-sas-overview.md).

> [!NOTE]
> Azure AD kimlik bilgilerini kullanarak kullanıcıların veya uygulamaların kimliğini doğrulamak, diğer yetkilendirme yolları üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Uygulamalarınızla paylaşılan anahtar yetkilendirmesi kullanmaya devam edebilirsiniz, ancak Azure AD atlama 'yi kullanarak hesap erişim anahtarınızı kodunuzla depolama gereksinimini ortadan kaldırabilirsiniz. Ayrıca, Depolama hesabınızdaki kaynaklara ayrıntılı erişim sağlamak için paylaşılan erişim imzaları (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD SAS belirteçlerini yönetmeye gerek kalmadan veya güvenliği aşılmış bir SAS iptal etme gereksinimi olmadan benzer yetenekler sunmaktadır.
>
> Microsoft, mümkün olduğunda Azure Depolama Blobu ve kuyruk uygulamaları için Azure AD yetkilendirmesi kullanılmasını önerir.

## <a name="copying-data-into-a-storage-account"></a>Bir depolama hesabına veri kopyalama

Microsoft, verilerinizi şirket içi depolama cihazlarından veya üçüncü taraf bulut depolama sağlayıcılardan içeri aktarmaya yönelik yardımcı programlar ve kitaplıklar sağlar. Kullandığınız çözüm, aktardığınız veri miktarına bağlıdır.

Genel amaçlı v1 veya blob depolama hesabından genel amaçlı bir v2 hesabına yükselttiğinizde, verileriniz otomatik olarak geçirilir. Microsoft, hesabınızı yükseltmek için bu patika önerir. Ancak, verileri genel amaçlı v1 hesabından bir BLOB depolama hesabına taşımaya karar verirseniz, aşağıda açıklanan araçları ve kitaplıkları kullanarak verilerinizi el ile geçirebilirsiniz.

### <a name="azcopy"></a>AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy komutunu, mevcut bir genel amaçlı depolama hesabından bir BLOB depolama hesabına kopyalamak veya şirket içi depolama cihazlarından verileri karşıya yüklemek için kullanabilirsiniz. Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Veri hareketi kitaplığı

.NET için Azure Depolama veri taşıma kitaplığı, AzCopy’yi çalıştıran çekirdek veri taşıma altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. AzCopy özelliklerinden yerel olarak yararlanmak için veri taşıma kitaplığını kullanabilirsiniz. Daha fazla bilgi için bkz. [.net Için Azure Storage veri taşıma kitaplığı](https://github.com/Azure/azure-storage-net-data-movement)

### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı

Verilerinizi genel amaçlı v1 depolama hesabından BLOB depolama hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure istemci kitaplıklarından veya Azure Storage Services REST API birini kullanın. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Azure depolama REST API hakkında daha fazla bilgi için bkz. [Azure Storage Services REST API başvurusu](https://docs.microsoft.com/rest/api/storageservices/).

> [!IMPORTANT]
> Bloblar, blobla birlikte istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. İstemci tarafı şifreleme ile şifrelenmiş bir blobu kopyalarsanız, kopyalama işleminin başta şifreleme ile ilgili meta veriler olmak üzere blob meta verilerini koruduğundan emin olun. Bir blobu şifreleme meta verileri olmadan kopyalarsanız blob içeriği tekrar alınamaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Depolama İstemci Tarafı Şifrelemesi](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="storage-account-billing"></a>Depolama hesabı faturalama

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](storage-account-create.md)
- [Blok blobu depolama hesabı oluşturma](../blobs/storage-blob-create-account-block-blob.md)
