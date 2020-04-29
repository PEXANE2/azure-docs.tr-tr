---
author: clemensv
ms.service: service-bus-relay
ms.topic: include
ms.date: 11/09/2018
ms.author: clemensv
ms.openlocfilehash: 500db9f6d299ea81b1f1b1be864df5d91ba1eae7
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "76020867"
---
1. [Azure Portal][Azure portal] oturum açın.
1. **Kaynak oluştur**’u seçin. Ardından, **tümleştirme** > **geçişi**' ni seçin. Listede **Geçiş**'i görmüyorsanız sağ üst köşedeki **Tümünü Gör**'ü seçin.
1. **Oluştur**' u seçin ve **ad** alanına bir ad alanı adı girin. Azure portal, adın kullanılabilir olup olmadığını denetler.
1. Ad alanını oluşturmak için bir Azure aboneliği seçin.
1. [Kaynak grubu](../articles/azure-resource-manager/management/manage-resource-groups-portal.md)için, ad alanının yerleştirileceği mevcut bir kaynak grubunu seçin ya da yeni bir tane oluşturun.  
1. Ad alanının barındırılması gereken ülkeyi veya bölgeyi seçin.

    ![Ad alanı oluşturma][create-namespace]

1. **Oluştur**’u seçin. Azure portal, ad alanınızı oluşturur ve etkinleştirilir. Birkaç dakika sonra sistem, hesabınız için kaynakları sağlar.

### <a name="get-management-credentials"></a>Yönetim kimlik bilgilerini alma

1. **Tüm kaynaklar**' ı seçin ve ardından yeni oluşturulan ad alanı adını seçin.
1. **Paylaşılan erişim ilkeleri**' ni seçin.  
1. **Paylaşılan erişim ilkeleri** bölümünde **RootManageSharedAccessKey** öğesini seçin.
1. **SAS ilkesi: RootManageSharedAccessKey**altında, **birincil bağlantı dizesi**' nin yanındaki **Kopyala** düğmesini seçin. Bu eylem, bağlantı dizesini daha sonra kullanmak üzere panonuza kopyalar. Bu değeri Not Defteri veya başka bir geçici konuma yapıştırın.
1. **Birincil anahtar** değerini daha sonra kullanmak üzere kopyalayıp geçici bir konuma yapıştırarak önceki adımı tekrarlayın.  

    ![bağlantı dizesi][connection-string]

<!--Image references-->

[create-namespace]: ./media/relay-create-namespace-portal/create-namespace-vs2019.png
[connection-info]: ./media/relay-create-namespace-portal/connection-info.png
[connection-string]: ./media/relay-create-namespace-portal/connection-string-vs2019.png
[Azure portal]: https://portal.azure.com
