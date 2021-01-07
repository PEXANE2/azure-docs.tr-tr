---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 09/30/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: e4231f5c4e947f29a72e581cf461ff737c1a0c82
ms.sourcegitcommit: 2aa52d30e7b733616d6d92633436e499fbe8b069
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/06/2021
ms.locfileid: "97956461"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Hedef** bölümünde **Azure** :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-profile-step-1.png" alt-text="Azure hedefi seç"::: ' i seçin.

1. **Belirli bir hedefte** **Azure işlev uygulaması (Windows)** öğesini seçin

    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-profile-step-2.png" alt-text="Azure İşlev Uygulaması seçin":::

1. **Işlev örneği**' nde **Yeni bir Azure işlevi oluştur...** öğesini seçin ve ardından aşağıdaki tabloda belirtilen değerleri kullanın:

    | Ayar      | Değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Ad** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. Bu adı kabul edin veya yeni bir ad girin. Geçerli karakterler şunlardır: `a-z` , `0-9` , ve `-` . |
    | **Abonelik** | Aboneliğiniz | Kullanılacak Azure aboneliği. Bu aboneliği kabul edin veya açılan listeden yeni bir tane seçin. |
    | **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** | Kaynak grubunuzun adı |  İşlev uygulamanızın oluşturulacağı kaynak grubu. Açılan listeden var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturmak için **Yeni** ' yi seçin.|
    | **[Plan türü](../articles/azure-functions/functions-scale.md)** | Tüketim | Projenizi bir [Tüketim planında](../articles/azure-functions/consumption-plan.md)çalışan bir işlev uygulamasında yayımladığınızda yalnızca işlevler uygulamanızın yürütmeleri için ödeme yaparsınız. Diğer barındırma planları daha yüksek maliyetler doğurur. |
    | **Konum** | App Service 'in konumu | Size yakın bir [bölgede](https://azure.microsoft.com/regions/) veya işlevlerinizin erişebileceği diğer hizmetlerden bir **konum** seçin. |
    | **[Azure Storage](../articles/azure-functions/storage-considerations.md)** | Genel amaçlı depolama hesabı | Işlevler çalışma zamanı için bir Azure depolama hesabı gereklidir. Genel amaçlı bir depolama hesabı yapılandırmak için **Yeni** ' yi seçin. [Depolama hesabı gereksinimlerini](../articles/azure-functions/storage-considerations.md#storage-account-requirements)karşılayan mevcut bir hesabı da seçebilirsiniz.  |

    ![App Service Oluştur iletişim kutusu](./media/functions-vstools-publish/functions-visual-studio-publish.png)

1. Azure 'da bir işlev uygulaması ve ilgili kaynakları oluşturmak için **Oluştur** ' u seçin. 
1. **İşlevler örneğinde**, **paket dosyasından Çalıştır** ' ın işaretli olduğundan emin olun. İşlev uygulamanız, [paket Çalıştır](../articles/azure-functions/run-functions-from-deployment-package.md) modu etkin olan [ZIP dağıtımı](../articles/azure-functions/functions-deployment-technologies.md#zip-deploy) kullanılarak dağıtılır. Bu, daha iyi performans ile sonuçlandığından, işlevler projeniz için önerilen dağıtım yöntemidir. 

    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-profile-step-4.png" alt-text="Profil oluşturmayı tamamlama":::

1. **Son**' u seçin ve Yayımla sayfasında, proje dosyalarınızı Içeren paketi Azure 'daki yeni işlev uygulamanıza dağıtmak için **Yayımla** ' yı seçin. 

    Dağıtım tamamlandıktan sonra, Azure 'da işlev uygulamasının kök URL 'SI **Yayımla** sekmesinde gösterilir. 
    
1.  Yayımla sekmesinde, **Cloud Explorer 'Da Yönet**' i seçin. Bu, Cloud Explorer 'da yeni işlev uygulaması Azure kaynağı ' nı açar. 
    
    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-complete.png" alt-text="Başarı iletisi yayımlama":::
    
    Cloud Explorer, Visual Studio 'Yu kullanarak sitenin içeriğini görüntüleyebilir, işlev uygulamasını başlatıp durdurabilir ve doğrudan Azure 'daki ve Azure portal uygulama kaynaklarına gidebilir. 
