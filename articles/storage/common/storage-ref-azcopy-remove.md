---
title: azcopy kaldırmak | Microsoft Dokümanlar
description: Bu makalede, azcopy remove komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: abce1acb88e920c0de7bbb6447ec9d838f10486c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033993"
---
# <a name="azcopy-remove"></a>azcopy kaldırma

Bir Azure depolama hesabından lekeleri veya dosyaları silin.

## <a name="synopsis"></a>Özet

```azcopy
azcopy remove [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="examples"></a>Örnekler

SAS ile tek bir lekeyi kaldırın:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

SAS ile tüm sanal dizini kaldırın:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Sanal dizin içindeki yalnızca üst teki lekeleri kaldırın, ancak alt dizinlerini kaldırın:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Sanal dizindeki lekelerin bir alt kümesini kaldırın (Örneğin: yalnızca jpg ve pdf dosyaları veya blob adı "exactName" ise):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --include="*.jpg;*.pdf;exactName"
```

Sanal dizinin tamamını kaldırın, ancak belirli lekeleri kapsamdan hariç tutar (Örneğin: foo ile başlayan veya çubukla biten her blob):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --exclude="foo*;*bar"
```

Bir dosyaya göreli yollarını (NOT URL kodlu) koyarak belirli lekeleri ve sanal dizinleri kaldırın:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/parent/dir]" --recursive=true --list-of-files=/usr/bar/list.txt
file content:
  dir1/dir2
  blob1
  blob2

```

Hiyerarşik ad alanı olan blob depolama hesabından tek bir dosyayı kaldırın (desteklenmeyen dahil/hariç tut).

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/file]?[SAS]"
```

Hiyerarşik ad alanı olan tek bir dizin blob depolama hesabı kaldırın (desteklenmeyen dahil/hariç):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

## <a name="options"></a>Seçenekler

**--exclude-yol dizesi**      Kaldırırken bu yolları hariç tinler. Bu seçenek joker karakter (*) desteklemez. Göreli yol önekini denetler. Örneğin: myFolder;myFolder/subDirName/file.pdf.

**--desen dizesini dışla,** adın desen listesiyle eşleştiği dosyaları hariç tut. Örneğin: *.jpg;*. pdf;exactName

**-h, --kaldırmak** için yardım

**--include-path** string Kaldırırken yalnızca bu yolları ekleyin. Bu seçenek joker karakter (*) desteklemez. Göreli yol önekini denetler. Örneğin: myFolder;myFolder/subDirName/file.pdf

**--include-desen dizesi** Yalnızca adın desen listesiyle eşleştiği dosyaları ekleyin. Örneğin: *.jpg;*. pdf;exactName

**--dosya listesi** dizesi Silinecek dosya ve dizinlistesini içeren bir dosyanın konumunu tanımlar. Göreli yollar satır sonları ile sınırlandırılmalı ve yollar URL kodlanmamalıdır.

**--log-level** string Günlük dosyasının günlük ayrıntılılığını tanımlayın. Kullanılabilir düzeyler şunlardır: INFO(tüm istek/yanıtlar), WARNING(yavaş yanıtlar), ERROR(yalnızca başarısız istekler) ve NONE (çıkış günlüğü yok). (varsayılan 'BİlGİ') (varsayılan "BİlGİ")

**--özyinelemeli**                Dizinler arasında eşitleme yaparken alt dizinlere özyinelemeli olarak bakın.

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
