---
title: Kaba yeniden yerelleştirme
description: Size yakın olan bağlantıları bulmak için kaba yeniden yerelleştirme kullanma hakkında bilgi edinin.
author: msftradford
manager: MehranAzimi-msft
services: azure-spatial-anchors
ms.author: parkerra
ms.date: 01/28/2021
ms.topic: conceptual
ms.service: azure-spatial-anchors
ms.custom: devx-track-csharp
ms.openlocfilehash: fc04242e61c748d7ae52e61e40206ba338a1b6aa
ms.sourcegitcommit: dd24c3f35e286c5b7f6c3467a256ff85343826ad
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2021
ms.locfileid: "99071148"
---
# <a name="coarse-relocalization"></a>Kaba yeniden yerelleştirme

Kaba yeniden yerelleştirme, soruya yaklaşık olarak kısa bir yanıt sağlayarak büyük ölçekli yerelleştirmeyi sağlayan bir özelliktir: *cihazım şimdi, ne kadar hangi içerikleri gözlemlerim?* Yanıt kesin değil, ancak bunun yerine şu biçimdedir: *Bu Tutturucuların yakınına yakınız; bunlardan birini konumlandırmayı deneyin*.

Kaba yeniden yerelleştirme, bağlantıları daha sonra hızlı sorgulama için kullanılan çeşitli cihaz algılayıcı okumaları ile etiketleyerek işe yarar. Açık Hava senaryolarında, algılayıcı verileri genellikle cihazın GPS (Global Konumlandırma Sistemi) konumudur. GPS kullanılabilir olmadığında veya güvenilir olmadığında (ınkapılar gibi), algılayıcı verileri WiFi erişim noktalarından ve aralıktaki Bluetooth işaretleriyle oluşur. Toplanan algılayıcı verileri, cihazlarınızın hangi bağlayıcıların yakınına hızlı bir şekilde belirlenmesi için Azure uzamsal bağlantıları tarafından kullanılan bir uzamsal dizinin sürdürülmesi için katkıda bulunur.

## <a name="when-to-use-coarse-relocalization"></a>Ne zaman kaba yeniden yerelleştirme kullanılacağı

Tenis mahkemesinin daha büyük bir alanında 35 ' den fazla uzamsal tutturulmaya planlandıysanız, büyük olasılıkla daha fazla yerelleştirme, uzamsal dizin oluşturma avantajlarından yararlanabilirsiniz.

Kaba reyerelleştirme tarafından etkinleştirilen bağlayıcıların hızlı bakış özelliği, dünya ölçeğinde (yani milyonlarca coğrafi olarak dağıtılmış) bağlayıcı koleksiyonları tarafından desteklenen uygulamaların geliştirilmesini basitleştirmek üzere tasarlanmıştır. Uzamsal dizin oluşturmanın karmaşıklığı tamamen gizlenir ve uygulama mantığınıza odaklanmanızı sağlar. Tüm çapa noktası ağır kaldırma işlemi, arka planda Azure uzamsal bağlantılarına göre yapılır.

## <a name="using-coarse-relocalization"></a>Kaba yeniden yerelleştirme kullanma

Azure uzamsal çıpası oluşturma ve sorgulama için tipik iş akışı, kaba yeniden yerelleştirme:
1.  Seçtiğiniz algılayıcı verilerini toplamak için bir algılayıcı parmak izi sağlayıcısı oluşturun ve yapılandırın.
2.  Bir Azure uzamsal bağlantı oturumu başlatın ve bağlayıcı oluşturun. Algılayıcı parmak izi etkin olduğundan, bağlantılar kaba bir yeniden yerelleştirme tarafından dağınık şekilde dizinlenir.
3.  Azure uzamsal bağlantı oturumunda adanmış arama ölçütlerini kullanarak kaba yeniden yerelleştirme kullanarak çevreleyen bağlantıları sorgulayın.

Uygulamanızda daha kaba bir yeniden yerelleştirme ayarlamak için aşağıdaki öğreticiye başvurabilirsiniz:
* [Unity 'de kaba yeniden yerelleştirme](../how-tos/set-up-coarse-reloc-unity.md)
* [Amaç için kaba yeniden yerelleştirme-C](../how-tos/set-up-coarse-reloc-objc.md)
* [Swift 'ta kaba yeniden yerelleştirme](../how-tos/set-up-coarse-reloc-swift.md)
* [Java 'da kaba yeniden yerelleştirme](../how-tos/set-up-coarse-reloc-java.md)
* [C++ ' da kaba yeniden yerelleştirme/NDK](../how-tos/set-up-coarse-reloc-cpp-ndk.md)
* [C++/Wınrt 'de kaba yeniden yerelleştirme](../how-tos/set-up-coarse-reloc-cpp-winrt.md)

## <a name="sensors-and-platforms"></a>Algılayıcılar ve platformlar

### <a name="platform-availability"></a>Platform kullanılabilirliği

Bağlantı hizmetine gönderebilmeniz gereken algılayıcı verisi türleri şunlardır:

* GPS konumu: enlem, Boylam, yükseklik.
* Aralıktaki WiFi erişim noktalarının sinyal gücü.
* Aralıktaki Bluetooth işaretlerinin sinyal gücü.

Aşağıdaki tabloda, desteklenen platformlarda algılayıcı verilerinin kullanılabilirliği ve platforma özgü tüm uyarılar özetlenmektedir:

|                 | HoloLens | Android | iOS |
|-----------------|----------|---------|-----|
| **YUVASı**         | <sup>1</sup> yok  | Evet<sup>2</sup> | Evet<sup>3</sup> |
| **WiFi**        | Evet<sup>4</sup> | Evet<sup>5</sup> | NO  |
| **BLO işaretleri** | Evet<sup>6</sup> | Evet<sup>6</sup> | Evet<sup>6</sup>|


<sup>1</sup> BIR dış GPS cihazı, Hololens ile ilişkilendirilebilir. Bir GPS izleyici ile HoloLens kullanmak istiyorsanız [desteğimize](../spatial-anchor-support.md) başvurun.<br/>
<sup>2</sup> [LocationManager][3] API 'LERI (GPS ve ağ) aracılığıyla desteklenir<br/>
<sup>3</sup> [cllocationmanager][4] API 'leri aracılığıyla desteklenir<br/>
<sup>4</sup> her 3 saniyede bir taramanın yaklaşık bir hızda desteklenir <br/>
<sup>5</sup> API düzeyi 28 ile başlayarak, WiFi taramaları 2 dakikada bir olacak şekilde 4 çağrı yapılır. Android 10 ' dan azaltma, Geliştirici ayarları menüsünden devre dışı bırakılabilir. Daha fazla bilgi için bkz. [Android belgeleri][5].<br/>
<sup>6</sup> Ile [Eddystone][1] ve [ıişaret][2] ile sınırlı

### <a name="which-sensor-to-enable"></a>Etkinleştirilecek algılayıcı

Algılayıcı seçimi, geliştirmekte olduğunuz uygulamaya ve platforma özgüdür.
Aşağıdaki diyagramda, yerelleştirme senaryosuna bağlı olarak sensörlerin birleşiminin etkinleştiribileceği bir başlangıç noktası verilmiştir:

![Etkin sensör seçiminin diyagramı](media/coarse-relocalization-enabling-sensors.png)

Aşağıdaki bölümlerde, her algılayıcı türü için avantaj ve sınırlamalar hakkında daha fazla öngörü verilmektedir.

### <a name="gps"></a>YUVASı

GPS, dış mekan senaryolar için go seçeneğidir.
Uygulamanızda GPS kullanırken, donanım tarafından sunulan okumalar tipik olarak şunları göz önünde bulundurun:

* zaman uyumsuz ve düşük sıklık (1 Hz 'den az).
* güvenilir olmayan/gürültülü (ortalama 7-b standart sapması).

Genel olarak, hem cihaz işletim sistemi hem de Azure uzamsal bağlantıları, bu sorunları azaltmak için ham GPS sinyalinde bazı filtreleme ve tahmin işlemi yapar. Bu ek işleme yakınsama için zaman gerektirir, bu nedenle en iyi sonuçlar için şunu deneyin:

* uygulamanızda olabildiğince erken bir algılayıcı parmak izi sağlayıcısı oluşturun
* algılayıcı parmak izi sağlayıcısını birden çok oturum arasında canlı tutun
* algılayıcı parmak izi sağlayıcısını birden çok oturum arasında paylaşma

Tüketici sınıfı GPS cihazları genellikle kesin olarak kullanılır. [Zandenbergen ve Barbeau][6] tarafından yapılan bir inceleme (2011), destekli cep telefonlarını (A-GPS) 7 ölçümle (oldukça büyük bir değer yok sayılır) bir şekilde raporlar. Bu ölçüm hatalarına yönelik hesaba geçmek için hizmet, bağlantıları GPS alanında olasılık dağıtımları olarak değerlendirir. Bu nedenle, bir tutturucu, büyük olasılıkla en olası alan bölgesidir (yani %95 ' ten fazla güven), gerçek, bilinmeyen GPS konumunu içerir.

GPS ile sorgulama yapılırken aynı düşünme uygulanır. Cihaz, doğru, bilinmeyen GPS konumu etrafında başka bir uzamsal güven bölgesi olarak temsil edilir. Yakın olan Tutturucuların bulunması, aşağıdaki görüntüde gösterildiği gibi, güvenilir bölgelere sahip olan bağlantıları cihazın güven bölgesine *yeterince yakın* bir şekilde bulmak için çeviri yapar:

![GPS ile bağlayıcı adayları seçimi](media/coarse-reloc-gps-separation-distance.png)

### <a name="wifi"></a>WiFi

HoloLens ve Android 'de, WiFi sinyal gücü, yoğun bir şekilde yeniden yerelleştirme sağlamak için iyi bir seçenek olabilir.
Bunun avantajı, WiFi erişim noktalarının (örneğin, ofis alanları veya alışveriş mallarından ortak olarak), ek bir kurulum gerekmeden mümkün olan en hızlı şekilde kullanılabilmelidir.

> [!NOTE]
> iOS, WiFi sinyali gücünü okumak için herhangi bir API sağlamaz ve bu nedenle, WiFi etkin kaba yeniden yerelleştirme için kullanılamaz.

Uygulamanızda WiFi kullanırken, donanım tarafından sunulan okumalar genellikle şunları göz önünde bulundurun:

* zaman uyumsuz ve düşük sıklık (0,1 Hz 'den az).
* işletim sistemi düzeyinde kısıtlanmış olabilir.
* güvenilmez/gürültülü (ortalama 3 dBm standart sapma üzerinde).

Azure uzamsal bağlantıları, bu sorunları azaltmak için bir oturum sırasında filtrelenmiş bir WiFi sinyali gücü eşlemesi oluşturmayı dener. En iyi sonuçlar için şunu denemeniz gerekir:

* ilk tutturucu yerleştirmekten önce oturum iyi oluşturun.
* oturumu mümkün olduğunca uzun tutun (yani, tüm tutturucuları ve tek bir oturumda sorgu oluşturun).

### <a name="bluetooth-beacons"></a>Bluetooth işaretleri
<a name="beaconsDetails"></a>

Bluetooth işaretlerini dikkatle dağıtmak, GPS 'nin eksik veya yanlış olduğu büyük ölçekli, daha iyi yerelleştirme senaryolarına yönelik iyi bir çözümdür. Ayrıca, üç platformda da desteklenen tek ınkapıdır yöntemidir.

İşaretlerin genellikle, UUID 'ler ve MAC adresleri gibi her şeyin yapılandırılabileceği çok yönlü cihazlardır. Azure uzamsal bağlantıları, işaretlerinin UUID 'ler tarafından benzersiz şekilde tanımlanmasını bekler. Bu benzersizlik, büyük olasılıkla hatalı sonuçlara neden olabilir. En iyi sonuçlar için şunları yapmanız gerekir:

* işaretlerine benzersiz UUID 'ler atayın.
* Bunları, alanınızı bir arada ele alan bir şekilde dağıtın ve bir boşluk olan herhangi bir noktadan en az 3 işaret erişilebilir.
* benzersiz işaret UUID 'ler listesini algılayıcı parmak izi sağlayıcısına geçirin

Bluetooth gibi radyo sinyalleri belirlenen engelleri aşmak 'den etkilenir ve diğer radyo sinyalleriyle karışabilir. Bu nedenlerden dolayı, alanın tek bir yere eklenip eklenmeyeceğini tahmin etmek zor olabilir. Daha iyi bir müşteri deneyimini güvence altına almak için, işaretlerinin kapsamını el ile test etmenizi öneririz. Bu işlem, aday cihazlarıyla alanınızda gezinerek ve Bluetooth 'un aralıklı olarak gösterildiği bir uygulamayla gerçekleştirilebilir. Kapsamı test ederken, alanınızda yer alan herhangi bir stratejik konumdan en az 3 işaretlerine ulaşabildiğinizden emin olun. Çok fazla sayıda işareti ayarlama bunlar arasında daha fazla girişim oluşmasına neden olabilir ve bu da kaba yeniden yerelleştirme doğruluğunu iyileştirmez.

Alanda hiçbir obstaci yoksa Bluetooth işaretlerinin genellikle 80 ölçüm kapsamı vardır.
Bu, büyük obgrafik içermeyen bir alan için, bir ızgara düzeninde her 40 ölçümle işaretleri dağıtabilecek anlamına gelir.

Pille çalışan bir işaret, sonuçları olumsuz yönde etkiler. bu nedenle, dağıtımınızı düşük veya kapalı pil için düzenli aralıklarla izlediğinizden emin olun.

Azure uzamsal bağlantıları yalnızca bilinen işaret yakınlık UUID 'ler listesindeki Bluetooth işaretlerini izler. Allow-listelenmiş UUID 'ler olarak programlanmış kötü amaçlı işaretleri, hizmetin kalitesini olumsuz yönde etkileyebilir. Bu nedenle, dağıtımını denetleyebileceğiniz, seçkin boşlukların en iyi sonucunu elde edersiniz.

### <a name="sensors-accuracy"></a>Sensör doğruluğu

Her ikisi de, hem bağlayıcı oluşturma hem de sorgular sırasında, döndürülen bağlantı kümesi üzerinde büyük bir etkisi olan GPS sinyalinin doğruluğu. Buna karşılık, WiFi/işaret tabanlı sorgular, sorguyla ortak olarak en az bir erişim noktası/işaret olan tüm bağlantıları kabul eder. Bu anlamda, WiFi/işaretlerine dayalı bir sorgunun sonucu genellikle erişim noktalarının/işaretlerinin ve çevre engellerin fiziksel aralığına göre belirlenir.
Aşağıdaki tabloda her algılayıcı türü için beklenen arama alanı tahmin edilecek:

| Algılayıcısı      | Arama alanı yarıçapı (yaklaşık) | Ayrıntılar |
|-------------|:-------:|---------|
| YUVASı         | 20 milyon-30 milyon | Diğer faktörler arasındaki GPS tarafından belirlenir. Raporlanan sayılar, bu 7 ölçüm olan,-GPS ile cep telefonlarına ait ortanca GPS doğruluğu için tahmin edilir. |
| WiFi        | 50 milyon-100 milyon | Kablosuz erişim noktalarının aralığına göre belirlenir. Sıklık, verici kuvveti, fiziksel engelleri, girişim vb. bağlıdır. |
| BLO işaretleri |  70 milyon | İşaret aralığına göre belirlenir. Sıklık, iletim gücüne, fiziksel engelleri, girişim vb. bağlıdır. |

<!-- Reference links in article -->
[1]: https://developers.google.com/beacons/eddystone
[2]: https://developer.apple.com/ibeacon/
[3]: https://developer.android.com/reference/android/location/LocationManager
[4]: https://developer.apple.com/documentation/corelocation/cllocationmanager?language=objc
[5]: https://developer.android.com/guide/topics/connectivity/wifi-scan
[6]: https://www.cambridge.org/core/journals/journal-of-navigation/article/positional-accuracy-of-assisted-gps-data-from-highsensitivity-gpsenabled-mobile-phones/E1EE20CD1A301C537BEE8EC66766B0A9
