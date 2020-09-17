---
title: Hızlı başlangıç-Docker kapsayıcısını kapsayıcı örneğine dağıtma-Docker CLı
description: Bu hızlı başlangıçta, bir yalıtılmış Azure Container örneğinde çalışan kapsayıcılı bir Web uygulamasını hızlıca dağıtmak için Docker CLı 'yı kullanırsınız
ms.topic: quickstart
ms.date: 09/14/2020
ms.custom: devx-track-javascript
ms.openlocfilehash: 37a6c430055e440d149b9a793c453251787267d4
ms.sourcegitcommit: 7374b41bb1469f2e3ef119ffaf735f03f5fad484
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/16/2020
ms.locfileid: "90709709"
---
# <a name="quickstart-deploy-a-container-instance-in-azure-using-the-docker-cli"></a>Hızlı başlangıç: Docker CLı kullanarak Azure 'da kapsayıcı örneği dağıtma

Azure 'da sunucusuz Docker kapsayıcılarını basitlik ve hızla çalıştırmak için Azure Container Instances kullanın. Bulutta yerel uygulamalar geliştirirken ve yerel geliştirmeden bulut dağıtımına sorunsuz bir şekilde geçiş yapmak istediğinizde, bir kapsayıcı örneğine istek üzerine dağıtın.

Bu hızlı başlangıçta, bir Docker kapsayıcısını dağıtmak ve uygulamasını Azure Container Instances ' de kullanılabilir hale getirmek için yerel Docker CLı komutlarını kullanırsınız. Bu özellik, [Docker ve Azure arasındaki tümleştirmeyle](https://docs.docker.com/engine/context/aci-integration/)etkinleştirilir. Bir komutu yürütmeden birkaç saniye sonra `docker run` , kapsayıcıda çalışan uygulamaya gidebilirsiniz:

:::image type="content" source="media/quickstart-docker-cli/view-application-running-in-an-azure-container-instance.png" alt-text="Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor":::

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][azure-account] oluşturun.

Bu hızlı başlangıçta, [Windows](https://desktop.docker.com/win/edge/Docker%20Desktop%20Installer.exe) veya [MacOS](https://desktop.docker.com/mac/edge/Docker.dmg)Için kullanılabilen Docker Desktop sürüm 2.3.0.5 veya üzeri bir sürümü gerekir. Veya [Linux Için Docker acı TÜMLEŞTIRMESI CLI](https://docs.docker.com/engine/context/aci-integration/#install-the-docker-aci-integration-cli-on-linux)'sını yükler. 

> [!IMPORTANT]
> Azure Container Instances özelliklerinin hepsi desteklenmez. [Aci-Integration-Beta](https://github.com/docker/aci-integration-beta) GitHub deposunda bir sorun oluşturarak Docker-Azure tümleştirmesi hakkında geri bildirim sağlayın.

[!INCLUDE [container-instances-create-docker-context](../../includes/container-instances-create-docker-context.md)]

## <a name="create-a-container"></a>Bir kapsayıcı oluşturma

Bir Docker bağlamı oluşturduktan sonra Azure 'da bir kapsayıcı oluşturabilirsiniz. Bu hızlı başlangıçta, ortak `mcr.microsoft.com/azuredocs/aci-helloworld` görüntüyü kullanırsınız. Bu görüntü, statik bir HTML sayfasına hizmet veren Node.js yazılmış küçük bir Web uygulamasını paketler.

İlk olarak, ACI bağlamına geçin. Sonraki tüm Docker komutları bu bağlamda çalışır.

```
docker context use myacicontext
```

`docker run`Internet 'e açık olan 80 numaralı bağlantı noktası Ile Azure Container örneğini oluşturmak için aşağıdaki komutu çalıştırın:

```
docker run -p 80:80 mcr.microsoft.com/azuredocs/aci-helloworld
```

Başarılı bir dağıtım için örnek çıkış:

```
[+] Running 2/2
 ⠿ hungry-kirch            Created                                                                               5.1s
 ⠿ single--container--aci  Done                                                                                 11.3s
hungry-kirch
```

`docker ps`Ortak IP adresi de dahil olmak üzere, çalışan kapsayıcı hakkında ayrıntılı bilgi almak için öğesini çalıştırın:

```
docker ps
```


Örnek çıkış, bu durumda *52.230.225.232*genel bir IP adresi gösterir:

```
CONTAINER ID        IMAGE                                        COMMAND             STATUS              PORTS
hungry-kirch        mcr.microsoft.com/azuredocs/aci-helloworld                       Running             52.230.225.232:80->80/tcp
```

 Şimdi tarayıcınızda IP adresine gidin. Aşağıdakine benzer bir web sayfası görüyorsanız kendinizi tebrik edebilirsiniz! Docker kapsayıcısında çalışan bir uygulamayı başarıyla Azure'a dağıttınız.

:::image type="content" source="media/quickstart-docker-cli/view-application-running-in-an-azure-container-instance.png" alt-text="Azure Container Instances kullanılarak dağıtılmış uygulama tarayıcıda görüntüleniyor":::

## <a name="pull-the-container-logs"></a>Kapsayıcı günlüklerini çekme

Kapsayıcıdaki veya üzerinde çalışan uygulamalardaki sorunları gidermek (veya yalnızca çıkışını görmek) istediğinizde kapsayıcı örneğinin günlüklerinden başlayın.

Örneğin, `docker logs` ACI bağlamındaki *hungın-Kirch* kapsayıcısının günlüklerini görmek için komutunu çalıştırın:

```azurecli-interactive
docker logs hungry-kirch
```

Çıkış, kapsayıcının günlüklerini görüntüler ve uygulamayı tarayıcınızda görüntülediğinizde oluşturulan HTTP GET isteklerini göstermelidir.

```output
listening on port 80
::ffff:10.240.255.55 - - [07/Jul/2020:17:43:53 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [07/Jul/2020:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
::ffff:10.240.255.55 - - [07/Jul/2020:17:44:36 +0000] "GET / HTTP/1.1" 304 - "-" "Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/72.0.3626.121 Safari/537.36"
```


## <a name="clean-up-resources"></a>Kaynakları temizleme

Kapsayıcı ile işiniz bittiğinde `docker rm` kaldırmak için öğesini çalıştırın. Bu komut, Azure Container örneğini durdurup siler.

```
docker rm hungry-kirch
```


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Docker ve Azure arasındaki tümleştirmeyi kullanarak genel görüntüden bir Azure Kapsayıcı örneği oluşturdunuz. [Docker belgelerindeki](https://docs.docker.com/engine/context/aci-integration/)tümleştirme senaryoları hakkında daha fazla bilgi edinin. 

Ayrıca, kapsayıcıları, görüntüleri ve bağlamlarını geliştirmek, çalıştırmak ve yönetmek için tümleşik bir deneyim için [Visual Studio Code Için Docker uzantısını](https://aka.ms/VSCodeDocker) da kullanabilirsiniz.

Azure Araçları 'nı kapsayıcı örnekleri oluşturmak ve yönetmek üzere kullanmak için bkz. [Azure CLI](container-instances-quickstart.md), [Azure PowerShell](container-instances-quickstart-powershell.md), [Azure Portal](container-instances-quickstart-portal.md)ve [Azure Resource Manager şablonu](container-instances-quickstart-template.md)kullanılarak diğer hızlı başlangıçlara bakın.

Çok kapsayıcılı bir uygulamayı yerel olarak tanımlayıp çalıştırmak ve sonra Azure Container Instances ' ye geçmek için Docker Compose kullanmak istiyorsanız öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Docker Compose öğreticisi](./tutorial-docker-compose.md)

<!-- LINKS - External -->

[azure-account]: https://azure.microsoft.com/free/
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

