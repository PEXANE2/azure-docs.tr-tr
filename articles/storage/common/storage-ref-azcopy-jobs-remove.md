---
title: azcopy işleri kaldırmak | Microsoft Dokümanlar
description: Bu makalede, azcopy işleri kaldırma komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: ba403c8d823b7ead0414521ebd51dc6f6601ccd8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034167"
---
# <a name="azcopy-jobs-remove"></a>azcopy işleri kaldırma

Verilen iş kimliğiyle ilişkili tüm dosyaları kaldırın.

> [!NOTE] 
> Günlük ve plan dosyalarının kaydedildiği konumu özelleştirebilirsiniz. Daha fazla bilgi için [azcopy env](storage-ref-azcopy-env.md) komutuna bakın.

```
azcopy jobs remove [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="examples"></a>Örnekler

```
  azcopy jobs rm e52247de-0323-b14d-4cc8-76e0be2e2d44
```

## <a name="options"></a>Seçenekler

**-h, --yardım**                Kaldırmak için yardım.

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

**--kap-mbps uint32**      Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.

**--output türü** dize Komutun çıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer 'metin'dir. (varsayılan "metin")

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy işleri](storage-ref-azcopy-jobs.md)
