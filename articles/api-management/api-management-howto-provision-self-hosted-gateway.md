---
title: Azure API Management şirket içinde barındırılan ağ geçidi sağlama | Microsoft Docs
description: Azure API Management 'de şirket içinde barındırılan bir ağ geçidini sağlamayı öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 03/31/2020
ms.author: apimpm
ms.openlocfilehash: e79248e16ca21ae84022f8ac7f280d93f489c6a4
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87050341"
---
# <a name="provision-a-self-hosted-gateway-in-azure-api-management"></a>Azure API Management şirket içinde barındırılan ağ geçidi sağlama

Azure API Management Örneğinizde bir ağ geçidi kaynağı sağlama, şirket içinde barındırılan bir ağ geçidini dağıtmak için bir önkoşuldur. Bu makalede, API Management bir ağ geçidi kaynağı sağlama adımları gösterilmektedir.

## <a name="prerequisites"></a>Önkoşullar

Şu hızlı başlangıcı doldurun: [Azure API Management örneği oluşturma](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="provision-a-self-hosted-gateway"></a>Şirket içinde barındırılan ağ geçidini sağlama

1. **Dağıtım ve altyapı**altında **ağ geçitlerini** seçin.
2. **+ Ekle**'ye tıklayın.
3. Ağ geçidinin **adını** ve **bölgesini** girin.
> [!TIP]
> **Bölge** , bu ağ geçidi kaynağıyla ilişkilendirilecek ağ geçidi düğümlerinin amaçlanan konumunu belirtir. Bu, herhangi bir Azure kaynağıyla ilişkili benzer bir özelliğe anlam olarak eşdeğerdir, ancak rastgele bir dize değeri atanabilir.

4. İsteğe bağlı olarak, ağ geçidi kaynağı için bir **Açıklama** girin.
5. İsteğe bağlı olarak, **+** Bu ağ geçidi kaynağıyla bir veya daha fazla API ilişkilendirmek Için **API 'ler** altında öğesini seçin.
> [!IMPORTANT]
> Varsayılan olarak, mevcut API 'lerden hiçbiri yeni ağ geçidi kaynağıyla ilişkilendirilecektir. Bu nedenle, bunları yeni ağ geçidi aracılığıyla çağırmaya yönelik girişimler yanıt olarak sonuçlanır `404 Resource Not Found` .

6. **Ekle**'ye tıklayın.

Ağ Geçidi kaynağı artık API Management örneğiniz tarafından sağlandı. Ağ geçidini dağıtmaya devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Şirket içinde barındırılan ağ geçidi hakkında daha fazla bilgi edinmek için bkz. [Azure API Management Self-barındırılan ağ geçidine genel bakış](self-hosted-gateway-overview.md)
* [Şirket içinde barındırılan ağ geçidini Kubernetes 'e dağıtma](how-to-deploy-self-hosted-gateway-kubernetes.md) hakkında daha fazla bilgi edinin
* [Şirket içinde barındırılan bir ağ geçidini Docker 'A dağıtma](how-to-deploy-self-hosted-gateway-docker.md) hakkında daha fazla bilgi edinin
