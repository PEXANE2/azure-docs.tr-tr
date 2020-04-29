---
title: Azure portal kullanarak doğrudan eşleme oluşturma veya değiştirme
titleSuffix: Azure
description: Azure portal kullanarak doğrudan eşleme oluşturma veya değiştirme
services: internet-peering
author: prmitiki
ms.service: internet-peering
ms.topic: article
ms.date: 11/27/2019
ms.author: prmitiki
ms.openlocfilehash: dcd6aaf584691005dd071a7aba5958070f598978
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "81681040"
---
# <a name="create-or-modify-a-direct-peering-by-using-the-azure-portal"></a>Azure portal kullanarak doğrudan eşleme oluşturma veya değiştirme

Bu makalede, Azure portal kullanarak bir Microsoft doğrudan eşlemesinin nasıl oluşturulacağı açıklanır. Bu makalede ayrıca kaynağın durumunu denetleme, güncelleştirme veya silme ve sağlamayı kaldırma işlemlerinin nasıl yapılacağı gösterilir.

İsterseniz, bu kılavuzu Azure [PowerShell](howto-direct-powershell.md)kullanarak tamamlayabilirsiniz.

## <a name="before-you-begin"></a>Başlamadan önce
* Yapılandırmaya başlamadan önce [önkoşulları](prerequisites.md) ve [doğrudan eşleme](walkthrough-direct-all.md) kılavuzunu gözden geçirin.
* Azure kaynaklarına Dönüştürülmeyen Microsoft ile doğrudan eşleme bağlantılarınız varsa, bkz. [portalı kullanarak eski bir doğrudan eşlemeyi Azure kaynağına dönüştürme](howto-legacy-direct-portal.md).

## <a name="create-and-provision-a-direct-peering"></a>Doğrudan eşleme oluşturma ve sağlama

### <a name="sign-in-to-the-portal-and-select-your-subscription"></a>Portalda oturum açın ve aboneliğinizi seçin
[!INCLUDE [Account](./includes/account-portal.md)]

### <a name="create-a-direct-peering"></a><a name=create></a>Doğrudan eşleme oluşturma

**Eşleme** kaynağını kullanarak yeni bir eşleme isteği oluşturabilirsiniz.

#### <a name="launch-resource-and-configure-basic-settings"></a>Kaynağı başlatın ve temel ayarları yapılandırın
[!INCLUDE [direct-peering-basic](./includes/direct-portal-basic.md)]

#### <a name="configure-connections-and-submit"></a>Bağlantıları yapılandırma ve gönderme
[!INCLUDE [direct-peering-configuration](./includes/direct-portal-configuration.md)]

### <a name="verify-direct-peering"></a><a name=get></a>Doğrudan eşlemeyi doğrula
[!INCLUDE [peering-direct-get-portal](./includes/direct-portal-get.md)]

## <a name="modify-a-direct-peering"></a><a name="modify"></a>Doğrudan eşlemeyi değiştirme
[!INCLUDE [peering-direct-modify-portal](./includes/direct-portal-modify.md)]

## <a name="deprovision-a-direct-peering"></a><a name="delete"></a>Doğrudan eşleme sağlamayı kaldırma
[!INCLUDE [peering-direct-delete-portal](./includes/delete.md)]

## <a name="next-steps"></a>Sonraki adımlar

* [Portalı kullanarak Exchange eşlemesi oluşturma veya değiştirme](howto-exchange-portal.md)
* [Portalı kullanarak eski bir Exchange eşlemesini Azure kaynağına dönüştürme](howto-legacy-exchange-portal.md)

## <a name="additional-resources"></a>Ek kaynaklar

Daha fazla bilgi için bkz. [Internet eşlemesi SSS](faqs.md).
