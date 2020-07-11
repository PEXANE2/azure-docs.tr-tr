---
title: Öğretici-Azure işlevine göre kapsayıcı grubunu tetikleme
description: Azure Container Instances oluşturmayı otomatikleştirmek için HTTP ile tetiklenen, sunucusuz bir PowerShell işlevi oluşturma
ms.topic: tutorial
ms.date: 06/10/2020
ms.custom: ''
ms.openlocfilehash: 298cf1452e514ede540e23d4e64f6dd1059cceab
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259749"
---
# <a name="tutorial-use-an-http-triggered-azure-function-to-create-a-container-group"></a>Öğretici: bir kapsayıcı grubu oluşturmak için HTTP ile tetiklenen bir Azure işlevi kullanın

[Azure işlevleri](../azure-functions/functions-overview.md) , bir http isteği, süreölçer veya bir Azure depolama kuyruğundaki ileti gibi çeşitli olaylara yanıt olarak komut dosyalarını veya kodu çalıştıran sunucusuz bir bilgi işlem hizmetidir.

Bu öğreticide, bir HTTP isteği alan ve bir [kapsayıcı grubunun](container-instances-container-groups.md)dağıtımını tetikleyen bir Azure işlevi oluşturacaksınız. Bu örnekte, Azure Container Instances ' de otomatik olarak kaynak oluşturmak için Azure Işlevleri kullanmanın temelleri gösterilmektedir. Daha karmaşık senaryolar veya diğer olay tetikleyicileri için örneği değiştirin veya genişletin. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * Temel HTTP ile tetiklenen bir PowerShell işlevi oluşturmak için [Azure işlevleri uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) ile Visual Studio Code kullanın.
> * İşlev uygulamasında bir kimliği etkinleştirin ve Azure kaynakları oluşturmak için izin verin.
> * Tek kapsayıcılı bir kapsayıcı grubunun dağıtımını otomatik hale getirmek için PowerShell işlevini değiştirin ve yeniden yayımlayın.
> * Kapsayıcının HTTP tarafından tetiklenen dağıtımını doğrulayın.

## <a name="prerequisites"></a>Önkoşullar

İşletim sisteminde Azure Işlevleri Uzantısı ile Visual Studio Code yüklemek ve kullanmak üzere önkoşulları [Visual Studio Code kullanarak Azure 'da ilk Işlevinizi oluşturma](../azure-functions/functions-create-first-function-vs-code.md?pivots=programming-language-powershell#configure-your-environment) konusuna bakın.

Bu makaledeki ek adımlar Azure PowerShell kullanır. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure PowerShell yüklemesi][azure-powershell-install] ve [Azure 'da oturum açma](/powershell/azure/get-started-azureps#sign-in-to-azure).

## <a name="create-a-basic-powershell-function"></a>Temel PowerShell işlevi oluşturma

HTTP tetikleyici şablonunu kullanarak bir PowerShell işlevi oluşturmak için [Azure 'da Ilk PowerShell işlevinizi oluşturma](../azure-functions/functions-create-first-function-vs-code.md?pivots=programming-language-powershell) bölümündeki adımları izleyin. Varsayılan Azure işlevi adı **Httptrigger**' i kullanın. Hızlı başlangıçta gösterildiği gibi, işlevi yerel olarak test edin ve projeyi Azure 'da bir işlev uygulamasında yayımlayın. Bu örnek, bir metin dizesi döndüren temel bir HTTP ile tetiklenen işlevdir. Bu makaledeki sonraki adımlarda, işlevi bir kapsayıcı grubu oluşturacak şekilde değiştirirsiniz.

Bu makalede, projeyi, işlev uygulaması adına (Ayrıca *myfunctionapp*) göre otomatik olarak adlandırılan bir Azure Kaynak grubundaki *myfunctionapp*adını kullanarak yayımladığınızı varsaymaktadır. Daha sonraki adımlarda benzersiz işlev uygulamanızın adını ve kaynak grubu adını değiştirin.

## <a name="enable-an-azure-managed-identity-in-the-function-app"></a>İşlev uygulamasında Azure tarafından yönetilen bir kimliği etkinleştirme

Aşağıdaki komutlar, işlev uygulamanızda sistem tarafından atanan bir [yönetilen kimliği](../app-service/overview-managed-identity.md?toc=/azure/azure-functions/toc.json#add-a-system-assigned-identity) etkinleştirir. Uygulamayı çalıştıran PowerShell ana bilgisayarı, bu kimlik kullanılarak Azure 'da otomatik olarak kimlik doğrulaması yapabilir ve bu sayede işlevlerin, kimliğin erişim izni verilen Azure hizmetlerinde işlem yapmasını sağlar. Bu öğreticide, işlev uygulamasının kaynak grubunda kaynak oluşturmak için yönetilen kimlik izinlerini verirsiniz. 

İşlev uygulamasına [bir kimlik ekleyin](../app-service/overview-managed-identity.md?tabs=dotnet#using-azure-powershell-1) :

```powershell
Update-AzFunctionApp -Name myfunctionapp `
    -ResourceGroupName myfunctionapp `
    -IdentityType SystemAssigned
```

Kaynak grubu kapsamındaki katkıda bulunan rolün kimliğini atayın:

```powershell
$SP=(Get-AzADServicePrincipal -DisplayName myfunctionapp).Id
$RG=(Get-AzResourceGroup -Name myfunctionapp).ResourceId
New-AzRoleAssignment -ObjectId $SP -RoleDefinitionName "Contributor" -Scope $RG
```

## <a name="modify-httptrigger-function"></a>HttpTrigger işlevini Değiştir

Bir kapsayıcı grubu oluşturmak için **Httptrigger** işlevinin PowerShell kodunu değiştirin. İşlevin dosyasında `run.ps1` Aşağıdaki kod bloğunu bulun. Bu kod, işlev URL 'sinde sorgu dizesi olarak geçirilmemişse bir ad değeri görüntüler:

```powershell
[...]
if ($name) {
    $body = "Hello, $name. This HTTP triggered function executed successfully."
}
[...]
```

Bu kodu aşağıdaki örnek blokla değiştirin. Burada, sorgu dizesinde bir ad değeri geçirilirse, [New-AzContainerGroup][new-azcontainergroup] cmdlet 'ini kullanarak bir kapsayıcı grubunu adlandırmak ve oluşturmak için kullanılır. *Myfunctionapp* kaynak grubu adını, işlev uygulamanızın kaynak grubu adıyla değiştirdiğinizden emin olun:

```powershell
[...]
if ($name) {
    New-AzContainerGroup -ResourceGroupName myfunctionapp -Name $name `
        -Image alpine -OsType Linux `
        -Command "echo 'Hello from an Azure container instance triggered by an Azure function'" `
        -RestartPolicy Never
    if ($?) {
        $body = "This HTTP triggered function executed successfully. Started container group $name"
    }
    else  {
        $body = "There was a problem starting the container group."
    }
[...]
```

Bu örnek, görüntüyü çalıştıran tek bir kapsayıcı örneğinden oluşan bir kapsayıcı grubu oluşturur `alpine` . Kapsayıcı tek bir komut çalıştırır `echo` ve sonra sonlanır. Gerçek dünyada bir örnekte, toplu iş çalıştırmak için bir veya daha fazla kapsayıcı grubu oluşturulmasını tetikleyebilirsiniz.
 
## <a name="test-function-app-locally"></a>İşlev uygulamasını yerel olarak test etme

İşlev uygulaması projesini Azure 'a yeniden yayımlayabilmeniz için işlevin yerel olarak çalıştığından emin olun. Yerel olarak çalıştırıldığında, işlev Azure kaynakları oluşturmaz. Ancak, bir sorgu dizesinde ad değeri geçirmeden işlev akışını ve ile test edebilirsiniz. İşlevde hata ayıklamak için bkz. [PowerShell Azure işlevleri 'nde yerel olarak hata ayıklama](../azure-functions/functions-debug-powershell-local.md).

## <a name="republish-azure-function-app"></a>Azure işlev uygulamasını yeniden yayımlama

İşlevin yerel olarak çalıştığını doğruladıktan sonra, projeyi Azure 'da var olan işlev uygulamasına yeniden yayımlayın.

1. Visual Studio Code ' de, komut paleti ' ni açın. Araması yapın ve seçin `Azure Functions: Deploy to Function App...` .
1. Zip ve dağıtım için geçerli çalışma klasörünü seçin.
1. Aboneliği ve ardından mevcut işlev uygulamasının adını (*myfunctionapp*) seçin. Önceki dağıtımın üzerine yazmak istediğinizi onaylayın.

İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Güncelleştirdiğiniz Azure kaynakları dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **çıktıyı görüntüle** ' yi seçin.

## <a name="run-the-function-in-azure"></a>İşlevi Azure 'da çalıştırma

Dağıtım başarıyla tamamlandıktan sonra işlev URL 'sini alın. Örneğin, **Httptrigger** işlev URL 'sini kopyalamak Için Visual Studio Code **Azure: Functions** alanını kullanın veya [Azure Portal](../azure-functions/functions-create-first-azure-function.md#test-the-function)işlev URL 'sini alın.

İşlev URL 'SI şu biçimdedir:

```
https://myfunctionapp.azurewebsites.net/api/HttpTrigger
```

### <a name="run-function-without-passing-a-name"></a>İşlevi bir ad geçirmeden Çalıştır

İlk test olarak, `curl` komutunu çalıştırın ve bir sorgu dizesi eklemeden Işlev URL 'sini geçirin `name` . 

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger"
```

İşlev 200 durum kodunu ve metnini döndürür `This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response` :

```
[...]
> GET /api/HttpTrigger? HTTP/1.1
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.64.1
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/1.1 200 OK
< Content-Length: 135
< Content-Type: text/plain; charset=utf-8
< Request-Context: appId=cid-v1:d0bd0123-f713-4579-8990-bb368a229c38
< Date: Wed, 10 Jun 2020 17:50:27 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
This HTTP triggered function executed successfully. Pass a name in the query string or in the request body for a personalized response.* Closing connection 0
```

### <a name="run-function-and-pass-the-name-of-a-container-group"></a>İşlevi Çalıştır ve bir kapsayıcı grubunun adını geçir

Şimdi komutu çalıştırın `curl` ve bir kapsayıcı grubunun (*mycontainergroup*) adını sorgu dizesi olarak ekleyin `?name=mycontainergroup` :

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger?name=mycontainergroup"
```

İşlev 200 durum kodunu döndürür ve kapsayıcı grubunun oluşturulmasını tetikler:

```
[...]
> GET /api/HttpTrigger1?name=mycontainergroup HTTP/1.1
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.64.1
> Accept: */*
> 
< HTTP/1.1 200 OK
< Content-Length: 92
< Content-Type: text/plain; charset=utf-8
< Request-Context: appId=cid-v1:d0bd0123-f713-4579-8990-bb368a229c38
< Date: Wed, 10 Jun 2020 17:54:31 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
This HTTP triggered function executed successfully. Started container group mycontainergroup* Closing connection 0
```

Kapsayıcının [Get-AzContainerInstanceLog][get-azcontainerinstancelog] komutuyla çalıştığını doğrulayın:

```azurecli
Get-AzContainerInstanceLog -ResourceGroupName myfunctionapp `
  -ContainerGroupName mycontainergroup 
```

Örnek çıktı:

```console
Hello from an Azure container instance triggered by an Azure function
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu öğreticide oluşturduğunuz kaynakların hiçbirine artık ihtiyacınız yoksa, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az Group Delete] [az-Group-Delete] komutunu yürütebilirsiniz. Bu komut, oluşturduğunuz işlev uygulamasının yanı sıra çalışan kapsayıcıyı ve tüm ilgili kaynakları da siler.

```azurecli-interactive
az group delete --name myfunctionapp
```

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir HTTP isteği alan ve bir kapsayıcı grubunun dağıtımını tetikleyen bir Azure işlevi oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * Temel HTTP ile tetiklenen bir PowerShell işlevi oluşturmak için Azure Işlevleri Uzantısı ile Visual Studio Code kullanın.
> * İşlev uygulamasında bir kimliği etkinleştirin ve Azure kaynakları oluşturmak için izin verin.
> * Tek kapsayıcılı bir kapsayıcı grubunun dağıtımını otomatik hale getirmek için PowerShell işlev kodunu değiştirin.
> * Kapsayıcının HTTP tarafından tetiklenen dağıtımını doğrulayın.

Kapsayıcılı bir işi başlatma ve izlemeye yönelik ayrıntılı bir örnek için bkz. PowerShell Azure Işlevleri ve Azure Container Instances ve birlikte gelen [kod örneği](https://github.com/anthonychu/functions-powershell-run-aci) [Ile olay temelli sunucusuz kapsayıcılar](https://dev.to/azure/event-driven-serverless-containers-with-powershell-azure-functions-and-azure-container-instances-e9b) .

Azure işlevleri oluşturma ve bir işlevler projesi yayımlama hakkında ayrıntılı yönergeler için bkz. [Azure işlevleri belgeleri](../azure-functions/index.yml) . 

<!-- IMAGES -->


<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->
[azure-powershell-install]: /powershell/azure/install-az-ps
[new-azcontainergroup]: /powershell/module/az.containerinstance/new-azcontainergroup
[get-azcontainerinstancelog]: /powershell/module/az.containerinstance/get-azcontainerinstancelog
