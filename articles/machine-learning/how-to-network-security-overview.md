---
title: Sanal ağ yalıtımı ve gizliliği genel bakış
titleSuffix: Azure Machine Learning
description: Çalışma alanı kaynaklarını ve işlem ortamlarını güvenli hale getirmek için Azure Machine Learning ile yalıtılmış bir Azure sanal ağı kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.reviewer: larryfr
ms.author: aashishb
author: aashishb
ms.date: 07/07/2020
ms.topic: conceptual
ms.custom: how-to, devx-track-python, references_regions
ms.openlocfilehash: 57746b833e238bbd0cc99ba103f710a9239ee5ba
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89665160"
---
# <a name="virtual-network-isolation-and-privacy-overview"></a>Sanal ağ yalıtımı ve gizliliği genel bakış

Bu makalede, Azure Machine Learning ' de ağ iletişimini güvenli hale getirmek için sanal ağların (VNet 'ler) nasıl kullanılacağını öğrenirsiniz. Bu makalede, bir sanal ağın tamamını nasıl yapılandıracağınızı göstermek için örnek bir senaryo kullanılmaktadır.

Bu makale, bir Azure Machine Learning iş akışını güvenli hale getirmek için size kılavuzluk eden beş bölümlü bir serinin bir parçasıdır. Öncelikle kavramları anlamak için bu genel bakış makalesini okumanız önemle tavsiye ederiz. 

Bu serideki diğer makaleler şunlardır:

**1. VNET 'e genel bakış**  >  [2. Çalışma alanı](how-to-secure-workspace-vnet.md)3 ' ü güvenli hale getirin  >  [. Eğitim ortamının](how-to-secure-training-vnet.md)4 ' ü koruyun  >  [. Invenli ortam](how-to-secure-inferencing-vnet.md)5 ' i güvenli hale getirin  >  [. Studio işlevselliğini etkinleştir](how-to-enable-studio-virtual-network.md)

## <a name="prerequisites"></a>Ön koşullar

Bu makalede, aşağıdaki konularda bilginiz olduğunu varsaymaktadır:
+ [Azure sanal ağları](https://docs.microsoft.com/azure/virtual-network/virtual-networks-overview)
+ [IP ağı](https://docs.microsoft.com/azure/virtual-network/virtual-network-ip-addresses-overview-arm)
+ [Azure Özel Bağlantı](how-to-configure-private-link.md)
+ [Ağ Güvenlik Grupları (NSG)](../virtual-network/security-overview.md)
+ [Ağ güvenlik duvarları](../firewall/overview.md)

## <a name="example-scenario"></a>Örnek senaryo

Bu bölümde, ortak bir ağ senaryosunun özel IP adresleriyle iletişim Azure Machine Learning güvenli hale getirmek için nasıl ayarlandığını öğrenirsiniz.

Aşağıdaki tabloda, hizmetlerin hem VNet hem de VNet olmadan Azure Machine Learning ağının farklı bölümlerine erişme şekli karşılaştırılır.

| Senaryo | Çalışma alanı | İlişkili kaynaklar | Eğitim işlem ortamı | In, işlem ortamını sınırlama |
|-|-|-|-|-|-|
|**Sanal ağ yok**| Genel IP | Genel IP | Genel IP | Genel IP |
|**Bir sanal ağdaki kaynakları güvenli hale getirme**| Özel IP (Özel uç nokta) | Genel IP (hizmet uç noktası) <br> **veya** <br> Özel IP (Özel uç nokta) | Özel IP | Özel IP  | 

* **Çalışma alanı** -çalışma alanındaki özel bağlantıya bağlanmak için sanal ağınızdan özel bir uç nokta oluşturun. Özel uç nokta, birkaç özel IP adresi aracılığıyla çalışma alanını VNET 'e bağlar.
* **İlişkili kaynak** -Azure depolama, Azure Key Vault ve Azure Container Services gibi çalışma alanı kaynaklarına bağlanmak için hizmet uç noktalarını veya özel uç noktaları kullanın.
    * **Hizmet uç noktaları** , sanal ağınızın kimliğini Azure hizmetine sağlar. Sanal ağınızda hizmet uç noktalarını etkinleştirdikten sonra, Azure hizmet kaynaklarını sanal ağınıza güvenli hale getirmek için bir sanal ağ kuralı ekleyebilirsiniz. Hizmet uç noktaları genel IP adreslerini kullanır.
    * **Özel uç noktalar** , sizi Azure özel bağlantısı tarafından desteklenen bir hizmete güvenli bir şekilde bağlayan ağ arabirimlerdir. Özel uç nokta, sanal ağınızdan bir özel IP adresi kullanarak hizmeti sanal ağınıza etkin bir şekilde getiriyor.
* **Eğitim işlem erişimi** -erişim eğitimi, Azure Machine Learning işlem örneği ve Azure Machine Learning işlem kümeleri gıbı özel IP adresleriyle güvenli şekilde işlem hedefleri. 
* **İşlem erişimini sınırlama** -özel IP adresleriyle Azure Kubernetes Hizmetleri (aks) işlem kümelerine erişin.


Sonraki beş bölümde yukarıda açıklanan ağ senaryosunun güvenliğini sağlama gösterilmektedir. Ağınızı güvenli hale getirmek için şunları yapmanız gerekir:

1. [**Çalışma alanının ve ilişkili kaynakların**](#secure-the-workspace-and-associated-resources)güvenliğini sağlayın.
1. [**Eğitim ortamının**](#secure-the-training-environment)güvenliğini sağlama.
1. [**İkinci dereceden sınırlama ortamının**](#secure-the-inferencing-environment)güvenliğini sağlama.
1. İsteğe bağlı: [**Studio işlevselliğini etkinleştirin**](#optional-enable-studio-functionality).
1. [ **Güvenlik Duvarı ayarlarını** yapılandırma](#configure-firewall-settings)

> [!TIP]
>  Bazı sanal ağ ve Azure hizmetleri birleşimleri, Enterprise Edition çalışma alanı gerektirir. Hangi senaryoların Enterprise Edition gerektirdiğini anlamak için aşağıdaki tabloyu kullanın:
>
> | Senaryo | Enterprise</br>Edition | Temel</br>Edition |
> | ----- |:-----:|:-----:| 
> | Sanal ağ veya özel bağlantı yok | ✔ | ✔ |
> | Özel bağlantı olmadan çalışma alanı. Bir sanal ağdaki diğer kaynaklar (Azure Container Registry hariç) | ✔ | ✔ |
> | Özel bağlantı olmadan çalışma alanı. Özel bağlantısı olan diğer kaynaklar | ✔ | |
> | Özel bağlantılı çalışma alanı. Bir sanal ağdaki diğer kaynaklar (Azure Container Registry hariç) | ✔ | ✔ |
> | Çalışma alanı ve özel bağlantısı olan diğer kaynaklar | ✔ | |
> | Özel bağlantılı çalışma alanı. Özel bağlantı veya sanal ağ içermeyen diğer kaynaklar | ✔ | ✔ |
> | Bir sanal ağda Azure Container Registry | ✔ | |
> | Çalışma alanı için müşteri tarafından yönetilen anahtarlar | ✔ | |
>


## <a name="secure-the-workspace-and-associated-resources"></a>Çalışma alanının ve ilişkili kaynakların güvenliğini sağlama

Çalışma alanınızı ve ilişkili kaynaklarınızı güvenli hale getirmek için aşağıdaki adımları kullanın. Bu adımlar, hizmetlerinizin sanal ağda iletişim kurmasına izin verir.

1. VNet ve çalışma alanınız arasında iletişimi etkinleştirmek için [özel bir bağlantı etkin çalışma alanı](how-to-secure-workspace-vnet.md#secure-the-workspace-with-private-endpoint) oluşturun.
1. Bir [hizmet uç noktası](../key-vault/general/overview-vnet-service-endpoints.md) veya [Özel uç nokta](../key-vault/general/private-link-service.md)ile sanal ağa Azure Key Vault ekleyin. Key Vault ["Güvenilen Microsoft hizmetlerinin bu güvenlik duvarını atlamasına Izin ver"](how-to-secure-workspace-vnet.md#secure-azure-key-vault)olarak ayarlayın.
1. Azure depolama hesabınızı sanal ağa bir [hizmet uç noktası](how-to-secure-workspace-vnet.md#secure-azure-storage-accounts) veya [özel bir uç nokta](../storage/common/storage-private-endpoints.md) ile ekleyin
1. [Azure Container Registry özel bir uç nokta kullanacak şekilde yapılandırın](how-to-secure-workspace-vnet.md#enable-azure-container-registry-acr) ve [Azure Container Instances alt ağ temsilcisini etkinleştirin](how-to-secure-inferencing-vnet.md#enable-azure-container-instances-aci).

![Çalışma alanının ve ilişkili kaynakların, hizmet uç noktaları veya VNet 'in içindeki özel uç noktalar üzerinden nasıl iletişim kurduğunu gösteren mimari diyagramı](./media/how-to-network-security-overview/secure-workspace-resources.png)

Bu adımların nasıl tamamlanacağı hakkında ayrıntılı yönergeler için, bkz. [güvenli bir Azure Machine Learning çalışma alanı](how-to-secure-workspace-vnet.md). 

### <a name="limitations"></a>Sınırlamalar

Çalışma alanınızın ve bir sanal ağ içindeki ilişkili kaynakların güvenliğini sağlamak aşağıdaki sınırlamalara sahiptir:
- Çalışma alanı özel bağlantısı yalnızca şu bölgelerde kullanılabilir: eastus, westus2, Güneydoğu ABD
    - Bu sınırlama, ilişkili kaynaklar için geçerlidir. Örneğin, herhangi bir Azure Machine Learning bölgesinde depolama için VNet 'i etkinleştirebilirsiniz.
- Tüm kaynaklar aynı VNet 'in arkasında olmalıdır. Ancak, aynı sanal ağ içindeki alt ağlara izin verilir.
- Tasarımcı, otomatik ml, etiketleme ve veri profili oluşturma gibi bazı Studio özellikleri, özel bir uç nokta kullanmak üzere yapılandırılmış depolama hesaplarıyla kullanılamaz. Bu Studio özelliklerini kullanmanız gerekiyorsa, bunun yerine hizmet uç noktalarını kullanın.

## <a name="secure-the-training-environment"></a>Eğitim ortamının güvenliğini sağlama

Bu bölümde, Azure Machine Learning 'de eğitim ortamının güvenliğini nasıl sağlayacağınızı öğreneceksiniz. Ayrıca, ağ yapılandırmalarının birlikte nasıl çalıştığını anlamak için Azure Machine Learning bir eğitim işini nasıl tamamladığını öğrenirsiniz.

Eğitim ortamının güvenliğini sağlamak için aşağıdaki adımları kullanın:

1. Eğitim işini çalıştırmak için [Sanal ağda bir Azure Machine Learning işlem örneği ve bilgisayar kümesi](how-to-secure-training-vnet.md#compute-instance) oluşturun.
1. Batch hizmetinin işleri işlem kaynaklarınıza gönderebilmesi için [Azure Batch hizmetinden gelen Iletişime Izin verin](how-to-secure-training-vnet.md#mlcports) . 

![Yönetilen işlem kümelerinin ve örneklerinin nasıl güvence altına alınacağını gösteren mimari diyagramı](./media/how-to-network-security-overview/secure-training-environment.png)

Bu adımların nasıl tamamlanacağı hakkında ayrıntılı yönergeler için bkz. [bir eğitim ortamını güvenli hale getirme](how-to-secure-training-vnet.md). 

### <a name="example-training-job-submission"></a>Örnek eğitim işi gönderimi 

Bu bölümde, eğitim işi göndermek için Azure Machine Learning hizmetleri arasında güvenli bir şekilde nasıl iletişim kurduğu hakkında bilgi edineceksiniz. Bu, iletişimin güvenliğini sağlamak için tüm yapılandırmalarınızın birlikte nasıl çalıştığını gösterir.

1. İstemci, eğitim betikleri ve eğitim verilerini bir hizmet veya özel uç nokta ile korunan depolama hesaplarına yükler.

1. İstemci, Azure Machine Learning çalışma alanına özel uç nokta aracılığıyla bir eğitim işi gönderir.

1. Azure Batch Hizmetleri işi çalışma alanından alır ve bilgi işlem kaynağıyla birlikte sağlanan ortak yük dengeleyici aracılığıyla eğitim işini işlem ortamına gönderir. 

1. İşlem kaynağı işi alır ve eğitime başlar. İşlem kaynakları, eğitim dosyalarını indirmek ve çıktıyı karşıya yüklemek için güvenli depolama hesaplarına erişir. 

![VNet kullanılırken bir Azure Machine Learning eğitim işinin gönderilme şeklini gösteren mimari diyagramı](./media/how-to-network-security-overview/secure-training-job-submission.png)


### <a name="limitations"></a>Sınırlamalar

- Azure Işlem örneği ve Azure Işlem kümeleri, çalışma alanı ve ilişkili kaynakları ile aynı VNet, bölge ve abonelikte olmalıdır. 

## <a name="secure-the-inferencing-environment"></a>İkinci dereceden sınırlama ortamının güvenliğini sağlama

Bu bölümde, bir ınıya sınırlama ortamının güvenliğini sağlamak için kullanılabilen seçenekleri öğrenirsiniz. Yüksek ölçekli, üretim dağıtımları için Azure Kubernetes Hizmetleri (AKS) kümelerini kullanmanızı öneririz.

Bir sanal ağdaki AKS kümeleri için iki seçeneğiniz vardır:

- Sanal ağınıza varsayılan bir AKS kümesi dağıtın veya ekleyin.
- Sanal ağınıza özel bir AKS kümesi iliştirin.

**Varsayılan AKS kümelerinin** genel IP adreslerine sahip bir denetim düzlemi vardır. Dağıtım sırasında sanal ağınıza varsayılan bir AKS kümesi ekleyebilir veya oluşturulduktan sonra bir küme iliştirebilirsiniz.

**Özel AKS kümelerinde** yalnızca özel IP 'ler üzerinden erişilebilen bir denetim düzlemi vardır. Küme oluşturulduktan sonra özel AKS kümeleri eklenmelidir.

Varsayılan ve özel kümelerin nasıl ekleneceği hakkında ayrıntılı yönergeler için bkz. [ınvenli bir ortamı güvenli hale getirme](how-to-secure-inferencing-vnet.md). 

Aşağıdaki ağ diyagramı, sanal ağa bağlı bir özel AKS kümesi ile güvenli bir Azure Machine Learning çalışma alanı gösterir.

![Sanal ağa özel AKS kümesi eklemeyi gösteren mimari diyagramı. AKS denetim düzlemi, müşteri sanal ağı dışına yerleştirildi](./media/how-to-network-security-overview/secure-inferencing-environment.png)

### <a name="limitations"></a>Sınırlamalar
- AKS kümeleri çalışma alanıyla aynı VNet 'e ve ilişkili kaynaklarına ait olmalıdır. 

## <a name="optional-enable-studio-functionality"></a>İsteğe bağlı: Studio işlevselliğini etkinleştir

[Çalışma alanının](#secure-the-workspace-and-associated-resources)  >  güvenliğini sağlama [Eğitim ortamının](#secure-the-training-environment)  >  güvenliğini sağlama [İkinci dereceden sınırlama ortamının](#secure-the-inferencing-environment)  >  güvenliğini sağlama **Studio Işlevselliğini etkinleştir**  >  [Güvenlik Duvarı ayarlarını yapılandırma](#configure-firewall-settings)

Studio, bir hizmet uç noktası ile yapılandırılmış bir depolama hesabındaki verilere erişebilse de bazı özellikler varsayılan olarak devre dışıdır:

* Studio 'daki verileri önizleyin.
* Tasarımcıda verileri görselleştirin.
* Bir oto ml denemesi gönder.
* Etiketleme projesi başlatın.

Bir depolama hizmeti uç noktası kullanırken tam işlevselliği etkinleştirmek için bkz. [sanal bir ağda Azure Machine Learning Studio 'Yu kullanma](how-to-enable-studio-virtual-network.md#access-data-using-the-studio). Şu anda, Studio, depolama özel uç noktalarını desteklemez.

### <a name="limitations"></a>Sınırlamalar
- Studio, Özel uç noktaları kullanacak şekilde yapılandırılmış depolama hesaplarındaki verilere erişemez. Tam işlevsellik için, depolama için hizmet uç noktalarını kullanmanız ve yönetilen kimlik kullanmanız gerekir.

## <a name="configure-firewall-settings"></a>Güvenlik duvarı ayarlarını yapılandırma

Güvenlik duvarınızı, Azure Machine Learning çalışma alanı kaynaklarınıza ve genel İnternet 'e erişimi denetlemek üzere yapılandırın. Azure Güvenlik Duvarı 'nı önerdiğimiz sırada ağınızın güvenliğini sağlamak için diğer güvenlik duvarı ürünlerini de kullanabilmeniz gerekir. Güvenlik duvarınız üzerinden iletişime izin verme hakkında sorularınız varsa, lütfen kullandığınız güvenlik duvarının belgelerine başvurun.

Güvenlik Duvarı ayarları hakkında daha fazla bilgi için bkz. [bir güvenlik duvarının arkasında çalışma alanı kullanma](how-to-access-azureml-behind-firewall.md).

## <a name="next-steps"></a>Sonraki adımlar

Bu makale, dört bölümlü bir sanal ağ serisinin bir parçasıdır. Bir sanal ağın güvenliğini nasıl sağlayacağınızı öğrenmek için makalelerin geri kalanına bakın:

* [2. Bölüm: sanal ağa genel bakış](how-to-secure-workspace-vnet.md)
* [3. kısım: eğitim ortamının güvenliğini sağlama](how-to-secure-training-vnet.md)
* [4. Bölüm: ikinci dereceden sınırlama ortamının güvenliğini sağlama](how-to-secure-inferencing-vnet.md)
* [5. Bölüm: Studio işlevselliğini etkinleştirme](how-to-enable-studio-virtual-network.md)
