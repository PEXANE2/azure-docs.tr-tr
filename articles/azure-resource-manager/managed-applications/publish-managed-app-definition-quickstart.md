---
title: Yönetilen uygulama tanımı oluştur
description: Kuruluşunuzun üyelerine yönelik bir Azure yönetilen uygulaması oluşturmayı gösterir.
author: tfitzmac
ms.topic: quickstart
ms.date: 09/13/2019
ms.author: tomfitz
ms.openlocfilehash: 11d4eb396d332327addcd85ed7badd36ef8f1d10
ms.sourcegitcommit: f788bc6bc524516f186386376ca6651ce80f334d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/03/2020
ms.locfileid: "75650348"
---
# <a name="publish-an-azure-managed-application-definition"></a>Azure yönetilen uygulaması tanımı yayımlama

Bu hızlı başlangıçta yönetilen uygulamalarla çalışmaya giriş bilgileri verilmektedir. Kuruluşunuzdaki kullanıcılar için bir iç kataloğa yönetilen uygulama tanımı eklersiniz. Girişi basitleştirmek amacıyla, yönetilen uygulamanız için dosyaları daha önce derledik. Bu dosyalar GitHub’da bulunabilir. [Hizmet kataloğu uygulaması oluşturma](publish-service-catalog-app.md) öğreticisinde bu dosyaların nasıl derlendiğini öğrenebilirsiniz.

İşlemi tamamladığınızda yönetilen uygulama tanımına sahip olacak **appDefinitionGroup** adlı bir kaynak grubuna sahip olacaksınız.

[!INCLUDE [cloud-shell-try-it.md](../../../includes/cloud-shell-try-it.md)]

## <a name="create-a-resource-group-for-definition"></a>Tanımınız için kaynak grubu oluşturma

Yönetilen uygulama tanımınız bir kaynak grubunda mevcuttur. Kaynak grubu, Azure kaynaklarının dağıtıldığı ve yönetildiği bir mantıksal koleksiyondur.

Kaynak grubu oluşturmak için aşağıdaki komutu kullanın:

```azurecli-interactive
az group create --name appDefinitionGroup --location westcentralus
```

## <a name="create-the-managed-application-definition"></a>Yönetilen uygulama tanımı oluşturma

Yönetilen uygulama tanımlarken bir kullanıcı, grup veya tüketici için kaynakları yöneten bir uygulama seçersiniz. Bu kimlik, atanan role göre yönetilen kaynak grubu üzerinde izinlere sahiptir. Genellikle, kaynakları yönetmek için bir Azure Active Directory grubu oluşturursunuz. Ancak, bu makalede kendi kimliğinizi kullanırsınız.

Kimliğinizin nesne kimliğini almak için aşağıdaki komutta kullanıcı asıl adınızı sağlayın:

```azurecli-interactive
userid=$(az ad user show --id example@contoso.org --query objectId --output tsv)
```

Bundan sonra, kullanıcıya erişim izni vermek istediğiniz RBAC yerleşik rolünün rol tanımı kimliği gerekir. Aşağıdaki komut, Sahip rolünün rol tanımı kimliğinin nasıl alınacağını gösterir:

```azurecli-interactive
roleid=$(az role definition list --name Owner --query [].name --output tsv)
```

Şimdi, yönetilen uygulama tanımı kaynağını oluşturun. Yönetilen uygulama yalnızca bir depolama hesabı içerir.

```azurecli-interactive
az managedapp definition create \
  --name "ManagedStorage" \
  --location "westcentralus" \
  --resource-group appDefinitionGroup \
  --lock-level ReadOnly \
  --display-name "Managed Storage Account" \
  --description "Managed Azure Storage Account" \
  --authorizations "$userid:$roleid" \
  --package-file-uri "https://github.com/Azure/azure-managedapp-samples/raw/master/Managed%20Application%20Sample%20Packages/201-managed-storage-account/managedstorage.zip"
```

Komut tamamlandığında, kaynak grubunuzda bir yönetilen uygulamanız olur. 

Yukarıdaki örnekte kullanılan parametrelerden bazıları şunlardır:

* **resource-group**: Yönetilen uygulama tanımının oluşturulduğu kaynak grubunun adı.
* **lock-level**: Yönetilen kaynak grubuna yerleştirilen kilit türü. Müşterinin bu kaynak grubunda istenmeyen işlemler gerçekleştirmesini engeller. ReadOnly şu anda desteklenen tek kilit düzeyidir. ReadOnly belirtildiğinde müşteri yalnızca yönetilen kaynak grubunda mevcut olan kaynakları okuyabilir. Yönetilen kaynak grubuna erişim izni verilen yayımcı kimlikleri kilitli olmaz.
* **authorizations**: Yönetilen kaynak grubuna izin vermek için kullanılan sorumlu kimliğini ve rol tanımı kimliğini açıklar. `<principalId>:<roleDefinitionId>` biçiminde belirtilir. Birden fazla değer kullanacaksanız `<principalId1>:<roleDefinitionId1> <principalId2>:<roleDefinitionId2>` biçiminde belirtin. Değerler boşlukla ayrılır.
* **package-file-uri**: Gerekli dosyaları içeren .zip paketinin konumu. Paket **mainTemplate.json** ve **createUiDefinition.json** dosyalarını içermelidir. **mainTemplate.json**, yönetilen uygulamanın bir parçası olarak oluşturulan Azure kaynaklarını tanımlar. Şablon normal bir Resource Manager şablonundan farklı değildir. **createUiDefinition.json**, portal üzerinden yönetilen uygulamayı oluşturan kullanıcılar için kullanıcı arabirimi oluşturur.

## <a name="next-steps"></a>Sonraki adımlar

Yönetilen uygulama tanımını yayımladınız. Şimdi bu tanımın bir örneğini nasıl dağıtacağınızı öğrenin.

> [!div class="nextstepaction"]
> [Hızlı başlangıç: Hizmet kataloğu uygulaması dağıtma](deploy-service-catalog-quickstart.md)