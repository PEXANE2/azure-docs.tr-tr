---
title: Azure şablonu kullanarak Azure Hizmet Veri Servisi ad alanı ve kuyruk oluşturma
description: 'Hızlı başlangıç: Azure Kaynak Yöneticisi şablonu kullanarak Hizmet Veri Servisi ad alanı ve kuyruk oluşturma'
services: service-bus-messaging
documentationcenter: .net
author: spelluru
manager: timlt
editor: ''
ms.assetid: a6bfb5fd-7b98-4588-8aa1-9d5f91b599b6
ms.service: service-bus-messaging
ms.devlang: tbd
ms.topic: quickstart
ms.custom: subject-armqs
ms.tgt_pltfrm: dotnet
ms.workload: na
ms.date: 03/30/2020
ms.author: spelluru
ms.openlocfilehash: b08253104eeb61f6bb09fde507473d235a996494
ms.sourcegitcommit: 7581df526837b1484de136cf6ae1560c21bf7e73
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80422653"
---
# <a name="quickstart-create-a-service-bus-namespace-and-a-queue-using-an-azure-resource-manager-template"></a>Hızlı başlangıç: Azure Kaynak Yöneticisi şablonu kullanarak Hizmet Veri Servisi ad alanı ve kuyruk oluşturma

Bu makalede, Hizmet Veri Servisi ad alanı ve bu ad alanı içinde bir kuyruk oluşturan bir Azure Kaynak Yöneticisi şablonu nasıl kullanılacağı gösterilmektedir. Makalede, hangi kaynakların dağıtıldığı ve dağıtım yürütüldüğünde belirtilen parametrelerin nasıl tanımlanılığı açıklanmaktadır. Bu şablonu kendi dağıtımlarınız için kullanabilir veya kendi gereksinimlerinize göre özelleştirebilirsiniz.

[!INCLUDE [About Azure Resource Manager](../../includes/resource-manager-quickstart-introduction.md)]

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz bir hesap oluşturun.](https://azure.microsoft.com/free/)

## <a name="prerequisites"></a>Ön koşullar

None

## <a name="create-a-service-bus-namespace-and-a-queue"></a>Hizmet Veri Günü ad alanı ve sıra oluşturma

### <a name="review-the-template"></a>Şablonu gözden geçirme

Bu hızlı başlatmada kullanılan şablon [Azure Quickstart şablonlarındandır.](https://azure.microsoft.com/resources/templates/201-servicebus-create-queue)

:::code language="json" source="~/quickstart-templates/201-servicebus-create-queue/azuredeploy.json" range="1-75" highlight="31-63":::

Şablonda tanımlanan kaynaklar şunlardır:

- [**Microsoft.ServiceBus/namespaces**](/azure/templates/microsoft.servicebus/namespaces)
- [**Microsoft.ServiceBus/namespaces/queues**](/azure/templates/microsoft.servicebus/namespaces/queues)

> [!NOTE]
> Aşağıdaki Azure Kaynak Yöneticisi şablonları karşıdan yükleme ve dağıtım için kullanılabilir.
>
> * [Sıra ve yetkilendirme kuralı yla Hizmet Veri Servisi ad alanı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)
> * [Konu ve abonelikle birlikte Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic.md)
> * [Service Bus ad alanı oluşturma](service-bus-resource-manager-namespace.md)
> * [Konu, abonelik ve kural içeren bir Hizmet Veri Günü ad alanı oluşturma](service-bus-resource-manager-namespace-topic-with-rule.md)

[Azure Quickstart şablonlarından](https://azure.microsoft.com/resources/templates/?resourceType=Microsoft.Servicebus&pageNumber=1&sort=Popular) daha fazla şablon bulabilirsiniz

### <a name="deploy-the-template"></a>Şablonu dağıtma

Bu şablonla, sıralı bir Hizmet Veri Günü ad alanı dağıtırsınız.

[Servis Veri Hizmetleri kuyrukları,](service-bus-queues-topics-subscriptions.md#queues) bir veya daha fazla rakip tüketiciye First In, First Out (FIFO) ileti teslimi sunar.

Dağıtımı otomatik olarak çalıştırmak için aşağıdaki düğmeyi tıklatın: Dağıtım için yeni bir kaynak grubu oluşturun, böylece daha sonra kolayca temizleyebilirsiniz.

[![Azure’a dağıtma](./media/service-bus-resource-manager-namespace-queue/deploybutton.png)](https://portal.azure.com/#create/Microsoft.Template/uri/https%3A%2F%2Fraw.githubusercontent.com%2FAzure%2Fazure-quickstart-templates%2Fmaster%2F201-servicebus-create-queue%2Fazuredeploy.json)

## <a name="verify-the-deployment"></a>Dağıtımı doğrulama

1. Dağıtım durumunu görmek için en üstteki **Bildirimler'i** seçin. Dağıtım başarılı olana kadar bekleyin. Ardından, Hizmet Veri Gönderi ad alanını içeren kaynak grubunun sayfasına gitmek için bildirim iletisindeki **kaynak grubuna git'i** seçin. 

    ![Dağıtımdan gelen bildirim](./media/service-bus-resource-manager-namespace-queue/notification.png)
2. Hizmet Veri Günü ad alanınızı kaynaklar listesinde gördüğünüzü doğrulayın. 

    ![Kaynak grubu - ad alanı](./media/service-bus-resource-manager-namespace-queue/resource-group-namespace.png)
3. **Hizmet Veri Aracı Ad Alanı** sayfasını görmek için listeden ad alanını seçin. 

## <a name="cleanup-resources"></a>Kaynakları temizleme

1. Azure portalında, kaynak grubunuz için **Kaynak grubu** sayfasına gidin.
2. Araç çubuğundan **Kaynak grubunu sil**’i seçin. 
3. Kaynak grubunun adını yazın ve **Sil'i**seçin. 

    ![Kaynak grubu - silme](./media/service-bus-resource-manager-namespace-queue/resource-group-delete.png)

## <a name="next-steps"></a>Sonraki adımlar

Ad alanı/sıra için yetkilendirme kuralının nasıl oluşturultuğa ilişkin aşağıdaki konuya bakın:

[Azure Kaynak Yöneticisi şablonu kullanarak ad alanı ve sıra için Hizmet Veri Çiş yetkilendirme kuralı oluşturma](service-bus-resource-manager-namespace-auth-rule.md)

Bu makaleleri görüntüleyerek bu kaynakları nasıl yöneteceklerini öğrenin:

* [Service Bus’ı PowerShell ile yönetme](service-bus-manage-with-ps.md)
* [Servis Veri Servisi Gezgini ile Servis Veri Servisi kaynaklarını yönetme](https://github.com/paolosalvatori/ServiceBusExplorer/releases)

[Authoring Azure Resource Manager templates]: ../azure-resource-manager/templates/template-syntax.md
[Service Bus namespace and queue template]: https://github.com/Azure/azure-quickstart-templates/blob/master/201-servicebus-create-queue/
[Azure Quickstart Templates]: https://azure.microsoft.com/documentation/templates/?term=service+bus
[Learn more about Service Bus queues]: service-bus-queues-topics-subscriptions.md
[Using Azure PowerShell with Azure Resource Manager]: ../azure-resource-manager/powershell-azure-resource-manager.md
[Using the Azure CLI for Mac, Linux, and Windows with Azure Resource Management]: ../azure-resource-manager/xplat-cli-azure-resource-manager.md
