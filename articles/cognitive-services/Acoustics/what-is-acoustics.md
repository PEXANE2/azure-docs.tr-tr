---
title: Project Acoustics genel bakış
titlesuffix: Azure Cognitive Services
description: Project Acoustics, etkileşimli tasarım denetimleriyle, bakmış dalga fizik benzetimini tümleştirerek, 3B etkileşimli deneyimler için bir Acoustics altyapısıdır.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: overview
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: f0685b65ce9204221e93a1ea4aa47a5e09eaac9f
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68932974"
---
# <a name="what-is-project-acoustics"></a>Project Acoustics nedir?
Project Acoustics, 3B etkileşimli deneyimler için bir Wave Acoustics altyapısıdır. El ile bölge işaretlemesi gerektirmeden, karmaşık sahnelerde oylama, portallama ve yankı etkileri gibi dalga efektlerini modellemez. Ayrıca oyun altyapısı ve ses ara yazılım tümleştirmesi de içerir. Project Acoustics ' fanlar, statik aydınlatmaya benzerdir: fiziksel bir taban çizgisi sağlamak için bakmalı ve daha ayrıntılı tasarım denetimleriyle sanatsal hedeflerinizi karşılamak için basit bir çalışma zamanı kullanın.

![War 4 Acoustics voxars gösteren Gears 'ın ekran görüntüsü](media/gears-with-voxels.jpg)

## <a name="using-wave-physics-for-interactive-acoustics"></a>Etkileşimli Acoustics için Wave fizik kullanma
Ray tabanlı Acoustics yöntemleri, tek bir kaynaktan dinleyici Ray cast kullanarak tahmini veya birkaç ışına sahip yerel sahne birimini tahmin ederek sürücü yankı düzeyini kontrol edebilir. Ancak, bir Pebble OCC, bir Boulder kadar olduğu için bu teknikler güvenilir olmayabilir. Işın, nesnelerin etrafında ses çıkarımları için bir hesap değil, korkma olarak bilinirdi. Project Acoustics ' simülasyonu, bu etkileri bir dalga tabanlı benzetim kullanarak yakalar. Sonuç daha öngörülebilir ve güvenilir.

Project Acoustics ' önemli yeniliği, geleneksel ses tasarımı kavramlarıyla birlikte iki akustik simülasyonu sağlar. Simülasyon sonuçlarını occlusiyon, portallama ve Reverb için geleneksel ses DSP parametrelerine dönüştürür. Tasarımcı bu çeviri işlemi üzerinde denetimleri kullanır. Project Acoustics 'ın arkasındaki temel teknolojiler hakkında daha fazla bilgi için [Araştırma Projesi sayfasını](https://www.microsoft.com/en-us/research/project/project-triton/)ziyaret edin.

![Sahne aracılığıyla yatay 2B bir dalga yayma dilimini gösteren animasyon](media/wave-simulation.gif)

## <a name="setup"></a>Kurulum
[Project Acoustics Unity tümleştirmesi](unity-integration.md) , sürükle ve bırak Ile bir Unity ses altyapısı eklentisi içerir. Her ses nesnesine bir Project Acoustics C# Controls bileşeni ekleyerek Unity ses kaynağı denetimlerini güçlendirin.

[Project Acoustics Unreal Integration](unreal-integration.md) , Unreal için düzenleyici ve oyun eklentileri ve bir Wwise karıştırıcı eklentisi içerir. Özel bir ses bileşeni, canlı Acoustics tasarım denetimleriyle gerçek olmayan tanıdık Wwise işlevselliğini genişletir. Tasarım denetimleri, karıştırıcı eklentisi üzerinde Wwise 'ta da sunulur.

## <a name="workflow"></a>İş akışı
* **Ön Bake:** Acoustics 'e hangi geometrinin yanıt verdiğini seçerek, örneğin, Light shafts yok sayarak başlayın. Sonra otomatik malzeme atamalarını düzenleyin ve dinleyici örneklemesini kılavuza kadar gezinme alanı seçin. Reverb/Portal/Oda bölgeleri için el ile biçimlendirme yoktur.
* **Hazırlama** Bir çözümleme adımı yerel olarak çalıştırılır, bu, yukarıdaki seçimlere bağlı olarak sahne üzerinde voxelileştirme ve diğer geometrik analizler yapar. Sonuçlar, sahne kurulumunun doğrulanması için düzenleyicide görselleştirilir. Hazırlama gönderimi sırasında, Voxel verileri Azure 'a gönderilir ve bir Acoustics oyun varlığını geri alırsınız.
* **Çalışma zamanı** Varlığı, düzeyinize yükleyin ve Acoustics dinlemek için hazırsınız demektir. Kaynak başına ayrıntılı denetimleri kullanarak Acoustics canlı düzenleyici 'yi tasarlayın. Denetimler Ayrıca düzey komut dosyası içinden de yönetilebilir.

## <a name="runtime-platforms"></a>Çalışma zamanı platformları
Project Acoustics çalışma zamanı eklentileri şu anda aşağıdaki platformlara dağıtılabilir:
* Windows
* macOS
* Android
* Xbox One

## <a name="editor-platforms"></a>Düzenleyici platformları
Project Acoustics Düzenleyicisi eklentisi aşağıdaki platformlar için kullanılabilir:
* Windows
* MacOS (yalnızca Unity)

## <a name="download"></a>İndirme
* [Project Acoustics Unity eklentisi ve örnekleri](https://www.microsoft.com/en-us/download/details.aspx?id=57346)
* [Proje Acoustics Unreal & Wwise eklentileri ve örnekleri](https://www.microsoft.com/download/details.aspx?id=58090)
  * Xbox ikilileri ve destek için aşağıdaki kaydolma formu aracılığıyla bizimle iletişim kurun

## <a name="contact-us"></a>Bizimle iletişim kurun
* [Project Acoustics tartışma ve sorun raporlama](https://github.com/microsoft/ProjectAcoustics/issues)
* [Project Acoustics üzerinde güncelleştirmeler almak için kaydolun](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRwMoAEhDCLJNqtVIPwQN6rpUOFRZREJRR0NIQllDOTQ1U0JMNVc4OFNFSy4u)

## <a name="next-steps"></a>Sonraki adımlar
* [Unity Için proje Acoustics hızlı başlangıç](unity-quickstart.md) veya [Unreal](unreal-quickstart.md) için deneme
* [Project Acoustics 'in ses tasarımı felsefesini](design-process.md) keşfet

