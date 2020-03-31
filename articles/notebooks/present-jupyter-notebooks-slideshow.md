---
title: Azure Dizüstü Bilgisayarlar Önizleme'de slayt gösterisi olarak bir Jupyter dizüstü bilgisayar sunma
description: Bir Jupyter not defterinde slayt gösterisi modu için hücreleri nasıl yapılandıracağınızı ve ardından RISE uzantısını kullanarak slayt gösterisini nasıl sunacağınızı öğrenin.
ms.topic: how-to
ms.date: 12/04/2018
ms.openlocfilehash: 05dd3d9c5580e208ecf6f9e6d762476b0b493a6c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75647127"
---
# <a name="run-a-notebook-slideshow-in-azure-notebooks-preview"></a>Azure Not Defteri Önizleme'de bir dizüstü bilgisayar slayt gösterisi çalıştırma

Azure Dizüstü Bilgisayarlar, bir dizüstü bilgisayarı doğrudan slayt gösterisi olarak sunmanıza olanak tanıyan Jupyter/IPython Slayt Gösterisi Uzantısı (RISE) ile önceden yapılandırılmıştır. Slayt gösterisinde, hücreler genellikle büyük ekranlarda sunulması uygun bir yazı tipi boyutu kullanılarak teker teker görüntülenir ve ayrı bir demo bilgisayara geçmek yerine kodu çalıştırmaya devam edebilirsiniz.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

Aşağıdaki resimde, Markdown ve kod hücrelerini hep birlikte görebileceğiniz standart not defteri görünümü gösterilmektedir:

![Standart görünümde bir not defteri](media/slideshow/slideshow-notebook-view.png)

Bir slayt gösterisi başlattığınızda, ilk hücre tarayıcıyı doldurmak için büyütülür, sol üstteki **X** slayt gösterisi **çıkar, ?** sol altta klavye kısayollarını görüntüler ve sağ alttaki oklar slaytlar arasında gezinir:

![Slayt gösterisi modunda bir not defteri](media/slideshow/slideshow-slide-view.png)

Slayt gösterisi için not defteri hazırlamak iki birincil etkinlik içerir:

1. İşaretleme hücreleri büyük yazı tipleriyle işlendiğinden, slayt gösterisinde bazı içerikler görünmeyebilir. Bu nedenle genellikle belirli bir hücredeki metin miktarını sınırlarsınız; dört ila altı satırlı bir üstbilgi genellikle en iyi şekilde çalışır. Daha fazla metniniz varsa, bu bilgileri birden çok hücreye bölün.

2. Slayt gösterisi hücre araç çubuğunu kullanarak slayt gösterisindeki her hücrenin davranışını yapılandırın. Hücre türleri gezinti düğmelerinin davranışını belirler.

## <a name="the-anatomy-of-a-slideshow"></a>Slayt gösterisinin anatomisi

Rasgele bir not defteri alır ve slayt gösterisi için kullanırsanız, genellikle tüm hücrelerin birbirine karışık olduğunu ve içeriğin çoğunun tarayıcı penceresinin alt kısmından gizlendiğini fark edersiniz. Etkili bir sunu yapmak için, Slayt Gösterisi hücre araç çubuğunu kullanarak her hücreye slayt gösterisi türü atamanız gerekir:

1. **Görünüm** menüsünde **Hücre Araç Çubuğu** > **Slayt Gösterisi'ni**seçin:

    ![Hücre slayt gösterisi araç çubuğunu açma](media/slideshow/slideshow-view-cell-toolbar.png)

1. Not Defteri'ndeki her hücrenin sağ üst tarafında **Slayt Türü** açılır açılır:

    ![Hücre slayt gösterisi araç çubuğu](media/slideshow/slideshow-cell-toolbar.png)

1. Her hücre için beş türden birini seçin:

    ![Hücre slayt gösterisi türleri](media/slideshow/slideshow-cell-slide-types.png)

    | Slayt türü | Davranış |
    | --- | --- |
    | - (ayarlı değil) | Hücre, slayt gösterisinde genellikle istenilen bir etki olmayan önceki hücreyle görüntülenir. |
    | Slayt | Hücre, gezinti denetiminin sol ve sağ okları kullanılarak gezinilen birincil bir slayttır. |
    | Alt slayt | Hücre "aşağıda" bir birincil slayt, navigasyon denetiminin aşağı ok kullanarak gezindi. Yukarı ok birincil slayta döner. Alt slaytlar, sununun ana yolunda atlayabileceğiniz ikincil malzeme için kullanılır, ancak gerekirse kolayca kullanılabilir. |
    | Parça | Hücre içeriği aşağı gezinti oku kullanırken önceki slayt veya alt slayt bağlamında görünür (yukarı ok kullanırken bir parça kaldırılır). Bu kodun slayt içinde görünmesini sağlamak için kod hücreli bir parça kullanabilir veya metin madde işaretlerinin tek tek görünmesini sağlamak için birden çok parça kullanabilirsiniz (sonraki bölümdeki örneğe bakın). Parçalar geçerli slayt üzerine inşa olduğundan, fazla parçalar tarayıcı penceresinin alt kısmındagörünür olmaz. |
    | Atla | Hücre slayt gösterisinde gösterilmez. |
    | Notlar | Hücre, slayt gösterisinde gösterilmeyen hoparlör notları olarak içerir. |

1. Başlangıçta, her hücre için **Slayt'ı** seçmek yararlıdır. Daha sonra slayt gösterisini çalıştırabilir ve uygun ayarlamalar yapabilirsiniz.

### <a name="example-fragment-cells-for-bullet-items"></a>Örnek: madde işareti öğeleri için parça hücreleri

Slayttaki madde işaretlerinin tek tek görünmesini sağlamak için slayt üstbilgisini **Slayt** türüne sahip bir Markdown hücresine yerleştirin ve ardından her bir madde işaretini **Parça** türüyle ayrı bir Markdown hücresine yerleştirin:

![Madde işareti öğeleri için birden çok İşaretle kapatma hücresi oluşturma örneği](media/slideshow/slideshow-fragments.png)

Slayt gösterisi, tüm madde işaretleri aynı hücrede yken daha fazla dikey aralıkla parçalar işledığından, bu kadar çok madde işareti öğesi kullanamayabilirsiniz.

## <a name="run-the-slideshow"></a>Slayt gösterisini çalıştırma

1. Herhangi bir Markdown hücresini düzenlediyseniz, HTML'lerini işlemek için çalıştırdığınızdan emin olun, aksi takdirde slayt gösterisinde Markdown *olarak* görünürler.

1. Her hücre için **Slayt Türünü** yapılandırdıktan sonra, slayt gösterisini başlatacak hücreyi seçin ve ardından ana araç çubuğundaki RISE **Slayt Gösterisine Gir/Çıkın** düğmesini seçin:

    ![Ana araç çubuğuna RISE Slayt Gösterisi düğmesini girin/Çıkın](media/slideshow/slideshow-start.png)

1. Slaytlar ve parçalar arasında gezinmek için gezinti denetiminde sol ve sağ okları kullanın. Denetimdeki metin *slide.sub-slide'ı*temsil eden bir sayı gösterir.

    ![Slayt gösterisi gezinme denetimi](media/slideshow/slideshow-navigation-control.png)

1. Slaytlar ve alt slaytlar ve parçalarıarasında gezinmek için, etkinleştirildiyse yukarı ve aşağı okları kullanın:

    ![Alt slaytlar için slayt gösterisi gezinti denetimleri](media/slideshow/slideshow-navigation-control-subslide.png)

1. Kod hücresinde, kodu çalıştırmak için oynat düğmesini kullanın; çıktı slaytta görünür:

    ![Kod hücresini çalıştırmak için oynat düğmesi](media/slideshow/slideshow-run-code-cell.png)

    ![Kod hücre çıktısı slayt gösterisinde görünür](media/slideshow/slideshow-run-code-cell-output.png)

    > [!Tip]
    > Hücre çıkışı slayt gösterisinde hücrenin bir parçası olarak kabul edilir. Bir hücreyi not defteri veya slayt gösterisi görünümünde çalıştırıyorsanız, çıktı diğer görünümde de görünür. Çıktıyı temizlemek için **Hücre** > **Akımı Çıktılarını** > **Temizle** komutunu (geçerli hücre için) veya**Tüm Çıktıları** >  **TemizLe** > (tüm hücreler için) kullanın.**Clear**

1. Slayt gösterisini bitirdikten sonra not defteri görünümüne dönmek için **X'i** kullanın.

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapilir: Projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapilir: Paketleri dizüstü bilgisayarın içinden yükleme](install-packages-jupyter-notebook.md)
- [Nasıl kullanılır: Veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Nasıl kullanılır: Veri kaynaklarına erişin](access-data-resources-jupyter-notebooks.md)
