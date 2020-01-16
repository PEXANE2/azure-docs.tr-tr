---
title: include dosyası
description: include dosyası
services: functions
author: ggailey777
manager: jeconnoc
ms.service: azure-functions
ms.topic: include
ms.date: 04/16/2019
ms.author: glenga
ms.custom: include file
ms.openlocfilehash: 66c100ecd40e9f0899f7686397339ab849315c3e
ms.sourcegitcommit: 3dc1a23a7570552f0d1cc2ffdfb915ea871e257c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/15/2020
ms.locfileid: "76021135"
---
## <a name="publish-the-project-to-azure"></a>Projeyi Azure'da yayımlama

Visual Studio Code, işlevler projenizi doğrudan Azure’da dağıtmanıza olanak sağlar. Süreç kapsamında, Azure abonelik bir işlev uygulaması ve ilgili kaynakları oluşturursunuz. İşlev uygulaması, işlevlerinize ilişkin bir yürütme bağlamı sağlar. Proje, Azure aboneliğinizdeki yeni işlev uygulamasında paketlenir ve dağıtılır.

Visual Studio Code, varsayılan olarak, işlev uygulamanızı oluşturmak için gereken tüm Azure kaynaklarını oluşturur. Bu kaynakların adları, seçtiğiniz işlev uygulaması adına göre yapılır. Oluşturulan kaynaklar üzerinde tam denetime sahip olmanız gerekiyorsa, bunun yerine [Gelişmiş seçenekleri kullanarak yayımlayabilirsiniz](../articles/azure-functions/functions-develop-vs-code.md#enable-publishing-with-advanced-create-options).

Bu bölümde, Azure 'da yeni bir işlev uygulaması oluşturduğunuzu varsayılmaktadır.

> [!IMPORTANT]
> Varolan bir işlev uygulamasına yayımladığınızda Azure’daki uygulamanın içeriğinin üzerine yazılır.

1. Visual Studio Code ' de, F1 tuşuna basarak komut paletini açın. Komut paletinde `Azure Functions: Deploy to function app...`arayıp seçin.

1. Oturum açmadıysanız, **Azure 'Da oturum açmanız**istenir. Ayrıca, **ücretsiz bir Azure hesabı da oluşturabilirsiniz**. Tarayıcıdan başarıyla oturum açtıktan sonra, Visual Studio Code ' ye geri dönün. 

1. Birden çok aboneliğiniz varsa, işlev uygulaması için **bir abonelik seçin** ve sonra da **Azure 'Da yeni işlev uygulaması oluştur**' u seçin.

1. İşlev uygulamanızı tanımlayan bir genelde benzersiz olan bir ad yazın ve Enter tuşuna basın. İşlev uygulaması adına ilişkin geçerli karakterler `a-z`, `0-9` ve `-` işaretidir.

    ENTER tuşuna bastığınızda, aboneliğinizde aşağıdaki Azure kaynakları oluşturulur:

    * **[Kaynak grubu](../articles/azure-resource-manager/management/overview.md)** : oluşturulan tüm Azure kaynaklarını içerir. Ad, işlev uygulamanızın adını temel alır.
    * **[Depolama hesabı](../articles/storage/common/storage-account-create.md)** : işlev uygulamanızın adını temel alan benzersiz bir adla standart bir depolama hesabı oluşturulur.
    * **[Barındırma planı](../articles/azure-functions/functions-scale.md)** : sunucusuz işlev uygulamanızı barındırmak için Batı ABD bölgede bir tüketim planı oluşturulur.
    * **İşlev uygulaması**: projeniz bu yeni işlev uygulamasında dağıtılır ve çalışır.

    İşlev uygulamanız oluşturulduktan sonra bir bildirim görüntülenir ve dağıtım paketi uygulanır. Oluşturduğunuz Azure kaynakları dahil olmak üzere oluşturma ve dağıtım sonuçlarını görüntülemek için bu bildirimde **çıktıyı görüntüle** ' yi seçin.

1. **Azure: Functions** alanına geri döndüğünüzde, aboneliğinizin altındaki yeni işlev uygulamasını genişletin. **İşlevler**' i genişletin, **httptrigger**öğesine sağ tıklayın ve ardından **işlev URL 'sini Kopyala**' yı seçin.

    ![Yeni HTTP tetikleyicisinin işlev URL 'sini kopyalayın](./media/functions-publish-project-vscode/function-copy-endpoint-url.png)
