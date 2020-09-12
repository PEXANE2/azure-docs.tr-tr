---
title: Özel uç nokta yapılandırma (Önizleme)
titleSuffix: Azure Machine Learning
description: Azure Machine Learning çalışma alanınıza sanal bir ağdan güvenli bir şekilde erişmek için Azure özel bağlantısı 'nı kullanın.
services: machine-learning
ms.service: machine-learning
ms.subservice: core
ms.topic: conceptual
ms.custom: how-to
ms.author: aashishb
author: aashishb
ms.reviewer: larryfr
ms.date: 09/03/2020
ms.openlocfilehash: b9b256a3d3c1636cac55bcb1790182240d2199c0
ms.sourcegitcommit: f8d2ae6f91be1ab0bc91ee45c379811905185d07
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/10/2020
ms.locfileid: "89661880"
---
# <a name="configure-azure-private-link-for-an-azure-machine-learning-workspace-preview"></a>Azure Machine Learning çalışma alanı için Azure özel bağlantısını yapılandırma (Önizleme)

Bu belgede, Azure özel bağlantısının Azure Machine Learning çalışma alanıyla nasıl kullanılacağını öğrenirsiniz. Azure Machine Learning için bir sanal ağ ayarlama hakkında daha fazla bilgi için bkz. [sanal ağ yalıtımı ve gizliliğe genel bakış](how-to-network-security-overview.md)

> [!IMPORTANT]
> Azure Machine Learning çalışma alanı ile Azure özel bağlantısı 'nın kullanımı Şu anda genel önizlemededir. Bu işlevsellik yalnızca **ABD Doğu**, **ABD Orta Güney** ve **ABD Batı 2** bölgelerinde kullanılabilir. Bu önizleme, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yükleri için önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure özel bağlantısı, özel bir uç nokta kullanarak çalışma alanınıza bağlanmanızı sağlar. Özel uç nokta, sanal ağınız içindeki özel IP adresleri kümesidir. Daha sonra çalışma alanınıza erişimi yalnızca özel IP adresleri üzerinden oluşacak şekilde sınırlayabilirsiniz. Özel bağlantı, veri taşması riskini azaltmaya yardımcı olur. Özel uç noktalar hakkında daha fazla bilgi için bkz. [Azure özel bağlantı](/azure/private-link/private-link-overview) makalesi.

> [!IMPORTANT]
> Azure özel bağlantısı, çalışma alanını silme veya işlem kaynaklarını yönetme gibi Azure denetim düzlemi 'ni (yönetim işlemleri) etkilemez. Örneğin, bir işlem hedefi oluşturma, güncelleştirme veya silme. Bu işlemler, genel Internet üzerinden normal olarak gerçekleştirilir. Azure Machine Learning Studio, API 'Ler (yayınlanan işlem hatları dahil) veya SDK özel uç noktasını kullanır gibi veri düzlemi işlemleri.
>
> Mozilla Firefox kullanıyorsanız, çalışma alanınızın özel uç noktasına erişmeye çalışırken sorunlarla karşılaşabilirsiniz. Bu sorun, Mozilla 'de HTTPS üzerinden DNS ile ilişkili olabilir. Google Chrome 'un Microsoft Edge 'i geçici çözüm olarak kullanmanızı öneririz.

> [!TIP]
> Azure Machine Learning işlem örneği, bir çalışma alanı ve özel uç nokta ile kullanılabilir. Bu özellik şu anda **ABD Doğu**, **ABD Orta Güney** ve **ABD Batı 2** bölgelerinde genel önizlemededir.

## <a name="create-a-workspace-that-uses-a-private-endpoint"></a>Özel uç nokta kullanan bir çalışma alanı oluşturma

> [!IMPORTANT]
> Şu anda yalnızca yeni bir Azure Machine Learning çalışma alanı oluştururken özel bir uç noktanın etkinleştirilmesini destekliyoruz.

' De şablon, [https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced](https://github.com/Azure/azure-quickstart-templates/tree/master/201-machine-learning-advanced) özel bir uç nokta ile çalışma alanı oluşturmak için kullanılabilir.

Özel uç noktalar dahil bu şablonu kullanma hakkında daha fazla bilgi için bkz. [Azure Machine Learning için bir çalışma alanı oluşturmak üzere Azure Resource Manager şablonu kullanma](how-to-create-workspace-template.md).

## <a name="using-a-workspace-over-a-private-endpoint"></a>Özel bir uç nokta üzerinde çalışma alanı kullanma

Çalışma alanına yönelik iletişime yalnızca sanal ağdan izin verildiğinden, çalışma alanını kullanan tüm geliştirme ortamları sanal ağın üyesi olmalıdır. Örneğin, sanal ağdaki bir sanal makine.

> [!IMPORTANT]
> Microsoft, bağlantının geçici kesintisini önlemek için özel bağlantı etkinleştirildikten sonra çalışma alanına bağlanan makinelerde DNS önbelleğini temizlemeye öneriyor. 

Azure sanal makineleri hakkında daha fazla bilgi için bkz. [sanal makineler belgeleri](/azure/virtual-machines/).


## <a name="next-steps"></a>Sonraki adımlar

Azure Machine Learning çalışma alanınızı güvenli hale getirme hakkında daha fazla bilgi için [sanal ağ yalıtımı ve gizliliği genel bakış](how-to-network-security-overview.md) makalesine bakın.