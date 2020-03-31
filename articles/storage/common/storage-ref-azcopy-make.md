---
title: azcopy yapmak | Microsoft Dokümanlar
description: Bu makalede, azcopy make komutu için referans bilgileri verilmektedir.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 67d685684c5227377a0f8a7e822a06e785a69d89
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034038"
---
# <a name="azcopy-make"></a>azcopy oluşturma

Kapsayıcı veya dosya paylaşımı oluşturur.

## <a name="synopsis"></a>Özet

Verilen kaynak URL'si tarafından temsil edilen bir kapsayıcı veya dosya paylaşımı oluşturun.

```azcopy
azcopy make [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="examples"></a>Örnekler

```azcopy
azcopy make "https://[account-name].[blob,file,dfs].core.windows.net/[top-level-resource-name]"
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h, --yardım|Yap komutu için yardım içeriğini göster. |
|--kota-gb uint32|Gigabayt (GiB) içindeki paylaşımın maksimum boyutunu belirtir, 0 dosya hizmetinin varsayılan kotasını kabul ettiğiniz anlamına gelir.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
