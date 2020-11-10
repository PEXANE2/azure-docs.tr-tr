---
title: Azure Red Hat OpenShift 4 için yerleşik kapsayıcı kayıt defterini yapılandırma
description: Azure Red Hat OpenShift 4 için yerleşik kapsayıcı kayıt defterini yapılandırma
author: jiangma
ms.author: jiangma
ms.service: container-service
ms.topic: conceptual
ms.date: 10/15/2020
ms.openlocfilehash: 917da58c7f5ac2294fc30cd385a4834fde3f5f0b
ms.sourcegitcommit: 17b36b13857f573639d19d2afb6f2aca74ae56c1
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/10/2020
ms.locfileid: "94415307"
---
# <a name="configure-built-in-container-registry-for-azure-red-hat-openshift-4"></a>Azure Red Hat OpenShift 4 için yerleşik kapsayıcı kayıt defterini yapılandırma

Bu makalede, Azure Red Hat OpenShift (ARO) 4 kümesi için yerleşik kapsayıcı görüntüsü kayıt defterini yapılandıracaksınız. Şunları öğrenirsiniz:

> [!div class="checklist"]
> * Azure AD kurulumu
> * OpenID Connect 'i ayarlama
> * Yerleşik kapsayıcı görüntüsü kayıt defterine erişin

## <a name="prerequisites"></a>Önkoşullar

* [Azure Red Hat OpenShift 4 kümesi oluşturma](/azure/openshift/tutorial-create-cluster)bölümündeki adımları izleyerek bir küme oluşturun. Bağımsız değişkeniyle küme oluşturmayı unutmayın `--pull-secret` `az aro create` .  Bu makalenin gerektirdiği şekilde, oturum açmak için Azure AD 'yi ayarlamak istiyorsanız bu gereklidir.
* [Azure Red Hat OpenShift 4 kümesine bağlanma](/azure/openshift/tutorial-connect-cluster)bölümündeki adımları izleyerek kümeye bağlanın.
   * Bu makalenin ilerleyen kısımlarında komutunu kullanacağımız için "OpenShift CLı 'yı Install" bölümündeki adımları izlediğinizden emin olun `oc` .
   * Gibi görünen küme konsolu URL 'sini unutmayın `https://console-openshift-console.apps.<random>.<region>.aroapp.io/` . `<random>`Ve değerleri `<region>` Bu makalede daha sonra kullanılacaktır.
   * `kubeadmin`Kimlik bilgilerini aklınızda edin. Bu makalede daha sonra kullanılacaktır.

## <a name="set-up-azure-active-directory"></a>Azure Active Directory ayarlama

Azure Active Directory (Azure AD) OpenID Connect (OıDC) uygular. OıDC, ARO kümesinde oturum açmak için Azure AD 'yi kullanmanıza olanak sağlar. Azure AD 'yi ayarlamak için aşağıdaki adımları izleyin.

1. [Hızlı başlangıç: bir kiracı ayarlama](/azure/active-directory/develop/quickstart-create-new-tenant)bölümündeki adımları Izleyerek BIR Azure AD kiracısı ayarlayın. Azure hesabınız zaten bir kiracıya sahip olabilir. Varsa, bu adımları izlemek için kiracıda yeterli ayrıcalıklara sahipseniz, bir tane oluşturmayı atlayabilirsiniz. **KIRACı kimliğinizi** aklınızda edin. Bu değer daha sonra kullanılacaktır.
2. [Azure Active Directory kullanarak Kullanıcı ekleme veya silme](/azure/active-directory/fundamentals/add-users-azure-active-directory)bölümündeki adımları izleyerek en az BIR Azure AD kullanıcısı oluşturun. Bu hesapları veya kendi uygulamanızı test etmek için kullanabilirsiniz. Oturum açmak için bu hesapların e-posta adreslerini ve parolalarını buraya göz atın.
3. [Hızlı başlangıç: bir uygulamayı Microsoft Identity platformu Ile kaydetme](/azure/active-directory/develop/quickstart-register-app)bölümündeki adımları IZLEYEREK Azure AD kiracınızda yeni bir uygulama kaydı oluşturun. 
   1. Ve değerleri hakkındaki önkoşullara göz önünde çekin `<random>` `<region>` . Aşağıdaki formüle göre bir URI oluşturmak için bu değerleri kullanın:

      `https://oauth-openshift.apps.<random>.<region>.aroapp.io/oauth2callback/openid`
   1. **Yeniden yönlendirme URI 'si** alanıyla ilgili adıma ulaştığınızda, ÖNCEKI adımdan URI 'yi girin.
   1. **Kaydet** ’i seçin.
   1. Uygulamanın **genel bakış** sayfasında, burada gösterildiği gibi, **uygulama (istemci) kimliği** için gösterilen değere göz atın.
      :::image type="content" source="media/built-in-container-registry/azure-ad-app-overview-client-id.png" alt-text="Azure AD uygulaması 'nın genel bakış sayfası.":::

4. Yeni bir istemci gizli dizisi oluşturun. 
   1. Yeni oluşturulan uygulama kaydında, sol gezinti bölmesinde **sertifikalar & gizlilikler** ' ı seçin.
   1. **Yeni istemci gizli dizisi** ’ni seçin.
   1. **Bir açıklama** girin ve **Ekle** ' yi seçin.
   1. Oluşturulan **istemci gizli** değerini kaydederek saklayın. Bu değer, makalenin ilerleyen kısımlarında kullanılacaktır.

### <a name="add-openid-connect-identity-provider"></a>OpenID Connect kimlik sağlayıcısı ekle

ARO, yerleşik bir kapsayıcı kayıt defteri sağlar.  Erişmek için, bu adımları izleyerek Azure AD OıDC kullanarak bir kimlik sağlayıcısı (ıDP) yapılandırın.

1. Tarayıcınızdan OpenShift Web konsolunda oturum açın `kubeadmin` .  Bu, öğreticideki adımları yürütürken kullanılan yordamın aynısına sahiptir [: bir Azure Red Hat OpenShift 4 kümesine bağlanma](/azure/openshift/tutorial-connect-cluster).
1. Sol gezinti bölmesinde **Yönetim**  >  **kümesi ayarları** ' nı seçin.
1. Sayfanın ortasında **genel yapılandırma** ' yı seçin.
1. Tablodaki **yapılandırma kaynağı** sütununda **OAuth** ' ı bulun ve seçin.
1. **Kimlik sağlayıcıları** altında **Ekle** ' yi seçin ve **OpenID Connect** ' i seçin.
1. **Adı** **OpenID** olarak ayarlayın.  Bu değer zaten doldurulmuş olabilir.
1. **ISTEMCI kimliğini** ve **istemci gizli** anahtarını önceki adımdaki değerler olarak ayarlayın.
1. **Veren URL 'si** değerini bulmak için bu adımı izleyin.
   1. **\<tenant-id>** URL 'de **Azure Active Directory ayarlanan** bölüm için bir kayıt ile değiştirin `https://login.microsoftonline.com/<tenant-id>/v2.0/.well-known/openid-configuration` .
   1. Azure portal etkileşimde bulunmak için kullandığınız tarayıcıda URL 'YI açın.
   1. Döndürülen JSON gövdesinde özellik **verenin** değerini kopyalayın ve **sertifikayı veren URL** etiketli metin kutusuna yapıştırın.  **Veren** değeri, bir şekilde görünür `https://login.microsoftonline.com/44p4o433-2q55-474q-on88-4on94469o74n/v2.0` .
1. Sayfanın sonuna gidin ve **Ekle** ' yi seçin.
   :::image type="content" source="media/built-in-container-registry/openid-connect-identity-provider.png" alt-text="OpenShift 'te OpenID Connect.":::
1. Tarayıcı penceresinin sağ üst köşesinde bulunan **Kuto: admin** düğmesini seçerek ve **Oturumu Kapat** ' ı seçerek OpenShift Web konsolunda oturumunuzu kapatın.

### <a name="access-the-built-in-container-image-registry"></a>Yerleşik kapsayıcı görüntüsü kayıt defterine erişin

Aşağıdaki yönergeler yerleşik kayıt defterine erişimi etkinleştirir.

#### <a name="define-the-azure-ad-user-to-be-an-administrator"></a>Azure AD kullanıcısını yönetici olacak şekilde tanımlama

1. Azure AD kullanıcısının kimlik bilgilerini kullanarak, tarayıcınızdan OpenShift Web konsolunda oturum açın.

   1. Konsolda oturum açmak için InPrivate, ınbilito veya diğer eşdeğer tarayıcı penceresi özelliğini kullanın.
   1. OıDC etkinleştirildikten sonra pencere farklı görünecektir.
   :::image type="content" source="media/built-in-container-registry/oidc-enabled-login-window.png" alt-text="OpenID Connect etkinleştirilmiş oturum açma penceresi.":::
   1. **OpenID** seçin

   > [!NOTE]
   > Burada oturum açmak için kullandığınız kullanıcı adını ve parolayı bir yere göz atın. Bu Kullanıcı adı ve parola, bu ve diğer makalelerdeki diğer eylemler için yönetici olarak çalışır.
1. Aşağıdaki adımları kullanarak OpenShift CLı ile oturum açın.  Tartışma için bu işlem olarak bilinir `oc login` .
   1. Web konsolunun sağ üst kısmında, oturum açmış kullanıcının bağlam menüsünü genişletin ve ardından **login komutunu Kopyala** ' yı seçin.
   1. Gerekirse, aynı kullanıcıyla yeni bir sekme penceresinde oturum açın.
   1. **Görüntüleme belirtecini** seçin.
   1. Aşağıda gösterildiği gibi, aşağıda belirtilen değeri **Bu belirteçle** panoya kopyalayın ve bir kabukta çalıştırın.

       ```bash
       oc login --token=XOdASlzeT7BHT0JZW6Fd4dl5EwHpeBlN27TAdWHseob --server=https://api.aqlm62xm.rnfghf.aroapp.io:6443
       Logged into "https://api.aqlm62xm.rnfghf.aroapp.io:6443" as "kube:admin" using the token provided.

       You have access to 57 projects, the list has been suppressed. You can list all projects with 'oc projects'

       Using project "default".
       ```

1. `oc whoami`Konsolunda çalıştırın ve çıktıyı farklı şekilde aklınızda yapın **\<aad-user>** .  Bu değeri makalenin ilerleyen kısımlarında kullanacağız.
1. OpenShift web konsolunun oturumunu kapatın. Tarayıcı penceresinin sağ üst köşesindeki düğmeyi seçin **\<aad-user>** ve ardından **Oturumu Kapat** ' ı seçin.


#### <a name="grant-the-azure-ad-user-the-necessary-roles-for-registry-interaction"></a>Azure AD kullanıcısına kayıt defteri etkileşimi için gerekli rolleri verme

1. Kimlik bilgilerini kullanarak tarayıcınızdan OpenShift Web konsolunda oturum açın `kubeadmin` .
1. Yukarıdaki adımları izleyerek OpenShift CLı 'de belirteci ile oturum açın `kubeadmin` `oc login` , ancak ile Web konsolunda oturum açtıktan sonra bunu yapın `kubeadmin` .
1. **AAD-User** için yerleşik kayıt defterine erişimi etkinleştirmek üzere aşağıdaki komutları yürütün.

   ```bash
   # Switch to project "openshift-image-registry"
   oc project openshift-image-registry
   ```

   Çıktı aşağıdakine benzer görünmelidir.

   ```bash
   Now using project "openshift-image-registry" on server "https://api.x8xl3f4y.eastus.aroapp.io:6443".
   ```

   ```bash
   # Expose the registry using "DefaultRoute"
   oc patch configs.imageregistry.operator.openshift.io/cluster --patch '{"spec":{"defaultRoute":true}}' --type=merge
   ```

   Çıktı aşağıdakine benzer görünmelidir.

   ```bash
   config.imageregistry.operator.openshift.io/cluster patched
   ```

   ```bash
   # Add roles to "aad-user" for pulling and pushing images
   # Note: replace "<aad-user>" with the one you wrote down before
   oc policy add-role-to-user registry-viewer <aad-user>
   ```

   Çıktı aşağıdakine benzer görünmelidir.

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-viewer added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

   ```bash
   oc policy add-role-to-user registry-editor <aad-user>
   ```

   Çıktı aşağıdakine benzer görünmelidir.

   ```bash
   clusterrole.rbac.authorization.k8s.io/registry-editor added: "kaaIjx75vFWovvKF7c02M0ya5qzwcSJ074RZBfXUc34"
   ```

#### <a name="obtain-the-container-registry-url"></a>Kapsayıcı kayıt defteri URL 'sini alma

`oc get route`Kapsayıcı kayıt defteri URL 'sini almak için, komutu aşağıda gösterildiği gibi kullanın.

```bash
# Note: the value of "Container Registry URL" in the output is the fully qualified registry name.
HOST=$(oc get route default-route --template='{{ .spec.host }}')
echo "Container Registry URL: $HOST"
```

   > [!NOTE]
   > **CONTAINER REGISTRY URL** 'sinin konsol çıkışını aklınızda edin. Bu kılavuzda ve sonraki bölümlerde tam kayıt defteri adı olarak kullanılır.

## <a name="next-steps"></a>Sonraki adımlar

OpenShift üzerinde bir uygulama dağıtarak yerleşik kapsayıcı görüntüsü kayıt defterini kullanın.  Java uygulamaları için, [Azure Red Hat OpenShift 4 kümesinde açık özgürlük/Web küre özgürlük Ile Java uygulaması dağıtma](howto-deploy-java-liberty-app.md)hakkında nasıl yapılır Kılavuzu ' nu izleyin.
