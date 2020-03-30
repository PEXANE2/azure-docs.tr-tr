---
title: azcopy işleri devam | Microsoft Dokümanlar
description: Bu makalede, azcopy işleri devam komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 3ee7879475801660b5200dddca88a0a81b2b6b9b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034153"
---
# <a name="azcopy-jobs-resume"></a>azcopy işleri sürdürme

Verilen iş kimliğiyle varolan işi devam ettirer.

## <a name="synopsis"></a>Özet

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--hedef-sas dizesi|Verilen JobId için hedef In Hedef SAS.|
|--dize hariç|Filtre: İşe devam ederken bu başarısız aktarım(lar)ı hariç tinleyin. Dosyalar ';' ile ayrılmalıdır.|
|-h, --yardım|Özgeçmiş komutu için yardım içeriğini göster.|
|--dize dahil|Filtre: yalnızca işe devam ederken bu başarısız aktarım(lar) ekleyin. Dosyalar ';' ile ayrılmalıdır.|
|--kaynak-sas dizesi |verilen JobId için kaynak SAS.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy işleri](storage-ref-azcopy-jobs.md)
