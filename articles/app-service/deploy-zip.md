---
title: ZIP veya WAR dosyasıyla kod dağıtma
description: Uygulamanızı ZIP dosyasıyla (veya Java geliştiricileri için bir SAVAŞ dosyası) Azure Uygulama Hizmetine nasıl dağıtılacayacak larınızı öğrenin.
ms.topic: article
ms.date: 08/12/2019
ms.reviewer: sisirap
ms.custom: seodec18
ms.openlocfilehash: 716f6813e37aec086a7d496e001fe2ca0f4aab57
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75945168"
---
# <a name="deploy-your-app-to-azure-app-service-with-a-zip-or-war-file"></a>Zip veya WAR dosyasıyla uygulamanızı Azure Uygulama Hizmetine dağıtın

Bu makalede, web uygulamanızı [Azure Uygulama Hizmeti'ne](overview.md)dağıtmak için zip dosyası veya WAR dosyası nasıl kullanılacağı gösterilmektedir. 

Bu ZIP dosyası dağıtımı, sürekli tümleştirme tabanlı dağıtımlara güç veren aynı Kudu hizmetini kullanır. Kudu, ZIP dosya dağıtımı için aşağıdaki işlevselliği destekler: 

- Önceki dağıtımdan kalan dosyaların silinmesi.
- Paket geri yüklemesini içeren varsayılan yapı işlemini açma seçeneği.
- Dağıtım komut dosyalarını çalıştırma da dahil olmak üzere dağıtım özelleştirme.  
- Dağıtım günlükleri. 
- 2048 MB dosya boyutu sınırı.

Daha fazla bilgi için [Bkz. Kudu belgeleri.](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)

WAR dosya dağıtımı, Java web uygulamanızı çalıştırmak için [WAR](https://wikipedia.org/wiki/WAR_(file_format)) dosyanızı App Service'e dağır. Bkz. [Savaş dosyayı dağıt.](#deploy-war-file)

## <a name="prerequisites"></a>Ön koşullar

Bu makaledeki adımları tamamlamak için [bir Uygulama Hizmeti uygulaması oluşturun](/azure/app-service/)veya başka bir öğretici için oluşturduğunuz bir uygulamayı kullanın.

[!INCLUDE [quickstarts-free-trial-note](../../includes/quickstarts-free-trial-note.md)]

[!INCLUDE [Create a project ZIP file](../../includes/app-service-web-deploy-zip-prepare.md)]

[!INCLUDE [Deploy ZIP file](../../includes/app-service-web-deploy-zip.md)]
Yukarıdaki bitiş noktası şu anda Linux Uygulama Hizmetleri için çalışmıyor. Bunun yerine FTP veya [ZIP dağıtma API'sini](https://docs.microsoft.com/azure/app-service/containers/app-service-linux-faq#continuous-integration-and-deployment) kullanmayı düşünün.

## <a name="deploy-zip-file-with-azure-cli"></a>Azure CLI ile ZIP dosyalarını dağıtma

Yüklenen ZIP dosyasını [az webapp dağıtım kaynağı config-zip](/cli/azure/webapp/deployment/source?view=azure-cli-latest#az-webapp-deployment-source-config-zip) komutunu kullanarak web uygulamanıza dağıtın.  

Aşağıdaki örnek, yüklediğiniz ZIP dosyasını dağır. Azure CLI'nin yerel yüklemesini kullanırken, yerel ZIP `--src`dosyanıza giden yolu belirtin.

```azurecli-interactive
az webapp deployment source config-zip --resource-group <group-name> --name <app-name> --src clouddrive/<filename>.zip
```

Bu komut ZIP içindeki dosyaları ve dizinleri App Service uygulama klasörünüze (`\home\site\wwwroot`) dağıtır ve uygulamayı yeniden başlatır.

Varsayılan olarak, dağıtım motoru zip dosyasının olduğu gibi çalışmaya hazır olduğunu ve herhangi bir yapı otomasyonu çalıştırmadığını varsayar. [Git dağıtımındakiyle](deploy-local-git.md)aynı yapı otomasyonunu etkinleştirmek için Bulut `SCM_DO_BUILD_DURING_DEPLOYMENT` [Kabuğu'nda](https://shell.azure.com)aşağıdaki komutu çalıştırarak uygulama ayarını ayarlayın:

```azurecli-interactive
az webapp config appsettings set --resource-group <group-name> --name <app-name> --settings SCM_DO_BUILD_DURING_DEPLOYMENT=true
```

Daha fazla bilgi için [Bkz. Kudu belgeleri.](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file-or-url)

[!INCLUDE [app-service-deploy-zip-push-rest](../../includes/app-service-deploy-zip-push-rest.md)]  

## <a name="deploy-war-file"></a>WAR dosyayı dağıt

Bir WAR dosyasını Uygulama Hizmetine dağıtmak `https://<app-name>.scm.azurewebsites.net/api/wardeploy`için, bir POST isteği gönderin. POST isteğinin ileti gövdesinde .war dosyası bulunmalıdır. Uygulamanızın dağıtım kimlik bilgileri, HTTP BASIC kimlik doğrulaması kullanılarak istekte belirtilir.

WAR `/api/wardeploy` dosyalarını dağıtırken her zaman kullanın. Bu API, WAR dosyanızı genişletecek ve paylaşılan dosya sürücüsüne yerleştirilecek. diğer dağıtım API'leri kullanmak tutarsız davranışlara neden olabilir. 

HTTP BASIC kimlik doğrulaması için Uygulama Hizmeti dağıtım kimlik bilgilerinize ihtiyacınız vardır. Dağıtım kimlik bilgilerinizi nasıl ayarlayınızı görmek için [kullanıcı düzeyindeki kimlik bilgilerini ayarla ve sıfırlama](deploy-configure-credentials.md#userscope)kınızda.

### <a name="with-curl"></a>cURL ile

Aşağıdaki örnekte bir .war dosyası dağıtmak için cURL aracı nı kullanır. Yer tutucuları `<username>` `<war-file-path>`değiştirin `<app-name>`ve . cURL tarafından istendiğinde, parolayı yazın.

```bash
curl -X POST -u <username> --data-binary @"<war-file-path>" https://<app-name>.scm.azurewebsites.net/api/wardeploy
```

### <a name="with-powershell"></a>PowerShell ile

Aşağıdaki örnekte [Publish-AzWebapp](/powershell/module/az.websites/publish-azwebapp) .war dosyasını yükleyin. Yer tutucuları `<group-name>` `<app-name>`değiştirin `<war-file-path>`ve .

```powershell
Publish-AzWebapp -ResourceGroupName <group-name> -Name <app-name> -ArchivePath <war-file-path>
```

[!INCLUDE [What happens to my app during deployment?](../../includes/app-service-deploy-atomicity.md)]

## <a name="next-steps"></a>Sonraki adımlar

Daha gelişmiş dağıtım senaryoları için [Git ile Azure'a dağıtmayı](deploy-local-git.md)deneyin. Azure'a Git tabanlı dağıtım sürüm denetimi, paket geri yükleme, MSBuild ve daha fazlasını sağlar.

## <a name="more-resources"></a>Diğer kaynaklar

* [Kudu: Zip dosyasından dağıtım](https://github.com/projectkudu/kudu/wiki/Deploying-from-a-zip-file)
* [Azure Uygulama Hizmeti Dağıtım Kimlik Bilgileri](deploy-ftp.md)
