---
title: azcopy işleri temiz | Microsoft Dokümanlar
description: Bu makalede, azcopy işleri temiz komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 7ae14c3606dfe6bffa8481682843f3f2e85c2131
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033715"
---
# <a name="azcopy-jobs-clean"></a>azcopy işleri temizleme

Tüm işler için tüm günlük ve plan dosyalarını kaldırma

```
azcopy jobs clean [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="examples"></a>Örnekler

```
  azcopy jobs clean --with-status=completed
```

## <a name="options"></a>Seçenekler

**-h, --yardım**                Temizlik için yardım.

**--status** string Yalnızca bu durumla ilgili işleri kaldırın, kullanılabilir değerler: İptal edildi, Tamamlandı, Başarısız Oldu, Tüm (varsayılan "Tümü")

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

**--kap-mbps uint32**      Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.

**--output türü** dize Komutun çıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer 'metin'dir. (varsayılan "metin")

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy işleri](storage-ref-azcopy-jobs.md)
