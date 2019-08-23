---
title: AzCopy ile v10 arasındaki kullanarak Azure Blob depolama alanına veya buradan veri aktarma | Microsoft Docs
description: Bu makale, kapsayıcılar oluşturmanıza, dosyaları kopyalamaya ve yerel dosya sistemleri ile kapsayıcılar arasında dizin eşitlemenize yardımcı olan AzCopy örnek komutlarının bir koleksiyonunu içerir.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: a95e643d6561f425484a21b5032b1df585d0460b
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69900336"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>AzCopy ve BLOB Storage ile veri aktarma

AzCopy, depolama hesaplarına veri kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, blob depolamayla çalışan örnek komutlar içerir.

## <a name="get-started"></a>başlarken

AzCopy ['i indirmek Için AzCopy ile çalışmaya başlama](storage-use-azcopy-v10.md) makalesini inceleyin ve depolama hizmetine yetkilendirme kimlik bilgilerini nasıl sağlayabileceğiniz yolları hakkında bilgi edinin.

> [!NOTE]
> Bu makaledeki örneklerde, `AzCopy login` komutunu kullanarak kimliğinizi doğruladığınızı varsayalım. AzCopy daha sonra blob depolamadaki verilere erişim yetkisi vermek için Azure AD hesabınızı kullanır.
>
> Blob verilerine erişim yetkisi vermek için bir SAS belirteci kullanıyorsanız, bu belirteci her AzCopy komutunda kaynak URL 'sine ekleyebilirsiniz.
>
> Örneğin: `https://<storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>"`

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir kapsayıcı oluşturmak için AzCopy `make` komutunu kullanabilirsiniz. Bu bölümdeki örneklerde adlı `mycontainer`bir kapsayıcı oluşturulur.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy make "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>` |
| **Örnek** | `azcopy make "https://mystorageaccount.blob.core.windows.net/mycontainer"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy make "https://mystorageaccount.dfs.core.windows.net/mycontainer"` |

## <a name="upload-files"></a>Dosyaları karşıya yükleme

Yerel bilgisayarınızdan dosya ve Dizin yüklemek `copy` için AzCopy komutunu kullanabilirsiniz.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Karşıya dosya yükle
> * Bir dizini karşıya yükle
> * Joker karakterler kullanarak karşıya dosya yükleme

> [!NOTE]
> AzCopy, dosyanın MD5 karma kodunu otomatik olarak hesaplamaz ve depolamaz. AzCopy 'in bunu yapmak istiyorsanız, her bir kopyala komutuna `--put-md5` bayrak ekleyin. Bu şekilde, blob indirildiğinde AzCopy, indirilen veriler için bir MD5 karması hesaplar ve BLOB `Content-md5` özelliğinde depolanan MD5 karmasının hesaplanan karmayla eşleştiğini doğrular.

### <a name="upload-a-file"></a>Karşıya dosya yükle

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-file-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>"` |
| **Örnek** | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory\myTextFile.txt" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt"` |

> [!NOTE]
> AzCopy varsayılan olarak blok bloblarına veri yükler. Dosyaları ekleme Blobları olarak yüklemek için veya sayfa Blobları bayrağını `--blob-type=[BlockBlob|PageBlob|AppendBlob]`kullanır.

### <a name="upload-a-directory"></a>Bir dizini karşıya yükle

Bu örnekte bir dizin (ve bu dizindeki tüm dosyalar) bir blob kapsayıcısına kopyalanır. Sonuç, kapsayıcıda aynı ada sahip bir dizindir.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-directory-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory" "https://mystorageaccount.dfs.core.windows.net/mycontainer" --recursive` |

Kapsayıcı içindeki bir dizine kopyalamak için, komut dizinizdeki bu dizinin adını belirtmeniz yeterlidir.

|    |     |
|--------|-----------|
| **Örnek** | `azcopy copy "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory" --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory" --recursive` |

Kapsayıcıda varolmayan bir dizinin adını belirtirseniz AzCopy, bu adla yeni bir dizin oluşturur.

### <a name="upload-the-contents-of-a-directory"></a>Bir dizinin içeriğini karşıya yükleme

Joker karakter sembolünü (*) kullanarak, içeren dizinin kendisini kopyalamadan bir dizinin içeriğini karşıya yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "<local-directory-path>\*" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>` |
| **Örnek** | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "C:\myDirectory\*" "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory"` |

> [!NOTE]
> Tüm alt dizinlerde dosyaları karşıya yüklemek için bayrağıekleyin.`--recursive`

## <a name="download-files"></a>Dosyaları indirme

Yerel bilgisayarınıza Bloblar, dizinler `copy` ve kapsayıcılar indirmek için AzCopy komutunu kullanabilirsiniz.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosya indir
> * Bir dizin indirin
> * Joker karakterler kullanarak dosyaları indirme

> [!NOTE]
> Bir Blobun `Content-md5`özellikdeğeri bir karma içeriyorsa, AzCopy indirilen veriler için bir MD5 karması hesaplar ve BLOB özelliğinde depolanan MD5 karmasının hesaplanan karmayla eşleştiğini doğrular. `Content-md5` Bu değerler eşleşmezse, Copy komutuna veya `--check-md5=NoCheck` `--check-md5=LogOnly` öğesini ekleyerek bu davranışı geçersiz kılmadığınız müddetçe indirme işlemi başarısız olur.

### <a name="download-a-file"></a>Dosya indir

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>" "<local-file-path>"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt" "C:\myDirectory\myTextFile.txt"` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt" "C:\myDirectory\myTextFile.txt"` |

### <a name="download-a-directory"></a>Bir dizin indirin

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>" "<local-directory-path>" --recursive` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory "C:\myDirectory"  --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy "https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory "C:\myDirectory"  --recursive` |

Bu örnek, indirilen tüm dosyaları içeren `C:\myDirectory\myBlobDirectory` adlı dizine neden olur.

### <a name="download-the-contents-of-a-directory"></a>Bir dizinin içeriğini indirin

Joker karakter sembolünü (*) kullanarak, bir dizinin içeriğini içeren dizini kopyalamadan indirebilirsiniz.

> [!NOTE]
> Şu anda, bu senaryo yalnızca hiyerarşik bir ad alanına sahip olmayan hesaplar için desteklenir.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<storage-account-name>.blob.core.windows.net/<container-name>/*" "<local-directory-path>/"` |
| **Örnek** | `azcopy copy "https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*" "C:\myDirectory"` |

> [!NOTE]
> Dosyaları tüm alt dizinlerde indirmek için bayrağıekleyin.`--recursive`

## <a name="copy-blobs-between-storage-accounts"></a>Depolama hesapları arasında blobları kopyalama

Diğer depolama hesaplarına Blobları kopyalamak için AzCopy kullanabilirsiniz. Kopyalama işlemi, komutun döndürdüğü zaman zaman uyumludur, yani tüm dosyalar kopyalanmış olduğunu gösterir.

AzCopy, [sunucudan sunucuya](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) [API 'ler](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)kullanır, bu nedenle veriler doğrudan depolama sunucuları arasında kopyalanır. Bu kopyalama işlemleri bilgisayarınızın ağ bant genişliğini kullanmaz. `AZCOPY_CONCURRENCY_VALUE` Ortam değişkeninin değerini ayarlayarak bu işlemlerin verimini artırabilirsiniz. Daha fazla bilgi için bkz. [aktarım hızını iyileştirme](storage-use-azcopy-configure.md#optimize-throughput).

> [!NOTE]
> Bu senaryoda, geçerli sürümde aşağıdaki sınırlamalar bulunur.
>
> - Yalnızca hiyerarşik bir ad alanı olmayan hesaplar desteklenir.
> - Her kaynak URL 'ye bir SAS belirteci eklemeniz gerekir. Azure Active Directory (AD) kullanarak yetkilendirme kimlik bilgilerini sağlarsanız, SAS belirtecini yalnızca hedef URL 'den atlayabilirsiniz.
>-  Premium Blok Blob depolama hesapları, erişim katmanlarını desteklemez. ' İ ayarlayarak `s2s-preserve-access-tier` `false` kopyalama işleminden bir Blobun erişim katmanını atlayın (örneğin: `--s2s-preserve-access-tier=false`).

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Bir blobu başka bir depolama hesabına kopyalama
> * Bir dizini başka bir depolama hesabına kopyalama
> * Kapsayıcıları başka bir depolama hesabına kopyalama
> * Tüm kapsayıcıları, dizinleri ve dosyaları başka bir depolama hesabına Kopyala

### <a name="copy-a-blob-to-another-storage-account"></a>Bir blobu başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>"` |
| **Örnek** | `azcopy copy "https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt"` |

### <a name="copy-a-directory-to-another-storage-account"></a>Bir dizini başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy copy "https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-a-containers-to-another-storage-account"></a>Kapsayıcıları başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<source-storage-account-name>.blob.core.windows.net/<container-name>?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy copy "https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="copy-all-containers-directories-and-blobs-to-another-storage-account"></a>Tüm kapsayıcıları, dizinleri ve Blobları başka bir depolama hesabına Kopyala

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy copy "https://<source-storage-account-name>.blob.core.windows.net/?<SAS-token>" "https://<destination-storage-account-name>.blob.core.windows.net/" --recursive"` |
| **Örnek** | `azcopy copy "https://mysourceaccount.blob.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D" "https://mydestinationaccount.blob.core.windows.net" --recursive` |

## <a name="synchronize-files"></a>Dosyaları eşitler

Bir yerel dosya sisteminin içeriğini bir blob kapsayıcısı ile eşzamanlı hale getirebilirsiniz. Eşitleme tek yönlü. Diğer bir deyişle, bu iki uç noktanın hangisinin kaynak olduğunu ve hedefin nerede olduğunu seçersiniz. Eşitleme, sunucuyu sunucu API 'Leri için de kullanır.

> [!NOTE]
> Şu anda, bu senaryo yalnızca hiyerarşik bir ad alanına sahip olmayan hesaplar için desteklenir. AzCopy 'in geçerli sürümü diğer kaynaklar ve hedefler arasında eşitlenmez (örneğin: Dosya depolama veya Amazon Web Services (AWS) S3 demetleri).

Komut `sync` , dosya adlarını ve son değiştirilme zaman damgalarını karşılaştırır. Bu dosyalar artık kaynak dizinde yoksa, hedef `true` dizindeki `prompt` dosyaları silmek için veya değerine isteğebağlıbayrağınıayarlayın.`--delete-destination`

AzCopy bayrağını, `--delete-destination` bir istem `true` sağlamadan dosyaları siler olarak ayarlarsanız. AzCopy bir dosyayı silmesinden önce bir istem görünmesini istiyorsanız, `--delete-destination` bayrağını olarak `prompt`ayarlayın.

> [!NOTE]
> Yanlışlıkla silinmeleri engellemek için, `--delete-destination=prompt|true` bayrağını kullanmadan önce [geçici silme](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) özelliğini etkinleştirdiğinizden emin olun.

### <a name="update-a-container-with-changes-to-a-local-file-system"></a>Yerel dosya sistemindeki değişikliklerle bir kapsayıcıyı güncelleştirme

Bu durumda, kapsayıcı hedef ve yerel dosya sistemi kaynağıdır.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy sync "<local-directory-path>" "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" --recursive` |
| **Örnek** | `azcopy sync "C:\myDirectory" "https://mystorageaccount.blob.core.windows.net/mycontainer" --recursive` |

### <a name="update-a-local-file-system-with-changes-to-a-container"></a>Bir kapsayıcıda değişikliklerle yerel bir dosya sistemini güncelleştirme

Bu durumda, yerel dosya sistemi hedefdir ve kapsayıcı kaynağıdır.

|    |     |
|--------|-----------|
| **Söz dizimi** | `azcopy sync "https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>" "C:\myDirectory" --recursive` |
| **Örnek** | `azcopy sync "https://mystorageaccount.blob.core.windows.net/mycontainer" "C:\myDirectory" --recursive` |
|

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerden herhangi birinde daha fazla örnek bulabilirsiniz:

- [AzCopy ile çalışmaya başlama](storage-use-azcopy-v10.md)

- [Öğretici: AzCopy kullanarak şirket içi verileri bulut depolamaya geçirme](storage-use-azcopy-migrate-on-premises-data.md)

- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)

- [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)
