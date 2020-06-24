---
title: AzCopy ile v10 arasındaki kullanarak Azure Blob depolama alanına veya buradan veri aktarma | Microsoft Docs
description: Bu makale, kapsayıcılar oluşturmanıza, dosyaları kopyalamaya ve yerel dosya sistemleri ile kapsayıcılar arasında dizin eşitlemenize yardımcı olan AzCopy örnek komutlarının bir koleksiyonunu içerir.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: da5eafbdffccc8175b5248ce7b9456d26adf97a2
ms.sourcegitcommit: 1383842d1ea4044e1e90bd3ca8a7dc9f1b439a54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/16/2020
ms.locfileid: "84817554"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>AzCopy ve BLOB Storage ile veri aktarma

AzCopy, depolama hesaplarına veri kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, blob depolamayla çalışan örnek komutlar içerir.

> [!TIP]
> Bu makaledeki örneklerde, tek tırnak (' ') ile yol bağımsız değişkenleri yer alınmalıdır. Windows komut kabuğu (cmd.exe) hariç tüm komut kabukta tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

## <a name="get-started"></a>başlarken

AzCopy ['i indirmek Için AzCopy ile çalışmaya başlama](storage-use-azcopy-v10.md) makalesini inceleyin ve depolama hizmetine yetkilendirme kimlik bilgilerini nasıl sağlayabileceğiniz yolları hakkında bilgi edinin.

> [!NOTE]
> Bu makaledeki örneklerde, komutunu kullanarak kimliğinizi doğruladığınızı varsayalım `AzCopy login` . AzCopy daha sonra blob depolamadaki verilere erişim yetkisi vermek için Azure AD hesabınızı kullanır.
>
> Blob verilerine erişim yetkisi vermek için bir SAS belirteci kullanıyorsanız, bu belirteci her AzCopy komutunda kaynak URL 'sine ekleyebilirsiniz.
>
> Örneğin: `'https://<storage-account-name>.blob.core.windows.net/<container-name><SAS-token>'`.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir kapsayıcı oluşturmak için [AzCopy Make](storage-ref-azcopy-make.md) komutunu kullanabilirsiniz. Bu bölümdeki örneklerde adlı bir kapsayıcı oluşturulur `mycontainer` .

|    |     |
|--------|-----------|
| **Syntax** | `azcopy make 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>'` |
| **Örnek** | `azcopy make 'https://mystorageaccount.blob.core.windows.net/mycontainer'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy make 'https://mystorageaccount.dfs.core.windows.net/mycontainer'` |

Ayrıntılı başvuru belgeleri için bkz. [AzCopy Make](storage-ref-azcopy-make.md).

## <a name="upload-files"></a>Dosyaları karşıya yükleme

Yerel bilgisayarınızdan dosya ve Dizin yüklemek için [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanabilirsiniz.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosyayı karşıya yükleme
> * Bir dizini karşıya yükle
> * Bir dizinin içeriğini karşıya yükleme 
> * Belirli dosyaları karşıya yükle

> [!TIP]
> İsteğe bağlı bayraklar kullanarak karşıya yükleme işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyaları ekleme Blobları veya sayfa Blobları olarak karşıya yükleyin.|**--BLOB-tür** = \[ BlockBlob \| pageblob \| appendblob\]|
> |Belirli bir erişim katmanına (Arşiv katmanı gibi) yükleyin.|**--Block-blob-Tier** = \[ Hiçbiri \| sık erişimli olmayan \| \| Arşiv\]|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-copy.md#options).

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-file-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>'` |
| **Örnek** | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt'` |

Dosya yolu veya dosya adında herhangi bir yerde joker karakter sembolünü (*) kullanarak da bir dosyayı karşıya yükleyebilirsiniz. Örneğin: `'C:\myDirectory\*.txt'` , veya `C:\my*\*.txt` .

### <a name="upload-a-directory"></a>Bir dizini karşıya yükle

Bu örnekte bir dizin (ve bu dizindeki tüm dosyalar) bir blob kapsayıcısına kopyalanır. Sonuç, kapsayıcıda aynı ada sahip bir dizindir.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --recursive` |

Kapsayıcı içindeki bir dizine kopyalamak için, komut dizinizdeki bu dizinin adını belirtmeniz yeterlidir.

|    |     |
|--------|-----------|
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' --recursive` |

Kapsayıcıda varolmayan bir dizinin adını belirtirseniz AzCopy, bu adla yeni bir dizin oluşturur.

### <a name="upload-the-contents-of-a-directory"></a>Bir dizinin içeriğini karşıya yükleme

Joker karakter sembolünü (*) kullanarak, içeren dizinin kendisini kopyalamadan bir dizinin içeriğini karşıya yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>\*' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>'` |
| **Örnek** | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory'` |

> [!NOTE]
> `--recursive`Tüm alt dizinlerde dosyaları karşıya yüklemek için bayrağı ekleyin.

### <a name="upload-specific-files"></a>Belirli dosyaları karşıya yükle

Tüm dosya adlarını belirtebilir veya joker karakterler (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok dosya adı belirtin

Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--include-path` . Tek bir dosya adını noktalı virgül () kullanarak ayırın `;` .

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-path <semicolon-separated-file-list>` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |

Bu örnekte, AzCopy `C:\myDirectory\photos` dizini ve `C:\myDirectory\documents\myFile.txt` dosyayı aktarır. `--recursive`Dizindeki tüm dosyaları aktarma seçeneğini dahil etmeniz gerekir `C:\myDirectory\photos` .

Ayrıca, seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz `--exclude-path` . Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

#### <a name="use-wildcard-characters"></a>Joker karakter kullan

Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--include-pattern` . Joker karakterleri içeren kısmi adlar belirtin. Bir semicolin () kullanarak adları ayırın `;` . 

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |

Ayrıca, seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz `--exclude-pattern` . Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

`--include-pattern`Ve `--exclude-pattern` seçenekleri, yalnızca dosya adları için geçerlidir, yola değildir.  Bir dizin ağacında var olan tüm metin dosyalarını kopyalamak istiyorsanız, tüm `–recursive` dizin ağacını almak için seçeneğini kullanın ve ardından öğesini kullanarak `–include-pattern` `*.txt` tüm metin dosyalarını alın.

## <a name="download-files"></a>Dosyaları indirme

Blob, dizin ve kapsayıcıları yerel bilgisayarınıza indirmek için [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanabilirsiniz.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosya indirme
> * Bir dizin indirin
> * Bir dizinin içeriğini indirin
> * Belirli dosyaları indir

> [!TIP]
> İsteğe bağlı bayraklar kullanarak indirme işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyaları otomatik olarak aç.|**--sıkıştırmayı aç**|
> |Kopyalama ile ilgili günlük girdilerinizi ne kadar ayrıntılı olarak istediğinizi belirtin.|**--günlük düzeyi** = \[ Uyarı \| hatası \| bilgisi \| yok\]|
> |Hedefte çakışan dosyaların ve Blobların üzerine yazılıp yazılmayacağını belirtin.|**--üzerine yaz** = \[ doğru \| yanlış \| ıfsourcenewer \| istemi\]|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-copy.md#options).

> [!NOTE]
> `Content-md5`Bir Blobun Özellik değeri bir karma içeriyorsa, AzCopy indirilen veriler için BIR MD5 karması hesaplar ve BLOB özelliğinde depolanan MD5 karmasının `Content-md5` hesaplanan karmayla eşleştiğini doğrular. Bu değerler eşleşmezse, `--check-md5=NoCheck` Copy komutuna veya öğesini ekleyerek bu davranışı geçersiz kılmadığınız müddetçe indirme işlemi başarısız olur `--check-md5=LogOnly` .

### <a name="download-a-file"></a>Dosya indirme

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |

### <a name="download-a-directory"></a>Bir dizin indirin

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>' '<local-directory-path>' --recursive` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |

Bu örnek `C:\myDirectory\myBlobDirectory` , indirilen tüm dosyaları içeren adlı dizine neden olur.

### <a name="download-the-contents-of-a-directory"></a>Bir dizinin içeriğini indirin

Joker karakter sembolünü (*) kullanarak, bir dizinin içeriğini içeren dizini kopyalamadan indirebilirsiniz.

> [!NOTE]
> Şu anda, bu senaryo yalnızca hiyerarşik bir ad alanına sahip olmayan hesaplar için desteklenir.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.blob.core.windows.net/<container-name>/*' '<local-directory-path>/'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*' 'C:\myDirectory'` |

> [!NOTE]
> `--recursive`Dosyaları tüm alt dizinlerde indirmek için bayrağı ekleyin.

### <a name="download-specific-files"></a>Belirli dosyaları indir

Tüm dosya adlarını belirtebilir veya joker karakterler (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok dosya adı belirtin

Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--include-path` . Tek dosya adlarını bir semicolin () kullanarak ayırın `;` .

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt'--recursive` |

Bu örnekte, AzCopy `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` dizini ve `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/documents/myFile.txt` dosyayı aktarır. `--recursive`Dizindeki tüm dosyaları aktarma seçeneğini dahil etmeniz gerekir `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` .

Ayrıca, seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz `--exclude-path` . Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

#### <a name="use-wildcard-characters"></a>Joker karakter kullan

Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--include-pattern` . Joker karakterleri içeren kısmi adlar belirtin. Bir semicolin () kullanarak adları ayırın `;` .

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |

Ayrıca, seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz `--exclude-pattern` . Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

`--include-pattern`Ve `--exclude-pattern` seçenekleri, yalnızca dosya adları için geçerlidir, yola değildir.  Bir dizin ağacında var olan tüm metin dosyalarını kopyalamak istiyorsanız, tüm `–recursive` dizin ağacını almak için seçeneğini kullanın ve ardından öğesini kullanarak `–include-pattern` `*.txt` tüm metin dosyalarını alın.

## <a name="copy-blobs-between-storage-accounts"></a>Blobu depolama hesapları arasında kopyalama

Diğer depolama hesaplarına Blobları kopyalamak için AzCopy kullanabilirsiniz. Kopyalama işlemi, komutun döndürdüğü zaman zaman uyumludur, yani tüm dosyalar kopyalanmış olduğunu gösterir. 

AzCopy, [sunucudan sunucuya](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) [API 'ler](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)kullanır, bu nedenle veriler doğrudan depolama sunucuları arasında kopyalanır. Bu kopyalama işlemleri bilgisayarınızın ağ bant genişliğini kullanmaz. Ortam değişkeninin değerini ayarlayarak bu işlemlerin verimini artırabilirsiniz `AZCOPY_CONCURRENCY_VALUE` . Daha fazla bilgi için bkz. [aktarım hızını iyileştirme](storage-use-azcopy-configure.md#optimize-throughput).

> [!NOTE]
> Bu senaryoda, geçerli sürümde aşağıdaki sınırlamalar bulunur.
>
> - Her kaynak URL 'ye bir SAS belirteci eklemeniz gerekir. Azure Active Directory (AD) kullanarak yetkilendirme kimlik bilgilerini sağlarsanız, SAS belirtecini yalnızca hedef URL 'den atlayabilirsiniz. Hedef hesabınızda uygun rolleri ayarladığınızdan emin olun. Bkz. [1. seçenek: Use Azure Active Directory](storage-use-azcopy-v10.md?toc=/azure/storage/blobs/toc.json#option-1-use-azure-active-directory).
>-  Premium Blok Blob depolama hesapları, erişim katmanlarını desteklemez. ' İ ayarlayarak kopyalama işleminden bir Blobun erişim katmanını atlayın `s2s-preserve-access-tier` `false` (örneğin: `--s2s-preserve-access-tier=false` ).

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Bir blobu başka bir depolama hesabına kopyalama
> * Bir dizini başka bir depolama hesabına kopyalama
> * Kapsayıcıyı başka bir depolama hesabına kopyalama
> * Tüm kapsayıcıları, dizinleri ve dosyaları başka bir depolama hesabına Kopyala

Bu örnekler, hiyerarşik bir ad alanı olan hesaplarla de çalışır. [Data Lake Storage çoklu protokol erişimi](../blobs/data-lake-storage-multi-protocol-access.md) , bu HESAPLARDA aynı URL sözdizimini () kullanmanıza olanak sağlar `blob.core.windows.net` .

> [!TIP]
> İsteğe bağlı bayraklar kullanarak kopyalama işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyaları ekleme Blobları veya sayfa Blobları olarak kopyalayın.|**--BLOB-tür** = \[ BlockBlob \| pageblob \| appendblob\]|
> |Belirli bir erişim katmanına (Arşiv katmanı gibi) kopyalayın.|**--Block-blob-Tier** = \[ Hiçbiri \| sık erişimli olmayan \| \| Arşiv\]|
> |Dosyaları otomatik olarak aç.|**--sıkıştırmayı aç** = \[ gzip \| söndür\]|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-copy.md#options).

### <a name="copy-a-blob-to-another-storage-account"></a>Bir blobu başka bir depolama hesabına kopyalama

`blob.core.windows.net`Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini () kullanın.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>'` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |

### <a name="copy-a-directory-to-another-storage-account"></a>Bir dizini başka bir depolama hesabına kopyalama

`blob.core.windows.net`Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini () kullanın.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-a-container-to-another-storage-account"></a>Kapsayıcıyı başka bir depolama hesabına kopyalama

`blob.core.windows.net`Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini () kullanın.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-all-containers-directories-and-blobs-to-another-storage-account"></a>Tüm kapsayıcıları, dizinleri ve Blobları başka bir depolama hesabına Kopyala

`blob.core.windows.net`Hiyerarşik bir ad alanı olan hesaplar için aynı URL söz dizimini () kullanın.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |

## <a name="synchronize-files"></a>Dosyaları eşitler

Bir yerel dosya sisteminin içeriğini bir blob kapsayıcısı ile eşzamanlı hale getirebilirsiniz. Kapsayıcıları ve sanal dizinleri birbiriyle de eşzamanlı hale getirebilirsiniz. Eşitleme tek yönlü. Diğer bir deyişle, bu iki uç noktanın hangisinin kaynak olduğunu ve hedefin nerede olduğunu seçersiniz. Eşitleme, sunucuyu sunucu API 'Leri için de kullanır. Bu bölümde sunulan örnekler, hiyerarşik bir ad alanı olan hesaplarla de çalışır. 

> [!NOTE]
> AzCopy 'in geçerli sürümü diğer kaynak ve hedefler arasında eşitlenmez (örneğin: dosya depolama veya Amazon Web Services (AWS) S3 demetleri).

[Eşitleme](storage-ref-azcopy-sync.md) komutu dosya adlarını ve son değiştirilme zaman damgalarını karşılaştırır. `--delete-destination` `true` `prompt` Bu dosyalar artık kaynak dizinde yoksa, hedef dizindeki dosyaları silmek için veya değerine isteğe bağlı bayrağını ayarlayın.

`--delete-destination` `true` AzCopy bayrağını, bir istem sağlamadan dosyaları siler olarak ayarlarsanız. AzCopy bir dosyayı silmesinden önce bir istem görünmesini istiyorsanız, `--delete-destination` bayrağını olarak ayarlayın `prompt` .

> [!NOTE]
> Yanlışlıkla silinmeleri engellemek için, bayrağını kullanmadan önce [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) özelliğini etkinleştirdiğinizden emin olun `--delete-destination=prompt|true` .

> [!TIP]
> İsteğe bağlı bayraklar kullanarak eşitleme işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |İndirme sırasında ne kadar kesin MD5 karmalarının doğrulanması gerektiğini belirtin.|**--Check-MD5** = \[ NoCheck \| LogOnly \| failiffarklı \| failiffarklıentormissing yok\]|
> |Dosyaları bir düzene göre hariç tutun.|**--exclude-Path**|
> |Eşitleme ile ilgili günlük girdilerinizi ne kadar ayrıntılı olarak istediğinizi belirtin.|**--günlük düzeyi** = \[ Uyarı \| hatası \| bilgisi \| yok\]|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-sync.md#options).

### <a name="update-a-container-with-changes-to-a-local-file-system"></a>Yerel dosya sistemindeki değişikliklerle bir kapsayıcıyı güncelleştirme

Bu durumda, kapsayıcı hedef ve yerel dosya sistemi kaynağıdır. 

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync '<local-directory-path>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy sync 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-local-file-system-with-changes-to-a-container"></a>Bir kapsayıcıda değişikliklerle yerel bir dosya sistemini güncelleştirme

Bu durumda, yerel dosya sistemi hedefdir ve kapsayıcı kaynağıdır.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync 'https://<storage-account-name>.blob.core.windows.net/<container-name>' 'C:\myDirectory' --recursive` |
| **Örnek** | `azcopy sync 'https://mystorageaccount.blob.core.windows.net/mycontainer' 'C:\myDirectory' --recursive` |

### <a name="update-a-container-with-changes-in-another-container"></a>Başka bir kapsayıcıdaki değişikliklerle bir kapsayıcıyı güncelleştirme

Bu komutta görüntülenen ilk kapsayıcı kaynağıdır. İkincisi, hedefin bir biridir.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/mycontainer' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Başka bir dosya paylaşımındaki bir dizinde bulunan değişikliklerle Dizin güncelleştirme

Bu komutta görüntülenen ilk dizin kaynağıdır. İkincisi, hedefin bir biridir.

|    |     |
|--------|-----------|
| **Syntax** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive` |
| **Örnek** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/<container-name>/myDirectory' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myDirectory' --recursive` |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerden herhangi birinde daha fazla örnek bulabilirsiniz:

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)

- [Öğretici: AzCopy kullanarak şirket içi verileri bulut depolamasına taşıma](storage-use-azcopy-migrate-on-premises-data.md)

- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

- [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)
