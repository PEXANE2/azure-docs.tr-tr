---
title: Kurumsal güvenlik ve idare
titleSuffix: Azure Machine Learning
description: 'Azure Machine Learning güvenli bir şekilde kullanın: kimlik doğrulama, yetkilendirme, ağ güvenliği, veri şifreleme ve izleme.'
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/09/2020
ms.openlocfilehash: eaaedc4404419e6028250a994077dd3df20b83d4
ms.sourcegitcommit: 6ab718e1be2767db2605eeebe974ee9e2c07022b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/12/2020
ms.locfileid: "94539660"
---
# <a name="enterprise-security-and-governance-for-azure-machine-learning"></a>Azure Machine Learning için kurumsal güvenlik ve idare

Bu makalede Azure Machine Learning için kullanılabilen güvenlik özellikleri hakkında bilgi edineceksiniz.

Bir bulut hizmeti kullandığınızda, erişimi yalnızca ihtiyacı olan kullanıcılarla kısıtlayabilmeniz en iyi uygulamadır. Hizmetini, hizmet tarafından kullanılan kimlik doğrulama ve yetkilendirme modelini inceleyerek başlatın. Ayrıca, ağ erişimini kısıtlamak veya şirket içi ağınızdaki kaynakları güvenli bir şekilde buluta katmak isteyebilirsiniz. Veri şifreleme Ayrıca, hem bekleyen hem de veriler hizmetler arasında taşınıyor. Ayrıca, uyumlu olmayan yapılandırmaların oluşturulması durumunda belirli yapılandırmaların veya günlüğün uygulanması için ilkeler oluşturmak isteyebilirsiniz. Son olarak, hizmeti izleyebilmeniz ve tüm etkinliklerin denetim günlüğünü üretmeniz gerekir.

> [!NOTE]
> Bu makaledeki bilgiler, Azure Machine Learning Python SDK sürümü 1.0.83.1 veya üzeri ile birlikte çalışmaktadır.

## <a name="authentication--authorization"></a>Kimlik doğrulama & yetkilendirmesi

Azure Machine Learning kaynakların çoğu kimlik doğrulaması için Azure Active Directory (Azure AD) ve yetkilendirme için rol tabanlı erişim denetimi (Azure RBAC) kullanır. Bunun özel durumları şunlardır:

* __SSH__ : Azure Machine Learning işlem örneği gibi bazı Işlem kaynaklarına SSH erişimini etkinleştirebilirsiniz. SSH erişimi anahtar tabanlı kimlik doğrulaması kullanır. SSH anahtarları oluşturma hakkında daha fazla bilgi için bkz. [SSH anahtarları oluşturma ve yönetme](../virtual-machines/linux/create-ssh-keys-detailed.md). SSH erişimini etkinleştirme hakkında daha fazla bilgi için bkz. [Azure Machine Learning işlem örneği oluşturma ve yönetme](how-to-create-manage-compute-instance.md).
* __Web Hizmetleri olarak dağıtılan modeller__ : Web hizmeti dağıtımları, __anahtar__ veya __belirteç__ tabanlı erişim denetimi kullanabilir. Anahtarlar statik dizelerdir. Belirteçler, bir Azure AD hesabı kullanılarak alınır. Daha fazla bilgi için bkz. [Web hizmeti olarak dağıtılan modeller için kimlik doğrulamasını yapılandırma](how-to-authenticate-web-service.md).

Azure veri depolama hizmetleri gibi Azure Machine Learning bağımlı olan belirli hizmetler kendi kimlik doğrulama ve yetkilendirme yöntemlerine sahiptir. Depolama Hizmetleri kimlik doğrulaması hakkında daha fazla bilgi için bkz. [Storage Services 'A bağlanma](how-to-access-data.md).

### <a name="azure-ad-authentication"></a>Azure AD kimlik doğrulaması

Azure Active Directory (Azure AD) kullanmak üzere yapılandırılmışsa Multi-Factor Authentication desteklenir. Kimlik doğrulama işlemi şu şekildedir:

1. İstemci Azure AD 'de oturum açar ve bir Azure Resource Manager belirteci alır.  Kullanıcılar ve hizmet sorumluları tam olarak desteklenmektedir.
1. İstemci belirteci Azure Resource Manager ve tüm Azure Machine Learning gösterir.
1. Machine Learning hizmeti, Kullanıcı işlem hedefine bir Machine Learning hizmet belirteci sağlar (örneğin, Machine Learning İşlem). Bu belirteç, Kullanıcı işlem hedefi tarafından, çalıştırma tamamlandıktan sonra Machine Learning hizmetine geri çağrı yapmak için kullanılır. Kapsam, çalışma alanıyla sınırlıdır.

[![Azure Machine Learning kimlik doğrulaması](media/concept-enterprise-security/authentication.png)](media/concept-enterprise-security/authentication.png#lightbox)

Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı Için kimlik doğrulaması](how-to-setup-authentication.md).

### <a name="azure-rbac"></a>Azure RBAC

Birden çok çalışma alanı oluşturabilirsiniz ve her çalışma alanı birden çok kişi tarafından paylaşılabilir. Azure AD hesabını Azure RBAC rollerine atayarak, kullanıcıların hangi özellikleri veya işlemlerini erişebileceğini kontrol edebilirsiniz. Yerleşik roller aşağıda verilmiştir:

* Sahip
* Katılımcı
* Okuyucu

Sahipler ve katkıda bulunanlar, çalışma alanına bağlı tüm işlem hedeflerini ve veri depolarını kullanabilir.  

Aşağıdaki tabloda, bazı önemli Azure Machine Learning işlemleri ve bunları gerçekleştirebilen roller listelenmektedir:

| Azure Machine Learning işlemi | Sahip | Katılımcı | Okuyucu |
| ---- |:----:|:----:|:----:|
| Çalışma alanı oluşturma | ✓ | ✓ | |
| Çalışma alanını paylaşma | ✓ | |  |
| İşlem hedefi oluştur | ✓ | ✓ | |
| İşlem hedefi Ekle | ✓ | ✓ | |
| Veri depoları Ekle | ✓ | ✓ | |
| Deneme Çalıştır | ✓ | ✓ | |
| Çalıştırmaları/ölçümleri görüntüle | ✓ | ✓ | ✓ |
| Modeli kaydetme | ✓ | ✓ | |
| Görüntü oluştur | ✓ | ✓ | |
| Web hizmetini dağıtma | ✓ | ✓ | |
| Modelleri/görüntüleri görüntüleme | ✓ | ✓ | ✓ |
| Web hizmetini çağır | ✓ | ✓ | ✓ |

Yerleşik roller ihtiyaçlarınızı karşılamıyorsa, özel roller oluşturabilirsiniz. Özel Roller bir çalışma alanı içindeki tüm işlemleri, örneğin işlem oluşturma, çalıştırma gönderme, bir veri deposunu kaydetme veya model dağıtma gibi tüm işlemleri denetler. Özel rollerin kümeler, veri depoları, modeller ve uç noktalar gibi bir çalışma alanının çeşitli kaynakları üzerinde okuma, yazma veya silme izinleri olabilir. Rolü belirli bir çalışma alanı düzeyinde, belirli bir kaynak grubu düzeyinde veya belirli bir abonelik düzeyinde kullanılabilir hale getirebilirsiniz. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanında kullanıcıları ve rolleri yönetme](how-to-assign-roles.md).

> [!IMPORTANT]
> Azure Machine Learning, Azure Blob depolama ve Azure Kubernetes hizmetleri gibi diğer Azure hizmetlerine bağımlıdır. Her Azure hizmetinin kendi Azure RBAC yapılandırması vardır. İstediğiniz erişim denetimi düzeyine ulaşmak için, Azure Machine Learning için hem Azure RBAC yapılandırması 'nı hem de Azure Machine Learning kullanılan Hizmetleri için uygulamanız gerekebilir.

> [!WARNING]
> Azure Machine Learning Azure Active Directory işletmeden işletmeye işbirliğiyle desteklenir, ancak şu anda Azure Active Directory işletmeden müşteriye işbirliğiyle desteklenmez.

### <a name="managed-identities"></a>Yönetilen kimlikler

Her çalışma alanı Ayrıca, çalışma alanıyla aynı ada sahip ilişkili bir sistem tarafından atanan [yönetilen kimliğe](../active-directory/managed-identities-azure-resources/overview.md) sahiptir. Yönetilen kimlik, çalışma alanı tarafından kullanılan kaynaklara güvenli bir şekilde erişmek için kullanılır. Bu, ekli kaynaklar üzerinde aşağıdaki izinlere sahiptir:

| Kaynak | İzinler |
| ----- | ----- |
| Çalışma alanı | Katılımcı |
| Depolama hesabı | Depolama Blob Verileri Katkıda Bulunanı |
| Key Vault | Tüm anahtarlar, gizlilikler, sertifikalara erişim |
| Azure Container Registry | Katılımcı |
| Çalışma alanını içeren kaynak grubu | Katılımcı |
| Anahtar kasasını içeren kaynak grubu (çalışma alanını içeren bunlardan farklıysa) | Katılımcı |

Yöneticilerin yönetilen kimliğin önceki tabloda bahsedilen kaynaklara erişimini iptal etmemenizi önermiyoruz. Yeniden eşitleme anahtarları işlemini kullanarak erişimi geri yükleyebilirsiniz.

Azure Machine Learning, `aml-` `Microsoft-AzureML-Support-App-` her çalışma alanı bölgesi için aboneliğinizde katkıda bulunan düzeyinde erişim ile ek bir uygulama (adı ile başlar) oluşturur. Örneğin, Doğu ABD ' de bir çalışma alanınız varsa ve bir diğeri aynı abonelikte Kuzey Avrupa, bu uygulamalardan ikisini de görürsünüz. Bu uygulamalar, işlem kaynaklarını yönetmenize yardımcı olmak için Azure Machine Learning sağlar.

İsteğe bağlı olarak, Azure sanal makineler ve Azure Machine Learning işlem kümesi ile birlikte kullanmak üzere kendi yönetilen kimliklerinizi yapılandırabilirsiniz. Bir VM ile yönetilen kimlik, tek bir kullanıcının Azure AD hesabı yerine SDK 'dan çalışma alanınıza erişmek için kullanılabilir. Bir işlem kümesi ile, yönetilen kimlik, eğitim işini çalıştıran kullanıcının erişimi olmayan güvenli veri depoları gibi kaynaklara erişmek için kullanılır. Daha fazla bilgi için bkz. [Azure Machine Learning çalışma alanı Için kimlik doğrulaması](how-to-setup-authentication.md).

## <a name="network-security-and-isolation"></a>Ağ güvenliği ve yalıtımı

Azure Machine Learning kaynaklara fiziksel erişimi kısıtlamak için Azure sanal ağ (VNet) kullanabilirsiniz. VNET 'ler, genel İnternet 'ten kısmen veya tamamen yalıtılmış olan ağ ortamları oluşturmanıza olanak tanır. Bu, çözümünüz için saldırı yüzeyini ve veri kaybı olasılığını azaltır.

Daha fazla bilgi için bkz. [sanal ağ yalıtımı ve gizliliğe genel bakış](how-to-network-security-overview.md).

<a id="encryption-at-rest"></a><a id="azure-blob-storage"></a>

## <a name="data-encryption"></a>Veri şifrelemesi

Azure Machine Learning çeşitli bilgi işlem kaynaklarını ve veri depolarını kullanır. Her birinin, bekleyen ve aktarım sırasında veri şifrelemesini nasıl desteklediğine ilişkin daha fazla bilgi edinmek için bkz. [Azure Machine Learning Ile veri şifreleme](concept-data-encryption.md).

### <a name="microsoft-generated-data"></a>Microsoft tarafından oluşturulan veriler

Microsoft, otomatik Machine Learning gibi hizmetleri kullanırken, birden çok modeli eğitmek için geçici, önceden işlenmiş bir veri oluşturabilir. Bu veriler, çalışma alanınızdaki bir veri deposunda depolanır ve bu da erişim denetimlerini ve şifrelemeyi uygun şekilde zorlamanıza olanak sağlar.

[Dağıtılmış uç noktanıza kaydedilen tanılama bilgilerini](how-to-enable-app-insights.md) Azure Application Insights örneğinize şifrelemek da isteyebilirsiniz.

## <a name="monitoring"></a>İzleme

Role ve izlenmekte göre Azure Machine Learning birkaç izleme senaryosu vardır.

| Rol | Kullanım için izleme | Açıklama |
| ---- | ----- | ----- |
| Yönetici, DevOps, MLOps | [Azure izleyici ölçümleri](#azure-monitor), [etkinlik günlüğü](#activity-log), [güvenlik açığı taraması](#vulnerability-scanning) | Hizmet düzeyi bilgileri |
| Veri bilimcisi, MLOps | [İzleme çalıştırmaları](#monitor-runs) | Eğitim çalıştırmaları sırasında günlüğe kaydedilen bilgiler |
| MLOps | [Model verilerini toplama](how-to-enable-data-collection.md), [Application Insights ile izleme](how-to-enable-app-insights.md) | Web Hizmetleri veya IoT Edge modülleri olarak dağıtılan modellerle günlüğe kaydedilen bilgiler|

### <a name="monitor-runs"></a>İzleme çalıştırmaları

Eğitim betiklerinizin içinden günlük kaydı bilgileri de dahil olmak üzere Azure Machine Learning deneme çalıştırmalarını izleyebilirsiniz. Bu bilgiler SDK, Azure CLı ve Studio aracılığıyla görüntülenebilir. Daha fazla bilgi için aşağıdaki makalelere bakın:

* [Eğitim çalıştırmalarını başlatın, izleyin ve iptal edin](how-to-manage-runs.md)
* [Günlükleri etkinleştirme](how-to-track-experiments.md)
* [Günlükleri görüntüleme](how-to-monitor-view-training-logs.md)
* [TensorBoard ile çalıştırmaları görselleştirme](how-to-monitor-tensorboard.md)

### <a name="azure-monitor"></a>Azure İzleyici

Azure Machine Learning çalışma alanınızın ölçümlerini görüntülemek ve izlemek için Azure Izleyici ölçümlerini kullanabilirsiniz. [Azure Portal](https://portal.azure.com), çalışma alanınızı seçin ve ardından **ölçümler** ' i seçin:

[![Bir çalışma alanı için örnek ölçümleri gösteren ekran görüntüsü](media/concept-enterprise-security/workspace-metrics.png)](media/concept-enterprise-security/workspace-metrics-expanded.png#lightbox)

Ölçümler, çalıştırmalar, dağıtımlar ve kayıtlar hakkındaki bilgileri içerir.

Daha fazla bilgi için bkz. [Azure izleyici 'de ölçümler](../azure-monitor/platform/data-platform-metrics.md).

### <a name="activity-log"></a>Etkinlik günlüğü

Çalışma alanında gerçekleştirilen çeşitli işlemleri görmek için bir çalışma alanının etkinlik günlüğünü görüntüleyebilirsiniz. Günlük, işlem adı, olay başlatıcısı ve zaman damgası gibi temel bilgileri içerir.

Bu ekran görüntüsünde bir çalışma alanının etkinlik günlüğü gösterilmektedir:

[![Bir çalışma alanının etkinlik günlüğünü gösteren ekran görüntüsü](media/concept-enterprise-security/workspace-activity-log.png)](media/concept-enterprise-security/workspace-activity-log-expanded.png#lightbox)

Puanlama isteği ayrıntıları Application Insights depolanır. Application Insights, bir çalışma alanı oluşturduğunuzda aboneliğinizde oluşturulur. Günlüğe kaydedilen bilgiler aşağıdakiler gibi alanları içerir:

* HTTPMethod
* Kullanıcı
* ComputeType
* RequestUrl
* Durum
* No
* Süre

> [!IMPORTANT]
> Azure Machine Learning çalışma alanındaki bazı eylemler etkinlik günlüğüne bilgileri günlüğe vermez. Örneğin, bir eğitimin başlangıcı ve bir modelin kaydı günlüğe kaydedilmez.
>
> Bu eylemlerden bazıları, çalışma alanınızın **Etkinlikler** alanında görünür ancak bu bildirimler etkinliği kimin başlattığını göstermez.

### <a name="vulnerability-scanning"></a>Güvenlik açığı taraması

Azure Güvenlik Merkezi, hibrit bulut iş yükleri arasında birleşik güvenlik yönetimi ve gelişmiş tehdit koruması sağlar. Azure Machine Learning için Azure Container Registry kaynağınızın ve Azure Kubernetes hizmet kaynaklarınızın taranmasını etkinleştirmeniz gerekir. Bkz. Güvenlik Merkezi [Ile Azure Kubernetes hizmet tümleştirmesi ve Güvenlik Merkezi ile](../security-center/defender-for-kubernetes-introduction.md) [Azure Container Registry görüntü tarama](../security-center/defender-for-container-registries-introduction.md) .

## <a name="audit-and-manage-compliance"></a>Uyumluluğu denetleme ve yönetme

[Azure ilkesi](../governance/policy/index.yml) , Azure kaynaklarının ilkelerinizle uyumlu olduğundan emin olmanızı sağlayan bir idare aracıdır. Azure Machine Learning, aşağıdaki ilkeleri atayabilirsiniz:

* **Müşteri tarafından yönetilen anahtar** : çalışma alanlarının müşteri tarafından yönetilen anahtar kullanması gerekip gerekmediğini denetleyin veya zorlayın.
* **Özel bağlantı** : çalışma alanlarının bir sanal ağla iletişim kurmak için özel uç nokta kullanıp kullanmadığını denetleyin.

Azure Ilkesi hakkında daha fazla bilgi için bkz. [Azure ilkesi belgeleri](../governance/policy/overview.md).

Azure Machine Learning özgü ilkeler hakkında daha fazla bilgi için bkz. [Azure ilkesiyle uyumluluğu denetleme ve yönetme](how-to-integrate-azure-policy.md).

## <a name="resource-locks"></a>Kaynak kilitleri

[!INCLUDE [resource locks](../../includes/machine-learning-resource-lock.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [TLS ile güvenli Azure Machine Learning Web Hizmetleri](how-to-secure-web-service.md)
* [Web hizmeti olarak dağıtılan bir Machine Learning modeli kullanma](how-to-consume-web-service.md)
* [Azure Güvenlik Duvarı ile Azure Machine Learning kullanma](how-to-access-azureml-behind-firewall.md)
* [Azure sanal ağ ile Azure Machine Learning kullanma](how-to-network-security-overview.md)
* [REST ve iletim sırasında veri şifrelemesi](concept-data-encryption.md)
* [Azure 'da gerçek zamanlı bir öneri API 'SI oluşturun](/azure/architecture/reference-architectures/ai/real-time-recommendation)
