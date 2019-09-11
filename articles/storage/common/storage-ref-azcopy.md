---
title: AzCopy | Microsoft Docs
description: Bu makale AzCopy komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 4da9206f4500941179d781a0fe2a57ad15d7393d
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195899"
---
# <a name="azcopy"></a>AzCopy

AzCopy, Azure depolama 'ya ve dışına veri taşınan bir komut satırı aracıdır.

## <a name="synopsis"></a>Özeti

Komutların genel biçimi: `azcopy [command] [arguments] --[flag-name]=[flag-value]`.

Sorunları bildirmek veya araç hakkında daha fazla bilgi edinmek için bkz [https://github.com/Azure/azure-storage-azcopy](https://github.com/Azure/azure-storage-azcopy).

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|-h,--yardım|AzCopy için yardım içeriğini gösterir.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy ile çalışmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy kopyası](storage-ref-azcopy-copy.md)
- [AzCopy belgesi](storage-ref-azcopy-doc.md)
- [AzCopy env](storage-ref-azcopy-env.md)
- [AzCopy işleri](storage-ref-azcopy-jobs.md)
- [AzCopy listesi](storage-ref-azcopy-list.md)
- [AzCopy oturum açma](storage-ref-azcopy-login.md)
- [AzCopy oturumu kapatma](storage-ref-azcopy-logout.md)
- [AzCopy oluştur](storage-ref-azcopy-make.md)
- [AzCopy kaldır](storage-ref-azcopy-remove.md)
- [AzCopy eşitleme](storage-ref-azcopy-sync.md)
