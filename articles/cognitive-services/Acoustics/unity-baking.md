---
title: Project Acoustics Unity Bake öğreticisi
titlesuffix: Azure Cognitive Services
description: Bu öğreticide, Unity 'de Project Acoustics ile Acoustics fırlama açıklanmaktadır.
services: cognitive-services
author: kegodin
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: kegodin
ROBOTS: NOINDEX
ms.openlocfilehash: 2362b3916d1b1f430350d975dc0b61914a777be2
ms.sourcegitcommit: ad9120a73d5072aac478f33b4dad47bf63aa1aaa
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/01/2019
ms.locfileid: "68706686"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Project Acoustics Unity Bake öğreticisi
Bu öğreticide, Unity 'de Project Acoustics ile Acoustics fırlama açıklanmaktadır.

Yazılım gereksinimleri:
* Windows için [Unity 2018.2 +](https://unity3d.com)
* Unity projenizde veya [Project Acoustics Unity örnek içeriğinde](unity-quickstart.md) [Tümleşik proje Acoustics eklentisi](unity-integration.md)
* İsteğe bağlı: Bulut bilgi işlem kullanarak bakışları hızlandırmak için [Azure Batch bir hesap](create-azure-account.md)

## <a name="open-the-project-acoustics-bake-window"></a>Project Acoustics hazırlama penceresini açın
Unity menüsünden **pencere > Acoustics** seçin:

![Acoustics Window menü seçeneğinin vurgulandığı Unity düzenleyicisinin ekran görüntüsü](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Gezinti kafesi oluşturma
Project Acoustics benzetim için dinleyici araştırma noktaları yerleştirmek üzere bir gezinti ağı kullanır. Unity 'nin [Gezinti kafesi iş akışını](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html)kullanabilir veya kendi ağ yayınınızı tasarlamak için başka bir 3B modelleme paketi kullanabilirsiniz. 

## <a name="mark-acoustic-scene-objects"></a>Akustik sahne nesnelerini işaretle
Project Acoustics simülasyonu için iki tür sahne nesnesi kullanır: simülasyonda bir şekilde yansıtacaktır ve occlude sesi alacak olan nesneler ve benzetimde dinleyici araştırma noktalarını kısıtlayan oynatıcı gezinti kafesi. Her iki nesne türü de **nesneler** sekmesi kullanılarak işaretlenir. 

Nesneleri işaretlemek yalnızca **AcousticsGeometry** veya **AcousticsNavigation** bileşenlerini nesnesine eklediğinden, nesneleri işaretlemek veya Işaretlerini kaldırmak için [Standart Unity bileşeni iş akışını](https://docs.unity3d.com/Manual/UsingComponents.html) da kullanabilirsiniz. Yalnızca kafesler ve Terrat 'lar işaretlenebilir. Diğer tüm nesne türleri yok sayılacak. Onay kutuları etkilenen tüm nesneleri işaretler veya işaretini kaldırılır.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>Akustik occlusiyon ve yansıma geometrisini işaretle
**Acoustics** penceresinin **nesneler** sekmesini açın. Nesnelerin occlude, yansıtmaları veya artışlarını devralarak seslerinin **Acoustics geometrisi** olarak işaretlenmesi. Acoustics geometrisi, toprak, duvarlar, Roofs, Windows & pencere camı, Rug 'ler ve büyük mobilya gibi şeyleri içerebilir. Bu nesneler için herhangi bir rastgele karmaşıklık düzeyi kullanabilirsiniz. Sahnenin simülasyonundan önce voxlik olduğu için, çok küçük yaprakları olan ağaçlar gibi son derece ayrıntılı kafesler, Basitleştirilmiş nesnelerden daha pahalı değildir.

Görünmez çakışma kafesleri gibi Acoustics etkilemeyen şeyleri eklemeyin.

Bir nesnenin araştırma hesaplaması sırasında dönüştürme işlemi (aşağıdaki yoklamalar sekmesi aracılığıyla) , hazırlama sonuçlarında düzeltilir. Sahnedeki işaretli nesnelerden herhangi birinin taşınması, araştırma hesaplamasının yeniden yapılması ve sahnenin yeniden gerçekleştirilmesi için gerekli olacaktır.

### <a name="mark-the-navigation-mesh"></a>Gezinti kafesi işaretle
Unity 'nin iş akışıyla oluşturulan gezinti kafesleri Acoustics sistemi tarafından alınacaktır. Kendi kafeslerinizi kullanmak için **nesneler** sekmesinden işaretleyin.

### <a name="for-reference-the-objects-tab-parts"></a>Başvuru için: Nesneler sekmesi bölümleri
Sekme sayfasının bölümleri şunlardır:

1. Sekme seçimi düğmeleri (**nesneler** sekmesi seçili). Bu düğmeleri, soldan sağa Acoustics Bake yapmak için çeşitli adımlarda izlenecek şekilde kullanın.
2. Bu sayfayı kullanarak yapmanız gerekenler hakkında kısa bir açıklama.
3. Hiyerarşi penceresi için kullanılabilir filtreler. Bunları daha kolay bir şekilde işaretleyebilmeniz için Hiyerarşi penceresini belirtilen türdeki nesnelere filtrelemek için bunu kullanın. Acoustics için henüz bir şey işaretlenmediyse, son iki seçenekten seçim yapmanız hiçbir şey göstermeyecektir. Ancak, bunu yaptıktan sonra işaretlenen nesneleri bulmak yararlı olabilir.
4. Hiçbir nesne seçili değilse, bu bölümde sahnenin tüm nesnelerinin durumu gösterilmektedir:
    * Toplam-sahnedeki etkin, gizli olmayan nesnelerin toplam sayısı.
    * Yoksayıldı-ağ Oluşturucu ya da Terrat olmayan nesne sayısı.
    * Kafes-sahnedeki kafes Oluşturucu nesnelerinin sayısı
    * Teryağmur-sahnedeki Teryağmur nesnelerinin sayısı
    * Geometri-"Acoustics Geometry" olarak işaretlenen sahnenin ağ veya Teryağmur nesnesi sayısı
    * Gezinti-"Acoustics Navigation" olarak işaretlenen sahnenin ağ veya Teryağmur nesnelerinin sayısı. Bu sayı Unity 'nin Navkafesi içermez.
5. Yalnızca kafesler ve Terrat 'lar olan sahnenin ' işaretleme özellikli ' nesnelerinin toplam sayısını gösterir. Acoustics için bu nesnelere geometri veya gezinti olarak işaretlemek (uygun bileşeni eklemek) için kullanabileceğiniz onay kutularını gösterir
6. Hiçbir şey seçili olmadığında, bu notta gerektiğinde işaretlemek için nesneleri seçmenizi hatırlatır. Sahnedeki tüm nesneleri hiçbir şey seçmeden işaretlemek için bir veya iki onay kutusu da denetleyebilirsiniz.
7. Nesneler seçildiğinde, bu bölüm yalnızca seçili nesnelerin durumunu gösterir.
8. ' İşaretle '-Able ' seçili nesnelerinin toplam sayısını gösterir. Onay kutularının denetlenmesi veya denetlenmesi, yalnızca seçilen nesneleri işaretler veya işaretini kaldırır.

Sahnede seçili bir şey yoksa, nesneler sekmesi aşağıdaki resme benzer şekilde görünür:

![Seçim olmadan Acoustics Objects sekmesinin ekran görüntüsü](media/objects-tab-no-selection-detail.png)

Sahnede veya hiyerarşi pencerenizde seçilmiş bir şey varsa, aşağıdaki resme benzer şekilde görünür:

![Seçim gösterilen Acoustics Objects sekmesinin ekran görüntüsü](media/objects-tab-selection-detail.png)

Bazı nesneler işaretliyse ve bazıları yoksa, uygun onay kutusu bir "mixed" değeri gösterir:

![Karışık seçim simgesi vurgulanmış şekilde Acoustics Objects sekmesinin ekran görüntüsü](media/mixed-object-selection-detail.png)

Onay kutusuna tıkladığınızda tüm nesneler işaretlenir ve yeniden tıklanması tüm nesnelerin işaretini kaldırılır.

Nesneler hem geometri hem de gezinme için işaretlenebilir.

## <a name="select-acoustic-materials"></a>Akustik malzemeleri seçin
Nesneleriniz işaretlendiğinde, **malzemeler** düğmesine tıklayın ve akustik malzemeler atayın. Project Acoustics malzemeleri sistemi Unity görsel malzemeler sistemine bağlıdır: iki nesnenin ayrı akustik malzemelere sahip olması için, ayrı görsel malzemelere sahip olmaları gerekir.

Akustik malzemeler her bir yüzeyden geri yansıtılan ses enerji miktarını denetler. Varsayılan akustik malzemeler somut ' a benzer. Project Acoustics, görsel malzeme adına göre malzemeler önerir. Bir çok büyük bir katsayı kaydırıcısını etkinleştirmek için, ' Custom ' akustik malzemesini bir malzemeye atayabilirsiniz.

Bir odadaki belirli bir malzemenin geri alma süresi, 0,01 ile 0,20 arasında çok sayıda malzemeyle ilgili değer elde eden, büyük bir ihtimalle kendi emme katsayısı ile ilgilidir. Bu aralığın dışında, yaygın olmayan katların bulunduğu malzemeler çok büyük bir yerdir.

![Bağımsız katsayı ile birlikte ters geçen sürenin negatif bağıntısını gösteren grafik](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Başvuru için: Malzemeler sekmesinin bölümleri
![Unity 'de Acoustics malzemeler sekmesinin ekran görüntüsü](media/materials-tab-detail.png)

1. Bu sayfayı getirmek için kullanılan **malzemeler** sekmesi düğmesi.
2. Bu sayfayı kullanarak yapmanız gerekenler hakkında kısa bir açıklama.
3. İşaretlendiğinde, yalnızca **Acoustics geometrisi** olarak işaretlenen nesneler tarafından kullanılan malzemeler listelenir. Aksi takdirde, sahnede kullanılan tüm malzemeler listelenecektir.
4. Aşağıdaki Acoustics sütunundaki bir açılan listeye tıkladığınızda gösterilen açılan menünün sırasını değiştirmek için bu seçenekleri kullanın (#6). **Ad** , akustik malzemeleri ada göre sıralar. "Absorptivity", bunları düşük ve yüksek olan absorptivity sırasıyla sıralar.
5. Sahnede kullanılan malzemelerin listesi alfabetik olarak sıralanır. **Yalnızca Işaretli göster** onay kutusu işaretliyse (#3), yalnızca **Acoustics geometrisi** olarak işaretlenen nesneler tarafından kullanılan malzemeler gösterilir. Buradaki bir malzemeye tıklanması, sahformdaki bu malzemeyi kullanan tüm nesneleri seçer.
6. Sahne malzemelerinin atandığı akustik malzemeleri gösterir. Bir sahne malzemesini farklı bir akustik malzemeye yeniden atamak için bir açılır listeye tıklayın. **Aşağıdaki sıralama Acoustics by:** yukarıdaki Seçenekler ' i kullanarak bir öğeye tıkladığınızda gösterilen menünün sıralama düzenini değiştirebilirsiniz (#4).
7. Önceki sütunda seçili olan malzemenin akustik bir katsayısını gösterir. Sıfır değeri tam yansıtıcı (hiçbir şekilde) anlamına gelir, 1 değeri mükemmel bir şekilde (yansıma yoktur). Seçili malzeme "Custom" olmadığı takdirde, bu katsayı katsayısı değiştirilemez.
8. "Custom" öğesine atanan bir malzeme için kaydırıcı artık devre dışı olmaz ve kaydırıcıyı kullanarak ya da bir değer yazarak veya bir değer yazarak bir veya daha fazla yazı katsayısı seçebilirsiniz.

## <a name="calculate-and-review-listener-probe-locations"></a>Dinleyici araştırma konumlarını hesapla ve gözden geçirme
Malzemeleri atadıktan sonra, yoklamalar sekmesine geçin ve benzetim için dinleyici araştırma noktaları yerleştirmek için **Hesapla** ' ya tıklayın.

### <a name="what-the-calculate-button-calculates"></a>"Calculate..." düğme hesaplar
**Calculate...** Button, sahneyi benzetim için hazırlamak üzere seçtiğiniz akustik sahne geometrisini kullanır:

1. Sahne alanı kafeslerden yararlanır ve bir Voxel birimini hesaplar. Voxel birimi, sahsitenizin tamamını kapsayan 3 boyutlu bir birimdir ve küçük üçüncü dereceden "voxfit" den oluşur. Voxbüyüklük 'ın boyutu simülasyon **çözünürlüğü** ayarı tarafından belirlenen benzetim sıklığıyla belirlenir. Her Voxel, "açık hava" ya da sahne geometrisi içeren olarak işaretlenir. Bir Voxel, geometri içeriyorsa, Voxel, bu geometriye atanan malzemenin büyük bir katsayısı ile etiketlenir.
2. Dinleyici araştırma noktaları yerleştirmek için gezinti kafesleri (es) kullanır. Algoritma, uzamsal kapsam ve benzetim süresinin ve dosya boyutunun rekabet sorunlarını dengeler, ancak dar ve küçük boşlukların her zaman kapsam miktarına sahip olmasını sağlar. Tipik araştırma noktası, küçük sahneler için 100 arasında, büyük sahneler için birkaç bin arasındadır.

Sahnenin boyutuna ve makinenizin hızına bağlı olarak, bu hesaplamalar birkaç dakika sürebilir.

### <a name="review-voxel-and-probe-placement"></a>Voxel ve araştırma yerleşimini gözden geçirin
Sahneye bakmaya hazırsınız olduğundan emin olmak için Voxel verilerinin ve araştırma noktası konumlarının her ikisini de önizleyin. Tamamlanmamış bir gezinti ağı veya eksik ya da ek akustik geometri, genellikle önizlemede hızlı bir şekilde görünür olur. Şeyler menüsü kullanılarak Voxel ve araştırma yerleştirmesi etkinleştirilebilir veya devre dışı bırakılabilir:

![Unity 'de şeyler menüsünün ekran görüntüsü](media/gizmos-menu.png)

Akustik geometriyi içeren Voxler yeşil küpler olarak gösterilir. Sahneye göz atın ve geometri olması gereken her şeyin Vox 'e sahip olduğunu doğrulayın. Sahne kameranın, Vox 'in gösterilmesi için nesnenin yaklaşık 5 metresinden fazla olması gerekebilir.

En iyi çözünürlük ile oluşturulan voxters çözünürlüğe göre karşılaştırırsanız, büyük ölçekli voxters 'ın büyük olduğunu görürsünüz.

![Unity düzenleyicisinde kaba voxde önizlemenin ekran görüntüsü](media/voxel-cubes-preview.png)

Simülasyon sonuçları, çalışma zamanında dinleyici araştırma noktası konumları arasında enterpolasyonlardır. Oyuncunun sahnede gezinildiği her yerde araştırma noktaları olup olmadığını kontrol edin.

![Unity düzenleyicisinde araştırma önizlemesinin ekran görüntüsü](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Sahne yeniden adlandırmalarına dikkat edin
Sahne adı, sahneyi araştırma noktası yerleşimini ve voxelileştirme dosyalarını depolayan dosyalara bağlamak için kullanılır. Araştırma noktaları hesaplandıktan sonra sahne yeniden adlandırılırsa, malzeme atama ve yerleştirme verileri kaybedilir ve yeniden çalıştırmanız gerekir.

### <a name="for-reference-parts-of-the-probes-tab"></a>Başvuru için: Yoklamalar sekmesinin bölümleri
![Unity 'de Acoustics araştırmaları sekmesinin ekran görüntüsü](media/probes-tab-detail.png)

1. Bu sayfayı getirmek Için kullanılan yoklamalar sekme düğmesi
2. Bu sayfayı kullanarak yapmanız gerekenler hakkında kısa bir açıklama
3. Bu ayarları kullanarak kaba bir benzetim veya ince simülasyon çözümü seçin. Kaba bir daha hızlıdır, ancak belirli bir avantajları vardır. Ayrıntılar için aşağıdaki [Bake çözümlemesine](bake-resolution.md) bakın.
4. Acoustics veri dosyalarının bu alan kullanılarak yerleştirilmesi gereken konumu seçin. "..." İle düğmeye tıklayın bir klasör seçici kullanmak için. Varsayılan olarak **varlıklardır/AcousticsData**. Bu konum altında bir **Düzenleyici** alt klasörü de oluşturulacaktır. Veri dosyaları hakkında daha fazla bilgi için aşağıdaki [veri dosyalarına](#Data-Files) bakın.
5. Bu sahnenin veri dosyaları burada belirtilen ön ek kullanılarak adlandırılacak. Varsayılan değer "Acoustics_ [Sahname]" dir.
6. Yoklamalar hesaplandıktan sonra yukarıdaki denetimler devre dışı bırakılır. Hesaplamaları silmek ve yeni ayarları kullanarak yeniden hesaplayabilmeniz için denetimleri etkinleştirmek üzere **Temizle** düğmesine tıklayın.
7. Sahneyi indirip araştırma noktası konumlarını hesaplamak için **Calculate...** düğmesine tıklayın. Bu, makinenizde yerel olarak yapılır ve bir fırt yapılmadan önce gerçekleştirilmelidir.

Project Acoustics 'in bu sürümünde, yoklamalar el ile yerleştirilemez ve **yoklamalar** sekmesinde belirtilen otomatik işlem aracılığıyla yerleştirilmelidir.

Daha ayrıntılı bir çözüm hakkında daha fazla bilgi için bkz. [Bake çözünürlüğü](bake-resolution.md) .

## <a name="bake-your-scene-using-azure-batch"></a>Azure Batch kullanarak sahne bakın
Azure Batch hizmetini kullanarak sahneye bulutta bir işlem kümesiyle bakmanıza neden olabilirsiniz. Project Acoustics Unity eklentisi, her Bake için bir Azure Batch kümesini oluşturmak, yönetmek ve bölmek için Azure Batch doğrudan bağlanır. **Bake** sekmesinde, Azure kimlik bilgilerinizi girin, bir küme makinesi türü ve boyutu seçin ve **Bake**' ye tıklayın.

### <a name="for-reference-parts-of-the-bake-tab"></a>Başvuru için: Bake sekmesinin parçaları
![Unity 'de Acoustics Bake sekmesinin ekran görüntüsü](media/bake-tab-details.png)

1. Bu sayfayı getirmek için kullanılan Bake sekmesi düğmesi.
2. Bu sayfada yapılacaklar hakkında kısa bir açıklama.
3. Azure hesabınız oluşturulduktan sonra Azure kimlik bilgilerinizi girme alanları. Daha fazla bilgi için bkz. [Azure Batch hesabı oluşturma](create-azure-account.md).
4. Acoustics araç takımı için Docker Image etiketi.
5. Aboneliklerinizi yönetmek, kullanımı izlemek ve faturalandırma bilgilerini görüntülemek için Azure portal başlatın. 
6. Hesaplama için kullanılacak Azure Batch işlem düğümü türü. Düğüm türü, Azure veri merkezi konumunuz tarafından desteklenmelidir. Emin değilseniz, **Standard_F8s_v2**adresinden ayrılın.
7. Bu hesaplama için kullanılacak düğüm sayısı. Buraya girdiğiniz sayı, fırt 'in tamamlanma süresini etkiler ve Azure Batch çekirdek ayırmala sınırlıdır. Varsayılan ayırma yalnızca iki 8 çekirdekli düğüm veya 1 16 çekirdek düğümüne izin verir, ancak Genişletilebilir. Çekirdek ayırma kısıtlamaları hakkında daha fazla bilgi için bkz. [Azure Batch hesabı oluşturma](create-azure-account.md).
8. İşlem havuzunuzu [düşük öncelikli düğümleri](https://docs.microsoft.com/azure/batch/batch-low-pri-vms)kullanacak şekilde yapılandırmak için bu onay kutusunu seçin. Düşük öncelikli işlem düğümleri çok daha düşük maliyetlidir, ancak her zaman kullanılamayabilir veya herhangi bir zamanda yok edilebilir.
9. **Yoklamanız** sekmesinde hesaplanan şekilde sahnenin araştırma sayısı. Yoklamaların sayısı, bulutta çalıştırılması gereken benzetimleri sayısını belirler. Yoklamalara göre daha fazla düğüm belirtemezsiniz.
10. İşinizin bulutta çalışması için gerçekleşmesi beklenen geçen süre miktarı. Bu, düğüm başlangıç saatini içermez. İş çalışmaya başladıktan sonra, sonuçları geri almadan önce ne kadar süreyle olması gerektiğine ilişkin olur. Bunun yalnızca bir tahmin olduğunu unutmayın.
11. Benzetimleri çalıştırmak için gereken bilgi işlem zamanının toplam miktarı. Bu, Azure 'da kullanılacak olan düğüm işlem zamanının toplam miktarıdır. Bu değeri kullanma hakkında daha fazla bilgi için bkz. [hazırlama maliyeti tahmini](#Estimating-bake-cost) .
12. Bu ileti, iş tamamlandığında hazırlama sonuçlarının kaydedileceği yeri belirtir.
13. (Yalnızca Gelişmiş kullanım) Bir nedenden dolayı, gönderdiğiniz bir fırt hakkında daha fazla bilgi almak için Unity 'ye zorlamanız gerekir (örneğin, başka bir makine kullanarak sonuçları indirdiniz), gönderilen işi unutmak için **durumu temizle** düğmesine tıklayın. Bu, sonuç dosyasının hazırlandığına indirilmeyeceği anlamına gelir ve bu , **işi iptal etme ile aynı değildir**. Çalışıyorsa, iş bulutta çalışmaya devam edecektir.
14. Fırt **düğmesine tıklayarak** , hazırlama 'yi buluta gönderebilirsiniz. Bir iş çalışırken bunun yerine **Iptal işi** gösterilir.
15. Bilgisayarınızda [Acoustics simülasyonu](#Local-bake)işlemeye hazırlar.
16. Bu alan, Bake 'nin durumunu gösterir. Tamamlandığında, Indirmeli gösterilmesi gerekir.

[Azure Portal](https://portal.azure.com)etkin işler, işlem havuzları ve depolama hakkında her zaman bir bilgi edinebilirsiniz.

Bir iş, **Işi iptal**etmek Için **Bake** düğmesi değişikliklerini çalıştırırken. Devam eden işi iptal etmek için bu düğmeyi kullanın. İş iptal edilmeden önce onaylamanız istenir. Bir işin iptal edilmesi geri alınamaz, hiçbir sonuç kullanılamayacak ve yine de kullanılan tüm Azure işlem süresi ücretlendirilecektir.

Bir Bake başladıktan sonra Unity 'yi kapatabilirsiniz. Projeye, düğüm türüne ve düğüm sayısına bağlı olarak, bir bulut fırından birkaç saat sürebilir. Projeyi yeniden yüklediğinizde ve Acoustics penceresini açtığınızda hazırlama iş durumu güncelleştirilecektir. İş tamamlanırsa, çıkış dosyası indirilir.

Azure kimlik bilgileri yerel makinenizde güvenli bir şekilde depolanır ve Unity düzenleyicinizle ilişkilendirilir. Yalnızca Azure ile güvenli bir bağlantı kurmak için kullanılır.

### <a name="Estimating-bake-cost"></a>Azure hazırlama maliyeti tahmini

Belirli bir fırt 'in maliyet olacağını tahmin etmek için, **Tahmini Işlem maliyeti**için gösterilen değeri alın ve bu süre, seçtiğiniz **VM düğüm türünün** yerel para birimindeki saatlik maliyet ile çarpın. Sonuç, düğümlerin çalışır duruma getirmek için gereken düğüm süresini içermez. Örneğin, düğüm türü için $0.40/sa maliyeti olan **Standard_F8s_v2** ' u seçerseniz ve tahmini işlem maliyeti 3 saat ve 57 dakika ise, işi çalıştırmak için tahmini maliyet $0,40 * ~ 4 saat = ~ $1,60 olur. Düğümlerin başlatılması, daha fazla zaman kaplamasından kaynaklanabilir. Saatlik düğüm maliyetini [Azure Batch fiyatlandırma](https://azure.microsoft.com/pricing/details/virtual-machines/linux) sayfasında bulabilirsiniz (kategori Için "işlem için iyileştirilmiş" veya "yüksek performanslı işlem" seçeneğini belirleyebilirsiniz).

## <a name="Local-bake"></a>Bilgisayarınızda sahne bakın
Sahninizi kendi bilgisayarınızda bulabilirsiniz. Bu, bir Azure Batch hesabı oluşturmadan önce küçük sahneler ile Acoustics ile denemeler yapmak için yararlı olabilir. Acoustics simülasyonun, sahnenin boyutuna bağlı olarak uzun zaman alabilir.

### <a name="minimum-hardware-requirements"></a>En düşük donanım gereksinimleri
* En az 8 çekirdeğe ve 32 GB RAM 'e sahip bir x86-64 işlemcisi

Örnek olarak, Intel Xeon E5-1660 @ 3 GHz ve 32 GB RAM 'e sahip 8 çekirdekli bir makinede sınamamız sırasında
* 100 yoklamaların bulunduğu küçük bir sahne, ince bir fırt veya 32 saat boyunca yaklaşık 2 saat sürebilir.
* 1000 araştırmaların bulunduğu orta ölçekli bir sahne, çok büyük bir fırt veya 21 gün boyunca yaklaşık 20 saat sürebilir.

### <a name="setup-docker"></a>Docker 'ı ayarlama
Simülasyonu işleyecek BILGISAYAR üzerinde Docker 'ı yükleyip yapılandırma
1. [Docker araç takımını](https://www.docker.com/products/docker-desktop)yükler.
2. Docker ayarlarını başlatın, "Gelişmiş" seçeneklerine gidin ve kaynakları en az 8 GB RAM 'e sahip olacak şekilde yapılandırın. Docker 'a ayırabileceğiniz CPU sayısı ne kadar yüksekse, fırt daha hızlı tamamlanır. ![Örnek Docker ayarlarının ekran görüntüsü](media/docker-settings.png)
3. "Paylaşılan sürücüler" e gidin ve işlenmek üzere kullanılan sürücü için paylaşımı açın.![Docker paylaşılan sürücü seçeneklerinin ekran görüntüsü](media/docker-shared-drives.png)

### <a name="run-local-bake"></a>Yerel hazırlama Çalıştır
1. **Bake** sekmesinde "yerel Bake hazırla" düğmesine tıklayın ve giriş dosyalarının ve yürütme betiklerinin kaydedileceği bir klasör seçin. Daha sonra, en düşük donanım gereksinimlerini karşılayan ve klasörü o makineye kopyalayarak Docker 'ın yüklü olduğu sürece herhangi bir makinede FIRE 'yi çalıştırabilirsiniz.
2. "Runlocalbake. bat" betiğini kullanarak benzetimi başlatın. Bu betik, proje Acoustics Docker görüntüsünü benzetim işleme için gereken araç kümesiyle getirecek ve benzetimi başlatacak. 
3. Simülasyon bittikten sonra, elde edilen. ACE dosyasını Unity projenize geri kopyalayın. Unity 'nin bunu bir ikili dosya olarak tanımasını sağlamak için dosya uzantısına ". Bytes" ekleyin (örneğin, "scene1. Ace. Bytes"). Benzetim için ayrıntılı Günlükler "AcousticsLog. txt" içinde depolanır. Herhangi bir sorunla karşılaşırsanız, bu dosyayı tanılamada yardımcı olacak şekilde paylaşabilirsiniz.

## <a name="Data-Files"></a>Hazırlama işlemi tarafından eklenen veri dosyaları

Hazırlama işlemi sırasında oluşturulan dört veri dosyası vardır. Biri simülasyon sonuçlarını içerir ve başlığınız ile birlikte gelir. Diğer kullanıcılar Unity Düzenleyicisi ile ilgili verileri depolar.

Simülasyon sonucu:
* **Varlıklar/AcousticsData/Acoustics\_[manzara adı]. Ace. Bytes**: Bu, çalışma zamanı arama tablosudur ve simülasyonu sonuçları ile voxtitilen akustik sahne öğelerini içerir. Bu dosyanın konumu ve adı, **yoklamalar** sekmesindeki alanlar kullanılarak değiştirilebilir.

Simülasyon sonuç dosyasını silmamak için dikkatli olmanız gerekmez. Sahnenin geri bakmasının dışında kurtarılabilir değildir.

Düzenleyici veri dosyaları:
* **Varlıklar/düzenleyici/[manzara adı]\_AcousticsParameters. varlık**: Bu dosya, Acoustics Kullanıcı arabirimindeki alanlara girdiğiniz verileri depolar. Bu dosyanın konumu ve adı değiştirilemez.
* **Varlıklar/AcousticsData/Editor/Acoustics_ [manzara adı]. insan sesi**: Bu dosya, araştırma sekmesindeki **hesapla...** düğmesi kullanılarak hesaplanan voxeliacoustics geometrisini ve malzeme özelliklerini depolar. Bu dosyanın konumu ve adı, **yoklamalar** sekmesindeki alanlar kullanılarak değiştirilebilir.
* **Varlıklar/AcousticsData/Editor/Acoustics\_[manzara adı]\_config. xml**: Bu dosya, yoklamalar sekmesindeki **Calculate...** düğmesini kullanarak hesaplanan simülasyon parametrelerini depolar. Bu dosyanın konumu ve adı, **yoklamalar** sekmesindeki alanlar kullanılarak değiştirilebilir.

## <a name="set-up-the-acoustics-lookup-table"></a>Acoustics arama tablosunu ayarlama
Projeyi **Acoustics** prefab öğesine sürükleyip Proje panelinden sahneye bırakın:

![Unity 'de Acoustics prefab ekran görüntüsü](media/acoustics-prefab.png)

**Projectacoustika** oyun nesnesine tıklayın ve Inspector paneline gidin. Hazırlama sonucunun konumunu belirtin (. ACE dosyası, **varlıklar/AcousticsData**) Acoustics Manager betiğine sürükleyip bırakarak veya metin kutusunun yanındaki daire düğmesine tıklayarak.

![Unity 'de Acoustics Manager prefab ekran görüntüsü](media/acoustics-manager.png)  

## <a name="next-steps"></a>Sonraki adımlar
* [Unity için tasarım denetimlerini](unity-workflow.md) keşfet
* [Project Acoustics tasarım kavramlarını](design-process.md) keşfet

