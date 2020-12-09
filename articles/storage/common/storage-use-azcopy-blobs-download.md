---
title: AzCopy ile v10 arasındaki kullanarak blob 'ları Azure Blob depolamadan indirin | Microsoft Docs
description: Bu makale, Azure Blob depolamadan blob 'ları indirmelerine yardımcı olan AzCopy örnek komutlarının bir koleksiyonunu içerir.
author: normesta
ms.service: storage
ms.topic: how-to
ms.date: 12/08/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: dineshm
ms.openlocfilehash: 0a2490a104d18f77a0ec326933f463eb4ebb4339
ms.sourcegitcommit: fec60094b829270387c104cc6c21257826fccc54
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2020
ms.locfileid: "96923972"
---
# <a name="download-blobs-from-azure-blob-storage-by-using-azcopy-v10"></a>AzCopy ile v10 arasındaki kullanarak blob 'ları Azure Blob depolamadan indirin

AzCopy ile v10 arasındaki komut satırı yardımcı programını kullanarak blob depolamadan Blobları ve dizinleri indirebilirsiniz. 

Dosyaları karşıya yükleme, blob depolamayla eşitleme veya hesaplar arasında blobları kopyalama gibi diğer görev türlerine yönelik örnekleri görmek için, bu makalenin [sonraki adımları](#next-steps) bölümünde sunulan bağlantılara bakın.

## <a name="get-started"></a>başlarken

AzCopy ['i indirmek Için AzCopy ile çalışmaya başlama](storage-use-azcopy-v10.md) makalesini inceleyin ve depolama hizmetine yetkilendirme kimlik bilgilerini nasıl sağlayabileceğiniz yolları hakkında bilgi edinin.

> [!NOTE] 
> Bu makaledeki örneklerde Azure Active Directory (Azure AD) kullanarak yetkilendirme kimlik bilgileri sağladığınızı varsayalım.
>
> Blob verilerine erişim yetkisi vermek için bir SAS belirteci kullanıyorsanız, bu belirteci her AzCopy komutunda kaynak URL 'sine ekleyebilirsiniz. Örneğin: `'https://<storage-account-name>.blob.core.windows.net/<container-name><SAS-token>'`.

## <a name="download-a-blob"></a>Blobu indirme

[AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanarak bir blobu indirin.

> [!TIP]
> Bu örnek tek tırnak (' ') ile yol bağımsız değişkenlerini barındırır. Windows komut kabuğu (cmd.exe) hariç tüm komut kabukta tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>' '<local-file-path>'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt' 'C:\myDirectory\myTextFile.txt'` |

> [!NOTE]
> `Content-md5`Bir Blobun Özellik değeri bir karma içeriyorsa, AzCopy indirilen veriler için BIR MD5 karması hesaplar ve BLOB özelliğinde depolanan MD5 karmasının `Content-md5` hesaplanan karmayla eşleştiğini doğrular. Bu değerler eşleşmezse, `--check-md5=NoCheck` Copy komutuna veya öğesini ekleyerek bu davranışı geçersiz kılmadığınız müddetçe indirme işlemi başarısız olur `--check-md5=LogOnly` .

## <a name="download-a-directory"></a>Bir dizin indirin

[AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanarak bir dizin indirin.

> [!TIP]
> Bu örnek tek tırnak (' ') ile yol bağımsız değişkenlerini barındırır. Windows komut kabuğu (cmd.exe) hariç tüm komut kabukta tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<directory-path>' '<local-directory-path>' --recursive` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myBlobDirectory' 'C:\myDirectory'  --recursive` |

Bu örnek `C:\myDirectory\myBlobDirectory` , indirilen tüm Blobları içeren adlı dizine neden olur.

## <a name="download-directory-contents"></a>Dizin içeriğini indirin

Joker karakter simgesini (*) kullanarak dizinin kendisini kopyalamadan içeriğini indirebilirsiniz.

> [!TIP]
> Bu örnek tek tırnak (' ') ile yol bağımsız değişkenlerini barındırır. Windows komut kabuğu (cmd.exe) hariç tüm komut kabukta tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

> [!NOTE]
> Şu anda, bu senaryo yalnızca hiyerarşik bir ad alanına sahip olmayan hesaplar için desteklenir.

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.blob.core.windows.net/<container-name>/*' '<local-directory-path>/'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myBlobDirectory/*' 'C:\myDirectory'` |

`--recursive`Dosyaları tüm alt dizinlere indirmek için bayrağı ekleyin.

## <a name="download-specific-blobs"></a>Belirli Blobları indirin

Tüm dosya adlarını, joker karakterlerle kısmi adları (*) veya tarih ve saatleri kullanarak belirli Blobları indirebilirsiniz. 

> [!TIP]
> Bu örnekler, yol bağımsız değişkenlerini tek tırnak (' ') ile çevreler. Windows komut kabuğu (cmd.exe) hariç tüm komut kabukta tek tırnak işaretleri kullanın. Bir Windows komut kabuğu (cmd.exe) kullanıyorsanız, yol bağımsız değişkenlerini tek tırnak (' ') yerine çift tırnak işareti ("") içine alın.

#### <a name="specify-multiple-complete-blob-names"></a>Birden çok tamamen blob adı belirtin

Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--include-path` . Tek blob adlarını bir semicolin () kullanarak ayırın `;` .

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>'  --include-path <semicolon-separated-file-list>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt' --recursive` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-path 'photos;documents\myFile.txt'--recursive` |

Bu örnekte, AzCopy `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` dizini ve `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/documents/myFile.txt` dosyayı aktarır. `--recursive`Dizindeki tüm Blobları aktarma seçeneğini ekleyin `https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/photos` .

Ayrıca, seçeneğini kullanarak blob 'ları hariç bırakabilirsiniz `--exclude-path` . Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

#### <a name="use-wildcard-characters"></a>Joker karakter kullan

Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--include-pattern` . Joker karakterleri içeren kısmi adlar belirtin. Bir semicolin () kullanarak adları ayırın `;` .

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>' '<local-directory-path>' --include-pattern <semicolon-separated-file-list-with-wildcard-characters>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory' 'C:\myDirectory'  --include-pattern 'myFile*.txt;*.pdf*'` |

Ayrıca, seçeneğini kullanarak blob 'ları hariç bırakabilirsiniz `--exclude-pattern` . Daha fazla bilgi edinmek için bkz. [AzCopy kopyalama](storage-ref-azcopy-copy.md) başvuru belgeleri.

`--include-pattern`Ve `--exclude-pattern` seçenekleri, yalnızca blob adları için geçerlidir, yola değildir.  Bir dizin ağacında var olan tüm metin dosyalarını (Blobları) kopyalamak istiyorsanız, tüm `–recursive` dizin ağacını almak için seçeneğini kullanın ve ardından öğesini kullanarak `–include-pattern` `*.txt` tüm metin dosyalarını alın.

#### <a name="download-blobs-that-were-modified-after-a-date-and-time"></a>Tarih ve saatten sonra değiştirilen Blobları indirin 

Seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--include-after` . ISO-8601 biçiminde bir tarih ve saat belirtin (örneğin: `2020-08-19T15:04:00Z` ). 

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-or-directory-name>/*' '<local-directory-path>' --include-after <Date-Time-in-ISO-8601-format>` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/FileDirectory/*' 'C:\myDirectory'  --include-after '2020-08-19T15:04:00Z'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/FileDirectory/*' 'C:\myDirectory'  --include-after '2020-08-19T15:04:00Z'` |

Ayrıntılı başvuru için bkz. [AzCopy kopyası](storage-ref-azcopy-copy.md) başvuru belgeleri.

#### <a name="download-previous-versions-of-a-blob"></a>Bir Blobun önceki sürümlerini indirin

[BLOB sürüm oluşturmayı](../blobs/versioning-enable.md)etkinleştirdiyseniz, bir Blobun bir veya daha fazla önceki sürümünü indirebilirsiniz. 

İlk olarak, [Sürüm kimliklerinin](../blobs/versioning-overview.md)bir listesini içeren bir metin dosyası oluşturun. Her sürüm KIMLIĞI ayrı bir satırda görünmelidir. Örnek: 

```
2020-08-17T05:50:34.2199403Z
2020-08-17T05:50:34.5041365Z
2020-08-17T05:50:36.7607103Z
```

Sonra, seçeneğiyle [AzCopy kopyalama](storage-ref-azcopy-copy.md) komutunu kullanın `--list-of-versions` . Sürüm listesini içeren metin dosyasının konumunu belirtin (örneğin: `D:\\list-of-versions.txt` ).  

#### <a name="download-a-blob-snapshot"></a>Blob anlık görüntüsü indirin

Blob anlık görüntüsünün **Tarih saat** değerine başvurarak bir [BLOB anlık görüntüsü](/azure/storage/blobs/snapshots-overview.md) indirebilirsiniz. 

|    |     |
|--------|-----------|
| **Sözdizimi** | `azcopy copy 'https://<storage-account-name>.<blob or dfs>.core.windows.net/<container-name>/<blob-path>?sharesnapshot=<DateTime-of-snapshot>' '<local-file-path>'` |
| **Örnek** | `azcopy copy 'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sharesnapshot=2020-09-23T08:21:07.0000000Z' 'C:\myDirectory\myTextFile.txt'` |
| **Örnek** (hiyerarşik ad alanı) | `azcopy copy 'https://mystorageaccount.dfs.core.windows.net/mycontainer/myTextFile.txt?sharesnapshot=2020-09-23T08:21:07.0000000Z' 'C:\myDirectory\myTextFile.txt'` |

> [!NOTE]
> Blob verilerine erişim yetkisi vermek için SAS belirteci kullanıyorsanız, SAS belirtecinden sonra anlık görüntü **tarih saati** ekleyin. Örneğin: `'https://mystorageaccount.blob.core.windows.net/mycontainer/myTextFile.txt?sv=2018-03-28&ss=bjqt&srs=sco&sp=rjklhjup&se=2019-05-10T04:37:48Z&st=2019-05-09T20:37:48Z&spr=https&sig=%2FSOVEFfsKDqRry4bk3qz1vAQFwY5DDzp2%2B%2F3Eykf%2FJLs%3D&sharesnapshot=2020-09-23T08:21:07.0000000Z'`.

## <a name="download-with-optional-flags"></a>İsteğe bağlı bayraklar ile indir

İsteğe bağlı bayraklar kullanarak indirme işleminizi ince ayar yapabilirsiniz. İşte birkaç örnek.

|Senaryo|Bayrak|
|---|---|
|Dosyaları otomatik olarak aç.|**--sıkıştırmayı aç**|
|Kopyalama ile ilgili günlük girdilerinizi ne kadar ayrıntılı olarak istediğinizi belirtin.|**--günlük düzeyi** = \[ Uyarı \| hatası \| bilgisi \| yok\]|
|Hedefte çakışan dosyaların ve Blobların üzerine yazılıp yazılmayacağını belirtin.|**--üzerine yaz** = \[ doğru \| yanlış \| ıfsourcenewer \| istemi\]|

Tüm liste için bkz. [Seçenekler](storage-ref-azcopy-copy.md#options).

## <a name="next-steps"></a>Sonraki adımlar

Bu makalelerde daha fazla örnek bulun:

- [Örnekler: karşıya yükle](storage-use-azcopy-blobs-upload.md)
- [Örnekler: hesap arasında kopyalama](storage-use-azcopy-blobs-copy.md)
- [Örnekler: Synchronize](storage-use-azcopy-blobs-synchronize.md)
- [Örnekler: Amazon S3 demetleri](storage-use-azcopy-s3.md)
- [Örnekler: Azure dosyaları](storage-use-azcopy-files.md)
- [Öğretici: AzCopy kullanarak şirket içi verileri bulut depolamasına taşıma](storage-use-azcopy-migrate-on-premises-data.md)
- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)