---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 03/06/2020
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 0e69a11ab81406f650049e3c7d4d446b6628b29b
ms.sourcegitcommit: 62c5557ff3b2247dafc8bb482256fef58ab41c17
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/03/2020
ms.locfileid: "80657502"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

2. **Yayımlama hedefini**seç'te, aşağıdaki tabloda belirtilen yayımlama seçeneklerini kullanın: 

    | Seçenek      | Açıklama                                |
    | ------------ |  -------------------------------------------------- |
    | **Azure Fonksiyonları Tüketim Planı** | [Bir Tüketim planında](../articles/azure-functions/functions-scale.md#consumption-plan)çalışan bir Azure bulut ortamında bir işlev uygulaması oluşturun. Bir Tüketim planı kullandığınızda, yalnızca işlevler uygulamanızın yürütülmesi için ödeme yaptığınızda, bu uygulamanın yürütülmesi için ödeme görürüz. Diğer barındırma planları daha yüksek maliyetlere neden oluyor. Tüketim planı dışında bir planda çalışıyorsanız, işlev [uygulamanızın ölçeklemesi](../articles/azure-functions/functions-scale.md)yönetmelisiniz.| 
    | **Yeni Oluştur** | Azure'da ilgili kaynaklara sahip yeni bir işlev uygulaması oluşturulur. <br/>**Varla'yı Seç'i**seçerseniz, Azure'daki varolan işlev uygulamasındaki tüm dosyalar yerel projedeki dosyalar tarafından üzerine yazılır. Bu seçeneği yalnızca varolan bir işlev uygulamasında güncelleştirmeleri yeniden yayımladığınızda kullanın. |
    | **Paket dosyasından çalıştırma** | İşlev uygulamanız [Paketten Çalıştır](../articles/azure-functions/run-functions-from-deployment-package.md) modu etkinken [Zip Dağıtımı](../articles/azure-functions/functions-deployment-technologies.md#zip-deploy) kullanılarak dağıtılır. Daha iyi performans la sonuçlanan bu dağıtım, işlevlerinizi çalıştırmanın önerilen yoludur. <br/>Bu seçeneği kullanmıyorsanız, Azure'da yayımlamadan önce işlev uygulama projenizin yerel olarak çalışmasını durdurduğunuzdan emin olun. |

    ![Yayımlama hedefi seçme](./media/functions-vstools-publish/functions-visual-studio-publish-profile.png)


3. **Profil Oluştur**'u seçin. Visual Studio'dan Azure hesabınızda oturum açmadıysanız, **Oturum Aç'ı'yı**seçin. Ayrıca ücretsiz bir Azure hesabı da oluşturabilirsiniz.

4. **Uygulama Hizmetinde: Yeni oluşturun,** aşağıdaki tabloda belirtilen değerleri kullanın:

    | Ayar      | Değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. Bu adı kabul edin veya yeni bir ad girin. Geçerli karakterler şunlardır: `a-z`, `0-9`, ve `-`. |
    | **Abonelik** | Aboneliğiniz | Kullanılacak Azure aboneliği. Bu aboneliği kabul edin veya açılan listeden yeni bir abonelik seçin. |
    | **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** | Kaynak grubunuzun adı |  İşlev uygulamanızı oluşturacak kaynak grubu. Açılan listeden varolan bir kaynak grubu seçin veya yeni bir kaynak grubu oluşturmak için **Yeni'yi** seçin.|
    | **[Hosting Planı](../articles/azure-functions/functions-scale.md)** | Barındırma planınızın adı | Sunucusuz bir planı yapılandırmak için **Yeni'yi** seçin. **Boyut**Altındaki **Tüketim'i** seçtiğinizden emin olun. Projenizi [Tüketim planında](../articles/azure-functions/functions-scale.md#consumption-plan)çalışan bir işlev uygulamasında yayımladığınızda, yalnızca işlevler uygulamanızın yürütülmesi için ödeme yaptığınızda ödeme görür. Diğer barındırma planları daha yüksek maliyetlere neden oluyor. **Tüketim**dışında bir planda çalışıyorsanız, [işlev uygulamanızın ölçeklemesi](../articles/azure-functions/functions-scale.md)yönetmelisiniz.  |
    | **Konum** | Uygulama hizmetinin konumu | Yakınınızdaki bir [bölgede](https://azure.microsoft.com/regions/) veya işlevlerinizin erişebileceği diğer hizmetlerde bir **Konum** seçin. |
    | **[Azure Storage](../articles/storage/common/storage-account-create.md)** | Genel amaçlı depolama hesabı | İşlevler çalışma zamanı tarafından bir Azure Depolama hesabı gereklidir. Genel amaçlı bir depolama hesabını yapılandırmak için **Yeni'yi** seçin. Ayrıca, [depolama hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements)karşılayan varolan bir hesap da seçebilirsiniz.  |

    ![App Service Oluştur iletişim kutusu](./media/functions-vstools-publish/functions-visual-studio-publish.png)

5. Bu ayarlarla Azure'da bir işlev uygulaması ve ilgili kaynakları oluşturmak ve işlev proje kodunuzu dağıtmak için **Oluştur'u** seçin. 

6. Yayımla'yı seçin ve dağıtım tamamlandıktan sonra Azure'daki işlev uygulamanızın adresi olan **Site URL** değerine not alın.

    ![Başarı iletisi yayımlama](./media/functions-vstools-publish/functions-visual-studio-publish-complete.png)
