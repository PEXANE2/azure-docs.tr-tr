---
title: Özel bir sağlayıcı oluşturma ve kullanma
description: Bu öğreticide, bir Azure özel sağlayıcısı oluşturma ve kullanma işlemlerinin nasıl yapılacağı gösterilmektedir. Azure 'da iş akışlarını değiştirmek için özel sağlayıcılar kullanın.
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 09df78955de6423244c2d8ec94e1e1c06ecab257
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75650033"
---
# <a name="create-and-use-a-custom-provider"></a>Özel bir sağlayıcı oluşturma ve kullanma

Özel sağlayıcı, Azure ile uç nokta arasında bir sözleşmedir. Özel sağlayıcılar sayesinde Azure 'da iş akışlarını değiştirebilirsiniz. Bu öğreticide, özel bir sağlayıcı oluşturma işlemi gösterilir. Azure özel sağlayıcıları hakkında bilginiz varsa bkz. [Azure özel kaynak sağlayıcılarına genel bakış](overview.md).

## <a name="create-a-custom-provider"></a>Özel sağlayıcı oluşturma

> [!NOTE]
> Bu öğretici, bir uç noktanın nasıl yazılacağını göstermez. Daha iyi bir uç noktalarınız yoksa, geçerli öğreticinin temelini oluşturan, [tekrar eden uç noktalar yazma öğreticisini](./tutorial-custom-providers-function-authoring.md)izleyin.

Bir uç nokta oluşturduktan sonra, sağlayıcı ve uç nokta arasında bir sözleşme oluşturmak için özel bir sağlayıcı oluşturabilirsiniz. Özel bir sağlayıcıyla, uç nokta tanımlarının bir listesini belirtebilirsiniz:

```JSON
{
  "name": "myEndpointDefinition",
  "routingType": "Proxy",
  "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
}
```

Özellik | Gerekli | Açıklama
---|---|---
**ada** | Yes | Uç nokta tanımının adı. Azure, bu adı API aracılığıyla/subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders altında kullanıma sunar<br>/resourceProviders/{resourceProviderName}/{endpointDefinitionName}
**routingType** | Hayır | Uç nokta sözleşme türü. Değer belirtilmezse, varsayılan olarak "proxy" olur.
**bkz** | Yes | İsteklerin yönlendirileceği uç nokta. Bu uç nokta, isteğin yanıtını ve tüm yan etkilerini işler.

**Uç noktanın** değeri, Azure işlevi uygulamasının tetikleyici URL 'sidir. `<yourapp>`, `<funcname>`, Ve `<functionkey>` yer tutucular, oluşturduğunuz işlev uygulamanızın değerleriyle değiştirilmelidir.

## <a name="define-custom-actions-and-resources"></a>Özel eylemleri ve kaynakları tanımlama

Özel sağlayıcı, **Eylemler** ve **Resourcetypes** özellikleri kapsamında modellenen uç nokta tanımlarının bir listesini içerir. **Actions** özelliği özel sağlayıcı tarafından kullanıma sunulan özel eylemlerle eşlenir ve **Resourcetypes** özelliği özel kaynaklarıdır. Bu öğreticide, Özel sağlayıcının adlı bir **Actions** özelliği `myCustomAction` ve **resourcetypes** özelliği adlı `myCustomResources`bir.

```JSON
{
  "properties": {
    "actions": [
      {
        "name": "myCustomAction",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ],
    "resourceTypes": [
      {
        "name": "myCustomResources",
        "routingType": "Proxy",
        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
      }
    ]
  },
  "location": "eastus"
}
```

## <a name="deploy-the-custom-provider"></a>Özel sağlayıcıyı dağıtma

> [!NOTE]
> **Uç nokta** değerlerini, önceki öğreticide oluşturulan işlev UYGULAMASıNDAKI tetikleyici URL 'siyle değiştirmelisiniz.

Önceki özel sağlayıcıyı bir Azure Resource Manager şablonu kullanarak dağıtabilirsiniz:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders",
            "name": "myCustomProvider",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "actions": [
                    {
                        "name": "myCustomAction",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ],
                "resourceTypes": [
                    {
                        "name": "myCustomResources",
                        "routingType": "Proxy",
                        "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
                    }
                ]
            }
        }
    ]
}
```

## <a name="use-custom-actions-and-resources"></a>Özel eylemleri ve kaynakları kullanma

Özel bir sağlayıcı oluşturduktan sonra yeni Azure API 'Lerini kullanabilirsiniz. Aşağıdaki sekmelerde özel bir sağlayıcının nasıl çağrılacağını ve kullanılacağı açıklanmaktadır.

### <a name="custom-actions"></a>Özel Eylemler

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!NOTE]
> Ve `{resourceGroupName}` yer tutucuları, `{subscriptionId}` özel sağlayıcıyı dağıttığınız aboneliğin ve kaynak grubuyla değiştirmelisiniz.

```azurecli-interactive
az resource invoke-action --action myCustomAction \
                          --ids /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider \
                          --request-body
                            '{
                                "hello": "world"
                            }'
```

Parametre | Gerekli | Açıklama
---|---|---
*ön* | Yes | Özel sağlayıcıda tanımlanan eylemin adı
*ayrılacak* | Yes | Özel sağlayıcının kaynak KIMLIĞI
*İstek gövdesi* | Hayır | Uç noktaya gönderilecek istek gövdesi

# <a name="template"></a>[Şablon](#tab/template)

Yok.

---

### <a name="custom-resources"></a>Özel kaynaklar

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!NOTE]
> Ve `{resourceGroupName}` yer tutucuları, `{subscriptionId}` özel sağlayıcıyı dağıttığınız aboneliğin ve kaynak grubuyla değiştirmelisiniz.

#### <a name="create-a-custom-resource"></a>Özel bir kaynak oluşturun

```azurecli-interactive
az resource create --is-full-object \
                   --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1 \
                   --properties
                    '{
                        "location": "eastus",
                        "properties": {
                            "hello" : "world"
                        }
                    }'
```

Parametre | Gerekli | Açıklama
---|---|---
*-Full-Object* | Yes | Özellikler nesnesinin konum, Etiketler, SKU veya plan gibi diğer seçenekleri içerip içermediğini gösterir.
*numarasını* | Yes | Özel kaynağın kaynak KIMLIĞI. Bu KIMLIK özel sağlayıcı kaynak KIMLIĞININ bir uzantısıdır.
*özelliklerinin* | Yes | Uç noktaya gönderilecek istek gövdesi.

#### <a name="delete-a-custom-resource"></a>Özel bir kaynağı silme

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parametre | Gerekli | Açıklama
---|---|---
*numarasını* | Yes | Özel kaynağın kaynak KIMLIĞI. Bu KIMLIK özel sağlayıcı kaynak KIMLIĞININ bir uzantısıdır.

#### <a name="retrieve-a-custom-resource"></a>Özel bir kaynak alma

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parametre | Gerekli | Açıklama
---|---|---
*numarasını* | Yes | Özel kaynağın kaynak KIMLIĞI. Bu KIMLIK özel sağlayıcı kaynak KIMLIĞININ bir uzantısıdır.

# <a name="template"></a>[Şablon](#tab/template)

Örnek bir Kaynak Yöneticisi şablonu:

```JSON
{
    "$schema": "http://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "resources": [
        {
            "type": "Microsoft.CustomProviders/resourceProviders/myCustomResources",
            "name": "myCustomProvider/myTestResourceName1",
            "apiVersion": "2018-09-01-preview",
            "location": "eastus",
            "properties": {
                "hello": "world"
            }
        }
    ]
}
```

Parametre | Gerekli | Açıklama
---|---|---
*resourceTypeName* | Yes | Özel `name` sağlayıcıda tanımlanan **Resourcetypes** özelliğinin değeri.
*resourceProviderName* | Yes | Özel sağlayıcı örneği adı.
*customResourceName* | Yes | Özel kaynak adı.

---

> [!NOTE]
> Özel sağlayıcıyı dağıtma ve kullanma işini tamamladıktan sonra, Azure işlevi uygulaması da dahil olmak üzere tüm oluşturulan kaynakları temizlemeyi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcılar hakkında bilgi edindiniz. Daha fazla bilgi için bkz.

- [Nasıl yapılır: Azure REST API özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapılır: Azure REST API özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
