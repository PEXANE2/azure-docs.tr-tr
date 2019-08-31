---
title: AzCopy oluştur | Microsoft Docs
description: Bu makale, AzCopy Make komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 9afcd8de1af42424649dd8e44fc07f7bfd881257
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195717"
---
# <a name="azcopy-make"></a>AzCopy oluştur

Bir kapsayıcı veya dosya paylaşma oluşturur.

## <a name="synopsis"></a>Özeti

Verilen kaynak URL 'SI ile temsil edilen bir kapsayıcı veya dosya paylaşma oluşturun.

```azcopy
azcopy make [resourceURL] [flags]
```

## <a name="examples"></a>Örnekler

```azcopy
azcopy make "https://[account-name].[blob,file,dfs].core.windows.net/[top-level-resource-name]"
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|Make komutu için yardım içeriğini göster. |
|--Kota-GB uint32|Paylaşımın en büyük boyutunu gigabayt (GiB) cinsinden belirtir, 0, dosya hizmetinin varsayılan kotasını kabul ettiğiniz anlamına gelir.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
