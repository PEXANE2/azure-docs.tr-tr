---
title: Okta çoklu oturum açma verilerini Azure Sentinel 'e bağlama | Microsoft Docs
description: Okta çoklu oturum açma verilerini Azure Sentinel 'e bağlamayı öğrenin.
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
ms.openlocfilehash: 37ade037b7f3c88f5ff33d7fc4640b19f366fe7a
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86532035"
---
# <a name="connect-your-okta-single-sign-on-to-azure-sentinel-with-azure-function"></a>Azure Işleviyle okta çoklu oturum açmanızı Azure Sentinel 'e bağlama

Okta çoklu oturum açma (SSO) Bağlayıcısı, Azure Sentinel ile tüm [okta çoklu oturum açma (SSO)](https://www.okta.com/products/single-sign-on/) güvenlik çözüm günlüklerinizi kolayca bağlamanıza olanak tanır, panoları görüntüleyebilir, özel uyarılar oluşturabilir ve araştırmayı geliştirebilirsiniz. Okta çoklu oturum açma ve Azure Sentinel arasında tümleştirme, REST API kullanarak günlük verileri çekmek için Azure Işlevleri 'ni kullanır.

> [!NOTE]
> Veriler, Azure Sentinel çalıştırdığınız çalışma alanının coğrafi konumunda depolanır.

## <a name="configure-and-connect-okta-single-sign-on"></a>Okta çoklu oturum açmayı yapılandırma ve bağlama

Azure Işlevleri doğrudan okta çoklu oturum açma 'dan olayları ve günlükleri tümleştirebilir ve çekebilir ve bunları Azure Sentinel 'e iletebilir.

1. Azure Sentinel portalında, **veri bağlayıcıları** ' na tıklayın ve **okta çoklu oturum açma** Bağlayıcısı ' nı seçin.

1. **Bağlayıcı sayfasını aç**' ı seçin.

1. **Okta çoklu oturum açma** sayfasındaki yönergeleri izleyin.

## <a name="find-your-data"></a>Verilerinizi bulun

Başarılı bir bağlantı kurulduktan sonra, veriler **Okta_CL** tablosunun altında Log Analytics görüntülenir.

## <a name="validate-connectivity"></a>Bağlantıyı doğrula

Günlüklerinizin Log Analytics görünene kadar 20 dakikadan bu kadar bir zaman çıkabilir.

## <a name="next-steps"></a>Sonraki adımlar

Bu belgede, Azure Işlev uygulamalarını kullanarak okta çoklu oturum açmayı Azure Sentinel 'e bağlamayı öğrendiniz. Azure Sentinel hakkında daha fazla bilgi edinmek için aşağıdaki makalelere bakın:

- [Verilerinize nasıl görünürlük alabileceğinizi ve olası tehditleri](quickstart-get-visibility.md)öğrenin.
- [Azure Sentinel ile tehditleri algılamaya](tutorial-detect-threats-built-in.md)başlayın.
- Verilerinizi izlemek için [çalışma kitaplarını kullanın](tutorial-monitor-your-data.md) .

