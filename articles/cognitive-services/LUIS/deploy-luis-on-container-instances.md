---
title: Azure Kapsayıcı örneklerinde LUIS kapsayıcısı dağıtma
titleSuffix: Azure Cognitive Services
description: LUIS kapsayıcısını azure kapsayıcıörneğine dağıtın ve bir web tarayıcısında test edin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: language-understanding
ms.topic: conceptual
ms.date: 04/07/2020
ms.author: aahi
ms.openlocfilehash: 08af17106846a0f5f7a0ccc2b01da1b2e15c1143
ms.sourcegitcommit: 2d7910337e66bbf4bd8ad47390c625f13551510b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/08/2020
ms.locfileid: "80879233"
---
# <a name="deploy-the-language-understanding-luis-container-to-azure-container-instances"></a>Dil Bilgisi (LUIS) kapsayıcısını Azure Kapsayıcıörneklerine dağıtma

Bilişsel Hizmetler [LUIS](luis-container-howto.md) kapsayıcısını Azure [Kapsayıcı örneklerine](https://docs.microsoft.com/azure/container-instances/)nasıl dağıtabileceğinizi öğrenin. Bu yordam, bir Anomali Dedektörü kaynağının oluşturulmasını göstermektedir. Sonra ilişkili konteyner görüntü çekme tartışmak. Son olarak, bir tarayıcıdan iki orkestrasyon egzersiz yeteneğini vurgulamak. Kapsayıcıların kullanılması, geliştiricilerin dikkatini altyapı yönetiminden uygulama geliştirmeye odaklanmak yerine uzaklaştırabilir.

[!INCLUDE [Prerequisites](../containers/includes/container-prerequisites.md)]

[!INCLUDE [Create LUIS resource](includes/create-luis-resource.md)]

[!INCLUDE [Gathering required parameters](../containers/includes/container-gathering-required-parameters.md)]

## <a name="create-an-azure-file-share"></a>Azure dosya paylaşımı oluşturma

LUIS kapsayıcısı `.gz` çalışma zamanında çekilen bir model dosyası gerektirir. Kapsayıcı, bu model dosyasına Kapsayıcı örneğinden bir ses montajı aracılığıyla erişebilmeli. Azure dosya paylaşımı oluşturma hakkında bilgi [için](../../storage/files/storage-how-to-create-file-share.md)bkz. Azure Depolama hesap adını, anahtarını ve dosya paylaşım adını daha sonra ihtiyaç darayacağınız için not alın.

### <a name="export-and-upload-packaged-luis-app"></a>Paketli LUIS uygulamasını dışa aktarma ve yükleme

LUIS modelini (paket uygulama) Azure dosya paylaşımına yüklemek için <a href="luis-container-howto.md#export-packaged-app-from-luis" target="_blank" rel="noopener">önce <span class="docon docon-navigate-external x-hidden-focus"> </span>LUIS portalından dışa aktarmanız </a>gerekir. Azure portalından, depolama hesabı kaynağının **Genel Bakış** sayfasına gidin ve **Dosya paylaşımlarını**seçin. Yakın zamanda oluşturduğunuz dosya paylaşım adını seçin ve ardından **Yükle** düğmesini seçin.

> [!div class="mx-imgBorder"]
> ![Dosya paylaşımına yükleme](media/luis-how-to-deploy-to-aci/upload-file-share.png)

LUIS model dosyasını yükleyin.

[!INCLUDE [Create LUIS Container instance resource](../containers/includes/create-container-instances-resource-from-azure-cli.md)]

[!INCLUDE [API documentation](../../../includes/cognitive-services-containers-api-documentation.md)]

[!INCLUDE [Next steps](../containers/includes/containers-next-steps.md)]
