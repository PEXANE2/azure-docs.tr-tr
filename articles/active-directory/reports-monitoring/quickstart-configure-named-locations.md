---
title: Azure Active Directory’de adlandırılmış konumları yapılandırma | Microsoft Docs
description: Adlandırılmış konumların nasıl yapılandırılacağını öğrenin.
services: active-directory
documentationcenter: ''
author: MarkusVi
manager: daveba
ms.assetid: f56e042a-78d5-4ea3-be33-94004f2a0fc3
ms.service: active-directory
ms.workload: identity
ms.subservice: report-monitor
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: quickstart
ms.date: 11/13/2018
ms.author: markvi
ms.reviewer: dhanyahk
ms.collection: M365-identity-device-management
ms.openlocfilehash: 94f4d17596936dd9d0ebbdae3c351cac9ed2a570
ms.sourcegitcommit: 58d3b3314df4ba3cabd4d4a6016b22fa5264f05a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/02/2020
ms.locfileid: "89299873"
---
# <a name="quickstart-configure-named-locations-in-azure-active-directory"></a>Hızlı Başlangıç: Azure Active Directory’de adlandırılmış konumları yapılandırma

Adlandırılmış konumlar ile, kuruluşunuzda güvenilir IP adresi aralıklarını etiketleyebilirsiniz. Azure AD, aşağıdakileri yapmak için adlandırılmış konumları kullanır:
- [Risk Algılamalarda](../identity-protection/overview-identity-protection.md)yanlış pozitif sonuçları algıla. Güvenilir bir konumdan oturum açılması, kullanıcının oturum açma riskini azaltır.   
- [Konum tabanlı koşullu erişimi](../conditional-access/location-condition.md)yapılandırın.

Bu hızlı başlangıçta, ortamınızda adlandırılmış konumları nasıl yapılandıracağınızı öğreneceksiniz.

## <a name="prerequisites"></a>Ön koşullar

Bu hızlı başlangıcı tamamlamak için aşağıdakilere ihtiyacınız vardır:

* Azure AD kiracısı. [Ücretsiz deneme](https://azure.microsoft.com/trial/get-started-active-directory/)için kaydolun. 
* Kiracı için genel yönetici olan bir kullanıcı.
* Kuruluşunuzda belirlenen ve güvenilir olan bir IP aralığı. IP aralığının **Sınıfsız Etki Alanları Arası Yönlendirme (CIDR)** biçiminde olması gerekir.

## <a name="configure-named-locations"></a>Adlandırılmış konumları yapılandırma

1. [Azure portalında](https://portal.azure.com) oturum açın.

2. Sol bölmede **Azure Active Directory**' yi seçin ve ardından **güvenlik** bölümünden **koşullu erişim** ' i seçin.

    ![Koşullu erişim sekmesi](./media/quickstart-configure-named-locations/entrypoint.png)

3. **Koşullu Erişim** sayfasında **Adlandırılmış konumlar**’ı ve **Yeni konum**’u seçin.

    ![Adlandırılmış konum](./media/quickstart-configure-named-locations/namedlocation.png)

6. Yeni sayfada formu doldurun. 

   * **Ad** kutusuna, adlandırılmış konumunuz için bir ad girin.
   * **IP aralıkları** kutusuna CIDR biçiminde IP aralığını girin.  
   * **Oluştur**’a tıklayın.
    
     ![Yeni dikey pencere](./media/quickstart-configure-named-locations/61.png)

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla bilgi için bkz.

- [Koşullu erişimde koşul olarak konum](../conditional-access/concept-conditional-access-conditions.md#locations).
- [Riskli oturum açma işlemleri raporu](../identity-protection/overview-identity-protection.md).