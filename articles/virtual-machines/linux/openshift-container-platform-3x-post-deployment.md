---
title: Azure dağıtım sonrası görevlerde OpenShift Kapsayıcı Platformu 3.11
description: OpenShift Kapsayıcı Platformu 3.11 kümesinden sonra için ek görevler dağıtıldı.
services: virtual-machines-linux
documentationcenter: virtual-machines
author: haroldwongms
manager: mdotson
editor: ''
tags: azure-resource-manager
ms.assetid: ''
ms.service: virtual-machines-linux
ms.topic: article
ms.tgt_pltfrm: vm-linux
ms.workload: infrastructure
ms.date: 10/14/2019
ms.author: haroldw
ms.openlocfilehash: 1635589b282dc33f6a1e9c2552dc8a73c67b9004
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80294740"
---
# <a name="post-deployment-tasks"></a>Dağıtım sonrası görevler

Bir OpenShift kümesini dağıttıktan sonra ek öğeler yapılandırabilirsiniz. Bu makale şunları kapsamaktadır:

- Azure Etkin Dizin (Azure AD) kullanarak tek oturum açma yapılandırma
- OpenShift'i izlemek için Azure Monitor günlükleri nasıl yapılandırılabilen
- Ölçümler ve günlüğe kaydetme nasıl yapılandırılır?
- Azure için Open Service Broker (OSBA) nasıl yüklenir?

## <a name="configure-single-sign-on-by-using-azure-active-directory"></a>Azure Active Directory'yi kullanarak tek oturum açma'yı yapılandırın

Kimlik doğrulaması için Azure Etkin Dizini'ni kullanmak için öncelikle bir Azure AD uygulama kaydı oluşturmanız gerekir. Bu işlem iki adım içerir: uygulama kaydı oluşturma ve izinleri yapılandırma.

### <a name="create-an-app-registration"></a>Uygulama kaydı oluşturma

Bu adımlar, uygulama kaydını oluşturmak için Azure CLI'yi ve izinleri ayarlamak için GUI'yi (portal) kullanır. Uygulama kaydını oluşturmak için aşağıdaki beş parça bilgiye ihtiyacınız var:

- Görüntü adı: Uygulama kayıt adı (örneğin, OCPAzureAD)
- Giriş sayfası: OpenShift konsol URL'si (örneğin, `https://masterdns343khhde.westus.cloudapp.azure.com/console`)
- Tanımlayıcı URI: OpenShift konsol URL 'si `https://masterdns343khhde.westus.cloudapp.azure.com/console`(örneğin, )
- Yanıt URL'si: Genel url'de ana `https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD`url ve uygulama kayıt adı (örneğin, )
- Parola: Güvenli parola (güçlü bir parola kullanın)

Aşağıdaki örnek, önceki bilgileri kullanarak bir uygulama kaydı oluşturur:

```azurecli
az ad app create --display-name OCPAzureAD --homepage https://masterdns343khhde.westus.cloudapp.azure.com/console --reply-urls https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/hwocpadint --identifier-uris https://masterdns343khhde.westus.cloudapp.azure.com/console --password {Strong Password}
```

Komut başarılı olursa, aşağıdakilere benzer bir JSON çıktısı alırsınız:

```json
{
  "appId": "12345678-ca3c-427b-9a04-ab12345cd678",
  "appPermissions": null,
  "availableToOtherTenants": false,
  "displayName": "OCPAzureAD",
  "homepage": "https://masterdns343khhde.westus.cloudapp.azure.com/console",
  "identifierUris": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/console"
  ],
  "objectId": "62cd74c9-42bb-4b9f-b2b5-b6ee88991c80",
  "objectType": "Application",
  "replyUrls": [
    "https://masterdns343khhde.westus.cloudapp.azure.com/oauth2callback/OCPAzureAD"
  ]
}
```

Daha sonraki bir adım için komuttan döndürülen appId özelliğine dikkat edin.

Azure portalında:

1. Azure Active Directory > **Uygulama** **Kaydı'nı**seçin.
2. Uygulama kaydınızı arayın (örneğin, OCPAzureAD).
3. Sonuçlarda, uygulama kaydını tıklatın.
4. **Ayarlar**altında, **Gerekli izinleri**seçin.
5. **Gerekli İzinler**altında **Ekle'yi**seçin.

   ![Uygulama Kaydı](media/openshift-post-deployment/app-registration.png)

6. Adım 1:API'yi seçin ve ardından **Windows Azure Etkin Dizini'ni (Microsoft.Azure.ActiveDirectory)** tıklatın. En altta **Seç'i** tıklatın.

   ![Uygulama Kaydı SELECT API](media/openshift-post-deployment/app-registration-select-api.png)

7. 2. Adımda: İzinleri seçin, Oturum **Aç'ı** seçin ve **Temsilci İzleri**altında kullanıcı profilini okuyun ve sonra **Seç'i**tıklatın.

   ![Uygulama Kayıt Erişimi](media/openshift-post-deployment/app-registration-access.png)

8. **Done** (Bitti) öğesini seçin.

### <a name="configure-openshift-for-azure-ad-authentication"></a>Azure AD kimlik doğrulaması için OpenShift'i yapılandırma

OpenShift'i azure AD'yi kimlik doğrulama sağlayıcısı olarak kullanacak şekilde yapılandırmak için /etc/origin/master/master-config.yaml dosyasının tüm ana düğümlerde düzenlenmesi gerekir.

Aşağıdaki CLI komutunu kullanarak kiracı kimliğini bulun:

```azurecli
az account show
```

Yaml dosyasında aşağıdaki satırları bulun:

```yaml
oauthConfig:
  assetPublicURL: https://masterdns343khhde.westus.cloudapp.azure.com/console/
  grantConfig:
    method: auto
  identityProviders:
  - challenge: true
    login: true
    mappingMethod: claim
    name: htpasswd_auth
    provider:
      apiVersion: v1
      file: /etc/origin/master/htpasswd
      kind: HTPasswdPasswordIdentityProvider
```

Önceki satırlardan hemen sonra aşağıdaki satırları ekleyin:

```yaml
  - name: <App Registration Name>
    challenge: false
    login: true
    mappingMethod: claim
    provider:
      apiVersion: v1
      kind: OpenIDIdentityProvider
      clientID: <appId>
      clientSecret: <Strong Password>
      claims:
        id:
        - sub
        preferredUsername:
        - unique_name
        name:
        - name
        email:
        - email
      urls:
        authorize: https://login.microsoftonline.com/<tenant Id>/oauth2/authorize
        token: https://login.microsoftonline.com/<tenant Id>/oauth2/token
```

Metin kimlik Sağlayıcıları altında doğru hizalandığından emin olun. Aşağıdaki CLI komutunu kullanarak kiracı kimliğini bulun:```az account show```

OpenShift ana hizmetlerini tüm ana düğümlerde yeniden başlatın:

```bash
sudo /usr/local/bin/master-restart api
sudo /usr/local/bin/master-restart controllers
```

OpenShift konsolunda artık kimlik doğrulama için iki seçenek görürsünüz: htpasswd_auth ve [Uygulama Kaydı].

## <a name="monitor-openshift-with-azure-monitor-logs"></a>Azure Monitör günlükleriyle OpenShift'i izleyin

Log Analytics aracısını OpenShift'e eklemenin üç yolu vardır.
- Linux için Log Analytics aracısını doğrudan her OpenShift düğümüne yükleyin
- Her OpenShift düğümünde Azure Monitör VM Uzantısını etkinleştirme
- Log Analytics aracısını OpenShift daemon-set olarak yükleme

Daha fazla bilgi için tüm [talimatları](https://docs.microsoft.com/azure/log-analytics/log-analytics-containers#configure-a-log-analytics-agent-for-red-hat-openshift) okuyun.

## <a name="configure-metrics-and-logging"></a>Ölçümleri ve günlüğe kaydetmeyi yapılandırma

Dala bağlı olarak, OpenShift Kapsayıcı Platformu ve OKD için Azure Kaynak Yöneticisi şablonları, yüklemenin bir parçası olarak ölçümleri etkinleştirmek ve günlüğe kaydetmek için giriş parametreleri sağlayabilir.

OpenShift Konteyner Platformu Marketplace teklifi, küme yüklemesi sırasında ölçümleri ve günlüğe kaydetmeyi etkinleştirmek için bir seçenek de sunar.

Kümenin yüklenmesi sırasında ölçümler / günlüğe kaydetme etkinleştirilmeseydi, olaydan sonra kolayca etkinleştirilebilir.

### <a name="azure-cloud-provider-in-use"></a>Azure Bulut Sağlayıcısı kullanılıyor

Dağıtım sırasında sağlanan kimlik bilgilerini kullanarak burç düğümüne veya ilk ana düğüme (şablon ve kullanılan dala dayalı) SSH. Aşağıdaki komutu sorun:

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True \
-e openshift_metrics_cassandra_storage_type=dynamic

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True \
-e openshift_logging_es_pvc_dynamic=true
```

### <a name="azure-cloud-provider-not-in-use"></a>Azure Bulut Sağlayıcısı kullanımda değil

```bash
ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-metrics/config.yml \
-e openshift_metrics_install_metrics=True

ansible-playbook /usr/share/ansible/openshift-ansible/playbooks/openshift-logging/config.yml \
-e openshift_logging_install_logging=True
```

## <a name="install-open-service-broker-for-azure-osba"></a>Azure için Açık Servis Aracısı (OSBA) yükle

Azure için Açık Servis Aracısı veya OSBA, Azure Bulut Hizmetleri'ni doğrudan OpenShift'ten sağlamanıza olanak tanır. Azure için Bir Açık Hizmet Aracısı API uygulamasında OSBA. Açık Hizmet Aracısı API, bulut yerel uygulamalarının kilitlenmeden bulut hizmetlerini yönetmek için kullanabileceği bulut sağlayıcıları için ortak bir dil tanımlayan bir spec'tir.

OSBA'yı OpenShift'e yüklemek için burada https://github.com/Azure/open-service-broker-azure#openshift-project-templatebulunan yönergeleri izleyin: . 
> [!NOTE]
> Yalnızca OpenShift Project Template bölümündeki adımları tamamlayın, Yükleme bölümünün tamamını tamamlayın.

## <a name="next-steps"></a>Sonraki adımlar

- [OpenShift Konteyner Platformu ile başlarken](https://docs.openshift.com)
