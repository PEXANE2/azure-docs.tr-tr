---
title: Kimlik doğrulamasını ayarlama
titleSuffix: Azure Machine Learning
description: Azure Machine Learning çeşitli kaynaklar ve iş akışları için kimlik doğrulamasını ayarlamayı ve yapılandırmayı öğrenin. Hizmet içinde kimlik doğrulamasını yapılandırmanın ve kullanmanın birden çok yolu vardır; geliştirme veya test amaçları için basit UI tabanlı kimlik doğrulamasından, tam Azure Active Directory hizmet sorumlusu kimlik doğrulamasına kadar birçok yol vardır.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: fcaa7a0c44851d6b48b40b01af4c8ec992c330b8
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79283543"
---
# <a name="set-up-authentication-for-azure-machine-learning-resources-and-workflows"></a>Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulamasını ayarlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning çeşitli kaynaklar ve iş akışları için kimlik doğrulaması ayarlamayı ve yapılandırmayı öğreneceksiniz. Hizmette kimlik doğrulamanın birden çok yolu vardır ve geliştirme veya test amacıyla basit kullanıcı arabirimi tabanlı kimlik doğrulamasının tam Azure Active Directory hizmet sorumlusu kimlik doğrulamasına kadar çeşitli yolları vardır. Bu makalede ayrıca, Web hizmeti kimlik doğrulamasının nasıl çalıştığı ve Azure Machine Learning REST API nasıl doğrulanacağı açıklanır.

Bu nasıl yapılır, aşağıdaki görevlerin nasıl yapılacağını gösterir:

* Test/geliştirme için etkileşimli UI kimlik doğrulamasını kullanma
* Hizmet sorumlusu kimlik doğrulamasını ayarlama
* Çalışma alanınızda kimlik doğrulaması
* Azure Machine Learning REST API için OAuth 2.0 taşıyıcı türü belirteçleri alın
* Web hizmeti kimlik doğrulamasını anlama

Azure Machine Learning içinde güvenlik ve kimlik doğrulamasına genel bir bakış için bkz. [kavram makalesi](concept-enterprise-security.md) .

## <a name="prerequisites"></a>Ön koşullar

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md)oluşturun.
* Azure Machine Learning SDK 'yı yüklemek için [geliştirme ortamınızı yapılandırın](how-to-configure-environment.md) veya SDK 'nın zaten yüklü olduğu bir [Azure MACHINE LEARNING Not defteri VM](concept-azure-machine-learning-architecture.md#compute-instance) 'si kullanın.

## <a name="interactive-authentication"></a>Etkileşimli kimlik doğrulaması

Bu hizmetin belgelerindeki çoğu örnek, test ve gösterim için basit bir yöntem olarak jupi not defterlerinde etkileşimli kimlik doğrulaması kullanır. Bu, ne oluşturmakta olduğunuzu test etmenin hafif bir yoludur. Otomatik olarak Kullanıcı arabirimi tabanlı kimlik doğrulama akışı isteyen iki işlev çağrısı vardır.

`from_config()` İşlevi çağırmak, istem olarak verilecek.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

İşlevi `from_config()` , çalışma alanı bağlantı bilgilerinizi IÇEREN bir JSON dosyası arar. Ayrıca, `Workspace` oluşturucuyu kullanarak bağlantı ayrıntılarını açıkça belirtebilirsiniz ve bu da etkileşimli kimlik doğrulaması için istemde bulunabilir. Her iki çağrı de eşdeğerdir.

```python
ws = Workspace(subscription_id="your-sub-id",
               resource_group="your-resource-group-id",
               workspace_name="your-workspace-name"
              )
```

Birden çok kiracıya erişiminiz varsa, sınıfı içeri aktarmanız ve hedeflediğiniz kiracıyı açıkça tanımlamanız gerekebilir. Oluşturucusunun `InteractiveLoginAuthentication` çağrılması, yukarıdaki çağrılara benzer bir oturum açma bilgileri de ister.

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
```

Test ve öğrenme için faydalı olsa da, etkileşimli kimlik doğrulaması otomatikleştirilmiş veya gözetimsiz iş akışları oluşturmanıza yardımcı olmaz. Hizmet sorumlusu kimlik doğrulamasını ayarlama, SDK 'Yı kullanan otomatikleştirilmiş işlemlere yönelik en iyi yaklaşımdır.

## <a name="set-up-service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulamasını ayarlama

Bu işlem, belirli bir Kullanıcı oturum açma işleminden ayrılmış kimlik doğrulamanın etkinleştirilmesi için gereklidir. Bu, otomatik iş akışlarında Azure Machine Learning Python SDK ' da kimlik doğrulaması yapmanıza olanak sağlar. Hizmet sorumlusu kimlik doğrulaması ayrıca [REST API kimlik](#azure-machine-learning-rest-api-auth)doğrulaması yapmanıza izin verir.

Hizmet sorumlusu kimlik doğrulamasını ayarlamak için öncelikle Azure Active Directory ' de bir uygulama kaydı oluşturun ve ardından ML çalışma alanınıza uygulamanızın rol tabanlı erişimini verirsiniz. Bu kurulumu tamamlamaya en kolay yolu, Azure portal [Azure Cloud Shell](https://azure.microsoft.com/features/cloud-shell/) . Portalda oturum açtıktan sonra, kabuğu açmak için adınızın `>_` yanındaki sayfanın sağ üst kısmındaki simgeye tıklayın.

Cloud Shell 'i Azure hesabınızda kullanmadıysanız, yazılan dosyaları depolamak için bir depolama hesabı kaynağı oluşturmanız gerekir. Genel olarak bu depolama hesabı, daha az bir aylık maliyet doğuracaktır. Ayrıca, daha önce aşağıdaki komutla kullanmadıysanız Machine Learning uzantısını da yükleyebilirsiniz.

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!NOTE]
> Aşağıdaki adımları gerçekleştirmek için abonelikte yönetici olmanız gerekir.

Ardından, hizmet sorumlusunu oluşturmak için aşağıdaki komutu çalıştırın. Bu örnekte **ml-auth**adına bir ad verin.

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth --name ml-auth
```

Çıktı aşağıdakine benzer bir JSON olacaktır. `clientId`, `clientSecret`, Ve `tenantId` alanlarını, bu makaledeki diğer adımlar için ihtiyaç duyduğunuz gibi bir yere göz atın.

```json
{
    "clientId": "your-client-id",
    "clientSecret": "your-client-secret",
    "subscriptionId": "your-sub-id",
    "tenantId": "your-tenant-id",
    "activeDirectoryEndpointUrl": "https://login.microsoftonline.com",
    "resourceManagerEndpointUrl": "https://management.azure.com",
    "activeDirectoryGraphResourceId": "https://graph.windows.net",
    "sqlManagementEndpointUrl": "https://management.core.windows.net:5555",
    "galleryEndpointUrl": "https://gallery.azure.com/",
    "managementEndpointUrl": "https://management.core.windows.net"
}
```

Daha sonra, yeni oluşturduğunuz hizmet sorumlusu hakkındaki ayrıntıları, `clientId` `--id` parametrenin girişi olarak yukarıdaki değeri kullanarak almak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az ad sp show --id your-client-id
```

Aşağıda, komuttan gelen JSON çıktısının basitleştirilmiş bir örneği verilmiştir. Bir sonraki adım için `objectId` değerine ihtiyacınız olacak şekilde, alanı buraya göz atın.

```json
{
    "accountEnabled": "True",
    "addIns": [],
    "appDisplayName": "ml-auth",
    ...
    ...
    ...
    "objectId": "your-sp-object-id",
    "objectType": "ServicePrincipal"
}
```

Ardından, Service Principal erişiminizi Machine Learning çalışma alanınıza atamak için aşağıdaki komutu kullanın. , `-w` Ve `-g` parametreleri için çalışma alanınızın adı ve kaynak grubu adı gerekir. `--user` Parametresi için, önceki adımdaki `objectId` değeri kullanın. `--role` Parametresi, hizmet sorumlusu için erişim rolünü ayarlamanıza olanak sağlar ve genel olarak, **sahibi** veya **katkıda**bulunanı kullanacaksınız. Her ikisinin de, işlem kümeleri ve veri depoları gibi mevcut kaynaklara yazma erişimi vardır, ancak bu kaynakları yalnızca **sahibi** sağlayabilir. 

```azurecli-interactive
az ml workspace share -w your-workspace-name -g your-resource-group-name --user your-sp-object-id --role owner
```

Bu çağrı hiçbir çıkış üretmez, ancak artık çalışma alanınız için hizmet sorumlusu kimlik doğrulaması ayarlamış olursunuz.

## <a name="authenticate-to-your-workspace"></a>Çalışma alanınızda kimlik doğrulama

Hizmet sorumlusu kimlik doğrulaması etkinleştirildikten sonra, bir kullanıcı olarak fiziksel olarak oturum açmadan SDK 'da çalışma alanınızda kimlik doğrulaması yapabilirsiniz. `ServicePrincipalAuthentication` Sınıf oluşturucusunu kullanın ve önceki adımlardan aldığınız değerleri parametreler olarak kullanın. `tenant_id` `tenantId` Parametresi, yukarıdakiyle eşlenir `service_principal_id` , ile eşlenir `clientId`ve `service_principal_password` ile eşlenir. `clientSecret`

```python
from azureml.core.authentication import ServicePrincipalAuthentication

sp = ServicePrincipalAuthentication(tenant_id="your-tenant-id", # tenantID
                                    service_principal_id="your-client-id", # clientId
                                    service_principal_password="your-client-secret") # clientSecret
```

`sp` Değişken artık doğrudan SDK 'da kullandığınız bir kimlik doğrulama nesnesini barındırır. Genel olarak, aşağıdaki kodda gösterildiği gibi, yukarıda kullanılan kimlikleri/gizli dizileri ortam değişkenlerinde depolamak iyi bir fikirdir.

```python
import os 

sp = ServicePrincipalAuthentication(tenant_id=os.environ['AML_TENANT_ID'],
                                    service_principal_id=os.environ['AML_PRINCIPAL_ID'],
                                    service_principal_password=os.environ['AML_PRINCIPAL_PASS'])
```

Python 'da çalışan ve SDK 'Yı ilk olarak kullanan otomatik iş akışları için bu nesneyi, kimlik doğrulamalarınız için çoğu durumda olduğu gibi kullanabilirsiniz. Aşağıdaki kod, yeni oluşturduğunuz auth nesnesini kullanarak çalışma alanınıza yönelik kimlik doğrular.

```python
from azureml.core import Workspace

ws = Workspace.get(name="ml-example", 
                   auth=sp,
                   subscription_id="your-sub-id")
ws.get_details()
```

## <a name="azure-machine-learning-rest-api-auth"></a>Azure Machine Learning REST API kimlik doğrulaması

Yukarıdaki adımlarda oluşturulan hizmet sorumlusu, Azure Machine Learning [REST API](https://docs.microsoft.com/rest/api/azureml/)kimlik doğrulaması için de kullanılabilir. Otomatik iş akışlarında gözetimsiz kimlik doğrulaması için hizmetten hizmete çağrılara izin veren Azure Active Directory [istemci kimlik bilgileri verme akışı](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)' nı kullanırsınız. Örnekler hem Python hem de Node. js ' de [adal kitaplığı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) ile uygulanır, ancak OpenID Connect 1,0 'yi destekleyen herhangi bir açık kaynak kitaplığı da kullanabilirsiniz. 

> [!NOTE]
> MSAL. js, ADAL 'den daha yeni bir kitaplıktır, ancak özellikle belirli bir kullanıcıya bağlı etkileşimli/UI kimlik doğrulaması için tasarlanan bir istemci tarafı kitaplığı olduğundan, MSAL. js ile istemci kimlik bilgilerini kullanarak hizmetten hizmete kimlik doğrulaması yapılamaz. REST API ile otomatikleştirilmiş iş akışları oluşturmak için aşağıda gösterildiği gibi ADAL kullanmanızı öneririz.

### <a name="nodejs"></a>Node.js

Node. js kullanarak bir kimlik doğrulama belirteci oluşturmak için aşağıdaki adımları kullanın. Ortamınızda öğesini çalıştırın `npm install adal-node`. Daha sonra, ve `tenantId` `clientSecret` Yukarıdaki `clientId`adımlarda oluşturduğunuz hizmet sorumlusunun aşağıdaki betikteki eşleşen değişkenlerin değerleri olarak değerini kullanın.

```javascript
const adal = require('adal-node').AuthenticationContext;

const authorityHostUrl = 'https://login.microsoftonline.com/';
const tenantId = 'your-tenant-id';
const authorityUrl = authorityHostUrl + tenantId;
const clientId = 'your-client-id';
const clientSecret = 'your-client-secret';
const resource = 'https://management.azure.com/';

const context = new adal(authorityUrl);

context.acquireTokenWithClientCredentials(
  resource,
  clientId,
  clientSecret,
  (err, tokenResponse) => {
    if (err) {
      console.log(`Token generation failed due to ${err}`);
    } else {
      console.dir(tokenResponse, { depth: null, colors: true });
    }
  }
);
```

Değişkeni `tokenResponse` , belirteci ve sona erme saati gibi ilişkili meta verileri içeren bir nesnedir. Belirteçler 1 saat için geçerlidir ve yeni bir belirteç almak için aynı çağrı çalıştırılarak yenilenebilir. Aşağıda örnek bir yanıt verilmiştir.

```javascript
{ 
    tokenType: 'Bearer',
    expiresIn: 3599,
    expiresOn: 2019-12-17T19:15:56.326Z,
    resource: 'https://management.azure.com/',
    accessToken: "random-oauth-token",
    isMRRT: true,
    _clientId: 'your-client-id',
    _authority: 'https://login.microsoftonline.com/your-tenant-id' 
}
```

Kimlik doğrulama `accessToken` belirtecini getirmek için özelliğini kullanın. API çağrıları yapmak için belirteci kullanma hakkında örnekler için [REST API belgelerine](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) bakın.

### <a name="python"></a>Python 

Python kullanarak bir kimlik doğrulama belirteci oluşturmak için aşağıdaki adımları kullanın. Ortamınızda öğesini çalıştırın `pip install adal`. Daha sonra, ve `tenantId` `clientSecret` Yukarıdaki `clientId`adımlarda oluşturduğunuz hizmet sorumlusunun, aşağıdaki komut dosyasında uygun değişkenlerin değerleri olarak değerlerini kullanın.

```python
from adal import AuthenticationContext

client_id = "your-client-id"
client_secret = "your-client-secret"
resource_url = "https://login.microsoftonline.com"
tenant_id = "your-tenant-id"
authority = "{}/{}".format(resource_url, tenant_id)

auth_context = AuthenticationContext(authority)
token_response = auth_context.acquire_token_with_client_credentials("https://management.azure.com/", client_id, client_secret)
print(token_response)
```

Değişken `token_response` , belirteci ve sona erme saati gibi ilişkili meta verileri içeren bir sözlüktür. Belirteçler 1 saat için geçerlidir ve yeni bir belirteç almak için aynı çağrı çalıştırılarak yenilenebilir. Aşağıda örnek bir yanıt verilmiştir.

```python
{
    'tokenType': 'Bearer', 
    'expiresIn': 3599, 
    'expiresOn': '2019-12-17 19:47:15.150205', 
    'resource': 'https://management.azure.com/', 
    'accessToken': 'random-oauth-token', 
    'isMRRT': True, 
    '_clientId': 'your-client-id', 
    '_authority': 'https://login.microsoftonline.com/your-tenant-id'
}
```

Kimlik `token_response["accessToken"]` doğrulama belirtecini getirmek için kullanın. API çağrıları yapmak için belirteci kullanma hakkında örnekler için [REST API belgelerine](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) bakın.

## <a name="web-service-authentication"></a>Web hizmeti kimlik doğrulaması

Azure Machine Learning Web Hizmetleri, yukarıda açıklanenden farklı bir kimlik doğrulama modelini kullanır. Dağıtılan Web Hizmetleri için kimlik doğrulamanın en kolay yolu, yenilenmesi gerekmeyen statik taşıyıcı türü kimlik doğrulama anahtarları üreten **anahtar tabanlı kimlik doğrulaması**kullanmaktır. Yalnızca dağıtılan bir Web hizmetinde kimlik doğrulaması yapmanız gerekiyorsa, yukarıda gösterildiği gibi hizmet ilkesi kimlik doğrulamasını ayarlamanız gerekmez.

Azure Kubernetes hizmetinde dağıtılan Web Hizmetleri, varsayılan olarak anahtar tabanlı kimlik doğrulama *etkindir* . Azure Container Instances dağıtılan hizmetlerde anahtar tabanlı kimlik doğrulaması varsayılan olarak *devre dışıdır* , ancak aci Web hizmeti oluştururken ayarı `auth_enabled=True`yaparak etkinleştirebilirsiniz. Aşağıda anahtar tabanlı kimlik doğrulaması etkinleştirilmiş bir acı dağıtım yapılandırması oluşturma örneği verilmiştir.

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(cpu_cores = 1,
                                                memory_gb = 1,
                                                auth_enabled=True)
```

Daha sonra, `Model` sınıfını kullanarak dağıtımda özel aci yapılandırmasını kullanabilirsiniz.

```python
from azureml.core.model import Model, InferenceConfig


inference_config = InferenceConfig(entry_script="score.py",
                                   environment=myenv)
aci_service = Model.deploy(workspace=ws,
                       name="aci_service_sample",
                       models=[model],
                       inference_config=inference_config,
                       deployment_config=aci_config)
aci_service.wait_for_deployment(True)
```

Kimlik doğrulama anahtarlarını getirmek için kullanın `aci_service.get_keys()`. Bir anahtarı yeniden oluşturmak için, `regen_key()` işlevini kullanın ve **birincil** ya da **İkincil**olarak geçirin.

```python
aci_service.regen_key("Primary")
# or
aci_service.regen_key("Secondary")
```

Web Hizmetleri ayrıca belirteç tabanlı kimlik doğrulamasını destekler, ancak yalnızca Azure Kubernetes hizmet dağıtımları için geçerlidir. Kimlik doğrulama hakkında daha fazla bilgi için bkz. Web hizmetlerini [kullanma](how-to-consume-web-service.md) .

### <a name="token-based-web-service-authentication"></a>Belirteç tabanlı Web hizmeti kimlik doğrulaması

Bir Web hizmeti için belirteç kimlik doğrulamasını etkinleştirdiğinizde, kullanıcılar, Web hizmetine erişmek için bir Azure Machine Learning JSON Web Token sunmalıdır. Belirtecin, belirtilen bir zaman çerçevesinde sonra süresi dolar ve çağrı yapmaya devam etmek için yenilenmesi gerekir.

* Belirteç kimlik doğrulaması, Azure Kubernetes hizmetine dağıtırken **Varsayılan olarak devre dışıdır** .
* Azure Container Instances ' a dağıtırken belirteç kimlik doğrulaması **desteklenmez** .

Belirteç kimlik doğrulamasını denetlemek için, bir `token_auth_enabled` dağıtımı oluştururken veya güncelleştirdiğinizde parametresini kullanın.

Belirteç kimlik doğrulaması etkinleştirilirse, bir JSON Web Token (JWT) `get_token` almak için yöntemini ve bu belirtecin sona erme süresini kullanabilirsiniz:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirtecin `refresh_by` zamanından sonra yeni bir belirteç istemeniz gerekir. Belirteçleri Python SDK 'sının dışında yenilemeniz gerekiyorsa, bir seçenek, daha önce anlatıldığı gibi, `service.get_token()` çağrıyı düzenli olarak yapmak için hizmet sorumlusu kimlik doğrulamasıyla REST API kullanmaktır.
>
> Azure Machine Learning çalışma alanınızı Azure Kubernetes hizmet kümeniz ile aynı bölgede oluşturmanızı önemle öneririz. 
>
> Bir belirteçle kimlik doğrulaması yapmak için Web hizmeti, Azure Machine Learning çalışma alanınızın oluşturulduğu bölgeye bir çağrı yapar. Çalışma alanınızın bölgesi kullanılamıyorsa, kümeniz çalışma alanınızdan farklı bir bölgede olsa bile, Web hizmetiniz için bir belirteç getirimeyeceksiniz. Sonuç olarak, çalışma alanınızın bölgesi yeniden kullanılabilir olana kadar Azure AD kimlik doğrulaması kullanılamaz. 
>
> Ayrıca, kümenizin bölgesi ve çalışma alanınızın bölgesi arasındaki mesafe arttıkça bir belirteç getirmek için o kadar sürer.

## <a name="next-steps"></a>Sonraki adımlar

* [Görüntü sınıflandırma modelini eğitme ve dağıtma](tutorial-train-models-with-aml.md).
* [Web hizmeti olarak dağıtılan bir Azure Machine Learning modeli](how-to-consume-web-service.md)kullanın.
