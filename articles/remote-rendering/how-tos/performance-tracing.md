---
title: İstemci tarafı performans izlemeleri oluşturma
description: WPF kullanarak istemci tarafı performans profilleme için en iyi uygulamalar
author: florianborn71
ms.author: flborn
ms.date: 12/11/2019
ms.topic: conceptual
ms.openlocfilehash: 1f4207a11f3ae3664023fccf6178b6db7cf253b9
ms.sourcegitcommit: 642a297b1c279454df792ca21fdaa9513b5c2f8b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/06/2020
ms.locfileid: "80681317"
---
# <a name="create-client-side-performance-traces"></a>İstemci tarafı performans izlemeleri oluşturma

Azure Uzaktan İşleme performansının istenilen kadar iyi olmamasının birçok nedeni vardır. Bulut sunucusundaki saf görüntüleme performansının yanı sıra, özellikle ağ bağlantısının kalitesi deneyim üzerinde önemli bir etkiye sahiptir. Sunucunun performansını profillemek için, bölüm [Sunucu tarafı performans sorgularına](../overview/features/performance-queries.md)bakın.

Bu *bölümde, performans izlemeleri*aracılığıyla olası istemci tarafındaki darboğazların nasıl belirlenecek olarak tanımlanmaları üzerinde duruluyor.

## <a name="getting-started"></a>Başlarken

Windows performans izleme işlevinde yeniyseniz, bu bölümde başlamanıza yardımcı olacak en temel terimler ve uygulamalardan bahsedilir.

### <a name="installation"></a>Yükleme

ARR ile izleme yapmak için kullanılan uygulamalar, tüm Windows geliştirme için kullanılabilecek genel amaçlı araçlardır. Bunlar [Windows Performans Araç Seti](https://docs.microsoft.com/windows-hardware/test/wpt/)aracılığıyla sağlanır. Bu araç kitini almak için [Windows Değerlendirme ve Dağıtım Kiti'ni](https://docs.microsoft.com/windows-hardware/get-started/adk-install)indirin.

### <a name="terminology"></a>Terminoloji

Performans izlemeleri hakkında bilgi ararken, kaçınılmaz olarak bir dizi terimle karşılaşabilirsiniz. En önemlileri şunlardır:

* `ETW`
* `ETL`
* `WPR`
* `WPA`

**ETW** [ **W**indows için **E**vent **T**yarış anlamına](https://docs.microsoft.com/windows/win32/etw/about-event-tracing)gelir. Windows'da yerleşik olan verimli çekirdek düzeyinde izleme olanağının yalnızca kapsamlı adıdır. ETW'yi destekleyen uygulamalar performans sorunlarını izlemek için yardımcı olabilecek eylemleri günlüğe kaydetmek için etkinlik *izleme* olarak adlandırılır. Varsayılan olarak, işletim sistemi zaten disk erişimleri, görev anahtarları ve benzeri şeyler için olaylar yayan. ARR gibi uygulamalar ayrıca, örneğin bırakılan çerçeveler, ağ gecikmesi vb. hakkında özel olaylar yarar.

**ETL** **E**vent **T**yarış **L**ogging anlamına gelir. Bu yalnızca bir izlemenin toplandığı (günlüğe kaydedilmiş) olduğu ve bu nedenle genellikle izleme verilerini depolayan dosyalar için dosya uzantısı olarak kullanıldığı anlamına gelir. Bu nedenle, bir izleme yaptığınızda, \*genellikle daha sonra bir .etl dosyası olacaktır.

**WPR,** [ **W**indows **P**erformance **R**ecorder](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-recorder) anlamına gelir ve olay izlerinin kaydını başlatan ve durduran uygulamanın adıdır. WPR, hangi olayları\*tam olarak günlüğe kaydetmek için yapılandıran bir profil dosyası (.wprp) alır. Böyle `wprp` bir dosya ARR SDK ile sağlanır. Bir masaüstü bilgisayarda izleme ler yaparken, WPR'yi doğrudan başlatabilirsiniz. HoloLens üzerinde bir izleme yaparken, genellikle yerine web arayüzü üzerinden gidin.

**WPA,** [ **W**indows **P**erformance **A**nalyzer](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer) anlamına gelir ve .etl dosyalarını açmak \*ve performans sorunlarını belirlemek için verileri elemek için kullanılan GUI uygulamasının adıdır. WPA, verileri çeşitli ölçütlere göre sıralamanızı, verileri çeşitli şekillerde görüntülemenizi, ayrıntıları araştırmanızı ve bilgileri ilişkilendirmenizi sağlar.

ETL izlemeleri herhangi bir Windows aygıtında (yerel PC, HoloLens, bulut sunucusu, vb.) oluşturulabilirken, genellikle diske kaydedilir ve bir masaüstü bilgisayarda WPA ile analiz edilir. ETL dosyaları, diğer geliştiricilere bir göz atabilmeleri için gönderilebilir. Ancak dosya yolları ve IP adresleri gibi hassas bilgilerin ETL izlerinde yakalanıp alınabileceğini unutmayın. ETW'yi iki şekilde kullanabilirsiniz: izleri kaydetmek veya izleri çözümlemek. İzleme leri kaydetmek doğrudan ileridir ve minimum kurulum gerektirir. Öte yandan izleri analiz wpa aracı ve araştırdığınız sorun hem iyi bir anlayış gerektirir. WPA öğrenme için genel malzeme aşağıda verilecektir, yanı sıra arr özgü izleri yorumlamak için nasıl yönergeleri.

## <a name="recording-a-trace-on-a-local-pc"></a>Yerel bir bilgisayarda izleme kaydetme

ARR performans sorunlarını belirlemek için doğrudan HoloLens üzerinde izleme yapmayı tercih etmelisiniz, çünkü gerçek performans özelliklerinin anlık görüntüsünü elde etmenin tek yolu bu. Ancak, özellikle HoloLens performans kısıtlamaları olmadan bir izleme yapmak istiyorsanız veya sadece WPA nasıl kullanılacağını öğrenmek istiyorsanız ve gerçekçi bir izleme gerekmez, burada bunu yapmak için nasıl.

### <a name="wpr-configuration"></a>WPR yapılandırması

1. Başlat menüsünden [Windows Performans Kaydedicisini](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-recorder) *başlatın.*
1. **Diğer Seçenekleri** Genişlet
1. **Profil Ekle'yi tıklatın...**
1. *AzureRemoteRenderingNetworkProfiling.wprp*dosyasını seçin. Bu dosyayı ARR SDK'da *Araçlar/ETLProfiles*altında bulabilirsiniz.
   Profil artık *Özel ölçümler*altında WPR listelenecektir. Tek etkin profil olduğundan emin olun.
1. *Birinci seviye triajı*genişlet:
    * Tek yapmak istediğiniz ARR ağ olaylarının hızlı bir izini yakalamaksa, bu seçeneği **devre dışı edin.**
    * ARR ağ olaylarını CPU veya bellek kullanımı gibi diğer sistem özellikleriyle ilişkilendirmeniz gerekiyorsa, bu seçeneği **etkinleştirin.**
    * Bu seçeneği etkinleştirirseniz, izleme büyük olasılıkla boyutu birden çok gigabayt olacak ve kaydetmek ve WPA açık uzun bir zaman alır.

Daha sonra WPR yapılandırmanız şu şekilde görünmelidir:

![WPR yapılandırması](./media/wpr.png)

### <a name="recording"></a>Kayıt

İzleme kaydetmeye başlamak için **Başlat'ı** tıklatın. İstediğiniz zaman kaydı başlatıp durdurabilirsiniz; bunu yapmadan önce uygulamanızı kapatmanız gerekmez. Gördüğünüz gibi, ETW her zaman tüm sistem için bir izleme kaydedecektir gibi, hangi uygulama izlemek için belirtmenize gerek yoktur. Dosya, `wprp` hangi olay türlerinin kaydedilen olduğunu belirtir.

Kaydı durdurmak ve ETL dosyasını nerede depolayacağınızda belirtmek için **Kaydet'i** tıklatın.

Artık doğrudan WPA'da açabileceğiniz veya başka birine gönderebileceğiniz bir ETL dosyanız var.

## <a name="recording-a-trace-on-a-hololens"></a>HoloLens'te iz kaydetme

HoloLens'te bir izleme kaydetmek için cihazınızı başlatın ve *Cihaz Portalını*açmak için IP adresini tarayıcıya girin.

![Cihaz Portalı](./media/wpr-hl.png)

1. Solda, *Performans > Performans İzleme'ye*gidin.
1. **Özel profilleri** seçin
1. **Gözat'ı tıklatın...**
1. *AzureRemoteRenderingNetworkProfiling.wprp*dosyasını seçin. Bu dosyayı ARR SDK'da *Araçlar/ETLProfiles*altında bulabilirsiniz.
1. **İzle Başlat'ı** tıklatın
1. HoloLens şu anda bir iz kaydediyor. Araştırmak istediğiniz performans sorunlarını tetiklediğinden emin olun. Ardından **İzlemeyi Durdur'u**tıklatın.
1. İzleme, web sayfasının alt kısmında listelenir. ETL dosyasını indirmek için sağ taraftaki disk simgesini tıklatın.

Artık doğrudan WPA'da açabileceğiniz veya başka birine gönderebileceğiniz bir ETL dosyanız var.

## <a name="analyzing-traces-with-wpa"></a>WPA ile izlemeleri analiz etme

### <a name="wpa-basics"></a>WPA temelleri

Windows Performans Çözümleyicisi, ETL dosyalarını açmak ve izleri incelemek için standart bir araçtır. WPA'nın nasıl çalıştığına ilişkin bir açıklama bu makalenin kapsamı dışındadır. Başlamak için şu kaynaklara göz atın:

* İlk genel bakış için [tanıtım videolarını](https://docs.microsoft.com/windows-hardware/test/wpt/windows-performance-analyzer) izleyin.
* WPA'nın kendisi, ortak adımları açıklayan bir *Başlarken* sekmesine sahiptir. Kullanılabilir konulara bir göz atın. Özellikle "Verileri Görüntüle" altında, belirli veriler için grafiklerin nasıl oluşturulabildiğini hızlı bir şekilde tanıtabilirsiniz.
* Bu web [sitesinde](https://randomascii.wordpress.com/2015/09/24/etw-central/)mükemmel bilgiler var, ancak, tüm yeni başlayanlar için ilgili değildir.

### <a name="graphing-data"></a>Verileri grafikleme

ARR izleme ile başlamak için, aşağıdaki parçaları bilmek iyidir.

![Performans grafiği](./media/wpa-graph.png)

Yukarıdaki resimde, verilerin izlenmesi tablosu ve aynı verilerin grafik gösterimi gösterilmektedir.

Alttaki tabloda, sarı (altın) çubuğu ve mavi çubuğu not edin. Bu çubukları sürükleyebilir ve herhangi bir konuma yerleştirebilirsiniz.

**Sarı çubuğun solundaki** tüm sütunlar **anahtar**olarak yorumlanır. Anahtarlar, sol üst penceredeki ağacı yapılandırmak için kullanılır. Burada iki *temel* sütun var, "Sağlayıcı Adı" ve "Görev Adı". Sonuç olarak sol üst penceredeki ağaç yapısı iki kat derinliğindedir. Sütunları yeniden sıralarsanız veya anahtar alanından sütun ekler veya kaldırırsanız, ağaç görünümündeki yapı değişir.

Sağ üst teki **grafik ekranı** için **mavi çubuğun sağındaki sütunlar** kullanılır. Çoğu zaman yalnızca ilk sütun kullanılır, ancak bazı grafik modları birden çok veri sütunu gerektirir. Satır grafiklerin çalışması için, bu sütundaki *toplama modunun* ayarlanması gerekir. 'Avg' veya 'Max' kullanın. Toplama modu, bir piksel birden çok olayiçeren bir aralığı kapladığında, grafiğin belirli bir pikseldeki değerini belirlemek için kullanılır. Bu, toplamayı 'Toplam'a ayarlayıp yakınlaştırıp uzaklaştırarak gözlemlenebilir.

Ortadaki sütunların özel bir anlamı yoktur.

![Etkinlik görünümü](./media/wpa-event-view.png)

Genel *Olaylar Görünümü* Düzenleyicisi'nde tüm sütunları görüntülenecek şekilde, toplama modunu, sıralamayı ve hangi sütunların anahtar olarak veya grafik olarak kullanıldığını yapılandırabilirsiniz. Yukarıdaki örnekte, **Alan 2** etkindir ve Alan 3 - 6 devre dışı bırakılır. Alan 2 genellikle bir ETW olayının ilk *özel veri* alanıdır ve bu nedenle bazı ağ gecikme değerini temsil eden ARR "FrameStatistics" olayları için kullanılır. Bu olayın diğer değerlerini görmek için diğer "Alan" sütunlarını etkinleştirin.

### <a name="presets"></a>Önayarları

Bir izlemeyi düzgün bir şekilde çözümlemek için, kendi iş akışınızı ve tercih edilen veri ekranınızı çözmeniz gerekir. Ancak, ARR'a özel olaylar açısından hızlı bir genel gün alınabilmek için Windows Software Protection Platform profilini ve dosyaları *Tools/ ETLProfiles*klasöründe açTıkırılıyor. Tam profil yüklemek için, WPA menü çubuğundan *Profiller > Uygula...'ı* seçin veya *Hazır Ayarlarım* paneli *(Ön Ayarlarım)*> Açılan Lar panelini açın ve *İçe Aktar'ı*seçin. Eski aşağıdaki resimde olduğu gibi tam bir WPA yapılandırması kuracak. İkincisi yalnızca çeşitli görünüm yapılandırmaları için ön ayarlar yapar ve arr olay verilerinin belirli bir parçası bakmak için hızlı bir görünüm açmak için izin verir.

![Önayarları](./media/wpa-arr-trace.png)

Yukarıdaki resimde çeşitli ARR'ye özgü olayların görünümleri ve genel CPU kullanımının bir görünümü gösterilmektedir.

## <a name="next-steps"></a>Sonraki adımlar

* [Sunucu tarafı performans sorguları](../overview/features/performance-queries.md)
