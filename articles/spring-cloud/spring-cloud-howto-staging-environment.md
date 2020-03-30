---
title: Azure Bahar Bulutu'nda bir hazırlama ortamı ayarlama | Microsoft Dokümanlar
description: Azure Bahar Bulutu ile mavi-yeşil dağıtımı nasıl kullanacağınızı öğrenin
author: bmitchell287
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 02/03/2020
ms.author: brendm
ms.openlocfilehash: 2e29f6a75b303518ac34ecf9b570bd7638cf0c3a
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79471039"
---
# <a name="set-up-a-staging-environment-in-azure-spring-cloud"></a>Azure Bahar Bulutu'nda bir hazırlama ortamı ayarlama

Bu makalede, Azure Bahar Bulutu'ndaki mavi-yeşil dağıtım deseni kullanılarak bir hazırlama dağıtımının nasıl ayarlanılacak anlatılmaktadır. Mavi/yeşil dağıtım, yeni (yeşil) sürüm dağıtıldığı sırada mevcut (mavi) sürümü canlı tutma prensibine dayanan bir Azure DevOps Sürekli Teslim düzenidir. Bu makalede, üretim dağıtımını doğrudan değiştirmeden bu hazırlama dağıtımını üretime nasıl koyacağınız gösterilmektedir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, [Bir Azure Bahar Bulutu uygulamasını başlatma yla ilgili öğreticimizden](spring-cloud-quickstart-launch-app-portal.md)PiggyMetrics uygulamasını zaten dağıtmış olduğunuz varsayar. PiggyMetrics üç uygulamadan oluşur: "ağ geçidi", "hesap hizmeti" ve "auth-service."  

Bu örnek için farklı bir uygulama kullanmak istiyorsanız, uygulamanın genel kullanıma açık bir bölümünde basit bir değişiklik yapmanız gerekir.  Bu değişiklik, evreleme dağıtımınızı üretimden ayırır.

>[!TIP]
> Azure Bulut Kabuğu, bu makaledeki yönergeleri çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLI'nin en son sürümleri de dahil olmak üzere yaygın, önceden yüklenmiş Azure araçlarına sahiptir. Azure aboneliğinizde oturum açtıysanız, Azure [Bulut Shell'inizi](https://shell.azure.com)başlatın.  Daha fazla bilgi için Azure [Bulut BulutU Bulutu'na Genel Bakış](../cloud-shell/overview.md)bölümüne bakın.

Azure Bahar Bulutu'nda bir hazırlama ortamı ayarlamak için sonraki bölümlerdeki yönergeleri izleyin.

## <a name="install-the-azure-cli-extension"></a>Azure CLI uzantısını yükleme

Aşağıdaki komutu kullanarak Azure CLI için Azure İlkbahar Bulutu uzantısını yükleyin:

```azurecli
az extension add --name spring-cloud
```
    
## <a name="view-all-deployments"></a>Tüm dağıtımları görüntüleme

Azure portalındaki hizmet örneğinize gidin ve tüm dağıtımları görüntülemek için **Dağıtım yönetimini** seçin. Daha fazla ayrıntı görüntülemek için her dağıtımı seçebilirsiniz.

## <a name="create-a-staging-deployment"></a>Hazırlama dağıtımı oluşturma

1. Yerel geliştirme ortamınızda, PiggyMetrics ağ geçidi uygulamasında küçük bir değişiklik yapın. Örneğin, *ağ geçidi/src/main/resources/static/css/launch.css* dosyasındaki rengi değiştirin. Bunu yapmak, iki dağıtımı kolayca ayırt etmenizi sağlar. Kavanoz paketini oluşturmak için aşağıdaki komutu çalıştırın: 

    ```console
    mvn clean package
    ```

1. Azure CLI'de yeni bir dağıtım oluşturun ve hazırlama dağıtım adını "yeşil" verin.

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app gateway -n green --jar-path gateway/target/gateway.jar
    ```

1. Dağıtım başarıyla bittikten sonra, **Uygulama Panosu'ndan**ağ geçidi sayfasına erişin ve soldaki **Uygulama Örnekleri** sekmesinde tüm örneklerinizi görüntüleyin.
  
> [!NOTE]
> Doğrulama tamamlanmadan önce trafiğin bu dağıtıma yönlendirilmemesine *OUT_OF_SERVICE.*

## <a name="verify-the-staging-deployment"></a>Hazırlama dağıtımını doğrulama

1. **Dağıtım yönetimi** sayfasına dönün ve yeni dağıtımınızı seçin. Dağıtım durumu *Çalışan'ı*göstermelidir. **Atama/Atadeğiştirme etki alanı** düğmesi gri renkte görünmelidir, çünkü ortam bir hazırlama ortamıdır.

1. Genel **Bakış** bölmesinde bir **Test Bitiş Noktası**görmeniz gerekir. Yeni bir tarayıcı penceresine kopyalayıp yapıştırın ve yeni PiggyMetrics sayfası görüntülenmelidir.

>[!TIP]
> * CSS dosyanızın doğru yüklendiğinden emin olmak için test bitiş noktanızın bir eğik çizgi (/) ile sona erdiğini doğrulayın.  
> * Tarayıcınız sayfayı görüntülemek için giriş kimlik bilgilerini girmenizi gerektiriyorsa, test bitiş noktanızı çözmek için [URL şifresini çözün.](https://www.urldecoder.org/) URL decode'u "https://\<kullanıcı adı>:\<password\<>@ cluster-name>.test.azureapps.io/gateway/green" şeklinde bir URL döndürür.  Bitiş noktanıza erişmek için bu formu kullanın.

>[!NOTE]    
> Config sunucu ayarları hem hazırlama ortamınız hem de üretim için geçerlidir. Örneğin, config sunucusundaki uygulama`server.servlet.context-path`ağ geçidiniz in bağlam yolunu ( ) *somepath*olarak ayarlarsanız, yeşil dağıtımınızın yolu "https://\<kullanıcı adı>:\<password>@\<cluster-name>.test.azureapps.io/gateway/green/somepath/..." olarak değiştirebilirsiniz.
 
 Bu noktada herkese açık uygulama ağ geçidinizi ziyaret ederseniz, yeni değişikliğiniz olmadan eski sayfayı görmeniz gerekir.
    
## <a name="set-the-green-deployment-as-the-production-environment"></a>Yeşil dağıtımı üretim ortamı olarak ayarlama

1. Evreleme ortamınızdaki değişikliği doğruladıktan sonra, bunu üretime itebilirsiniz. Dağıtım **yönetimine**dönün ve **ağ geçidi** uygulaması onay kutusunu seçin.

2. **Dağıtımı Ayarla'yı**seçin.
3. Üretim **Dağıtım** listesinde **Yeşil'i**seçin ve sonra **Uygula'yı**seçin.
4. Ağ geçidi uygulamanıza **Genel Bakış** sayfanıza gidin. Ağ geçidi uygulamanız için zaten bir etki alanı atadıysanız, URL **Genel Bakış** bölmesinde görünür. Değiştirilmiş PiggyMetrics sayfasını görüntülemek için URL'yi seçin ve siteye gidin.

>[!NOTE]
> Yeşil dağıtımı üretim ortamı olarak ayarladıktan sonra, önceki dağıtım hazırlama dağıtımı olur.

## <a name="modify-the-staging-deployment"></a>Hazırlama dağıtımını değiştirme

Değişikliğinizden memnun değilseniz, Azure CLI'yi kullanarak uygulama kodunuzu değiştirebilir, yeni bir kavanoz paketi oluşturabilir ve yeşil dağıtımınıza yükleyebilirsiniz.

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-the-staging-deployment"></a>Hazırlama dağıtımını silme

Hazırlama dağıtımınızı Azure bağlantı noktasından silmek için, hazırlama dağıtım sayfanıza gidin ve ardından **Sil** düğmesini seçin.

Alternatif olarak, aşağıdaki komutu çalıştırarak hazırlama dağıtımınızı Azure CLI'sinden silin:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```
