---
title: azcopy işleri göstermek | Microsoft Dokümanlar
description: Bu makalede, azcopy işleri için referans bilgileri komutu gösterir.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 7b5f566757dd77a61f252b123d0c9c1b74303fbe
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74034129"
---
# <a name="azcopy-jobs-show"></a>azcopy işleri gösterme

Verilen iş kimliği için ayrıntılı bilgileri gösterir.

## <a name="synopsis"></a>Özet

Yalnızca iş kimliği bayrak olmadan sağlanırsa, işin ilerleme özeti döndürülür.

Bu komutu çalıştırdığınızda görünen bayt sayısı ve yüzde tam, yalnızca iş yerinde tamamlanan dosyaları yansıtır. Kısmen tamamlanmış dosyaları yansıtmaz.

`with-status` Bayrak ayarlanırsa, verilen değeri olan işteki aktarımlistesi gösterilir.

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h, --yardım|Gösteri komutu için yardım içeriğini gösterir.|
|--durum dizesi ile|Yalnızca bu durumla iş aktarımlarını listele, kullanılabilir değerleri: Başlangıç, Başarı, Başarısız|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy işleri](storage-ref-azcopy-jobs.md)
