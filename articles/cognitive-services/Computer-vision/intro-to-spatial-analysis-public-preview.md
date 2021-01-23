---
title: Görüntü İşleme uzamsal Analize Giriş
titleSuffix: Azure Cognitive Services
description: Bu belgede Görüntü İşleme uzamsal analiz kapsayıcısının temel kavramları ve özellikleri açıklanmaktadır.
services: cognitive-services
author: tchristiani
manager: nitinme
ms.author: terrychr
ms.service: cognitive-services
ms.subservice: computer-vision
ms.topic: conceptual
ms.date: 12/14/2020
ms.openlocfilehash: f90e4e5e187977f0ee77a565ff9143902ea3a10d
ms.sourcegitcommit: 78ecfbc831405e8d0f932c9aafcdf59589f81978
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/23/2021
ms.locfileid: "98736844"
---
# <a name="introduction-to-computer-vision-spatial-analysis"></a>Görüntü İşleme uzamsal Analize Giriş

Görüntü İşleme uzamsal analizler, kuruluşların belirli bir alanda bulunan hareketleri ve iletişim durumunu anlamak için fiziksel alanlarının değerlerini en üst düzeye çıkarmasına yardımcı olan Azure bilişsel hizmetler 'in yeni bir özelliğidir Görüntü İşleme. CCTV veya gözetim kameralarından video almanıza, video akışlarından Öngörüler çıkarmak için AI işlemlerini çalıştırmanıza ve diğer sistemler tarafından kullanılacak olayları oluşturmanıza olanak tanır. Bir kamera akışından gelen girişler sayesinde bir AI işlemi, yüz maskesi ve sosyal distancing yönergeleri ile bir boşluk veya ölçüm uyumluluğu girme gibi şeyler yapabilir.

## <a name="the-basics-of-spatial-analysis"></a>Uzamsal analizin temelleri

Günümüzde uzamsal çözümlemenin temel işlemleri, video al, videodaki kişileri algılayan, kişileri bir süre içinde hareket ettikleri sırada izleyen bir işlem hattı üzerine oluşturulmuştur ve insanlar ilgi alanları ile etkileşime geçtiğinde olaylar oluşturur.

## <a name="spatial-analysis-terms"></a>Uzamsal analiz koşulları

| Süre | Tanım |
|------|------------|
| İnsanların algılanması | Bu bileşen "Bu görüntüdeki kişiler nerede?" sorusunu yanıtlar. Bir görüntüde insanlar bulur ve kişilerin izleme bileşenine her birinin konumunu gösteren bir sınırlayıcı kutu geçirir. |
| Kişi Izleme | Bu bileşen, insanların bir kameranın önünde dolaştığından zaman içinde algılamaları bağlar. Bu, insanların genellikle kişilerin bu şekilde yaptığı genel görünüşler hakkında bilgi ve temel bilgiler hakkında zamana bağlı mantık kullanır. Birden çok kamera genelinde kişileri izlemez. Bir kişi, bir kameradan yaklaşık bir dakikadan uzun bir süre boyunca görünüm alanı içeriyorsa ve ardından kamera görünümünü yeniden girerse, sistem bunu yeni bir kişi olarak alır. İnsanlar Izleme, kameraları genelinde kişileri benzersiz bir şekilde tanımlamaz. Yüz tanıma veya yürüyüş izlemeyi kullanmaz. |
| Yüz maskesini algılama | Bu bileşen bir kişinin, kameranın görünüm alanındaki yüzünün konumunu algılar ve bir yüz maskesinin varlığını tanımlar. Bunu yapmak için, AI işlemi videonun görüntülerini tarar; bir yüz algılandığında hizmetin yüz etrafında bir sınırlayıcı kutu sağladığını burada bulabilirsiniz. Nesne algılama yeteneklerini kullanarak, sınırlayıcı kutudaki yüz maskeleri varlığını belirler. Yüz maskesini algılama, yüz özniteliklerini tahmin eden veya sınıflandırarak ya da yüz tanımayı gerçekleştirerek başka bir yüzden bir yüzü ayırt etme işlemini kapsamaz. |
| Ilgilendiğiniz bölge | Bu, yapılandırmanın bir parçası olarak giriş videosunda tanımlanan bir bölge veya satırdır. Bir kişi, videonun bölgesiyle etkileşime geçtiğinde sistem bir olay oluşturur. Örneğin, PersonCrossingLine işlemi için videoda bir satır tanımlanmıştır. Bir kişi bu satırı aştığında bir olay oluşturulur. |
| Olay | Bir olay, uzamsal çözümlemenin birincil çıktısından oluşur. Her işlem belirli bir olayı düzenli aralıklarla (örn.) yayar. dakikada bir kez veya belirli bir tetikleyici gerçekleştiğinde. Olay, giriş videosunda ne olduğu ile ilgili bilgiler içerir, ancak herhangi bir görüntü veya video içermez. Örneğin, PeopleCount işlemi, kişi sayısı (tetikleyici) veya her dakikada bir (düzenli olarak) her değiştiğinde güncelleştirilmiş sayımı içeren bir olay yayabilir. |

## <a name="example-use-cases-for-spatial-analysis"></a>Uzamsal analize yönelik örnek kullanım örnekleri

Aşağıda, bir uzamsal analiz tasarlıyoruz ve test ettiğimiz için göz önünde bulundurmanız gereken örnek kullanım örnekleri verilmiştir.

**Sosyal distancing uyumluluğu** -ofis alanında insanlar arasındaki mesafeyi ölçerek sosyal distancing uyumluluğunu izlemek için uzamsal analizler kullanan birkaç kamera bulunur. Tesisler Yöneticisi, çalışma alanını ayarlamak ve sosyal distancing kolaylaştırmak için zaman içinde sosyal distancing uyumluluğun toplam istatistiklerini gösteren ısı haritalarını 'ı kullanabilir.

**Alışverişçinin Analizi** -bir Market Mağazası, merchandising değişikliklerinin mağaza trafiğinden etkisini ölçmek için ürün ekranları işaret eden kameraları kullanır. Sistem, mağaza yöneticisinin hangi yeni ürünlerin görevlendirmede en fazla değişiklik olduğunu belirlemesine izin verir.

**Kuyruk yönetimi** -kullanıma alma sıralarında gösterilen kameralar, bekleme süresi çok uzun olduğunda yöneticilere uyarı sağlar ve bu da daha fazla satır açılmasına izin verir. Kuyruk bırakma sırasındaki geçmiş verileri, tüketici davranışına yönelik öngörüler sağlar.

**Yüz maskesi uyumluluğu** – Retail mağazalarda, mağaza 'da yürüyen müşterilerin, güvenlik uyumluluğunu korumak ve maske kullanım eğilimleriyle ilgili Öngörüler elde etmek için toplu istatistikleri analiz etmek üzere mağaza 'dan işaret eden kameraları kullanmasını sağlayabilirsiniz. 

**Doluluk & Analizi oluşturma** -bir Office derleme, ktfall ve insanların çalışma alanını nasıl kullandıkları hakkında önemli alanlara odaklanan kameraları kullanır. İçgörüler, Bina yöneticisinin, işlerinize daha iyi hizmet vermek için hizmet ve düzen ayarlamasına olanak tanır.

**En düşük personel algılama** -bir veri merkezinde, kameralar sunucular etrafında etkinlik izler. Çalışanlar hassas donanımları fiziksel olarak düzeltdiklerinde, her zaman güvenlik nedeniyle onarım sırasında iki kişinin bulunması gerekir. Kameralar, bu kılavuz 'in izlendiğini doğrulamak için kullanılır.

**Çalışma alanı iyileştirmesi** -hızlı bir şekilde restorana, çalışan iş akışı hakkında toplu bilgiler oluşturmak için, mutfak 'deki kameralar kullanılır. Bu, yöneticiler tarafından takım için süreçler ve eğitimi geliştirmek üzere kullanılır.

## <a name="considerations-when-choosing-a-use-case"></a>Kullanım durumu seçerken dikkat edilecek noktalar

**Kritik güvenlik uyarısı** verme-uzamsal analizler, kritik güvenli gerçek zamanlı uyarı için tasarlanmamıştır. Bir kişi mevcut olduğunda ağır bir makine parçasını kapatma gibi, yaralanmasını engellemek için müdahale tetiklemeye yönelik gerçek zamanlı uyarıların gerekli olduğu senaryolar için güvenmemelidir. Bu, sınırlı/yasak bir alan girme gibi riskli davranışı azaltmak için istatistikler ve müdahale kullanan risk azaltma için kullanılabilir.

**İstihdam ile ilgili kararlar Için kullanmaktan kaçının** -uzamsal analizler, bir alan içindeki kişilerin konumu ve hareketiyle ilgili dayalı ölçümleri sağlar. Bu veriler, toplama işlemi geliştirmesi için yararlı olabileceğinden, veriler tek tek çalışan performansının iyi bir göstergesi değildir ve işle ilgili kararlar vermek için kullanılmamalıdır.

**Sağlık hizmetleri ile ilgili kararlar Için kullanmaktan kaçının** -uzamsal analiz, kişilerin hareketlerine ilişkin dayalı ve kısmi veriler sağlar. Veriler, sistem durumu ile ilgili kararlar vermek için uygun değildir.

**Korumalı alanlarda kullanmaktan kaçının** -kamera konumlarını ve konumlarını değerlendirerek, kişilerin açılarını ve bölge alanlarını belirleyerek, restrooms gibi korumalı alanları fazla görünmemek için kişilerin gizliliğini koruyun.

**Okulların veya elderde dikkatli olma tesislerinde kullanımı dikkatlice göz önünde bulundurun** -uzamsal analiz, 18 yaş ve 65 yaş kapsamında olan sömürmeyi amaçlama 'ı içeren verilerle büyük ölçüde sınanmamıştır. Müşterilerin, senaryolarındaki bu yaştaki ortamlarda kendi senaryosuna yönelik hata hızlarını kapsamlı bir şekilde değerlendirmesini öneririz.

**Genel alanlarda kullanımı dikkatle değerlendirin** -ortak boşluklardan gelen toplamayı en aza indirmek için kamera konumlarını ve konumlarını değerlendirin, tüm ilgi alanları ve bölge bölgelerini değerlendirin. Cadde ve Park gibi genel alanlarda aydınlatma ve hava durumu, uzamsal analiz sisteminin performansını önemli ölçüde etkiler ve ortak alanlarda etkili bir açıklama sağlamak son derece zordur.

## <a name="spatial-analysis-gating-for-public-preview"></a>Genel önizleme için uzamsal analiz geçişi

Uzamsal analizler için tasarlanan senaryolar için kullanıldığından emin olmak için, bu teknolojiyi bir uygulama işlemi aracılığıyla müşteriler için kullanılabilir hale getiririz. Uzamsal Analize erişim sağlamak için, çevrimiçi alma formumuzu doldurarak başlamanız gerekir. [Uygulamanızı buradan başlatın](https://forms.office.com/Pages/ResponsePage.aspx?id=v4j5cvGGr0GRqy180BHbRyQZ7B8Cg2FEjpibPziwPcZUNlQ4SEVORFVLTjlBSzNLRlo0UzRRVVNPVy4u).

Uzamsal analiz Genel önizlemesine erişim, Microsoft 'un bu geçişli önizleme sırasında sınırlı sayıda müşteriyi desteklemeye yönelik uygunluk ölçütlerinizle, dileyici sürecimize ve kullanılabilirliğine bağlı olarak Microsoft 'un kendi takdirine tabidir. Genel önizlemede, Microsoft ile önemli bir ilişkiye sahip olan müşterilere bakıyoruz, önerilen kullanım durumlarında bizimle çalışmaya ilgileniyor ve sorumlu AI taahhütleriyle birlikte tutulması gereken ek senaryolar.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Uzamsal analize yönelik özellikler ve sınırlamalar](/legal/cognitive-services/computer-vision/accuracy-and-limitations?context=%2fazure%2fcognitive-services%2fComputer-vision%2fcontext%2fcontext)