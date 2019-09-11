---
title: AzCopy işleri | Microsoft Docs
description: Bu makale, AzCopy işleri komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: bd5469691f8d859bfcbca7479cdcb2e468a94e26
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195847"
---
# <a name="azcopy-jobs"></a>AzCopy işleri

İşlerin yönetimiyle ilgili alt komutlar.

## <a name="examples"></a>Örnekler

```azcopy
azcopy jobs show [jobID]
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|İşler komutu için yardım içeriğini göster.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
- [AzCopy işleri listesi](storage-ref-azcopy-jobs-list.md)
- [AzCopy işleri özgeçmişi](storage-ref-azcopy-jobs-resume.md)
- [AzCopy işlerinin gösterilmesi](storage-ref-azcopy-jobs-show.md)
