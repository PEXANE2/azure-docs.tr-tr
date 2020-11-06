---
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/28/2020
ms.author: glenga
ms.openlocfilehash: a525d1e14e642a64235c263ba29bf7a181bf9e30
ms.sourcegitcommit: 7cc10b9c3c12c97a2903d01293e42e442f8ac751
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/06/2020
ms.locfileid: "93420997"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Bu bölümde, Azure aboneliğinizde bir işlev uygulaması ve ilgili kaynaklar oluşturup kodunuzu dağıtırsınız.

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır.


1. Etkinlik çubuğunda Azure simgesini seçin, sonra **Azure: işlevler** alanında, **işlev uygulamasına dağıt uygulama...** düğmesini seçin.

    ![Projenizi Azure 'da yayımlayın](media/functions-publish-project-vscode/function-app-publish-project.png)

1. İstemlere aşağıdaki bilgileri sağlayın:

    - **Klasör seçin** : çalışma alanınızdan bir klasör seçin veya işlev uygulamanızı içeren bir klasöre gidin. Zaten açılmış geçerli bir işlev uygulamanız varsa bunu göremezsiniz.

    - **Abonelik seçin** : kullanılacak aboneliği seçin. Yalnızca bir aboneliğiniz varsa bunu göremezsiniz.

    - **Azure 'da işlev uygulaması seçin** : Seç `- Create new Function App` . ( `Advanced` Bu makalede kapsanmayan seçeneğini seçmeyin.)
      
    - **İşlev uygulaması için genel olarak benzersiz bir ad girin** : URL yolunda geçerli olan bir ad yazın. Yazdığınız ad, Azure Işlevlerinde benzersiz olduğundan emin olmak için onaylanır.
    
    - **Yeni kaynaklar için bir konum seçin** : daha iyi performans için, size yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. 
    
1.  Tamamlandığında, aşağıdaki Azure kaynakları, işlev uygulamanızın adına göre adlar kullanılarak aboneliğinizde oluşturulur:
    
    - İlgili kaynaklar için mantıksal kapsayıcı olan bir kaynak grubu.
    - Projeleriniz hakkında durum ve diğer bilgileri tutan standart bir Azure depolama hesabı.
    - Sunucusuz işlev uygulamanız için temel Konağı tanımlayan bir tüketim planı. 
    - İşlev kodunuzu yürütmek için ortam sağlayan bir işlev uygulaması. İşlev uygulaması, aynı barındırma planı içindeki kaynakların daha kolay yönetilmesi, dağıtılması ve paylaşılması için işlevleri bir mantıksal birim olarak gruplandırmanıza olanak tanır.
    - Sunucusuz işlevinizin kullanımını izleyen işlev uygulamasına bağlı bir Application Insights örneği.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. 
    
1. Oluşturduğunuz Azure kaynakları dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **çıktıyı görüntüle** ' yi seçin. Bildirimi kaçırırsanız, sağ alt köşedeki zil simgesini seçerek tekrar görüntüleyin.

    ![Tüm bildirimi oluştur](media/functions-publish-project-vscode/function-create-notifications.png)
