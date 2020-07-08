---
title: AzCopy kaldır | Microsoft Docs
description: Bu makale AzCopy Remove komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 05/04/2020
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: ab085b9a41120a9f56c1c2e39a89def8c3893747
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84221076"
---
# <a name="azcopy-remove"></a>azcopy kaldırma

Blob 'ları veya dosyaları bir Azure Storage hesabından silin.

## <a name="synopsis"></a>Özeti

```azcopy
azcopy remove [resourceURL] [flags]
```

## <a name="related-conceptual-articles"></a>İlgili kavramsal makaleler

- [AzCopy’yi kullanmaya başlama](storage-use-azcopy-v10.md)
- [AzCopy ve BLOB Storage ile veri aktarma](storage-use-azcopy-blobs.md)
- [AzCopy ve dosya depolama ile veri aktarma](storage-use-azcopy-files.md)
- [AzCopy 'i yapılandırma, iyileştirme ve sorun giderme](storage-use-azcopy-configure.md)

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
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --include-pattern="*.jpg;*.pdf;exactName"
```

Sanal bir dizinin tamamını kaldırın, ancak belirli Blobları kapsamdan hariç tutun (örneğin: her blob, Foo ile başlayan veya çubukla bitiyor):

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/directory]?[SAS]" --recursive=true --exclude-pattern="foo*;*bar"
```

Belirli blob 'ları ve sanal dizinleri, göreli yollarını (URL kodlamalı DEĞIL) bir dosyaya koyarak kaldırın:

```azcopy
azcopy rm "https://[account].blob.core.windows.net/[container]/[path/to/parent/dir]" --recursive=true --list-of-files=/usr/bar/list.txt
file content:
  dir1/dir2
  blob1
  blob2

```

Hiyerarşik ad alanı olan bir BLOB depolama hesabından tek bir dosyayı kaldırın (dahil etme/hariç tutma desteklenmiyor).

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/file]?[SAS]"
```

Hiyerarşik ad alanı olan bir BLOB depolama hesabı olan tek bir dizini kaldırma (dahil etme/hariç tutma desteklenmez):

```azcopy
azcopy rm "https://[account].dfs.core.windows.net/[container]/[path/to/directory]?[SAS]"
```

## <a name="options"></a>Seçenekler

**--Delete-Snapshot** String varsayılan olarak, blob 'un anlık görüntüleri varsa silme işlemi başarısız olur. Kök blobu ve tüm anlık görüntülerini kaldırmak için ' include ' belirtin; Alternatif olarak yalnızca anlık görüntüleri kaldırmak için ' yalnızca ', kök blobu tut ' i belirtin.

**--exclude-Path** dizesi kaldırılırken Bu yolları hariç tutar. Bu seçenek joker karakterleri (*) desteklemez. Göreli yol önekini denetler. Örneğin: myFolder; myFolder/subDirName/file.pdf.

**--exclude-model** dize, adın model listesiyle eşleştiği dosyaları hariç tutar. Örneğin: *. jpg;*. PDF; exactName

**--** -------salt-    Bir Azure dosyaları dosyası veya klasörü silinirken, mevcut nesnenin salt okunurdur özniteliği ayarlanmış olsa bile silme işlemini çalışmaya zorlayın

**-h,--** kaldırma için yardım yardımı

**--Include-Path** dize yalnızca kaldırılırken Bu yolları içerir. Bu seçenek joker karakterleri (*) desteklemez. Göreli yol önekini denetler. Örneğin: myFolder; myFolder/subDirName/file.pdf

**--Include-model** dize yalnızca adı, model listesiyle eşleşen dosyaları içerir. Örneğin: *. jpg;*. PDF; exactName

**--dosya listesi** dizesi, silinecek dosya ve dizinlerin listesini içeren bir dosyanın konumunu tanımlar. Göreli yollar satır sonları ile sınırlandırılmalıdır ve yolların URL kodlamalı olmaması gerekir.

**--günlük düzeyi** dize günlük dosyası için günlük ayrıntı düzeyini tanımlar. Kullanılabilir düzeyler şunlardır: BILGI (tüm istekler/yanıtlar), uyarı (yavaş yanıtlar), hata (yalnızca başarısız istekler) ve HIÇBIRI (çıktı günlüğü yok). (varsayılan ' BILGI ') (varsayılan "BILGI")

**--özyinelemeli**                Dizinler arasında eşitleme yaparken alt dizinlere yinelemeli olarak bakın.

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

|Seçenek|Açıklama|
|---|---|
|--Cap-Mbps uint32|Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.|
|--çıkış türü dize|Komutun çıktısının biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer "Text" değeridir.|
|--Güvenilen-Microsoft-sonekler dizesi   |Azure Active Directory oturum açma belirteçlerinin gönderilebileceği ek etki alanı soneklerini belirtir.  Varsayılan değer '*. Core.Windows.net;*' dir. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net '. Burada listelenenler varsayılan olarak eklenir. Güvenlik için yalnızca Microsoft Azure etki alanlarını Buraya yerleştirmeniz gerekir. Birden çok girişi noktalı virgülle ayırın.|

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
