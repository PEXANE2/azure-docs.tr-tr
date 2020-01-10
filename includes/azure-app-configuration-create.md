---
author: yegu
ms.author: yegu
ms.service: azure-app-configuration
ms.topic: include
ms.date: 12/03/2019
ms.openlocfilehash: ceeeb5ee155624e050f36e733a464c2cb21db88c
ms.sourcegitcommit: c32050b936e0ac9db136b05d4d696e92fefdf068
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/08/2020
ms.locfileid: "75750297"
---
1. Yeni bir uygulama yapılandırma deposu oluşturmak için [Azure Portal](https://portal.azure.com)oturum açın. Giriş sayfasının sol üst köşesinde **kaynak oluştur**' u seçin. Market 'te **Ara** kutusuna **uygulama yapılandırması** ' nı girin ve ENTER ' u seçin.

    ![Uygulama yapılandırması ara](media/azure-app-configuration-create/azure-portal-search.png)

1. Arama sonuçlarından **uygulama yapılandırması** ' nı seçin ve ardından **Oluştur**' u seçin.

    ![Oluştur’u seçin](media/azure-app-configuration-create/azure-portal-app-configuration-create.png)

1. **Uygulama yapılandırması** > **Oluştur** bölmesinde, aşağıdaki ayarları girin:

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Uygulama yapılandırma deposu kaynağı için kullanılacak benzersiz bir kaynak adı girin. Ad 1 - 63 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakterini içermelidir. Ad `-` karakteriyle başlayamaz veya bitemez ve ardışık `-` karakterler geçerli değildir.  |
    | **Abonelik** | Aboneliğiniz | Uygulama yapılandırmasını test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa, bu otomatik olarak seçilir ve **abonelik** listesi gösterilmez. |
    | **Kaynak grubu** | *AppConfigTestResources* | Uygulama yapılandırma deposu kaynağınız için bir kaynak grubu seçin veya oluşturun. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğiniz birden çok kaynağı düzenlemek için yararlıdır. Daha fazla bilgi için bkz. [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](/azure/azure-resource-manager/resource-group-overview). |
    | **Konum** | *Central US* | Uygulama yapılandırma deponuzın barındırıldığı coğrafi konumu belirtmek için **konum** ' i kullanın. En iyi performansı elde etmek için, kaynağı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturun. |

    ![Uygulama yapılandırma deposu kaynağı oluşturma](media/azure-app-configuration-create/azure-portal-app-configuration-create-settings.png)

1. **Oluştur**’u seçin. Dağıtım birkaç dakika sürebilir.

1. Dağıtım bittikten sonra, **erişim anahtarlarına** > **Ayarlar** ' ı seçin. Birincil salt okunurdur anahtar bağlantı dizesini bir yere unutmayın. Uygulamanızı oluşturduğunuz uygulama yapılandırma deposuyla iletişim kuracak şekilde yapılandırmak için bu bağlantı dizesini daha sonra kullanacaksınız.
