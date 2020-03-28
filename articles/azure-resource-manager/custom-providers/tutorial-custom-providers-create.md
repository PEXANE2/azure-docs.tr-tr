---
title: Özel bir sağlayıcı oluşturma ve kullanma
description: Bu öğretici, bir Azure Özel Sağlayıcısı'nın nasıl oluşturulup kullanılacağını gösterir. Azure'da iş akışlarını değiştirmek için özel sağlayıcıları kullanın.
author: jjbfour
ms.topic: tutorial
ms.date: 06/19/2019
ms.author: jobreen
ms.openlocfilehash: 09df78955de6423244c2d8ec94e1e1c06ecab257
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "75650033"
---
# <a name="create-and-use-a-custom-provider"></a>Özel bir sağlayıcı oluşturma ve kullanma

Özel sağlayıcı, Azure ile bitiş noktası arasındaki bir sözleşmedir. Özel sağlayıcılarla Azure'da iş akışlarını değiştirebilirsiniz. Bu öğretici, özel bir sağlayıcı oluşturma işlemini gösterir. Azure Özel Sağlayıcıları'nı bilmiyorsanız, [Azure Özel Kaynak Sağlayıcıları'na genel bakış bölümüne](overview.md)bakın.

## <a name="create-a-custom-provider"></a>Özel bir sağlayıcı oluşturma

> [!NOTE]
> Bu öğretici, bir bitiş noktasının nasıl yazılabildiğini göstermez. Eğer dinlendirici bir bitiş noktanız yoksa, geçerli öğreticinin temeli olan [RESTful uç noktalarının yazlanması](./tutorial-custom-providers-function-authoring.md)yla ilgili öğreticiyi izleyin.

Bir bitiş noktası oluşturduktan sonra, sağlayıcı ve bitiş noktası arasında bir sözleşme oluşturmak için özel bir sağlayıcı oluşturabilirsiniz. Özel bir sağlayıcıyla, uç nokta tanımlarının bir listesini belirtebilirsiniz:

```JSON
{
  "name": "myEndpointDefinition",
  "routingType": "Proxy",
  "endpoint": "https://<yourapp>.azurewebsites.net/api/<funcname>?code=<functionkey>"
}
```

Özellik | Gerekli | Açıklama
---|---|---
**Adı** | Evet | Bitiş noktası tanımının adı. Azure bu adı API aracılığıyla /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders altında ortaya çıkarır<br>/resourceProviders/{resourceProviderName}/{endpointDefinitionName}
**yönlendirmeTürü** | Hayır | Uç nokta sözleşme türü. Değer belirtilmemişse, varsayılan olarak "Proxy" olarak adlandırılır.
**Bitiş noktası** | Evet | İstekleri yönlendirmek için bitiş noktası. Bu uç nokta yanıtı ve isteğin herhangi bir yan etkileri işler.

**Bitiş noktasının** değeri, Azure işlev uygulamasının tetikleyici URL'sidir. , `<yourapp>` `<funcname>`, `<functionkey>` ve yer tutucular oluşturulan işlev uygulamanız için değerlerle değiştirilmelidir.

## <a name="define-custom-actions-and-resources"></a>Özel eylemleri ve kaynakları tanımlama

Özel **sağlayıcı, eylemler** ve **kaynak Türleri** özellikleri altında modellenen uç nokta tanımlarının bir listesini içerir. **Eylemler** özelliği özel sağlayıcı tarafından maruz özel eylemlereşler ve **kaynak Türleri** özelliği özel kaynaklardır. Bu öğreticide, özel sağlayıcı **actions** adlı `myCustomAction` bir eylem özelliği `myCustomResources`ve bir kaynak **Türleri** özelliği adlı vardır.

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
> **Bitiş noktası** değerlerini önceki öğreticide oluşturulan işlev uygulamasından tetikleyici URL ile değiştirmeniz gerekir.

Azure Kaynak Yöneticisi şablonu kullanarak önceki özel sağlayıcıyı dağıtabilirsiniz:

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

## <a name="use-custom-actions-and-resources"></a>Özel eylemler ve kaynaklar kullanma

Özel bir sağlayıcı oluşturduktan sonra, yeni Azure API'lerini kullanabilirsiniz. Aşağıdaki sekmeler, özel bir sağlayıcının nasıl arayacağını ve kullanılacağını açıklar.

### <a name="custom-actions"></a>Özel eylemler

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!NOTE]
> Yer `{subscriptionId}` tutucuları, `{resourceGroupName}` özel sağlayıcıyı dağıttığınız abonelik ve kaynak grubuyla değiştirmeniz gerekir.

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
*Eylem* | Evet | Özel sağlayıcıda tanımlanan eylemin adı
*Kimlik* | Evet | Özel sağlayıcının kaynak kimliği
*istek-gövde* | Hayır | Bitiş noktasına gönderilecek istek gövdesi

# <a name="template"></a>[Şablon](#tab/template)

Yok.

---

### <a name="custom-resources"></a>Özel kaynaklar

# <a name="azure-cli"></a>[Azure CLI](#tab/azure-cli)

> [!NOTE]
> Yer `{subscriptionId}` tutucuları, `{resourceGroupName}` özel sağlayıcıyı dağıttığınız abonelik ve kaynak grubuyla değiştirmeniz gerekir.

#### <a name="create-a-custom-resource"></a>Özel kaynak oluşturma

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
*tam nesne* | Evet | Özellikleri nesne konum, etiketler, SKU veya plan gibi diğer seçenekleri içerip içermediğini gösterir.
*Kimliği* | Evet | Özel kaynağın kaynak kimliği. Bu kimlik, özel sağlayıcı kaynak kimliğinin bir uzantısıdır.
*Özellikler* | Evet | Bitiş noktasına gönderilecek istek gövdesi.

#### <a name="delete-a-custom-resource"></a>Özel bir kaynağı silme

```azurecli-interactive
az resource delete --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parametre | Gerekli | Açıklama
---|---|---
*Kimliği* | Evet | Özel kaynağın kaynak kimliği. Bu kimlik, özel sağlayıcı kaynak kimliğinin bir uzantısıdır.

#### <a name="retrieve-a-custom-resource"></a>Özel bir kaynak alma

```azurecli-interactive
az resource show --id /subscriptions/{subscriptionId}/resourceGroups/{resourceGroupName}/providers/Microsoft.CustomProviders/resourceProviders/myCustomProvider/myCustomResources/myTestResourceName1
```

Parametre | Gerekli | Açıklama
---|---|---
*Kimliği* | Evet | Özel kaynağın kaynak kimliği. Bu kimlik, özel sağlayıcı kaynak kimliğinin bir uzantısıdır.

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
*kaynakTypeName* | Evet | Özel `name` sağlayıcıda tanımlanan **kaynak Türleri** özelliğinin değeri.
*kaynakProviderName* | Evet | Özel sağlayıcı örnek adı.
*özelResourceName* | Evet | Özel kaynak adı.

---

> [!NOTE]
> Özel sağlayıcıyı dağıtmayı ve kullanmayı bitirdikten sonra, Azure işlev uygulaması da dahil olmak üzere oluşturulan tüm kaynakları temizlemeyi unutmayın.

## <a name="next-steps"></a>Sonraki adımlar

Bu makalede, özel sağlayıcılar hakkında öğrendim. Daha fazla bilgi için bkz.

- [Nasıl yapilir: Azure REST API'sine özel eylemler ekleme](./custom-providers-action-endpoint-how-to.md)
- [Nasıl yapilir: Azure REST API'sine özel kaynak ekleme](./custom-providers-resources-endpoint-how-to.md)
