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
ms.topic: how-to
ms.date: 07/06/2020
ms.author: iainfou
ms.openlocfilehash: 993a73a42887b3869c097d14b6945a1093ac6889
ms.sourcegitcommit: 5b6acff3d1d0603904929cc529ecbcfcde90d88b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/21/2020
ms.locfileid: "88723070"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Azure Active Directory Domain Services yönetilen bir etki alanını Azure portal kullanarak silme

Artık Azure Active Directory Domain Services (Azure AD DS) yönetilen bir etki alanına ihtiyacınız yoksa silebilirsiniz. Azure AD DS yönetilen bir etki alanını kapatma veya geçici olarak devre dışı bırakma seçeneği yoktur. Yönetilen etki alanını silme, Azure AD kiracısını silmez veya olumsuz şekilde etkilemez.

Bu makalede, yönetilen bir etki alanını silmek için Azure portal nasıl kullanılacağı gösterilmektedir.

> [!WARNING]
> **Silme kalıcıdır ve geri alınamaz.**
> 
> Yönetilen bir etki alanını sildiğinizde, aşağıdaki adımlar oluşur:
>   * Yönetilen etki alanı için etki alanı denetleyicileri, sanal ağdan de sağlanmış ve kaldırılır.
>   * Yönetilen etki alanındaki veriler kalıcı olarak silinir. Bu veriler, oluşturduğunuz özel OU, GPO 'Lar, özel DNS kayıtları, hizmet sorumluları, GMSAs vb. içerir.
>   * Yönetilen etki alanına katılmış makineler, etki alanıyla olan güven ilişkilerini kaybeder ve etki alanından katılmaması gerekir.
>       * Bu makinelerde kurumsal AD kimlik bilgilerini kullanarak oturum açamazsınız. Bunun yerine, makinenin yerel yönetici kimlik bilgilerini kullanmanız gerekir.

## <a name="delete-the-managed-domain"></a>Yönetilen etki alanını sil

Yönetilen bir etki alanını silmek için aşağıdaki adımları izleyin:

1. Azure portal, araması yapın ve **Azure AD Domain Services**seçin.
1. Yönetilen etki alanınız için *aaddscontoso.com*gibi bir ad seçin.
1. **Genel Bakış** sayfasında **Sil**’i seçin. Silmeyi onaylamak için, yönetilen etki alanının etki alanı adını yeniden yazın ve **Sil**' i seçin.

Yönetilen etki alanını silmek için 15-20 dakika veya daha fazla sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS 'da görmek istediğiniz özelliklerle ilgili [geri bildirimleri paylaşmayı][feedback] göz önünde bulundurun.

Azure AD DS 'ı yeniden kullanmaya başlamak istiyorsanız, bkz. [Azure Active Directory Domain Services yönetilen etki alanı oluşturma ve yapılandırma][create-instance].

<!-- INTERNAL LINKS -->
[feedback]: https://feedback.azure.com/forums/169401-azure-active-directory?category_id=160593%3fcategory_id%3d160593
[create-instance]: tutorial-create-instance.md