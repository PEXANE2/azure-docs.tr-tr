---
title: AzCopy kullanarak Amazon S3 'ten Azure depolama 'ya veri kopyalama | Microsoft Docs
description: AzCopy ve Amazon S3 demetleri ile veri aktarma
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: a3180593eaf8c01c772fd761d88b5f5b9f7657ee
ms.sourcegitcommit: b5106424cd7531c7084a4ac6657c4d67a05f7068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/14/2020
ms.locfileid: "75941500"
---
# <a name="copy-data-from-amazon-s3-to-azure-storage-by-using-azcopy"></a>AzCopy kullanarak Amazon S3 'ten Azure depolama 'ya veri kopyalama

AzCopy, bir depolama hesabına blob veya dosya kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, AzCopy kullanarak Amazon Web Services (AWS) S3 ' den Azure Blob depolama alanına nesneleri, dizinleri ve demetleri kopyalamanıza yardımcı olur.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Yetkilendirme kimlik bilgilerini nasıl sağlayabileceklerini seçin

* Azure depolama ile yetkilendirmek için Azure Active Directory (AD) veya paylaşılan erişim Imzası (SAS) belirteci kullanın.

* AWS S3 ile yetkilendirmek için bir AWS erişim anahtarı ve gizli bir erişim anahtarı kullanın.

### <a name="authorize-with-azure-storage"></a>Azure depolama ile yetkilendirme

AzCopy 'i indirmek için [AzCopy kullanmaya başlama](storage-use-azcopy-v10.md) makalesine bakın ve depolama hizmetine yetkilendirme kimlik bilgilerini nasıl sağlayacağınız seçin.

> [!NOTE]
> Bu makaledeki örneklerde, kimlik doğrulamasının `AzCopy login` komutunu kullanarak doğruladığınızı varsayalım. AzCopy daha sonra blob depolamadaki verilere erişim yetkisi vermek için Azure AD hesabınızı kullanır.
>
> Blob verilerine erişim yetkisi vermek için bir SAS belirteci kullanıyorsanız, bu belirteci her AzCopy komutunda kaynak URL 'sine ekleyebilirsiniz.
>
> Örneğin: `https://mystorageaccount.blob.core.windows.net/mycontainer?<SAS-token>`.

### <a name="authorize-with-aws-s3"></a>AWS S3 ile yetkilendirme

AWS erişim anahtarınızı ve gizli erişim anahtarınızı toplayın ve ardından bu ortam değişkenlerini ayarlayın:

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AWS_ACCESS_KEY_ID=<access-key>`<br>`set AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Linux** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **MacOS** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>`|

## <a name="copy-objects-directories-and-buckets"></a>Nesneleri, dizinleri ve demetleri kopyalama

AzCopy, [URL API 'Den put bloğunu](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) kullanır, bu nedenle veriler doğrudan AWS S3 ve depolama sunucuları arasında kopyalanır. Bu kopyalama işlemleri bilgisayarınızın ağ bant genişliğini kullanmaz.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Kopyalama işleminden sonra S3 demetlerinden verileri kaldırmaya karar verirseniz, verileri kaldırmadan önce verilerin depolama hesabınıza düzgün şekilde kopyalandığından emin olun.

> [!TIP]
> Bu bölümdeki örnekler, yol bağımsız değişkenlerini tek tırnak (' ') ile çevreler. Windows komut kabuğu (cmd. exe) dışında tüm komut kabukleriyle tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd. exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

 Bu örnekler, hiyerarşik bir ad alanı olan hesaplarla de çalışır. [Data Lake Storage çoklu protokol erişimi](../blobs/data-lake-storage-multi-protocol-access.md) , bu HESAPLARDA aynı URL sözdizimini (`blob.core.windows.net`) kullanmanıza olanak sağlar. 

### <a name="copy-an-object"></a>Nesne kopyalama

Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini (`blob.core.windows.net`) kullanın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>/<object-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>'` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com/mybucket/myobject' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myblob'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://s3.amazonaws.com/mybucket/myobject' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myblob'` |

> [!NOTE]
> Bu makaledeki örnekler AWS S3 demetleri için yol stili URL 'Leri kullanır (örneğin: `http://s3.amazonaws.com/<bucket-name>`). 
>
> Ayrıca, sanal barındırılan stil URL 'Leri de kullanabilirsiniz (örneğin: `http://bucket.s3.amazonaws.com`). 
>
> Demetlerin sanal barındırılması hakkında daha fazla bilgi edinmek için bkz. [demetlerin sanal barındırılması]] (https://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html).

### <a name="copy-a-directory"></a>Bir dizini kopyalama

Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini (`blob.core.windows.net`) kullanın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>/<directory-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-a-bucket"></a>Demeti kopyalama

Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini (`blob.core.windows.net`) kullanın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com/mybucket' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-all-buckets-in-all-regions"></a>Tüm bölgelerdeki tüm demetleri Kopyala

Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini (`blob.core.windows.net`) kullanın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy 'https://s3.amazonaws.com/' 'https://<storage-account-name>.blob.core.windows.net' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com' 'https://mystorageaccount.blob.core.windows.net' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-all-buckets-in-a-specific-s3-region"></a>Belirli bir S3 bölgesindeki tüm demetleri Kopyala

Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini (`blob.core.windows.net`) kullanın.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy 'https://s3-<region-name>.amazonaws.com/' 'https://<storage-account-name>.blob.core.windows.net' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3-rds.eu-north-1.amazonaws.com' 'https://mystorageaccount.blob.core.windows.net' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

## <a name="handle-differences-in-object-naming-rules"></a>Nesne adlandırma kurallarındaki farkları işleme

AWS S3, demet adları için Azure Blob kapsayıcılarıyla karşılaştırıldığında farklı bir adlandırma kuralları kümesine sahiptir. [Buradan](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules)hakkında bilgi edinebilirsiniz. Bir demet grubunu bir Azure depolama hesabına kopyalamayı seçerseniz, adlandırma farklılıkları nedeniyle kopyalama işlemi başarısız olabilir.

AzCopy, ortaya çıkabilecek en yaygın sorunların ikisini de gerçekleştirir; ardışık kısa çizgi içeren noktalar ve demetleri içeren demetler. AWS S3 demet adları nokta ve ardışık tireler içerebilir, ancak Azure 'da bir kapsayıcı olamaz. AzCopy, ardışık çizgilerden oluşan sayıları temsil eden bir sayı ile kısa çizgilerden ve ardışık çizgilerden oluşan noktaları değiştirir (örneğin: `my----bucket` adlı bir demet `my-4-bucket`olur. 

Ayrıca, dosya üzerinde AzCopy kopyaları olarak, adlandırma çakışmalarını denetler ve bunları çözmeye çalışır. Örneğin, adı `bucket-name` ve `bucket.name`olan demetler varsa AzCopy, önce `bucket.name` adlı bir demeti `bucket-name` ve sonra `bucket-name-2`olarak çözer.

## <a name="handle-differences-in-object-metadata"></a>Nesne meta verilerinde farkları işleme

AWS S3 ve Azure, nesne anahtarlarının adlarındaki farklı karakter kümelerine izin veriyor. AWS S3 tarafından [burada](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys)kullanılan karakterler hakkında bilgi edinebilirsiniz. Azure tarafında, blob nesne anahtarları [ C# tanımlayıcılar](https://docs.microsoft.com/dotnet/csharp/language-reference/)için adlandırma kurallarına uyar.

AzCopy `copy` komutunun bir parçası olarak, dosyanın meta verilerinin uyumsuz anahtar adlarını içerdiği dosyaları nasıl işlemek istediğinizi belirten isteğe bağlı bir `s2s-invalid-metadata-handle` bayrağı değeri sağlayabilirsiniz. Aşağıdaki tabloda her bayrak değeri açıklanmaktadır.

| Bayrak değeri | Açıklama  |
|--------|-----------|
| **Excludeifgeçersiz** | (Varsayılan seçenek) Meta veriler aktarılan nesneye dahil değildir. AzCopy bir uyarı kaydeder. |
| **Failifgeçersiz** | Nesneler kopyalanmaz. AzCopy bir hatayı günlüğe kaydeder ve aktarım özetinde görüntülenen başarısız olan sayıma bu hatayı ekler.  |
| **Renameifgeçersiz**  | AzCopy geçersiz meta veri anahtarını çözer ve çözümlenen meta veri anahtar değer çiftini kullanarak nesneyi Azure 'a kopyalar. AzCopy 'ın nesne anahtarlarını yeniden adlandırma hakkında tam olarak hangi adımların alınacağını öğrenmek için aşağıdaki aşağıdaki [nesne anahtarlarını](#rename-logic) yeniden adlandırma bölümüne bakın. AzCopy anahtarı yeniden adlandıramaz, nesne kopyalanmaz. |

<a id="rename-logic" />

### <a name="how-azcopy-renames-object-keys"></a>AzCopy nesne anahtarlarını yeniden adlandırmalar

AzCopy şu adımları gerçekleştirir:

1. Geçersiz karakterleri ' _ ' ile değiştirir.

2. Yeni bir geçerli anahtarın başlangıcına `rename_` dize ekler.

   Bu anahtar, özgün meta veri **değerini**kaydetmek için kullanılacaktır.

3. Yeni bir geçerli anahtarın başlangıcına `rename_key_` dize ekler.
   Bu anahtar, özgün meta veriler geçersiz **anahtarını**kaydetmek için kullanılacaktır.
   Meta veri anahtarı BLOB depolama hizmetindeki bir değer olarak korunduğu için Azure tarafında meta verileri denemek ve kurtarmak üzere bu anahtarı kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerden herhangi birinde daha fazla örnek bulabilirsiniz:

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)

- [AzCopy ve BLOB Storage ile veri aktarma](storage-use-azcopy-blobs.md)

- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)