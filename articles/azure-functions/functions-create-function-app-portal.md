---
title: Azure portalından işlev uygulaması oluşturma
description: Portaldan Azure 'da yeni bir işlev uygulaması oluşturun.
ms.topic: conceptual
ms.date: 08/29/2019
ms.custom: mvc
ms.openlocfilehash: 086a543e75d083094d4dfa789e71afaba5da6d6e
ms.sourcegitcommit: 512d4d56660f37d5d4c896b2e9666ddcdbaf0c35
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/14/2020
ms.locfileid: "79368800"
---
# <a name="create-a-function-app-from-the-azure-portal"></a>Azure portalından işlev uygulaması oluşturma

Bu konuda, Azure portal bir işlev uygulaması oluşturmak için Azure Işlevlerinin nasıl kullanılacağı gösterilmektedir. İşlev uygulaması, tek tek işlevlerin yürütülmesini barındıran kapsayıcıdır. 

## <a name="create-a-function-app"></a>İşlev uygulaması oluşturma

[!INCLUDE [functions-create-function-app-portal](../../includes/functions-create-function-app-portal.md)]

İşlev uygulaması oluşturulduktan sonra bir veya daha fazla dilde tek tek işlevler oluşturabilirsiniz. [Portalı kullanarak](functions-create-first-azure-function.md#create-function), [sürekli dağıtım](functions-continuous-deployment.md) aracılığıyla veya [FTP ile karşıya yükleyerek](https://github.com/projectkudu/kudu/wiki/Accessing-files-via-ftp) çeşitli işlevler oluşturun.

## <a name="service-plans"></a>Hizmet planları

Azure Işlevleri üç farklı hizmet planına sahiptir: tüketim planı, Premium plan ve adanmış (App Service) plan. İşlev uygulamanız oluşturulduğunda hizmet planınızı seçmeniz gerekir ve daha sonra değiştirilemez. Daha fazla bilgi edinmek için bkz. [Azure İşlevleri barındırma planı seçme](functions-scale.md).

JavaScript işlevlerini adanmış (App Service) bir planda çalıştırmayı planlıyorsanız, daha az çekirdeğe sahip bir plan seçmelisiniz. Daha fazla bilgi edinmek için bkz. [İşlevler için JavaScript başvurusu](functions-reference-node.md#choose-single-vcpu-app-service-plans).

<a name="storage-account-requirements"></a>

## <a name="storage-account-requirements"></a>Depolama hesabı gereksinimleri

Bir işlev uygulaması oluştururken blob, kuyruk ve tablo depolamayı destekleyen genel amaçlı bir Azure depolama hesabı oluşturmanız veya bağlamanız gerekir. Dahili olarak İşlevler tetikleyicileri yönetme ve işlev yürütmelerini günlüğe kaydetme gibi işlemler için Depolama’yı kullanır. Yalnızca blob depolama hesapları, Azure Premium Depolama ve ZRS çoğaltmalı genel amaçlı depolama hesapları gibi bazı depolama hesapları kuyrukları ve tabloları desteklemez. Bir işlev uygulaması oluşturulurken bu hesaplar Depolama Hesabı dikey penceresinden filtrelenir.

>[!NOTE]
>Tüketim barındırma planı kullanılırken işlev kodunuz ve bağlama yapılandırma dosyalarınız ana depolama hesabındaki Azure Dosya depolama alanında saklanır. Ana depolama hesabını sildiğinizde bu içerik silinir ve kurtarılamaz.

Depolama hesabı türleri hakkında daha fazla bilgi edinmek için bkz. [Azure Depolama Hizmetlerine Giriş](../storage/common/storage-introduction.md#azure-storage-services). 

## <a name="next-steps"></a>Sonraki adımlar

Azure portal, Işlevleri oluşturmayı ve denemeyi daha kolay hale getirir, ancak [Yerel geliştirmeyi](functions-develop-local.md)öneririz. Portalda bir işlev uygulaması oluşturduktan sonra yine de bir işlev eklemeniz gerekir. 

> [!div class="nextstepaction"]
> [HTTP ile tetiklenen bir işlev ekleme](functions-create-first-azure-function.md#create-function)
