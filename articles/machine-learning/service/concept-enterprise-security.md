---
title: Kurumsal güvenlik
titleSuffix: Azure Machine Learning service
description: 'Azure Machine Learning hizmeti güvenli bir şekilde kullanın: kimlik doğrulama, yetkilendirme, ağ güvenliği, veri şifreleme ve izleme.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 08/07/2019
ms.openlocfilehash: 510f58cc0b71fb75ac6f5e15fc883c3caf4a8f9a
ms.sourcegitcommit: d3dced0ff3ba8e78d003060d9dafb56763184d69
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69897946"
---
# <a name="enterprise-security-for-azure-machine-learning-service"></a>Azure Machine Learning hizmeti için kurumsal güvenlik

Bu makalede, Azure Machine Learning hizmeti ile kullanılabilen güvenlik özellikleri hakkında bilgi edineceksiniz.

Bir bulut hizmeti kullanırken, erişimi yalnızca ihtiyacı olan kullanıcılarla kısıtlamak en iyi uygulamadır. Bu, hizmet tarafından kullanılan kimlik doğrulama ve yetkilendirme modelini öğrenilerek başlar. Ayrıca, ağ erişimini kısıtlamak veya şirket içi ağınızdaki kaynakları güvenli bir şekilde buluta katmak isteyebilirsiniz. Veri şifrelemesi de hem bekleyen hem de veriler hizmetler arasında taşınıyor. Son olarak, hizmeti izleyebilmeniz ve tüm etkinliklerin denetim günlüğünü üretmeniz gerekir.

## <a name="authentication"></a>Authentication

Aynı için Azure Active Directory (Azure AD) yapılandırılmışsa çok faktörlü kimlik doğrulaması desteklenir.

* İstemci Azure AD 'ye oturum açar ve Azure Resource Manager belirtecini alır.  Kullanıcılar ve hizmet sorumluları tam olarak desteklenmektedir.
* İstemci, tüm Azure Machine Learning Hizmetleri & Azure Resource Manager belirteç sunar
* Azure Machine Learning hizmeti, Kullanıcı işlemine bir Azure Machine Learning belirteci sağlar. Örneğin, Machine Learning İşlem. Bu belirteç, çalışma tamamlandıktan sonra Azure Machine Learning hizmetine (kapsamı çalışma alanına sınır) geri çağırmak için Kullanıcı işlemi tarafından kullanılır.

[![Azure Machine Learning hizmetinde kimlik doğrulamanın nasıl çalıştığını gösteren ekran görüntüsü](./media/enterprise-readiness/authentication.png)](./media/enterprise-readiness/authentication-expanded.png)

### <a name="authentication-for-web-service-deployment"></a>Web hizmeti dağıtımı için kimlik doğrulaması

Azure Machine Learning Web Hizmetleri, anahtar ve belirteç için iki kimlik doğrulama biçimini destekler. Her bir Web hizmeti tek seferde yalnızca bir kimlik doğrulama biçimi etkinleştirebilir.

|Kimlik Doğrulama Yöntemi|ACI|AKS|
|---|---|---|
|Anahtar|Varsayılan olarak devre dışı| Varsayılan olarak etkin|
|Belirteç| Yok| Varsayılan olarak devre dışı |

#### <a name="authentication-with-keys"></a>Anahtarlar ile kimlik doğrulama

Bir dağıtım için anahtar kimlik doğrulamasını etkinleştirdiğinizde, otomatik olarak kimlik doğrulama anahtarları oluşturursunuz.

* Azure Kubernetes hizmetine dağıtım yaparken, varsayılan olarak kimlik doğrulaması etkinleştirilir.
* Azure Container Instances dağıtım yaparken, varsayılan olarak kimlik doğrulaması devre dışıdır.

Anahtar kimlik doğrulamasını etkinleştirmek için, bir `auth_enabled` dağıtım oluştururken veya güncelleştirirken parametresini kullanın.

Anahtar kimlik doğrulaması etkinleştirilirse, birincil ve ikincil kimlik doğrulama `get_keys` anahtarını almak için yöntemini kullanabilirsiniz:

```python
primary, secondary = service.get_keys()
print(primary)
```

> [!IMPORTANT]
> Bir anahtarı yeniden oluşturmanız gerekiyorsa, şunu kullanın[`service.regen_key`](https://docs.microsoft.com/python/api/azureml-core/azureml.core.webservice(class)?view=azure-ml-py)

#### <a name="authentication-with-tokens"></a>Belirteçlerle kimlik doğrulama

Bir Web hizmeti için belirteç kimlik doğrulamasını etkinleştirdiğinizde, bir kullanıcının, Web hizmetine erişmek için bir Azure Machine Learning JSON Web Token sağlaması gerekir.

* Azure Kubernetes hizmetine dağıtım yaparken belirteç kimlik doğrulaması varsayılan olarak devre dışıdır.
* Azure Container Instances dağıtım yaparken belirteç kimlik doğrulaması desteklenmez.

Belirteç kimlik doğrulamasını denetlemek için, bir `token_auth_enabled` dağıtım oluştururken veya güncelleştirirken parametresini kullanın.

Belirteç kimlik doğrulaması etkinleştirilirse, bir JWT belirteci almak için `get_token` yöntemini ve bu belirtecin sona erme süresini kullanabilirsiniz:

```python
token, refresh_by = service.get_token()
print(token)
```

> [!IMPORTANT]
> Belirtecin `refresh_by` zamanından sonra yeni bir belirteç istemeniz gerekir.
>
> Azure Machine Learning çalışma alanınızı Azure Kubernetes hizmet kümeniz ile aynı bölgede oluşturmanızı önemle öneririz. Bir belirteçle kimlik doğrulaması yapmak için Web hizmeti, Azure Machine Learning çalışma alanınızın oluşturulduğu bölgeye bir çağrı yapar. Çalışma alanınızın bölgesi kullanılamıyorsa, kümeniz çalışma alanınızdan farklı bir bölgedeyse, Web hizmeti için de bir belirteç getirimeyeceksiniz. Bu, çalışma alanınızın bölgesi yeniden kullanılabilir olana kadar Azure AD kimlik doğrulamasının kullanılamamasına neden olur. Ayrıca, kümenizin bölgesi ve çalışma alanınızın bölgesi arasındaki mesafe ne kadar fazlaysa bir belirteci getirmek için bu daha uzun sürer.

## <a name="authorization"></a>Authorization

Her bir çalışma alanı birden çok kişi tarafından paylaşılabilir ve birden çok çalışma alanı oluşturabilirsiniz. Bir çalışma alanını paylaştığınızda kullanıcılara aşağıdaki rolleri atayarak erişimi denetleyebilirsiniz:

* Sahip
* Katılımcı
* Okuyucu

Aşağıdaki tabloda, bazı önemli Azure Machine Learning hizmet işlemleri ve bunları gerçekleştirebilen roller listelenmektedir:

| Azure Machine Learning hizmeti Işlemi | Sahip | Katılımcı | Okuyucu |
| ---- |:----:|:----:|:----:|
| Çalışma Alanı Oluştur | ✓ | ✓ | |
| Çalışma alanını paylaşma | ✓ | |  |
| Işlem oluştur | ✓ | ✓ | |
| Işlem iliştirme | ✓ | ✓ | |
| Veri depoları Ekle | ✓ | ✓ | |
| Deneme çalıştırma | ✓ | ✓ | |
| Çalıştırmaları/ölçümleri görüntüle | ✓ | ✓ | ✓ |
| Modeli kaydetme | ✓ | ✓ | |
| Görüntü oluştur | ✓ | ✓ | |
| Web hizmetini dağıtma | ✓ | ✓ | |
| Modelleri/görüntüleri görüntüleme | ✓ | ✓ | ✓ |
| Web hizmetini çağır | ✓ | ✓ | ✓ |

Yerleşik roller gereksinimleriniz için yetersizse, özel roller de oluşturabilirsiniz. Desteklediğimiz tek özel roller, çalışma alanındaki işlemler ve Machine Learning İşlem. Özel rollerin çalışma alanında ve işlem kaynağında okuma, yazma veya silme izinleri olabilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanında kullanıcıları ve rolleri yönetme](how-to-assign-roles.md)

### <a name="securing-compute-and-data"></a>İşlem ve verilerin güvenliğini sağlama

Sahipler ve katkıda bulunanlar, çalışma alanına bağlı tüm işlem hedeflerini ve veri depolarını kullanabilir.  
Her çalışma alanında aynı zamanda, ilişkili sistem tarafından atanan yönetilen kimlik (çalışma alanıyla aynı ada sahip), çalışma alanında kullanılan eklenmiş kaynaklarda aşağıdaki izinlerle bulunur:

Yönetilen kimlikler hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/overview)

| Resource | İzinler |
| ----- | ----- |
| Çalışma alanı | Katılımcı |
| Depolama Hesabı | Depolama Blob Verileri Katkıda Bulunanı |
| Key Vault | Tüm anahtarlar, gizlilikler, sertifikalara erişim |
| Azure Container Registry | Katılımcı |
| Çalışma alanını içeren kaynak grubu | Katılımcı |
| Key Vault içeren kaynak grubu (çalışma alanını içeren bunlardan farklıysa) | Katılımcı |

Yöneticilerin yönetilen kimliğin erişimini, yukarıda bahsedilen kaynaklara iptal etmeleri önerilir. Erişim, yeniden eşitleme anahtarları işlemiyle geri yüklenebilir.

Azure Machine Learning hizmeti, her çalışma alanı bölgesi için aboneliğinizde katkıda `aml-`bulunan düzey erişimi olan ek bir uygulama (adıyla başlar) oluşturur. For ex. Doğu ABD ' de bir çalışma alanınız ve aynı abonelikte Kuzey Avrupa başka bir çalışma alanı varsa, bu iki uygulamayı da görürsünüz. Bu, Azure Machine Learning hizmetinin işlem kaynaklarını yönetmeye yardımcı olması için gereklidir.

## <a name="network-security"></a>Ağ güvenliği

Azure Machine Learning hizmeti, işlem kaynakları için diğer Azure hizmetlerini kullanır. İşlem kaynakları (işlem hedefleri) modelleri eğitmek ve dağıtmak için kullanılır. Bu işlem hedefleri, bir sanal ağ içinde oluşturulabilir. Örneğin, bir modeli eğitme ve sonra modeli Azure Kubernetes Service (AKS) ' e dağıtmak için Microsoft Veri Bilimi Sanal Makinesi kullanabilirsiniz.  

Daha fazla bilgi için bkz. [Sanal ağda denemeleri ve çıkarımı çalıştırma](how-to-enable-virtual-network.md).

## <a name="data-encryption"></a>Veri şifreleme

### <a name="encryption-at-rest"></a>Bekleme sırasında şifreleme

#### <a name="azure-blob-storage"></a>Azure Blob Depolama

Azure Machine Learning hizmeti, Azure Machine Learning hizmeti çalışma alanına bağlı olan ve kullanıcının aboneliğinde yer alan Azure Blob depolama hesabında anlık görüntüler, çıktılar ve Günlükler depolar. Azure Blob depolama alanında depolanan tüm veriler, Microsoft tarafından yönetilen anahtarlar kullanılarak Rest 'te şifrelenir.

Azure Blob depolama alanında depolanan veriler için kendi anahtarlarınızı getirme hakkında daha fazla bilgi için, [Depolama Hizmeti Şifrelemesi Azure Key Vault müşteri tarafından yönetilen anahtarları kullanma](https://docs.microsoft.com/azure/storage/common/storage-service-encryption-customer-managed-keys)konusuna bakın.

Eğitim verileri genellikle Azure Blob Storage 'da da depolanır, bu sayede işlem eğitimi için erişilebilir olur. Bu depolama Azure Machine Learning tarafından yönetilmez ancak uzak bir dosya sistemi olarak işlem 'a bağlanır.

Çalışma alanınızda kullanılan Azure depolama hesapları için erişim anahtarlarını yeniden oluşturma hakkında daha fazla bilgi için bkz. [depolama erişim anahtarlarını yeniden oluştur](how-to-change-storage-access-key.md) makalesi.

#### <a name="cosmos-db"></a>Cosmos DB

Azure Machine Learning hizmeti, ölçüm ve meta verileri Azure Machine Learning hizmeti tarafından yönetilen bir Microsoft aboneliğinde bulunan Cosmos DB depolar. Cosmos DB depolanan tüm veriler, Microsoft tarafından yönetilen anahtarlar kullanılarak Rest 'te şifrelenir.

#### <a name="azure-container-registry-acr"></a>Azure Container Registry (ACR)

Kayıt defterinizde (ACR) bulunan tüm kapsayıcı görüntüleri, bekleyen olarak şifrelenir. Azure, bir görüntüyü depolamadan önce otomatik olarak şifreler ve Azure Machine Learning hizmeti görüntüyü çeker.

#### <a name="machine-learning-compute"></a>Machine Learning İşlemi

Her işlem düğümü için işletim sistemi diski, Azure depolama 'da depolanır Azure Machine Learning hizmet depolama hesaplarında Microsoft tarafından yönetilen anahtarlar kullanılarak şifrelenir. Bu işlem hedefi kısa ömürlü ve hiçbir çalışan sıraya alma işlemi olmadığında kümeler normal şekilde ölçeklendirilir. Temel alınan sanal makine, için de sağlanmış ve işletim sistemi diski silinmiş. Azure disk şifrelemesi, işletim sistemi diski için desteklenmez.
Her bir sanal makinenin işletim sistemi işlemleri için yerel bir geçici diski de vardır. Disk, eğitim verilerini hazırlamak için isteğe bağlı olarak da kullanılabilir. Disk şifrelenmedi.
Rest 'in Azure 'da nasıl çalıştığı hakkında daha fazla bilgi için, bkz. [Azure veri şifreleme-Rest](https://docs.microsoft.com/azure/security/fundamentals/encryption-atrest).

### <a name="encryption-in-transit"></a>Aktarım sırasında şifreleme

Çeşitli Azure Machine Learning mikro hizmetler ile iç iletişimin yanı sıra Puanlama uç noktasını çağırmaya yönelik dış iletişim, SSL kullanılarak desteklenir. Tüm Azure depolama erişimi de güvenli bir kanal üzerinden yapılır.
Daha fazla bilgi için bkz. [SSL kullanarak güvenli Azure Machine Learning Web Hizmetleri](https://docs.microsoft.com/azure/machine-learning/service/how-to-secure-web-service).

### <a name="using-azure-key-vault"></a>Azure Key Vault kullanma

Çalışma alanıyla ilişkili Key Vault örnek, Azure Machine Learning hizmeti tarafından çeşitli türlerdeki kimlik bilgilerini depolamak için kullanılır:

* İlişkili depolama hesabı bağlantı dizesi
* Azure Container Repository deposu örneklerine parolalar
* Veri depolarına yönelik bağlantı dizeleri.

HDI HDInsight ve VM gibi hedefleri hesaplamak için SSH parolaları ve anahtarları, Microsoft aboneliğiyle ilişkili ayrı bir Key Vault depolanır. Azure Machine Learning hizmet, denemeleri çalıştırmak üzere VM/HDInsight 'a bağlanmak için kendi SSH anahtarlarını oluşturur, yetkilendirir ve depolar.
Her çalışma alanı, Key Vault tüm anahtarlar, gizlilikler ve sertifikalara erişimi olan ilişkili sistem tarafından atanan bir yönetilen kimliğe (çalışma alanıyla aynı ada sahip) sahiptir.

## <a name="monitoring"></a>İzleme

### <a name="metrics"></a>Ölçümler

Azure Izleyici ölçümleri, Azure Machine Learning hizmeti çalışma alanınızın ölçümlerini görüntülemek ve izlemek için kullanılabilir. [Azure Portal](https://portal.azure.com), çalışma alanınızı seçin ve ardından __ölçümler__ bağlantısını kullanın.

[![Bir çalışma alanı için örnek ölçümleri gösteren ekran görüntüsü](./media/enterprise-readiness/workspace-metrics.png)](./media/enterprise-readiness/workspace-metrics-expanded.png)

Ölçümler, çalıştırmalar, dağıtımlar ve kayıtlar hakkındaki bilgileri içerir.

Daha fazla bilgi için bkz. [Azure izleyici 'de ölçümler](/azure/azure-monitor/platform/data-platform-metrics).

### <a name="activity-log"></a>Etkinlik günlüğü

Çalışma alanında gerçekleştirilen çeşitli işlemleri görmek için etkinlik günlüğünü çalışma alanında görebilir ve işlem adı, olay tarafından başlatılan olay, zaman damgası vb. gibi temel bilgileri alın.

Aşağıdaki ekran görüntüsünde bir çalışma alanı için etkinlik günlüğü gösterilmektedir:

[![Bir çalışma alanı altındaki etkinlik günlüğünü gösteren ekran görüntüsü](./media/enterprise-readiness/workspace-activity-log.png)](./media/enterprise-readiness/workspace-activity-log-expanded.png)

Puanlama isteği ayrıntıları, çalışma alanı oluşturulurken kullanıcının aboneliğinde oluşturulan Application Insight 'da depolanır. Günlüğe kaydedilen bilgiler HTTPMethod, UserAgent, ComputeType, RequestUrl, StatusCode, RequestId, Duration vb. gibi alanları içerir.

> [!IMPORTANT]
> Azure Machine Learning çalışma alanındaki bazı eylemler etkinlik günlüğüne bilgi vermez. Örneğin, bir eğitimi başlatma veya bir modeli kaydetme.
>
> Bu eylemlerden bazıları, çalışma alanınızın __Etkinlikler__ alanında görünür, ancak aktiviteyi kimin başlattığını göstermez.

## <a name="data-flow-diagram"></a>Veri akışı diyagramı

### <a name="create-workspace"></a>Çalışma alanı oluşturma

Aşağıdaki diyagramda, çalışma alanı oluşturma iş akışı gösterilmektedir.
Kullanıcı, desteklenen Azure Machine Learning hizmet istemcilerinden (CLı, Python SDK, Azure portal) Azure AD 'de oturum açar ve uygun Azure Resource Manager belirtecini ister. Kullanıcı daha sonra çalışma alanını oluşturmak için Azure Resource Manager çağırır.  Azure Resource Manager, çalışma alanını sağlamak için Azure Machine Learning hizmeti kaynak sağlayıcısıyla iletişim kurar.  Çalışma alanı oluşturma sırasında müşterinin aboneliğinde ek kaynaklar oluşturulur:

* Anahtar Kasası (gizli dizileri depolamak için)
* Bir Azure depolama hesabı (blob & FileShare dahil)
* Azure Container Registry (çıkarım/Puanlama ve deneme için Docker görüntülerini depolamak için)
* Application Insights (Telemetriyi depolamak için)

Bir çalışma alanına bağlı diğer hesaplar (Azure Kubernetes hizmeti, VM vb.), gerektiğinde müşteriler tarafından da sağlanabilir.

[![Çalışma alanı oluştur iş akışını gösteren ekran görüntüsü](./media/enterprise-readiness/create-workspace.png)](./media/enterprise-readiness/create-workspace-expanded.png)

### <a name="save-source-code-training-scripts"></a>Kaynak kodunu kaydet (eğitim betikleri)

Aşağıdaki diyagramda, kod anlık görüntüsü iş akışı gösterilmektedir.
Bir Azure Machine Learning hizmet çalışma alanıyla ilişkili, kaynak kodu (eğitim betikleri) içeren dizinlerdir (denemeleri).  Bu betikler müşterinin yerel makinesinde ve bulutta depolanır (müşterinin aboneliği altındaki Azure Blob depolama alanında). Kod anlık görüntüleri, geçmiş denetimi için yürütme veya denetleme için kullanılır.

[![Çalışma alanı oluştur iş akışını gösteren ekran görüntüsü](./media/enterprise-readiness/code-snapshot.png)](./media/enterprise-readiness/code-snapshot-expanded.png)

### <a name="training"></a>Eğitim

Aşağıdaki diyagramda eğitim iş akışı gösterilmektedir.

* Azure Machine Learning hizmeti, yukarıda kaydedilen kod anlık görüntüsünün anlık görüntü KIMLIĞIYLE çağrılır
* Azure Machine Learning hizmeti, Azure Machine Learning hizmetine geri konuşmak için Machine Learning İşlem/VM gibi işlem hedefleri tarafından daha sonra kullanılan Azure Machine Learning hizmet belirtecini & çalıştırma KIMLIĞI (isteğe bağlı) oluşturur
* Yönetilen bir işlem (örn.) seçebilirsiniz. Machine Learning İşlem) veya yönetilmeyen işlem (örn. VM) eğitim işlerinizi çalıştırın. Veri akışı aşağıdaki senaryolar için açıklanmaktadır:
* (VM/HDInsight – Microsoft aboneliğindeki Key Vault SSH kimlik bilgileri kullanılarak erişilir) Azure Machine Learning hizmet, işlem hedefinde yönetim kodunu şu şekilde çalıştırır:

   1. Ortamı hazırlar. (Docker, VM ve yerel için de bir seçenektir. Docker kapsayıcısında deneme çalıştırmanın nasıl çalıştığını anlamak için Machine Learning İşlem için aşağıdaki adımlara bakın.)
   1. Kodu indirir.
   1. Ortam değişkenlerini ve yapılandırmaları ayarlar.
   1. Kullanıcı betiğini çalıştırır (yukarıda belirtilen kod anlık görüntüsü).

* (Machine Learning İşlem – çalışma alanı tarafından yönetilen kimlik kullanılarak erişilir) Machine Learning İşlem yönetilen bir işlem olduğundan, Microsoft aboneliği kapsamında çalıştığı bir sonuç olarak Microsoft tarafından yönetilir.

   1. Gerekirse uzak Docker oluşturma kapalıdır.
   1. Yönetim kodunu Kullanıcı Azure FileShare 'e yazar.
   1. Kapsayıcıyı bir başlangıç komutuyla, diğer bir deyişle, önceki adımda açıklandığı gibi yönetim kodu ile başlatır.

#### <a name="querying-runs-and-metrics"></a>Çalıştırmaları ve ölçümleri sorgulama

Bu adım, eğitim sürecinin, *çalışma ölçümlerini* Cosmos DB depolanacağı Azure Machine Learning hizmetine geri yazdığı akışta gösterilmektedir. İstemciler, Cosmos DB çekme ölçümlerini alacak ve istemciye geri döndürecek Azure Machine Learning hizmeti çağırabilir.

[![Çalışma alanı oluştur iş akışını gösteren ekran görüntüsü](./media/enterprise-readiness/training-and-metrics.png)](./media/enterprise-readiness/training-and-metrics-expanded.png)

### <a name="creating-web-services"></a>Web Hizmetleri oluşturma

Aşağıdaki diyagramda çıkarım iş akışı gösterilmektedir. Çıkarım veya model Puanlama, dağıtılan modelin tahmin için en yaygın olarak üretim verilerinde kullanıldığı aşamadır.
Ayrıntıları aşağıda görebilirsiniz:

* Kullanıcı Azure ML SDK gibi bir istemciyi kullanarak bir modeli kaydeder
* Kullanıcı model, puan dosyası ve diğer model bağımlılıklarını kullanarak görüntü oluşturuyor
* Docker görüntüsü oluşturulur ve ACR 'de depolanır
* Web hizmeti, yukarıda oluşturulan görüntüyü kullanarak işlem hedefine (acı/AKS) dağıtılır
* Puanlama isteği ayrıntıları, Kullanıcı aboneliğindeki Application Insight 'da depolanır
* Telemetri ayrıca Microsoft/Azure aboneliğine gönderilir

[![Çalışma alanı oluştur iş akışını gösteren ekran görüntüsü](./media/enterprise-readiness/inferencing.png)](./media/enterprise-readiness/inferencing-expanded.png)

## <a name="next-steps"></a>Sonraki adımlar

* [Azure Machine Learning web hizmetleri SSL ile güvenli hale getirme](how-to-secure-web-service.md)
* [Bir web hizmeti olarak ML modeli kullanma](how-to-consume-web-service.md)
* [Batch Öngörüler çalıştırma](how-to-run-batch-predictions.md)
* [Application Insights Azure Machine Learning modellerinizi izleyin](how-to-enable-app-insights.md)
* [Üretimde modeller için veri toplama](how-to-enable-data-collection.md)
* [Azure Machine Learning hizmeti SDK 'Sı](https://docs.microsoft.com/python/api/overview/azure/ml/intro?view=azure-ml-py)
* [Azure sanal ağları ile Azure Machine Learning hizmetini kullanma](how-to-enable-virtual-network.md)
* [Öneri sistemleri oluşturmak için en iyi uygulamalar](https://github.com/Microsoft/Recommenders)
* [Azure 'da gerçek zamanlı bir öneri API 'SI oluşturun](https://docs.microsoft.com/azure/architecture/reference-architectures/ai/real-time-recommendation)
