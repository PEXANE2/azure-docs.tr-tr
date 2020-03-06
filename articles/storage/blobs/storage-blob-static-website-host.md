---
title: 'Öğretici: Blob storage - Azure depolama üzerinde statik Web sitesi barındırma'
description: Statik Web sitesi barındırmak için bir depolama hesabı yapılandırma hakkında bilgi edinin ve Azure depolama için statik bir Web sitesi dağıtın.
author: normesta
ms.service: storage
ms.subservice: blobs
ms.topic: tutorial
ms.date: 1/22/2020
ms.author: normesta
ms.reviewer: dineshm
ms.openlocfilehash: 03850315a05f569d2c6ba9405b6ec38bb6b1305d
ms.sourcegitcommit: 021ccbbd42dea64d45d4129d70fff5148a1759fd
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2020
ms.locfileid: "78330404"
---
<!---Customer intent: I want to host files for a static website in Blob storage and access the website from an Azure endpoint.--->

# <a name="tutorial-host-a-static-website-on-blob-storage"></a>Öğretici: Blob Depolama üzerinde statik Web sitesi barındırma

Bu öğreticide, Azure depolama 'ya statik bir Web sitesi derleyip dağıtmayı öğreneceksiniz. İşlemi tamamladığınızda, kullanıcıların herkese açık şekilde erişebileceği statik bir Web sitesi olacaktır. 

Bu öğreticide şunların nasıl yapıldığını öğrenirsiniz:

> [!div class="checklist"]
> * Statik Web sitesi barındırma yapılandırın
> * Hello World Web sitesi dağıtma

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

> [!NOTE] 
> Genel amaçlı v2 standart depolama hesabı oluşturduğunuzdan emin olun. Statik Web siteleri diğer depolama hesabı türleri için kullanılamaz.

Bu öğretici, bir programcılar için ücretsiz bir araç olan [Visual Studio Code](https://code.visualstudio.com/download)kullanır ve statik Web sitesini oluşturup bir Azure depolama hesabına dağıtabilir.

Visual Studio Code'u yükledikten sonra Azure depolama Önizleme uzantıyı yükleyin. Bu uzantı Visual Studio Code ile Azure depolama yönetim işlevini tümleştirir. Uzantı, Azure Depolama'ya statik Web sitenizi dağıtmak için kullanır. Uzantıyı yüklemek için:

1. Visual Studio Code'u başlatın.
2. Araç çubuğunda **Uzantılar**' a tıklayın. *Azure Storage*' ı arayın ve listeden **Azure Storage** uzantısı ' nı seçin. Ardından, uzantıyı yüklemek için **Install** düğmesine tıklayın.

    ![VS Code'da Azure depolama uzantıyı yükleme](media/storage-blob-static-website-host/install-extension-vs-code.png)

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın

Başlamak için [Azure Portal](https://portal.azure.com/) oturum açın.

## <a name="configure-static-website-hosting"></a>Statik Web sitesi barındırma yapılandırın

İlk adım, Azure portalında statik bir Web sitesi barındırmak için depolama hesabınızın yapılandırmaktır. Hesabınızı statik Web sitesi barındırma için yapılandırdığınızda, Azure Storage otomatik olarak *$Web*adlı bir kapsayıcı oluşturur. *$Web* kapsayıcı, statik Web siteniz için dosyaları içerecektir. 

1. [Azure Portal](https://portal.azure.com/) Web tarayıcınızda açın. 
1. Depolama hesabınızı bulun ve hesabına genel bakış görüntüler.
1. Statik Web siteleri için yapılandırma sayfasını göstermek üzere **statik Web sitesi** ' ni seçin.
1. Depolama hesabı için statik Web sitesi barındırmayı etkinleştirmek üzere **etkin** ' i seçin.
1. **Dizin belgesi adı** alanında *index. html*' nin varsayılan dizin sayfasını belirtin. Bir kullanıcı, statik Web sitesi köküne gittiğinde varsayılan dizin sayfası görüntülenir.  
1. **Hata belgesi yolu** alanında, *404. html*' nin varsayılan hata sayfasını belirtin. Statik Web sitenize var olmayan bir sayfaya gitmek bir kullanıcı çalıştığında varsayılan hata sayfası görüntülenir.
1. **Kaydet** düğmesine tıklayın. Azure portalında statik Web sitesi uç noktanızı artık görüntüler. 

    ![Bir depolama hesabı için statik Web sitesi barındırma etkinleştir](media/storage-blob-static-website-host/enable-static-website-hosting.png)

## <a name="deploy-a-hello-world-website"></a>Hello World Web sitesi dağıtma

Ardından, Visual Studio Code ile bir Hello World web sayfası oluşturabilir ve barındırılan Azure depolama hesabınızdaki statik Web sitesi dağıtın.

1. Yerel dosya sisteminizde *mywebsite* adlı boş bir klasör oluşturun. 
1. Visual Studio Code başlatın ve **Gezgin** panelinden yeni oluşturduğunuz klasörü açın.

    ![Visual Studio code'da klasörü aç](media/storage-blob-static-website-host/open-folder-vs-code.png)

1. *Mywebsite* klasöründe varsayılan dizin dosyasını oluşturun ve *index. html*olarak adlandırın.

    ![Visual Studio Code'da varsayılan dizin dosyası oluşturma](media/storage-blob-static-website-host/create-index-file-vs-code.png)

1. Düzenleyicide *index. html* dosyasını açın, dosyaya aşağıdaki metni yapıştırın ve kaydedin:

    ```
    <h1>Hello World!</h1>
    ```

1. Varsayılan hata dosyasını oluşturun ve *404. html*olarak adlandırın.
1. Düzenleyicide *404. html* dosyasını açın, aşağıdaki metni dosyaya yapıştırın ve kaydedin:

    ```
    <h1>404</h1>
    ```

1. **Gezgin** panelinde *mywebsite* klasörü altına sağ tıklayın ve Web sitenizi dağıtmak Için **statik Web sitesine dağıt...** seçeneğini belirleyin. Aboneliklerin listesini almak için Azure'da oturum açarken istenir.

1. Statik Web sitesi barındırma etkinleştirdiğiniz depolama hesabını içeren aboneliği seçin. Ardından, istendiğinde depolama hesabını seçin.

Visual Studio Code şimdi web uç noktanıza dosyalarınızı karşıya yükleyin ve başarı durum çubuğunu göster. Azure'da görüntülemek için Web sitesini başlatın.

Eğitmeni başarıyla tamamladınız ve Azure 'a bir statik Web sitesi dağıttınız.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, statik Web sitesi barındırma için Azure depolama hesabınızı yapılandırmayı ve bir Azure uç noktasına statik bir Web sitesi oluşturmayı ve dağıtmayı öğrendiniz.

Ardından, statik Web siteniz ile özel bir etki alanı yapılandırmayı öğrenin.

> [!div class="nextstepaction"]
> [Özel bir etki alanını Azure Blob depolama uç noktasıyla eşleme](storage-custom-domain-name.md)
