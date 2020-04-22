---
title: Azure portalını kullanarak Exchange eşlemi oluşturma veya değiştirme
titleSuffix: Azure
description: Azure portalını kullanarak Exchange eşlemi oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: e4f2ee72cbe17c094567aab5c7cc4720b02cde68
ms.sourcegitcommit: acb82fc770128234f2e9222939826e3ade3a2a28
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/21/2020
ms.locfileid: "81680934"
---
# <a name="create-or-modify-an-exchange-peering-by-using-the-azure-portal"></a>Azure portalını kullanarak Exchange eşlemi oluşturma veya değiştirme

Bu makalede, Azure portalını kullanarak Microsoft Exchange eşlemesinin nasıl oluşturulacak olduğu açıklanmaktadır. Bu makalede, kaynağın durumunu denetlemek, güncelleştirmek veya silmek ve deprovision nasıl gösterir.

İsterseniz, [PowerShell](howto-exchange-powershell.md)kullanarak bu kılavuzu tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [ön koşulları](prerequisites.md) ve Exchange bakış [geçiş ini](walkthrough-exchange-all.md) gözden geçirin.
* Microsoft ile Azure kaynaklarına dönüştürülmemiş Exchange eşlemeleri zaten varsa, [portalı kullanarak azure kaynağına bakan eski bir Exchange'i dönüştür'e](howto-legacy-exchange-portal.md)bakın.

## <a name="create-and-provision-an-exchange-peering"></a>Exchange eşleme oluşturma ve sağlama

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-an-exchange-peering"></a><a name=create></a>Exchange eşleme oluşturma

**Peering** kaynağını kullanarak yeni bir eşleme isteği oluşturabilirsiniz.

#### <a name="launch-the-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırır
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [exchange-peering-configuration](./includes/exchange-portal-configuration.md)]

### <a name="verify-an-exchange-peering"></a><a name=get></a>Exchange eşlemelerini doğrulama
[!INCLUDE [peering-exchange-get-portal](./includes/exchange-portal-get.md)]

## <a name="modify-an-exchange-peering"></a><a name="modify"></a>Exchange eşleme sini değiştirme
[!INCLUDE [peering-exchange-modify-portal](./includes/exchange-portal-modify.md)]

## <a name="deprovision-an-exchange-peering"></a><a name="delete"></a>Exchange'e bakmayı deprovision
[!INCLUDE [peering-exchange-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Doğrudan bir bakış oluşturma veya değiştirme](howto-direct-portal.md)
* [Portalı kullanarak bir azure kaynağına doğrudan bakış dönüştürme](howto-legacy-direct-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için [Internet'e bakan SSS'lere](faqs.md)bakın.
