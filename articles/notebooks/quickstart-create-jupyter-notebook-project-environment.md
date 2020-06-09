---
title: Özel bir ortamla Azure Notebooks önizleme projesi oluşturma
description: Azure Notebooks önizlemede, belirli bir yüklü paketler ve başlatma betikleri kümesiyle yapılandırılmış yeni bir proje oluşturun.
ms.topic: quickstart
ms.date: 12/04/2018
ms.custom: tracking-python
ms.openlocfilehash: 03935b9672c25db11b91733fac1c63f1f5978b87
ms.sourcegitcommit: 964af22b530263bb17fff94fd859321d37745d13
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84559741"
---
# <a name="quickstart-create-a-project-with-a-custom-environment-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Notebooks önizlemede özel ortamla bir proje oluşturma

Azure Notebooks bir proje, belirli kurulum komutlarıyla yapılandırılabilecek bir ortamla birlikte Not defterleri, veri dosyaları, belgeler, resimler gibi bir dosya koleksiyonudur. Projeyi proje ile tanımlayarak, projeyi kendi Azure Notebooks olarak klonlayan herkes gerekli ortamı yeniden oluşturmak için gereken tüm bilgileri içerir.

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

## <a name="create-a-project"></a>Proje oluşturma

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md)).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projelerim tarayıcı penceresinin üst kısmında bağlantı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında **+ Yeni proje** ' yi seçin (klavye kısayolu: n); düğme yalnızca **+** tarayıcı penceresi dar gibi görünebilir:

    ![Projelerim sayfasında yeni proje komutu](media/quickstarts/new-project-command.png)

1. Görüntülenen **Yeni proje oluştur** açılır penceresinde aşağıdaki ayrıntıları girin veya ayarlayın, sonra **Oluştur**' u seçin.

    - **Proje adı**: özel bir ortama sahip proje
    - **Proje kimliği**: proje-özel-ortam
    - **Ortak proje**: (işaretsiz)
    - **README.MD oluşturma**: (işaretsiz)

1. Birkaç dakika sonra Azure Notebooks yeni projeye götürür. **+ Yeni** açılan listesini (yalnızca olarak görünebilir **+** ) seçip **Not defteri**' ni seçerek projeye bir not defteri ekleyin.

1. Not defterine özel ortam gibi bir ad verin *. ipynb*, dil için **Python 3,6** ' i seçin ve **Yeni**' yi seçin.

## <a name="add-a-custom-setup-step"></a>Özel Kurulum adımı ekleme

1. Proje sayfasında **proje ayarları**' nı seçin.

    ![Proje ayarları komutu](media/quickstarts/project-settings-command.png)

1. **Proje ayarları** açılır penceresinde **ortam** sekmesini seçin ve ardından **ortam kurulum adımları**altında **+ Ekle**' yi seçin:

    ![Yeni ortam Kurulum adımı ekleme komutu](media/quickstarts/environment-add-command.png)

1. **+ Add** komutu, bir işlem tarafından tanımlanan bir adım ve projenizdeki dosyalardan seçilen bir hedef dosya oluşturur. Aşağıdaki işlemler desteklenir:

   | Çalışma | Description |
   | --- | --- |
   | Requirements. txt | Python projeleri, bağımlılıklarını bir requirements. txt dosyasında tanımlar. Bu seçenekle, projenin dosya listesinden uygun dosyayı seçin ve görüntülenen ek açılan listede Python sürümünü de seçin. Gerekirse, projeye geri dönmek için **iptal** ' i seçin, dosyayı karşıya yükleyin veya oluşturun, ardından **proje ayarları**  >  **ortamı** sekmesine dönüp yeni bir adım oluşturun. Bu adımla birlikte, projede bir not defterini çalıştırmak otomatik olarak çalıştırılır`pip install -r <file>` |
   | Kabuk betiği | Ortamı başlatmak için çalıştırmak istediğiniz komutları içeren bir bash kabuğu betiğini (genellikle *. sh* uzantılı bir dosya) göstermek için kullanın. |
   | Environment. yıml | Bir ortamı yönetmek için Conda kullanan bir Python projesi, bağımlılıkları tanımlayacak *ortamlar. yıml* dosyasını kullanır. Bu seçenekle, projenin dosya listesinden uygun dosyayı seçin. |

   > [!WARNING]
   > Bu bir önizleme hizmeti geliştirildiği `Environment.yml` için, ayarın projenize beklendiği şekilde uygulanamadığı bilinen bir sorun vardır. İçindeki proje ve jupi Not defterleri, belirtilen ortam dosyasını yok olarak yüklemez.

1. Herhangi bir kurulum adımını kaldırmak için adımın sağındaki **X** seçeneğini belirleyin.

1. Tüm kurulum adımları yerinde olduğunda, **Kaydet**' i seçin. (Değişiklikleri atmak için **iptal** ' i seçin).

1. Ortamınızı test etmek için yeni bir not defteri oluşturup çalıştırın, sonra, bir Python deyimi kullanma gibi, ortamdaki bir pakete bağlı olan deyimlerle bir kod hücresi oluşturun `import` . İfade başarılı olursa, gerekli paket ortama başarıyla yüklendi.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Notebooks projeleri yönetme ve yapılandırma](configure-manage-azure-notebooks-projects.md)

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri çalıştırma](tutorial-create-run-jupyter-notebook.md)
