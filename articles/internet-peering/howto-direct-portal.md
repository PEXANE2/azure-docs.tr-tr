---
title: Azure portalını kullanarak Doğrudan bir bakış oluşturma veya değiştirme
titleSuffix: Azure
description: Azure portalını kullanarak Doğrudan bir bakış oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: dcd6aaf584691005dd071a7aba5958070f598978
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81681040"
---
# <a name="create-or-modify-a-direct-peering-by-using-the-azure-portal"></a>Azure portalını kullanarak Doğrudan bir bakış oluşturma veya değiştirme

Bu makalede, Azure portalını kullanarak Microsoft Direct'in nasıl bir bakış oluşturmak için oluşturulacak ları açıklanmaktadır. Bu makalede, kaynağın durumunu denetlemek, güncelleştirmek veya silmek ve deprovision nasıl gösterir.

İsterseniz, Azure [PowerShell'i](howto-direct-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Doğrudan bakan [gözden geçirin.](walkthrough-direct-all.md)
* Microsoft ile Azure kaynaklarına dönüştürülmemiş doğrudan bakış bağlantılarınız varsa, [portalı kullanarak bir azure kaynağına doğrudan bakan bir eskiye dönüştür'e](howto-legacy-direct-portal.md)bakın.

## <a name="create-and-provision-a-direct-peering"></a>Doğrudan bir bakış oluşturma ve sağlama

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Doğrudan eşleme oluşturma

**Peering** kaynağını kullanarak yeni bir eşleme isteği oluşturabilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan bakmayı doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Doğrudan eşleme değiştirme
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Doğrudan bir bakış hükmünü deprovision
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak azure kaynağına bakan eski bir Exchange'i dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.
