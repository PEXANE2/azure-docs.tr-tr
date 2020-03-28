---
title: Unreal ile Project Acoustics hızlı bir başlangıç
titlesuffix: Azure Cognitive Services
description: Unreal ve Wwise'daki Project Acoustics tasarım denetimlerini denemek ve Windows masaüstüne dağıtmak için örnek içeriği kullanın.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: d3afcded894f72626a4f24bcbe85c34ac1329c29
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72242915"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Proje Akustik Unreal / Wwise quickstart
Bu hızlı başlangıçta, Unreal Engine ve Wwise için örnek içerik kullanarak Project Acoustics tasarım denetimlerini deneyeceksiniz.

Örnek içeriği kullanmak için yazılım gereksinimleri:
* [Gerçek Dışı Motor](https://www.unrealengine.com/) 4.22
* [AudioKintic Wwise](https://www.audiokinetic.com/products/wwise/) 2019.1.2

## <a name="download-the-sample-package"></a>Örnek paketi indirin
Project [Acoustics Unreal ve Wwise örnek paketini](https://www.microsoft.com/download/details.aspx?id=58090)indirin. Örnek paket şunları içerir:
- Unreal Engine projesi
- Unreal projesi için Wwise projesi
- Proje Akustik Wwise eklentisi

## <a name="set-up-the-project-acoustics-sample-project"></a>Project Acoustics örnek projesini ayarlama
İlk olarak, Project Acoustics eklentisini Wwise'a takın. Ardından, Wwise ikililerini Unreal projesine dağıtın. Ardından, Project Acoustics'i desteklemek için Wwise Unreal eklentisini ayarlayın.

### <a name="install-the-project-acoustics-wwise-plug-in"></a>Project Acoustics Wwise eklentisini yükleyin
Wwise Launcher'ı açın. **Eklentiler** sekmesinde, **Yeni Eklentileri Yükle'nin**altında, **dizinden ekle'yi**seçin. İndirdiğiniz pakete dahil edilen *AcousticsWwisePlugin\ProjectAcoustics* dizinini seçin.

![Wwise Başlatıcısı'na Wwise eklentisini yükleme seçeneği](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Project Acoustics Unreal örnek projesine Wwise ikilileri ekleyin
1. Wwise Launcher'da **Unreal Engine** sekmesini seçin. 
1. **Son Gerçek Dışı Motor Projeleri'nin**yanındaki "hamburger" (simge) menüsünü seçin ve ardından proje için **Gözat'ı**seçin. *Akustik/Akustik\AkustikOyun\Uproject*paketinde örnek Unreal project *.uproject* dosyasını açın.

   ![Wwise Launcher unreal sekmesi](media/wwise-unreal-tab.png)

3. Project Acoustics örnek projesinin yanında, **Projeye Tümle Wwise'ı**seçin.

   ![Wwise Launcher, Akustik Oyunu Unreal projesini Tümleştirme seçeneği yle birlikte vurgular.](media/wwise-acoustics-game-project.png)

### <a name="extend-wwise-unreal-plug-in-functionality"></a>Wwise Unreal eklenti işlevini genişletin
Project Acoustics Unreal eklentisinin Wwise Unreal eklentisi API'sinden açığa çıkan ek davranışlara ihtiyacı vardır. Bu değişiklikleri otomatikleştirmek için Project Acoustics Unreal eklentisiyle birlikte gelen toplu dosyayı çalıştırın.
* Inside *AcousticsGame\Plugins\ProjectAcoustics\Resources*, *run PatchWwise.bat*.

    ![Windows Explorer penceresinde vurgulanan Wwise projesini yamalamak için komut dosyası](media/patch-wwise-script.png)

* DirectX SDK yüklü yoksa: Wwise sürümüne bağlı olarak kullandığınız, `DXSDK_DIR` *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs*içeren satırı dışarı yorum gerekebilir:

    !["DXSDK" gösteren kod düzenleyicisi yorumladı](media/directx-sdk-comment.png)

* Visual Studio 2019'u kullanarak derlerseniz: Wwise ile bağlantı hatası nın çözümünü yapmak **için,** *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs'de* varsayılan *değeri* manuel olarak değiştirin:

    ![VSVersion ile kod düzenleyicisi "vc150" olarak değiştirildi](media/vsversion-comment.png)

### <a name="open-the-unreal-project"></a>Unreal Projesini Açın 
Unreal projesini açtığınızda, modülleri yeniden oluşturmanızı ister. **Evet'i**seçin.

Projeyi açma, yapı hataları nedeniyle başarısız olursa, Project Acoustics Wwise eklentisini Project Acoustics örnek projesinde kullanılan Wwise'ın aynı sürümüne yüklediğinizi denetleyin.

[AudioKintic Wwise](https://www.audiokinetic.com/products/wwise/) sürümünü 2019.1 sürümünden daha önce kullanıyorsanız, Project Acoustics örnek projesini kullanarak ses bankaları oluşturamazsınız. Wwise sürüm 2019.1'i örnek projeye entegre etmeniz gerekir.

## <a name="experiment-with-project-acoustics-design-controls"></a>Project Acoustics tasarım kontrolleri ile denemeler
Unreal editöründeki oynat düğmesini seçerek sahnenin nasıl göründüğünü dinleyin. Hareket etmek için W, A, S ve D tuşlarını ve fareyi kullanın. Ek denetimler için klavye kısayollarını görmek için F1'i seçin.

Aşağıdaki bilgiler, denemek için bazı tasarım etkinlikleri açıklar.

### <a name="modify-occlusion-and-transmission"></a>Oklüzyon ve iletimi değiştirin
Her Unreal ses aktöründe kaynak başına Project Acoustics tasarım denetimleri vardır.

![Unreal Editor akustik tasarım kontrolleri](media/demo-scene-sound-source-design-controls.png)

**Oklüzyon** çarpanı 1'den büyükse (varsayılan değer 1'dir), oklüzyon abartılır. 1'den küçük bir ayar, oklüzyon efektini daha ince hale getirir.

Duvardan iletimi etkinleştirmek **için, Şanzıman (dB)** kaydırıcısını en düşük seviyesinden uzağa taşıyın.

### <a name="modify-wetness-for-a-source"></a>Bir kaynak için ıslaklığı değiştirme
ıslaklığın mesafeyle ne kadar hızlı değiştiğini değiştirmek için **Algısal Mesafe Çözgü'si**kullanın. Project Acoustics simülasyon yoluyla uzay boyunca ıslak seviyeleri hesaplar. Düzeyleri mesafe ile sorunsuz değişir ve algısal mesafe ipuçları sağlar. Bu etkiyi abartmak için, mesafe çözgü artırmak için mesafe ile ilgili ıslak düzeylerini artırmak. 1'den küçük değerlerin çarpıtılması, mesafe tabanlı yankıdeğişimini daha ince hale getirin. Iskan **(dB)** ayarı ile bu yönde daha ince ayarlamalar da yapabilirsiniz.

Boşluk boyunca çürüme süresini artırmak **için, Bozunma Zaman Ölçeğini**ayarlayın. Simülasyon sonucunun 1,5 saniyelik bir çürüme süresi olduğu bir durum düşünün. **Çürüme Süresi Ölçeği'ni** kaynağa uygulanan 3 saniyelik bozunma süresiyle 2 sonuç olarak ayarlamak.

### <a name="modify-distance-based-attenuation"></a>Mesafe tabanlı zayıflamayı değiştirme
Project Acoustics Wwise mikser eklentisi, Wwise'da yerleşik olan kaynak başına mesafe tabanlı zayıflamaya saygı duyar. Bu eğrinin değiştirilmesi kuru yol düzeyini değiştirir. Project Acoustics eklentisi, simülasyon ve tasarım kontrolleri tarafından belirtilen ıslak/kuru karışımı korumak için ıslak seviyeyi ayarlar.

![Wwise zayıflama eğrisi paneli, simülasyon sınırından önce 0'a giden zayıflamayı gösterir](media/demo-sounds-attenuation.png)

Project Acoustics, simüle edilen her oyuncu konumunun etrafında ortalanmış bir "simülasyon bölgesi" kutusunda işlem eder. Örnek paketteki akustik varlıklar 45 metrelik bir simülasyon bölgesi yarıçapı ile pişirildi. Zayıflama 45 metreden önce 0'a düşecek şekilde tasarlanmıştır. Bu düşüş sıkı bir gereklilik olmasa da, dinleyicinin 45 metre içindeki sadece geometrinin sesleri tıkadığı uyrunu taşır.

## <a name="next-steps"></a>Sonraki adımlar
* [Project Acoustics eklentisini](unreal-integration.md) Unreal projenize entegre edin.
* Kendi fırınlarınız için [bir Azure hesabı oluşturun.](create-azure-account.md)
