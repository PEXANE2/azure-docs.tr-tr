---
author: mikben
ms.service: azure-communication-services
ms.topic: include
ms.date: 9/1/2020
ms.author: mikben
ms.openlocfilehash: 11b9c553573d9e6188ba634b4cb966d6a9b850b4
ms.sourcegitcommit: 53acd9895a4a395efa6d7cd41d7f78e392b9cfbe
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/22/2020
ms.locfileid: "90948297"
---
## <a name="prerequisites"></a>Önkoşullar

- Etkin aboneliği olan bir Azure hesabı. [Ücretsiz hesap oluşturun](https://azure.microsoft.com/free/dotnet/).

## <a name="create-azure-communication-resource"></a>Azure Iletişim kaynağı oluşturma

Azure Communication Services kaynağı oluşturmak için öncelikle [Azure Portal](https://portal.azure.com)oturum açın. Sayfanın sol üst köşesinde **+ kaynak oluştur**' u seçin. 

:::image type="content" source="../media/create-a-communication-resource/create-resource-plus-sign.png" alt-text="Azure portal kaynak oluştur düğmesinin vurgulandığı ekran görüntüsü.":::

**Market girişini ara** ' ya veya portalın üst kısmındaki arama çubuğu ' na **iletişim** girin.

:::image type="content" source="../media/create-a-communication-resource/searchbar-communication-portal.png" alt-text="Arama çubuğunda iletişim hizmetleri araması gösteren ekran görüntüsü.":::

Sonuçlarda **Iletişim Hizmetleri** ' ni seçin ve ardından **Ekle**' yi seçin.

:::image type="content" source="../media/create-a-communication-resource/add-communication-portal.png" alt-text="Ekle düğmesini vurgulayan Azure panelini gösteren ekran görüntüsü.":::

Artık Iletişim Hizmetleri kaynağınızı yapılandırabilirsiniz. Oluşturma işlemindeki ilk sayfada şunları belirtmeniz istenir:

* Abonelik
* Kaynak grubu (yeni bir tane oluşturabilir veya var olan bir kaynak grubunu seçebilirsiniz)
* Iletişim Hizmetleri kaynağının adı
* Kaynağın ilişkilendirileceği Coğrafya

Sonraki adımda, kaynağa etiketler atayabilirsiniz. Etiketler, Azure kaynaklarınızı düzenlemek için kullanılabilir. Etiketler hakkında daha fazla bilgi için bkz. [kaynak etiketleme belgeleri](https://docs.microsoft.com/azure/azure-resource-manager/management/tag-resources) .

Son olarak, yapılandırmanızı gözden geçirebilir ve kaynağı **oluşturabilirsiniz** . Dağıtımın tamamlanması birkaç dakika sürer.

## <a name="manage-your-communication-services-resource"></a>Iletişim Hizmetleri kaynağınızı yönetme

Iletişim Hizmetleri kaynağınızı yönetmek için [Azure Portal](https://portal.azure.com)gidin ve **Azure iletişim hizmetleri**' ni arayıp seçin.

**Iletişim Hizmetleri** sayfasında, kaynağınızın adını seçin.

Kaynağınız için **genel bakış** sayfası, göz at, durdur, Başlat, yeniden Başlat ve Sil gibi temel yönetime yönelik seçenekler içerir. Kaynak sayfanızın sol menüsünde daha fazla yapılandırma seçeneği bulabilirsiniz.