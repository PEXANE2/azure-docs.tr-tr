---
title: Azure Portal'dan işlev uygulaması oluşturma | Microsoft Docs
description: Portaldan Azure App Service’te yeni bir işlev uygulaması oluşturun.
services: functions
documentationcenter: na
author: ggailey777
manager: jeconnoc
ms.assetid: ''
ms.service: azure-functions
ms.topic: quickstart
ms.date: 04/11/2017
ms.author: glenga
ms.custom: mvc
ms.openlocfilehash: eb362ec652b306a12a41e7e96dcbc86638369c17
ms.sourcegitcommit: 44e85b95baf7dfb9e92fb38f03c2a1bc31765415
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/28/2019
ms.locfileid: "70085885"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Azure portalından işlev uygulaması oluşturma

Azure İşlev Uygulamaları, Azure App Service altyapısını kullanır. Bu konuda, Azure portalında işlev uygulaması oluşturma açıklanır. İşlev uygulaması, tek tek işlevlerin yürütülmesini barındıran kapsayıcıdır. App Service barındırma planında bir işlev uygulaması oluşturduğunuzda işlev uygulamanız tüm App Service özelliklerini kullanabilir.

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

İşlev uygulaması oluştururken yalnızca harf, sayı ve kısa çizgi içerebilecek bir **Uygulama adı** sağlayın. Alt çizgi ( **_** ) izin verilen bir karakter değildir.

Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Depolama hesabınızın adının Azure içinde benzersiz olması gerekir. 

İşlev uygulaması oluşturulduktan sonra bir veya daha fazla dilde tek tek işlevler oluşturabilirsiniz. [Portalı kullanarak](functions-create-first-azure-function.md#create-function), [sürekli dağıtım](functions-continuous-deployment.md) aracılığıyla veya [FTP ile karşıya yükleyerek](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp) çeşitli işlevler oluşturun.

## <a name="service-plans"></a>Hizmet planları

Azure Işlevleri iki farklı hizmet planına sahiptir: Tüketim planı ve App Service planı. Tüketim planı kodunuz çalışırken otomatik olarak işlem gücü ayırır, gerektiğinde yükü kaldıracak şekilde ölçeği genişletir ve sonra kod çalışmadığı sırada ölçeği daraltır. App Service planı, işlev uygulamanızın App Service olanaklarına erişmesine imkan tanır. Hizmet planınızı işlev uygulamanız oluşturulurken seçmelisiniz ve şu anda bu plan değiştirilemez. Daha fazla bilgi edinmek için bkz. [Azure İşlevleri barındırma planı seçme](functions-scale.md).

Bir App Service planında JavaScript işlevleri çalıştırmayı planlıyorsanız daha az çekirdek içeren bir plan seçmelisiniz. Daha fazla bilgi edinmek için bkz. [İşlevler için JavaScript başvurusu](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

App Service’te bir işlev uygulaması oluştururken Blob, Kuyruk ve Tablo depolamayı destekleyen genel amaçlı bir Azure Depolama hesabı oluşturmanız veya bağlamanız gerekir. Dahili olarak İşlevler tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Depolama’yı kullanır. Yalnızca blob depolama hesapları, Azure Premium Depolama ve ZRS çoğaltmalı genel amaçlı depolama hesapları gibi bazı depolama hesapları kuyrukları ve tabloları desteklemez. Bir işlev uygulaması oluşturulurken bu hesaplar Depolama Hesabı dikey penceresinden filtrelenir.

>[!NOTE]
>Tüketim barındırma planı kullanılırken işlev kodunuz ve bağlama yapılandırma dosyalarınız ana depolama hesabındaki Azure Dosya depolama alanında saklanır. Ana depolama hesabını sildiğinizde bu içerik silinir ve kurtarılamaz.

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Depolama Hizmetlerine Giriş](../storage/common/storage-introduction.md#azure-storage-services). 

## <a name="next-steps"></a>Sonraki adımlar

Azure portal, Işlevleri oluşturmayı ve denemeyi daha kolay hale getirir, ancak [Yerel geliştirmeyi](functions-develop-local.md)öneririz. Portalda bir işlev uygulaması oluşturduktan sonra yine de bir işlev eklemeniz gerekir. 

> [!div class="nextstepaction"]
> [HTTP ile tetiklenen bir işlev ekleme](functions-create-first-azure-function.md#create-function)
