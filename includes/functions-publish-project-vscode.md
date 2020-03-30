---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: 256510f855256e648ae9203f46eb9f66c9ffaed6
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "77029155"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Bu bölümde, Azure aboneliğinizde bir işlev uygulaması ve ilgili kaynaklar oluşturur ve ardından kodunuzu dağıtırsınız. 

1. Etkinlik çubuğundaki Azure simgesini, ardından **Azure: İşlevler** alanında' seçeneğini belirleyin, **işlev uygulaması için Dağıt düğmesini seçin...** düğmesini seçin.

    ![Projenizi Azure'da yayımlayın](media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **Aboneliği seçin**: Kullanmak için aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu görmezsiniz.

    + **Azure'da İşlev** `+ Create new Function App` Uygulamasını `Advanced`Seçin : Seçin (değil). Bu makale, gelişmiş [yayımlama akışını](../articles/azure-functions/functions-develop-vs-code.md#enable-publishing-with-advanced-create-options)desteklemez. 
    
    >[!IMPORTANT]
    > Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır. 
    
    + **İşlev uygulaması için genel olarak benzersiz bir ad girin**: URL yolunda geçerli olan bir ad yazın. Yazdığınız ad, Azure İşlevlerinde benzersiz olduğundan emin olmak için doğrulanır. 
    
    ::: zone pivot="programming-language-python"
    + **Çalışma zamanı seçin**: Yerel olarak çalıştırdığınız Python sürümünü seçin. Sürümünüzü kontrol `python --version` etmek için komutu kullanabilirsiniz.
    ::: zone-end

    ::: zone pivot="programming-language-javascript,programming-language-typescript"
    + **Çalışma zamanı seçin**: Yerel olarak çalıştırdığınız Node.js sürümünü seçin. Sürümünüzü kontrol `node --version` etmek için komutu kullanabilirsiniz.
    ::: zone-end

    + **Yeni kaynaklar için bir konum seçin**: Daha iyi performans için size en yakın [bölgeyi](https://azure.microsoft.com/regions/) seçin. 
    
1.  Tamamlandığında, aboneliğinizde aşağıdaki Azure kaynakları oluşturulur:

    + **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)**: Oluşturulan tüm Azure kaynaklarını içerir. Ad, işlev uygulama adınıza bağlıdır.
    + **[Depolama hesabı](../articles//storage/common/storage-introduction.md#types-of-storage-accounts)**: İşlev uygulama adınızı temel alan benzersiz bir adla standart bir Depolama hesabı oluşturulur.
    + **[Barındırma planı](../articles/azure-functions/functions-scale.md)**: Sunucusuz işlev uygulamanızı barındırmak için Batı ABD bölgesinde bir tüketim planı oluşturulur.
    + **Fonksiyon uygulaması**: Projeniz bu yeni işlev uygulamasına dağıtılır ve çalışır.
    + **Uygulama Öngörüleri**: İşlev uygulamanıza bağlı bir örnek, işlev adınıza göre oluşturulur.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. 
    
1. Oluşturduğunuz Azure kaynakları da dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **Çıktıyı** Görüntüle'yi seçin. Bildirimi kaçırırsanız, tekrar görmek için sağ alt köşedeki çan simgesini seçin.

    ![Tam bildirim oluşturma](media/functions-publish-project-vscode/function-create-notifications.png)
