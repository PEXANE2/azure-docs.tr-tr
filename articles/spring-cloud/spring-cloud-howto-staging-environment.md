---
title: Azure yay bulutu 'nda hazırlama ortamı ayarlama | Microsoft Docs
description: Azure Spring Cloud ile mavi yeşil dağıtımı nasıl kullanacağınızı öğrenin
author: jpconnock
ms.service: spring-cloud
ms.topic: conceptual
ms.date: 10/31/2019
ms.author: jeconnoc
ms.openlocfilehash: 24ce4dee04e4daf3eaee4144f8dc56de5867bbca
ms.sourcegitcommit: c62a68ed80289d0daada860b837c31625b0fa0f0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/05/2019
ms.locfileid: "73607219"
---
# <a name="how-to-set-up-a-staging-environment"></a>Hazırlık ortamını ayarlama

Bu makalede, Azure Spring Cloud 'daki mavi-yeşil dağıtım modelini kullanarak bir hazırlama dağıtımının nasıl kullanılacağı gösterilecektir. Ayrıca, bu hazırlama dağıtımını üretim dağıtımını doğrudan değiştirmeden üretime nasıl koyabileceğiniz gösterilir.

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, [bir uygulamayı başlatmak için öğreticimizden](spring-cloud-quickstart-launch-app-portal.md)zaten Pıof ölçümleri uygulamasını dağıttığınızı varsaymış olursunuz. Pıof ölçümleri üç uygulama içerir: "Gateway", "Account-Service" ve "Auth-Service".  

Bu örnek için kullanmak istediğiniz farklı bir uygulamanız varsa, uygulamanın genel kullanıma açık bölümünde basit bir değişiklik yapmanız gerekir.  Bu değişiklik, üretimden hazırlama dağıtımınızı farklılaştırır.

>[!TIP]
> Azure Cloud Shell, bu makaledeki adımları çalıştırmak için kullanabileceğiniz ücretsiz bir etkileşimli kabuktur.  Git, JDK, Maven ve Azure CLı 'nin en son sürümleri de dahil olmak üzere önceden yüklenmiş ortak Azure araçları vardır. Azure aboneliğinizde oturum açtıysanız, shell.azure.com adresinden [Azure Cloud Shell](https://shell.azure.com) başlatın.  [Belgelerimizi okuyarak](../cloud-shell/overview.md) Azure Cloud Shell hakkında daha fazla bilgi edinebilirsiniz

Bu makaleyi gerçekleştirmek için:


## <a name="install-the-azure-cli-extension"></a>Azure CLı uzantısını yükler

Aşağıdaki komutu kullanarak Azure CLı için Azure yay bulutu uzantısını yükler

```azurecli
az extension add --name spring-cloud
```
    
## <a name="view-all-deployments"></a>Tüm dağıtımları görüntüle

Tüm dağıtımları görüntülemek için Azure portal hizmet örneğinize gidin ve **dağıtım yönetimi** ' ni seçin. Daha fazla ayrıntı için her bir dağıtımı seçebilirsiniz.

## <a name="create-a-staging-deployment"></a>Hazırlama dağıtımı oluşturma

1. Yerel geliştirme ortamınızda, Piggy ölçüsünün ağ geçidi uygulamasında küçük bir değişiklik yapın. Örneğin, `gateway/src/main/resources/static/css/launch.css`rengi değiştirin. Bu, iki dağıtımı kolayca ayırt etmenize olanak sağlayacak. Jar paketini derlemek için aşağıdaki komutu çalıştırın: 

    ```azurecli
    mvn clean package
    ```

1. Azure CLı ile yeni bir dağıtım oluşturun ve bunu hazırlama dağıtım adı "yeşil" olarak verir.

    ```azurecli
    az spring-cloud app deployment create -g <resource-group-name> -s <service-instance-name> --app gateway -n green --jar-path gateway/target/gateway.jar
    ```

1. Dağıtım başarıyla tamamlandıktan sonra, **uygulama panosundan** ağ geçidi sayfasına erişin ve sol taraftaki **uygulama örnekleri** sekmesinde tüm örneklerinizi görüntüleyin.
  
> [!NOTE]
> Doğrulama işlemi tamamlanmadan önce trafiğin Bu dağıtıma yönlendirilmemesi için bulma durumu "OUT_OF_SERVICE" olur.

## <a name="verify-the-staging-deployment"></a>Hazırlama dağıtımını doğrulama

1. **dağıtım yönetim** sayfasına dönün ve yeni dağıtımınızı seçin. Dağıtım durumu **çalışıyor**olarak gösterilmelidir. "Etki alanı ata/atamayı kaldır" düğmesi, hazırlama ortamı olduğundan devre dışı bırakılır.

1. **Genel bakış** sayfasında, bir **sınama uç noktası**görmeniz gerekir. Kopyalayıp yeni bir tarayıcı sayfasına yapıştırın ve yeni Piggy ölçümleri sayfasını görmeniz gerekir.

>[!TIP]
> * CSS 'nin doğru şekilde yüklendiğinden emin olmak için test uç noktanızın "/" ile bitdiğini doğrulayın.  
> * Tarayıcınızda sayfayı görüntülemek için oturum açma kimlik bilgilerini girmeniz gerekiyorsa, test uç noktanızın kodunu çözmek için [URL kodunu çöz](https://www.urldecoder.org/) kullanın. URL kod çözme "https://\<Kullanıcı adı >:\<Password > @\<Cluster-name >. test. azureapps. IO/Gateway/yeşil" biçiminde bir URL döndürür.  Uç noktanıza erişmek için bunu kullanın.

>[!NOTE]    
> Yapılandırma sunucusu ayarları, hazırlama ortamınız ve üretim için de geçerlidir. Örneğin, yapılandırma sunucusundaki uygulama ağ geçidiniz için bağlam yolunu (`server.servlet.context-path`) *somepath*olarak ayarlarsanız, yeşil dağıtımınızın yolu değişir: "https://\<username >:\<Password > @\<Cluster-name >. test.azureapps.io/gateway/green/somepath/... "
 
 Bu noktada herkese açık uygulama ağ geçidinizi ziyaret ederseniz, eski sayfayı yeni değiştirmeksizin görmeniz gerekir.
    
## <a name="set-the-green-as-production-deployment"></a>Yeşil üretim dağıtımı olarak ayarlama

1. Hazırlama ortamınızda yaptığınız değişikliği doğruladıktan sonra üretime gönderebilirsiniz. **Dağıtım yönetimine** dönün ve "ağ geçidi" uygulamasından önce onay kutusunu seçin.
2. "Dağıtımı ayarla" yı seçin.
3. "ÜRETIM DAĞıTıMı" menüsünden "yeşil" seçeneğini belirleyin ve **Uygula** ' yı seçin.
4. Ağ Geçidi uygulamanıza **genel bakış** sayfasına gidin. Ağ Geçidi uygulamanız için zaten bir etki alanı atadıysanız, URL **genel bakış** sayfasında görüntülenir. URL 'YI ziyaret ederek değiştirilen Piggy ölçümleri sayfasını görürsünüz.

>[!NOTE]
> Yeşil dağıtım üretim ortamına ayarlandığında, önceki dağıtım hazırlama dağıtımı olur.

## <a name="modify-the-staging-deployment"></a>Hazırlama dağıtımını değiştirme

Değişiklikden memnun değilseniz, uygulama kodunuzu değiştirebilir, yeni bir jar paketi oluşturabilir ve Azure CLı kullanarak yeşil dağıtımınıza yükleyebilirsiniz.

```azurecli
az spring-cloud app deploy  -g <resource-group-name> -s <service-instance-name> -n gateway -d green --jar-path gateway.jar
```

## <a name="delete-a-staging-deployment"></a>Hazırlama dağıtımını silme

Hazırlama dağıtımı sayfanıza gidip **Sil** düğmesini seçerek, hazırlama dağıtımınızı Azure bağlantı noktasından silin.

Alternatif olarak, hazırlama dağıtımınızı Azure CLı 'den aşağıdaki komutla silebilirsiniz:

```azurecli
az spring-cloud app deployment delete -n <staging-deployment-name> -g <resource-group-name> -s <service-instance-name> --app gateway
```
