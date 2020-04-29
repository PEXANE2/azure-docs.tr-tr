---
title: AzCopy ile v10 arasındaki kullanarak Azure dosyalarına veri aktarma | Microsoft Docs
description: AzCopy ve dosya depolama ile veri aktarma.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 7f47dd05035772744fb212ef8914b25979af61e2
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82137170"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>AzCopy ve dosya depolama ile veri aktarma 

AzCopy, bir depolama hesabına blob veya dosya kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makale, Azure dosyaları ile çalışan örnek komutlar içerir.

Başlamadan önce AzCopy [ile çalışmaya başlama](storage-use-azcopy-v10.md) makalesini okuyun ve aracı hakkında bilgi edinin.

> [!TIP]
> Bu makaledeki örneklerde, tek tırnak (' ') ile yol bağımsız değişkenleri yer alınmalıdır. Windows komut kabuğu (cmd. exe) dışında tüm komut kabukleriyle tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd. exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

## <a name="create-file-shares"></a>Dosya paylaşımları oluşturma

Bir dosya paylaşma oluşturmak için [AzCopy Make](storage-ref-azcopy-make.md) komutunu kullanabilirsiniz. Bu bölümdeki örnek adlı `myfileshare`bir dosya paylaşma oluşturur.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy make 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>'` |
| **Örneğinde** | `azcopy make 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

Ayrıntılı başvuru belgeleri için bkz. [AzCopy Make](storage-ref-azcopy-make.md).

## <a name="upload-files"></a>Dosyaları karşıya yükleme

Yerel bilgisayarınızdan dosya ve Dizin yüklemek için [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanabilirsiniz.

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosyayı karşıya yükleme
> * Bir dizini karşıya yükle
> * Bir dizinin içeriğini karşıya yükleme
> * Belirli bir dosyayı karşıya yükle

> [!TIP]
> İsteğe bağlı bayraklar kullanarak karşıya yükleme işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Erişim denetim listelerini (ACL 'Ler) dosyalarla birlikte kopyalayın.|**--Preserve-SMB-Permissions**=\[true\|false\]|
> |SMB Özellik bilgilerini dosyalarla birlikte kopyalayın.|**--Preserve-SMB-Info**=\[true\|false\]|
> |Dosyaları ekleme Blobları veya sayfa Blobları olarak karşıya yükleyin.|**--blob-tür**=\[Blok Blobu\|pageblob\|appendblob\]|
> |Belirli bir erişim katmanına (Arşiv katmanı gibi) yükleyin.|**--Block-blob-katman**=\[yok\|sık\|erişimli\|seyrek Arşiv\]|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-copy.md#options).

> [!NOTE]
> AzCopy, dosyanın MD5 karma kodunu otomatik olarak hesaplamaz ve depolamaz. AzCopy 'in bunu yapmak istiyorsanız, her bir kopyala komutuna `--put-md5` bayrak ekleyin. Bu şekilde, Dosya indirildiğinde AzCopy, indirilen veriler için bir MD5 karması hesaplar ve dosyanın `Content-md5` ÖZELLIĞINDE depolanan MD5 karmasının hesaplanan karmayla eşleştiğini doğrular.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-file-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name><SAS-token>'` |
| **Örneğinde** | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

Dosya yolu veya dosya adında herhangi bir yerde joker karakter sembolünü (*) kullanarak da bir dosyayı karşıya yükleyebilirsiniz. Örneğin: `'C:\myDirectory\*.txt'`, veya `C:\my*\*.txt`.

### <a name="upload-a-directory"></a>Bir dizini karşıya yükle

Bu örnekte bir dizin (ve bu dizindeki tüm dosyalar) bir dosya paylaşımında kopyalanır. Sonuç, dosya paylaşımındaki aynı ada sahip bir dizindir.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örneğinde** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' --recursive` |

Dosya paylaşımındaki bir dizine kopyalamak için, komut dizeniz içindeki bu dizinin adını belirtmeniz yeterlidir.

|    |     |
|--------|-----------|
| **Örneğinde** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' --recursive` |

Dosya paylaşımında var olmayan bir dizinin adını belirtirseniz AzCopy, bu adla yeni bir dizin oluşturur.

### <a name="upload-the-contents-of-a-directory"></a>Bir dizinin içeriğini karşıya yükleme

Joker karakter sembolünü (*) kullanarak, içeren dizinin kendisini kopyalamadan bir dizinin içeriğini karşıya yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>/*' 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>` |
| **Örneğinde** | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Tüm alt `--recursive` dizinlerde dosyaları karşıya yüklemek için bayrağı ekleyin.

### <a name="upload-specific-files"></a>Belirli dosyaları karşıya yükle

Tüm dosya adlarını belirtebilir veya joker karakterler (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok dosya adı belirtin

`--include-path` Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın. Tek bir dosya adını noktalı virgül (`;`) kullanarak ayırın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' --include-path <semicolon-separated-file-list>` |
| **Örneğinde** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --include-path 'photos;documents\myFile.txt'` |

Bu örnekte, AzCopy `C:\myDirectory\photos` dizini ve `C:\myDirectory\documents\myFile.txt` dosyayı aktarır. `C:\myDirectory\photos` Dizindeki tüm dosyaları aktarma `--recursive` seçeneğini dahil etmeniz gerekir.

Ayrıca, `--exclude-path` seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz. Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

#### <a name="use-wildcard-characters"></a>Joker karakter kullan

`--include-pattern` Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın. Joker karakterleri içeren kısmi adlar belirtin. Bir semicolin (`;`) kullanarak adları ayırın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örneğinde** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --include-pattern 'myFile*.txt;*.pdf*'` |

Ayrıca, `--exclude-pattern` seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz. Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

Ve `--include-pattern` `--exclude-pattern` seçenekleri, yalnızca dosya adları için geçerlidir, yola değildir.  Bir dizin ağacında var olan tüm metin dosyalarını kopyalamak istiyorsanız, tüm dizin ağacını almak için `–recursive` seçeneğini kullanın ve ardından öğesini kullanarak `–include-pattern` `*.txt` tüm metin dosyalarını alın.

## <a name="download-files"></a>Dosyaları indirme

Dosya, dizin ve dosya paylaşımlarını yerel bilgisayarınıza indirmek için [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanabilirsiniz.

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
> |Erişim denetim listelerini (ACL 'Ler) dosyalarla birlikte kopyalayın.|**--Preserve-SMB-Permissions**=\[true\|false\]|
> |SMB Özellik bilgilerini dosyalarla birlikte kopyalayın.|**--Preserve-SMB-Info**=\[true\|false\]|
> |Dosyaları otomatik olarak aç.|**--sıkıştırmayı aç**|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-copy.md#options).

> [!NOTE]
> Bir dosyanın `Content-md5` Özellik değeri bir karma Içeriyorsa, AzCopy indirilen veriler IÇIN bir MD5 karması hesaplar ve dosyanın `Content-md5` özelliğinde depolanan MD5 karmasının hesaplanan karmayla eşleştiğini doğrular. Bu değerler eşleşmezse, Copy komutuna veya `--check-md5=NoCheck` `--check-md5=LogOnly` öğesini ekleyerek bu davranışı geçersiz kılmadığınız müddetçe indirme işlemi başarısız olur.

### <a name="download-a-file"></a>Dosya indirme

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>' '<local-file-path>'` |
| **Örneğinde** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory\myTextFile.txt'` |

### <a name="download-a-directory"></a>Bir dizin indirin

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>' '<local-directory-path>' --recursive` |
| **Örneğinde** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory'  --recursive` |

Bu örnek, indirilen tüm dosyaları içeren `C:\myDirectory\myFileShareDirectory` adlı dizine neden olur.

### <a name="download-the-contents-of-a-directory"></a>Bir dizinin içeriğini indirin

Joker karakter sembolünü (*) kullanarak, bir dizinin içeriğini içeren dizini kopyalamadan indirebilirsiniz.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/*<SAS-token>' '<local-directory-path>/'` |
| **Örneğinde** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory'` |

> [!NOTE]
> Dosyaları tüm `--recursive` alt dizinlerde indirmek için bayrağı ekleyin.

### <a name="download-specific-files"></a>Belirli dosyaları indir

Tüm dosya adlarını belirtebilir veya joker karakterler (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok dosya adı belirtin

`--include-path` Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın. Tek dosya adlarını bir semicolin (`;`) kullanarak ayırın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>` |
| **Örneğinde** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive` |

Bu örnekte, AzCopy `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/photos` dizini ve `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/documents/myFile.txt` dosyayı aktarır. `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/photos` Dizindeki tüm dosyaları aktarma `--recursive` seçeneğini dahil etmeniz gerekir.

Ayrıca, `--exclude-path` seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz. Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

#### <a name="use-wildcard-characters"></a>Joker karakter kullan

`--include-pattern` Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın. Joker karakterleri içeren kısmi adlar belirtin. Bir semicolin (`;`) kullanarak adları ayırın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name><SAS-token>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örneğinde** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |

Ayrıca, `--exclude-pattern` seçeneğini kullanarak dosyaları dışarıda bırakabilirsiniz. Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

Ve `--include-pattern` `--exclude-pattern` seçenekleri, yalnızca dosya adları için geçerlidir, yola değildir.  Bir dizin ağacında var olan tüm metin dosyalarını kopyalamak istiyorsanız, tüm dizin ağacını almak için `–recursive` seçeneğini kullanın ve ardından öğesini kullanarak `–include-pattern` `*.txt` tüm metin dosyalarını alın.

## <a name="copy-files-between-storage-accounts"></a>Depolama hesapları arasında dosyalar kopyalama

Daha fazla depolama hesabına dosya kopyalamak için AzCopy kullanabilirsiniz. Kopyalama işlemi, komutun döndürdüğü zaman zaman uyumludur, yani tüm dosyalar kopyalanmış olduğunu gösterir.

AzCopy, [sunucudan sunucuya](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) [API 'ler](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)kullanır, bu nedenle veriler doğrudan depolama sunucuları arasında kopyalanır. Bu kopyalama işlemleri bilgisayarınızın ağ bant genişliğini kullanmaz. `AZCOPY_CONCURRENCY_VALUE` Ortam değişkeninin değerini ayarlayarak bu işlemlerin verimini artırabilirsiniz. Daha fazla bilgi için bkz. [aktarım hızını iyileştirme](storage-use-azcopy-configure.md#optimize-throughput).

Bu bölüm aşağıdaki örnekleri içerir:

> [!div class="checklist"]
> * Dosyayı başka bir depolama hesabına Kopyala
> * Bir dizini başka bir depolama hesabına kopyalama
> * Dosya paylaşımından başka bir depolama hesabına kopyalama
> * Tüm dosya paylaşımlarını, dizinleri ve dosyaları başka bir depolama hesabına Kopyala

> [!TIP]
> İsteğe bağlı bayraklar kullanarak kopyalama işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Erişim denetim listelerini (ACL 'Ler) dosyalarla birlikte kopyalayın.|**--Preserve-SMB-Permissions**=\[true\|false\]|
> |SMB Özellik bilgilerini dosyalarla birlikte kopyalayın.|**--Preserve-SMB-Info**=\[true\|false\]|
> |Dosyaları ekleme Blobları veya sayfa Blobları olarak kopyalayın.|**--blob-tür**=\[Blok Blobu\|pageblob\|appendblob\]|
> |Belirli bir erişim katmanına (Arşiv katmanı gibi) kopyalayın.|**--Block-blob-katman**=\[yok\|sık\|erişimli\|seyrek Arşiv\]|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-copy.md#options).

### <a name="copy-a-file-to-another-storage-account"></a>Dosyayı başka bir depolama hesabına Kopyala

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>'` |
| **Örneğinde** | `azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D'` |

### <a name="copy-a-directory-to-another-storage-account"></a>Bir dizini başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örneğinde** | `azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="copy-a-file-share-to-another-storage-account"></a>Dosya paylaşımından başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örneğinde** | `azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="copy-all-file-shares-directories-and-files-to-another-storage-account"></a>Tüm dosya paylaşımlarını, dizinleri ve dosyaları başka bir depolama hesabına Kopyala

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<SAS-token>' --recursive'` |
| **Örneğinde** | `azcopy copy 'https://mysourceaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

## <a name="synchronize-files"></a>Dosyaları eşitler

Bir dosya paylaşımının içeriğini başka bir dosya paylaşımıyla eşzamanlı hale getirebilirsiniz. Ayrıca, bir dosya paylaşımındaki dizinin içeriğini başka bir dosya paylaşımında bulunan bir dizinin içeriğiyle de eşzamanlı hale getirebilirsiniz. Eşitleme tek yönlü. Diğer bir deyişle, bu iki uç noktanın hangisinin kaynak olduğunu ve hedefin nerede olduğunu seçersiniz. Eşitleme, sunucuyu sunucu API 'Leri için de kullanır.

> [!NOTE]
> Şu anda, bu senaryo yalnızca hiyerarşik bir ad alanına sahip olmayan hesaplar için desteklenir. AzCopy 'in geçerli sürümü, Azure dosyaları ile BLOB depolama arasında eşitlenmez.

[Eşitleme](storage-ref-azcopy-sync.md) komutu dosya adlarını ve son değiştirilme zaman damgalarını karşılaştırır. Bu dosyalar `--delete-destination` artık kaynak dizinde yoksa, hedef `true` dizindeki `prompt` dosyaları silmek için veya değerine isteğe bağlı bayrağını ayarlayın.

AzCopy bayrağını, `--delete-destination` bir istem `true` sağlamadan dosyaları siler olarak ayarlarsanız. AzCopy bir dosyayı silmesinden önce bir istem görünmesini istiyorsanız, `--delete-destination` bayrağını olarak `prompt`ayarlayın.

> [!TIP]
> İsteğe bağlı bayraklar kullanarak eşitleme işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |İndirme sırasında ne kadar kesin MD5 karmalarının doğrulanması gerektiğini belirtin.|**--Check-MD5**=\[nocheck\|LogOnly\|failiffarklı\|failiffarklıentormissing\]|
> |Dosyaları bir düzene göre hariç tutun.|**--exclude-Path**|
> |Eşitleme ile ilgili günlük girdilerinizi ne kadar ayrıntılı olarak istediğinizi belirtin.|**--günlük düzeyi**=\[Uyarı\|hatası\|bilgisi\|yok\]|
> 
> Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-sync.md#options).

### <a name="update-a-file-share-with-changes-to-another-file-share"></a>Başka bir dosya paylaşımındaki değişikliklerle dosya paylaşma güncelleştirme

Bu komutta görünen ilk dosya paylaşımının kaynağı. İkincisi, hedefin bir biridir.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örneğinde** | `azcopy sync 'https://mysourceaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Başka bir dosya paylaşımındaki bir dizinde bulunan değişikliklerle Dizin güncelleştirme

Bu komutta görüntülenen ilk dizin kaynağıdır. İkincisi, hedefin bir biridir.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-name><SAS-token>' --recursive` |
| **Örneğinde** | `azcopy sync 'https://mysourceaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="update-a-file-share-to-match-the-contents-of-a-share-snapshot"></a>Bir dosya paylaşımının paylaşma anlık görüntüsünün içeriğiyle eşleşecek şekilde güncelleştirilmesi

Bu komutta görünen ilk dosya paylaşımının kaynağı. URI 'nin sonunda, dizeyi `&sharesnapshot=` ve ardından anlık görüntünün **Tarih saat** değerini ekleyin. 

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>&sharesnapsot<snapshot-ID>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örneğinde** | `azcopy sync 'https://mysourceaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D&sharesnapshot=2020-03-03T20%3A24%3A13.0000000Z' 'https://mydestinationaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

Anlık görüntü paylaşma hakkında daha fazla bilgi için bkz. [Azure dosyaları için paylaşılan anlık görüntülere genel bakış](https://docs.microsoft.com/azure/storage/files/storage-snapshots-files).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerden herhangi birinde daha fazla örnek bulabilirsiniz:

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)

- [AzCopy ve BLOB Storage ile veri aktarma](storage-use-azcopy-blobs.md)

- [AzCopy ve Amazon S3 demetleri ile veri aktarma](storage-use-azcopy-s3.md)

- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)
