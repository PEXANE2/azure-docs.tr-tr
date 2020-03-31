---
title: ACR Görevlerine genel bakış
description: Azure Kapsayıcı Kayıt Defteri'nde bulutta güvenli, otomatik kapsayıcı görüntü oluşturma, yönetim ve yama sağlayan bir özellik paketi olan ACR Görevleri'ne giriş.
ms.topic: article
ms.date: 01/22/2020
ms.openlocfilehash: 4fda57c1d7c866f2e6f72b04d75e53f91e995baf
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79087273"
---
# <a name="automate-container-image-builds-and-maintenance-with-acr-tasks"></a>ACR Görevleri ile kapsayıcı görüntü oluşturma ve bakım oluşturmayı otomatikleştirin

Kapsayıcılar, uygulama ve geliştirici bağımlılıklarını altyapı ve operasyonel gereksinimlerden yalıtarak yeni sanallaştırma düzeyleri sağlar. Ancak geriye kalan, bu uygulama sanallaştırmasının kapsayıcı yaşam döngüsü üzerinde nasıl yönetildiğini ve yamalı olduğunu ele alma gereğidir.

## <a name="what-is-acr-tasks"></a>ACR Görevleri Nedir?

**ACR Görevleri,** Azure Kapsayıcı Kayıt Defteri'nde yer alan bir özellik paketidir. Linux, Windows ve ARM gibi [platformlar](#image-platforms) için bulut tabanlı kapsayıcı görüntü oluşturma sağlar ve Docker kapsayıcılarınız için [işletim sistemi ve çerçeve yamalarını](#automate-os-and-framework-patching) otomatikleştirebilir. ACR Görevleri, isteğe bağlı kapsayıcı görüntü oluşturmalarıyla "iç döngü" geliştirme döngünüzün yalnızca buluta genişletilmesinin aynı zamanda kaynak kodu güncelleştirmeleri, kapsayıcının temel görüntüsündeki güncelleştirmeler veya zamanlayıcılar tarafından tetiklenen otomatik yapılara da olanak tanır. Örneğin, temel görüntü güncelleştirme tetikleyicileri ile, sabit kapsayıcıların ilkelerine bağlı kalarak güvenli ortamları koruyarak işletim sisteminizi ve uygulama çerçevesini düzeltme iş akışınızı otomatikleştirebilirsiniz.

## <a name="task-scenarios"></a>Görev senaryoları

ACR Görevleri, kapsayıcı görüntüleri ve diğer yapıları oluşturmak ve korumak için çeşitli senaryoları destekler. Ayrıntılar için bu makaledeki aşağıdaki bölümlere bakın.

* **[Hızlı görev](#quick-task)** - Azure'da, yerel bir Docker Engine yüklemesine gerek kalmadan tek bir kapsayıcı görüntüsünü isteğe bağlı olarak konteyner kayıt defterine oluşturun ve itin. Bulutun içinde düşün. `docker build` `docker push`
* **Otomatik olarak tetiklenen görevler** - Görüntü oluşturmak için bir veya daha fazla *tetikleyiciyi* etkinleştirin:
  * **[Kaynak kodu güncelleştirmede tetikleyici](#trigger-task-on-source-code-update)** 
  * **[Temel görüntü güncelleştirmesi üzerinde tetikleyici](#automate-os-and-framework-patching)** 
  * **[Zamanlamada tetikleyici](#schedule-a-task)** 
* **[Çok aşamalı görev](#multi-step-tasks)** - ACR Görevlerinin tek görüntü oluşturma ve itme yeteneğini çok adımlı, çok kapsayıcı tabanlı iş akışlarıyla genişletin. 

Her ACR Görevi ilişkili bir [kaynak kodu bağlamı](#context-locations) vardır - bir kapsayıcı görüntüsü veya başka bir yapı oluşturmak için kullanılan kaynak dosyaları kümesinin konumu. Örnek bağlamlar bir Git deposu veya yerel bir dosya sistemi içerir.

Görevler ayrıca [çalıştırılabilen değişkenlerden](container-registry-tasks-reference-yaml.md#run-variables)de yararlanabilir, böylece görev tanımlarını yeniden kullanabilir ve resimler ve yapılar için etiketleri standartlaştırabilirsiniz.

## <a name="quick-task"></a>Hızlı görev

İç-döngü geliştirme döngüsü, kod yazma yinelemeli süreci, bina, ve kaynak denetimi taahhüt etmeden önce uygulamanızı test, gerçekten konteyner yaşam döngüsü yönetiminin başlangıcıdır.

İlk kod satırınızı işlemeden önce, ACR Tasks'in [hızlı görev](container-registry-tutorial-quick-task.md) özelliği, kapsayıcı görüntü yapılarınızı Azure'a boşaltarak tümleşik bir geliştirme deneyimi sağlayabilir. Hızlı görevlerle, otomatik yapı tanımlarınızı doğrulayabilir ve kodunuzu işlemeden önce olası sorunları yakalayabilirsiniz.

Azure CLI'deki az `docker build` [kısaltma][az-acr-build] komutu tanıdık biçimi kullanarak bir [bağlam](#context-locations) (oluşturulacak dosya kümesi) alır, ona ACR Görevleri gönderir ve varsayılan olarak, tamamlanıncınca yerleşik görüntüyü kayıt defterine iter.

Giriş için Azure Kapsayıcı Kayıt Defteri'nde [bir kapsayıcı resmi oluşturmak ve çalıştırmak](container-registry-quickstart-task-cli.md) için hızlı başlat'a bakın.  

ACR Görevleri bir kapsayıcı yaşam döngüsü ilkel olarak tasarlanmıştır. Örneğin, ACR Görevlerini CI/CD çözümünüze entegre edin. Bir [hizmet ilkesi][az-login-service-principal]ile [az giriş][az-login] yürüterek, CI / CD çözüm sonra görüntü oluşturur başlatmak için [az acr yapı][az-acr-build] komutları verebilir.

İlk ACR Görevleri öğreticisinde hızlı görevleri nasıl kullanacağınızı öğrenin, [Azure Kapsayıcı Kayıt Defteri Görevleri ile bulutta kapsayıcı görüntüleri oluşturun.](container-registry-tutorial-quick-task.md)

> [!TIP]
> Bir resmi dockerfile olmadan doğrudan kaynak koddan oluşturmak ve itmek istiyorsanız, Azure Kapsayıcı Kayıt Defteri [az kısaltma paketi oluşturma][az-acr-pack-build] komutu (önizleme) sağlar. Bu [araç, Cloud Native Buildpacks](https://buildpacks.io/)kullanarak uygulama kaynak kodundan bir görüntü oluşturur ve iter.

## <a name="trigger-task-on-source-code-update"></a>Kaynak kodu güncelleştirmesinde görevi tetikleme

Kod işlendiğinde veya çekme isteği yapıldığında veya güncelleştirildiğinde, GitHub veya Azure DevOps'teki genel veya özel git deposuna bir kapsayıcı görüntüsü oluşturma veya çok adımlı görev tetikleyin. Örneğin, bir Git deposu ve isteğe bağlı olarak bir dal ve Dockerfile belirterek Bir yapı göreviazure CLI komut [az acr görev oluşturmak][az-acr-task-create] ile yapılandırın. Ekibiniz depodaki kodu güncellediğinde, ACR Görevler tarafından oluşturulan bir webhook, repo'da tanımlanan kapsayıcı görüntüsünün oluşturulmasını tetikler. 

ACR Görevleri, görev bağlamı olarak bir Git repo ayarladığınızda aşağıdaki tetikleyicileri destekler:

| Tetikleyici | Varsayılan olarak etkindir |
| ------- | ------------------ |
| İşleme | Evet |
| Çekme isteği | Hayır |

Kaynak kodu güncelleştirme tetikleyicisini yapılandırmak için, genel veya özel GitHub veya Azure DevOps repo'sunda web hook'u ayarlamak için görevi kişisel erişim belirteci (PAT) sağlamanız gerekir.

> [!NOTE]
> Şu anda, ACR Görevleri GitHub Enterprise depolarında istek tetikleyicilerini ayırmaveya çekme'yi desteklemez.

İkinci ACR Görevleri öğreticisinde kaynak kodu yla ilgili yapıları nasıl tetikleyecek, [Azure Kapsayıcı Kayıt Defteri Görevleri ile kapsayıcı oluşturmayı otomatikleştirin.](container-registry-tutorial-build-task.md)

## <a name="automate-os-and-framework-patching"></a>İşletim sistemi ve çerçeve yaması otomatikleştirin

ACR Görevleri'nin konteyner oluşturma iş akışınızı gerçekten geliştirme gücü, *temel görüntüye*yapılan bir güncelleştirmeyi algılama yeteneğinden gelir. Çoğu kapsayıcı görüntüsünün özelliği olan temel görüntü, bir veya daha fazla uygulama görüntüsünün temel aldığı bir üst görüntüdür. Temel görüntüler genellikle işletim sistemi ve bazen uygulama çerçeveleri içerir. 

Bir uygulama görüntüsü oluştururken temel görüntüye olan bağımlılığı izlemek için bir ACR görevi ayarlayabilirsiniz. Güncelleştirilmiş temel görüntü kayıt defterinize itildiğinde veya bir temel görüntü Docker Hub gibi genel bir repo'da güncelleştirildiğinde, ACR Görevleri bu resme dayalı olarak otomatik olarak herhangi bir uygulama görüntüsü oluşturabilir.
Bu otomatik algılama ve yeniden oluşturma yla, ACR Görevleri, güncelleştirilmiş temel görüntünüze başvuran her uygulama görüntüsünü el ile izlemek ve güncelleştirmek için normalde gereken zaman ve çabadan tasarruf etmenizi sağlar.

ACR Görevleri için [temel görüntü güncelleştirme tetikleyicileri](container-registry-tasks-base-images.md) hakkında daha fazla bilgi edinin. Ayrıca, temel görüntü, bir [Azure kapsayıcı kayıt defterinde güncelleştirildiğinde, öğretici Otomatikleştir kapsayıcı görüntüsündeki](container-registry-tutorial-base-image-update.md) bir kapsayıcı kayıt defterine itildiğinde görüntü oluşturmayı nasıl tetiklediğinizi öğrenin

## <a name="schedule-a-task"></a>Görev zamanlama

Görevi oluşturduğunuzda veya güncellediğinizde bir veya daha fazla *zamanlayıcı tetikleyicisini* ayarlayarak bir görevi isteğe bağlı olarak zamanlayın. Görev zamanlama, kapsayıcı iş yüklerini tanımlanmış bir zamanlamada çalıştırmak veya düzenli olarak kayıt defterinize itilen görüntüler üzerinde bakım işlemleri veya testler çalıştırmak için yararlıdır. Ayrıntılar için [bkz.](container-registry-tasks-scheduled.md)

## <a name="multi-step-tasks"></a>Çok adımlı görevler

Çok adımlı görevler, bulutta kapsayıcı görüntüleri oluşturmak, test etmek ve yamalama için adım tabanlı görev tanımı ve yürütme sağlar. [YAML dosyasında](container-registry-tasks-reference-yaml.md) tanımlanan görev adımları, kapsayıcı görüntüleri veya diğer yapılar için tek tek yapı ve itme işlemlerini belirtir. Ayrıca, her adımın kapsayıcıyı kendi yürütme ortamı olarak kullanmasıyla bir veya daha fazla kapsayıcının yürütülmesini de tanımlayabilirler.

Örneğin, aşağıdakileri otomatikleştiren çok aşamalı bir görev oluşturabilirsiniz:

1. Web uygulaması görüntüsü oluşturma
1. Web uygulama kapsayıcısını çalıştırma
1. Web uygulaması test görüntüsü oluşturma
1. Çalışan uygulama kapsayıcısına karşı testler gerçekleştiren web uygulama test kapsayıcısını çalıştırma
1. Testler geçerse, bir Miğfer grafiği arşiv paketi oluşturun
1. Yeni `helm upgrade` Helm grafik arşiv paketini kullanarak gerçekleştirme

Çok adımlı görevler, adım lar arası bağımlılık desteğiyle bir görüntüyü daha uyumlu adımlara bölmenize, çalıştırmanızı ve test etmenizi sağlar. ACR Görevleri'ndeki çok aşamalı görevlerde, görüntü oluşturma, sınama ve işletim sistemi ve çerçeve yaması iş akışları üzerinde daha ayrıntılı denetime sahip siniz.

[ACR Görevleri'ndeki çok adımlı yapı, test ve yama görevlerinde çok adımlı görevler](container-registry-tasks-multi-step.md)hakkında bilgi edinin.

## <a name="context-locations"></a>Bağlam konumları

Aşağıdaki tablo, ACR Görevleri için desteklenen bağlam konumlarının birkaç örneğini gösterir:

| Bağlam konumu | Açıklama | Örnek |
| ---------------- | ----------- | ------- |
| Yerel dosya sistemi | Yerel dosya sistemindeki bir dizindeki dosyalar. | `/home/user/projects/myapp` |
| GitHub ana dalı | Ortak veya özel Bir GitHub deposunun ana (veya diğer varsayılan) dalındaki dosyalar.  | `https://github.com/gituser/myapp-repo.git` |
| GitHub şubesi | Genel veya özel GitHub repo'nun belirli bir dalı.| `https://github.com/gituser/myapp-repo.git#mybranch` |
| GitHub alt klasörü | Ortak veya özel GitHub repo'sundaki bir alt klasördeki dosyalar. Örnek, bir dal ve alt klasör belirtiminin birleşimini gösterir. | `https://github.com/gituser/myapp-repo.git#mybranch:myfolder` |
| GitHub işlemek | Özel bir genel veya özel GitHub repo taahhüt. Örnek, işleme karma (SHA) ve alt klasör belirtiminin birleşimini gösterir. | `https://github.com/gituser/myapp-repo.git#git-commit-hash:myfolder` |
| Azure DevOps alt klasörü | Ortak veya özel Bir Azure reposundaki bir alt klasördeki dosyalar. Örnek, dal ve alt klasör belirtiminin birleşimini gösterir. | `https://dev.azure.com/user/myproject/_git/myapp-repo#mybranch:myfolder` |
| Uzak tarball | Uzak bir web sunucusundaki sıkıştırılmış arşivdeki dosyalar. | `http://remoteserver/myapp.tar.gz` |

> [!NOTE]
> Bir görev için bağlam olarak özel bir Git repo kullanırken, kişisel erişim belirteci (PAT) sağlamanız gerekir.

## <a name="image-platforms"></a>Görüntü platformları

Varsayılan olarak, ACR Görevleri Linux işletim sistemi ve amd64 mimarisi için görüntüler oluşturur. Diğer `--platform` mimariler için Windows görüntüleri veya Linux görüntüleri oluşturmak için etiketi belirtin. İşletim sistemi ve isteğe bağlı olarak desteklenen bir `--platform Linux/arm`mimariyi işletim sistemi/mimari biçiminde belirtin (örneğin, ). ARM mimarileri için isteğe bağlı olarak işletim sistemi/mimari/varyant `--platform Linux/arm64/v8`biçiminde bir varyant belirtin (örneğin,):

| İşletim Sistemi | Mimari|
| --- | ------- | 
| Linux | amd64<br/>arm<br/>arm64<br/>386 |
| Windows | amd64 |

## <a name="view-task-output"></a>Görev çıktısını görüntüleme

Her görev çalışması, görev adımlarının başarılı çalışıp çalışmadığını belirlemek için denetleyebileceğiniz günlük çıktısı oluşturur. Bir görevi el ile tetiklediğinde, görev çalışması için günlük çıktısı konsola aktarılır ve daha sonra almak için de depolanır. Bir görev otomatik olarak tetiklendiğinde, örneğin bir kaynak kodu işleme veya temel resim güncelleştirmesi tarafından, görev günlükleri yalnızca depolanır. Azure portalındaki çalışma günlüklerini görüntüleyin veya [az acr görev günlükleri](/cli/azure/acr/task#az-acr-task-logs) komutunu kullanın.

[Görev günlüklerini görüntüleme ve yönetme](container-registry-tasks-logs.md)hakkında daha fazla bilgi edin.

## <a name="next-steps"></a>Sonraki adımlar

Bulutta kapsayıcı görüntü oluşturma ve bakım oluşturmayı otomatikleştirmek için hazır olduğunuzda, [ACR Görevler öğretici serisine](container-registry-tutorial-quick-task.md)göz atın.

Azure konteyner kayıtlarınızla çalışmak için Visual [Studio Kodu için Docker Uzantısı'nı](https://code.visualstudio.com/docs/azure/docker) ve Azure [Hesabı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.azure-account) uzantısını isteğe bağlı olarak yükleyin. Görüntüleri bir Azure kapsayıcı kayıt defterine çekin ve itin veya Tümü Visual Studio Kodu'nda bulunan ACR Görevleri'ni çalıştırın.

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
[az-acr-pack-build]: /cli/azure/acr/pack#az-acr-pack-build
[az-acr-task]: /cli/azure/acr/task
[az-acr-task-create]: /cli/azure/acr/task#az-acr-task-create
[az-login]: /cli/azure/reference-index#az-login
[az-login-service-principal]: /cli/azure/authenticate-azure-cli

<!-- IMAGES -->
[quick-build-01-fork]: ./media/container-registry-tutorial-quick-build/quick-build-01-fork.png
[quick-build-02-browser]: ./media/container-registry-tutorial-quick-build/quick-build-02-browser.png
