---
title: Proje Akustik Sık Sorulan Sorular
titlesuffix: Azure Cognitive Services
description: Bu sayfa, İndirme talimatları ve pişirme işlemi de dahil olmak üzere Project Acoustics hakkında sık sorulan soruların yanıtlarını sağlar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: fa4b6499260219b0eb8ea4df4b4ccfd5263b57bb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "75770212"
---
# <a name="project-acoustics-frequently-asked-questions"></a>Proje Akustik Sık Sorulan Sorular

## <a name="what-is-project-acoustics"></a>Project Acoustics nedir?

Project Acoustics eklentileri paketi, statik aydınlatmaya benzer şekilde çalışma zamanından önce ses dalgası davranışını hesaplayan bir akustik sistemdir. Bulut dalga fiziği hesaplamalarının ağır kaldırma yapar, bu nedenle çalışma zamanı CPU maliyeti düşüktür.  

## <a name="where-can-i-download-the-plugin"></a>Eklentiyi nereden indirebilirim?

[Project Acoustics Unity eklentisini](https://www.microsoft.com/download/details.aspx?id=57346) veya [Project Acoustics Unreal eklentisini](https://www.microsoft.com/download/details.aspx?id=58090)indirebilirsiniz.

## <a name="does-project-acoustics-support-ltxgt-platform"></a>Project Acoustics &lt;x&gt; platform'u destekliyor mu?

Project Acoustics platform desteği müşteri ihtiyaçlarına göre gelişir. Ek platformlar için destek hakkında bilgi almak için lütfen [Project Acoustics konu forumundan](https://github.com/microsoft/ProjectAcoustics/issues) bize ulaşın.

## <a name="is-azure-used-at-runtime"></a>Azure çalışma zamanında mı kullanılır?

Hayır, bulut tümleştirmesi yalnızca sahne kurulumunun bir parçası olarak ön işlem aşamasında kullanılır.
 
## <a name="what-is-simulation-input"></a>Simülasyon girişi nedir? 

Simülasyon girişi 3D sahne, sanal ortam veya oyun seviyesidir. Project Acoustics, düzgün oklüzyon ve saçılma dahil olmak üzere ses fiziğine yakından model veren 3D hacimsel dalga simülasyonları gerçekleştirir.
 
## <a name="what-is-the-runtime-cost"></a>Çalışma süresi maliyeti nedir?

Akustik, her kare de kaynak başına CPU'nun yaklaşık %0,01'ini alır. RAM kullanımı sahne boyutuna bağlıdır ve 10 ila 100 MB arasında değişebilir.
 
## <a name="do-i-need-to-simplify-the-level-geometry-control-triangle-count-make-meshes-watertight"></a>Seviye geometrisini basitleştirmem gerekiyor mu? Kontrol üçgeni sayısı mı? Meshes su geçirmez yapmak?

Hayır. Sistem doğrudan ayrıntılı seviye geometrisi yutacaktır. Bu dahili işleme için voxelized olacaktır.
 
## <a name="whats-in-the-runtime-lookup-table"></a>Çalışma zamanı arama tablosunda ne var?

ACE dosyası, çok sayıda kaynak ve dinleyici konum çiftleri arasındaki akustik parametrelerin yanı sıra parametre enterpolasyonu için kullanılan vokselize sahne geometrisi tablosunu içerir.
 
## <a name="can-project-acoustics-handle-moving-sources"></a>Project Acoustics hareketli kaynakları işleyebilir mi?

Evet, Project Acoustics arama tablosuna danışır ve her kenedeki ses DSP'yi günceller, böylece hareketli kaynakları ve dinleyiciyi işleyebilir.
 
## <a name="can-project-acoustics-handle-dynamic-geometry-closing-doors-walls-blown-away"></a>Project Acoustics dinamik geometriyi işleyebilir mi? Kapıları kapatmak mı? Duvarlar havaya mı uçtu?

Hayır. Akustik parametreler, oyun seviyesinin statik durumuna göre önceden hesaplanır. Kapı geometrisini akustikten çıkarmanızı ve daha sonra kurulu teknikleri kullanarak yok edilebilir ve taşınabilir oyun nesnelerinin durumuna göre ek tıkanıklık uygulamanızı öneririz.
 
## <a name="does-project-acoustics-use-acoustic-materials"></a>Project Acoustics akustik malzeme kullanıyor mu?

Evet. Malzemeler sizin seviyenizdeki fiziksel malzeme adlarından seçilir ve bu da emziteyi yönlendiren bir performans sağlar.
 
## <a name="what-do-the-probes-represent"></a>"Sondalar" neyi temsil ediyor?

Sondalar olası oyuncu konumlarının bir örneğidir. Her sonda, sonda konumundan kaynaklanan sahnenin ayrı bir dalga simülasyonunu temsil eder. Çalışma zamanında, dinleyici konumu için akustik parametreler yakındaki sonda konumlarından enterpolasyonludur.
 
## <a name="why-spend-so-much-compute-in-the-cloud-what-does-it-buy-me"></a>Neden bulutta bu kadar çok hesaplama harcıyorsun? Bana ne alıyor?

Project Acoustics, her mimari yönü göz önünde bulundurarak ultra karmaşık sanal ortamlar için bile doğru ve güvenilir akustik parametreler sağlar. Çizim hacimlerinin manuel çalışması olmadan düzgün oklüzyon ve tıkanıklık ve dinamik reverb varyasyonu sağlar. Çalışma süresi boyunca CPU'da kalan tüm ışık.

## <a name="what-exactly-happens-during-baking"></a>"Pişirme" sırasında tam olarak ne olur?

Bir fırında, her dinleyici sondası merkezli kübik simülasyon bölgelerinin akustik dalga simülasyonlarından oluşur.

## <a name="is-my-source-content-secure"></a>Kaynak içeriğim güvenli mi?

Project Acoustics kaynak sahne geometrisini buluta yüklemez. Bunun yerine simülasyon, sonda konum verileriyle birleştirilen ve özel bir biçimde depolanan sahnenizin voxelizasyonu nda çalışır.     

## <a name="next-steps"></a>Sonraki adımlar
* Project [Acoustics Unity örnek içeriğini](unity-quickstart.md) veya [Unreal örnek içeriğini](unreal-quickstart.md) deneyin

