---
title: Etiketleme projesindeki resimleri etiketleme
title.suffix: Azure Machine Learning
description: Azure Machine Learning etiketleme projesindeki veri etiketleme araçlarını kullanmayı öğrenin.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 1e27fca86613757c36ac664e2e449cabed68d550
ms.sourcegitcommit: aee08b05a4e72b192a6e62a8fb581a7b08b9c02a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/09/2020
ms.locfileid: "75772457"
---
# <a name="tag-images-in-a-labeling-project"></a>Etiketleme projesindeki resimleri etiketleme

Proje yöneticiniz Azure Machine Learning ' de [bir etiketleme projesi](https://docs.microsoft.com/azure/machine-learning/how-to-create-labeling-projects#create-a-labeling-project) oluşturduktan sonra etiketleme aracını kullanarak bir Machine Learning projesi için hızlı bir şekilde veri hazırlayabilir. Bu makalede açıklanır:

> [!div class="checklist"]
> * Etiketleme projelerinize erişme
> * Etiketleme Araçları
> * Belirli etiketleme görevleri için araçları kullanma

## <a name="prerequisites"></a>Ön koşullar

* Çalışan bir veri etiketleme projesi için etiketleme portalı URL 'SI
* Kuruluş ve proje için bir [Microsoft hesabı](https://account.microsoft.com/account) veya Azure Active Directory hesabı

> [!NOTE]
> Proje Yöneticisi etiketleme portalı URL 'sini, **proje ayrıntıları** sayfasının **Ayrıntılar** sekmesinde bulabilir.

## <a name="sign-in-to-the-projects-labeling-portal"></a>Projenin etiketleme portalında oturum açın

Proje Yöneticisi tarafından sunulan etiketleme portalı URL 'sine gidin. Yöneticinin sizi takıma eklemek için kullandığı e-posta hesabını kullanarak oturum açın. Çoğu kullanıcı için Microsoft hesabı olacaktır. Etiketleme projesi Azure Active Directory kullanıyorsa, oturum açacaksınız.

## <a name="understand-the-labeling-task"></a>Etiketleme görevini anlayın

Oturum açtıktan sonra projenin genel bakış sayfasını görürsünüz.

**Ayrıntılı yönergeleri görüntüle**' ye gidin. Bu yönergeler projenize özeldir. Bunlar, söz konusu verilerin türünü, kararlarınızı nasıl yapmanız gerektiğini ve diğer ilgili bilgileri açıklar. Bu bilgileri okuduktan sonra proje sayfasına dönüp **etiketlemeyi Başlat**' ı seçin.

## <a name="common-features-of-the-labeling-task"></a>Etiketleme görevinin ortak özellikleri

Tüm görüntü etiketleme görevlerinde, proje yöneticisi tarafından belirtilen bir kümeden uygun bir etiketi veya etiketleri seçersiniz. Klavyenizdeki sayı tuşlarını kullanarak ilk dokuz etiketi seçebilirsiniz.  

Görüntü sınıflandırma görevlerinde birden çok görüntüyü aynı anda görüntülemeyi seçebilirsiniz. Düzeni seçmek için görüntü alanının üzerindeki simgeleri kullanın. Görüntülenecek tüm görüntüleri eşzamanlı olarak seçmek için **Tümünü Seç**' i kullanın. Tek tek görüntüleri seçmek için görüntünün sağ üst köşesindeki dairesel seçim düğmesini kullanın. Etiket uygulamak için en az bir görüntü seçmelisiniz. Birden çok görüntü seçerseniz, seçtiğiniz herhangi bir etiket seçili tüm görüntülere uygulanır.

Burada iki-by iki düzen seçtik ve "Mammal" etiketini, ve Orca 'nın görüntülerine uygulamak üzeresiniz. Parça görüntüsü zaten "Cartilaginou balığı" olarak etiketlendi ve Iguana henüz etiketlendi.

![Birden çok resim düzeni ve seçimi](./media/how-to-label-images/layouts.png)

> [!Important] 
> Yalnızca etiketli verilerin yeni bir sayfasına sahip olduğunuzda mizanpajları değiştirin. Mizanpajları değiştirmek sayfanın devam eden etiketleme işini temizler.

Sayfadaki tüm görüntüleri etiketlediyseniz Azure, **Gönder** düğmesine izin vermez. Çalışmanızı kaydetmek için **Gönder** ' i seçin.

Bir yandan veriler için Etiketler gönderdikten sonra, Azure sayfayı iş kuyruğundan yeni bir görüntü kümesiyle yeniler.

## <a name="tag-images-for-multi-class-classification"></a>Birden çok sınıf sınıflandırması için resimleri etiketleme

Projeniz "Image Classification Multi-Class" türünde ise, tüm görüntüye tek bir etiket atarsınız. Yönergeleri istediğiniz zaman gözden geçirmek için, **yönergeler** sayfasına gidin ve **ayrıntılı yönergeleri görüntüle**' yi seçin.

Bir resme etiket atadıktan sonra bir hata yaptık, bu hatayı çözebilirsiniz. Etiketi temizlemek için görüntünün altında görüntülenen etikette "**X**" işaretini seçin. Veya resmi seçip başka bir sınıf seçin. Yeni seçilen değer, daha önce uygulanan etiketin yerine geçecek.

## <a name="tag-images-for-multi-label-classification"></a>Çok etiketli sınıflandırmanın resimlerini etiketleme

"Görüntü sınıflandırması çoklu etiketi" türünde bir proje üzerinde çalışıyorsanız, bir görüntüye bir *veya daha fazla* etiket uygularsınız. Projeye özgü yönleri görmek için **yönergeler** ' i seçin ve **ayrıntılı yönergeleri görüntüleyin**.

Etiketlemek istediğiniz görüntüyü seçin ve ardından etiketi seçin. Etiket tüm seçili görüntülere uygulanır ve ardından görüntülerin seçimi kaldırılır. Daha fazla etiket uygulamak için görüntüleri yeniden seçmeniz gerekir. Aşağıdaki animasyon çok etiketli etiketlemesini göstermektedir:

1. "Okyanus" etiketini uygulamak için **Tümünü Seç** kullanılır.
1. Tek bir görüntü seçilir ve "closeup" olarak etiketlenir.
1. Üç görüntü seçilir ve "geniş açılı" olarak etiketlenir.

![Animasyon MultiLabel akışını gösterir](./media/how-to-label-images/multilabel.gif)

Bir hata düzeltmek için "**X**" düğmesine tıklayarak tek bir etiketi temizleyin veya görüntüleri seçin ve ardından etiketi seçerek seçili tüm görüntülerden etiketi temizler. Bu senaryo burada gösterilmiştir. "Land" tıklanması, seçili iki görüntüden bu etiketi temizler.

![Bir ekran görüntüsü birden çok seçimi gösterir](./media/how-to-label-images/multiple-deselection.png)

Azure yalnızca, her görüntüye en az bir etiket uyguladıktan sonra **Gönder** düğmesini etkinleştirir. Çalışmanızı kaydetmek için **Gönder** ' i seçin.

## <a name="tag-images-and-specify-bounding-boxes-for-object-detection"></a>Resimleri etiketleme ve nesne algılaması için sınırlayıcı kutuları belirtme

Projeniz "nesne kimliği (sınırlayıcı kutular)" türündedir, görüntüde bir veya daha fazla sınırlayıcı kutu belirtip her kutuya bir etiket uygularsınız. Görüntülerde, her biri tek etiketli birden çok sınırlayıcı kutusu olabilir. Projenizde birden fazla sınırlayıcı kutunun kullanıldığını öğrenmek için **ayrıntılı yönergeleri görüntüle** ' i kullanın.

1. Oluşturmayı planladığınız sınırlama kutusu için bir etiket seçin.
1. Dikdörtgen kutu **aracı** ![dikdörtgen kutu aracını seçin](./media/how-to-label-images/rectangular-box-tool.png) veya "R" seçeneğini belirleyin.
3. Kaba bir sınırlayıcı kutu oluşturmak için, Hedefinizdeki çapraz doğrultuda tıklayın ve sürükleyin. Sınırlayıcı kutuyu ayarlamak için kenarları veya köşeleri sürükleyin.

![Bir ekran görüntüsü temel sınırlayıcı kutusu oluşturmayı gösterir.](./media/how-to-label-images/bounding-box-sequence.png)

Bir sınırlayıcı kutuyu silmek için, oluşturulduktan sonra sınırlayıcı kutunun yanında görünen X şeklindeki hedefe tıklayın.

Varolan bir sınırlayıcı kutunun etiketini değiştiremezsiniz. Etiket atama hatası yaparsanız, sınırlayıcı kutuyu silmeniz ve doğru etiketle yeni bir tane oluşturmanız gerekir.

Varsayılan olarak, varolan sınırlayıcı kutuları düzenleyebilirsiniz. Kilit **/kilit açma** bölgeleri aracı ![kilit/kilit açma aracı](./media/how-to-label-images/lock-bounding-boxes-tool.png) veya "L" Bu davranışa geçiş yapar. Bölgeler kilitliyse, yalnızca yeni bir sınırlayıcı kutunun şeklini veya konumunu değiştirebilirsiniz.

Mevcut bir sınırlayıcı kutuyu ayarlamak **için bölge işleme aracı ![** bölgeleri işleme aracı](./media/how-to-label-images/regions-tool.png) veya "d" kullanın. Şekli ayarlamak için kenarları veya köşeleri sürükleyin. Tüm sınırlayıcı kutusunu sürüklemek için iç öğesine tıklayın. Bir bölgeyi düzenleyemezsiniz, büyük olasılıkla bir bölgeyi **Kilitle/kilidini aç** aracını atlayabilirsiniz.

Şablon-kutusu aracı](./media/how-to-label-images/template-box-tool.png) veya "T" ![**şablon tabanlı Box** aracını kullanarak aynı boyutta birden fazla sınırlayıcı kutu oluşturun. Görüntüde hiçbir sınırlayıcı kutu yoksa ve şablon tabanlı kutuları etkinleştirirseniz, araç 50-50 piksel kutu üretir. Bir sınırlayıcı kutusu oluşturup şablon tabanlı kutuları etkinleştirirseniz, yeni sınırlama kutuları oluşturduğunuz son kutunun boyutu olur. Şablon tabanlı kutular, yerleştirme sonrasında yeniden boyutlandırılabilir. Şablon tabanlı bir kutunun yeniden boyutlandırılması yalnızca belirli bir kutuyu yeniden boyutlandırır.

Geçerli görüntüdeki *Tüm* sınırlayıcı kutuları silmek için, **tüm bölgeleri** Sil aracını ![bölgeleri sil araç](./media/how-to-label-images/delete-regions-tool.png)seçin.

Bir görüntü için sınırlayıcı kutuları oluşturduktan sonra, çalışmanızı kaydetmek için **Gönder** ' i seçin veya sürmekte olan çalışmanız kaydedilmez.

## <a name="finish-up"></a>Bitirme

Etiketli verilerin bir sayfasını gönderdiğinizde Azure, bir iş kuyruğundan sizin için etiketlendirilmiş yeni veri atar. Etiketli daha fazla veri yoksa, bunu portal giriş sayfasının bağlantısı ile birlikte gösteren bir ileti alırsınız.

Etiketleme tamamlandığında, etiketleme portalının sağ üst köşesindeki adınızı seçin ve ardından **Oturumu Kapat**' ı seçin. Oturumu kapatma, sonunda Azure "sizin zaman aşımına uğrar" ve verilerinizi başka bir etiketleyici 'ye atacaktır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure 'da görüntü sınıflandırma modellerini eğitme](https://docs.microsoft.com/azure/machine-learning/tutorial-train-models-with-aml) hakkında bilgi edinin
* [Azure kullanarak nesne algılama hakkında bilgi edinin ve "daha hızlı R-CNN" tekniği](https://www.microsoft.com/developerblog/2017/10/24/bird-detection-with-azure-ml-workbench/)
