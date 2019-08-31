---
title: AzCopy işleri özgeçmişi | Microsoft Docs
description: Bu makale, AzCopy işleri özgeçmişi komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 8786da42f000a2f13279499159a7af424aa10748
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195795"
---
# <a name="azcopy-jobs-resume"></a>AzCopy işleri özgeçmişi

Verilen iş KIMLIĞI ile mevcut işi sürdürür.

## <a name="synopsis"></a>Özeti

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--hedef-SAS dizesi|Verilen iş kimliği için hedefin hedef SAS 'ları.|
|--dizeyi Dışla|Filtreyle İşi sürdürülürken bu başarısız olan aktarım (ler) i hariç tutun. Dosyalar '; ' ile ayrılmalıdır.|
|-h,--yardım|Özgeçmişi komutu için yardım içeriğini göster.|
|--Include dizesi|Filtre: iş sürdürülürken yalnızca bu başarısız aktarmalar dahil edin. Dosyalar '; ' ile ayrılmalıdır.|
|--Kaynak-SAS dizesi |verilen iş kimliği için kaynağın kaynak SAS 'ları.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy işleri](storage-ref-azcopy-jobs.md)
