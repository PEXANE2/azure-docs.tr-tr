---
title: Azure yay bulutu 'nda yapılandırma sunucunuzu ayarlama | Microsoft Docs
description: Bu öğreticide, Azure yay bulutunuz için Azure portal bir yay bulutu yapılandırma sunucusu ayarlamayı öğreneceksiniz.
services: spring-cloud
ms.service: spring-cloud
ms.topic: tutorial
ms.reviewer: jeconnoc
ms.author: v-vasuke
author: v-vasuke
ms.date: 08/08/2019
ms.openlocfilehash: f959d216859ded54de52de474ee3150f91bcfff1
ms.sourcegitcommit: d773b5743cb54b8cbcfa5c5e4d21d5b45a58b081
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/08/2019
ms.locfileid: "72038926"
---
# <a name="tutorial-set-up-a-spring-cloud-config-server-for-your-service"></a>Öğretici: hizmetiniz için bir yay bulutu yapılandırma sunucusu ayarlama

Bu öğreticide, bir yay bulutu yapılandırma sunucusunu Azure Spring Cloud Service 'e nasıl bağlayacaksınız.

Spring Cloud config, dağıtılmış bir sistemde externalized yapılandırması için sunucu ve istemci tarafı desteği sağlar. Yapılandırma sunucusu ile tüm ortamlarda uygulamalara yönelik dış özellikleri yönetmek için merkezi bir yerdir. Daha fazla bilgi edinmek için [Spring Cloud config Server başvurusunu](https://spring.io/projects/spring-cloud-config)ziyaret edin.

## <a name="prerequisites"></a>Önkoşullar
* Azure aboneliği. Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun. 
* Zaten sağlanmış ve Azure yay bulut hizmeti çalıştırılıyor.  Azure yay bulut hizmeti sağlamak ve başlatmak için [Bu hızlı](spring-cloud-quickstart-launch-app-cli.md) başlangıcı doldurun.


## <a name="restriction"></a>Kısıtlama

__Yapılandırma sunucusunu__ git arka ucu ile kullandığınızda bazı kısıtlamalar vardır. __Yapılandırma sunucusuna__ ve __hizmet bulmaya__erişmek için bazı özellikler otomatik olarak uygulama ortamınıza eklenecektir. Ayrıca, bu özellikleri **yapılandırma sunucusu** dosyalarınızda yapılandırırsanız, çakışmalar ve beklenmeyen davranışlarla karşılaşabilirsiniz.  Özellikler şunları içerir: 

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

Azure Spring Cloud, yapılandırma sunucusu dosyalarınızı depolamak için Azure DevOps, GitHub, GitLab ve BitBucket 'ı destekler.  Deponuzu hazırladığınızda, yapılandırma dosyalarını aşağıdaki yönergelerle yapın ve burada saklayın.

Ayrıca, bazı yapılandırılabilir özellikler yalnızca bazı türler için kullanılabilir. Aşağıdaki alt bölümlerde her depo türü için özellikler listelenmektedir.


### <a name="public-repository"></a>Ortak depo

Ortak bir depo kullanılırken, yapılandırılabilir özelliklerdir.

Ortak `Git` deposunu kurmak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Yalnızca tire ("-") ayrılmış sözcükler adlandırma kuralı şimdilik desteklenir. Örneğin, `default-label` ' ı kullanırsınız ancak `defaultLabel` ' i kullanın.

| Özellik        | Gereklidir | Özellik                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` `uri` `http://`, `https://`, `git@` veya `ssh://` ile başlatılmış olmalıdır. |
| `default-label` | `no`     | @No__t-0 deposunun varsayılan etiketi, deponun `branch name`, `tag name` veya `commit-id` olması gerekir. |
| `search-paths`  | `no`     | @No__t-0 deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |

------

### <a name="private-repository-with-ssh-authentication"></a>SSH kimlik doğrulaması ile özel depo

@No__t-1 ile özel `Git` deposu kurmak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Yalnızca tire ("-") ayrılmış sözcükler adlandırma kuralı şimdilik desteklenir. Örneğin, `default-label` ' ı kullanırsınız ancak `defaultLabel` ' i kullanın.

| Özellik                   | Gereklidir | Özellik                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | `yes`    | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` `uri` `http://`, `https://`, `git@` veya `ssh://` ile başlatılmış olmalıdır. |
| `default-label`            | `no`     | @No__t-0 deposunun varsayılan etiketi, deponun `branch name`, `tag name` veya `commit-id` olması gerekir. |
| `search-paths`             | `no`     | @No__t-0 deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |
| `private-key`              | `no`     | @No__t-1 deposuna erişmek için `Ssh` özel anahtarı `uri` `git@` veya `ssh://` ile başlatıldığında __gereklidir__ . |
| `host-key`                 | `no`     | Git deposu sunucusunun ana bilgisayar anahtarı, `host-key-algorithm` ' ı kapsayan algoritma önekini içermemelidir. |
| `host-key-algorithm`       | `no`     | Ana bilgisayar anahtar algoritması `ssh-dss` ' dan biri olmalıdır. `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` ve `ecdsa-sha2-nistp521`. Yalnızca `host-key` varsa gereklidir. |
| `strict-host-key-checking` | `no`     | @No__t-0 ' ı kullanırken yapılandırma sunucusunun başlayamayacağını belirtir, `true` (varsayılan değer) veya `false` olmalıdır. |

-----

### <a name="private-repository-with-basic-authentication"></a>Temel kimlik doğrulaması ile özel depo

Temel kimlik doğrulaması ile özel Git deposunu kurmak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Yalnızca tire ("-") ayrılmış sözcükler adlandırma kuralı şimdilik desteklenir. Örneğin, `default-label` ' ı kullanabilirsiniz, ancak `defaultLabel` ' i kullanabilirsiniz.

| Özellik        | Gereklidir | Özellik                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | `yes`    | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` `uri` `http://`, `https://`, `git@` veya `ssh://` ile başlatılmış olmalıdır. |
| `default-label` | `no`     | @No__t-0 deposunun varsayılan etiketi, deponun `branch name`, `tag name` veya `commit-id` olması gerekir. |
| `search-paths`  | `no`     | @No__t-0 deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |
| `username`      | `no`     | @No__t-1 depo sunucusuna erişmek için kullanılan @no__t- __3 @no__t-__ 3 depo sunucusu desteği `Http Basic Authentication`. |
| `password`      | `no`     | @No__t-0 depo sunucusuna erişmek için kullanılan parola, __gerekli__ @no__t 2 depo sunucusu desteği `Http Basic Authentication`. |

> [!NOTE]
> GitHub gibi bazı `Git` depo sunucuları, `HTTP Basic Authentication` için parola olarak bir "kişisel belirteç" veya "erişim belirteci" destekler. Bu tür bir belirteci burada parola olarak kullanabilirsiniz ve "kişisel belirteç" veya "erişim-belirteç" kullanım süresini sona ermeyecektir. Ancak BitBucket ve Azure DevOps gibi git deposu sunucularında, belirtecin süresi bir veya iki saat içinde sona erer ve bu seçenek Azure Spring Cloud ile kullanılması için önemli olmayan bir şekilde yapılır.

### <a name="git-repositories-with-pattern"></a>Desenli Git depoları

Aşağıdaki şekilde Git depoları ayarlamak için kullanılan yapılandırılabilir tüm özellikler aşağıda listelenmiştir.

> [!NOTE]
> Yalnızca tire ("-") ayrılmış sözcükler adlandırma kuralı şimdilik desteklenir. Örneğin, `default-label` ' ı kullanabilirsiniz, ancak `defaultLabel` ' i kullanabilirsiniz.

| Özellik                           | Gereklidir         | Özellik                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | `no`             | Bir harita, verilen ada sahip `Git` depoları ayarlarından oluşur. |
| `repos."uri"`                      | `yes` `repos` | Yapılandırma sunucusu arka ucu olarak kullanılan `Git` `uri` `http://`, `https://`, `git@` veya `ssh://` ile başlatılmış olmalıdır. |
| `repos."name"`                     | `yes` `repos` | Yalnızca `repos` varsa, __gerekli__ bir `Git` deposunun tanımlanması için bir ad. Örneğin, yukarıdaki `team-A`, `team-B`. |
| `repos."pattern"`                  | `no`             | Her bir model için uygulama adını eşleştirmek için kullanılan bir dize dizisi joker karakterlerle @no__t 0 biçimi alır. |
| `repos."default-label"`            | `no`             | @No__t-0 deposunun varsayılan etiketi, deponun `branch name`, `tag name` veya `commit-id` olması gerekir. |
| `repos."search-paths` "             | `no`             | @No__t-0 deposunun alt dizinlerinde arama yapmak için kullanılan bir dize dizisi. |
| `repos."username"`                 | `no`             | @No__t-1 depo sunucusuna erişmek için kullanılan @no__t- __3 @no__t-__ 3 depo sunucusu desteği `Http Basic Authentication`. |
| `repos."password"`                 | `no`             | @No__t-0 depo sunucusuna erişmek için kullanılan parola, __gerekli__ @no__t 2 depo sunucusu desteği `Http Basic Authentication`. |
| `repos."private-key"`              | `no`             | @No__t-1 deposuna erişmek için `Ssh` özel anahtarı `uri` `git@` veya `ssh://` ile başlatıldığında __gereklidir__ . |
| `repos."host-key"`                 | `no`             | Git deposu sunucusunun ana bilgisayar anahtarı, `host-key-algorithm` ' ı kapsayan algoritma önekini içermemelidir. |
| `repos."host-key-algorithm"`       | `no`             | Ana bilgisayar anahtar algoritması `ssh-dss` ' dan biri olmalıdır. `ssh-rsa`, `ecdsa-sha2-nistp256`, `ecdsa-sha2-nistp384` ve `ecdsa-sha2-nistp521`. Yalnızca `host-key` varsa __gereklidir__ . |
| `repos."strict-host-key-checking"` | `no`             | @No__t-0 ' ı kullanırken yapılandırma sunucusunun başlayamayacağını belirtir, `true` (varsayılan değer) veya `false` olmalıdır. |

### <a name="import-applicationyml-file-from-spring-cloud-config"></a>Yay bulutu yapılandırmadan `application.yml` dosyasını içeri aktar

Varsayılan yapılandırma sunucusu ayarlarını doğrudan [Spring Cloud config](https://spring.io/projects/spring-cloud-config) Web sitesinden içeri aktarabilirsiniz. Bunu doğrudan Azure portal yapabilirsiniz, bu nedenle yapılandırma sunucusu dosyalarınızı veya deponuzu hazırlamak için hemen herhangi bir adım uygulamanız gerekmez.

## <a name="attaching-your-config-server-repository-to-azure-spring-cloud"></a>Yapılandırma sunucusu deponuzu Azure Spring buluta iliştirme

Yapılandırma dosyalarınız bir depoda kayıtlı olduğuna göre, Azure Spring Cloud 'ı buna bağlamanız gerekir.

1. [Azure Portal](https://portal.azure.com)’da oturum açın.

1. Azure Spring Cloud **genel bakış** sayfanıza gidin.

1. Sol taraftaki menüdeki **Ayarlar** başlığı altındaki **yapılandırma sunucusu** sekmesine gidin.

### <a name="public-repository"></a>Ortak depo

Deponuz ortak ise, **genel** düğmesine tıklayıp URL 'yi yapıştırmanız yeterlidir.

### <a name="private-repository"></a>Özel depo

Azure Spring Cloud, SSH kimlik doğrulamasını destekler. Deponuza ortak anahtar eklemek için Azure portal yönergeleri izleyin. Daha sonra, özel anahtarınızı yapılandırma dosyasına eklediğinizden emin olun.

Yapılandırma sunucunuzu ayarlamayı son vermek için **Uygula** ' ya tıklayın.


## <a name="delete-your-app-configuration"></a>Uygulama yapılandırmanızı silme

Bir yapılandırma dosyasını kaydettikten sonra **yapılandırma** sekmesinde **uygulama yapılandırmasını sil** düğmesi görünür. Bu işlem, mevcut ayarlarınızı tamamen siler. Yapılandırma sunucunuzu GitHub 'dan Azure DevOps 'a geçme gibi başka bir kaynağa bağlamak istiyorsanız bunu yapmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, yapılandırma sunucusunu etkinleştirme ve yapılandırma hakkında daha fazla öğrendiniz. Uygulamanızı yönetme hakkında daha fazla bilgi edinmek için uygulamanızı el ile ölçeklendirmeye yönelik öğreticiye geçin.

> [!div class="nextstepaction"]
> [Azure Spring Cloud uygulamanızı nasıl el ile Ölçeklendireceğinizi öğrenin](spring-cloud-tutorial-scale-manual.md).

