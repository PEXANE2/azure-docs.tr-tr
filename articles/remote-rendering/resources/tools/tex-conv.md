---
title: TexConv - Doku dönüştürme aracı
description: TexConv komut satırı aracı için kullanım kılavuzu
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 1d9b2ca163b70435a6c0e245e66492e8e2866639
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80680030"
---
# <a name="texconv---texture-conversion-tool"></a>TexConv - Doku dönüştürme aracı

TexConv, PNG, TGA, JPEG ve DDS gibi tipik giriş biçimlerindeki dokuları çalışma zamanı tüketimi için optimize edilmiş biçimlerde işlemek için bir komut satırı aracıdır.
En yaygın senaryo tek bir giriş dosyasını `A.xxx` optimize edilmiş `B.yyy`bir biçime dönüştürmek olsa da, aracın gelişmiş kullanımlar için birçok ek seçeneği vardır.

## <a name="command-line-help"></a>Komut satırı yardımı

`--help` Parametre ile TexConv.exe çalıştıran tüm kullanılabilir seçenekleri listeleyecek. Ayrıca, TexConv ne yaptığını anlamadan yardımcı olmak için, yürütülürken kullanılan seçenekleri yazdırır. Ayrıntılar için bu çıktıya başvurun.

## <a name="general-usage"></a>Genel kullanım

TexConv her zaman **tam olarak bir çıkış** dosyası üretir. Çıktıyı birleştirmek için **birden çok giriş** dosyası kullanabilir. Montaj için, aynı zamanda hangi kanal *(Kırmızı, Yeşil, Mavi* veya *Alfa)* hangi giriş dosyası almak ve çıkış görüntünün hangi kanaliçine taşımak için söyler bir **kanal eşleme**gerekir.

En düz ileri komut satırı şudur:

```cmd
TexConv.exe -out D:/result.dds -in0 D:/img.jpg -rgba in0
```

- `-out`çıktı dosyasını ve biçimini belirtir
- `-in0`ilk giriş görüntüsünü belirtir
- `-rgba`çıkış görüntüsünün dört kanalı da kullanması gerektiğini ve giriş görüntüsünden 1:1 alınması gerektiğini söyler

## <a name="multiple-input-files"></a>Birden çok giriş dosyası

Birden çok giriş dosyasından çıktıyı birleştirmek için, artan `-in` sayıda seçeneği kullanarak her giriş dosyasını belirtin:

```cmd
-in0 D:/img0.jpg -in1 D:/img1.jpg -in2 D:/img2.jpg ...
```

2B dokulardan bir küp eşleme oluştururken, `-right` `-left`bir `-top` `-bottom`de `-front` `-back` , `-px` `-nx`, `-py` `-ny` `-pz`, `-nz`, , , , , , .

Bu girdileri çıktı dosyasıyla eşleştirmek için uygun bir kanal eşleme gerekir.

## <a name="channel-mappings"></a>Kanal eşlemeleri

Kanal eşleme seçenekleri, verilen çıktı kanallarını dolduracak girişin hangisini belirleyeceklerini belirtir. Her kanalın girişini ayrı ayrı şu şekilde belirtebilirsiniz:

```cmd
-r in0.b -g in0.g -b in0.r -a in1.r
```

Burada çıkış RGB kanalları ilk giriş görüntü kullanılarak doldurulur ama kırmızı ve mavi takas alırsınız. Çıktının alfa kanalı, ikinci giriş görüntüsünün kırmızı kanalındaki değerlerle doldurulur.

Her kanal için eşlemenin ayrı ayrı belirtilmesi en büyük esnekliği sağlar. Kolaylık sağlamak için aynı "swizzling" işleçleri kullanılarak yazılabilir:

```cmd
-rgb in0.bgr -a in1.r
```

### <a name="output-channels"></a>Çıkış kanalları

Aşağıdaki kanal eşleme seçenekleri mevcuttur:

- `-r`, `-g` `-b`, `-a` , : Bunlar tek kanallı atamaları belirtir
- `-rg`: Kırmızı ve yeşil kanal atamalarını belirtin.
- `-rgb`: Kırmızı, yeşil ve mavi kanal atamalarını belirtin.
- `-rgba`: Dört kanal atamalarını da belirtir.

Yalnızca R, RG veya RGB kanalından bahsetmek, TexConv'a sırasıyla yalnızca 1, 2 veya 3 kanallı bir çıktı dosyası oluşturmasını bildirir.

### <a name="input-swizzling"></a>Giriş swizzling

Hangi giriş dokusunun hangi çıkış kanalını doldurması gerektiğini belirtirken, giriş ibareyi döndürebilirsiniz:

- `-rgba in0`eşdeğerdir`-rgba in0.rgba`
- `-rgba in0.bgra`giriş kanallarını döndürür
- `-rgb in0.rrr`tüm kanallara kırmızı kanal çoğaltır

Kanallar siyah veya beyaz ile de doldurulabilir:

- `-rgb in0 -a white`4 kanal çıkış dokusu oluşturacak ama alfayı tam opak olarak ayarlayacak
- `-rg black -b white`tamamen mavi bir doku yaratacak

## <a name="common-options"></a>Sık kullanılan seçenekler

En ilginç seçenekler aşağıda listelenmiştir. Daha fazla seçenek `TexConv --help`tarafından listelenir.

### <a name="output-type"></a>Çıkış türü

- `-type 2D`: Çıktı normal bir 2B görüntü olacaktır.
- `-type Cubemap`: Çıktı bir küp eşlem görüntüsü olacaktır. Yalnızca DDS çıktı dosyaları için desteklenir. Bu belirtildiğinde, bir 6 düzenli 2D giriş görüntüleri küp eşler oluşturabilirsiniz.

### <a name="image-compression"></a>Görüntü sıkıştırma

- `-compression none`: Çıkış görüntüsü sıkıştırılmamış olacaktır.
- `-compression medium`: Desteklenirse, çıktı görüntüsü çok fazla kaliteden ödün vermeden sıkıştırma kullanır.
- `-compression high`: Desteklenirse, çıktı görüntüsü daha küçük bir dosya lehine sıkıştırma ve fedakarlık kalitesini kullanır.

### <a name="mipmaps"></a>Mipmaps

Varsayılan olarak, TexConv çıktı biçimi desteklediğinde mipmaps oluşturur.

- `-mipmaps none`: Mipmaps oluşturulmayacak.
- `-mipmaps Linear`: Desteklenirse, bir kutu filtresi kullanılarak mipmaps oluşturulur.

### <a name="usage-srgb--gamma-correction"></a>Kullanım (sRGB / gama düzeltme)

Bu `-usage` seçenek çıktının amacını belirtir ve böylece TexConv'a giriş ve çıktı dosyalarına gama düzeltmesi uygulayıp uygulamayacağını söyler. Kullanım yalnızca RGB kanallarını etkiler. Alfa kanalı her zaman 'doğrusal' değerler içerdiği kabul edilir. Kullanım belirtilmemişse, 'otomatik' modu ilk giriş görüntüsünün biçimi ve dosya adından kullanımı algılamaya çalışır. Örneğin, tek ve çift kanallı çıkış biçimleri her zaman doğrusaldır. TexConv'un hangi kararı verdiğine görmek için çıktıyı kontrol edin.

- `-usage Linear`: Çıktı görüntüsü renkleri temsil eden değerler içerir. Bu genellikle metalik ve pürüzlülük dokular için durum, yanı sıra maskeler her türlü.

- `-usage Color`: Çıkış görüntüsü, diffüz/albedo haritaları gibi renkleri temsil eder. SRGB bayrağı çıkış DDS üstbilgisinde ayarlanır.

- `-usage HDR`: Çıktı dosyası kodlama için piksel başına 8 bitten fazla kullanmalıdır. Sonuç olarak tüm değerler doğrusal alanda depolanır. HDR dokuları için verilerin rengi veya diğer verileri temsil edip etmediği önemli değildir.

- `-usage NormalMap`: Çıkış görüntüsü teğet uzay normal eşlemi temsil eder. Değerler normale dönecek ve mipmap hesaplaması biraz optimize edilecektir.

- `-usage NormalMap_Inverted`: Çıkış, Y girişinin ters yönünü gösteren teğet uzay normal bir haritadır.

### <a name="image-rescaling"></a>Görüntü yeniden ölçekleme

- `-minRes 64`: Çıktının minimum çözünürlüğünü belirtir. Giriş görüntüsü daha küçükse, yükseltilir.
- `-maxRes 1024`: Çıktının maksimum çözünürlüğünü belirtir. Giriş görüntüsü daha büyükse, küçültülmüş olur.
- `-downscale 1`: Bu 0'dan büyükse, giriş görüntüleri N kez çözünürlükte yarıya indirilir. Genel bir kalite indirimi uygulamak için bunu kullanın.

## <a name="examples"></a>Örnekler

### <a name="convert-a-color-texture"></a>Renk dokusunu dönüştürme

```cmd
TexConv.exe -out D:/diffuse.dds -in0 D:/diffuse.jpg -rgba in0 -usage color
```

### <a name="convert-a-normal-map"></a>Normal bir haritayı dönüştürme

```cmd
TexConv.exe -out D:/normalmap.dds -in0 D:/normalmap.png -rgb in0 -usage normalmap
```

### <a name="create-an-hdr-cubemap"></a>HDR küp haritası oluşturma

```cmd
TexConv.exe -out "D:/skybox.dds" -in0 "D:/skymap.hdr" -rgba in0 -type cubemap -usage hdr
```

HDR küp eşlemleri için harika bir kaynak [hdrihaven.com.](https://hdrihaven.com/hdris/)

### <a name="bake-multiple-images-into-one"></a>Birden fazla görüntüyü tek bir resim halinde pişirin

```cmd
TexConv.exe -out "D:/Baked.dds" -in0 "D:/metal.tga" -in1 "D:/roughness.png" -in2 "D:/DiffuseAlpha.dds" -r in1.r -g in0.r -b black -a in2.a -usage linear
```

### <a name="extract-a-single-channel"></a>Tek bir kanalı ayıklama

```cmd
TexConv.exe -out D:/alpha-mask-only.dds -in0 D:/DiffuseAlpha.dds -r in0.a
```
