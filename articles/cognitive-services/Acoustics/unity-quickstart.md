---
title: Unity Ile Project Acoustics hızlı başlangıç
titlesuffix: Azure Cognitive Services
description: Örnek içerik kullanarak Unity 'de Project Acoustics tasarım denetimleriyle denemeler yapın ve Windows Masaüstü ' ne dağıtın.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 0ea020ca76381a4ae5d6b6e480c94e63f9aa2dab
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933113"
---
# <a name="project-acoustics-unity-quickstart"></a>Project Acoustics Unity hızlı başlangıç
Simülasyon ile desteklenen tasarım denetimleriyle denemek için Unity için Project Acoustics örnek içeriğini kullanın.

Yazılım gereksinimleri:
* Windows için [Unity 2018.2 +](https://unity3d.com)
* [Project Acoustics örnek içerik paketi](https://www.microsoft.com/download/details.aspx?id=57346)

Örnek pakete neler dahildir?
* Geometri, ses kaynakları ve oyunplay denetimleriyle Unity sahneyi
* Project Acoustics eklentisi 
* Sahne için bakmış Acoustics varlıkları

## <a name="import-the-sample-package"></a>Örnek paketi içeri aktarma
Örnek paketi yeni bir Unity projesine aktarın. 
* Unity 'de varlıklar ' a gidin **> paketi özel paket > Içeri aktar...**

    ![Unity Içeri aktarma paketi seçeneklerinin ekran görüntüsü](media/import-package.png)  

* **Projectacoustiği. unitypackage** seçin

* Unity paketini projenizle bütünleştirmek için **Içeri aktar** düğmesine tıklayın  
  
    ![Unity Içeri aktarma paketi iletişim kutusunun ekran görüntüsü](media/import-dialog.png)  

Paketi mevcut bir projeye aktardıysanız, ek adımlar ve notlar için bkz. [Unity tümleştirmesi](unity-integration.md) .

>[!NOTE] 
>İçeri aktarma işlemi tamamlandıktan sonra konsol günlüğünde birkaç hata görüntülenir.  Sonraki adıma geçin ve Unity 'yi yeniden başlatın.

## <a name="restart-unity"></a>Unity 'yi yeniden Başlat
Acoustics araç setinin hazırlama kısmı, .NET 4. x Scripting Runtime sürümü gerektirir. Paket içeri aktarma, Unity oynatıcı ayarlarınızı güncelleştirecek. Bu ayarın etkili olması için Unity 'yi yeniden başlatın.

**Oynatıcı ayarlarını**açarak bu ayarın geçerli olduğunu doğrulayabilirsiniz:

![Unity oynatıcı ayarları bölmesinin ekran görüntüsü](media/player-settings.png)  

![.NET 4,5 seçiliyken Unity oynatıcı ayarları panelinin ekran görüntüsü](media/net45.png)  

>[!NOTE]
>Bir ekran görüntüsü Unity 2018. x öğesinden alınmıştır. Unity 'nin daha yeni sürümleri farklılık gösterebilir.

## <a name="open-the-project-acoustics-bake-window"></a>Project Acoustics hazırlama penceresini açın
Unity menüsünden **pencere > Acoustics** seçin:

![Acoustics Window menü seçeneğinin vurgulandığı Unity düzenleyicisinin ekran görüntüsü](media/window-acoustics.png)

**Acoustics** adlı yeni bir kayan pencere görüntülenir.  Bu pencere, akustik benzetim özelliklerinin ayarlandığı yerdir.

![Acoustics penceresi açık olan Unity Düzenleyicisi ekran görüntüsü](media/unity-editor-plugin-window.png)  

## <a name="experiment-with-design-controls"></a>Tasarım denetimleriyle deneyin
**Projectacousticssample** klasöründe örnek sahneyi açın ve Unity düzenleyicisinde Oynat düğmesine tıklayın. Gezinmek için W, A, S, D ve fare kullanın. Sahnenin Acoustics ve olmadan nasıl seslendiğini karşılaştırmak için, kaplama metni kırmızıya dönüşene kadar **R** düğmesine basın ve "Acoustics: Devre dışı. " Diğer denetimlerin klavye kısayollarını görmek için **F1** tuşuna basın. Denetimler Ayrıca, gerçekleştirilecek eylemi seçmek için sağ tıklanarak ve sonra eylemi gerçekleştirmek için sola tıklanarak da kullanılır.

Betik **AcousticsAdjust** , örnek sahnedeki, kaynak başına tasarım parametrelerini sağlayan ses kaynaklarına iliştirilir. 

![Unity AcousticsAdjust betiğinin ekran görüntüsü](media/acoustics-adjust.png)

Aşağıdakiler, belirtilen denetimlerle üretilebilen bazı etkileri araştırır. Her denetim hakkında ayrıntılı bilgi için [Project Acoustics Unity tasarım öğreticisine](unity-workflow.md)bakın.

### <a name="modify-distance-based-attenuation"></a>Uzaklık tabanlı zayıflamayı değiştirme
**Project Acoustics** Unity spatializer eklentisi tarafından sunulan ses DSP 'Si, Unity düzenleyicisinde yerleşik olarak bulunan ve kaynak başına uzaklık tabanlı zayıflatılaşmaya uyar. Uzaklık tabanlı zayıflalama denetimleri, **3B ses ayarları**altında, ses kaynaklarının **Inspector** panelinde bulunan **ses kaynağı** bileşenidir:

![Unity uzaklık zayıflalama seçenekleri panelinin ekran görüntüsü](media/distance-attenuation.png)

Project Acoustics, bir "simülasyon bölgesi" kutusunda, Player konumu etrafında ortalanmış bir hesaplama gerçekleştirir. Örnek paketteki Acoustics varlıkları, Player 'ın simülasyonu bölge boyutu olan bir simülasyon bölgesi boyutuna bakdığından, ses zayıflalama 45 ile yaklaşık 0 ' a düşecek şekilde tasarlanmalıdır.

### <a name="modify-occlusion-and-transmission"></a>Occlusiyon ve iletimi değiştirme
* **Occlusiyon** çarpanı 1 ' den büyükse (varsayılan değer 1 ' dir), occlusiyon alınır. 1 ' den az ayarlandığında, occluson efekti daha hafif hale gelir.

* Duvar üzerinden iletimi etkinleştirmek için, **iletim (DB)** kaydırıcısını en düşük düzeye taşıyın. 

### <a name="modify-wetness-for-a-source"></a>Bir kaynak için wetliği değiştirme
* Mesafede ne kadar hızlı bir şekilde değişiklik yapıldığını değiştirmek için, **algısal Distance warp**' ı kullanın. **Project Acoustics** ,, mesafeden ayrılan alanın tamamında giyme düzeylerini hesaplar ve bu da, mesafeden farklı farklılık gösterir ve algısal uzaklık ipuçları sağlar. Uzaklık çarpmasının artırılması, mesafeyi ilgili ıslak düzeylerini artırarak bu etkiyi ayırır. 1 ' den küçük değerleri çarpıtma, uzaklığa göre ters geçiş değişikliğini daha hafif hale getirir. Bu efekt, **Wetlik (DB)** ayarlanarak daha ayrıntılı ayrıntı olarak da ayarlanabilir.

* **Decay zaman ölçeğini**ayarlayarak boşluk boyunca Decay süresini artırın. Belirli bir kaynak-dinleyici konumu çiftinin simülasyon sonucu 1,5 ' ın deklik süresi ise ve **zaman ölçeği** 2 olarak ayarlandıysa, kaynağa uygulanan Decay süresi 3S olur.

## <a name="next-steps"></a>Sonraki adımlar
* [Unity tabanlı proje Acoustics tasarım denetimlerinde](unity-workflow.md) tüm ayrıntıları okuyun
* [Tasarım sürecinin](design-process.md) arkasındaki kavramları daha fazla araştırın
* Ön ek ve hazırlama süreçlerini araştırmak için [bir Azure hesabı oluşturun](create-azure-account.md)

