---
title: AzCopy kullanarak Amazon S3'ten Azure Depolama'ya verileri kopyalama | Microsoft Dokümanlar
description: AzCopy ve Amazon S3 kovaları ile veri aktarımı
services: storage
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 01/13/2020
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: a3180593eaf8c01c772fd761d88b5f5b9f7657ee
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75941500"
---
# <a name="copy-data-from-amazon-s3-to-azure-storage-by-using-azcopy"></a>AzCopy kullanarak Amazon S3'ten Azure Depolama'ya verileri kopyalama

AzCopy, bir depolama hesabına veya bir depolama hesabından blobveya dosya kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, AzCopy kullanarak Amazon Web Services (AWS) S3'ten Azure blob depolamasına kadar nesneleri, dizinleri ve kovaları kopyalamanıza yardımcı olur.

## <a name="choose-how-youll-provide-authorization-credentials"></a>Yetkilendirme kimlik bilgilerini nasıl sağlayacağınızı seçin

* Azure Depolama ile yetkilendirmek için Azure Etkin Dizin (AD) veya Paylaşılan Erişim İmzası (SAS) belirteci kullanın.

* AWS S3 ile yetkilendirmek için bir AWS erişim anahtarı ve gizli erişim anahtarı kullanın.

### <a name="authorize-with-azure-storage"></a>Azure Depolama ile Yetkilendirme

AzCopy'yi indirmek için [AzCopy makalesini](storage-use-azcopy-v10.md) başlatın ve depolama hizmetine yetki kimlik bilgilerini nasıl sağlayacağınızı seçin.

> [!NOTE]
> Bu makaledeki örnekler, komutu kullanarak kimliğinizi doğruladığınızı `AzCopy login` varsayar. AzCopy daha sonra Blob depolamasundaki verilere erişimi yetkilendirmek için Azure AD hesabınızı kullanır.
>
> Blob verilerine erişimi yetkilendirmek için bir SAS belirteci kullanmak isterseniz, bu belirteci her AzCopy komutundaki kaynak URL'ye ekleyebilirsiniz.
>
> Örneğin: `https://mystorageaccount.blob.core.windows.net/mycontainer?<SAS-token>`.

### <a name="authorize-with-aws-s3"></a>AWS S3 ile yetkilendirme

AWS erişim anahtarınızı ve gizli erişim anahtarınızı toplayın ve aşağıdaki ortam değişkenlerini ayarlayın:

| İşletim sistemi | Komut  |
|--------|-----------|
| **Windows** | `set AWS_ACCESS_KEY_ID=<access-key>`<br>`set AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Linux** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>` |
| **Macos** | `export AWS_ACCESS_KEY_ID=<access-key>`<br>`export AWS_SECRET_ACCESS_KEY=<secret-access-key>`|

## <a name="copy-objects-directories-and-buckets"></a>Nesneleri, dizinleri ve kovaları kopyalama

AzCopy [URL API'den Blok La'yı](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) kullanır, böylece veriler doğrudan AWS S3 ve depolama sunucuları arasında kopyalanır. Bu kopyalama işlemleri bilgisayarınızın ağ bant genişliğini kullanmaz.

> [!IMPORTANT]
> Bu özellik şu anda önizleme sürümündedir. Bir kopyalama işleminden sonra S3 kovalarınızdan verileri kaldırmaya karar verirseniz, verileri kaldırmadan önce verilerin depolama hesabınıza doğru kopyalandığını doğruladığını zedebilirsiniz.

> [!TIP]
> Bu bölümdeki örnekler, yol bağımsız değişkenlerini tek tırnak ('') ile kaplar. Windows Komut Uyruşu (cmd.exe) hariç tüm komut kabuklarında tek tırnak işareti kullanın. Windows Komut Uyruşu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak ('') yerine çift tırnak ("") içeren bir şekilde içine çekin.

 Bu örnekler, hiyerarşik ad alanı olan hesaplarla da çalışır. [Veri Gölü Depolama'daki çoklu protokol erişimi,](../blobs/data-lake-storage-multi-protocol-access.md) bu hesaplarda`blob.core.windows.net`aynı URL sözdizimini () kullanmanıza olanak tanır. 

### <a name="copy-an-object"></a>Nesneyi kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>/<object-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>/<blob-name>'` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com/mybucket/myobject' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myblob'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://s3.amazonaws.com/mybucket/myobject' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myblob'` |

> [!NOTE]
> Bu makaledeki örnekler, AWS S3 kovaları için yol stili `http://s3.amazonaws.com/<bucket-name>`URL'leri kullanır (Örneğin: ). 
>
> Sanal barındırılan stil URL'lerini de kullanabilirsiniz `http://bucket.s3.amazonaws.com`(Örneğin: ). 
>
> Kovaların sanal barındırma sı hakkında daha fazla bilgi edinmek içinhttps://docs.aws.amazon.com/AmazonS3/latest/dev/VirtualHosting.html)[Kovaların Sanal Barındırma]]'una bakın.

### <a name="copy-a-directory"></a>Bir dizini kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>/<directory-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-a-bucket"></a>Kova kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://s3.amazonaws.com/<bucket-name>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com/mybucket' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-all-buckets-in-all-regions"></a>Tüm bölgelerdeki tüm kovaları kopyala

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://s3.amazonaws.com/' 'https://<storage-account-name>.blob.core.windows.net' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3.amazonaws.com' 'https://mystorageaccount.blob.core.windows.net' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

### <a name="copy-all-buckets-in-a-specific-s3-region"></a>Belirli bir S3 bölgesindeki tüm kovaları kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://s3-<region-name>.amazonaws.com/' 'https://<storage-account-name>.blob.core.windows.net' --recursive=true` |
| **Örnek** | `azcopy copy 'https://s3-rds.eu-north-1.amazonaws.com' 'https://mystorageaccount.blob.core.windows.net' --recursive=true` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://s3.amazonaws.com/mybucket/mydirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/mydirectory' --recursive=true` |

## <a name="handle-differences-in-object-naming-rules"></a>Nesne adlandırma kurallarındaki farklılıkları işleme

AWS S3, Azure blob kapsayıcılarına kıyasla kova adları için farklı bir adlandırma kuralı kümesine sahiptir. [Burada](https://docs.aws.amazon.com/AmazonS3/latest/dev/BucketRestrictions.html#bucketnamingrules)onlar hakkında okuyabilirsiniz. Bir grup kovayı bir Azure depolama hesabına kopyalamayı seçerseniz, adlandırma farklılıkları nedeniyle kopyalama işlemi başarısız olabilir.

AzCopy ortaya çıkabilecek en yaygın sorunlardan ikisini işler; dönemleri ve ardışık tireler içeren kovalar. AWS S3 kova adları dönemleri ve ardışık tireler içerebilir, ancak Azure'daki bir kapsayıcı bunu yapamaz. AzCopy, dönemleri tireler ve ardışık tirelerle, ardışık tire sayısını temsil eden bir sayıyla değiştirir (Örneğin: adlı `my----bucket` bir kova olur. `my-4-bucket` 

Ayrıca, AzCopy dosyaları üzerinde kopyalar gibi, bu adlandırma çakışması için denetler ve bunları çözmek için çalışır. Örneğin, adı `bucket-name` ve `bucket.name`, AzCopy ilk `bucket.name` `bucket-name` ve sonra `bucket-name-2`adlı bir kova giderir kova varsa .

## <a name="handle-differences-in-object-metadata"></a>Nesne meta verilerindeki farklılıkları işleme

AWS S3 ve Azure, nesne anahtarlarının adlarında farklı karakter kümelerine izin verir. Burada AWS S3 [kullandığı](https://docs.aws.amazon.com/AmazonS3/latest/dev/UsingMetadata.html#object-keys)karakterler hakkında okuyabilirsiniz. Azure tarafında, blob nesne anahtarları [C# tanımlayıcıları](https://docs.microsoft.com/dotnet/csharp/language-reference/)için adlandırma kurallarına bağlıdır.

AzCopy `copy` komutunun bir parçası olarak, dosyanın `s2s-invalid-metadata-handle` meta verilerinin uyumsuz anahtar adları içerdiği dosyaları nasıl işlemek istediğinizi belirten isteğe bağlı bayrak için bir değer sağlayabilirsiniz. Aşağıdaki tabloda her bayrak değeri açıklanmaktadır.

| Bayrak değeri | Açıklama  |
|--------|-----------|
| **ExcludeIfGeçersiz** | (Varsayılan seçenek) Meta veriler aktarılan nesneye dahil değildir. AzCopy bir uyarı kaydeder. |
| **FailIfGeçersiz** | Nesneler kopyalanmıyor. AzCopy bir hatayı kaydeder ve aktarım özetinde görünen başarısız sayımda bu hatayı içerir.  |
| **Yeniden AdlandırmaGeçersiz**  | AzCopy geçersiz meta veri anahtarını çözer ve çözülmüş meta veri anahtar değeri çiftini kullanarak nesneyi Azure'a kopyalar. AzCopy nesne anahtarlarını yeniden adlandırmak için tam olarak hangi adımları atar öğrenmek için aşağıdaki [AzCopy nesne anahtarlarını nasıl yeniden adlandırır](#rename-logic) bölümüne bakın. AzCopy anahtarı yeniden adlandıramıyorsa, nesne kopyalanamaz. |

<a id="rename-logic" />

### <a name="how-azcopy-renames-object-keys"></a>AzCopy nesne anahtarlarını yeniden adlandırır

AzCopy şu adımları gerçekleştirir:

1. Geçersiz karakterleri '_' ile değiştirir.

2. Dizeyi `rename_` yeni bir geçerli anahtarın başına ekler.

   Bu anahtar, özgün meta veri **değerini**kaydetmek için kullanılacaktır.

3. Dizeyi `rename_key_` yeni bir geçerli anahtarın başına ekler.
   Bu anahtar, özgün meta verileri geçersiz **anahtarı**kaydetmek için kullanılacaktır.
   Meta veri anahtarı Blob depolama hizmetinde bir değer olarak korunduğundan, bu anahtarı Azure tarafındaki meta verileri kurtarmak için kullanabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerin herhangi birinde daha fazla örnek bulabilirsiniz:

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)

- [AzCopy ve blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)

- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)

- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)