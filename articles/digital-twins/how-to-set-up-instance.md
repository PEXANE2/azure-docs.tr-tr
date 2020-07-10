---
title: Azure Digital Twins örneği oluşturma
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS hizmeti 'nin bir örneğini ayarlama.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: fecacbd2c7c6549a1321367157bb179321779ca9
ms.sourcegitcommit: 3541c9cae8a12bdf457f1383e3557eb85a9b3187
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/09/2020
ms.locfileid: "86206512"
---
# <a name="set-up-an-azure-digital-twins-instance"></a>Azure dijital TWINS örneği ayarlama

Bu makale, yeni bir Azure dijital TWINS örneği kurmak için temel adımlarda size yol gösterir. Bu, örneğin örneğini oluşturmayı ve örneğe [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md) izinlerinin atanmasını içerir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="set-up-an-azure-digital-twins-instance"></a>Azure dijital TWINS örneği ayarlama

Daha sonra, bu nasıl yapılır ile kullanılmak üzere yeni bir Azure Kaynak grubu oluşturacaksınız. Daha sonra, bu kaynak grubunda **Azure dijital TWINS 'in yeni bir örneğini oluşturabilirsiniz** . 

Ayrıca, örneğiniz için bir ad girmeniz ve dağıtım için bir bölge seçmeniz gerekir. Azure dijital TWINS 'i destekleyen bölgeleri görmek için [bölgeye göre kullanılabilen Azure ürünlerini](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins)ziyaret edin.

>[!NOTE]
> Yeni örneğin adı bölge içinde benzersiz olmalıdır (yani, bu bölgedeki başka bir Azure dijital TWINS örneği zaten seçtiğiniz adı kullanıyorsa, farklı bir ad seçmeniz gerekir).

Kaynak grubunu ve örneği aşağıdaki komutlarla oluşturun:

```azurecli
az group create --location <region> --name <name-for-your-resource-group>
az dt create --dt-name <name-for-your-Azure-Digital-Twins-instance> -g <your-resource-group> -l <region>
```

Bu komutların sonucu, oluşturduğunuz kaynaklarla ilgili bilgiler olarak şu şekilde görünür:

:::image type="content" source="media/how-to-set-up-instance/create-instance.png" alt-text="Kaynak grubu ve Azure dijital TWINS örneğinin başarıyla oluşturulmasıyla Komut penceresi":::

Azure dijital TWINS örneğinin *ana bilgisayar*adı, *adı*ve *kaynak grubu* 'ndan çıkış ' a bakın. Bunlar, kimlik doğrulama ve ilgili Azure kaynaklarını ayarlamak için Azure dijital TWINS örneğinizle çalışmaya devam ederken ihtiyacınız olabilecek tüm anahtar değerlerdir.

> [!TIP]
> Bu özellikleri, örneğinizin tüm özellikleriyle birlikte, çalıştırarak dilediğiniz zaman görebilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

### <a name="assign-azure-active-directory-permissions"></a>Azure Active Directory izinlerini ata

Azure dijital TWINS, rol tabanlı erişim denetimi (RBAC) için [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md) kullanır. Yani, Azure dijital TWINS örneğiniz için veri düzlemi çağrısı yapabilmeniz için önce bu izinlerle kendinize bir rol atamanız gerekir.

Azure dijital TWINS 'i bir istemci uygulamasıyla kullanabilmek için, istemci uygulamanızın Azure dijital TWINS 'de kimlik doğrulaması yapabilmesine de dikkat etmeniz gerekir. Bu, [nasıl yapılır: istemci uygulamasında kimlik doğrulama](how-to-authenticate-client.md)hakkında okuyabilmeniz için bir Azure ACTIVE DIRECTORY (AAD) uygulama kaydı ayarlanarak yapılır.

#### <a name="assign-yourself-a-role"></a>Kendinize bir rol atayın

Azure aboneliğinizdeki AAD kiracısı ile ilişkili e-postanızı kullanarak Azure dijital TWINS örneğinde kendiniz için bir rol ataması oluşturun. 

Bunu yapabilmeleri için Azure aboneliğinizde bir sahip olarak sınıflandırılmalıdır. Bunu `az role assignment list --assignee <your-Azure-email>` , komutunu çalıştırarak ve sonra *Roledefinitionname* değerinin *sahip*olduğu çıktıda doğrulayarak kontrol edebilirsiniz. Değerin *katkıda* bulunduğunu veya *sahip*dışında bir şeyi fark ederseniz, aboneliğinizde izin vermek için lütfen güç ile abonelik yöneticinizle iletişime geçin. Rolü tüm abonelikte yükseltebilir, böylece aşağıdaki komutu çalıştırabilirsiniz veya bir sahip sizin yerinize Azure dijital TWINS izinlerinizi kurmak için sizin adınıza aşağıdaki komutu çalıştırabilir.

Azure dijital TWINS örneğiniz için Kullanıcı "sahibi" izinlerinizi atamak için aşağıdaki komutu kullanın (Azure aboneliğinin sahibi tarafından çalıştırılmalıdır):

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<your-AAD-email>" --role "Azure Digital Twins Owner (Preview)"
```

Bu komutun sonucu oluşturulan rol ataması hakkında bilgi verilir.

> [!TIP]
> Bunun yerine *400: BadRequest* hatası alırsanız, Kullanıcı için *ObjectID* 'yi almak için aşağıdaki komutu çalıştırın:
> ```azurecli
> az ad user show --id <your-AAD-email> --query objectId
> ```
> Sonra, e-postanız yerine kullanıcının *nesne kimliğini* kullanarak rol atama komutunu tekrarlayın.

Artık bir Azure dijital TWINS örneğiniz var ve bunu yönetme izinleri var.

## <a name="next-steps"></a>Sonraki adımlar

Örneğiniz ile çalışmak üzere bir istemci uygulamasını ayarlama ve kimlik doğrulama hakkında bilgi için bkz..
* [Nasıl yapılır: istemci uygulamasının kimliğini doğrulama](how-to-authenticate-client.md)
