---
title: Azure AD galeri uygulamasını yapılandırmada yönetici kimlik bilgilerini kaydetme sorunu
description: Azure AD Uygulama Galerisi'nde listelenen bir uygulamaya kullanıcı sağlama yapılandırması yaparken karşılaşılan sık karşılaşılan sorunları giderme
services: active-directory
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.assetid: ''
ms.service: active-directory
ms.subservice: app-provisioning
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 02/21/2018
ms.author: mimart
ms.reviewer: arvinh
ms.collection: M365-identity-device-management
ms.openlocfilehash: 3ff80224037001e41daf49cd6fc21439b2cc5cff
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77522875"
---
# <a name="problem-saving-administrator-credentials-while-configuring-user-provisioning-to-an-azure-active-directory-gallery-application"></a>Kullanıcı sağlamayı Azure Etkin Dizin Galerisi uygulamasına yapılandırırken yönetici kimlik bilgilerini kaydetme sorunu 

Kurumsal bir uygulama için [otomatik kullanıcı sağlama](user-provisioning.md) yapılandırmak için Azure portalını kullanırken, aşağıdaki durumlarda karşılaşabilirsiniz:

* Uygulama için girilen **Yönetici Kimlik Bilgileri** geçerlidir ve Test **Bağlantısı** düğmesi çalışır. Ancak, kimlik bilgileri kaydedilemez ve Azure portalı genel bir hata iletisi döndürür.

SAML tabanlı tek oturum açma da aynı uygulama için yapılandırılırsa, hatanın en olası nedeni Azure AD'nin sertifikalar ve kimlik bilgileri için dahili uygulama başına depolama sınırının aşılmasıdır.

Azure AD şu anda tüm sertifikalar, gizli belirteçler, kimlik bilgileri ve bir uygulamanın tek bir örneğiyle ilişkili ilgili yapılandırma verileri (Azure AD'de hizmet temel kaydı olarak da bilinir) için en fazla 1024 bayt depolama kapasitesine sahiptir.

SAML tabanlı tek oturum açma yapılandırıldığınızda, SAML belirteçlerini imzalamak için kullanılan sertifika burada depolanır ve genellikle alanın %50'sinden fazlasını tüketir.

Kullanıcı sağlamanın kurulumu sırasında girilen tüm gizli belirteçler, URL'ler, bildirim e-postaları, kullanıcı adları ve parolalar depolama sınırının aşılmasına neden olabilir.

## <a name="how-to-work-around-this-issue"></a>Bu sorun nasıl çözülecek? 

Bugün bu sorunu çözmenin iki olası yolu vardır:

1. **Biri tek oturum açma ve diğeri kullanıcı sağlama için olmak** üzere iki galeri uygulama örneği kullanın - Örnek olarak [LinkedIn Yükseltme](../saas-apps/linkedinelevate-tutorial.md) galeri uygulamasını alarak, Galeriden LinkedIn Yükseltme'yi ekleyebilir ve tek oturum açma için yapılandırabilirsiniz. Sağlama için Azure AD uygulama galerisinden Başka bir LinkedIn Yükseltme örneği ekleyin ve adını "LinkedIn Yükseltme (Sağlama)" olarak adlandırın. Bu ikinci örnek için, [sağlama,,](../saas-apps/linkedinelevate-provisioning-tutorial.md)ancak tek bir oturum açma yapılandırma. Bu geçici çözüm kullanırken, aynı kullanıcıların ve grupların her iki uygulamaya [da atanması](../manage-apps/assign-user-or-group-access-portal.md) gerekir. 

2. **Depolanan yapılandırma verisi miktarını azaltın** - Sağlama sekmesinin [Yönetici kimlik bilgileri](user-provisioning.md#how-do-i-set-up-automatic-provisioning-to-an-application) bölümüne girilen tüm veriler SAML sertifikasıyla aynı yerde depolanır. Tüm bu verilerin uzunluğunu azaltmak mümkün olmasa da, **Bildirim E-postası** gibi bazı isteğe bağlı yapılandırma alanları kaldırılabilir.

## <a name="next-steps"></a>Sonraki adımlar
[SaaS uygulamalarında kullanıcı sağlama ve sağlamanın yapılandırılması](user-provisioning.md)
