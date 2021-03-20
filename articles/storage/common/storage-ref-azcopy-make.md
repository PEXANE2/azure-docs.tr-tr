---
title: AzCopy oluştur | Microsoft Docs
description: Bu makale, AzCopy Make komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 07/24/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: f6d9645e614730d2eb5fad8bca30486e9afef618
ms.sourcegitcommit: 910a1a38711966cb171050db245fc3b22abc8c5f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/19/2021
ms.locfileid: "98878350"
---
# <a name="azcopy-make"></a>azcopy oluşturma

Bir kapsayıcı veya dosya paylaşma oluşturur.

## <a name="synopsis"></a>Özeti

Verilen kaynak URL 'SI ile temsil edilen bir kapsayıcı veya dosya paylaşma oluşturun.

```azcopy
azcopy make [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve BLOB Storage ile veri aktarma](./storage-use-azcopy-v10.md#transfer-data)
- [AzCopy ve dosya depolama ile verileri aktarma](storage-use-azcopy-files.md)
- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)

## <a name="examples"></a>Örnekler

```azcopy
azcopy make "https://[account-name].[blob,file,dfs].core.windows.net/[top-level-resource-name]"
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|Make komutu için yardım içeriğini göster. |
|--Kota-GB uint32|Paylaşımın en büyük boyutunu gigabayt (GB) cinsinden belirtir, sıfır, dosya hizmetinin varsayılan kotasını kabul ettiğiniz anlamına gelir.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps float|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|
|--Güvenilen-Microsoft-sonekler dizesi   |Azure Active Directory oturum açma belirteçlerinin gönderilebileceği ek etki alanı soneklerini belirtir.  Varsayılan değer '*. Core.Windows.net;*' dir. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net '. Burada listelenenler varsayılan olarak eklenir. Güvenlik için yalnızca Microsoft Azure etki alanlarını Buraya yerleştirmeniz gerekir. Birden çok girişi noktalı virgülle ayırın.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
