---
title: 'Öğretici: Maverics Identity Orchestrator SAML Bağlayıcısı ile çoklu oturum açma (SSO) Azure Active Directory tümleştirme | Microsoft Docs'
description: Azure Active Directory ve Maverics Identity Orchestrator SAML Bağlayıcısı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
author: jeevansd
manager: CelesteDG
ms.reviewer: celested
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.topic: tutorial
ms.date: 08/12/2020
ms.author: jeedes
ms.openlocfilehash: 116ee7c8db3070a667c21a052bec739fd397a2dd
ms.sourcegitcommit: ac7ae29773faaa6b1f7836868565517cd48561b2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/25/2020
ms.locfileid: "88825571"
---
# <a name="tutorial-integrate-azure-ad-single-sign-on-with-maverics-identity-orchestrator-saml-connector"></a>Öğretici: Azure AD çoklu oturum açmayı Maverics Identity Orchestrator SAML Bağlayıcısı ile tümleştirme

Strata, kimlik doğrulama ve erişim denetimi için şirket içi uygulamaları Azure Active Directory (Azure AD) ile tümleştirmenin basit bir yolunu sunar.

Bu makalede, Maverics kimlik Orchestrator 'ın nasıl yapılandırılacağı anlatılmaktadır:
* Şirket içi bir kimlik sisteminden, eski şirket içi bir uygulamada oturum açma sırasında Kullanıcı aracılığıyla Azure AD 'ye artımlı olarak geçiş yapın.
* CA Sitedefteri veya Oracle Erişim Yöneticisi gibi eski bir Web erişimi yönetim ürününden oturum açma isteklerini Azure AD 'ye yönlendirin.
* Kullanıcının kimlik doğrulamasını Azure AD 'ye göre doğruladıktan sonra HTTP üstbilgileri veya özel oturum tanımlama bilgileri kullanılarak korunan şirket içi uygulamalarda kullanıcıların kimliğini doğrulayın.

Strata, şirket içinde veya bulutta dağıtabileceğiniz yazılımlar sağlar. Karma ve çok kiracılı kuruluşlar için dağıtılmış kimlik yönetimi oluşturmak üzere kimlik sağlayıcılarını keşfetmenizi, bağlanmanızı ve düzenlemenizi sağlar.

Bu öğreticide, kimlik doğrulama ve erişim denetimi için Azure AD kullanmak üzere eski bir Web erişim yönetimi ürünü (CA Sitedefteri) tarafından korunan şirket içi bir Web uygulamasının nasıl geçirileceği gösterilmektedir. Temel adımlar şunlardır:
1. Maverics Identity Orchestrator 'ı yükler.
2. Kurumsal uygulamanızı Azure AD 'ye kaydedin ve SAML tabanlı çoklu oturum açma (SSO) için Maverics Azure AD SAML sıfır kod bağlayıcısını kullanacak şekilde yapılandırın.
3. Maverics 'Yi Sitedefteri ve Basit Dizin Erişim Protokolü (LDAP) Kullanıcı deposu ile tümleştirin.
4. Azure Anahtar Kasası kurun ve Maverics 'yı gizli dizi yönetim sağlayıcısı olarak kullanmak üzere yapılandırın.
5. Şirket içi bir Java Web uygulamasına erişim sağlamak için Maverics kullanarak Kullanıcı geçişi ve oturum soyutlama gösterir.

Ek yükleme ve yapılandırma yönergeleri için [Strata Web sitesine](https://strata.io/docs)gidin.

## <a name="prerequisites"></a>Önkoşullar

- Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
- Maverics Identity Orchestrator SAML Bağlayıcısı SSO özellikli aboneliği. Maverics yazılımını edinmek için [Strata satışları](mailto:sales@strata.io)ile görüşün.

## <a name="install-maverics-identity-orchestrator"></a>Maverics kimlik Orchestrator 'ı yükler

Maverics kimlik Orchestrator yüklemesini kullanmaya başlamak için bkz. [yükleme yönergeleri](https://strata.io/docs).

### <a name="system-requirements"></a>Sistem gereksinimleri
* Desteklenen işletim sistemleri
  * RHEL 7+
  * CentOS 7+

* Bağımlılıklar
  * Systemd

### <a name="installation"></a>Yükleme

1. En son Maverics RedHat Paket Yöneticisi (RPM) paketini edinin. Paketi, Maverics yazılımını yüklemek istediğiniz sisteme kopyalayın.

2. Maverics paketini yükleyerek dosya adınızı yerine koyun `maverics.rpm` .

    `sudo rpm -Uvf maverics.rpm`

3. Maverics 'yı yükledikten sonra, altında bir hizmet olarak çalışacaktır `systemd` . Hizmetin çalıştığını doğrulamak için aşağıdaki komutu yürütün:

    `sudo systemctl status maverics`

Varsayılan olarak, Maverics */usr/local/bin* dizinine yüklenir.

Maverics yükledikten sonra, */etc/Maverics* dizininde varsayılan *Maverics. YAML* dosyası oluşturulur. Ve dahil olmak üzere yapılandırmanızı düzenlemeden önce `workflows` `connectors` yapılandırma dosyanız şöyle görünür:

```yaml
# © Strata Identity Inc. 2020. All Rights Reserved. Patents Pending.

version: 0.1
listenAddress: ":7474"
```
## <a name="configuration-options"></a>Yapılandırma seçenekleri
### <a name="version"></a>Sürüm
`version`Bu alan, yapılandırma dosyasının hangi sürümünün kullanıldığını bildirir. Sürüm belirtilmemişse, en son yapılandırma sürümü kullanılacaktır.

```yaml
version: 0.1
```
### <a name="listenaddress"></a>listenAddress
`listenAddress` hangi adres Orchestrator 'ın dinleneceğini bildirir. Adresin ana bilgisayar bölümü boşsa, Orchestrator tüm kullanılabilir tek noktaya yayın ve yerel sistemin IP adreslerini dinler. Adresin bağlantı noktası bölümü boşsa, bir bağlantı noktası numarası otomatik olarak seçilir.

```yaml
listenAddress: ":453"
```
### <a name="tls"></a>TLS

`tls`Alan, Aktarım Katmanı Güvenliği (TLS) nesnelerinin haritasını bildirir. TLS nesneleri bağlayıcılar ve Orchestrator sunucusu tarafından kullanılabilir. Tüm kullanılabilir TLS seçenekleri için `transport` paket belgelerine bakın.

Microsoft Azure SAML tabanlı SSO kullandığınızda TLS üzerinden iletişim gerektirir. Sertifika oluşturma hakkında daha fazla bilgi için bkz. [şifreme Web sitesine](https://letsencrypt.org/getting-started/)gidin.

`maverics`Anahtar, Orchestrator sunucusu için ayrılmıştır. Diğer tüm anahtarlar kullanılabilir ve belirli bir bağlayıcıya bir TLS nesnesi eklemek için kullanılabilir.

```yaml
tls:
  maverics:
    certFile: /etc/maverics/maverics.cert
    keyFile: /etc/maverics/maverics.key
```  
### <a name="include-files"></a>Dosyaları dahil etme

`connectors` `workflows` Aşağıdaki örneğe göre kendi kendi kendine ayrı yapılandırma dosyalarını tanımlayabilir ve bunları kullanarak *Maverics. YAML* dosyasında başvurabilirsiniz `includeFiles` :

```yaml
includeFiles:
  - workflow/sessionAbstraction.yaml
  - connector/AzureAD-saml.yaml
  - connector/siteminder.yaml
  ```

Bu öğreticide tek bir *Maverics. YAML* yapılandırma dosyası kullanılmaktadır.

## <a name="use-azure-key-vault-as-your-secrets-provider"></a>Gizli dizi sağlayıcınız olarak Azure Key Vault kullanın

### <a name="manage-secrets"></a>Gizli dizileri yönetme

Gizli dizileri yüklemek için Maverics çeşitli gizli yönetim çözümleriyle tümleştirilebilir. Geçerli tümleştirmeler bir dosya, HashiCorp kasası ve Azure Key Vault içerir. Gizli bir yönetim çözümü belirtilmemişse, Maverics varsayılan değerleri *Maverics. YAML* dosyasının dışında düz metin olarak yüklemeyi sağlar.

Bir *Maverics. YAML* yapılandırma dosyasında bir değeri gizli olarak bildirmek için, parolayı açılı ayraç içine alın:

  ```yaml
  connectors:
  - name: AzureAD
    type: AzureAD
    apiToken: <AzureADAPIToken>
    oauthClientID: <AzureADOAuthClientID>
    oauthClientSecret: <AzureADOAuthClientSecret>
  ```

### <a name="load-secrets-from-a-file"></a>Bir dosyadan gizli dizileri yükle

1. Bir dosyadan gizli dizileri yüklemek için, şunu kullanarak ortam değişkenini `MAVERICS_SECRET_PROVIDER` */etc/Maverics/Maverics.env* dosyasına ekleyin:

   `MAVERICS_SECRET_PROVIDER=secretfile:///<PATH TO SECRETS FILE>`

2. Şunu çalıştırarak Maverics hizmetini yeniden başlatın:

   `sudo systemctl restart maverics`

*Gizli dizileri. YAML* dosya içerikleri herhangi bir sayıda olabilir `secrets` .

```yaml
secrets:
  AzureADAPIToken: aReallyGoodToken
  AzureADOAuthClientID: aReallyUniqueID
  AzureADOAuthClientSecret: aReallyGoodSecret
```
### <a name="set-up-an-azure-key-vault"></a>Azure Anahtar Kasası ayarlama

Azure portal veya Azure CLı kullanarak bir Azure Anahtar Kasası ayarlayabilirsiniz.

**Azure portalı kullanma**
1. [Azure portalında](https://portal.azure.com) oturum açın.
1. [Yeni bir Anahtar Kasası oluşturun](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#create-a-vault).
1. [Gizli dizileri anahtar kasasına ekleyin](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault).
1. [Bir uygulamayı Azure AD 'ye kaydedin](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application).
1. [Bir uygulamayı gizli anahtar kullanması Için yetkilendirin](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault).

**Azure CLI kullanma**

1. [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)'yi açın ve aşağıdaki komutu girin:

    ```shell
    az login
    ```

1. Aşağıdaki komutu çalıştırarak yeni bir Anahtar Kasası oluşturun:
    ```shell
    az keyvault create --name "[VAULT_NAME]" --resource-group "[RESOURCE_GROUP]" --location "[REGION]"
    ```

1. Aşağıdaki komutu çalıştırarak gizli dizileri anahtar kasasına ekleyin:
    ```shell
    az keyvault secret set --vault-name "[VAULT_NAME]" --name "[SECRET_NAME]" --value "[SECRET_VALUE]"
    ```

1. Aşağıdaki komutu çalıştırarak bir uygulamayı Azure AD 'ye kaydedin:
    ```shell
    az ad sp create-for-rbac -n "MavericsKeyVault" --skip-assignment > azure-credentials.json
    ```

1. Aşağıdaki komutu çalıştırarak bir uygulamayı gizli dizi kullanacak şekilde yetkilendirin:
    ```shell
    az keyvault set-policy --name "[VAULT_NAME]" --spn [APPID] --secret-permissions list get
    #APPID can be found in the azure-credentials.json
    generated in the previous step
    ```

1. Azure anahtar kasaınızdan gizli dizileri yüklemek için,/etc/Maverics/Maverics.env dosyasındaki ortam değişkenini `MAVERICS_SECRET_PROVIDER` aşağıdaki biçimde */etc/maverics/maverics.env* *azure-credentials.js* dosyasında bulunan kimlik bilgilerini kullanarak ayarlayın:
 
   `MAVERICS_SECRET_PROVIDER='azurekeyvault://<KEYVAULT NAME>.vault.azure.net?clientID=<APPID>&clientSecret=<PASSWORD>&tenantID=<TENANT>'`

1. Maverics hizmetini yeniden başlatın: `sudo systemctl restart maverics`

## <a name="configure-your-application-in-azure-ad-for-saml-based-sso"></a>Azure AD 'de SAML tabanlı SSO için uygulamanızı yapılandırma

1. Azure AD kiracınızda **Kurumsal uygulamalar**' a gidin, **Maverics kimlik Orchestrator SAML bağlayıcısını**arayın ve ardından seçin.

1. Uygulamanın yeni geçirilmiş kullanıcılar için çalışmasını sağlamak için, Maverics Identity Orchestrator SAML Bağlayıcısı **Özellikler** bölmesinde, **gereken Kullanıcı atamasını** **Hayır** olarak ayarlayın.

1. Maverics kimlik Orchestrator SAML bağlayıcısına **genel bakış** bölmesinde, **Tekli oturum açmayı ayarla**' yı seçin ve ardından **SAML**' yi seçin.

1. Maverics kimlik Orchestrator SAML Bağlayıcısı **SAML tabanlı oturum açma** bölmesinde, **Düzenle** (kurşun kalem simgesi) düğmesini seçerek **temel SAML yapılandırmasını** düzenleyin.

   !["Temel SAML yapılandırması" düzenleme düğmesinin ekran görüntüsü.](common/edit-urls.png)

1. Aşağıdaki biçimde bir URL yazarak **VARLıK kimliğini** girin: `https://<SUBDOMAIN>.maverics.org` . Varlık KIMLIĞI, Kiracıdaki uygulamalar arasında benzersiz olmalıdır. Maverics yapılandırmasına dahil etmek için buraya girilen değeri kaydedin.

1. **Yanıt URL** 'sini şu biçimde girin: `https://<AZURECOMPANY.COM>/<MY_APP>/` . 

1. **Oturum açma URL 'sini** şu biçimde girin: `https://<AZURE-COMPANY.COM>/<MY_APP>/<LOGIN PAGE>` . 

1. **Kaydet**’i seçin.

1. **SAML Imzalama sertifikası** bölümünde, **uygulama Federasyon meta verileri URL 'Sini**kopyalamak için **Kopyala** düğmesini seçin ve bilgisayarınıza kaydedin.

    !["SAML Imzalama sertifikası" kopyalama düğmesinin ekran görüntüsü.](common/copy-metadataurl.png)

## <a name="configure-maverics-identity-orchestrator-azure-ad-saml-connector"></a>Maverics kimlik Orchestrator Azure AD SAML bağlayıcısını yapılandırma

Maverics kimlik Orchestrator Azure AD Bağlayıcısı, OpenID Connect ve SAML Connect 'i destekler. Bağlayıcıyı yapılandırmak için aşağıdakileri yapın: 

1. SAML tabanlı SSO 'yu etkinleştirmek için, ayarlayın `authType: saml` .

1. Değerini `samlMetadataURL` aşağıdaki biçimde oluşturun: `samlMetadataURL:https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>` .

1. Kullanıcılar Azure kimlik bilgileriyle oturum açtıktan sonra, Azure 'un uygulamanızda yeniden yönlendirileceği URL 'YI tanımlayın. Şu biçimi kullanın: `samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>` .

1. Değeri, daha önce yapılandırılan EntityId 'den kopyalayın: `samlEntityID: https://<SUBDOMAIN>.maverics.org` .

1. SAML yanıtını göndermek için Azure AD 'nin kullanacağı yanıt URL 'sinden değeri kopyalayın: `samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>` .

1. [OpenSSL aracını](https://www.openssl.org/source/)kullanarak Maverics kimlik Orchestrator oturum bilgilerini korumak için kullanılan bir JSON Web token (JWT) imzalama anahtarı oluşturun:

    ```shell 
    openssl rand 64 | base64
    ```
1. Yanıtı `jwtSigningKey` yapılandırma özelliğine kopyalayın: `jwtSigningKey: TBHPvTtu6NUqU84H3Q45grcv9WDJLHgTioqRhB8QGiVzghKlu1mHgP1QHVTAZZjzLlTBmQwgsSoWxGHRcT4Bcw==` .

## <a name="attributes-and-attribute-mapping"></a>Öznitelikler ve öznitelik eşleme
Öznitelik eşleme, Kullanıcı oluşturulduktan sonra bir kaynak şirket içi Kullanıcı dizininden bir Azure AD kiracısına kullanıcı özniteliklerinin eşlemesini tanımlamak için kullanılır.

Öznitelikler, bir talepteki uygulamaya hangi kullanıcı verilerinin döndürüleceğini, oturum tanımlama bilgilerine geçirilebileceğini veya HTTP üst bilgi değişkenlerinde uygulamaya geçirilmesini sağlar.

## <a name="configure-the-maverics-identity-orchestrator-azure-ad-saml-connector-yaml-file"></a>Maverics kimlik Orchestrator Azure AD SAML Bağlayıcısı YAML dosyasını yapılandırma

Maverics kimlik Orchestrator Azure AD Bağlayıcısı yapılandırmanız şuna benzeyecektir:

```yaml
- name: AzureAD
  type: azure
  authType: saml
  samlMetadataURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>
  samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>
  samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>
  jwtSigningKey: <SIGNING KEY>
  samlEntityID: https://<SUBDOMAIN>.maverics.org
  attributeMapping:
    displayName: username
    mailNickname: givenName
    givenName: givenName
    surname: sn
    userPrincipalName: mail
    password: password
```

## <a name="migrate-users-to-an-azure-ad-tenant"></a>Kullanıcıları bir Azure AD kiracısına geçirme

Kullanıcıları CA Sitedefteri, Oracle Access Manager veya IBM Tivoli gibi bir Web erişimi yönetim ürününden artımlı olarak geçirmek için bu yapılandırmayı izleyin. Ayrıca, bunları bir Basit Dizin Erişim Protokolü (LDAP) dizininden veya bir SQL veritabanından geçirebilirsiniz.

### <a name="configure-your-application-permissions-in-azure-ad-to-create-users"></a>Kullanıcı oluşturmak için Azure AD 'de uygulama izinlerinizi yapılandırma

1. Azure AD kiracınızda adresine gidin `App registrations` ve **Maverics ıDENTITY Orchestrator SAML bağlayıcı** uygulamasını seçin.

1. **Maverics Identity Orchestrator SAML Bağlayıcısı | Sertifikalar & gizlilikler** Bölmesi ' ni seçin `New client secret` ve ardından süre sonu seçeneğini belirleyin. Gizli dizi kopyalamak için **Kopyala** düğmesini seçin ve bilgisayarınıza kaydedin.

1. **Maverics Identity Orchestrator SAML Bağlayıcısı | API izinleri** bölmesi, **izin Ekle** ' yi SEÇIN ve ardından **apı izinleri ıste** bölmesinde **Microsoft Graph** ve **Uygulama izinleri**' ni seçin. 

1. Sonraki ekranda **User. ReadWrite. All**' ı seçin ve ardından **izin Ekle**' yi seçin. 

1. **API izinleri** bölmesine dönün, **yönetici izni ver**' i seçin.

### <a name="configure-the-maverics-identity-orchestrator-saml-connector-yaml-file-for-user-migration"></a>Maverics Identity Orchestrator SAML Bağlayıcısı YAML dosyasını Kullanıcı geçişi için yapılandırma

Kullanıcı geçişi iş akışını etkinleştirmek için bu ek özellikleri yapılandırma dosyasına ekleyin:
1. **Azure Graph URL 'sini** şu biçimde girin: `graphURL: https://graph.microsoft.com` .
1. **OAuth belirteci URL 'sini** şu biçimde girin: `oauthTokenURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>` .
1. Önceden oluşturulan istemci parolasını şu biçimde girin: `oauthClientSecret: <CLIENT SECRET>` .


Son Maverics kimlik Orchestrator Azure AD Bağlayıcısı yapılandırma dosyanız şuna benzeyecektir:

```yaml
- name: AzureAD
  type: azure
  authType: saml
  samlMetadataURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>
  samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>
  samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>
  jwtSigningKey: TBHPvTtu6NUqU84H3Q45grcv9WDJLHgTioqRhB8QGiVzghKlu1mHgP1QHVTAZZjzLlTBmQwgsSoWxGHRcT4Bcw==
  samlEntityID: https://<SUBDOMAIN>.maverics.org
  graphURL: https://graph.microsoft.com
  oauthTokenURL: https://login.microsoftonline.com/<TENANT ID>/oauth2/v2.0/token
  oauthClientID: <APP ID>
  oauthClientSecret: <NEW CLIENT SECRET>
  attributeMapping:
    displayName: username
    mailNickname: givenName
    givenName: givenName
    surname: sn
    userPrincipalName: mail
    password: password
```

### <a name="configure-maverics-zero-code-connector-for-siteminder"></a>Site, Sitedefteri için Maverics sıfır kod bağlayıcısını yapılandırma

Kullanıcıları bir Azure AD kiracısına geçirmek için Sitedefteri bağlayıcısını kullanın. Yeni oluşturulan Azure AD kimliklerini ve kimlik bilgilerini kullanarak, içindeki kullanıcıları Sitedefteri tarafından korunan eski şirket içi uygulamalarda günlüğe kaydedin.

Bu öğreticide, Sitedefteri, form tabanlı kimlik doğrulaması ve tanımlama bilgisini kullanarak eski uygulamayı korumak üzere yapılandırılmıştır `SMSESSION` . HTTP üstbilgileri aracılığıyla kimlik doğrulaması ve oturum bilgilerini tüketen bir uygulamayla tümleşmek için, üst bilgi öykünme yapılandırmasını bağlayıcıya eklemeniz gerekir.

Bu örnekte, `username` özniteliği `SM_USER` http üstbilgisiyle eşlenir:

```yaml
  headers:
    SM_USER: username
```

`proxyPass`İsteklerin proxy olarak ayarlandığı konuma ayarlayın. Genellikle, bu konum korunan uygulamanın ana bilgisayarı olur.

`loginPage` kullanıcıları kimlik doğrulaması için yeniden yönlendirirse Sitedefteri tarafından şu anda kullanılan oturum açma formunun URL 'siyle eşleşmelidir.

```yaml
connectors:
- name: siteminder-login-form
  type: siteminder
  loginType: form
  loginPage: /siteminderagent/forms/login.fcc
  proxyPass: http://host.company.com
```

### <a name="configure-maverics-zero-code-connector-for-ldap"></a>LDAP için Maverics sıfır kod bağlayıcısını yapılandırma

Uygulamalar Sitedefteri gibi bir Web erişim yönetimi (WAM) ürünü tarafından korunduğunda, Kullanıcı kimlikleri ve öznitelikleri genellikle bir LDAP dizininde depolanır.

Bu bağlayıcı yapılandırması, LDAP dizinine bağlanmayı gösterir. Bağlayıcı, geçiş iş akışı sırasında doğru Kullanıcı profili bilgilerinin toplanabilmesi ve Azure AD 'de buna karşılık gelen bir kullanıcının oluşturulabilmesi için Sitedefteri için Kullanıcı Mağazası olarak yapılandırılır.

* `baseDN` dizinde LDAP aramasının gerçekleştirileceği konumu belirtir.

* `url` , bağlanılacak LDAP sunucusunun adresi ve bağlantı noktasıdır.

* `serviceAccountUsername` , genellikle bir BIND DN (örneğin,) olarak ifade edilen LDAP sunucusuna bağlanmak için kullanılan kullanıcı adıdır `CN=Directory Manager` .

* `serviceAccountPassword` , LDAP sunucusuna bağlanmak için kullanılan paroladır. Bu değer, daha önce yapılandırılmış Azure Anahtar Kasası örneğinde depolanır.  

* `userAttributes` sorgusuna yönelik kullanıcı ile ilgili özniteliklerin listesini tanımlar. Bu öznitelikler daha sonra karşılık gelen Azure AD özniteliklerine eşlenir.

```yaml
- name: company-ldap
  type: ldap
  url: "ldap://ldap.company.com:389"
  baseDN: ou=People,o=company,c=US
  serviceAccountUsername: uid=admin,ou=Admins,o=company,c=US
  serviceAccountPassword: <vaulted-password>
  userAttributes:
    - uid
    - cn
    - givenName
    - sn
    - mail
    - mobile
```

### <a name="configure-the-migration-workflow"></a>Geçiş iş akışını yapılandırma

Geçiş iş akışı yapılandırması, Maverics kullanıcıları Sitedefteri veya LDAP 'den Azure AD 'ye nasıl geçirdiğini belirler.

Bu iş akışı:
- Sitedefteri bağlayıcısını kullanarak Sitebir oturum açma kimliğini ara. Kullanıcı kimlik bilgileri, Sitedefteri kimlik doğrulaması ile onaylanır ve sonra iş akışının sonraki adımlarına geçirilir.
- Sitedefteri kullanıcı deposundan Kullanıcı profili özniteliklerini alır.
- Azure AD kiracınızda Kullanıcı oluşturmak için Microsoft Graph API 'sine bir istek yapar.

Geçiş iş akışını yapılandırmak için aşağıdakileri yapın:

1. İş akışına bir ad verin (örneğin, **Sitedefteri 'Ni Azure AD geçişine**).
1. İş akışının `endpoint` açığa alındığı, `actions` Bu iş akışının isteklere yanıt olarak tetikleneceği bir http yolu olan öğesini belirtin. `endpoint`Genellikle, proxy kullanan uygulamaya karşılık gelir (örneğin, `/my_app` ). Değer hem baştaki hem de sondaki eğik çizgileri içermelidir.
1. `actions`İş akışına uygun şekilde ekleyin.

   a. `login`Sitedefteri Bağlayıcısı için yöntemi tanımlayın. Bağlayıcı değeri, bağlayıcı yapılandırmasındaki ad değeriyle eşleşmelidir.

   b. `getprofile`LDAP bağlayıcısının yöntemini tanımlayın.

   c.  `createuser`AzureAD Connector için yöntemi tanımlayın.

    ```yaml
      workflows:
      - name: SiteMinder to Azure AD Migration
        endpoint: /my_app/
        actions:
        - connector: siteminder-login-form
          method: login
        - connector: company-ldap
          method: getprofile
        - connector: AzureAD
          method: createuser
    ```
### <a name="verify-the-migration-workflow"></a>Geçiş iş akışını doğrulama

1. Maverics hizmeti zaten çalışmıyorsa, aşağıdaki komutu yürüterek başlatın: 

   `sudo systemctl start maverics`

1. Proxy kullanan oturum açma URL 'sine gidin `http://host.company.com/my_app` .
1. Uygulamada oturum açmak için kullanılan Kullanıcı kimlik bilgilerini, Sitedefteri tarafından korunurken sağlayın.
4. **Ev**  >  **kullanıcılarına git | **Kullanıcının Azure AD kiracısında oluşturulduğunu doğrulamak için tüm kullanıcılar.  

### <a name="configure-the-session-abstraction-workflow"></a>Oturum soyutlama iş akışını yapılandırma

Oturum soyutlama iş akışı, eski şirket içi Web uygulaması için kimlik doğrulama ve erişim denetimini Azure AD kiracısına taşıdır.

Azure Bağlayıcısı, `login` bir oturumun mevcut olmadığı varsayılarak kullanıcıyı oturum açma URL 'sine yeniden yönlendirmek için yöntemini kullanır.

Kimliği doğrulandıktan sonra, sonuç olarak oluşturulan oturum belirteci Maverics 'a geçirilir. Sitedefteri bağlayıcısının yöntemi, `emulate` tanımlama bilgisi tabanlı oturuma veya üst bilgi tabanlı oturuma öykünmek için kullanılır ve sonra, isteği uygulamanın gerektirdiği ek özniteliklerle süsleyerek.

1. İş akışına bir ad verin (örneğin, **Sitedefteri oturum soyutlama**).
1. Proxy olan `endpoint` uygulamaya karşılık gelen öğesini belirtin. Değer hem baştaki hem de sondaki eğik çizgi içermelidir (örneğin, `/my_app/` ).
1. `actions`İş akışına uygun şekilde ekleyin.

   a. `login`Azure Bağlayıcısı için yöntemi tanımlayın. `connector`Değerin `name` bağlayıcı yapılandırmasındaki değerle eşleşmesi gerekir.

   b. `emulate`Sitedefteri Bağlayıcısı için yöntemi tanımlayın.

     ```yaml
      - name: SiteMinder Session Abstraction
        endpoint: /my_app/
        actions:
      - connector: azure
        method: login
      - connector: siteminder-login-form
        method: emulate
     ```
### <a name="verify-the-session-abstraction-workflow"></a>Oturum soyutlama iş akışını doğrulama

1. Proxy kullanan uygulama URL 'sine gidin `https://<AZURECOMPANY.COM>/<MY_APP>` . 
    
    Proxy oturum açma sayfasına yönlendirilirsiniz.

1. Azure AD Kullanıcı kimlik bilgilerini girin.

   Doğrudan Sitedefteri tarafından kimliğiniz doğrulanmış olsanız da uygulamaya yönlendirilmelisiniz.
