---
title: Jupyter Not Defteri, azure'da bir slayt gösterisi olarak sunmak
description: Slayt gösterisi modu hücre bir Jupyter not defteri yapılandırın ve ardından ortaya çıkmasına neden uzantısını kullanarak slayt gösterisi sunmak nasıl.
ms.topic: article
ms.date: 12/04/2018
ms.openlocfilehash: d180eaf571fa57191e3b0856020b02f05d05e344
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277586"
---
# <a name="run-a-notebook-slideshow"></a>Bir not defteri slayt gösterisi çalıştırın

Azure not defterleri ile Jupyter/Ipython slayt gösterisi uzantısı (bir slayt gösterisi olarak doğrudan bir not defteri göstermenize olanak sağlayan HACMİNDEKİ) önceden yapılandırılmış. Bir slayt gösterisi hücreleri genellikle görüntülenen teker teker büyük ekranlar ve size sunmak kod hala çalıştırabilirsiniz için uygun bir yazı tipi boyutu yerine ayrı bir tanıtım bilgisayara geçiş ' dir.

Aşağıdaki görüntüde, Markdown ve kod hücreleri tümünü bir araya görebileceğiniz standart not defteri görünümü gösterilmektedir:

![Standart görünümünde bir not defteri](media/slideshow/slideshow-notebook-view.png)

Bir slayt gösterisi başlattığınızda, ilk hücre tarayıcıyı dolduracak şekilde genişletilir ve sol üst taraftaki **X** **, slayt** gösterisinin çıkar. alt sol görüntüler slaytlar arasında klavye kısayolları ve sağ alt köşesinde oklar gidin:

![Slayt gösterisi modunda bir not defteri](media/slideshow/slideshow-slide-view.png)

Bir not defteri için slayt gösterisi hazırlama birincil iki etkinlik içerir:

1. Markdown hücreleri büyük yazı tipleriyle işlendiği için bazı içerikleri slayt gösterisi olarak görünmeyebilir. Bu nedenle genellikle belirli bir hücre metinde miktarı sınırı, genellikle dört ile altı çizgili bir üst bilgi en iyi şekilde çalışır. Daha fazla metin varsa, bu bilgileri birden çok hücreye bölün.

2. Her bir hücresinde davranışını slayt gösterisi hücresi araç çubuğunu kullanarak slayt gösterisi olarak yapılandırın. Gezinti düğmelerinin davranışını hücresi türlerini belirleyin.

## <a name="the-anatomy-of-a-slideshow"></a>Slayt gösterisi anatomisi

Rastgele bir not alın ve bunu kullanmak için bir slayt gösterisi, tüm hücreleri birlikte önlenmiş ve içeriğin tarayıcı penceresinin alt kısmındaki gizli genellikle bulun. Etkili bir sunum yapmak, daha sonra bir slayt gösterisi türü slayt gösterisi hücresi araç çubuğunu kullanarak her hücreye atamanız gerekir:

1. **Görünüm** menüsünde **hücre araç çubuğu** > **Slayt**çubuğunu seçin:

    ![Hücre slayt gösterisi araç kapatma](media/slideshow/slideshow-view-cell-toolbar.png)

1. Not defterindeki her hücrenin sağ üst köşesinde bir **slayt türü** açılır listesi görünür:

    ![Hücre slayt gösterisi araç çubuğu](media/slideshow/slideshow-cell-toolbar.png)

1. Her bir hücresinde beş türlerinden birini seçin:

    ![Hücre slayt gösterisi türleri](media/slideshow/slideshow-cell-slide-types.png)

    | Slayt türü | Davranış |
    | --- | --- |
    | -(ayarlanmamış) | Hücre sık istenen bir slayt gösterisi etkili değildir önceki hücrenin görüntülenir. |
    | Slayt | Hücre Gezinti denetimin sol ve sağ okları kullanarak çıkıldığında birincil bir slayttır. |
    | Alt slayt | "Below" Gezinti denetimi aşağı okunu kullanarak gittiğinizde, birincil bir slayt hücre var. Yukarı Ok birincil slayta döndürür. Alt slaytları bir sunu ana yolunda Atla, ancak gerekirse kullanıma hazır ikincil malzeme için kullanılır. |
    | Parça | Hücre içeriğini önceki slayt veya alt slayt bağlamında (bir parça yukarı ok kullanırken kaldırılır) gezinti oku kullanılırken görünür. Birden çok parçaya (sonraki bölümde örneğe bakın) tek bir metin madde işaretleri görünür yapmak için kullanabilirsiniz veya bir parça, kod içinde bir slayt görünür hale getirmek için bir kod hücresi ile kullanabilirsiniz. Geçerli slaytta parçaları oluşturmak için aşırı parçaları tarayıcı penceresinin alt kısmındaki devre dışı görünür olmayacaktır. |
    | Atla | Hücre slayt gösterisi olarak gösterilmez. |
    | Notlar | Slayt gösterisi olarak gösterilmez Konuşmacı notları, hücre içerir. |

1. Başlangıçta, her hücre için **Slayt** seçmek yararlıdır. Ardından, slayt gösterisi çalıştırın ve uygun ayarlamaları yapın.

### <a name="example-fragment-cells-for-bullet-items"></a>Örnek: parça hücreleri maddesi öğeleri için

Bir slaytta yer alan madde işaretlerinin tek tek görünmesi için, slayt başlığını **Slayt** türüyle bir markaşağı hücresine yerleştirip, her bir madde işaretini **parça** türüyle ayrı bir markaşağı hücresine yerleştirin:

![Birden fazla Markdown hücre maddesi öğeleri için oluşturma örneği](media/slideshow/slideshow-fragments.png)

Slayt gösterisi madde işaretleri aynı hücrede olduğunda, daha fazla dikey daha aralığı ile parçalarını işler çünkü çok madde işaretlerini kullanmanız mümkün olmayabilir.

## <a name="run-the-slideshow"></a>Slayt gösterisi çalıştırın

1. Herhangi bir Markaşağı hücresini düzenlediyseniz, bunları HTML olarak işlemek için çalıştırdığınızdan emin olun, aksi takdirde slayt gösterisinde markaşağı *olarak* görünürler.

1. Her hücre için **Slayt türünü** yapılandırdıktan sonra, slayt gösterisinin başlatılacağı hücreyi seçin, sonra ana araç çubuğundaki **Slayt gösterisini gir/çık** düğmesini seçin:

    ![Ana araç çubuğunda girin/çıkış YÜKSELMEYE slayt gösterisi](media/slideshow/slideshow-start.png)

1. Parçaları yanı sıra slaytları arasında gezinmek için sol ve sağ ok Gezinti denetimi olarak kullanın. Denetimdeki metin, *Slide. alt slaytı*temsil eden bir sayı gösterir.

    ![Slayt gösterisi gezinme kontrolü](media/slideshow/slideshow-navigation-control.png)

1. Slayt ve alt slaytları yanı arasında parçalar gezinmek için yukarı ve etkinleştirilirse, oklar aşağı:

    ![Slayt gösterisi gezinti denetimlerinin alt dilimleyiciler için](media/slideshow/slideshow-navigation-control-subslide.png)

1. Kodu hücreyi temel kodu çalıştırmak için Yürüt düğmesini kullanın; Çıktı, slaytta görünür:

    ![Kod hücresini çalıştırmak için YÜRÜT düğmesine](media/slideshow/slideshow-run-code-cell.png)

    ![Kodu hücreyi çıkış slayt gösterisi görünür.](media/slideshow/slideshow-run-code-cell-output.png)

    > [!Tip]
    > Hücre çıkış bir slayt gösterisi hücresinde bir parçası olarak kabul edilir. Not Defteri veya Slayt Gösterisi Görünümü'nde bir hücre çalıştırırsanız, çıkış diğer görünümünde de görüntülenir. Çıktıyı temizlemek için, **Şu anki çıktıları** > **hücreyi** kullanın > **Temizle** komutu (geçerli hücre Için) veya **hücre** > **tüm çıktılar** > **Temizle** (tüm hücreler için).

1. Slayt gösterisine işiniz bittiğinde, **X** ' i kullanarak not defteri görünümüne geri dönün.

## <a name="next-steps"></a>Sonraki adımlar

- [Nasıl yapılır: projeleri yapılandırma ve yönetme](configure-manage-azure-notebooks-projects.md)
- [Nasıl yapılır: bir not defteri içinden paket yüklemesi](install-packages-jupyter-notebook.md)
- [Nasıl yapılır: veri dosyalarıyla çalışma](work-with-project-data-files.md)
- [Nasıl yapılır: veri kaynaklarına erişme](access-data-resources-jupyter-notebooks.md)
