---
title: azcopy tezgah | Microsoft Dokümanlar
description: Bu makalede, azcopy tezgah komutu için referans bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 8570bce87aeea5473b4aadf9bd30bc0a648a6f0f
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72518310"
---
# <a name="azcopy-bench"></a>azcopy ortamı

Test verilerini belirli bir hedefe yükleyerek bir performans ölçütü çalıştırın. Test verileri otomatik olarak oluşturulur.

Kıyaslama komutu, aşağıdakiler dışında ,'kopya' olarak aynı yükleme işlemini yürütür:

  - Kaynak parametresi yok.  Komut yalnızca bir hedef URL gerektirir. Geçerli sürümde, bu hedef URL bir blob kapsayıcıbakın.
  
  - Yük, kaç dosyanın otomatik olarak oluşturulduğunu ve ne kadar büyük olduklarını kontrol eden komut satırı parametreleri tarafından açıklanır. Üretim süreci tamamen bellekte gerçekleşir. Disk kullanılmaz.
  
  - Kopyalama komutu için kullanılabilen isteğe bağlı parametrelerden yalnızca birkaçı desteklenir.
  
  - Ek tanılar ölçülür ve raporlanır.
  
  - Varsayılan olarak, aktarılan veriler test çalışmasının sonunda silinir.

Kıyaslama modu, maksimum verimi sağlayan paralel TCP bağlantı sayısına otomatik olarak uyum sağlar. Bu sayı sonunda görüntülenir. Otomatik ayarlamayı önlemek için, AZCOPY_CONCURRENCY_VALUE ortamı değişkenini belirli sayıda bağlantıya ayarlayın.

Tüm olağan kimlik doğrulama türleri desteklenir. Ancak, kıyaslama için en uygun yaklaşım genellikle bir SAS belirteci ile boş bir kapsayıcı oluşturmak ve SAS kimlik doğrulaması kullanmaktır.

## <a name="examples"></a>Örnekler

```azcopy
azcopy bench [destination] [flags]
```

Varsayılan parametrelere sahip bir kıyaslama testi çalıştırın (1 Gbps'ye kadar olan karşılaştırma ağları için uygundur):'

- azcopy tezgah "https://[hesap].blob.core.windows.net/[konteyner]? <SAS>"

Her biri 2 GiB boyutunda olmak üzere 100 dosya yükleyen bir kıyaslama testi çalıştırın: (örneğin 10 Gbps gibi hızlı bir ağda kıyaslama için uygundur):'

- azcopy tezgah "https://[hesap].blob.core.windows.net/[konteyner]? <SAS>" --dosya sayısı 100 --dosya başına boyut 2G

Yukarıdaki gibi, ancak 50.000 dosya kullanın, her 8 MiB boyutunda ve md5 karp'larını hesaplamak (aynı şekilde --put-md5 bayrağı kopyalama komutu bunu yapar). Kıyaslama yaparken --put-md5'in amacı, MD5 hesaplamasının seçili dosya sayısı ve boyutu için iş ortasını etkileyip etkilemediğini test etmektir:

- azcopy tezgah "https://[hesap].blob.core.windows.net/[konteyner]? <SAS>" --dosya sayısı 50000 --dosya başına boyut 8M --put-md5

## <a name="options"></a>Seçenekler

**--blob tipi** dize Hedefteki blob türünü tanımlar. Farklı blob türleri kıyaslama izin vermek için kullanılır. Kopyalama komutundaki aynı adlandırılmış parametreyle aynıdır (varsayılan "Algılama").

**--blok boyutu-mb** float Bu blok boyutunu kullanın (MiB'de belirtilir). Varsayılan, dosya boyutuna göre otomatik olarak hesaplanır. Ondalık kesirlere izin verilir - örneğin 0,25. Kopyalama komutundaki aynı adlandırılmış parametreyle aynıdır.

**--sil-test-verileri**  Doğruysa, kıyaslama verileri kıyaslama çalışmasının sonunda silinir.  Verileri hedefte tutmak istiyorsanız (örneğin, kıyaslama modu dışındaki el ile testler için kullanmak (varsayılan true) için yanlış olarak ayarlayın.

**--dosya sayısı** uint Kullanılacak otomatik oluşturulan veri dosyalarının sayısı (varsayılan 100).

**-h, --yardım**  Tezgah için yardım

**--günlük düzeyi** dizesi Günlük dosyasının günlük ayrıntılılığını, kullanılabilir düzeyleri: INFO(tüm istek/yanıtlar), UYARI(yavaş yanıtlar), HATA (yalnızca başarısız istekler) ve NONE(çıkış günlüğü yok) olarak tanımlayın. (varsayılan "BİlGİ")

**--put-md5**  Her dosyanın bir MD5 karma sını oluşturun ve karmayı hedef blob/dosyanın İçerik-MD5 özelliği olarak kaydedin. (Varsayılan olarak karma oluşturulmadı.) Kopyalama komutundaki aynı adlandırılmış parametreyle aynıdır.

**--otomatik** olarak oluşturulan her veri dosyasının dosya başına boyut dize boyutu. K, M veya G. E.g. tarafından hemen takip edilen bir sayı olmalıdır. 12k veya 200G (varsayılan "250M").

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

**--kap-mbps uint32**  Transfer hızını saniyede megabit olarak kaplar. Anlık iş artışı kapaktan biraz farklı olabilir. Bu seçenek sıfıra ayarlanmışsa veya atlanırsa, iş elde etme kapaklı değildir.

**--output türü** dize Komutun çıktısının biçimi. Seçenekler şunlardır: metin, json. Varsayılan değer 'metin'dir. (varsayılan "metin").

## <a name="see-also"></a>Ayrıca bkz.

- [azcopy](storage-ref-azcopy.md)
