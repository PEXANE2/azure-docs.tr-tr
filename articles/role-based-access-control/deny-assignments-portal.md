---
title: Azure portal ile Azure kaynakları için atamaları reddetme listesi
description: Azure portal kullanarak belirli kapsamlardaki belirli Azure Kaynak eylemlerine erişimi reddedilmiş kullanıcıları, grupları, hizmet sorumlularını ve yönetilen kimlikleri nasıl listeleyeceğinizi öğrenin.
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
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "77137433"
---
# <a name="list-deny-assignments-for-azure-resources-using-the-azure-portal"></a>Azure portal kullanarak Azure kaynakları için atamaları reddetme listesi

Bir rol ataması erişime izin verirse, [atamaları Reddet](deny-assignments.md) , kullanıcıların belirli Azure Kaynak eylemlerini gerçekleştirmesini engeller. Bu makalede, Azure portal kullanarak reddedilen atamaları listeleme açıklanmaktadır.

> [!NOTE]
> Kendi reddetme atamalarınızı doğrudan oluşturamazsınız. Atamaları reddetme hakkında daha fazla bilgi için bkz. [atamaları reddetme](deny-assignments.md).

## <a name="prerequisites"></a>Ön koşullar

Reddetme atama hakkında bilgi almak için, şunları yapmanız gerekir:

- `Microsoft.Authorization/denyAssignments/read`[Azure kaynakları için en yerleşik rollerde](built-in-roles.md)bulunan izin.

## <a name="list-deny-assignments"></a>Ret atamalarını listeleme

Abonelik veya yönetim grubu kapsamındaki atamaları Reddet ' i listelemek için bu adımları izleyin.

1. Azure portal, **tüm hizmetler** ve ardından **Yönetim grupları** veya **abonelikler**' e tıklayın.

1. Listelemek istediğiniz yönetim grubuna veya aboneliğe tıklayın.

1. **Erişim denetimi (IAM)** öğesine tıklayın.

1. **Atamaları Reddet** sekmesine tıklayın (veya atamaları kaldır kutucuğunda **görüntüle** düğmesine tıklayın).

    Bu kapsamda herhangi bir reddetme ataması varsa veya bu kapsama devralınmışsa, bunlar listelenecektir.

    ![Erişim denetimi-atamaları Reddet sekmesi](./media/deny-assignments-portal/access-control-deny-assignments.png)

1. Ek sütunları göstermek için **Sütunları Düzenle**' ye tıklayın.

    ![Atamaları Reddet-sütunlar](./media/deny-assignments-portal/deny-assignments-columns.png)

    |  |  |
    | --- | --- |
    | **Adı** | Reddetme atamasının adı. |
    | **Sorumlu türü** | Kullanıcı, Grup, sistem tanımlı grup veya hizmet sorumlusu. |
    | **Reddedildi**  | Reddetme atamasında bulunan güvenlik sorumlusunun adı. |
    | **Kimlik** | Reddetme ataması için benzersiz tanımlayıcı. |
    | **Dışlanan sorumlular** | Reddetme atamasından dışlanan güvenlik sorumluları olup olmadığı. |
    | **Alt öğeler için uygulanmıyor** | Reddetme atamasının alt kapsamlara devralınıp alınmayacağını belirtir. |
    | **Korunan sistem** | Reddetme atamasının Azure tarafından yönetilip yönetilmediği. Şu anda her zaman Evet. |
    | **Kapsam** | Yönetim grubu, abonelik, kaynak grubu veya kaynak. |

1. Etkin öğelerin herhangi birine onay işareti ekleyin ve seçili sütunları göstermek için **Tamam** ' ı tıklatın.

## <a name="list-details-about-a-deny-assignment"></a>Reddetme atama hakkındaki ayrıntıları listeleyin

Reddetme atama hakkında ek ayrıntıları listelemek için aşağıdaki adımları izleyin.

1. Önceki bölümde açıklandığı gibi **atamaları Reddet** bölmesini açın.

1. **Kullanıcılar** dikey penceresini açmak için atama adına izin verme ' ye tıklayın.

    ![Atama reddetme-kullanıcılar](./media/deny-assignments-portal/deny-assignment-users.png)

    **Kullanıcılar** dikey penceresi aşağıdaki iki bölümü içerir.

    |  |  |
    | --- | --- |
    | **Reddetme ataması için geçerlidir**  | Reddetme atamasının geçerli olduğu güvenlik sorumluları. |
    | **Atama dışladığı reddetme** | Reddetme atamasından dışlanan güvenlik sorumluları. |

    **Sistem tarafından tanımlanan sorumlu** , BIR Azure AD dizinindeki tüm kullanıcıları, grupları, hizmet sorumlularını ve yönetilen kimlikleri temsil eder.

1. Reddedilen izinlerin bir listesini görmek için, **Reddedilen izinler**' e tıklayın.

    ![Atama reddi Izinlerini Reddet](./media/deny-assignments-portal/deny-assignment-denied-permissions.png)

    | Eylem türü | Açıklama |
    | --- | --- |
    | **Eylemler**  | Yönetim işlemleri reddedildi. |
    | **NotActions** | Yönetim işlemleri Reddedilenler yönetim işleminden dışlandı. |
    | **Veri eylemleri**  | Reddedilen veri işlemleri. |
    | **NotDataActions** | Veri işlemleri, reddedilen veri işleminden dışlandı. |

    Önceki ekran görüntüsünde gösterilen örnek için aşağıdakiler geçerli izinlerdir:

    - İşlem işlemleri dışında veri düzleminde tüm depolama işlemleri reddedilir.

1. Reddetme atamasının özelliklerini görmek için **Özellikler**' e tıklayın.

    ![Atamayı reddetme-Özellikler](./media/deny-assignments-portal/deny-assignment-properties.png)

    **Özellikler** dikey penceresinde, reddetme atama adı, kimlik, açıklama ve kapsamı görebilirsiniz. **Alt öğe anahtarı için uygulanmıyor** , reddetme atamasının alt kapsamlara devralınıp alınmayacağını belirtir. **Sistem korumalı** anahtar, bu reddetme atamasının Azure tarafından yönetilip yönetilmediğini belirtir. Şu anda bu, her durumda **Evet** 'tir.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure kaynakları için reddedilen atamaları anlama](deny-assignments.md)
* [Azure PowerShell kullanarak Azure kaynakları için atamaları reddetme listesi](deny-assignments-powershell.md)
