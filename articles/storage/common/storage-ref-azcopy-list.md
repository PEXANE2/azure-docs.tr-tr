---
title: AzCopy listesi | Microsoft Docs
description: Bu makale, AzCopy list komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 8c40241c49917c6db6663f346aed0ec5d3b96be7
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195951"
---
# <a name="azcopy-list"></a>AzCopy listesi

Belirli bir kaynaktaki varlıkları listeler.

## <a name="synopsis"></a>Özeti

Geçerli sürümde yalnızca blob kapsayıcıları desteklenir.

```azcopy
azcopy list [containerURL] [flags]
```

## <a name="examples"></a>Örnekler

```azcopy
azcopy list [containerURL]
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|Liste komutu için yardım içeriğini göster.|
|--Makine-okunabilir|Dosya boyutlarını bayt olarak listeler.|
|--Mega-birimleri|Birimleri 1000 1024 değil siparişlerinde görüntüler.|
|--çalışma-döküm|Toplam dosya sayısını ve boyutlarını sayar.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
