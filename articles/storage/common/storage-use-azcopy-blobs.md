---
title: AzCopy v10 kullanarak Azure Blob depolama alanına veya Azure Blob depolamadan veri aktarımı | Microsoft Dokümanlar
description: Bu makalede, kapsayıcılar oluşturmanıza, dosyaları kopyalamanıza ve yerel dosya sistemleri ve kapsayıcılar arasında dizinleri eşitlemenize yardımcı olan AzCopy örnek komutları koleksiyonu bulunur.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 73685f124f93bb541f33b3b70727d90ce22b3cdd
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263446"
---
# <a name="transfer-data-with-azcopy-and-blob-storage"></a>AzCopy ve Blob depolama ile veri aktarımı

AzCopy, depolama hesaplarına, depo hesaplarına veya bunlar arasında verileri kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makalede, Blob depolama ile çalışan örnek komutları içerir.

> [!TIP]
> Bu makaledeki örnekler, yol bağımsız değişkenlerini tek tırnak ('') ile kaplar. Windows Komut Uyruşu (cmd.exe) hariç tüm komut kabuklarında tek tırnak işareti kullanın. Windows Komut Uyruşu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak ('') yerine çift tırnak ("") içeren bir şekilde içine çekin.

## <a name="get-started"></a>Kullanmaya başlayın

AzCopy'yi indirmek ve depolama hizmetine yetki kimlik bilgilerini sağlama yolları hakkında bilgi edinmek için [AzCopy makalesine başlayın.](storage-use-azcopy-v10.md)

> [!NOTE]
> Bu makaledeki örnekler, komutu kullanarak kimliğinizi doğruladığınızı `AzCopy login` varsayar. AzCopy daha sonra Blob depolamasundaki verilere erişimi yetkilendirmek için Azure AD hesabınızı kullanır.
>
> Blob verilerine erişimi yetkilendirmek için bir SAS belirteci kullanmak isterseniz, bu belirteci her AzCopy komutundaki kaynak URL'ye ekleyebilirsiniz.
>
> Örneğin: `'https://<storage-account-name>.blob.core.windows.net/<container-name><SAS-token>'`.

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir kapsayıcı oluşturmak için [azcopy make](storage-ref-azcopy-make.md) komutunu kullanabilirsiniz. Bu bölümdeki örneklerde `mycontainer`.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy make 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>'` |
| **Örnek** | `azcopy make 'https://mystorageaccount.blob.core.windows.net/mycontainer'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy make 'https://mystorageaccount.dfs.core.windows.net/mycontainer'` |

Ayrıntılı başvuru dokümanları için [azcopy make](storage-ref-azcopy-make.md)adresine bakın.

## <a name="upload-files"></a>Dosyaları karşıya yükleme

Yerel bilgisayarınızdan dosya ve dizin yüklemek için [azcopy copy komutunu](storage-ref-azcopy-copy.md) kullanabilirsiniz.

Bu bölümde aşağıdaki örnekler yer almaktadır:

> [!div class="checklist"]
> * Dosyayı karşıya yükleme
> * Dizin yükleme
> * Dizinin içeriğini yükleme 
> * Belirli dosyaları yükleme

> [!TIP]
> İsteğe bağlı bayraklar kullanarak yükleme işleminde değişiklik yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyaları Append Blobs veya Page Blobs olarak yükleyin.|**--blob tipi**=\[BlockBlob PageBlob\|\|AppendBlob\]|
> |Belirli bir erişim katmanına (arşiv katmanı gibi) yükleyin.|**--block-blob-tier**=\[\|None\|\|Hot Cool Arşiv\]|
> |Dosyaları otomatik olarak dekomprese edin.|**--dekompresör**=\[gzip\|deflate\]|
> 
> Tam liste için [seçeneklere](storage-ref-azcopy-copy.md#options)bakın.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-file-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-name>'` |
| **Örnek** | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt'` |

Dosya yolunun veya dosya adının herhangi bir yerine joker karakter simgesi (*) kullanarak dosya yükleyebilirsiniz. Örneğin: `'C:\myDirectory\*.txt'`, `C:\my*\*.txt`veya .

### <a name="upload-a-directory"></a>Dizin yükleme

Bu örnek, bir dizini (ve bu dizindeki tüm dosyaları) bir blob kapsayıcısına kopyalar. Sonuç, kapsayıcıda aynı ada göre bir dizindir.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --recursive` |

Kapsayıcı içindeki bir dizine kopyalamak için komut dizenizde bu dizinin adını belirtmeniz gerekir.

|    |     |
|--------|-----------|
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' --recursive` |

Kapsayıcıda bulunmayan bir dizinin adını belirtirseniz, AzCopy bu ada göre yeni bir dizin oluşturur.

### <a name="upload-the-contents-of-a-directory"></a>Dizinin içeriğini yükleme

Joker karakter simgesini (*) kullanarak, içeren dizinin kendisini kopyalamadan bir dizinin içeriğini yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>\*' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>'` |
| **Örnek** | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory'` |

> [!NOTE]
> Tüm alt `--recursive` dizinlere dosya yüklemek için bayrağı ekleyin.

### <a name="upload-specific-files"></a>Belirli dosyaları yükleme

Tam dosya adları belirtebilir veya joker karakter (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok tam dosya adı belirtin

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-path` kopyalama komutunu kullanın. Bir semicolon kullanarak ayrı ayrı`;`ayrı dosya adları ( ).

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-path <semicolon-separated-file-list>` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-path 'photos;documents\myFile.txt' --recursive` |

Bu örnekte, AzCopy `C:\myDirectory\photos` dizini `C:\myDirectory\documents\myFile.txt` ve dosyayı aktarMaktadır. Tüm dosyaları `--recursive` `C:\myDirectory\photos` dizine aktarma seçeneğini eklemeniz gerekir.

`--exclude-path` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

#### <a name="use-wildcard-characters"></a>Joker karakter kullanma

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-pattern` kopyalama komutunu kullanın. Joker karakter içeren kısmi adlar belirtin. Bir semicolin kullanarak ayrı`;`adlar ( ). 

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.dfs.core.windows.net/mycontainer' --include-pattern 'myFile*.txt;*.pdf*'` |

`--exclude-pattern` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

Ve `--include-pattern` `--exclude-pattern` seçenekleri yalnızca dosya adları için değil, yol için geçerlidir.  Dizin ağacında bulunan tüm metin dosyalarını kopyalamak istiyorsanız, `–recursive` tüm dizin ağacını almak için `–include-pattern` seçeneği `*.txt` kullanın ve ardından tüm metin dosyalarını almak için ve belirtin.

## <a name="download-files"></a>Dosyaları indirme

Lekeleri, dizinleri ve kapsayıcıları yerel bilgisayarınıza indirmek için [azcopy copy copy](storage-ref-azcopy-copy.md) komutunu kullanabilirsiniz.

Bu bölümde aşağıdaki örnekler yer almaktadır:

> [!div class="checklist"]
> * Dosya indirme
> * Bir dizin indirin
> * Dizinin içeriğini indirin
> * Belirli dosyaları karşıdan yükleme

> [!TIP]
> İsteğe bağlı bayraklar kullanarak indirme işlemini ayarlayabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyaları otomatik olarak dekomprese edin.|**--dekompresör**=\[gzip\|deflate\]|
> |Kopyayla ilgili günlük girişlerinizin ne kadar ayrıntılı olmasını istediğinizi belirtin.|**--günlük seviyesi**=\[\|UYARI HATASI\|BİlGİlerİ\|YOK\]|
> |Hedefteki çakışan dosyaların ve lekelerin üzerine yazıp yazılmayı ve nasıl yazarak yazacaklarını belirtin.|**--overwrite**=\[\|true\|false ifSourceNewer\|istemi\]|
> 
> Tam liste için [seçeneklere](storage-ref-azcopy-copy.md#options)bakın.

> [!NOTE]
> Bir `Content-md5` blob'un özellik değeri karma içeriyorsa, AzCopy indirilen veriler için bir MD5 karmasını hesaplar ve blob'un `Content-md5` özelliğinde depolanan MD5 karmasının hesaplanan karmayla eşleştiğini doğrular. Bu değerler eşleşmiyorsa, bu davranışı ekleyerek `--check-md5=NoCheck` veya `--check-md5=LogOnly` kopyalama komutuna ekleyerek geçersiz kılmadığınız sürece karşıdan yükleme başarısız olur.

### <a name="download-a-file"></a>Dosya indirme

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |

### <a name="download-a-directory"></a>Bir dizin indirin

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>' '<local-directory-path>' --recursive` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |

Bu örnek, indirilen tüm `C:\myDirectory\myBlobDirectory` dosyaları içeren adlı bir dizini ile sonuçlanır.

### <a name="download-the-contents-of-a-directory"></a>Dizinin içeriğini indirin

Joker karakter simgesini (*) kullanarak, içeren dizinin kendisini kopyalamadan bir dizinin içeriğini indirebilirsiniz.

> [!NOTE]
> Şu anda, bu senaryo yalnızca hiyerarşik ad alanı olmayan hesaplar için desteklenir.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.blob.core.windows.net/<container-name>/*' '<local-directory-path>/'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*' 'C:\myDirectory'` |

> [!NOTE]
> Tüm alt `--recursive` dizinlerde dosyaları indirmek için bayrağı ekleyin.

### <a name="download-specific-files"></a>Belirli dosyaları karşıdan yükleme

Tam dosya adları belirtebilir veya joker karakter (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok tam dosya adı belirtin

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-path` kopyalama komutunu kullanın. Bir semicolin kullanarak ayrı ayrı`;`ayrı dosya adları ( ).

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt'--recursive` |

Bu örnekte, AzCopy `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` dizini `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/documents/myFile.txt` ve dosyayı aktarMaktadır. Tüm dosyaları `--recursive` `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` dizine aktarma seçeneğini eklemeniz gerekir.

`--exclude-path` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

#### <a name="use-wildcard-characters"></a>Joker karakter kullanma

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-pattern` kopyalama komutunu kullanın. Joker karakter içeren kısmi adlar belirtin. Bir semicolin kullanarak ayrı`;`adlar ( ).

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |

`--exclude-pattern` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

Ve `--include-pattern` `--exclude-pattern` seçenekleri yalnızca dosya adları için değil, yol için geçerlidir.  Dizin ağacında bulunan tüm metin dosyalarını kopyalamak istiyorsanız, `–recursive` tüm dizin ağacını almak için `–include-pattern` seçeneği `*.txt` kullanın ve ardından tüm metin dosyalarını almak için ve belirtin.

## <a name="copy-blobs-between-storage-accounts"></a>Blobu depolama hesapları arasında kopyalama

Lekeleri diğer depolama hesaplarına kopyalamak için AzCopy'yi kullanabilirsiniz. Kopyalama işlemi eşzamanlıdır, bu nedenle komut döndüğünde tüm dosyaların kopyalandığını gösterir. 

AzCopy [sunucudan sunucuya](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) [API'ler](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)kullanır, böylece veriler doğrudan depolama sunucuları arasında kopyalanır. Bu kopyalama işlemleri bilgisayarınızın ağ bant genişliğini kullanmaz. `AZCOPY_CONCURRENCY_VALUE` Ortam değişkeninin değerini ayarlayarak bu işlemlerin iş kısmını artırabilirsiniz. Daha fazla bilgi için, [iş akışını optimize etme](storage-use-azcopy-configure.md#optimize-throughput)'ye bakın.

> [!NOTE]
> Bu senaryo, geçerli sürümde aşağıdaki sınırlamalar vardır.
>
> - Her kaynak URL'ye bir SAS belirteci ekleniz gerekir. Azure Active Directory (AD) kullanarak yetkilendirme kimlik bilgileri sağlarsanız, SAS belirtecinizi yalnızca hedef URL'den atlayabilirsiniz.
>-  Premium blok blob depolama hesapları erişim katmanlarını desteklemez. Bir blob'un erişim katmanını kopyalama işleminden `s2s-preserve-access-tier` ayarlayarak `false` atlayın `--s2s-preserve-access-tier=false`(Örneğin: ).

Bu bölümde aşağıdaki örnekler yer almaktadır:

> [!div class="checklist"]
> * Blob'u başka bir depolama hesabına kopyalama
> * Dizini başka bir depolama hesabına kopyalama
> * Kapsayıcıyı başka bir depolama hesabına kopyalama
> * Tüm kapsayıcıları, dizinleri ve dosyaları başka bir depolama hesabına kopyalama

Bu örnekler, hiyerarşik ad alanı olan hesaplarla da çalışır. [Veri Gölü Depolama'daki çoklu protokol erişimi,](../blobs/data-lake-storage-multi-protocol-access.md) bu hesaplarda`blob.core.windows.net`aynı URL sözdizimini () kullanmanıza olanak tanır.

> [!TIP]
> İsteğe bağlı bayraklar kullanarak kopyalama işleminizde değişiklik yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyaları Apend Blobs veya Page Blobs olarak kopyalayın.|**--blob tipi**=\[BlockBlob PageBlob\|\|AppendBlob\]|
> |Belirli bir erişim katmanına (arşiv katmanı gibi) kopyalayın.|**--block-blob-tier**=\[\|None\|\|Hot Cool Arşiv\]|
> |Dosyaları otomatik olarak dekomprese edin.|**--dekompresör**=\[gzip\|deflate\]|
> 
> Tam liste için [seçeneklere](storage-ref-azcopy-copy.md#options)bakın.

### <a name="copy-a-blob-to-another-storage-account"></a>Blob'u başka bir depolama hesabına kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<blob-path>'` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myTextFile.txt'` |

### <a name="copy-a-directory-to-another-storage-account"></a>Dizini başka bir depolama hesabına kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-path><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-a-container-to-another-storage-account"></a>Kapsayıcıyı başka bir depolama hesabına kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<container-name><SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="copy-all-containers-directories-and-blobs-to-another-storage-account"></a>Tüm kapsayıcıları, dizinleri ve lekeleri başka bir depolama hesabına kopyalama

Hiyerarşik ad alanına`blob.core.windows.net`sahip hesaplar için aynı URL sözdizimini ( ) kullanın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.blob.core.windows.net/<SAS-token>' 'https://<destination-storage-account-name>.blob.core.windows.net/' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.blob.core.windows.net/?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |
| **Örnek** (hiyerarşik ad alanı)| `azcopy copy 'https://mysourceaccount.blob.core.windows.net/?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.blob.core.windows.net' --recursive` |

## <a name="synchronize-files"></a>Dosyaları eşitle

Yerel bir dosya sisteminin içeriğini bir blob kapsayıcısıyla eşitleyebilirsiniz. Kapsayıcıları ve sanal dizinleri de birbirleriyle eşitleyebilirsiniz. Eşitleme tek yönlüdür. Başka bir deyişle, bu iki uç noktadan hangisinin kaynak, hangisinin hedef olduğunu seçersiniz. Eşitleme de sunucu API'ler için sunucu kullanır. Bu bölümde sunulan örnekler, hiyerarşik ad alanına sahip hesaplarla da çalışır. 

> [!NOTE]
> AzCopy'nin geçerli sürümü diğer kaynaklar ve hedefler arasında eşitleme yapmaz (Örneğin: Dosya depolama veya Amazon Web Services (AWS) S3 kovaları).

[Eşitleme](storage-ref-azcopy-sync.md) komutu dosya adlarını ve son değiştirilen zaman damgalarını karşılaştırır. İsteğe bağlı bayrağı, `true` kaynak `prompt` dizinde artık yoksa hedef dizinindeki dosyaların değerine veya silinmeye ayarlayın. `--delete-destination`

`--delete-destination` Bayrağı AzCopy olarak `true` ayarlarsanız, bir istem sağlamadan dosyaları siler. AzCopy bir dosyayı silmeden önce bir istem `--delete-destination` görünmesini `prompt`istiyorsanız, bayrağı ' ya ayarlayın.

> [!NOTE]
> Yanlışlıkla silmeleri önlemek için, bayrağı kullanmadan önce [yumuşak](https://docs.microsoft.com/azure/storage/blobs/storage-blob-soft-delete) `--delete-destination=prompt|true` silme özelliğini etkinleştirdiğinizden emin olun.

> [!TIP]
> İsteğe bağlı bayraklar kullanarak eşitleme işlemini değiştirebilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |İndirme de MD5'in ne kadar kesin olarak doğrulanması gerektiğini belirtin.|**--check-md5**=\[NoCheck\|\|LogOnly\|FailIfDifferent FailIfDifferentOrMissing\]|
> |Bir desene dayalı dosyaları hariç tut.|**--dışlama-yol**|
> |Eşitleme yle ilgili günlük girişlerinizin ne kadar ayrıntılı olmasını istediğinizi belirtin.|**--günlük seviyesi**=\[\|UYARI HATASI\|BİlGİlerİ\|YOK\]|
> 
> Tam liste için [seçeneklere](storage-ref-azcopy-sync.md#options)bakın.

### <a name="update-a-container-with-changes-to-a-local-file-system"></a>Yerel dosya sisteminde değişiklik olan bir kapsayıcıyı güncelleştirme

Bu durumda, kapsayıcı hedef ve yerel dosya sistemi kaynaktır. 

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync '<local-directory-path>' 'https://<storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy sync 'C:\myDirectory' 'https://mystorageaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-local-file-system-with-changes-to-a-container"></a>Yerel bir dosya sistemini kapsayıcıdaki değişikliklerle güncelleştirme

Bu durumda, yerel dosya sistemi hedef, kapsayıcı kaynaktır.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<storage-account-name>.blob.core.windows.net/<container-name>' 'C:\myDirectory' --recursive` |
| **Örnek** | `azcopy sync 'https://mystorageaccount.blob.core.windows.net/mycontainer' 'C:\myDirectory' --recursive` |

### <a name="update-a-container-with-changes-in-another-container"></a>Başka bir kapsayıcıda değişiklik olan bir kapsayıcıyı güncelleştirme

Bu komutta görünen ilk kapsayıcı kaynaktır. İkincisi hedef.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>' --recursive` |
| **Örnek** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/mycontainer' 'https://mydestinationaccount.blob.core.windows.net/mycontainer' --recursive` |

### <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Başka bir dosya paylaşımında dizin değişikliği olan bir dizini güncelleştirme

Bu komutta görünen ilk dizin kaynaktır. İkincisi hedef.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' 'https://<destination-storage-account-name>.blob.core.windows.net/<container-name>/<directory-name>' --recursive` |
| **Örnek** | `azcopy sync 'https://mysourceaccount.blob.core.windows.net/<container-name>/myDirectory' 'https://mydestinationaccount.blob.core.windows.net/mycontainer/myDirectory' --recursive` |

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerin herhangi birinde daha fazla örnek bulabilirsiniz:

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)

- [Öğretici: AzCopy kullanarak şirket içi verileri bulut depolamasına taşıma](storage-use-azcopy-migrate-on-premises-data.md)

- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)

- [AzCopy ve Amazon S3 kovaları ile veri aktarımı](storage-use-azcopy-s3.md)

- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)
