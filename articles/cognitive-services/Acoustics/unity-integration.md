---
title: Project Acoustics Unity entegrasyonu ve dağıtımı
titlesuffix: Azure Cognitive Services
description: Bu makalede, Project Acoustics Unity eklentisinin Unity projenize nasıl entegre ediletileni açıklanmaktadır.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: a8ddb0e4ca2ee4396a25a70c8b60b653aebb72d8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72243006"
---
# <a name="project-acoustics-unity-integration"></a>Project Acoustics Unity entegrasyonu
Bu makalede, Project Acoustics Unity eklentisinin Unity projenize nasıl entegre ediletileni açıklanmaktadır.

Yazılım gereksinimleri:
* Windows için [Unity 2018.2+](https://unity3d.com)
* [Proje Akustik Birlik paketi](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plug-in"></a>Eklentiyi alma
1. Akustik UnityPackage'ı projenize aktarın. 
 Birlik içinde, **Varlıklar** > **İthalat Paketi** > **Özel Paket**gidin.

    ![Birlik İthalat Paketi menüsü](media/import-package.png)  

1. **ProjectAcoustics.unitypackage'ı**seçin.

1. Birlik paketini projenize entegre etmek için **İçe Aktar** düğmesini seçin.

    ![Birlik İçe Alma Paketi iletişim kutusu](media/import-dialog.png)  

Eklentiyi varolan bir projeye aktarıyorsanız, projenizin proje kökünde zaten bir *mcs.rsp* dosyası olabilir. Bu dosya C# derleyicisine seçenekleri belirtir. Bu dosyanın içeriğini Project Acoustics eklentisiyle birlikte gelen mcs.rsp dosyasıyla birleştirin.

## <a name="enable-the-plug-in"></a>Eklentiyi etkinleştirme
Akustik araç setinin pişirme kısmı .NET 4 gerektirir. *x* komut dosyası çalışma zamanı sürümü. Paket alma, Unity oyuncu ayarlarınızı günceller. Bu ayarın etkili olması için Birlik'i yeniden başlatın.

![Unity Player Ayarları paneli](media/player-settings.png)

![.NET 4.5'in seçildiği Unity Player Ayarları paneli](media/net45.png)

## <a name="set-up-audio-dsp"></a>Ses DSP'yi ayarlama
Project Acoustics, Unity ses motoru mekanlaştırıcı çerçevesine entegre edilen ses çalışma süresi DSP içerir. Hem HRTF tabanlı hem de kaydırma mekansallaştırma içerir. Project Acoustics DSP'yi etkinleştirmek için, Unity ses ayarlarını açmak için**Proje Ayarları** > **Ses'i** **Edit'e** > gidin. Projeniz için **Uzlaştırıcı Eklentisi** olarak **Project Acoustics'i** seçin. **DSP Arabellek** *Boyutu'nun en iyi performans*olarak ayarlandıklarından emin olun.

![Birlik Proje Ayarları menüsü](media/project-settings.png)  

![Project Acoustics spatializer seçili Unity Spatializer ayarları paneli](media/choose-spatializer.png)

Sonra, Ses Karıştırıcı **(Pencere** > **Ses Karıştırıcı)** açın. En az bir mikser, bir grup ile olduğundan emin olun. Eğer yoksa, **+** **Mikserler'in**sağındaki düğmeyi seçin. Efektler bölümündeki kanal şeridinin altına sağ tıklayın ve **Microsoft Acoustics Mikser** efektini ekleyin. Aynı anda yalnızca bir Project Acoustics mikseri desteklenir.

![Project Acoustics mikserine ev sahipliği yapan Unity Audio Mikser](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Ses kaynaklarında akustik etkinleştirme
Ses kaynağı oluşturma: AudioSource denetçi panelinin altındaki **Spatialize** onay kutusunu seçin. **Uzamsal Karışım** tam *3D*olarak ayarlandıemin olun.  

![Birlik Ses Kaynak paneli](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Akustik tasarımı etkinleştirme
Ek kaynak tasarım parametrelerini etkinleştirmek için *Akustiği Ayarla* komut dosyasını sahnenizdeki bir ses kaynağına ekleyin: **Bileşen Ekle'yi** seçin ve **Komut Dosyası** > **Akustik Ayarla'yı**seçin.

![Unity AcousticsAdjust komut dosyası](media/acoustics-adjust.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Unity için Project Acoustics ile sahnenizi pişirin.](unity-baking.md)
* Sahnenizi bulutta pişirmek için [bir Azure Toplu İş hesabı oluşturun.](create-azure-account.md)
* Project [Acoustics Unity tasarım sürecini](unity-workflow.md)keşfedin.
