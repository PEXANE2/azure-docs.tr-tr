---
title: AzCopy tezgahtır | Microsoft Docs
description: Bu makale, AzCopy tezgahtır komutu için başvuru bilgileri sağlar.
author: normesta
ms.service: storage
ms.topic: reference
ms.date: 10/16/2019
ms.author: normesta
ms.subservice: common
ms.reviewer: zezha-msft
ms.openlocfilehash: 40ff6c6c76e255945681e678ef296ffcf9978f61
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "84485181"
---
# <a name="azcopy-benchmark"></a>AzCopy kıyaslaması

Test verilerini belirtilen hedefe yükleyerek bir performans kıyaslaması çalıştırır. Test verileri otomatik olarak oluşturulur.

Kıyaslama komutu, ' kopya ' olarak aynı karşıya yükleme işlemini çalıştırır, ancak şunları hariç:

  - Kaynak parametre yok.  Komut yalnızca bir hedef URL gerektirir. 
  
  - Yük, kaç dosya otomatik olarak oluşturulduğunu ve bunların boyutunu denetleyen komut satırı parametreleriyle açıklanmıştır. Oluşturma işlemi tamamen bellekte gerçekleşir. Disk kullanılmıyor.
  
  - Kopyalama komutu için kullanılabilen isteğe bağlı parametrelerin yalnızca birkaçı desteklenir.
  
  - Ek Tanılamalar ölçülür ve raporlanır.
  
  - Varsayılan olarak, aktarılan veriler Test çalıştırmasının sonunda silinir.

Kıyaslama modu, en yüksek aktarım hızını sağlayan paralel TCP bağlantısı sayısına otomatik olarak ayarlanır. Bu numara sonda görüntülenir. Oto ayarlamayı engellemek için AZCOPY_CONCURRENCY_VALUE ortam değişkenini belirli sayıda bağlantı olarak ayarlayın.

Tüm olağan kimlik doğrulama türleri desteklenir. Ancak, sınama için en kullanışlı yaklaşım genellikle SAS belirteciyle boş bir kapsayıcı oluşturmak ve SAS kimlik doğrulamasını kullanmaktır.

## <a name="examples"></a>Örnekler

```azcopy
azcopy benchmark [destination] [flags]
```

Varsayılan parametrelerle bir kıyaslama testi çalıştırın (ağları 1 GB/sn 'ye kadar sınama için uygun): '

- AzCopy tezgahtır "https://[hesap]. blob. Core. Windows. net/[Container]? <SAS> "

100 dosyalarını karşıya yükleyen bir kıyaslama testi çalıştırın, her 2 GiB boyutu: (hızlı bir ağda (örneğin, 10 Gbps) bir değerlendirme için uygundur): '

- AzCopy tezgahtır "https://[hesap]. blob. Core. Windows. net/[Container]? <SAS> " --File-Count 100--dosya başına

Bir kıyaslama testi çalıştırın ancak 50.000 dosya kullanın, her 8 MIB bir boyut kullanır ve MD5 karmalarına göre işlem `--put-md5` yapar (Bu, bayrağın Copy komutunda bunu yaparken olduğu şekilde). `--put-md5`Değerlendirme zaman amacı, MD5 hesaplamasının seçili dosya sayısı ve boyutu için üretilen işi etkileyip etkilemediğini test etsağlamaktır:

- AzCopy tezgahtır "https://[hesap]. blob. Core. Windows. net/[Container]? <SAS> " --File-Count 50000--dosya başına boyutu 8Dk--put-MD5

## <a name="options"></a>Seçenekler

**--BLOB türü** dize, hedefteki blob türünü tanımlar. Farklı blob türlerini benchişaretlemek için kullanılır. Copy komutunda aynı adlandırılmış parametre ile özdeş (varsayılan "Algıla").

**--blok-boyut-MB** float bu blok boyutunu kullanın (MIB 'de belirtilir). Varsayılan değer, dosya boyutuna göre otomatik olarak hesaplanır. Ondalık kesirlere izin verilir-ör. 0,25. Copy komutunda aynı adlandırılmış parametre ile özdeş.

**--Delete-test-Data**  Doğru ise, kıyaslama verileri kıyaslama çalıştırmasının sonunda silinir.  Verileri hedefteki tutmak istiyorsanız bu değeri false olarak ayarlayın. Örneğin, kıyaslama modu dışındaki el ile testler için kullanmak üzere (varsayılan doğru).

**--File-Count** uint kullanılacak otomatik olarak oluşturulan veri dosyası sayısı (varsayılan 100).

**-h,--yardım**  Tezgahtır için yardım

**--günlük düzeyi** dize günlük dosyası, kullanılabilir düzeyler: bilgi (tüm istekler/yanıtlar), uyarı (yavaş yanıtlar), hata (yalnızca başarısız istekler) ve hiçbiri (çıktı günlüğü yok) için günlük ayrıntı düzeylerini tanımlar. (varsayılan "BILGI")

**--PUT-MD5**  Her bir dosyanın MD5 karmasını oluşturun ve karmayı hedef blob/dosyanın Content-MD5 özelliği olarak kaydedin. (Varsayılan olarak, karma oluşturulmaz.) Copy komutunda aynı adlandırılmış parametre ile özdeş.

**--** her otomatik oluşturulan veri dosyasının dosya başına dize boyutu. Hemen arkasından K, M veya G gelen bir sayı olmalıdır. örn. 12k veya 200G (varsayılan "250M").

## <a name="options-inherited-from-parent-commands"></a>Üst komutlardan devralınan seçenekler

**--Cap-Mbps uint32**  Saniye başına megabit cinsinden aktarım hızının üst sınırı. Kısa süre içinde işlem hacmi büyük bir farklılık gösterebilir. Bu seçenek sıfır olarak ayarlandıysa veya atlanırsa, üretilen iş işleme alınır.

**--komut çıktısının çıkış türü** dize biçimi. Seçenekler şunlardır: Text, JSON. Varsayılan değer ' text ' değeridir. (varsayılan "metin").

**--Güvenilen-Microsoft-Suffixes** dizesi Azure Active Directory oturum açma belirteçlerinin gönderilebileceği ek etki alanı soneklerini belirtir.  Varsayılan değer '*. Core.Windows.net;*' dir. core.chinacloudapi.cn; *. Core.cloudapi.de;*. core.usgovcloudapi.net '. Burada listelenenler varsayılan olarak eklenir. Güvenlik için yalnızca Microsoft Azure etki alanlarını Buraya yerleştirmeniz gerekir. Birden çok girişi noktalı virgülle ayırın.

## <a name="see-also"></a>Ayrıca bkz.

- [AzCopy](storage-ref-azcopy.md)
