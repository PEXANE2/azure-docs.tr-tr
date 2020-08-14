---
title: 'Öğretici Azure Active Directory: Maverics Identity Orchestrator SAML Bağlayıcısı ile çoklu oturum açma (SSO) Tümleştirmesi | Microsoft Docs'
description: Azure Active Directory ve Maverics Identity Orchestrator SAML Bağlayıcısı arasında çoklu oturum açmayı nasıl yapılandıracağınızı öğrenin.
services: active-directory
documentationCenter: na
author: jeevansd
manager: mtillman
ms.reviewer: barbkess
ms.assetid: 9cad791f-8746-4584-bf4e-e281b709fb2b
ms.service: active-directory
ms.subservice: saas-app-tutorial
ms.workload: identity
ms.tgt_pltfrm: na
ms.topic: tutorial
ms.date: 08/12/2020
ms.author: jeedes
ms.collection: M365-identity-device-management
ms.openlocfilehash: 4e923e051116b70b2db35d1ac710a40941305aae
ms.sourcegitcommit: 4913da04fd0f3cf7710ec08d0c1867b62c2effe7
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/14/2020
ms.locfileid: "88214337"
---
# <a name="tutorial-azure-active-directory-single-sign-on-sso-integration-with-maverics-identity-orchestrator-saml-connector"></a>Öğretici: Maverics Identity Orchestrator SAML Bağlayıcısı ile çoklu oturum açma (SSO) Tümleştirmesi Azure Active Directory

## <a name="introduction"></a>Giriş

Strata, kimlik doğrulama ve erişim denetimi için şirket içi uygulamaları Azure AD ile tümleştirmeye yönelik basit bir yol sağlar.

Bu kılavuzda, Maverics kimlik Orchestrator 'ın nasıl yapılandırılacağı anlatılmaktadır &trade; :
* Şirket içi bir kimlik sisteminden, eski şirket içi bir uygulamada oturum açma sırasında Kullanıcı aracılığıyla Azure AD 'ye artımlı olarak geçiş yapın.
* CA Sitedefteri veya Oracle Erişim Yöneticisi gibi eski bir Web erişimi yönetim ürününden oturum açma isteklerini Azure AD 'ye yönlendirin.
* Kullanıcının kimlik doğrulamasını Azure AD 'ye göre doğruladıktan sonra http üstbilgileri veya özel oturum tanımlama bilgileri kullanılarak korunan şirket içi uygulamalarda kullanıcıların kimliğini doğrulayın.

Strata, karma ve çok kiracılı kuruluşlar için dağıtılmış kimlik yönetimi oluşturmak üzere kimlik sağlayıcılarını bulma, bağlama ve düzenleme için şirket içinde veya bulutta dağıtan yazılımlar sağlar.

Bu öğreticide, kimlik doğrulama ve erişim denetimi için Azure AD kullanmak üzere eski bir Web erişim yönetimi ürünü (CA Sitedefteri) tarafından şu anda korunan şirket içi bir Web uygulamasının nasıl geçirileceği gösterilmektedir.
1. Maverics kimlik Orchestrator 'ı yükler&trade;
2. Kurumsal uygulamanızı Azure AD 'ye kaydedin ve &trade; SAML tabanlı SSO Için Maverics Azure AD SAML sıfır kod bağlayıcısını kullanacak şekilde yapılandırın.
3. Maverics 'Yi Sitedefteri ve LDAP kullanıcı deposu ile tümleştirin.
4. Azure Key Vault ayarlayın ve bunları gizli dizi yönetim sağlayıcısı olarak kullanmak üzere Maverics yapılandırma.
5. Şirket içi bir Java Web uygulamasına erişim sağlamak için Maverics kullanarak Kullanıcı geçişi ve oturum soyutlama gösterir.

Ek yükleme ve yapılandırma yönergeleri için lütfen şu adresi ziyaret edin https://strata.io/docs

## <a name="prerequisites"></a>Ön koşullar

- Bir Azure AD aboneliği. Aboneliğiniz yoksa [ücretsiz bir hesap](https://azure.microsoft.com/free/)alabilirsiniz.
- Maverics Identity Orchestrator SAML Bağlayıcısı çoklu oturum açma (SSO) etkin aboneliği. Maverics yazılımını almak için lütfen iletişim kurun sales@strata.io

## <a name="install-maverics-identity-orchestratortrade"></a>Maverics kimlik Orchestrator 'ı yükler&trade;

Maverics Identity Orchestrator yüklemesini kullanmaya başlamak için lütfen şu adresten yükleme yönergelerine bakın: https://strata.io/docs

## <a name="system-requirements"></a>Sistem gereksinimleri
### <a name="supported-operating-systems"></a>Desteklenen İşletim Sistemleri
* RHEL 7+
* CentOS 7+

### <a name="dependencies"></a>Bağımlılıklar
* Systemd

## <a name="installation"></a>Yükleme

1. En son Maverics RPM paketini edinin. Paketi, Maverics yazılımını yüklemek istediğiniz sisteme kopyalayın.

2. Dosya dosyanızı yerine, Maverics paketini yükledikten sonra `maverics.rpm` .

    `sudo rpm -Uvf maverics.rpm`

3. Maverics yükledikten sonra, altında bir hizmet olarak çalışacaktır `systemd` . Hizmetin çalıştığını doğrulamak için aşağıdaki komutu yürütün.

    `sudo systemctl status maverics`

Varsayılan olarak, Maverics `/usr/local/bin` dizine yüklenir.

Maverics yükledikten sonra, varsayılan `maverics.yaml` Dosya `/etc/maverics` dizininde oluşturulur. Ve dahil olmak üzere yapılandırmanızı düzenlemeden önce `workflows` `connectors` yapılandırma dosyanız şöyle görünür:

```yaml
# © Strata Identity Inc. 2020. All Rights Reserved. Patents Pending.

version: 0.1
listenAddress: ":7474"
```
## <a name="config-options"></a>Yapılandırma seçenekleri
### <a name="version"></a>Sürüm
`version`Alan, hangi yapılandırma dosyası sürümünün kullanıldığını bildirir. Belirtilmemişse, en son yapılandırma sürümü kullanılacaktır.

```yaml
version: 0.1
```
### <a name="listen-address"></a>Dinleme adresi
`listenAddress` Orchestrator 'ın hangi adreste dinleneceğini bildirir. Adresin ana bilgisayar bölümü boşsa, Orchestrator tüm kullanılabilir tek noktaya yayın ve yerel sistemin IP adreslerini dinler. Adresin bağlantı noktası bölümü boşsa, bir bağlantı noktası numarası otomatik olarak seçilir.

```yaml
listenAddress: ":453"
```
### <a name="tls"></a>TLS

`tls`Alan, aktarım katmanı güvenlik nesnelerinin haritasını bildirir. TLS nesneleri, bağlayıcı ve Orchestrator sunucusu tarafından kullanılabilir. Tüm kullanılabilir TLS seçenekleri için bkz `transport` . paket belgeleri.

Microsoft Azure, SAML tabanlı SSO kullanırken TLS üzerinden iletişim gerektirir, sertifikalarınızı oluşturmak için [burada](https://letsencrypt.org/getting-started/) daha fazla bilgi bulabilirsiniz.

`maverics`Anahtar, Orchestrator sunucusu için ayrılmıştır. Diğer tüm anahtarlar kullanılabilir ve belirli bir bağlayıcıya bir TLS nesnesi eklemek için kullanılabilir.

```yaml
tls:
  maverics:
    certFile: /etc/maverics/maverics.cert
    keyFile: /etc/maverics/maverics.key
```  
### <a name="include-files"></a>İçerme dosyaları

`connectors` ve `workflows` kendi, ayrı yapılandırma dosyalarında tanımlanabilir ve `maverics.yaml` `includeFiles` Aşağıdaki örnek başına kullanılarak başvurulabilir.
```yaml
includeFiles:
  - workflow/sessionAbstraction.yaml
  - connector/AzureAD-saml.yaml
  - connector/siteminder.yaml
  ```

Bu öğreticide tek bir `maverics.yaml` yapılandırma dosyası kullanılmaktadır.

## <a name="using-azure-key-vault-as-your-secrets-provider"></a>Gizli dizi sağlayıcınız olarak Azure Key Vault kullanma

### <a name="secret-management"></a>Gizli dizi yönetimi

Maverics, gizli dizileri yüklemek için çeşitli gizli yönetim çözümleriyle tümleştirme özelliğine sahiptir. Geçerli tümleştirmeler bir dosya, HashiCorp kasası ve Azure Key Vault içerir. Gizli bir yönetim çözümü belirtilmemişse Maverics varsayılan olarak gizli olmayan gizli dizileri yüklemeyi sağlar `maverics.yaml` .
Bir değeri bir yapılandırma dosyasında gizli olarak bildirmek için `maverics.yaml` , parolayı açılı parantezle sarın:

  ```yaml
  connectors:
  - name: AzureAD
    type: AzureAD
    apiToken: <AzureADAPIToken>
    oauthClientID: <AzureADOAuthClientID>
    oauthClientSecret: <AzureADOAuthClientSecret>
  ```

### <a name="file"></a>Dosya

Bir dosyadan gizli dizileri yüklemek için, ortam değişkenini dosyasına şu `MAVERICS_SECRET_PROVIDER`  `/etc/maverics/maverics.env` şekilde ekleyin:

`MAVERICS_SECRET_PROVIDER=secretfile:///<PATH TO SECRETS FILE>`

Ardından Maverics hizmetini yeniden başlatın: `sudo systemctl restart maverics`

`secrets.yaml`Dosya içerikleri herhangi bir sayıda olabilir `secrets` .
```yaml
secrets:
  AzureADAPIToken: aReallyGoodToken
  AzureADOAuthClientID: aReallyUniqueID
  AzureADOAuthClientSecret: aReallyGoodSecret
```
### <a name="azure-key-vault"></a>Azure Key Vault

Aşağıdaki adımlarda, [Azure Portal](https://portal.azure.com) kullanılarak veya [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)kullanılarak Azure Key Vault nasıl ayarlanacağı gösterilmektedir:

1. Azure portal kullanarak veya CLı komutunu kullanarak [oturum açın](https://portal.azure.com) :
    ```shell
    az login
    ```

2. [Yeni bir kasa oluşturun](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#create-a-vault)veya CLI komutunu kullanarak:
    ```shell
    az keyvault create --name "[VAULT_NAME]" --resource-group "[RESOURCE_GROUP]" --location "[REGION]"
    ```

3. [Gizli dizileri Key Vault](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault)veya CLI komutunu kullanarak ekleyin:
    ```shell
    az keyvault secret set --vault-name "[VAULT_NAME]" --name "[SECRET_NAME]" --value "[SECRET_VALUE]"
    ```

4. Azure Active Directory veya CLı komutu kullanarak [bir uygulamayı kaydedin](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal#create-an-azure-active-directory-application):
    ```shell
    az ad sp create-for-rbac -n "MavericsKeyVault" --skip-assignment > azure-credentials.json
    ```

5. [Bir uygulamayı gizli dizi kullanacak şekilde](https://docs.microsoft.com/azure/key-vault/secrets/quick-create-portal#add-a-secret-to-key-vault)veya CLI komutunu kullanarak Yetkilendir:
    ```shell
    az keyvault set-policy --name "[VAULT_NAME]" --spn [APPID] --secret-permissions list get
    #APPID can be found in the azure-credentials.json
    generated in the previous step
    ```

Azure Anahtar Kasası 'ndan gizli dizileri yüklemek için, dosyadaki ortam değişkenini `MAVERICS_SECRET_PROVIDER` `/etc/maverics/maverics.env` , aşağıdaki model kullanılarak dosyada azure-credentials.jsbulunan kimlik bilgileriyle ayarlayın: `MAVERICS_SECRET_PROVIDER='azurekeyvault://<KEYVAULT NAME>.vault.azure.net?clientID=<APPID>&clientSecret=<PASSWORD>&tenantID=<TENANT>'`

Ardından Maverics hizmetini yeniden başlatın: `sudo systemctl restart maverics`

## <a name="configure-your-application-in-azure-ad-for-saml-based-sso"></a>Azure AD 'de SAML tabanlı SSO için uygulamanızı yapılandırma

1. Azure Active Directory kiracınızda şuraya gidin `Enterprise applications` , arama yapın `Maverics Identity Orchestrator SAML Connector` ve seçin.

2. ' Maverics kimlik Orchestrator SAML Bağlayıcısı ' üzerinde | Özellikler sayfası, `User assignment required?` uygulamanın yeni geçirilmiş kullanıcılar için çalışmasını sağlamak Için Hayır olarak ayarlayın.

3. ' Maverics kimlik Orchestrator SAML Bağlayıcısı ' üzerinde | Genel Bakış sayfası ' nı seçin `Setup single sign-on` ve ardından öğesini seçin `SAML` .

4. ' Maverics kimlik Orchestrator SAML Bağlayıcısı ' üzerinde | SAML tabanlı oturum açma, temel SAML yapılandırmasını düzenleyin.

   ![Temel SAML yapılandırmasını düzenle](common/edit-urls.png)

5. `Entity ID`aşağıdaki model kullanılarak URL 'yi yazın: `https://<SUBDOMAIN>.maverics.org` . `Entity ID`Kiracıdaki uygulamalar arasında benzersiz olmalıdır. Maverics yapılandırmasına dahil etmek için buraya girilen değeri kaydedin.

6. Aşağıdaki kalıbı kullanarak yanıt URL 'sini ayarlayın: `https://<AZURECOMPANY.COM>/<MY_APP>/` . 

7. Aşağıdaki kalıbı kullanarak oturum açma URL 'sini ayarlayın: `https://<AZURE-COMPANY.COM>/<MY_APP>/<LOGIN PAGE>` ve Kaydet ' e tıklayın.

8. SAML Imzalama sertifikası bölümüne gidin ve Kopyala düğmesine tıklayarak uygulama Federasyon meta verileri URL 'Sini kopyalayın ve bilgisayarınıza kaydedin.

    ![Sertifika indirme bağlantısı](common/copy-metadataurl.png)

## <a name="maverics-identity-orchestrator-azure-ad-saml-connector-configuration"></a>Maverics Identity Orchestrator Azure AD SAML Bağlayıcısı yapılandırması

Maverics kimlik Orchestrator Azure AD Bağlayıcısı şunları destekler: 
- OpenID Connect
- SAML Connect 

1. SAML tabanlı SSO 'yu etkinleştirmek için, ayarlayın `authType: saml` .

1. Şunu için değer oluşturun `samlMetadataURL` : `samlMetadataURL:https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>`

1. Şimdi Azure 'un, Azure kimlik bilgileriyle oturum açtıktan sonra uygulamanızda yeniden yönlendirileceği URL 'YI tanımlayın.
`samlRedirectURL: https://<AZURECOMPANY.COM>/<MY_APP>`

1. Değeri yukarıda yapılandırılan EntityId 'den kopyalayın: `samlEntityID: https://<SUBDOMAIN>.maverics.org`

1. SAML yanıtını göndermek için, Azure AD 'nin kullanacağı yanıt URL 'sinden değeri kopyalayın.
`samlConsumerServiceURL: https://<AZURE-COMPANY.COM>/<MY_APP>`

1. &trade; [OpenSSL aracını](https://www.openssl.org/source/)kullanarak Maverics kimlik Orchestrator oturum bilgilerini korumak IÇIN kullanılan bir JWT imzalama anahtarı oluşturun:

    ```shell 
    openssl rand 64 | base64
    ```
1. Yanıtı `jwtSigningKey` yapılandırma özelliğine kopyalayın: `jwtSigningKey: TBHPvTtu6NUqU84H3Q45grcv9WDJLHgTioqRhB8QGiVzghKlu1mHgP1QHVTAZZjzLlTBmQwgsSoWxGHRcT4Bcw==`

## <a name="attributes-and-attribute-mapping"></a>Öznitelikler ve öznitelik eşleme
Öznitelik eşleme, kullanıcılar sağlandığında, bir kaynak şirket içi Kullanıcı dizininden Azure AD 'ye kullanıcı özniteliklerinin eşlenmesinin tanımlanması için kullanılır.

Öznitelikler, bir talepteki uygulamaya döndürülen kullanıcı verilerini, oturum tanımlama bilgilerine geçirilir veya http üstbilgi değişkenlerinde uygulamaya geçirilir.

## <a name="configure-maverics-identity-orchestrator-azure-ad-saml-connector-yaml"></a>Maverics kimlik Orchestrator Azure AD SAML Bağlayıcısı YAML 'Yi yapılandırma

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

## <a name="migrate-users-to-azure-ad"></a>Kullanıcıları Azure AD 'ye geçirme

Kullanıcıları CA Sitedefteri, Oracle Erişim Yöneticisi veya IBM Tivoli gibi bir Web erişimi yönetim ürününden artımlı olarak geçirmek için bu yapılandırmayı izleyin. bir LDAP dizini; veya bir SQL veritabanı.

## <a name="configure-your-application-permissions-in-azure-ad-to-create-users"></a>Kullanıcı oluşturmak için Azure AD 'de uygulama izinlerinizi yapılandırma

1. Azure Active Directory kiracınızda `App registrations` ' Maverics kimlik Orchestrator SAML Bağlayıcısı ' uygulamasını seçin

2. ' Maverics kimlik Orchestrator SAML Bağlayıcısı ' üzerinde | Sertifikalar &, `New client secret` süre sonu seçeneğini belirleyip seçin. Parolayı kopyalayıp bilgisayarınıza kaydetmek için Kopyala düğmesine tıklayın.

3. ' Maverics kimlik Orchestrator SAML Bağlayıcısı ' üzerinde | API izinleri ' ni seçin `Add permission` ve ardından API Izinleri iste ' `Microsoft Graph` yi seçin ve ardından öğesini seçin `Application permissions` . Sonraki ekranda `User.ReadWrite.All` öğesini seçin ve ardından öğesini seçin `Add permissions` . Bu, API izinlerine geri dönerek seçim sağlar `Grant admin consent` .


## <a name="configure-the-maverics-identity-orchestrator-saml-connector-yaml-for-user-migration"></a>Maverics Identity Orchestrator SAML Bağlayıcısı YAML 'yi Kullanıcı geçişi için yapılandırma

Kullanıcı geçişi iş akışını etkinleştirmek için bu ek özellikleri yapılandırma dosyasına ekleyin:
1. Azure Graph URL 'sini ayarlayın: `graphURL: https://graph.microsoft.com`
1. Şu kalıbı izleyerek OAuth belirteci URL 'sini ayarlayın: `oauthTokenURL: https://login.microsoftonline.com/<TENANT ID>/federationmetadata/2007-06/federationmetadata.xml?appid=<APP ID>`
1. Yukarıda oluşturulan Istemci gizliliğini ayarlayın: `oauthClientSecret: <CLIENT SECRET>`


Son Maverics kimlik Orchestrator Azure AD Bağlayıcısı yapılandırmanız şuna benzeyecektir:
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

## <a name="configure-the-maverics-zero-code-connectortrade-for-siteminder"></a>Maverics sıfır kod bağlayıcısını &trade; Sitedefteri için yapılandırma

Sitedefteri Bağlayıcısı, kullanıcıları Azure AD 'ye geçirmek ve yeni oluşturulan Azure AD kimliklerini ve kimlik bilgilerini kullanarak Sitedefteri tarafından korunan eski şirket içi uygulamalara oturum açmak için kullanılır.

Bu öğreticide, Siteanımsatıcısı, eski uygulamayı form tabanlı kimlik doğrulamasıyla ve tanımlama bilgisini kullanarak koruyacak şekilde yapılandırılmıştır `SMSESSION` . Http üstbilgileri aracılığıyla kimlik doğrulaması ve oturum kullanan bir uygulamayla tümleştirilecek şekilde, üst bilgi öykünme yapılandırmasını bağlayıcıya eklemeniz gerekir.

Bu örnekte, `username` özniteliği `SM_USER` http üstbilgisiyle eşlenir:
```yaml
  headers:
    SM_USER: username
```

Öğesini, `proxyPass` isteklerinin proxy olarak ayarlandığı konuma ayarlayın. Genellikle bu, korumalı uygulamanın ana bilgisayarı olur.

`loginPage` kullanıcılar kimlik doğrulaması için yeniden yönlendirilirken Sitedefteri tarafından şu anda kullanılan oturum açma biçiminin URL 'siyle eşleşmelidir.

```yaml
connectors:
- name: siteminder-login-form
  type: siteminder
  loginType: form
  loginPage: /siteminderagent/forms/login.fcc
  proxyPass: http://host.company.com
```

## <a name="configure-the-maverics-zero-code-connectortrade-for-ldap"></a>LDAP için Maverics sıfır kod bağlayıcısını yapılandırma &trade;

Uygulamalar Sitedefteri gibi bir WAM ürünü tarafından korunduğunda, Kullanıcı kimlikleri ve öznitelikleri genellikle bir LDAP dizininde depolanır.

Bu bağlayıcı yapılandırmasında, geçiş iş akışı sırasında doğru Kullanıcı profili bilgilerinin toplanabilmesi ve Azure AD 'de buna karşılık gelen bir kullanıcının oluşturulabilmesi için, Sitedefteri için Kullanıcı Mağazası olarak yapılandırılmış LDAP dizinine nasıl bağlanabileceğiniz gösterilmektedir.

* `baseDN` dizinde LDAP aramasının gerçekleştirileceği konumu belirtir.

* `url` , bağlanılacak LDAP sunucusunun adresi ve bağlantı noktasıdır.

* `serviceAccountUsername` , genellikle bir bağlama DN 'si olarak ifade edilen LDAP sunucusuna bağlanmak için kullanılan kullanıcı adıdır (örneğin,) `CN=Directory Manager` .

* `serviceAccountPassword` LDAP sunucusuna bağlanmak için kullanılan parola. Bu değer, daha önce yapılandırılan Azure Key Vault örneğinde depolanır.  

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

## <a name="configure-the-migration-workflow"></a>Geçiş iş akışını yapılandırma

Geçiş iş akışı yapılandırması, Maverics kullanıcıları Sitedefteri/LDAP 'den Azure AD 'ye nasıl geçireceğinizi belirler.

Bu iş akışı:
- Sitedefteri bağlayıcısını kullanarak Sitebir oturum açma kimliğini ara. Kullanıcı kimlik bilgileri, Sitedefteri kimlik doğrulaması ile onaylanır ve sonra iş akışının sonraki adımlarına geçirilir.
- Sitedefteri kullanıcı deposundan Kullanıcı profili özniteliklerini alır.
- Azure AD kiracınızda Kullanıcı oluşturmak için Microsoft Graph API 'sine bir istek yapar.

Adımlar:
1. İş akışına bir ad verin, örneğin, Azure AD geçişine.
2. İş akışının `endpoint` açığa çıkarılan, `actions` Bu iş akışının istekleri yanıt olarak tetiklediği bir http yolu olan öğesini belirtin. `endpoint`Genellikle, proxy olan uygulamaya karşılık gelir, ör `/my_app` . Değer hem baştaki hem de sondaki eğik çizgileri içermelidir.
3. `actions`İş akışına uygun şekilde ekleyin.
    - `login`Sitedefteri Bağlayıcısı için yöntemi tanımlayın. Bağlayıcı değeri, bağlayıcı yapılandırmasındaki ad değeriyle eşleşmelidir.
     - `getprofile`LDAP bağlayıcısının yöntemini tanımlayın.
     - `createuser`AzureAD Bağlayıcısı için tanımlayın.

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

1. Maverics hizmeti zaten çalışmıyorsa, aşağıdaki komutu yürüterek başlatın: `sudo systemctl start maverics`

2. Proxy kullanan oturum açma URL 'sine gidin: `http://host.company.com/my_app` .
3. Sitedefteri tarafından korunurken uygulamada oturum açmak için kullanılan Kullanıcı kimlik bilgilerini sağlayın.
4. Ev > kullanıcılarına gidin | Tüm kullanıcılar, Azure AD kiracısında Kullanıcı oluşturulduğunu doğrular.  

## <a name="configure-the-session-abstraction-workflow"></a>Oturum soyutlama iş akışını yapılandırma

Oturum soyutlama iş akışı, eski şirket içi Web uygulaması için kimlik doğrulama ve erişim denetimini Azure AD 'ye taşıdır.

Azure Bağlayıcısı, `login` oturum yok varsayıldığında kullanıcıyı oturum açma URL 'sine yönlendirmek için yöntemini kullanır.

Kimliği doğrulandıktan sonra, sonuç olarak oluşturulan oturum belirteci Maverics 'e geçirilir ve Sitedefteri bağlayıcısının `emulate` yöntemi, tanımlama bilgisi tabanlı oturuma ve/veya üst bilgi tabanlı oturuma öykünmek için kullanılır ve sonra isteği uygulamanın gerektirdiği ek özniteliklerle süsler.

1. İş akışına bir ad verin, örn. Sitedefteri oturum soyutlaması.
2. `endpoint`Proxy kullanan uygulamaya karşılık gelen öğesini belirtin. Değer, hem baştaki hem de sondaki eğik çizgileri içermelidir, örn `/my_app/` .
3. `actions`İş akışına uygun şekilde ekleyin.
    - `login`Azure Bağlayıcısı için yöntemi tanımlayın. `connector`Değerin `name` bağlayıcı yapılandırmasındaki değerle eşleşmesi gerekir.
    - `emulate`Sitedefteri Bağlayıcısı için yöntemi tanımlayın.

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

1. Proxy kullanan uygulama URL 'sine gidin: `https://<AZURECOMPANY.COM>/<MY_APP>` . Kullanıcı proxy oturum açma sayfasına yönlendirilir.
2. Azure AD Kullanıcı kimlik bilgilerini girin.
3. Kullanıcı, doğrudan Sitedefteri tarafından kimliği doğrulanmış olarak uygulamaya yönlendirilmelidir.
