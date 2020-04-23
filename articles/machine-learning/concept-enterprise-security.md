---
title: Kurumsal güvenlik
titleSuffix: Azure Machine Learning
description: "Azure Machine Learning'i güvenli bir şekilde kullanın: kimlik doğrulama, yetkilendirme, ağ güvenliği, veri şifreleme ve izleme."
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 03/13/2020
ms.openlocfilehash: 1af1a1ccd8bff8fc4b578ecdeec3ac5f7c2352b1
ms.sourcegitcommit: 09a124d851fbbab7bc0b14efd6ef4e0275c7ee88
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82082144"
---
# <a name="enterprise-security-for-azure-machine-learning"></a>Azure Machine Learning için kurumsal güvenlik

Bu makalede, Azure Machine Learning için kullanılabilen güvenlik özellikleri hakkında bilgi edineceksiniz.

Bir bulut hizmeti kullandığınızda, en iyi yöntem erişimi yalnızca gereksinim duyan kullanıcılarla sınırlamaktır. Hizmet tarafından kullanılan kimlik doğrulama ve yetkilendirme modelini anlayarak başlayın. Ayrıca ağ erişimini kısıtlamak veya bulutla şirket içi ağınızdaki kaynaklara güvenli bir şekilde katılmak da isteyebilirsiniz. Veri şifreleme, hem istirahatte hem de veriler hizmetler arasında hareket ederken de hayati önem taşır. Son olarak, hizmeti izleyebilmeniz ve tüm etkinliklerin bir denetim günlüğü oluşturabilmeniz gerekir.

> [!NOTE]
> Bu makaledeki bilgiler Azure Machine Learning Python SDK sürüm 1.0.83.1 veya üzeri sürümlerle çalışır.

## <a name="authentication"></a>Kimlik Doğrulaması

Azure Etkin Dizin (Azure AD) kullanılacak şekilde yapılandırılırsa çok faktörlü kimlik doğrulama sı desteklenir. Kimlik doğrulama işlemi aşağıda veda edin:

1. İstemci Azure AD'de kaydolur ve bir Azure Kaynak Yöneticisi belirteci alır.  Kullanıcılar ve hizmet ilkeleri tam olarak desteklenir.
1. İstemci belirteci Azure Kaynak Yöneticisi'ne ve tüm Azure Machine Learning'e sunar.
1. Machine Learning hizmeti, kullanıcı bilgi işlem hedefine (örneğin, Machine Learning Compute) bir Machine Learning hizmeti belirteci sağlar. Bu belirteç, çalışma tamamlandıktan sonra Machine Learning hizmetine geri çağırmak için kullanıcı bilgi işlem hedefi tarafından kullanılır. Kapsam çalışma alanıyla sınırlıdır.

[![Azure Machine Learning'de kimlik doğrulama](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication-expanded.png#lightbox)

Daha fazla bilgi için bkz. Azure [Machine Learning kaynakları ve iş akışları için kimlik doğrulaması ayarlama.](how-to-setup-authentication.md) Bu makalede, hizmet ilkeleri ve otomatik iş akışları nın kullanılması da dahil olmak üzere kimlik doğrulama ile ilgili bilgiler ve örnekler verilmektedir.

### <a name="authentication-for-web-service-deployment"></a>Web hizmeti dağıtımı için kimlik doğrulama

Azure Machine Learning, web hizmetleri için iki kimlik doğrulama biçimini destekler: anahtar ve belirteç. Her web hizmeti aynı anda yalnızca bir kimlik doğrulama biçimi etkinleştirebilir.

|Kimlik doğrulama Yöntemi|Açıklama|Azure Container Instances|AKS|
|---|---|---|---|
|Anahtar|Anahtarlar statiktir ve yenilenmesi gerekmez. Anahtarlar el ile yeniden oluşturulabilir.|Varsayılan olarak devre dışı| Varsayılan olarak etkindir|
|Belirteç|Belirteçlerin süresi belirli bir süre sonra sona erer ve yenilenmesi gerekir.| Kullanılamaz| Varsayılan olarak devre dışı |

Kod örnekleri için [web hizmeti kimlik doğrulama bölümüne](how-to-setup-authentication.md#web-service-authentication)bakın.

## <a name="authorization"></a>Yetkilendirme

Birden çok çalışma alanı oluşturabilirsiniz ve her çalışma alanı birden çok kişi tarafından paylaşılabilir. Bir çalışma alanını paylaştığınızda, bu rolleri kullanıcılara atayarak bu alana erişimi denetleyebilirsiniz:

* Sahip
* Katılımcı
* Okuyucu

Aşağıdaki tabloda, önemli Azure Machine Learning işlemlerinden bazıları ve bunları gerçekleştirebilecek roller listelemektedir:

| Azure Machine Learning işlemi | Sahip | Katılımcı | Okuyucu |
| ---- |:----:|:----:|:----:|
| Çalışma alanı oluşturma | ✓ | ✓ | |
| Çalışma alanını paylaşma | ✓ | |  |
| Çalışma alanını Enterprise sürümüne yükseltin | ✓ | |
| İşlem hedefi oluşturma | ✓ | ✓ | |
| İşlem hedefi ekleme | ✓ | ✓ | |
| Veri depoları ekleme | ✓ | ✓ | |
| Denemeyi çalıştır | ✓ | ✓ | |
| Çalıştırmaları/ölçümleri görüntüleme | ✓ | ✓ | ✓ |
| Modeli kaydetme | ✓ | ✓ | |
| Görüntü oluşturma | ✓ | ✓ | |
| Web hizmetini dağıtma | ✓ | ✓ | |
| Modelleri/görüntüleri görüntüleme | ✓ | ✓ | ✓ |
| Web hizmetini arayın | ✓ | ✓ | ✓ |

Yerleşik roller gereksinimlerinizi karşılamazsa, özel roller oluşturabilirsiniz. Özel roller yalnızca çalışma alanı ve Machine Learning Compute işlemleri için desteklenir. Özel roller, çalışma alanında ve bu çalışma alanındaki bilgi işlem kaynağında izinleri okumuş, yazabilir veya silebilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz. Daha fazla bilgi için [bkz.](how-to-assign-roles.md)

> [!WARNING]
> Azure Machine Learning şu anda Azure Active Directory iş-iş işbirliği ile desteklenmez.

### <a name="securing-compute-targets-and-data"></a>Bilgi işlem hedeflerinin ve verilerin güvenliğini sağlama

Sahipleri ve katkıda bulunanlar, çalışma alanına bağlı tüm bilgi işlem hedeflerini ve veri depolarını kullanabilir.  

Her çalışma alanı, çalışma alanıyla aynı ada sahip ilişkili bir sistem atanmış yönetilen kimliğe de sahiptir. Yönetilen kimlik, çalışma alanında kullanılan ekli kaynaklar da aşağıdaki izinlere sahiptir.

Yönetilen kimlikler hakkında daha fazla bilgi için Azure [kaynakları için Yönetilen kimlikler'e](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)bakın.

| Kaynak | İzinler |
| ----- | ----- |
| Çalışma alanı | Katılımcı |
| Depolama hesabı | Depolama Blob Veri Katılımcısı |
| Key Vault | Tüm anahtarlara, sırlara, sertifikalara erişim |
| Azure Container Registry | Katılımcı |
| Çalışma alanını içeren kaynak grubu | Katılımcı |
| Anahtar tonoziçeren kaynak grubu (çalışma alanını içerenden farklıysa) | Katılımcı |

Yöneticilerin yönetilen kimliğin önceki tabloda belirtilen kaynaklara erişimini iptal etmesini önermiyoruz. Resync tuşları işlemini kullanarak erişimi geri yükleyebilirsiniz.

Azure Machine Learning, her çalışma alanı `aml-` bölgesi `Microsoft-AzureML-Support-App-`için aboneliğinizde katılımcı düzeyinde erişimiçeren ek bir uygulama oluşturur (ad ile başlar veya ) Örneğin, Doğu ABD'de bir çalışma alanınız ve Aynı abonelikte Kuzey Avrupa'da bir çalışma alanınız varsa, bu uygulamalardan ikisini görürsünüz. Bu uygulamalar, Azure Machine Learning'in bilgi işlem kaynaklarını yönetmenize yardımcı olmasını sağlar.

## <a name="network-security"></a>Ağ güvenliği

Azure Machine Learning, bilgi işlem kaynakları için diğer Azure hizmetlerine dayanır. Modelleri eğitmek ve dağıtmak için hesaplama kaynakları (işlem hedefleri) kullanılır. Bu bilgi işlem hedeflerini sanal ağda oluşturabilirsiniz. Örneğin, bir modeli eğitmek ve ardından modeli AKS'ye dağıtmak için Azure Veri Bilimi Sanal Makinesi'ni kullanabilirsiniz.  

Daha fazla bilgi için, [sanal ağda denemeleri ve çıkarımları nasıl çalıştırılabilirsiniz.](how-to-enable-virtual-network.md)

Çalışma alanınız için Azure Özel Bağlantısını da etkinleştirebilirsiniz. Private Link, bir Azure Sanal Ağı'ndan iletişimi çalışma alanınızla sınırlamanızı sağlar. Daha fazla bilgi için [Özel Bağlantının nasıl yapılandırılabildiğini](how-to-configure-private-link.md)öğrenin.

> [!TIP]
> Çalışma alanınız ve diğer Azure kaynaklarınız arasındaki iletişimi korumak için sanal ağı ve Özel Bağlantıyı birleştirebilirsiniz. Ancak, bazı kombinasyonlar Bir Enterprise sürümü çalışma alanı gerektirir. Hangi senaryoların Enterprise sürümü gerektirdiğini anlamak için aşağıdaki tabloyu kullanın:
>
> | Senaryo | Enterprise</br>Edition | Temel</br>Edition |
> | ----- |:-----:|:-----:| 
> | Sanal ağ veya Özel Bağlantı yok | ✔ | ✔ |
> | Private Link olmadan çalışma alanı. Sanal ağdaki diğer kaynaklar (Azure Kapsayıcı Kayıt Defteri hariç) | ✔ | ✔ |
> | Private Link olmadan çalışma alanı. Private Link ile diğer kaynaklar | ✔ | |
> | Private Link ile çalışma alanı. Sanal ağdaki diğer kaynaklar (Azure Kapsayıcı Kayıt Defteri hariç) | ✔ | ✔ |
> | Çalışma alanı ve Private Link ile diğer kaynaklar | ✔ | |
> | Private Link ile çalışma alanı. Private Link veya sanal ağ olmayan diğer kaynaklar | ✔ | ✔ |
> | Sanal ağda Azure Kapsayıcı Kayıt Defteri | ✔ | |
> | Çalışma alanı için Müşteri Yönetilen Anahtarlar | ✔ | |
> 

> [!WARNING]
> Azure Machine Learning bilgi işlem örnekleri önizlemesi, Private Link'in etkin olduğu bir çalışma alanında desteklenmez.
> 
> Azure Machine Learning, özel bağlantı etkinleştirilmiş bir Azure Kubernetes Hizmeti kullanmayı desteklemez. Bunun yerine, Azure Kubernetes Hizmetini sanal ağda kullanabilirsiniz. Daha fazla bilgi için bkz: [Azure Sanal Ağı'ndaki Güvenli Azure ML deneme ve çıkarım işleri.](how-to-enable-virtual-network.md)

## <a name="data-encryption"></a>Veri şifrelemesi

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

> [!IMPORTANT]
> Çalışma alanınız hassas veriler içeriyorsa, çalışma alanınızı oluştururken [hbi_workspace bayrağını](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-) ayarlamanızı öneririz. 

Bayrak, Microsoft'un `hbi_workspace` tanılama amacıyla topladığı veri miktarını denetler ve Microsoft yönetilen ortamlarda ek şifreleme sağlar. Buna ek olarak aşağıdakileri sağlar:

* Amlcompute kümenizdeki yerel karalama diskini şifrelemeye başlar, bu abonelikte daha önce herhangi bir küme oluşturmamış sanız. Aksi takdirde, işlem kümelerinizin kazı kazan diskinin şifresini etkinleştirmek için bir destek bileti yükseltmeniz gerekir 
* Yerel karalama diskinizi çalıştırmalar arasında temizler
* Depolama hesabınız, konteyner kayıt defteriniz ve SSH hesabınız için kimlik bilgilerini yürütme katmanından anahtar kasanızı kullanarak işlem kümelerinize güvenli bir şekilde geçirir
* Temel toplu iş havuzlarının AzureMachineLearningService dışındaki harici hizmetler tarafından çağrılmamasını sağlamak için IP filtreleme sağlar


Azure'da şifrelemenin nasıl çalıştığı hakkında daha fazla bilgi için Azure [veri şifrelemesi'ne bakın.](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest)

#### <a name="azure-blob-storage"></a>Azure Blob depolama

Azure Machine Learning, Azure Machine Learning çalışma alanına ve aboneliğinize bağlı Azure Blob depolama hesabında anlık görüntüleri, çıktıları ve günlükleri saklar. Azure Blob depolama alanında depolanan tüm veriler Microsoft tarafından yönetilen anahtarlarla birlikte şifrelenir.

Azure Blob depolama alanında depolanan veriler için kendi anahtarlarınızı nasıl kullanacağınız hakkında bilgi için Azure [Anahtar Kasası'nda müşteri tarafından yönetilen anahtarlarla Azure Depolama şifrelemesine](../storage/common/storage-encryption-keys-portal.md)bakın.

Eğitim verileri genellikle Azure Blob depolama alanında da depolanır, böylece bilgi işlem hedeflerini niçin erişilebilir hale getirir. Bu depolama, Azure Machine Learning tarafından yönetilmiyor, ancak uzak bir dosya sistemi olarak hedefleri hesaplamak için monte edilmiştir.

Anahtarınızı __döndürmeniz veya iptal__ etmeniz gerekiyorsa, bunu istediğiniz zaman yapabilirsiniz. Bir anahtarı döndürürken, depolama hesabı verileri istirahatte şifrelemek için yeni anahtarı (en son sürüm) kullanmaya başlar. Bir anahtarı iptal ederken (devre dışı bırakırken), depolama hesabı başarısız istekleri halleder. Genellikle rotasyon veya iptal etkili olması için bir saat sürer.

Erişim anahtarlarını yeniden oluşturma hakkında bilgi [için](how-to-change-storage-access-key.md)bkz.

#### <a name="azure-cosmos-db"></a>Azure Cosmos DB

Azure Machine Learning ölçümleri ve meta verileri bir Azure Cosmos DB örneğinde depolar. Bu örnek, Azure Machine Learning tarafından yönetilen bir Microsoft aboneliğiyle ilişkilidir. Azure Cosmos DB'de depolanan tüm veriler Microsoft tarafından yönetilen anahtarlarla birlikte şifrelenir.

Azure Cosmos DB örneğini şifrelemek için kendi (müşteri tarafından yönetilen) anahtarlarınızı kullanmak için çalışma alanınızla birlikte kullanmak üzere özel bir Cosmos DB örneği oluşturabilirsiniz. Microsoft aboneliğimizde barındırılan çok kiracılı Cosmos DB örneğinin dışında, çalışma geçmişi bilgileri gibi verilerinizi depolamak istiyorsanız bu yaklaşımı öneririz. 

Aboneliğinizde müşteri tarafından yönetilen anahtarlarla cosmos DB örneği sağlamayı etkinleştirmek için aşağıdaki işlemleri gerçekleştirin:

* Cosmos DB için müşteri tarafından yönetilen anahtar özelliklerini etkinleştirin. Şu anda, bu özelliği kullanmak için erişim istemeniz gerekir. Bunu yapmak için [cosmosdbpm@microsoft.com](mailto:cosmosdbpm@microsoft.com)lütfen .

* Azure Machine Learning ve Azure Cosmos DB kaynak sağlayıcılarını aboneliğinizde kaydedin, eğer daha önce yapılmamışsa.

* Makine Öğrenimi Uygulamasını (Kimlik ve Erişim Yönetimi'nde) aboneliğinizde katılımcı izinleriyle yetkilendirin.

    ![Portalda Kimlik ve Erişim Yönetiminde 'Azure Machine Learning Uygulamasını' yetkilendirme](./media/concept-enterprise-security/authorize-azure-machine-learning.png)

* Azure Machine Learning çalışma alanını oluştururken aşağıdaki parametreleri kullanın. Her iki parametre de zorunludur ve SDK, CLI, REST API'lerinde ve Kaynak Yöneticisi şablonlarında desteklenir.

    * `resource_cmk_uri`: Bu parametre, anahtar kasanızdaki müşteri yönetilen anahtarın tam kaynak URI'sidir ve [anahtara ait sürüm bilgileri](../key-vault/about-keys-secrets-and-certificates.md#objects-identifiers-and-versioning)de dahil olmak üzere. 

    * `cmk_keyvault`: Bu parametre, aboneliğinizdeki anahtar kasasının kaynak kimliğidir. Bu anahtar kasasının Azure Machine Learning çalışma alanı için kullanacağınız aynı bölgede ve abonelikte olması gerekir. 
    
        > [!NOTE]
        > Bu anahtar kasa örneği, çalışma alanını sağlarken Azure Machine Learning tarafından oluşturulan anahtar kasasından farklı olabilir. Çalışma alanı için aynı anahtar kasa örneğini kullanmak istiyorsanız, [key_vault parametresini](https://docs.microsoft.com/python/api/azureml-core/azureml.core.workspace(class)?view=azure-ml-py#create-name--auth-none--subscription-id-none--resource-group-none--location-none--create-resource-group-true--sku--basic---friendly-name-none--storage-account-none--key-vault-none--app-insights-none--container-registry-none--cmk-keyvault-none--resource-cmk-uri-none--hbi-workspace-false--default-cpu-compute-target-none--default-gpu-compute-target-none--exist-ok-false--show-output-true-)kullanarak çalışma alanını sağlarken aynı anahtar kasasını geçirin. 

Bu Cosmos DB örneği, aboneliğinizde Microsoft tarafından yönetilen bir kaynak grubunda oluşturulur. Yönetilen kaynak grubu biçiminde adlandırılır<AML Workspace Resource Group Name><GUID>

> [!IMPORTANT]
> * Bu Cosmos DB örneğini silmeniz gerekiyorsa, onu kullanan Azure Machine Learning çalışma alanını silmeniz gerekir. 
> * Bu Cosmos DB hesabı için varsayılan [__İstek Birimleri__](../cosmos-db/request-units.md) __8000__olarak ayarlanır. Bu değeri değiştirmek desteklenmez. 

Anahtarınızı __döndürmeniz veya iptal__ etmeniz gerekiyorsa, bunu istediğiniz zaman yapabilirsiniz. Bir anahtarı döndürürken, Cosmos DB verileri istirahatte şifrelemek için yeni anahtarı (en son sürüm) kullanmaya başlar. Bir anahtarı iptal ederken (devre dışı bırakırken), Cosmos DB başarısız istekleri halleder. Genellikle rotasyon veya iptal etkili olması için bir saat sürer.

Cosmos DB ile müşteri tarafından yönetilen anahtarlar hakkında daha fazla bilgi için Azure [Cosmos DB hesabınız için müşteri tarafından yönetilen anahtarları yapılandır'a](../cosmos-db/how-to-setup-cmk.md)bakın.

#### <a name="azure-container-registry"></a>Azure Container Registry

Kayıt defterinizdeki tüm kapsayıcı resimleri (Azure Kapsayıcı Kayıt Defteri) istirahatte şifrelenir. Azure, görüntüyü depolamadan önce görüntüyü otomatik olarak şifreler ve Azure Machine Learning görüntüyü çektiğinde şifresini çözer.

Azure Kapsayıcı Kayıt Defterinizi şifrelemek için kendi (müşteri tarafından yönetilen) anahtarlarınızı kullanmak için, çalışma alanını sağlarken kendi ACR'nizi oluşturmanız ve eklemeniz veya çalışma alanı sağlama sırasında oluşturulan varsayılan örneği şifrelemeniz gerekir.

Varolan bir Azure Kapsayıcı Kayıt Defteri'ni kullanarak çalışma alanı oluşturma örneği için aşağıdaki makalelere bakın:

* [Azure CLI ile Azure Machine Learning için bir çalışma alanı oluşturun.](how-to-manage-workspace-cli.md)
* [Azure Makine Öğrenimi için bir çalışma alanı oluşturmak için Azure Kaynak Yöneticisi şablonu kullanın](how-to-create-workspace-template.md)

#### <a name="azure-container-instance"></a>Azure Container Örneği

Dağıtılmış bir Azure Kapsayıcı Örneği (ACI) kaynağını müşteri tarafından yönetilen anahtarları kullanarak şifreleyebilirsiniz. ACI için kullanılan müşteri tarafından yönetilen anahtar, çalışma alanınız için Azure Anahtar Kasası'nda depolanabilir. Anahtar oluşturma hakkında bilgi için, [müşteri tarafından yönetilen bir anahtarla verileri şifrele'ye](../container-instances/container-instances-encrypt-data.md#generate-a-new-key)bakın.

Bir modeli Azure Kapsayıcı Örneği'ne dağıtırken anahtarı kullanmak için, yeni bir dağıtım yapılandırması oluşturun. `AciWebservice.deploy_configuration()` Aşağıdaki parametreleri kullanarak anahtar bilgilerini sağlayın:

* `cmk_vault_base_url`: Anahtarı içeren anahtar kasasının URL'si.
* `cmk_key_name`: Anahtarın adı.
* `cmk_key_version`: Anahtarın versiyonu.

Dağıtım yapılandırması oluşturma ve kullanma hakkında daha fazla bilgi için aşağıdaki makalelere bakın:

* [AciWebservice.deploy_configuration()](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice.aci.aciwebservice?view=azure-ml-py#deploy-configuration-cpu-cores-none--memory-gb-none--tags-none--properties-none--description-none--location-none--auth-enabled-none--ssl-enabled-none--enable-app-insights-none--ssl-cert-pem-file-none--ssl-key-pem-file-none--ssl-cname-none--dns-name-label-none--primary-key-none--secondary-key-none--collect-model-data-none--cmk-vault-base-url-none--cmk-key-name-none--cmk-key-version-none-) referans
* [Dağıtım nereye ve nasıl yapılır?](how-to-deploy-and-where.md)
* [Azure Kapsayıcı Örneklerine bir model dağıtma](how-to-deploy-azure-container-instance.md)

ACI ile müşteri tarafından yönetilen bir anahtarı kullanma hakkında daha fazla bilgi için, [müşteri tarafından yönetilen bir anahtarla verileri şifrele'ye](../container-instances/container-instances-encrypt-data.md#encrypt-data-with-a-customer-managed-key)bakın.

#### <a name="azure-kubernetes-service"></a>Azure Kubernetes Service

Dağıtılmış bir Azure Kubernetes Hizmeti kaynağını istediğiniz zaman müşteri tarafından yönetilen anahtarları kullanarak şifreleyebilirsiniz. Daha fazla bilgi için azure [Kubernetes Hizmeti ile kendi anahtarlarınızı getir'](../aks/azure-disk-customer-managed-keys.md)e bakın. 

Bu işlem, Kubernetes kümesinde dağıtılan sanal makinelerin hem Veri hem de Işletim Sistemi Diskini şifrelemenize olanak tanır.

> [!IMPORTANT]
> Bu işlem yalnızca AKS K8s sürüm 1.17 veya daha yüksek çalışır. Azure Machine Learning, 13 Ocak 2020'de AKS 1.17 için destek ekledi.

#### <a name="machine-learning-compute"></a>Makine Öğrenimi Hesaplama

Azure Depolama'da depolanan her bir bilgi işlem düğümü için işletim sistemi diski, Azure Machine Learning depolama hesaplarında Microsoft tarafından yönetilen anahtarlarla şifrelenir. Bu işlem hedefi geçicidir ve kümeler genellikle hiçbir çalıştırma sıraya girince küçültülr. Altta yatan sanal makine de-provisioned ve işletim sistemi diski silinir. Azure Disk Şifreleme, işletim sistemi diski için desteklenmez.

Her sanal makinede işletim sistemi işlemleri için yerel bir geçici disk de bulunur. İsterseniz, eğitim verilerini sahnelemek için diski kullanabilirsiniz. Disk, `hbi_workspace` parametre olarak ayarlanmış çalışma alanları için `TRUE`varsayılan olarak şifrelenir. Bu ortam yalnızca çalışma süreniz boyunca kısa ömürlüdür ve şifreleme desteği yalnızca sistem tarafından yönetilen anahtarlar ile sınırlıdır.

#### <a name="azure-databricks"></a>Azure Databricks

Azure Veri Tuğlaları Azure Machine Learning ardışık hatlarında kullanılabilir. Varsayılan olarak, Azure Databricks tarafından kullanılan Databricks Dosya Sistemi (DBFS), Microsoft tarafından yönetilen bir anahtar kullanılarak şifrelenir. Azure Veri Tuğlalarını müşteri tarafından yönetilen anahtarları kullanacak şekilde yapılandırmak için, [varsayılan (kök) DBFS'de müşteri tarafından yönetilen anahtarları yapılandır'a](/azure/databricks/security/customer-managed-keys-dbfs)bakın.

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme

Azure Machine Learning, çeşitli Azure Machine Learning mikro hizmetleri arasında dahili iletişimi sağlamak için TLS kullanır. Tüm Azure Depolama erişimi de güvenli bir kanal üzerinden gerçekleşir.

Puanlama uç noktasına harici aramaları güvenli hale getirmek için Azure Machine Learning TLS kullanır. Daha fazla bilgi için Azure [Machine Learning aracılığıyla bir web hizmetini güvence altına almak için TLS'yi kullanın'a](https://docs.microsoft.com/azure/machine-learning/how-to-secure-web-service)bakın.

### <a name="using-azure-key-vault"></a>Azure Anahtar Kasası Kullanma

Azure Machine Learning, çeşitli kimlik bilgilerini depolamak için çalışma alanıyla ilişkili Azure Anahtar Kasası örneğini kullanır:

* İlişkili depolama hesabı bağlantı dizesi
* Azure Kapsayıcı Deposu örneklerinin parolaları
* Veri depolarına bağlantı dizeleri

Azure HDInsight ve VM'ler gibi bilgi işlem hedeflerinin SSH parolaları ve anahtarları, Microsoft aboneliğiyle ilişkili ayrı bir anahtar kasasında depolanır. Azure Machine Learning, kullanıcılar tarafından sağlanan parolaları veya anahtarları saklamaz. Bunun yerine, denemeleri çalıştırmak için VM'lere ve HDInsight'a bağlanmak için kendi SSH anahtarlarını oluşturur, yetkilendirir ve saklar.

Her çalışma alanı, çalışma alanıyla aynı ada sahip ilişkili bir sistem atanmış yönetilen kimliğe sahiptir. Bu yönetilen kimlik, anahtar kasasındaki tüm anahtarlara, sırlara ve sertifikalara erişebilir.

## <a name="data-collection-and-handling"></a>Veri toplama ve işleme

### <a name="microsoft-collected-data"></a>Microsoft veri topladı

Microsoft, kaynak adları (örneğin veri kümesi adı veya makine öğrenimi deneme adı) veya tanılama amacıyla iş ortamı değişkenleri gibi kullanıcı dışı tanımlayıcı bilgiler toplayabilir. Tüm bu veriler Microsoft'a ait aboneliklerde barındırılan depolama alanında Microsoft tarafından yönetilen anahtarlar kullanılarak depolanır ve [Microsoft'un standart Gizlilik ilkesi ve veri işleme standartlarını](https://privacy.microsoft.com/privacystatement)izler.

Microsoft ayrıca, hassas bilgileri (hesap anahtar sırları gibi) ortam değişkenlerinde saklamamanızı da önerir. Ortam değişkenleri günlüğe kaydedilir, şifrelenir ve bizim tarafından depolanır. Benzer şekilde [runid](https://docs.microsoft.com/python/api/azureml-core/azureml.core.run%28class%29?view=azure-ml-py)adlandırma, kullanıcı adları veya gizli proje adları gibi hassas bilgileri dahil kaçının. Bu bilgiler, Microsoft Destek mühendisleri tarafından erişilebilen telemetri günlüklerinde görünebilir.

Çalışma alanını `hbi_workspace` `TRUE` sağlarken parametreyi ayarlayarak toplanan tanılama verilerini devre dışı bırakabilirsiniz. Bu işlev, AzureML Python SDK, CLI, REST API'leri veya Azure Kaynak Yöneticisi şablonları kullanılırken desteklenir.

### <a name="microsoft-generated-data"></a>Microsoft tarafından oluşturulan veriler

Otomatik Makine Öğrenimi gibi hizmetleri kullanırken, Microsoft birden çok modeli eğitmek için geçici, önceden işlenmiş bir veri oluşturabilir. Bu veriler, erişim denetimlerini ve şifrelemeyi uygun şekilde zorlamanızı sağlayan çalışma alanınızdaki bir veri deposunda depolanır.

[Dağıtılan bitiş noktanızdan günlüğe kaydedilen tanılama bilgilerini](how-to-enable-app-insights.md) Azure Uygulama İstatistikleri örneğinize şifrelemek de isteyebilirsiniz.

## <a name="monitoring"></a>İzleme

### <a name="metrics"></a>Ölçümler

Azure Makine Öğrenimi çalışma alanınız için ölçümleri görüntülemek ve izlemek için Azure Monitor ölçümlerini kullanabilirsiniz. Azure [portalında,](https://portal.azure.com)çalışma alanınızı seçin ve ardından **Ölçümler'i**seçin:

[![Çalışma alanı için örnek ölçümleri gösteren ekran görüntüsü](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Ölçümler, çalıştırmalar, dağıtımlar ve kayıtlar la ilgili bilgileri içerir.

Daha fazla bilgi için [Azure Monitor'daki Ölçümler'e](/azure/azure-monitor/platform/data-platform-metrics)bakın.

### <a name="activity-log"></a>Etkinlik günlüğü

Çalışma alanında gerçekleştirilen çeşitli işlemleri görmek için bir çalışma alanının etkinlik günlüğünü görüntüleyebilirsiniz. Günlük, işlem adı, olay başlatıcısı ve zaman damgası gibi temel bilgileri içerir.

Bu ekran görüntüsü, çalışma alanının etkinlik günlüğünü gösterir:

[![Çalışma alanının etkinlik günlüğünü gösteren ekran görüntüsü](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Puanlama isteği ayrıntıları Application Insights'ta saklanır. Bir çalışma alanı oluşturduğunuzda, aboneliğinizde Uygulama Öngörüleri oluşturulur. Günlüğe kaydedilmiş bilgiler gibi alanları içerir:

* HTTPYöntemi
* Useragent
* ComputeType
* İstekUrl
* Statuscode
* Requestıd
* Süre

> [!IMPORTANT]
> Azure Machine Learning çalışma alanındaki bazı eylemler, bilgileri etkinlik günlüğüne kaydetmez. Örneğin, bir eğitim çalışmasının başlangıcı ve bir modelin kaydı günlüğe kaydedilmez.
>
> Bu eylemlerden bazıları çalışma alanınızın **Etkinlikler** alanında görünür, ancak bu bildirimler etkinliği kimin başlattığını göstermez.

## <a name="data-flow-diagrams"></a>Veri akış diyagramları

### <a name="create-workspace"></a>Çalışma alanı oluşturma

Aşağıdaki diyagram, çalışma alanı oluşturma iş akışını gösterir.

* Desteklenen Azure Machine Learning istemcilerinden (Azure CLI, Python SDK, Azure portalı) Azure AD'de oturum açar ve uygun Azure Kaynak Yöneticisi belirteci isteyin.
* Çalışma alanını oluşturmak için Azure Kaynak Yöneticisi'ni ararsınız. 
* Azure Kaynak Yöneticisi, çalışma alanını sağlamak için Azure Machine Learning kaynak sağlayıcısıyla bağlantı kurun.

Çalışma alanı oluşturma sırasında kullanıcının aboneliğinde ek kaynaklar oluşturulur:

* Key Vault (sırları saklamak için)
* Azure depolama hesabı (blob ve dosya paylaşımı dahil)
* Azure Konteyner Kayıt Defteri (Docker görüntülerini çıkarım/puanlama ve deneme için depolamak için)
* Uygulama Öngörüleri (telemetri depolamak için)

Kullanıcı ayrıca, gerektiğinde bir çalışma alanına (Azure Kubernetes Hizmeti veya VM'ler gibi) eklenen diğer işlem hedeflerini de sağlayabilir.

[![Çalışma alanı iş akışı oluşturma](media/concept-enterprise-security/create-workspace.png)](media/concept-enterprise-security/create-workspace-expanded.png#lightbox)

### <a name="save-source-code-training-scripts"></a>Kaynak kodu kaydetme (eğitim komut dosyaları)

Aşağıdaki diyagram, kod anlık görüntü iş akışını gösterir.

Azure Machine Learning çalışma alanıyla ilişkili olarak kaynak kodu (eğitim komut dosyaları) içeren dizinler (denemeler) bulunur. Bu komut dosyaları yerel makinenizde ve bulutta (aboneliğiniz için Azure Blob depolama alanında) depolanır. Kod anlık görüntüleri yürütme veya denetim için geçmiş denetim için kullanılır.

[![Kod anlık iş akışı](media/concept-enterprise-security/code-snapshot.png)](media/concept-enterprise-security/code-snapshot-expanded.png#lightbox)

### <a name="training"></a>Eğitim

Aşağıdaki diyagram, eğitim iş akışını gösterir.

* Azure Machine Learning, önceki bölümde kaydedilen kod anlık görüntüsü için anlık görüntü kimliğiyle çağrılır.
* Azure Machine Learning, machine learning hizmetiyle iletişim kurmak için Machine Learning Compute/VM gibi bilgi işlem hedefleri tarafından daha sonra kullanılan bir çalışma kimliği (isteğe bağlı) ve Bir Machine Learning hizmet belirteci oluşturur.
* Eğitim işlerini çalıştırmak için yönetilen bir bilgi işlem hedefi (Machine Learning Compute gibi) veya yönetilmeyen bir bilgi işlem hedefi (VM'ler gibi) seçebilirsiniz. Her iki senaryo için de veri akışları şunlardır:
   * Microsoft aboneliğindeki önemli bir kasada SSH kimlik bilgileri tarafından erişilen VM/HDInsight. Azure Machine Learning, aşağıdakileri içeren bilgi işlem hedefinde yönetim kodu çalıştırAn:

   1. Çevreyi hazırlar. (Docker, VM'ler ve yerel bilgisayarlar için bir seçenektir. Docker kaplarında yapılan deneylerin nasıl çalıştığını anlamak için Makine Öğrenimi İşlemi için aşağıdaki adımlara bakın.)
   1. Kodu karşıdan yükler.
   1. Ortam değişkenleri ve yapılandırmaları ayarlar.
   1. Kullanıcı komut dosyalarını çalıştırın (önceki bölümde belirtilen kod anlık görüntüsü).

   * Machine Learning Compute, çalışma alanı tarafından yönetilen bir kimlik üzerinden erişilen.
Machine Learning Compute yönetilen bir bilgi işlem hedefi olduğundan (diğer bir şekilde Microsoft tarafından yönetilir) Microsoft aboneliğiniz altında çalışır.

   1. Uzaktan Docker inşaatı, gerekirse başladı.
   1. Yönetim kodu, kullanıcının Azure Dosyaları paylaşımına yazılır.
   1. Kapsayıcı bir ilk komutile başlatılır. Diğer bir deyişle, önceki adımda açıklandığı gibi yönetim kodu.

#### <a name="querying-runs-and-metrics"></a>Çalıştırmaları ve ölçümleri sorgulama

Aşağıdaki akış diyagramında, bu adım, eğitim bilgiişlem hedefi çalışma ölçümlerini Cosmos DB veritabanındaki depolamadan Azure Machine Learning'e geri yazdığında gerçekleşir. İstemciler Azure Machine Learning'i arayabilir. Machine Learning de Cosmos DB veritabanından ölçümleri çeker ve istemciye geri döndürecektir.

[![Eğitim iş akışı](media/concept-enterprise-security/training-and-metrics.png)](media/concept-enterprise-security/training-and-metrics-expanded.png#lightbox)

### <a name="creating-web-services"></a>Web hizmetleri oluşturma

Aşağıdaki diyagram çıkarım iş akışını gösterir. Çıkarım veya model puanlama, dağıtılan modelin tahmin için kullanıldığı aşamadır, en yaygın olarak üretim verilerinde.

İşte ayrıntılar şunlardır:

* Kullanıcı, Azure Machine Learning SDK gibi bir istemci kullanarak bir modeli kaydeder.
* Kullanıcı bir model, puan dosyası ve diğer model bağımlılıkları kullanarak bir görüntü oluşturur.
* Docker görüntüsü oluşturulur ve Azure Kapsayıcı Kayıt Defteri'nde depolanır.
* Web hizmeti, önceki adımda oluşturulan görüntü kullanılarak işlem hedefine (Konteyner Örnekleri/AKS) dağıtılır.
* Puanlama isteği ayrıntıları, kullanıcının aboneliğinde bulunan Application Insights'ta depolanır.
* Telemetri de Microsoft/Azure aboneliğine itilir.

[![Çıkarım iş akışı](media/concept-enterprise-security/inferencing.png)](media/concept-enterprise-security/inferencing-expanded.png#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

* [TLS ile Güvenli Azure Machine Learning web hizmetleri](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir Machine Learning modelini tüketin](how-to-consume-web-service.md)
* [Toplu iş tahminleri nasıl çalıştırılabilen](how-to-use-parallel-run-step.md)
* [Azure Machine Learning modellerinizi Uygulama Öngörüleri ile izleyin](how-to-enable-app-insights.md)
* [Üretimdeki modeller için veri toplama](how-to-enable-data-collection.md)
* [Azure Machine Learning SDK’sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure Sanal Ağı ile Azure Makine Öğrenimini Kullanma](how-to-enable-virtual-network.md)
* [Öneri sistemleri oluşturmak için en iyi uygulamalar](https://github.com/Microsoft/Recommenders)
* [Azure'da gerçek zamanlı öneri API'sı oluşturma](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
