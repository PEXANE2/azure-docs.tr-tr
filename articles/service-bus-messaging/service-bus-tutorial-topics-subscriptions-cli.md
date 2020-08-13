---
title: Azure CLı kullanarak Service Bus konular ve abonelikler oluşturma
description: Bu hızlı başlangıçta, Azure CLı kullanarak ilgili konuya bir Service Bus konu ve abonelik oluşturmayı öğreneceksiniz.
ms.date: 06/23/2020
ms.topic: quickstart
author: spelluru
ms.author: spelluru
ms.openlocfilehash: 3a6535a13ab00c4e22ac4cd8c2de5a5bbb02d0a8
ms.sourcegitcommit: 9ce0350a74a3d32f4a9459b414616ca1401b415a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88189802"
---
# <a name="use-azure-cli-to-create-a-service-bus-topic-and-subscriptions-to-the-topic"></a>Konuya bir Service Bus konu ve abonelik oluşturmak için Azure CLı kullanma
Bu hızlı başlangıçta, bir Service Bus konu oluşturmak ve ardından bu konuya abonelikler oluşturmak için Azure CLı 'yi kullanırsınız. 

## <a name="what-are-service-bus-topics-and-subscriptions"></a>Service Bus konuları ve abonelikleri nelerdir?
Service Bus konuları ve abonelikleri *publish/subscribe* mesajlaşma iletişim modelini destekler. Konular ve abonelikler kullanıldığında, dağıtılmış uygulamanın bileşenleri birbirleriyle doğrudan iletişim kurmazlar; bunun yerine bir aracı gibi davranan bir konu aracılığıyla iletileri değiş tokuş eder.

![TopicConcepts](./media/service-bus-java-how-to-use-topics-subscriptions/sb-topics-01.png)

Her iletinin tek bir tüketici tarafından işlendiği Service Bus kuyruklarının aksine, konular ve abonelikler, Yayımla/abone ol ' u kullanarak bire çok bir iletişim biçimi sağlar. Bir konuya birden fazla abonelik kaydedilebilir. Bir konuya ileti gönderildiğinde, bundan sonra, bağımsız olarak ele almak/işlemek amacıyla her abonelik için kullanılabilir hale getirilir. Bir konuya abone olunması, konuya gönderilmiş olan iletilerin kopyaların alan sanal kuyruğa benzer. İsteğe bağlı olarak, bir konu için filtre kurallarını abonelik başına temelinde kaydedebilirsiniz. Bu, hangi konu aboneliklerinin hangi konuyla ilgili olduğunu filtrelemenizi veya kısıtlamanıza olanak sağlar.

Service Bus konuları ve abonelikleri, çok sayıda kullanıcı ve uygulama genelinde çok sayıda iletiyi işlemek için ölçeklendirmenize olanak tanır.

## <a name="prerequisites"></a>Ön koşullar
Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap][free account] oluşturabilirsiniz.

Bu hızlı başlangıçta, Azure portal oturum açtıktan sonra başlatabilmeniz Azure Cloud Shell kullanırsınız. Azure Cloud Shell hakkındaki ayrıntılar için bkz. [Azure Cloud Shell Genel Bakış](../cloud-shell/overview.md). Ayrıca, makinenizde Azure PowerShell [yükleyip](/cli/azure/install-azure-cli) kullanabilirsiniz. 

## <a name="create-a-service-bus-topic-and-subscriptions"></a>Service Bus konu başlığı ve abonelikleri oluşturma
Her [konu başlığı aboneliği](service-bus-messaging-overview.md#topics) her iletinin bir kopyasını alabilir. Konular, protokol ve anlam açılarından Service Bus kuyrukları ile tam olarak uyumludur. Service Bus konu başlıkları, filtreleme koşullarını ve ileti özelliklerini belirleyen veya değiştiren isteğe bağlı eylemleri olan geniş bir seçim kuralı yelpazesini destekler. Bir kural eşleştiğinde bir ileti oluşturulur. Kurallar, filtreler ve eylemler hakkında daha fazla bilgi edinmek için bu [bağlantıyı](topic-filters.md) izleyin.

1. [Azure portal](https://portal.azure.com) oturum açın.
2. Aşağıdaki görüntüde gösterilen simgeyi seçerek Azure Cloud Shell başlatın. Cloud Shell **PowerShell** modundaysa **Bash** moduna geçin. 

    :::image type="content" source="./media/service-bus-quickstart-powershell/launch-cloud-shell.png" alt-text="Cloud Shell Başlat":::
3. Bir Azure Kaynak grubu oluşturmak için aşağıdaki komutu çalıştırın. İsterseniz kaynak grubu adını ve konumunu güncelleştirin. 

    ```azurecli-interactive
    az group create --name MyResourceGroup --location eastus
    ```
4. Service Bus mesajlaşma ad alanı oluşturmak için aşağıdaki komutu çalıştırın. Ad alanının adını benzersiz olacak şekilde güncelleştirin. 

    ```azurecli-interactive
    namespaceName=MyNameSpace$RANDOM
    az servicebus namespace create --resource-group MyResourceGroup --name $namespaceName --location eastus
    ```
5. Ad alanında bir konu oluşturmak için aşağıdaki komutu çalıştırın. 

    ```azurecli-interactive
    az servicebus topic create --resource-group MyResourceGroup   --namespace-name $namespaceName --name MyTopic
    ```
6. Konuya ilk aboneliği oluşturun
    
    ```azurecli-interactive
    az servicebus topic subscription create --resource-group MyResourceGroup --namespace-name $namespaceName --topic-name MyTopic --name S1    
    ```
6. İkinci aboneliği konuya oluşturun
    
    ```azurecli-interactive
    az servicebus topic subscription create --resource-group MyResourceGroup --namespace-name $namespaceName --topic-name MyTopic --name S2    
    ```
6. Konuya üçüncü abonelik oluşturun
    
    ```azurecli-interactive
    az servicebus topic subscription create --resource-group MyResourceGroup --namespace-name $namespaceName --topic-name MyTopic --name S3    
    ```
7. Özel özellikler kullanarak filtre içeren ilk abonelikte bir filtre oluşturun ( `StoreId` `Store1` , `Store2` ve, ve `Store3` ).

    ```azurecli-interactive
    az servicebus topic subscription rule create --resource-group MyResourceGroup --namespace-name $namespaceName --topic-name MyTopic --subscription-name S1 --name MyFilter --filter-sql-expression "StoreId IN ('Store1','Store2','Store3')"    
    ```
8. Müşteri özelliklerini kullanarak filtre ile ikinci abonelikte filtre oluşturma ( `StoreId = Store4` )

    ```azurecli-interactive
    az servicebus topic subscription rule create --resource-group MyResourceGroup --namespace-name $namespaceName --topic-name myTopic --subscription-name S2 --name MySecondFilter --filter-sql-expression "StoreId = 'Store4'"    
    ```
9. Müşteri özelliklerini kullanarak filtre ile üçüncü abonelikte filtre oluşturun (,,, `StoreId` `Store1` `Store2` `Store3` veya `Store4` ).

    ```azurecli-interactive
    az servicebus topic subscription rule create --resource-group MyResourceGroup --namespace-name $namespaceName --topic-name MyTopic --subscription-name S3 --name MyThirdFilter --filter-sql-expression "StoreId IN ('Store1','Store2','Store3', 'Store4')"     
    ```
10. Ad alanı için birincil bağlantı dizesini almak için aşağıdaki komutu çalıştırın. Bu bağlantı dizesini kuyruğa bağlanmak ve ileti göndermek ve almak için kullanırsınız. 

    ```azurecli-interactive
    az servicebus namespace authorization-rule keys list --resource-group MyResourceGroup --namespace-name $namespaceName --name RootManageSharedAccessKey --query primaryConnectionString --output tsv    
    ```

    Bağlantı dizesini ve konu adını göz önünde edin. Bunları ileti göndermek ve almak için kullanırsınız. 
    

## <a name="next-steps"></a>Sonraki adımlar
Bir konuya ileti gönderme ve bu iletileri bir abonelik aracılığıyla alma hakkında bilgi edinmek için aşağıdaki makaleye bakın: TOC 'de programlama dilini seçin. 

> [!div class="nextstepaction"]
> [İleti yayımlama ve iletilere abone olma](service-bus-dotnet-how-to-use-topics-subscriptions.md)


[free account]: https://azure.microsoft.com/free/?ref=microsoft.com&utm_source=microsoft.com&utm_medium=docs&utm_campaign=visualstudio
[fully qualified domain name]: https://wikipedia.org/wiki/Fully_qualified_domain_name
[Install the Azure CLI]: /cli/azure/install-azure-cli
[az group create]: /cli/azure/group#az_group_create