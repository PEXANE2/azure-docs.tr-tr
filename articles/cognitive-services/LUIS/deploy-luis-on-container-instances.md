---
title: Azure Container Instances üzerinde LUSıS kapsayıcısını dağıtma
titleSuffix: Azure Cognitive Services
description: LUSıS kapsayıcısını bir Azure Container örneğine dağıtın ve bir Web tarayıcısında test edin.
services: cognitive-services
author: IEvangelist
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 01/06/2020
ms.author: dapine
ms.openlocfilehash: 30fd19634f6054b8b636dabcb4ef83b118554468
ms.sourcegitcommit: 2f8ff235b1456ccfd527e07d55149e0c0f0647cc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/07/2020
ms.locfileid: "75689428"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Azure Container Instances 'a Language Understanding (LUSıS) kapsayıcısını dağıtma

Bilişsel Hizmetler [lusıs](luis-container-howto.md) kapsayıcısını Azure [Container Instances](https://docs.microsoft.com/azure/container-instances/)'a dağıtmayı öğrenin. Bu yordam bir anomali algılayıcısı kaynağı oluşturmayı gösterir. Ardından, ilişkili kapsayıcı görüntüsünü çekme hakkında tartışın. Son olarak, bir tarayıcıdan ikisini de düzenleme özelliğini vurgulayacağız. Kapsayıcıları kullanmak, geliştiricilerin bu dikkatini, uygulama geliştirmeye odaklanmadan, altyapının bir şekilde yönetilmesini sağlar.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

LUSıS kapsayıcısı, çalışma zamanında çekilecek bir `.gz` modeli dosyası gerektirir. Kapsayıcı, kapsayıcı örneğinden bir birim bağlaması aracılığıyla bu model dosyasına erişebilmelidir. Azure dosya paylaşma oluşturma hakkında bilgi için bkz. [dosya paylaşma oluşturma](../../storage/files/storage-how-to-create-file-share.md). Daha sonra ihtiyacınız olacak şekilde Azure depolama hesabı adı, anahtar ve dosya paylaşımının adını göz önünde ayırın.

### <a name="export-and-upload-packaged-luis-app"></a>Paketlenmiş LUSıS uygulamasını dışarı ve karşıya yükleme

LUO modelini (paketlenmiş uygulama) Azure dosya paylaşımında karşıya yüklemek için <a href="luis-container-howto.md#export-packaged-app-from-luis" target="_blank" rel="noopener"> <span class="docon docon-navigate-external x-hidden-focus"> </span>önce bu dosyayı Luo portalından dışarı aktarmanız </a>gerekir. Azure portal, depolama hesabı kaynağının **genel bakış** sayfasına gidin ve **dosya paylaşımları**' nı seçin. Son oluşturduğunuz dosya paylaşımının adını seçip **karşıya yükle** düğmesini seçin.

> [!div class="mx-imgBorder"]
> ![dosya paylaşımında karşıya yükle](media/luis-how-to-deploy-to-aci/upload-file-share.png)

LUSıS model dosyasını karşıya yükleyin.

[!INCLUDE [Create LUIS Container instance resource](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
