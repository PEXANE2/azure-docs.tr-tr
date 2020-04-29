---
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: include
ms.date: 1/31/2020
ms.openlocfilehash: 089a199da08503e13dbd42518970c3b7ed8a984d
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77619325"
---
1. Yeni bir uygulama yapılandırma deposu oluşturmak için [Azure Portal](https://portal.azure.com)oturum açın. Giriş sayfasının sol üst köşesinde **kaynak oluştur**' u seçin. Market 'te **Ara** kutusuna **uygulama yapılandırması** ' nı girin ve ENTER ' u seçin.

    ![Uygulama yapılandırması ara](media/azure-app-configuration-create/azure-portal-search.png)

1. Arama sonuçlarından **uygulama yapılandırması** ' nı seçin ve ardından **Oluştur**' u seçin.

    ![Oluştur’u seçin](media/azure-app-configuration-create/azure-portal-app-configuration-create.png)

1. **Uygulama yapılandırması** > **Oluştur** bölmesinde, aşağıdaki ayarları girin:

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | Uygulama yapılandırma deposu kaynağı için kullanılacak benzersiz bir kaynak adı girin. Ad, 5 ila 50 karakter arasında bir dize olmalı ve yalnızca rakam, harf ve `-` karakter içermelidir. Ad, `-` karakteriyle başlayamaz veya bitemez.  |
    | **Abonelik** | Aboneliğiniz | Uygulama yapılandırmasını test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa, bu otomatik olarak seçilir ve **abonelik** listesi gösterilmez. |
    | **Kaynak grubu** | *AppConfigTestResources* | Uygulama yapılandırma deposu kaynağınız için bir kaynak grubu seçin veya oluşturun. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğiniz birden çok kaynağı düzenlemek için yararlıdır. Daha fazla bilgi için [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](/azure/azure-resource-manager/resource-group-overview) konusunu inceleyin. |
    | **Konum** | *Orta ABD* | Uygulama yapılandırma deponuzın barındırıldığı coğrafi konumu belirtmek için **konum** ' i kullanın. En iyi performansı elde etmek için, kaynağı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturun. |
    | **Fiyatlandırma Katmanı** | *Ücretsiz* | İstediğiniz fiyatlandırma katmanını seçin. Daha fazla ayrıntı için lütfen [uygulama yapılandırma fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/app-configuration/)bakın.

    ![Uygulama yapılandırma deposu kaynağı oluşturma](media/azure-app-configuration-create/azure-portal-app-configuration-create-settings.png)

1. **Oluştur**’u seçin. Dağıtım birkaç dakika sürebilir.

1. Dağıtım bittikten sonra **Ayarlar** > **erişim Anahtarlar**' ı seçin. Birincil salt okunurdur anahtar bağlantı dizesini bir yere unutmayın. Uygulamanızı oluşturduğunuz uygulama yapılandırma deposuyla iletişim kuracak şekilde yapılandırmak için bu bağlantı dizesini daha sonra kullanacaksınız.
