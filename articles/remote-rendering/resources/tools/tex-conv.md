---
title: TexConv-doku dönüştürme aracı
description: TexConv komut satırı aracı için Kullanıcı el kitabı
author: jakrams
ms.author: jakras
ms.date: 02/11/2020
ms.topic: article
ms.openlocfilehash: 1d9b2ca163b70435a6c0e245e66492e8e2866639
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "80680030"
---
# <a name="texconv---texture-conversion-tool"></a>TexConv-doku dönüştürme aracı

TexConv, PNG, TGA, JPEG ve DDS gibi tipik giriş biçimlerinden, çalışma zamanı tüketimine yönelik iyileştirilmiş biçimlere kadar dokuları işlemek için bir komut satırı aracıdır.
En yaygın senaryo, tek bir giriş dosyasını `A.xxx` iyileştirilmiş bir biçime `B.yyy`dönüştürirken, araç gelişmiş kullanımlar için birçok ek seçeneğe sahiptir.

## <a name="command-line-help"></a>Komut satırı yardımı

`--help` Parametresi Ile texconv. exe ' yi çalıştırmak, tüm kullanılabilir seçenekleri listeler. Ayrıca, TexConv, ne yaptığını anlamanıza yardımcı olmak için, çalıştırıldığında kullanılan seçenekleri yazdırır. Ayrıntılar için bu çıkışa bakın.

## <a name="general-usage"></a>Genel kullanım

TexConv her zaman **tam olarak bir çıkış** dosyası üretir. Çıktıyı birleştirmek için **birden çok giriş** dosyası kullanabilir. Derleme için Ayrıca, bu, hangi kanalın (*kırmızı, yeşil, mavi* veya *Alfa*) hangi giriş dosyasından ele alınacağı ve bunu çıkış resminin hangi kanalında taşıyacağını belirten bir **Kanal eşlemesine**de ihtiyaç duyuyor.

En düz ileri sarma komut satırı budur:

```cmd
TexConv.exe -out D:/result.dds -in0 D:/img.jpg -rgba in0
```

- `-out`çıkış dosyasını ve biçimini belirtir
- `-in0`ilk giriş görüntüsünü belirtir
- `-rgba`çıkış resminin tüm dört kanalı kullanması gerektiğini ve giriş görüntüsünden 1:1 alınması gerektiğini söyler

## <a name="multiple-input-files"></a>Birden çok giriş dosyası

Birden çok giriş dosyasındaki çıktıyı birleştirmek için, her giriş dosyasını artan bir sayıyla birlikte `-in` kullanarak belirtin:

```cmd
-in0 D:/img0.jpg -in1 D:/img1.jpg -in2 D:/img2.jpg ...
```

`-right`2B dokulardan bir küp harita montajı sırasında, birisi, `-left` `-top` `-bottom`,,, `-front`, `-back` `-px` `-nx` `-py` `-ny` `-pz` `-nz`,,,,,,,,,,,,,,,

Bu girişleri çıkış dosyasına eşlemek için uygun bir kanal eşleştirmesi gerekir.

## <a name="channel-mappings"></a>Kanal eşlemeleri

Kanal eşleme seçenekleri, verilen çıkış kanallarının hangi girişte doldurulacağını belirtir. Her bir kanalın girişini şöyle ayrı belirtebilirsiniz:

```cmd
-r in0.b -g in0.g -b in0.r -a in1.r
```

Burada çıktının RGB kanalları ilk giriş görüntüsü kullanılarak doldurulacak ancak kırmızı ve mavi, takas edilecek. Çıktının alfa kanalı, ikinci giriş resminin kırmızı kanalındaki değerlerle doldurulur.

Her kanal için eşlemenin belirtilmesi, en büyük esnekliği sağlar. Daha kolay olması için "swizzling" işleçleri kullanılarak yazılabilir:

```cmd
-rgb in0.bgr -a in1.r
```

### <a name="output-channels"></a>Çıkış kanalları

Aşağıdaki kanal eşleme seçenekleri kullanılabilir:

- `-r``-b`, `-g`,, `-a` : Bu tek kanal atamalarını belirtir
- `-rg`: Kırmızı ve yeşil kanal atamalarını belirtin.
- `-rgb`: Kırmızı, yeşil ve mavi kanal atamalarını belirtin.
- `-rgba`: Dört kanal atamasını belirtir.

Yalnızca R, RG veya RGB kanalının bahsetmesi, TexConv 'ın sırasıyla yalnızca 1, 2 veya 3 kanallı bir çıkış dosyası oluşturmasını söyler.

### <a name="input-swizzling"></a>Giriş swizzling

Hangi giriş dokusunun hangi çıkış kanalını dolduracağı konusunda bir Swizzle, bir tane girişi içerebilir:

- `-rgba in0`eşdeğerdir`-rgba in0.rgba`
- `-rgba in0.bgra`Giriş kanallarını Swizzle
- `-rgb in0.rrr`kırmızı kanalı tüm kanallarla çoğaltır

Ayrıca, kanalları siyah ya da beyazla doldurabilir.

- `-rgb in0 -a white`4 kanal çıkış dokusu oluşturur, ancak alfa 'yi tamamen opak olarak ayarlar
- `-rg black -b white`tamamen mavi bir doku oluşturacak

## <a name="common-options"></a>Ortak seçenekler

En ilginç seçenekler aşağıda listelenmiştir. Daha fazla seçenek tarafından `TexConv --help`listelenmiştir.

### <a name="output-type"></a>Çıkış türü

- `-type 2D`: Çıktı normal bir 2B görüntü olacaktır.
- `-type Cubemap`: Çıktı bir küp harita görüntüsü olacaktır. Yalnızca DDS çıkış dosyaları için desteklenir. Bu belirtildiğinde, bir tane, 6 normal 2B giriş görüntülerinden küp harita 'i birleştirebilir.

### <a name="image-compression"></a>Görüntü sıkıştırma

- `-compression none`: Çıkış resminin sıkıştırması kaldırılacak.
- `-compression medium`: Destekleniyorsa, çıkış resmi çok fazla kaliteden ödün vermeden sıkıştırmayı kullanacaktır.
- `-compression high`: Destekleniyorsa, çıkış resmi daha küçük bir dosya için sıkıştırmayı ve feice kalitesini kullanır.

### <a name="mipmaps"></a>Mipmap

Varsayılan olarak, TexConv çıkış biçimi onu desteklediğinde, msunucudan mı haritaları oluşturur.

- `-mipmaps none`: Msunucudan haritalar oluşturulmayacak.
- `-mipmaps Linear`: Destekleniyorsa, bir kutu filtresi kullanılarak mı haritaları oluşturulacaktır.

### <a name="usage-srgb--gamma-correction"></a>Kullanım (sRGB/Gamma düzeltme)

Bu `-usage` seçenek, çıktının amacını belirtir ve bu nedenle, giriş ve çıkış dosyalarına gama düzeltmesinin uygulanıp uygulanamayacağını belirten texconv öğesine söyler. Kullanım yalnızca RGB kanallarını etkiler. Alfa kanalı her zaman ' doğrusal ' değerler içerecek şekilde değerlendirilir. Kullanım belirtilmemişse, ' Auto ' modu, ilk giriş görüntüsünün biçiminden ve dosya adından kullanımı algılamaya çalışır. Örneğin, tek ve çift kanal çıkış biçimleri her zaman doğrusal değildir. Hangi karar TexConv yaptığını görmek için çıktıyı kontrol edin.

- `-usage Linear`: Çıkış resmi, renkleri temsil eden değerler içeriyor. Bu genellikle metalik ve kabalık dokuların yanı sıra tüm maske türlerini kullanır.

- `-usage Color`: Çıkış resmi, dağıtma/Albedo haritaları gibi rengi temsil eder. SRGB bayrağı output DDS üst bilgisinde ayarlanacak.

- `-usage HDR`: Çıkış dosyası kodlama için piksel başına 8 bitten fazlasını kullanmalıdır. Sonuç olarak tüm değerler doğrusal alanda depolanır. HDR dokuları için, verilerin rengi veya diğer verileri temsil etmeksizin önemi yoktur.

- `-usage NormalMap`: Çıkış resmi bir teğet-Space normal eşlemesini temsil eder. Değerler normalleştirilir ve mipmap hesaplama biraz iyileştirecektir.

- `-usage NormalMap_Inverted`: Çıktı, Y ile bir teğet-Space normal eşlemedir ve girişin ters yönünde işaret eder.

### <a name="image-rescaling"></a>Görüntü yeniden oluşturma

- `-minRes 64`: Çıktının en düşük çözünürlüğünü belirtir. Giriş resmi daha küçükse, ölçeklendirilmez.
- `-maxRes 1024`: Çıktının en yüksek çözünürlüğünü belirtir. Giriş resmi daha büyükse, küçük ölçekli olur.
- `-downscale 1`: Bu 0 ' dan büyükse, giriş görüntüleri çözüm N kez yarıya iner. Genel kalite azaltma uygulamak için bunu kullanın.

## <a name="examples"></a>Örnekler

### <a name="convert-a-color-texture"></a>Renk dokusunu Dönüştür

```cmd
TexConv.exe -out D:/diffuse.dds -in0 D:/diffuse.jpg -rgba in0 -usage color
```

### <a name="convert-a-normal-map"></a>Normal Haritayı Dönüştür

```cmd
TexConv.exe -out D:/normalmap.dds -in0 D:/normalmap.png -rgb in0 -usage normalmap
```

### <a name="create-an-hdr-cubemap"></a>HDR küp harita oluşturma

```cmd
TexConv.exe -out "D:/skybox.dds" -in0 "D:/skymap.hdr" -rgba in0 -type cubemap -usage hdr
```

HDR cubemaps için harika bir kaynak [hdrihaven.com](https://hdrihaven.com/hdris/).

### <a name="bake-multiple-images-into-one"></a>Birden çok görüntüyü tek bir bakışta

```cmd
TexConv.exe -out "D:/Baked.dds" -in0 "D:/metal.tga" -in1 "D:/roughness.png" -in2 "D:/DiffuseAlpha.dds" -r in1.r -g in0.r -b black -a in2.a -usage linear
```

### <a name="extract-a-single-channel"></a>Tek bir kanalı Ayıkla

```cmd
TexConv.exe -out D:/alpha-mask-only.dds -in0 D:/DiffuseAlpha.dds -r in0.a
```
