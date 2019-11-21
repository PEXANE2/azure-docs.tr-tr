---
title: Create a function app from the Azure Portal
description: Create a new function app in Azure from the portal.
ms.topic: conceptual
ms.date: 08/29/2019
ms.custom: mvc
ms.openlocfilehash: 74eaa6837f362c849277a761da3ae79c3a8ac353
ms.sourcegitcommit: d6b68b907e5158b451239e4c09bb55eccb5fef89
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74230780"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Azure portalından işlev uygulaması oluşturma

This topic shows you how to use Azure Functions to create a function app in the Azure portal. İşlev uygulaması, tek tek işlevlerin yürütülmesini barındıran kapsayıcıdır. 

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

İşlev uygulaması oluştururken yalnızca harf, sayı ve kısa çizgi içerebilecek bir **Uygulama adı** sağlayın. Alt çizgi ( **_** ) izin verilen bir karakter değildir.

Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. 

İşlev uygulaması oluşturulduktan sonra bir veya daha fazla dilde tek tek işlevler oluşturabilirsiniz. [Portalı kullanarak](functions-create-first-azure-function.md#create-function), [sürekli dağıtım](functions-continuous-deployment.md) aracılığıyla veya [FTP ile karşıya yükleyerek](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp) çeşitli işlevler oluşturun.

## <a name="service-plans"></a>Hizmet planları

Azure Functions has three different service plans: Consumption plan, Premium plan, and Dedicated (App Service) plan. You must choose your service plan when your function app is created, and it cannot subsequently be changed. Daha fazla bilgi edinmek için bkz. [Azure İşlevleri barındırma planı seçme](functions-scale.md).

If you are planning to run JavaScript functions on a Dedicated (App Service) plan, you should choose a plan with fewer cores. Daha fazla bilgi edinmek için bkz. [İşlevler için JavaScript başvurusu](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

When creating a function app, you must create or link to a general-purpose Azure Storage account that supports Blob, Queue, and Table storage. Dahili olarak İşlevler tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Depolama’yı kullanır. Yalnızca blob depolama hesapları, Azure Premium Depolama ve ZRS çoğaltmalı genel amaçlı depolama hesapları gibi bazı depolama hesapları kuyrukları ve tabloları desteklemez. Bir işlev uygulaması oluşturulurken bu hesaplar Depolama Hesabı dikey penceresinden filtrelenir.

>[!NOTE]
>Tüketim barındırma planı kullanılırken işlev kodunuz ve bağlama yapılandırma dosyalarınız ana depolama hesabındaki Azure Dosya depolama alanında saklanır. Ana depolama hesabını sildiğinizde bu içerik silinir ve kurtarılamaz.

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Depolama Hizmetlerine Giriş](../storage/common/storage-introduction.md#azure-storage-services). 

## <a name="next-steps"></a>Sonraki adımlar

While the Azure portal makes it easy to create and try out Functions, we recommend [local development](functions-develop-local.md). After creating a function app in the portal, you still need to add a function. 

> [!div class="nextstepaction"]
> [Add an HTTP triggered function](functions-create-first-azure-function.md#create-function)
