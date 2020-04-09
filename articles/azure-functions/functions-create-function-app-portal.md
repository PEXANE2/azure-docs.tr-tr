---
title: Azure portalından işlev uygulaması oluşturma
description: Portaldan Azure'da yeni bir işlev uygulaması oluşturun.
ms.topic: how-to
ms.date: 08/29/2019
ms.custom: mvc
ms.openlocfilehash: 001b4e4f0ea7fbacd232b2a87abfe353f34919bd
ms.sourcegitcommit: 7d8158fcdcc25107dfda98a355bf4ee6343c0f5c
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/09/2020
ms.locfileid: "80985023"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Azure portalından işlev uygulaması oluşturma

Bu konu, Azure portalında bir işlev uygulaması oluşturmak için Azure İşlevlerini nasıl kullanacağınızı gösterir. İşlev uygulaması, tek tek işlevlerin yürütülmesini barındıran kapsayıcıdır. 

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

İşlev uygulaması oluşturulduktan sonra bir veya daha fazla dilde tek tek işlevler oluşturabilirsiniz. [Portalı kullanarak](functions-create-first-azure-function.md#create-function), [sürekli dağıtım](functions-continuous-deployment.md) aracılığıyla veya [FTP ile karşıya yükleyerek](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp) çeşitli işlevler oluşturun.

## <a name="service-plans"></a>Hizmet planları

Azure İşlevleri'nin üç farklı hizmet planı vardır: Tüketim planı, Premium plan ve Özel (App Service) planı. İşlev uygulamanız oluşturulduğunda hizmet planınızı seçmeniz gerekir ve bu plan daha sonra değiştirilemez. Daha fazla bilgi edinmek için bkz. [Azure İşlevleri barındırma planı seçme](functions-scale.md).

JavaScript işlevlerini Özel (App Service) planında çalıştırmayı planlıyorsanız, daha az çekirdekiçeren bir plan seçmeniz gerekir. Daha fazla bilgi edinmek için bkz. [İşlevler için JavaScript başvurusu](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Bir işlev uygulaması oluştururken, Blob, Queue ve Tablo depolama alanını destekleyen genel amaçlı bir Azure Depolama hesabı oluşturmanız veya bağlantı kurmanız gerekir. Dahili olarak İşlevler tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Depolama’yı kullanır. Yalnızca blob depolama hesapları, Azure Premium Depolama ve ZRS çoğaltmalı genel amaçlı depolama hesapları gibi bazı depolama hesapları kuyrukları ve tabloları desteklemez. Bir işlev uygulaması oluşturulurken bu hesaplar Depolama Hesabı dikey penceresinden filtrelenir.

>[!NOTE]
>Tüketim barındırma planı kullanılırken işlev kodunuz ve bağlama yapılandırma dosyalarınız ana depolama hesabındaki Azure Dosya depolama alanında saklanır. Ana depolama hesabını sildiğinizde bu içerik silinir ve kurtarılamaz.

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Depolama Hizmetlerine Giriş](../storage/common/storage-introduction.md#core-storage-services). 

## <a name="next-steps"></a>Sonraki adımlar

Azure portalı İşlevler oluşturmayı ve denemeyi kolaylaştırırken, [yerel geliştirmeyi](functions-develop-local.md)öneririz. Portalda bir işlev uygulaması oluşturduktan sonra, yine de bir işlev eklemeniz gerekir. 

> [!div class="nextstepaction"]
> [HTTP tetiklenen işlev ekleme](functions-create-first-azure-function.md#create-function)
