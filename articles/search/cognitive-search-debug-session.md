---
title: Hata ayıklama oturumları kavramları (Önizleme)
titleSuffix: Azure Cognitive Search
description: Azure portal üzerinden erişilen hata ayıklama oturumları, AI zenginleştirme ardışık düzeninde hataları tanımlayabilir ve giderebileceğiniz, değişiklikleri doğrulayabileceğiniz ve değişiklikleri becerileri 'e ileten bir IDE sağlar. Hata ayıklama oturumları önizlemededir.
manager: nitinme
author: tchristiani
ms.author: terrychr
ms.service: cognitive-search
ms.topic: conceptual
ms.date: 05/19/2020
ms.openlocfilehash: edce8f3460e92eef4f6665b1b38a61582d6841ad
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85560386"
---
# <a name="debug-sessions-in-azure-cognitive-search"></a>Azure Bilişsel Arama 'de hata ayıklama oturumları

Hata ayıklama oturumları, Azure portal var olan bir beceri ile birlikte çalışarak bir görsel düzenleyicidir. Bir hata ayıklama oturumunda hataları tanımlayabilir ve çözümleyebilir, değişiklikleri doğrulayabilir ve değişiklikleri AI zenginleştirme ardışık düzeninde bir üretim beceri gönderebilirsiniz.

> [!Important]
> Hata ayıklama oturumları, bir hizmet düzeyi sözleşmesi olmadan sunulan bir önizleme özelliğidir ve üretim iş yükleri için önerilmez. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).
>

## <a name="using-debug-sessions"></a>Hata ayıklama oturumlarını kullanma

Bir oturum başlattığınızda, hizmet, beceri sınamak için tek bir belgenin kullanıldığı Beceri, dizin oluşturucunun ve dizinin bir kopyasını oluşturur. Oturum içinde yapılan değişiklikler oturuma kaydedilir. Değişiklikler kendisine uygulanmamışsa, hata ayıklama oturumunda yapılan değişiklikler üretim beceri etkilemez. Değişiklikleri yürütmek üretim beceri üzerine yazılacak.

Bir hata ayıklama oturumu sırasında, bir beceri düzenleyebilir, İnceleme ve belirli bir belge bağlamında zenginleştirme ağacındaki her bir düğümü doğrulayabilirsiniz. Enzenginleştirme ardışık düzen sorunları çözümlendikten sonra, değişiklikler oturuma kaydedilebilir ve üretimde beceri 'e kaydedilmiş olabilir. 

Enzenginleştirme ardışık düzeninde herhangi bir hata yoksa, bir belgeyi artımlı olarak zenginleştirmek için bir hata ayıklama oturumu kullanılabilir, değişiklikleri uygulamadan önce her değişikliği test edebilir ve doğrulayabilir.

## <a name="creating-a-debug-session"></a>Hata ayıklama oturumu oluşturma

Bir hata ayıklama oturumu başlatmak için, dahil olmak üzere mevcut bir AI zenginleştirme ardışık düzenine sahip olmanız gerekir; bir veri kaynağı, bir beceri, Dizin Oluşturucu ve bir dizin. Bir hata ayıklama oturumu yapılandırmak için oturumu yeniden yazmanız ve Dizin Oluşturucu çalıştırması sırasında yetenek yürütmelerinin önbelleğe almak için kullanılacak bir genel amaçlı depolama hesabı sağlamanız gerekir. Ayrıca, çalıştırılacak Dizin oluşturucuyu seçmeniz gerekecektir. Dizin oluşturucunun, veri kaynağına, beceri ve dizine depolanmış başvuruları vardır. Hata ayıklama oturumu, veri kaynağındaki ilk belgeyi varsayılan olarak belirler veya veri kaynağında adım adım ilerlemek için bir belge belirtebilirsiniz.

> [!div class="mx-imgBorder"]
> ![Hata ayıklama oturumu oluşturma](media/cognitive-search-debug/debug-session-new.png)

## <a name="debug-session-features"></a>Hata ayıklama oturumu özellikleri

Hata ayıklama oturumu, seçilen belgedeki beceri yürütülerek başlar. Hata ayıklama oturumu, Beceri içindeki her işlemle ilişkili ek meta verileri kaydeder. İşlem hattının beceri yürütmeleri tarafından oluşturulan meta veriler, beceri ile ilgili sorunları belirlemenize ve gidermeye yardımcı olmak için kullanılan aşağıdaki özellikler kümesini bilgilendirir.

## <a name="ai-enrichments"></a>AI zenginleştirme

Yetenekler, belgeyi temsil eden bir veya daha fazla zenginleştirme ağacı yürütür. Beceri çıkışları veya zenginler için bir ağaç kullanmak, gerçekleştirilen tüm zenginleştirme hakkında kapsamlı bir görünüm sağlar. Tüm belgeye bakabilir ve zenginleştirme ağacının her bir düğümünü inceleyebilirsiniz. Bu perspektif, nesneleri şekillendirip daha kolay hale getirir. Bu biçim Ayrıca, ağaçtaki her bir düğümün türü, yolu ve içeriği için görsel ipucu sağlar.

## <a name="skill-graph"></a>Beceri grafiği

**Beceri grafiği** görünümü beceri hiyerarşik, görsel bir gösterimini sağlar. Grafik, yeteneklerin yürütüldüğü sırayı en alta doğru temsil eden bir gösterimidir. Diğer yeteneklerin çıktısına bağlı olan yetenekler, grafikte daha düşük görüntülenir. Hiyerarşide aynı düzeydeki yetenekler paralel olarak çalıştırılabilir. 

Grafikteki bir yeteneğin seçilmesi kendisine bağlı olan becerileri, girişlerini oluşturan düğümleri ve çıktılarını kabul eden düğümleri vurgulayacaktır. Her yetenek düğümü, türünü, hatalarını veya uyarılarını ve yürütme sayılarını görüntüler. **Beceri grafiği** , hangi beceriye hata ayıklama veya geliştirme olacağını seçtirecek yerdir. Bir yetenek seçtiğinizde, ayrıntılar, grafiğin sağındaki yetenek ayrıntıları bölmesinde görüntülenir.

> [!div class="mx-imgBorder"]
> ![Beceri grafiği](media/cognitive-search-debug/skills-graph.png)

## <a name="skill-details"></a>Yetenek ayrıntıları

Yetenek ayrıntıları bölmesi, yetenek **grafiğinde**bu yetenek vurgulandığında belirli bir yetenek ile çalışmaya yönelik bir alan kümesi görüntüler. Beceri ayarlarının ayrıntılarını gözden geçirebilir ve düzenleyebilirsiniz. Yeteneğin JSON tanımı sağlanır. Beceriye ait yürütmenin ve hataların ve uyarıların ayrıntıları da görüntülenir. Yetenek **ayarları** sekmesi & beceri **JSON Düzenleyicisi** , yeteneğin doğrudan düzenlenmesine izin verir. , [`</>`](#expression-evaluator) Yetenekler girişlerinin ve çıktıların ifadelerini görüntülemek ve düzenlemekte bir pencere açar.

Yetenek ayarları penceresindeki iç içe geçmiş giriş denetimleri, tahminler için karmaşık şekiller, karmaşık bir tür alanı için çıkış alanı eşlemeleri veya bir beceriye giriş oluşturmak için kullanılabilir. **İfade değerlendirici**ile kullanıldığında, iç içe geçmiş girişler kolay test ve doğrulama ifadesi oluşturucuyu sağlar.

## <a name="skill-execution-history"></a>Beceri yürütme geçmişi

Bir yetenek, tek bir belge için bir beceri içinde birden çok kez çalıştırılabilir. Örneğin, her belgeden ayıklanan her bir görüntü için OCR becerisi bir kez yürütülür. Yetenek ayrıntıları bölmesinde **yürütmeler** sekmesi, becerinin her bir çağrısına daha derin bir bakış sağlayan becerinin yürütme geçmişini görüntüler. 

Yürütme geçmişi, belirli bir zenginleştirme tarafından oluşturulan yeteneğe geri izlenmesini sağlar. Bir yetenek girişine tıkladığınızda, bu girişi oluşturan yeteneğe gider. Bu durum, bir aşağı akış beceriye bildirimde bulunan bir sorunun kök nedeninin tanımlanmasını sağlar. 

Olası bir sorun tanımlandığında, yürütme geçmişi belirli girdileri oluşturan yeteneklerin bağlantılarını görüntüleyerek bir yığın izleme gibi bir özellik sunar. Bir girişle ilişkili beceriye tıkladığınızda, tüm hataları gidermeye yönelik yeteneğe gider veya belirli bir sorunu izlemeye devam edin.

Özel bir beceri oluştururken veya özel bir beceriyle hata ayıkladığında, yürütme geçmişinde bir yetenek çağrısı için istek oluşturma seçeneği vardır.

## <a name="enriched-data-structure"></a>Zenginleştirilmiş veri yapısı

**Zenginleştirilmiş veri yapısı** bölmesi, her bir zenginleştirme ve kaynak yeteneğin bağlamını ayrıntılandıran beceri aracılığıyla belgenin enzenginleştirmelerinin olduğunu gösterir. **İfade değerlendiricisi** , her bir zenginleştirme içeriğini görüntülemek için de kullanılabilir.

> [!div class="mx-imgBorder"]
> ![Zenginleştirilmiş veri yapısı](media/cognitive-search-debug/enriched-data-structure-display.png)

## <a name="expression-evaluator"></a>İfade değerlendirici

**İfade değerlendirici** herhangi bir yolun değerine hızlı bir göz atma sağlar. Bir beceri veya projeksiyon için girişlerin veya bağlamın güncelleştirilmeden önce yolun düzenlenmesine ve sonuçların test edilmesine olanak tanır.

## <a name="errorswarnings"></a>Hatalar/uyarılar

Bu pencerede, beceri tarafından hata ayıklama oturumunda belgeye karşı yürütüldüğü şekilde ürettiği tüm hatalar ve uyarılar görüntülenir.

## <a name="limitations"></a>Sınırlamalar

Hata ayıklama seslerininin tüm genel kullanıma açık veri kaynaklarıyla, en çok önizleme veri kaynaklarıyla çalışır. Cosmos DB MongoDB API (Önizleme) ve Cassandra API (Önizleme) Şu anda desteklenmiyor.

## <a name="next-steps"></a>Sonraki adımlar

Artık hata ayıklama oturumlarının öğelerini anladığınıza göre, uygulamalı bir deneyim için öğreticiyi deneyin.

> [!div class="nextstepaction"]
> [Hata ayıklama oturumlarını keşfet Özellik öğreticisi](https://docs.microsoft.com/azure/search/cognitive-search-tutorial-debug-sessions)