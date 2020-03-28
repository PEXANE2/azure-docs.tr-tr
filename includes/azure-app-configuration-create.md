---
author: lisaguthrie
ms.author: lcozzens
ms.service: azure-app-configuration
ms.topic: include
ms.date: 1/31/2020
ms.openlocfilehash: 089a199da08503e13dbd42518970c3b7ed8a984d
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "77619325"
---
1. Yeni bir Uygulama Yapılandırma mağazası oluşturmak için [Azure portalında](https://portal.azure.com)oturum açın. Ana sayfanın sol üst köşesinde **kaynak oluştur'u**seçin. Pazar **Yeri Ara** kutusunda, Uygulama **Yapılandırması'nı** girin ve Enter'u seçin.

    ![Uygulama Yapılandırması Ara](media/azure-app-configuration-create/azure-portal-search.png)

1. Arama sonuçlarından **Uygulama Yapılandırması'nı** seçin ve ardından **Oluştur'u**seçin.

    ![Oluştur’u seçin](media/azure-app-configuration-create/azure-portal-app-configuration-create.png)

1. Uygulama **Yapılandırma** > **oluştur** bölmesine aşağıdaki ayarları girin:

    | Ayar | Önerilen değer | Açıklama |
    |---|---|---|
    | **Kaynak adı** | Genel olarak benzersiz bir ad | App Configuration deposu kaynağı için kullanmak üzere benzersiz bir kaynak adı girin. Ad 5 ile 50 karakter arasında bir dize olmalı ve `-` yalnızca sayılar, harfler ve karakter içermelidir. Ad `-` karakterle başlayıp sonlaşamaz.  |
    | **Abonelik** | Aboneliğiniz | Uygulama Yapılandırmasını test etmek için kullanmak istediğiniz Azure aboneliğini seçin. Hesabınızda yalnızca bir abonelik varsa, otomatik olarak seçilir ve **Abonelik** listesi görüntülenmez. |
    | **Kaynak grubu** | *AppConfigTestResources* | Uygulama Yapılandırma mağaza kaynağınız için bir kaynak grubu seçin veya oluşturun. Bu grup, kaynak grubunu silerek aynı anda silmek isteyebileceğin birden çok kaynağı düzenlemek için yararlıdır. Daha fazla bilgi için [Azure kaynaklarınızı yönetmek için kaynak gruplarını kullanma](/azure/azure-resource-manager/resource-group-overview) konusunu inceleyin. |
    | **Konum** | *Orta ABD* | Uygulama yapılandırma mağazanızın barındırıldığı coğrafi konumu belirtmek için **Konum'u** kullanın. En iyi performans için, kaynağı uygulamanızın diğer bileşenleriyle aynı bölgede oluşturun. |
    | **Fiyatlandırma katmanı** | *Ücretsiz* | İstediğiniz fiyatlandırma katmanını seçin. Daha fazla bilgi için lütfen [Uygulama Yapılandırması fiyatlandırma sayfasına](https://azure.microsoft.com/pricing/details/app-configuration/)bakın.

    ![Uygulama Yapılandırma deposu kaynağı oluşturma](media/azure-app-configuration-create/azure-portal-app-configuration-create-settings.png)

1. **Oluştur'u**seçin. Dağıtım birkaç dakika sürebilir.

1. Dağıtım bittikten sonra **Ayarlar** > **Erişim Anahtarları'nı**seçin. Birincil salt okunur anahtar bağlantı dizesini not edin. Bu bağlantı dizesini daha sonra, uygulamanızı oluşturduğunuz Uygulama Yapılandırma deposuyla iletişim kurmak üzere yapılandırmak için kullanırsınız.
