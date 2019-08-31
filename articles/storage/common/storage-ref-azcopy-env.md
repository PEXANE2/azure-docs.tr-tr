---
title: AzCopy env | Microsoft Docs
description: Bu makale AzCopy env komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 15e72493190e1bc56e779c22695bc51bd05da940
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195782"
---
# <a name="azcopy-env"></a>AzCopy env

AzCopy 'in davranışını yapılandırabileceği ortam değişkenlerini gösterir.

## <a name="synopsis"></a>Özeti

```azcopy
azcopy env [flags]
```

> [!IMPORTANT]
> Bir ortam değişkenini komut satırını kullanarak ayarlarsanız, bu değişken komut satırı geçmişinizde okunabilir olacaktır. Komut satırı geçmişinizden kimlik bilgilerini içeren değişkenleri temizlemeyi göz önünde bulundurun. Değişkenlerin geçmişinizde görünmesini önlemek için, kullanıcıya kimlik bilgilerini istemek ve ortam değişkenini ayarlamak için bir komut dosyası kullanabilirsiniz.

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|-h,--yardım|Env komutu için yardım içeriğini gösterir. |
|--hassas göster|Gizli/gizli ortam değişkenlerini gösterir.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
