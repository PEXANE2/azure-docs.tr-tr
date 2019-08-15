---
title: Sorun giderme
titleSuffix: Azure Dev Spaces
services: azure-dev-spaces
ms.service: azure-dev-spaces
author: zr-msft
ms.author: zarhoads
ms.date: 09/11/2018
ms.topic: conceptual
description: Azure’da kapsayıcılar ve mikro hizmetlerle hızlı Kubernetes geliştirme
keywords: 'Docker, Kubernetes, Azure, AKS, Azure Kubernetes hizmeti, kapsayıcılar, Held, hizmet ağı, hizmet kafesi yönlendirme, kubectl, k8s '
ms.openlocfilehash: 6ab2e0866c4e6c5cc8f89cb490504f6ca6a076fc
ms.sourcegitcommit: b12a25fc93559820cd9c925f9d0766d6a8963703
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2019
ms.locfileid: "69019639"
---
# <a name="troubleshooting-guide"></a>Sorun giderme kılavuzu

Bu kılavuz, Azure geliştirme alanları kullanılırken olabilir sık karşılaşılan sorunlar hakkında bilgi içerir.

Azure Dev Spaces kullanırken bir sorununuz varsa, [Azure dev Spaces GitHub deposunda bir sorun](https://github.com/Azure/dev-spaces/issues)oluşturun.

## <a name="enabling-detailed-logging"></a>Ayrıntılı günlük kaydını etkinleştirme

Sorunları daha etkili bir şekilde gidermek için inceleme için daha ayrıntılı Günlükler oluşturmaya yardımcı olabilir.

Visual Studio uzantısı için `MS_VS_AZUREDEVSPACES_TOOLS_LOGGING_ENABLED` ortam değişkeni 1. Visual Studio ortam değişkeni için etkili olması için yeniden emin olun. Etkinleştirildikten sonra, ayrıntılı Günlükler `%TEMP%\Microsoft.VisualStudio.Azure.DevSpaces.Tools` dizininize yazılır.

CLI, komut yürütme sırasında daha fazla bilgi kullanarak çıkarabilirsiniz `--verbose` geçin. Daha ayrıntılı günlükleri de göz atabilirsiniz `%TEMP%\Azure Dev Spaces`. Mac bilgisayarlarda, çalıştırarak TEMP dizininde bulunabilir `echo $TMPDIR` bir terminal penceresinden. Bir Linux bilgisayarında, TEMP dizini genellikle olan `/tmp`.

## <a name="debugging-services-with-multiple-instances"></a>Birden çok örnek ile hata ayıklama Hizmetleri

Şu anda, Azure Dev Spaces tek bir örneğin veya Pod hata ayıklaması yaparken en iyi şekilde çalışıyor. Azds. YAML dosyası, Kubernetes 'in hizmetinize çalıştığı düğüm sayısını gösteren bir, *Replicacount*ayarı içerir. Uygulamanızı belirli bir hizmet için birden çok Pod çalıştıracak şekilde yapılandırmak üzere replicacount değerini değiştirirseniz, hata ayıklayıcı alfabetik olarak listelendiğinde ilk Pod 'a iliştirir. Özgün Pod geri dönüştürüldüğünde hata ayıklayıcı farklı bir pod 'a iliştirir, muhtemelen beklenmedik davranışa neden olur.

## <a name="error-failed-to-create-azure-dev-spaces-controller"></a>'Azure geliştirme alanları denetleyicisi oluşturmak için başarısız' hatası

### <a name="reason"></a>Reason
Bir şeyler denetleyicisi oluşturulmasını yanlış gittiğinde şu hatayla karşılaşabilirsiniz. Geçici bir hatalarsa, onarmak için denetleyiciyi silin ve yeniden oluşturun.

### <a name="try"></a>Deneme

Denetleyiciyi silin:

```bash
azds remove -g <resource group name> -n <cluster name>
```

Bir denetleyiciyi silmek için Azure Dev Spaces CLı kullanmanız gerekir. Bir denetleyiciyi Visual Studio 'dan silmek mümkün değildir. Ayrıca, Azure Cloud Shell bir denetleyiciyi silebilmek için Azure Cloud Shell Azure Dev Spaces CLı 'yı yükleyemezsiniz.

Azure Dev Spaces CLı yüklü değilse, önce aşağıdaki komutu kullanarak bu dosyayı yükleyebilir, sonra denetleyicinizi silebilirsiniz:

```cmd
az aks use-dev-spaces -g <resource group name> -n <cluster name>
```

Denetleyici yeniden CLI veya Visual Studio'dan yapılabilir. Örnekler için bkz. [.NET Core](quickstart-netcore-visualstudio.md) hızlı başlangıçlarla [Takım geliştirme](quickstart-team-development.md) veya geliştirme.

## <a name="error-service-cannot-be-started"></a>Hata 'hizmeti başlatılamıyor.'

Başlatmak hizmet kodunuzu başarısız olduğunda bu hatayı görebilirsiniz. Genellikle kullanıcı kodunda nedenidir. Daha fazla tanı bilgilerini almak için komutlar ve ayarlar aşağıdaki değişiklikleri yapın:

### <a name="try"></a>Deneyin:

Komut satırında:

Kullanırken _azds.exe_, kullanın verbose komut satırı seçeneğini kullanıp çıkış biçimini belirtmek için output komut satırı seçeneği.
 
```cmd
azds up --verbose --output json
```

Visual Studio'da:

1. Açık **Araçlar > Seçenekler** altında **projeler ve çözümler**, seçin **derleme ve çalıştırma**.
2. Ayarlarını değiştirmek için **MSBuild proje oluşturması çıkış ayrıntısı** için **ayrıntılı** veya **tanılama**.

    ![Ekran Araçlar, Seçenekler iletişim kutusu](media/common/VerbositySetting.PNG)
    
### <a name="multi-stage-dockerfiles"></a>Çok aşamalı dockerfile'ları:
Bir çok aşamalı Dockerfile kullanılırken bir *hizmet başlatılamıyor* hatası alıyorsunuz. Bu durumda, ayrıntılı çıktı aşağıdaki metni içerir:

```cmd
$ azds up -v
Using dev space 'default' with target 'AksClusterName'
Synchronizing files...6s
Installing Helm chart...2s
Waiting for container image build...10s
Building container image...
Step 1/12 : FROM [imagename:tag] AS base
Error parsing reference: "[imagename:tag] AS base" is not a valid repository/tag: invalid reference format
Failed to build container image.
Service cannot be started.
```

Bu hata, AKS düğümlerinin çok aşamalı derlemeleri desteklemeyen daha eski bir Docker sürümünü çalıştırdığı için oluşur. Çok aşamalı derlemelerin önüne geçmek için Dockerfile dosyanızı yeniden yazın.

### <a name="rerunning-a-service-after-controller-re-creation"></a>Denetleyici yeniden oluşturulduktan sonra bir hizmeti yeniden çalıştırma
Bir hizmeti kaldırdıktan sonra yeniden çalıştırmayı denerken bir *Hizmet başlatılamaz* ve bu kümeyle ilişkili Azure dev Spaces denetleyiciyi yeniden oluşturursunuz. Bu durumda, ayrıntılı çıktı aşağıdaki metni içerir:

```cmd
Installing Helm chart...
Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
Helm install failed with exit code '1': Release "azds-33d46b-default-webapp1" does not exist. Installing it now.
Error: release azds-33d46b-default-webapp1 failed: services "webapp1" already exists
```

Bu hata, dev Spaces denetleyicisinin kaldırılması bu denetleyici tarafından daha önce yüklenen hizmetleri kaldırmadığı için oluşur. Eski Hizmetleri yerinde olduğundan denetleyicisi yeniden oluşturma ve ardından yeni denetleyicisi kullanarak hizmetlerini çalıştırma denemesi başarısız olur.

Bu sorunu gidermek için `kubectl delete` komutunu kullanarak eski Hizmetleri kümeinizden el ile kaldırın ve ardından yeni hizmetleri yüklemek için geliştirme alanlarını yeniden çalıştırın.

## <a name="dns-name-resolution-fails-for-a-public-url-associated-with-a-dev-spaces-service"></a>Geliştirme alanları hizmeti ile ilişkilendirilen genel bir URL için DNS adı çözümlemesi başarısız olur

`azds prep` Komutuna olan `--public` anahtarı belirterek veya Visual Studio 'daki `Publicly Accessible` onay kutusunu seçerek hizmetiniz için genel bir URL uç noktası yapılandırabilirsiniz. Genel DNS adı, hizmetinizi geliştirme alanlarında çalıştırdığınızda otomatik olarak kaydedilir. Bu DNS adı kayıtlı değilse, genel URL 'ye bağlanılırken bir *sayfa görüntülenemiyor* veya Web tarayıcınızda *siteye ulaşılamıyor* hatası görürsünüz.

### <a name="try"></a>Deneyin:

Geliştirme alanları hizmetlerinizle ilişkili tüm URL'lerin listesini aşağıdaki komutu kullanabilirsiniz:

```cmd
azds list-uris
```

Bir URL ise *bekleyen* geliştirme alanları tamamlamak için DNS kaydı için hala bekliyor anlamına gelen durumu. Bazı durumlarda, kayıt tamamlanması birkaç dakika sürer. Geliştirme alanları localhost tünel DNS kaydında beklenirken kullanabileceğiniz her hizmet için de açılır.

URL, 5 dakikadan uzun bir süre boyunca *bekleme* durumunda kalırsa, genel uç noktasını veya genel uç noktasını elde eden NGINX giriş denetleyicisi Pod 'sini oluşturan dış DNS Pod ile ilgili bir sorun olduğunu gösterebilir. Bu pod'ları silmek için aşağıdaki komutları kullanabilirsiniz. AKS, silinen pods 'yi otomatik olarak yeniden oluşturur.

```cmd
kubectl delete pod -n kube-system -l app=addon-http-application-routing-external-dns
kubectl delete pod -n kube-system -l app=addon-http-application-routing-nginx-ingress
```

## <a name="error-required-tools-and-configurations-are-missing"></a>'Araçlar ve yapılandırmaları eksik gerekli' hatası

VS Code başlatırken bu hata oluşabilir: "[Azure geliştirme alanları] araçları ve derleme ve '[Proje adı]' hata ayıklama yapılandırmaları gerekli eksik."
Hata, o azds.exe yol ortam değişkeninde değil VS Code'da görüldüğü anlamına gelir.

### <a name="try"></a>Deneyin:

VS Code, PATH ortam değişkenine düzgün olarak ayarlandığı bir komut isteminden başlatın.

## <a name="error-required-tools-to-build-and-debug-projectname-are-out-of-date"></a>Hata "derleme ve hata ayıklama için gerekli araçlar ' ProjectName ' güncel değil."

Azure Dev Spaces için VS Code uzantısının daha yeni bir sürümüne sahipseniz, ancak Azure Dev Spaces CLı 'nin daha eski bir sürümü olan bu hatayı Visual Studio Code görürsünüz.

### <a name="try"></a>Deneme

Azure Dev Spaces CLı 'nın en son sürümünü indirip yükleyin:

* [Windows](https://aka.ms/get-azds-windows)
* [Mac](https://aka.ms/get-azds-mac)
* [Linux](https://aka.ms/get-azds-linux)

## <a name="error-azds-is-not-recognized-as-an-internal-or-external-command-operable-program-or-batch-file"></a>Hata 'azds' iç ya da dış komut, çalıştırılabilir program veya toplu iş dosyası tanınmıyor
 
Bu hata azds.exe yüklü değil veya doğru bir şekilde yapılandırıldığını görebilirsiniz.

### <a name="try"></a>Deneyin:

1. AZD. exe için% ProgramFiles%/Microsoft SDKs\Azure\Azure dev Spaces CLı konumunu denetleyin. Yoktur, bu konuma PATH ortam değişkenine ekleyin.
2. Azds.exe yüklü değilse aşağıdaki komutu çalıştırın:

    ```cmd
    az aks use-dev-spaces -n <cluster-name> -g <resource-group>
    ```

## <a name="warning-dockerfile-could-not-be-generated-due-to-unsupported-language"></a>Uyarı 'Dockerfile nedeniyle desteklenmeyen dil üretilemedi'
Azure geliştirme alanları, C# ve Node.js için yerel destek sağlar. Çalıştırdığınızda *azds hazırlığı* bu dillerden birinde yazılan kod içeren bir dizinde Azure geliştirme alanları otomatik olarak uygun bir Dockerfile sizin için oluşturur.

Azure Dev Spaces başka dillerde yazılmış kodla kullanmaya devam edebilirsiniz, ancak ilk kez azları çalıştırmadan önce Dockerfile 'ı el ile oluşturmanız gerekir .

### <a name="try"></a>Deneyin:
Uygulamanız, Azure Dev Spaces yerel olarak desteklenmeyen bir dilde yazılmışsa, kodunuzu çalıştıran bir kapsayıcı görüntüsü oluşturmak için uygun bir Dockerfile sağlamanız gerekir. Docker, Dockerfiles ve gereksinimlerinize uygun bir Dockerfile başvurusu yazmanıza yardımcı olabilecek bir [dockerfile başvurusu](https://docs.docker.com/engine/reference/builder/) [yazmak için en iyi yöntemlerin bir listesini](https://docs.docker.com/develop/develop-images/dockerfile_best-practices/) sağlar.

Uygun bir Dockerfile sağlandıktan çalışır durumda geçebilirsiniz *yukarı azds* Azure geliştirme alanlarında uygulamanızı çalıştırmak için.

## <a name="error-upstream-connect-error-or-disconnectreset-before-headers"></a>Hata 'yukarı bağlantı hatası veya üst bilgileri önce bağlantıyı kes/reset'
Hizmetinizi erişmeye çalışırken bu hatayı görebilirsiniz. Örneğin, ne zaman, hizmetin URL'sini bir tarayıcıda gidin. 

### <a name="reason"></a>Neden 
Kapsayıcı bağlantı noktası kullanılamaz. Bu sorun nedeniyle oluşabilir: 
* , Yine yerleşik dağıtılan ve sürecinde kapsayıcıdır. Çalıştırırsanız bu sorun ortaya çıkabilecek `azds up` veya hata ayıklayıcıyı başlatın ve ardından başarıyla dağıtıldığını önce kapsayıcı erişmeyi deneyin.
* Bağlantı noktası yapılandırması arasında tutarlı değil, _Dockerfile_, Helm grafiği ve sunucu kodlar bir bağlantı noktası açar.

### <a name="try"></a>Deneyin:
1. Kapsayıcı yerleşik/dağıtılan aşamasında olan 2-3 saniye bekleyin ve hizmete tekrar erişmeyi deneyin. 
1. Bağlantı noktası yapılandırmanızı denetleyin. Belirtilen bağlantı noktası numaraları aşağıdaki varlıkların tümünde **özdeş** olmalıdır:
    * **Dockerfile** `EXPOSE` Yönerge tarafından belirtilen.
    * **[Helb grafiği](https://docs.helm.sh):** , `externalPort` Bir hizmetin ve `internalPort` değerleri tarafından belirtilir (genellikle bir `values.yml` dosyada bulunur),
    * Örneğin, Node.js içinde uygulama kodunda açılan tüm bağlantı noktaları: `var server = app.listen(80, function () {...}`


## <a name="config-file-not-found"></a>Yapılandırma dosyası bulunamadı
Çalıştırmadan `azds up` ve aşağıdaki hatayı alıyorsunuz: `Config file not found: .../azds.yaml`

### <a name="reason"></a>Neden
Çalıştırmalısınız `azds up` çalıştırmak istediğiniz kodu kök dizininden ve Azure Dev alanları ile çalıştırmak için kod klasörü başlatmanız gerekir.

### <a name="try"></a>Deneyin:
1. Geçerli dizin hizmeti kodunuzu içeren kök klasöre değiştirin. 
1. Yoksa bir _azds.yaml_ çalıştırın kod klasörü dosyasında `azds prep` Docker, Kubernetes ve Azure Dev alanları varlıklar oluşturmak için.

## <a name="error-the-pipe-program-azds-exited-unexpectedly-with-code-126"></a>Hata: ' Kanal programı ' azds ', kod 126 ile beklenmedik bir şekilde çıktı. '
VS Code hata ayıklayıcı başlatılıyor, bazen bu hataya neden olabilir.

### <a name="try"></a>Deneyin:
1. VS Code kapatıp yeniden açın.
2. F5'e yeniden'e basın.

## <a name="debugging-error-failed-to-find-debugger-extension-for-typecoreclr"></a>'Hata ayıklama uzantısı türü: coreclr bulmak için başarısız' hata ayıklama
VS Code hata ayıklayıcısı çalıştırma, hata raporları: `Failed to find debugger extension for type:coreclr.`

### <a name="reason"></a>Neden
C# geliştirme makinenizde yüklü VS Code uzantı yoktur. Uzantı C# , .NET Core (CoreCLR) için hata ayıklama desteği içerir.

### <a name="try"></a>Deneyin:
Yükleme [C# için VS Code uzantısı](https://marketplace.visualstudio.com/items?itemName=ms-vscode.csharp).

## <a name="debugging-error-configured-debug-type-coreclr-is-not-supported"></a>'Yapılandırıldı hata ayıklama türü 'coreclr' desteklenmeyen' hata ayıklama
VS Code hata ayıklayıcısı çalıştırma, hata raporları: `Configured debug type 'coreclr' is not supported.`

### <a name="reason"></a>Neden
Azure geliştirme geliştirme makinenizde yüklü alanları için VS Code uzantı yoktur.

### <a name="try"></a>Deneyin:
Yükleme [Azure geliştirme alanları için VS Code uzantısı](get-started-netcore.md).

## <a name="debugging-error-invalid-cwd-value-src-the-system-cannot-find-the-file-specified-or-launch-program-srcpath-to-project-binary-does-not-exist"></a>Hata ayıklama "geçersiz 'cwd' değeri ' / src'. Sistem belirtilen dosyayı bulamıyor." veya "Başlat: '/ src / [Proje ikili dosya yolu]' programı yok"
VS Code hata ayıklayıcısı çalıştırma, hata raporları `Invalid 'cwd' value '/src'. The system cannot find the file specified.` ve/veya `launch: program '/src/[path to project executable]' does not exist`

### <a name="reason"></a>Neden
Varsayılan olarak, VS Code uzantısı kullanan `src` kapsayıcı üzerindeki proje için çalışma dizini olarak. Güncelleştirdiyseniz, `Dockerfile` farklı bir çalışma dizini belirtmek için bu hatayı görebilirsiniz.

### <a name="try"></a>Deneyin:
Güncelleştirme `launch.json` altında dosya `.vscode` proje klasörünüze alt dizinidir. Değişiklik `configurations->cwd` yönergesi ile aynı dizine işaret edecek şekilde `WORKDIR` projenizin tanımlı `Dockerfile`. Güncelleştirmeniz gerekebilir `configurations->program` de yönergesi.

## <a name="the-type-or-namespace-name-mylibrary-could-not-be-found"></a>'Kitaplığım' tür veya ad alanı bulunamadı

### <a name="reason"></a>Reason 
Derleme bağlamı, varsayılan olarak proje/hizmet düzeyidir ve bu nedenle kullandığınız bir kitaplık projesi bulunamıyor.

### <a name="try"></a>Deneyin:
Yapmanız gerekenler:
1. Değiştirme _azds.yaml_ derleme bağlamı için çözüm düzeyi ayarlamak için dosya.
2. Değiştirme _Dockerfile_ ve _Dockerfile.develop_ projesine başvuruda bulunmak için dosyaları ( _.csproj_) dosyaları doğru bir şekilde yeni göre derleme bağlamı.
3. Bir yerde bir _.dockerignore_ dosya .sln dosyasını ve gerektiği gibi değiştirin.

Bir örneğe göz bulabilirsiniz https://github.com/sgreenmsft/buildcontextsample

## <a name="microsoftdevspacesregisteraction-authorization-error"></a>'Microsoft.DevSpaces/register/action' Yetkilendirme hatası
Azure Dev Spaces yönetmek için Azure aboneliğinizde *sahip* veya *katkıda bulunan* erişime ihtiyacınız vardır. Dev alanlarını yönetmeye çalışıyorsanız ve ilişkili Azure aboneliğine *sahip* veya *katılımcı* erişiminiz yoksa bu hatayı görebilirsiniz.
`The client '<User email/Id>' with object id '<Guid>' does not have authorization to perform action 'Microsoft.DevSpaces/register/action' over scope '/subscriptions/<Subscription Id>'.`

### <a name="reason"></a>Reason
Seçilen Azure aboneliği kaydedilmemiş `Microsoft.DevSpaces` ad alanı.

### <a name="try"></a>Deneyin:
Azure aboneliğinin sahibi veya katkıda bulunan erişimi olan el ile kaydetmek için aşağıdaki Azure CLI komutunu çalıştırabilirsiniz `Microsoft.DevSpaces` ad alanı:

```cmd
az provider register --namespace Microsoft.DevSpaces
```

## <a name="dev-spaces-times-out-at-waiting-for-container-image-build-step-with-aks-virtual-nodes"></a>Geliştirme alanları zaman aşımına uğrar ve AKS sanal düğümleri ile *ilgili kapsayıcı görüntüsü oluşturma...* adımı

### <a name="reason"></a>Reason
Bu zaman aşımı, bir [aks sanal düğümünde](https://docs.microsoft.com/azure/aks/virtual-nodes-portal)çalışacak şekilde yapılandırılmış bir hizmeti çalıştırmak Için geliştirme alanlarını kullanmaya çalıştığınızda oluşur. Geliştirme alanları şu anda sanal düğümlerde Hizmetleri oluşturmayı veya hata ayıklamayı desteklememektedir.

Anahtarla`--verbose` çalıştırırsanız `azds up` veya Visual Studio 'da ayrıntılı günlük kaydını etkinleştirirseniz, ek ayrıntı görürsünüz:

```cmd
$ azds up --verbose

Installed chart in 2s
Waiting for container image build...
pods/mywebapi-76cf5f69bb-lgprv: Scheduled: Successfully assigned default/mywebapi-76cf5f69bb-lgprv to virtual-node-aci-linux
Streaming build container logs for service 'mywebapi' failed with: Timed out after 601.3037572 seconds trying to start build logs streaming operation. 10m 1s
Container image build failed
```

Yukarıdaki komut, hizmetin Pod öğesinin sanal düğüm *-aci-Linux*'a atandığını gösterir ve bu sanal bir düğümdür.

### <a name="try"></a>Deneyin:
Hizmetin sanal bir düğümde çalışmasına izin veren tüm *Nodeselector* ve/veya *toleranations* değerlerini kaldırmak için helb grafiğini güncelleştirin. Bu değerler genellikle grafiğin `values.yaml` dosyasında tanımlanmıştır.

Geliştirme alanları aracılığıyla derlemek/hata ayıklamak istediğiniz hizmet bir VM düğümünde çalışıyorsa, sanal düğümler özelliği etkinleştirilmiş bir AKS kümesini kullanmaya devam edebilirsiniz. Bu, varsayılan yapılandırmadır.

## <a name="error-could-not-find-a-ready-tiller-pod-when-launching-dev-spaces"></a>"Hata: hazır tiller pod bulunamadı" geliştirme alanları başlatırken

### <a name="reason"></a>Neden
Helm istemci artık kümede çalışan Tiller pod konuşabilirsiniz değilse bu hata oluşur.

### <a name="try"></a>Deneyin:
Genellikle, kümenizin aracı düğümleri yeniden başlatılıyor, bu sorunu çözer.

## <a name="error-release-azds-identifier-spacename-servicename-failed-services-servicename-already-exists-or-pull-access-denied-for-servicename-repository-does-not-exist-or-may-require-docker-login"></a>"Hata: azds-\<tanımlayıcı\>\>spacename-ServiceName başarısız oldu: Hizmetler'\<HizmetAdı\<-\<\> \>' zaten var "veya" ServiceName \<\>için çekme erişimi reddedildi, depo yok veya ' Docker login' gerektirebilir "

### <a name="reason"></a>Reason
Bu hatalar, aynı geliştirme alanı içinde dev Spaces komutlarıyla `helm install`( `azds up` ve `azds down`gibi) çalışan doğrudan `helm upgrade`Held `helm delete`komutlarının (örneğin, veya) karıştıradıysanız meydana gelebilir. Bu durum, dev Spaces 'ın aynı geliştirme alanında çalışan kendi Tiller örneğinizle çakışan kendi Tiller örneğine sahip olduğu için oluşur.

### <a name="try"></a>Deneyin:
Aynı AKS kümesinde hem HELI komutları hem de dev Spaces komutlarının kullanılması ince, ancak her dev Spaces özellikli ad alanı birini ya da diğerini kullanmalıdır.

Örneğin, tüm uygulamanızı bir üst geliştirme alanında çalıştırmak için Held komutunu kullandığınızı varsayalım. Bu üst öğeden alt dev alanları oluşturabilir, her bir hizmeti alt geliştirme alanında çalıştırmak için geliştirme alanlarını kullanabilir ve Hizmetleri birlikte test edebilirsiniz. Değişikliklerinizi iade etmeye hazırsanız, güncelleştirilmiş kodu üst geliştirme alanına dağıtmak için Held komutunu kullanın. Ana geliştirme `azds up` alanında güncelleştirilmiş hizmeti çalıştırmak için kullanmayın, çünkü başlangıçta Held kullanılarak çalıştırılan hizmetle çakışacaktır.

## <a name="azure-dev-spaces-proxy-can-interfere-with-other-pods-running-in-a-dev-space"></a>Bir geliştirme alanında çalışan diğer pod'ları ile Azure geliştirme alanları proxy etkileyebilir

### <a name="reason"></a>Neden
AKS kümenizde bir ad alanı üzerinde geliştirme alanları etkinleştirdiğinizde, ek bir kapsayıcı olarak adlandırılan _mindaro proxy_ her biri, ad alanı içinde çalışan pod'ların yüklenir. Bu kapsayıcı Pod 'daki hizmetlere yapılan çağrıları karşılar ve bu, dev Spaces 'ın takım geliştirme özelliklerine integral olur. Ancak, bu FID 'ler üzerinde çalışan belirli hizmetlerden herhangi bir engel olabilir. Redde için Azure önbelleği çalıştıran bir pod 'nin, birincil/ikincil iletişimde bağlantı hatalarına ve hatalara yol açmasından dolayı, bu bir hata oluşması bilinmektedir.

### <a name="try"></a>Deneyin:
Etkilenen Pod 'yi, geliştirme alanları etkin olmayan küme içindeki bir ad alanına taşıyabilirsiniz . Uygulamanızın geri kalanı, dev Spaces özellikli bir ad alanı içinde çalışmaya devam edebilir. Geliştirme alanları yüklemez _mindaro proxy_ kapsayıcı geliştirme olmayan alanları içinde etkin ad alanları.

## <a name="azure-dev-spaces-doesnt-seem-to-use-my-existing-dockerfile-to-build-a-container"></a>Azure geliştirme alanları bir kapsayıcı oluşturmak için var olan my Dockerfile kullanmak gibi görünüyor

### <a name="reason"></a>Neden
Azure geliştirme alanları, belirli bir işaret edecek şekilde yapılandırılabilir _Dockerfile_ projenizdeki. Kapsayıcınız derlemek istediğiniz _dockerfile_ ' ı Azure dev Spaces görünürse, hangi dockerfile 'ın kullanılacağını açıkça Azure dev Spaces söylemeniz gerekebilir. 

### <a name="try"></a>Deneyin:
Azure Dev Spaces, projenizde oluşturulan _azds. YAML_ dosyasını açın. Yapılandırma- *> Geliştirme-> derleme-> dockerfile* yönergesini kullanarak kullanmak Istediğiniz dockerfile dosyasını işaret edin:

```
...
configurations:
  develop:
    build:
      dockerfile: Dockerfile.develop
```

## <a name="error-internal-watch-failed-watch-enospc-when-attaching-debugging-to-a-nodejs-application"></a>Bir Node. js uygulamasına hata ayıklama eklenirken "Iç izleme başarısız oldu: {ENOSPC" izleme

### <a name="reason"></a>Reason

Bir hata ayıklayıcıyla eklemeye çalıştığınız Node. js uygulamasıyla Pod 'ı çalıştıran düğüm, *FS. Inotify. Max _user_watch* değerini aştı. Bazı durumlarda, [varsayılan *FS. inotify. Max _user_watch* değeri bir hata ayıklayıcıyı doğrudan Pod 'a eklemeyi işlemek için çok küçük olabilir](https://github.com/Azure/AKS/issues/772).

### <a name="try"></a>Deneme
Bu soruna yönelik geçici bir geçici çözüm olarak, kümedeki her düğümde *FS. inotify. Max _user_watch* değerlerini artırmanız ve değişikliklerin etkili olması için bu düğümü yeniden başlatmanız.

## <a name="new-pods-are-not-starting"></a>Yeni Pod 'ler başlamıyor

### <a name="reason"></a>Reason

Kubernetes başlatıcısı, kümedeki *Küme Yöneticisi* rolünde RBAC izin değişiklikleri nedeniyle yeni pod için pod belirtimini uygulayamaz. Yeni Pod 'ın geçersiz bir pod özelliği olabilir, örneğin Pod ile ilişkili hizmet hesabı artık yok. Başlatıcı sorunu nedeniyle *bekleyen* bir durumda olan Pod 'leri görmek için şu `kubectl get pods` komutu kullanın:

```bash
kubectl get pods --all-namespaces --include-uninitialized
```

Bu sorun, Azure Dev Spaces etkinleştirilmemiş olan ad alanları da dahil olmak üzere kümedeki *tüm ad alanlarında* yer işareti etkileyebilir.

### <a name="try"></a>Deneme

[Dev Spaces CLI 'yı en son sürüme güncelleştirme](./how-to/upgrade-tools.md#update-the-dev-spaces-cli-extension-and-command-line-tools) ve ardından Azure dev Spaces denetleyicisinden *azds ınitializeryapılandırmasını* silme:

```bash
az aks get-credentials --resource-group <resource group name> --name <cluster name>
kubectl delete InitializerConfiguration azds
```

*Azds ınitializerconfiguration* 'ı Azure dev Spaces denetleyicisinden kaldırdıktan sonra, `kubectl delete` *bekleyen* bir durumdaki tüm yığınları kaldırmak için kullanın. Tüm bekleyen Pod 'ler kaldırıldıktan sonra, yığınlarınızı yeniden dağıtın.

Yeni yığınların yeniden dağıtım sonrasında *bekleyen* bir durumda takılmasına devam ediyorsa, *bekleyen* bir `kubectl delete` durumdaki tüm yığınları kaldırmak için kullanın. Tüm bekleyen Pod 'ler kaldırıldıktan sonra, denetleyiciyi kümeden silin ve yeniden yükleyin:

```bash
azds remove -g <resource group name> -n <cluster name>
azds controller create --name <cluster name> -g <resource group name> -tn <cluster name>
```

Denetleyicinizin yeniden yüklenmesi sırasında, yığınlarınızı yeniden dağıtın.

## <a name="incorrect-rbac-permissions-for-calling-dev-spaces-controller-and-apis"></a>Dev Spaces denetleyicisi ve API 'Leri çağırmak için yanlış RBAC izinleri

### <a name="reason"></a>Reason
Azure Dev Spaces denetleyicisine erişen kullanıcının AKS kümesinde admin *kubeconfig* 'i okumak için erişimi olmalıdır. Örneğin, bu izin [yerleşik Azure Kubernetes hizmet kümesi yönetici rolünde](../aks/control-kubeconfig-access.md#available-cluster-roles-permissions)bulunur. Azure Dev Spaces denetleyicisine erişen kullanıcının denetleyici için *katkıda* bulunan veya *sahip* RBAC rolüne de sahip olması gerekir.

### <a name="try"></a>Deneme
Bir kullanıcının bir AKS kümesi için izinlerini güncelleştirme hakkında daha fazla ayrıntıya [buradan](../aks/control-kubeconfig-access.md#assign-role-permissions-to-a-user-or-group)ulaşabilirsiniz.

Denetleyicinin Kullanıcı RBAC rolünü güncelleştirmek için:

1. [https://portal.azure.com](https://portal.azure.com ) adresinden Azure portalında oturum açın.
1. Genellikle AKS kümeniz ile aynı olan denetleyiciyi içeren kaynak grubuna gidin.
1. *Gizli türleri göster* onay kutusunu etkinleştirin.
1. Denetleyiciye tıklayın.
1. *Access Control (IAM)* bölmesini açın.
1. *Rol atamaları* sekmesine tıklayın.
1. *Ekle* ' ye tıklayın ve *rol ataması ekleyin*.
    * *Rol* Için *katkıda bulunan* veya *sahip*seçeneklerinden birini belirleyin.
    * *Azure AD Kullanıcı, Grup veya hizmet sorumlusu*seçmek *için erişim atama* için.
    * İzin vermek istediğiniz kullanıcı için arama ' yı *seçin* .
1. *Kaydet*’e tıklayın.

## <a name="controller-create-failing-due-to-controller-name-length"></a>Denetleyici adı uzunluğu nedeniyle denetleyici oluşturma başarısız

### <a name="reason"></a>Reason
Azure Dev Spaces denetleyicisinin adı 31 karakterden uzun olamaz. Bir AKS kümesinde dev alanlarını etkinleştirdiğinizde veya bir denetleyici oluşturduğunuzda denetleyicinin adı 31 karakteri aşarsa şu şekilde bir hata alırsınız:

*' A-Controller-Name-------------------Doğu-US ' kümesi için bir dev Spaces denetleyicisi oluşturulamadı: Azure Dev Spaces denetleyicisi adı ' a-Controller-Name-------------------Doğu-US ' geçersizdir. Kısıtlamalar ihlal edildi: Azure Dev Spaces denetleyicisi adları yalnızca en fazla 31 karakter uzunluğunda olabilir*

### <a name="try"></a>Deneme

Alternatif ada sahip bir denetleyici oluşturun:

```cmd
azds controller create --name my-controller --target-name MyAKS --resource-group MyResourceGroup
```

## <a name="enabling-dev-spaces-failing-when-windows-node-pools-are-added-to-an-aks-cluster"></a>Bir AKS kümesine Windows düğüm havuzları eklendiğinde geliştirme alanlarını etkinleştirme başarısız oluyor

### <a name="reason"></a>Reason
Şu anda, Azure Dev Spaces yalnızca Linux düğüm ve düğümleri üzerinde çalışmak üzere tasarlanmıştır. Windows düğüm havuzu içeren bir aks kümeniz varsa, Azure dev Spaces Pod 'nin yalnızca Linux düğümlerinde zamanlandığından emin olmanız gerekir. Bir Azure Dev Spaces Pod bir Windows düğümünde çalışmak üzere zamanlanırsa, Pod başlatılmaz ve dev alanlarının etkinleştirilmesi başarısız olur.

### <a name="try"></a>Deneme
Linux 'lerin bir Windows düğümünde çalıştırılmak üzere zamanlanmadığından emin olmak için AKS kümenize [bir Taint ekleyin](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations) .

## <a name="error-found-no-untainted-linux-nodes-in-ready-state-on-the-cluster-there-needs-to-be-at-least-one-untainted-linux-node-in-ready-state-to-deploy-pods-in-azds-namespace"></a>Hata "kümede ayırma durumunda olmayan Linux düğümü bulunamadı. ' AZD ' ad alanında Pod 'leri dağıtmak için, uygun durumda en az bir adet bir Linux düğümü olması gerekir. "

### <a name="reason"></a>Reason

Azure dev Spaces, aks kümenizde bir denetleyici oluşturamadı, çünkü Pod 'yi zamanlamak için serbest bir durumda olmayan bir düğüm bulunamıyor. Azure Dev Spaces, kullanım sürelerinin toleranlarını belirtmeden zamanlamaya izin veren, *Ready* durumunda en az bir Linux düğümü gerektirir.

### <a name="try"></a>Deneme
En az bir Linux düğümünün tolerans belirtmeden zamanlamalara izin verdiğinden emin olmak için AKS kümenizde [taınt yapılandırmanızı güncelleştirin](../aks/operator-best-practices-advanced-scheduler.md#provide-dedicated-nodes-using-taints-and-tolerations) . Ayrıca, tolerans belirtmeden düğüm zamanlamaya izin veren en az bir Linux düğümünün, *Ready* durumunda olduğundan emin olun. Düğümünüz, *hazırlık* durumuna ulaşmak uzun sürüyorsa, düğümünüz yeniden başlatmayı deneyebilirsiniz.

## <a name="error-azure-dev-spaces-cli-not-installed-properly-when-running-az-aks-use-dev-spaces"></a>Çalıştırılırken "Azure Dev Spaces CLı düzgün yüklenmemiş" hatası`az aks use-dev-spaces`

### <a name="reason"></a>Reason
CLı Azure Dev Spaces güncelleştirme, yükleme yolunu değiştirdi. Azure CLı 'nın 2.0.63 'den önceki bir sürümünü kullanıyorsanız, bu hatayı görebilirsiniz. Azure CLı sürümünüzü göstermek için kullanın `az --version`.

```bash
$ az --version
azure-cli                         2.0.60 *
...
```

2\.0.63 öncesinde bir Azure CLI sürümü `az aks use-dev-spaces` ile çalışırken hata iletisine rağmen yükleme başarılı olur. Herhangi bir sorun olmadan kullanmaya `azds` devam edebilirsiniz.

### <a name="try"></a>Deneme
[Azure CLI](/cli/azure/install-azure-cli?view=azure-cli-latest) yüklemenizi 2.0.63 veya sonraki bir sürüme güncelleştirin. Bu işlem, çalışırken `az aks use-dev-spaces`aldığınız hata iletisini çözer. Alternatif olarak, geçerli Azure CLı sürümünüzü ve Azure Dev Spaces CLı 'yi kullanmaya devam edebilirsiniz.


## <a name="horizontal-pod-autoscaling-not-working-in-a-dev-space"></a>Yatay Pod otomatik ölçeklendirme bir geliştirme alanında çalışmıyor

### <a name="reason"></a>Reason

Bir hizmeti bir geliştirme alanında çalıştırdığınızda, bu hizmetin Pod 'ı, [izleme için ek kapsayıcılarla birlikte](how-dev-spaces-works.md#prepare-your-aks-cluster) eklenir ve pod içindeki tüm kapsayıcılar, yatay Pod otomatik ölçeklendirme için kaynak limitlerinin ve isteklerin ayarlanmış olması gerekir. 


Pod belirtilerinize `azds.io/proxy-resources` ek açıklama eklenerek, eklenen kapsayıcı (devspaces-proxy) için kaynak istekleri ve sınırları uygulanabilir. Değer, proxy için kapsayıcı belirtiminin kaynaklar bölümünü temsil eden bir JSON nesnesi olarak ayarlanmalıdır.

### <a name="try"></a>Deneme

Pod belirtimde uygulanacak bir proxy kaynakları ek açıklamasına örnek aşağıda verilmiştir.
```
azds.io/proxy-resources: "{\"Limits\": {\"cpu\": \"300m\",\"memory\": \"400Mi\"},\"Requests\": {\"cpu\": \"150m\",\"memory\": \"200Mi\"}}"
```