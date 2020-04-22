---
title: Quickstart - Kaynak Yöneticisi şablonlarını kullanarak profil ve bitiş noktası oluşturma
titleSuffix: Azure Content Delivery Network
description: Kaynak Yöneticisi şablonlarını kullanarak Azure İçerik Teslim Ağı profili ve bitiş noktası oluşturmayı öğrenin
services: cdn
documentationcenter: ''
author: senthuransivananthan
manager: danielgi
editor: ''
ms.assetid: cbc2ff98-916d-4339-8959-622823c5b772
ms.service: azure-cdn
ms.workload: ''
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 03/05/2019
ms.author: allensu
ms.custom: mvc
ms.openlocfilehash: dfd7b933502b96c0952a24dbee563e9b537dcdd8
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81683467"
---
# <a name="quickstart-create-an-azure-cdn-profile-and-endpoint-using-resource-manager-template"></a>Hızlı başlatma: Kaynak Yöneticisi şablonu kullanarak Bir Azure CDN profili ve bitiş noktası oluşturma

Bu hızlı başlangıçta, CLI'yi kullanarak bir Azure Kaynak Yöneticisi şablonu dağıtırsınız. Oluşturduğunuz şablon, web uygulamanızın önüne bir CDN profili ve CDN bitiş noktası dağıtır.
Bu adımları tamamlamak için yaklaşık on dakika sürer.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="prequisites"></a>Ön Koşullar

Bu hızlı başlangıç amacıyla, Origin olarak kullanmak için bir Web Uygulaması olması gerekir. Bu hızlı başlatmada kullanılan örnek Web Uygulaması,https://cdndemo.azurewebsites.net

Daha fazla bilgi için [azure'da statik bir HTML web uygulaması oluşturma 'na](https://docs.microsoft.com/azure/app-service/app-service-web-get-started-html)bakın.

## <a name="create-a-resource-group"></a>Kaynak grubu oluşturma

Tüm kaynaklar aynı kaynak grubunda dağıtılmalıdır.

Seçtiğiniz konumda kaynak grubu oluşturun. Bu örnek, Doğu ABD konumunda cdn adlı bir kaynak grubunun oluşturulmasını gösterir.

```azurecli-interactive
az group create --name cdn --location eastus
```

![Yeni Kaynak Grubu](./media/create-profile-resource-manager-template/cdn-create-resource-group.png)

## <a name="create-the-resource-manager-template"></a>Resource Manager şablonu oluşturma

Bu adımda, kaynakları dağıtan bir şablon dosyası oluşturursunuz.

Bu örnek, Genel Web Sitesi Hızlandırma senaryosunda yürürken, yapılandırılabilen birçok ayar vardır. Bu ayarlar Azure Kaynak Yöneticisi şablon uyşunda kullanılabilir. Lütfen [CDN Profili](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles) ve [CDN Profil Bitiş Noktası](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles/endpoints)için referanslar bakın.

Microsoft CDN'nin içerik türü listesini değiştirmeyi desteklemediğini unutmayın.

Şablonu **kaynak yöneticisi-cdn.json**olarak kaydedin.

```json
{
    "$schema": "https://schema.management.azure.com/schemas/2015-01-01/deploymentTemplate.json#",
    "contentVersion": "1.0.0.0",
    "parameters": {
        "cdnProfileSku": {
            "type": "string",
            "allowedValues": [
                "Standard_Microsoft",
                "Standard_Akamai",
                "Standard_Verizon",
                "Premium_Verizon"
            ]
        },
        "endpointOriginHostName": {
            "type": "string"
        }
    },
    "variables": {
        "profile": {
            "name": "[replace(toLower(parameters('cdnProfileSku')), '_', '-')]"
        },
        "endpoint": {
            "name": "[replace(toLower(parameters('endpointOriginHostName')), '.', '-')]",
            "originHostName": "[parameters('endpointOriginHostName')]"
        }
    },
    "resources": [
        {
            "type": "Microsoft.Cdn/profiles",
            "apiVersion": "2017-10-12",
            "location": "[resourceGroup().location]",
            "name": "[variables('profile').name]",
            "sku": {
                "name": "[parameters('cdnProfileSku')]"
            }
        },
        {
            "dependsOn": [
                "[resourceId('Microsoft.Cdn/profiles', variables('profile').name)]"
            ],
            "type": "Microsoft.Cdn/profiles/endpoints",
            "apiVersion": "2017-10-12",
            "location": "[resourceGroup().location]",
            "name": "[concat(variables('profile').name, '/', variables('endpoint').name)]",
            "properties": {
                "hostName": "[concat(variables('endpoint').name, '.azureedge.net')]",
                "originHostHeader": "[variables('endpoint').originHostName]",
                "isHttpAllowed": true,
                "isHttpsAllowed": true,
                "queryStringCachingBehavior": "IgnoreQueryString",
                "origins": [
                    {
                        "name": "[replace(variables('endpoint').originHostName, '.', '-')]",
                        "properties": {
                            "hostName": "[variables('endpoint').originHostName]",
                            "httpPort": 80,
                            "httpsPort": 443
                        }
                    }
                ],
                "contentTypesToCompress": [
                    "application/eot",
                    "application/font",
                    "application/font-sfnt",
                    "application/javascript",
                    "application/json",
                    "application/opentype",
                    "application/otf",
                    "application/pkcs7-mime",
                    "application/truetype",
                    "application/ttf",
                    "application/vnd.ms-fontobject",
                    "application/xhtml+xml",
                    "application/xml",
                    "application/xml+rss",
                    "application/x-font-opentype",
                    "application/x-font-truetype",
                    "application/x-font-ttf",
                    "application/x-httpd-cgi",
                    "application/x-javascript",
                    "application/x-mpegurl",
                    "application/x-opentype",
                    "application/x-otf",
                    "application/x-perl",
                    "application/x-ttf",
                    "font/eot",
                    "font/ttf",
                    "font/otf",
                    "font/opentype",
                    "image/svg+xml",
                    "text/css",
                    "text/csv",
                    "text/html",
                    "text/javascript",
                    "text/js",
                    "text/plain",
                    "text/richtext",
                    "text/tab-separated-values",
                    "text/xml",
                    "text/x-script",
                    "text/x-component",
                    "text/x-java-source"
                ],
                "isCompressionEnabled": true,
                "optimizationType": "GeneralWebDelivery"
            }
        }
    ],
    "outputs": {
        "cdnUrl": {
            "type": "string",
            "value": "[concat('https://', variables('endpoint').name, '.azureedge.net')]"
        }
    }
}
```

## <a name="create-the-resources"></a>Kaynakları oluşturma

Azure CLI'yi kullanarak şablonu dağıtın. 2 giriş için istenir:

**cdnProfileSku** - kullanmak istediğiniz CDN sağlayıcısı. Seçenekler şunlardır:

* Standard_Microsoft
* Standard_Akamai
* Standard_Verizon
* Premium_Verizon.

**endpointOriginHostName** - CDN üzerinden sunulacak bitiş noktası, örneğin, cdndemo.azurewebsites.net.

```azurecli-interactive
az group deployment create --resource-group cdn --template-file arm-cdn.json
```

![Kaynak Yöneticisi Şablonu Dağıtma](./media/create-profile-resource-manager-template/cdn-deploy-resource-manager.png)

## <a name="view-the-cdn-profile"></a>CDN profilini görüntüle

```azurecli-interactive
az cdn profile list --resource-group cdn -o table
```

![CDN Profilini Görüntüle](./media/create-profile-resource-manager-template/cdn-view-profile.png)

## <a name="view-the-cdn-endpoint-for-the-profile-standard-microsoft"></a>Standart profil için CDN Bitiş Noktasını görüntüleyin

```azurecli-interactive
az cdn endpoint list --profile-name standard-microsoft --resource-group cdn -o table
```

![CDN Bitiş Noktasını Görüntüle](./media/create-profile-resource-manager-template/cdn-view-endpoint.png)

İçeriği görüntülemek için HostName'yi kullanın. Örneğin, https:\//cdndemo-azurewebsites-net.azureedge.net tarayıcınızı kullanarak erişin.

## <a name="clean-up"></a>Temizleme

Kaynak grubunun silinmesi, kaynak grubunda dağıtılan tüm kaynakları otomatik olarak kaldırır.

```azurecli-interactive
az group delete --name cdn
```

![Kaynak Grubunu Sil](./media/create-profile-resource-manager-template/cdn-delete-resource-group.png)

## <a name="references"></a>Başvurular

* CDN Profili - [Azure Kaynak Yöneticisi Şablon Başvurusu](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles)
* CDN Endpoint - [Azure Kaynak Yöneticisi ŞablonU Başvuru Belgeleri](https://docs.microsoft.com/azure/templates/microsoft.cdn/2017-10-12/profiles/endpoints)

## <a name="next-steps"></a>Sonraki adımlar

CDN uç noktanıza özel etki alanı ekleme hakkında bilgi edinmek için aşağıdaki öğreticiye bakın:

> [!div class="nextstepaction"]
> [Öğretici: Azure CDN uç noktanıza özel etki alanı ekleme](cdn-map-content-to-custom-domain.md)
