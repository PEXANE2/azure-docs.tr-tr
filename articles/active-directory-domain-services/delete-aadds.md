---
title: Azure Etkin Dizin Etki Alanı Hizmetlerini Silme | Microsoft Dokümanlar
description: Azure portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen etki alanını nasıl devre dışı atabileceğinizi veya silmeyi öğrenin
services: active-directory-ds
author: iainfoulds
manager: daveba
ms.assetid: 89e407e1-e1e0-49d1-8b89-de11484eee46
ms.service: active-directory
ms.subservice: domain-services
ms.workload: identity
ms.topic: conceptual
ms.date: 03/30/2020
ms.author: iainfou
ms.openlocfilehash: e6fcb53995fd255386e5b291937d6d50c850c724
ms.sourcegitcommit: efefce53f1b75e5d90e27d3fd3719e146983a780
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/01/2020
ms.locfileid: "80476302"
---
# <a name="delete-an-azure-active-directory-domain-services-managed-domain-using-the-azure-portal"></a>Azure portalını kullanarak Azure Etkin Dizin Etki Alanı Hizmetleri yönetilen etki alanını silme

Artık yönetilen bir etki alanına ihtiyacınız yoksa, bir Azure Etkin Dizin Etki Alanı Hizmetleri (Azure AD DS) örneğini silebilirsiniz. Azure AD DS yönetilen bir etki alanını kapatma veya geçici olarak devre dışı etme seçeneği yoktur. Azure AD DS yönetilen etki alanını silmek, Azure AD kiracısını silmez veya olumsuz etkilemez. Bu makalede, Azure AD DS yönetilen etki alanını silmek için Azure portalını nasıl kullanacağınızı gösterilmektedir.

> [!WARNING]
> **Silme kalıcıdır ve geri alınamaz.**
> Azure AD DS yönetilen bir etki alanını sildiğinizde aşağıdaki adımlar oluşur:
>   * Yönetilen etki alanı için etki alanı denetleyicileri, sağlanan ve sanal ağdan kaldırılır.
>   * Yönetilen etki alanındaki veriler kalıcı olarak silinir. Bu veriler, oluşturduğunuz özel İş'leri, GPO'ları, özel DNS kayıtlarını, hizmet ilkelerini, GDO'ları vb. içerir.
>   * Yönetilen etki alanına katılan makineler etki alanıyla olan güven ilişkilerini kaybeder ve etki alanından katılmaması gerekir.
>       * Kurumsal AD kimlik bilgilerini kullanarak bu makinelerde oturum açamazsınız. Bunun yerine, makine için yerel yönetici kimlik bilgilerini kullanmanız gerekir.

## <a name="delete-the-managed-domain"></a>Yönetilen etki alanını silme

Azure AD DS yönetilen etki alanını silmek için aşağıdaki adımları tamamlayın:

1. Azure **portalında, Azure AD Etki Alanı Hizmetlerini**arayın ve seçin.
1. azure AD DS yönetilen etki alanınızın adını seçin( *örneğin aaddscontoso.com.*
1. **Genel Bakış** sayfasında **Sil**’i seçin. Silme işlemini onaylamak için yönetilen etki alanının etki alanı adını yeniden yazın ve sonra **Sil'i**seçin.

Azure AD DS yönetilen etki alanını silmek 15-20 dakika veya daha uzun sürebilir.

## <a name="next-steps"></a>Sonraki adımlar

Azure AD DS'de görmek istediğiniz özellikler için [geri bildirim paylaşmayı][feedback] düşünün.

Azure AD DS'yi yeniden kurmak istiyorsanız, [Azure Etkin Dizin Etki Alanı Hizmetleri örneği oluştur ve yapılandırma][create-instance]kınızda.

<!-- INTERNAL LINKS -->
[feedback]: contact-us.md
[create-instance]: tutorial-create-instance.md
