---
title: Veri etiketleme projesi oluşturma
titleSuffix: Azure Machine Learning
description: Makine öğrenimi için verileri etiketlemek için etiketleme projeleri oluşturmayı ve çalıştırmayı öğrenin.  Araçlar ml destekli etiketleme, ya da görev ile yardımcı olmak için döngü etiketleme insan içerir.
author: sdgilley
ms.author: sgilley
ms.service: machine-learning
ms.topic: tutorial
ms.date: 03/01/2020
ms.openlocfilehash: d39cf8745c6f53cb11bb12561fd452325fe52ac6
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79296957"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Veri etiketleme projesi oluşturma ve etiketleri dışa aktarma 

[!INCLUDE [aml-applies-to-basic-enterprise-sku](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Makine öğrenimi projelerinde hacimli verilerin etiketlanması genellikle baş ağrısıdır. Görüntü sınıflandırması veya nesne algılama gibi bilgisayar görme bileşenine sahip projeler genellikle binlerce görüntü için etiket gerektirir.
 
[Azure Machine Learning,](https://ml.azure.com/) etiketleme projeleri oluşturmak, yönetmek ve izlemek için merkezi bir yer sağlar. Etiketleme görevlerini verimli bir şekilde yönetmek için verileri, etiketleri ve ekip üyelerini koordine etmek için kullanın. Machine Learning, çok etiketli veya çok katmanlı görüntü sınıflandırması ve sınırlı kutularla nesne tanımlamayı destekler.

Machine Learning ilerlemeyi izler ve tamamlanmamış etiketleme görevlerinin sırasını korur. Etiketçilerin katılmak için azure hesabına ihtiyacı yoktur. Microsoft hesabınızveya [Azure Etkin Dizininizle](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)kimlik doğrulaması yaptıktan sonra, zamanlarının izin verdiği kadar etiketleme yapabilir.

Projeyi başlatıp durdurursunuz, etiketleyiciler ve takımlar ekleyin ve kaldırırsınız ve etiketleme ilerlemesini izlersiniz. Etiketli verileri COCO biçiminde veya Azure Machine Learning veri kümesi olarak dışa aktarabilirsiniz.

> [!Important]
> Şu anda yalnızca görüntü sınıflandırma ve nesne tanımlama etiketleme projeleri desteklenir. Ayrıca, veri görüntülerinin bir Azure blob veri deposunda bulunması gerekir. (Varolan bir veri deponuz yoksa, proje oluşturma sırasında resim yükleyebilirsiniz.) 

Bu makalede, nasıl öğreneceksiniz:

> [!div class="checklist"]
> * Proje oluşturma
> * Projenin verilerini ve yapısını belirtin
> * Proje üzerinde çalışan ekipleri ve kişileri yönetme
> * Projeyi çalıştırın ve izleyin
> * Etiketleri dışa aktarma


## <a name="prerequisites"></a>Ön koşullar

* Yerel dosyalarda veya Azure blob depolamasında etiketlemek istediğiniz veriler.
* Uygulamak istediğiniz etiket kümesi.
* Etiketleme talimatları.
* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://aka.ms/AMLFree) bir hesap oluşturun.
* Bir Makine Öğrenme çalışma alanı. Bkz. [Azure Makine Öğrenimi çalışma alanı oluşturun.](how-to-manage-workspace.md)

## <a name="create-a-labeling-project"></a>Etiketleme projesi oluşturma

Etiketleme projeleri Azure Machine Learning'den yönetilir. Projelerinizi ve kişilerinizi yönetmek için **Etiketleme projeleri** sayfasını kullanırsınız. Bir projede bir veya daha fazla takım atanmış ve bir takımın bu projeye atanmış bir veya daha fazla kişisi vardır.

Verileriniz azure blob depolama alanında ysa, etiketleme projesini oluşturmadan önce verilerinizi veri deposu olarak kullanılabilir hale getirmelisiniz. Ayrıntılar için [veri depoları oluştur ve kaydedin.](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores)

Proje oluşturmak için **proje ekle'yi**seçin. Projeye uygun bir ad verin ve **Etiketleme görev türünü**seçin.

![Proje oluşturma sihirbazını etiketleme](./media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Bir sınıf kümesinden görüntüye yalnızca tek bir *sınıf* uygulamak istediğinizde projeler için Görüntü Sınıflandırma **Çok sınıflı'yı** seçin.
* Bir sınıf kümesinden bir resme *bir veya daha fazla* etiket uygulamak istediğinizde projeler için Resim Sınıflandırma Çok **etiketli'yi** seçin. Örneğin, bir köpek bir fotoğraf hem *köpek* hem de *gündüz*ile etiketlenmiş olabilir.
* Görüntüdeki her nesneye bir sınıf ve sınırlayıcı bir kutu atamak istediğiniz projeler için **Nesne Tanımlama (Sınırlayıcı Kutu)** seçeneğini belirleyin.

Devam etmeye hazır olduğunuzda **İleri'yi** seçin.

## <a name="specify-the-data-to-label"></a>Etiketlemek için verileri belirtin

Verilerinizi içeren bir veri kümesi oluşturduysanız, **varolan bir veri kümesi** açılır listesini seç'ten seçin. Veya varolan bir Azure veri deposukullanmak veya yerel dosyaları yüklemek için **veri kümesi oluştur'u** seçin.


### <a name="create-a-dataset-from-an-azure-datastore"></a>Azure veri deposundan veri kümesi oluşturma

Çoğu durumda, yalnızca yerel dosyaları yüklemek için iyi. Ancak [Azure Depolama Gezgini,](https://azure.microsoft.com/features/storage-explorer/) büyük miktarda veri aktarmak için daha hızlı ve daha sağlam bir yol sağlar. Dosyaları taşımanın varsayılan yolu olarak Depolama Gezgini'ni öneririz.

Azure Blob depolama alanında depolamış olduğunuz verilerden veri kümesi oluşturmak için:

1. **Datastore'dan** **veri kümesi** > oluştur'u seçin.
1. Veri kümenize bir **Ad** atayın.
1. **Dataset türü**olarak **Dosya'yı** seçin.  
1. Veri mağazasını seçin.
1. Verileriniz blob depolama alanınızdaki bir alt klasördeyse, yolu seçmek için **Gözat'ı** seçin' i seçin.
    * Tüm dosyaları seçili yolun alt klasörlerine dahil etmek için yola "/**" ekleyin.
    * Geçerli kapsayıcıdaki*/* ve alt klasörlerine tüm verileri eklemek için "* .*" ekleyin.
1. Veri kümeniz için bir açıklama sağlayın.
1. **Sonraki'ni**seçin.
1. Detayları onayla. Ayarları değiştirmek için **Geri'yi** seçin veya veri kümesini oluşturmak için **Oluştur'u** seçin.

> [!NOTE]
> Seçtiğiniz veriler projenize yüklenir.  Proje oluşturulduktan sonra veri deposuna daha fazla veri eklemek bu projede görünmez.  

### <a name="create-a-dataset-from-uploaded-data"></a>Yüklenen verilerden veri kümesi oluşturma

Verilerinizi doğrudan yüklemek için:

1.  > **Yerel dosyalardan**veri **kümesi oluştur'u**seçin.
1. Veri kümenize bir **Ad** atayın.
1. **Dataset türü**olarak "Dosya"yı seçin.
1. *İsteğe bağlı:* Veri deposunu, kapsayıcıyı ve verilerinize giden yolu özelleştirmek için **Gelişmiş ayarları** seçin.
1. Yüklenenebilmek için yerel dosyaları seçmek için **Gözat'ı** seçin.
1. Veri setinizin açıklamasını sağlayın.
1. **Sonraki'ni**seçin.
1. Detayları onayla. Ayarları değiştirmek için **Geri'yi** seçin veya veri kümesini oluşturmak için **Oluştur'u** seçin.

Veriler, Machine Learning çalışma alanınızın varsayılan blob deposuna ("workspaceblobstore") yüklenir.

## <a name="specify-label-classes"></a>Etiket sınıflarını belirtin

Etiket **sınıfları** sayfasında, verilerinizi kategorilere ayırmak için sınıflar kümesini belirtin. Bunu dikkatli yapın, çünkü etiketleyicilerinizin doğruluğu ve hızı sınıflar arasında seçim yapma yeteneğinden etkilenecektir. Örneğin, bitkiler veya hayvanlar için tam cins ve türleri hecelemek yerine, bir alan kodu kullanın veya cinsi kısaltın.

Satır başına bir etiket girin. Yeni **+** bir satır eklemek için düğmeyi kullanın. 3 veya 4'ten fazla etiketiniz varsa ancak 10'dan azsa, etiketlerin işlerini hızlandırmak için sayı tuşlarını kullanabilmeleri için numaraları ("1: ", "2: ") olan adları öne kullanabilirsiniz.

## <a name="describe-the-labeling-task"></a>Etiketleme görevini açıkla

Etiketleme görevini açıkça açıklamak önemlidir. Etiketleme **yönergeleri** sayfasında, talimatları etiketlemek için harici bir siteye bağlantı ekleyebilir veya sayfadaki edit kutusuna yönergeler sağlayabilirsiniz. Yönergeleri görev odaklı ve hedef kitleye uygun tutun. Şu soruları göz önünde bulundurun:

* Görecekleri etiketler nelerdir ve aralarından nasıl birini seçecekler? Başvuru metni var mı?
* Hiçbir etiket uygun görünmüyorsa ne yapmalılar?
* Birden çok etiket uygun görünüyorsa ne yapmalılar?
* Bir etikete hangi güven eşiğini uygulamalıdır? Eğer emin değilseniz onların "en iyi tahmin" istiyor musunuz?
* Kısmen tıkanmış veya üst üste binen nesnelerle ne yapmalılar?
* İlgi çekici bir nesne görüntünün kenarına kırpılırsa ne yapmalılar?
* Bir hata yaptıklarını düşünürlerse etiket gönderdikten sonra ne yapmalılar?

Sınırlayıcı kutular için önemli sorular şunlardır:

* Bu görev için sınırlayıcı kutu nasıl tanımlanır? Tamamen nesnenin iç üzerinde mi olmalı, yoksa dış yüzeyde mi olmalı? Mümkün olduğunca yakın kırpılmış olmalı, ya da bazı temizleme kabul edilebilir mi?
* Etiketleyicilerin sınırlayıcı kutuları tanımlamada ne kadar bakım ve tutarlılık uygulamalarını bekliyorsunuz?
* Görüntüde kısmen gösterilen nesne nasıl etiketlenir? 
* Kısmen diğer nesne tarafından kapsanan nesne nasıl etiketilir?

>[!NOTE]
> Etiketçilerin 1-9 tuşlarını kullanarak ilk 9 etiketi seçebileceğinden emin olun.

## <a name="use-ml-assisted-labeling"></a>ML destekli etiketleme yi kullanma

[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-enterprise-sku.md)]

**ML destekli etiketleme** sayfası, etiketleme görevini hızlandırmak için otomatik makine öğrenme modellerini tetiklemenizi sağlar. Etiketleme projenizin başında, görüntüler olası önyargıyı azaltmak için rasgele bir sıraya karıştırılır. Ancak, veri kümesinde bulunan tüm önyargılar eğitilmiş modele yansıtılır. Örneğin, resimlerinizin %80'i tek bir sınıfa aitse, modeli eğitmek için kullanılan verilerin yaklaşık %80'i bu sınıfta olacaktır. Bu eğitim aktif öğrenmeyi içermez.

Bu özellik görüntü sınıflandırma (çok sınıflı veya çok etiketli) görevler için kullanılabilir.  

*ML destekli etiketlemeyi etkinleştir'i* seçin ve iki aşamadan oluşan destekli etiketlemeyi etkinleştirmek için bir GPU belirtin:
* Kümeleme
* Ön etiketleme

Destekli etiketlemeyi başlatmak için gereken etiketlenmiş görüntülerin tam sayısı sabit bir numara değildir.  Bu, bir etiketleme projesinden diğerine önemli ölçüde değişebilir. Bazı projelerde, 300 resim el ile etiketlendikten sonra bazen ön etiket veya küme görevlerini görmek mümkündür. ML Destekli Etiketleme, eğitim sürecini başlatmak için önceden eğitilmiş bir model kullanan *Transfer Learning*adlı bir teknik kullanır. Veri setinizin sınıfları önceden eğitilmiş modeldekilere benziyorsa, ön etiketler yalnızca birkaç yüz el ile etiketlenmiş resimden sonra kullanılabilir. Veri kümeniz modeli önceden eğitmek için kullanılan verilerden önemli ölçüde farklıysa, çok daha uzun sürebilir.

Son etiketler hala etiketleyicinin girişine dayandığından, bu teknoloji bazen *döngü etiketinde insan* olarak adlandırılır.

### <a name="clustering"></a>Kümeleme

Belirli sayıda etiket gönderildikten sonra, makine öğrenme modeli benzer görüntüleri bir araya gruplandırmaya başlar.  Bu benzer görüntüler, manuel etiketlemeyi hızlandırmak için aynı ekranda etiketleyicilere sunulur. Kümeleme, özellikle etiketleyici 4, 6 veya 9 görüntüden oluşan bir ızgara görüntülerken kullanışlıdır. 

Bir makine öğrenme modeli el ile etiketlenmiş verilerinizde eğitildikten sonra, model son tam bağlı katmanına kesilir. Etiketlenmemiş görüntüler daha sonra kesilen modelden genellikle "katıştırma" veya "featurization" olarak bilinen bir işlemle geçirilir. Bu, her görüntüyü bu model katmanı tarafından tanımlanan yüksek boyutlu bir alana yerleştirir. Boşluktaki en yakın komşular olan görüntüler kümeleme görevleri için kullanılır. 

### <a name="prelabeling"></a>Ön etiketleme

Daha fazla görüntü etiketi gönderildikten sonra, resim etiketlerini tahmin etmek için bir sınıflandırma modeli kullanılır.  Etiketleyici artık her resimde zaten bulunan tahmin edilmiş etiketleriçeren sayfaları görür.  Görev daha sonra bu etiketleri gözden geçirmek ve sayfayı göndermeden önce yanlış etiketli görüntüleri düzeltmektir.  

Bir makine öğrenme modeli el ile etiketlenmiş verilerinizde eğitildikten sonra, model çeşitli güven eşiklerinde doğruluğunu belirlemek için el ile etiketlenmiş görüntülerin bir test kümesi nde değerlendirilir. Bu değerlendirme işlemi, modelin ön etiketleri göstermek için yeterince doğru olduğu bir güven eşiğini belirlemek için kullanılır. Model daha sonra etiketlenmemiş verilere göre değerlendirilir. Ön etiketleme için bu eşiğe göre öngörülere daha güvenli görüntüler kullanılır.

> [!NOTE]
> ML destekli etiketleme **yalnızca** Enterprise sürümü çalışma alanlarında kullanılabilir.

## <a name="initialize-the-labeling-project"></a>Etiketleme projesini başlatma

Etiketleme projesi başharfe büründürülmeden sonra, projenin bazı yönleri değişmezdir. Görev türünü veya veri kümesini değiştiremezsiniz. Görev açıklaması için etiketleri ve URL'yi *değiştirebilirsiniz.* Projeyi oluşturmadan önce ayarları dikkatle gözden geçirin. Projeyi gönderdikten sonra, projeyi Initializing olarak göstereceği **Veri Etiketleme** ana sayfasına geri **döndürülürsunuz.** Bu sayfa otomatik olarak yenilenmez. Bu nedenle, duraklattıktan sonra, **projenin oluşturulduğu**durumu görmek için sayfayı el ile yenileyin.

## <a name="manage-teams-and-people"></a>Ekipleri ve kişileri yönetme

Varsayılan olarak, oluşturduğunuz her etiketleme projesi, üye olarak sizinle birlikte yeni bir takım alır. Ancak takımlar projeler arasında da paylaşılabilir. Ve projeler birden fazla takım olabilir. Takım oluşturmak için **Takımlar** sayfasında **takım ekle'yi** seçin.

**İnsanlar** sayfasındaki insanları yönetirsin. E-posta adresine göre kişi ekleme ve kaldırma. Her etiketleyicinin microsoft hesabınız veya Azure Etkin Dizini aracılığıyla kimlik doğrulaması yapmak zorundadır.  

Bir kişiyi ekledikten sonra, bu kişiyi bir veya daha fazla takıma atayabilirsiniz: **Takımlar** sayfasına gidin, takımı seçin ve ardından **kişileri atayın** veya **kişileri kaldırın'ı**seçin.

Ekibe e-posta göndermek için Takım **ayrıntıları** sayfasını görüntülemek için ekibi seçin. Bu sayfada, takımdaki herkesin adreslerini içeren bir e-posta taslağı açmak için **E-posta ekibini** seçin.

## <a name="run-and-monitor-the-project"></a>Projeyi çalıştırın ve izleyin

Projeyi başlattıktan sonra Azure projeyi çalıştırmaya başlar. **Proje ayrıntılarına**gitmek için ana **Veri Etiketleme** sayfasındaprojeyi seçin. **Pano** sekmesi etiketleme görevinin ilerlemesini gösterir.

**Veriler** sekmesinde, veri kümenizi görebilir ve etiketli verileri inceleyebilirsiniz. Yanlış etiketlenmiş veriler görüyorsanız, verileri seçin ve etiketleri kaldıracak ve verileri etiketlenmemiş sıraya geri koyacak olan **Reddet'i**seçin.

Projeye takım atamak veya atamak için **Takım** sekmesini kullanın.

Projeyi duraklatmak veya yeniden **Pause**/başlatmak için**Başlat düğmesini** seçin. Verileri yalnızca proje çalışırken etiketleyebilirsiniz.

**Etiket verilerini**seçerek verileri doğrudan **Proje ayrıntıları** sayfasından etiketleyebilirsiniz.

## <a name="add-new-label-class-to-a-project"></a>Projeye yeni etiket sınıfı ekleme

Etiketleme işlemi sırasında, resimlerinizi sınıflandırmak için ek etiketlere ihtiyaç duyulduğunu görebilirsiniz.  Örneğin, kafa karıştırıcı görüntüleri belirtmek için "Bilinmeyen" veya "Diğer" etiketi eklemek isteyebilirsiniz.

Projeye bir veya daha fazla etiket eklemek için bu adımları kullanın:

1. Ana **Veri Etiketleme** sayfasındaprojeyi seçin.
1. Sayfanın üst kısmında, etiketçilerin etkinliklerini durdurmak için **Duraklat'ı** seçin.
1. **Ayrıntılar** sekmesini seçin.
1. Soldaki listede **Etiket sınıflarını**seçin.
1. Listenin en üstünde , + **Etiket** ![Ekle'yi seçin Etiket ekle](media/how-to-create-labeling-projects/add-label.png)
1. Formda, yeni etiketinizi ekleyin ve nasıl devam edeceğinizi seçin.  Bir görüntü için kullanılabilir etiketleri değiştirdiğiniz için, zaten etiketlenmiş verileri nasıl işeceğinizi seçersiniz:
    * Varolan tüm etiketleri kaldırarak baştan başlayın.  Yeni tam etiket kümesiyle etiketlemeye en baştan başlamak istiyorsanız bu seçeneği belirleyin. 
    * Varolan tüm etiketleri tutarak baştan başlayın.  Tüm verileri etiketlenmemiş olarak işaretlemek için bu seçeneği belirleyin, ancak varolan etiketleri daha önce etiketlenmiş görüntüler için varsayılan etiket olarak tutun.
    * Varolan tüm etiketleri tutarak devam edin. Zaten olduğu gibi etiketlenmiş tüm verileri tutmak için bu seçeneği belirleyin ve henüz etiketlenmeyen veriler için yeni etiketi kullanmaya başlayın.
1. Yeni etiket(ler) için talimatlar sayfanızı gerektiği gibi değiştirin.
1. Tüm yeni etiketleri ekledikten sonra, sayfanın üst kısmında projeyi yeniden başlatmak için **Başlat'ı** seçin.  

## <a name="export-the-labels"></a>Etiketleri dışa aktarma

Makine Öğrenimi denemesi için etiket verilerini istediğiniz zaman dışa aktarabilirsiniz. Resim etiketleri [COCO biçiminde](http://cocodataset.org/#format-data) veya Azure Machine Learning veri kümesi olarak dışa aktarılabilir. Etiketleme projenizin **Proje ayrıntıları** sayfasındaki **Dışa** Aktarma düğmesini kullanın.

COCO dosyası, Azure Machine Learning çalışma alanının varsayılan blob deposunda *dışa aktarma/coco*içindeki bir klasörde oluşturulur. Machine Learning'in **Datasets** bölümünden dışa aktarılan Azure Machine Learning veri kümesine erişebilirsiniz. Veri kümesi ayrıntıları sayfası, etiketlerinize Python'dan erişmek için örnek kod da sağlar.

![Dışa aktarılan veri kümesi](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Görüntü sınıflandırması veya nesne algılama](how-to-label-images.md) için etiket görüntüleri
* [Azure Machine Learning ve Machine Learning Studio (klasik)](compare-azure-ml-to-studio-classic.md) hakkında daha fazla bilgi edinin
