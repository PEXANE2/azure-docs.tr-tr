---
title: Kimlik doğrulamasını ayarlama
titleSuffix: Azure Machine Learning
description: Azure Machine Learning çeşitli kaynaklar ve iş akışları için kimlik doğrulamasını ayarlamayı ve yapılandırmayı öğrenin. Hizmet içinde kimlik doğrulamasını yapılandırmanın ve kullanmanın birden çok yolu vardır; geliştirme veya test amaçları için basit UI tabanlı kimlik doğrulamasından, tam Azure Active Directory hizmet sorumlusu kimlik doğrulamasına kadar birçok yol vardır.
services: machine-learning
author: larryfr
ms.author: larryfr
ms.reviewer: larryfr
ms.service: machine-learning
ms.subservice: core
ms.date: 06/17/2020
ms.topic: conceptual
ms.custom: how-to, has-adal-ref, devx-track-javascript
ms.openlocfilehash: 867babcf7160ce6e589197be0a3186e5a2156238
ms.sourcegitcommit: e71da24cc108efc2c194007f976f74dd596ab013
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/29/2020
ms.locfileid: "87423584"
---
# <a name="set-up-authentication-for-azure-machine-learning-resources-and-workflows"></a>Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulamasını ayarlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Azure Machine Learning çalışma alanınızda ve Web Hizmetleri olarak dağıtılan modellerle kimlik doğrulaması yapmayı öğrenin.

Genel olarak, Azure Machine Learning ile kullanabileceğiniz iki tür kimlik doğrulaması vardır:

* __Etkileşimli__: hesabınızı doğrudan kimlik doğrulamak veya kimlik doğrulama için kullanılan bir belirteç almak üzere Azure Active Directory ' de kullanırsınız. Etkileşimli kimlik doğrulaması deneme ve yinelemeli geliştirme sırasında kullanılır. Ya da kaynaklara erişimi (bir Web hizmeti gibi) Kullanıcı başına temelinde denetlemek istediğiniz yerdir.
* __Hizmet sorumlusu__: Azure Active Directory ' de bir hizmet sorumlusu hesabı oluşturur ve bir belirteç almak veya bir belirteci almak için kullanın. Hizmet sorumlusu, Kullanıcı etkileşimi gerektirmeden hizmette kimlik doğrulaması yapmak için otomatik bir işlem gerektiğinde kullanılır. Örneğin, eğitim kodu her değiştiğinde bir modeli gösteren ve test eden bir sürekli tümleştirme ve dağıtım betiği. Hizmetin kimliğini doğrulamak için son kullanıcının gerekli olmasını istemiyorsanız bir Web hizmetinde kimlik doğrulaması yapmak için bir belirteç almak üzere bir hizmet sorumlusu de kullanabilirsiniz. Ya da son kullanıcı kimlik doğrulamasının Azure Active Directory kullanarak doğrudan gerçekleştirilmediğini.

Kullanılan kimlik doğrulama türünden bağımsız olarak, kaynaklara izin verilen erişim düzeyini kapsam için rol tabanlı erişim denetimi (RBAC) kullanılır. Örneğin, dağıtılan bir modelin erişim belirtecini almak için kullanılan bir hesabın yalnızca çalışma alanına okuma erişimi olması gerekir. RBAC hakkında daha fazla bilgi için bkz. [Azure Machine Learning erişimi yönetme](how-to-assign-roles.md).

## <a name="prerequisites"></a>Ön koşullar

* [Azure Machine Learning çalışma alanı](how-to-manage-workspace.md)oluşturun.
* Azure Machine Learning SDK 'yı yüklemek için [geliştirme ortamınızı yapılandırın](how-to-configure-environment.md) veya SDK 'nın zaten yüklü olduğu bir [Azure MACHINE LEARNING Not defteri VM](concept-azure-machine-learning-architecture.md#compute-instance) 'si kullanın.

## <a name="interactive-authentication"></a>Etkileşimli kimlik doğrulaması

Belgelerde ve örneklerde birçok örnek etkileşimli kimlik doğrulaması kullanır. Örneğin, SDK kullanırken, otomatik olarak Kullanıcı arabirimi tabanlı kimlik doğrulama akışı isteyen iki işlev çağrısı vardır:

* İşlevi çağırmak `from_config()` , istem olarak verilecek.

    ```python
    from azureml.core import Workspace
    ws = Workspace.from_config()
    ```

    `from_config()` işlevi, çalışma alanı bağlantı bilgilerinizi içeren bir JSON dosyası arar.

* `Workspace`Aboneliği, kaynak grubunu ve çalışma alanı bilgilerini sağlamak için Oluşturucuyu kullanmak etkileşimli kimlik doğrulaması için de istemde yer alacak.

    ```python
    ws = Workspace(subscription_id="your-sub-id",
                  resource_group="your-resource-group-id",
                  workspace_name="your-workspace-name"
                  )
    ```

> [!TIP]
> Birden çok kiracıya erişiminiz varsa, sınıfı içeri aktarmanız ve hedeflediğiniz kiracıyı açıkça tanımlamanız gerekebilir. Oluşturucusunun çağrılması, `InteractiveLoginAuthentication` Yukarıdaki çağrılara benzer bir oturum açma bilgileri de ister.
>
> ```python
> from azureml.core.authentication import InteractiveLoginAuthentication
> interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
> ```

## <a name="service-principal-authentication"></a>Hizmet sorumlusu kimlik doğrulaması

Hizmet sorumlusu (SP) kimlik doğrulamasını kullanmak için, önce SP 'yi oluşturmanız ve çalışma alanınıza erişim vermeniz gerekir. Daha önce belirtildiği gibi, erişimi denetlemek için Azure rol tabanlı erişim denetimi (RBAC) kullanılır, bu nedenle SP 'ye verilecek erişime de karar vermelisiniz.

> [!IMPORTANT]
> Hizmet sorumlusu kullanırken, için kullanılan __görev için gereken en düşük erişimi__ verin. Örneğin, için tümü bir Web dağıtımı için erişim belirtecini okumasından, bir hizmet sorumlusu sahibine veya katkıda bulunan erişime izin vermezsiniz.
>
> En az erişim verme nedeni, hizmet sorumlusunun kimlik doğrulaması için bir parola kullanması ve parolanın bir Otomasyon betiğinin parçası olarak depolanması olabilir. Parolanın sızmış olması halinde, belirli bir görev için gereken en düşük erişime sahip olmak, SP 'nin kötü amaçlı kullanımını en aza indirir.

SP oluşturmanın en kolay yolu [Azure CLI](https://docs.microsoft.com/cli/azure/install-azure-cli?view=azure-cli-latest)'yı kullanarak çalışma alanınıza erişim izni vermenizi sağlar. Bir hizmet sorumlusu oluşturmak ve çalışma alanınıza erişim sağlamak için aşağıdaki adımları kullanın:

> [!NOTE]
> Bu adımların tümünü gerçekleştirmek için abonelikte yönetici olmanız gerekir.

1. Azure aboneliğinizde kimlik doğrulaması yapın:

    ```azurecli-interactive
    az login
    ```

    CLI varsayılan tarayıcınızı açabiliyorsa, tarayıcıyı açar ve oturum açma sayfasını yükler. Aksi takdirde, bir tarayıcı açmanız ve komut satırındaki yönergeleri izlemeniz gerekir. Yönergeler, [https://aka.ms/devicelogin](https://aka.ms/devicelogin) bir yetkilendirme koduna göz atmaya ve girmeye yönelik bilgiler içerir.

    [!INCLUDE [select-subscription](../../includes/machine-learning-cli-subscription.md)] 

    Diğer kimlik doğrulama yöntemleri için bkz. [Azure CLI Ile oturum açma](https://docs.microsoft.com/cli/azure/authenticate-azure-cli?view=azure-cli-latest).

1. Azure Machine Learning uzantısını yükler:

    ```azurecli-interactive
    az extension add -n azure-cli-ml
    ```

1. Hizmet sorumlusu oluşturun. Aşağıdaki örnekte, **ml-auth** ADLı bir SP oluşturulur:

    ```azurecli-interactive
    az ad sp create-for-rbac --sdk-auth --name ml-auth
    ```

    Çıktı aşağıdakine benzer bir JSON olacaktır. `clientId`, `clientSecret` , Ve `tenantId` alanlarını, bu makaledeki diğer adımlar için ihtiyaç duyduğunuz gibi bir yere göz atın.

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

1. Önceki adımda döndürülen değeri kullanarak hizmet sorumlusunun ayrıntılarını alın `clientId` :

    ```azurecli-interactive
    az ad sp show --id your-client-id
    ```

    Aşağıdaki JSON, komuttan gelen çıktının basitleştirilmiş bir örneğidir. Bir `objectId` sonraki adım için değerine ihtiyacınız olacak şekilde, alanı buraya göz atın.

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

1. SP 'nin Azure Machine Learning çalışma alanınıza erişmesine izin verin. , Ve parametreleri için çalışma alanınızın adı ve kaynak grubu adı gerekir `-w` `-g` . Parametresi için `--user` , `objectId` önceki adımdaki değeri kullanın. `--role`Parametresi, hizmet sorumlusu için erişim rolünü ayarlamanıza olanak sağlar. Aşağıdaki örnekte, SP, **sahip** rolüne atanır. 

    > [!IMPORTANT]
    > Sahip erişimi, hizmet sorumlusunun çalışma alanınızdaki neredeyse her türlü işlemi yapmasına izin verir. Bu belgede erişim izni vermeyi göstermek için kullanılır; bir üretim ortamında, Microsoft Hizmet sorumlusuna, istediğiniz rolü gerçekleştirmek için gereken en düşük erişimi vermeyi önerir. Daha fazla bilgi için bkz. [Azure Machine Learning erişimi yönetme](how-to-assign-roles.md).

    ```azurecli-interactive
    az ml workspace share -w your-workspace-name -g your-resource-group-name --user your-sp-object-id --role owner
    ```

    Bu çağrı, başarı durumunda herhangi bir çıktı oluşturmaz.

### <a name="use-a-service-principal-from-the-sdk"></a>SDK 'dan bir hizmet sorumlusu kullanma

SDK 'dan çalışma alanınızda kimlik doğrulaması gerçekleştirmek için hizmet sorumlusunu kullanarak `ServicePrincipalAuthentication` sınıf oluşturucusunu kullanın. Hizmet sağlayıcısını parametre olarak oluştururken aldığınız değerleri kullanın. `tenant_id`Parametresi, `tenantId` yukarıdakiyle eşlenir, ile `service_principal_id` eşlenir `clientId` ve ile `service_principal_password` eşlenir `clientSecret` .

```python
from azureml.core.authentication import ServicePrincipalAuthentication

sp = ServicePrincipalAuthentication(tenant_id="your-tenant-id", # tenantID
                                    service_principal_id="your-client-id", # clientId
                                    service_principal_password="your-client-secret") # clientSecret
```

`sp`Değişken artık doğrudan SDK 'da kullandığınız bir kimlik doğrulama nesnesini barındırır. Genel olarak, aşağıdaki kodda gösterildiği gibi, yukarıda kullanılan kimlikleri/gizli dizileri ortam değişkenlerinde depolamak iyi bir fikirdir. Ortam değişkenlerinde depolama, bilgilerin yanlışlıkla bir GitHub deposuna denetlenmesini önler.

```python
import os

sp = ServicePrincipalAuthentication(tenant_id=os.environ['AML_TENANT_ID'],
                                    service_principal_id=os.environ['AML_PRINCIPAL_ID'],
                                    service_principal_password=os.environ['AML_PRINCIPAL_PASS'])
```

Python 'da çalışan ve SDK 'Yı ilk olarak kullanan otomatik iş akışları için bu nesneyi, kimlik doğrulamalarınız için çoğu durumda olduğu gibi kullanabilirsiniz. Aşağıdaki kod, oluşturduğunuz auth nesnesini kullanarak çalışma alanınızın kimliğini doğrular.

```python
from azureml.core import Workspace

ws = Workspace.get(name="ml-example",
                   auth=sp,
                   subscription_id="your-sub-id")
ws.get_details()
```

### <a name="use-a-service-principal-from-the-azure-cli"></a>Azure CLı 'dan bir hizmet sorumlusu kullanma

Azure CLı komutları için hizmet sorumlusu kullanabilirsiniz. Daha fazla bilgi için bkz. [hizmet sorumlusu kullanarak oturum açma](https://docs.microsoft.com/cli/azure/create-an-azure-service-principal-azure-cli?view=azure-cli-latest#sign-in-using-a-service-principal).

### <a name="use-a-service-principal-with-the-rest-api-preview"></a>REST API (Önizleme) ile hizmet sorumlusu kullanma

Hizmet sorumlusu Ayrıca Azure Machine Learning [REST API](https://docs.microsoft.com/rest/api/azureml/) (Önizleme) kimlik doğrulaması için de kullanılabilir. Otomatik iş akışlarında gözetimsiz kimlik doğrulaması için hizmetten hizmete çağrılara izin veren Azure Active Directory [istemci kimlik bilgileri verme akışı](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)' nı kullanırsınız. Örnekler hem Python hem de Node.js için [adal kitaplığı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) ile uygulanır, ancak OpenID Connect 1,0 'yi destekleyen herhangi bir açık kaynak kitaplığı da kullanabilirsiniz.

> [!NOTE]
> MSAL.js, ADAL 'dan daha yeni bir kitaplıktır, ancak özellikle belirli bir kullanıcıya bağlı etkileşimli/UI kimlik doğrulaması için tasarlanan bir istemci tarafı kitaplığı olduğundan, MSAL.js istemci kimlik bilgilerini kullanarak hizmetten hizmete kimlik doğrulaması yapılamaz. REST API ile otomatikleştirilmiş iş akışları oluşturmak için aşağıda gösterildiği gibi ADAL kullanmanızı öneririz.

#### <a name="nodejs"></a>Node.js

Node.js kullanarak bir kimlik doğrulama belirteci oluşturmak için aşağıdaki adımları kullanın. Ortamınızda öğesini çalıştırın `npm install adal-node` . Daha sonra, `tenantId` `clientId` ve `clientSecret` Yukarıdaki adımlarda oluşturduğunuz hizmet sorumlusunun aşağıdaki betikteki eşleşen değişkenlerin değerleri olarak değerini kullanın.

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

Değişkeni, `tokenResponse` belirteci ve sona erme saati gibi ilişkili meta verileri içeren bir nesnedir. Belirteçler 1 saat için geçerlidir ve yeni bir belirteç almak için aynı çağrı çalıştırılarak yenilenebilir. Aşağıdaki kod parçacığı örnek bir yanıttır.

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

`accessToken`Kimlik doğrulama belirtecini getirmek için özelliğini kullanın. API çağrıları yapmak için belirteci kullanma hakkında örnekler için [REST API belgelerine](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) bakın.

#### <a name="python"></a>Python

Python kullanarak bir kimlik doğrulama belirteci oluşturmak için aşağıdaki adımları kullanın. Ortamınızda öğesini çalıştırın `pip install adal` . Daha sonra, `tenantId` `clientId` ve `clientSecret` Yukarıdaki adımlarda oluşturduğunuz hizmet sorumlusunun, aşağıdaki komut dosyasında uygun değişkenlerin değerleri olarak değerlerini kullanın.

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

Değişken, `token_response` belirteci ve sona erme saati gibi ilişkili meta verileri içeren bir sözlüktür. Belirteçler 1 saat için geçerlidir ve yeni bir belirteç almak için aynı çağrı çalıştırılarak yenilenebilir. Aşağıdaki kod parçacığı örnek bir yanıttır.

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

`token_response["accessToken"]`Kimlik doğrulama belirtecini getirmek için kullanın. API çağrıları yapmak için belirteci kullanma hakkında örnekler için [REST API belgelerine](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) bakın.

## <a name="web-service-authentication"></a>Web hizmeti kimlik doğrulaması

Azure Machine Learning tarafından oluşturulan model dağıtımları iki kimlik doğrulama yöntemi sağlar:

* **anahtar tabanlı**: Web hizmetinde kimlik doğrulaması yapmak için statik anahtar kullanılır.
* **belirteç tabanlı**: bir geçici belirtecin çalışma alanından alınması ve Web hizmetinde kimlik doğrulaması yapmak için kullanılması gerekir. Bu belirtecin bir süre sonra süresi dolar ve Web hizmetiyle çalışmaya devam etmek için yenilenmesi gerekir.

    > [!NOTE]
    > Belirteç tabanlı kimlik doğrulaması yalnızca Azure Kubernetes hizmetine dağıtılmasında kullanılabilir.

### <a name="key-based-web-service-authentication"></a>Anahtar tabanlı Web hizmeti kimlik doğrulaması

Azure Kubernetes Service (AKS) üzerinde dağıtılan Web Hizmetleri, varsayılan olarak anahtar tabanlı kimlik doğrulama *etkindir* . Azure Container Instances (ACI) dağıtılan hizmetlerde varsayılan olarak anahtar tabanlı kimlik doğrulaması *devre dışıdır* , ancak `auth_enabled=True` aci Web-Service oluşturma sırasında ayarı yaparak etkinleştirebilirsiniz. Aşağıdaki kod, anahtar tabanlı kimlik doğrulaması etkinleştirilmiş bir acı dağıtım yapılandırması oluşturma örneğidir.

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(cpu_cores = 1,
                                                memory_gb = 1,
                                                auth_enabled=True)
```

Daha sonra, sınıfını kullanarak dağıtımda özel ACI yapılandırmasını kullanabilirsiniz `Model` .

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

Kimlik doğrulama anahtarlarını getirmek için kullanın `aci_service.get_keys()` . Bir anahtarı yeniden oluşturmak için, `regen_key()` işlevini kullanın ve **birincil** ya da **İkincil**olarak geçirin.

```python
aci_service.regen_key("Primary")
# or
aci_service.regen_key("Secondary")
```

Dağıtılan bir modele kimlik doğrulama hakkında daha fazla bilgi için bkz. [Web hizmeti olarak dağıtılan bir model için Istemci oluşturma](how-to-consume-web-service.md).

### <a name="token-based-web-service-authentication"></a>Belirteç tabanlı Web hizmeti kimlik doğrulaması

Bir Web hizmeti için belirteç kimlik doğrulamasını etkinleştirdiğinizde, kullanıcılar, Web hizmetine erişmek için bir Azure Machine Learning JSON Web Token sunmalıdır. Belirtilen sürenin sonunda belirteç geçersiz olur ve çağrı yapmaya devam edilebilmesi için yenilenmesi gerekir.

* Belirteç kimlik doğrulaması, Azure Kubernetes hizmetine dağıtırken **Varsayılan olarak devre dışıdır** .
* Azure Container Instances ' a dağıtırken belirteç kimlik doğrulaması **desteklenmez** .
* Belirteç kimlik doğrulaması **, anahtar tabanlı kimlik doğrulama ile aynı anda kullanılamaz**.

Belirteç kimlik doğrulamasını denetlemek için, `token_auth_enabled` bir dağıtım oluştururken veya güncelleştirdiğinizde parametresini kullanın:

```python
from azureml.core.webservice import AksWebservice
from azureml.core.model import Model, InferenceConfig

# Create the config
aks_config = AksWebservice.deploy_configuration()

#  Enable token auth and disable (key) auth on the webservice
aks_config = AksWebservice.deploy_configuration(token_auth_enabled=True, auth_enabled=False)

aks_service_name ='aks-service-1'

# deploy the model
aks_service = Model.deploy(workspace=ws,
                           name=aks_service_name,
                           models=[model],
                           inference_config=inference_config,
                           deployment_config=aks_config,
                           deployment_target=aks_target)

aks_service.wait_for_deployment(show_output = True)
```

Belirteç kimlik doğrulaması etkinleştirilirse, `get_token` bir JSON Web token (JWT) almak için yöntemini ve bu belirtecin sona erme süresini kullanabilirsiniz:

> [!TIP]
> Belirteci almak için bir hizmet sorumlusu kullanırsanız ve bir belirteci almak için gereken en az erişime sahip olmasını istiyorsanız, bunu çalışma alanı için **okuyucu** rolüne atayın.

```python
token, refresh_by = aks_service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirtecin `refresh_by` süresi dolduktan sonra yeni bir belirteç istemeniz gerekir. Belirteçleri Python SDK 'sının dışında yenilemeniz gerekiyorsa, bir seçenek, `service.get_token()` daha önce anlatıldığı gibi, çağrıyı düzenli olarak yapmak için hizmet sorumlusu kimlik doğrulamasıyla REST API kullanmaktır.
>
> Azure Machine Learning çalışma alanınızı Azure Kubernetes hizmet kümeniz ile aynı bölgede oluşturmanızı önemle öneririz.
>
> Bir belirteçle kimlik doğrulaması yapmak için Web hizmeti, Azure Machine Learning çalışma alanınızın oluşturulduğu bölgeye bir çağrı yapar. Çalışma alanınızın bölgesi kullanılamıyorsa, kümeniz çalışma alanınızdan farklı bir bölgede olsa bile, Web hizmetiniz için bir belirteç getirimeyeceksiniz. Sonuç olarak, çalışma alanınızın bölgesi yeniden kullanılabilir olana kadar Azure AD kimlik doğrulaması kullanılamaz.
>
> Ayrıca, kümenizin bölgesi ve çalışma alanınızın bölgesi arasındaki mesafe arttıkça bir belirteç getirmek için o kadar sürer.

## <a name="next-steps"></a>Sonraki adımlar

* [Eğitiminde gizli dizileri kullanma](how-to-use-secrets-in-runs.md).
* [Görüntü sınıflandırma modelini eğitme ve dağıtma](tutorial-train-models-with-aml.md).
* [Web hizmeti olarak dağıtılan bir Azure Machine Learning modeli](how-to-consume-web-service.md)kullanın.
