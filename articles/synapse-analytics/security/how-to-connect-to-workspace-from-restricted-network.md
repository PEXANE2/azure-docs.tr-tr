---
title: Kısıtlanmış bir ağdan SYNAPSE Studio çalışma alanı kaynağına bağlanma
description: Bu makalede, kısıtlı bir ağdan Azure SYNAPSE Studio çalışma alanı kaynaklarınıza nasıl bağlanacaksınız
author: xujxu
ms.service: synapse-analytics
ms.topic: how-to
ms.subservice: security
ms.date: 10/25/2020
ms.author: xujiang1
ms.reviewer: jrasnick
ms.openlocfilehash: 5d28b8f2ff3045c9fdf5e8a866419a22bfbc6504
ms.sourcegitcommit: 96918333d87f4029d4d6af7ac44635c833abb3da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/04/2020
ms.locfileid: "93321822"
---
# <a name="connect-to-synapse-studio-workspace-resources-from-a-restricted-network"></a>Kısıtlanmış bir ağdan SYNAPSE Studio çalışma alanı kaynaklarına bağlanma

Bu makalenin hedef okuyucu, şirketin kısıtlı ağını yöneten şirket BT yöneticudur. BT Yöneticisi, bu kısıtlı ağ içindeki iş istasyonu ile Azure SYNAPSE Studio arasındaki ağ bağlantısını etkinleştirmek üzere kullanılır.

Bu makalede, kısıtlı bir ağ ortamından Azure SYNAPSE çalışma alanınıza nasıl bağlanacağınızı öğreneceksiniz. 

## <a name="prerequisites"></a>Önkoşullar

* **Azure aboneliği** : Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir Azure hesabı](https://azure.microsoft.com/free/) oluşturun.
* **Azure SYNAPSE çalışma alanı** : bir Synapse Studio yoksa, Azure SYNAPSE Analytics 'Ten bir Synapse çalışma alanı oluşturun. Çalışma alanı adı aşağıdaki 4. adımda gerekecektir.
* **Kısıtlı ağ** : kısıtlı ağ, şirket BT Yöneticisi tarafından korunur. BT Yöneticisi, ağ ilkesini yapılandırma iznine sahiptir. Sanal ağ adı ve alt ağı aşağıdaki adım 3 ' te gerekli olacaktır.



## <a name="step-1-add-network-outbound-security-rules-to-the-restricted-network"></a>1. Adım: kısıtlanmış ağa ağ giden güvenlik kuralları ekleme

Dört hizmet etiketine sahip dört ağ giden güvenlik kuralı eklemeniz gerekir. [Hizmet etiketlerine genel bakış](/azure/virtual-network/service-tags-overview.md) hakkında daha fazla bilgi edinin 
* AzureResourceManager
* Azurefrontkapısı. ön uç
* AzureActiveDirectory
* AzureMonitor (Isteğe bağlı. Bu kural türünü yalnızca, verileri Microsoft 'a paylaşmak istediğinizde ekleyin.)

Giden kuralı ayrıntılarını aşağıda gösterildiği gibi **Azure Resource Manager** . Diğer üç kuralı oluştururken, " **hedef hizmet etiketi** " değerini, açılan seçim listesinden " **Azurefrontkapı. ön uç** ", " **AzureActiveDirectory** ", " **AzureMonitor** " hizmet etiketi adını seçerek değiştirin.

![AzureResourceManager](./media/how-to-connect-to-workspace-from-restricted-network/arm-servicetag.png)


## <a name="step-2-create-azure-synapse-analytics-private-link-hubs"></a>2. Adım: Azure SYNAPSE Analytics (özel bağlantı hub 'ları) oluşturma

Azure portal 'ten bir Azure SYNAPSE Analytics (özel bağlantı hub 'ları) oluşturmanız gerekir. Azure portal aracılığıyla " **Azure SYNAPSE Analytics (özel bağlantı hub 'ları)** " araması yapın ve gerekli alanı doldurup oluşturun. 

> [!Note]
> Bölge, SYNAPSE çalışma alanınızın bulunduğu bir ile aynı olmalıdır.

![SYNAPSE Analytics özel bağlantı hub 'ları oluşturma](./media/how-to-connect-to-workspace-from-restricted-network/private-links.png)

## <a name="step-3-create-private-link-endpoint-for-synapse-studio-gateway"></a>3. Adım: SYNAPSE Studio Gateway için özel bağlantı uç noktası oluşturma

SYNAPSE Studio Gateway 'e erişmek için Azure portal adresinden özel bağlantı uç noktası oluşturmanız gerekir. Azure portal aracılığıyla " **özel bağlantı** " araması yapın. " **Özel bağlantı merkezi** " içinde " **Özel uç nokta oluştur** " u seçin ve ardından gerekli alanı doldurup oluşturun. 

> [!Note]
> Bölge, SYNAPSE çalışma alanınızın bulunduğu bir ile aynı olmalıdır.

![SYNAPSE Studio 1 için özel uç nokta oluşturuluyor](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-1.png)

" **Kaynak** " öğesinin bir sonraki sekmesinde, yukarıdaki 2. adımda oluşturulan özel bağlantı merkezini seçin.

![SYNAPSE Studio 2 için özel uç nokta oluşturma](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-2.png)

" **Yapılandırma** " öğesinin bir sonraki sekmesinde, 
* " **Sanal ağ** " için sahip olduğunuz kısıtlı sanal ağ adını seçin.
* " **Alt ağ** " için kısıtlanmış sanal ağın alt ağını seçin. 
* " **Özel DNS bölgesi Ile tümleştirme** " Için " **Evet** " i seçin.

![SYNAPSE Studio 3 için özel uç nokta oluşturma](./media/how-to-connect-to-workspace-from-restricted-network/plink-endpoint-3.png)

Özel bağlantı uç noktası oluşturulduktan sonra, SYNAPSE Studio Web aracının oturum açma sayfasına erişebilirsiniz. Ancak, bir sonraki adımı tamamlamadan, SYNAPSE çalışma alanınızın içindeki kaynaklara henüz erişemeyeceksiniz.

## <a name="step-4-create-private-link-endpoints-for-synapse-studio-workspace-resource"></a>4. Adım: SYNAPSE Studio çalışma alanı kaynağı için özel bağlantı uç noktaları oluşturma

SYNAPSE Studio çalışma alanı kaynağınızın içindeki kaynaklara erişmek için, " **geliştirme** " türü " **hedef alt-kaynak** " olan en az bir özel bağlantı uç noktası oluşturmanız ve " **SQL** " veya " **sqlondemand** " türlerine sahip olan iki diğer Isteğe bağlı özel bağlantı uç noktası oluşturmanız gereken SYNAPSE Studio çalışma alanındaki kaynaklara bağlıdır. SYNAPSE Studio çalışma alanı için bu özel bağlantı noktası oluşturma, uç nokta oluşturma ile benzerdir.  

" **Kaynak** " sekmesinin altındaki alanlara dikkat edin:
* " **Kaynak türü** " Için " **Microsoft. SYNAPSE/Workspaces** " seçeneğini belirleyin.
* Daha önce oluşturduğunuz " **kaynak** " Için " **yourçalışmaalanıadı** " seçeneğini belirleyin.
* " **Hedef alt-kaynak** " bölümünde uç nokta türünü seçin:
  * **SQL: SQL** havuzunda SQL sorgu yürütmesi içindir.
  * **Sqlondemand** : SQL yerleşik sorgu yürütmesi içindir.
  * **Geliştirme** : SYNAPSE Studio çalışma alanları içindeki diğer her şeye erişmek içindir. Bu tür ile en az özel bağlantı uç noktası oluşturmanız gerekir.

![SYNAPSE Studio çalışma alanı için özel uç nokta oluşturuluyor](./media/how-to-connect-to-workspace-from-restricted-network/plinks-endpoint-ws-1.png)

Şimdi, All kümesi. SYNAPSE Studio çalışma alanı kaynağınızın erişimine erişebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

[Yönetilen çalışma alanı sanal ağı](./synapse-workspace-managed-vnet.md) hakkında daha fazla bilgi edinin

[Yönetilen özel uç noktalar](./synapse-workspace-managed-private-endpoints.md) hakkında daha fazla bilgi edinin
