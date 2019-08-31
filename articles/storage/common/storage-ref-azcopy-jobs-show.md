---
title: AzCopy işleri göster | Microsoft Docs
description: Bu makale, AzCopy işleri göster komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: d4369bd0c986ee20a0796436fea47509a711de4f
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195964"
---
# <a name="azcopy-jobs-show"></a>AzCopy işlerinin gösterilmesi

Verilen iş KIMLIĞI için ayrıntılı bilgileri gösterir.

## <a name="synopsis"></a>Özeti

Yalnızca iş KIMLIĞI bayrak olmadan sağlanırsa, işin ilerleme Özeti döndürülür.

`with-status` Bayrak ayarlandıysa, söz konusu değere sahip iş içindeki aktarımların listesi görüntülenir.

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|Göster komutu için yardım içeriğini gösterir.|
|--durum dizesi|Yalnızca bu duruma sahip iş aktarımlarını, kullanılabilir değerleri listeleyin: Başlatıldı, başarılı, başarısız|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy işleri](storage-ref-azcopy-jobs.md)
