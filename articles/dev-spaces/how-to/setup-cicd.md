---
title: Azure Dev Spaces ile CI/CD kullanma
services: azure-dev-spaces
author: DrEsteban
ms.author: stevenry
ms.date: 12/17/2018
ms.topic: conceptual
manager: gwallace
description: Azure DevOps'leri Azure Geliştirme Alanları ile kullanarak sürekli tümleştirme/sürekli dağıtım ayarlamayı öğrenin
keywords: Docker, Kubernetes, Azure, AKS, Azure Konteyner Hizmeti, konteynerler
ms.openlocfilehash: 66ff2080ad44098757a5d9360fd3307e65f7431a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75438456"
---
# <a name="use-cicd-with-azure-dev-spaces"></a>Azure Dev Spaces ile CI/CD kullanma

Bu makale, Dev Spaces etkinleştirilmiş Azure Kubernetes Hizmetine (AKS) sürekli tümleştirme/sürekli dağıtım (CI/CD) ayarlama konusunda size yol göstermektedir. AKS'ye CI/CD, işlenen kod kaynak deponuza itildiğinde uygulama güncellemelerinin otomatik olarak dağıtılmasını sağlar. Geliştirme Spaces etkin küme ile birlikte CI/CD'yi kullanmak yararlıdır, çünkü takımın çalışması için uygulamanın taban çizgisini güncel tutabilir.

![Örnek CI/CD diyagramı](../media/common/ci-cd-simple.png)

Bu makale size Azure DevOps ile rehberlik etse de, aynı kavramlar Jenkins, TeamCity, vb. gibi CI/CD sistemleri için de geçerli olacaktır.

## <a name="prerequisites"></a>Ön koşullar
* [Azure Geliştirme Alanları etkinleştirilmiş Azure Kubernetes Hizmeti (AKS) kümesi](../get-started-netcore.md)
* [Azure Dev Spaces CLI yüklü](upgrade-tools.md)
* [Proje ile Azure DevOps organizasyonu](https://docs.microsoft.com/azure/devops/user-guide/sign-up-invite-teammates?view=vsts)
* [Azure Kapsayıcı Kayıt Defteri (ACR)](../../container-registry/container-registry-get-started-azure-cli.md)
    * Azure Kapsayıcı Kayıt Defteri [yöneticisi hesap](../../container-registry/container-registry-authentication.md#admin-account) ayrıntıları kullanılabilir
* [AKS kümenizi Azure Kapsayıcı Kayıt Defterinizden çekmeleri için yetkilendirme](../../aks/cluster-container-registry-integration.md)

## <a name="download-sample-code"></a>Örnek kodu indirin
Zaman aşkına, bizim örnek kod GitHub deposunun bir çatal oluşturalım. Git https://github.com/Azure/dev-spaces ve **Çatal'ı**seç. Çatal işlemi tamamlandıktan sonra, deponun çatallı sürümünü yerel olarak **klonlayın.** Varsayılan olarak _ana_ şube kullanıma alınacaktır, ancak _çatalınız_ sırasında da aktarılması gereken azds_updates dalına zaman kazandıran bazı değişiklikler ekledik. _azds_updates_ şubesi, Dev Spaces öğretici bölümlerinde el ile yapmanızı istediğimiz güncelleştirmelerin yanı sıra CI/CD sisteminin dağıtımını kolaylaştırmak için önceden yapılmış bazı YAML ve JSON dosyalarını içerir. Yerel deponuzdaki `git checkout -b azds_updates origin/azds_updates` _azds_updates_ şubesine göz atın gibi bir komut kullanabilirsiniz.

## <a name="dev-spaces-setup"></a>Dev Spaces kurulumu
Komutu `azds space select` kullanarak _dev_ adlı yeni bir alan oluşturun. _Dev_ alanı, kod değişikliklerinizi zorlamak için CI/CD ardınız tarafından kullanılır. Ayrıca _dev_dayalı _çocuk alanları_ oluşturmak için kullanılacaktır.

```cmd
azds space select -n dev
```

Bir üst dev alanı seçmek istendiğinde, _ \<\>hiçbiri'ni_seçin.

Dev alanınız oluşturulduktan sonra, ana bilgisayar sonekini belirlemeniz gerekir. Azure `azds show-context` Dev Spaces Giriş Denetleyicisinin ana bilgisayar sonekini göstermek için komutu kullanın.

```cmd
$ azds show-context
Name   ResourceGroup    DevSpace  HostSuffix
-----  ---------------  --------  ----------------------
MyAKS  MyResourceGroup  dev       fedcba098.eus.azds.io
```

Yukarıdaki örnekte, ana bilgisayar soneki _fedcba098.eus.azds.io._ Bu değer daha sonra sürüm tanımınızı oluştururken kullanılır.

_Geliştirme_ alanı her zaman deponun en son durumunu, bir temeli içerir, böylece geliştiriciler daha büyük uygulama bağlamında yalıtılmış değişikliklerini sınamak için _dev'den_ _alt alan_ oluşturabilirler. Bu kavram Dev Spaces öğreticiler daha ayrıntılı olarak ele alınmıştır.

## <a name="creating-the-build-definition"></a>Yapı tanımıoluşturma
Azure DevOps takım projenizi bir web tarayıcısında açın ve _Pipelines_ bölümüne gidin. İlk olarak, Azure DevOps sitesinin sağ üst kısmındaki profil fotoğrafınızı tıklayın, önizleme özellikleri bölmesini açın ve _Yeni YAML boru hattı oluşturma deneyimini_devre dışı bırakmak:

![Açılış önizleme özellikleri bölmesi](../media/common/preview-feature-open.png)

Devre dışı etme seçeneği:

![Yeni YAML boru hattı oluşturma deneyimi seçeneği](../media/common/yaml-pipeline-preview-feature.png)

> [!Note]
> Azure DevOps _Yeni YAML ardışık hat geliştirme deneyimi_ önizleme özelliği, şu anda önceden tanımlanmış yapı ardışık hatları oluşturmayla çakışıyor. Önceden tanımlanmış yapı boru hattımızı dağıtmak için şimdilik devre dışı balmanız gerekir.

_azds_updates_ *şubesinde, mywebapi* ve *webfrontend*için gerekli yapı adımlarını tanımlayan basit bir [Azure Pipeline YAML'yi](https://docs.microsoft.com/azure/devops/pipelines/yaml-schema?view=vsts&tabs=schema) dahil ettik.

Seçtiğiniz dile bağlı olarak, boru hattı YAML aşağıdakine benzer bir yolda iade edildi:`samples/dotnetcore/getting-started/azure-pipelines.dotnetcore.yml`

Bu dosyadan bir Ardışık Hatlar oluşturmak için:
1. DevOps proje ana sayfanızda, Yapı > Boru Hatları'na gidin.
1. **Yeni** yapı ardışık bir yapı oluşturma seçeneğini seçin.
1. Kaynak olarak **GitHub'ı** seçin, gerekirse GitHub hesabınızla yetki verin ve _dev-spaces_ örnek uygulama deposunun çatallı sürümünden _azds_updates_ şubesini seçin.
1. **Şablonolarak Yapılandırma'yı kod olarak**veya **YAML**olarak seçin.
1. Şimdi yapı ardışık hattınız için bir yapılandırma sayfası sunulur. Yukarıda belirtildiği gibi **...** düğmesini kullanarak **YAML dosya yolu** için dile özgü yola gidin. Örneğin, `samples/dotnetcore/getting-started/azure-pipelines.dotnet.yml`.
1. **Değişkenler** sekmesine gidin.
1. [Azure Kapsayıcı Kayıt Defteri yönetici hesabınızın](../../container-registry/container-registry-authentication.md#admin-account)kullanıcı adı olan _dockerId'i_ değişken olarak el ile ekleyin. (Makalede bahsedilen ön koşullar)
1. [Azure Konteyner Kayıt Defteri yönetici hesabınızın](../../container-registry/container-registry-authentication.md#admin-account)şifresi olan _dockerPassword'i_ değişken olarak el ile ekleyin. Güvenlik amacıyla _dockerPassword'i_ Gizli olarak (kilit simgesini seçerek) belirttiğinden emin olun.
1. **& sırayı kaydet'i**seçin.

Şimdi otomatik olarak github çatal azds_updates şubesine itti herhangi bir güncelleme için *mywebapi* ve *webfrontend* inşa edecek _bir_ CI çözüm var. Azure portalına giderek, Azure Konteyner Kayıt Defterinizi seçerek ve **Depolar** sekmesine göz atarak Docker görüntülerinin itildiğini doğrulayabilirsiniz. Görüntülerin oluşturulması ve konteyner kayıt defterinizde görünmesi birkaç dakika sürebilir.

![Azure Kapsayıcı Kayıt Defteri depoları](../media/common/ci-cd-images-verify.png)

## <a name="creating-the-release-definition"></a>Sürüm tanımını oluşturma

1. DevOps proje ana sayfanızda, Pipelines > Releases sayfasına gidin
1. Henüz sürüm tanımı içermeyen yepyeni bir DevOps Projesi'nde çalışıyorsanız, devam etmeden önce boş bir sürüm tanımı oluşturmanız gerekir. Varolan bir sürüm tanımı olana kadar İçe Aktarma seçeneği UI'de görüntüalmaz.
1. Solda + **Yeni** düğmesini tıklatın ve ardından **bir boru hattı nı al'ı**tıklatın.
1. **Gözat'ı** `samples/release.json` tıklatın ve projenizden seçim inizi seçin.
1. **Tamam**'a tıklayın. Pipeline bölmesinin sürüm tanımı nı edit sayfasıyla yüklendiğine dikkat edin. Ayrıca, kümeye özgü ayrıntıları gösteren ve hala yapılandırılması gereken bazı kırmızı uyarı simgeleri olduğuna da dikkat edin.
1. Pipeline bölmesinin solunda, **yapı tonu ekle'yi** tıklatın.
1. **Kaynak** açılır düşüşünde, daha önce oluşturduğunuz yapı ardışık hattını seçin.
1. Varsayılan **sürüm**için, **etiketleri olan yapı ardışık etki alanı varsayılan dalından En Son'u**seçin.
1. **Etiketleri** boş bırakın.
1. Kaynak **takma adını** `drop`' a ayarlayın. **Kaynak diğer ad** değeri önceden tanımlanmış sürüm görevleri tarafından kullanılır, bu nedenle ayarlanmalıdır.
1. **Ekle**’ye tıklayın.
1. Şimdi, aşağıda gösterildiği gibi, `drop` yeni oluşturulan yapı kaynağındaki yıldırım simgesine tıklayın:

    ![Açıklama artefaktı sürekli dağıtım kurulumu](../media/common/release-artifact-cd-setup.png)
1. Sürekli **dağıtım tetikleyicisini**etkinleştirin.
1. **Pipeline'ın** yanındaki **Görevler** sekmesinin üzerine tıklayın ve _geliştirme_ aşaması görevlerini düzenlemek için _dev'i_ tıklatın.
1. Bağlantı Türü altında **Azure Kaynak Yöneticisi'nin** seçildiğini **doğrulayın.** ve kırmızı ile vurgulanan üç açılır ![denetimi görürsünüz: Sürüm tanımı kurulumu](../media/common/release-setup-tasks.png)
1. Azure Dev Spaces ile kullandığınız Azure aboneliğini seçin. **Ayrıca Authorize'u**tıklatmanız gerekebilir.
1. Azure Dev Spaces ile kullandığınız kaynak grubunu ve kümeyi seçin.
1. Agent **iş**tıklayın.
1. **Agent havuzu**altında **Barındırılan Ubuntu 1604'ü** seçin.
1. En üstteki **Görevler** seçicinin üzerine oturun, prod sahne görevlerini düzenlemek için _prod'a_ tıklayın. _prod_
1. Bağlantı Türü altında **Azure Kaynak Yöneticisi'nin** seçildiğini **doğrulayın.** azure dev alanları yla kullandığınız Azure aboneliğini, kaynak grubunu ve kümeyi seçin.
1. Agent **iş**tıklayın.
1. **Agent havuzu**altında **Barındırılan Ubuntu 1604'ü** seçin.
1. Sürümünüzün değişkenlerini güncelleştirmek için **Değişkenler** sekmesini tıklatın.
1. **DevSpacesHostSonefix** değerini **UPDATE_ME'dan** ana bilgisayar sonekinize güncelleştirin. `azds show-context` Komutu daha önce çalıştırdığınızda ana bilgisayar soneki görüntülenir.
1. Sağ üstte **Kaydet'i** tıklatın ve **Tamam'** ı tıklatın.
1. **+ Release** (Kaydet düğmesinin yanında) ve **sürüm oluştur'u**tıklatın.
1. **Artefaktlar**altında, yapı ardışık etki alanınızdaki en son yapının seçildiğini doğrulayın.
1. **Oluştur'u**tıklatın.

*Miwebapi* ve *webfrontend* grafiklerini _dev_ üst düzey alanda Kubernetes kümenize dağıtarak otomatik bir sürüm süreci başlayacaktır. Azure DevOps web portalında yayınlanmanızın ilerlemesini izleyebilirsiniz:

1. **Ardışık Hatlar**altında **Sürümler** bölümüne gidin.
1. Örnek uygulama için sürüm ardışık hattını tıklatın.
1. En son sürümün adını tıklayın.
1. **Aşamalar** altında **dev** kutusunun üzerine tover ve **Günlükleri**tıklatın.

Sürüm, tüm görevler tamamlandığında yapılır.

> [!TIP]
> Sürümyükseltme FAILED gibi bir hata iletisi ile başarısız *olursa: koşul bekleyen zamanlanmış,* [Kubernetes panosu kullanarak](../../aks/kubernetes-dashboard.md)kümebölmeleri inceleyerek deneyin. Bölmelerin görüntü "azdsexample.azurecr.io/mywebapi:122" çekmede başarısız olması gibi hata iletileriyle başlayamadığını *görürseniz: rpc hatası: kod = Bilinmeyen https://azdsexample.azurecr.io/v2/mywebapi/manifests/122: desc = Daemon'dan hata yanıtı: Yetkisiz alın: kimlik doğrulama gerekli*, bunun nedeni kümenizin Azure Kapsayıcı Kayıt Defterinizden çekme yetkisine sahip olmaması olabilir. Azure Kapsayıcı Kayıt Defteri ön [koşulunuzdan yararlanmak için AKS kümenizi yetkilendir'i tamamladığınızdan](../../aks/cluster-container-registry-integration.md) emin olun.

Artık GitHub çatalını dev spaces örnek uygulamalarınız için tam otomatik bir CI/CD ardışık hattınız var. Her zaman işlemek ve kod itin, yapı boru hattı inşa edecek ve özel ACR örneğine *mywebapi* ve *webfrontend* görüntüleri itin. Ardından sürüm ardışık hattı, her uygulama için Miğfer grafiğini Dev Spaces etkin kümenizdeki _dev_ alana dağıtır.

## <a name="accessing-your-_dev_-services"></a>_Geliştirme_ hizmetlerinize erişim
Dağıtımdan sonra, *webfrontend'in* _dev_ sürümüne genel bir `http://dev.webfrontend.fedcba098.eus.azds.io`URL ile erişilebilir: . Bu URL'yi komutu `azds list-uri` çalıştırarak bulabilirsiniz: 

```cmd
$ azds list-uris

Uri                                           Status
--------------------------------------------  ---------
http://dev.webfrontend.fedcba098.eus.azds.io  Available
```

## <a name="deploying-to-production"></a>Üretime Dağıtma

Bu öğreticide oluşturulan CI/CD sistemini kullanarak belirli bir sürümü el ile _prod'a_ tanıtmak için:
1. **Ardışık Hatlar**altında **Sürümler** bölümüne gidin.
1. Örnek uygulama için sürüm ardışık hattını tıklatın.
1. En son sürümün adını tıklayın.
1. Aşamalar altındaki **prod** kutusunun üzerine **titreyin** ve **Dağıt'ı**tıklatın.
    ![Üretime yükseltme](../media/common/prod-promote.png)
1. **Aşamalar** altında tekrar **prod** kutusunun üzerine hover ve **Günlükleri**tıklatın.

Sürüm, tüm görevler tamamlandığında yapılır.

CI/CD ardışık boru hattının _prod_ hizmetlerine erişim sağlamak için Dev Spaces _prod_ Ingress denetleyicisi yerine bir yük dengeleyicisi kullanır. _Prod_ aşamasında dağıtılan hizmetlere DNS adları yerine IP adresleri olarak erişilebilir. Üretim ortamında, hizmetlerinizi kendi DNS yapılandırmanıza göre barındırmak için kendi Giriş denetleyicinizi oluşturmayı seçebilirsiniz.

Webfrontend hizmetinin IP'sini belirlemek için, günlük çıktısını genişletmek için **Print webfrontend public IP** adımını tıklatın. **Webfrontend** uygulamasına erişmek için günlük çıkışında görüntülenen IP'yi kullanın.

```cmd
...
2019-02-25T22:53:02.3237187Z webfrontend can be accessed at http://52.170.231.44
2019-02-25T22:53:02.3320366Z ##[section]Finishing: Print webfrontend public IP
...
```

## <a name="dev-spaces-instrumentation-in-production"></a>Üretimde Dev Spaces enstrümantasyonu
Dev Spaces enstrümantasyonu uygulamanızın normal çalışmasına engel _olmayacak_ şekilde tasarlanmış olsa da, üretim iş yüklerinizi Dev Spaces ile etkinleştirilen bir Kubernetes ad alanında çalıştırmanızı öneririz. Bu tür Kubernetes ad alanını kullanmak, `kubectl` CLI'yi kullanarak üretim ad alanınızı oluşturmanız veya CI/CD sisteminizin ilk Miğfer dağıtımı sırasında oluşturmasına izin vermeniz gerektiği anlamına gelir. Dev Spaces aracını kullanarak bir alan _seçmek_ veya başka bir şekilde oluşturmak, bu ad alanına Dev Spaces enstrümantasyonunu ekler.

Burada özellik geliştirme, 'dev' ortamı _ve_ üretim, tek bir Kubernetes küme tüm destekleyen bir örnek ad alanı yapısı:

![Örnek ad alanı yapısı](../media/common/cicd-namespaces.png)

> [!Tip]
> Zaten bir `prod` boşluk oluşturduysanız ve yalnızca Onu Dev Spaces enstrümantasyonundan hariç tutmak istiyorsanız (silmeden!), bunu aşağıdaki Dev Spaces CLI komutuyla yapabilirsiniz:
>
> `azds space remove -n prod --no-delete`
>
> Bunu yaptıktan sonra `prod` ad alanındaki tüm bölmeleri silmeniz gerekebilir, böylece Dev Spaces enstrümantasyonu olmadan yeniden oluşturulabilirler.

## <a name="next-steps"></a>Sonraki adımlar

> [!div class="nextstepaction"]
> [Azure Geliştirme Alanları'nı kullanarak ekip geliştirme hakkında bilgi edinin](../team-development-netcore.md)