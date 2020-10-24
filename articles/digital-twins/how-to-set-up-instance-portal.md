---
title: Örnek ayarlama ve kimlik doğrulaması (portal)
titleSuffix: Azure Digital Twins
description: Azure portal kullanarak Azure dijital TWINS hizmeti örneğini ayarlama bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 0dfc93987fb2ca25b9e397e4aa91adcaaafd8a38
ms.sourcegitcommit: 3bcce2e26935f523226ea269f034e0d75aa6693a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/23/2020
ms.locfileid: "92478867"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-portal"></a>Azure dijital TWINS örneği ve kimlik doğrulaması (portal) ayarlama

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Bu makalede, örneği oluşturma ve kimlik doğrulamasını ayarlama dahil olmak üzere **Yeni bir Azure dijital TWINS örneği ayarlama**adımları ele alınmaktadır. Bu makaleyi tamamladıktan sonra, programlamaya başlamak için kullanabileceğiniz bir Azure dijital TWINS örneğine sahip olursunuz.

Bu makalenin bu sürümü, Azure portal kullanarak bu adımları el ile tek tek gider. Azure portalı komut satırı araçlarına bir alternatif sağlayan web tabanlı birleşik bir konsoldur.
* CLı kullanarak bu adımları el ile yapmak için, bu makalenin CLı sürümüne bakın: [*nasıl yapılır: bir örnek ve kimlik doğrulaması (CLI) ayarlama*](how-to-set-up-instance-cli.md).
* Bir dağıtım betiği örneği kullanarak otomatikleştirilmiş bir kurulum aracılığıyla çalıştırmak için, bu makalenin betikleştirilmiş sürümüne bakın: [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (komut dosyalı)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps.md](../../includes/digital-twins-setup-steps.md)]
[!INCLUDE [digital-twins-setup-permissions.md](../../includes/digital-twins-setup-permissions.md)]

## <a name="create-the-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini oluşturma

Bu bölümde, [Azure Portal](https://ms.portal.azure.com/)kullanarak **Azure Digital TWINS 'in yeni bir örneğini oluşturacaksınız** . Portala gidin ve kimlik bilgilerinizle oturum açın.

Portalda bir kez, Azure Hizmetleri giriş sayfası menüsünde _kaynak oluştur_ ' u seçerek başlayın.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-resource.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

Arama kutusunda *Azure dijital TWINS* araması yapın ve sonuçlardan **Azure dijital TWINS (Önizleme)** hizmetini seçin. Yeni bir hizmet örneği oluşturmak için _Oluştur_ düğmesini seçin.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

Aşağıdaki *kaynak oluştur* sayfasında, aşağıda verilen değerleri girin:
* **Abonelik**: kullandığınız Azure aboneliği
  - **Kaynak grubu**: Örneğin dağıtılacağı bir kaynak grubu. Zaten bir kaynak grubunuz yoksa, *Yeni bağlantı oluştur* ' u seçerek ve yeni bir kaynak grubu için bir ad girerek buradan bir tane oluşturabilirsiniz.
* **Konum**: dağıtım Için bir Azure dijital TWINS özellikli bölgesi. Bölgesel destek hakkında daha fazla bilgi için [*bölgeye göre sunulan Azure ürünlerini ziyaret edin (Azure dijital TWINS)*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
* **Kaynak adı**: Azure dijital TWINS örneğiniz için bir ad. Yeni örneğin adı, aboneliğinizin bölgesi içinde benzersiz olmalıdır (yani, aboneliğiniz seçtiğiniz adı kullanan bölgede başka bir Azure dijital TWINS örneğine sahipse, farklı bir ad seçmeniz istenir).

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

İşiniz bittiğinde, _gözden geçir + oluştur_' u seçin. Bu işlem sizi, girdiğiniz örnek ayrıntılarını gözden geçirebileceğiniz ve _Oluştur_'a vurtabileceğiniz bir özet sayfasına götürür. 

### <a name="verify-success-and-collect-important-values"></a>Başarıyı doğrulama ve önemli değerleri toplama

*Oluşturma*'yı gönderdikten sonra, Azure bildirimlerinde, örnek dağıtımının durumunu Portal simgesi çubuğunun yanı da görebilirsiniz. Bildirim dağıtımın başarılı olduğunu gösterir ve oluşturduğunuz örneği görüntülemek için _Kaynağa Git_ düğmesini seçebileceksiniz.

:::image type="content" source="media/how-to-set-up-instance/portal/notifications-deployment.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

Alternatif olarak, dağıtım başarısız olursa bildirim neden olduğunu gösterecektir. Hata iletisindeki önerileri gözlemleyin ve örneği oluşturmayı yeniden deneyin.

>[!TIP]
>Örneğiniz oluşturulduktan sonra, Azure portal arama çubuğunda örneğinizin adını arayarak istediğiniz zaman sayfasına dönebilirsiniz.

Örneğin *genel bakış* sayfasından *adına*, *kaynak grubuna*ve *ana bilgisayar adına*göz atın. Bunlar, Azure dijital TWINS örneğiniz ile çalışmaya devam ederken ihtiyacınız olabilecek tüm önemli değerlerdir. Diğer kullanıcılar örnekle programlama yapacaktır, bu değerleri bunlarla paylaşmanız gerekir.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

Artık hazır bir Azure dijital TWINS örneğiniz var. Daha sonra, uygun Azure Kullanıcı izinlerini yönetmek için size izin verirsiniz.

## <a name="set-up-user-access-permissions"></a>Kullanıcı erişim izinlerini ayarlama

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

İlk olarak, Azure portal Azure dijital TWINS örneğinizin sayfasını açın. Örneğin menüsünde, *erişim denetimi (IAM)* seçeneğini belirleyin. *Rol ataması Ekle*altında *Ekle* düğmesini seçin.

:::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-1.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

Aşağıdaki *rol ataması Ekle* sayfasında, değerleri girin (Azure aboneliğinde [yeterli izinlere](#prerequisites-permission-requirements) sahip bir kullanıcı tarafından tamamlanmalıdır):
* **Rol**: açılan menüden *Azure dijital TWINS veri sahibini* seçin
* **Erişim ata**: açılan menüden *Azure AD Kullanıcı, Grup veya hizmet sorumlusu ' nı* seçin
* **Seç**: atanacak kullanıcının adını veya e-posta adresini arayın. Sonucu seçtiğinizde, Kullanıcı *Seçili Üyeler* bölümünde görünür.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-2.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Ayrıntıları girmeyi tamamladığınızda *Kaydet* düğmesine basın.

### <a name="verify-success"></a>Başarıyı doğrula

*Erişim denetimi (IAM)* altında ayarladığınız rol atamasını rol atamaları > görüntüleyebilirsiniz. Kullanıcının bir *Azure dijital TWINS veri sahibi*rolüyle listede gösterilmesi gerekir. 

:::image type="content" source="media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

Artık bir Azure dijital TWINS örneğiniz var ve bunu yönetmek için izinler atandı.

## <a name="next-steps"></a>Sonraki adımlar

Azure Digital TWINS CLı komutlarını kullanarak örneğiniz için tek REST API çağrılarını test edin: 
* [az DT Reference](/cli/azure/ext/azure-iot/dt?preserve-view=true&view=azure-cli-latest)
* [*Nasıl yapılır: Azure dijital TWINS CLı 'sını kullanma*](how-to-use-cli.md)

Ya da bkz. kimlik doğrulama kodu ile bir istemci uygulamasını örneğinize bağlama:
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)