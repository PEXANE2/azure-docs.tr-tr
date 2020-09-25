---
title: Azure VMware çözümünü dağıtma
description: Azure portal kullanarak Azure VMware çözümünü dağıtma adımları.
ms.topic: include
ms.date: 09/21/2020
ms.openlocfilehash: 4fa2925cf284871c4a168d2b69d4fbd3a81de878
ms.sourcegitcommit: 32c521a2ef396d121e71ba682e098092ac673b30
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/25/2020
ms.locfileid: "91254661"
---
<!-- Used in avs-deployment.md and tutorial-create-private-cloud.md -->

1. [Azure Portal](https://portal.azure.com) oturum açın.

1. **Yeni kaynak oluştur**' u seçin. **Market** metin kutusu yazın `Azure VMware Solution` ve listeden **Azure VMware çözümü** ' nü seçin. **Azure VMware Çözüm** penceresinde **Oluştur** ' u seçin.

1. **Temel bilgiler** sekmesinde, alanlar için değerler girin. Aşağıdaki tabloda alanlarıyla ilgili özellikler listelenmiştir.

   | Alan   | Değer  |
   | ---| --- |
   | **Abonelik** | Dağıtım için kullanmayı planladığınız abonelik.|
   | **Kaynak grubu** | Özel bulut kaynaklarınızın kaynak grubu. |
   | **Konum** | **Doğu ABD**gibi bir konum seçin.|
   | **Kaynak adı** | Azure VMware çözümünüz özel bulutunuzun adı. |
   | **SKU** | Şu SKU değerini seçin: AV36 |
   | **Bilgisayarlarınızı** | Özel bulut kümesine eklenecek ana bilgisayar sayısı. Varsayılan değer, dağıtımdan sonra ortaya çıkarılan veya düşürülen 3 ' dir.  |
   | **vCenter yönetici parolası** | Bir bulut Yöneticisi parolası girin. |
   | **NSX-T Yöneticisi parolası** | NSX-T yönetici parolası girin. |
   | **Adres bloğu** | Özel bulut için CıDR ağı için bir IP adresi bloğu girin, örneğin, 10.175.0.0/22. |
   | **Sanal Ağ** | Azure VMware çözümü özel bulutu için bir sanal ağ seçin veya yeni bir tane oluşturun.  |

   :::image type="content" source="../media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="Temel bilgiler sekmesinde, alanlar için değerler girin." border="true":::

1. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin. Sonraki ekranda, girilen bilgileri doğrulayın. Bilgilerin tümü doğru ise **Oluştur**' u seçin.

   > [!NOTE]
   > Bu adım kabaca iki saat sürer. 

1. Dağıtımın başarılı olduğunu doğrulayın. Oluşturduğunuz kaynak grubuna gidin ve özel bulutunuzu seçin.  Dağıtım tamamlandığında **başarılı** durumunu görürsünüz. 

   :::image type="content" source="../media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Dağıtımın başarılı olduğunu doğrulayın." border="true":::