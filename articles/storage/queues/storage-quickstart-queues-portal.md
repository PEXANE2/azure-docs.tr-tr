---
title: 'Quickstart: Portalda Azure Depolama kuyrukları oluşturma'
description: Kuyruk oluşturmak için Azure portalını kullanın. Ardından, ileti eklemek, ileti özelliklerini görüntülemek ve iletiyi sıradan çıkarmak için Azure portalını kullanın.
author: mhopkins-msft
ms.author: mhopkins
ms.date: 03/06/2019
ms.service: storage
ms.subservice: queues
ms.topic: quickstart
ms.reviewer: cbrooks
ms.openlocfilehash: 2876d2a680aa41372557a9f04c1d6a5eed56b8ae
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "74269240"
---
# <a name="quickstart-create-a-queue-and-add-a-message-with-the-azure-portal"></a>Hızlı başlangıç: Azure portalı ile kuyruk oluşturun ve ileti ekleyin

Bu hızlı başlangıçta, Azure Depolama'da kuyruk oluşturmak ve ileti eklemek ve sırayı çıkarmak için [Azure portalını](https://portal.azure.com/) nasıl kullanacağınızı öğrenirsiniz.

## <a name="prerequisites"></a>Ön koşullar

[!INCLUDE [storage-quickstart-prereq-include](../../../includes/storage-quickstart-prereq-include.md)]

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Azure portalında kuyruk oluşturmak için aşağıdaki adımları izleyin:

1. Azure portalında yeni depolama hesabınıza gidin.
2. Depolama hesabının sol menüsünde Sıra **hizmeti** bölümüne gidin ve **ardından Kuyruklar'ı**seçin.
3. + **Sıra** düğmesini seçin.
4. Yeni sıranız için bir ad yazın. Sıra adı küçük olmalı, bir harf veya sayıyla başlamalı ve yalnızca harfleri, sayıları ve tire (-) karakterini içermelidir.
6. Sırayı oluşturmak için **Tamam'ı** seçin.

    ![Azure portalında nasıl kuyruk oluşturulacak larını gösteren ekran görüntüsü](media/storage-quickstart-queues-portal/create-queue.png)

## <a name="add-a-message"></a>İleti ekleme

Ardından, yeni kuyruğa bir ileti ekleyin. Bir ileti nin boyutu 64 KB'ye kadar olabilir.

1. Depolama hesabındaki kuyruklar listesinden yeni sırayı seçin.
1. Sıraya ileti eklemek için **+ İleti ekle** düğmesini seçin. **İleti metin** alanına bir ileti girin. 
1. İletinin süresinin ne zaman dolduğunu belirtin. İletinin kuyrukta kalabileceğinin maksimum süresi 7 gündür.
1. İletiyi Base64 olarak kodlayıp kodlamayacağını belirtin. İkili verilerin kodlanması önerilir.
1. İletiyi eklemek için **Tamam** düğmesini seçin.

    ![Kuyruğa ileti eklemeyi gösteren ekran görüntüsü](media/storage-quickstart-queues-portal/add-message.png)

## <a name="view-message-properties"></a>İleti özelliklerini görüntüleme

Bir ileti ekledikten sonra, Azure portalı kuyruktaki tüm iletilerin listesini görüntüler. İleti kimliğini, iletinin içeriğini, ileti ekleme süresini ve iletinin son kullanma süresini görüntüleyebilirsiniz. Bu iletinin kaç kez sıradan silindiğini de görebilirsiniz.

![İleti özelliklerini gösteren ekran görüntüsü](media/storage-quickstart-queues-portal/view-message-properties.png)

## <a name="dequeue-a-message"></a>Bir iletiyi kuyruktan çıkarma

Azure portalından sıranın önünden gelen bir iletiyi dequeue'den çıkarabilirsiniz. Bir iletiyi sıradan ayırdığınızda, ileti silinir. 

Dequeueing her zaman kuyruktaki en eski iletiyi kaldırır. 

![Portaldan bir iletinin nasıl sıradan çıkarılabildiğini gösteren ekran görüntüsü](media/storage-quickstart-queues-portal/dequeue-message.png)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir sıra oluşturmayı, ileti eklemeyi, ileti özelliklerini görüntülemeyi ve Azure portalındaki bir iletiyi nasıl sıradan ayırabileceğinizi öğrendiniz.

> [!div class="nextstepaction"]
> [Azure Kuyrukları nedir?](storage-queues-introduction.md)
