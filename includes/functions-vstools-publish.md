---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
ms.service: azure-functions
ms.topic: include
ms.date: 11/02/2018
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 689889588aba4da888a7d66f5e1d45dfde71d520
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021297"
---
1. **Çözüm Gezgini**'nde projeye sağ tıklayın ve **Yayımla**'yı seçin.

2. **Bir yayımlama hedefi seçin** iletişim kutusunda, görüntünün altındaki tabloda belirtilen Yayımla seçeneklerini kullanın: 

    ![Yayımlama hedefi seçme](./media/functions-vstools-publish/functions-visual-studio-publish-profile.png)

    | Seçenek      | Açıklama                                |
    | ------------ |  -------------------------------------------------- |
    | **Azure Işlevleri tüketim planı** |   Projenizi bir [Tüketim planında](../articles/azure-functions/functions-scale.md#consumption-plan)çalışan bir işlev uygulamasında yayımladığınızda yalnızca işlevler uygulamanızın yürütmeleri için ödeme yaparsınız. Diğer barındırma planları daha yüksek maliyetler doğurur. Daha fazla bilgi için bkz. [Azure işlevleri ölçeklendirme ve barındırma](../articles/azure-functions/functions-scale.md). | 
    | **Yeni oluştur** | Azure 'da, ilgili kaynaklarla yeni bir işlev uygulaması oluşturulur. **Varolanı Seç**'i seçtiğinizde, yerel projedeki dosyalar Azure'da mevcut işlev uygulamasındaki tüm dosyaların üzerine yazılır. Bu seçeneği yalnızca mevcut işlev uygulamasına yeniden güncelleştirme yayımlarken kullanın. |
    | **Paket dosyasından Çalıştır** | İşlev uygulamanız, [paket Çalıştır](../articles/azure-functions/run-functions-from-deployment-package.md) modu etkin olan [ZIP dağıtımı](../articles/azure-functions/functions-deployment-technologies.md#zip-deploy) kullanılarak dağıtılır. Bu, işlevlerinizi çalıştırmanın önerilen yoludur ve daha iyi performans elde edilir. Bu seçeneği kullanmadığınız durumlarda, Azure 'da yayımlamadan önce işlev uygulaması projenizin yerel olarak çalışmasını durdurduğunuzdan emin olun. |


3. **Yayımla**’yı seçin. Visual Studio 'dan Azure hesabınızda zaten oturum açmadıysanız **oturum aç**' ı seçin. Ayrıca, ücretsiz bir Azure hesabı da oluşturabilirsiniz.

4. **Azure App Service: Yeni oluştur** iletişim kutusunda, görüntünün altındaki tabloda belirtilen **barındırma** ayarlarını kullanın:

    ![App Service iletişim kutusu oluşturma](./media/functions-vstools-publish/functions-visual-studio-publish.png)

    | Ayar      | Önerilen değer  | Açıklama                                |
    | ------------ |  ------- | -------------------------------------------------- |
    | **Adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı benzersiz şekilde tanımlayan ad. Geçerli karakterler: `a-z`, `0-9`, ve `-`. |
    | **Abonelik** | Aboneliğinizi seçin | Kullanılacak Azure aboneliği. |
    | **[Kaynak Grubu](../articles/azure-resource-manager/management/overview.md)** | myResourceGroup |  İşlev uygulamanızın oluşturulacağı kaynak grubunun adı. Yeni kaynak grubu oluşturmak **Yeni**'yi seçin.|
    | **[Barındırma planı](../articles/azure-functions/functions-scale.md)** | Tüketim planı | Sunucusuz bir plan oluşturmak için **Yeni** ' yi seçtikten sonra **Boyut** ' un altında **tüketimi** seçtiğinizden emin olun. Ayrıca, kendinize veya işlevlerinizin erişeceği diğer hizmetlere yakın bir [bölgede](https://azure.microsoft.com/regions/) yer alan bir **Konum** seçin. **Tüketim** dışında bir planda çalıştırdığınızda, [işlev uygulamanızın ölçeklendirmesini](../articles/azure-functions/functions-scale.md) yönetmelisiniz.  |
    | **[Azure Depolama](../articles/storage/common/storage-account-create.md)** | Genel amaçlı depolama hesabı | İşlevler çalışma zamanı için bir Azure depolama hesabı gereklidir. Genel amaçlı bir depolama hesabı oluşturmak için **Yeni** ' yi seçin. Dilerseniz [depolama hesabı gereksinimlerini](../articles/azure-functions/functions-scale.md#storage-account-requirements) karşılayan mevcut bir hesap da kullanabilirsiniz.  |

5. Azure 'da bu ayarlarla bir işlev uygulaması ve ilgili kaynaklar oluşturmak ve işlev proje kodunuzu dağıtmak için **Oluştur** ' u seçin. 

6. Dağıtım tamamlandığında, **Site URL'si** değerini (Azure'daki işlev uygulamanızın adresi) not edin.

    ![Başarı iletisi yayımlama](./media/functions-vstools-publish/functions-visual-studio-publish-complete.png)
