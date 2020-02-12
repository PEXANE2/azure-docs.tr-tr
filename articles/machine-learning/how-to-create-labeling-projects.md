---
title: Veri etiketleme projesi oluşturma
titleSuffix: Azure Machine Learning
description: Machine Learning için verileri etiketlemek üzere etiketleme projeleri oluşturmak ve çalıştırmak için Ho hakkında bilgi edinin.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: e469837c8e374e62824bd8f7a7feb110ed1be9c9
ms.sourcegitcommit: 812bc3c318f513cefc5b767de8754a6da888befc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/12/2020
ms.locfileid: "77153120"
---
# <a name="create-a-data-labeling-project-and-export-labels"></a>Veri etiketleme projesi ve dışarı aktarma etiketleri oluşturma 

Machine Learning projelerinde Voluminous verilerinin etiketlenmesi genellikle bir headache 'dir. Görüntü sınıflandırması veya nesne algılama gibi bir bilgisayar vizyonu bileşeni olan projeler genellikle binlerce görüntü için Etiketler gerektirir.
 
[Azure Machine Learning](https://ml.azure.com/) etiketleme projelerini oluşturmak, yönetmek ve izlemek için size merkezi bir yer sunar. Etiketleme görevlerini verimli bir şekilde yönetmek için verileri, etiketleri ve ekip üyelerini koordine etmek üzere kullanın. Machine Learning, çok etiketli veya çok sınıflı ve nesne tanımlamasının sınırlı kutular ile birlikte görüntü sınıflandırmasını destekler.

Machine Learning ilerleme durumunu izler ve tamamlanmamış etiketleme görevlerinin kuyruğunu korur. Etiketleyicilerin katılması için bir Azure hesabı gerekmez. Microsoft hesabı veya [Azure Active Directory](https://docs.microsoft.com/azure/active-directory/active-directory-whatis)kimlik doğrulamasından geçtikten sonra, bu dosyalar zaman içinde izin verdiğinden çok etiketleme yapabilir.

Machine Learning, projeyi başlatıp durdurur, kişi ve takımlar ekleyip kaldırır ve ilerlemeyi izleyebilirsiniz. Etiketli verileri COCO formatında veya Azure Machine Learning veri kümesi olarak dışarı aktarabilirsiniz.

> [!Important]
> Şu anda yalnızca görüntü sınıflandırması ve nesne kimliği etiketleme projeleri desteklenir. Ayrıca, veri görüntülerinin bir Azure blob veri deposunda bulunması gerekir. (Mevcut bir veri deposu yoksa, proje oluşturma sırasında görüntüleri karşıya yükleyebilirsiniz.) 

Bu makalede aşağıdakileri nasıl yapacağınızı öğreneceksiniz:

> [!div class="checklist"]
> * Proje oluşturma
> * Projenin verilerini ve yapısını belirtin
> * Projede çalışan takımları ve kişileri yönetme
> * Projeyi çalıştırma ve izleme
> * Etiketleri dışarı aktarma


## <a name="prerequisites"></a>Önkoşullar

* Yerel dosyalarda veya Azure Storage 'da etiketlemek istediğiniz veriler.
* Uygulamak istediğiniz Etiketler kümesi.
* Etiketleme için yönergeler.
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://aka.ms/AMLFree) oluşturun.
* Machine Learning çalışma alanı. Bkz. [Azure Machine Learning çalışma alanı oluşturma](how-to-manage-workspace.md).

## <a name="create-a-labeling-project"></a>Etiketleme projesi oluşturma

Etiketleme projeleri Azure Machine Learning yönetilir. Projelerinizi ve kişilerinizi yönetmek için, **proje etiketleme** sayfasını kullanın. Bir projede kendisine atanmış bir veya daha fazla takım vardır ve bir takıma bir veya daha fazla kişi atanmış olur.

Verileriniz zaten Azure Blob depolamadaki etiketleme projesini oluşturmadan önce onu bir veri deposu olarak kullanılabilir hale getirin. Ayrıntılar için bkz. [veri depoları oluşturma ve kaydetme](https://docs.microsoft.com/azure/machine-learning/how-to-access-data#create-and-register-datastores).

Proje oluşturmak için **Proje Ekle**' yi seçin. Projeye uygun bir ad verin ve **etiketleme görev türünü**seçin.

![Proje oluşturma Sihirbazı etiketleme](./media/how-to-create-labeling-projects/labeling-creation-wizard.png)

* Bir sınıf kümesinden bir görüntüye bir *veya daha fazla* etiket uygulamak istediğinizde, projeler Için **görüntü sınıflandırması çoklu etiketi** ' ni seçin. Örneğin, bir köpek fotoğrafı hem *köpek* hem de *Daytime*ile etiketlenebilir.
* Bir sınıf kümesinden bir görüntüye yalnızca *tek bir sınıf* uygulamak istediğinizde, projeler Için **görüntü sınıflandırması çoklu sınıfı** ' nı seçin.
* Bir görüntü içindeki her nesneye bir sınıf ve sınırlayıcı kutu atamak istediğinizde projeler için **nesne tanımlama (sınırlayıcı kutu)** seçeneğini belirleyin.

Devam etmeye hazırsanız **İleri ' yi** seçin.

## <a name="specify-the-data-to-label"></a>Etiketlemek için verileri belirtin

Verilerinizi içeren bir veri kümesi zaten oluşturduysanız, **mevcut bir veri kümesi seçin** açılır listesinden bunu seçin. Ya da mevcut bir Azure veri deposunu kullanmak veya yerel dosyaları karşıya yüklemek için **veri kümesi oluştur** ' u seçin.

### <a name="create-a-dataset-from-an-azure-datastore"></a>Azure veri deposundan veri kümesi oluşturma

Çoğu durumda, yalnızca yerel dosyaları karşıya yüklemek çok uygundur. Ancak [Azure Depolama Gezgini](https://azure.microsoft.com/features/storage-explorer/) büyük miktarda veriyi aktarmanın daha hızlı ve daha sağlam bir yolunu sağlar. Dosyaları taşımanın varsayılan yolu olarak Depolama Gezgini önerilir.

Azure Blob depolamada zaten depoladığınız verilerden veri kümesi oluşturmak için:

1. Veri **deposundan** > **veri kümesi oluştur** ' u seçin.
1. Veri kümenize bir **ad** atayın.
1. **Veri kümesi türü**olarak **Dosya** ' yı seçin.  
1. Veri deposunu seçin.
1. Verileriniz blob depolamalarınızın içindeki bir alt klasördeyse, yolu seçmek için, **Araştır** ' ı seçin.
    * Seçili yolun alt klasörlerindeki tüm dosyaları dahil etmek için yola "/* *" ekleyin.
    * Tüm verileri geçerli kapsayıcıya ve alt klasörlerine dahil etmek için "* */* . *" ekleyin.
1. Veri kümeniz için bir açıklama sağlayın.
1. **İleri**’yi seçin.
1. Ayrıntıları onaylayın. Ayarları değiştirmek için **geri** ' yi veya veri kümesini oluşturmak için **Oluştur** ' u seçin.

### <a name="create-a-dataset-from-uploaded-data"></a>Karşıya yüklenen verilerden veri kümesi oluşturma

Verilerinizi doğrudan karşıya yüklemek için:

1. **Yerel dosyalardan** **veri kümesi > oluştur** ' u seçin.
1. Veri kümenize bir **ad** atayın.
1. **Veri kümesi türü**olarak "dosya" yı seçin.
1. *Isteğe bağlı:* Veri deposu, kapsayıcı ve yolun yolunu özelleştirmek için **Gelişmiş ayarlar** ' ı seçin.
1. Karşıya yüklenecek yerel dosyaları seçmek için **Araştır** ' ı seçin.
1. Veri kümeniz için bir açıklama sağlayın.
1. **İleri**’yi seçin.
1. Ayrıntıları onaylayın. Ayarları değiştirmek için **geri** ' yi veya veri kümesini oluşturmak için **Oluştur** ' u seçin.

Veriler, Machine Learning çalışma alanınızın varsayılan blob deposuna ("Workspace BlobStore") yüklenir.

## <a name="specify-label-classes"></a>Etiket sınıfları belirtme

**Etiket sınıfları** sayfasında, verilerinizi sınıflandırmak için sınıflar kümesini belirtin. Bu özelliği dikkatle yapın, etiketlerinizin doğruluğu ve hızı sınıflar arasında seçim yapabileceğinden etkilenecektir. Örneğin, bitkiler veya hayvanlar için tam Genus ve türler yazım denetimi yapmak yerine bir alan kodu kullanın veya genabd 'yi kısaltılabilir.

Satır başına bir etiket girin. Yeni bir satır eklemek için **+** düğmesini kullanın. 3 veya 4 ' ten fazla etiket varsa ve 10 ' dan küçükse, Etiketleyiciler, çalışmalarını hızlandırmak için sayı tuşlarını kullanabilmesi için, adlara ("1:", "2:") önek eklemek isteyebilirsiniz.

## <a name="describe-the-labeling-task"></a>Etiketleme görevini açıkla

Etiketleme görevi açıkça açıklanmak önemlidir. **Etiketleme yönergeleri** sayfasında, bir dış siteye yönergeler için bir bağlantı ekleyebilirsiniz. Yönergeleri görev yönelimli ve hedef kitleye uygun halde tutun. Bu soruları göz önünde bulundurun:

* Göreceğiniz Etiketler nelerdir ve bunların arasından nasıl seçim yapılır? Başvurmak için bir başvuru metni var mı?
* Uygun bir etiket yoksa ne yapmalı?
* Birden çok etiket uygun görünüyorsa ne yapmalı?
* Bir etikete hangi güven eşiğinin uygulanmaları gerekir? Belirli değillerse "en iyi tahminlerinden" istediklerinizi ister misiniz?
* Kısmen veya çakışan nesnelerle ne yapmalı?
* İlgilendiğiniz bir nesne görüntünün kenarıyla kırpıldıklarında ne yapmalı?
* Bir etiketi gönderdikten sonra, bir hata yaptıklarında ne yapacaklarınız gerekir?

Sınırlayıcı kutular için, önemli sorular şunlardır:

* Bu görev için sınırlayıcı kutu nasıl tanımlanır? Tamamen nesnenin iç kısmında mi olmalı, yoksa dış mi olmalıdır? Mümkün olduğunca yakından kırpılmalıdır mi yoksa bazı bir açıklığı kabul edilebilir mi?
* Etiketleyicilerin sınırlayıcı kutuları tanımlamaya ne kadar uygun olduğunu ve tutarlılığını düşünüyorsunuz?

>[!NOTE]
> Etiketleyiciler 1-9 numaralı anahtarları kullanarak ilk 9 etiketi seçebileceğini unutmayın.

## <a name="initialize-the-labeling-project"></a>Etiketleme projesini başlatma

Etiketleme projesi başlatıldıktan sonra, projenin bazı yönleri sabittir. Görev türünü veya veri kümesini değiştiremezsiniz. Görev açıklamasının etiketlerini ve URL *'sini değiştirebilirsiniz.* Projeyi oluşturmadan önce ayarları dikkatle gözden geçirin. Projeyi gönderdikten sonra, **veri etiketleme** giriş sayfasına döndürülürsünüz, bu da projeyi **başlatılıyor**olarak gösterir. Bu sayfa otomatik olarak yenilenmez. Bu nedenle, bir duraklama sonrasında, projenin durumunu **oluşturulduğu**şekilde görmek için sayfayı el ile yenileyin.

## <a name="manage-teams-and-people"></a>Takımları ve kişileri yönetme

Varsayılan olarak, oluşturduğunuz her etiketleme projesi bir üye olarak sizinle yeni bir ekip alır. Ancak takımlar, projeler arasında da paylaşılabilir. Ve projeleri birden fazla takıma sahip olabilir. Takım oluşturmak için **takımlar** sayfasında **ekip Ekle** ' yi seçin.

**Kişiler sayfasında kişileri** yönetirsiniz. E-posta adresine göre kişi ekleyin ve kaldırın. Her bir etiketleyici, Microsoft hesabı veya Azure Active Directory aracılığıyla kimlik doğrulaması yapmak için kullanılır.  

Bir kişi ekledikten sonra bu kişiyi bir veya daha fazla takıma atayabilirsiniz: **takımlar** sayfasına gidin, takımı seçin ve sonra **kişi ata** veya **Kişileri Kaldır**' ı seçin.

Ekibe e-posta göndermek için takım **ayrıntıları** sayfasını görüntülemek üzere ekibi seçin. Bu sayfada, ekip üzerinde herkesin adreslerine sahip bir e-posta taslağı açmak için **e-posta ekibi** ' ni seçin.

## <a name="run-and-monitor-the-project"></a>Projeyi çalıştırma ve izleme

Projeyi başlattıktan sonra Azure bu uygulamayı çalıştırmaya başlayacaktır. **Proje ayrıntılarına**gitmek Için ana **veri etiketleme** sayfasında projeyi seçin. **Pano** sekmesi etiketleme görevinin ilerlemesini gösterir.

**Veri** sekmesinde, veri kümenizi görebilir ve etiketli verileri gözden geçirebilirsiniz. Hatalı etiketlenmiş verileri görürseniz, bunu seçin ve **Reddet**' i seçin. Bu, etiketleri kaldırır ve verileri etiketsiz kuyruğa geri koyar.

**Takıma takım** atamak veya ekip atamasını kaldırmak için takım sekmesini kullanın.

Projeyi duraklatmak veya yeniden başlatmak için **duraklat**/**Başlat** düğmesini seçin. Yalnızca proje çalışırken verileri etiketleyebilir.

**Etiket verileri**' ni seçerek doğrudan **proje ayrıntıları** sayfasından verileri etiketleyebilir.

## <a name="add-labels-to-a-project"></a>Bir projeye etiketler ekleme

Etiketleme işlemi sırasında, görüntülerinizi sınıflandırmak için ek etiketlerin gerekli olduğunu fark edebilirsiniz.  Örneğin, kafa karıştırıcı görüntüleri göstermek için bir "Bilinmeyen" veya "diğer" etiketi eklemek isteyebilirsiniz.

Bir projeye bir veya daha fazla etiket eklemek için bu adımları kullanın:

1. Ana **veri etiketleme** sayfasında projeyi seçin.
1. Sayfanın en üstünde bulunan etiketleyicilerinin etkinliğini durdurmak için **Duraklat** ' ı seçin.
1. **Ayrıntılar** sekmesini seçin.
1. Sol taraftaki listede **etiket sınıfları**' nı seçin.
1. Listenin en üstünde **+** etiket ekle ![etiket ekle ' yi seçin](media/how-to-create-labeling-projects/add-label.png)
1. Formunda, yeni etiketinizi ekleyin ve nasıl devam etmek istediğinizi seçin.  Bir görüntü için kullanılabilir etiketleri değiştirdiğiniz için, zaten etiketlenmiş verilerin nasıl davranalınacağını seçersiniz:
    * Tüm mevcut etiketleri kaldırarak baştan başlayın.  Yeni tam Etiketler kümesiyle başlayarak etiketlemeye başlamak istiyorsanız bu seçeneği belirleyin. 
    * Tüm mevcut etiketleri koruyarak, baştan başlayın.  Tüm verileri etiketsiz olarak işaretlemek için bu seçeneği belirleyin, ancak önceden etiketlenmiş görüntüler için mevcut etiketleri varsayılan etiket olarak tutun.
    * Tüm mevcut etiketleri koruyarak devam edin. Tüm verilerin zaten olduğu şekilde etiketlendiği ve henüz etiketsiz veriler için yeni etiketi kullanmaya başlamış olan bu seçeneği belirleyin.
1. Yönergeler sayfanızı yeni etiketler için gereken şekilde değiştirin.
1. Tüm yeni etiketleri ekledikten sonra, sayfanın üst kısmında, projeyi yeniden başlatmak için **Başlat** ' ı seçin.  

## <a name="export-the-labels"></a>Etiketleri dışarı aktarma

Machine Learning deneme için etiket verilerini dilediğiniz zaman dışarı aktarabilirsiniz. Resim etiketleri [Coco formatında](http://cocodataset.org/#format-data) veya Azure Machine Learning veri kümesi olarak aktarılabilir. Etiketleme projenizin **proje ayrıntıları** sayfasında **dışarı aktar** düğmesini kullanın.

COCO dosyası, Azure Machine Learning çalışma alanının varsayılan Blob deposunda *dışarı aktarma/Coco*içindeki bir klasörde oluşturulur. Machine Learning **veri kümeleri** bölümünde, dışarıya aktarılmış Azure Machine Learning veri kümesine erişebilirsiniz. Veri kümesi ayrıntıları sayfası, Python 'daki etiketlerinize erişmek için örnek kod de sağlar.

![Aktarılmış veri kümesi](./media/how-to-create-labeling-projects/exported-dataset.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Resim sınıflandırması veya nesne algılaması](how-to-label-images.md) için etiket resimleri
* [Azure Machine Learning ve Machine Learning Studio (klasik)](compare-azure-ml-to-studio-classic.md) hakkında daha fazla bilgi edinin
