---
title: Azure VMware çözümü geçiş kutusunu oluşturma
description: Azure VMware çözümü geçiş kutusunu oluşturma adımları.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: c46bd61d9003f157877a1fd0df9adb9e8143a63f
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91298516"
---
<!-- Used in avs-deployment.md and tutorial-access-private-cloud.md -->

1. Kaynak grubunda **+ Ekle** ' yi seçin ve **Microsoft Windows 10**' u seçin ve ardından **Oluştur**' u seçin.

   :::image type="content" source="../media/tutorial-access-private-cloud/ss8-azure-w10vm-create.png" alt-text="Bir sıçrama kutusu için yeni bir Windows 10 VM ekleyin." border="true":::

1. Alanlara gerekli bilgileri girin ve ardından **gözden geçir + oluştur**' u seçin. 

   Alanlarla ilgili daha fazla bilgi için aşağıdaki tabloya bakın.

   | Alan | Değer |
   | --- | --- |
   | **Abonelik** | Değer, kaynak grubuna ait abonelikle önceden doldurulur. |
   | **Kaynak grubu** | Değer, önceki öğreticide oluşturduğunuz geçerli kaynak grubu için önceden doldurulur.  |
   | **Sanal makine adı** | VM için benzersiz bir ad girin. |
   | **Bölge** | VM 'nin coğrafi konumunu seçin. |
   | **Kullanılabilirlik seçenekleri** | Varsayılan değeri seçili bırakın. |
   | **Görüntü** | VM görüntüsünü seçin. |
   | **Boyut** | Varsayılan boyut değerini bırakın. |
   | **Kimlik doğrulaması türü**  | **Parola**seçin. |
   | **Kullanıcı adı** | VM 'de oturum açmak için Kullanıcı adını girin. |
   | **Parola** | VM 'de oturum açmak için parolayı girin. |
   | **Parolayı onayla** | VM 'de oturum açmak için parolayı girin. |
   | **Genel gelen bağlantı noktaları** | **Hiçbiri** seçeneğini belirtin. Hiçbiri ' ni seçerseniz, sanal makine erişimini denetlemek için [JIT erişimini](../../security-center/security-center-just-in-time.md#jit-configure) yalnızca ona erişmek istediğinizde kullanabilirsiniz.  |


1. Doğrulama başarılı olduktan sonra, sanal makine oluşturma işlemini başlatmak için **Oluştur** ' u seçin.

   :::image type="content" source="../media/tutorial-access-private-cloud/ss11-review-create-wjb01.png" alt-text="Bir sıçrama kutusu için yeni bir Windows 10 VM oluşturun." border="true":::