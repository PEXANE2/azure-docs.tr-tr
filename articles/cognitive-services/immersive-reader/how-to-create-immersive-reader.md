---
title: Tam Ekran Okuyucu Kaynağı oluşturma
titleSuffix: Azure Cognitive Services
description: Bu makalede, özel bir alt etki alanına sahip yeni bir Sürükleyici Okuyucu kaynağının nasıl oluşturulacağı ve azure kiracınızda Azure AD'yi nasıl yapılandıracağı gösterilmektedir.
services: cognitive-services
author: rwaller
manager: guillasi
ms.service: cognitive-services
ms.subservice: immersive-reader
ms.topic: conceptual
ms.date: 07/22/2019
ms.author: rwaller
ms.openlocfilehash: 41efe4592c65ae3cdd85ce1b212554e50691905a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78330728"
---
# <a name="create-an-immersive-reader-resource-and-configure-azure-active-directory-authentication"></a>Sürükleyici Bir Reader kaynağı oluşturun ve Azure Etkin Dizin kimlik doğrulaması yapılandırın

Bu makalede, Sürükleyici Bir Okuyucu kaynağı oluşturacak ve Azure Etkin Dizin (Azure AD) kimlik doğrulaması yapılandıracak bir komut dosyası salıyoruz. İster bu komut dosyasında ister portalda olsun, Her Sürükleyici Okuyucu kaynağı oluşturulduğunda, azure AD izinleriyle de yapılandırılmalıdır. Bu senaryo bu nda sana yardımcı olacak.

Komut dosyası, sizin için gerekli tüm Sürükleyici Okuyucu ve Azure AD kaynaklarını tek adımda oluşturmak ve yapılandırmak için tasarlanmıştır. Ancak, örneğin Azure portalında zaten bir tane oluşturmuşsanız, azure AD kimlik doğrulamasını varolan bir Sürükleyici Okuyucu kaynağı için de yapılandırabilirsiniz.

Bazı müşteriler için, geliştirme ve üretim e karşı olmak üzere birden çok Sürükleyici Okuyucu kaynağı oluşturmak veya hizmetinizin dağıtılan birden çok farklı bölge için gerekebilir. Bu gibi durumlarda, farklı Sürükleyici Okuyucu kaynakları oluşturmak ve bunları Azure AD izinleriyle yapılandırmak için komut dosyasını birden çok kez kullanabilirsiniz.

Komut dosyası esnek olacak şekilde tasarlanmıştır. Önce aboneliğinizde varolan Immersive Reader ve Azure AD kaynaklarını arar ve bunları yalnızca zaten yoksa gerektiği gibi oluşturur. İlk kez bir Sürükleyici Reader kaynağı oluşturuyorsanız, komut dosyası ihtiyacınız olan her şeyi yapar. Azure AD'yi yalnızca portalda oluşturulan varolan bir Sürükleyici Okuyucu kaynağı için yapılandırmak için kullanmak istiyorsanız, bunu da yapar. Ayrıca, birden çok Sürükleyici Okuyucu kaynağı oluşturmak ve yapılandırmak için de kullanılabilir.

## <a name="set-up-powershell-environment"></a>PowerShell ortamını ayarlama

1. Azure Bulut [Kabuğu'nu](https://docs.microsoft.com/azure/cloud-shell/overview)açarak başlayın. Bulut kabuğunun sol üstteki açılır da powershell olarak ayarlandığından `pwsh`veya yazarak ayarlandığından emin olun.

1. Aşağıdaki kod parçacıklarını kopyalayıp kabuk üzerine yapıştırın.

    ```azurepowershell-interactive
    function Create-ImmersiveReaderResource(
        [Parameter(Mandatory=$true, Position=0)] [String] $SubscriptionName,
        [Parameter(Mandatory=$true)] [String] $ResourceName,
        [Parameter(Mandatory=$true)] [String] $ResourceSubdomain,
        [Parameter(Mandatory=$true)] [String] $ResourceSKU,
        [Parameter(Mandatory=$true)] [String] $ResourceLocation,
        [Parameter(Mandatory=$true)] [String] $ResourceGroupName,
        [Parameter(Mandatory=$true)] [String] $ResourceGroupLocation,
        [Parameter(Mandatory=$true)] [String] $AADAppDisplayName="ImmersiveReaderAAD",
        [Parameter(Mandatory=$true)] [String] $AADAppIdentifierUri,
        [Parameter(Mandatory=$true)] [String] $AADAppClientSecret
    )
    {
        $unused = ''
        if (-not [System.Uri]::TryCreate($AADAppIdentifierUri, [System.UriKind]::Absolute, [ref] $unused)) {
            throw "Error: AADAppIdentifierUri must be a valid URI"
        }

        Write-Host "Setting the active subscription to '$SubscriptionName'"
        $subscriptionExists = Get-AzSubscription -SubscriptionName $SubscriptionName
        if (-not $subscriptionExists) {
            throw "Error: Subscription does not exist"
        }
        az account set --subscription $SubscriptionName

        $resourceGroupExists = az group exists --name $ResourceGroupName
        if ($resourceGroupExists -eq "false") {
            Write-Host "Resource group does not exist. Creating resource group"
            $groupResult = az group create --name $ResourceGroupName --location $ResourceGroupLocation
            if (-not $groupResult) {
                throw "Error: Failed to create resource group"
            }
            Write-Host "Resource group created successfully"
        }

        # Create an Immersive Reader resource if it doesn't already exist
        $resourceId = az cognitiveservices account show --resource-group $ResourceGroupName --name $ResourceName --query "id" -o tsv
        if (-not $resourceId) {
            Write-Host "Creating the new Immersive Reader resource '$ResourceName' (SKU '$ResourceSKU') in '$ResourceLocation' with subdomain '$ResourceSubdomain'"
            $resourceId = az cognitiveservices account create `
                            --name $ResourceName `
                            --resource-group $ResourceGroupName `
                            --kind ImmersiveReader `
                            --sku $ResourceSKU `
                            --location $ResourceLocation `
                            --custom-domain $ResourceSubdomain `
                            --query "id" `
                            -o tsv

            if (-not $resourceId) {
                throw "Error: Failed to create Immersive Reader resource"
            }
            Write-Host "Immersive Reader resource created successfully"
        }

        # Create an Azure Active Directory app if it doesn't already exist
        $clientId = az ad app show --id $AADAppIdentifierUri --query "appId" -o tsv
        if (-not $clientId) {
            Write-Host "Creating new Azure Active Directory app"
            $clientId = az ad app create --password $AADAppClientSecret --display-name $AADAppDisplayName --identifier-uris $AADAppIdentifierUri --query "appId" -o tsv

            if (-not $clientId) {
                throw "Error: Failed to create Azure Active Directory app"
            }
            Write-Host "Azure Active Directory app created successfully"
        }

        # Create a service principal if it doesn't already exist
        $principalId = az ad sp show --id $AADAppIdentifierUri --query "objectId" -o tsv
        if (-not $principalId) {
            Write-Host "Creating new service principal"
            az ad sp create --id $clientId | Out-Null
            $principalId = az ad sp show --id $AADAppIdentifierUri --query "objectId" -o tsv

            if (-not $principalId) {
                throw "Error: Failed to create new service principal"
            }
            Write-Host "New service principal created successfully"
        }

        # Sleep for 5 seconds to allow the new service principal to propagate
        Write-Host "Sleeping for 5 seconds"
        Start-Sleep -Seconds 5

        Write-Host "Granting service principal access to the newly created Immersive Reader resource"
        $accessResult = az role assignment create --assignee $principalId --scope $resourceId --role "Cognitive Services User"
        if (-not $accessResult) {
            throw "Error: Failed to grant service principal access"
        }
        Write-Host "Service principal access granted successfully"

        # Grab the tenant ID, which is needed when obtaining an Azure AD token
        $tenantId = az account show --query "tenantId" -o tsv

        # Collect the information needed to obtain an Azure AD token into one object
        $result = @{}
        $result.TenantId = $tenantId
        $result.ClientId = $clientId
        $result.ClientSecret = $AADAppClientSecret
        $result.Subdomain = $ResourceSubdomain

        Write-Host "Success! " -ForegroundColor Green -NoNewline
        Write-Host "Save the following JSON object to a text file for future reference:"
        Write-Output (ConvertTo-Json $result)
    }
    ```

1. Parametreleri `Create-ImmersiveReaderResource`uygun şekilde sağlayarak işlevi çalıştırın.

    ```azurepowershell-interactive
    Create-ImmersiveReaderResource
      -SubscriptionName '<SUBSCRIPTION_NAME>' `
      -ResourceName '<RESOURCE_NAME>' `
      -ResourceSubdomain '<RESOURCE_SUBDOMAIN>' `
      -ResourceSKU '<RESOURCE_SKU>' `
      -ResourceLocation '<RESOURCE_LOCATION>' `
      -ResourceGroupName '<RESOURCE_GROUP_NAME>' `
      -ResourceGroupLocation '<RESOURCE_GROUP_LOCATION>' `
      -AADAppDisplayName '<AAD_APP_DISPLAY_NAME>' `
      -AADAppIdentifierUri '<AAD_APP_IDENTIFIER_URI>' `
      -AADAppClientSecret '<AAD_APP_CLIENT_SECRET>'
    ```

    | Parametre | Yorumlar |
    | --- | --- |
    | SubscriptionName |Sürükleyici Okuyucu kaynağınız için kullanılacak Azure aboneliğinin adı. Kaynak oluşturmak için aboneliğiniz olması gerekir. |
    | ResourceName |  Alfasayısal olmalıdır ve '-' ilk veya son karakter olmadığı sürece '-'içerebilir. Uzunluk 63 karakteri geçemez.|
    | ResourceSubdomain |Sürükleyici Reader kaynağınız için özel bir alt etki alanı gereklidir. Alt etki alanı, Reader'ı başlatmak için Sürükleyici Okuyucu hizmetini ararken SDK tarafından kullanılır. Alt etki alanı genel olarak benzersiz olmalıdır. Alt etki alanı alfasayısal olmalıdır ve '-' ilk veya son karakter olmadığı sürece '-'içerebilir. Uzunluk 63 karakteri geçemez. Kaynak zaten varsa, bu parametre isteğe bağlıdır. |
    | KaynakSKU |Seçenekler: `S0`. Mevcut her SKU hakkında daha fazla bilgi edinmek için [Bilişsel Hizmetler fiyatlandırma sayfamızı](https://azure.microsoft.com/pricing/details/cognitive-services/immersive-reader/) ziyaret edin. Kaynak zaten varsa, bu parametre isteğe bağlıdır. |
    | ResourceLocation |Seçenekler: `eastus` `eastus2`, `southcentralus` `westus`, `westus2` `australiaeast`, `southeastasia` `centralindia`, `japaneast` `northeurope`, `uksouth` `westeurope`, , , , , , . Kaynak zaten varsa, bu parametre isteğe bağlıdır. |
    | ResourceGroupName |Kaynaklar, abonelikler içindeki kaynak gruplarında oluşturulur. Varolan bir kaynak grubunun adını sağlama. Kaynak grubu zaten yoksa, bu ada sahip yeni bir grup oluşturulur. |
    | Kaynak GrubuKonum |Kaynak grubunuz yoksa, grubu oluşturmak için bir konum sağlamanız gerekir. Konumların listesini bulmak için `az account list-locations`çalıştırın. Döndürülen sonucun *ad* özelliğini (boşluk suz) kullanın. Kaynak grubunuz zaten varsa, bu parametre isteğe bağlıdır. |
    | AADAppDisplayName |Azure Active Directory uygulama görüntü adı. Varolan bir Azure AD uygulaması bulunmazsa, bu ada sahip yeni bir uygulama oluşturulur. Azure AD uygulaması zaten varsa, bu parametre isteğe bağlıdır. |
    | AADAppIdentifierUri |Azure AD uygulaması için URI. Varolan bir Azure AD uygulaması bulunmazsa, bu URI ile yeni bir uygulama oluşturulur. Örneğin, `https://immersivereaderaad-mycompany`. |
    | AADAppClientSecret |Immersive Reader başlatmak için bir belirteç alırken daha sonra kimlik doğrulaması için kullanılacak oluşturduğunuz bir parola. Parola en az 16 karakter uzunluğunda, en az 1 özel karakter içermeli ve en az 1 sayısal karakter içermelidir. |

1. JSON çıktısını daha sonra kullanmak üzere bir metin dosyasına kopyalayın. Çıktı aşağıdaki gibi görünmelidir.

    ```json
    {
      "TenantId": "...",
      "ClientId": "...",
      "ClientSecret": "...",
      "Subdomain": "..."
    }
    ```

## <a name="next-steps"></a>Sonraki adımlar

* Node.js kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Düğüm.js quickstart'ı](./quickstart-nodejs.md) görüntüleyin
* Python kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Python öğreticisini](./tutorial-python.md) görüntüleyin
* Swift'i kullanarak Sürükleyici Reader SDK ile başka neler yapabileceğinizi görmek için [Swift öğreticisini](./tutorial-ios-picture-immersive-reader.md) görüntüleyin
* [Immersive Reader SDK](https://github.com/microsoft/immersive-reader-sdk) ve [Immersive Reader SDK Referans](./reference.md) keşfedin




