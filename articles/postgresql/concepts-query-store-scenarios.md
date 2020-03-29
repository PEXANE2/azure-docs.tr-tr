---
title: Sorgu Mağazası senaryoları - PostgreSQL için Azure Veritabanı - Tek Sunucu
description: Bu makalede, PostgreSQL - Single Server için Azure Veritabanı'ndaki Sorgu Deposu için bazı senaryolar açıklanmaktadır.
author: rachel-msft
ms.author: raagyema
ms.service: postgresql
ms.topic: conceptual
ms.date: 5/6/2019
ms.openlocfilehash: 31e3f82b6ea1b1fc15c0832dc03edce2a59f1e1b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74768359"
---
# <a name="usage-scenarios-for-query-store"></a>Sorgu Deposu için kullanım senaryoları

**Aşağıdakiler için geçerlidir:** PostgreSQL için Azure Veritabanı - Tek Sunucu sürümleri 9.6, 10, 11

Sorgu Mağazası'nı, öngörülebilir iş yükü performansını izlemenin ve sürdürmenin kritik öneme sahip olduğu çok çeşitli senaryolarda kullanabilirsiniz. Aşağıdaki örnekleri inceleyin: 
- En pahalı sorguları tanımlama ve ala 
- A/B testi 
- Yükseltmeler sırasında performansı sabit tutma 
- Geçici iş yüklerinin tanımlanması ve iyileştirilmesi 

## <a name="identify-and-tune-expensive-queries"></a>Pahalı sorguları belirleme ve ayarlama 

### <a name="identify-longest-running-queries"></a>En uzun süre çalışan sorguları belirleme 
En uzun süre çalışan sorguları hızla tanımlamak için Azure portalındaki [Sorgu Performansı Öngörüsyün](concepts-query-performance-insight.md) görünümünü kullanın. Bu sorgular genellikle önemli miktarda kaynak tüketme eğilimindedir. En uzun süre çalışan sorularınızı optimize etmek, sisteminizde çalışan diğer sorgular tarafından kullanılmak üzere kaynakları serbest katarak performansı artırabilir. 

### <a name="target-queries-with-performance-deltas"></a>Performans deltaları ile hedef sorguları 
Sorgu Deposu performans verilerini zaman pencerelerine dilimler, böylece bir sorgunun performansını zaman içinde izleyebilirsiniz. Bu, harcanan toplam sürede tam olarak hangi sorguların artışa katkıda olduğunu belirlemenize yardımcı olur. Sonuç olarak, iş yükünüzün hedeflenmiş sorun giderme sini yapabilirsiniz.

### <a name="tuning-expensive-queries"></a>Pahalı sorguları tonlama 
En düşük performansa sahip bir sorgu tanımladığınızda, yaptığınız eylem sorunun yapısına bağlıdır: 
- Önerilen dizinler olup olmadığını belirlemek için [Performans Önerileri'ni](concepts-performance-recommendations.md) kullanın. Evet ise, dizin oluşturun ve dizin oluşturduktan sonra sorgu performansını değerlendirmek için Sorgu Deposu'nu kullanın. 
- Sorgu tarafından kullanılan temel tablolar için istatistiklerin güncel olduğundan emin olun.
- Pahalı sorguları yeniden yazmayı düşünün. Örneğin, sorgu parametrelendirmesi yararlanmak ve dinamik SQL kullanımını azaltmak. Veritabanı tarafında veri filtreleme uygulama gibi verileri okurken en iyi mantığı uygulayın, uygulama tarafında değil. 


## <a name="ab-testing"></a>A/B testi 
Başlatmayı planladığınız bir uygulama değişikliğinden önce ve sonra iş yükü performansını karşılaştırmak için Sorgu Mağazası'nı kullanın. Ortamın veya uygulama değişikliğinin iş yükü performansına etkisini değerlendirmek için Sorgu Mağazası'nı kullanma senaryolarına örnekler: 
- Uygulamanın yeni bir sürümünü kullanıma alma. 
- Sunucuya ek kaynak ekleme. 
- Pahalı sorgular tarafından başvurulan tablolarda eksik dizinler oluşturma. 
 
Bu senaryolardan herhangi birinde, aşağıdaki iş akışını uygulayın: 
1. Performans taban çizgisi oluşturmak için planlanan değişiklikten önce sorgu alanı ile iş yükünüzü çalıştırın. 
2. Uygulama değişikliğini zaman içinde kontrol edilen anda uygulayın. 
3. Değişiklikten sonra sistemin performans görüntüsünü oluşturmak için iş yükünü yeterince çalıştırmaya devam edin. 
4. Değişiklikten önceki ve sonraki sonuçları karşılaştırın. 
5. Değişikliği mi yoksa geri alma mı yapacağına karar verin. 


## <a name="identify-and-improve-ad-hoc-workloads"></a>Geçici iş yüklerini belirleme ve geliştirme 
Bazı iş yüklerinde, genel uygulama performansını artırmak için ayarlaabileceğiniz baskın sorgular yoktur. Bu iş yükleri genellikle, her biri sistem kaynaklarının bir bölümünü tüketen nispeten çok sayıda benzersiz sorguyla karakterize edilir. Her benzersiz sorgu seyrek olarak yürütülür, bu nedenle tek tek çalışma zamanı tüketimi kritik değildir. Diğer taraftan, uygulamanın her zaman yeni sorgular ürettiği göz önüne alındığında, sistem kaynaklarının önemli bir kısmı sorgu derlemesi için harcanmaktadır, bu da en iyi değildir. Genellikle, bu durum, uygulamanız sorgular oluşturursa (depolanmış yordamları veya parametreli sorguları kullanmak yerine) veya varsayılan olarak sorgu oluşturan nesne ilişkisieşleme çerçevelerine dayanıyorsa olur. 
 
Uygulama kodunun denetimindeyseniz, depolanan yordamları veya parametreli sorguları kullanmak için veri erişim katmanını yeniden yazmayı düşünebilirsiniz. Ancak, bu durum, tüm veritabanı (tüm sorgular) veya aynı sorgu karma ile tek tek sorgu şablonları için sorgu parametreleştirme zorlayarak uygulama değişiklikleri olmadan da geliştirilebilir. 

## <a name="next-steps"></a>Sonraki adımlar
- [Sorgu Mağazası'nı kullanmak için en iyi uygulamalar](concepts-query-store-best-practices.md) hakkında daha fazla bilgi edinin