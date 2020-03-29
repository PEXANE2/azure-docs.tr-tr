---
title: Proje Akustik Unreal ve Wwise entegrasyonu
titlesuffix: Azure Cognitive Services
description: Bu makalede, Project Acoustics Unreal ve Wwise eklentilerinin projenize entegrasyonu açıklanmaktadır.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: conceptual
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: e57212a3002390754aaebc5f2aa9ffb10af230a2
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "72243053"
---
# <a name="project-acoustics-unreal-and-wwise-integration"></a>Proje Akustik Unreal ve Wwise entegrasyonu
Bu makalede, Project Acoustics eklenti paketinin mevcut Unreal ve Wwise oyun projenize nasıl entegre edilebildiğini açıklanmaktadır.

Yazılım gereksinimleri:
* [Gerçek Dışı Motor](https://www.unrealengine.com/) 4.20+
* [AudioKintic Wwise](https://www.audiokinetic.com/products/wwise/) 2018.1
* [Unreal için Wwise eklentisi](https://www.audiokinetic.com/library/?source=UE4&id=index.html)
  
  Wwise Unreal eklentisi yerine Wwise SDK'nın doğrudan entegrasyonunu kullanıyorsanız, Project Acoustics Unreal eklentisi'ne başvurun ve Wwise API çağrılarını ayarlayın.

Project Acoustics'i Wwise dışında bir ses motoruyla kullanmak [için, Project Acoustics tartışma forumunda](https://github.com/microsoft/ProjectAcoustics/issues)bir geliştirme isteğinde bulunun. Project Acoustics Unreal eklentisini kullanarak akustik verilerini sorgulayabilir ve motorunuza API çağrıları yapabilirsiniz.

## <a name="download-project-acoustics"></a>Proje Akustiğini İndir
Project [Acoustics Unreal ve Wwise eklenti paketini](https://www.microsoft.com/download/details.aspx?id=58090) indirin.

Pakete Unreal Engine eklentisi ve Wwise mikser eklentisi dahildir. Unreal eklentisi düzenleyici ve çalışma zamanı tümleştirmesağlar. Oyun sırasında Project Acoustics Unreal eklentisi, her kare için her oyun nesnesi için tıkanma gibi parametreleri hesaplar. Bu parametreler Wwise API çağrılarına çevrilir.

## <a name="integration-steps"></a>Tümleştirme adımları

Paketi yüklemek ve oyununuza dağıtmak için aşağıdaki adımları izleyin.

### <a name="install-the-project-acoustics-mixer-plug-in"></a>Project Acoustics mikser eklentisini yükleyin
1. Wwise fırlatıcısını açın. **Eklentiler** sekmesinde, Yeni **Eklentileri Yükle'nin**altında, **Dizin'den Ekle'yi**seçin.

    ![Wwise başlatıcısı bir eklenti yükleyin](media/wwise-install-new-plugin.png)

1. İndirme paketinde bulunan *AcousticsWwisePlugin\ProjectAcoustics* dizinini seçin. Wwise mikser eklentisi paketini içerir.

   Wwise eklentiyi kuracak. Project Acoustics, Wwise'daki yüklü eklentiler listesinde görünmelidir.  
![Project Acoustics kurulumundan sonra Wwise yüklü eklentiler listesi](media/unreal-integration-post-mixer-plugin-install.png)

### <a name="dedeploy-wwise-into-your-game"></a>Wwise'ı oyununuza dağıtın
Wwise'ı zaten entegre etmiş olsanız bile Wwise'ı oyununuza yeniden dağıtın. Bu adım, Project Acoustics Wwise eklentisini entegre eder.

   > [!NOTE]
   > **Motor eklentisi:** Wwise bir Unreal C++ projesinde oyun eklentisi olarak yüklüyse, bu adımı atlayın. Bunun yerine bir motor eklentisi olarak yüklenmişse, örneğin Unreal projeniz yalnızca Blueprint olduğu için, mikser eklentimizle Wwise dağıtımı daha karmaşıktır. Sahte boş bir Unreal C++ projesi oluşturun. Açılırsa Unreal düzenleyicisini kapatın ve Wwise'ı kukla projeye dağıtmak için kalan yordamı izleyin. Ardından, dağıtılan Wwise eklentisini kopyalayın.
 
1. Wwise başlatıcısından, **Unreal Engine** sekmesini seçin. Son Gerçek Dışı **Motor Projeleri'nin** yanındaki "hamburger" (simge) menüsünü seçin ve ardından **proje için Gözat'ı**seçin. Oyununuzun Unreal project *.project* dosyanızı açın.

    ![Wwise başlatıcısı Unreal sekmesi](media/wwise-unreal-tab.png)

1. **Projede Wwise'ı Tümleştir'i** veya **Projede Wwise'ı Değiştir'i**seçin. Bu adım, Project Acoustics mikser eklentisi de dahil olmak üzere Wwise ikililerini projenize entegre eder.

   > [!NOTE]
   > **Motor eklentisi:** Wwise'ı motor eklentisi olarak kullanıyorsanız ve daha önce açıklandığı gibi sahte proje oluşturduysanız, Wwise'ın dağıttığı klasörü kopyalayın: *[DummyUProject]\Plugins\Wwise*. *[UESource]\Engine\Plugins\Wwise*üzerine yapıştırın. *[DummyUProject]* boş Unreal C++ proje yoludur ve *[UESource]* Unreal Engine kaynaklarının yüklendiği yerdir. Klasörü kopyaladıktan sonra, sahte projeyi silebilirsiniz.

### <a name="add-the-project-acoustics-unreal-plug-in-to-your-game"></a>Project Acoustics Unreal eklentisini oyununuza ekleyin
 
1. Eklenti paketindeki *Unreal\ProjectAcoustics* klasörünü kopyalayın. *[UProjectDir]\Eklentiler\ProjectAcoustics*, *[UProjectDir]* oyununuzun *.uproject* dosyasını içeren proje klasörü olduğu yeni bir klasör oluşturun.

   > [!NOTE]
   > **Motor eklentisi**: Wwise'ı motor eklentisi olarak kullanıyorsanız, Project Acoustics'i unreal motor eklentisi olarak da kullanmalısınız. Daha önce atıfta bulunulan hedef dizini yerine *[UESource]\Engine\Plugins\ProjectAcoustics*kullanın.

1. *ProjectAcoustics* klasörün yanında bir *Wwise* klasörü gördüğünüzü onaylayın. Daha önce dağıttığınız mikser eklentisi için ikili lerle birlikte Wwise eklentisini içerir.

### <a name="extend-wwise-unreal-plug-in-functionality"></a>Wwise Unreal eklenti işlevini genişletin
Project Acoustics Unreal eklentisi, [bu yönergeler](https://www.audiokinetic.com/library/?source=UE4&id=using__initialsetup.html)uyarınca Wwise Unreal plug-in API'den açığa çıkan ek davranışlar gerektirir. Yama işlemini otomatikleştirmek için bir toplu iş dosyası da ekledik.

* Inside *Eklentileri\ProjectAcoustics\Resources*, *PatchWwise.bat*çalıştırın. Aşağıdaki örnek görüntü, AcousticsGame örnek projemizi kullanır.

    ![Wwise vurgulanan yama komut dosyası ile bir Windows Explorer penceresi](media/patch-wwise-script.png)

* DirectX SDK yüklü yoksa: Wwise sürümüne bağlı olarak kullandığınız, `DXSDK_DIR` *AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs*içeren satırı dışarı yorum gerekebilir:

    !['DXSDK' gösteren kod düzenleyicisi yorumladı](media/directx-sdk-comment.png)

* Visual Studio 2019'u kullanarak derlerseniz: Wwise ile bağlantı hatası nın `VSVersion` çözümünü yapmak *için, AcousticsGame\Plugins\Wwise\Source\AkAudio\AkAudio.Build.cs'deki* varsayılan değeri **vc150'ye**manuel olarak değiştirin:

    !["VSVersion" gösteren kod düzenleyicisi "vc150" olarak değiştirildi](media/vsversion-comment.png)

### <a name="build-the-game-and-check-that-python-is-enabled"></a>Oyunu oluşturun ve Python'un etkin olup olmadığını kontrol edin

1. Oyununuzu derle ve doğru şekilde oluşturduğundan emin olun. İnşa etmiyorsa, devam etmeden önce önceki adımları dikkatle denetleyin.

1. Unreal Editor'da projenizi açın.

    > [!NOTE]
    > **Motor eklentisi:** ProjectAcoustics'i motor eklentisi olarak kullanıyorsanız, "yerleşik" eklentileri altında etkinleştirildiğinden de emin olun.

    Project Acoustics'in tümleşik olduğunu gösteren yeni bir mod görmeniz gerekir.

    ![Unreal'de akustik modu dolu](media/acoustics-mode-full.png)

1. Editör tümleştirmesinin düzgün şekilde çalışmasını sağlayacak şekilde Unreal için Python eklentisinin etkin olduğunu doğrulayın.

    ![Unreal düzenleyicisindeki Python uzantıları etkin](media/ensure-python.png)

### <a name="set-up-your-wwise-project-to-use-project-acoustics"></a>Project Acoustics'i kullanacak Wwise projenizi ayarlayın

Örnek bir Wwise projesi, karşıdan alınan örneklere dahildir. Bu talimatlarla birlikte görüntülemenizi öneririz. Bu makalenin sonraki ekran görüntüleri bu projeden.

#### <a name="bus-setup"></a>Veri otobüsü kurulumu
Project Acoustics Unreal eklentisi, tam adı `Project Acoustics Bus`olan bir veri yolunda ilişkili mikser eklentisini arayacaktır. Bu aynı ada sahip yeni bir ses veri otobüsü oluşturun. Mikser eklentisi çeşitli yapılandırmalarda çalışabilir. Ama şimdilik, biz sadece yankı işleme için kullanılacağını varsayıyoruz. Bu otobüs Akustik kullanan tüm kaynaklar için karışık reverb sinyali taşıyacak. Herhangi bir otobüs karıştırma yapısına yukarı karıştırabilir. Örnek indirmede yer alan Wwise örnek projesinden bir örnek burada gösterilmiştir.

![Project Acoustics Bus'u gösteren Wwise otobüsleri](media/acoustics-bus.png)

1. Veri yolundaki kanal yapılandırmasını *1,0*, *2,0*, *4,0,* *5,1*veya *7,1*olarak ayarlayın. Başka bir ayar, otobüste çıkış olmamasına neden olur.

    ![Project Acoustics Bus için kanal config seçenekleri](media/acoustics-bus-channel-config.png)

1. Project Acoustics Bus ayrıntılarına gidin ve **Mikser Eklentisi** sekmesini görebileceğinize emin olun.

    ![Project Acoustics Bus etkin mixer eklentisi sekmesi ile Wwise](media/mixer-tab-enable.png)

1. **Mikser Eklentisekmesine** gidin ve proje akustik mikser eklentisini veri tonuna ekleyin.

    ![Project Acoustics Mikser Eklentisinin Wwise veri otobüsüne nasıl ekleyeceğinin diyagramı](media/add-mixer-plugin.png)

#### <a name="actor-mixer-hierarchy-setup"></a>Aktör-mikser hiyerarşisi kurulumu
En iyi performans için Project Acoustics, ses dijital sinyal işleme işlemini tüm kaynaklara aynı anda uygular. Yani, eklenti bir mikser eklentisi olarak çalışması gerekir. Wwise, çıkış veri yolunda mikser eklentilerinin olmasını gerektirir, ancak çıkış veri cemiği genellikle kuru çıkış sinyalini taşır. Project Acoustics, kuru sinyalin aux otobüslerden yönlendirilmesini, ıslak sinyalin `Project Acoustics Bus`ise . Aşağıdaki işlem, bu sinyal akışına kademeli geçişi destekler.

Eğer *ayak sesleri,* *silah*içeren bir aktör-karıştırıcı hiyerarşisi ile mevcut bir proje var say, ve üst düzeyde diğerleri. Her kuru karışımı için karşılık gelen bir çıkış veri mesuliyonu vardır. Akustik kullanmak için ayak seslerini geçirmek istediğinizi varsayalım. İlk olarak, ayak sesleri çıkış otobüs bir çocuk kuru submix taşımak için karşılık gelen bir aux bus oluşturun. Örneğin, tam adı önemli olmasa da, otobüsleri düzenlemek için aşağıdaki resimde "Kuru" öneki kullandık. Herhangi bir metre veya etkileri ayak sesleri otobüs üzerinde vardı hala eskisi gibi çalışacaktır.

![Önerilen Wwise kuru karışım kurulumu](media/wwise-dry-mix-setup.png)

Ardından, Project *Acoustics Bus'un* **Çıkış Veri Tos'u**olarak ayarlandığı ve kullanıcı tanımlı aux bus olarak ayarlanmış *Dry_Footsteps* ile Footsteps aktör-mikseriçin veri günü çıkış yapısını aşağıdaki gibi değiştirin.

![Önerilen Wwise aktör karıştırıcı veri günü kurulumu](media/actor-mixer-bus-settings.png)

Şimdi tüm ayak sesleri akustik tedavi olsun ve Proje Akustik Veri Tos kendi reverb çıktı. Kuru sinyal Dry_Footsteps ile yönlendirilir ve her zamanki gibi mekansallaştırılır.

Project Acoustics yalnızca dünyada 3B konumu olan sesler için geçerlidir. [Wwise belgelerini](https://blog.audiokinetic.com/out-with-the-old-in-with-the-new-positioning-revamped-in-wwise-2018.1/)takiben konumlandırma özellikleri gösterildiği gibi ayarlanmalıdır. **3B Uzamsallaştırma** ayarı gerektiğinde *Pozisyon* veya *Konum + Yönlendirme* olabilir.

![Önerilen Wwise Aktör konumlandırma ayarları](media/wwise-positioning.png)

Akış yukarı karıştırır Başka bir veri otobüsü için **Output Bus** ayarlayamadığınız Proje *Akustik Veri Tos*. Wwise bu gereksinimi mikser eklentilerine uygular.

Akustik kullanmamak için ayak izlerinde aktör-mikser hiyerarşisinde bir çocuk istiyorsanız, bunu devre dışı bırakmak için üzerinde "geçersiz kılma üst" kullanabilirsiniz.

Oyundaki herhangi bir aktör-mikserde yankı için oyun tanımlı veya kullanıcı tanımlı göndermeler kullanıyorsanız, reverb'i iki kez uygulamamak için bunları aktör-mikserüzerinde kapatın.

#### <a name="spatialization"></a>Mekansallaştırma
Project Acoustics Wwise mikser eklentisi varsayılan olarak kıvrım lı yankı uygular ve Wwise'ı kaydırma mekansallaştırma yapmaya bırakır. Project Acoustics'i bu varsayılan reverb-only yapılandırmasında kullandığınızda, kuru karışımınızda herhangi bir kanal yapılandırması ve mekansallaştırma yöntemini kullanabilirsiniz. Böylece, Project Acoustics reverb ile hemen hemen her mekansallaştırıcıyı karıştırıp eşleştirebilirsiniz. Seçenekleriniz [Ambisonics tabanlı binaural spatializers](https://www.audiokinetic.com/products/ambisonics-in-wwise/) ve [Windows Sonic](https://docs.microsoft.com/windows/desktop/CoreAudio/spatial-sound)içerir.
 
Project Acoustics, hem nesne tabanlı yüksek çözünürlüklü HRTF oluşturmayı hem de kaydırmayı destekleyen isteğe bağlı bir uzamsallaştırıcı içerir. Mikser eklenti ayarlarındaki **Mekansallaştırma Yap** onay kutusunu seçin ve *HRTF* veya *Kaydırma*arasında seçim yapın. Ayrıca, Project Acoustics mikser eklentisi ve Wwise tarafından iki kez mekansallaştırmayı önlemek için kullanıcı tanımlı aux'u devre dışı bırakın. Bir ses bankası rejenerasyonu gerektirdiğinden, uzamsallaştırma modu gerçek zamanlı olarak değiştirilemez. Unreal'i yeniden başlatın ve mikser eklentisi config değişikliklerini **(Mekansallaştırma Yap** onay kutusu ayarı) tümleştirmek için oynatma'yı seçmeden önce soundbanks'i yeniden oluşturun.

![Wwise Mikser Eklentisi Mekansallaştırma ayarları](media/mixer-spatial-settings.png)

Ne yazık ki, diğer nesne tabanlı uzamsal eklentileri şu anda desteklenmez. Mikser eklentileri olarak uygulanırlar ve Wwise birden fazla mikser eklentilerinin tek bir aktör-miksere atanmasına izin vermez.  

### <a name="audio-setup-in-unreal"></a>Unreal'de ses kurulumu
1. İlk olarak, *İçerik\Akustik'e*yerleştirilecek bir akustik varlık üretmek için oyun seviyenizi pişirmeniz gerekir. [Unreal Bake Tutorial](unreal-baking.md)danışın. Bazı önceden pişmiş düzeyleri örnek pakete dahildir.

1. Sahnenizde bir Akustik uzay aktörü oluşturun. Bu aktörlerden sadece birini bir seviyede oluşturun, çünkü tüm seviyenin akustiğini temsil eder.

    ![Unreal editöründe akustik uzay aktörü nün yaratılması](media/create-acoustics-space.png)

1. Pişmiş akustik veri kıymetini Akustik uzay aktöründeki Akustik veri yuvasına atayın. Sahneniz artık akustik var!

    ![Unreal editöründe akustik varlık ataması](media/acoustics-asset-assign.png)

1. Boş bir aktör ekleyin. Aşağıdaki gibi yapılandırın.

    ![Unreal düzenleyiciboş bir aktörakustik bileşen kullanımını gösterir](media/acoustics-component-usage.png)

       
    <sup>1</sup> Aktöre akustik ses bileşeni ekleyin. Bu bileşen, Wwise ses bileşenine Project Acoustics işlevselliği ekler.
        
    <sup>2</sup> **Başlat'ta Oyna kutusu** varsayılan olarak seçilir. Bu ayar, düzey başlangıç düzeyinde ilişkili bir Wwise olayını tetikler.</li>
         
    <sup>3</sup> Kaynak la ilgili ekrandaki hata ayıklama bilgilerini yazdırmak için **Akustik Parametreleri Göster** onay kutusunu kullanın.

    ![Hata ayıklama değerleri etkin ses kaynağı üzerinde Unreal düzenleyici akustik paneli](media/debug-values.png)

    <sup>4</sup> Her zamanki Wwise iş akışına göre bir Wwise olayı atayın.
       
    <sup>5</sup> **Mekansal Ses Kullanımı'nın** kapalı olduğundan emin olun. Belirli bir ses bileşeni için Project Acoustics kullanıyorsanız, akustik için Aynı anda Wwise'ın Uzamsal Ses motorlarını kullanamazsınız.</li>
       
İşinizi tamamlandı. Sahneyi dolaşın ve akustik efektleri keşfedin!

## <a name="next-steps"></a>Sonraki adımlar
* Proje [Akustik Unreal / Wwise Tasarım Öğretici](unreal-workflow.md)deneyin.
* Oyun sahneleri [için fırında nasıl öğrenin.](unreal-baking.md)
