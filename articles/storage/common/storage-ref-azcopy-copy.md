---
title: AzCopy kopyası | Microsoft Docs
description: Bu makale, AzCopy kopyalama komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: c15d188e333bea5e74fa65d2bbdf38ae7fadc246
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195743"
---
# <a name="azcopy-copy"></a>AzCopy kopyası

Kaynak verileri bir hedef konuma kopyalar.

## <a name="synopsis"></a>Özeti

Desteklenen yönergeler şunlardır:

- Yerel < > Azure Blobu (SAS veya OAuth kimlik doğrulaması)
- Yerel < > Azure dosyası (Share/Directory SAS kimlik doğrulaması)
- Yerel <-> ADLS Gen 2 (SAS, OAuth veya SharedKey kimlik doğrulaması)
- Azure Blob (SAS veya genel) <-> Azure Blob (SAS veya OAuth kimlik doğrulaması)
- Azure dosya (SAS)-> Azure Blok Blobu (SAS veya OAuth kimlik doğrulaması)
- AWS S3 (erişim anahtarı)-> Azure Blok Blobu (SAS veya OAuth kimlik doğrulaması)

Daha fazla bilgi için lütfen örneklere bakın.

### <a name="advanced"></a>Gelişmiş

AzCopy, dosya uzantısına veya içeriğe (uzantı belirtilmediyse) göre yerel diskten karşıya yüklenirken dosyaların içerik türünü otomatik olarak algılar.

Yerleşik arama tablosu küçüktür, ancak UNIX üzerinde, bu adlardan biri veya birkaçı altında varsa, yerel sistemin MIME. Types dosyaları tarafından Genişletilebilir:

- /etc/Mime.exe
- /etc/apache2/mime.types
- /etc/apache/mime.types

Windows 'da MIME türleri kayıt defterinden ayıklanır. Bu özellik bir bayrağın yardımıyla kapatılabilir. Lütfen bayrak bölümüne bakın.

> [!IMPORTANT]
> Bir ortam değişkenini komut satırını kullanarak ayarlarsanız, bu değişken komut satırı geçmişinizde okunabilir olacaktır. Komut satırı geçmişinizden kimlik bilgilerini içeren değişkenleri temizlemeyi göz önünde bulundurun. Değişkenlerin geçmişinizde görünmesini önlemek için, kullanıcıya kimlik bilgilerini istemek ve ortam değişkenini ayarlamak için bir komut dosyası kullanabilirsiniz.

```azcopy
azcopy copy [source] [destination] [flags]
```

## <a name="examples"></a>Örnekler

OAuth kimlik doğrulaması kullanarak tek bir dosyayı karşıya yükleyin.

AzCopy 'a henüz oturum açmadıysanız, aşağıdaki komutu çalıştırmadan önce lütfen `azcopy login` komutunu kullanın.

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Yukarıdaki gibi, ancak bu kez dosya içeriğinin MD5 karmasını de hesaplar ve BLOB 'un Content-MD5 özelliği olarak kaydeder:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

SAS ile tek bir dosyayı karşıya yükle:

```azcopy
azcopy cp "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Yöneltme kullanarak SAS ile tek bir dosya yükleme (yalnızca blok Blobları):

```azcopy
cat "/path/to/file.txt" | azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Tüm dizini SAS ile karşıya yükleyin:

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

veya

```azcopy
azcopy cp "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --put-md5
```

Joker karakterler kullanarak bir SAS içeren bir dosya kümesini karşıya yükleyin:

```azcopy
azcopy cp "/path/*foo/*bar/*.pdf" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

Joker karakterler kullanarak dosyaları ve dizinleri SAS ile karşıya yükleme:

```azcopy
azcopy cp "/path/*foo/*bar*" "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

OAuth kimlik doğrulaması kullanarak tek bir dosyayı indirin.

AzCopy 'a henüz oturum açmadıysanız, aşağıdaki komutu çalıştırmadan önce lütfen `azcopy login` komutunu kullanın.

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]" "/path/to/file.txt"
```

SAS içeren tek bir dosyayı indirme:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "/path/to/file.txt"
```

Yöneltme kullanarak SAS içeren tek bir dosyayı indirme (yalnızca blok Blobları):

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" > "/path/to/file.txt"
```

Bir SAS ile tüm dizini indirin:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "/path/to/dir" --recursive=true
```

Joker karakterler kullanarak SAS içeren bir dosya kümesi indirin:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/foo*?[SAS]" "/path/to/dir"
```

Joker karakterler kullanarak bir SAS ile dosya ve Dizin indirme:

```azcopy
azcopy cp "https://[account].blob.core.windows.net/[container]/foo*?[SAS]" "/path/to/dir" --recursive=true
```

SAS içeren tek bir blobu SAS içeren başka bir bloba kopyalama:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

SAS içeren tek bir Blobun OAuth belirtecini içeren başka bir bloba kopyalayın.

AzCopy 'a henüz oturum açmadıysanız, aşağıdaki komutu çalıştırmadan önce lütfen `azcopy login` komutunu kullanın. OAuth belirteci, hedef depolama hesabına erişmek için kullanılır.

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]"
```

SAS ile blob sanal dizininden bir dizinin tamamını SAS içeren başka bir blob sanal dizinine kopyalayın:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

SAS içeren blob hesabındaki tüm hesap verilerini SAS içeren başka bir blob hesabına kopyalayın:

```azcopy
azcopy cp "https://[srcaccount].blob.core.windows.net?[SAS]" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

S3 'ten Access Key 'e sahip tek bir nesneyi SAS içeren blob 'a kopyalayın:

S3 kaynağı için AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ortam değişkenini ayarlayın.

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[object]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Bir dizinin tamamını, S3 'den erişim anahtarına sahip blob sanal dizinine SAS ile kopyalayın:

```azcopy
azcopy cp "https://s3.amazonaws.com/[bucket]/[folder]" "https://[destaccount].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

S3 https://docs.aws.amazon.com/AmazonS3/latest/user-guide/using-folders.html için hangi [klasör] anlama geldiğini öğrenmek için bkz. S3 kaynağı için AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ortam değişkenini ayarlayın.

S3 hizmetindeki tüm demetleri, SAS içeren blob hesabına erişim anahtarı ile kopyalayın:

S3 kaynağı için AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ortam değişkenini ayarlayın.

```azcopy
azcopy cp "https://s3.amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

Bir S3 bölgesindeki tüm demetleri SAS içeren blob hesabına erişim anahtarı ile kopyalayın:

```azcopy
azcopy cp "https://s3-[region].amazonaws.com/" "https://[destaccount].blob.core.windows.net?[SAS]" --recursive=true
```

S3 kaynağı için AWS_ACCESS_KEY_ID ve AWS_SECRET_ACCESS_KEY ortam değişkenini ayarlayın.

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--blob türü dize|Hedefteki blob türünü tanımlar. Bu, Blobları karşıya yüklemek için ve hesaplar arasında kopyalama sırasında kullanılır (varsayılan "none").|
|--Block-blob-katmanlı dize|Blok Blobu bu blob katmanını kullanarak Azure depolama 'ya yükleyin. (varsayılan "none").|
|--blok-boyut-MB float |Azure depolama 'ya yükleme yaparken ve Azure depolama 'dan indirerek bu blok boyutunu (MIB 'de belirtilmiştir) kullanın. Varsayılan değer, dosya boyutuna göre otomatik olarak hesaplanır. Ondalık kesirlere izin verilir (örneğin: 0,25).|
|--Cache-Control dizesi|Cache-Control üst bilgisini ayarlayın. İndirilmek üzere döndürüldü.|
|--Check-MD5 dizesi|İndirme sırasında ne kadar tam MD5 karmalarının doğrulanması gerektiğini belirtir. Yalnızca indirme sırasında kullanılabilir. Kullanılabilir seçenekler: NoCheck, LogOnly, Failiffarklı, Failiffarklıentormissing. (varsayılan "Failiffarklı")|
|--Content-Disposition dizesi|Content-Disposition üst bilgisini ayarlayın. İndirilmek üzere döndürüldü.|
|--Content-Encoding dizesi|İçerik kodlama üst bilgisini ayarlayın. İndirilmek üzere döndürüldü.|
|--Content-Language dizesi|İçerik-dil üst bilgisini ayarlayın. İndirilmek üzere döndürüldü.|
|--Content-Type dizesi |Dosyanın içerik türünü belirtir. Tahmin yok-MIME türü anlamına gelir. İndirilmek üzere döndürüldü.|
|--dizeyi Dışla|Kopyalama sırasında bu dosyaları dışlayın. \* Kullanımını destekler.|
|--exclude-blob türü dize|İsteğe bağlı olarak, bloblardan veya hesaptan blob kopyalanırken hariç tutulacak blob türünü (BlockBlob/PageBlob/AppendBlob) belirtir. Bu bayrağın kullanılması, verileri Azure olmayan hizmetten hizmetten kopyalamak için geçerli değildir. Birden fazla BLOB '; ' ile ayrılmalıdır.|
|--takip-symlinks|Yerel dosya sisteminden karşıya yüklerken sembolik bağlantıları izleyin.|
|--noktadan dizeye|İsteğe bağlı olarak kaynak hedef birleşimini belirtir. Örneğin: LocalBlob, BlobLocal, LocalBlobFS.|
|-h,--yardım|Copy komutu için yardım içeriğini gösterir. |
|--günlük düzeyi dize|Kullanılabilir düzeyler için günlük dosyası ayrıntı düzeyini tanımlayın: BILGI (tüm istekler/yanıtlar), uyarı (yavaş yanıtlar), hata (yalnızca başarısız istekler) ve HIÇBIRI (çıktı günlüğü yok). (varsayılan "BILGI").|
|--meta veri dizesi|Bu anahtar-değer çiftleri ile Azure depolama 'ya meta veri olarak yükleyin.|
|--No-tahmin-MIME türü|AzCopy 'in, dosyanın uzantısına veya içeriğine göre içerik türünü algılamasını önler.|
|--üzerine yaz|Bu bayrak true olarak ayarlandıysa hedefteki çakışan dosyaların/Blobların üzerine yazın. (varsayılan doğru).|
|--Page-blob-katman dizesi |Bu blob katmanını kullanarak sayfa blobunu Azure depolama 'ya yükleyin. (varsayılan "none").|
|--Preserve-son değiştirilme zamanı|Yalnızca hedef dosya sistemi olduğunda kullanılabilir.|
|--Put-MD5|Her bir dosyanın MD5 karmasını oluşturun ve karmayı hedef Blobun veya dosyanın Content-MD5 özelliği olarak kaydedin. (Varsayılan olarak, karma oluşturulmaz.) Yalnızca karşıya yükleme sırasında kullanılabilir.|
|--özyinelemeli|Yerel dosya sisteminden karşıya yüklerken alt dizinlere yinelemeli olarak bakın.|
|--S2S-Algıla-kaynak-değiştirildi|Numaralandırdıktan sonra kaynağın değiştirilip değiştirilmediğini denetleyin. S2S kopyası için kaynak uzak bir kaynaktır ve kaynağın değişip değişmediğini doğrulamak için ek istek maliyetleri gerekir.|
|--S2S-tanıtıcı-geçersiz-meta veri dizesi |Geçersiz meta veri anahtarlarının nasıl işleneceğini belirtir. Kullanılabilir seçenekler: Excludeifgeçersiz, Failifgeçersiz, Renameifgeçersiz. (varsayılan "Excludeifgeçersiz").|
|--S2S-Preserve-Access-Tier|Hizmetten hizmete kopyalama sırasında erişim katmanını koruyun. Hedef depolama hesabının erişim katmanını ayarlamayı desteklediğinden emin olmak için lütfen [Azure Blob depolama: sık erişimli, seyrek erişimli ve arşiv erişim katmanlarını](../blobs/storage-blob-storage-tiers.md) inceleyin. Erişim katmanını ayarlamanın desteklenme durumlarında, lütfen s2sPreserveAccessTier = false kullanarak erişim katmanını kopyalamayı atlayın.  (varsayılan doğru).|
|--S2S-Preserve-Özellikler|Hizmetten hizmete kopyalama sırasında tam özellikleri koruyun. S3 ve Azure dosya için tek dosya kaynağı için, liste işlemi nesne ve dosyaların tam özelliklerini döndürmediğinden, tam özellikleri korumak için AzCopy 'ın nesne ve dosya başına bir ek istek gönderebilmesi gerekir. (varsayılan doğru).|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
