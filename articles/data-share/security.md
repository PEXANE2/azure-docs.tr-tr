---
title: Azure veri paylaşımıyla ilgili güvenliğe genel bakış
description: Azure veri paylaşımıyla ilgili güvenliğe genel bakış
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 06/05/2020
ms.openlocfilehash: 10f31b74b461941b15f13e45f90b5fbc408c90fe
ms.sourcegitcommit: d7008edadc9993df960817ad4c5521efa69ffa9f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86108422"
---
# <a name="security-overview-for-azure-data-share"></a>Azure veri paylaşımıyla ilgili güvenliğe genel bakış

Bu makalede, Azure veri paylaşma hizmetine yönelik bir güvenliğe genel bakış sunulmaktadır.

## <a name="security-overview"></a>Güvenliğe genel bakış

Azure veri paylaşımında, Azure 'un bekleyen ve aktarım sırasında verileri korumak için sunduğu temel güvenlik yararlanır. Veriler, temel alınan veri deposu tarafından desteklenerek Rest 'de şifrelenir. Veriler de aktarım sırasında şifrelenir. Veri paylaşımıyla ilgili meta veriler de REST ve aktarım sırasında şifrelenir. 

Erişim denetimleri, yetkilendirilmiş olanlar tarafından erişilebildiğinden emin olmak için Azure veri paylaşımında kaynak düzeyinde ayarlanabilir. 

Azure veri paylaşımı, veri paylaşımı için kullanılmakta olan veri depolarına erişmek için yönetilen kimliğin (daha önce MSI olarak bilinirdi) yararlanır. Veri sağlayıcısı ve veri tüketicisi arasında kimlik bilgileri değişimi yoktur. Yönetilen kimlik hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler](https://docs.microsoft.com/azure/active-directory/managed-identities-azure-resources/services-support-managed-identities). Veri paylaşmak için gereken roller ve izinler hakkında daha fazla bilgi için, [Roller ve gereksinimlere](concepts-roles-permissions.md)bakın.

## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.




