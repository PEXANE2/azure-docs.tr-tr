---
title: Azure dijital TWINS örneği oluşturma
titleSuffix: Azure Digital Twins
description: Bkz. Azure dijital TWINS hizmeti 'nin bir örneğini ayarlama.
author: baanders
ms.author: baanders
ms.date: 4/22/2020
ms.topic: how-to
ms.service: digital-twins
ms.openlocfilehash: f097861f97e7ec62019fa631e2ac608d2e0c5024
ms.sourcegitcommit: 1de57529ab349341447d77a0717f6ced5335074e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/09/2020
ms.locfileid: "84612876"
---
# <a name="set-up-an-azure-digital-twins-instance"></a>Azure dijital TWINS örneği ayarlama

Bu makale, yeni bir Azure dijital TWINS örneği kurmak için temel adımlarda size yol gösterir. Bu, örneğin örneğini oluşturmayı ve örneğe [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md) izinlerinin atanmasını içerir.

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

[!INCLUDE [Cloud Shell for Azure Digital Twins](../../includes/digital-twins-cloud-shell.md)]

## <a name="set-up-an-azure-digital-twins-instance"></a>Azure dijital TWINS örneği ayarlama

Ardından, bu nasıl yapılır, bu şekilde kullanılmak üzere yeni bir Azure Kaynak grubu oluşturmak için aşağıdaki komutları çalıştırın ve ardından bu kaynak grubunda Azure dijital TWINS 'in yeni bir örneğini oluşturun.

```azurecli
az group create --location <region> --name <name-for-your-resource-group>
az dt create --dt-name <name-for-your-Azure-Digital-Twins-instance> -g <your-resource-group> -l <region>
```

> [!TIP]
> Azure CLı 'de komutlara geçirilebilecek Azure bölge adlarının bir listesini çıkarmak için şu komutu çalıştırın:
> ```azurecli
> az account list-locations -o table
> ```
> Azure dijital TWINS 'i destekleyen bölgeleri görmek için [bölgeye göre kullanılabilen Azure ürünlerini](https://azure.microsoft.com/global-infrastructure/services/?products=digital-twins)ziyaret edin.

Bu komutların sonucu, oluşturduğunuz kaynaklarla ilgili bilgiler olarak şu şekilde görünür:

:::image type="content" source="media/how-to-set-up-instance/create-instance.png" alt-text="Kaynak grubu ve Azure dijital TWINS örneğinin başarıyla oluşturulmasıyla Komut penceresi":::

Azure dijital TWINS örneğinin *ana bilgisayar*adı, *adı*ve *kaynak grubu* 'ndan çıkış ' a bakın. Bunlar, kimlik doğrulama ve ilgili Azure kaynaklarını ayarlamak için Azure dijital TWINS örneğinizle çalışmaya devam ederken ihtiyacınız olabilecek tüm anahtar değerlerdir.

> [!TIP]
> Bu özellikleri, örneğinizin tüm özellikleriyle birlikte, çalıştırarak dilediğiniz zaman görebilirsiniz `az dt show --dt-name <your-Azure-Digital-Twins-instance>` .

### <a name="assign-azure-active-directory-permissions"></a>Azure Active Directory izinlerini ata

Azure dijital TWINS, rol tabanlı erişim denetimi (RBAC) için [Azure Active Directory (AAD)](../active-directory/fundamentals/active-directory-whatis.md) kullanır. Yani, Azure dijital TWINS örneğiniz için veri düzlemi çağrısı yapabilmeniz için önce bu izinlerle kendinize bir rol atamanız gerekir.

Azure dijital TWINS 'i bir istemci uygulamasıyla kullanabilmek için, istemci uygulamanızın Azure dijital TWINS 'de kimlik doğrulaması yapabilmesine de dikkat etmeniz gerekir. Bu, [nasıl yapılır: istemci uygulamasında kimlik doğrulama](how-to-authenticate-client.md)hakkında okuyabilmeniz için bir Azure ACTIVE DIRECTORY (AAD) uygulama kaydı ayarlanarak yapılır.

#### <a name="assign-yourself-a-role"></a>Kendinize bir rol atayın

Azure aboneliğinizdeki AAD kiracısı ile ilişkili e-postanızı kullanarak kendiniz için bir rol ataması oluşturun. İlk olarak, Azure aboneliğinizde bir sahip olarak sınıflandırdığınızdan emin olun. Daha sonra, Azure dijital TWINS örneğiniz için kullanıcıyı bir sahip rolüne atamak üzere aşağıdaki komutu kullanabilirsiniz:

```azurecli
az dt role-assignment create --dt-name <your-Azure-Digital-Twins-instance> --assignee "<your-AAD-email>" --role "Azure Digital Twins Owner (Preview)"
```

Bu komutun sonucu, oluşturduğunuz rol ataması hakkında bilgi verilir.

> [!TIP]
> Bunun yerine *400: BadRequest* hatası alırsanız, Kullanıcı için *ObjectID* 'yi almak için aşağıdaki komutu çalıştırın:
> ```azurecli
> az ad user show --id <your-AAD-email> --query objectId
> ```
> Sonra, e-postanız yerine kullanıcının *nesne kimliğini* kullanarak rol atama komutunu tekrarlayın.

Artık hazır bir Azure dijital TWINS örneğiniz var.

## <a name="next-steps"></a>Sonraki adımlar

Örneğiniz ile çalışmak üzere bir istemci uygulamasını ayarlama ve kimlik doğrulama hakkında bilgi için bkz..
* [Nasıl yapılır: istemci uygulamasının kimliğini doğrulama](how-to-authenticate-client.md)
