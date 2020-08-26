---
title: Azure aboneliği durumları
description: Bu makalede Azure aboneliğinin farklı durumları açıklanır.
keywords: azure aboneliği durumu
author: anuragdalmia
ms.reviewer: banders
tags: billing
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: conceptual
ms.date: 08/20/2020
ms.author: andalmia
ms.openlocfilehash: 70d03b4aeeaecee522552b381ce1309485719dc6
ms.sourcegitcommit: 56cbd6d97cb52e61ceb6d3894abe1977713354d9
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/20/2020
ms.locfileid: "88686692"
---
# <a name="azure-subscription-states"></a>Azure aboneliği durumları

Bu makalede Azure aboneliğinin çeşitli olası durumları açıklanır. Bu durumların Azure portalı abonelik alanlarında **Durum** olarak gösterildiğini görürsünüz.

| Abonelik Durumu | Açıklama |
|-------------| ----------------|
| **Etkin**/**Etkinleştirildi** | Azure aboneliğiniz etkindir. Aboneliği kullanarak yeni kaynaklar dağıtabilir ve mevcut kaynakları yönetebilirsiniz.<br><br>[Aboneliğinize kaydedilen](../../azure-resource-manager/management/resource-providers-and-types.md#azure-portal) kaynak sağlayıcıları için tüm işlemler (PUT, PATCH, DELETE, POST, GET) kullanılabilir. |
| **Silindi** | Azure aboneliğiniz tüm temel kaynakları/verileriyle birlikte silindi.<br><br>Kullanılabilir işlem yok. |
| **Devre dışı** | Azure aboneliğiniz devre dışı bırakıldı ve artık Azure kaynaklarını oluşturmak veya yönetmek için kullanılamaz. Bu durumdayken sanal makineleriniz serbest bırakılır, geçici IP adresleri serbest bırakılır, depolama salt okunur olur ve diğer hizmetler devre dışı bırakılır. Abonelik aşağıdaki nedenlerden dolayı devre dışı bırakılabilir: Krediniz dolmuş olabilir. Harcama limitinize ulaşmış olabilirsiniz. Vadesi geçmiş bir faturanız olabilir. Kredi kartı limitiniz aşılmış olabilir. Belki de açıkça devre dışı bırakılmış veya iptal edilmiştir. Abonelik türüne bağlı olarak bir abonelik 1 - 90 gün arası devre dışı bırakılmış durumda kalabilir. Bu sürenin sonunda kalıcı olarak silinir. Daha fazla bilgi için bkz. [Devre dışı bırakılmış Azure aboneliğini yeniden etkinleştirme](subscription-disabled.md).<br><br>Kaynakları oluşturma veya güncelleştirme işlemleri (PUT, PATCH) devre dışı bırakıldı. Bir eylem alan işlemler (POST) de devre dışı bırakıldı. Kaynakları alabilir ve silebilirsiniz (GET, DELETE). Kaynaklarınız hala kullanılabilir. |
| **Süresi Doldu** | Azure aboneliğiniz iptal edildiği için aboneliğinizin süresi dolmuştur. Süresi dolmuş bir aboneliği yeniden etkinleştirebilirsiniz. Daha fazla bilgi için bkz. [Devre dışı bırakılmış Azure aboneliğini yeniden etkinleştirme](subscription-disabled.md).<br><br>Kaynakları oluşturma veya güncelleştirme işlemleri (PUT, PATCH) devre dışı bırakıldı. Bir eylem alan işlemler (POST) de devre dışı bırakıldı. Kaynakları alabilir ve silebilirsiniz (GET, DELETE).|
| **Süresi Geçti** | Azure aboneliğinizin bekleyen ödemesi vardır. Aboneliğiniz hala etkindir ama ücretlerin ödenememesi aboneliğin devre dışı bırakılmasına yol açabilir. Daha fazla bilgi için bkz. [Azure aboneliğinizde vadesi geçmiş bakiye sorununu çözme](resolve-past-due-balance.md).<br><br>Tüm işlemler kullanılabilir. |
| **Uyarı** | Azure aboneliğiniz uyarı durumunda. Normal olarak kullanılabilse de, uyarının kaynaklandığı soruna çözüm getirilememesi durumunda kısa süre içinde devre dışı bırakılacak. Abonelik vadesinde ödenmediyse, kullanıcı tarafından iptal edildiyse, aboneliğin süresi dolduysa veya benzeri bir durum oluştuysa abonelik uyarı durumunda olabilir.<br><br>Tüm işlemler kullanılabilir. |
