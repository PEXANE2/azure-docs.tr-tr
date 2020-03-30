---
title: azcopy listesi | Microsoft Dokümanlar
description: Bu makalede, azcopy listesi komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: f02c1afadf18a7d3170eb178696487464e4a0bd3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034087"
---
# <a name="azcopy-list"></a>azcopy listesi

Belirli bir kaynaktaki varlıkları listeler.

## <a name="synopsis"></a>Özet

Geçerli sürümde yalnızca Blob kapsayıcıları desteklenir.

```azcopy
azcopy list [containerURL] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="examples"></a>Örnekler

```azcopy
azcopy list [containerURL]
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h, --yardım|Liste komutu için yardım içeriğini göster.|
|--makine tarafından okunabilir|Dosya boyutlarını baytlarla listeler.|
|--mega birimler|Birimleri 1024 yerine 1000 sırada görüntüler.|
|--çalışan-tally|Toplam dosya sayısını ve boyutlarını sayar.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
