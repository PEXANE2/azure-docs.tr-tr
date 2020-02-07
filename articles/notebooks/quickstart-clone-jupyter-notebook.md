---
title: Azure Notebooks Preview ile bir Jupyter Not defterini GitHub 'dan kopyalama
description: Hızla bir GitHub deposundan bir Jupyter not defterine kopyalayın ve Azure not defterleri hesabınızdaki çalıştırın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: d0f3a12ff04e115074c3821c5e29652484710bca
ms.sourcegitcommit: db2d402883035150f4f89d94ef79219b1604c5ba
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/07/2020
ms.locfileid: "77064606"
---
# <a name="quickstart-clone-a-notebook-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Notebooks önizlemede bir not defteri kopyalama

Bu hızlı başlangıçta, GitHub ' da depolanan bir Jupyter Not defterini Azure Notebooks hesabına kopyalayabilirsiniz. 

GitHub depoları, Jupyıter Not defterleri için depolama ve sürüm denetimi sağlar. Ortak çalışanlar depoların yerel kopyalarını korur ve not defterlerini bu kopyalardan çalıştırır. Bir Jupyter Not defterini GitHub 'dan Azure Notebooks hesabınıza kopyalama, Not defterinin bağımsız bir kopyasını oluşturur. Değişiklikler yalnızca Azure Notebooks hesabınızda depolanır ve özgün GitHub deposunu etkilemez. 

Azure Notebooks kopya bulutta olduğundan, bu dosyayı yerel kopya yapmadan veya bilgisayarlarında Jupyıter 'ın yüklü olması gereken ortak çalışanlarla paylaşabilirsiniz. Ayrıca, bir not defterini yalnızca kendi projesi için bir başlangıç noktası olarak kopyalayabilir veya veri dosyalarını elde edebilirsiniz. 

## <a name="prerequisites"></a>Önkoşullar
Yok.

## <a name="clone-azure-cognitive-services-notebooks"></a>Azure Bilişsel hizmetler not defterlerini kopyalama

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md).

1. Ortak profil sayfanızda sayfanın en üstündeki **Projelerim** ' nı seçin:

    ![Projeleri bağlantımı tarayıcı penceresinin üst kısmındaki](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında, yukarı ok düğmesini (klavye kısayolu: U; tarayıcı penceresi yeterince geniş olduğunda **GitHub deposu karşıya yükle** olarak görünür) seçin:

    ![Projelerim sayfasında GitHub deposu komutunu karşıya yükle](media/quickstarts/upload-github-repo-command.png)

1. Görüntülenen **GitHub deposunu karşıya yükle** ' de, aşağıdaki ayrıntıları girin veya ayarlayın ve ardından **içeri aktar**' ı seçin:

   - **GitHub deposu**: Microsoft/bilişsel hizmetler-not defterleri (Bu ad, [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks)adresindeki Azure bilişsel hizmetler Için Jupyter not defterlerini klonlar).
   - **Yinelemeli olarak Kopyala**: (işaretsiz)
   - **Proje adı**: bilişsel hizmetler kopyası
   - **Proje kimliği**: bilişsel hizmetler-Kopyala
   - **Genel**: (işaretsiz)

     ![Depo bilgilerini toplamak için GitHub deposu yükleme açılan penceresini karşıya yükleyin](media/quickstarts/upload-github-repo-popup.png)

1. İşlem tamamlanırken arkanıza endişelenmeyin; bir depo kopyalama, birkaç dakika sürebilir.

1. Kopyalama tamamlandıktan sonra Azure not defterleri, yeni projeye görebileceğiniz tüm dosyaların kopyalarını alır.

    [![](media/quickstarts/completed-clone.png "View of a completed clone")](media/quickstarts/completed-clone.png#lightbox)

## <a name="share-a-notebook"></a>Bir not defteri paylaşın

1. Klonlanan proje kopyanızı paylaşmak için, **paylaşma** denetimini kullanın veya bir bağlantı alın, bağlantıyı içeren HTML veya markı kodu alın veya şu bağlantıyla bir e-posta iletisi oluşturun:

    ![Proje paylaşımı komutu](media/quickstarts/share-project-command.png)

1. Projeyi kopyalarken **ortak** seçeneği temizlediğiniz için kopya özeldir. Kopyanızı ortak hale getirmek için **proje ayarları**' nı seçin, açılan pencerede **ortak proje** seçeneğini ayarlayın ve ardından **Kaydet**' i seçin.

1. Bir not defteri çalıştırmak için projeyi seçin. Örneğin, Azure Bilişsel hizmetler deposundaki her bir not defteri kendi müstakil Hızlı Başlangıç ' dir. Aşağıdaki resimde bir Bilişsel hizmetler API'si abonelik anahtarı ekleme ve "bunnies" için arama terimi "Yavru köpekler" değiştirme sonra BingImageSearchAPI not defteri kullanmanın sonucunu göstermektedir:

    ![Github'dan kopyaladığınız Jupyter not defteri çalıştırma](media/quickstarts/clone-notebook-result.png)

1. Not defterini çalıştırmayı tamamladıktan sonra **dosya** > Kapat ' ı seçin **ve** sonra da Durdur ' u ve tarayıcı penceresini kapatın.

1. Projede tek bir not defterini paylaşmak için not defterine sağ tıklayıp **Bağlantıyı Kopyala** ' yı seçin (klavye kısayolu: y):

    ![Bağlantı için ayrı bir not defteri kopyalamak için bağlam menüsü komutu](media/quickstarts/copy-link-to-individual-notebook.png)

1. Not defterleri dışındaki dosyaları düzenlemek için, projedeki dosyaya sağ tıklayın ve **dosyayı Düzenle** ' yi seçin (klavye kısayolu: ı). Varsayılan eylem, **Çalıştır** (klavye kısayolu: r) yalnızca dosya içeriğini gösterir ve düzenlenmesine izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri oluşturma ve çalıştırma](tutorial-create-run-jupyter-notebook.md)
