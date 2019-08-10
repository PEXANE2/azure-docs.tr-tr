---
title: Project Acoustics Unity tümleştirmesi ve dağıtımı
titlesuffix: Azure Cognitive Services
description: Bu nasıl yapılır-proje Acoustics Unity eklentisinin Unity projenize tümleştirilmesini açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 54bc98e0ddba0292c6a5dbb07f2bbdfce6a1cb45
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933153"
---
# <a name="project-acoustics-unity-integration"></a>Project Acoustics Unity tümleştirmesi
Bu nasıl yapılır-proje Acoustics Unity eklentisinin Unity projenize tümleştirilmesini açıklar.

Yazılım gereksinimleri:
* Windows için [Unity 2018.2 +](https://unity3d.com)
* [Project Acoustics Unity paketi](https://www.microsoft.com/download/details.aspx?id=57346)

## <a name="import-the-plugin"></a>Eklentiyi içeri aktarma
Acoustics UnityPackage 'ı projenize aktarın. 
* Unity 'de varlıklar ' a gidin **> paketi özel paket > Içeri aktar...**

    ![Unity Içeri aktarma paketi menüsünün ekran görüntüsü](media/import-package.png)  

* **Projectacoustiği. unitypackage** seçin

* Unity paketini projenizle bütünleştirmek için **Içeri aktar** düğmesine tıklayın  

    ![Unity Içeri aktarma paketi iletişim kutusunun ekran görüntüsü](media/import-dialog.png)  

Eklentiyi var olan bir projeye aktardıysanız, projeniz proje kökünde bir **MCS. rsp** dosyasına sahip olabilir ve bu, C# derleyicinin seçeneklerini belirtir. Bu dosyanın içeriğini Project Acoustics eklentisi ile birlikte gelen MCS. rsp dosyası ile birleştirmeniz gerekir.

## <a name="enable-the-plugin"></a>Eklentiyi etkinleştirin
Acoustics araç setinin hazırlama kısmı, .NET 4. x Scripting Runtime sürümü gerektirir. Paket içeri aktarma, Unity oynatıcı ayarlarınızı güncelleştirecek. Bu ayarın etkili olması için Unity 'yi yeniden başlatın.

![Unity oynatıcı ayarları bölmesinin ekran görüntüsü](media/player-settings.png)

![.NET 4,5 seçiliyken Unity oynatıcı ayarları panelinin ekran görüntüsü](media/net45.png)

## <a name="set-up-audio-dsp"></a>Ses DSP 'si ayarlama
Project Acoustics, Unity Audio Engine spatializer çerçevesi ile tümleşen ses çalışma zamanı DSP 'sini içerir. Hem HRTF tabanlı hem de birlikte bulunan istenmeyen kullanım özelliğini içerir. Proje Acoustics DSP 'yi etkinleştirin **> proje ayarlarını düzenle > Ses**' i kullanarak Unity ses ayarlarını açıp projeniz Için **Spatializer eklentisi** olarak **Project Acoustics** ' yi seçin. **DSP arabellek boyutunun** en iyi performansa ayarlandığından emin olun.

![Unity proje ayarları bölmesinin ekran görüntüsü](media/project-settings.png)  

![Project Acoustics spatializer seçiliyken Unity Spatializer ayarları bölmesinin ekran görüntüsü](media/choose-spatializer.png)

Ardından ses karıştırıcı (**pencere > Ses karıştırıcı**) öğesini açın. Tek bir grup ile en az bir karıştırıcıya sahip olduğunuzdan emin olun. Aksi takdirde, **karıştırıcılar**' nın sağındaki ' + ' düğmesine tıklayın. Etkiler bölümünde kanal şeridinin altına sağ tıklayın ve **Project Acoustics Mixer** efektini ekleyin. Tek seferde yalnızca bir Project Acoustics Mixer 'in desteklendiğini unutmayın.

![Proje Acoustics Mixer 'i barındıran Unity ses karıştırıcı ekran görüntüsü](media/audio-mixer.png)

## <a name="enable-acoustics-on-sound-sources"></a>Acoustics 'i ses kaynaklarında etkinleştir
Bir ses kaynağı oluşturun. **Gereksiz**kaynak denetçisi panelinin altındaki onay kutusuna tıklayın. **Uzamsal Blend** 'In tam 3B olarak ayarlandığından emin olun.  

![Unity ses kaynağı panelinin ekran görüntüsü](media/audio-source.png)

## <a name="enable-acoustic-design"></a>Akustik tasarımı etkinleştir
Ek kaynak Tasarım parametrelerini etkinleştirmek için **AcousticsAdjust** betiğini sahne 'inizdeki bir ses kaynağına ekleyerek **Bileşen Ekle** ' ye tıklayıp **betikler > Acoustics ayarla**' ya tıklayın:

![Unity AcousticsAdjust betiğinin ekran görüntüsü](media/acoustics-adjust.png)

## <a name="next-steps"></a>Sonraki adımlar
* [Unity için Project Acoustics ile sahne bakın](unity-baking.md)
* Bulutta sahne [bakan bir Azure Batch hesabı oluşturun](create-azure-account.md)
* [Project Acoustics Unity tasarım sürecini](unity-workflow.md)gezin.

