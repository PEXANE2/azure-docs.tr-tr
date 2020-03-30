---
title: Azure API Yönetimi'nde kendi kendine barındırılan bir ağ geçidi sağlama | Microsoft Dokümanlar
description: Azure API Yönetimi'nde kendi kendine barındırılan bir ağ geçidini nasıl sağladığınızı öğrenin.
services: api-management
documentationcenter: ''
author: miaojiang
manager: gwallace
editor: ''
ms.service: api-management
ms.workload: mobile
ms.tgt_pltfrm: na
ms.topic: article
ms.date: 10/31/2019
ms.author: apimpm
ms.openlocfilehash: d33c5f75234ad7165a9062ecc3bb2a00d502f8c3
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74075282"
---
# <a name="provision-a-self-hosted-gateway-in-azure-api-management"></a>Azure API Yönetimi'nde kendi kendine barındırılan bir ağ geçidi sağlama

Azure API Yönetimi örneğinde ağ geçidi kaynağı sağlama, kendi kendine barındırılan bir ağ geçidi dağıtmak için bir ön koşuldur. Bu makalede, API Yönetimi'nde bir ağ geçidi kaynağı sağlama adımları üzerinden yürür.

> [!NOTE]
> Kendi kendine barındırılan ağ geçidi özelliği önizlemededir. Önizleme sırasında, kendi kendine barındırılan ağ geçidi ek ücret ödemeden yalnızca Geliştirici ve Premium katmanlarında kullanılabilir. Geliştirici katmanı, kendi kendine barındırılan tek bir ağ geçidi dağıtımıyla sınırlıdır.

## <a name="prerequisites"></a>Ön koşullar

Aşağıdaki hızlı başlatmayı tamamlayın: [Azure API Yönetimi örneği oluşturun](get-started-create-service-instance.md)

[!INCLUDE [api-management-navigate-to-instance.md](../../includes/api-management-navigate-to-instance.md)]

## <a name="provision-a-self-hosted-gateway"></a>Şirket içinde barındırılan ağ geçidini sağlama

1. **Ayarlar'ın**altından **Ağ Geçitleri'ni** seçin.
2. **+ Ekle**'ye tıklayın.
3. Ağ geçidinin **Adını** ve **Bölgesini** girin.
> [!TIP]
> **Bölge,** bu ağ geçidi kaynağıyla ilişkilendirilecek ağ geçidi düğümlerinin amaçlanan konumunu belirtir. Herhangi bir Azure kaynağıyla ilişkili benzer bir özelliğe semantik olarak eşdeğerdir, ancak rasgele bir dize değeri atanabilir.

4. İsteğe bağlı olarak, ağ geçidi kaynağının **açıklamasını** girin.
5. İsteğe bağlı **+** olarak, bir veya daha fazla API'yi bu ağ geçidi kaynağıyla ilişkilendirmek için **API'ler** altında n'leri seçin.
> [!TIP]
> API'nin **Ayarlar** sekmesindeki bir ağ geçidinden bir API'yi ilişkilendirebilir ve kaldırabilirsiniz.

> [!IMPORTANT]
> Varsayılan olarak, varolan API'lerin hiçbiri yeni ağ geçidi kaynağıyla ilişkilendirilmez. Bu nedenle, yeni ağ geçidi üzerinden `404 Resource Not Found` bunları çağırmak için girişimleri yanıtlar neden olacaktır.

6. **Ekle**’ye tıklayın.

Şimdi ağ geçidi kaynağı API Yönetimi örneğiniz de sağlanmıştır. Ağ geçidini dağıtmaya devam edebilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* Kendi kendine barındırılan ağ geçidi hakkında daha fazla bilgi edinmek için [Azure API Yönetimi'ne kendi kendine barındırılan ağ geçidine genel bakış](self-hosted-gateway-overview.md)
* [Kubernetes'e kendi barındırılan ağ geçidini](api-management-howto-deploy-self-hosted-gateway-to-k8s.md) nasıl dağıtılayaçıklayabilenler hakkında daha fazla bilgi edinin
* [Docker'a kendi kendine barındırılan bir ağ geçidini nasıl dağıtılayaçıklayabilen](api-management-howto-deploy-self-hosted-gateway-to-docker.md) ler hakkında daha fazla bilgi edinin
