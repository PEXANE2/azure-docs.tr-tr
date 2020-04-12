---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 01/12/2020
ms.author: glenga
ms.openlocfilehash: d8665b4cec3357baee5d6c1b77b5719645575419
ms.sourcegitcommit: ae3d707f1fe68ba5d7d206be1ca82958f12751e8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/10/2020
ms.locfileid: "81112845"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Bu bölümde, Azure aboneliğinizde bir işlev uygulaması ve ilgili kaynaklar oluşturur ve ardından kodunuzu dağıtırsınız. 

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır. 


1. Etkinlik çubuğundaki Azure simgesini, ardından **Azure: İşlevler** alanında' seçeneğini belirleyin, **işlev uygulaması için Dağıt düğmesini seçin...** düğmesini seçin.

    ![Projenizi Azure'da yayımlayın](media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlerde aşağıdaki bilgileri sağlayın:

    + **Aboneliği seçin**: Kullanmak için aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu görmezsiniz.

    + **Azure'da İşlev** `+ Create new Function App`Uygulamasını Seçin : Seçin. (Bu makalede yer `Advanced` almayan seçeneği seçmeyin.)
      
    + **İşlev uygulaması için genel olarak benzersiz bir ad girin**: URL yolunda geçerli olan bir ad yazın. Yazdığınız ad, Azure İşlevlerinde benzersiz olduğundan emin olmak için doğrulanır. 
    
    ::: zone pivot="programming-language-python"
    + **Çalışma zamanı seçin**: Yerel olarak çalıştırdığınız Python sürümünü seçin. Sürümünüzü kontrol `python --version` etmek için komutu kullanabilirsiniz.
    ::: zone-end

    ::: zone pivot="programming-language-javascript,programming-language-typescript"
    + **Çalışma zamanı seçin**: Yerel olarak çalıştırdığınız Node.js sürümünü seçin. Sürümünüzü kontrol `node --version` etmek için komutu kullanabilirsiniz.
    ::: zone-end

    + **Yeni kaynaklar için bir konum seçin**: Daha iyi performans için size en yakın [bölgeyi](https://azure.microsoft.com/regions/) seçin. 
    
1.  Tamamlandığında, işlev uygulama adınıza göre adlar kullanılarak aboneliğinizde aşağıdaki Azure kaynakları oluşturulur:
    
    + İlgili kaynaklar için mantıksal bir kapsayıcı olan kaynak grubu.
    + Projelerinizle ilgili durumu ve diğer bilgileri koruyan standart bir Azure Depolama hesabı.
    + Sunucusuz işlev uygulamanızın temel ana bilgisayarını tanımlayan bir tüketim planı. 
    + İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. İşlev uygulaması, işlevleri aynı barındırma planı içinde kaynakların daha kolay yönetimi, dağıtımı ve paylaşımı için mantıksal bir birim olarak gruplandırmanıza olanak tanır.
    + Sunucusuz işlevinizin kullanımını izleyen işlev uygulamasına bağlı bir Application Insights örneği.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. 
    
1. Oluşturduğunuz Azure kaynakları da dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **Çıktıyı** Görüntüle'yi seçin. Bildirimi kaçırırsanız, tekrar görmek için sağ alt köşedeki çan simgesini seçin.

    ![Tam bildirim oluşturma](media/functions-publish-project-vscode/function-create-notifications.png)
