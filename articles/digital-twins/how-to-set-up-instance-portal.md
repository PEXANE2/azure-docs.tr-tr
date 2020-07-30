---
title: Örnek ve kimlik doğrulaması ayarlama (portal)
titleSuffix: Azure Digital Twins
description: Azure portal kullanarak Azure dijital TWINS hizmeti örneğini ayarlama bölümüne bakın.
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 73b7171b89b26926992e95f77e376e7bb7731eff
ms.sourcegitcommit: 42107c62f721da8550621a4651b3ef6c68704cd3
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87408409"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-portal"></a>Azure dijital TWINS örneği ve kimlik doğrulaması (portal) ayarlama

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Bu makalede, örneği oluşturma ve kimlik doğrulamasını ayarlama dahil olmak üzere **Yeni bir Azure dijital TWINS örneği ayarlama**adımları ele alınmaktadır. Bu makaleyi tamamladıktan sonra, programlamaya başlamak için kullanabileceğiniz bir Azure dijital TWINS örneğine sahip olursunuz.

Bu makalenin bu sürümü, Azure portal kullanarak bu adımları el ile tek tek gider. Azure portalı komut satırı araçlarına bir alternatif sağlayan web tabanlı birleşik bir konsoldur.
* CLı kullanarak bu adımları el ile yapmak için, bu makalenin CLı sürümüne bakın: [*nasıl yapılır: bir örnek ve kimlik doğrulaması (CLI) ayarlama*](how-to-set-up-instance-cli.md).
* Bir dağıtım betiği örneği kullanarak otomatikleştirilmiş bir kurulum aracılığıyla çalıştırmak için, bu makalenin betikleştirilmiş sürümüne bakın: [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (komut dosyalı)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps.md](../../includes/digital-twins-setup-steps.md)]
 
Ardından [Azure Portal](https://ms.portal.azure.com/) kimlik bilgilerinizle oturum açın.

## <a name="prerequisites-permission-requirements"></a>Önkoşullar: Izin gereksinimleri

Bu makaledeki tüm adımları tamamlayabilmeniz için, Azure aboneliğinizde bir sahip olarak sınıflandırılmalıdır. 

İzin düzeyinizi Azure portal [abonelik sayfasında](https://portal.azure.com/#blade/Microsoft_Azure_Billing/SubscriptionsBlade) kontrol edebilirsiniz (Bu bağlantıyı kullanabilir veya Portal arama çubuğu ile *abonelikler* bulabilirsiniz). Kullanmakta olduğunuz aboneliğin adını bulun ve *rol* sütununda rolü görüntüleyin. Sahibiyseniz, bu değer *sahip*olur:

:::image type="content" source="media/how-to-set-up-instance/portal/subscriptions-role.png" alt-text="Kullanıcı sahip olarak gösteren Azure portal Abonelikler sayfasının görünümü" lightbox="media/how-to-set-up-instance/portal/subscriptions-role.png":::

Değerin *katkıda* bulunduğunu veya *sahip*dışında bir şeyi fark ederseniz aşağıdaki yollarla devam edebilirsiniz:
* Sizin adınıza bu makaledeki adımları tamamlamaya yönelik abonelik sahibine ve sahibine yönelik isteğe başvurun
* Abonelik sahibine veya Kullanıcı erişimi yönetici rolüne sahip birine abone olur ve devam etmek için gereken izinlere sahip olacak şekilde aboneliğinizi aboneliğe sahip olarak yükseltmenizi isteyin. Bunun uygun olup olmadığı, kuruluşunuza ve onun içindeki rolünüze bağlıdır.

## <a name="create-the-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini oluşturma

Bu bölümde, Azure portal kullanarak **Azure Digital TWINS 'in yeni bir örneğini oluşturacaksınız** .

[Azure Portal](https://ms.portal.azure.com/)oturum açtıktan sonra, Azure Hizmetleri giriş sayfası menüsünde _kaynak oluştur_ ' u seçerek başlayın.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-resource.png" alt-text="Azure portal ana sayfasından ' kaynak oluştur ' seçeneği seçiliyor":::

Arama kutusunda *Azure dijital TWINS* araması yapın ve sonuçlardan **Azure dijital TWINS (Önizleme)** hizmetini seçin. Yeni bir hizmet örneği oluşturmak için _Oluştur_ düğmesini seçin.

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins.png" alt-text="Azure dijital TWINS hizmeti sayfasından ' oluştur ' seçeneğini belirleme":::

Aşağıdaki *kaynak oluştur* sayfasında, aşağıda verilen değerleri girin:
* **Abonelik**: kullandığınız Azure aboneliği
  - **Kaynak grubu**: Örneğin dağıtılacağı bir kaynak grubu. Zaten bir kaynak grubunuz yoksa, *Yeni bağlantı oluştur* ' u seçerek ve yeni bir kaynak grubu için bir ad girerek buradan bir tane oluşturabilirsiniz.
* **Konum**: dağıtım Için bir Azure dijital TWINS özellikli bölgesi. Bölgesel destek hakkında daha fazla bilgi için [*bölgeye göre sunulan Azure ürünlerini ziyaret edin (Azure dijital TWINS)*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins).
* **Kaynak adı**: Azure dijital TWINS örneğiniz için bir ad. Yeni örneğin adı, aboneliğinizin bölgesi içinde benzersiz olmalıdır (yani, aboneliğiniz seçtiğiniz adı kullanan bölgede başka bir Azure dijital TWINS örneğine sahipse, farklı bir ad seçmeniz istenir).

:::image type="content" source= "media/how-to-set-up-instance/portal/create-azure-digital-twins-2.png" alt-text="Azure dijital TWINS kaynağı oluşturmak için açıklanan değerleri doldurma":::

İşiniz bittiğinde, _gözden geçir + oluştur_' u seçin. Bu işlem sizi, girdiğiniz örnek ayrıntılarını gözden geçirebileceğiniz ve _Oluştur_'a vurtabileceğiniz bir özet sayfasına götürür. 

### <a name="verify-success"></a>Başarıyı doğrula

*Oluşturma*'yı gönderdikten sonra, Azure bildirimlerinde, örnek dağıtımının durumunu Portal simgesi çubuğunun yanı da görebilirsiniz. Bildirim dağıtımın başarılı olduğunu gösterir ve oluşturduğunuz örneği görüntülemek için _Kaynağa Git_ düğmesini seçebileceksiniz.

:::image type="content" source="media/how-to-set-up-instance/portal/notifications-deployment.png" alt-text="Başarılı bir dağıtım gösteren ve ' kaynağa git ' düğmesini vurgulayan Azure bildirimlerinin görünümü":::

Alternatif olarak, dağıtım başarısız olursa bildirim neden olduğunu gösterecektir. Hata iletisindeki önerileri gözlemleyin ve örneği oluşturmayı yeniden deneyin.

>[!TIP]
>Örneğiniz oluşturulduktan sonra, Azure portal arama çubuğunda örneğinizin adını arayarak istediğiniz zaman sayfasına dönebilirsiniz.

Örneğin *genel bakış* sayfasından *adına*, *kaynak grubuna*ve *ana bilgisayar adına*göz atın. Bunlar, Azure dijital TWINS örneğiniz ile çalışmaya devam ederken ihtiyacınız olabilecek tüm önemli değerlerdir. Diğer kullanıcılar örnekle programlama yapacaktır, bu değerleri bunlarla paylaşmanız gerekir.

:::image type="content" source="media/how-to-set-up-instance/portal/instance-important-values.png" alt-text="Örneğin genel bakış sayfasından önemli değerleri vurgulama":::

Artık hazır bir Azure dijital TWINS örneğiniz var. Daha sonra, uygun Azure Kullanıcı izinlerini yönetmek için size izin verirsiniz.

## <a name="set-up-user-access-permissions"></a>Kullanıcı erişim izinlerini ayarlama

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

İlk olarak, Azure portal Azure dijital TWINS örneğinizin sayfasını açın. Örneğin menüsünde, *erişim denetimi (IAM)* seçeneğini belirleyin. *Rol ataması Ekle*altında *Ekle* düğmesini seçin.

:::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-1.png" alt-text="' Erişim denetimi (ıAM) ' sayfasından rol ataması eklemeyi seçme":::

Aşağıdaki *rol ataması Ekle* sayfasında, değerleri girin (Azure aboneliğinin sahibi tarafından tamamlanmalıdır):
* **Rol**: açılan menüden *Azure dijital TWINS sahibini (Önizleme)* seçin
* **Erişim ata**: açılan menüden *Azure AD Kullanıcı, Grup veya hizmet sorumlusu ' nı* seçin
* **Seç**: atanacak kullanıcının adını veya e-posta adresini arayın. Sonucu seçtiğinizde, Kullanıcı *Seçili Üyeler* bölümünde görünür.

:::row:::
    :::column:::
        :::image type="content" source="media/how-to-set-up-instance/portal/add-role-assignment-2.png" alt-text="Listelenen alanlar ' rol ataması Ekle ' iletişim kutusunda dolduruluyor":::
    :::column-end:::
    :::column:::
    :::column-end:::
:::row-end:::

Ayrıntıları girmeyi tamamladığınızda *Kaydet* düğmesine basın.

### <a name="verify-success"></a>Başarıyı doğrula

*Erişim denetimi (IAM)* altında ayarladığınız rol atamasını rol atamaları > görüntüleyebilirsiniz. Kullanıcının bir *Azure dijital TWINS sahibi (Önizleme)* rolüyle listede gösterilmesi gerekir. 

:::image type="content" source="media/how-to-set-up-instance/portal/verify-role-assignment.png" alt-text="Azure portal bir Azure dijital TWINS örneği için rol atamalarının görünümü":::

Artık bir Azure dijital TWINS örneğiniz var ve bunu yönetmek için izinler atandı. Daha sonra, bir istemci uygulamasının bu uygulamaya erişmesi için izinleri ayarlayacaksınız.

## <a name="set-up-access-permissions-for-client-applications"></a>İstemci uygulamaları için erişim izinlerini ayarlama

[!INCLUDE [digital-twins-setup-app-registration.md](../../includes/digital-twins-setup-app-registration.md)]

Azure portal [Azure Active Directory](https://portal.azure.com/#blade/Microsoft_AAD_IAM/ActiveDirectoryMenuBlade/Overview) giderek başlayın (Bu bağlantıyı kullanabilir veya Portal arama çubuğundan bulabilirsiniz). Hizmet menüsünden *uygulama kayıtları* ' i ve sonra *+ Yeni kayıt*' ı seçin.

:::image type="content" source="media/how-to-set-up-instance/portal/new-registration.png" alt-text="' Uygulama kayıtları ' menü seçeneğini ve ' + yeni kayıt ' düğmesini vurgulayarak Azure portal Azure AD hizmeti sayfasının görünümü":::

Aşağıdaki *uygulama kaydet* sayfasında, istenen değerleri girin:
* **Ad**: kayıt ile Ilişkilendirilecek BIR Azure AD uygulama görünen adı
* **Desteklenen hesap türleri**: *yalnızca bu kuruluş dizinindeki hesapları seçin (yalnızca varsayılan dizin-tek kiracı)*
* **Yeniden yönlendirme URI 'si**: Azure AD uygulaması Için BIR *Azure AD uygulama yanıt URL 'si* . Kullanabilirsiniz `http://localhost` .

İşiniz bittiğinde *Kaydet* düğmesine basın.

:::image type="content" source="media/how-to-set-up-instance/portal/register-an-application.png" alt-text="Belirtilen değerlerin doldurulduğu ' bir uygulama kaydetme ' sayfasının görünümü":::

Kayıt kurulumu tamamlandığında, Portal sizi ayrıntılar sayfasına yönlendirecektir.

### <a name="provide-azure-digital-twins-api-permission"></a>Azure dijital TWINS API 'SI izni sağlama

Daha sonra, temel izinlerle oluşturduğunuz uygulama kaydını Azure dijital TWINS API 'Leri için yapılandırın.

Uygulama kaydınız için portal sayfasında, menüden *API izinleri* ' ni seçin. Aşağıdaki izinler sayfasında *+ Izin Ekle* düğmesine basın.

:::image type="content" source="media/how-to-set-up-instance/portal/add-permission.png" alt-text="Azure portal ' API izinleri ' menü seçeneğini ve ' + izin Ekle ' düğmesini vurgulayarak uygulama kaydının görünümü":::

Aşağıdaki *API Izinleri iste* sayfasında *Kuruluşumun kullandığı API* 'lere geçin ve *Azure dijital TWINS*için arama yapın. Azure dijital TWINS API 'Leri için izin atamaya devam etmek için arama sonuçlarından *Azure dijital TWINS* ' i seçin.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-1.png" alt-text="Azure dijital TWINS 'i gösteren ' API Izinleri ıste ' sayfasının arama sonucunun görünümü":::

Ardından, bu API 'Lere verilecek izinleri seçersiniz. **Oku (1)** iznini genişletin ve bu uygulama kayıt okuyucuyu ve yazıcı izinlerini vermek için *Read. Write* yazan kutusunu işaretleyin.

:::image type="content" source="media/how-to-set-up-instance/portal/request-api-permissions-2.png" alt-text="Azure Digital TWINS API 'Leri için ' Read. Write ' izinleri seçilerek ' API Izinleri ıste ' sayfasının görünümü":::

Bittiğinde *Izinleri Ekle* düğmesine basın.

### <a name="verify-success"></a>Başarıyı doğrula

*API izinleri* sayfasına geri döndüğünüzde, Azure Digital TWINS için okuma/yazma izinlerini yansıtan bir girdi olduğunu doğrulayın:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-api-permissions.png" alt-text="Azure dijital TWINS için ' okuma/yazma erişimi ' gösteren Azure AD uygulama kaydı için API izinlerinin Portal görünümü":::

Ayrıca, API izinleri eklenirken Azure dijital TWINS bilgileriyle otomatik olarak güncellenen Azure Digital TWINS ile *ilgilimanifest.jsüzerinde*uygulama kaydı içinde olan bağlantıyı doğrulayabilirsiniz.

Bunu yapmak için menüden *bildirim* ' ı seçerek uygulama kaydının bildirim kodunu görüntüleyin. Kod penceresinin en altına kaydırın ve altındaki bu alanları bulun `requiredResourceAccess` . Değerler aşağıdaki ekran görüntüsünde olanlarla eşleşmelidir:

:::image type="content" source="media/how-to-set-up-instance/portal/verify-manifest.png" alt-text="Azure AD uygulama kaydı bildiriminin Portal görünümü. ' RequiredResourceAccess ' altında iç içe geçmiş bir ' Resourceappıd ' değeri, 0b07f429-9f4b-4714-9392-cc5e8e80c8b0 ve bir ' resourceAccess > ID ' değeri 4589bd03-58cb-4e6c-b17f-b580e39652f8 ' dir":::

### <a name="collect-important-values"></a>Önemli değerleri topla

Ardından, uygulama kaydının ayrıntılarını görmek için menü çubuğundan *genel bakış* ' ı seçin:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Uygulama kaydı için önemli değerlerin Portal görünümü":::

**Sayfanızda gösterilen** *uygulama (istemci) kimliğini* ve *Dizin (kiracı) kimliğini* bir yere göz atın. Bu değerler daha sonra [Azure dijital TWINS API 'lerinde istemci uygulamasının kimliğini doğrulamak](how-to-authenticate-client.md)için gerekecektir. Bu tür uygulamalar için kod yazmak üzere bir kişi değilseniz, bu değerleri olacak kişiyle paylaşmanız gerekir.

### <a name="other-possible-steps-for-your-organization"></a>Kuruluşunuz için olası diğer adımlar

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulamasının kimlik doğrulama kodunu yazarak istemci uygulamanızı örneğinize bağlama konusuna bakın:
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)