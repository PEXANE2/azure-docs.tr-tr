---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: f1553a5c9d55366b2764877b48d0606ff8e0b370
ms.sourcegitcommit: 5d6ce6dceaf883dbafeb44517ff3df5cd153f929
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/29/2020
ms.locfileid: "76842186"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Bu bölümde, Azure aboneliğinizde bir işlev uygulaması ve ilgili kaynaklar oluşturup kodunuzu dağıtırsınız. 

1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında, **işlev uygulamasına dağıt uygulama...** düğmesini seçin.

    ![Projenizi Azure 'da yayımlayın](media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlere aşağıdaki bilgileri sağlayın:

    ::: zone pivot="programming-language-csharp,programming-language-powershell"

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----- |
    | Abonelik seçme | Aboneliğiniz | Birden çok aboneliğiniz olduğunda gösterilir. |
    | Azure 'da İşlev Uygulaması seçin | + Yeni İşlev Uygulaması Oluştur | Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır. |
    | İşlev uygulaması için genel olarak benzersiz bir ad girin | Benzersiz ad | İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir. |
    | Yeni kaynaklar için bir konum seçin | Bölge | Size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. | 

    ::: zone-end

    ::: zone pivot="programming-language-javascript,programming-language-typescript,programming-language-python"

    | İstem | Değer | Açıklama |
    | ------ | ----- | ----- |
    | Abonelik seçme | Aboneliğiniz | Birden çok aboneliğiniz olduğunda gösterilir. |
    | Azure 'da İşlev Uygulaması seçin | + Yeni İşlev Uygulaması Oluştur | Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır. |
    | İşlev uygulaması için genel olarak benzersiz bir ad girin | Benzersiz ad | İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir. |
    | Çalışma zamanı seçin | Sürümünüz | Yerel olarak çalıştırdığınız dil sürümünü seçin. |
    | Yeni kaynaklar için bir konum seçin | Bölge | Size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. | 

    ::: zone-end

    
1.  Tamamlandığında, aboneliğinizde aşağıdaki Azure kaynakları oluşturulur:

    + **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** : oluşturulan tüm Azure kaynaklarını içerir. Ad, işlev uygulamanızın adını temel alır.
    + **[Depolama hesabı](../articles//storage/common/storage-introduction.md#types-of-storage-accounts)** : işlev uygulamanızın adını temel alan benzersiz bir adla standart bir depolama hesabı oluşturulur.
    + **[Barındırma planı](../articles/azure-functions/functions-scale.md)** : sunucusuz işlev uygulamanızı barındırmak için Batı ABD bölgede bir tüketim planı oluşturulur.
    + **İşlev uygulaması**: projeniz bu yeni işlev uygulamasında dağıtılır ve çalışır.
    + **[Application Insights]()** : işlev uygulamanıza bağlı olan bir örnek, işlev adınız temel alınarak oluşturulur.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. 
    
1. Oluşturduğunuz Azure kaynakları dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **çıktıyı görüntüle** ' yi seçin.

    ![Tüm bildirimi oluştur](media/functions-publish-project-vscode/function-create-notifications.png)

1. Yan çubuktaki **Azure: Functions** bölümüne geri döndüğünüzde, aboneliğinizin altındaki yeni işlev uygulamasını genişletin. **İşlevler**' i genişletin, **httpexample**üzerinde sağ tıklayın (Windows) veya CTRL + tıklama (MacOS) seçeneğini belirleyin ve ardından **işlev URL 'sini Kopyala**' yı seçin.

    ![Yeni HTTP tetikleyicisinin işlev URL 'sini kopyalayın](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
