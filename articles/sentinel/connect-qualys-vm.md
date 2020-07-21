---
title: Qualys güvenlik açığı yönetim verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Qualys güvenlik açığı yönetimi verilerini Azure Sentinel 'e bağlamayı öğrenin.
services: sentinel
documentationcenter: na
author: yelevin
manager: rkarlin
editor: ''
ms.service: azure-sentinel
ms.subservice: azure-sentinel
ms.devlang: na
ms.topic: how-to
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 07/17/2020
ms.author: yelevin
ms.openlocfilehash: 44002a8f4ab3b644e3530ee2d2fc06a7af271fbe
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86531965"
---
# <a name="connect-your-qualys-vm-to-azure-sentinel-with-azure-function"></a>Qualys VM 'nizi Azure Işlevi ile Azure Sentinel 'e bağlama

Qualys güvenlik açığı yönetimi (VM) Bağlayıcısı sayesinde tüm [Qualys VM](https://www.qualys.com/apps/vulnerability-management/) güvenlik çözüm günlüklerinizi Azure Sentinel ile kolayca bağlamanıza, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Qualys VM ve Azure Sentinel arasındaki tümleştirme, REST API kullanarak günlük verileri çekmek için Azure Işlevleri kullanımını sağlar.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-qualys-vm"></a>Qualys VM 'yi yapılandırma ve bağlama

Azure Işlevleri, olayları ve günlükleri doğrudan Qualys VM 'den tümleştirebilir ve çekebilir ve bunları Azure Sentinel 'e iletebilir.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **Qualys güvenlik açığı yönetim** Bağlayıcısı ' nı seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. **Qualys güvenlik açığı yönetimi** sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **QualysHostDetection_CL** tablosunun altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Işlev uygulamalarını kullanarak Qualys VM 'yi Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .
