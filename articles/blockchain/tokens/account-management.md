---
title: Azure Blockchain Belirteçleri hesap yönetimi
description: Azure Blockchain Belirteçleri hesap yönetimini kullanarak, blockchain eylemlerine erişimi denetlemek için gruplar oluşturabilir ve blockchain hesaplarını bağlayabilirsiniz.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: 9931ef59e613501ba6feaedf3ac5d4721f0df752
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74326110"
---
# <a name="azure-blockchain-tokens-account-management"></a>Azure Blockchain Belirteçleri hesap yönetimi

[!INCLUDE [Preview note](./includes/preview.md)]

Blockchain çözümü için kullanıcılar, Azure Blockchain Belirteçleri hizmetiyle oluşturulan belirteçlere farklı erişim düzeyleri gerektirebilir. Çoğu blockchain senaryosunda, genel muhasebede bulunan farklı blockchain hesaplarını planlamanız ve dağıtmanız gerekir. Ayrıca katılımcılar arasında erişimi yönetmeniz gerekir.Azure Blockchain Belirteçleri hesap yönetimini kullanarak, blockchain eylemlerine erişimi denetlemek için gruplar oluşturabilir ve blockchain hesaplarını bağlayabilirsiniz.

## <a name="blockchain-networks"></a>Blockchain ağları

Azure Blockchain Belirteçleri, bir dizi blockchain ağı arasında belirteçlerin dağıtılmasını ve yönetilmesini sağlar. Tek bir blockchain defterini veya birkaç blockchain defterini hizmete bağlayabilirsiniz.

## <a name="accounts"></a>Hesaplar

Azure Blockchain Belirteçleri'ne bağlı blockchain ağları için hizmet, hesabı özel-ortak anahtar çiftleri oluşturur ve yönetir ve işlem imzalama ve gönderme gerçekleştirir. Azure Blockchain Belirteçleri, hesapları genel muhasebedeki ortak anahtar kimliğiyle eşleştirmek için kimlik eşlemede de sağlar.

## <a name="groups"></a>Gruplar

Gruplar, bağlı ağlar arasında çok sayıda blockchain hesabını yönetmenize olanak tanır. Dizindeki hangi uygulamaların ve kullanıcıların Hesapları Azure Blockchain Belirteçleri API'leri aracılığıyla kullanabilme yeteneğine sahip olduğunu izleyebilir ve denetleyebilirsiniz. Örneğin, farklı iş hatlarını veya farklı rolleri ve blockchain belirteçlerine erişimi temsil eden bir hesap kümesi gruplandırmak olabilir.

Bir grubu bir Azure Etkin Dizin kullanıcısı veya hizmet yöneticisiyle de ilişkilendirebilirsiniz ve bu müdürün grup ve ilişkili hesaplarına izinleri vardır.  

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir Azure [Blockchain Belirteçleri şablonları](templates.md)hakkında daha fazla bilgi edinin.
