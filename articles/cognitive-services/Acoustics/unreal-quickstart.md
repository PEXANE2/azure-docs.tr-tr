---
title: Proje Acoustics hızlı başlangıç-gerçek olmayan
titlesuffix: Azure Cognitive Services
description: Örnek içerik kullanarak, proje Acoustics tasarım denetimleriyle gerçek olmayan ve Wwise 'ta deneyin ve Windows Masaüstü ' ne dağıtın.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: 06023b2758d09fe8ebe7c1301ef1a03d9c54aa41
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68704766"
---
# <a name="project-acoustics-unrealwwise-quickstart"></a>Proje Acoustics Unreal/Wwise hızlı başlangıç
Bu hızlı başlangıçta, Unreal Engine ve Wwise için sunulan örnek içeriği kullanarak Project Acoustics tasarım denetimleriyle denemeler yapacaksınız.

Yazılım gereksinimleri:
* [Unreal Engine](https://www.unrealengine.com/) 4,21
* [Audiokinetik Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1.6

## <a name="download-the-sample-package"></a>Örnek paketi indirin
[Project Acoustics Unreal + Wwise örnek paketini](https://www.microsoft.com/download/details.aspx?id=58090)indirin. Örnek paket gerçek olmayan bir altyapı projesi, gerçek olmayan projenin Wwise projesi ve Project Acoustics Wwise eklentisi içerir.

## <a name="set-up-the-project-acoustics-sample-project"></a>Project Acoustics örnek projesini ayarlama
Project Acoustics Unreal/Wwise örnek projesini ayarlamak için, önce Project Acoustics eklentisini Wwise 'a yüklemeniz gerekir. Ardından Wwise ikililerini gerçek olmayan projeye dağıtıp Wwise 'ın Unreal eklentisini proje Acoustics destekleyecek şekilde ayarlayın.

### <a name="install-the-project-acoustics-wwise-plugin"></a>Projeyi Acoustics Wwise eklentisini yükler
Wwise başlatıcısı 'nı açın, **Eklentiler** sekmesinde **yeni eklentiler yüklensin**bölümünde **dizinden Ekle**' yi seçin. İndirdiğiniz pakette bulunan dizini seçin. `AcousticsWwisePlugin\ProjectAcoustics`

![Wwise başlatıcısı eklentisini yüklemeyi gösteren Wwise başlatıcısı ekran görüntüsü](media/wwise-install-new-plugin.png)

### <a name="add-wwise-binaries-to-the-project-acoustics-unreal-sample-project"></a>Proje Acoustics Unreal örnek projesine Wwise ikilileri ekleme
Wwise Başlatıcısı ' ndan, **Unreal Engine** sekmesine tıklayın, ardından **en son Unreal Engine projelerinin** yanındaki hamburger menüsüne tıklayın ve **proje için araştır**' ı seçin. `.uproject` Pakette`AcousticsSample\AcousticsGame\AcousticsGame.uproject`gerçek olmayan örnek proje dosyasını açın.

![Wwise başlatıcısı 'nın gerçek olmayan sekmesinin ekran görüntüsü](media/wwise-unreal-tab.png)

Ardından, Project Acoustics örnek projesinin yanında, **projede Wwise 'ı tümleştir**' e tıklayın.

![Acoustics oyununu gerçek olmayan projeyi gösteren Wwise başlatıcısı ekran görüntüsü](media/wwise-acoustics-game-project.png)

### <a name="extend-wwises-unreal-plugin-functionality"></a>Wwise 'ın gerçek olmayan eklenti işlevselliğini Genişlet
Project Acoustics Unreal eklentisi, Wwise Unreal Plugin API 'sinden ek davranış gösterilmesini gerektirir. Bu değişiklikleri otomatikleştirmek için Project Acoustics Unreal eklentisi ile birlikte sunulan toplu iş dosyasını çalıştırın:
* İçinde `AcousticsGame\Plugins\ProjectAcoustics\Resources`, öğesini `PatchWwise.bat`çalıştırın.

    ![Wwise projesine yama için betiği gösteren Windows Gezgini penceresinin ekran görüntüsü](media/patch-wwise-script.png)

* DirectX SDK yüklü değilse, kullanmakta olduğunuz Wwise sürümüne bağlı olarak, `DXSDK_DIR` içinde `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`yer alan satırı açıklamanız gerekebilir:

    ![DXSDK açıklamalı açıklaması gösteren kod Düzenleyicisi ekran görüntüsü](media/directx-sdk-comment.png)

### <a name="open-the-unreal-project"></a>Gerçek olmayan projeyi açın. 
Bu, modülleri yeniden oluşturmanız istenir; Evet ' e tıklayın.

>Projeyi açmak derleme hatalarıyla başarısız olursa, Project Acoustics Wwise eklentisini Project Acoustics örnek projesinde kullanılan Wwise sürümüne yüklediğinizden emin olun.

>[Audiokinetic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1.6 kullanmıyorsanız, ses bankalarını, örnek projede sesçmadan önce yeniden oluşturmanız gerekecektir.

## <a name="experiment-with-project-acoustics-design-controls"></a>Project Acoustics tasarım denetimleriyle deneyin
Gerçek olmayan düzenleyicideki Oynat düğmesine tıklayarak sahnenin nasıl seslere dinleyebildiğini dinleyin. Masaüstünde, gezinmek için W, A, S, D ve fare kullanın. Diğer denetimlerin klavye kısayollarını görmek için **F1** tuşuna basın. Deneyebileceğiniz bazı tasarım etkinlikleri aşağıda verilmiştir:

### <a name="modify-occlusion-and-transmission"></a>Occlusiyon ve iletimi değiştirme
Her bir gerçek olmayan ses aktör üzerinde kaynak başına proje Acoustics tasarım denetimleri vardır:

![Gerçek olmayan düzenleyici Acoustics tasarım denetimlerinin ekran görüntüsü](media/demo-scene-sound-source-design-controls.png)

**Occlusiyon** çarpanı 1 ' den büyükse (varsayılan değer 1 ' dir), occlusiyon alınır. 1 ' den az ayarlandığında, occluson efekti daha hafif hale gelir.

Duvar üzerinden iletimi etkinleştirmek için, **iletim (DB)** kaydırıcısını en düşük düzeye taşıyın. 

### <a name="modify-wetness-for-a-source"></a>Bir kaynak için wetliği değiştirme
Mesafede ne kadar hızlı bir şekilde değişiklik yapıldığını değiştirmek için, **algısal Distance warp**' ı kullanın. Project Acoustics,, mesafeden ayrılan alanın tamamında giyme düzeylerini hesaplar ve bu da, mesafeden farklı farklılık gösterir ve algısal uzaklık ipuçları sağlar. Uzaklık çarpmasının artırılması, mesafeyi ilgili ıslak düzeylerini artırarak bu etkiyi ayırır. 1 ' den küçük değerleri çarpıtma, uzaklığa göre ters geçiş değişikliğini daha hafif hale getirir. Bu efekt, **Wetlik (DB)** ayarlanarak daha ayrıntılı ayrıntı olarak da ayarlanabilir.

**Zaman ölçeğini zaman ölçeğini**ayarlayarak boşluk boyunca Decay süresini artırın. Simülasyon sonucunun 1,5 ' lik bir süre olduğu durumu göz önünde bulundurun. **Decay zaman ölçeğini** 2 olarak ayarlamak, 3 s kaynağına uygulanan bir zaman ile sonuçlanır.

### <a name="modify-distance-based-attenuation"></a>Uzaklık tabanlı zayıflamayı değiştirme
Project Acoustics Wwise Mixer eklentisi, Wwise 'ta oluşturulan kaynak başına uzaklık tabanlı zayıflamayı uyar. Bu eğrinin değiştirilmesi, Kuru yol düzeyini değiştirecek. Project Acoustics eklentisi, benzetim ve tasarım denetimleri tarafından belirtilen ıslak kuru karışımını sürdürmek için ıslak düzeyini ayarlar.

![Simülasyon sınırının önüne sıfır ile devam eden zayıflalama ile Wwise zayıflalama eğrisi bölmesinin ekran görüntüsü](media/demo-sounds-attenuation.png)

Project Acoustics, her bir sanal oynatıcı konumunun etrafında ortalanan bir "simülasyon bölgesi" kutusunda hesaplama yapar. Örnek paketteki Acoustics varlıkları, 45 ile bir simülasyon bölge yarıçapı ile fırlaştırılmış ve zayıflatılmaları, 45 No 'dan önce 0 ' a düşecek şekilde tasarlandı. Bu azalma kesin bir gereksinim olmadığından, yalnızca dinleyicinin 45/' de yer alan geometrinin, sesleri occlım olarak yürütür.

## <a name="next-steps"></a>Sonraki adımlar
* [Project Acoustics](unreal-integration.md) eklentisini gerçek olmayan projenizden tümleştirin
* Kendiniz içerik hazırlamak için [bir Azure hesabı oluşturun](create-azure-account.md)


