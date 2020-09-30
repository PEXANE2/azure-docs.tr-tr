---
title: Azure VMware çözümünü dağıtma
description: Azure portal kullanarak Azure VMware çözümünü dağıtma adımları.
ms.topic: include
ms.date: 09/28/2020
ms.openlocfilehash: 4d0881721cd8c13d1b6c9fb3a29e4cdb6d6a753f
ms.sourcegitcommit: a422b86148cba668c7332e15480c5995ad72fa76
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 09/30/2020
ms.locfileid: "91578333"
---
<!-- Used in deploy-azure-vmware-solution.md and tutorial-create-private-cloud.md -->

1. [Azure portalında](https://portal.azure.com) oturum açın.

1. **Yeni kaynak oluştur**' u seçin. **Market** metin kutusu yazın `Azure VMware Solution` ve listeden **Azure VMware çözümü** ' nü seçin. **Azure VMware Çözüm** penceresinde **Oluştur**' u seçin.

1. **Temel bilgiler** sekmesinde, alanlar için değerler girin. Aşağıdaki tabloda alanlarıyla ilgili özellikler listelenmiştir.

   | Alan   | Değer  |
   | ---| --- |
   | **Abonelik** | Dağıtım için kullanmayı planladığınız abonelik.|
   | **Kaynak grubu** | Özel bulut kaynaklarınızın kaynak grubu. |
   | **Konum** | **Doğu ABD**gibi bir konum seçin.|
   | **Kaynak adı** | Azure VMware çözümünüz özel bulutunuzun adı. |
   | **SKU** | Şu SKU değerini seçin: AV36 |
   | **Ana bilgisayarlar** | Özel bulut kümesine eklenecek ana bilgisayar sayısı. Varsayılan değer, dağıtımdan sonra ortaya çıkarılan veya düşürülen 3 ' dir.  |
   | **vCenter yönetici parolası** | Bir bulut Yöneticisi parolası girin. |
   | **NSX-T Yöneticisi parolası** | NSX-T yönetici parolası girin. |
   | **Adres bloğu** | Özel bulut için CıDR ağı için bir IP adresi bloğu girin, örneğin, 10.175.0.0/22. |
   | **Sanal ağ** | Azure VMware çözümü özel bulutu için bir sanal ağ seçin veya yeni bir tane oluşturun.  |

   :::image type="content" source="../media/tutorial-create-private-cloud/create-private-cloud.png" alt-text="Temel bilgiler sekmesinde, alanlar için değerler girin." border="true":::

1. İşiniz bittiğinde, **gözden geçir + oluştur**' u seçin. Sonraki ekranda, girilen bilgileri doğrulayın. Bilgilerin tümü doğru ise **Oluştur**' u seçin.

   > [!NOTE]
   > Bu adım kabaca iki saat sürer. 

1. Dağıtımın başarılı olduğunu doğrulayın. Oluşturduğunuz kaynak grubuna gidin ve özel bulutunuzu seçin.  Dağıtım tamamlandığında **başarılı** durumunu görürsünüz. 

   :::image type="content" source="../media/tutorial-create-private-cloud/validate-deployment.png" alt-text="Temel bilgiler sekmesinde, alanlar için değerler girin." border="true":::