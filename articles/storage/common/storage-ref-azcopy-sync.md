---
title: AzCopy eşitleme | Microsoft Docs
description: Bu makale AzCopy Sync komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 08/26/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: fb6c3b711a89ae7e4ef403a75927c4c6172523d0
ms.sourcegitcommit: 532335f703ac7f6e1d2cc1b155c69fc258816ede
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70195769"
---
# <a name="azcopy-sync"></a>AzCopy eşitleme

Kaynak konumunu hedef konuma çoğaltır.

## <a name="synopsis"></a>Özeti

Son değiştirme zamanları karşılaştırma için kullanılır. Hedefteki son değiştirilme zamanı daha yeni ise dosya atlanır.

Desteklenen çiftler şunlardır:

- Yerel < > Azure Blobu (SAS veya OAuth kimlik doğrulaması kullanılabilir)

Eşitle komutu kopyalama komutundan farklı yollarla farklılık gösterir:

  1. Özyinelemeli bayrak varsayılan olarak açık olur.
  2. Kaynak ve hedef desenler içermemelidir (* veya? gibi).
  3. Include ve exclude bayrakları, dosya adlarıyla eşleşen desenlerin bir listesi olabilir. Lütfen çizim için örnek bölümüne bakın.
  4. Hedefte mevcut olmayan dosya veya blob 'lar varsa, kullanıcıdan onları silmesi istenir.

     Bu istem, silme sorusuna otomatik olarak yanıt vermek için karşılık gelen bayraklar kullanılarak susuyor olabilir.

### <a name="advanced"></a>Gelişmiş

AzCopy, dosya uzantısına veya içeriğe (uzantı belirtilmediyse) göre yerel diskten karşıya yüklenirken dosyaların içerik türünü otomatik olarak algılar.

Yerleşik arama tablosu küçüktür, ancak UNIX üzerinde, bu adlardan biri veya birkaçı altında varsa, yerel sistemin MIME. Types dosyaları tarafından Genişletilebilir:

- /etc/Mime.exe
- /etc/apache2/mime.types
- /etc/apache/mime.types

Windows 'da MIME türleri kayıt defterinden ayıklanır.

```azcopy
azcopy sync [flags]
```

## <a name="examples"></a>Örnekler

Tek bir dosyayı eşitleyin:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]"
```

Yukarıdaki gibi, ancak bu kez dosya içeriğinin MD5 karmasını de hesaplar ve BLOB 'un Content-MD5 özelliği olarak kaydeder:

```azcopy
azcopy sync "/path/to/file.txt" "https://[account].blob.core.windows.net/[container]/[path/to/blob]" --put-md5
```

Tüm dizini alt dizinleriyle birlikte eşitleyin (özyinelemeli, varsayılan olarak açık olduğunu unutmayın):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]"
```

veya

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --put-md5
```

Yalnızca bir dizinin içindeki en üstteki dosyaları eşitleyin, ancak alt dizinlerini değil:

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --recursive=false
```

Dizindeki dosyaların bir alt kümesini eşitleyin (örneğin: yalnızca jpg ve PDF dosyaları Için veya dosya adı "exactName" ise):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --include="*.jpg;*.pdf;exactName"
```

Bir dizinin tamamını eşitleyin, ancak belirli dosyaları kapsamdan dışlayın (örneğin: Foo ile başlayan her dosya ve çubukla biter):

```azcopy
azcopy sync "/path/to/dir" "https://[account].blob.core.windows.net/[container]/[path/to/virtual/dir]" --exclude="foo*;*bar"
```

> [!NOTE]
> dahil etme/hariç tutma bayrakları birlikte kullanılırsa, yalnızca içerme desenleriyle eşleşen dosyalar aranabilir, ancak dışlama desenleriyle eşleşen dosyalar her zaman yok sayılır.

## <a name="options"></a>Seçenekler

|Seçenek|Açıklama|
|--|--|
|--blok-boyut-MB float|Azure depolama 'ya yükleme yaparken veya Azure Storage 'dan indirerek bu blok boyutunu (MIB 'de belirtilmiştir) kullanın. Varsayılan değer, dosya boyutuna göre otomatik olarak hesaplanır. Ondalık kesirlere izin verilir (örneğin: 0,25).|
|--Check-MD5 dizesi|İndirme sırasında ne kadar tam MD5 karmalarının doğrulanması gerektiğini belirtir. Bu seçenek yalnızca indirme sırasında kullanılabilir. Kullanılabilir değerler şunlardır: NoCheck, LogOnly, Failiffarklı, Failiffarklıentormissing. (varsayılan "Failiffarklı").|
|--Sil-hedef dize|kaynak üzerinde mevcut olmayan hedefteki ek dosyaların silinip oluşturulmayacağını tanımlar. True, false veya Prompt olarak ayarlanabilir. Sor olarak ayarlandıysa, kullanıcıya dosyaları ve Blobları silmeye başlamadan önce bir soru sorulur. (varsayılan "false").|
|--dizeyi Dışla|Adın, model listesiyle eşleştiği dosyaları dışlayın. Örneğin: *. jpg;* . PDF; exactName.|
|-h,--yardım|Eşitleme komutu için yardım içeriğini göster.|
|--Include dizesi|Yalnızca adın, model listesiyle eşleştiği dosyaları ekleyin. Örneğin: *. jpg;* . PDF; exactName.|
|--günlük düzeyi dize|Kullanılabilir düzeyler için günlük dosyası ayrıntı düzeyini tanımlayın: BILGI (tüm istekler/yanıtlar), uyarı (yavaş yanıtlar), hata (yalnızca başarısız istekler) ve HIÇBIRI (çıktı günlüğü yok). (varsayılan "BILGI").|
|--Put-MD5|Her bir dosyanın MD5 karmasını oluşturun ve karmayı hedef Blobun veya dosyanın Content-MD5 özelliği olarak kaydedin. (Varsayılan olarak, karma oluşturulmaz.) Yalnızca karşıya yükleme sırasında kullanılabilir.|
|--özyinelemeli|Varsayılan olarak, dizinler arasında eşitleme yaparken alt dizinlere yinelemeli olarak bakın. (varsayılan doğru).|

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
