---
title: Öğretici - Azure işlevine göre kapsayıcı grubunu tetikleme
description: Azure kapsayıcı örneklerinin oluşturulmasını otomatikleştirmek için HTTP tarafından tetiklenen, sunucusuz bir PowerShell işlevi oluşturun
ms.topic: tutorial
ms.date: 09/20/2019
ms.custom: ''
ms.openlocfilehash: 9dbb22a2449e4c41bff802ab827da4489fc7ffeb
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "78331034"
---
# <a name="tutorial-use-an-http-triggered-azure-function-to-create-a-container-group"></a>Öğretici: Kapsayıcı grubu oluşturmak için HTTP tarafından tetiklenen bir Azure işlevi kullanın

[Azure İşlevler,](../azure-functions/functions-overview.md) Azure Depolama kuyruğundaki HTTP isteği, zamanlayıcı veya ileti gibi çeşitli olaylara yanıt olarak komut dosyalarını veya kodu çalıştırabilen sunucusuz bir bilgi işlem hizmetidir.

Bu öğreticide, bir HTTP isteği alan ve bir kapsayıcı [grubunun](container-instances-container-groups.md)dağıtımını tetikleyen bir Azure işlevi oluşturursunuz. Bu örnek, Azure Kapsayıcı Örnekleri'nde otomatik olarak kaynak oluşturmak için Azure İşlevlerini kullanmanın temellerini gösterir. Daha karmaşık senaryolar veya diğer olay tetikleyicileri için örneği değiştirin veya genişletin. 

Aşağıdakileri nasıl yapacağınızı öğrenirsiniz:

> [!div class="checklist"]
> * TEMEL HTTP tetiklenen PowerShell işlevi oluşturmak için [Azure İşlevler uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-azuretools.vscode-azurefunctions) ile Visual Studio Kodunu kullanın.
> * İşlev uygulamasında bir kimlik etkinleştirin ve azure kaynakları oluşturma izni verin.
> * Tek kapsayıcı kapsayıcı grubunun dağıtımını otomatikleştirmek için PowerShell işlevini değiştirin ve yeniden yayımlayın.
> * Kapsayıcının HTTP tarafından tetiklenen dağıtımını doğrulayın.

> [!IMPORTANT]
> Azure İşlevler için PowerShell şu anda önizlemede. Önizlemeler, [ek kullanım koşullarını][terms-of-use] kabul etmeniz şartıyla kullanımınıza sunulur. Bu özelliğin bazı yönleri genel kullanıma açılmadan önce değişebilir.

## <a name="prerequisites"></a>Ön koşullar

İşletim sisteminizdeki Azure İşlevleri ile Visual Studio Code'u yüklemek ve kullanmak için ön koşullar için [Azure'daki ilk işlevinizi oluşturun'](/azure/azure-functions/functions-create-first-function-vs-code?pivots=programming-language-powershell#configure-your-environment) e bakın.

Bu makaledeki bazı adımlar Azure CLI'yi kullanır. Bu adımları tamamlamak için Azure Bulut Kabuğu'nu veya Azure CLI'nin yerel yüklemesini kullanabilirsiniz. Yüklemeniz veya yükseltmeniz gerekirse, bkz. [Azure CLI yükleme][azure-cli-install].

## <a name="create-a-basic-powershell-function"></a>Temel bir PowerShell işlevi oluşturun

HTTP Trigger şablonunu kullanarak bir PowerShell işlevi oluşturmak için [Azure'daki ilk PowerShell işlevinizi oluştur'daki](../azure-functions/functions-create-first-function-powershell.md) adımları izleyin. Varsayılan Azure işlev adını kullanın **HttpTrigger**. Hızlı başlatmada gösterildiği gibi, işlevi yerel olarak sınlayın ve projeyi Azure'daki bir işlev uygulamasında yayımlayın. Bu örnek, metin dizesi döndüren temel bir HTTP tetiklenen işlevdir. Bu makalenin sonraki adımlarında, bir kapsayıcı grubu oluşturmak için işlevi değiştirirsiniz.

Bu makalede, işlevi uygulama adına (ayrıca *myfunctionapp)* göre otomatik olarak adlandırılmış bir Azure kaynak grubunda myfunctionapp adını kullanarak projeyi yayımladığınızvarsakabul *edilmiştir.* Daha sonraki adımlarda benzersiz işlev uygulama adınızı ve kaynak grup adınızı değiştirin.

## <a name="enable-an-azure-managed-identity-in-the-function-app"></a>İşlev uygulamasında Azure tarafından yönetilen bir kimliği etkinleştirme

Artık işlev uygulamanızda sistem tarafından atanmış yönetilen bir [kimliği](../app-service/overview-managed-identity.md?toc=/azure/azure-functions/toc.json#add-a-system-assigned-identity) etkinleştirin. Uygulamayı çalıştıran PowerShell ana bilgisayar, bu kimliği kullanarak otomatik olarak kimlik doğrulaması yapabilir ve işlevlerin kimliğin erişildiği Azure hizmetlerinde işlem yapmasını sağlar. Bu eğitimde, işlev uygulamasının kaynak grubunda kaynak oluşturmak için yönetilen kimlik izinleri verirsiniz. 

Önce işlev uygulamasının kaynak grubunun kimliğini almak ve bir ortam değişkeninde depolamak için [az grup göster][az-group-show] komutunu kullanın. Bu örnek, komutu Bir Bash kabuğunda çalıştırdığınızı varsayar.

```azurecli
rgID=$(az group show --name myfunctionapp --query id --output tsv)
```

İşlev uygulamasına yerel bir kimlik atamak ve kaynak grubuna katkıda bulunan bir rol atamak için [az functionapp kimlik uygulamasını][az-functionapp-identity-app-assign] çalıştırın. Bu rol, kimliğin kaynak grubundaki kapsayıcı grupları gibi ek kaynaklar oluşturmasına olanak tanır.

```azurecli
az functionapp identity assign \
  --name myfunctionapp \
  --resource-group myfunctionapp \
  --role contributor --scope $rgID
```

## <a name="modify-httptrigger-function"></a>HttpTrigger işlevini değiştirin

Bir kapsayıcı grubu oluşturmak için **HttpTrigger** işlevi için PowerShell kodunu değiştirin. İşlev `run.ps1` için dosyada, aşağıdaki kod bloğunu bulun. Bu kod, işlev URL'sinde sorgu dizesi olarak geçirilirse, bir ad değeri görüntüler:

```powershell
[...]
if ($name) {
    $status = [HttpStatusCode]::OK
    $body = "Hello $name"
}
[...]
```

Bu kodu aşağıdaki örnek blokla değiştirin. Burada, sorgu dizesinde bir ad değeri geçirilirse, [Yeni-AzContainerGroup][new-azcontainergroup] cmdlet'ini kullanarak bir kapsayıcı grubu adlandırmak ve oluşturmak için kullanılır. Kaynak grubu adı *myfunctionapp'ı* işlev uygulamanız için kaynak grubunun adı ile değiştirdiğinizden emin olun:

```powershell
[...]
if ($name) {
    $status = [HttpStatusCode]::OK
    New-AzContainerGroup -ResourceGroupName myfunctionapp -Name $name `
        -Image alpine -OsType Linux `
        -Command "echo 'Hello from an Azure container instance triggered by an Azure function'" `
        -RestartPolicy Never
    $body = "Started container group $name"
}
[...]
```

Bu örnek, `alpine` görüntüyü çalıştıran tek bir kapsayıcı örneğinden oluşan bir kapsayıcı grubu oluşturur. Kapsayıcı tek `echo` bir komutu çalıştırır ve sonra sona erer. Gerçek bir örnekte, toplu iş çalıştırmak için bir veya daha fazla kapsayıcı grubu oluşturulmasını tetikleyebilirsiniz.
 
## <a name="test-function-app-locally"></a>Test fonksiyonu uygulaması yerel

İşlev uygulaması projesini Azure'da yeniden yayımlamadan önce işlevin yerel olarak çalıştığından emin olun. [PowerShell quickstart'ta](../azure-functions/functions-create-first-function-powershell.md)gösterildiği gibi, PowerShell komut dosyasına yerel `Wait-Debugger` bir kesme noktası ve üzerine bir çağrı ekleyin. Hata ayıklama kılavuzu için, [hata ayıklama PowerShell Azure Fonksiyonlarını yerel olarak](../azure-functions/functions-debug-powershell-local.md)görün.


## <a name="republish-azure-function-app"></a>Azure işlev uygulamasını yeniden yayımla

İşlevin yerel bilgisayarınızda doğru çalıştığını doğruladıktan sonra, projeyi Azure'daki mevcut işlev uygulamasında yeniden yayımlama zamanı dır.

> [!NOTE]
> İşlevlerinizi Azure'da yayımlamadan önce yapılan `Wait-Debugger` çağrıları kaldırmayı unutmayın.

1. Visual Studio Code'da Komut Paleti'ni açın. Arama ve `Azure Functions: Deploy to function app...`seçin.
1. Zip ve dağıtmak için geçerli çalışma klasörünü seçin.
1. Aboneliği ve ardından varolan işlev uygulamasının *(myfunctionapp)* adını seçin. Önceki dağıtımın üzerine yazmak istediğinizi onaylayın.

İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Güncellediğiniz Azure kaynakları da dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **Çıktıyı** Görüntüle'yi seçin.

## <a name="run-the-function-in-azure"></a>İşlevi Azure'da çalıştırın

Dağıtım başarıyla tamamlandıktan sonra işlev URL'sini alın. Örneğin, **HttpTrigger** işlev URL'sini kopyalamak veya [Azure portalındaki](../azure-functions/functions-create-first-azure-function.md#test-the-function)işlev URL'sini almak için Visual Studio kodundaki **Azure: İşlevler** alanını kullanın.

İşlev URL'si benzersiz bir kod içerir ve formdandır:

```
https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==
```

### <a name="run-function-without-passing-a-name"></a>Bir ad geçmeden işlevi çalıştırma

İlk test olarak, `curl` komutu çalıştırın ve bir `name` sorgu dizesini eklemeden işlev URL'sini geçirin. İşlevinizin benzersiz kodunu eklediğinden emin olun.

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M=="
```

Fonksiyon durum kodu 400 ve `Please pass a name on the query string or in the request body`metin döndürür:

```
[...]
> GET /api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M== HTTP/2
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.54.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 400 
< content-length: 62
< content-type: text/plain; charset=utf-8
< date: Mon, 05 Aug 2019 22:08:15 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
Please pass a name on the query string or in the request body.
```

### <a name="run-function-and-pass-the-name-of-a-container-group"></a>İşlev çalıştırın ve bir kapsayıcı grubunun adını geçirin

Şimdi bir `curl` kapsayıcı grubunun adını *(mycontainergroup)* sorgu dizesi `&name=mycontainergroup`olarak ekleyerek komutu çalıştırın:

```bash
curl --verbose "https://myfunctionapp.azurewebsites.net/api/HttpTrigger?code=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==&name=mycontainergroup"
```

İşlev durum kodu 200'ü döndürür ve kapsayıcı grubunun oluşturulmasını tetikler:

```
[...]
> GET /api/HttpTrigger?ode=bmF/GljyfFWISqO0GngDPCtCQF4meRcBiHEoaQGeRv/Srx6dRcrk2M==&name=mycontainergroup HTTP/2
> Host: myfunctionapp.azurewebsites.net
> User-Agent: curl/7.54.0
> Accept: */*
> 
* Connection state changed (MAX_CONCURRENT_STREAMS updated)!
< HTTP/2 200 
< content-length: 28
< content-type: text/plain; charset=utf-8
< date: Mon, 05 Aug 2019 22:15:30 GMT
< 
* Connection #0 to host myfunctionapp.azurewebsites.net left intact
Started container group mycontainergroup
```

Kapsayıcının az kapsayıcı [günlükleri][az-container-logs] komutuyla çalıştırdığını doğrulayın:

```azurecli
az container logs --resource-group myfunctionapp --name mycontainergroup
```

Örnek çıktı:

```console
Hello from an Azure container instance triggered by an Azure function
```

## <a name="clean-up-resources"></a>Kaynakları temizleme

Bu eğitimde oluşturduğunuz kaynaklardan herhangi biri artık ihtiyacınız yoksa, kaynak grubunu ve içerdiği tüm kaynakları kaldırmak için [az grubu silme][az-group-delete] komutunu çalıştırabilirsiniz. Bu komut, oluşturduğunuz kapsayıcı kayıt defterinin yanı sıra çalışan kapsayıcıyı ve tüm ilişkili kaynakları siler.

```azurecli-interactive
az group delete --name myfunctionapp
```

## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, bir HTTP isteği alan ve bir kapsayıcı grubunun dağıtımını tetikleyen bir Azure işlevi oluşturdunuz. Şunları öğrendiniz:

> [!div class="checklist"]
> * TEMEL HTTP tetiklenen PowerShell işlevi oluşturmak için Azure İşlevler uzantısı ile Visual Studio Kodunu kullanın.
> * İşlev uygulamasında bir kimlik etkinleştirin ve azure kaynakları oluşturma izni verin.
> * Tek kapsayıcı kapsayıcı grubunun dağıtımını otomatikleştirmek için PowerShell işlev kodunu değiştirin.
> * Kapsayıcının HTTP tarafından tetiklenen dağıtımını doğrulayın.

Kapsayıcı laştırılmış bir işi başlatmak ve izlemek için ayrıntılı bir örnek için, [PowerShell Azure İşlevlerini ve Azure Kapsayıcı Örneklerini içeren Etkinlik Odaklı Sunucusuz Kapsayıcılar ve](https://dev.to/azure/event-driven-serverless-containers-with-powershell-azure-functions-and-azure-container-instances-e9b) beraberindeki [kod örneğine](https://github.com/anthonychu/functions-powershell-run-aci)bakın.

Azure işlevleri oluşturma ve bir işlev projesi yayınlama hakkında ayrıntılı kılavuz için [Azure İşlevleri belgelerine](/azure/azure-functions/) bakın. 

<!-- IMAGES -->


<!-- LINKS - external -->
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - internal -->

[azure-cli-install]: /cli/azure/install-azure-cli
[az-group-show]: /cli/azure/group#az-group-show
[az-group-delete]: /cli/azure/group#az-group-delete
[az-functionapp-identity-app-assign]: /cli/azure/functionapp/identity#az-functionapp-identity-assign
[new-azcontainergroup]: /powershell/module/az.containerinstance/new-azcontainergroup
[az-container-logs]: /cli/azure/container#az-container-logs
