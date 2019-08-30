---
title: Azure Container Registry görevlerle kapsayıcı görüntülerini oluşturmayı ve düzeltme ekini otomatik hale getirme (ACR görevleri)
description: Bulutta güvenli, otomatik kapsayıcı görüntüsü oluşturma, yönetim ve düzeltme eki uygulama sağlayan bir Azure Container Registry özellik paketi olan ACR görevlerine giriş.
services: container-registry
author: dlepow
manager: gwallace
ms.service: container-registry
ms.topic: article
ms.date: 06/12/2019
ms.author: danlep
ms.openlocfilehash: bc32ce59a7ec99278fb193f375d4ca945c227d2f
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70172184"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>ACR görevleriyle kapsayıcı görüntüsü derlemelerini ve bakımını otomatikleştirin

Kapsayıcılar, altyapı ve işletimsel gereksinimlerden uygulama ve geliştirici bağımlılıklarını yalıtmak için yeni sanallaştırma düzeyleri sağlar. Ancak, bu uygulama sanallaştırmanın kapsayıcı yaşam döngüsü üzerinde nasıl yönetildiğini ve düzeltme eki ekleneceğini ele alma gereksinimiyle birlikte kalır.

## <a name="what-is-acr-tasks"></a>ACR görevleri nedir?

**ACR görevleri** Azure Container Registry içindeki bir özellik paketidir. Linux, Windows ve ARM için bulut tabanlı kapsayıcı görüntüsü oluşturma ve Docker kapsayıcılarınız için [Işletim sistemi ve çerçeve düzeltme ekini](#automate-os-and-framework-patching) otomatik hale getirebilir. ACR görevleri, isteğe bağlı kapsayıcı görüntüsü Derlemeleriyle "iç döngüyle" geliştirme döngüsünü buluta genişletmez, ancak aynı zamanda kaynak kodu işlemesinde otomatik yapıları veya bir kapsayıcının temel görüntüsünü güncelleştirildiğinde de olanak sağlar. Temel görüntü güncelleştirme Tetikleyicileri ile, işletim sistemi ve uygulama çerçevesi düzeltme eki uygulama iş akışınızı otomatikleştirerek, sabit kapsayıcıların ilkelerine bağlı olarak güvenli ortamları koruyun.

ACR görevlerle kapsayıcı görüntülerini dört şekilde oluşturun ve test edin:

* [Hızlı görev](#quick-task): Yerel bir Docker altyapısı yüklemesine gerek kalmadan, Azure 'da kapsayıcı görüntülerini talep üzerine oluşturun ve gönderin. `docker build` Buluta`docker push` göz önünde bulundurun. Yerel kaynak kodundan veya git deposundan derleme.
* [Kaynak kodu Işlemede oluştur](#automatic-build-on-source-code-commit): Kod bir git deposuna işlendiği zaman bir kapsayıcı görüntü derlemesini otomatik olarak tetikleyin.
* [Temel görüntü güncelleştirmesinde oluştur](#automate-os-and-framework-patching): Bu görüntünün temel görüntüsü güncelleştirildiği zaman bir kapsayıcı görüntüsü derlemesini tetikler.
* [Çok adımlı görevler](#multi-step-tasks): Görüntüler oluşturan, kapsayıcıları komut olarak çalıştıran ve görüntüleri bir kayıt defterine ileten çok adımlı görevleri tanımlayın. ACR görevlerinin bu özelliği, isteğe bağlı görev yürütmeyi ve paralel görüntü oluşturma, test ve gönderme işlemlerini destekler.

## <a name="quick-task"></a>Hızlı görev

Kaynak denetimine işlemeden önce uygulamanızı kod yazma, oluşturma ve test etme için yinelemeli bir işlem olan iç döngü geliştirme döngüsü, kapsayıcı yaşam döngüsü yönetiminin başlangıcıdır.

İlk kod satırlarınızı işlemeden önce, ACR görevlerinin [hızlı görev](container-registry-tutorial-quick-task.md) özelliği, kapsayıcı görüntüsü derlemelerinizi Azure 'a devrederek bir tümleşik geliştirme deneyimi sağlayabilir. Hızlı görevlerle, kodunuzu işlemeden önce otomatik derleme tanımlarınızı doğrulayabilirsiniz ve olası sorunları yakalayabilin.

Tanıdık `docker build` biçimi kullanarak, Azure CLI 'deki [az ACR Build][az-acr-build] komutu bir *bağlam* alır (oluşturulacak dosya kümesi), ACR görevlerini gönderir ve varsayılan olarak, oluşturulan görüntüyü tamamlandıktan sonra kayıt defterine gönderir.

Giriş için bkz. Azure Container Registry [bir kapsayıcı görüntüsü oluşturma ve çalıştırma](container-registry-quickstart-task-cli.md) hızlı başlangıcı.  

Aşağıdaki tabloda ACR görevleri için desteklenen bağlam konumlarına yönelik birkaç örnek gösterilmektedir:

| Bağlam konumu | Açıklama | Örnek |
| ---------------- | ----------- | ------- |
| Yerel dosya sistemi | Yerel dosya sisteminde bir dizin içindeki dosyalar. | `/home/user/projects/myapp` |
| GitHub ana dalı | GitHub deposunun ana (veya diğer varsayılan) daldaki dosyalar.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub dalı | GitHub deposunun belirli bir dalı.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub alt klasörü | GitHub deposunda bulunan bir alt klasör içindeki dosyalar. Örnek, bir dal ve alt klasör belirtiminin birleşimini gösterir. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| Uzak tarbol | Uzak Web sunucusu üzerindeki sıkıştırılmış arşivdeki dosyalar. | `http://remoteserver/myapp.tar.gz` |

Varsayılan olarak ACR görevleri, Linux işletim sistemi ve AMD64 mimarisi için görüntüler oluşturur. Diğer mimarilere yönelik Windows görüntülerini veya Linux görüntülerini oluşturmak için etiketibelirtin.`--platform` İşletim sistemini ve isteğe bağlı olarak desteklenen bir mimariyi OS/Architecture biçiminde belirtin (örneğin, `--platform Linux/arm`). ARM mimarileri için isteğe bağlı olarak OS/Architecture/VARIANT biçiminde bir değişken belirtin (örneğin, `--platform Linux/arm64/v8`):

| OS | Mimari|
| --- | ------- | 
| Linux | 'tür<br/>uzaklığını<br/>arm64<br/>386 |
| Windows | 'tür |

ACR görevleri kapsayıcı yaşam döngüsü temel olarak tasarlanmıştır. Örneğin, ACR görevlerini CI/CD çözümünüz ile tümleştirin. Bir [hizmet sorumlusu][az-login-service-principal]ile [az oturum açma][az-login] ÇALıŞTıRARAK, CI/CD çözümünüz görüntü yapılarını açmak için [az ACR Build][az-acr-build] komutları verebilir.

İlk ACR görevleri öğreticisinde hızlı görevleri nasıl kullanacağınızı öğrenin, [bulutta kapsayıcı görüntülerini Azure Container Registry görevlerle oluşturun](container-registry-tutorial-quick-task.md).

## <a name="automatic-build-on-source-code-commit"></a>Kaynak kodu işlemede otomatik derleme

Kod, GitHub veya Azure DevOps 'daki bir git deposuna işlendiği zaman bir kapsayıcı görüntüsü derlemesini otomatik olarak tetiklemek için ACR görevlerini kullanın. Derleme görevleri, Azure CLı komutu [az ACR göreviyle][az-acr-task]yapılandırılabilir, bir git deposu ve isteğe bağlı olarak bir dal ve Dockerfile belirtmenize izin verir. Takımınız depoya kod işleişlerse, ACR görevler tarafından oluşturulan bir Web kancası depoda tanımlanan kapsayıcı görüntüsünün derlemesini tetikler.

> [!IMPORTANT]
> Daha önce `az acr build-task` komutuyla önizleme sırasında görevler oluşturduysanız, bu görevlerin [az ACR Task][az-acr-task] komutu kullanılarak yeniden oluşturulması gerekir.

İkinci ACR görevleri öğreticisinde kaynak kodu işlemesinde derlemelerin nasıl tetikleneceğini öğrenin, [Azure Container Registry görevlerle kapsayıcı görüntüsü derlemelerini otomatikleştirin](container-registry-tutorial-build-task.md).

## <a name="automate-os-and-framework-patching"></a>İşletim sistemi ve çerçeve düzeltme ekini otomatikleştirme

ACR görevlerinin, kapsayıcı derleme iş akışınızı gerçek anlamda geliştirme gücü, bir temel görüntüde güncelleştirme algılama özelliğinden geliyor. Güncelleştirilmiş temel görüntü kayıt defterinize gönderildiğinde veya Docker Hub 'daki gibi bir genel depoda bir temel görüntü güncelleştirilirse, ACR görevleri bu uygulamayı temel alan herhangi bir uygulama görüntüsünü otomatik olarak oluşturabilir.

Kapsayıcı görüntüleri, *temel* görüntülere ve *uygulama* görüntülerine büyük ölçüde kategorilere ayrılabilir. Temel görüntüleriniz genellikle uygulamanızın oluşturulduğu işletim sistemini ve uygulama çerçevelerini, diğer özelleştirmelerle birlikte içerir. Bu temel görüntüler genellikle genel yukarı akış görüntülerini temel alır, örneğin: [Alp Linux][base-alpine], [Windows][base-windows], [.net][base-dotnet]veya [Node. js][base-node]. Uygulama görüntülerinizin birkaçı ortak bir temel görüntü paylaşabilir.

Bir işletim sistemi veya uygulama çerçevesi görüntüsü, örneğin, kritik bir işletim sistemi güvenlik düzeltme ekine sahip olan yukarı akış bakımını tarafından güncelleştirildiği zaman, temel görüntülerinizi de kritik düzeltme içerecek şekilde güncelleştirmeniz gerekir. Ayrıca, bu yukarı akış düzeltmelerini temel görüntıza dahil etmek için her bir uygulama görüntüsünün yeniden oluşturulması gerekir.

ACR görevleri bir kapsayıcı görüntüsü oluşturduğunda temel görüntü bağımlılıklarını dinamik olarak bulduğu için, bir uygulama görüntüsünün temel görüntüsünün ne zaman güncelleştirileceğini algılayabilir. Önceden yapılandırılmış bir [derleme göreviyle](container-registry-tutorial-base-image-update.md#create-a-task), ACR görevleri daha sonra **her uygulama görüntüsünü sizin için otomatik olarak yeniden oluşturur** . Bu otomatik algılama ve yeniden oluşturma ile, ACR görevleri, güncelleştirilmiş temel görüntenize başvuran her bir uygulama görüntüsünü el ile izlemek ve güncelleştirmek için normalde gereken zaman ve çabayı kaydeder.

Bir ACR görevi, temel görüntü aşağıdaki konumlardan birinde olduğunda bir temel görüntü güncelleştirmesini izler:

* Görevin çalıştığı aynı Azure Container kayıt defteri
* Aynı bölgedeki başka bir Azure Kapsayıcı kayıt defteri 
* Docker Hub 'da ortak depo
* Microsoft Container Registry genel depo

Üçüncü ACR görevleri öğreticisinde işletim sistemi ve çerçeve düzeltme eki uygulama hakkında daha fazla bilgi edinin. [Azure Container Registry görevlerle, temel görüntü güncelleştirme üzerinde görüntü derlemelerini otomatikleştirin](container-registry-tutorial-base-image-update.md).

## <a name="multi-step-tasks"></a>Çok adımlı görevler

Çok adımlı görevler, bulutta kapsayıcı görüntüleri oluşturmak, test etmek ve düzeltme eki uygulamak için adım tabanlı görev tanımı ve yürütme sağlar. Görev adımları, tek tek kapsayıcı derleme ve gönderme işlemlerini tanımlar. Ayrıca, her adımın kapsayıcıyı kendi yürütme ortamı olarak kullanmasıyla bir veya daha fazla kapsayıcının yürütülmesini de tanımlayabilirler.

Örneğin, aşağıdakileri otomatikleştiren bir çok adımlı görev oluşturabilirsiniz:

1. Web uygulaması görüntüsü oluşturma
1. Web uygulaması kapsayıcısını çalıştırma
1. Web uygulaması test görüntüsü oluşturma
1. Çalışan uygulama kapsayıcısına karşı testler gerçekleştiren Web uygulaması test kapsayıcısını çalıştırın
1. Testler başarılı olursa bir Helu grafik arşiv paketi oluşturun
1. Yeni helmchart arşiv paketini kullanarak bir `helm upgrade` gerçekleştir

Çok adımlı görevler, bir görüntünün oluşturma, çalıştırma ve test etme özelliğini, adım adım bağımlılık desteğiyle daha birleştirilebilir adımlara bölmeye olanak tanır. ACR görevlerinde çok adımlı görevlerle, görüntü oluşturma, test etme ve işletim sistemi ve çerçeve düzeltme eki uygulama iş akışları üzerinde daha ayrıntılı denetime sahip olursunuz.

[ACR görevlerinde multi-step Build, test ve Patch görevlerini Çalıştır](container-registry-tasks-multi-step.md)bölümünde çok adımlı görevler hakkında bilgi edinin.

## <a name="view-task-logs"></a>Görev günlüklerini görüntüle

Her görev çalıştırması, görev adımlarının başarıyla çalışıp çalışmadığını belirleyebilmek için inceleyebileceğiniz günlük çıktısı üretir. Görevi tetiklemek için [az ACR Build](/cli/azure/acr#az-acr-build), [az ACR Run](/cli/azure/acr#az-acr-run)veya [az ACR Task Run](/cli/azure/acr/task#az-acr-task-run) komutunu kullanırsanız, görev çalıştırmasının günlük çıktısı konsola akışla kaydedilir ve ayrıca daha sonra almak üzere saklanır. Azure portal çalışan bir görevin günlüklerini görüntüleyin veya [az ACR görev günlükleri](/cli/azure/acr/task#az-acr-task-logs) komutunu kullanın.

2019 Temmuz 'dan başlayarak, bir kayıt defterindeki görev çalıştırmaları için veriler ve Günlükler varsayılan olarak 30 gün boyunca korunur ve sonra otomatik olarak temizlenir. Bir görev çalıştırmasının verilerini arşivlemek istiyorsanız, [az ACR Task Update-Run](/cli/azure/acr/task#az-acr-task-update-run) komutunu kullanarak arşivlemeyi etkinleştirin. Aşağıdaki örnek, kayıt defteri *myregistry*içinde *CF11* çalıştırması görevi için arşivlemeyi mümkün bir şekilde sunar.

```azurecli
az acr task update-run --registry myregistry --run-id cf11 --no-archive false
```

## <a name="next-steps"></a>Sonraki adımlar

Bulutta kapsayıcı görüntülerinizi oluşturarak işletim sistemi ve çerçeve düzeltme eki uygulamayı otomatik hale getirmeye hazır olduğunuzda, üç bölümlü [ACR görevler öğretici serisini](container-registry-tutorial-quick-task.md)inceleyin.

İsteğe bağlı olarak [Visual Studio Code Için Docker uzantısını](https://code.visualstudio.com/docs/azure/docker) ve Azure Container Registry 'larınız ile birlikte çalışmak Için [Azure hesap](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısını yükler. Azure Container Registry 'ye görüntü çekme ve gönderme veya ACR görevlerini Visual Studio Code.

<!-- LINKS - External -->
[base-alpine]: https://hub.docker.com/_/alpine/
[base-dotnet]: https://hub.docker.com/r/microsoft/dotnet/
[base-node]: https://hub.docker.com/_/node/
[base-windows]: https://hub.docker.com/r/microsoft/nanoserver/
[sample-archive]: https://github.com/Azure-Samples/acr-build-helloworld-node/archive/master.zip
[terms-of-use]: https://azure.microsoft.com/support/legal/preview-supplemental-terms/

<!-- LINKS - Internal -->
[azure-cli]: /cli/azure/install-azure-cli
[az-acr-build]: /cli/azure/acr#az-acr-build
[az-acr-task]: /cli/azure/acr
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
