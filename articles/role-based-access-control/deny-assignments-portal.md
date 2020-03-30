---
title: Azure portalı ile Azure kaynakları için atamaları reddetme listesi
description: Azure portalını kullanarak belirli kapsamlarda belirli Azure kaynak eylemlerine erişimi engellenen kullanıcıları, grupları, hizmet ilkelerini ve yönetilen kimlikleri nasıl listeleyiş süreceğinizi öğrenin.
services: active-directory
documentationcenter: ''
author: rolyon
manager: mtillman
ms.assetid: 8078f366-a2c4-4fbb-a44b-fc39fd89df81
ms.service: role-based-access-control
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: identity
ms.date: 06/10/2019
ms.author: rolyon
ms.reviewer: bagovind
ms.openlocfilehash: 4db76e5c6191457346ca1f95678cf73843334d3b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77137433"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-azure-portal"></a>Azure portalını kullanarak Azure kaynakları için atamaları reddetme listesi

[Atamaları reddetme,](deny-assignments.md) bir rol ataması onlara erişim izni verse bile kullanıcıların belirli Azure kaynak eylemlerini gerçekleştirmesini engeller. Bu makalede, Azure portalını kullanarak reddedilen atamaların nasıl listelenebildiğini açıklanmaktadır.

> [!NOTE]
> Doğrudan kendi reddi atamaları oluşturamaz. Reddet atamalarının nasıl oluşturulduğu hakkında bilgi için [bkz.](deny-assignments.md)

## <a name="prerequisites"></a>Ön koşullar

Reddet ataması hakkında bilgi almak için şunları yapmış olmalısınız:

- `Microsoft.Authorization/denyAssignments/read`Azure kaynakları için en [yerleşik rollere](built-in-roles.md)dahil olan izin.

## <a name="list-deny-assignments"></a>Ret atamalarını listeleme

Abonelik veya yönetim grubu kapsamındaki atamaları reddetmeyi listelemek için aşağıdaki adımları izleyin.

1. Azure portalında, **Tüm hizmetler** ve ardından **Yönetim grupları** veya **Abonelikler'i**tıklatın.

1. Listelemek istediğiniz yönetim grubunu veya aboneliği tıklatın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. Atamaları **Reddet** sekmesini tıklatın (veya atamaları reddet döşemesini görüntüle'deki **Görünüm** düğmesini tıklatın).

    Bu kapsamda herhangi bir reddedilme ataması varsa veya bu kapsamda devralınmışsa, bunlar listelenir.

    ![Erişim denetimi - Atamaları reddet sekmesi](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Ek sütunları görüntülemek için **Sütunları Edit'i**tıklatın.

    ![Atamaları reddet - Sütunlar](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Adı** | Reddet atamanın adı. |
    | **Sorumlu türü** | Kullanıcı, grup, sistem tanımlı grup veya hizmet ilkesi. |
    | **Reddedildi**  | Reddet atamasında yer alan güvenlik sorumlusunun adı. |
    | **Kimlik** | Reddet ataması için benzersiz tanımlayıcı. |
    | **Hariç ilkeler** | Reddedilen atamanın dışında olan güvenlik ilkelerinin olup olmadığı. |
    | **Çocuklar için geçerli değildir** | Reddet atamasının alt kapsamlara devredilip devralmadığı. |
    | **Sistem korumalı** | Reddet atamasının Azure tarafından yönetilip yönetilmediği. Şu anda, her zaman Evet. |
    | **Kapsam** | Yönetim grubu, abonelik, kaynak grubu veya kaynak. |

1. Etkin leştirilmiş öğelerden herhangi biri için bir onay işareti ekleyin ve ardından seçili sütunları görüntülemek için **Tamam'ı** tıklatın.

## <a name="list-details-about-a-deny-assignment"></a>Reddet atamayla ilgili ayrıntıları listele

Reddet atamayla ilgili ek ayrıntıları listelemek için aşağıdaki adımları izleyin.

1. Önceki bölümde açıklandığı gibi **Reddet atamaları** bölmesini açın.

1. **Kullanıcılar** bıçağını açmak için atama adını reddet'i tıklatın.

    ![Atamayı reddet - Kullanıcılar](./media/deny-assignments-portal/deny-assignment-users.png)

    **Kullanıcılar** bıçağı aşağıdaki iki bölümü içerir.

    |  |  |
    | --- | --- |
    | **Reddet ataması geçerlidir**  | Reddet atamasının geçerli olduğu güvenlik ilkeleri. |
    | **Atamayı reddet hariç** | Reddedilen atamanın dışında olan güvenlik ilkeleri. |

    **Sistem Tanımlı Asıl,** Azure AD dizinindeki tüm kullanıcıları, grupları, hizmet ilkelerini ve yönetilen kimlikleri temsil eder.

1. Reddedilen izinlerin listesini görmek için **Reddedilen İzinler'i**tıklatın.

    ![Atamayı reddet - Reddedilen İzinler](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Eylem türü | Açıklama |
    | --- | --- |
    | **Eylemler**  | Yönetim işlemleri reddedildi. |
    | **NotActions** | Reddedilen yönetim işlemleri hariç tutuldu. |
    | **DataActions**  | Veri işlemleri reddedildi. |
    | **NotDataActions** | Reddedilen veri işlemi dışında veri işlemleri. |

    Önceki ekran görüntüsünde gösterilen örnekiçin, etkili izinler şunlardır:

    - Veri düzlemindeki tüm depolama işlemleri bilgi işlem işlemleri dışında reddedilir.

1. Reddet atamanın özelliklerini görmek için **Özellikler'i**tıklatın.

    ![Atamayı reddet - Özellikler](./media/deny-assignments-portal/deny-assignment-properties.png)

    **Özellikler** bıçağında, reddet atama adını, kimliğini, açıklamasını ve kapsamını görebilirsiniz. Alt anahtar **için geçerli değildir,** reddet atamanın alt kapsamlara devredilip devralınmadığını gösterir. **Sistem korumalı** anahtar, bu reddet atamasının Azure tarafından yönetilip yönetilmediğini gösterir. Şu anda, bu her durumda **Evet'** tir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynakları için atamaları reddetmeyi anlama](deny-assignments.md)
* [Azure PowerShell kullanarak Azure kaynakları için atamaları reddetme listesi](deny-assignments-powershell.md)
