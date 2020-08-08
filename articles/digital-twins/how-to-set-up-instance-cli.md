---
title: Örnek ayarlama ve kimlik doğrulaması (CLI)
titleSuffix: Azure Digital Twins
description: CLı kullanarak Azure dijital TWINS hizmeti örneğini ayarlama bölümüne bakın
author: baanders
ms.author: baanders
ms.date: 7/23/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: 50a7fe866d236a7edb30b3cae5ef076d3ebbca56
ms.sourcegitcommit: 98854e3bd1ab04ce42816cae1892ed0caeedf461
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/07/2020
ms.locfileid: "88009724"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-cli"></a>Azure dijital TWINS örneği ve kimlik doğrulaması (CLı) ayarlama

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Bu makalede, örneği oluşturma ve kimlik doğrulamasını ayarlama dahil olmak üzere **Yeni bir Azure dijital TWINS örneği ayarlama**adımları ele alınmaktadır. Bu makaleyi tamamladıktan sonra, programlamaya başlamak için kullanabileceğiniz bir Azure dijital TWINS örneğine sahip olursunuz.

Bu makalenin bu sürümü, tek tek, CLı kullanılarak bu adımları el ile bir arada ilerler.
* Azure portal kullanarak bu adımları el ile almak için, bu makalenin Portal sürümüne bakın: [*nasıl yapılır: bir örnek ve kimlik doğrulaması (portal) ayarlama*](how-to-set-up-instance-portal.md).
* Bir dağıtım betiği örneği kullanarak otomatikleştirilmiş bir kurulum aracılığıyla çalıştırmak için, bu makalenin betikleştirilmiş sürümüne bakın: [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (komut dosyalı)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-steps-prereq.md](../../includes/digital-twins-setup-steps-prereq.md)]
[!INCLUDE [cloud-shell-try-it.md](../../includes/cloud-shell-try-it.md)]

## <a name="set-up-cloud-shell-session"></a>Cloud Shell oturum ayarlama
[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="create-the-azure-digital-twins-instance"></a>Azure dijital TWINS örneğini oluşturma

Bu bölümde, Cloud Shell komutunu kullanarak **Azure Digital TWINS 'in yeni bir örneğini oluşturacaksınız** . Şunları sağlamanız gerekir:
* İçinde dağıtılacak bir kaynak grubu. Zaten var olan bir kaynak grubunuz yoksa şu komutla bir tane oluşturabilirsiniz:
    ```azurecli
    az group create --location <region> --name <name-for-your-resource-group>
    ```
* Dağıtım için bir bölge. Azure dijital TWINS 'i destekleyen bölgeleri görmek için [*bölgeye göre kullanılabilen Azure ürünlerini*](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins)ziyaret edin.
* Örneğiniz için bir ad. Yeni örneğin adı, aboneliğinizin bölgesi içinde benzersiz olmalıdır (yani, aboneliğiniz seçtiğiniz adı kullanan bölgede başka bir Azure dijital TWINS örneğine sahipse, farklı bir ad seçmeniz istenir).

Örneği oluşturmak için aşağıdaki komutta bu değerleri kullanın:

```azurecli
az dt create --dt-name <name-for-your-Azure-Digital-Twins-instance> -g <your-resource-group> -l <region>
```

### <a name="verify-success-and-collect-important-values"></a>Başarıyı doğrulama ve önemli değerleri toplama

Örnek başarılı bir şekilde oluşturulduysa Cloud Shell sonucu, oluşturduğunuz kaynakla ilgili bilgiler olarak aşağıdaki gibi görünür:

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/create-instance.png" alt-text="Kaynak grubu ve Azure dijital TWINS örneğinin başarıyla oluşturulmasıyla Komut penceresi":::

Azure dijital TWINS örneğinin *ana bilgisayar*adı, *adı*ve *kaynak grubu* 'ndan çıkış ' a bakın. Bunlar, kimlik doğrulama ve ilgili Azure kaynaklarını ayarlamak için Azure dijital TWINS örneğinizle çalışmaya devam ederken ihtiyacınız olabilecek tüm önemli değerlerdir. Diğer kullanıcılar örnekle programlama yapacaktır, bu değerleri bunlarla paylaşmanız gerekir.

> [!TIP]
> Bu özellikleri, örneğinizin tüm özellikleriyle birlikte, çalıştırarak dilediğiniz zaman görebilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

Artık hazır bir Azure dijital TWINS örneğiniz var. Daha sonra, uygun Azure Kullanıcı izinlerini yönetmek için size izin verirsiniz.

## <a name="set-up-user-access-permissions"></a>Kullanıcı erişim izinlerini ayarlama

[!INCLUDE [digital-twins-setup-role-assignment.md](../../includes/digital-twins-setup-role-assignment.md)]

Rolü atamak için aşağıdaki komutu kullanın (Azure aboneliğinde [yeterli izinlere](#prerequisites-permission-requirements) sahip bir kullanıcı tarafından çalıştırılması gerekir):

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<Azure-AD-email-of-user-to-assign>" --role "Azure Digital Twins Owner (Preview)"
```

Bu komutun sonucu oluşturulan rol ataması hakkında bilgi verilir.

> [!NOTE]
> Bu komut, CLı 'nin **grafik veritabanında kullanıcı veya hizmet sorumlusu bulamadığını**söyleyen bir hata döndürürse:
>
> E-postaları yerine kullanıcının *nesne kimliğini* kullanın. Bu, kişisel [Microsoft hesaplarındaki (MSAs)](https://account.microsoft.com/account)kullanıcılar için gerçekleşebilir. 
>
> Kullanıcı hesabını seçmek ve ayrıntılarını açmak için [Azure Active Directory kullanıcıların Azure Portal sayfasını](https://portal.azure.com/#blade/Microsoft_AAD_IAM/UsersManagementMenuBlade/AllUsers) kullanın. Kullanıcının *ObjectID*'yi kopyalayın:
>
> :::image type="content" source="media/includes/user-id.png" alt-text="' Nesne KIMLIĞI ' alanındaki GUID 'YI vurgulamak Azure portal Kullanıcı sayfasının görünümü" lightbox="media/includes/user-id.png":::
>
> Sonra, e-posta yerine kullanıcının *nesne kimliğini* kullanarak rol atama listesi komutunu tekrarlayın.

### <a name="verify-success"></a>Başarıyı doğrula

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

Artık bir Azure dijital TWINS örneğiniz var ve bunu yönetmek için izinler atandı. Daha sonra, bir istemci uygulamasının bu uygulamaya erişmesi için izinleri ayarlayacaksınız.

## <a name="set-up-access-permissions-for-client-applications"></a>İstemci uygulamaları için erişim izinlerini ayarlama

[!INCLUDE [digital-twins-setup-app-registration.md](../../includes/digital-twins-setup-app-registration.md)]

Bir uygulama kaydı oluşturmak için Azure dijital TWINS API 'Leri için kaynak kimliklerini ve API 'nin temel izinlerini sağlamanız gerekir.

Çalışma dizininizde, yeni bir dosya oluşturun ve bu ayrıntıları yapılandırmak için aşağıdaki JSON kod parçacığını girin: 

```json
[{
    "resourceAppId": "0b07f429-9f4b-4714-9392-cc5e8e80c8b0",
    "resourceAccess": [
     {
       "id": "4589bd03-58cb-4e6c-b17f-b580e39652f8",
       "type": "Scope"
     }
    ]
}]
``` 

Bu dosyayı _**manifest.js**_ olarak kaydedin.

> [!NOTE] 
> `https://digitaltwins.azure.net`GUID yerine, Azure Digital TWINS kaynak uygulama kimliği için "kolay ve" insan tarafından okunabilen bir dizenin kullanılabileceği bazı konumlar vardır `0b07f429-9f4b-4714-9392-cc5e8e80c8b0` . Örneğin, bu belge genelinde birçok örnek, MSAL kitaplığı ile kimlik doğrulaması kullanır ve kolay dize bu şekilde kullanılabilir. Ancak, uygulama kaydını oluşturma adımında, KIMLIğIN GUID biçimi yukarıda gösterildiği gibi gereklidir. 

Sonra bu dosyayı Cloud Shell karşıya yüklersiniz. Cloud Shell pencerenizde, "dosyaları karşıya yükle/Indir" simgesine tıklayın ve "karşıya yükle" yi seçin.

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/cloud-shell-upload.png" alt-text="Karşıya yükleme seçeneğinin seçimini gösteren Cloud Shell pencere":::
Yeni oluşturduğunuz *manifest.js* gidin ve "Aç" düğmesine basın.

Ardından, bir uygulama kaydı oluşturmak için aşağıdaki komutu çalıştırın (yer tutucuları gerektiği gibi değiştirin):

```azurecli
az ad app create --display-name <name-for-your-app-registration> --native-app --required-resource-accesses manifest.json --reply-url http://localhost
```

İşte oluşturduğunuz kayıt hakkındaki bilgileri gösteren bu komutun çıktısının bir alıntısı:

:::image type="content" source="media/how-to-set-up-instance/cloud-shell/new-app-registration.png" alt-text="Yeni Azure AD uygulama kaydı Cloud Shell çıkışı":::

### <a name="verify-success"></a>Başarıyı doğrula

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

İlk olarak, karşıya yüklenen *manifest.js* ayarların kayıt üzerinde doğru şekilde ayarlandığını doğrulayın. Bunu yapmak için, menü çubuğundan *bildirim* ' ı seçerek uygulama kaydının bildirim kodunu görüntüleyin. Kod penceresinin alt kısmına ilerleyin ve içindeki *manifest.js* alanları arayın `requiredResourceAccess` :

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

### <a name="collect-important-values"></a>Önemli değerleri topla

Ardından, uygulama kaydının ayrıntılarını görmek için menü çubuğundan *genel bakış* ' ı seçin:

:::image type="content" source="media/how-to-set-up-instance/portal/app-important-values.png" alt-text="Uygulama kaydı için önemli değerlerin Portal görünümü":::

**Sayfanızda gösterilen** *uygulama (istemci) kimliğini* ve *Dizin (kiracı) kimliğini* bir yere göz atın. Bu değerler daha sonra [Azure dijital TWINS API 'lerinde istemci uygulamasının kimliğini doğrulamak](how-to-authenticate-client.md)için gerekecektir. Bu tür uygulamalar için kod yazabileceksiniz, bu değerleri olacak kişiyle paylaşmanız gerekir.

### <a name="other-possible-steps-for-your-organization"></a>Kuruluşunuz için olası diğer adımlar

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulamasının kimlik doğrulama kodunu yazarak istemci uygulamanızı örneğinize bağlama konusuna bakın:
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)