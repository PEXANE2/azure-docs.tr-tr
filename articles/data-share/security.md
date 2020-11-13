---
title: Azure Veri Paylaşımı için güvenliğe genel bakış
description: Azure Veri Paylaşımı için güvenliğe genel bakış
author: jifems
ms.author: jife
ms.service: data-share
ms.topic: how-to
ms.date: 10/30/2020
ms.openlocfilehash: 1fdf026e9271ef6eb30c2b4ca96a04880b65be75
ms.sourcegitcommit: 1d6ec4b6f60b7d9759269ce55b00c5ac5fb57d32
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/13/2020
ms.locfileid: "94578102"
---
# <a name="security-overview-for-azure-data-share"></a>Azure Veri Paylaşımı için güvenliğe genel bakış

Bu makalede, Azure veri paylaşma hizmetine yönelik bir güvenliğe genel bakış sunulmaktadır.

## <a name="security-overview"></a>Güvenliğe genel bakış

Azure veri paylaşımında, Azure 'un bekleyen ve aktarım sırasında verileri korumak için sunduğu temel güvenlik yararlanır. Veriler, temel alınan veri deposu tarafından desteklenerek Rest 'de şifrelenir. Ayrıca, veriler TLS 1,2 kullanılarak iletimde şifrelenir. Veri paylaşımıyla ilgili meta veriler de REST ve aktarım sırasında şifrelenir. Azure veri paylaşım, paylaşılmakta olan müşteri verilerinin içeriğini depolamaz.

Azure veri paylaşımı, veri paylaşımı için kullanılmakta olan veri depolarına erişmek için yönetilen kimliğin (daha önce MSI olarak bilinirdi) yararlanır. Veri sağlayıcısı ve veri tüketicisi arasında kimlik bilgileri değişimi yoktur. Yönetilen kimlik hakkında daha fazla bilgi için bkz. [Azure kaynakları Için Yönetilen kimlikler](../active-directory/managed-identities-azure-resources/services-support-managed-identities.md). Veri paylaşmak için gereken roller ve izinler hakkında daha fazla bilgi için, [Roller ve gereksinimlere](concepts-roles-permissions.md)bakın.

Azure veri paylaşımında erişim denetimleri, yetkilendirilmiş olanlar tarafından erişilebildiğinden emin olmak için veri paylaşımının kaynak düzeyinde ayarlanabilir. 

## <a name="share-data-from-or-to-data-stores-with-firewall-enabled"></a>Güvenlik Duvarı etkinken veri depolarından veya veri depolarında veri paylaşma
Güvenlik Duvarı açıkken veya depolama hesaplarına veri paylaşmak için depolama hesabınızda **Güvenilen Microsoft hizmetlerine Izin ver** ' i etkinleştirmeniz gerekir. Ayrıntılar için bkz. [Azure Storage güvenlik duvarlarını ve sanal ağları yapılandırma](
https://docs.microsoft.com/azure/storage/common/storage-network-security#trusted-microsoft-services) .


## <a name="next-steps"></a>Sonraki adımlar

Verileri paylaşmaya nasıl başlayacağınızı öğrenmek için [verilerinizi paylaşma](share-your-data.md) öğreticiye geçin.
