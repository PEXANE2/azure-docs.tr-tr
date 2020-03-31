---
title: Kimlik doğrulamasını ayarlama
titleSuffix: Azure Machine Learning
description: Azure Machine Learning'de çeşitli kaynaklar ve iş akışları için kimlik doğrulaması nasıl ayarlayıp yapılandırıştırmayı öğrenin. Geliştirme veya sınama amacıyla basit Kullanıcı Arabirimi tabanlı auth'tan tam Azure Active Directory hizmeti asıl kimlik doğrulamasına kadar hizmet içinde kimlik doğrulamayı yapılandırmanın ve kullanmanın birden çok yolu vardır.
services: machine-learning
author: trevorbye
ms.author: trbye
ms.reviewer: trbye
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.date: 12/17/2019
ms.openlocfilehash: fcaa7a0c44851d6b48b40b01af4c8ec992c330b8
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79283543"
---
# <a name="set-up-authentication-for-azure-machine-learning-resources-and-workflows"></a>Azure Machine Learning kaynakları ve iş akışları için kimlik doğrulaması ayarlama
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning'de çeşitli kaynaklar ve iş akışları için kimlik doğrulaması nasıl ayarlayıp yapılandırıştırabileceğinizi öğreneceksiniz. Geliştirme veya sınama amacıyla basit Kullanıcı Arabirimi tabanlı auth'tan tam Azure Active Directory hizmeti asıl kimlik doğrulaması arasında değişen hizmete kimlik doğrulamanın birden çok yolu vardır. Bu makalede, web hizmeti kimlik doğrulamasının nasıl çalıştığı ve Azure Machine Learning REST API'de kimlik doğrulama nın nasıl olduğu arasındaki farklar da açıklanmaktadır.

Bu nasıl yapılacağını gösterir aşağıdaki görevleri nasıl yapacağınızı gösterir:

* Sınama/geliştirme için etkileşimli Kullanıcı Arabirimi kimlik doğrulaması kullanma
* Hizmet temel kimlik doğrulaması ayarlama
* Çalışma alanınıza kimlik doğrulama
* Azure Machine Learning REST API için OAuth2.0 taşıyıcı tipi belirteçleri alın
* Web hizmeti kimlik doğrulamasını anlama

Azure Machine Learning'deki güvenlik ve kimlik doğrulamaya genel bir bakış için [kavram makalesine](concept-enterprise-security.md) bakın.

## <a name="prerequisites"></a>Ön koşullar

* Azure [Machine Learning çalışma alanı](how-to-manage-workspace.md)oluşturun.
* [Geliştirme ortamınızı](how-to-configure-environment.md) Azure Machine Learning SDK'yı yükecek şekilde yapılandırın veya SDK yüklü olan [bir Azure Machine Learning Notebook VM](concept-azure-machine-learning-architecture.md#compute-instance) kullanın.

## <a name="interactive-authentication"></a>Etkileşimli kimlik doğrulaması

Bu hizmetiçin belgelerdeki çoğu örnek, test ve gösterim için basit bir yöntem olarak Jupyter dizüstü bilgisayarlarda etkileşimli kimlik doğrulamakullanır. Bu, ne inşa ettiğinizi test etmek için hafif bir yoldur. Otomatik olarak bir UI tabanlı kimlik doğrulama akışı ile sizi isteyecek iki işlev çağrısı vardır.

İşlevin `from_config()` çağrılması istemi ni verecektir.

```python
from azureml.core import Workspace
ws = Workspace.from_config()
```

İşlev, `from_config()` çalışma alanı bağlantı bilgilerinizi içeren bir JSON dosyası arar. Ayrıca, etkileşimli kimlik doğrulaması için `Workspace` de istenecek olan oluşturucuyu kullanarak bağlantı ayrıntılarını açıkça belirtebilirsiniz. Her iki arama da eşdeğerdir.

```python
ws = Workspace(subscription_id="your-sub-id",
               resource_group="your-resource-group-id",
               workspace_name="your-workspace-name"
              )
```

Birden çok kiracıya erişiminiz varsa, sınıfı içe aktarmanız ve hangi kiracıyı hedeflediğinizi açıkça tanımlamanız gerekebilir. Oluşturucuyu aramak `InteractiveLoginAuthentication` da yukarıdaki aramalara benzer şekilde oturum açmanızı ister.

```python
from azureml.core.authentication import InteractiveLoginAuthentication
interactive_auth = InteractiveLoginAuthentication(tenant_id="your-tenant-id")
```

İnteraktif kimlik doğrulama, test ve öğrenme için yararlı olsa da, otomatik veya başsız iş akışları oluşturmanıza yardımcı olmaz. Hizmet temel kimlik doğrulaması ayarlama SDK kullanan otomatik işlemler için en iyi yaklaşımdır.

## <a name="set-up-service-principal-authentication"></a>Hizmet temel kimlik doğrulaması ayarlama

Bu işlem, otomatik iş akışlarında Azure Machine Learning Python SDK'ya kimlik doğrulamanızı sağlayan belirli bir kullanıcı oturumunu etkinleştirmek için gereklidir. Hizmet ana kimlik doğrulaması da [REST API için kimlik doğrulama](#azure-machine-learning-rest-api-auth)sağlar.

Hizmet temel kimlik doğrulaması ayarlamak için önce Azure Active Directory'de bir uygulama kaydı oluşturur ve ardından UYGULAMAnize ML çalışma alanınıza rol tabanlı erişim verirsiniz. Bu kurulumu tamamlamanın en kolay yolu Azure portalındaki [Azure Bulut Kabuğu'ndan](https://azure.microsoft.com/features/cloud-shell/) geçer. Portala giriş yaptıktan sonra, `>_` kabuğu açmak için adınızın yanındaki sayfanın sağ üst kısmındaki simgeyi tıklatın.

Azure hesabınızda bulut kabuğunu daha önce kullanmadıysanız, yazılmış dosyaları depolamak için bir depolama hesabı kaynağı oluşturmanız gerekir. Genel olarak bu depolama hesabı ihmal edilebilir bir aylık maliyete tabi olacaktır. Ayrıca, daha önce aşağıdaki komutla kullanmadıysanız makine öğrenimi uzantısını yükleyin.

```azurecli-interactive
az extension add -n azure-cli-ml
```

> [!NOTE]
> Aşağıdaki adımları gerçekleştirmek için abonelikte yönetici olmalısınız.

Ardından, hizmet ilkesini oluşturmak için aşağıdaki komutu çalıştırın. Bir isim ver, bu durumda **ml-auth**.

```azurecli-interactive
az ad sp create-for-rbac --sdk-auth --name ml-auth
```

Çıktı aşağıdakine benzer bir JSON olacaktır. Bu makaledeki `clientId` `clientSecret`diğer `tenantId` adımlar için ihtiyacınız olacağı için , , ve alanları dikkate alın.

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

Ardından, `clientId` `--id` parametreye giriş olarak yukarıdan gelen değeri kullanarak, yeni oluşturduğunuz hizmet ilkesiyle ilgili ayrıntıları almak için aşağıdaki komutu çalıştırın.

```azurecli-interactive
az ad sp show --id your-client-id
```

Aşağıda komuttan JSON çıkışıbasitleştirilmiş bir örneği dir. Bir sonraki `objectId` adım için değerine ihtiyacınız olacağından, alana dikkat edin.

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

Ardından, makine öğrenimi çalışma alanınıza hizmet temel erişimiatamak için aşağıdaki komutu kullanın. Çalışma alanı adınızı ve kaynak grup adı `-w` ve `-g` parametreler için sırasıyla gerekir. Parametre `--user` için önceki `objectId` adımdaki değeri kullanın. `--role` Parametre, hizmet sorumlusunun erişim rolünü ayarlamanızı sağlar ve genel olarak **sahibinizi** veya **katkıda bulunanı**kullanırsınız. Her ikisi de bilgi işlem kümeleri ve veri depoları gibi varolan kaynaklara yazma erişimine sahiptir, ancak bu kaynakları yalnızca **sahibi** sağlayabilir. 

```azurecli-interactive
az ml workspace share -w your-workspace-name -g your-resource-group-name --user your-sp-object-id --role owner
```

Bu çağrı herhangi bir çıktı üretmez, ancak artık çalışma alanınız için hizmet temel kimlik doğrulaması ayarlanmıştır.

## <a name="authenticate-to-your-workspace"></a>Çalışma alanınızda kimlik doğrulaması

Artık hizmet ana tr'niz etkinleştirildiğinden, kullanıcı olarak fiziksel olarak oturum açmadan SDK'daki çalışma alanınıza kimlik doğrulayabilirsiniz. Sınıf `ServicePrincipalAuthentication` oluşturucuyu kullanın ve önceki adımlardan aldığınız değerleri parametre olarak kullanın. Parametre `tenant_id` haritaları `tenantId` yukarıdan, `service_principal_id` haritalar `clientId`için `service_principal_password` , `clientSecret`ve haritalar .

```python
from azureml.core.authentication import ServicePrincipalAuthentication

sp = ServicePrincipalAuthentication(tenant_id="your-tenant-id", # tenantID
                                    service_principal_id="your-client-id", # clientId
                                    service_principal_password="your-client-secret") # clientSecret
```

Değişken `sp` şimdi doğrudan SDK'da kullandığınız bir kimlik doğrulama nesnesini tutar. Genel olarak, aşağıdaki kodda gösterildiği gibi, yukarıda kullanılan kimlikleri/sırları ortam değişkenlerinde saklamak iyi bir fikirdir.

```python
import os 

sp = ServicePrincipalAuthentication(tenant_id=os.environ['AML_TENANT_ID'],
                                    service_principal_id=os.environ['AML_PRINCIPAL_ID'],
                                    service_principal_password=os.environ['AML_PRINCIPAL_PASS'])
```

Python'da çalışan ve öncelikle SDK'yı kullanan otomatik iş akışları için, bu nesneyi çoğu durumda kimlik doğrulamanız için olduğu gibi kullanabilirsiniz. Aşağıdaki kod, az önce oluşturduğunuz auth nesnesini kullanarak çalışma alanınıza doğrulanır.

```python
from azureml.core import Workspace

ws = Workspace.get(name="ml-example", 
                   auth=sp,
                   subscription_id="your-sub-id")
ws.get_details()
```

## <a name="azure-machine-learning-rest-api-auth"></a>Azure Machine Learning REST API auth

Yukarıdaki adımlarda oluşturulan hizmet ilkesi, Azure Machine Learning [REST API'nın](https://docs.microsoft.com/rest/api/azureml/)kimliğini doğrulamak için de kullanılabilir. Otomatik iş akışlarında başsız kimlik doğrulaması için hizmetten hizmete çağrılara izin veren Azure Active Directory [istemci kimlik bilgilerini](https://docs.microsoft.com/azure/active-directory/develop/v1-oauth2-client-creds-grant-flow)kullanırsınız. Örnekler Hem Python hem de Node.js'de [ADAL kitaplığı](https://docs.microsoft.com/azure/active-directory/develop/active-directory-authentication-libraries) ile uygulanır, ancak OpenID Connect 1.0'ı destekleyen açık kaynak kitaplığını da kullanabilirsiniz. 

> [!NOTE]
> MSAL.js, ADAL'dan daha yeni bir kitaplıktır, ancak öncelikle belirli bir kullanıcıya bağlı etkileşimli/Kullanıcı Arabirimi kimlik doğrulaması için tasarlanmış bir istemci tarafı kitaplığı olduğundan, MSAL.js ile istemci kimlik bilgilerini kullanarak hizmete hizmet kimlik doğrulaması yapamazsınız. REST API ile otomatik iş akışları oluşturmak için aşağıda gösterildiği gibi ADAL kullanmanızı öneririz.

### <a name="nodejs"></a>Node.js

Node.js kullanarak auth belirteci oluşturmak için aşağıdaki adımları kullanın. Çevrenizde çalıştırın. `npm install adal-node` Ardından, yukarıdaki `tenantId` `clientId`adımlarda `clientSecret` oluşturduğunuz hizmet sorumlusu ve aşağıdaki komut dosyasındaki eşleşen değişkenler için değerler olarak ,' ınızı kullanın.

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

Değişken, `tokenResponse` son kullanma süresi gibi belirteç ve ilişkili meta verileri içeren bir nesnedir. Belirteçler 1 saat için geçerlidir ve yeni bir belirteç almak için aynı aramayı yeniden çalıştırarak yenilenebilir. Aşağıda örnek bir yanıt veremistir.

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

Auth `accessToken` belirteci getirmek için özelliği kullanın. BELIRTeçle ilgili örnekler için [REST API belgelerine](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) bakın.

### <a name="python"></a>Python 

Python kullanarak auth belirteci oluşturmak için aşağıdaki adımları kullanın. Çevrenizde çalıştırın. `pip install adal` Ardından, yukarıdaki `tenantId` `clientId`adımlarda `clientSecret` oluşturduğunuz hizmet sorumlusu ve aşağıdaki komut dosyasındaki uygun değişkenler için değer olarak , ve hizmet sorumlusundan kullanın.

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

Değişken, `token_response` son kullanma süresi gibi belirteç ve ilişkili meta verileri içeren bir sözlüktür. Belirteçler 1 saat için geçerlidir ve yeni bir belirteç almak için aynı aramayı yeniden çalıştırarak yenilenebilir. Aşağıda örnek bir yanıt veremistir.

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

Auth belirteci getirmek için kullanın. `token_response["accessToken"]` BELIRTeçle ilgili örnekler için [REST API belgelerine](https://github.com/microsoft/MLOps/tree/master/examples/AzureML-REST-API) bakın.

## <a name="web-service-authentication"></a>Web hizmeti kimlik doğrulaması

Azure Machine Learning'deki Web hizmetleri, yukarıda açıklanandan farklı bir kimlik doğrulama deseni kullanır. Dağıtılan web hizmetlerine kimlik doğrulamanın en kolay yolu, yenilenmesi gerekmeyen statik taşıyıcı türü kimlik doğrulama anahtarları oluşturan **anahtar tabanlı kimlik doğrulamasını**kullanmaktır. Yalnızca dağıtılan bir web hizmetine kimlik doğrulamanız gerekiyorsa, yukarıda gösterildiği gibi hizmet ilkesi kimlik doğrulaması ayarlamanız gerekmez.

Azure Kubernetes Hizmeti'nde dağıtılan web hizmetleri varsayılan olarak anahtar tabanlı auth *özelliğine sahiptir.* Azure Kapsayıcı Örnekleri dağıtılan hizmetler varsayılan olarak anahtar tabanlı auth devre dışı `auth_enabled=True` *bırakılmış,* ancak ACI web hizmetini oluştururken ayarlayarak bunu etkinleştirebilirsiniz. Aşağıda, anahtar tabanlı auth etkinleştirilmiş bir ACI dağıtım yapılandırması oluşturma örneği verilmiştir.

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(cpu_cores = 1,
                                                memory_gb = 1,
                                                auth_enabled=True)
```

Ardından, sınıfı kullanarak dağıtımda özel ACI yapılandırmasını `Model` kullanabilirsiniz.

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

Auth tuşlarını almak `aci_service.get_keys()`için. Bir anahtarı yeniden oluşturmak `regen_key()` için, işlevi kullanın ve **Birincil** veya **İkincil'den geçirin.**

```python
aci_service.regen_key("Primary")
# or
aci_service.regen_key("Secondary")
```

Web hizmetleri, belirteç tabanlı kimlik doğrulamayı da destekler, ancak yalnızca Azure Kubernetes Hizmeti dağıtımları için. Kimlik doğrulaması hakkında ek bilgi için web hizmetlerinin [nasıl tüketilmesi](how-to-consume-web-service.md) gerektiğini görün.

### <a name="token-based-web-service-authentication"></a>Belirteç tabanlı web hizmeti kimlik doğrulaması

Bir web hizmeti için belirteç kimlik doğrulaması etkinleştirdiğinizde, kullanıcılara erişmek için web hizmetine bir Azure Machine Learning JSON Web Belirteci sunmalıdır. Belirteç belirli bir zaman diliminden sonra sona erer ve arama yapmaya devam etmek için yenilenmesi gerekir.

* Azure Kubernetes Hizmetine dağıtırken belirteç kimlik doğrulaması **varsayılan olarak devre dışı bırakılır.**
* Azure Kapsayıcı Örnekleri'ne dağıttığınızda belirteç kimlik doğrulaması **desteklenmez.**

Belirteç kimlik doğrulamasını `token_auth_enabled` denetlemek için, dağıtım oluştururken veya güncellerken parametreyi kullanın.

Belirteç kimlik doğrulaması etkinse, bir JSON Web Belirteci (JWT) almak için `get_token` yöntemi kullanabilirsiniz ve bu belirteç son kullanma süresi:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirteç `refresh_by` saatinden sonra yeni bir belirteç talep etmeniz gerekir. Python SDK dışındaki belirteçleri yenilemeniz gerekiyorsa, daha önce tartışıldığı gibi `service.get_token()` aramayı düzenli aralıklarla yapmak için REST API'yi hizmet temel kimlik doğrulaması ile kullanmak tır.
>
> Azure Makine Öğrenimi çalışma alanınızı Azure Kubernetes Hizmet kümenizle aynı bölgede oluşturmanızı şiddetle öneririz. 
>
> Web hizmeti, bir belirteçle kimlik doğrulaması yapmak için Azure Machine Learning çalışma alanınızın oluşturulduğu bölgeye bir arama yapar. Çalışma alanınızın bölgesi kullanılamıyorsa, kümeniz çalışma alanınızdan farklı bir bölgede olsa bile web hizmetiniz için bir belirteç getiremezsiniz. Sonuç olarak, çalışma alanınız bölgeniz yeniden kullanılabilir olana kadar Azure AD Kimlik Doğrulaması kullanılamıyor. 
>
> Ayrıca, kümenizin bölgesi yle çalışma alanınızın bölgesi arasındaki mesafe ne kadar büyükse, bir belirteci getirmesi o kadar uzun sürer.

## <a name="next-steps"></a>Sonraki adımlar

* [Bir görüntü sınıflandırma modeli eğitin ve dağıtın.](tutorial-train-models-with-aml.md)
* [Web hizmeti olarak dağıtılan bir Azure Machine Learning modelini tüketin.](how-to-consume-web-service.md)
