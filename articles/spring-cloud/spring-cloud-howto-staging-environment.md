---
title: Azure yay bulutu 'nda hazırlama ortamı ayarlama | Microsoft Docs
description: Azure Spring Cloud ile mavi yeşil dağıtımı nasıl kullanacağınızı öğrenin
author: MikeDodaro
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 01/14/2021
ms.author: brendm
ms.custom: devx-track-java, devx-track-azurecli
ms.openlocfilehash: 991a335207fc29cef7b243d7e520dd5f62ff691f
ms.sourcegitcommit: 2dd0932ba9925b6d8e3be34822cc389cade21b0d
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/01/2021
ms.locfileid: "99226126"
---
# <a name="set-up-a-staging-environment-in-azure-spring-cloud"></a>Azure yay bulutu 'nda hazırlama ortamı ayarlama

**Bu makale şu şekilde geçerlidir:** ✔️ Java

Bu makalede, Azure Spring Cloud 'daki mavi yeşil dağıtım modelini kullanarak bir hazırlama dağıtımının nasıl ayarlanacağı açıklanır. Mavi/yeşil dağıtım, yeni (yeşil) sürüm dağıtıldığı sırada mevcut (mavi) sürümü canlı tutma prensibine dayanan bir Azure DevOps Sürekli Teslim düzenidir. Bu makalede, üretim dağıtımını doğrudan değiştirmeden bu hazırlama dağıtımını üretime nasıl koyabileceğiniz gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

* *Standart* **fiyatlandırma katmanıyla** Azure yay bulutu örneği.
* Çalışan bir uygulama.  Bkz. [hızlı başlangıç: Ilk Azure Spring Cloud uygulamanızı dağıtma](spring-cloud-quickstart.md).
* Azure CLı [ASC uzantısı](https://docs.microsoft.com/cli/azure/azure-cli-extensions-overview)

Bu örnek için farklı bir uygulama kullanmak istiyorsanız, uygulamanın genel kullanıma açık bir bölümünde basit bir değişiklik yapmanız gerekir.  Bu değişiklik, üretimden hazırlama dağıtımınızı farklılaştırır.

>[!TIP]
> Azure Cloud Shell, bu makaledeki yönergeleri çalıştırmak için kullanabileceğiniz, ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere, yaygın, önceden yüklenmiş Azure araçlarına sahiptir. Azure aboneliğinizde oturum açtıysanız [Azure Cloud Shell](https://shell.azure.com)başlatın.  Daha fazla bilgi edinmek için bkz. [Azure Cloud Shell Genel Bakış](../cloud-shell/overview.md).

Azure Spring Cloud 'da hazırlama ortamı ayarlamak için, sonraki bölümlerdeki yönergeleri izleyin.

## <a name="install-the-azure-cli-extension"></a>Azure CLı uzantısını yükler

Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler:

```azurecli
az extension add --name spring-cloud
```
    
## <a name="view-apps-and-deployments"></a>Uygulamaları ve dağıtımları görüntüleme

Aşağıdaki yordamları kullanarak dağıtılan uygulamaları görüntüleyin.

1. Azure portal Azure Spring Cloud örneğinize gidin.

1. Sol gezinti bölmesinden **dağıtımlar**' a açın.

    [![Dağıtım-kullanımdan kaldırıldı](media/spring-cloud-blue-green-staging/deployments.png)](media/spring-cloud-blue-green-staging/deployments.png)

1. Hizmet örneğiniz için uygulamaları görüntülemek için "uygulamalar" dikey penceresini açın.

    [![Uygulamalar-Pano](media/spring-cloud-blue-green-staging/app-dashboard.png)](media/spring-cloud-blue-green-staging/app-dashboard.png)

1. Bir uygulamaya tıklayıp ayrıntıları görüntüleyebilirsiniz.

    [![Uygulamalar-genel bakış](media/spring-cloud-blue-green-staging/app-overview.png)](media/spring-cloud-blue-green-staging/app-overview.png)

1. Uygulamanın tüm dağıtımlarını görmek için **dağıtımlar** dikey penceresini açın. Dağıtım Kılavuzu, dağıtımın üretim veya hazırlık olduğunu gösterir.

    [![Dağıtımlar panosu](media/spring-cloud-blue-green-staging/deployments-dashboard.png)](media/spring-cloud-blue-green-staging/deployments-dashboard.png)

1. Dağıtıma genel bakış ' ı görüntülemek için dağıtımın adına tıklayabilirsiniz. Bu durumda, tek dağıtım *varsayılan* olarak adlandırılır.

    [![Dağıtıma genel bakış](media/spring-cloud-blue-green-staging/deployments-overview.png)](media/spring-cloud-blue-green-staging/deployments-overview.png)
    

## <a name="create-a-staging-deployment"></a>Hazırlama dağıtımı oluşturma

1. Yerel geliştirme ortamınızda uygulamanızda küçük bir değişiklik yapın. Bunun yapılması iki dağıtımı kolayca ayırt etmenizi sağlar. Jar paketini derlemek için aşağıdaki komutu çalıştırın: 

    ```console
    mvn clean package -DskipTests
    ```

1. Azure CLı 'de yeni bir dağıtım oluşturun ve bunu hazırlama dağıtım adı "yeşil" olarak verin.

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app <appName> -n green --jar-path gateway/target/gateway.jar
    ```

1. CLı dağıtımı başarıyla tamamlandıktan sonra **uygulama panosundan** uygulama sayfasına erişin ve sol taraftaki **dağıtımlar** sekmesinde tüm örneklerinizi görüntüleyin.

   [![Yeşil dağıtıldıktan sonra dağıtımlar panosu](media/spring-cloud-blue-green-staging/deployments-dashboard-2.png)](media/spring-cloud-blue-green-staging/deployments-dashboard-2.png)

  
> [!NOTE]
> Doğrulama işlemi tamamlanmadan önce trafiğin Bu dağıtıma yönlendirilmemesi için bulma durumu *OUT_OF_SERVICE* .

## <a name="verify-the-staging-deployment"></a>Hazırlama dağıtımını doğrulama

Yeşil hazırlama geliştirmenin çalıştığını doğrulamak için:
1. **dağıtımlar** ' a gidin ve `green` **hazırlama dağıtımına** tıklayın.
1. **Genel bakış** sayfasında, **Test uç noktasına** tıklayın.
1. Bu, yaptığınız değişiklikleri gösteren hazırlama derlemesini açar.

>[!TIP]
> * CSS dosyasının doğru bir şekilde yüklendiğinden emin olmak için, test uç noktanızın eğik çizgiyle (/) bitdiğini doğrulayın.  
> * Tarayıcınızda sayfayı görüntülemek için oturum açma kimlik bilgilerini girmeniz gerekiyorsa, test uç noktanızın kodunu çözmek için [URL kodunu çöz](https://www.urldecoder.org/) kullanın. URL kod çözme "https:// \<username> : \<password> @ \<cluster-name> . test.azureapps.io/Gateway/Green" biçiminde bir URL döndürür.  Uç noktanıza erişmek için bu formu kullanın.

>[!NOTE]    
> Yapılandırma sunucusu ayarları hem hazırlama ortamınız hem de üretim için geçerlidir. Örneğin, `server.servlet.context-path` yapılandırma sunucusundaki uygulama ağ geçidiniz için bağlam yolunu () bir *somepath* olarak ayarlarsanız, yeşil dağıtımınızın yolu "https:// \<username> : \<password> @ \<cluster-name> . test.azureapps.io/Gateway/Green/somepath/..." olarak değişir.
 
 Bu noktada herkese açık uygulama ağ geçidinizi ziyaret ederseniz, eski sayfayı yeni değiştirmeksizin görmeniz gerekir.
    
## <a name="set-the-green-deployment-as-the-production-environment"></a>Yeşil dağıtımı üretim ortamı olarak ayarlama

1. Hazırlama ortamınızda yaptığınız değişikliği doğruladıktan sonra üretime gönderebilirsiniz. **Dağıtım yönetimine** dönün ve şu anda içinde olan uygulamayı seçin `Production` .

1. **Kayıt durumundan** sonra üç noktaya tıklayın ve üretim derlemesini olarak ayarlayın `staging` .

   [![Dağıtımlar hazırlama dağıtımını ayarladı](media/spring-cloud-blue-green-staging/set-staging-deployment.png)](media/spring-cloud-blue-green-staging/set-staging-deployment.png)

1. **Dağıtım yönetimi** sayfasına dönün. `green`Dağıtımını olarak ayarlayın `production` . Ayar tamamlandığında `green` dağıtım durumlarınızın gösterilmesi gerekir.  Bu artık çalışan üretim yapıdır.

   [![Dağıtımlar hazırlama dağıtım sonucunu ayarla](media/spring-cloud-blue-green-staging/set-staging-deployment-result.png)](media/spring-cloud-blue-green-staging/set-staging-deployment-result.png)

1. Uygulamanın URL 'SI değişikliklerinizi görüntülemelidir.

>[!NOTE]
> Yeşil dağıtımı üretim ortamı olarak ayarladıktan sonra, önceki dağıtım hazırlama dağıtımı olur.

## <a name="modify-the-staging-deployment"></a>Hazırlama dağıtımını değiştirme

Değişikliklerinizi tatmin ediyorsanız, uygulama kodunuzu değiştirebilir, yeni bir jar paketi oluşturabilir ve Azure CLı kullanarak yeşil dağıtımınıza yükleyebilirsiniz.

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-the-staging-deployment"></a>Hazırlama dağıtımını Sil

Hazırlama dağıtımınızı Azure bağlantı noktasından silmek için, hazırlama dağıtımı sayfanıza gidin ve **Sil** düğmesini seçin.

Alternatif olarak, aşağıdaki komutu çalıştırarak hazırlama dağıtımınızı Azure CLı 'den silebilirsiniz:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```

## <a name="next-steps"></a>Sonraki adımlar

* [Azure yay bulutu için CI/CD](https://review.docs.microsoft.com/azure/spring-cloud/spring-cloud-howto-cicd?branch=pr-en-us-142929&pivots=programming-language-java)
