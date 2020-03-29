---
title: Azure Medya Hizmetleri v3 API'ye bağlanın - Python
description: Bu makalede, Python ile Media Services v3 API'ye nasıl bağlanılalışiz gösteriş.
services: media-services
documentationcenter: ''
author: Juliako
manager: femila
editor: ''
ms.service: media-services
ms.workload: media
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: article
ms.date: 04/15/2019
ms.author: juliako
ms.openlocfilehash: 98a8cdf4120cf56184eb5735249640e3423acdf4
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74888470"
---
# <a name="connect-to-media-services-v3-api---python"></a>Medya Hizmetlerine bağlan v3 API - Python

Bu makalede, hizmet ana oturum açma yöntemini kullanarak Azure Media Services v3 Python SDK'ya nasıl bağlanabileceğinizi gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

- [Python'u python.org](https://www.python.org/downloads/) indir
- Ortam değişkenini `PATH` ayarladıklıolun
- [Bir Medya Hizmetleri hesabı oluşturun.](create-account-cli-how-to.md) Kaynak grup adını ve Medya Hizmetleri hesap adını hatırladığından emin olun.
- [Access API'leri](access-api-cli-how-to.md) konusundaki adımları izleyin. Abonelik kimliğini, uygulama kimliğini (istemci kimliğini), kimlik doğrulama anahtarını (gizli) ve daha sonraki adımda gereksinim duyduğunuz kiracı kimliğini kaydedin.

> [!IMPORTANT]
> Adlandırma kurallarını gözden [geçirin.](media-services-apis-overview.md#naming-conventions)

## <a name="install-the-modules"></a>Modülleri yükleme

Python'u kullanarak Azure Medya Hizmetleri ile çalışmak için bu modülleri yüklemeniz gerekir.

* Active `azure-mgmt-resource` Directory için Azure modüllerini içeren modül.
* Medya `azure-mgmt-media` Hizmetleri varlıklarını içeren modül.

Bir komut satırı aracı nı açın ve modülleri yüklemek için aşağıdaki komutları kullanın.

```
pip3 install azure-mgmt-resource
pip3 install azure-mgmt-media==1.1.1
```

## <a name="connect-to-the-python-client"></a>Python istemcisine bağlan

1. Uzantılı dosya `.py` oluşturma
1. En sevdiğiniz düzenleyicide dosyayı açma
1. Dosyaya izleyen kodu ekleyin. Kod gerekli modülleri içeri aktar ve Medya Hizmetleri'ne bağlanmak için gereken Active Directory kimlik bilgilerini oluşturur.

      Değişkenlerin değerlerini [Access API'lerinden](access-api-cli-how-to.md) aldığınız değerlere ayarlama

      ```
      import adal
      from msrestazure.azure_active_directory import AdalAuthentication
      from msrestazure.azure_cloud import AZURE_PUBLIC_CLOUD
      from azure.mgmt.media import AzureMediaServices
      from azure.mgmt.media.models import MediaService

      RESOURCE = 'https://management.core.windows.net/'
      # Tenant ID for your Azure Subscription
      TENANT_ID = '00000000-0000-0000-000000000000'
      # Your Service Principal App ID
      CLIENT = '00000000-0000-0000-000000000000'
      # Your Service Principal Password
      KEY = '00000000-0000-0000-000000000000'
      # Your Azure Subscription ID
      SUBSCRIPTION_ID = '00000000-0000-0000-000000000000'
      # Your Azure Media Service (AMS) Account Name
      ACCOUNT_NAME = 'amsv3account'
      # Your Resource Group Name
      RESOUCE_GROUP_NAME = 'AMSv3ResourceGroup'

      LOGIN_ENDPOINT = AZURE_PUBLIC_CLOUD.endpoints.active_directory
      RESOURCE = AZURE_PUBLIC_CLOUD.endpoints.active_directory_resource_id

      context = adal.AuthenticationContext(LOGIN_ENDPOINT + '/' + TENANT_ID)
      credentials = AdalAuthentication(
          context.acquire_token_with_client_credentials,
          RESOURCE,
          CLIENT,
          KEY
      )

      # The AMS Client
      # You can now use this object to perform different operations to your AMS account.
      client = AzureMediaServices(credentials, SUBSCRIPTION_ID)

      print("signed in")

      # Now that you are authenticated, you can manipulate the entities.
      # For example, list assets in your AMS account
      print (client.assets.list(RESOUCE_GROUP_NAME, ACCOUNT_NAME).get(0))
      ```

1. Dosyayı çalıştırma

## <a name="next-steps"></a>Sonraki adımlar

- [Python SDK](https://aka.ms/ams-v3-python-sdk)kullanın.
- Medya Hizmetleri [Python ref](https://aka.ms/ams-v3-python-ref) belgelerini gözden geçirin.
