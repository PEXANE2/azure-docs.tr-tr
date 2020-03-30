---
title: azcopy env | Microsoft Dokümanlar
description: Bu makalede, azcopy env komutu için referans bilgileri verilmektedir.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: cc0ed5f1eec76bedc21106c90e5e82332e27ce3c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74033756"
---
# <a name="azcopy-env"></a>azcopy ortamı

AzCopy'nin davranışını yapılandırabilen ortam değişkenlerini gösterir.

## <a name="synopsis"></a>Özet

```azcopy
azcopy env [flags]
```

> [!IMPORTANT]
> Komut satırını kullanarak bir ortam değişkeni ayarlarsanız, bu değişken komut satırı geçmişinizde okunabilir. Komut satırı geçmişinizden kimlik bilgilerini içeren değişkenleri temizlemeyi düşünün. Değişkenlerin geçmişinizde görünmesini engellemek için, kullanıcıdan kimlik bilgilerini almak ve ortam değişkenini ayarlamak için bir komut dosyası kullanabilirsiniz.

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve Blob depolama ile veri aktarımı](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarımı](storage-use-azcopy-files.md)
- [AzCopy'i yapılandırma, en iyi duruma getirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h, --yardım|Env komutu için yardım içeriğini gösterir. |
|--gösteriye duyarlı|Hassas/gizli ortam değişkenlerini gösterir.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--kap-mbps uint32|Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.|
|--çıkış türü dize|Komutçıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer "metin"dir.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
