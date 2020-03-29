---
title: Akustik Simülasyon ile Tasarım Kavramları
titlesuffix: Azure Cognitive Services
description: Bu kavramsal bakış, Project Acoustics'in ses tasarım sürecine akustik simülasyonu nasıl dahil ladığını açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 11e1e3f45b5198ddedb6c31fcd354185adef445d
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "68854303"
---
# <a name="project-acoustics-design-process-concepts"></a>Proje Akustik Tasarım Süreci Kavramları

Bu kavramsal bakış, Project Acoustics'in ses tasarım sürecine fiziksel akustik simülasyonu nasıl dahil ladığını açıklar.

## <a name="sound-design-with-audio-dsp-parameters"></a>Ses DSP parametreleri ile ses tasarımı

3D etkileşimli başlıklar, ses motorunda barındırılan ses dijital sinyal işleme (DSP) bloklarını kullanarak belirli seslerini elde ederler. Bu bloklar basit karıştırma, yankı, yankı, gecikme, dengeleme, sıkıştırma ve sınırlama ve diğer etkileri karmaşıklık aralığı. Bu efektler üzerinde parametrelerin seçilmesi, düzenlenmesi ve ayarlanması, deneyimin estetik ve oynanış hedeflerine ulaşabilen bir ses grafiği oluşturan ses tasarımcısının sorumluluğundadır.

Etkileşimli bir başlıkta, sesler ve dinleyici 3B alan boyunca ilerlerken, bu parametreler değişen koşullara nasıl uyum sağlar? Ses tasarımcısı genellikle yankı efektlerinde değişiklikler elde etmek için parametre değişikliklerini tetiklemek için programlanmış alan boyunca hacimleri düzenler, örneğin, ya da dinleyici sahnenin bir bölümünden diğerine geçerken karışımdaki sesleri örer. Bu etkilerden bazılarını otomatikleştirebilen akustik sistemler de mevcuttur.

3D başlıklar, daldırma ve oyun hedeflerinin bir karışımını elde etmek için fizik odaklı ama tasarımcı tarafından ayarlanan aydınlatma ve kinematik fizik sistemleri kullanır. Görsel bir tasarımcı tek tek piksel değerlerini ayarlamaz, daha çok görsel estetik ve CPU maliyetlerini kapatmak için fiziksel tabanlı 3B modelleri, malzemeleri ve ışık taşıma sistemlerini ayarlar. Ses için eşdeğer süreç ne olurdu? Project Acoustics bu sorunun araştırılmasında ilk adımdır. Önce akustik enerjiyi bir boşluktan taşımanın ne anlama geldiğini konuşacağız.

![Reverb bölgeleri ile kaplanmıştır AltSpace sahneekran görüntüsü](media/reverb-zones-altspace.png)

## <a name="impulse-responses-acoustically-connecting-two-points-in-space"></a>Dürtü tepkileri: Uzayda iki noktayı akustik olarak birbirine bağlama

Ses tasarımına aşinaysanız, akustik dürtü yanıtlarını biliyor olabilirsiniz. Akustik dürtü tepkisi, bir sesin bir kaynaktan dinleyiciye taşınmasını modeller. Bu nedenle bir dürtü tepkisi oda akustiğinin tıkanma ve yankılanma gibi her ilginç etkisini yakalayabilir. Impuls yanıtları da ses DSP efektleri ölçeklendirmek için izin bazı güçlü özelliklere sahiptir. İki ses sinyalinibir araya getirmek ve bir impuls yanıtı ile işleme, impuls yanıtını her sinyale ayrı ayrı uygulamak ve sonuçları eklemekle aynı sonucu verir. Akustik yayılma ve impuls tepkileri de işlenen sese bağlı değildir, yalnızca modellenen sahneye ve kaynak ve dinleyici konumları'na bağlıdır. Kısacası, bir dürtü tepkisi ses yayılımı üzerinde sahnenin etkisini damıtıyor.

Bir dürtü tepkisi her ilginç oda akustik efektyakalar, ve biz bir filtre ile verimli bir ses uygulayabilirsiniz, ve ölçüm veya simülasyon dürtü yanıtları alabilirsiniz. Ama ya akustiğin fiziği tam olarak eşleştirmesini değil, bir sahnenin duygusal taleplerine uygun olmasını istemiyorsak? Ama piksel değerleri gibi, dürtü tepkisi sadece binlerce sayının bir listesidir, estetik ihtiyaçları karşılamak için nasıl ayarlayabiliriz? Peki ya kapılardan geçerken ya da engellerin arkasından geçerken sorunsuz bir şekilde değişen tıkanıklık/tıkanıklık istiyorsak, düzgün bir etki elde etmek için kaç dürtü yanıtına ihtiyacımız var? Ya kaynak hızlı hareket ederse? Nasıl interpolate edeceğiz?

Bu interaktif başlıklar akustik bazı yönleri için simülasyon ve dürtü yanıtları kullanmak zor geliyor. Ama yine de bizim tanıdık ses DSP etki parametreleri ile simülasyon bizim dürtü yanıtları bağlayabilir eğer tasarımcı ayarlamaları destekleyen bir ses taşıma sistemi inşa edebilirsiniz.

## <a name="connecting-simulation-to-audio-dsp-with-parameters"></a>Simülasyonu ses DSP'sine parametrelerle bağlama

Bir dürtü yanıtı her ilginç (ve her ilginç olmayan) akustik etki içerir. Ses DSP blokları, parametreleri düzgün ayarlandığında, ilginç akustik etki yapabilir. Bir 3B sahnede ses taşıma otomatikleştirmek için bir ses DSP blok sürücü akustik simülasyon kullanarak bir dürtü yanıtı ses DSP parametreleri ölçme sadece bir konudur. Bu ölçüm oklüzyon, tıkanıklık, portalling ve yankılanma gibi bazı yaygın ve önemli akustik etkiler için iyi anlaşılmaktadır.

Ama simülasyon doğrudan ses DSP parametrelerine bağlı ise, nerede tasarımcı ayarı nedir? Ne kazandık? İtici tepkileri atarak ve birkaç DSP parametresini koruyarak önemli miktarda hafıza kazanıyoruz. Ve tasarımcıya nihai sonuç üzerinde biraz güç vermek için, sadece simülasyon ve ses DSP arasında tasarımcı eklemek için bir yol bulmak gerekir.

![Parametreleri kaplanmış stilize impuls yanıtı ile grafik](media/acoustic-parameters.png)

## <a name="sound-design-by-transforming-audio-dsp-parameters-from-simulation"></a>Ses DSP parametrelerini simülasyondan dönüştürerek ses tasarımı

Güneş gözlüğünüzün dünya görüşünüz üzerindeki etkisini göz önünde bulundurun. Parlak bir günde, gözlük daha rahat bir şey parlaklık azaltabilir. Karanlık bir odada hiçbir şey göremeyebilirsin. Gözlük her durumda parlaklık belirli bir düzeyde ayarlamak yok; Her şeyi daha karanlık hale getirsinler.

Ses DSP'mizi oklüzyon ve yankı parametrelerini kullanarak sürmek için simülasyon kullanırsak, DSP'nin 'gördüğü' parametreleri ayarlamak için simülatörden sonra bir filtre ekleyebiliriz. Filtre oklüzyon veya reverb kuyruk uzunluğu belirli bir düzeyde zorlamak olmaz, çok güneş gözlüğü gibi her odada aynı parlaklık yapmazlar. Filtre sadece her oklüzokde daha az yapabilir. Ya da daha fazla tıkama. Bir 'karartma' oklüzyon parametre filtresi ekleyerek ve ayarlayarak, büyük, açık odalar hala çok az veya hiç oklüzyon etkisi olurdu, kapı lar bir ortamdan güçlü bir tıka lık etkisine artarken, etki geçişlerinde yumuşaklığı korurken simülasyon sağlar.

Bu paradigmada, tasarımcının görevi her durum için akustik parametreleri seçmekten, simülasyondan gelen en önemli DSP parametrelerine uygulanacak filtreleri seçmeye ve ayarlamaya kadar değişir. Bu oklüzyon ve yankı etkileri ve karışımı kaynakların varlığı yoğunluğu daha yüksek endişeleri yumuşak geçişler kurma dar endişeleri tasarımcının faaliyetlerini yükseltir. Tabii ki, durum gerektirdiğinde, bir filtre her zaman sadece belirli bir durumda belirli bir kaynak için DSP parametrelerini seçerek geri dönmektir.

## <a name="sound-design-in-project-acoustics"></a>Project Acoustics'te ses tasarımı

Project Acoustics paketi yukarıda açıklanan bileşenlerin her birini entegre eder: bir simülatör, parametreleri ayıklayan ve akustik varlık, ses DSP ve filtre seçimi oluşturur bir kodlayıcı. Project Acoustics ile ses tasarımı, simülasyondan elde edilen ve ses DSP'sine uygulanan ve oyun editörü ve ses motorunun içinde açıkta olan tıkanma ve yankı parametrelerini ayarlayan filtreler için parametrelerin seçilmesini gerektirir.

## <a name="next-steps"></a>Sonraki adımlar
* [Unity için Project Acoustics quickstart'ı](unity-quickstart.md) veya [Unreal için Project Acoustics quickstart'ı](unreal-quickstart.md) kullanarak tasarım paradigmasını deneyin
* Unity [için Project Acoustics tasarım denetimlerini](unity-workflow.md) veya [Unreal için Project Acoustics tasarım denetimlerini](unreal-workflow.md) keşfedin

