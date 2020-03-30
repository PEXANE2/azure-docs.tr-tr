---
title: TLS kullanarak güvenli web hizmetleri
titleSuffix: Azure Machine Learning
description: Azure Machine Learning aracılığıyla dağıtılan bir web hizmetini güvence altına almak için HTTPS'yi nasıl etkinleştireceklerini öğrenin. Azure Machine Learning, web hizmetleri olarak dağıtılan modelleri güvenli hale getirmek için TLS sürüm 1.2'yi kullanır.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.reviewer: jmartens
ms.author: aashishb
author: aashishb
ms.date: 03/05/2020
ms.custom: seodec18
ms.openlocfilehash: a58b0120feaba907c62bc646f4f85d9185227fed
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80287348"
---
# <a name="use-tls-to-secure-a-web-service-through-azure-machine-learning"></a>Azure Machine Learning aracılığıyla bir web hizmetini güvence altına almak için TLS'yi kullanın
[!INCLUDE [applies-to-skus](../../includes/aml-applies-to-basic-enterprise-sku.md)]

Bu makalede, Azure Machine Learning aracılığıyla dağıtılan bir web hizmetinin nasıl güvence altına alabileceğiniz gösterilmektedir.

WEB hizmetlerine erişimi kısıtlamak ve istemcilerin gönderdiği verileri güvence altına almak için [HTTPS'yi](https://en.wikipedia.org/wiki/HTTPS) kullanırsınız. HTTPS, ikisi arasındaki iletişimi şifreleyerek bir istemci ve bir web hizmeti arasındaki iletişimin güvenliğini sağlamaya yardımcı olur. Şifreleme, [Aktarım Katmanı Güvenliği 'ni (TLS)](https://en.wikipedia.org/wiki/Transport_Layer_Security)kullanır. TLS bazen hala TLS selefi olan *Güvenli Soketler Katmanı* (SSL) olarak adlandırılır.

> [!TIP]
> Azure Machine Learning SDK, güvenli iletişimle ilgili özellikler için "SSL" terimini kullanır. Bu, web hizmetinizin *TLS*kullanmadığı anlamına gelmez. SSL sadece daha yaygın olarak tanınan bir terimdir.
>
> Özellikle, Azure Machine Learning aracılığıyla dağıtılan web hizmetleri yalnızca TLS sürüm 1.2'yi destekler.

TLS ve SSL, şifreleme ve kimlik doğrulamaya yardımcı olan *dijital sertifikalara*güvenir. Dijital sertifikaların nasıl çalıştığı hakkında daha fazla bilgi için Vikipedi konusu [Ortak anahtar altyapısına](https://en.wikipedia.org/wiki/Public_key_infrastructure)bakın.

> [!WARNING]
> HTTPS'yi web hizmetiniz için kullanmıyorsanız, hizmete gönderilen ve hizmetten gönderilen veriler internetteki diğer kişiler tarafından görülebilir.
>
> HTTPS ayrıca istemcinin bağlandığı sunucunun orijinalliğini doğrulamasını da sağlar. Bu özellik, istemcileri [ortadaki adam](https://en.wikipedia.org/wiki/Man-in-the-middle_attack) saldırılarına karşı korur.

Bu, bir web hizmetini güvence altına almak için genel bir işlemdir:

1. Bir etki alanı adı alın.

2. Dijital sertifika alın.

3. TLS etkinken web hizmetini dağıtın veya güncelleyin.

4. Web hizmetini işaret etmek için DNS'nizi güncelleştirin.

> [!IMPORTANT]
> Azure Kubernetes Hizmeti'ne (AKS) dağıtım yapıyorsanız, kendi sertifikanızı satın alabilir veya Microsoft tarafından sağlanan bir sertifikayı kullanabilirsiniz. Microsoft'tan bir sertifika kullanıyorsanız, bir etki alanı adı veya TLS/SSL sertifikası almanız gerekmez. Daha fazla bilgi için [TLS'yi etkinleştir ve](#enable) bu makalenin dağıt bölümüne bakın.

[Dağıtım hedefleri](how-to-deploy-and-where.md)arasında s'yi güvenli hale aldığınızda küçük farklar vardır.

## <a name="get-a-domain-name"></a>Etki alanı adı alma

Zaten bir etki alanı adınız yoksa, bir *etki alanı adı kayıt şirketinden*satın alın. Süreç ve fiyat kayıt şirketleri arasında farklılık gösterir. Kayıt şirketi etki alanı adını yönetmek için araçlar sağlar. Bu araçları, tam nitelikli bir alan adı (FQDN)\.(www contoso.com gibi) web hizmetinizi barındıran IP adresiyle eşlemek için kullanırsınız.

## <a name="get-a-tlsssl-certificate"></a>TLS/SSL sertifikası alın

TLS/SSL sertifikası (dijital sertifika) almanın birçok yolu vardır. En yaygın olanı bir *sertifika yetkilisinden* (CA) satın almaktır. Sertifikayı nereden aldığınızdan bağımsız olarak, aşağıdaki dosyalara ihtiyacınız var:

* Bir **sertifika**. Sertifikanın tam sertifika zincirini içermesi ve "PEM kodlanmış" olması gerekir.
* Bir **anahtar.** Anahtar da PEM kodlanmış olmalıdır.

Sertifika istediğinizde, web hizmeti için kullanmayı planladığınız adresin FQDN'sini sağlamanız gerekir\.(örneğin, www contoso.com). Sertifikaya damgalanan adres ve istemcilerin kullandığı adres, web hizmetinin kimliğini doğrulamak için karşılaştırılır. Bu adresler eşleşmiyorsa, istemci bir hata iletisi alır.

> [!TIP]
> Sertifika yetkilisi sertifikayı ve anahtarı PEM kodlanmış dosyalar olarak sağlayamazsa, biçimi değiştirmek için [OpenSSL](https://www.openssl.org/) gibi bir yardımcı program kullanabilirsiniz.

> [!WARNING]
> Yalnızca geliştirme için *kendi imzalı* sertifikaları kullanın. Bunları üretim ortamlarında kullanmayın. Kendi imzalı sertifikalar istemci uygulamalarınızda sorunlara neden olabilir. Daha fazla bilgi için istemci uygulamanızın kullandığı ağ kitaplıkları için belgelere bakın.

## <a name="enable-tls-and-deploy"></a><a id="enable"></a>TLS'yi etkinleştirin ve dağıtın

TLS etkinleştirilmiş hizmeti dağıtmak (veya yeniden dağıtmak için, *ssl_enabled* parametresini uygun olduğu her yerde "True" olarak ayarlayın. *ssl_certificate* parametresini *sertifika* dosyasının değerine ayarlayın. *ssl_key* *anahtar* dosyasının değerine ayarlayın.

### <a name="deploy-on-aks-and-field-programmable-gate-array-fpga"></a>AKS ve alan programlanabilir geçit dizisinde (FPGA) dağıtma

  > [!NOTE]
  > Bu bölümdeki bilgiler, tasarımcı için güvenli bir web hizmeti dağıttığınızda da geçerlidir. Python SDK'yı kullanmaya aşina değilseniz, [Python için Azure Machine Learning SDK nedir?](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)

AKS'ye dağıtırken, yeni bir AKS kümesi oluşturabilir veya varolan bir küme ekleyebilirsiniz. Küme oluşturma veya ekleme hakkında daha fazla bilgi için [bkz.](how-to-deploy-azure-kubernetes-service.md)
  
-  Yeni bir küme oluşturursanız, **[AksCompute.provisioning_configuration()](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#provisioning-configuration-agent-count-none--vm-size-none--ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--location-none--vnet-resourcegroup-name-none--vnet-name-none--subnet-name-none--service-cidr-none--dns-service-ip-none--docker-bridge-cidr-none--cluster-purpose-none--load-balancer-type-none--load-balancer-subnet-none-)** kullanın.
- Varolan bir küme yi bağlarsanız, **[AksCompute.attach_configuration()](/python/api/azureml-core/azureml.core.compute.akscompute?view=azure-ml-py#attach-configuration-resource-group-none--cluster-name-none--resource-id-none--cluster-purpose-none-)** kullanın. Her ikisi de **enable_ssl** yöntemi olan bir yapılandırma nesnesi döndürün.

**enable_ssl** yöntemi, Microsoft tarafından sağlanan bir sertifikayı veya satın aldığınız bir sertifikayı kullanabilir.

  * Microsoft'tan bir sertifika kullandığınızda, *leaf_domain_label* parametresini kullanmanız gerekir. Bu parametre, hizmetiçin DNS adını oluşturur. Örneğin, "contoso" değeri "contoso\<altı-rasgele karakterler> bir etki alanı adı oluşturur. \<azureregion>.cloudapp.azure.com", azureregion> hizmeti içeren bölgedir. \< İsteğe bağlı olarak, varolan *leaf_domain_label*üzerine yazmak için *overwrite_existing_domain* parametresini kullanabilirsiniz.

    TLS etkinleştirilmiş hizmeti dağıtmak (veya yeniden dağıtmak için, *ssl_enabled* parametresini uygun olduğu her yerde "True" olarak ayarlayın. *ssl_certificate* parametresini *sertifika* dosyasının değerine ayarlayın. *ssl_key* *anahtar* dosyasının değerine ayarlayın.

    > [!IMPORTANT]
    > Microsoft'tan bir sertifika kullandığınızda, kendi sertifikanızı veya etki alanı adınızı satın almanız gerekmez.

    Aşağıdaki örnek, Microsoft'tan TLS/SSL sertifikası sağlayan bir yapılandırmanın nasıl oluşturulabildiğini gösterir:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable TLS
    provisioning_config = AksCompute.provisioning_configuration()
    # Leaf domain label generates a name using the formula
    #  "<leaf-domain-label>######.<azure-region>.cloudapp.azure.net"
    #  where "######" is a random series of characters
    provisioning_config.enable_ssl(leaf_domain_label = "contoso")


    # Config used to attach an existing AKS cluster to your workspace and enable TLS
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                          cluster_name = cluster_name)
    # Leaf domain label generates a name using the formula
    #  "<leaf-domain-label>######.<azure-region>.cloudapp.azure.net"
    #  where "######" is a random series of characters
    attach_config.enable_ssl(leaf_domain_label = "contoso")
    ```

  * *Satın aldığınız bir sertifikayı*kullandığınızda, *ssl_cert_pem_file*, *ssl_key_pem_file*ve *ssl_cname* parametrelerini kullanırsınız. Aşağıdaki örnek, satın aldığınız TLS/SSL sertifikasını kullanan bir yapılandırma oluşturmak için *.pem* dosyalarının nasıl kullanılacağını gösterir:

    ```python
    from azureml.core.compute import AksCompute
    # Config used to create a new AKS cluster and enable TLS
    provisioning_config = AksCompute.provisioning_configuration()
    provisioning_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    # Config used to attach an existing AKS cluster to your workspace and enable SSL
    attach_config = AksCompute.attach_configuration(resource_group = resource_group,
                                         cluster_name = cluster_name)
    attach_config.enable_ssl(ssl_cert_pem_file="cert.pem",
                                        ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
    ```

*enable_ssl*hakkında daha fazla bilgi için [AksProvisioningConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksprovisioningconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-) ve [AksAttachConfiguration.enable_ssl()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksattachconfiguration?view=azure-ml-py#enable-ssl-ssl-cname-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--leaf-domain-label-none--overwrite-existing-domain-false-)konusuna bakın.

### <a name="deploy-on-azure-container-instances"></a>Azure Kapsayıcı Örneklerinde Dağıtma

Azure Kapsayıcı Örnekleri'ne dağıttığınızda, aşağıdaki kod parçacığının gösterdiği gibi TLS ile ilgili parametreler için değerler sağlarsınız:

```python
from azureml.core.webservice import AciWebservice

aci_config = AciWebservice.deploy_configuration(
    ssl_enabled=True, ssl_cert_pem_file="cert.pem", ssl_key_pem_file="key.pem", ssl_cname="www.contoso.com")
```

Daha fazla bilgi için [Bkz. AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aciwebservice#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-).

## <a name="update-your-dns"></a>DNS'nizi güncelleştirin

Ardından, web hizmetini işaret etmek için DNS'nizi güncelleştirmeniz gerekir.

+ **Konteyner Örnekleri için:**

  Etki alanı adınız için DNS kaydını güncelleştirmek için alan adı kayıt şirketinizdeki araçları kullanın. Kayıt, hizmetin IP adresini işaret etmelidir.

  İstemcilerin, kayıt şirketine ve alan adı için yapılandırılan "yaşama süresine" (TTL) bağlı olarak alan adını çözmesi için dakikalar veya saatler gecikebilir.

+ **AKS için:**

  > [!WARNING]
  > Hizmeti Microsoft'tan bir sertifika kullanarak oluşturmak için *leaf_domain_label* kullandıysanız, kümenin DNS değerini el ile güncelleştirmeyin. Değer otomatik olarak ayarlanmalıdır.

  Aks kümesinin Genel IP Adresinin DNS'sini sol bölmedeki **Ayarlar** altında **Yapılandırma** sekmesinde güncelleştirin. (Aşağıdaki resme bakın.) Ortak IP Adresi, AKS aracı düğümlerini ve diğer ağ kaynaklarını içeren kaynak grubu altında oluşturulan bir kaynak türüdür.

  [![Azure Machine Learning: TLS ile web hizmetlerinin güvenliğini sağlamak](./media/how-to-secure-web-service/aks-public-ip-address.png)](./media/how-to-secure-web-service/aks-public-ip-address-expanded.png)

## <a name="update-the-tlsssl-certificate"></a>TLS/SSL sertifikasını güncelleştirin

TLS/SSL sertifikalarının süresi doluyor ve yenilenmesi gerekiyor. Genellikle bu her yıl olur. Azure Kubernetes Hizmeti'ne dağıtılan modeller için sertifikanızı güncelleştirmek ve yenilemek için aşağıdaki bölümlerdeki bilgileri kullanın:

### <a name="update-a-microsoft-generated-certificate"></a>Microsoft tarafından oluşturulan sertifikayı güncelleştirme

Sertifika ilk olarak Microsoft tarafından oluşturulduysa (hizmeti oluşturmak için *leaf_domain_label* kullanırken), sertifikayı güncelleştirmek için aşağıdaki örneklerden birini kullanın:

**SDK'yı kullan**

```python
from azureml.core.compute import AksCompute
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute.aks import SslConfiguration

# Get the existing cluster
aks_target = AksCompute(ws, clustername)

# Update the existing certificate by referencing the leaf domain label
ssl_configuration = SslConfiguration(leaf_domain_label="myaks", overwrite_existing_domain=True)
update_config = AksUpdateConfiguration(ssl_configuration)
aks_target.update(update_config)
```

**CLI kullanma**

```azurecli
az ml computetarget update aks -g "myresourcegroup" -w "myresourceworkspace" -n "myaks" --ssl-leaf-domain-label "myaks" --ssl-overwrite-domain True
```

Daha fazla bilgi için aşağıdaki başvuru dokümanlarına bakın:

* [SslConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.sslconfiguration?view=azure-ml-py)
* [AksUpdateConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksupdateconfiguration?view=azure-ml-py)

### <a name="update-custom-certificate"></a>Özel sertifikayı güncelleştirme

Sertifika ilk olarak bir sertifika yetkilisi tarafından oluşturulduysa, aşağıdaki adımları kullanın:

1. Sertifikayı yenilemek için sertifika yetkilisi tarafından sağlanan belgeleri kullanın. Bu işlem yeni sertifika dosyaları oluşturur.

1. Hizmeti yeni sertifikayla güncelleştirmek için SDK veya CLI'yi kullanın:

    **SDK'yı kullan**

    ```python
    from azureml.core.compute import AksCompute
    from azureml.core.compute.aks import AksUpdateConfiguration
    from azureml.core.compute.aks import SslConfiguration
    
    # Read the certificate file
    def get_content(file_name):
        with open(file_name, 'r') as f:
            return f.read()

    # Get the existing cluster
    aks_target = AksCompute(ws, clustername)
    
    # Update cluster with custom certificate
    ssl_configuration = SslConfiguration(cname="myaks", cert=get_content('cert.pem'), key=get_content('key.pem'))
    update_config = AksUpdateConfiguration(ssl_configuration)
    aks_target.update(update_config)
    ```

    **CLI kullanma**

    ```azurecli
    az ml computetarget update aks -g "myresourcegroup" -w "myresourceworkspace" -n "myaks" --ssl-cname "myaks"--ssl-cert-file "cert.pem" --ssl-key-file "key.pem"
    ```

Daha fazla bilgi için aşağıdaki başvuru dokümanlarına bakın:

* [SslConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.sslconfiguration?view=azure-ml-py)
* [AksUpdateConfiguration](https://docs.microsoft.com/python/api/azureml-core/azureml.core.compute.aks.aksupdateconfiguration?view=azure-ml-py)

## <a name="disable-tls"></a>TLS'yi devre dışı

Azure Kubernetes Hizmetine dağıtılan bir model için TLS'yi devre dışı kılmış olmak için, bir `SslConfiguration` güncelleştirme `status="Disabled"`gerçekleştirin:

```python
from azureml.core.compute import AksCompute
from azureml.core.compute.aks import AksUpdateConfiguration
from azureml.core.compute.aks import SslConfiguration

# Get the existing cluster
aks_target = AksCompute(ws, clustername)

# Disable TLS
ssl_configuration = SslConfiguration(status="Disabled")
update_config = AksUpdateConfiguration(ssl_configuration)
aks_target.update(update_config)
```

## <a name="next-steps"></a>Sonraki adımlar
Şunları nasıl yapacağınızı öğrenin:
+ [Web hizmeti olarak dağıtılan bir makine öğrenme modelini tüketin](how-to-consume-web-service.md)
+ [Bir Azure sanal ağında denemeleri ve çıkarımları güvenli bir şekilde çalıştırın](how-to-enable-virtual-network.md)
