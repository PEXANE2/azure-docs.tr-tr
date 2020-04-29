---
title: Azure Notebooks Preview ile bir Jupyter Not defterini GitHub 'dan kopyalama
description: Bir GitHub deposundan hızlı bir şekilde Jupyter Not defterini kopyalayın ve Azure Notebooks hesabınızda çalıştırın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: d0f3a12ff04e115074c3821c5e29652484710bca
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77064606"
---
# <a name="quickstart-clone-a-notebook-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Notebooks önizlemede bir not defteri kopyalama

Bu hızlı başlangıçta, GitHub ' da depolanan bir Jupyter Not defterini Azure Notebooks hesabına kopyalayabilirsiniz. 

GitHub depoları, Jupyıter Not defterleri için depolama ve sürüm denetimi sağlar. Ortak çalışanlar depoların yerel kopyalarını korur ve not defterlerini bu kopyalardan çalıştırır. Bir Jupyter Not defterini GitHub 'dan Azure Notebooks hesabınıza kopyalama, Not defterinin bağımsız bir kopyasını oluşturur. Değişiklikler yalnızca Azure Notebooks hesabınızda depolanır ve özgün GitHub deposunu etkilemez. 

Azure Notebooks kopya bulutta olduğundan, bu dosyayı yerel kopya yapmadan veya bilgisayarlarında Jupyıter 'ın yüklü olması gereken ortak çalışanlarla paylaşabilirsiniz. Ayrıca, bir not defterini yalnızca kendi projesi için bir başlangıç noktası olarak kopyalayabilir veya veri dosyalarını elde edebilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
Yok.

## <a name="clone-azure-cognitive-services-notebooks"></a>Azure bilişsel hizmetler not defterlerini Kopyala

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projelerim tarayıcı penceresinin üst kısmında bağlantı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında, yukarı ok düğmesini (klavye kısayolu: U; tarayıcı penceresi yeterince geniş olduğunda **GitHub deposu karşıya yükle** olarak görünür) seçin:

    ![Projelerim sayfasında GitHub deposu komutunu karşıya yükle](media/quickstarts/upload-github-repo-command.png)

1. Görüntülenen **GitHub deposunu karşıya yükle** ' de, aşağıdaki ayrıntıları girin veya ayarlayın ve ardından **içeri aktar**' ı seçin:

   - **GitHub deposu**: Microsoft/bilişsel hizmetler-not defterleri (Bu ad, Azure bilişsel Hizmetleri Için Jupyter Not [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks)defterlerini klonlar).
   - **Yinelemeli olarak Kopyala**: (işaretsiz)
   - **Proje adı**: bilişsel hizmetler kopyası
   - **Proje kimliği**: bilişsel hizmetler-Kopyala
   - **Genel**: (işaretsiz)

     ![Depo bilgilerini toplamak için GitHub deposu yükleme açılan penceresini karşıya yükleyin](media/quickstarts/upload-github-repo-popup.png)

1. İşlem tamamlanırken sabırlı olun; bir depoyu kopyalamak birkaç dakika sürebilir.

1. Kopyalama işlemi tamamlandıktan sonra, Azure Notebooks tüm dosyaların kopyalarını görebileceğiniz yeni projeye götürür.

    [![](media/quickstarts/completed-clone.png "View of a completed clone")](media/quickstarts/completed-clone.png#lightbox)

## <a name="share-a-notebook"></a>Bir not defteri paylaşma

1. Klonlanan proje kopyanızı paylaşmak için, **paylaşma** denetimini kullanın veya bir bağlantı alın, bağlantıyı içeren HTML veya markı kodu alın veya şu bağlantıyla bir e-posta iletisi oluşturun:

    ![Proje paylaşma komutu](media/quickstarts/share-project-command.png)

1. Projeyi kopyalarken **ortak** seçeneği temizlediğiniz için kopya özeldir. Kopyanızı ortak hale getirmek için **proje ayarları**' nı seçin, açılan pencerede **ortak proje** seçeneğini ayarlayın ve ardından **Kaydet**' i seçin.

1. Projede çalıştırmak için bir not defteri seçin. Örneğin, Azure bilişsel hizmetler deposundaki her bir not defteri, kendi kendine dahil olan kendi hızlı başlangıçsıdır. Aşağıdaki görüntüde, bir bilişsel hizmetler API 'SI abonelik anahtarı eklendikten ve "Puppies" arama terimini "bies" olarak değiştirerek Bingimagesearchapı Not defterinin kullanılması sonucu gösterilmektedir:

    ![GitHub 'dan kopyalanmış Jupyter Not defterini çalıştırma](media/quickstarts/clone-notebook-result.png)

1. Not defterini çalıştırmayı tamamladığınızda, **Dosya** > **Kapat** ' ı ve sonra da Durdur ' u seçerek not defterini ve tarayıcı penceresini kapatın.

1. Projede tek bir not defterini paylaşmak için not defterine sağ tıklayıp **Bağlantıyı Kopyala** ' yı seçin (klavye kısayolu: y):

    ![Tek bir not defterine bağlantı kopyalamak için bağlam menüsü komutu](media/quickstarts/copy-link-to-individual-notebook.png)

1. Not defterleri dışındaki dosyaları düzenlemek için, projedeki dosyaya sağ tıklayın ve **dosyayı Düzenle** ' yi seçin (klavye kısayolu: ı). Varsayılan eylem, **Çalıştır** (klavye kısayolu: r) yalnızca dosya içeriğini gösterir ve düzenlenmesine izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri oluşturma ve çalıştırma](tutorial-create-run-jupyter-notebook.md)
