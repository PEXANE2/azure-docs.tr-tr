---
title: Azure Dizüstü Bilgisayarlar Önizleme ile GitHub'dan bir Jupyter dizüstü bilgisayar klonlama
description: Bir Jupyter dizüstü bilgisayarını GitHub deposundan hızla klonla ve Azure Not Defterleri hesabınızda çalıştırın.
ms.topic: quickstart
ms.date: 12/04/2018
ms.openlocfilehash: d0f3a12ff04e115074c3821c5e29652484710bca
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77064606"
---
# <a name="quickstart-clone-a-notebook-in-azure-notebooks-preview"></a>Hızlı başlangıç: Azure Dizüstü Bilgisayarlar Önizleme'de not defterini klonlama

Bu hızlı başlangıçta, GitHub'da depolanan bir Jupyter not defterini Azure Dizüstü Bilgisayarlar hesabına kopyalarsınız. 

GitHub depoları Jupyter dizüstü bilgisayarlar için depolama ve sürüm denetimi sağlar. Ortak çalışanlar depoların yerel kopyalarını korur ve not defterlerini bu kopyalardan çalıştırın. GitHub'dan Bir Jupyter not defterini Azure Notebook hesabınıza klonlamak, not defterinin bağımsız bir kopyasını oluşturur. Değişiklikler yalnızca Azure Not Defterleri hesabınızda depolanır ve özgün GitHub deposunu etkilemez. 

Azure Not Defterleri klonunuz bulutta olduğundan, bunu yerel kopya yapmaya veya bilgisayarlarına Jupyter yüklemesi gerekmeyen ortak çalışanlarla paylaşabilirsiniz. Ayrıca, yalnızca kendi projeniz için bir başlangıç noktası olarak veya veri dosyaları elde etmek için bir not defterini klonlaabilirsiniz. 

## <a name="prerequisites"></a>Ön koşullar
Yok.

## <a name="clone-azure-cognitive-services-notebooks"></a>Azure Bilişsel Hizmetleri dizüstü bilgisayarlarını klonla

[!INCLUDE [notebooks-status](../../includes/notebooks-status.md)]

1. Azure [Not Defterleri'ne](https://notebooks.azure.com) gidin ve oturum açın. Ayrıntılar için [Quickstart - Azure Not Defterlerinde oturum açın.](quickstart-sign-in-azure-notebooks.md)

1. Herkese açık profil sayfanızdan, sayfanın üst kısmında **Projelerim'i** seçin:

    ![Tarayıcı penceresinin üst kısmındaki Projelerim bağlantısı](media/quickstarts/my-projects-link.png)

1. **Projelerim** sayfasında yukarı ok düğmesini seçin (klavye kısayolu: U; tarayıcı penceresi yeterince geniş olduğunda düğme **GitHub Repo Yükle** olarak görünür):

    ![Projelerim sayfasına GitHub Repo komutunu yükleyin](media/quickstarts/upload-github-repo-command.png)

1. Aşağıdaki ayrıntıları görünen, giren veya ayarlayan **Upload GitHub Deposu'nda** **Içe Aktar'ı**seçin:

   - **GitHub deposu**: Microsoft/cognitive-services-notebook (bu ad, Azure Bilişsel Hizmetler için Jupyter dizüstü bilgisayarlarını [https://github.com/Microsoft/cognitive-services-notebooks](https://github.com/Microsoft/cognitive-services-notebooks)klonlar).
   - **Klon özyinelemeli:**(temizlenmiş)
   - **Proje adı**: Bilişsel Hizmetler Klonu
   - **Proje Kimliği**: bilişsel-hizmetler-klon
   - **Genel**: (temizlendi)

     ![Repo bilgilerini toplamak için GitHub Repo açılır penceresini yükleyin](media/quickstarts/upload-github-repo-popup.png)

1. İşlem tamamlarken sabırlı olun; bir depo klonlama birkaç dakika sürebilir.

1. Klonlama tamamlandıktan sonra Azure Not Defterleri sizi tüm dosyaların kopyalarını görebileceğiniz yeni projeye götürür.

    [![](media/quickstarts/completed-clone.png "View of a completed clone")](media/quickstarts/completed-clone.png#lightbox)

## <a name="share-a-notebook"></a>Not defterini paylaşma

1. Klonlanan projenin kopyasını paylaşmak **için, Paylaşım** denetimini kullanın veya bir bağlantı edinin, bağlantıyı içeren HTML veya Markdown kodu edinin veya bağlantıyla bir e-posta iletisi oluşturun:

    ![Proje payı komutu](media/quickstarts/share-project-command.png)

1. Projeyi klonlarken **Genel** seçeneği temizlediğiniz için, klon özeldir. Kopyanızı herkese açık hale getirmek için **Proje Ayarları'nı**seçin, açılır pencerede **Herkese Açık proje** seçeneğini ayarlayın ve ardından **Kaydet'i**seçin.

1. Çalıştırmak için projede bir not defteri seçin. Örneğin, Azure Bilişsel Hizmetler deposundaki her dizüstü bilgisayarın kendi kendine yeten Quickstart'ı vardır. Aşağıdaki resim, Bilişsel Hizmetler API abonelik anahtarı nı ekledikten ve "yavru" arama terimini "bunnies" olarak değiştirdikten sonra BingImageSearchAPI not defterini kullanmanın sonucunu gösterir:

    ![GitHub'dan klonlanmış Jupyter dizüstü bilgisayar çalıştırma](media/quickstarts/clone-notebook-result.png)

1. Not defterini çalıştırmayı bitirdiğinizde, **Dosya** > **Kapat'ı** seçin ve not defterini ve tarayıcı penceresini kapatmak için durun.

1. Projede tek bir not defterini paylaşmak için not defterine sağ tıklayın ve **Kopyala bağlantısını** seçin (klavye kısayolu: y):

    ![Tek bir not defterine bağlantı kopyalamak için bağlam menüsü komutu](media/quickstarts/copy-link-to-individual-notebook.png)

1. Not defteri dışındaki dosyaları yeniden deletmek için projedeki dosyayı sağ tıklatın ve **Dosyayı Edit'i** seçin (klavye kısayolu: i). Varsayılan eylem, **Çalıştır** (klavye kısayolu: r), yalnızca dosya içeriğini gösterir ve düzenlemeye izin vermez.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Öğretici: Doğrusal regresyon yapmak için bir Jupyter dizüstü bilgisayar oluşturun ve çalıştırın](tutorial-create-run-jupyter-notebook.md)
