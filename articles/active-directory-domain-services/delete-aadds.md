---
title: Azure Active Directory Domain Services devre dışı bırak | Microsoft Docs
description: Azure portal kullanarak Azure Active Directory Domain Services devre dışı bırak
services: active-directory-ds
documentationcenter: ''
author: iainfoulds
manager: daveba
editor: curtand
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/14/2019
ms.author: iainfou
ms.openlocfilehash: d5917ad94212c8b18d4362528bdfbafb02aec808
ms.sourcegitcommit: ee61ec9b09c8c87e7dfc72ef47175d934e6019cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/30/2019
ms.locfileid: "70171963"
---
# <a name="disable-azure-active-directory-domain-services-using-the-azure-portal"></a>Azure portal kullanarak Azure Active Directory Domain Services devre dışı bırak
Bu makalede, Azure AD dizininiz için Azure Active Directory (AD) etki alanı hizmetlerini devre dışı bırakmak üzere Azure portal nasıl kullanılacağı gösterilmektedir.

> [!WARNING]
> **Silme kalıcıdır ve geri alınamaz.**
> Dikkatli ilerleyin! Yönetilen etki alanını sildiğinizde:
>   * Yönetilen etki alanı için etki alanı denetleyicileri, sanal ağdan de sağlanmış ve kaldırılır.
>   * Yönetilen etki alanındaki veriler kalıcı olarak silinir. Bu, yönetilen etki alanında oluşturduğunuz özel OU, GPO 'Lar, özel DNS kayıtları, hizmet sorumluları, GMSAs vb. içerir.
>   * Yönetilen etki alanına katılmış makineler, etki alanıyla olan güven ilişkilerini kaybeder ve etki alanından katılmaması gerekir.
>   * Bu makinelerde kurumsal AD kimlik bilgilerini kullanarak oturum açılamıyor. Bunun yerine makinenin yerel yönetici kimlik bilgilerini kullanın.
> Yönetilen etki alanını silmek, Azure AD dizininizi silmez, aksi takdirde dizini olumsuz yönde etkiler.

Azure AD Domain Services yönetilen etki alanınızı silmek için aşağıdaki adımları gerçekleştirin:
1. Azure portal [Azure AD Domain Services uzantısına](https://portal.azure.com/#blade/HubsExtension/Resources/resourceType/Microsoft.AAD%2FdomainServices) gidin.
2. Yönetilen etki alanının adına tıklayın.

    ![Silinecek etki alanını seçin](./media/getting-started/domain-services-delete-select-domain.png)

3. **Genel bakış** sayfasında **Sil** düğmesine tıklayın.

    ![Etki alanını sil](./media/getting-started/domain-services-delete-domain.png)

4. Silmeyi onaylamak için, yönetilen etki alanının DNS etki alanı adını yazın. İşiniz bittiğinde **Sil** düğmesine tıklayın.

    ![Etki alanı onayını Sil](./media/getting-started/domain-services-delete-domain-confirm.png)

Yönetilen etki alanını silmek için 15-20 dakika veya daha fazla sürebilir.

Gelecekte Azure AD Domain Services seçtiğiniz özellikleri anlamanıza yardımcı olması için [geri bildirimi paylaşmayı](contact-us.md) düşünün. Bu geri bildirim, hizmeti dağıtım gereksinimlerinize ve kullanım örneklerine daha iyi uyacak şekilde gelişmemize yardımcı olur.
