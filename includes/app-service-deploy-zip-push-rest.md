---
author: cephalin
ms.service: app-service
ms.topic: include
ms.date: 08/12/2019
ms.author: cephalin
ms.openlocfilehash: 92e39f128e90ba83a919388e217f0edc86f81770
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75769684"
---
## <a name="deploy-zip-file-with-rest-apis"></a><a name="rest"></a>REST API'leri ile ZIP dosyalarını dağıtma 

.zip dosyasını Azure'da uygulamanıza dağıtmak için [dağıtım hizmeti REST API'lerini](https://github.com/projectkudu/kudu/wiki/REST-API) kullanabilirsiniz. Dağıtmak için https://<app_name>.scm.azurewebsites.net/api/zipdeploy adresine POSTA isteği gönderin. POST isteği ileti gövdesinde .zip dosyasını içermelidir. Uygulamanızın dağıtım kimlik bilgileri, HTTP BASIC kimlik doğrulaması kullanılarak istekte belirtilir. Daha fazla bilgi için [.zip push dağıtım başvurusuna](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)bakın. 

HTTP BASIC kimlik doğrulaması için Uygulama Hizmeti dağıtım kimlik bilgilerinize ihtiyacınız vardır. Dağıtım kimlik bilgilerinizi nasıl ayarlayınızı görmek için [kullanıcı düzeyindeki kimlik bilgilerini ayarla ve sıfırlama](../articles/app-service/deploy-configure-credentials.md#userscope)kınızda.

### <a name="with-curl"></a>cURL ile

Aşağıdaki örnekte bir .zip dosyası dağıtmak için cURL aracı nı kullanır. Yer tutucuları `<deployment_user>` `<zip_file_path>`değiştirin `<app_name>`ve . cURL tarafından istendiğinde, parolayı yazın.

```bash
curl -X POST -u <deployment_user> --data-binary @"<zip_file_path>" https://<app_name>.scm.azurewebsites.net/api/zipdeploy
```

Bu istek yüklenen .zip dosyasından itme dağıtımını tetikler. Aşağıdaki cURL örneğinde gösterildiği `https://<app_name>.scm.azurewebsites.net/api/deployments` gibi, bitiş noktasını kullanarak geçerli ve geçmiş dağıtımları gözden geçirebilirsiniz. Yine, `<app_name>` uygulamanızın adı ve `<deployment_user>` dağıtım kimlik bilgilerinizin kullanıcı adı ile değiştirin.

```bash
curl -u <deployment_user> https://<app_name>.scm.azurewebsites.net/api/deployments
```

### <a name="with-powershell"></a>PowerShell ile

Aşağıdaki örnekte [Publish-AzWebapp](/powershell/module/az.websites/publish-azwebapp) .zip dosyasını yükleyin. Yer tutucuları `<group-name>` `<app-name>`değiştirin `<zip-file-path>`ve .

```powershell
Publish-AzWebapp -ResourceGroupName <group-name> -Name <app-name> -ArchivePath <zip-file-path>
```

Bu istek yüklenen .zip dosyasından itme dağıtımını tetikler. 

Geçerli ve geçmiş dağıtımları gözden geçirmek için aşağıdaki komutları çalıştırın. Yine, `<deployment-user>`değiştirin `<deployment-password>`, `<app-name>` , ve yer tutucular.

```bash
$username = "<deployment-user>"
$password = "<deployment-password>"
$apiUrl = "https://<app-name>.scm.azurewebsites.net/api/deployments"
$base64AuthInfo = [Convert]::ToBase64String([Text.Encoding]::ASCII.GetBytes(("{0}:{1}" -f $username, $password)))
$userAgent = "powershell/1.0"
Invoke-RestMethod -Uri $apiUrl -Headers @{Authorization=("Basic {0}" -f $base64AuthInfo)} -UserAgent $userAgent -Method GET
```
