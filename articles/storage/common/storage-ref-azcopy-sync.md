---
title: azcopy senkronizasyon | Microsoft Dokümanlar
description: Bu makalede, azcopy eşitleme komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 1bff46c8584934ab8bcffce74763edc8363533d6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "76988252"
---
# <a name="azcopy-sync"></a>azcopy eşitleme

Kaynak konumu hedef konuma kopyalar.

## <a name="synopsis"></a>Özet

Son değiştirilen saatler karşılaştırma için kullanılır. Hedefteki son değiştirilen süre daha yeniyse dosya atlanır.

Desteklenen çiftler şunlardır:

- yerel < > Azure Blob (SAS veya OAuth kimlik doğrulaması kullanılabilir)
- Azure Blob < > Azure Blob (Kaynak bir SAS içermelidir veya herkese açık; Hedef için SAS veya OAuth kimlik doğrulaması kullanılabilir)
- Azure Dosyası < > Azure Dosyası (Kaynak bir SAS içermelidir veya herkese açık olmalıdır; Hedef için SAS kimlik doğrulaması kullanılmalıdır)

Eşitleme komutu kopyalama komutundan çeşitli şekillerde farklıdır:

1. Varsayılan olarak, özyinelemeli bayrak doğrudur ve tüm alt dizinleri eşitler. Özyinelemeli bayrak yanlışsa, yalnızca dizin içindeki üst düzey dosyaları eşitleme.
2. Sanal dizinler arasında eşitleme yaparken, sanal dizinlerden biriyle aynı ada sahip bir leke varsa, yola bir çizgi ekleyin (örneklere bakın).
3. 'DeleteDestination' bayrağı doğru veya istemi olarak ayarlanmışsa, eşitleme kaynağında bulunmayan hedefteki dosyaları ve lekeleri siler.

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

### <a name="advanced"></a>Gelişmiş

Bir dosya uzantısı belirtmezseniz, AzCopy dosya uzantısıveya içeriğe bağlı olarak yerel diskten yükleme yaparken dosyaların içerik türünü otomatik olarak algılar (uzantı belirtilmemişse).

Yerleşik arama tablosu küçüktür, ancak Unix'te, bu adlardan biri veya birkaçı altında varsa yerel sistemin mim.types dosyası(lar) tarafından artırılır:

- /etc/mime.types
- /etc/apache2/mime.types
- /etc/apache/mime.types

Windows'da MIME türleri kayıt defterinden ayıklanır.

```azcopy
azcopy sync <source> <destination> [flags]
```

## <a name="examples"></a>Örnekler

Tek bir dosyayı eşitleme:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Yukarıdaki gibi, ama bu kez, aynı zamanda dosya içeriğinin MD5 karma hesap ve blob İçerik-MD5 özelliği olarak kaydedin:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Alt dizinlerini içeren tüm dizinleri eşitleme (özyinelemenin varsayılan olarak açık olduğunu unutmayın):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]"
```

or

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --put-md5
```

Yalnızca bir dizin içindeki en iyi dosyaları eşitleyin, ancak alt dizinlerini eşitlemeyin:

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Bir dizindeki dosyaların bir alt kümesini eşitleyin (Örneğin: yalnızca jpg ve pdf dosyaları veya dosya adı "exactName" ise):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --include="*.jpg;*.pdf;exactName"
```

Tüm dizineşitle, ancak belirli dosyaları kapsamdan hariç tut (Örneğin: foo ile başlayan veya çubukla biten her dosya):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --exclude="foo*;*bar"
```

Tek bir lekeyi eşitle:

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Sanal dizini eşitleme:

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=true
```

Bir blob ile aynı ada sahip sanal bir dizin eşitleme (ayrıştırmak için yola bir çizgi ekleme):

```azcopy
azcopy sync "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]/?[SAS]" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]/" --recursive=true
```

Azure Dosya dizini (Blob ile aynı sözdizimi):

```azcopy
azcopy sync "https://[account].file.core.windows.net/[share]/[path/to/dir]?[SAS]" "https://[account].file.core.windows.net/[share]/[path/to/dir]" --recursive=true
```

> [!NOTE]
> Ekle/hariç bayraklar birlikte kullanılırsa, yalnızca dahil desenleriyle eşleşen dosyalara bakılır, ancak dışlama desenleri eşleşen dosyalar her zaman yoksayılır.

## <a name="options"></a>Seçenekler

**--block-size-mb** float Azure Depolama'ya yüklerken veya Azure Depolama'dan indirirken bu blok boyutunu (MiB'de belirtilir) kullanın. Varsayılan, dosya boyutuna göre otomatik olarak hesaplanır. Ondalık kesirlere izin verilir (Örneğin: 0.25).

**--check-md5** dizesi İndirilirken MD5 hashes'in ne kadar kesin olarak doğrulanması gerektiğini belirtir. Bu seçenek yalnızca indirirken kullanılabilir. Kullanılabilir değerler şunlardır: NoCheck, LogOnly, FailIfDifferent, FailIfDifferentOrMissing. (varsayılan 'FailIfDifferent'). (varsayılan "FailIfDifferent")

**--delete-destination** string Kaynakta bulunmayan hedeften ek dosyaların silinip silinmeyeceğini tanımlar. Doğru, yanlış veya istemi ayarlanabilir. İstem için ayarlanırsa, kullanıcıya silme için dosya ve blobs zamanlamadan önce bir soru sorulur. (varsayılan 'false'). (varsayılan "false")

**--öznitelik dizesini dışla (Yalnızca** Windows) Öznitelikleri öznitelik listesiyle eşleşen dosyaları hariç tut. Örneğin: A; S; R

**--exclude-path** string Kopyalanırken bu yolları hariç tut. Bu seçenek joker karakter (*) desteklemez. Göreli yol önekini denetler (Örneğin: myFolder;myFolder/subDirName/file.pdf). Hesap geçişi ile birlikte kullanıldığında, yollar kapsayıcı adını içermez.

**--desen dizesini dışla,** adın desen listesiyle eşleştiği dosyaları hariç tut. Örneğin: \*.jpg; \*.pdf;exactName

**-h, --senkronizasyon** için yardım

**--include-öznitelikler** dizesi (yalnızca Windows) Yalnızca öznitelikleri öznitelik listesiyle eşleşen dosyaları ekleyin. Örneğin: A; S; R

**--include-desen dizesi** Yalnızca adın desen listesiyle eşleştiği dosyaları ekleyin. Örneğin: \*.jpg; \*.pdf;exactName

**--günlük düzeyi** dizesi Günlük dosyasının günlük ayrıntılılığını, kullanılabilir düzeyleri: INFO(tüm istek ve yanıtlar), UYARI(yavaş yanıtlar), HATA (yalnızca başarısız istekler) ve NONE(çıkış günlüğü yok) olarak tanımlayın. (varsayılan BİlGİ). (varsayılan "BİlGİ")

**--put-md5**                     Her dosyanın bir MD5 karma sını oluşturun ve karmayı hedef blob veya dosyanın İçerik-MD5 özelliği olarak kaydedin. (Varsayılan olarak karma oluşturulmadı.) Yalnızca yükleme yaparken kullanılabilir.

**--özyinelemeli**                   Varsayılan olarak doğru, dizinler arasında eşitleme yaparken alt dizinlere özyinelemeli olarak bakın. (varsayılan true). (varsayılan true)

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
