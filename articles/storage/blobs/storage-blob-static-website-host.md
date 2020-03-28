---
title: 'Öğretici: Blob depolamada statik bir web sitesi barındırın - Azure Depolama'
description: Statik web sitesi barındırma için bir depolama hesabını nasıl yapılandıracak ve statik bir web sitesini Azure Depolama'ya nasıl dağıtılayabilirsiniz öğrenin.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: tutorial
ms.date: 1/22/2020
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: 03850315a05f569d2c6ba9405b6ec38bb6b1305d
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78330404"
---
<!---Customer intent: I want to host files for a static website in Blob storage and access the website from an Azure endpoint.--->

# <a name="tutorial-host-a-static-website-on-blob-storage"></a>Öğretici: Blob Depolama'da statik bir web sitesi barındırma

Bu eğitimde, Statik bir web sitesini Azure Depolama'ya nasıl oluşturup dağıtacağınız öğrenilir. İşi bittiğinde, kullanıcıların herkese açık olarak erişebileceği statik bir web siteniz olacaktır. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Statik web sitesi barındırma yapılandırma
> * Hello World web sitesini dağıtın

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

> [!NOTE] 
> Genel amaçlı v2 Standart depolama hesabı oluşturduğunuzdan emin olun. Statik web siteleri başka bir depolama hesabı türünde kullanılamaz.

Bu öğretici, statik web sitesini oluşturmak ve bir Azure Depolama hesabına dağıtmak için programcılar için ücretsiz bir araç olan [Visual Studio Code'u](https://code.visualstudio.com/download)kullanır.

Visual Studio Code'u yükledikten sonra Azure Depolama önizleme uzantısını yükleyin. Bu uzantı, Azure Depolama yönetimi işlevini Visual Studio Code ile tümleştirir. Uzantıyı statik web sitenizi Azure Depolama'ya dağıtmak için kullanırsınız. Uzantıyı yüklemek için:

1. Visual Studio Code'u başlatın.
2. Araç çubuğunda **Uzantılar'ı**tıklatın. Azure *Depolama'yı*arayın ve listeden **Azure Depolama** uzantısını seçin. Ardından uzantıyı yüklemek için **Yükle** düğmesini tıklatın.

    ![Azure Depolama uzantısını VS Kodu'na yükleme](media/storage-blob-static-website-host/install-extension-vs-code.png)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Başlamak için [Azure portalında](https://portal.azure.com/) oturum açın.

## <a name="configure-static-website-hosting"></a>Statik web sitesi barındırma yapılandırma

İlk adım, azure portalında statik bir web sitesi barındıracak şekilde depolama hesabınızı yapılandırmaktır. Statik web sitesi barındırma için hesabınızı yapılandırdığınızda, Azure Depolama otomatik olarak *$web*adında bir kapsayıcı oluşturur. *$web* kapsayıcı statik web sitenizin dosyalarını içerecektir. 

1. Azure [portalını](https://portal.azure.com/) web tarayıcınızda açın. 
1. Depolama hesabınızı bulun ve hesaba genel bakışı görüntüleyin.
1. Statik web **sitelerinin** yapılandırma sayfasını görüntülemek için Statik web sitesini seçin.
1. Depolama hesabı için statik web sitesi barındırmayı etkinleştirmek için **Etkin'i** seçin.
1. **Dizin belge adı** alanında, *index.html*varsayılan dizin sayfası belirtin. Varsayılan dizin sayfası, bir kullanıcı statik web sitenizin köküne gittiğinde görüntülenir.  
1. Hata **belge yolu** *alanında, 404.html*varsayılan hata sayfası belirtin. Varsayılan hata sayfası, kullanıcı statik web sitenizde bulunmayan bir sayfaya gezinmeye çalıştığında görüntülenir.
1. **Kaydet**'e tıklayın. Azure portalı artık statik web sitesi bitiş noktanızı görüntüler. 

    ![Depolama hesabı için statik web sitesi barındırmayı etkinleştirme](media/storage-blob-static-website-host/enable-static-website-hosting.png)

## <a name="deploy-a-hello-world-website"></a>Hello World web sitesini dağıtın

Ardından, Visual Studio Code içeren bir Hello World web sayfası oluşturun ve azure depolama hesabınızda barındırılan statik web sitesine dağıtın.

1. Yerel dosya sisteminizde *web sitem* adlı boş bir klasör oluşturun. 
1. Visual Studio Code'u başlatın ve **Explorer** panelinden oluşturduğunuz klasörü açın.

    ![Visual Studio Code'da klasörü açma](media/storage-blob-static-website-host/open-folder-vs-code.png)

1. Varsayılan dizin dosyasını *web sitesi* klasöründe oluşturun ve *index.html*adını ver.

    ![Visual Studio Code'da varsayılan dizin dosyasını oluşturma](media/storage-blob-static-website-host/create-index-file-vs-code.png)

1. Düzenleyicide *index.html'i* açın, aşağıdaki metni dosyaya yapıştırın ve kaydedin:

    ```
    <h1>Hello World!</h1>
    ```

1. Varsayılan hata dosyasını oluşturun ve *404.html*adını.
1. Editörde *404.html'yi* açın, aşağıdaki metni dosyaya yapıştırın ve kaydedin:

    ```
    <h1>404</h1>
    ```

1. **Explorer** panelindeki *web sitesi* klasörü altında sağ tıklayın ve web sitenizi dağıtmak **için Statik Web Sitesine Dağıt'ı seçin...** Aboneliklerin listesini almak için Azure'da oturum açmanız istenir.

1. Statik web sitesi barındırmaözelliğini etkinleştirdiğiniz depolama hesabını içeren aboneliği seçin. Ardından, istendiğinde depolama hesabını seçin.

Visual Studio Code artık dosyalarınızı web bitiş noktanıza yükler ve başarı durumu çubuğunu gösterir. Web sitesini Azure'da görüntülemek için başlatın.

Öğreticiyi başarıyla tamamladınız ve statik bir web sitesini Azure'a dağıttınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, statik web sitesi barındırma için Azure Depolama hesabınızı nasıl yapılandırabileceğinizi ve statik bir web sitesini Azure bitiş noktasına nasıl oluşturup dağıtabileceğinizi öğrendiniz.

Ardından, statik web sitenizle özel bir etki alanını nasıl yapılandırıştırılamayı öğrenin.

> [!div class="nextstepaction"]
> [Azure Blob Depolama bitiş noktasıyla özel bir etki alanı haritası](storage-custom-domain-name.md)
