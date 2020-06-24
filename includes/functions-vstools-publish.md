---
title: dosya dahil etme
description: dosya dahil etme
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: db6f5927e1cbdb3527014ccbb277c7ec20b02600
ms.sourcegitcommit: f1132db5c8ad5a0f2193d751e341e1cd31989854
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 05/31/2020
ms.locfileid: "84231518"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

1. **Bir yayımlama hedefi seçin**alanında, aşağıdaki tabloda belirtilen yayımlama seçeneklerini kullanın: 

    | Seçenek      | Description                                |
    | ------------ |  -------------------------------------------------- |
    | **Azure Işlevleri tüketim planı** | Bir [Tüketim planında](../articles/azure-functions/functions-scale.md#consumption-plan)çalışan bir Azure bulut ortamında bir işlev uygulaması oluşturun. Bir tüketim planı kullandığınızda yalnızca işlevler uygulamanızın yürütmeleri için ödeme yaparsınız. Diğer barındırma planları daha yüksek maliyetler doğurur. Tüketim planı dışında bir planda çalıştırırsanız, [işlev uygulamanızın ölçeklendirmesini](../articles/azure-functions/functions-scale.md)yönetmeniz gerekir.| 
    | **Yeni oluştur** | Azure 'da, ilgili kaynaklarla yeni bir işlev uygulaması oluşturulur. <br/>**Varolanı Seç**' i seçerseniz, mevcut Işlev uygulamasındaki Azure 'daki tüm dosyaların yerel projedeki dosyalarla üzerine yazılır. Bu seçeneği yalnızca, var olan bir işlev uygulamasına güncelleştirmeleri yeniden yayımladığınızda kullanın. |
    | **Paket dosyasından Çalıştır** | İşlev uygulamanız, [paket Çalıştır](../articles/azure-functions/run-functions-from-deployment-package.md) modu etkin olan [ZIP dağıtımı](../articles/azure-functions/functions-deployment-technologies.md#zip-deploy) kullanılarak dağıtılır. Bu dağıtım, daha iyi performans elde eden işlevlerinizi çalıştırmanın önerilen yoludur. <br/>Bu seçeneği kullanmazsanız, Azure 'da yayımlamadan önce işlev uygulaması projenizin yerel olarak çalışmasını durdurduğunuzdan emin olun. |

    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-profile.png" alt-text="Yayımlama profili oluşturma":::


1. **Profil Oluştur**'u seçin. Visual Studio 'dan Azure hesabınızda zaten oturum açmadıysanız **oturum aç**' ı seçin. Ayrıca, ücretsiz bir Azure hesabı da oluşturabilirsiniz.

1. **App Service: Yeni oluştur**' da, aşağıdaki tabloda belirtilen değerleri kullanın:

    | Ayar      | Değer  | Description                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. Bu adı kabul edin veya yeni bir ad girin. Geçerli karakterler şunlardır: `a-z` , `0-9` , ve `-` . |
    | **Abonelik** | Aboneliğiniz | Kullanılacak Azure aboneliği. Bu aboneliği kabul edin veya açılan listeden yeni bir tane seçin. |
    | **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** | Kaynak grubunuzun adı |  İşlev uygulamanızın oluşturulacağı kaynak grubu. Açılan listeden var olan bir kaynak grubunu seçin veya yeni bir kaynak grubu oluşturmak için **Yeni** ' yi seçin.|
    | **[Barındırma planı](../articles/azure-functions/functions-scale.md)** | Barındırma planınızın adı | Sunucusuz bir plan yapılandırmak için **Yeni** ' yi seçin. **Boyut**' un altında **tüketimi** seçtiğinizden emin olun. Projenizi bir [Tüketim planında](../articles/azure-functions/functions-scale.md#consumption-plan)çalışan bir işlev uygulamasında yayımladığınızda yalnızca işlevler uygulamanızın yürütmeleri için ödeme yaparsınız. Diğer barındırma planları daha yüksek maliyetler doğurur. **Tüketim**dışında bir planda çalıştırırsanız, [işlev uygulamanızın ölçeklendirmesini](../articles/azure-functions/functions-scale.md)yönetmeniz gerekir.  |
    | **Konum** | App Service 'in konumu | Size yakın bir [bölgede](https://azure.microsoft.com/regions/) veya işlevlerinizin erişebileceği diğer hizmetlerden bir **konum** seçin. |
    | **[Azure Depolama](../articles/storage/common/storage-account-create.md)** | Genel amaçlı depolama hesabı | Işlevler çalışma zamanı için bir Azure depolama hesabı gereklidir. Genel amaçlı bir depolama hesabı yapılandırmak için **Yeni** ' yi seçin. [Depolama hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements)karşılayan mevcut bir hesabı da seçebilirsiniz.  |

    ![App Service Oluştur iletişim kutusu](./media/functions-vstools-publish/functions-visual-studio-publish.png)

1. Azure 'da bu ayarlarla bir işlev uygulaması ve ilgili kaynakları oluşturmak ve işlev proje kodunuzu dağıtmak için **Oluştur** ' u seçin. 

1. **Yayımla** ' yı seçin ve dağıtımın tamamlanmasını bekleyin. 

    Dağıtım tamamlandıktan sonra, Azure 'da işlev uygulamasının kök URL 'SI **Yayımla** sekmesinde gösterilir. 
    
1.  Yayımla sekmesinde, **Cloud Explorer 'Da Yönet**' i seçin. Bu, Cloud Explorer 'da yeni işlev uygulaması Azure kaynağı ' nı açar. 
    
    :::image type="content" source="media/functions-vstools-publish/functions-visual-studio-publish-complete.png" alt-text="Başarı iletisi yayımlama":::
    
    Cloud Explorer, Visual Studio 'Yu kullanarak sitenin içeriğini görüntüleyebilir, işlev uygulamasını başlatıp durdurabilir ve doğrudan Azure 'daki ve Azure portal uygulama kaynaklarına gidebilir. 