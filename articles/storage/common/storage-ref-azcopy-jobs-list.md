---
title: AzCopy işleri listesi | Microsoft Docs
description: Bu makale, AzCopy işleri listesi komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 50af05c7d59b9d25e0f97ad9bdee73d12848dc3a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "98878265"
---
# <a name="azcopy-jobs-list"></a>azcopy iş listesi

Tüm işlere ilişkin bilgileri görüntüler.

## <a name="synopsis"></a>Özeti

```azcopy
azcopy jobs list [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve BLOB Storage ile veri aktarma](./storage-use-azcopy-v10.md#transfer-data)
- [AzCopy ve dosya depolama ile verileri aktarma](storage-use-azcopy-files.md)
- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|Liste komutu için yardım içeriğini göster.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps float|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|
|--Güvenilen-Microsoft-sonekler dizesi   | Azure Active Directory oturum açma belirteçlerinin gönderilebileceği ek etki alanı soneklerini belirtir.  Varsayılan değer '*. Core.Windows.net;*' dir. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net '. Burada listelenenler varsayılan olarak eklenir. Güvenlik için yalnızca Microsoft Azure etki alanlarını Buraya yerleştirmeniz gerekir. Birden çok girişi noktalı virgülle ayırın.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy işleri](storage-ref-azcopy-jobs.md)
