---
title: Depolama hesabına genel bakış
titleSuffix: Azure Storage
description: Azure Depolama hesabı oluşturma ve kullanma seçeneklerini anlayın.
services: storage
author: tamram
ms.service: storage
ms.topic: conceptual
ms.date: 01/17/2020
ms.author: tamram
ms.subservice: common
ms.openlocfilehash: 3694a1d04ec25f44cfcf9395bdd148e2fd3c0d9f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79371571"
---
# <a name="storage-account-overview"></a>Depolama hesabına genel bakış

Azure depolama hesabı, tüm Azure Depolama veri nesnelerinizin tümlerini içerir: lekeler, dosyalar, kuyruklar, tablolar ve diskler. Depolama hesabı, Azure Depolama verileriniz için dünyanın herhangi bir yerinden HTTP veya HTTPS üzerinden erişilebilen benzersiz bir ad alanı sağlar. Azure depolama hesabınızdaki veriler dayanıklıdır ve yüksek oranda kullanılabilir, güvenli ve büyük ölçüde ölçeklenebilir.

Azure depolama hesabı oluşturmayı öğrenmek için [bkz.](storage-account-create.md)

## <a name="types-of-storage-accounts"></a>Depolama hesabı türleri

[!INCLUDE [storage-account-types-include](../../../includes/storage-account-types-include.md)]

### <a name="general-purpose-v2-accounts"></a>Genel amaçlı v2 hesapları

Genel amaçlı v2 depolama hesapları en son Azure Depolama özelliklerini destekler ve genel amaçlı v1 ve Blob depolama hesaplarının tüm işlevselliğini içerir. Genel amaçlı v2 hesapları, Azure Depolama için gigabayt başına en düşük kapasite fiyatlarının yanı sıra endüstriyle rekabet eden işlem fiyatlarını da sunar. Genel amaçlı v2 depolama hesapları şu Azure Depolama hizmetlerini destekler:

- Blobs (tüm türler: Blok, Ek, Sayfa)
- Veri Gölü Gen2
- Dosyalar
- Diskler
- Kuyruklar
- Tablolar

> [!NOTE]
> Microsoft, çoğu senaryo için genel amaçlı bir v2 depolama hesabı kullanmanızı önerir. Genel amaçlı bir v1 veya Blob depolama hesabını, kesinti olmaksızın ve veri kopyalamaya gerek kalmadan genel amaçlı bir v2 hesabına kolayca yükseltebilirsiniz.
>
> Genel amaçlı bir v2 hesabına yükseltme hakkında daha fazla bilgi için genel [amaçlı v2 depolama hesabına yükseltme](storage-account-upgrade.md)konusuna bakın.

Genel amaçlı v2 depolama hesapları, kullanım alışkanlıklarınıza göre veri depolamak için birden çok erişim katmanı sunar. Daha fazla bilgi [için, blok blob verileri için Access katmanlarına](#access-tiers-for-block-blob-data)bakın.

### <a name="general-purpose-v1-accounts"></a>Genel amaçlı v1 hesapları

Genel amaçlı v1 depolama hesapları tüm Azure Depolama hizmetlerine erişim sağlar, ancak en son özelliklere veya gigabayt başına en düşük fiyatlandırmaya sahip olmayabilir. Genel amaçlı v1 depolama hesapları şu Azure Depolama hizmetlerini destekler:

- Blobs (her türlü)
- Dosyalar
- Diskler
- Kuyruklar
- Tablolar

Çoğu durumda genel amaçlı v2 hesapları kullanmalısınız. Bu senaryolar için genel amaçlı v1 hesaplarını kullanabilirsiniz:

- Uygulamalarınız Azure klasik dağıtım modelini gerektirir. Genel amaçlı v2 hesapları ve Blob depolama hesapları yalnızca Azure Kaynak Yöneticisi dağıtım modelini destekler.

- Uygulamalarınız işlem yoğun veya önemli coğrafi çoğaltma bant genişliği kullanın, ancak büyük kapasite gerektirmez. Bu durumda, genel amaçlı v1 en ekonomik seçim olabilir.

- Depolama Hizmetleri REST [API'sinin](https://msdn.microsoft.com/library/azure/dd894041.aspx) 2014-02-14'ten önceki bir sürümünü veya 4.x'ten daha düşük bir sürümü olan istemci kitaplığını kullanırsınız. Uygulamanızı yükseltemezsiniz.

### <a name="blockblobstorage-accounts"></a>BlockBlobStorage hesapları

BlockBlobStorage hesabı, yapılandırılmamış nesne verilerini blok blobs veya ek blobs olarak depolamak için premium performans katmanında özel bir depolama hesabıdır. BlockBlobStorage hesapları, genel amaçlı v2 ve BlobStorage hesaplarıyla karşılaştırıldığında düşük, tutarlı gecikme ve daha yüksek işlem oranları sağlar.

BlockBlobStorage hesapları şu anda katmanlamayı sıcak, havalı veya arşiv erişim katmanlarına kadar desteklemiyor. Bu tür depolama hesabı sayfa lekelerini, tabloları veya kuyrukları desteklemez.

### <a name="filestorage-accounts"></a>FileStorage hesapları

FileStorage hesabı, premium dosya paylaşımlarını depolamak ve oluşturmak için kullanılan özel bir depolama hesabıdır. Bu depolama hesabı türü dosyaları destekler, ancak blobs, ek blobs, sayfa lekeleri, tablolar veya kuyrukları engellemez.

FileStorage hesapları, IOPS patlaması gibi benzersiz performansa adanmış özellikler sunar. Bu özellikler hakkında daha fazla bilgi için, Dosyalar planlama kılavuzunun [Dosya paylaşımı depolama katmanları](../files/storage-files-planning.md#storage-tiers) bölümüne bakın.

## <a name="naming-storage-accounts"></a>Depolama hesaplarını adlandırma

Depolama hesabınızı adlandırırken şu kuralları göz önünde bulundurun:

- Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir.
- Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. İki depolama hesabı aynı ada sahip olamaz.

## <a name="performance-tiers"></a>Performans katmanları

Oluşturduğunuz depolama hesabının türüne bağlı olarak, standart ve premium performans katmanları arasında seçim yapabilirsiniz.

### <a name="general-purpose-storage-accounts"></a>Genel amaçlı depolama hesapları

Genel amaçlı depolama hesapları aşağıdaki performans katmanlarından biri için yapılandırılabilir:

- Blobs, dosyalar, tablolar, kuyruklar ve Azure sanal makine diskleri depolamak için standart bir performans katmanı. Standart depolama hesapları için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için, [standart depolama hesapları için Ölçeklenebilirlik hedeflerine](scalability-targets-standard-account.md)bakın.
- Yönetilmeyen sanal makine disklerini depolamak için üstün bir performans katmanı. Microsoft, yönetilmeyen diskler yerine Azure sanal makineleri ile yönetilen diskler kullanmanızı önerir. Premium performans katmanı için ölçeklenebilirlik hedefleri hakkında daha fazla bilgi için, [premium sayfa blob depolama hesapları için Ölçeklenebilirlik hedeflerine](../blobs/scalability-targets-premium-page-blobs.md)bakın.

### <a name="blockblobstorage-storage-accounts"></a>BlockBlobStorage depolama hesapları

BlockBlobStorage depolama hesapları, blok lekeleri ve ek blobs depolamak için bir prim performans katmanı sağlar. Daha fazla bilgi için, [premium blok blob depolama hesapları için Ölçeklenebilirlik hedeflerine](../blobs/scalability-targets-premium-block-blobs.md)bakın.

### <a name="filestorage-storage-accounts"></a>FileStorage depolama hesapları

FileStorage depolama hesapları, Azure dosya paylaşımları için bir premium performans katmanı sağlar. Daha fazla bilgi için Azure [Dosyaları ölçeklenebilirliği ve performans hedefleri'ne](../files/storage-files-scale-targets.md)bakın.

## <a name="access-tiers-for-block-blob-data"></a>Blok blob verileri için erişim katmanları

Azure Depolama, kullanım desenlerine dayalı blok blob verilerine erişmek için farklı seçenekler sunar. Azure Depolama'daki her erişim katmanı belirli bir veri kullanımı modeli için optimize edilebiyi kullanır. İhtiyaçlarınız için doğru erişim katmanını seçerek, blok blob verilerinizi en uygun maliyetli şekilde depolayabilirsiniz.

Kullanılabilir erişim katmanları şunlardır:

- **Sıcak** erişim katmanı. Bu katman, depolama hesabındaki nesnelerin sık erişeabilmesi için optimize edilebiyi optimize edin. Depolama maliyetleri daha yüksekken, sıcak katmandaki verilere erişmek en uygun maliyetli dir. Yeni depolama hesapları varsayılan olarak sıcak katmanda oluşturulur.
- **Cool** erişim katmanı. Bu katman, seyrek erişilen ve en az 30 gün boyunca depolanan büyük miktarda veriyi depolamak için optimize edilmiştir. Verileri serin katmanda depolamak daha uygun maliyetlidir, ancak bu verilere erişmek sıcak katmandaki verilere erişmekten daha pahalı olabilir.
- **Arşiv** katmanı. Bu katman yalnızca tek tek blok lekeleri için kullanılabilir. Arşiv katmanı, birkaç saatlik geri alma gecikmesini tolere edebilen ve arşiv katmanında en az 180 gün kalacak veriler için optimize edilecektir. Arşiv katmanı, veri depolamak için en uygun maliyetli seçenektir. Ancak, bu verilere erişmek, sıcak veya serin katmanlarda verilere erişmekten daha pahalıdır.

Verilerinizin kullanım deseninde bir değişiklik varsa, istediğiniz zaman bu erişim katmanları arasında geçiş yapabilirsiniz. Erişim katmanları hakkında daha fazla bilgi için [Azure Blob depolama alanına bakın: sıcak, serin ve arşiv erişim katmanları.](../blobs/storage-blob-storage-tiers.md)

> [!IMPORTANT]
> Varolan bir depolama hesabı veya blob için erişim katmanının değiştirilmesi ek ücretlere neden olabilir. Daha fazla bilgi için [Depolama hesabı faturalandırma bölümüne](#storage-account-billing)bakın.

## <a name="redundancy"></a>Yedeklilik

[!INCLUDE [storage-common-redundancy-options](../../../includes/storage-common-redundancy-options.md)]

## <a name="encryption"></a>Şifreleme

Depolama hesabınızdaki tüm veriler servis tarafında şifrelenir. Şifreleme hakkında daha fazla bilgi [için, veriler için Azure Depolama Hizmeti Şifrelemesi'ne](storage-service-encryption.md)bakın.

## <a name="storage-account-endpoints"></a>Depolama hesabı uç noktaları

Depolama hesabı, verileriniz için Azure'da benzersiz bir ad alanı sağlar. Azure Depolama'da depoladığınız her nesnenin benzersiz hesap adınızı içeren bir adresi vardır. Hesap adı ve Azure Depolama hizmeti bitiş noktasının birleşimi depolama hesabınız için uç noktaları oluşturur.

Örneğin, genel amaçlı depolama *hesabınıza mystorage account*adı verebiliyorsa, bu hesabın varsayılan uç noktaları şunlardır:

- Blob depolama:`https://*mystorageaccount*.blob.core.windows.net`
- Tablo depolama:`https://*mystorageaccount*.table.core.windows.net`
- Sıra depolama:`https://*mystorageaccount*.queue.core.windows.net`
- Azure Dosyaları:`https://*mystorageaccount*.file.core.windows.net`

> [!NOTE]
> Blob ve blob depolama hesaplarını blok yalnızca Blob hizmet bitiş noktasını ortaya çıkarır.

Nesnenin depolama hesabındaki konumunu bitiş noktasına ekleyerek bir depolama hesabındaki bir nesneye erişmek için URL'yi oluşturun. Örneğin bir blob adresi şu biçimde olabilir: http://*mystorageaccount*.blob.core.windows.net/*mycontainer*/*myblob*.

Ayrıca, depolama hesabınızı lekeler için özel bir etki alanı kullanacak şekilde yapılandırabilirsiniz. Daha fazla bilgi için bkz. Azure [Depolama hesabınız için özel bir etki alanı adı yapılandırın.](../blobs/storage-custom-domain-name.md)  

## <a name="control-access-to-account-data"></a>Hesap verilerine erişimi denetleme

Varsayılan olarak, hesabınızdaki veriler yalnızca siz, yani hesap sahibi tarafından kullanılabilir. Verilerinize kimlerin erişebileceğini ve hangi izinlere sahip olduklarını kontrol elabilirsiniz.

Depolama hesabınıza karşı yapılan her istek yetkili olmalıdır. Hizmet düzeyinde, istek geçerli bir *Yetkilendirme* üstbilgisini içermelidir. Özellikle, bu üstbilgi, hizmetin isteği yürütmeden önce doğrulanması için gereken tüm bilgileri içerir.

Aşağıdaki yaklaşımlardan herhangi birini kullanarak depolama hesabınızdaki verilere erişim izni verebilirsiniz:

- **Azure Etkin Dizini:** Blob ve kuyruk verilerine erişmek için bir kullanıcının, grubun veya başka bir kimliğin kimliğini doğrulamak için Azure Etkin Dizin (Azure AD) kimlik bilgilerini kullanın. Bir kimliğin kimlik doğrulaması başarılı olursa, Azure AD isteği Azure Blob depolama alanına veya Sıra depolama alanına yetkilendirmede kullanılacak bir belirteci döndürür. Daha fazla bilgi için Azure [Etkin Dizin'i kullanarak Azure Depolama'ya Kimlik Doğrulama erişimi](storage-auth-aad.md)ne rendesi 'ye bakın.
- **Paylaşılan Anahtar yetkilendirmesi:** Azure Depolama'ya erişmek için uygulamanızın çalışma zamanında kullandığı bir bağlantı dizesi oluşturmak için depolama hesabı erişim anahtarınızı kullanın. Bağlantı dizesindeki değerler, Azure Depolama'ya geçirilen *Yetkilendirme* üstbilgisini oluşturmak için kullanılır. Daha fazla bilgi için [bkz.](storage-configure-connection-string.md)
- **Paylaşılan erişim imzası:** Azure AD yetkilendirmesini kullanmıyorsanız, depolama hesabınızdaki kaynaklara erişimi devretmek için paylaşılan erişim imzasını kullanın. Paylaşılan erişim imzası, URL'deki Azure Depolama'ya bir isteği yetkilendirmek için gereken tüm bilgileri kapsayan bir belirteçtir. Depolama kaynağını, verilen izinleri ve izinlerin paylaşılan erişim imzasının bir parçası olarak geçerli olduğu aralığı belirtebilirsiniz. Daha fazla bilgi için bkz. [paylaşılan erişim imzalarını (SAS) kullanma.](storage-sas-overview.md)

> [!NOTE]
> Azure AD kimlik bilgilerini kullanarak kullanıcıların veya uygulamaların doğrulaması, diğer yetkilendirme araçları üzerinde üstün güvenlik ve kullanım kolaylığı sağlar. Uygulamalarınızla Paylaşılan Anahtar yetkilendirmesini kullanmaya devam edebilirken, Azure AD'yi kullanmak, hesap erişim anahtarınızı kodunuzla depolama gereksinimini alar. Depolama hesabınızdaki kaynaklara ayrıntılı erişim sağlamak için paylaşılan erişim imzalarını (SAS) kullanmaya devam edebilirsiniz, ancak Azure AD, SAS belirteçlerini yönetmeye veya gizliliği ihlal edilmiş bir SAS'ı iptal etme endişesi duymadan benzer özellikler sunar.
>
> Microsoft, Azure Depolama blob ve sıra uygulamalarınız için mümkün olduğunda Azure AD yetkilendirmesi kullanmanızı önerir.

## <a name="copying-data-into-a-storage-account"></a>Verileri depolama hesabına kopyalama

Microsoft, verilerinizi şirket içi depolama aygıtlarından veya üçüncü taraf bulut depolama sağlayıcılarından almak için yardımcı programlar ve kitaplıklar sağlar. Hangi çözümü kullandığınız aktardığınız veri miktarına bağlıdır.

Genel amaçlı bir v1 veya Blob depolama hesabından genel amaçlı bir v2 hesabına yükselttiğinde, verileriniz otomatik olarak geçirilir. Microsoft, hesabınızı yükseltmek için bu yolu önerir. Ancak, verileri genel amaçlı bir v1 hesabından Blob depolama hesabına taşımaya karar verirseniz, aşağıda açıklanan araçları ve kitaplıkları kullanarak verilerinizi el ile aktarırsınız.

### <a name="azcopy"></a>AzCopy

AzCopy, verilerin Azure Storage’a ve Azure Storage’dan yüksek performansla kopyalanması için tasarlanmış bir Windows komut satırı yardımcı programıdır. AzCopy'i, verileri varolan genel amaçlı bir depolama hesabından Blob depolama hesabına kopyalamak veya şirket içi depolama aygıtlarından veri yüklemek için kullanabilirsiniz. Daha fazla bilgi için bkz. [AzCopy Komut Satırı Yardımcı Programı ile Veri Aktarma](../common/storage-use-azcopy.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

### <a name="data-movement-library"></a>Veri hareketi kitaplığı

.NET için Azure Depolama veri taşıma kitaplığı, AzCopy’yi çalıştıran çekirdek veri taşıma altyapısını temel alır. Kitaplık, AzCopy’ye benzer yüksek performanslı, güvenilir ve kolay veri aktarımı işlemleri için tasarlanmıştır. AzCopy özelliklerinden yerel olarak yararlanmak için veri hareketi kitaplığını kullanabilirsiniz. Daha fazla bilgi için [.NET için Azure Depolama Veri Hareketi Kitaplığı'na](https://github.com/Azure/azure-storage-net-data-movement) bakın

### <a name="rest-api-or-client-library"></a>REST API’si veya istemci kitaplığı

Verilerinizi genel amaçlı bir v1 depolama hesabından Blob depolama hesabına geçirmek için özel bir uygulama oluşturabilirsiniz. Azure istemci kitaplıklarından birini veya Azure depolama hizmetleri REST API'yi kullanın. Azure Storage NET, Java, C++, Node.JS, PHP, Ruby ve Python gibi birden fazla dilde ve platformda zengin istemci kitaplıkları sağlar. İstemci kitaplıkları yeniden deneme mantığı, günlüğe kaydetme ve paralel karşıya yüklemeler gibi gelişmiş özellikler sunar. HTTP/HTTPS istekleri yapan herhangi bir dil tarafından çağrılabilen REST API’sine karşı doğrudan da geliştirebilirsiniz.

Azure Depolama REST API'si hakkında daha fazla bilgi için Azure [Depolama Hizmetleri REST API Başvurusu'na](https://docs.microsoft.com/rest/api/storageservices/)bakın.

> [!IMPORTANT]
> Bloblar, blobla birlikte istemci tarafı şifreleme depolama şifrelemesiyle ilgili meta veriler kullanılarak depolanır. İstemci tarafı şifreleme ile şifrelenmiş bir blobu kopyalarsanız, kopyalama işleminin başta şifreleme ile ilgili meta veriler olmak üzere blob meta verilerini koruduğundan emin olun. Bir blobu şifreleme meta verileri olmadan kopyalarsanız blob içeriği tekrar alınamaz. Şifrelemeyle ilgili meta veriler hakkında daha fazla bilgi için bkz. [Azure Depolama İstemci Tarafı Şifrelemesi](../common/storage-client-side-encryption.md?toc=%2fazure%2fstorage%2fblobs%2ftoc.json).

## <a name="storage-account-billing"></a>Depolama hesabı faturalama

[!INCLUDE [storage-account-billing-include](../../../includes/storage-account-billing-include.md)]

## <a name="next-steps"></a>Sonraki adımlar

- [Depolama hesabı oluşturma](storage-account-create.md)
- [Blok blobu depolama hesabı oluşturma](../blobs/storage-blob-create-account-block-blob.md)
