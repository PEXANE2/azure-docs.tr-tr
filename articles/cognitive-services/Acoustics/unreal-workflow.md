---
title: Proje Akustik Unreal Tasarım Eğitimi
titlesuffix: Azure Cognitive Services
description: Bu öğretici, Unreal ve Wwise'daki Project Acoustics'in tasarım iş akışını açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 817a11171c5b4b4ef205e5fbb04f9b6d6d85b248
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68854257"
---
# <a name="project-acoustics-unrealwwise-design-tutorial"></a>Proje Akustik Unreal / Wwise Tasarım Eğitimi
Bu öğretici, Unreal ve Wwise'daki Project Acoustics'in tasarım kurulumlarını ve iş akışını açıklar.

Yazılım ön koşulları:
* Project Acoustics Wwise ve Unreal eklentileri ile gerçek dışı bir proje

Project Acoustics ile gerçek dışı bir proje almak için şunları yapabilirsiniz:
* Unreal projenize Project Acoustics eklemek için [Project Acoustics Unreal tümleştirme](unreal-integration.md) yönergelerini izleyin
* Veya Project [Acoustics örnek projesini](unreal-quickstart.md)kullanın.

## <a name="setup-project-wide-wwise-properties"></a>Proje genelinde Wwise özelliklerini kurulum
Wwise, Project Acoustics eklentisinin Wwise ses DSP'sini nasıl yönlendirdiğini etkileyen küresel tıkanıklık ve tıkanıklık eğrilerine sahiptir.

### <a name="design-wwise-occlusion-curves"></a>Wwise oklüzyon eğrileri tasarla
Project Acoustics etkin olduğunda, Wwise'da ayarladığınız oklüzyon hacmine, düşük geçişli filtreye (LPF) ve yüksek geçişli filtre (HPF) eğrilerine yanıt verir. 100 oklüzyon değeri için birim eğrisi türünü -100 dB değeriyle doğrusal olarak ayarlamanızı öneririz.

Bu ayar ile, Project Acoustics simülasyonu -18 dB'lik bir tıkanıklık oluşturuyorsa, X=18'de aşağıdaki eğriye giriş yapacak ve buna karşılık gelen Y değeri uygulanan zayıflamadır. Yarım tıkama yapmak için, bitiş noktasını -100 dB yerine -50 dB'ye veya tıkanıklığı abartmak için -200 dB'ye ayarlayın. Oyununuz için en uygun olan herhangi bir eğriyi uyarlayabilir ve ince ayar yapabilirsiniz.
 
![Wwise oklüzyon eğrisi düzenleyicisinin ekran görüntüsü](media/wwise-occlusion-curve.png)

### <a name="disable-wwise-obstruction-curves"></a>Wwise tıkanıklık eğrilerini devre dışı
Wwise obstrüksiyon eğrileri izole olarak kuru seviyeyi etkiler ancak Project Acoustics ıslak/kuru oranları uygulamak için tasarım kontrolleri ve simülasyon kullanır. Tıkanıklık hacmi eğrisini devre dışı bırakmanızı öneririz. Isliyi tasarlamak için daha sonra açıklanan ıslaklık ayarlama denetimini kullanın.
 
Obstrüksiyon LPF/HPF eğrilerini başka amaçlarla kullanıyorsanız, bunları X=0'da Y=0 olarak ayarladığınızdan emin olun (diğer bir deyişle, tıkanıklık olmadığında LPF veya HPF yoktur).

![Wwise tıkanıklık eğrisi düzenleyicisinin ekran görüntüsü](media/wwise-obstruction-curve.png)

### <a name="design-project-acoustics-mixer-parameters"></a>Tasarım Projesi Akustik karıştırıcı parametreleri
Project Acoustics Bus'un mikser eklenti sekmesini ziyaret ederek genel yankı özelliklerini kontrol edebilirsiniz. Mikser eklentisinin ayarları panelini açmak için "Project Acoustics Mikser (Özel)" düğmesine çift tıklayın.

Ayrıca mikser eklentisinin "Mekansallaştırma Yap" seçeneğine sahip olduğunu da görebilirsiniz. Project Acoustic'nin yerleşik uzamsallaştırmasını kullanmayı tercih ediyorsanız, "Mekansallaştırma Yap" onay kutusunu işaretleyin ve HRTF veya Panning'den birini seçin. Kurduğunuz Dry Aux otobüslerini devre dışı bıraktığınızdan emin olun, aksi takdirde doğrudan yolu iki kez duyarsınız. Reverb karışımı üzerinde küresel kontrol egzersiz için "Isluk Ayarlama" ve "Reverb Time Scale Factor" kullanın. Unreal'i yeniden başlatmanız, ardından 'Mekansallaştırma Yap' onay kutusu gibi mikser eklentisi config değişikliklerini almak için play tuşuna basmadan önce soundbanks'i yeniden oluşturmanız gerektiğini unutmayın.

![Project Acoustics Wwise mikser eklentiseçeneklerinin ekran görüntüsü](media/mixer-plugin-global-settings.png)

## <a name="set-project-acoustics-design-controls-in-the-wwise-actor-mixer-hierarchy"></a>Wwise aktör-mikser hiyerarşisinde Project Acoustics tasarım denetimlerini ayarlayın
Tek bir aktör-mikser parametrelerini kontrol etmek için, Actor-Mikser'e çift tıklayın ve mikser Eklentisi sekmesine tıklayın. Burada ses başına herhangi bir parametreyi değiştirebilirsiniz. Bu değerler Unreal tarafından belirlenenlerle birleşir (aşağıda açıklanmıştır). Örneğin, Project Acoustics Unreal eklentisi bir nesne üzerinde Dış Mekan Ayarı'nı 0,5'e ayarlarsa ve Wwise onu -0,25 olarak ayarlarsa, bu sese uygulanan dış mekan ayarı 0,25 olur.

![Wwise aktör-mikser hiyerarşisinde ses karıştırıcı ayarları başına ekran görüntüsü](media/per-sound-mixer-settings.png)

### <a name="ensure-the-aux-bus-has-dry-send-and-output-bus-has-wet-send"></a>Aux bus kuru gönderme ve çıkış otobüsü ıslak göndermek olduğundan emin olun
Gerekli aktör-mikser kurulumu Wwise her zamanki kuru ve ıslak yönlendirme kavşaklar unutmayın. Aktör-mikserçıkış veri yolunda (Project Acoustics Bus'a ayarlanmış) reverb sinyali ve kullanıcı tanımlı aux veri yolunda kuru sinyal üretir. Bu yönlendirme, Project Acoustics Wwise eklentisinin kullandığı Wwise mikser eklentisi API'sinin özellikleri nedeniyle gereklidir.

![Project Acoustics için ses tasarım yönergelerini gösteren Wwise editörünün ekran görüntüsü](media/voice-design-guidelines.png)
 
### <a name="set-up-distance-attenuation-curves"></a>Uzaklık zayıflama eğrilerini ayarlama
Project Acoustics'i kullanan aktör-mikserler tarafından kullanılan herhangi bir zayıflama eğrisinin kullanıcı tanımlı aux send setini "çıkış veri sesi" olarak ayarladığından emin olun. Wwise bunu varsayılan olarak yeni oluşturulan zayıflama eğrileri için yapar. Varolan bir projeyi geçirtiyorsanız, eğri ayarlarınızı kontrol edin.

Varsayılan olarak, Project Acoustics simülasyonu oyuncu konumu etrafında 45 metre lik bir yarıçapa sahiptir. Zayıflama eğrinizi genellikle bu mesafe nin etrafında -200 dB'ye ayarlamanızı öneririz. Bu mesafe zor bir kısıtlama değil. Silah gibi bazı sesler için daha büyük bir yarıçap isteyebilirsiniz. Bu gibi durumlarda, uyarı oyuncu konumu 45 m içinde sadece geometri katılacak olmasıdır. Eğer oyuncu bir odada ysa ve bir ses kaynağı odanın dışında ysa ve 100 m uzaktaysa, düzgün bir şekilde tıkanacaktır. Kaynak bir odada ysa ve oyuncu dışarıda ve 100 m uzaktaysa, düzgün bir şekilde tıkanmış olmayacaktır.

![Wwise zayıflama eğrilerinin ekran görüntüsü](media/atten-curve.png)

### <a name="post-mixer-equalization"></a>Post Mikser Dengeleme ###
 Yapmak isteyebilirsiniz başka bir şey bir yazı karıştırıcı ekolayzır eklemektir. Project Acoustics veri tobunu tipik bir yankı veri yolunda (varsayılan yankı modunda) olarak değerlendirebilir ve eşitleme yapmak için üzerine bir filtre koyabilirsiniz. Bunun bir örneğini Project Acoustics Wwise Sample Project'te görebilirsiniz.

![Wwise post-mikser EQ ekran görüntüsü](media/wwise-post-mixer-eq.png)

Örneğin, yüksek geçiş filtresi, patlamalı, gerçekçi olmayan yankı sağlayan yakın alan kayıtlarından bas işlemeye yardımcı olabilir. Ayrıca, EQ'yu RTPC'ler aracılığıyla ayarlayarak, oyun zamanında yankının rengini değiştirmenize olanak tanıyan daha fazla fırın sonrası kontrol elde edebilirsiniz.

## <a name="set-up-scene-wide-project-acoustics-properties"></a>Sahne genelinde Project Acoustics özelliklerini ayarlama

Akustik Uzay aktör sistemin davranışını değiştirmek ve hata ayıklama yararlıdır birçok denetimleri ortaya çıkarır.

![Unreal Akustik Uzay Kontrolleri Ekran Görüntüsü](media/acoustics-space-controls.png)

* **Akustik Veri:** Bu alana İçerik/Akustik dizininden pişmiş akustik bir varlık atanmalıdır. Project Acoustics eklentisi, projenizin paket dizinlerine Otomatik olarak İçerik/Akustik dizinini ekler.
* **Kiremit boyutu:** Akustik verilerinRAM'a yüklenmesini istediğiniz dinleyicinin çevresindeki bölgenin kapsamları. Dinleyici sondaları hemen oynatıcının etrafına yüklendiği sürece, sonuçlar tüm problar için akustik veri yüklemekle aynıdır. Daha büyük döşemeler daha fazla RAM kullanır, ancak disk G/Ç'yi azaltır
* **Otomatik Akış:** Etkinleştirildiğinde, dinleyici yüklenen bölgenin kenarına ulaştığında otomatik olarak yeni kutucuklar yükler. Devre dışı bırakıldığında, kod veya planlar aracılığıyla yeni kutucukları el ile yüklemeniz gerekir
* **Önbellek Ölçeği:** akustik sorgular için kullanılan önbelleğin boyutunu denetler. Daha küçük bir önbellek daha az RAM kullanır, ancak her sorgu için CPU kullanımını artırabilir.
* **Akustik Etkin:** Akustik simülasyonunun hızlı A/B'nin değiştirilebilmesini sağlamak için hata ayıklama denetimi. Bu denetim, sevkiyat yapılandırmalarında yoksayılır. Denetim, belirli bir ses hatasının akustik hesaplamalardan veya Wwise projesindeki başka bir sorundan kaynaklandığını bulmak için yararlıdır.
* **Güncelleme Mesafeleri:** Mesafe sorguları için önceden pişirilen akustik bilgilerini kullanmak istiyorsanız bu seçeneği kullanın. Bu sorgular ışın dökümlerine benzer, ancak önceden hesaplanmıştır, bu nedenle çok daha az CPU alır. Örnek kullanım, dinleyiciye en yakın yüzeydeki ayrık yansımalar içindir. Bunu tam olarak kullanmak için mesafeleri sorgulamak için kod veya Blueprints kullanmanız gerekir.
* **Beraberlik İstatistikleri:** UE'ler `stat Acoustics` size CPU bilgileri sağlasa da, bu durum ekranı ekranın sol üst kısmında şu anda yüklenen ACE dosyasını, RAM kullanımını ve diğer durum bilgilerini gösterir.
* **Voxels çizin:** Çalışma zamanı enterpolasyonu sırasında kullanılan voxel ızgarasını gösteren dinleyiciye yakın yer kaplama voxel'leri. Bir yayımlayıcı çalışma zamanı voxel içinde ise, akustik sorguları başarısız olur.
* **Sondaları Çiz:** Bu sahnenin tüm sondalarını göster. Yük durumuna bağlı olarak farklı renklerde olurlar.
* **Çekme Mesafeleri:** Güncelleştirme Mesafeleri etkinse, bu, dinleyicinin etrafındaki nicel yönlerde dinleyiciye en yakın yüzeyde bir kutu gösterir.

## <a name="actor-specific-acoustics-design-controls"></a>Aktöre özel akustik tasarım kontrolleri
Bu tasarım denetimleri Unreal'de tek bir ses bileşenine göre kapsama sahiptir.

![Gerçek Dışı Ses Bileşeni Kontrollerinin Ekran Görüntüsü](media/audio-component-controls.png)

* **Oklüzyon Çarpanı:** Oklüzyon etkisini kontrol eder. 1 > değerleri tıkanıklığı arttırır. 1 <değerleri en aza indirir.
* **Isolgunluk Ayarı:** Ek reverb dB
* **Bozunma Zamanı Çarpanı:** RT60'ı akustik simülasyonunun çıkışına göre çarpan olarak kontrol eder
* **Dış Mekan Ayarı:** Yankılanmanın ne kadar açık havada olduğunu kontrol eder. 0'a yakın değerler daha kapalı, 1'e yakın daha açık havada. Bu ayarlama katkı maddesidir, bu nedenle -1'e ayar yapmak iç mekanlarda zorlanır, +1'e ayarlayarak dış mekanları zorlar.
* **Şanzıman Db:** Bu yükseklik ve görüş hattı tabanlı mesafe zayıflaması ile birlikte ek bir duvar dan ses render.
* **Islör Oranı Mesafe Çözgü:** Doğrudan yolu etkilemeden, kaynaktaki yankılanma özelliklerini daha yakın/daha uzakta yatabilecek şekilde ayarlar.
* **Başlangıç'ta oyna:** Sesin olay başında otomatik olarak çalıp çalmayacağını belirtmek için titreyin. Varsayılan olarak etkindir.
* **Akustik Parametreleri Göster:** Hata ayıklama bilgilerini doğrudan oyun içinde bileşenin üstüne görüntüleyin. (sadece sevkiyat dışı yapılandırmalar için)

## <a name="blueprint-functionality"></a>Blueprint işlevselliği
Akustik Uzay aktörüne, harita yükleme veya seviye komut dosyası ile ayarları değiştirme gibi işlevler sağlayan plan aracılığıyla erişilebilir. Burada iki örnek veriyoruz.

### <a name="add-finer-grained-control-over-streaming-load"></a>Akış yükü üzerinde daha ince taneli denetim ekleme
Oyuncu konumuna göre otomatik olarak akış yapmak yerine akustik veri akışını kendiniz yönetmek için Force Load Tile plan işlevini kullanabilirsiniz:

![Unreal'de Blueprint Streaming seçeneklerinin ekran görüntüsü](media/blueprint-streaming.png)

* **Hedef:** AkustikSpace aktör
* **Merkez Konumu:** Veri yüklenmesi gereken bölgenin merkezi
* **Sondaları Döşeme Dışında Boşaltın:** İşaretlenirse, yeni bölgede olmayan tüm sondalar RAM'den indirilir. İşaretlenmemişse, varolan problar da belleğe yüklenirken yeni bölge belleğe yüklenir
* **Tamamlanma Bloğu:** Döşeme yükünü eşzamanlı bir işlem yapar

Force Load Tile'yı çağırmadan önce döşeme boyutu zaten ayarlanmalıdır. Örneğin, ace dosyasını yüklemek, döşeme boyutunu ayarlamak ve bir bölgede akış yapmak için böyle bir şey yapabilirsiniz:

![Unreal'de Akış Kurulumu seçeneklerinin ekran görüntüsü](media/streaming-setup.png)

Bu örnekte kullanılan Yük Akustik Veri planı işlevi aşağıdaki parametrelere sahiptir:

* **Hedef:** Akustik Uzay aktörü.
* **Yeni Bake:** Yüklenecek akustik veri varlığı. Bu boş/null ayarlayarak yeni bir yükleme den geçerli fırında boşaltacaktır.

### <a name="optionally-query-for-surface-proximity"></a>Yüzey yakınlığı için isteğe bağlı olarak sorgu
Dinleyicinin etrafında belirli bir yönde yüzeylerin ne kadar yakın olduğunu görmek istiyorsanız, Sorgu Mesafesi işlevini kullanabilirsiniz. Bu fonksiyon, yönlü gecikmeli yansımaları yönlendirmek veya yüzey yakınlığı yla yönlendirilen diğer oyun mantığı için yararlı olabilir. Sonuçlar akustik arama tablosundan çıkarıldığından sorgu, bir ışın kalıbından daha ucuzdur.

![Örnek Plan mesafesi sorgusunun ekran görüntüsü](media/distance-query.png)

* **Hedef:** AkustikSpace aktör
* **Yöne Bak:** Sorguyönü, dinleyici de ortalanmış
* **Mesafe:** Sorgu başarılı olursa, en yakın yüzeye uzaklık
* **İade Değeri:** Boolean - sorgu başarılı, aksi takdirde yanlış eğer doğru

## <a name="next-steps"></a>Sonraki adımlar
* [Tasarım sürecinin](design-process.md) arkasındaki kavramları keşfedin
* Kendi sahnenizi pişirmek için [bir Azure hesabı oluşturun](create-azure-account.md)


