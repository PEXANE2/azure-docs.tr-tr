---
title: Örnek ayarlama ve kimlik doğrulaması (el ile)
titleSuffix: Azure Digital Twins
description: Doğru kimlik doğrulaması da dahil olmak üzere Azure dijital TWINS hizmeti örneğini ayarlama bölümüne bakın. El ile sürüm.
author: baanders
ms.author: baanders
ms.date: 7/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: d2d5ce0bc988badc6f25726206a953d87de7eaa2
ms.sourcegitcommit: f353fe5acd9698aa31631f38dd32790d889b4dbb
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87371473"
---
# <a name="set-up-an-azure-digital-twins-instance-and-authentication-manual"></a>Azure dijital TWINS örneği ve kimlik doğrulaması ayarlama (el ile)

[!INCLUDE [digital-twins-setup-selector.md](../../includes/digital-twins-setup-selector.md)]

Bu makalede, örneği oluşturma ve kimlik doğrulamasını ayarlama dahil olmak üzere **Yeni bir Azure dijital TWINS örneği ayarlama**adımları ele alınmaktadır. Bu makaleyi tamamladıktan sonra, programlamaya başlamak için kullanabileceğiniz bir Azure dijital TWINS örneğine sahip olursunuz.

Bu makalenin bu sürümü, bunlardan biri olan bu adımları el ile geçer. Bir dağıtım betiği örneği kullanarak otomatikleştirilmiş bir kurulum aracılığıyla çalıştırmak için, bu makalenin betikleştirilmiş sürümüne bakın: [*nasıl yapılır: örnek ve kimlik doğrulaması ayarlama (komut dosyalı)*](how-to-set-up-instance-scripted.md).

[!INCLUDE [digital-twins-setup-starter.md](../../includes/digital-twins-setup-starter.md)]

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

### <a name="verify-success"></a>Başarıyı doğrula

Örnek başarılı bir şekilde oluşturulduysa Cloud Shell sonucu, oluşturduğunuz kaynakla ilgili bilgiler olarak aşağıdaki gibi görünür:

:::image type="content" source="media/how-to-set-up-instance/create-instance.png" alt-text="Kaynak grubu ve Azure dijital TWINS örneğinin başarıyla oluşturulmasıyla Komut penceresi":::

Azure dijital TWINS örneğinin *ana bilgisayar*adı, *adı*ve *kaynak grubu* 'ndan çıkış ' a bakın. Bunlar, kimlik doğrulama ve ilgili Azure kaynaklarını ayarlamak için Azure dijital TWINS örneğinizle çalışmaya devam ederken ihtiyacınız olabilecek tüm önemli değerlerdir.

> [!TIP]
> Bu özellikleri, örneğinizin tüm özellikleriyle birlikte, çalıştırarak dilediğiniz zaman görebilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

Artık hazır bir Azure dijital TWINS örneğiniz var. Daha sonra, uygun Azure Kullanıcı izinlerini yönetmek için size izin verirsiniz.

## <a name="set-up-your-users-access-permissions"></a>Kullanıcının erişim izinlerini ayarlama

Azure dijital TWINS, rol tabanlı erişim denetimi (RBAC) için [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) kullanır. Bu, bir Kullanıcı Azure dijital TWINS örneğiniz için veri düzlemi çağrısı yapmadan önce, bu kullanıcıya öncelikle izinleri olan bir rol atanması gerektiği anlamına gelir.

Azure dijital TWINS için bu rol, _**Azure dijital TWINS sahibindedir (Önizleme)**_. Kavramlar ve güvenlik hakkında daha fazla bilgi için bkz. [*Azure dijital TWINS çözümleri Için güvenlik*](concepts-security.md).

Bu bölümde Azure dijital TWINS örneğindeki bir kullanıcı için Azure aboneliğinizdeki Azure AD kiracısı ile ilişkili e-postaları aracılığıyla nasıl rol ataması oluşturacağınız gösterilmektedir. Rolünüze ve Azure aboneliğinizde izinleriniz temelinde, bunu kendiniz için ayarlarsınız ya da Azure dijital TWINS örneğini yönetebileceğiniz başka bir kişi adına ayarlamanız gerekir.

### <a name="assign-the-role"></a>Rolü ata

Bir Azure dijital TWINS örneğini yönetmek üzere bir Kullanıcı izinleri vermek için, bu kullanıcılara _**Azure dijital TWINS sahibi (Önizleme)**_ rolünü örnek içinde atamanız gerekir. 

> [!NOTE]
> Bu rolün Azure AD *sahibi* rolünden farklı olduğunu, bu da Azure dijital TWINS örneğinin kapsamına da atanmadığını unutmayın. Bunlar iki ayrı yönetim rolleridir ve Azure AD *sahibi* , *Azure Digital TWINS sahibine (Önizleme)* verilen veri düzlemi özelliklerine erişim izni vermez.

Rolü atamak için aşağıdaki komutu kullanın (bir Azure aboneliğinin sahibi tarafından çalıştırılması gerekir):

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<Azure-AD-email-of-user-to-assign>" --role "Azure Digital Twins Owner (Preview)"
```

Bu komutun sonucu oluşturulan rol ataması hakkında bilgi verilir.

> [!TIP]
> Bunun yerine *400: BadRequest* hatası alırsanız, Kullanıcı için *ObjectID* 'yi almak üzere aşağıdaki komutu çalıştırın:
> ```azurecli
> az ad user show --id <Azure-AD-email-of-user-to-assign> --query objectId
> ```
> Sonra, e-posta yerine kullanıcının *nesne kimliğini* kullanarak rol atama komutunu tekrarlayın.

### <a name="verify-success"></a>Başarıyı doğrula

[!INCLUDE [digital-twins-setup-verify-role-assignment.md](../../includes/digital-twins-setup-verify-role-assignment.md)]

Artık bir Azure dijital TWINS örneğiniz var ve bunu yönetmek için izinler atandı. Daha sonra, bir istemci uygulamasının bu uygulamaya erişmesi için izinleri ayarlayacaksınız.

## <a name="set-up-access-permissions-for-client-applications"></a>İstemci uygulamaları için erişim izinlerini ayarlama

Bir Azure dijital TWINS örneği ayarladıktan sonra, oluşturduğunuz bir istemci uygulaması aracılığıyla bu örnekle etkileşimde bulunmak yaygındır. Bunu yapmak için, istemci uygulamasının Azure dijital ikikine karşı kimlik doğrulaması yapabildiğinizden emin olmanız gerekir. Bu işlem, istemci uygulamanızın kullanması için bir [Azure Active Directory (Azure AD)](../active-directory/fundamentals/active-directory-whatis.md) **uygulama kaydı** ayarlanarak yapılır.

Bu uygulama kaydı, [Azure dijital TWINS API 'leri](how-to-use-apis-sdks.md)için erişim izinlerini yapılandırdığınız yerdir. Daha sonra, istemci uygulaması uygulama kaydında kimlik doğrulaması yapacak ve bu nedenle API 'lere yapılandırılmış erişim izinleri verilmeyecektir.

>[!TIP]
> Abonelik sahibi olarak, her yeni Azure dijital TWINS örneği için yeni bir uygulama kaydı ayarlamayı *veya* bunu yalnızca bir kez yapmayı tercih edebilir ve abonelikteki tüm Azure dijital TWINS örnekleri arasında paylaşılacak tek bir uygulama kaydı oluşturabilirsiniz.

### <a name="create-the-registration"></a>Kayıt oluşturma

Bir uygulama kaydı oluşturmak için Azure dijital TWINS API 'Leri için kaynak kimliklerini ve API 'nin temel izinlerini sağlamanız gerekir. Çalışma dizininizde, yeni bir dosya açın ve bu ayrıntıları yapılandırmak için aşağıdaki JSON kod parçacığını girin: 

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

Bu dosyayı *manifest.js*olarak kaydedin.

> [!NOTE] 
> `https://digitaltwins.azure.net`GUID yerine, Azure Digital TWINS kaynak uygulama kimliği için "kolay ve" insan tarafından okunabilen bir dizenin kullanılabileceği bazı konumlar vardır `0b07f429-9f4b-4714-9392-cc5e8e80c8b0` . Örneğin, bu belge genelinde birçok örnek, MSAL kitaplığı ile kimlik doğrulaması kullanır ve kolay dize bu şekilde kullanılabilir. Ancak, uygulama kaydını oluşturma adımında, KIMLIğIN GUID biçimi yukarıda gösterildiği gibi gereklidir. 

Cloud Shell pencerenizde, "dosyaları karşıya yükle/Indir" simgesine tıklayın ve "karşıya yükle" yi seçin.

:::image type="content" source="media/how-to-set-up-instance/cloud-shell-upload.png" alt-text="Karşıya yükleme seçeneğinin seçimini gösteren Cloud Shell pencere":::
Yeni oluşturduğunuz *manifest.js* gidin ve "Aç" düğmesine basın.

Ardından, bir uygulama kaydı oluşturmak için aşağıdaki komutu çalıştırın (yer tutucuları gerektiği gibi değiştirin):

```azurecli
az ad app create --display-name <name-for-your-app> --native-app --required-resource-accesses manifest.json --reply-url http://localhost
```

İşte oluşturduğunuz kayıt hakkındaki bilgileri gösteren bu komutun çıktısının bir alıntısı:

:::image type="content" source="media/how-to-set-up-instance/new-app-registration.png" alt-text="Yeni Azure AD uygulama kaydı Cloud Shell çıkışı":::

### <a name="verify-success"></a>Başarıyı doğrula

[!INCLUDE [digital-twins-setup-verify-app-registration-1.md](../../includes/digital-twins-setup-verify-app-registration-1.md)]

İlk olarak, karşıya yüklenen *manifest.js* ayarların kayıt üzerinde doğru şekilde ayarlandığını doğrulayın. Bunu yapmak için, menü çubuğundan *bildirim* ' ı seçerek uygulama kaydının bildirim kodunu görüntüleyin. Kod penceresinin alt kısmına ilerleyin ve içindeki *manifest.js* alanları arayın `requiredResourceAccess` :

[!INCLUDE [digital-twins-setup-verify-app-registration-2.md](../../includes/digital-twins-setup-verify-app-registration-2.md)]

### <a name="collect-important-values"></a>Önemli değerleri topla

Ardından, uygulama kaydının ayrıntılarını görmek için menü çubuğundan *genel bakış* ' ı seçin:

:::image type="content" source="media/how-to-set-up-instance/app-important-values.png" alt-text="Uygulama kaydı için önemli değerlerin Portal görünümü":::

**Sayfanızda gösterilen** *uygulama (istemci) kimliğini* ve *Dizin (kiracı) kimliğini* bir yere göz atın. Bu değerler daha sonra [Azure dijital TWINS API 'lerinde istemci uygulamasının kimliğini doğrulamak](how-to-authenticate-client.md)için gerekecektir. Bu tür uygulamalar için kod yazabileceksiniz, bu değerleri olacak kişiyle paylaşmanız gerekir.

### <a name="other-possible-steps-for-your-organization"></a>Kuruluşunuz için olası diğer adımlar

[!INCLUDE [digital-twins-setup-additional-requirements.md](../../includes/digital-twins-setup-additional-requirements.md)]

## <a name="next-steps"></a>Sonraki adımlar

İstemci uygulamasının kimlik doğrulama kodunu yazarak istemci uygulamanızı örneğinize bağlama konusuna bakın:
* [*Nasıl yapılır: uygulama kimlik doğrulama kodunu yazma*](how-to-authenticate-client.md)
