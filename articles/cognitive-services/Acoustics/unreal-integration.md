---
title: Proje Acoustics Unreal ve Wwise tümleştirmesi
titlesuffix: Azure Cognitive Services
description: Bu nasıl yapılır-proje Acoustics Unreal ve Wwise eklentileri projenize tümleştirmeyi açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 47f39e8dcd96ea3bdba564df348e9b89a6b036ba
ms.sourcegitcommit: 13a289ba57cfae728831e6d38b7f82dae165e59d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/09/2019
ms.locfileid: "68933189"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Proje Acoustics Unreal ve Wwise tümleştirmesi
Bu nasıl yapılır, mevcut olmayan ve Wwise oyun projenize Project Acoustics eklenti paketinin ayrıntılı tümleştirme adımlarını sağlar. 

Yazılım gereksinimleri:
* [Unreal Engine](https://www.unrealengine.com/) 4.20 +
* [Audiokinetik Wwise](https://www.audiokinetic.com/products/wwise/) 2018,1.\*
* [Gerçek olmayan için Wwise eklentisi](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  * Wwise Unreal eklentileri kullanmak yerine Wwise SDK 'sının doğrudan tümleştirmesini kullanıyorsanız, Project Acoustics Unreal Plugin 'e başvurun ve Wwise API çağrılarını ayarlayın.

Proje Acoustics 'yi Wwise dışında bir ses altyapısıyla kullanmak istiyorsanız, [Project Acoustics tartışma forumu](https://github.com/microsoft/ProjectAcoustics/issues)üzerinde bir geliştirme isteği yapın. Project Acoustics Unreal eklentisini kullanarak Acoustics verilerini sorgulayabilir ve sonra altyapıya yönelik API çağrıları yapabilirsiniz.

## <a name="download-project-acoustics"></a>Proje Acoustics indir
Henüz yapmadıysanız, [projeyi Acoustics Unreal & Wwise eklenti paketini](https://www.microsoft.com/download/details.aspx?id=58090)) indirin. 

Pakette, bir Unreal Engine eklentisi ve bir Wwise Mixer eklentisi ekledik. Unreal eklentisi düzenleyici ve çalışma zamanı tümleştirmesi sağlar. Oyunplay sırasında Project Acoustics Unreal eklentisi her bir oyun nesnesi için her kare için occlusiyon gibi parametreleri hesaplar. Bu parametreler, Wwise API çağrılarına çevrilir.

## <a name="review-integration-steps"></a>Tümleştirme adımlarını gözden geçirin

Paketi yüklemeye ve oyununuza dağıtmaya yönelik bu temel adımlar vardır.
1. Projeyi Acoustics Wwise Mixer eklentisini yükler
2. (Yeniden) oyununuza Wwise dağıtın. Bu adım, karıştırıcı eklentisini oyun projenize yayar.
3. Oyununuza Acoustics Unreal eklentisini proje ekleyin
4. Wwise 'ın gerçek olmayan eklenti işlevselliğini Genişlet
5. Oyun oluştur ve Python denetimi etkinleştirildi
6. Proje Acoustics kullanmak için Wwise projenizi ayarlama
7. Ses kurulumu, gerçek olmayan

## <a name="1-install-the-project-acoustics-mixer-plugin"></a>1. Project Acoustics Mixer eklentisini yükler
* Wwise başlatıcısı 'nı açın, **Eklentiler** sekmesinde **yeni eklentiler yüklensin**bölümünde **dizinden Ekle**' yi seçin. 

    ![Wwise başlatıcısı 'nda eklenti yükleme ekran görüntüsü](media/wwise-install-new-plugin.png)

* İndirdiğiniz pakette bulunan dizini seçin. `AcousticsWwisePlugin\ProjectAcoustics` Wwise karıştırıcı eklenti paketini içerir.

* Wwise, eklentiyi yükleyecek. Project Acoustics artık Wwise 'daki yüklü eklentiler listesinde görünür.  
![Project Acoustics yüklemesinden sonra, Wwise yüklü eklenti listesinin ekran görüntüsü](media/unreal-integration-post-mixer-plugin-install.png)

## <a name="2-redeploy-wwise-into-your-game"></a>2. (Yeniden) oyununuza Wwise dağıtma
Wwise 'ı zaten tümleştirmiş olsanız bile oyununuza yeniden dağıtın. Bu, Project Acoustics Wwise eklentisini seçer.

* **Motor eklentisi:** Gerçek C++ olmayan bir projede oyun eklentisi olarak yüklüyse, bu adımı atlayın. Eğer gerçek olmayan projeniz yalnızca Blueprint olduğundan, örneğin, bir altyapı eklentisi olarak yüklendiyse, karıştırıcı eklentisi ile Wwise dağıtımı daha karmaşıktır. Kukla, boş olmayan bir C++ proje oluşturun, gerçek zamanlı olmayan bir düzenleyici açılırsa kapatın ve Wwise 'i Bu kukla projeye dağıtmak için kalan yordamı izleyin. Sonra dağıtılan Wwise eklentisini kopyalayın.
 
* Wwise Başlatıcısı ' ndan, **Unreal Engine** sekmesine tıklayın, ardından **en son Unreal Engine projelerinin** yanındaki hamburger menüsüne tıklayın ve **proje için araştır**' ı seçin. Oyununuzun gerçek olmayan proje `.uproject` dosyasını açın.

    ![Wwise başlatıcısı 'nın gerçek olmayan sekmesinin ekran görüntüsü](media/wwise-unreal-tab.png)

* Ardından **projede Wwise 'ı tümleştir** ' e tıklayın veya **projede Wwise 'ı değiştirin**. Bu adım (yeniden), proje Acoustics Mixer eklentisi de dahil olmak üzere Wwise ikililerini projenize tümleştirir.

* **Motor eklentisi:** Wwise 'ı bir motor eklentisi olarak kullanıyorsanız ve yukarıdaki gibi sözde projeyi oluşturduysanız, Wwise dağıtılan `[DummyUProject]\Plugins\Wwise` klasörü kopyalayın ve üzerine `[UESource]\Engine\Plugins\Wwise`yapıştırın. `[DummyUProject]`, gerçek C++ olmayan proje yolundan ve `[UESource]` gerçek olmayan altyapı kaynaklarının yüklü olduğu yerdir. Kopyalamayı tamamladıktan sonra, sözde projeyi silebilirsiniz.

## <a name="3-add-the-project-acoustics-unreal-plugin-to-your-game"></a>3. Oyununuza Acoustics Unreal eklentisini proje ekleyin
 
* Eklenti paketindeki `[UProjectDir]\Plugins\ProjectAcoustics` `UProjectDir` `.uproject` klasörü kopyalayın ve oyunun proje klasörü olan dosyayı içeren yeni bir klasör oluşturun. `Unreal\ProjectAcoustics`
  * **Motor eklentisi**: Motor eklentisi olarak Wwise kullanıyorsanız, Project Acoustics ' ı gerçek olmayan bir altyapı eklentisi olarak kullanmanız gerekir. Yukarıdaki hedef dizin yerine şunu kullanın: `[UESource]\Engine\Plugins\ProjectAcoustics`.

* Klasörün yanında bir `Wwise` klasör görtığınızdan emin olun. `ProjectAcoustics` Yukarıdaki 2. adımda dağıttığınız karıştırıcı eklentisi için ikili dosyalarla birlikte Wwise eklentisini içerir.

## <a name="4-extend-wwises-unreal-plugin-functionality"></a>4. Wwise 'ın gerçek olmayan eklenti işlevselliğini Genişlet
* Project Acoustics Unreal eklentisi, [Bu yönergelere](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html)göre Wwise Unreal Plugin API 'sinden ek davranış gösterilmesini gerektirir. Düzeltme eki uygulama yordamını otomatikleştirmek için bir toplu iş dosyası ekledik. 
* İçinde `Plugins\ProjectAcoustics\Resources`, öğesini `PatchWwise.bat`çalıştırın. Aşağıdaki örnek görüntü AcousticsGame örnek projemizi kullanır.

    ![Wwise 'a düzeltme eki uygulama komut dosyasını vurgulayan Windows Gezgini penceresi ekran görüntüsü](media/patch-wwise-script.png)

* DirectX SDK yüklü değilse, kullanmakta olduğunuz Wwise sürümüne bağlı olarak, `DXSDK_DIR` içinde `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs`yer alan satırı açıklamanız gerekebilir:

    ![DXSDK açıklamalı açıklaması gösteren kod Düzenleyicisi ekran görüntüsü](media/directx-sdk-comment.png)

* Visual Studio 2019 ile derlerseniz, bir bağlama hatasını Wwise ile geçici olarak çözmek için, ' `VSVersion` `vc150`de `AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs` varsayılan değeri el ile düzenleyin:

    ![VSVersion öğesinin vc150 olarak değiştirildiğini gösteren kod düzenleyicisinin ekran görüntüsü](media/vsversion-comment.png)

## <a name="5-build-game-and-check-python-is-enabled"></a>5. Oyun oluştur ve Python denetimi etkinleştirildi

* Oyununuzu derleyin ve doğru şekilde derlendiğinden emin olun. Aksi takdirde, devam etmeden önce önceki adımları dikkatle inceleyin. 
* Projenizi gerçek olmayan düzenleyicide açın. 
* **Motor eklentisi:** Projectacoustik as Engine eklentisi olarak kullanılıyorsa, "yerleşik" eklentiler altında listelenen etkin olduğundan da emin olun.
* Project Acoustics 'in tümleştirildiğini gösteren yeni bir mod görmeniz gerekir.

    ![Acoustics modunun tam gösterildiği reel olmayan ekran görüntüsü](media/acoustics-mode-full.png)

* Gerçek olmayan etkin için Python eklentisine sahip olduğunu doğrulayın. Bu, düzenleyici tümleştirmesinin düzgün çalışması için gereklidir.

    ![Gerçek olmayan düzenleyicide Python uzantılarının etkinleştirilmesinin ekran görüntüsü](media/ensure-python.png)

## <a name="6-wwise-project-setup"></a>6. Wwise proje kurulumu

Samples Download ile örnek bir Wwise projesi dahil edilmiştir. Bu yönergeleri izleyerek bu yönergelere bakmaya önerilir. Aşağıdaki ekran görüntüleri bu projeden alınmıştır.

### <a name="bus-setup"></a>Veri yolu kurulumu
* Project Acoustics Unreal eklentisi, bu ***tam*** adı taşıyan bir veri yolundaki ilişkili karıştırıcı eklentisini arayacaktır: `Project Acoustics Bus`. Bu adla yeni bir ses veri yolu oluşturun. Karıştırıcı eklentisi çeşitli yapılandırmalarda çalışabilir, ancak şimdilik yalnızca yankı işlemini yapmak için kullanılacak varsayıyoruz. Bu veri yolu, Acoustics kullanan tüm kaynaklar için karma ters b sinyalini taşır. Bir veri yolu karıştırma yapısına yukarı akış karıştırabilir, örnek indirmaya dahil edilen Wwise örnek projenizden aşağıda gösterildiği gibi bir örnek aşağıda gösterilmiştir.

    ![Project Acoustics Bus 'ı gösteren Wwise busses ekran görüntüsü](media/acoustics-bus.png)

* Veri yolundaki kanal yapılandırmasının şunlardan birine ayarlanması gerekir: `1.0, 2.0, 4.0, 5.1 or 7.1`. Diğer yapılandırmalarını bu veri yolunda hiçbir çıkış oluşmasına neden olur.

    ![Project Acoustics Bus için kanal yapılandırma seçeneklerinin ekran görüntüsü](media/acoustics-bus-channel-config.png)

* Şimdi Project Acoustics Bus ayrıntılarına gidin ve karıştırıcı eklentisi sekmesini görebilmeniz gerekir

    ![Project Acoustics Bus için Mixer eklenti sekmesinin nasıl etkinleştirileceğini gösteren Wwise 'ın ekran görüntüsü](media/mixer-tab-enable.png)

* Ardından, karıştırıcı eklenti sekmesine gidin ve Project Acoustics Mixer eklentisini veri yoluna ekleyin

    ![Project Acoustics Mixer eklentisinin nasıl ekleneceğini gösteren Wwise Bus 'ın ekran görüntüsü](media/add-mixer-plugin.png)

### <a name="actor-mixer-hierarchy-setup"></a>Aktör-karıştırıcı hiyerarşisi kurulumu
* Project Acoustics, performans nedenleriyle tüm kaynaklara aynı anda ses DSP 'si uygular. Bu, eklentinin bir karıştırıcı eklentisi olarak çalışmasını gerektirir. Wwise, çıkış veri yolu üzerinde olması için karıştırıcı eklentileri gerektirir, ancak çıktı veri yolu, genellikle kuru çıkış sinyalini taşır. Project Acoustics, `Project Acoustics Bus`ıslak sinyali üzerinde işlenirken kuru sinyalinin yedek busler aracılığıyla yönlendirilmesini gerektirir. Aşağıdaki işlem, bu sinyal akışına aşamalı geçişi destekler.

* En üst düzeyde yer adımları, Weapons ve diğerlerini içeren aktör karıştırıcı hiyerarşisi olan mevcut bir projeniz olduğunu varsayalım. Her birinin, Kuru karışımı için karşılık gelen çıkış veri yolu vardır. Diyelim ki, Acoustics kullanmaya yönelik adımları geçirmek istediğinizi varsayalım. İlk olarak, ilgili bir AUX veri yolu oluşturun ve bu, bir alt karışımını, bu çıkış veri yolu yolunun bir alt öğesi olan. Örneğin, tam ad önemli olmasa da, bunları düzenlemek için aşağıdaki görüntüde "kuru" önekini kullandık. Bu adım veri yolunda bulunan tüm ölçümler veya efektler daha önce olduğu gibi çalışır.

    ![Önerilen Wwise kuru karışımı kurulumunun ekran görüntüsü](media/wwise-dry-mix-setup.png)

* Daha sonra, aşağıdaki gibi, WITH, oyuncu-Mixer için Bus çıkış yapısını, Project Acoustics Bus as output Bus olarak ayarlanmış şekilde değiştirin ve Dry_Footsteps, Kullanıcı tanımlı bir AUX veri yolu olarak ayarlanır.

    ![Önerilen Wwise aktör karıştırıcı veri yolu kurulumunun ekran görüntüsü](media/actor-mixer-bus-settings.png)

* Artık tüm bu adımlar Acoustics işleme alır ve proje Acoustics veri yolunda ters b 'yi çıktı. Kuru sinyali, her zamanki gibi Dry_Footsteps ve maça aracılığıyla yönlendirilir.

* Project Acoustics yalnızca dünyanın bir 3B konumuna sahip olan sesler için geçerlidir. Aşağıdaki [Wwise belgelerinde](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/), konumlandırma özelliklerinin gösterildiği gibi ayarlanması gerekir. "3B Istenmeyen kullanım" ayarı, gerektiğinde "konum" ya da "konum + yönlendirme" olabilir.

    ![Önerilen Wwise aktör konumlandırma ayarlarının ekran görüntüsü](media/wwise-positioning.png)

* Çıktı veri yolunu **Project Acoustics Bus** 'a veren diğer veri yoluna ayarlama çalışmaz. Wwise, bu gereksinimi karıştırıcı eklentilerden uygular.

* It adımlarında aktör-karıştırıcı hiyerarşisinde Acoustics kullanmayan bir alt öğe istiyorsanız, bunu kabul etmek için her zaman üzerinde "üst öğeyi geçersiz kıl" seçeneğini kullanabilirsiniz.

* Oydaki herhangi bir oyuncu karıştırıcı için oyun veya Kullanıcı tanımlı kullanıcı tanımlı gönderme kullanıyorsanız, bu aktör-mixer ' i kapatarak, ters b 'yi iki kez uygulamayı önleyin.

### <a name="spatialization"></a>Gereksiz duruma getirme
Varsayılan olarak, Project Acoustics Wwise Mixer eklentisi, bir evlenme yankı uygular ve bu da, maça 'ı kaydırmak için Wwise 'tan bırakır. Project Acoustics 'i yalnızca bu varsayılan ters b yapılandırmasında kullanırken, kurutma karışımında herhangi bir kanal yapılandırması ve istenmeyen kullanım yöntemini kullanmaya ücretsiz olursunuz. böylece, ' ters b ' Project Acoustics ile neredeyse her türlü gereksiz kullanımı karıştırıp eşleştirebilirsiniz. Seçenekleriniz [Ambisonics tabanlı Binaural spatialsleyiciler](https://www.audiokinetic.com/products/ambisonics-in-wwise/) ve [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound)' i içerir.
 
Project Acoustics, nesne tabanlı yüksek çözünürlüklü HRTF 'leri ve yatay kaydırmayı destekleyen isteğe bağlı bir spatializer içerir. Karıştırıcı eklenti ayarlarında "Istenmeyen kullanım yapma" onay kutusunu işaretleyin ve her iki kez de proje Acoustics Mixer eklentisi ve Wwise ile her iki kez de bir kez göz önüne geçmek için Kullanıcı tanımlı yedek göndermeleri ' ı bir kez daha göz önüne alın. Gereksiz duruma getirme modu, bir ses Bankası yeniden oluşturma gerektirdiğinden gerçek zamanlı olarak değiştirilemez. ' Istenmeyen kullanım yapma ' onay kutusu gibi karıştırıcı eklenti yapılandırma değişikliklerini seçmek için, kapatmadan önce Unreal ' i yeniden başlatmanız ve ardından soundbankalarını yeniden oluşturmanız gerekir.

![Wwise karıştırıcı eklentisinin gereksiz hale getirme ayarlarının ekran görüntüsü](media/mixer-spatial-settings.png)

Ne yazık ki, diğer nesne tabanlı spatializer eklentileri, karıştırıcı eklentileri olarak uygulandığından ve Wwise Şu anda tek bir aktör karıştırıcı 'ya atanmış birden çok karıştırıcı eklenti yapmasına izin vermediğinden, bu anda desteklenmez.  

## <a name="7-audio-setup-in-unreal"></a>7. Ses kurulumu, gerçek olmayan
* İlk olarak, içine `Content\Acoustics`yerleştirilecek bir Acoustics varlığı oluşturmak için oyun düzeyinize bakmeniz gerekir. [Gerçek olmayan Bake öğreticiye](unreal-baking.md) danışın ve buradan çalışmaya kaldığınız yerden ulaşabilirsiniz. Önceden tanımlanmış bazı düzeyler örnek paketine dahildir.
* Sahnede bir Acoustics Space aktör oluşturun. Bu aktörlerin yalnızca birini bir düzey içinde oluşturun. Bu, tüm düzeyin Acoustics temsil eder. 

    ![Acoustics Space aktör oluşturmayı gösteren gerçek olmayan düzenleyicinin ekran görüntüsü](media/create-acoustics-space.png)

* Şimdi, Acoustics veri yuvasını, daha önce Acoustics Space aktör üzerinde bulunan veri yuvasına atayın. Sahnede artık Acoustics vardır!

    ![Acoustics varlık atamasını gösteren gerçek olmayan düzenleyicinin ekran görüntüsü](media/acoustics-asset-assign.png)

* Şimdi boş bir aktör ekleyin ve şunları yapın:

    ![Boş bir aktör içinde Acoustics bileşeni kullanımını gösteren gerçek olmayan düzenleyicinin ekran görüntüsü](media/acoustics-component-usage.png)

1. Aktör için Acoustics ses bileşeni ekleyin. Bu bileşen, Wwise ses bileşenini proje Acoustics işlevselliği ile genişletir.
2. Başlangıçta çal kutusu varsayılan olarak denetlenir ve bu işlem, düzey başlangıcında ilişkili Wwise olayını tetikler.
3. Kaynakla ilgili ekran hata ayıklama bilgilerini yazdırmak için Acoustics parametrelerini göster onay kutusunu kullanın.  
    ![Hata ayıklama değerleri etkin olan ses kaynağında gerçek olmayan düzenleyici Acoustics panelinin ekran görüntüsü](media/debug-values.png)
4. Her zamanki Wwise iş akışı başına bir Wwise olayı atama
5. Uzamsal ses kullan özelliğinin kapalı olduğundan emin olun. Şu anda, belirli bir ses bileşeni için Project Acoustics kullanıyorsanız, Acoustics için Wwise 'ın uzamsal ses altyapısını aynı anda kullanamazsınız.

İşinizi tamamlandı. Sahnenin etrafında ilerleyin ve akustik etkileri keşfedebilirsiniz!

## <a name="next-steps"></a>Sonraki adımlar
* Gerçek zamanlı olmayan/Wwise 'da proje Acoustics için [Tasarım](unreal-workflow.md) öğreticisi
* Oyun sahnelerinizi [nasıl yapabileceğinizi öğrenin](unreal-baking.md) 
