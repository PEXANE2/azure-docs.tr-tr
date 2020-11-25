---
title: Azure Container Instances üzerinde LUSıS kapsayıcısını dağıtma
titleSuffix: Azure Cognitive Services
description: LUSıS kapsayıcısını bir Azure Container örneğine dağıtın ve bir Web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: edc2ad0f895b8a1bb6448ce1cdf79b1b2ce83951
ms.sourcegitcommit: 10d00006fec1f4b69289ce18fdd0452c3458eca5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/21/2020
ms.locfileid: "95997213"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Azure Container Instances 'a Language Understanding (LUSıS) kapsayıcısını dağıtma

Bilişsel Hizmetler [lusıs](luis-container-howto.md) kapsayıcısını Azure [Container Instances](../../container-instances/index.yml)'a dağıtmayı öğrenin. Bu yordam bir anomali algılayıcısı kaynağı oluşturmayı gösterir. Ardından, ilişkili kapsayıcı görüntüsünü çekme hakkında tartışın. Son olarak, bir tarayıcıdan ikisini de düzenleme özelliğini vurgulayacağız. Kapsayıcıları kullanmak, geliştiricilerin bu dikkatini, uygulama geliştirmeye odaklanmadan, altyapının bir şekilde yönetilmesini sağlar.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

LUSıS kapsayıcısı, `.gz` çalışma zamanında çekilecek bir model dosyası gerektiriyor. Kapsayıcı, kapsayıcı örneğinden bir birim bağlaması aracılığıyla bu model dosyasına erişebilmelidir. Azure dosya paylaşma oluşturma hakkında bilgi için bkz. [dosya paylaşma oluşturma](../../storage/files/storage-how-to-create-file-share.md). Daha sonra ihtiyacınız olacak şekilde Azure depolama hesabı adı, anahtar ve dosya paylaşımının adını göz önünde ayırın.

### <a name="export-and-upload-packaged-luis-app"></a>Paketlenmiş LUSıS uygulamasını dışarı ve karşıya yükleme

LUO modelini (paketlenmiş uygulama) Azure dosya paylaşımında karşıya yüklemek için <a href="luis-container-howto.md#export-packaged-app-from-luis" target="_blank" rel="noopener"> <span class="docon docon-navigate-external x-hidden-focus"></span> önce bu dosyayı Luo portalından dışarı aktarmanız </a>gerekir. Azure portal, depolama hesabı kaynağının **genel bakış** sayfasına gidin ve **dosya paylaşımları**' nı seçin. Son oluşturduğunuz dosya paylaşımının adını seçip **karşıya yükle** düğmesini seçin.

> [!div class="mx-imgBorder"]
> ![Dosya paylaşımıyla karşıya yükle](media/luis-how-to-deploy-to-aci/upload-file-share.png)

LUSıS model dosyasını karşıya yükleyin.

[!INCLUDE [Create LUIS Container instance resource](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]