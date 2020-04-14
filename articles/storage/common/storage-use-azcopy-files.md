---
title: AzCopy v10 kullanarak Azure Dosyalarına veya Azure Dosyalarına veri aktarımı | Microsoft Dokümanlar
description: AzCopy ve dosya depolama ile veri aktarımı.
author: normesta
ms.service: storage
ms.topic: conceptual
ms.date: 04/10/2020
ms.author: normesta
ms.subservice: common
ms.openlocfilehash: 59f5733009424c60f2b9c48e68d70bbc29ad7095
ms.sourcegitcommit: 8dc84e8b04390f39a3c11e9b0eaf3264861fcafc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/13/2020
ms.locfileid: "81263378"
---
# <a name="transfer-data-with-azcopy-and-file-storage"></a>AzCopy ve dosya depolama ile veri aktarımı 

AzCopy, bir depolama hesabına veya bir depolama hesabından blobveya dosya kopyalamak için kullanabileceğiniz bir komut satırı yardımcı programıdır. Bu makalede, Azure Dosyaları ile çalışan örnek komutlar içerir.

Başlamadan önce, AzCopy'yi indirmek ve aracı tanımak için [AzCopy makalesine başlayın'](storage-use-azcopy-v10.md) a bakın.

> [!TIP]
> Bu makaledeki örnekler, yol bağımsız değişkenlerini tek tırnak ('') ile kaplar. Windows Komut Uyruşu (cmd.exe) hariç tüm komut kabuklarında tek tırnak işareti kullanın. Windows Komut Uyruşu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak ('') yerine çift tırnak ("") içeren bir şekilde içine çekin.

## <a name="create-file-shares"></a>Dosya paylaşımları oluşturma

Bir dosya paylaşımı oluşturmak için [azcopy make](storage-ref-azcopy-make.md) komutunu kullanabilirsiniz. Bu bölümdeki örnek, . `myfileshare`

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy make 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>'` |
| **Örnek** | `azcopy make 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

Ayrıntılı başvuru dokümanları için [azcopy make](storage-ref-azcopy-make.md)adresine bakın.

## <a name="upload-files"></a>Dosyaları karşıya yükleme

Yerel bilgisayarınızdan dosya ve dizin yüklemek için [azcopy copy komutunu](storage-ref-azcopy-copy.md) kullanabilirsiniz.

Bu bölümde aşağıdaki örnekler yer almaktadır:

> [!div class="checklist"]
> * Dosyayı karşıya yükleme
> * Dizin yükleme
> * Dizinin içeriğini yükleme
> * Belirli bir dosyayı yükleme

> [!TIP]
> İsteğe bağlı bayraklar kullanarak yükleme işleminde değişiklik yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyalarla birlikte erişim denetim listelerini (ALA'lar) kopyalayın.|**--korumak-smb-izinleri**=\[\|doğru yanlış\]|
> |Dosyalarla birlikte Kobİ özellik bilgilerini kopyalayın.|**--preserve-smb-info**=\[\|doğru yanlış\]|
> |Dosyaları Append Blobs veya Page Blobs olarak yükleyin.|**--blob tipi**=\[BlockBlob PageBlob\|\|AppendBlob\]|
> |Belirli bir erişim katmanına (arşiv katmanı gibi) yükleyin.|**--block-blob-tier**=\[\|None\|\|Hot Cool Arşiv\]|
> 
> Tam liste için [seçeneklere](storage-ref-azcopy-copy.md#options)bakın.

> [!NOTE]
> AzCopy, dosyanın md5 karma kodunu otomatik olarak hesaplamaz ve depolamaz. AzCopy'nin bunu yapmasını istiyorsanız, bayrağı `--put-md5` her kopyalama komutuna eklayın. Bu şekilde, dosya indirildiğinde, AzCopy indirilen veriler için bir MD5 karma sını hesaplar ve dosyanın `Content-md5` özelliğinde depolanan MD5 karmasının hesaplanan karmayla eşleştiğini doğrular.

### <a name="upload-a-file"></a>Dosyayı karşıya yükleme

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-file-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-name><SAS-token>'` |
| **Örnek** | `azcopy copy 'C:\myDirectory\myTextFile.txt' 'https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D'` |

Dosya yolunun veya dosya adının herhangi bir yerine joker karakter simgesi (*) kullanarak dosya yükleyebilirsiniz. Örneğin: `'C:\myDirectory\*.txt'`, `C:\my*\*.txt`veya .

### <a name="upload-a-directory"></a>Dizin yükleme

Bu örnek, bir dizin (ve bu dizindeki tüm dosyaları) bir dosya paylaşımına kopyalar. Sonuç, dosya paylaşımında aynı ada göre bir dizinidir.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' --recursive` |

Dosya paylaşımıiçindeki bir dizine kopyalamak için komut dizenizde o dizinin adını belirtmeniz gerekiyor.

|    |     |
|--------|-----------|
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' --recursive` |

Dosya paylaşımında bulunmayan bir dizinin adını belirtirseniz, AzCopy bu ada göre yeni bir dizin oluşturur.

### <a name="upload-the-contents-of-a-directory"></a>Dizinin içeriğini yükleme

Joker karakter simgesini (*) kullanarak, içeren dizinin kendisini kopyalamadan bir dizinin içeriğini yükleyebilirsiniz.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>/*' 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>` |
| **Örnek** | `azcopy copy 'C:\myDirectory\*' 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D"` |

> [!NOTE]
> Tüm alt `--recursive` dizinlere dosya yüklemek için bayrağı ekleyin.

### <a name="upload-specific-files"></a>Belirli dosyaları yükleme

Tam dosya adları belirtebilir veya joker karakter (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok tam dosya adı belirtin

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-path` kopyalama komutunu kullanın. Bir semicolon kullanarak ayrı ayrı`;`ayrı dosya adları ( ).

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' --include-path <semicolon-separated-file-list>` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --include-path 'photos;documents\myFile.txt'` |

Bu örnekte, AzCopy `C:\myDirectory\photos` dizini `C:\myDirectory\documents\myFile.txt` ve dosyayı aktarMaktadır. Tüm dosyaları `--recursive` `C:\myDirectory\photos` dizine aktarma seçeneğini eklemeniz gerekir.

`--exclude-path` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

#### <a name="use-wildcard-characters"></a>Joker karakter kullanma

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-pattern` kopyalama komutunu kullanın. Joker karakter içeren kısmi adlar belirtin. Bir semicolin kullanarak ayrı`;`adlar ( ).

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy '<local-directory-path>' 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örnek** | `azcopy copy 'C:\myDirectory' 'https://mystorageaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --include-pattern 'myFile*.txt;*.pdf*'` |

`--exclude-pattern` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

Ve `--include-pattern` `--exclude-pattern` seçenekleri yalnızca dosya adları için değil, yol için geçerlidir.  Dizin ağacında bulunan tüm metin dosyalarını kopyalamak istiyorsanız, `–recursive` tüm dizin ağacını almak için `–include-pattern` seçeneği `*.txt` kullanın ve ardından tüm metin dosyalarını almak için ve belirtin.

## <a name="download-files"></a>Dosyaları indirme

Dosyaları, dizinleri ve dosya paylaşımlarını yerel bilgisayarınıza indirmek için [azcopy copy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanabilirsiniz.

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
> |Dosyalarla birlikte erişim denetim listelerini (ALA'lar) kopyalayın.|**--korumak-smb-izinleri**=\[\|doğru yanlış\]|
> |Dosyalarla birlikte Kobİ özellik bilgilerini kopyalayın.|**--preserve-smb-info**=\[\|doğru yanlış\]|
> |Dosyaları otomatik olarak dekomprese edin.|**--dekompresör**=\[gzip\|deflate\]|
> 
> Tam liste için [seçeneklere](storage-ref-azcopy-copy.md#options)bakın.

> [!NOTE]
> Bir `Content-md5` dosyanın özellik değeri karma içeriyorsa, AzCopy indirilen veriler için bir MD5 karma sını hesaplar ve dosyanın `Content-md5` özelliğinde depolanan MD5 karmasının hesaplanan karmayla eşleştiğini doğrular. Bu değerler eşleşmiyorsa, bu davranışı ekleyerek `--check-md5=NoCheck` veya `--check-md5=LogOnly` kopyalama komutuna ekleyerek geçersiz kılmadığınız sürece karşıdan yükleme başarısız olur.

### <a name="download-a-file"></a>Dosya indirme

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>' '<local-file-path>'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory\myTextFile.txt'` |

### <a name="download-a-directory"></a>Bir dizin indirin

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>' '<local-directory-path>' --recursive` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory'  --recursive` |

Bu örnek, indirilen tüm `C:\myDirectory\myFileShareDirectory` dosyaları içeren adlı bir dizini ile sonuçlanır.

### <a name="download-the-contents-of-a-directory"></a>Dizinin içeriğini indirin

Joker karakter simgesini (*) kullanarak, içeren dizinin kendisini kopyalamadan bir dizinin içeriğini indirebilirsiniz.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-name>/*<SAS-token>' '<local-directory-path>/'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myfileshare/myFileShareDirectory/*?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D' 'C:\myDirectory'` |

> [!NOTE]
> Tüm alt `--recursive` dizinlerde dosyaları indirmek için bayrağı ekleyin.

### <a name="download-specific-files"></a>Belirli dosyaları karşıdan yükleme

Tam dosya adları belirtebilir veya joker karakter (*) ile kısmi adlar kullanabilirsiniz.

#### <a name="specify-multiple-complete-file-names"></a>Birden çok tam dosya adı belirtin

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-path` kopyalama komutunu kullanın. Bir semicolin kullanarak ayrı ayrı`;`ayrı dosya adları ( ).

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.file.core.windows.net/<file-share-or-directory-name><SAS-token>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive` |

Bu örnekte, AzCopy `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/photos` dizini `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/documents/myFile.txt` ve dosyayı aktarMaktadır. Tüm dosyaları `--recursive` `https://mystorageaccount.file.core.windows.net/myFileShare/myDirectory/photos` dizine aktarma seçeneğini eklemeniz gerekir.

`--exclude-path` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

#### <a name="use-wildcard-characters"></a>Joker karakter kullanma

Seçeneği ile [azcopy](storage-ref-azcopy-copy.md) copy `--include-pattern` kopyalama komutunu kullanın. Joker karakter içeren kısmi adlar belirtin. Bir semicolin kullanarak ayrı`;`adlar ( ).

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name><SAS-token>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |

`--exclude-pattern` Ayrıca, seçeneği kullanarak dosyaları hariç tutabilirsiniz. Daha fazla bilgi için [azcopy copy](storage-ref-azcopy-copy.md) başvuru dokümanlarına bakın.

Ve `--include-pattern` `--exclude-pattern` seçenekleri yalnızca dosya adları için değil, yol için geçerlidir.  Dizin ağacında bulunan tüm metin dosyalarını kopyalamak istiyorsanız, `–recursive` tüm dizin ağacını almak için `–include-pattern` seçeneği `*.txt` kullanın ve ardından tüm metin dosyalarını almak için ve belirtin.

## <a name="copy-files-between-storage-accounts"></a>Depolama hesapları arasında dosyalar kopyalama

Dosyaları diğer depolama hesaplarına kopyalamak için AzCopy'yi kullanabilirsiniz. Kopyalama işlemi eşzamanlıdır, bu nedenle komut döndüğünde tüm dosyaların kopyalandığını gösterir.

AzCopy [sunucudan sunucuya](https://docs.microsoft.com/rest/api/storageservices/put-block-from-url) [API'ler](https://docs.microsoft.com/rest/api/storageservices/put-page-from-url)kullanır, böylece veriler doğrudan depolama sunucuları arasında kopyalanır. Bu kopyalama işlemleri bilgisayarınızın ağ bant genişliğini kullanmaz. `AZCOPY_CONCURRENCY_VALUE` Ortam değişkeninin değerini ayarlayarak bu işlemlerin iş kısmını artırabilirsiniz. Daha fazla bilgi için, [iş akışını optimize etme](storage-use-azcopy-configure.md#optimize-throughput)'ye bakın.

Bu bölümde aşağıdaki örnekler yer almaktadır:

> [!div class="checklist"]
> * Dosyayı başka bir depolama hesabına kopyalama
> * Dizini başka bir depolama hesabına kopyalama
> * Dosya paylaşımını başka bir depolama hesabına kopyalama
> * Tüm dosya paylaşımlarını, dizinleri ve dosyaları başka bir depolama hesabına kopyalama

> [!TIP]
> İsteğe bağlı bayraklar kullanarak kopyalama işleminizde değişiklik yapabilirsiniz. İşte birkaç örnek.
>
> |Senaryo|Bayrak|
> |---|---|
> |Dosyalarla birlikte erişim denetim listelerini (ALA'lar) kopyalayın.|**--korumak-smb-izinleri**=\[\|doğru yanlış\]|
> |Dosyalarla birlikte Kobİ özellik bilgilerini kopyalayın.|**--preserve-smb-info**=\[\|doğru yanlış\]|
> |Dosyaları Apend Blobs veya Page Blobs olarak kopyalayın.|**--blob tipi**=\[BlockBlob PageBlob\|\|AppendBlob\]|
> |Belirli bir erişim katmanına (arşiv katmanı gibi) kopyalayın.|**--block-blob-tier**=\[\|None\|\|Hot Cool Arşiv\]|
> 
> Tam liste için [seçeneklere](storage-ref-azcopy-copy.md#options)bakın.

### <a name="copy-a-file-to-another-storage-account"></a>Dosyayı başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name>/<file-path><SAS-token>'` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D'` |

### <a name="copy-a-directory-to-another-storage-account"></a>Dizini başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-path><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer/myBlobDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="copy-a-file-share-to-another-storage-account"></a>Dosya paylaşımını başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/mycontainer?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="copy-all-file-shares-directories-and-files-to-another-storage-account"></a>Tüm dosya paylaşımlarını, dizinleri ve dosyaları başka bir depolama hesabına kopyalama

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<source-storage-account-name>.file.core.windows.net/<SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<SAS-token>' --recursive'` |
| **Örnek** | `azcopy copy 'https://mysourceaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

## <a name="synchronize-files"></a>Dosyaları eşitle

Bir dosya paylaşımının içeriğini başka bir dosya paylaşımıyla eşitleyebilirsiniz. Ayrıca, bir dosya paylaşımındaki dizin içeriğini başka bir dosya paylaşımında bulunan bir dizinin içeriğiyle eşitleyebilirsiniz. Eşitleme tek yönlüdür. Başka bir deyişle, bu iki uç noktadan hangisinin kaynak, hangisinin hedef olduğunu seçersiniz. Eşitleme de sunucu API'ler için sunucu kullanır.

> [!NOTE]
> Şu anda, bu senaryo yalnızca hiyerarşik ad alanı olmayan hesaplar için desteklenir. AzCopy'nin geçerli sürümü Azure Dosyaları ve Blob Depolama arasında eşitleme yapmaz.

[Eşitleme](storage-ref-azcopy-sync.md) komutu dosya adlarını ve son değiştirilen zaman damgalarını karşılaştırır. İsteğe bağlı bayrağı, `true` kaynak `prompt` dizinde artık yoksa hedef dizinindeki dosyaların değerine veya silinmeye ayarlayın. `--delete-destination`

`--delete-destination` Bayrağı AzCopy olarak `true` ayarlarsanız, bir istem sağlamadan dosyaları siler. AzCopy bir dosyayı silmeden önce bir istem `--delete-destination` görünmesini `prompt`istiyorsanız, bayrağı ' ya ayarlayın.

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

### <a name="update-a-file-share-with-changes-to-another-file-share"></a>Başka bir dosya paylaşımında yapılan değişikliklerle dosya paylaşımını güncelleştirme

Bu komutta görünen ilk dosya paylaşımı kaynaktır. İkincisi hedef.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örnek** | `azcopy sync 'https://mysourceaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="update-a-directory-with-changes-to-a-directory-in-another-file-share"></a>Başka bir dosya paylaşımında dizin değişikliği olan bir dizini güncelleştirme

Bu komutta görünen ilk dizin kaynaktır. İkincisi hedef.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-name><SAS-token>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name>/<directory-name><SAS-token>' --recursive` |
| **Örnek** | `azcopy sync 'https://mysourceaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' 'https://mydestinationaccount.file.core.windows.net/myFileShare/myDirectory?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

### <a name="update-a-file-share-to-match-the-contents-of-a-share-snapshot"></a>Bir dosya paylaşımını paylaşım anlık görüntüsünün içeriğiyle eşleşecek şekilde güncelleştirme

Bu komutta görünen ilk dosya paylaşımı kaynaktır. URI'nin sonunda, enstantanenin `&sharesnapshot=` **DateTime** değerini izleyen dizeyi tamamla. 

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy sync 'https://<source-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>&sharesnapsot<snapshot-ID>' 'https://<destination-storage-account-name>.file.core.windows.net/<file-share-name><SAS-token>' --recursive` |
| **Örnek** | `azcopy sync 'https://mysourceaccount.file.core.windows.net/myfileShare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D&sharesnapshot=2020-03-03T20%3A24%3A13.0000000Z' 'https://mydestinationaccount.file.core.windows.net/myfileshare?sv=2018-03-28&ss=bfqt&srt=sco&sp=rwdlacup&se=2019-07-04T05:30:08Z&st=2019-07-03T21:30:08Z&spr=https&sig=CAfhgnc9gdGktvB=ska7bAiqIddM845yiyFwdMH481QA8%3D' --recursive` |

Paylaşım anlık görüntüleri hakkında daha fazla bilgi edinmek [için Azure Dosyaları için paylaşım anlık görüntülerine genel bakış bölümüne](https://docs.microsoft.com/azure/storage/files/storage-snapshots-files)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerin herhangi birinde daha fazla örnek bulabilirsiniz:

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)

- [AzCopy ve blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)

- [AzCopy ve Amazon S3 kovaları ile veri aktarımı](storage-use-azcopy-s3.md)

- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)
