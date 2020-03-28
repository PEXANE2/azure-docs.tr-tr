---
title: Unity ile Proje Akustik Quickstart
titlesuffix: Azure Cognitive Services
description: Unity'deki Project Acoustics tasarım denetimlerini denemek ve Windows Desktop'a dağıtmak için örnek içeriği kullanın.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: quickstart
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: fabdd221ef99414eae0156babbd76dedb1f0745d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72243015"
---
# <a name="project-acoustics-unity-quickstart"></a>Proje Akustik Birlik Quickstart
Simülasyon destekli tasarım denetimlerini denemek için Unity için Project Acoustics örnek içeriğini kullanın.

Yazılım gereksinimleri:
* Windows için [Unity 2018.2+](https://unity3d.com)
* [Project Acoustics örnek içerik paketi](https://www.microsoft.com/download/details.aspx?id=57346)

Örnek pakette neler var?
* Geometri, ses kaynakları ve oynanış kontrolleri ile birlik sahnesi
* Proje Akustik eklentisi
* Sahne için pişmiş akustik varlıklar

## <a name="import-the-sample-package"></a>Örnek paketi alma
Örnek paketi yeni bir Birlik projesine aktarın.
1. Birlik içinde, **Varlıklar** > **İthalat Paketi** > **Özel Paket**gidin.

    ![Birlik İthalat Paketi seçenekleri](media/import-package.png)  

1. **ProjectAcoustics.unitypackage'ı**seçin.

1. Birlik paketini projenize entegre etmek için **İçe Aktar** düğmesini seçin.  
  
    ![Birlik İçe Alma Paketi iletişim kutusu](media/import-dialog.png)  

Paketi varolan bir projeye almak için ek adımlar ve notlar için [Birlik tümleştirmesi'ne](unity-integration.md) bakın.

>[!NOTE]
>Alma işlemi tamamlandıktan sonra konsol günlüğünde birkaç hata iletisi görüntülenir. Bir sonraki adıma devam edin ve Birlik'i yeniden başlatın.

## <a name="restart-unity"></a>Birliği Yeniden Başlat
Akustik araç setinin pişirme kısmı .NET 4 gerektirir. *x* komut dosyası çalışma zamanı sürümü. Paket alma, Unity oyuncu ayarlarınızı güncelleştirir. Bu ayarın etkili olması için Birlik'i yeniden başlatın. Ayarın etkin olduğunu doğrulamak için **Player** ayarlarını açın:

![Birlik Proje Ayarları menüsü](media/player-settings.png)  

![.NET 4.x seçilmiş Unity Player Ayarları paneli](media/net45.png)  

>[!NOTE]
>Bu ekran görüntüsü Unity 2018'den alınmıştır. *x*. Resim, Unity'nin yeni sürümlerinde farklılık gösterebilir.

## <a name="open-the-project-acoustics-bake-window"></a>Project Acoustics fırın penceresini açın
Unity'de **Pencere** menüsünde **Akustik'i** seçin.

![Pencere menüsünde vurgulanan Akustik seçeneğine sahip Unity düzenleyicisi](media/window-acoustics.png)

Kayan bir **Akustik** penceresi açılır. Bu pencere, akustik simülasyonun özelliklerini ayarladığınız yerdir.

![Akustik penceresi açık Unity düzenleyicisi](media/unity-editor-plugin-window.png)  

## <a name="experiment-with-the-design-controls"></a>Tasarım denetimleri ile deneme
*ProjectAcousticsSample* klasöründeki örnek sahneyi açın ve Unity düzenleyicisindeki oynat düğmesini seçin. Hareket etmek için W, A, S ve D tuşlarını ve fareyi kullanın. Sahnenin akustikli ve akustiksiz seslerini karşılaştırmak için, bindirme metni kırmızıya dönene ve "Akustik: Devre Dışı" gösterene kadar R tuşunu seçin. Daha fazla denetim için klavye kısayollarını görmek için F1'i seçin. Ayrıca bir eylem seçmek için sağ tıklatın ve sonra bu eylemi yapmak için sol tıklatın.

*AcousticsAdjust* komut dosyası örnek sahnedeki ses kaynaklarına iliştirilir. Kaynak başına tasarım parametrelerini sağlar.

![Unity AcousticsAdjust komut dosyası](media/acoustics-adjust.png)

Aşağıdaki bölümlerde kullanılabilir denetimleri kullanarak oluşturabileceğiniz bazı efektler inceletir. Her kontrol hakkında ayrıntılı bilgi için [Project Acoustics Unity Design Tutorial](unity-workflow.md)bölümüne bakın.

### <a name="modify-distance-based-attenuation"></a>Mesafe tabanlı zayıflamayı değiştirme
**Project Acoustics** Unity spatializer eklentisindeki ses dijital sinyal işleme, Unity düzenleyicisi içine yerleşik olan kaynak başına mesafe tabanlı zayıflamaya saygı duyar. Mesafe tabanlı zayıflama için kontroller, **3B Ses Ayarları**altında ses kaynaklarının **Müfettiş** panelinde bulunan **Ses Kaynağı** bileşenindedir:

![Birlik mesafesi zayıflama seçenekleri paneli](media/distance-attenuation.png)

Project Acoustics, oyuncunun bulunduğu yerin etrafında ortalanmış bir "simülasyon bölgesi" kutusunda işlem eder. Örnek paketteki akustik varlıklar, oyuncunun etrafında 45 metre lik bir simülasyon bölgesi büyüklüğünde pişirildi. Bu nedenle, ses zayıflaması yaklaşık 45 m'de 0'a düşecek şekilde tasarlanmalıdır.

### <a name="modify-occlusion-and-transmission"></a>Oklüzyon ve iletimi değiştirin
* **Oklüzyon** çarpanı 1'den büyükse (varsayılan değer 1'dir), oklüzyon abartılır. Oklüzyon efektini daha ince hale getirmek için, 1'den az olarak ayarlayın.

* Duvardan iletimi etkinleştirmek **için, Şanzıman (dB)** kaydırıcısını en düşük ayardan uzağa taşıyın.

### <a name="modify-wetness-for-a-source"></a>Bir kaynak için ıslaklığı değiştirme
* ıslaklığın mesafeyle ne kadar hızlı değiştiğini değiştirmek için **Algısal Mesafe Çözgü'si**kullanın. Simülasyon sayesinde Project Acoustics, algısal mesafe ipuçları sağlayan ve mesafeyle sorunsuz bir şekilde değişen ıslak seviyeleri hesaplar. Mesafe çözgüsinin artırılması, mesafeye bağlı ıslak seviyeleri artırarak bu etkiyi abartmakta dır. 1'den küçük değerlerin çarpıtılması, mesafe tabanlı yankıdeğişimini daha ince hale getirin.

   Bu yönde daha hassas ayarlamalar yapmak için **ıslaklık (dB)** ayarını değiştirin.

* Boşluk boyunca çürüme süresini artırmak **için, Bozunma Zaman Ölçeğini**ayarlayın. Belirli bir kaynak dinleyici konum çifti için simülasyon sonucu 1,5 saniyelik bir bozunma süresi yse ve **Bozunma Süresi Ölçeği** 2 olarak ayarlanmışsa, kaynağa uygulanan bozunma süresi 3 saniyedir.

## <a name="next-steps"></a>Sonraki adımlar
* Unity tabanlı [Project Acoustics tasarım denetimleri](unity-workflow.md)hakkındaki ayrıntıları okuyun.
* [Tasarım sürecinin](design-process.md)arkasındaki kavramları daha fazla keşfedin.
* Ön pişirme ve pişirme işlemlerini keşfetmek için [bir Azure hesabı oluşturun.](create-azure-account.md)
