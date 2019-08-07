---
title: Azure Sentinel önizlemesinde Fusion 'ı etkinleştirin | Microsoft Docs
description: Azure Sentinel 'de Fusion 'ı nasıl etkinleştireceğinizi öğrenin.
services: sentinel
documentationcenter: na
author: rkarlin
manager: rkarlin
editor: ''
ms.assetid: 82becf50-6628-47e4-b3d7-18d7d72d505f
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 2/28/2019
ms.author: rkarlin
ms.openlocfilehash: f90bb86632886d8a2ca6c6e8c60d3b79f5d0b9e9
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68780437"
---
# <a name="enable-fusion"></a>Fusion etkinleştirme

> [!IMPORTANT]
> Azure Sentinel Şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Azure Sentinel 'deki Machine Learning, baştan itibaren yerleşik olarak bulunur. Güvenlik analistlerini, güvenlik veri bilimcileri ve mühendislerini üretken hale getirmek için ML yenilikleri ile sistemi geliştirdik. Bu tür bir yenilik, özellikle de uyarı FAG 'yi azaltmak için oluşturulmuş Azure Sentinel Fusion ' dur.

Fusion, Azure AD Kimlik Koruması ve Microsoft Cloud App Security gibi farklı ürünlerden milyonlarca daha düşük uygunlukta anormal etkinlik arasında ilişki kurmak için grafik tabanlı makine öğrenimi algoritmalarını kullanır ve bunları Yönetilebilir sayıda ilginç güvenlik olayları.

## <a name="enable-fusion"></a>Fusion etkinleştirme

1. Azure portal Cloud Shell açmak için simgeyi seçin.
  ![Cloud Shell](./media/connect-fusion/cloud-shell.png)

2.  Aşağıda açılan **Cloud Shell Windows 'A hoş geldiniz** sayfasında PowerShell ' i seçin.

3.  Azure Sentinel 'i dağıttığınız aboneliği seçin ve **depolama alanını oluşturun**.

4. Kimliğiniz doğrulandıktan ve Azure sürücünüz derlendikten sonra, komut isteminde aşağıdaki komutları çalıştırın:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=true --subscription "{Subscription Guid}"

## <a name="disable-fusion"></a>Fusion 'ı devre dışı bırak

Yukarıdaki yordamın aynısını izleyin ve aşağıdaki komutu çalıştırın:

            az resource update --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --set properties.IsEnabled=false --subscription "{Subscription Guid}"

## <a name="view-the-status-of-fusion"></a>Fusion 'un durumunu görüntüleme

            az resource show --ids /subscriptions/{Subscription Guid}/resourceGroups/{Log analytics resource Group Name}/providers/Microsoft.OperationalInsights/workspaces/{Log analytics workspace Name}/providers/Microsoft.SecurityInsights/settings/Fusion --api-version 2019-01-01-preview --subscription "{Subscription Guid}"


## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Sentinel 'de Fusion 'u etkinleştirmeyi öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:
- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats.md)başlayın.

