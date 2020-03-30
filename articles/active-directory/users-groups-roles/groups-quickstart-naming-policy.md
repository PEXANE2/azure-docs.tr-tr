---
title: Grup adlandırma ilkesi hızlı başlat - Azure Etkin Dizini | Microsoft Dokümanlar
description: Azure Active Directory'de yeni kullanıcıların eklenmesini var olan kullanıcıların silinmesini açıklar
services: active-directory
documentationcenter: ''
author: curtand
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.subservice: users-groups-roles
ms.topic: quickstart
ms.date: 11/08/2019
ms.author: curtand
ms.reviewer: krbain
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: 84e419ac5c3d292b7e630d1ebb3d3b9f59ef7b8a
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74026943"
---
# <a name="quickstart-naming-policy-for-groups-in-azure-active-directory"></a>Hızlı başlangıç: Azure Active Directory'de gruplar için adlandırma ilkesi

Bu hızlı başlangıçta kiracınızın gruplarını sıralama ve arama konusunda yardımcı olması amacıyla Azure Active Directory (Azure AD) kiracınızda kullanıcı tarafından oluşturulan Office 365 grupları için adlandırma ilkesini ayarlayacaksınız. Adlandırma ilkesini aşağıdaki gibi amaçlar için kullanabilirsiniz:

* Grubun işlevini, üyelerini, coğrafi bölgesini veya grubu oluşturan kişiyi paylaşma.
* Adres defterindeki grupların kategorilere ayrılmasına yardımcı olma.
* Belirli sözcüklerin grup adlarında ve diğer adlarında kullanılmasını engelleme.

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="configure-the-group-naming-policy-for-a-tenant-using-azure-portal"></a>Azure portalını kullanarak kiracı için grup adlandırma ilkesini yapılandırma

1. Kullanıcı yöneticisi hesabıyla [Azure AD yönetici merkezinde](https://aad.portal.azure.com) oturum açın.
1. **Gruplar'ı**seçin, ardından Adlandırma ilkesi sayfasını açmak için **Adlandırma ilkesini** seçin.

    ![yönetici merkezinde Adlandırma ilkesi sayfasını açma](./media/groups-naming-policy/policy.png)

### <a name="view-or-edit-the-prefix-suffix-naming-policy"></a>Önek-sonek adlandırma ilkesini görüntüleme veya düzeltme

1. Adlandırma **ilkesi** sayfasında **Grup adlandırma ilkesini**seçin.
1. Adlandırma ilkesinin bir parçası olarak uygulamak istediğiniz öznitelikleri veya dizeleri seçerek geçerli önek veya sonek adlandırma ilkelerini tek tek görüntüleyebilir veya düzenleme yapabilirsiniz.
1. Bir önek veya sonek listeden kaldırmak için önek veya sonek seçin ve sonra **Sil'i**seçin. Aynı anda birden çok öğe silinebilir.
1. Yürürlüğe girecek ilke değişiklikleriniz için **Kaydet'i** seçin.

### <a name="view-or-edit-the-custom-blocked-words"></a>Özel engellenen sözcükleri görüntüleme veya görüntüleme

1. Adlandırma **ilkesi** sayfasında **Engellenen sözcükleri**seçin.

    ![adlandırma ilkesi için engellenen sözcükler listesini edin ve yükleyin](./media/groups-naming-policy/blockedwords.png)

1. **İndir'i**seçerek özel engellenen sözcüklerin geçerli listesini görüntüleyin veya düzenleme.
1. Dosya simgesini seçerek özel engellenen sözcüklerin yeni listesini yükleyin.
1. Yürürlüğe girecek ilke değişiklikleriniz için **Kaydet'i** seçin.

İşte bu kadar. Adlandırma ilkenizi ayarladınız ve özel engellenen sözcüklerinizi eklediniz.

## <a name="clean-up-resources"></a>Kaynakları temizleme

### <a name="remove-the-naming-policy-using-azure-portal"></a>Azure portalını kullanarak adlandırma ilkesini kaldırma

1. Adlandırma **ilkesi** sayfasında, **silme ilkesini**seçin.
1. Silme işlemini onayladıktan sonra, tüm öneek-sonek adlandırma ilkesi ve özel engellenen sözcükler de dahil olmak üzere adlandırma ilkesi kaldırılır.

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, Azure REKLAM kuruluşunuz için adlandırma ilkesini Azure portalı üzerinden nasıl ayarlayabileceğinizi öğrendiniz.

İlke adlandırma, teknik kısıtlamalar, özel engellenmiş sözcüklerin bir listesini ekleme ve Office 365 uygulamaları arasında son kullanıcı deneyimleri dahil olmak üzere daha fazla bilgi için bir sonraki makaleye ilerleyin.
> [!div class="nextstepaction"]
> [Adlandırma ilkesi PowerShell](groups-naming-policy.md)