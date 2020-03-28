---
title: Proje Akustik Birlik Tasarım Eğitimi
titlesuffix: Azure Cognitive Services
description: Bu öğretici, Unity Proje Akustik için tasarım iş akışını açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: fd00e4105ce4edae9d014df2a83c5ae3aaf778da
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68854272"
---
# <a name="project-acoustics-unity-design-tutorial"></a>Proje Akustik Birlik Tasarım Eğitimi
Bu öğretici, Unity'deki Project Acoustics'in tasarım araçlarını ve iş akışını açıklar.

Ön koşullar:
* Windows için Unity 2018.2+
* Pişmiş akustik varlık ile bir Birlik sahnesi

Bu öğretici için, pişmiş akustik varlık ile bir Birlik sahne iki şekilde alabilirsiniz:
* [Unity projenize Project Acoustics ekleyin,](unity-integration.md)ardından [Azure Toplu Hesap alın](create-azure-account.md)ve Unity [sahnenizi pişirin](unity-baking.md)
* Veya Project [Acoustics Unity örnek içeriğini](unity-quickstart.md)kullanın.

## <a name="review-design-process-concepts"></a>Tasarım süreci kavramlarını gözden geçirme
Project Acoustics kaynaklarınızı işlemek için ortak ses dijital sinyal işleme (DSP) yöntemlerini kullanır ve oklüzyon, ıslak/kuru karışım ve yankılanma kuyruk uzunluğu (RT60) dahil olmak üzere tanıdık akustik özellikleri üzerinde kontrol sağlar. Ancak temel [Project Acoustics tasarım süreci konsepti,](design-process.md) bu özellikleri doğrudan ayarlamak yerine, simülasyon sonuçlarının bu özellikleri yönlendirmek için nasıl kullanıldığını kontrol etmektir. Her denetim için varsayılan ayarlar fiziksel olarak doğru akustiği temsil eder.

## <a name="design-acoustics-for-each-source"></a>Her kaynak için akustik tasarlayın
Project Acoustics, kaynağa özel akustik tasarım denetimleri sağlar. Bu, bazı kaynakları vurgulayarak ve diğerlerini vurgulayarak bir sahnedekarışımı kontrol etmenizi sağlar.

### <a name="adjust-distance-based-attenuation"></a>Mesafe tabanlı zayıflamayı ayarlama
**Project Acoustics** Unity spatializer eklentisi tarafından sağlanan ses DSP, Unity Editor'da yerleşik olarak bulunan kaynak başına mesafe tabanlı zayıflamaya saygı duyar. Mesafe tabanlı zayıflama **denetimleri, 3B Ses Ayarları**altında, ses kaynaklarının **Müfettiş** panelinde bulunan **Ses Kaynağı** bileşeninde bulunmaktadır:

![Birlik mesafesi zayıflama seçenekleri panelinin ekran görüntüsü](media/distance-attenuation.png)

Akustik, oyuncunun bulunduğu yerin etrafında ortalanmış bir "simülasyon bölgesi" kutusunda hesaplama yapar. Bir ses kaynağı bu simülasyon bölgesinin dışında bulunan oynatıcıdan uzaksa, yalnızca kutunun içindeki geometri, oklüzörler oyuncunun yakınında yken oldukça iyi çalışan ses yayılımını (tıkamaya neden olmak gibi) etkiler. Ancak, oynatıcının açık alanda olduğu ancak oklüderlerin uzak ses kaynağına yakın olduğu durumlarda, ses gerçekçi olmayan bir şekilde ayrıştırılabilir. Önerilen geçici çözüm, bu gibi durumlarda ses zayıflamasının yaklaşık 45 m'de 0'a düşmesini sağlamaktır, bu da oyuncunun kutunun kenarına olan varsayılan yatay uzaklığıdır.

![Unity SpeakerMode seçenek panelinin ekran görüntüsü](media/speaker-mode.png)

### <a name="adjust-occlusion-and-transmission"></a>Oklüzyon ve iletimi ayarlama
AcousticsAdjust komut dosyasının bir kaynağa **takılması,** o kaynak için ayar parametrelerini sağlar. Komut dosyasını eklemek **için, Denetçi** panelinin altındaki **Bileşen Ekle'yi** tıklatın ve Komut > **Akustik Ayarla'ya**gidin. Komut dosyasının altı denetimi vardır:

![Unity AcousticsAdjust komut dosyasının ekran görüntüsü](media/acoustics-adjust.png)

* **Etkinleştir Akustik** - Bu kaynağa akustik uygulanıp uygulanmadığını denetler. İşaretlenmediğinde, kaynak HRTF'ler veya kaydırma ile uzamsallaştırılacaktır, ancak akustik olmayacaktır. Bu, seviye ve bozunma süresi gibi tıkanıklık, tıkanıklık veya dinamik yankı parametreleri anlamına gelir. Yankılanma hala sabit bir seviye ve çürüme süresi ile uygulanır.
* **Oklüzyon** - Akustik sistemi tarafından hesaplanan oklüzyon dB seviyesine bir çarpan uygulayın. Bu çarpan 1'den büyükse, oklüzyon abartılı olurken, 1'den küçük değerler oklüzyon etkisini daha ince ve 0 devre dışı kalarak bir değer yaratır.
* **İletim (dB)** - Geometri yoluyla iletimin neden olduğu zayıflamayı (dB'de) ayarlayın. İletimi devre dışı bırakabilmek için bu kaydırıcıyı en düşük düzeye ayarlayın. Akustik sahne geometrisi (portaling) etrafında gelen olarak ilk kuru ses mekansallaştırır. İletim, görüş hattı yönünde uzamsallaştırılan ek bir kuru varış sağlar. Kaynak için uzaklık zayıflama eğriside de uygulandığını unutmayın.

### <a name="adjust-reverberation"></a>Yankılanmayı ayarlama
* **Islürlik (dB)** - Kaynaktan uzaklığa göre dB'deki reverb gücünü ayarlar. Pozitif değerler sesi daha yankı uyandırırken, negatif değerler sesi daha kuru hale sağlar. Eğri düzenleyicisini getirmek için eğri denetimine (yeşil çizgi) tıklayın. Noktaları eklemek ve istediğiniz işlevi oluşturmak için bu noktaları sürükleyerek sol tıklatarak eğriyi değiştirin. X ekseni kaynaktan uzak, y ekseni ise dB'de yankı ayarıdır. Düzenleme eğrileri hakkında daha fazla bilgi için bu [Birlik Kılavuzu'na](https://docs.unity3d.com/Manual/EditingCurves.html)bakın. Eğriyi varsayılana geri ayarlamak **için, Islaçıklık'a** sağ tıklayın ve **Sıfırla'yı**seçin.
* **Bozunma Zaman Ölçeği** - Bir çarpanı bozunma süresi için ayarlar. Örneğin, fırın sonucu 750 milisaniyelik bir bozunma süresi belirtir, ancak bu değer 1,5 olarak ayarlanırsa, kaynağa uygulanan bozunma süresi 1.125 milisaniyedir.
* **Outdoorness** - akustik sisteminin tahmini bir katkı ayarlamanasıl "açık" bir kaynak üzerinde yankı sesi gerekir. Bu değeri 1'e ayarlamak, bir kaynağın her zaman tamamen açık havada ses çıkarmasını, -1'e ayarlanması ise bir kaynağın tamamen kapalı mekanda ses çıkarmasını sağlayacaktır.

**AcousticsAdjustExperimental** komut dosyasının bir kaynağa takılması, o kaynak için ek deneysel ayar parametreleri sağlar. Komut dosyasını eklemek **için, Denetçi** panelinin altındaki **Bileşen Ekle'yi** tıklatın ve Komut > Akustik **Için Gidin.** Şu anda bir deneysel kontrol vardır:

![Unity AcousticsAdjustExperimental komut dosyasının ekran görüntüsü](media/acoustics-adjust-experimental.png)

* **Algısal Mesafe Çözgü** - Kuru-ıslak oranı hesaplamak için kullanılan mesafeye üstel çözgü uygulayın. Akustik sistemi, mesafeile sorunsuz bir şekilde değişen ve algısal mesafe ipuçları sağlayan alan boyunca ıslak seviyeleri hesaplar. 1'den büyük değerleri çarpıtma, mesafeyle ilgili yankı düzeylerini artırarak bu etkiyi abartır ve sesi "uzak" hale getirir. 1'den küçük değerleri çarpıtma mesafe tabanlı yankı değişikliği daha ince hale getirerek sesi daha "mevcut" hale getirir.

## <a name="design-acoustics-for-all-sources"></a>Tüm kaynaklar için akustik tasarlayın
Tüm kaynakların parametrelerini ayarlamak için Unity'nin **Ses**Karıştırıcısı'ndaki kanal şeridine tıklayın ve **Project Acoustics Mikser** efektindeki parametreleri ayarlayın.

![Project Acoustics Unity Mixer özelleştirme panelinin ekran görüntüsü](media/mixer-parameters.png)

* **Islaklık Ayarla** - Kaynak dinleyici mesafesine göre sahnedeki tüm kaynaklarda dB'deki yankı gücünü ayarlar. Pozitif değerler sesi daha yankı uyandırırken, negatif değerler sesi daha kuru hale sağlar.
* **RT60 Ölçeği** - Reverb süresi için çarpan calar.
* **Kaydırma'yı kullanın** - Sesin binaural (0) veya çok kanallı kaydırma (1) olarak çıktı olup olmadığını denetler. 1'den başka herhangi bir değer binaural'ı gösterir. Binaural çıkış kulaklıkile kullanılmak üzere HRTF'ler ile mekansallaştırılır ve çok kanallı çıkış vbap ile çok kanallı surround hoparlör sistemleri ile kullanılmak üzere uzamsallaştırılır. Çok kanallı panner kullanıyorsanız, **Project Settings** > **Audio**altında bulunan cihaz ayarlarınızla eşleşen hoparlör modunu seçtiğinizden emin olun.

## <a name="check-proper-sound-source-placement"></a>Uygun ses kaynağı yerleşimini kontrol edin
İşgal edilmiş voxel'lerin içine yerleştirilen ses kaynakları akustik tedavi göremez. Voxel'ler görünür sahne geometrisini aştıklarından, görsel geometri tarafından engellenmemiş görünürken bir kaynağı bir voksilin içine yerleştirmek mümkündür. Project Acoustics voxels'i, **Scene** view'in sağ üst kısmındaki **Gizmos** menüsündeki voxel ızgara onay kutusunu değiştirerek görüntüleyebilirsiniz.

![Unity Gizmos menüsünün ekran görüntüsü](media/gizmos-menu.png)  

Voxel ekran, oyundaki görsel bileşenlerin kendilerine uygulanan bir dönüşüme sahip olup olmadığını belirlemede de yardımcı olabilir. Eğer öyleyse, **Akustik Yöneticisi**barındıran GameObject için aynı dönüşümü uygulayın.

### <a name="bake-time-vs-run-time-voxels"></a>Pişirme süresi vs çalışma süresi voxels
Oyun tasarım saatinde editör penceresinde ve çalışma zamanında oyun penceresinde voxels görüntülemek mümkündür. Voxels boyutu bu görünümlerde farklıdır. Bunun nedeni, akustik çalışma zamanı enterpolasyonunun daha düzgün enterpolasyon sonuçları için daha ince bir voxel ızgarası kullanmasıdır. Ses kaynağı yerleşimi çalışma zamanı voxels kullanılarak doğrulanmalıdır.

Tasarım süresi voxels:

![Tasarım süresi boyunca Project Acoustics voxels ekran görüntüsü](media/voxels-design-time.png)

Çalışma zamanı voxels:

![Çalışma süresi boyunca Project Acoustics voxels ekran görüntüsü](media/voxels-runtime.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Tasarım sürecinin](design-process.md) arkasındaki kavramları vurgulayan örnek çalışmaları keşfedin

