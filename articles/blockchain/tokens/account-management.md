---
title: Azure blok zinciri belirteçleri hesap yönetimi
description: Azure blok zinciri belirteçleri hesap yönetimini kullanarak, blok zinciri eylemlerine erişimi denetlemek için gruplar oluşturabilir ve blok zinciri hesapları bağlayabilirsiniz.
ms.date: 11/04/2019
ms.topic: conceptual
ms.reviewer: brendal
ms.openlocfilehash: 9931ef59e613501ba6feaedf3ac5d4721f0df752
ms.sourcegitcommit: b77e97709663c0c9f84d95c1f0578fcfcb3b2a6c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/22/2019
ms.locfileid: "74326110"
---
# <a name="azure-blockchain-tokens-account-management"></a>Azure blok zinciri belirteçleri hesap yönetimi

[!INCLUDE [Preview note](./includes/preview.md)]

Bir blok zinciri çözümü için, kullanıcılar Azure blok zinciri belirteçleri hizmeti ile oluşturulan belirteçlere farklı düzeylerde erişim gerektirebilir. Çoğu blok zinciri senaryosunda, genel muhasebede bulunan farklı blok zinciri hesapları planlamanız ve dağıtmanız gerekir. Ayrıca katılımcılar arasındaki erişimi de yönetmeniz gerekir. Azure blok zinciri belirteçleri hesap yönetimini kullanarak, blok zinciri eylemlerine erişimi denetlemek için gruplar oluşturabilir ve blok zinciri hesapları bağlayabilirsiniz.

## <a name="blockchain-networks"></a>Blok zinciri ağları

Azure blok zinciri belirteçleri, bir blok zinciri ağları kümesi genelinde belirteçlerin dağıtımını ve yönetilmesini mümkün bir şekilde sunar. Hizmete tek bir blok zinciri defteri veya çeşitli blok zinciri bağlantısı bağlayabilirsiniz.

## <a name="accounts"></a>Hesaplar

Azure blok zinciri belirteçlerine bağlı blok zinciri ağları için hizmet, hesap özel ortak anahtar çiftlerini oluşturup yönetir ve işlem imzalama ve gönderme gerçekleştirir. Azure blok zinciri belirteçleri, hesap için ortak anahtar kimliği olan hesapları eşleştirmek üzere kimlik eşlemesi de sağlar.

## <a name="groups"></a>Gruplar

Gruplar, bağlı ağlarda çok sayıda blok zinciri hesabını yönetmenizi sağlar. Dizindeki hangi uygulamaların ve kullanıcıların Azure blok zinciri belirteçleri API 'Leri aracılığıyla hesapları kullanma yeteneğinin olduğunu izleyip denetleyebilirsiniz. Örneğin, farklı iş hatlarını veya farklı rolleri temsil eden bir hesap kümesini gruplandırabilir ve blok zinciri belirteçlerine erişin.

Ayrıca, bir grubu Azure Active Directory Kullanıcı veya hizmet sorumlusu ile ilişkilendirebilirsiniz ve bu sorumlunun grup ve ilişkili hesapları için izinleri vardır.  

## <a name="next-steps"></a>Sonraki adımlar

Kullanılabilir [Azure blok zinciri belirteçleri şablonları](templates.md)hakkında daha fazla bilgi edinin.
