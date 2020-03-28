---
title: Etiketleme projesindeki görüntüleri etiketleme
title.suffix: Azure Machine Learning
description: Azure Machine Learning etiketleme projesinde veri etiketleme araçlarını nasıl kullanacağınızı öğrenin.
author: lobrien
ms.author: laobri
ms.service: machine-learning
ms.topic: tutorial
ms.date: 11/04/2019
ms.openlocfilehash: 2e14b669aadeec4c6a7245be8940051ec604059b
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "79409567"
---
# <a name="tag-images-in-a-labeling-project"></a>Etiketleme projesindeki görüntüleri etiketleme

Proje yöneticiniz Azure Machine Learning'de [bir etiketleme projesi oluşturduktan](https://docs.microsoft.com/azure/machine-learning/how-to-create-labeling-projects#create-a-labeling-project) sonra, bir Makine Öğrenimi projesi için hızla veri hazırlamak için etiketleme aracını kullanabilirsiniz. Bu makalede açıklanır:

> [!div class="checklist"]
> * Etiketleme projelerinize nasıl erişilir?
> * Etiketleme araçları
> * Belirli etiketleme görevleri için araçlar nasıl kullanılır?

## <a name="prerequisites"></a>Ön koşullar

* Çalışan bir veri etiketleme projesi için etiketleme portalı URL'si
* Kuruluş ve proje için bir [Microsoft hesabı](https://account.microsoft.com/account) veya Azure Etkin Dizin hesabı

> [!NOTE]
> Proje yöneticisi, Proje **ayrıntıları** sayfasının **Ayrıntılar** sekmesinde etiketleme portalı URL'sini bulabilir.

## <a name="sign-in-to-the-projects-labeling-portal"></a>Projenin etiketleme portalında oturum açın

Proje yöneticisi tarafından sağlanan etiketleme portalı URL'sine gidin. Yöneticinin sizi takıma eklemek için kullandığı e-posta hesabını kullanarak oturum açın. Çoğu kullanıcı için bu, Microsoft hesabınız olacaktır. Etiketleme projesi Azure Active Directory kullanıyorsa, bu şekilde oturum açacaksınız.

## <a name="understand-the-labeling-task"></a>Etiketleme görevini anlama

Oturum açmadan sonra projenin genel bakış sayfasını görürsünüz.

Ayrıntılı **yönergeleri Görüntüleyin'** e gidin. Bu yönergeler projenize özeldir. Bunlar, karşı karşıya olduğunuz veri türünü, kararlarınızı nasıl vermeniz gerektiğini ve diğer ilgili bilgileri açıklar. Bu bilgileri okuduktan sonra proje sayfasına dönün ve **etiketlemeyi başlat'ı**seçin.

## <a name="common-features-of-the-labeling-task"></a>Etiketleme görevinin ortak özellikleri

Tüm görüntü etiketleme görevlerinde, proje yöneticisi tarafından belirtilen bir kümeden uygun bir etiket veya etiket seçersiniz. Klavyenizdeki sayı tuşlarını kullanarak ilk dokuz etiketi seçebilirsiniz.  

Görüntü sınıflandırma görevlerinde, birden çok görüntüyü aynı anda görüntülemeyi seçebilirsiniz. Düzeni seçmek için görüntü alanının üzerindeki simgeleri kullanın. Görüntülenen tüm görüntüleri aynı anda seçmek için **Tümünü Seç'i**kullanın. Tek tek görüntüleri seçmek için, resmin sağ üst köşesindeki dairesel seçim düğmesini kullanın. Etiket uygulamak için en az bir resim seçmeniz gerekir. Birden çok resim seçerseniz, seçtiğiniz herhangi bir etiket seçili tüm resimlere uygulanır.

Burada ikişer ikişer bir düzen seçtik ve ayı ve orka görüntülerine "Memeli" etiketini uygulamak üzereyiz. Köpekbalığının görüntüsü zaten "Kıkırdak balık" olarak etiketlendi ve iguana henüz etiketlenmemiş.

![Birden çok görüntü düzeni ve seçim](./media/how-to-label-images/layouts.png)

> [!Important] 
> Yalnızca etiketlenmemiş yeni bir veri sayfanız olduğunda mizanpajları değiştirin. Düzenler arasında geçiş yapmak, sayfanın devam eden etiketleme çalışmasını temizler.

Azure, sayfadaki tüm resimleri etiketlediğinizde **Gönder** düğmesini etkinleştirirken. Çalışmanızı kaydetmek için **Gönder'i** seçin.

Eldeki veriler için etiketler gönderdikten sonra Azure, sayfayı iş kuyruğundan yeni bir resim kümesiyle yeniler.

### <a name="assisted-machine-learning"></a>Destekli makine öğrenimi 

Çok sınıflı veya çok etiketli sınıflandırma görevi sırasında makine öğrenimi algoritmaları tetiklenebilir. Bu algoritmalar projenizde etkinse, aşağıdakileri görebilirsiniz:

* Bir miktar resim etiketlendikten sonra, Proje adının yanında ekranın üst kısmında **kümelenmiş Görevler** görebilirsiniz.  Bu, görüntülerin aynı sayfada benzer görüntüleri sunmak için birlikte gruplandırıldığı anlamına gelir.  Bu nedenle, gruplandırmadan yararlanmak için birden çok görüntü görünümünden birine geçin.  

* Daha sonraki bir noktada, proje adının yanında **önceden etiketlenmiş Görevler** görebilirsiniz.  Görüntüler daha sonra bir makine öğrenme sınıflandırma modelinden gelen önerilen bir etiketle görünür. Hiçbir makine öğrenme modeli % 100 doğruluk vardır. Yalnızca modelin kendinden emin olduğu görüntüleri kullansak da, bu görüntüler yine de yanlış etiketlenmiş olabilir.  Bu etiketleri gördüğünüzde, sayfayı göndermeden önce yanlış etiketleri düzeltin.  

Özellikle bir etiketleme projesinin başlarında, makine öğrenimi modeli yalnızca küçük bir görüntü alt kümesini önceden etiketleyecek kadar doğru olabilir. Bu görüntüler etiketlendikten sonra, etiketleme projesi bir sonraki model eğitimi turu için daha fazla veri toplamak için manuel etiketlemeye geri döner. Zamanla, model görüntülerin daha yüksek bir oranı hakkında daha emin hale gelir, projede daha sonra daha fazla ön etiket görevleri ile sonuçlanır.

## <a name="tag-images-for-multi-class-classification"></a>Çok sınıflı sınıflandırma için etiket görüntüleri

Projeniz "Görüntü Sınıflandırma Sıtkı Sınıflı" türündeyse, görüntünün tamamına tek bir etiket atarsınız. Talimatları istediğiniz zaman gözden geçirmek için **Talimatlar** sayfasına gidin ve **ayrıntılı yönergeleri görüntüle'yi**seçin.

Bir görüntüye etiket atadıktan sonra bir hata yaptığınızı fark ederseniz, bunu düzeltebilirsiniz. Etiketi temizlemek için görüntünün altında görüntülenen etiketteki "**X**" seçeneğini belirleyin. Veya görüntüyü seçin ve başka bir sınıf seçin. Yeni seçilen değer, daha önce uygulanan etiketin yerini alır.

## <a name="tag-images-for-multi-label-classification"></a>Çok etiketli sınıflandırma için etiket resimleri

"Resim Sınıflandırma Çok Etiketli" türünde bir proje üzerinde çalışıyorsanız, bir resme bir *veya daha fazla* etiket uygularsınız. Projeye özel yönergeleri görmek için **Yönergeler'i** seçin ve **ayrıntılı yönergeleri görüntüleyin'** e gidin.

Etiketlemek istediğiniz resmi seçin ve ardından etiketi seçin. Etiket tüm seçili resimlere uygulanır ve ardından görüntüler seçilir. Daha fazla etiket uygulamak için görüntüleri yeniden seçmeniz gerekir. Aşağıdaki animasyon, çok etiketli etiketlemeyi gösterir:

1. "Okyanus" etiketini uygulamak için kullanılan **tümünü seçin.**
1. Tek bir resim seçilir ve "Yakın Çekim" olarak etiketlenir.
1. Üç resim seçilir ve "Geniş açı" olarak etiketlenir.

![Animasyon çoklu etiket akışını gösterir](./media/how-to-label-images/multilabel.gif)

Bir hatayı düzeltmek için, tek bir etiketi temizlemek veya görüntüleri seçmek için "**X**" düğmesini tıklatın ve ardından seçilen tüm resimlerden etiketi temizleyen etiketi seçin. Bu senaryo burada gösterilir. "Arazi"ye tıkladığınızda bu etiketi seçilen iki resimden temizler.

![Ekran görüntüsü birden çok deselections gösterir](./media/how-to-label-images/multiple-deselection.png)

Azure, her görüntüye yalnızca en az bir etiket uyguladıktan sonra **Gönder** düğmesini etkinleştirir. Çalışmanızı kaydetmek için **Gönder'i** seçin.


## <a name="tag-images-and-specify-bounding-boxes-for-object-detection"></a>Görüntüleri etiketleme ve nesne algılama için sınırlayıcı kutuları belirtin

Projeniz "Nesne Tanımlama (Sınırlayıcı Kutular) türündeyse", görüntüde bir veya daha fazla sınırlayıcı kutu belirtir ve her kutuya bir etiket uygularsınız. Görüntülerin her biri tek bir etikete sahip birden çok sınırlayıcı kutusu olabilir. Projenizde birden çok sınırlayıcı kutu kullanıp kullanılmamasını belirlemek için **ayrıntılı yürütme yü**

1. Oluşturmayı planladığınız sınırlayıcı kutu için bir etiket seçin.
1. Dikdörtgen **kutu** aracı ![Dikdörtgen kutu aracını](./media/how-to-label-images/rectangular-box-tool.png) seçin veya "R" seçeneğini belirleyin.
3. Kaba bir sınırlama kutusu oluşturmak için hedef üzerinde çapraz olarak tıklayın ve sürükleyin. Sınırlayıcı kutuyu ayarlamak için kenarları veya köşeleri sürükleyin.

![Ekran görüntüsü, temel sınırlayıcı kutu oluşturmayı gösterir.](./media/how-to-label-images/bounding-box-sequence.png)

Sınırlayıcı kutusunu silmek için, oluşturulduktan sonra sınırlama kutusunun yanında görünen X şeklindeki hedefi tıklatın.

Varolan bir sınırlandırma kutusunun etiketini değiştiremezsiniz. Etiket ataması hatası yaparsanız, sınırlayıcı kutusunu silmeniz ve doğru etikete sahip yeni bir etiket oluşturmanız gerekir.

Varsayılan olarak, varolan sınırlayıcı kutuları da edinebilirsiniz. **Bölgelere Kilitle/kilidini açma** ![aracı](./media/how-to-label-images/lock-bounding-boxes-tool.png) Bölgeler aracını kilitler/kilidini açma aracı veya "L" bu davranışı geçiştirir. Bölgeler kilitliyse, yalnızca yeni bir sınırlama kutusunun şeklini veya konumunu değiştirebilirsiniz.

Varolan bir ![sınırkutusunu](./media/how-to-label-images/regions-tool.png) ayarlamak için **Bölgeler işleme** aracını veya "M" kullanın. Şekli ayarlamak için kenarları veya köşeleri sürükleyin. Tüm sınırlayıcı kutuyu sürükleyebilmek için iç mekana tıklayın. Bir bölgeyi değiştiremezseniz, büyük olasılıkla **bölgelere kilitle/aç** aracını değiştirmişsinizdir.

Aynı boyutta birden çok ![sınırlayıcı](./media/how-to-label-images/template-box-tool.png) kutu oluşturmak için Şablon **tabanlı kutu** aracı Şablon kutusu aracını veya "T" aracını kullanın. Görüntüde sınırlayıcı kutuları yoksa ve şablon tabanlı kutuları etkinleştiriyorsanız, araç 50'ye 50 piksellik kutular üretir. Bir sınırlayıcı kutu oluşturur ve ardından şablon tabanlı kutuları etkinleştirirseniz, yeni sınırlayıcı kutular oluşturduğunuz son kutuboyutunda olur. Şablon tabanlı kutular yerleştirildikten sonra yeniden boyutlandırılabilir. Şablon tabanlı bir kutuyu yeniden boyutlandırma, yalnızca o kutuyu yeniden boyutlandırıyor.

Geçerli resimdeki *tüm* sınırlayıcı kutuları silmek **için, tüm bölgeleri sil** aracı ![nı seçin bölgeleri sil aracını](./media/how-to-label-images/delete-regions-tool.png)seçin.

Bir resim için sınırlayıcı kutuları oluşturduktan sonra, çalışmanızı kaydetmek için **Gönder'i** seçin veya devam eden çalışmanız kaydedilmez.

## <a name="finish-up"></a>Bitirme

Etiketli verilerin bir sayfasını gönderdiğinizde, Azure iş kuyruğundan size yeni etiketlenmemiş veriler atar. Daha fazla etiketlenmemiş veri yoksa, portal ana sayfasına bir bağlantıyla birlikte bunu belirten bir ileti alırsınız.

Etiketlemeyi bitirdiğinizde, etiketleme portalının sağ üst köşesinde adınızı seçin ve ardından **oturumunuzu seçin.** Oturumunuzu imzalamazsanız, sonunda Azure "zaman ınız dolacak" ve verilerinizi başka bir etiketleyiciye atar.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure'da görüntü sınıflandırma modellerini eğitmeyi](https://docs.microsoft.com/azure/machine-learning/tutorial-train-models-with-aml) öğrenin

