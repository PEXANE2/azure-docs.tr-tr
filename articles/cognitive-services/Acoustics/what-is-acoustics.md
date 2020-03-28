---
title: Proje Akustiğine Genel Bakış
titlesuffix: Azure Cognitive Services
description: Project Acoustics, pişmiş dalga fiziği simülasyonu ile interaktif tasarım kontrolleri birleştiren 3D interaktif deneyimler için bir akustik motorudur.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 65678f08399f378b8580eed79e49197dd4d84c64
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "71351142"
---
# <a name="what-is-project-acoustics"></a>Project Acoustics nedir?
Project Acoustics, 3D etkileşimli deneyimler için bir dalga akustiği motorudur. Manuel bölge biçimlendirmesi veya CPU yoğun ışınlama gerektirmeden karmaşık sahnelerde tıkanıklık, tıkanıklık, portallama ve yankılanma efektleri gibi dalga efektlerini modeller. Ayrıca oyun motoru ve ses ara tümleştirme içerir. Project Acoustics'in felsefesi statik aydınlatmaya benzer: fiziksel bir temel sağlamak için ayrıntılı fiziği çevrimdışı pişirin ve sanal dünyanızın akustiği için sanatsal hedeflerinize ulaşmak için etkileyici tasarım denetimleri ile hafif bir çalışma süresi kullanın.

![Gears of War 4'ten akustik voxels'i gösteren ekran görüntüsü](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Etkileşimli akustik için dalga fiziği kullanma
Ray tabanlı akustik yöntemleri, tek bir kaynaktan dinleyiciye ışın dökümkullanarak tıkanmayı kontrol edebilir veya yerel sahne hacmini birkaç ışınla tahmin ederek yankı yıkabilir. Ama bu teknikler güvenilmez olabilir çünkü bir çakıl taşı bir kaya kadar tıkalı. Işınlar, sesin nesnelerin etrafında bükülme şeklini hesaba katmaz, kırınım olarak bilinen bir fenomen. Project Acoustics'in simülasyonu bu efektleri dalga tabanlı bir simülasyon kullanarak yakalar. Akustik daha öngörülebilir, doğru ve sorunsuz.

Project Acoustics'in en önemli yeniliği, gerçek ses dalgası tabanlı akustik simülasyonu geleneksel ses tasarım konseptleriyle çiftlendirmektir. Bu oklüzyon, portaling ve yankı için geleneksel ses DSP parametreleri simülasyon sonuçları çevirir. Tasarımcı bu çeviri işlemi üzerindeki denetimleri kullanır. Project Acoustics'in arkasındaki temel teknolojiler hakkında daha fazla bilgi için [araştırma projesi sayfasını](https://www.microsoft.com/en-us/research/project/project-triton/)ziyaret edin.

![Bir sahnede yatay 2B dalga yayılımı dilimini gösteren animasyon](media/wave-simulation.gif)

## <a name="video-presentation-from-gdc-2019-30-min"></a>GDC 2019'dan Video Sunumu (~30 dk)
[![Proje Akustik Video](https://img.youtube.com/vi/uY4G-GUAQIE/0.jpg)](https://www.youtube.com/watch?v=uY4G-GUAQIE "Video oynatmak için tıklayın")

## <a name="setup"></a>Kurulum
[Project Acoustics Unity tümleştirmesi](unity-integration.md) sürükle ve bırak'dır ve Unity ses motoru eklentisi içerir. Her ses nesnesine bir Project Acoustics C# denetim bileşeni ekleyerek Unity ses kaynağı denetimlerini artırın.

[Project Acoustics Unreal entegrasyonu,](unreal-integration.md) Unreal için editör ve oyun eklentileri ve Wwise mikser eklentisi içerir. Özel bir ses bileşeni, canlı akustik tasarım kontrolleri ile Unreal içinde tanıdık Wwise işlevselliğini genişletir. Tasarım kontrolleri de mikser eklentisi wwise maruz kalır.

## <a name="workflow"></a>İş akışı
* **Ön fırında:** Örneğin ışık millerini görmezden gelerek, hangi geometrinin akustiğine yanıt veriştiğini seçerek fırını ayarlamaya başlayın. Ardından, dinleyici örneklemesine rehberlik etmek için otomatik malzeme atamalarını ve gezinme alanlarını seçin. Reverb/portal/oda bölgeleri için manuel biçimlendirme yoktur.
* **Fırında:** Yukarıdaki seçimlere göre sahnede voxelization ve diğer geometrik analizler yapan yerel olarak bir analiz adımı çalıştırılır. Sonuçlar, sahne kurulumlarını doğrulamak için düzenleyicide görselleştirilmiştir. Fırında gönderimde, voxel verileri Azure'a gönderilir ve bir akustik oyun varlığını geri alırsınız.
* **Çalışma süresi:** Varlığı seviyenize yükleyin ve seviyenizdeki akustiği dinlemeye hazır olun. Kaynak başına tanecikli kontroller kullanarak akustiği editörde canlı olarak tasarlayın. Denetimler, düzey komut dosyası ndan da sürülebilir.

## <a name="runtime-platforms"></a>Çalışma zamanı platformları
Project Acoustics çalışma zamanı eklentileri şu anda aşağıdaki platformlara dağıtılabilir:
* Windows
* macOS
* Android
* Xbox One

## <a name="editor-platforms"></a>Editör platformları
Project Acoustics düzenleyici eklentisi aşağıdaki platformlar için kullanılabilir:
* Windows
* MacOS (Yalnızca Birlik)

## <a name="download"></a>İndirme
* [Proje Akustik Unity eklentisi ve örnekleri](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [Proje Akustik Unreal & Wwise eklentileri ve örnekleri](https://www.microsoft.com/download/details.aspx?id=58090)
  * Xbox ikilileri ve diğer destek [için, forum](https://github.com/microsoft/ProjectAcoustics/issues)aracılığıyla bize ulaşın.

## <a name="contact-us"></a>Bizimle iletişim kurun
* [Proje Akustik tartışma ve konu raporlama](https://github.com/microsoft/ProjectAcoustics/issues)

## <a name="next-steps"></a>Sonraki adımlar
* Unity veya [Unreal](unreal-quickstart.md) için Project [Acoustics hızlı başlatış](unity-quickstart.md)
* Project [Acoustics'in ses tasarım felsefesini](design-process.md) keşfedin

