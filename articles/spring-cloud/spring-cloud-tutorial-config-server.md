---
title: Öğretici - Config Server örneğini Azure Bahar Bulutu'nda ayarlama
description: Bu eğitimde, Azure portalınızda Azure Bulutu'nuz için Bir Bahar Bulutu Config Server örneğini nasıl ayarlayabileceğinizi öğreneceksiniz
ms.service: spring-cloud
ms.topic: tutorial
ms.author: brendm
author: bmitchell287
ms.date: 10/18/2019
ms.openlocfilehash: 5e0b5633a153583117cfe0d90ec5c0e7c5f2a147
ms.sourcegitcommit: 0947111b263015136bca0e6ec5a8c570b3f700ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/24/2020
ms.locfileid: "76277522"
---
# <a name="tutorial-set-up-a-spring-cloud-config-server-instance-for-your-service"></a>Öğretici: Hizmetiniz için Bir Bahar Bulutu Config Server örneği ayarlama

Bu makalede, Bir Bahar Bulutu Config Server örneğini Azure Bulut hizmetinize nasıl bağlayabileceğiniz gösterilmektedir.

Yay Bulut Config dağıtılmış bir sistemde haricileştirilmiş yapılandırma için sunucu ve istemci tarafı desteği sağlar. Config Server örneğinde, tüm ortamlardaki uygulamaların dış özelliklerini yönetmek için merkezi bir yere sahipsiniz. Daha fazla bilgi için [Bkz. Spring Cloud Config Server başvurusu.](https://spring.io/projects/spring-cloud-config)

## <a name="prerequisites"></a>Ön koşullar
* Azure aboneliği. Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun. 
* Zaten sağlanan ve çalışan bir Azure İlkbahar Bulut hizmeti. Bir Azure Bahar Bulutu hizmeti kurmak ve başlatmak için [Bkz. Quickstart: Azure CLI'yi kullanarak bir Java Bahar uygulaması başlatın.](spring-cloud-quickstart-launch-app-cli.md)

## <a name="restriction"></a>Kısıtlama

Bir Git arka uç ile Config Server kullandığınızda bazı kısıtlamalar vardır. Config Server ve Service Discovery'ye erişmek için bazı özellikler uygulama ortamınıza otomatik olarak enjekte edilir. Bu özellikleri Config Server dosyalarınızdan da yapılandırırsanız, çakışmalar ve beklenmeyen davranışlarla karşılaşabilirsiniz. Özellikleri şunlardır: 

```yaml
eureka.client.service-url.defaultZone
eureka.client.tls.keystore
server.port
spring.cloud.config.tls.keystore
spring.application.name
```

> [!CAUTION]
> Yukarıdaki özellikleri Config Server uygulama dosyalarınıza _koymamanızı_ şiddetle öneririz.

## <a name="create-your-config-server-files"></a>Config Server dosyalarınızı oluşturun

Azure Yay Bulutu, Config Server dosyalarınızı depolamak için Azure DevOps, GitHub, GitLab ve Bitbucket'ı destekler. Deponuz hazır olduğunda, yapılandırma dosyalarını aşağıdaki yönergeleri içeren oluşturun ve bunları orada saklayın.

Ayrıca, bazı yapılandırılabilir özellikler yalnızca belirli türler için kullanılabilir. Aşağıdaki alt bölümlerde her depo türü için özellikler listelenmiştir.

### <a name="public-repository"></a>Genel depo

Ortak bir depo kullandığınızda, yapılandırılabilir özellikleriniz daha sınırlıdır.

Ortak Git deposunu ayarlamak için kullanılan tüm yapılandırılabilir özellikler aşağıdaki tabloda listelenir:

> [!NOTE]
> Sözcükleri ayırmak için tire (-) kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, *varsayılan etiket,* ancak varsayılan *Etiket*kullanabilirsiniz.

| Özellik        | Gerekli | Özellik                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | Evet    | Config Server arka uç olarak kullanılan Git deposunun URI *http://* ile başlar , *https://*, *git@*, veya *ssh://*. |
| `default-label` | Hayır     | Git deposunun varsayılan etiketi, *deponun şube adı,* *etiket adı*veya *commit-id* olmalıdır. |
| `search-paths`  | Hayır     | Git deposunun alt dizinlerini aramak için kullanılan bir dizi dize. |

------

### <a name="private-repository-with-ssh-authentication"></a>SSH kimlik doğrulaması ile özel depo

SSH ile özel Git deposu kurmak için kullanılan tüm yapılandırılabilir özellikler aşağıdaki tabloda listelenir:

> [!NOTE]
> Sözcükleri ayırmak için tire (-) kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, *varsayılan etiket,* ancak varsayılan *Etiket*kullanabilirsiniz.

| Özellik                   | Gerekli | Özellik                                                      |
| :------------------------- | -------- | ------------------------------------------------------------ |
| `uri`                      | Evet    | Config Server arka uç olarak kullanılan Git deposunun URI, *http://* ile başlanmalıdır , *https://*, *git@*, veya *ssh://*. |
| `default-label`            | Hayır     | Git deposunun varsayılan etiketi, *deponun şube adı,* *etiket adı*veya *commit-id* olmalıdır. |
| `search-paths`             | Hayır     | Git deposunun alt dizinlerini aramak için kullanılan bir dizi dize. |
| `private-key`              | Hayır     | URI *git@* veya *ssh://* ile başladığında _gerekli_ Git deposuna erişmek için SSH özel anahtarı. |
| `host-key`                 | Hayır     | Git deposu sunucusunun ana bilgisayar anahtarı, algoritma önekinin kapsadığı `host-key-algorithm`gibi içermemelidir. |
| `host-key-algorithm`       | Hayır     | Ana anahtar algoritması, *ssh-dss*olmalıdır , *ssh-rsa*, *ecdsa-sha2-nistp256*, *ecdsa-sha2-nistp384*, veya *ecdsa-sha2-nistp521*. *Yalnızca* `host-key` varsa gereklidir. |
| `strict-host-key-checking` | Hayır     | Config Server örneğinin özel `host-key`sunucudan yararlanArak başlatılıp başlatılmayacağını gösterir. *Doğru* (varsayılan değer) veya *yanlış*olmalıdır. |

-----

### <a name="private-repository-with-basic-authentication"></a>Temel kimlik doğrulaması ile özel depo

Temel kimlik doğrulaması ile özel Git deposu nu ayarlamak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Sözcükleri ayırmak için tire (-) kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, *varsayılan etiket*değil, *varsayılan Etiket*kullanın.

| Özellik        | Gerekli | Özellik                                                      |
| :-------------- | -------- | ------------------------------------------------------------ |
| `uri`           | Evet    | Config Server arka uç olarak kullanılan Git deposunun URI *http://*, *https://*, *git@*, veya *ssh://* ile başlatılmalıdır. |
| `default-label` | Hayır     | Git deposunun varsayılan etiketi, *deponun şube adı,* *etiket adı*veya *commit-id* olmalıdır. |
| `search-paths`  | Hayır     | Git deposunun alt dizinlerini aramak için kullanılan bir dizi dize. |
| `username`      | Hayır     | Git deposu sunucusuna erişmek için kullanılan kullanıcı adı, Git deposu sunucusu desteklediğinde `Http Basic Authentication` _gereklidir._ |
| `password`      | Hayır     | Git deposu sunucusuna erişmek için kullanılan parola, Git deposu sunucusu desteklediğinde `Http Basic Authentication` _gereklidir._ |

> [!NOTE]
> Birçok `Git` depo sunucusu, HTTP Temel Kimlik Doğrulama'nın parolaları yerine belirteçlerin kullanımını destekler. GitHub gibi bazı depolar belirteçlerin süresiz olarak devam etmesine izin verir. Ancak, Azure DevOps de dahil olmak üzere bazı Git depo sunucuları, belirteçlerin birkaç saat içinde sona ermesini zorlar. Belirteçlerin süresinin dolmasına neden olan depolar, Azure İlkbahar Bulutu ile belirteç tabanlı kimlik doğrulamasını kullanmamalıdır.

### <a name="git-repositories-with-pattern"></a>Desenli Git depoları

Git depolarını desenli olarak ayarlamak için kullanılan tüm yapılandırılabilir özellikler aşağıda listelenmiştir.

> [!NOTE]
> Sözcükleri ayırmak için tire (-) kullanmak, şu anda desteklenen tek adlandırma kuralıdır. Örneğin, *varsayılan etiket*değil, *varsayılan Etiket*kullanın.

| Özellik                           | Gerekli         | Özellik                                                      |
| :--------------------------------- | ---------------- | ------------------------------------------------------------ |
| `repos`                            | Hayır             | Belirli bir ada sahip git deposunun ayarlarından oluşan bir harita. |
| `repos."uri"`                      | Evet üzerinde`repos` | Config Server arka uç olarak kullanılan Git deposunun URI *http://*, *https://*, *git@*, veya *ssh://* ile başlatılmalıdır. |
| `repos."name"`                     | Evet üzerinde`repos` | Git deposunda tanımlanmamı _gereken,_ yalnızca `repos` varsa gerekli olan bir ad. Örneğin, *takım-A*, *takım-B*. |
| `repos."pattern"`                  | Hayır             | Uygulama adı eşleştirmek için kullanılan dizeler dizisi. Her desen için, `{application}/{profile}` joker karakterlerle biçimi kullanın. |
| `repos."default-label"`            | Hayır             | Git deposunun varsayılan etiketi, *deponun şube adı,* *etiket adı*veya *commit-id* olmalıdır. |
| `repos."search-paths`"             | Hayır             | Git deposunun alt dizinlerini aramak için kullanılan bir dizi dize. |
| `repos."username"`                 | Hayır             | Git deposu sunucusuna erişmek için kullanılan kullanıcı adı, Git deposu sunucusu desteklediğinde `Http Basic Authentication` _gereklidir._ |
| `repos."password"`                 | Hayır             | Git deposu sunucusuna erişmek için kullanılan parola, Git deposu sunucusu desteklediğinde `Http Basic Authentication` _gereklidir._ |
| `repos."private-key"`              | Hayır             | URI *git@* veya *ssh://* ile başladığında _gerekli_ Git deposuna erişmek için SSH özel anahtarı. |
| `repos."host-key"`                 | Hayır             | Git deposu sunucusunun ana bilgisayar anahtarı, algoritma önekinin kapsadığı `host-key-algorithm`gibi içermemelidir. |
| `repos."host-key-algorithm"`       | Hayır             | Ana anahtar algoritması, *ssh-dss*olmalıdır , *ssh-rsa*, *ecdsa-sha2-nistp256*, *ecdsa-sha2-nistp384*, veya *ecdsa-sha2-nistp521*. *Yalnızca* `host-key` varsa gereklidir. |
| `repos."strict-host-key-checking"` | Hayır             | Config Server örneğinin özel `host-key`sunucudan yararlanArak başlatılıp başlatılmayacağını gösterir. *Doğru* (varsayılan değer) veya *yanlış*olmalıdır. |

## <a name="attach-your-config-server-repository-to-azure-spring-cloud"></a>Config Server deponuzu Azure Bahar Bulutu'na takın

Yapılandırma dosyalarınız bir depoya kaydedildigine göre, Azure Bahar Bulutu'nu buna bağlamanız gerekir.

1. [Azure portalında](https://portal.azure.com)oturum açın.

1. Azure Bahar Bulutuna **Genel Bakış** sayfanıza gidin.

1. Yapılandırmak için hizmeti seçin.

1. Hizmet sayfasının sol bölmesinde, **Ayarlar** **altında, Config Server** sekmesini seçin.

![Config Server penceresi](media/spring-cloud-tutorial-config-server/portal-config-server.png)

### <a name="enter-repository-information-directly-to-the-azure-portal"></a>Depo bilgilerini doğrudan Azure portalına girin

#### <a name="default-repository"></a>Varsayılan depo

* **Genel depo**: Varsayılan **depo** bölümünde, **Uri** kutusuna, depo URI yapıştırın.  **Etiketi** **config**olarak ayarlayın. **Kimlik Doğrulama** ayarının **Herkese Açık**olduğundan emin olun ve ardından bitirmek için **Uygula'yı** seçin. 

* **Özel depo**: Azure Yay Bulutu temel parola/belirteç tabanlı kimlik doğrulamasını ve SSH'yi destekler.

    * **Temel Kimlik Doğrulama**: **Varsayılan depo** bölümünde, **Uri** kutusunda, depo URI'yi yapıştırın ve ardından **Kimlik Doğrulama** ("kalem" simgesi) düğmesini seçin. Kimlik **Doğrulamayı Edin** bölmesinde, **Kimlik Doğrulama türü** açılır listesinde HTTP **Basic'i**seçin ve ardından Azure Bahar Bulutu'na erişim sağlamak için kullanıcı adınızı ve parolanızı/belirteçlerinizi girin. **Tamam'ı**seçin ve config Server örneğini ayarlamayı bitirmek için **Uygula'yı** seçin.

    ![Kimlik Doğrulama bölmesini Edit](media/spring-cloud-tutorial-config-server/basic-auth.png)
    
    > [!CAUTION]
    > GitHub gibi bazı Git depo sunucuları, **Temel Kimlik Doğrulama**için kişisel bir *belirteç* veya parola gibi bir *erişim belirteci*kullanır. Bu tür bir belirteci Azure Bahar Bulutu'nda parola olarak kullanabilirsiniz, çünkü bu belirteçlerin süresi asla dolmaz. Ancak Bitbucket ve Azure DevOps gibi diğer Git depo sunucuları için *erişim belirteci* bir veya iki saat içinde sona erer. Bu, Azure Bahar Bulutu ile bu depo sunucularını kullandığınızda seçeneğin geçerli olmadığı anlamına gelir.

    * **SSH**: **Varsayılan depo** bölümünde, **Uri** kutusunda, depo URI'yi yapıştırın ve ardından **Kimlik Doğrulama** ("kalem" simgesi) düğmesini seçin. Kimlik **Doğrulamayı Edit** bölmesinde, **Kimlik Doğrulama türü** açılır listesinde **SSH'yi**seçin ve ardından Özel **anahtarınızı**girin. İsteğe bağlı olarak, **Ana Bilgisayar anahtarı** ve Ana Bilgisayar anahtar **algoritmanızı belirtin.** Ortak anahtarınızı Config Server deponuza eklediğinden emin olun. **Tamam'ı**seçin ve config Server örneğini ayarlamayı bitirmek için **Uygula'yı** seçin.

    ![Kimlik Doğrulama bölmesini Edit](media/spring-cloud-tutorial-config-server/ssh-auth.png)

#### <a name="pattern-repository"></a>Desen deposu

Hizmetinizi yapılandırmak için isteğe bağlı bir **Desen deposu** kullanmak istiyorsanız, **URI** ve **Kimlik Doğrulama'yı** **Varsayılan depoyla**aynı şekilde belirtin. Desenin için bir **Ad** eklediğinden emin olun ve ardından örneğinize eklemek için **Uygula'yı** seçin. 

### <a name="enter-repository-information-into-a-yaml-file"></a>Depo bilgilerini BIR YAML dosyasına girin

Depo ayarlarınızla birlikte bir YAML dosyası yazdıysanız, dosyayı doğrudan yerel makinenizden Azure Yay Bulutu'na aktarabilirsiniz. Temel kimlik doğrulaması ile özel bir depo için basit bir YAML dosyası şu gibi görünür:

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

Ayarlar **Ala'yı** seçin ve proje dizininizden YAML dosyasını seçin. **İçe**Aktar'ı `async` seçin ve ardından **Bildirimlerinizden** bir işlem açılır. 1-2 dakika sonra, başarı yı bildirmelidir.

![Config Server Bildirimleri bölmesi](media/spring-cloud-tutorial-config-server/local-yml-success.png)


YAML dosyanızdaki bilgiler Azure portalında görüntülenmelidir. Bitirmek için **Uygula'yı** seçin. 


## <a name="delete-your-app-configuration"></a>Uygulama yapılandırmanızı silme

Yapılandırma dosyasını kaydettikten sonra, **Yapılandırma** sekmesinde **Uygulama yapılandırmasını sil** düğmesi görüntülenir. Config Server örneğini GitHub'dan Azure DevOps'lere geçmek gibi başka bir kaynağa bağlamak istiyorsanız bu örneği seçmelisiniz.



## <a name="next-steps"></a>Sonraki adımlar

Bu eğitimde, Spring Cloud Config Server örneğini nasıl etkinleştirecek ve yapılandırılabiliyorsunuz. Uygulamanızı yönetme hakkında daha fazla bilgi edinmek için, uygulamanızı el ile ölçekleme yle ilgili öğreticiye devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Azure Bahar Bulutu'nda bir uygulamayı ölçeklendirin](spring-cloud-tutorial-scale-manual.md)
