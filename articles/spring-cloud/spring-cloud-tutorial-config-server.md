---
title: Azure yay bulutu 'nda yapılandırma sunucunuzu ayarlama | Microsoft Docs
description: Bu öğreticide, Azure yay bulutunuz için Azure portal bir yay bulutu yapılandırma sunucusu ayarlamayı öğreneceksiniz.
services: spring-cloud
ms.service: spring-cloud
ms.topic: tutorial
ms.reviewer: jeconnoc
ms.author: v-vasuke
author: v-vasuke
ms.date: 10/18/2019
ms.openlocfilehash: 3a091c22f49ec31029a1808c10e675a4d0960fb4
ms.sourcegitcommit: 98ce5583e376943aaa9773bf8efe0b324a55e58c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73177940"
---
# <a name="tutorial-set-up-a-spring-cloud-config-server-for-your-service"></a>Öğretici: hizmetiniz için bir yay bulutu yapılandırma sunucusu ayarlama

Bu öğreticide, bir yay bulutu yapılandırma sunucusunu Azure Spring Cloud Service 'e nasıl bağlayacaksınız.

Spring Cloud config, dağıtılmış bir sistemde externalized yapılandırması için sunucu ve istemci tarafı desteği sağlar. Yapılandırma sunucusu ile tüm ortamlarda uygulamalara yönelik dış özellikleri yönetmek için merkezi bir yerdir. Daha fazla bilgi edinmek için [Spring Cloud config Server başvurusunu](https://spring.io/projects/spring-cloud-config)ziyaret edin.

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
* Zaten sağlanmış ve Azure yay bulut hizmeti çalıştırılıyor.  Azure yay bulut hizmeti sağlamak ve başlatmak için [Bu hızlı](spring-cloud-quickstart-launch-app-cli.md) başlangıcı doldurun.

## <a name="restriction"></a>Kısıtlama

__Yapılandırma sunucusunu__ git arka ucu ile kullandığınızda bazı kısıtlamalar vardır. __Yapılandırma sunucusuna__ ve __hizmet bulmaya__erişmek için bazı özellikler otomatik olarak uygulama ortamınıza eklenecektir. Ayrıca, bu özellikleri **yapılandırma sunucusu** dosyalarınızda yapılandırırsanız, çakışmalar ve beklenmeyen davranışlarla karşılaşabilirsiniz. Özellikler şunları içerir: 

```yaml
eureka.client.service-url.defaultZone
eureka.client.tls.keystore
server.port
spring.cloud.config.tls.keystore
spring.application.name
```
> [!CAUTION]
> Yukarıdaki özellikleri __config Server__ uygulama __dosyalarınıza yerleştirmemenizi__ kesinlikle öneririz.

## <a name="create-your-config-server-files"></a>Yapılandırma sunucusu dosyalarınızı oluşturma

Azure Spring Cloud, yapılandırma sunucusu dosyalarınızı depolamak için Azure DevOps, GitHub, GitLab ve BitBucket 'ı destekler. Deponuzu hazırladığınızda, yapılandırma dosyalarını aşağıdaki yönergelerle yapın ve burada saklayın.

Ayrıca, bazı yapılandırılabilir özellikler yalnızca bazı türler için kullanılabilir. Aşağıdaki alt bölümlerde her depo türü için özellikler listelenmektedir.

### <a name="public-repository"></a>Ortak depo

Ortak bir depo kullanılırken, yapılandırılabilir özelliklerdir.

Ortak `Git` deposunu kurmak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Sözcükleri ayırmak için tire ("-") kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, `defaultLabel``default-label`kullanabilirsiniz, ancak kullanamazsınız.

| Özellik        | Gereklidir | Özellik                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` deposunun `uri`, `http://`, `https://`, `git@`veya `ssh://`ile başlatılmış olmalıdır. |
| `default-label` | `no`     | `Git` deposunun varsayılan etiketi, deponun `branch name`, `tag name`veya `commit-id` olmalıdır. |
| `search-paths`  | `no`     | `Git` deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |

------

### <a name="private-repository-with-ssh-authentication"></a>SSH kimlik doğrulaması ile özel depo

`Ssh` ile özel `Git` deposunu kurmak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Sözcükleri ayırmak için tire ("-") kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, `defaultLabel``default-label`kullanabilirsiniz, ancak kullanamazsınız.

| Özellik                   | Gereklidir | Özellik                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | `yes`    | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` deposunun `uri`, `http://`, `https://`, `git@`veya `ssh://`ile başlatılmış olmalıdır. |
| `default-label`            | `no`     | `Git` deposunun varsayılan etiketi, deponun `branch name`, `tag name`veya `commit-id` olmalıdır. |
| `search-paths`             | `no`     | `Git` deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |
| `private-key`              | `no`     | `Git` deposuna erişmek için `Ssh` özel anahtar, `uri` `git@` veya `ssh://`ile başladığında __gereklidir__ . |
| `host-key`                 | `no`     | Git deposu sunucusunun ana bilgisayar anahtarı, `host-key-algorithm` ' ı kapsayan algoritma önekini içermemelidir. |
| `host-key-algorithm`       | `no`     | Konak anahtar algoritması `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384`veya `ecdsa-sha2-nistp521`olmalıdır. Yalnızca `host-key` varsa gereklidir. |
| `strict-host-key-checking` | `no`     | Yapılandırma sunucusunun özel `host-key`kullanılırken başlatılıp başlatılmayacağını belirtir. `true` (varsayılan değer) veya `false`olmalıdır. |

-----

### <a name="private-repository-with-basic-authentication"></a>Temel kimlik doğrulaması ile özel depo

Temel kimlik doğrulaması ile özel Git deposunu kurmak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Sözcükleri ayırmak için tire ("-") kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, `default-label` `defaultLabel`değil ' i kullanın.

| Özellik        | Gereklidir | Özellik                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` deposunun `uri`, `http://`, `https://`, `git@`veya `ssh://`ile başlatılmış olmalıdır. |
| `default-label` | `no`     | `Git` deposunun varsayılan etiketi, deponun `branch name`, `tag name`veya `commit-id` olmalıdır. |
| `search-paths`  | `no`     | `Git` deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |
| `username`      | `no`     | `Git` deposu sunucusu `Http Basic Authentication`desteklediğinde, __gerekli__ olan `Git` depo sunucusuna erişmek için kullanılan `username`. |
| `password`      | `no`     | `Git` depo sunucusu `Http Basic Authentication`destekliyorsa, __gereken__ `Git` Repository sunucusuna erişmek için kullanılan parola. |

> [!NOTE]
> GitHub gibi bazı `Git` depo sunucuları, `HTTP Basic Authentication` için parola olarak bir "kişisel belirteç" veya "erişim belirteci" destekler. Bu tür bir belirteci burada parola olarak kullanabilirsiniz ve "kişisel belirteç" veya "erişim-belirteç" kullanım süresini sona ermeyecektir. Ancak, BitBucket ve Azure DevOps gibi git deposu sunucularında, belirtecin süresi bir veya iki saat içinde sona erer ve bu seçenek Azure Spring Cloud ile kullanım için uygun değildir.

### <a name="git-repositories-with-pattern"></a>Desenli Git depoları

Aşağıdaki şekilde Git depoları ayarlamak için kullanılan yapılandırılabilir tüm özellikler aşağıda listelenmiştir.

> [!NOTE]
> Sözcükleri ayırmak için tire ("-") kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, `default-label` `defaultLabel`değil ' i kullanın.

| Özellik                           | Gereklidir         | Özellik                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | `no`             | Verilen bir ada sahip bir `Git` deposu ayarlarından oluşan bir harita. |
| `repos."uri"`                      | `yes` `repos` | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` deposunun `uri`, `http://`, `https://`, `git@`veya `ssh://`ile başlatılmış olmalıdır. |
| `repos."name"`                     | `yes` `repos` | Yalnızca `repos` varsa, __gerekli__ bir `Git` deposunu tanımlamak için bir ad. Örneğin, yukarıdaki `team-A`, `team-B`. |
| `repos."pattern"`                  | `no`             | Bir uygulama adıyla eşleşen dizeler dizisi. Her bir düzende `{application}/{profile}` biçimini joker karakterlerle kullanın. |
| `repos."default-label"`            | `no`             | `Git` deposunun varsayılan etiketi, deponun `branch name`, `tag name`veya `commit-id` olmalıdır. |
| `repos."search-paths` "             | `no`             | `Git` deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |
| `repos."username"`                 | `no`             | `Git` deposu sunucusu `Http Basic Authentication`desteklediğinde, __gerekli__ olan `Git` depo sunucusuna erişmek için kullanılan `username`. |
| `repos."password"`                 | `no`             | `Git` depo sunucusu `Http Basic Authentication`destekliyorsa, __gereken__ `Git` Repository sunucusuna erişmek için kullanılan parola. |
| `repos."private-key"`              | `no`             | `Git` depoya erişmek için `Ssh` özel anahtar, `uri` `git@` veya `ssh://`ile başladığında __gereklidir__ . |
| `repos."host-key"`                 | `no`             | Git deposu sunucusunun ana bilgisayar anahtarı, `host-key-algorithm` ' ı kapsayan algoritma önekini içermemelidir. |
| `repos."host-key-algorithm"`       | `no`             | Konak anahtar algoritması `ssh-dss`, `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384`veya `ecdsa-sha2-nistp521`olmalıdır. Yalnızca `host-key` varsa __gereklidir__ . |
| `repos."strict-host-key-checking"` | `no`             | Yapılandırma sunucusunun özel `host-key`kullanılırken başlatılıp başlatılmayacağını belirtir. `true` (varsayılan değer) veya `false`olmalıdır. |

## <a name="attaching-your-config-server-repository-to-azure-spring-cloud"></a>Yapılandırma sunucusu deponuzu Azure Spring buluta iliştirme

Yapılandırma dosyalarınız bir depoda kayıtlı olduğuna göre, Azure Spring Cloud 'ı buna bağlamanız gerekir.

1. [Azure Portal](https://portal.azure.com)’da oturum açın.

1. Azure Spring Cloud **genel bakış** sayfanıza gidin.

1. Sol taraftaki menüdeki **Ayarlar** başlığı altındaki **yapılandırma sunucusu** sekmesine gidin.

![pencere ekran görüntüsü](media/spring-cloud-tutorial-config-server/portal-config-server.png)

### <a name="input-repository-information-directly-to-the-azure-portal"></a>Depo bilgilerini doğrudan Azure portal giriş

#### <a name="default-repository"></a>Varsayılan depo

* Ortak depo: **varsayılan depo** bölümünde, **URI** bölümüne depo URI 'Sini yapıştırın ve **kimlik doğrulama** ayarının **genel**olduğundan emin olun. Ardından, **son ' a** tıklayın. 

* Özel depo: Azure yay bulutu, temel parola/belirteç tabanlı kimlik doğrulama ve SSH 'yi destekler.

    * Temel kimlik doğrulaması: **varsayılan depo** bölümünde, **URI** bölümüne deponun URI 'Sini yapıştırın ve ardından **kimlik doğrulamaya**tıklayın. **Kimlik doğrulama türü** olarak **temel** ' yı seçin ve Azure yay bulutuna erişim sağlamak için Kullanıcı adınızı ve parolanızı/belirtecinizi girin. Yapılandırma sunucunuzu ayarlamayı bitirmeden **Tamam** ' a ve **Uygula** ' ya tıklayın.

    ![pencere ekran görüntüsü](media/spring-cloud-tutorial-config-server/basic-auth.png)
    
    > [!CAUTION]
    > GitHub gibi bazı git deposu sunucuları, **temel kimlik doğrulaması**için bir parola gibi bir `personal-token` veya `access-token` kullanır. Bu tür bir belirteci Azure yay bulutunda hiçbir zaman dolmayacağı için parola olarak kullanabilirsiniz. Ancak BitBucket ve Azure DevOps gibi diğer git deposu sunucuları için `access-token` süresi bir veya iki saat içinde dolacak. Bu seçenek, Azure Spring Cloud ile bu depo sunucuları kullanılırken önemli değildir.]

    * SSH: **varsayılan depo** bölümünde, deponun URI 'sini **Uri** bölümüne yapıştırın ve **kimlik doğrulaması**' na tıklayın. **Kimlik doğrulama türü** olarak **SSH** ' ı seçin ve **özel anahtarınızı**girin. İsteğe bağlı olarak, **ana bilgisayar anahtarınızı** ve **ana bilgisayar anahtar algoritmanızı**belirtebilirsiniz. Yapılandırma sunucusu deponuza ortak anahtarınızı eklediğinizden emin olun. Yapılandırma sunucunuzu ayarlamayı bitirmeden **Tamam** ' a ve **Uygula** ' ya tıklayın.

    ![pencere ekran görüntüsü](media/spring-cloud-tutorial-config-server/ssh-auth.png)

#### <a name="pattern-repository"></a>Model deposu

Hizmetinizi yapılandırmak için isteğe bağlı bir **model deposu** kullanmak Istiyorsanız, **URI** 'Yi ve **kimlik doğrulamasını** **varsayılan depoyla**aynı şekilde belirtin. Hiyerarşiniz için bir **ad** eklediğinizden emin olun ve ardından örneğine eklemek için **Uygula** ' ya tıklayın. 

### <a name="enter-repository-information-into-a-yaml-file"></a>Bir YAML dosyasına depo bilgilerini girin

Depo ayarlarınızla bir YAML dosyası yazdıysanız, YAML Dosyanızı doğrudan yerel makinenizden Azure Spring Cloud 'a aktarabilirsiniz. Temel kimlik doğrulaması ile özel bir depo için basit bir YAML dosyası şuna benzer:

```yml
spring:
    cloud:
        config:
            server:
                git:
                    uri: https://github.com/azure-spring-cloud-samples/config-server-repository.git
                    username: <username>
                    password: <password/token>

```

**Ayarları Içeri aktar** düğmesine tıklayın ve ardından proje dizininizdeki `.yml` dosyasını seçin. **Içeri aktar**' a tıklayın, sonra **bildirimlerinizin** bir `async` işlemi açılır. 1-2 dakika sonra, başarıyı bildirmeli.

![pencere ekran görüntüsü](media/spring-cloud-tutorial-config-server/local-yml-success.png)


Azure portal görünen YAML dosyanızdaki bilgileri görmeniz gerekir. Son için **Uygula** ' ya tıklayın. 


## <a name="delete-your-app-configuration"></a>Uygulama yapılandırmanızı silme

Bir yapılandırma dosyasını kaydettikten sonra **yapılandırma** sekmesinde **uygulama yapılandırmasını sil** düğmesi görünür. Bu işlem, mevcut ayarlarınızı tamamen siler. Yapılandırma sunucunuzu GitHub 'dan Azure DevOps 'a geçme gibi başka bir kaynağa bağlamak istiyorsanız bunu yapmanız gerekir.



## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yapılandırma sunucusunu etkinleştirme ve yapılandırma hakkında daha fazla öğrendiniz. Uygulamanızı yönetme hakkında daha fazla bilgi edinmek için uygulamanızı el ile ölçeklendirmeye yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı nasıl el ile Ölçeklendireceğinizi öğrenin](spring-cloud-tutorial-scale-manual.md).

