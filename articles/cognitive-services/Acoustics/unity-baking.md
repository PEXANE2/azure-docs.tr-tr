---
title: Proje Akustik Birlik Bake Öğretici
titlesuffix: Azure Cognitive Services
description: Bu öğretici, Unity Project Acoustics ile akustik pişirme açıklar.
services: cognitive-services
author: NoelCross
manager: nitinme
ms.service: cognitive-services
ms.subservice: acoustics
ms.topic: tutorial
ms.date: 03/20/2019
ms.author: noelc
ROBOTS: NOINDEX
ms.openlocfilehash: 310decf8053ea16ba46250ba3aabe81c9c254e5e
ms.sourcegitcommit: 9ee0cbaf3a67f9c7442b79f5ae2e97a4dfc8227b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "72243118"
---
# <a name="project-acoustics-unity-bake-tutorial"></a>Proje Akustik Birlik Bake Öğretici
Bu öğretici, Unity Project Acoustics kullanarak akustik pişirme açıklar.

Yazılım gereksinimleri:
* Windows veya MacOS için [Unity 2018.2+](https://unity3d.com)
* Unity projenize veya [Project Acoustics Unity örnek içeriğine](unity-quickstart.md) [entegre edilmiş Project Acoustics eklentisi](unity-integration.md)
* *İsteğe bağlı:* Bulut bilgi işlem kullanarak fırınları hızlandırmak için bir [Azure Toplu İş hesabı](create-azure-account.md)

## <a name="open-the-project-acoustics-bake-window"></a>Project Acoustics fırın penceresini açın
Unity'de **Pencere** menüsünden **Akustik'i** seçin.

![Pencere menüsünde vurgulanan Akustik seçeneğine sahip Unity düzenleyicisi](media/window-acoustics.png)

## <a name="create-a-navigation-mesh"></a>Gezinme kafesi oluşturma
Project Acoustics simülasyon için dinleyici sonda noktaları yerleştirmek için bir navigasyon örgü kullanır. Unity [gezinti örgü iş akışını](https://docs.unity3d.com/Manual/nav-BuildingNavMesh.html)kullanabilirsiniz. Veya kendi örgü tasarlamak için farklı bir 3B modelleme paketi kullanabilirsiniz.

## <a name="mark-acoustic-scene-objects"></a>Akustik sahne nesnelerini işaretle
Project Acoustics simülasyon için iki tür sahne nesnesi kullanır:
- Simülasyonda sesi yansıtan ve tıkayan nesneler
- Simülasyonda dinleyici sonda noktalarını kısıtlayan oyuncu navigasyon kafesi

Her iki nesne türü de **Nesneler** sekmesi kullanılarak işaretlenir.

Nesneleri işaretlemek yalnızca *nesneye AkustikGeometri* veya *Akustik Navigasyon* bileşenleri ni eklediklerinden, nesneleri işaretlemek veya işaretlemek için [standart Unity bileşeni iş akışını](https://docs.unity3d.com/Manual/UsingComponents.html) da kullanabilirsiniz. Yalnızca kafes görüntüleyicilerini ve arazileri işaretleyebilirsiniz. Diğer tüm nesne türleri yoksayılır. Onay kutuları etkilenen tüm nesneleri işaretler veya işaretini işaretler.

### <a name="mark-acoustic-occlusion-and-reflection-geometry"></a>İşaret akustik oklüzyon ve yansıma geometrisi
**Akustik** penceresinin **Nesneler** sekmesini açın. Herhangi bir nesneyi, sesi tıkayacak, yansıtacak veya emerlerse *Akustik Geometri* olarak işaretleyin. Akustik geometri zemin, duvarlar, çatılar, pencere ve pencere camı, kilim ve büyük mobilya gibi şeyler içerebilir. Bu nesneler için herhangi bir rasgele karmaşıklık düzeyini kullanabilirsiniz. Sahne simülasyondan önce voxelized olduğundan, çok yaprağı olan ağaçlar gibi son derece ayrıntılı meshes, basitleştirilmiş nesneleri daha pişirmek için daha pahalı değildir.

Görünmez çarpışma meshes gibi akustiği etkilememesi gereken şeyler eklemeyin.

Sonda hesaplaması sırasında **(Problar** sekmesi aracılığıyla) bir nesnenin dönüşümü fırın sonuçlarında sabitlenir. Sahnede işaretli nesneler daha sonra taşınırsa, sonda hesaplama ve pişirme yeniden yapılmalıdır.

### <a name="mark-the-navigation-mesh"></a>Gezinti örgüsi işaretle
Unity iş akışı aracılığıyla oluşturulan gezinti meshes Akustik sistemi tarafından alınacaktır. Kendi kaçalınızı kullanmak için nesneler **sekmesinden** işaretleyin.

### <a name="for-reference-the-objects-tab-parts"></a>Başvuru için: Nesneler sekmesi parçaları
Sekme sayfasının bölümleri (açıklamalardan sonra resimde) şunlardır:

1. Sekme seçim düğmeleri **(Nesneler** sekmesi seçili). Bir akustik fırında çeşitli adımlar arasında hareket etmek için bu düğmeleri kullanın, soldan sağa.
1. Bu sekmeyi kullanarak neler yapabileceğinizin kısa bir açıklaması.
1. Hiyerarşi penceresi için kullanılabilir filtreler. Hiyerarşi penceresini, kolayca işaretleyebilmeniz için belirtilen türdeki nesnelere filtrelemek için bu seçenekleri kullanın. Henüz akustik için bir şey işaretlemediyseniz, son iki seçeneği seçmek size hiçbir şey göstermez. Ancak, bu seçenekler işaretlendikten sonra nesneleri bulmanıza yardımcı olur.
1. Hiçbir nesne seçilmediğinde, bu bölümde sahnedeki tüm nesnelerin durumu gösterilmektedir.
    * Toplam: Etkin, gizli olmayan nesnelerin toplam sayısı.
    * Yoksayılan: Kafes oluşturan veya arazi oluşturmayan nesnelerin sayısı.
    * Kafes: Kafes işleyici nesnelerinin sayısı.
    * Arazi: Arazi nesnelerinin sayısı.
    * Geometri: **Akustik Geometri**olarak işaretlenmiş kafes veya arazi nesnelerinin sayısı.
    * Gezinme: **Akustik Gezinti**olarak işaretlenmiş kafes veya arazi nesnelerinin sayısı. Bu numara Unity NavMesh'i içermez.
1. Yalnızca kafes görüntüleyicileri ve arazileri olan sahnedeki toplam "işaretlenebilir" nesne sayısı. Bu nesneleri akustik için geometri veya gezinme olarak işaretlemek (uygun bileşeni eklemek) için onay kutularını kullanın.
1. Hiçbir şey seçilmediğinde, bu not gerekirse işaretleme için nesneleri seçmenizi hatırlatır. Ayrıca, sahnedeki tüm nesneleri işaretlemek için bir veya her iki onay kutusunu da seçebilirsiniz.
1. Nesneler seçildiğinde, bu bölümde yalnızca seçili nesnelerin durumunu gösterir.
1. Seçili nesnelerin toplam sayısı "işaretlenebilir". Onay kutularını seçmek veya temizlemek yalnızca seçili nesneleri işaretler veya işaretleri unmarks.

Sahnenizde hiçbir şey seçilmediyse, **Nesneler** sekmesi aşağıdaki resme benzer.

![Akustik Nesneler sekmesi hiçbir şey seçili](media/objects-tab-no-selection-detail.png)

Sahneveya hiyerarşi pencerenizde seçili bir şey varsa, sekme aşağıdaki resme benzer.

![Seçimlerle Akustik Nesneler sekmesi](media/objects-tab-selection-detail.png)

Bazı nesneler işaretlenmişse ve bazıları işaretlenmemişse, uygun onay kutuları aşağıdaki resimdeki gibi "karışık" bir değer gösterir.

![Vurgulanan simgelerin karışık bir seçkisi ile Akustik Nesneler sekmesi](media/mixed-object-selection-detail.png)

Tüm öğeleri işaretlemek için onay kutusunu seçin. Tüm nesnelerin işaretini çıkarmak için seçin.

Nesneler hem geometri hem de gezinme için işaretlenebilir.

## <a name="select-acoustic-materials"></a>Akustik malzemeleri seçin
Nesneleriniz işaretlendikten sonra **Malzemeler** düğmesini seçin. Sonra akustik malzemeler atayın. Project Acoustics malzeme sistemi Unity görsel malzeme sistemine bağlıdır. İki nesnenin ayrı akustik malzemelere sahip olması için ayrı görsel malzemelere sahip olmaları gerekir.

Akustik malzeme seçimi, her yüzeyden yansıyan ses enerjisi miktarını belirler. Varsayılan akustik malzeme nin çelikten benzer emilimi vardır. Project Acoustics görsel malzeme adına dayalı malzemeler önerir. Ayrıca, ayarlanabilir soğurma katsayısı kaydırıcısını etkinleştirmek için akustik malzeme **Özel'i** bir malzemeye atayabilirsiniz.

Bir odadaki bir malzemenin yankılanma süresi, emme katsayısı ile ters orantılıdır. Çoğu malzeme0,01 ile 0,20 aralığında emme değerlerine sahiptir. Bu aralığın dışında emme katsayıları olan malzemeler çok emicidir.

![Grafik, yankılanma süresi ve soğurma katsayısının negatif korelasyonuna neden olur.](media/reverb-time-graph.png)

### <a name="for-reference-parts-of-the-materials-tab"></a>Başvuru için: Malzemeler sekmesinin bölümleri
![Unity Akustik Malzemeler sekmesi](media/materials-tab-detail.png)
1. **Malzemeler** düğmesi bu sekmeyi görüntüler.
2. Bu sekmeyi kullanarak neler yapabileceğinizin kısa bir açıklaması.
3. Bu onay kutusu seçildiğinde, yalnızca **Akustik Geometri** olarak işaretlenmiş nesneler tarafından kullanılan malzemeler listelenir. Aksi takdirde, sahnede kullanılan tüm malzemeler listelenir.
4. **Akustik** sütunundaki (#6) menüdeki seçeneklerin sırasını değiştirmek için bu seçenekleri kullanın. Akustik malzemeleri **Ada** veya **Absorptivity'e**göre düşükten yükseğe sıralayın.
5. Sahnede kullanılan malzemelerin alfabetik olarak sıralanmış bir listesi. Yalnızca **İşaretli Göster** onay kutusu seçilirse (#3), yalnızca **Akustik Geometri** olarak işaretlenmiş nesneler tarafından kullanılan malzemeler gösterilir. Bu malzemeyi kullanan sahnedeki tüm nesneleri seçmek için burada bir malzeme seçin.
6. Sahne materyalinin atandığı akustik malzeme. O sahne malzemesine atanan akustik malzemeyi değiştirmek için herhangi bir öğeyi seçin. Bu menülerin sıralama sırasını değiştirmek **için, Akustikleri** Sırala seçeneklerini (#4) kullanın.
7. Soldaki sütunda seçilen malzemenin akustik soğurma katsayısı (#6). 0 değeri mükemmel yansıtıcı (emme yok) anlamına gelirken, 1 mükemmel derecede emilimi (yansıma yok) anlamına gelir. Seçilen malzeme Özel olmadığı sürece soğurma katsayısı **değiştirilemez.**
8. **Özel**olarak işaretlenmiş bir malzeme için kaydırıcı etkinleştirilir. Bir emme katsayısı atamak için kaydırıcıyı taşıyabilir veya bir değer yazabilirsiniz.

## <a name="calculate-and-review-listener-probe-locations"></a>Dinleyici sonda konumlarını hesaplama ve inceleme
Malzemeleri atadıktan sonra **Problar** sekmesine **Calculate** geçin.

### <a name="what-the-calculate-button-does"></a>"Hesapla" düğmesi ne işe yarar
**Hesapla** düğmesi, sahnenizi simülasyona hazırlamak için seçtiğiniz akustik sahne geometrisini kullanır:

- Bu sahne meshes geometri alır ve bir *voxel hacmi*hesaplar. Voxel hacmi küçük kübik "voxels oluşan tüm sahnenin bir hacmidir." Voxel boyutu Simülasyon **Çözünürlük** ayarı tarafından kontrol edilen simülasyon frekansı tarafından belirlenir. Her voxel "açık hava" veya sahne geometrisi içeren olarak işaretlenir. Bir voxel geometri içeriyorsa, voxel o geometriye atanan malzemenin soğurma katsayısı ile etiketlenir.
- Dinleyici sonda noktalarını yerleştirmek için navigasyon meshes kullanır. Algoritma, mekansal kapsama alanı ve simülasyon süresi ve dosya boyutu ile ilgili rakip endişeleri dengeler. Bu dar koridorlar ve küçük alanlarda her zaman bazı kapsama sahip olmasını sağlamayı amaçlamaktadır. Tipik sonda noktası sayıları küçük sahneler için 100 ile birkaç bin arasında değişir.

Sahnenizin boyutuna ve makinenizin hızına bağlı olarak, bu hesaplamalar birkaç dakika sürebilir.

### <a name="review-voxel-and-probe-placement"></a>Voxel ve prob yerleşimini gözden geçirin
Sahnenizi pişirmeye hazır olduğunuzdan emin olmak için hem voxel verilerini hem de sonda noktası konumlarını önizleyin. Eksik bir gezinme kafesi veya eksik veya ekstra akustik geometri genellikle önizlemede kolayca görülebilir. **Gizmos** menüsünü kullanarak voxel ve prob yerleşimini etkinleştirin veya devre dışı edin.

![Birlik içinde Gizmos menüsü](media/gizmos-menu.png)

Akustik geometri içeren voxel'ler yeşil küpler olarak gösterilir. Sahnenizi keşfedin ve geometri olması gereken her şeyin voxels olduğunu doğrulayın. Sahne kamerası voxels göstermek için nesnenin yaklaşık 5 metre içinde olmalıdır.

Oluşturulan voxel'leri kaba çözünürlükle ince çözünürlükle karşılaştırırsanız, kaba voxel'lerin iki kat daha büyük olduğunu görürsünüz.

![Unity editöründe kaba voxels önizleme](media/voxel-cubes-preview.png)

Simülasyon sonuçları, çalışma zamanında dinleyici sonda noktası konumları arasında enterpolasyona tabidir. Oyuncunun olay yerinde gitmesi beklenen yerlerin yakınında sonda noktaları olup olmadığını kontrol edin.

![Sondalar Unity editöründe önizleme](media/probes-preview.png)

### <a name="take-care-with-scene-renames"></a>Sahne yeniden adlarıyla dikkat edin
Sahne adı, sahneyi sonda noktası yerleşimini ve voxelization'ı depolayan dosyalara bağlamak için kullanılır. Sonda noktaları hesaplandıktan sonra sahneyi yeniden adlandıracak olursa, malzeme atama ve yerleşim verileri kaybolur ve yeniden çalıştırılmalıdır.

### <a name="for-reference-parts-of-the-probes-tab"></a>Başvuru için: Problar sekmesinin bölümleri
![Unity Akustik Problar sekmesi](media/probes-tab-detail.png)

1. **Sondalar** düğmesi bu sekmeyi getirir.
2. Bu sekmede yapabileceklerin kısa bir açıklaması.
3. Kaba veya ince simülasyon çözünürlüğü ayarlamak için bu seçenekleri kullanın. Kaba daha hızlı, ama dengeler var. Ayrıntılar için [Bake çözünürlüğüne](bake-resolution.md)bakın.
4. Akustik veri dosyalarının nereye yerleştirilen yeri belirtir. Klasör seçiciye erişmek için "**...**" düğmesini seçin. Varsayılan konum *Varlıklar/Akustik Veridir.* Bu konumda bir *Düzenleyici* alt klasörü de oluşturulur. Daha fazla bilgi için, bu makalenin ilerleyen saatlerinde fırın [işlemi tarafından eklenen Veri dosyalarına](#Data-Files)bakın.
5. Burada belirtilen önek, bu sahnenin veri dosyalarını adlandırmak için kullanılır. Varsayılan değer "Acoustics_*[Sahne Adı]*"dır.
6. Sondalar hesaplandıktan sonra, az önce tarif ettiğimiz kontroller devre dışı bırakılır. Hesaplamaları silmek ve yeni ayarlarla yeniden hesaplayabilmeniz için denetimleri etkinleştirmek için **Temizle** düğmesini seçin.
7. Sahneyi **Calculate** voxelize etmek ve sonda noktası konumlarını hesaplamak için Hesapla'yı seçin. Hesaplama makinenizde yerel olarak yapılır. Bu bir fırında yapmadan önce yapılmalıdır.

Project Acoustics'in bu sürümünde, problar el ile yerleştirilemez. **Problar** sekmesindeki otomatik işlemi kullanın.

Kaba karşı ince çözünürlük hakkında daha fazla bilgi [için, Bkz. Bake çözünürlüğü.](bake-resolution.md)

## <a name="bake-your-scene-by-using-azure-batch"></a>Azure Toplu İşlemini kullanarak sahnenizi pişirin
Azure Toplu İş hizmetini kullanarak sahnenizi buluttaki bir işlem kümesinde pişirebilirsiniz. Project Acoustics Unity eklentisi, her fırında bir Azure Toplu Iş kümesini anlık olarak oluşturmak, yönetmek ve yıkmak için doğrudan Azure Toplu İş'e bağlanır. Fırında **sekmesinde,** Azure kimlik bilgilerinizi girin, küme makinesi türünü ve boyutunu seçin ve ardından **Bake'yi**seçin.

### <a name="for-reference-parts-of-the-bake-tab"></a>Başvuru için: Bake sekmesinin bölümleri
![Akustik Fırında Sekmesi Birlik](media/bake-tab-details.png)

1. **Bake** düğmesi bu sekmeyi görüntüler.
2. Bu sayfada yapabileceklerinin kısa bir açıklaması.
3. Azure hesabınız oluşturulduktan sonra bu alanlara Azure kimlik bilgilerinizi girin. Daha fazla bilgi için [bkz.](create-azure-account.md)
4. Akustik araç seti için Docker görüntü etiket alanı.
5. Aboneliklerinizi yönetmek, kullanımı izlemek ve fatura bilgilerini görüntülemek için Azure portalını açar.
6. Hesaplama için kullanılacak Azure Toplu Iş bölümü işlem düğümü türünü belirtir. Düğüm türü Azure veri merkezi konumunuz tarafından desteklenmelidir. Emin değilseniz, **Standard_F8s_v2**olarak bırakın.
7. Hesaplama için kullanılacak düğüm sayısı. Bu sayı pişirme süresini etkiler. Azure Toplu Toplu toplu tahsisinizle sınırlıdır. Varsayılan ayırma yalnızca iki 8 çekirdekli düğüme veya bir 16 çekirdekli düğüme izin verir, ancak genişletilebilir. Temel ayırma kısıtlamaları hakkında daha fazla bilgi için [bkz.](create-azure-account.md)
8. Düşük [öncelikli düğümleri](https://docs.microsoft.com/azure/batch/batch-low-pri-vms)kullanacak şekilde bilgi işlem havuzunuzu yapılandırmak için bu onay kutusunu seçin. Düşük öncelikli işlem düğümlerinin maliyeti çok daha düşüktür. Ama her zaman mevcut olmayabilir veya herhangi bir zamanda preempted olabilir.
9. **Sondalar** sekmesinde hesaplanan sahneniz için sonda sayısı. Sonda sayısı bulutta çalıştırılması gereken simülasyonların sayısını belirler. Sondalardan daha fazla düğüm belirtemezsiniz.
10. Düğüm başlatma süresi hariç olmak üzere, işinizin bulutta çalışması için gereken sürenin tahmini. İş çalışmaya başladıktan sonra, bu alan sonuçları geri almanız için ne kadar süre yle ilgili bir tahmin gösterir.
11. Simülasyonları çalıştırmak için gereken toplam işlem süresi miktarı. Bu değer, Azure'da kullanılacak düğüm işlem süresinin toplam tutarıdır. Daha fazla bilgi için, bu makalenin ilerleyen saatlerinde [Azure'da pişirme maliyetini tahmin](#Estimating-bake-cost) edin' e bakın.
12. Bu ileti, iş bittiğinde fırın sonuçlarının nerede kaydolacağını bildirir.
13. *(Gelişmiş kullanım sadece:)* Bu düğme, Gönderdiğiniz bir fırında unutmak için Birlik zorlar. Örneğin, sonuçları başka bir makine kullanarak indirdiyseniz, bu işi unutmak için **Durumu Temizle** düğmesini seçin. Sonuç dosyası, hazır olduğunda *not* karşıdan yüklenmez. *Bu, işi iptal etmekle aynı şey değildir. İş, çalışıyorsa, bulutta çalışmaya devam edecektir.*
14. Fırını buluta göndermek için bu düğmeyi seçin. Bir iş çalışırken, bu düğme **İşi İptal Et**olur.
15. Bilgisayarınızda akustik simülasyon işleme için hazırlamak için bu [düğmeyi](#Local-bake)seçin.
16. Bu alan fırın Durumunu gösterir. Fırın tamamlandığında "İndirilen" i miş gibi gelir.

[Azure portalında](https://portal.azure.com)etkin işler, bilgi işlem havuzları ve depolama hakkında her zaman tam bilgi alabilirsiniz.

Bir iş çalışırken, **İşi** **İptal**Et düğmesi etiketi değişir. Devam eden işi iptal etmek için bu düğmeyi kullanın. Onaylamanız istenir. Bir işi iptal etmek geri alınamaz. İptal ettiğinizde, hiçbir sonuç kullanılamaz, ancak kullanılan herhangi bir Azure işlem süresi için yine de ücretlendirilirsiniz.

Bir fırına başladıktan sonra Unity'yi kapatabilirsiniz. Projeye, düğüm türüne ve düğüm sayısına bağlı olarak, bulut fırını birkaç saat sürebilir. Projeyi yeniden yüklediğinizde ve Akustik penceresini açtığınızda, fırında iş durumu güncelleştirilir. İş tamamlanırsa, çıktı dosyası karşıdan yüklenir.

Güvenlik için Azure kimlik bilgileri yerel makinenizde depolanır ve Unity düzenleyicinizle ilişkilendirilir. Bunlar yalnızca Azure'a güvenli bir bağlantı kurmak için kullanılır.

## <a name="find-the-status-of-a-running-job-on-the-azure-portal"></a>Azure portalında çalışan bir işin durumunu bulma

1. **Bake** sekmesinde iş kimliğini bulun.

    ![Bake sekmesinde vurgulanan Birlik fırın iş kimliği](media/unity-job-id.png)  

2. Azure [portalını](https://portal.azure.com)açın, fırında kullanılan Toplu Iş hesabına gidin ve **İşler'i**seçin.

    ![Azure portalındaki İşler bağlantısı](media/azure-batch-jobs.png)  

3. İş listesinde iş kimliğini arayın.

   ![Azure portalında vurgulanan fırında iş durumu](media/azure-bake-job-status.png)  

4. İlgili görevlerin durumunu ve genel iş durumunu görmek için iş kimliğini seçin.

   ![Fırında görev durumu](media/azure-batch-task-state.png)  


### <a name="estimate-azure-bake-cost"></a><a name="Estimating-bake-cost"></a>Azure fırın maliyetini tahmin edin

Bir fırının ne kadara mal olacağını tahmin etmek için, bir süre olan **Tahmini İşlem Maliyeti** değeriyle başlayın. Seçtiğiniz **VM Düğümü Türü** için bu değeri yerel para biriminizdeki saatlik maliyetle çarpın. Sonucun düğümleri çalışır hale getirmek için gereken düğüm süresini içermeyeceğini unutmayın.

Örneğin, $0.40/sa maliyeti olan düğüm türünüz için **Standard_F8s_v2** seçtiğinizi varsayın. Tahmini **İşlem Maliyeti** 3 saat 57 dakika ise, işi çalıştırmak için tahmini maliyet $0,40 * ~4 saat = ~$1,60 olacaktır. Düğümleri başlatmanın ek süresi nedeniyle gerçek maliyet büyük olasılıkla biraz daha yüksek olacaktır.

[Azure Toplu İş fiyatlandırması](https://azure.microsoft.com/pricing/details/virtual-machines/linux)sırasında saatlik düğüm maliyetini bulun. (Kategori olarak **en iyi duruma getirilmiş işlem** veya Yüksek **performanslı işlem** seçeneğini belirleyin.)

## <a name="bake-your-scene-on-your-pc"></a><a name="Local-bake"></a>Sahnenizi bilgisayarınızda pişirin
Ayrıca kendi pc üzerinde sahne pişirebilirsiniz. Bu yöntem, bir Azure Toplu İş hesabı oluşturmadan önce küçük sahneler için akustik denemeleri için yararlı olabilir. Ancak yerel akustik simülasyonsahnenin boyutuna bağlı olarak uzun bir zaman alabilir unutmayın.

### <a name="minimum-hardware-requirements"></a>En düşük donanım gereksinimleri
* En az 8 çekirdekli ve 32 GB RAM ile x86-64 işlemci
* Docker'ı çalıştırmak için [etkinleştirilen Hyper-V](https://docs.microsoft.com/virtualization/hyper-v-on-windows/quick-start/enable-hyper-v)

Örnek olarak, 8 çekirdekli bir makine üzerinde test, Intel Xeon E5-1660 @ 3 GHz ve 32 GB RAM:
* 100 probları ile küçük bir sahne kaba bir fırında veya 32 saat ince bir fırında için yaklaşık 2 saat sürdü.
* 1.000 sondaiçeren orta boy bir sahne kaba bir fırında yaklaşık 20 saat, ince bir fırında 21 gün sürdü.

### <a name="set-up-docker"></a>Docker'ı ayarlama
Simülasyonu işleyecek pc'ye Docker'ı yükleyin ve yapılandırın:
1. [Docker Masaüstü'nü](https://www.docker.com/products/docker-desktop)yükleyin.
2. Docker ayarlarını açın, **Gelişmiş'e**gidin ve kaynakları en az 8 GB RAM için yapılandırın. Docker'a ne kadar çok CPU ayırırsanız, fırında o kadar hızlı tamamlanır.  
![Örnek Docker ayarları](media/docker-settings.png)
1. Paylaşılan **Sürücüler'e**gidin ve işleme için kullanılan sürücü için paylaşımı açın.  
![Docker paylaşılan sürücü seçenekleri](media/docker-shared-drives.png)

### <a name="run-the-local-bake"></a>Yerel fırında çalıştırın
1. Fırında **Yerel Fırında** **Hazırla** düğmesini seçin. Ardından, giriş dosyalarını ve yürütme komut dosyalarını kaydetmek için bir klasör konumu seçin. Daha sonra, minimum donanım gereksinimlerini karşıladığı sürece herhangi bir makinede pişirebilirsiniz ve klasörü o makineye kopyalayarak Docker'ı yükleyebilirsiniz.
2. Simülasyonu başlatmak için *Windows'da runlocalbake.bat* komut dosyasını veya MacOS'taki *runlocalbake.sh* komut dosyasını çalıştırın. Bu komut dosyası, Simülasyon işleme için gerekli araç seti ile Project Acoustics Docker görüntüsünü getirir ve simülasyonu başlatır.
3. Simülasyon tamamlandıktan sonra, ortaya çıkan *.ace* dosyasını Unity projenize kopyalayın. Unity'nin dosyayı ikili bir dosya olarak tanıdığından emin olmak için dosya uzantısına ".bayt" ekler (örneğin, "Scene1.ace.bytes"). Simülasyon için ayrıntılı günlükler *AcousticsLog.txt'de saklanır.* Herhangi bir sorunla karşılaştıysanız, sorunu tanılamaya yardımcı olması için bu dosyayı inceleyin.

## <a name="data-files-added-by-the-bake-process"></a><a name="Data-Files"></a>Fırın işlemi tarafından eklenen veri dosyaları

Aşağıdaki dört veri dosyaları pişirme işlemi sırasında oluşturulur. Bir simülasyon sonuçları ve başlık ile gemi içerir. Diğerleri Unity düzenleyiciyle ilgili verileri depolar.

Simülasyon sonucu:
* *Varlıklar/AcousticsData/Acoustics\_[SceneName].ace.bytes*: Bu dosya çalışma zamanı arama tablosudur. Simülasyon sonuçlarını ve voxelized akustik sahne öğelerini içerir. Bu dosyanın adını ve konumunu **Sondalar** sekmesinde değiştirebilirsiniz.

   *Simülasyon sonuç dosyasını silmemeye dikkat edin. Olay yerini yeniden yapmak dışında kurtarılamaz.*

Düzenleyici veri dosyaları:
* *Varlıklar/Düzenleyici/[SceneName]\_AcousticsParameters.asset*: Bu dosya, Akustik Kullanıcı Arabirimi'ndeki alanlara girdiğiniz verileri depolar. Bu dosyanın adını ve konumunu değiştiremezsiniz.
* *Varlıklar/AcousticsData/Editor/Acoustics_[SceneName].vox*: Bu dosya, **Problar** sekmesindeki **Hesapla** düğmesini seçtiğinizde voxelized akustik geometrisini ve hesaplanan malzeme özelliklerini depolar. Bu dosyanın adını ve konumunu **Sondalar** sekmesinde değiştirebilirsiniz.
* *Varlıklar/AcousticsData/Editor/Acoustics\_[SceneName]\_config.xml*: Bu **dosya, Hesapla'yı**seçtiğinizde hesaplanan simülasyon parametrelerini depolar. Bu dosyanın adını ve konumunu **Sondalar** sekmesinde değiştirebilirsiniz.

## <a name="set-up-the-acoustics-lookup-table"></a>Akustik arama tablosunu ayarlama
**Proje** Akustik prefabrikini proje panelinden sahnenize sürükleyin:

![Birlik Içinde Akustik prefabrik](media/acoustics-prefab.png)

**ProjectAcoustics** oyun nesnesini seçin ve denetçi paneline gidin. Fırın sonucunuzun konumunu belirtin (.ace dosyası, *Varlıklar/Akustik*Veri'de): Akustik Yöneticisi komut dosyasına sürükleyin veya metin kutusunun yanındaki daire düğmesini seçin.

![Unity'de Akustik Yöneticisi prefabrik](media/acoustics-manager.png)

## <a name="next-steps"></a>Sonraki adımlar
* Unity [tasarım denetimlerini](unity-workflow.md)keşfedin.
* [Project Acoustics tasarım konseptlerini](design-process.md)keşfedin.
