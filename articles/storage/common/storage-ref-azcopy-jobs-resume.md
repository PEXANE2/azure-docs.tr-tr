---
title: AzCopy işleri özgeçmişi | Microsoft Docs
description: Bu makale, AzCopy işleri özgeçmişi komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: dd25bec04d651c01d622f0652a29a65069421786
ms.sourcegitcommit: dccb85aed33d9251048024faf7ef23c94d695145
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/28/2020
ms.locfileid: "87281965"
---
# <a name="azcopy-jobs-resume"></a>azcopy işleri sürdürme

Verilen iş KIMLIĞI ile mevcut işi sürdürür.

## <a name="synopsis"></a>Özeti

```azcopy
azcopy jobs resume [jobID] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve BLOB Storage ile veri aktarma](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile verileri aktarma](storage-use-azcopy-files.md)
- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--hedef-SAS dizesi|Verilen Iş KIMLIĞI için hedefin hedef SAS 'si.|
|--dizeyi Dışla|Filtre: iş sürdürülürken bu başarısız olan aktarım (ler) i hariç tut. Dosyalar '; ' ile ayrılmalıdır.|
|-h,--yardım|Özgeçmişi komutu için yardım içeriğini göster.|
|--Include dizesi|Filtre: iş sürdürülürken yalnızca bu başarısız aktarmalar dahil edin. Dosyalar '; ' ile ayrılmalıdır.|
|--Kaynak-SAS dizesi |verilen Iş KIMLIĞI için kaynağın kaynak SAS 'si.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps float|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|
|--Güvenilen-Microsoft-sonekler dizesi   |Azure Active Directory oturum açma belirteçlerinin gönderilebileceği ek etki alanı soneklerini belirtir.  Varsayılan değer '*. Core.Windows.net;*' dir. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net '. Burada listelenenler varsayılan olarak eklenir. Güvenlik için yalnızca Microsoft Azure etki alanlarını Buraya yerleştirmeniz gerekir. Birden çok girişi noktalı virgülle ayırın.|

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy işleri](storage-ref-azcopy-jobs.md)
