---
title: Azure Resource Manager şablonu kullanarak App Service uygulaması oluşturma
description: App Service dağıtmanın birçok yolu olan bir Azure Resource Manager şablonu kullanarak saniyeler içinde Azure App Service ilk uygulamanızı oluşturun.
author: msangapu-msft
ms.author: msangapu
ms.assetid: 582bb3c2-164b-42f5-b081-95bfcb7a502a
ms.topic: quickstart
ms.date: 05/25/2020
ms.custom: subject-armqs
zone_pivot_groups: app-service-platform-windows-linux
ms.openlocfilehash: e577616e0976ca050a55c8524e68129545ed1912
ms.sourcegitcommit: 3be3537ead3388a6810410dfbfe19fc210f89fec
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89653600"
---
# <a name="create-app-service-app-using-an-azure-resource-manager-template"></a>Azure Resource Manager şablonu kullanarak App Service uygulaması oluşturma

Azure Resource Manager şablonu ve Cloud Shell [Azure CLI](https://docs.microsoft.com/cli/azure/get-started-with-azure-cli) kullanarak buluta uygulama dağıtarak [Azure App Service](overview.md) kullanmaya başlayın. Ücretsiz bir App Service katmanı kullandığınız için, bu hızlı başlangıcı tamamlamaya yönelik bir ücret ödeirsiniz.

 [!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

## <a name="prerequisites"></a>Önkoşullar

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

## <a name="create-an-azure-app-service-app"></a>Azure App Service uygulaması oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

::: zone pivot="platform-windows"
Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/) alınmıştır. Bir App Service planını ve bir App Service uygulamasını Windows üzerinde dağıtır. .NET Core, .NET Framework, PHP, Node.js ve statik HTML uygulamalarıyla uyumludur. Java için bkz. [Java uygulaması oluşturma](app-service-web-get-started-java.md). 

[!code-json[<Azure Resource Manager template App Service Windows app>](~/quickstart-templates/101-app-service-docs-windows/azuredeploy.json)]

Şablonda iki Azure kaynağı tanımlanmıştır:

* [**Microsoft. Web/sunucugrupları**](/azure/templates/microsoft.web/serverfarms): App Service planı oluşturun.
* [**Microsoft. Web/Sites**](/azure/templates/microsoft.web/sites): App Service uygulaması oluşturun.

Bu şablon, kolaylık olması için önceden tanımlanmış birkaç parametre içerir. Parametre Varsayılanları ve bunların açıklamaları için aşağıdaki tabloya bakın:

| Parametreler | Tür    | Varsayılan değer                | Açıklama |
|------------|---------|------------------------------|-------------|
| webAppName | string  | "webApp- **[`<uniqueString>`](/azure/azure-resource-manager/templates/template-functions-string#uniquestring)** " | Uygulama adı |
| location   | string  | "[[resourceGroup (). Location](/azure/azure-resource-manager/templates/template-functions-resource#resourcegroup)]" | Uygulama bölgesi |
| isteyin        | string  | F1                         | Örnek boyutu (F1 = ücretsiz katman) |
| language   | string  | .net                       | Programlama dili yığını (.net, php, Node, HTML) |
| helloWorld | boolean | Yanlış                        | Doğru = "Merhaba Dünya" uygulamasını dağıt |
| repoUrl    | string  | " "                          | Dış git deposu (isteğe bağlı) |
::: zone-end
::: zone pivot="platform-linux"
Bu hızlı başlangıçta kullanılan şablon [Azure Hızlı Başlangıç şablonlarından](https://github.com/Azure/azure-quickstart-templates/) alınmıştır. Linux üzerinde bir App Service planı ve bir App Service uygulaması dağıtır. App Service tüm desteklenen programlama dilleri ile uyumludur.

[!code-json[<Azure Resource Manager template App Service Linux app>](~/quickstart-templates/101-app-service-docs-linux/azuredeploy.json)]

Şablonda iki Azure kaynağı tanımlanmıştır:

* [**Microsoft. Web/sunucugrupları**](/azure/templates/microsoft.web/serverfarms): App Service planı oluşturun.
* [**Microsoft. Web/Sites**](/azure/templates/microsoft.web/sites): App Service uygulaması oluşturun.

Bu şablon, kolaylık olması için önceden tanımlanmış birkaç parametre içerir. Parametre Varsayılanları ve bunların açıklamaları için aşağıdaki tabloya bakın:

| Parametreler | Tür    | Varsayılan değer                | Açıklama |
|------------|---------|------------------------------|-------------|
| webAppName | string  | "webApp- **[`<uniqueString>`](/azure/azure-resource-manager/templates/template-functions-string#uniquestring)** " | Uygulama adı |
| location   | string  | "[[resourceGroup (). Location](/azure/azure-resource-manager/templates/template-functions-resource#resourcegroup)]" | Uygulama bölgesi |
| isteyin        | string  | F1                         | Örnek boyutu (F1 = ücretsiz katman) |
| linuxFxVersion   | string  | "DOTNETCORE&#124;3,0        | "Programlama dil yığını &#124; sürümü" |
| repoUrl    | string  | " "                          | Dış git deposu (isteğe bağlı) |

---
::: zone-end


### <a name="deploy-the-template"></a>Şablonu dağıtma

Azure CLı, şablonu dağıtmak için burada kullanılır. Azure portal, Azure PowerShell ve REST API de kullanabilirsiniz. Diğer dağıtım yöntemlerini öğrenmek için bkz. [şablonları dağıtma](../azure-resource-manager/templates/deploy-powershell.md). 

Aşağıdaki kod, bir kaynak grubu, bir App Service planı ve bir Web uygulaması oluşturur. Sizin için varsayılan bir kaynak grubu, App Service planı ve konum ayarlanmış. `<app-name>`Genel olarak benzersiz bir uygulama adıyla değiştirin (geçerli karakterler `a-z` , `0-9` ve `-` ).

::: zone pivot="platform-windows"
Windows 'a bir .NET Framework uygulaması dağıtmak için aşağıdaki kodu çalıştırın.

```azurecli-interactive
az group create --name myResourceGroup --location "southcentralus" &&
az deployment group create --resource-group myResourceGroup \
--parameters language=".net" sample="true" webAppName="<app-name>" \
--template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-app-service-docs-windows/azuredeploy.json"
::: zone-end
::: zone pivot="platform-linux"
Run the code below to create a Python app on Linux. 

```azurecli-interactive
az group create --name myResourceGroup --location "southcentralus" &&
az deployment group create --resource-group myResourceGroup --parameters webAppName="<app-name>" linuxFxVersion="PYTHON|3.7" \
--template-uri "https://raw.githubusercontent.com/Azure/azure-quickstart-templates/master/101-app-service-docs-linux/azuredeploy.json"
```

Farklı bir dil yığınını dağıtmak için `linuxFxVersion` uygun değerlerle güncelleştirin. Örnekler aşağıda gösterilmiştir. Geçerli sürümleri göstermek için Cloud Shell aşağıdaki komutu çalıştırın: `az webapp config show --resource-group myResourceGroup --name <app-name> --query linuxFxVersion`

| Dil    | Örnek                                              |
|-------------|------------------------------------------------------|
| **.NET**    | linuxFxVersion = "DOTNETCORE&#124;3,0"                 |
| **PHP**     | linuxFxVersion = "PHP&#124;7,4"                        |
| **Node.js** | linuxFxVersion = "NODE&#124;10,15"                     |
| **Java**    | linuxFxVersion = "JAVA&#124;1,8 &#124;TOMCAT&#124;9,0" |
| **Python**  | linuxFxVersion = "PYTHON&#124;3,7"                     |
| **Ruby**    | linuxFxVersion = "RUBY&#124;2,6"                       |

---
::: zone-end

> [!NOTE]
> Daha fazla [Azure App Service şablon örneği](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Sites)bulabilirsiniz.


## <a name="validate-the-deployment"></a>Dağıtımı doğrulama

' A gidin `http://<app_name>.azurewebsites.net/` ve oluşturulduğunu doğrulayın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli olmadığında [kaynak grubunu silin](../azure-resource-manager/management/delete-resource-group.md?tabs=azure-portal#delete-resource-group).

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Yerel Git’ten dağıtım](deploy-local-git.md)

> [!div class="nextstepaction"]
> [SQL Veritabanı ile ASP.NET Core](tutorial-dotnetcore-sqldb-app.md)

> [!div class="nextstepaction"]
> [Postgres ile Python](tutorial-python-postgresql-app.md)

> [!div class="nextstepaction"]
> [MySQL ile PHP](tutorial-php-mysql-app.md)

> [!div class="nextstepaction"]
> [Java ile Azure SQL veritabanı 'na bağlanma](/azure/sql-database/sql-database-connect-query-java?toc=%2Fazure%2Fjava%2Ftoc.json)

> [!div class="nextstepaction"]
> [Özel etki alanı eşleme](app-service-web-tutorial-custom-domain.md)