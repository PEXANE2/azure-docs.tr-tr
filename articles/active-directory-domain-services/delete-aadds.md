---
title: Azure Active Directory Domain Services Sil | Microsoft Docs
description: Azure portal kullanarak Azure Active Directory Domain Services yönetilen bir etki alanını devre dışı bırakmayı veya silmeyi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 11/26/2019
ms.author: iainfou
ms.openlocfilehash: ee4c41ca3179eead0e4fd470a02a5cdfa4a6d43a
ms.sourcegitcommit: c69c8c5c783db26c19e885f10b94d77ad625d8b4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74705248"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Azure Active Directory Domain Services yönetilen bir etki alanını Azure portal kullanarak silme

Artık yönetilen bir etki alanına ihtiyacınız yoksa bir Azure Active Directory Domain Services (Azure AD DS) örneğini silebilirsiniz. Azure AD DS yönetilen bir etki alanını kapatma veya geçici olarak devre dışı bırakma seçeneği yoktur. Azure AD DS yönetilen etki alanını silme, Azure AD kiracısı silmez veya olumsuz yönde etkilemez. Bu makalede, Azure AD DS yönetilen bir etki alanını silmek için Azure portal nasıl kullanılacağı gösterilmektedir.

> [!WARNING]
> **Silme kalıcıdır ve geri alınamaz.**
> Azure AD DS yönetilen bir etki alanını sildiğinizde, aşağıdaki adımlar oluşur:
>   * Yönetilen etki alanı için etki alanı denetleyicileri, sanal ağdan de sağlanmış ve kaldırılır.
>   * Yönetilen etki alanındaki veriler kalıcı olarak silinir. Bu veriler, oluşturduğunuz özel OU, GPO 'Lar, özel DNS kayıtları, hizmet sorumluları, GMSAs vb. içerir.
>   * Yönetilen etki alanına katılmış makineler, etki alanıyla olan güven ilişkilerini kaybeder ve etki alanından katılmaması gerekir.
>       * Bu makinelerde kurumsal AD kimlik bilgilerini kullanarak oturum açamazsınız. Bunun yerine, makinenin yerel yönetici kimlik bilgilerini kullanmanız gerekir.

## <a name="delete-the-managed-domain"></a>Yönetilen etki alanını sil

Azure AD DS yönetilen bir etki alanını silmek için aşağıdaki adımları izleyin:

1. Azure portal, araması yapın ve **Azure AD Domain Services**seçin.
1. Azure AD DS yönetilen etki alanının adını (örneğin, *aadds.contoso.com*) seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin. Silmeyi onaylamak için, yönetilen etki alanının etki alanı adını yeniden yazın ve **Sil**' i seçin.

Azure AD DS yönetilen etki alanını silmek için 15-20 dakika veya daha fazla sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS 'da görmek istediğiniz özelliklerle ilgili [geri bildirimleri paylaşmayı][feedback] göz önünde bulundurun.

Azure AD DS 'ı yeniden kullanmaya başlamak istiyorsanız, bkz. [Azure Active Directory Domain Services örneği oluşturma ve yapılandırma][create-instance].

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md
