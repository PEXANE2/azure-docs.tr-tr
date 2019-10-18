---
title: AzCopy işleri göster | Microsoft Docs
description: Bu makale, AzCopy işleri göster komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 42b8bde9adb6980ff2c7004d43b02fc1fdc38363
ms.sourcegitcommit: 12de9c927bc63868168056c39ccaa16d44cdc646
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/17/2019
ms.locfileid: "72513461"
---
# <a name="azcopy-jobs-show"></a>azcopy işleri gösterme

Verilen iş KIMLIĞI için ayrıntılı bilgileri gösterir.

## <a name="synopsis"></a>Özeti

Yalnızca iş KIMLIĞI bayrak olmadan sağlanırsa, işin ilerleme Özeti döndürülür.

Bu komutu çalıştırdığınızda görüntülenen bayt sayısı ve tamamlanma yüzdesi, yalnızca işte tamamlanan dosyaları yansıtır. Kısmen tamamlanmış dosyaları yansıtmaz.

@No__t_0 bayrağı ayarlandıysa, söz konusu değere sahip iş içindeki aktarımların listesi görüntülenir.

```azcopy
azcopy jobs show [jobID] [flags]
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|Göster komutu için yardım içeriğini gösterir.|
|--durum dizesi|Yalnızca bu duruma sahip iş aktarımlarını Listele, kullanılabilir değerler: başlatıldı, başarılı, başarısız oldu|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy işleri](storage-ref-azcopy-jobs.md)
