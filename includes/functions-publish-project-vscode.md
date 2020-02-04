---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: 8c63d314c253152d2815a70831870fe331071c68
ms.sourcegitcommit: 42517355cc32890b1686de996c7913c98634e348
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/02/2020
ms.locfileid: "76964164"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Bu bölümde, Azure aboneliğinizde bir işlev uygulaması ve ilgili kaynaklar oluşturup kodunuzu dağıtırsınız. 

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında, **işlev uygulamasına dağıt uygulama...** düğmesini seçin.

    ![Projenizi Azure 'da yayımlayın](media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlere aşağıdaki bilgileri sağlayın:

    + **Abonelik seçin**: kullanılacak aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu göremezsiniz.

    + **Azure 'da işlev uygulaması seçin**: `+ Create new Function App` (`Advanced`değil) seçeneğini belirleyin. Bu makale, [Gelişmiş yayımlama akışını](../articles/azure-functions/functions-develop-vs-code.md#enable-publishing-with-advanced-create-options)desteklemez. 
    
    >[!IMPORTANT]
    > Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır. 
    
    + **İşlev uygulaması için genel olarak benzersiz bir ad girin**: URL yolunda geçerli olan bir ad yazın. Yazdığınız ad, Azure Işlevlerinde benzersiz olduğundan emin olmak için onaylanır. 
    
    ::: zone pivot="programming-language-python"
    + **Çalışma zamanı seçin**: yerel olarak çalıştırdığınız Python sürümünü seçin. Sürümünüzü denetlemek için `python --version` komutunu kullanabilirsiniz.
    ::: zone-end

    ::: zone pivot="programming-language-javascript,programming-language-typescript"
    + **Çalışma zamanı seçin**: yerel olarak çalıştırdığınız Node. js sürümünü seçin. Sürümünüzü denetlemek için `node --version` komutunu kullanabilirsiniz.
    ::: zone-end

    + **Yeni kaynaklar için bir konum seçin**: daha iyi performans için, size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. 
    
1.  Tamamlandığında, aboneliğinizde aşağıdaki Azure kaynakları oluşturulur:

    + **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** : oluşturulan tüm Azure kaynaklarını içerir. Ad, işlev uygulamanızın adını temel alır.
    + **[Depolama hesabı](../articles//storage/common/storage-introduction.md#types-of-storage-accounts)** : işlev uygulamanızın adını temel alan benzersiz bir adla standart bir depolama hesabı oluşturulur.
    + **[Barındırma planı](../articles/azure-functions/functions-scale.md)** : sunucusuz işlev uygulamanızı barındırmak için Batı ABD bölgede bir tüketim planı oluşturulur.
    + **İşlev uygulaması**: projeniz bu yeni işlev uygulamasında dağıtılır ve çalışır.
    + **[Application Insights]()** : işlev uygulamanıza bağlı olan bir örnek, işlev adınız temel alınarak oluşturulur.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. 
    
1. Oluşturduğunuz Azure kaynakları dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **çıktıyı görüntüle** ' yi seçin. Bildirimi kaçırırsanız, sağ alt köşedeki zil simgesini seçerek tekrar görüntüleyin.

    ![Tüm bildirimi oluştur](media/functions-publish-project-vscode/function-create-notifications.png)
