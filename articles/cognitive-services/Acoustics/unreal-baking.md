---
title: Proje Akustik Unreal Bake Öğretici
titlesuffix: Azure Cognitive Services
description: Bu belge, Unreal düzenleyici uzantısını kullanarak akustik fırın gönderme işlemini açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 7a868a5f9b06499e23710399733b0659d97f900d
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "68854897"
---
# <a name="project-acoustics-unreal-bake-tutorial"></a>Proje Akustik Unreal Bake Öğretici
Bu belge, Unreal düzenleyici uzantısını kullanarak akustik fırın gönderme işlemini açıklar.

Bir fırında yapmak için beş adım vardır:

1. Oyuncu navigasyon kafesinizi oluşturma veya etiketleme
2. Etiket akustik geometrisi
3. Geometriye akustik malzeme özellikleri atama
4. Sonda yerleşimini önizleme
5. Fırında

## <a name="open-the-project-acoustics-editor-mode"></a>Project Acoustics düzenleyici modunu açın

Project Acoustics eklenti paketini projenize aktarın. Bu konuda yardım [için, Unreal Integration](unreal-integration.md) konusuna bakın. Eklenti entegre olduktan sonra, yeni Akustik Modu simgesine tıklayarak Akustik UI'yi açın.

![Unreal Editor Akustik Modu seçeneğiekran görüntüsü](media/acoustics-mode.png)

## <a name="tag-actors-for-acoustics"></a>Akustik için tag aktörler

Nesneler sekmesi, Akustik Modu'nu açtığınızda görüntülenen ilk sekmedir. Aktörlere **AcousticsGeometry** veya **AcousticsNavigation** etiketlerini ekleyen düzeyinizdeki aktörleri etiketlemek için bu sekmeyi kullanın.

World Outliner'da bir veya daha fazla nesne seçin veya belirli bir kategorideki tüm nesneleri seçmenize yardımcı olmak için **Toplu Seçim** bölümünü kullanın. Nesneler seçildikten sonra, istenen etiketi seçili nesnelere uygulamak için **Etiketleme** bölümünü kullanın.

Bir şey ne **AcousticsGeometri** ne de **AcousticsNavigation** etiketi varsa, simülasyonda göz ardı edilecektir. Yalnızca statik meshes, navigasyon meshes ve manzara desteklenir. Başka bir şey etiketlemek, bu göz ardı edilecektir.

### <a name="for-reference-the-objects-tab-parts"></a>Başvuru için: Nesneler sekmesi parçaları

![Unreal Akustik Nesneler sekmesinin Ekran Görüntüsü](media/unreal-objects-tab-details.png)

1. Sekme seçim düğmeleri (**Nesneler** sekmesi seçili). Yukarıdan aşağıya, bir akustik fırında yapmanın çeşitli adımları ile yürümek için bu düğmeleri kullanın.
2. Bu sayfayı kullanarak yapmanız gerekenlerhakkında kısa bir açıklama.
3. Düzeyindeki aktörler için kullanılabilir seçiciler.
4. **Seç'i** tıklattığınızda, denetlenen aktör türlerinden en az biriyle eşleşen düzeydeki tüm nesneleri seçilir.
5. **Tümünü Seç'i** tıklattığınızda geçerli seçim temizler. Bu kaçış tuşuna vurmakla aynı şey.
6. Geometri veya Gezinme etiketini seçili aktörlere uygulayıp uygulamayacağınızı seçmek için bu radyo düğmelerini kullanın.
7. **Tag'i** tıklatmak, seçili etiketi şu anda seçili tüm aktörlere ekler.
8. **Etiket'i** tıklatmak, seçili etiketi şu anda seçili tüm aktörlerden kaldırır.
9. Etiketli **Seç'i** tıklattığınızda geçerli seçimi temizler ve şu anda seçili etiketi olan tüm aktörleri seçer.
10. Bu istatistikler, her etiket türüyle kaç aktörün etiketlenmiş olduğunu gösterir.

### <a name="tag-acoustics-occlusion-and-reflection-geometry"></a>Etiket akustik oklüzyon ve yansıma geometrisi

Akustik penceresinin Nesneler sekmesini açın. Herhangi bir nesneyi, sesi tıkayacak, yansıtacak veya emerlerse Akustik Geometri olarak işaretleyin. Akustik geometri zemin, duvarlar, çatılar, pencere & pencere camı, kilim ve büyük mobilya gibi şeyler içerebilir. Bu nesneler için herhangi bir rasgele karmaşıklık düzeyini kullanabilirsiniz. Sahne simülasyondan önce voxelized olduğundan, çok küçük yaprakları olan ağaçlar gibi son derece ayrıntılı meshes, basitleştirilmiş nesneleri daha pişirmek için daha pahalı değildir.

Görünmez çarpışma meshes gibi akustiği etkilememesi gereken şeyler eklemeyin.

Bir nesnenin sonda hesaplaması sırasındaki dönüşümü (aşağıdaki Problar sekmesi aracılığıyla) fırın sonuçlarında sabitlenir. İşaretli nesnelerden herhangi birinin sahnede taşınması, sonda hesaplamasının yeniden yapılmasını ve sahnenin yeniden yapılmasını gerektirir.

### <a name="create-or-tag-a-navigation-mesh"></a>Navigasyon ağı oluşturma veya etiketleme

Simülasyon için sonda noktaları yerleştirmek için bir navigasyon kafesi kullanılır. Unreal'in Nav [Mesh Bounds Birimini](https://api.unrealengine.com/INT/Engine/AI/BehaviorTrees/QuickStart/2/index.html)kullanabilir veya kendi navigasyon kafesinizi belirtebilirsiniz. En az bir nesneyi **Akustik Gezinti**olarak etiketlemeniz gerekir. Unreal'in Gezinti örgüsi kullanıyorsanız, önce bu kafesi oluşturduğundan emin olun.

### <a name="acoustics-volumes"></a>Akustik Hacimleri ###

**Akustik Hacimleri**ile navigasyon alanlarınızda yapabileceğiniz gelişmiş özelleştirme ler de vardır. **Akustik Birimleri,** sahnenize ekleyebileceğiniz ve gezinme örgüsünü ekleyeceğiniz ve yok sayabileceğiniz alanları seçmenize olanak tanıyan aktörlerdir. Aktör, "Ekle" ve "Hariç Tut" arasında değiştirilebilen bir özelliği ortaya çıkarır. "Ekle" birimleri, yalnızca içindeki gezinti kafesinin alanlarının dikkate alınmasını sağlar ve "Hariç Tut" birimleri bu alanları yoksayılması gereken alanları işaretler. "Hariç" birimleri her zaman "Ekle" ciltler sonra uygulanır. Nesneler sekmesinde olağan işlemde **Akustik Ses birimlerini** **Akustik Gezinti** olarak etiketlediğinden emin olun. Bu aktörler otomatik olarak ***etiketlenmemiştir.***

![Unreal'deki Akustik Hacim özelliklerinin ekran görüntüsü](media/unreal-acoustics-volume-properties.png)

"Dışlama" hacimleri esas olarak kaynak kullanımını sıkılaştırmak için sondaların yerleştirilemeyecekleri yerler üzerinde ince taneli denetim sağlamak içindir.

![Unreal'de Akustik HacmiNi Hariç Tut Ekran Görüntüsü](media/unreal-acoustics-volume-exclude.png)

"Ekle" birimleri, sahnenizi birden çok akustik bölgeye ayırmak gibi bir sahnenin el ile bölümlerini oluşturmak için yararlıdır. Örneğin, büyük bir sahne varsa, kilometre kare, ve akustik pişirmek istediğiniz ilgi iki alan var. Sahnede iki büyük "Ekle" birim çizebilir ve her biri için birer birer ACE dosyaları oluşturabilirsiniz. Daha sonra oyunda, oyuncu her döşemeye yaklaştığında uygun ACE dosyasını yüklemek için plan çağrılarıyla birlikte tetik hacimleri kullanabilirsiniz.

**Akustik Hacimleri** geometriyi ***değil,*** yalnızca gezinmeyi kısıtlar. Bir "Include" **Akustik Hacmi** içindeki her prob, dalga simülasyonları gerçekleştirirken ses in dışında gerekli tüm geometriyi çekmeye devam edecektir. Bu nedenle, oyuncunun bir bölümden diğerine geçişinden kaynaklanan tıkanıklık veya diğer akustikte herhangi bir süreksizlik olmamalıdır.

## <a name="select-acoustic-materials"></a>Akustik malzemeleri seçin

Nesneleriniz etiketlendikten sonra Malzemeler Sekmesine gitmek için **Malzemeler** düğmesini tıklatın. Bu sekme, düzeydeki her malzeme için malzeme özelliklerini belirtmek için kullanılır. Herhangi bir aktör etiketlendirilmeden önce, boş olacaktır.

Akustik malzemeler her yüzeyden yansıyan ses enerjisi miktarını kontrol edin. Varsayılan akustik malzeme nin betona benzer emilimi vardır. Project Acoustics, sahne malzemesi adına dayalı malzemeler önerir.

Bir odadaki bir malzemenin yankılanma süresi, emilim katsayısı ile ters orantılıdır ve çoğu malzeme 0,01 ile 0,20 aralığında emme değerlerine sahiptir. Bu aralığın üzerinde emme katsayıları olan malzemeler çok emici. Örneğin, bir oda çok yankı lı geliyorsa, duvarların, zeminin veya tavanın akustik malzemesini daha yüksek absorptivity bir şeye değiştirin. Akustik malzeme ataması, o sahne malzemesini kullanan tüm aktörler için geçerlidir.

![Yankılanma süresinin emilim katsayısı ile negatif korelasyongösteren grafiği](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Başvuru için: Malzemeler sekmesinin bölümleri

![Unreal Akustik Nesneler sekmesinin Ekran Görüntüsü](media/unreal-materials-tab-details.png)

1. **Malzemeler** sekmesi düğmesi, bu sayfayı açmak için kullanılır.
2. Bu sayfayı kullanarak yapmanız gerekenlerhakkında kısa bir açıklama.
3. **AkustikGeometri**olarak etiketlenen aktörlerden alınan düzeyde kullanılan malzemelerin listesi. Burada bir malzemeyi tıklattığınızda, bu malzemeyi kullanan sahnedeki tüm nesneler seçilir.
4. Sahne materyalinin atandığı akustik malzemeyi gösterir. Sahne materyalini farklı bir akustik malzemeye yeniden atamak için açılır bir dosyayı tıklatın.
5. Önceki sütunda seçilen malzemenin akustik emme katsayısını gösterir. Sıfır değeri mükemmel yansıtıcı (emme yok) anlamına gelirken, 1 değeri mükemmel bir şekilde emilimi (yansıma yok) anlamına gelir. Bu değeri değiştirmek Akustik Malzeme (adım #4) **Özel**olarak güncellenir.

Sahnenizdeki malzemelerde değişiklik yaparsanız, bu değişikliklerin **Malzemeler** sekmesine yansıtTığını görmek için Project Acoustics eklentisinde sekmeleri değiştirmeniz gerekir.

## <a name="calculate-and-review-listener-probe-locations"></a>Dinleyici sonda konumlarını hesaplama ve inceleme

Malzemeleri atadıktan **sonra, Sondalar** sekmesine geçin.

### <a name="for-reference-parts-of-the-probes-tab"></a>Başvuru için: Problar sekmesinin bölümleri

![Unreal Akustik Problar sekmesinin Ekran Görüntüsü](media/unreal-probes-tab-details.png)

1. Bu sayfayı açmak için kullanılan **Sondalar** sekmesi düğmesi
2. Bu sayfayı kullanarak yapmanız gerekenlerhakkında kısa bir açıklama
3. Kaba veya ince bir simülasyon çözünürlüğü seçmek için bunu kullanın. Kaba daha hızlı, ama bazı trades vardır. Ayrıntılar için aşağıdaki [Fırın Çözünürlüğüne](bake-resolution.md) bakın.
4. Bu alanı kullanarak akustik veri dosyalarının yerleştirilebileceği konumu seçin. "..." düğmesine tıklayın bir klasör seçici kullanmak için. Veri dosyaları hakkında daha fazla bilgi için aşağıdaki [Veri Dosyaları'na](#Data-Files) bakın.
5. Bu sahnenin veri dosyaları burada sağlanan önek kullanılarak adlandırılacaktır. Varsayılan değer "[Düzey Adı]_AcousticsData"dir.
6. Sahneyi voxelize etmek ve sonda noktası konumlarını hesaplamak için **Hesapla** düğmesini tıklatın. Bu makinenizde yerel olarak yapılır ve bir fırında yapmadan önce yapılmalıdır. Sondalar hesaplandıktan sonra yukarıdaki denetimler devre dışı bırakılır ve bu düğme **"Temizle"** demek üzere değişecektir. Hesaplamaları silmek ve yeni ayarlar kullanarak yeniden hesaplayabilmeniz için denetimleri etkinleştirmek için **Temizle** düğmesini tıklatın.

Problar, **Problar** sekmesinde sağlanan otomatik işlem yoluyla yerleştirilmelidir.


### <a name="what-the-calculate-button-calculates"></a>"Hesapla" düğmesi ne hesaplar

**Hesapla** düğmesi şimdiye kadar sağladığınız tüm verileri (geometri, navigasyon, malzeme ve kaba/ince ayar) alır ve birkaç adımdan geçer:

1. Bu sahne meshes geometri alır ve bir voxel hacmi hesaplar. Voxel hacmi, tüm sahnenizi içine alan ve küçük kübik "voxels" oluşur 3 boyutlu bir hacimdir. Voxels boyutu **Simülasyon Çözünürlük** ayarı tarafından belirlenen simülasyon frekansı tarafından belirlenir. Her voxel ya "açık hava" olarak işaretlenir ya da sahne geometrisi içeren. Bir voxel geometri içeriyorsa, voxel o geometriye atanan malzemenin soğurma katsayısı ile etiketlenir.
2. Daha sonra, oynatıcının gidebileceği akustik olarak ilginç konumları hesaplamak için gezinti verilerini kullanır. Bu kapı ve koridorlar gibi küçük alanları içeren bu yerlerin oldukça küçük bir dizi bulmaya çalışır, ve daha sonra odalara, açık alanlar. Küçük sahneler için bu genellikle 100'den az konum, büyük sahneler ise bine kadar olabilir.
3. Hesaplatır son dinleyici konumlarının her biri için, "açık" alanı, içinde olduğu odanın boyutu, vb. gibi bir dizi parametre belirler.
4. Bu hesaplamaların sonuçları belirttiğiniz konumdaki dosyalarda depolanır (Aşağıdaki [Veri Dosyalarına](#Data-Files) bakın)

Sahnenizin boyutuna ve makinenizin hızına bağlı olarak, bu hesaplamalar birkaç dakika sürebilir.

Bu hesaplamalar tamamlandıktan sonra, fırının size iyi sonuçlar vermesini sağlamaya yardımcı olmak için hem voxel verilerini hem de sonda noktası konumlarını önizleyebilirsiniz. Kötü bir gezinme kafesi veya eksik/ekstra geometri gibi şeyler genellikle önizlemede hızlı bir şekilde görünür böylece düzeltebilirsiniz.


## <a name="debug-display"></a>Hata ayıklama ekranı

Sonda hesaplama tamamlandıktan sonra, yeni bir aktör Dünya Outliner **AcousticsDebugRenderer**denilen görünecektir. Render **Sondaları** ve **Render Voxels** onay kutularını denetlemek, düzenleyici görünümünde hata ayıklama görüntüsünü etkinleştirecektir.

![Unreal Editor'da Akustik Debug Renderer aktörlerini gösteren ekran görüntüsü](media/acoustics-debug-renderer.png)

Seviyenizde herhangi bir voxel veya prob görmüyorsanız, görüntüleme portunda gerçek zamanlı görüntülemenin etkinleştirildiğinden emin olun.

![Unreal'de gerçek zamanlı görüntüleme seçeneğinin ekran görüntüsü](media/unreal-real-time-rendering.png)

### <a name="voxels"></a>Voxels

Voxels katılımcı geometri etrafında yeşil küpler olarak sahne penceresinde gösterilir. Yalnızca hava içeren voxels gösterilmez. Simülasyonda kullanılacak tam voxel hacmini gösteren tüm sahnenin etrafında büyük bir yeşil kutu vardır.
Sahnenizde dolaşın ve akustik tıkanan geometrinin voxels olduğunu doğrulayın. Ayrıca, çarpışma meshes gibi akustik olmayan nesnelerin voxelized olmadığını kontrol edin. Sahne kamerası voxels göstermek için nesnenin yaklaşık 5 metre içinde olmalıdır.

Kaba çözünürlük ile ince çözünürlük ile oluşturulan voxels karşılaştırırsanız, kaba voxels iki kat daha büyük olduğunu göreceksiniz.

![Unreal editörüakustik voxels önizleme Ekran görüntüsü](media/unreal-voxel-preview.png)

### <a name="probe-points"></a>Sonda noktaları

Sonda noktaları olası oyuncu (dinleyici) konumları ile eşanlamlıdır. Pişirme yaparken, simülasyon akustiği her sonda noktasına olası tüm kaynak konumlarını bağlayan hesaplar. Çalışma zamanında, dinleyici konumu yakındaki sonda noktaları arasında enterpolasyona tabidir.

Sonda noktalarının oyuncunun olay yerinde seyahat etmesinin beklendiği herhangi bir yerde olup olmadığını kontrol etmek önemlidir. Prob noktaları Project Acoustics motoru tarafından navigasyon örgüse yerleştirilir ve hareket ettirilemediğinden veya düzenlenemez, bu nedenle sonda noktalarını inceleyerek navigasyon örgüsi olası tüm oyuncu konumlarını kapsadığından emin olun.

![Unreal akustik probları önizleme Ekran görüntüsü](media/unreal-probes-preview.png)

Kaba vs ince çözünürlük hakkında daha fazla bilgi için [Bake Resolution'a](bake-resolution.md) bakın.

## <a name="bake-your-level-using-azure-batch"></a>Azure Toplu İşlemini kullanarak seviyenizi yükseltin

Azure Toplu İşlem hizmetini kullanarak sahnenizi bulutta bir işlem kümesiyle pişirebilirsiniz. Project Acoustics Unreal eklentisi, her fırında bir Azure Toplu Iş kümesini anlık olarak oluşturmak, yönetmek ve yıkmak için doğrudan Azure Toplu İş'e bağlanır. Fırında sekmesine Azure kimlik bilgilerinizi girin, küme makinesi türünü ve boyutunu seçin ve Bake'yi tıklatın.

### <a name="for-reference-parts-of-the-bake-tab"></a>Başvuru için: Fırın sekmesinin bölümleri

![Unreal Akustik Bake sekmesiekran görüntüsü](media/unreal-bake-tab-details.png)

1. Bu sayfayı açmak için kullanılan Bake Tab düğmesi.
2. Bu sayfada ne yapacağınıkısa bir açıklama.
3. Azure hesabınız oluşturulduktan sonra Azure Kimlik Bilgilerinize girebilmek için alanlar. Daha fazla bilgi için [bkz.](create-azure-account.md)
4. Aboneliklerinizi yönetmek, kullanımı izlemek ve fatura bilgilerini görüntülemek vb. için Azure portalını başlatın. 
5. Hesaplama için kullanılacak azure toplu işlem düğümü türü. Düğüm türü Azure veri merkezi konumunuz tarafından desteklenmelidir. Emin değilse, **Standard_F8s_v2**bırakın.
6. Bu hesaplama için kullanılacak düğüm sayısı. Buraya girdiğiniz sayı, fırını tamamlama süresini etkiler ve Azure Toplu toplu tahsisinizle sınırlıdır. Varsayılan ayırma yalnızca iki 8 çekirdekli düğüme veya bir 16 çekirdek düğüme izin verir, ancak genişletilebilir. Temel ayırma kısıtlamaları hakkında daha fazla bilgi için [bkz.](create-azure-account.md)
7. Düşük [öncelikli düğümleri](https://docs.microsoft.com/azure/batch/batch-low-pri-vms)kullanacak şekilde bilgi işlem havuzunuzu yapılandırmak için bu onay kutusunu seçin. Düşük öncelikli işlem düğümleri çok daha düşük maliyete sahiptir, ancak her zaman kullanılamayabilir veya herhangi bir zamanda preempted olabilir.
8. İşinizin bulutta çalışması için geçen süre miktarı. Bu düğüm başlangıç saatini içermez. Bir kez iş çalışmaya başlar, bu sonuçları geri almak için önce ne kadar olması gerektiği ile ilgilidir. Bunun yalnızca bir tahmin olduğunu unutmayın.
9. Simülasyonları çalıştırmak için gereken toplam işlem süresi miktarı. Bu, Azure'da kullanılacak düğüm işlem süresinin toplam tutarıdır. Bu değeri kullanma hakkında daha fazla bilgi için aşağıdaki [fırın maliyetini tahmin](#Estimating-bake-cost) etme bakın.
10. Fırında'yı buluta göndermek için Bake düğmesini tıklatın. Bir iş çalışırken, bu yerine **İş İptal** gösterir. Bu sekmede herhangi bir hata varsa veya **Sondalar** sekmesindeki iş akışı tamamlanmamışsa, bu düğme devre dışı bırakılır.
11. **Sondalar** sekmesinde hesaplanan sahneniz için sonda sayısı. Sonda sayısı bulutta çalıştırılması gereken simülasyonların sayısını belirler. Sondalardan daha fazla düğüm belirtemezsiniz.
12. Bu ileti, size işin geçerli durumunu veya bu sekmede herhangi bir hata varsa, bu hataların ne olduğunu bildirir.

[Azure portalında](https://portal.azure.com)etkin işler, bilgi işlem havuzları ve depolama hakkında her zaman tam bilgi alabilirsiniz.

Bir iş çalışırken **İşi** **İptal**Et düğmesini değiştirin. Devam eden işi iptal etmek için bu düğmeyi kullanın. Bir işi iptal etmek geri alınamaz, sonuç kullanılamaz ve iptalden önce kullanılan azure işlem süresi için ücretlendirilirsiniz.

Bir kez bir fırında başladıktan sonra, Unreal kapatabilirsiniz. Projeye, düğüm türüne ve düğüm sayısına bağlı olarak, bulut fırını birkaç saat sürebilir. Projeyi yeniden yüklediğinizde ve Akustik penceresini açtığınızda, fırında iş durumu güncelleştirilir. İş tamamlandıysa, çıktı dosyası karşıdan yüklenir.

Azure kimlik bilgileri yerel makinenizde güvenli bir şekilde depolanır ve Gerçek Dışı projenizle ilişkilendirilir. Bunlar yalnızca Azure'a güvenli bir bağlantı kurmak için kullanılır.

### <a name="estimating-azure-bake-cost"></a><a name="Estimating-bake-cost"></a>Azure fırın maliyetini tahmin etme

Belirli bir fırının ne kadara mal olacağını tahmin etmek için, bir süre olan **Tahmini İşlem Maliyeti**için gösterilen değeri alın ve seçtiğiniz **VM Düğümü Türü'nün** yerel para biriminizdeki saatlik maliyetle çarpın. Sonuç düğümleri çalışır hale getirmek için gereken düğüm süresini içermez. Örneğin, $0,40/saat maliyeti olan düğüm türünüz için **Standard_F8s_v2** seçerseniz ve Tahmini İşlem Maliyeti 3 saat 57 dakika ise, işi çalıştırmak için tahmini maliyet 0,40 TL * ~4 saat = ~1,60 TL olacaktır. Fiili maliyet, düğümleri başlatmanın fazladan süresi nedeniyle büyük olasılıkla biraz daha yüksek olacaktır. Saatlik düğüm maliyetini [Azure Toplu Toplu Fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/linux) sayfasında bulabilirsiniz (kategori için "En iyi duruma getirin" veya "Yüksek performanslı bilgi işlem" seçeneğini belirleyin).

### <a name="reviewing-the-bake-results"></a>Fırın sonuçlarını gözden geçirme

Fırında tamamlandıktan sonra, voxels ve prob noktaları runtime eklentisi çalıştırarak beklenen yerlerde olup olmadığını kontrol edin.

## <a name="data-files"></a><a name="Data-Files"></a>Veri dosyaları

Çeşitli noktalarda bu eklenti tarafından oluşturulan dört veri dosyaları vardır. Çalışma zamanında bunlardan yalnızca birine ihtiyaç duyulur ve projenizin paketleme yoluna otomatik olarak eklenen İçerik/Akustik klasörüne yerleştirilir. Diğer üçü Akustik Veri klasöründe dir ve paketlenmemiştir.

* **[Project]/Config/ProjectAcoustics.cfg**: Bu dosya, Akustik Modu Kullanıcı UI'deki alanlara girdiğiniz verileri depolar. Bu dosyanın konumu ve adı değiştirilemez. Bu dosyada fırını etkileyen, ancak gelişmiş kullanıcılar için olan ve değiştirilmemesi gereken başka değerler de vardır.
* **[Project]/Content/Acoustics/[LevelName]\_AcousticsData.ace**: Bu dosya, fırın simülasyonu sırasında oluşturulan dosyadır ve sahnenizin akustiğini işlemek için çalışma zamanı tarafından kullanılan arama verilerini içerir. Bu dosyanın konumu ve **adı, Sondalar** Sekmesindeki alanlar kullanılarak değiştirilebilir. Oluşturulduktan sonra bu dosyayı yeniden adlandırmak istiyorsanız, Unreal projenizden UAsset'i silin, Dosya Gezgini'nde Unreal dışında dosyayı yeniden adlandırın ve ardından yeni bir UAsset oluşturmak için bu dosyayı Unreal'e yeniden aktarın. UAsset'i tek başına yeniden adlandırmak çalışmaz.
* **[Project]/Plugins/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData.vox**: Bu dosya voxelized akustik geometrisini ve malzeme özelliklerini depolar. **Problar** Sekmesindeki **Hesapla** düğmesini kullanarak hesaplanır. Bu dosyanın konumu ve **adı, Sondalar** Sekmesindeki alanlar kullanılarak değiştirilebilir.
* **[Project]/Eklentiler/ProjectAcoustics/AcousticsData/[LevelName]\_AcousticsData\_config.xml**: Bu dosya, **Problar** Sekmesindeki **Hesapla** düğmesi kullanılarak hesaplanan parametreleri depolar. Bu dosyanın konumu ve **adı, Sondalar** Sekmesindeki alanlar kullanılarak değiştirilebilir.

Azure'dan indirilen *.ace dosyasını silmemeye dikkat edin. Bu dosya, sahneyi yeniden düzenlemedışında kurtarılamaz.

## <a name="next-steps"></a>Sonraki adımlar
* [Unreal için tasarım denetimlerini](unreal-workflow.md) keşfedin
* Project [Acoustics tasarım konseptlerini](design-process.md) keşfedin

