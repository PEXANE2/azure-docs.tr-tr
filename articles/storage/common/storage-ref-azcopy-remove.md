---
title: AzCopy kaldır | Microsoft Docs
description: Bu makale AzCopy Remove komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 0cc366ab2cdad9c7258dca905d8f4a06472119fe
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195912"
---
# <a name="azcopy-remove"></a>AzCopy kaldır

Azure Depolama Blobu, dosya ve Azure Data Lake Storage 2. varlıkları siler.

## <a name="synopsis"></a>Özeti

```azcopy
azcopy remove [resourceURL] [flags]
```

## <a name="examples"></a>Örnekler

SAS içeren tek bir blobu kaldırma:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/blob]?[SAS]"
```

Bir sanal dizinin tamamını SAS ile kaldırın:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true
```

Yalnızca bir sanal dizin içindeki en üstteki Blobları Kaldır ancak alt dizinlerini kaldırın:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Bir sanal dizindeki Blobların bir alt kümesini kaldırın (örneğin: yalnızca jpg ve PDF dosyaları ya da blob adı "exactName" ise):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --include="*.jpg;*.pdf;exactName"
```

Sanal bir dizinin tamamını kaldırın, ancak belirli Blobları kapsamdan hariç tutun (örneğin: her blob, Foo ile başlayan veya çubukla bitiyor):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --exclude="foo*;*bar"
```

Tek bir dosyayı Data Lake Storage 2. Kaldır (dahil etme ve hariç tutma desteklenmiyor):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/file]?[SAS]"
```

Tek bir dizini Data Lake Storage 2. kaldırın (dahil etme ve hariç tutma desteklenmez):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--dizeyi Dışla|Adın, model listesiyle eşleştiği dosyaları dışlayın. Örneğin: *. jpg;* . PDF; exactName|
|-h,--yardım|Remove komutu için yardım içeriğini göster.|
|--Include dizesi|Yalnızca adın, model listesiyle eşleştiği dosyaları ekleyin. Örneğin: *. jpg;* . PDF; exactName|
|--günlük düzeyi dize|Günlük dosyası için günlük ayrıntı düzeyini tanımlayın. Kullanılabilir düzeyler şunları içerir: BILGI (tüm istekler/yanıtlar), uyarı (yavaş yanıtlar), hata (yalnızca başarısız istekler) ve HIÇBIRI (çıktı günlüğü yok). (varsayılan "BILGI")|
|--özyinelemeli|Dizinler arasında eşitleme yaparken alt dizinlere yinelemeli olarak bakın.|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
