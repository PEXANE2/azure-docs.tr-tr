---
title: Azure not defterleri ile Jupyter not defteri github'dan kopyalama
description: Hızla bir GitHub deposundan bir Jupyter not defterine kopyalayın ve Azure not defterleri hesabınızdaki çalıştırın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: 500bd3f85409bb2f5e7b73be0478694695cc9c00
ms.sourcegitcommit: 653e9f61b24940561061bd65b2486e232e41ead4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74277522"
---
# <a name="quickstart-clone-a-notebook"></a>Hızlı Başlangıç: Kopya bir not defteri

Birçok veri bilimcileri ve geliştiricileri, not defterlerini birçok farklı proje türü için depolama ve sürüm denetimi sağlayan ücretsiz bir hizmet olan [GitHub depolarında](https://github.com)depolar. GitHub, genellikle yerel olarak çalıştırılan Jupyter notebooks işbirliği olarak kullanılır. Böyle durumlarda, her bir ortak çalışanı depoyu yerel bir kopyasını tutar ve Not Defterleri bu kopyasından çalışır.

Kopyalama kopyasını bir GitHub Not Defteri, bunun yerine Azure not defterleri hesabınızdaki oluşturur. Bu kopyası, özgün depodan bağımsızdır; değişiklikler yalnızca Azure not defterleri hesabınızda depolanan ve özgün etkilemez. Bulutta, kopya olduğu için proje değil herhangi bir yerel kopyalarını veya hatta Jupyter kendi bilgisayarlarında yüklü olan diğer ortak çalışanlar ile paylaşabilirsiniz. Ayrıca, yalnızca bir başlangıç noktası kendi veya veri dosyalarını almak için olarak bir proje için bir not defteri kopyalama.

## <a name="clone-azure-cognitive-services-notebooks"></a>Azure Bilişsel hizmetler not defterlerini kopyalama

1. [Azure Notebooks](https://notebooks.azure.com) gidin ve oturum açın. (Ayrıntılar için bkz. [hızlı başlangıç-Azure Notebooks Için oturum açma](quickstart-sign-in-azure-notebooks.md)).

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
> [Öğretici: doğrusal regresyon yapmak için bir Jupyter Not defteri çalıştırma](tutorial-create-run-jupyter-notebook.md)
