---
title: Azure Işlevlerini bir sanal ağla bütünleştirmek için özel uç noktaları kullanın
description: Bir işlevi Azure sanal ağına bağlamayı ve özel uç noktalarla kilitlemeyi gösteren adım adım öğretici
ms.topic: article
ms.date: 2/22/2021
ms.openlocfilehash: a7bad58167009b4089724165813eb061996f1e6b
ms.sourcegitcommit: dda0d51d3d0e34d07faf231033d744ca4f2bbf4a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/05/2021
ms.locfileid: "102200215"
---
# <a name="tutorial-integrate-azure-functions-with-an-azure-virtual-network-using-private-endpoints"></a>Öğretici: özel uç noktaları kullanarak Azure Işlevlerini Azure sanal ağıyla tümleştirme

Bu öğreticide, Azure Işlevleri 'ni kullanarak özel uç noktalarıyla bir Azure sanal ağındaki kaynaklara nasıl bağlanabilmeniz gösterilmektedir. Service Bus kuyruğu tetikleyicisi kullanan bir sanal ağın arkasında kilitli bir depolama hesabıyla bir işlev oluşturacaksınız.

> [!div class="checklist"]
> * Premium planda bir işlev uygulaması oluşturma
> * Azure kaynakları (Service Bus, depolama hesabı, sanal ağ) oluşturma
> * Özel bir uç noktanın arkasında depolama hesabınızı kilitleme
> * Özel bir uç noktanın arkasında Service Bus kilitleme
> * Hem Service Bus hem de HTTP tetikleyicilerine sahip bir işlev uygulaması dağıtın.
> * İşlev uygulamanızı özel bir uç noktanın arkasında kilitleme
> * İşlev uygulamanızın sanal ağın arkasında güvenli olduğunu görmek için test edin
> * Kaynakları temizleme

## <a name="create-a-function-app-in-a-premium-plan"></a>Premium planda bir işlev uygulaması oluşturma

İlk olarak, bu öğretici C# kullanacağı için [Premium planda] bir .NET işlevi uygulaması oluşturursunuz. Diğer diller de Windows 'da desteklenmektedir. Bu plan, sanal ağ tümleştirmesini desteklerken sunucusuz ölçek sağlar.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**'u seçin.

1. **Yeni** sayfasında, **işlem**  >  **işlev uygulaması**' yi seçin.

1. **Temel bilgiler** sayfasında, aşağıdaki tabloda belirtilen işlev uygulaması ayarlarını kullanın:

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **Abonelik** | Aboneliğiniz | Bu yeni işlev uygulamasının oluşturulduğu abonelik. |
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)** |  *myResourceGroup* | İşlev uygulamanızın oluşturulacağı yeni kaynak grubunun adı. |
    | **İşlev Uygulamasının adı** | Genel olarak benzersiz bir ad | Yeni işlev uygulamanızı tanımlayan ad. Geçerli karakterler şunlardır: `a-z` (büyük/küçük harf duyarsız), `0-9` ve `-`.  |
    |**Yayımla**| Kod | Kod dosyalarını veya Docker kapsayıcısını yayımlama seçeneği. |
    | **Çalışma zamanı yığını** | .NET | Bu öğretici .NET kullanır |
    |**Bölge**| Tercih edilen bölge | Size yakın bir bölge seçin ve işlevlerinizin erişebileceği diğer hizmetlere yakın bir [bölge](https://azure.microsoft.com/regions/) seçin. |

1. Ileri 'yi seçin **: barındırma**. **Barındırma** sayfasında, aşağıdaki ayarları girin:

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Depolama hesabı](../storage/common/storage-account-create.md)** |  Genel olarak benzersiz bir ad |  İşlev uygulamanız tarafından kullanılan bir depolama hesabı oluşturun. Depolama hesabı adları 3 ile 24 karakter arasında olmalı ve yalnızca sayıyla küçük harf içermelidir. Ayrıca, [depolama hesabı gereksinimlerini](./storage-considerations.md#storage-account-requirements)karşılaması gereken mevcut bir hesabı da kullanabilirsiniz. |
    |**İşletim sistemi**| Windows | Bu öğretici Windows kullanır |
    | **[Planlama](./functions-scale.md)** | Premium | Kaynakların işlev uygulamanıza nasıl ayrılacağını tanımlayan barındırma planı. **Premium**' u seçin. Varsayılan olarak, yeni bir App Service planı oluşturulur. Varsayılan **SKU ve boyut** **EP1**, burada EP, _elastik Premium_ için temsil eder. Daha fazla bilgi edinmek için [Premium SKU 'ların listesine](./functions-premium-plan.md#available-instance-skus)bakın.<br/>Premium bir planda JavaScript işlevlerini çalıştırırken, daha az vCPU içeren bir örnek seçmeniz gerekir. Daha fazla bilgi için bkz. [tek çekirdekli Premium planları seçme](./functions-reference-node.md#considerations-for-javascript-functions).  |

1. **İleri: izleme** öğesini seçin. **İzleme** sayfasında, aşağıdaki ayarları girin:

    | Ayar      | Önerilen değer  | Açıklama |
    | ------------ | ---------------- | ----------- |
    | **[Application Insights](./functions-monitoring.md)** | Varsayılan | En yakın desteklenen bölgede aynı *uygulama adının* Application Insights kaynağını oluşturur. Bu ayarı genişleterek, verilerinizi depolamak için **Yeni kaynak adını** değiştirebilir veya [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/) 'da farklı bir **konum** seçebilirsiniz. |

1. Uygulama yapılandırma seçimlerini gözden geçirmek için **gözden geçir + oluştur** ' u seçin.

1. **Gözden geçir + oluştur** sayfasında ayarlarınızı gözden geçirin ve sonra işlev uygulamasını sağlamak ve dağıtmak için **Oluştur** ' u seçin.

1. Portalın sağ üst köşesindeki **Bildirimler** simgesini seçin ve **dağıtım başarılı** iletisini izleyin.

1. Yeni işlev uygulamanızı görüntülemek için **Kaynağa git**’i seçin. **Panoya sabitle ' yi** de seçebilirsiniz. Sabitleme, panonuzdan bu işlev uygulama kaynağına döndürülmesini kolaylaştırır.

1. Tebrikler! Premium işlev uygulamanızı başarıyla oluşturdunuz!

## <a name="create-azure-resources"></a>Azure kaynakları oluşturma

### <a name="create-a-storage-account"></a>Depolama hesabı oluşturma

Sanal ağlar için, işlev uygulamanızın ilk oluşturulmasında oluşturulmuş olan ayrı bir depolama hesabı gereklidir.

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**'u seçin.

1. Yeni sayfada **depolama hesabı** araması yapın ve **Oluştur** ' u seçin.

1. **Temel bilgiler** sekmesinde, ayarları aşağıdaki tabloda belirtilen şekilde ayarlayın. Rest varsayılan olarak bırakılabilir:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)**  | myResourceGroup | İşlev uygulamanız ile oluşturduğunuz kaynak grubunu seçin. |
    | **Ad** | mysecurestorage| Özel uç noktanın uygulanacağı depolama hesabınızın adı. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Üzerinde işlev uygulamanızı oluşturduğunuz bölgeyi seçin. |

1. **Gözden geçir ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin.

### <a name="create-a-service-bus"></a>Service Bus oluşturma

1. Azure portalı menüsünde veya **Giriş** sayfasında **Kaynak oluştur**'u seçin.

1. Yeni sayfada **Service Bus** araması yapın ve **Oluştur**' u seçin.

1. **Temel bilgiler** sekmesinde, ayarları aşağıdaki tabloda belirtilen şekilde ayarlayın. Rest varsayılan olarak bırakılabilir:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. |
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)**  | myResourceGroup | İşlev uygulamanız ile oluşturduğunuz kaynak grubunu seçin. |
    | **Ad** | myServiceBus| Özel uç noktanın uygulanacağı hizmet veri yolu adı. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Üzerinde işlev uygulamanızı oluşturduğunuz bölgeyi seçin. |
    | **Fiyatlandırma katmanı** | Premium | Service Bus ile özel uç noktaları kullanmak için bu katmanı seçin. |

1. **Gözden geçir ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin.

### <a name="create-a-virtual-network"></a>Sanal ağ oluşturma

Bu öğreticideki Azure kaynakları bir sanal ağ ile tümleştirin veya bir sanal ağ içine yerleştirilir. Ağ trafiğinin sanal ağ ile birlikte tutulması için özel uç noktaları kullanacaksınız.

Öğretici iki alt ağ oluşturur:
- **varsayılan**: özel uç noktalar için alt ağ. Özel IP adresleri bu alt ağdan verilir.
- **işlevler**: Azure işlevleri sanal ağ tümleştirmesi için alt ağ. Bu alt ağ, işlev uygulamasına atanmış.

Şimdi, işlev uygulamasının tümleştirildiği sanal ağı oluşturun.

1. Azure portalı menüsünde veya Giriş sayfasında **Kaynak oluştur**'u seçin.

1. Yeni sayfada, **sanal ağ** araması yapın ve **Oluştur**' u seçin.

1. **Temel bilgiler** sekmesinde, sanal ağ ayarlarını aşağıda belirtilen şekilde kullanın:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)**  | myResourceGroup | İşlev uygulamanız ile oluşturduğunuz kaynak grubunu seçin. |
    | **Ad** | myVirtualNet| İşlev uygulamanızın bağlanacağı sanal ağınızın adı. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | Üzerinde işlev uygulamanızı oluşturduğunuz bölgeyi seçin. |

1. **IP adresleri** sekmesinde **alt ağ ekle**' yi seçin. Bir alt ağ eklenirken aşağıda belirtilen ayarları kullanın:

    :::image type="content" source="./media/functions-create-vnet/1-create-vnet-ip-address.png" alt-text="Sanal ağ yapılandırması oluşturma görünümünün ekran görüntüsü.":::

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Alt ağ adı** |  işlevleri | İşlev uygulamanızın bağlanacağı alt ağın adı. | 
    | **Alt ağ adres aralığı** | 10.0.1.0/24 | Yukarıdaki görüntüdeki IPv4 adres alanım 10.0.0.0/16 ' dır. Yukarıdaki 10.1.0.0/16 ise önerilen *alt ağ adres aralığı* 10.1.1.0/24 olur. |

1. **Gözden geçir ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin.

## <a name="lock-down-your-storage-account-with-private-endpoints"></a>Özel uç noktalarla depolama hesabınızı kilitleme

Azure özel uç noktaları, belirli Azure kaynaklarına özel bir IP adresi kullanarak bağlanmak için kullanılır. Bu bağlantı, ağ trafiğinin seçili sanal ağ içinde kalmasını ve erişimin yalnızca belirli kaynaklar için kullanılabilir olmasını sağlar. Şimdi Azure dosya depolama ve Azure Blob depolama için özel uç noktaları depolama hesabınızla oluşturun.

1. Yeni depolama hesabınızda, sol taraftaki menüden **ağ** ' ı seçin.

1. **Özel uç nokta bağlantıları** sekmesini seçin ve **Özel uç nokta**' ı seçin.

    :::image type="content" source="./media/functions-create-vnet/2-navigate-private-endpoint-store.png" alt-text="Depolama hesabı için özel uç noktalar oluşturmak üzere gidilecek ekran görüntüsü.":::

1. **Temel bilgiler** sekmesinde, aşağıda belirtildiği gibi özel uç nokta ayarlarını kullanın:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)**  | myResourceGroup | İşlev uygulamanız ile oluşturduğunuz kaynak grubunu seçin. | |
    | **Ad** | dosya-uç noktası | Depolama hesabınızdaki dosyaların özel uç noktasının adı. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | İçinde depolama hesabınızı oluşturduğunuz bölgeyi seçin. |

1. **Kaynak** sekmesinde, aşağıda belirtildiği gibi özel uç nokta ayarlarını kullanın:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **Kaynak türü**  | Microsoft. Storage/storageAccounts | Bu, depolama hesaplarının kaynak türüdür. |
    | **Kaynak** | mysecurestorage | Yeni oluşturduğunuz depolama hesabı |
    | **Hedef alt kaynak** |  dosyası | Bu özel uç nokta, depolama hesabındaki dosyalar için kullanılacaktır. |

1. **Yapılandırma** sekmesinde, alt ağ ayarı için **varsayılan** ' ı seçin.

1. **Gözden geçir ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin. Sanal ağdaki kaynaklar artık depolama dosyalarıyla iletişim kurabilir.

1. Bloblar için başka bir özel uç nokta oluşturun. **Kaynaklar** sekmesi için aşağıdaki ayarları kullanın. Diğer tüm ayarlar için, yeni izlediğiniz dosya özel uç nokta oluşturma adımlarından aynı ayarları kullanın.

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **Kaynak türü**  | Microsoft. Storage/storageAccounts | Bu, depolama hesaplarının kaynak türüdür. |
    | **Kaynak** | mysecurestorage | Yeni oluşturduğunuz depolama hesabı |
    | **Hedef alt kaynak** | blob | Bu özel uç nokta, depolama hesabındaki Bloblar için kullanılacaktır. |

## <a name="lock-down-your-service-bus-with-a-private-endpoint"></a>Service Bus 'ı özel bir uç nokta ile kilitleme

Şimdi Azure Service Bus için özel uç nokta oluşturun.

1. Yeni hizmet veri yolunda sol menüden **ağ** ' ı seçin.

1. **Özel uç nokta bağlantıları** sekmesini seçin ve **Özel uç nokta**' ı seçin.

    :::image type="content" source="./media/functions-create-vnet/3-navigate-private-endpoint-service-bus.png" alt-text="Service Bus için özel uç noktalara gitme ekran görüntüsü.":::

1. **Temel bilgiler** sekmesinde, aşağıda belirtildiği gibi özel uç nokta ayarlarını kullanın:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **[Kaynak grubu](../azure-resource-manager/management/overview.md)**  | myResourceGroup | İşlev uygulamanız ile oluşturduğunuz kaynak grubunu seçin. |
    | **Ad** | SB-uç nokta | Depolama hesabınızdaki dosyaların özel uç noktasının adı. |
    | **[Region](https://azure.microsoft.com/regions/)** | myFunctionRegion | İçinde depolama hesabınızı oluşturduğunuz bölgeyi seçin. |

1. **Kaynak** sekmesinde, aşağıda belirtildiği gibi özel uç nokta ayarlarını kullanın:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Abonelik** | Aboneliğiniz | Kaynaklarınızın oluşturulduğu abonelik. | 
    | **Kaynak türü**  | Microsoft. ServiceBus/ad alanları | Bu, Service Bus için kaynak türüdür. |
    | **Kaynak** | myServiceBus | Öğreticide daha önce oluşturduğunuz Service Bus. |
    | **Hedef alt kaynak** | ad alanı | Bu özel uç nokta, hizmet veri yolundan ad alanı için kullanılacaktır. |

1. **Yapılandırma** sekmesinde, alt ağ ayarı için **varsayılan** ' ı seçin.

1. **Gözden geçir ve oluştur**’u seçin. Doğrulama tamamlandıktan sonra **Oluştur**' u seçin. Sanal ağdaki kaynaklar artık Service Bus ile konuşabilir.

## <a name="create-a-file-share"></a>Dosya paylaşımı oluşturma

1. Oluşturduğunuz depolama hesabında, sol taraftaki menüde **dosya paylaşımları** ' nı seçin.

1. **+ Dosya paylaşımları**' nı seçin. Bu öğreticinin amaçları doğrultusunda **dosyaları** dosya paylaşımının adı olarak belirtin.

    :::image type="content" source="./media/functions-create-vnet/4-create-file-share.png" alt-text="Depolama hesabında bir dosya paylaşımının nasıl oluşturulacağı ekran görüntüsü.":::

## <a name="get-storage-account-connection-string"></a>Depolama hesabı bağlantı dizesi al

1. Oluşturduğunuz depolama hesabında, Sol menüdeki **erişim tuşları** ' nı seçin.

1. **Anahtarları göster**' i seçin. KEY1 bağlantı dizesini kopyalayın ve kaydedin. Uygulama ayarlarını yapılandırırken bu bağlantı dizesine daha sonra ihtiyacımız olacak.

    :::image type="content" source="./media/functions-create-vnet/5-get-store-connection-string.png" alt-text="Depolama hesabı bağlantı dizesinin nasıl alınacağını gösteren ekran görüntüsü.":::

## <a name="create-a-queue"></a>Bir kuyruk oluşturma

Bu, Azure Işlevlerinizin Service Bus tetikleyicisinin olayları aldığı sıra olacaktır.

1. Service Bus 'da sol taraftaki menüden **Kuyruklar** ' ı seçin.

1. **Paylaşılan erişim ilkeleri**' ni seçin. Bu öğreticinin amaçları **doğrultusunda kuyruğu kuyruğun adı olarak belirtin** .

    :::image type="content" source="./media/functions-create-vnet/6-create-queue.png" alt-text="Service Bus kuyruğu oluşturma ekranının ekran görüntüsü.":::

## <a name="get-service-bus-connection-string"></a>Service Bus bağlantı dizesi al

1. Service Bus 'da, sol taraftaki menüden **paylaşılan erişim ilkeleri** ' ni seçin.

1. **RootManageSharedAccessKey** öğesini seçin. **Birincil bağlantı dizesini** kopyalayın ve kaydedin. Uygulama ayarlarını yapılandırırken bu bağlantı dizesine daha sonra ihtiyacımız olacak.

    :::image type="content" source="./media/functions-create-vnet/7-get-service-bus-connection-string.png" alt-text="Service Bus bağlantı dizesinin nasıl alınacağını gösteren ekran görüntüsü.":::

## <a name="integrate-function-app-with-your-virtual-network"></a>İşlev uygulamasını sanal ağınızla tümleştirin

İşlev uygulamanızı sanal ağlarla birlikte kullanmak için bir alt ağa katılmanız gerekir. Azure Işlevleri sanal ağ tümleştirmesi ve bu öğreticide oluşturulan diğer tüm özel uç noktalar için varsayılan alt ağ için belirli bir alt ağ kullanıyoruz.

1. İşlev uygulamanızda, sol taraftaki menüden **ağ** ' ı seçin.

1. VNet tümleştirmesi altında **yapılandırmak için buraya tıklayın ' ı** seçin.

    :::image type="content" source="./media/functions-create-vnet/8-connect-app-vnet.png" alt-text="Sanal ağ tümleştirmesine nasıl gidebileceğiniz ekran görüntüsü.":::

1. **VNET Ekle** 'yi seçin

1. **Sanal ağ** altında açılan dikey pencerede, daha önce oluşturduğunuz sanal ağı seçin.

1. Daha önce oluşturulan **işlevler** olarak adlandırılan **alt ağı** seçin. İşlev uygulamanız artık sanal ağınızla tümleşiktir!

    :::image type="content" source="./media/functions-create-vnet/9-connect-app-subnet.png" alt-text="Bir işlev uygulamasının bir alt ağa nasıl bağlanacağını gösteren ekran görüntüsü.":::

## <a name="configure-your-function-app-settings-for-private-endpoints"></a>İşlev uygulaması ayarlarınızı özel uç noktalar için yapılandırma

1. İşlev uygulamanızda, sol menüden **yapılandırma** ' yı seçin.

1. İşlev uygulamanızı sanal ağlarla birlikte kullanmak için aşağıdaki uygulama ayarlarının güncellenmesi gerekir. Uygun şekilde, uygulama ayarları tablosunun en sağdaki sütunundaki **+ Yeni uygulama ayarı** **' nı** veya kurşun kalem ' i seçin. İşiniz bittiğinde **Kaydet**’i seçin.

    :::image type="content" source="./media/functions-create-vnet/10-configure-app-settings.png" alt-text="Özel uç noktalar için işlev uygulama ayarlarının nasıl yapılandırılacağı ekran görüntüsü.":::

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **AzureWebJobsStorage** | mysecurestorageConnectionString | Oluşturduğunuz depolama hesabının bağlantı dizesi. Bu, [depolama hesabı bağlantı dizesi al](#get-storage-account-connection-string)' dan alınan depolama bağlantı dizesidir. Bu ayar değiştirilerek, işlev uygulamanız artık çalışma zamanında normal işlemler için güvenli depolama hesabı 'nı kullanacaktır. | 
    | **WEBSITE_CONTENTAZUREFILECONNECTIONSTRING**  | mysecurestorageConnectionString | Oluşturduğunuz depolama hesabının bağlantı dizesi. Bu ayar değiştirilerek, işlev uygulamanız artık dağıtım sırasında kullanılan Azure dosyaları için güvenli depolama hesabını kullanacaktır. |
    | **WEBSITE_CONTENTSHARE** | files | Depolama hesabında oluşturduğunuz dosya paylaşımının adı. Bu uygulama ayarı WEBSITE_CONTENTAZUREFILECONNECTIONSTRING birlikte kullanılır. |
    | **SERVICEBUS_CONNECTION** | myServiceBusConnectionString | Service Bus bağlantı dizesi için bir uygulama ayarı oluşturun. Bu, [Get Service Bus bağlantı dizesinden](#get-service-bus-connection-string)alınan depolama bağlantı dizesidir.|
    | **WEBSITE_CONTENTOVERVNET** | 1 | Bu uygulama ayarını oluştur. 1 değeri, depolama hesabınızı bir sanal ağla sınırlandırdığınızda, işlev uygulamanızın ölçeklendirilmesini sağlar. Depolama hesabınızı bir sanal ağ ile kısıtlamadan bu ayarı etkinleştirmeniz gerekir. |
    | **WEBSITE_DNS_SERVER** | 168.63.129.16 | Bu uygulama ayarını oluştur. Uygulamanız bir sanal ağ ile tümleştirdikten sonra, sanal ağ ile aynı DNS sunucusunu kullanacaktır. Bu, gereken iki ayarlardan biridir ve özel uç noktalar kullanılırken, işlev uygulamanızın Azure DNS özel bölgeler ile çalışıyor olması gerekir. Bu ayarlar, uygulamanızdaki tüm giden çağrıları sanal ağınıza gönderir. |
    | **WEBSITE_VNET_ROUTE_ALL** | 1 | Bu uygulama ayarını oluştur. Uygulamanız bir sanal ağ ile tümleştirdikten sonra, sanal ağ ile aynı DNS sunucusunu kullanacaktır. Bu, gereken iki ayarlardan biridir ve özel uç noktalar kullanılırken, işlev uygulamanızın Azure DNS özel bölgeler ile çalışıyor olması gerekir. Bu ayarlar, uygulamanızdaki tüm giden çağrıları sanal ağınıza gönderir. |

1. **Yapılandırma** görünümü ' nde durun, **işlev çalışma zamanı ayarları** sekmesini seçin.

1. **Çalışma zamanı ölçek Izlemeyi** **Açık** olarak ayarlayın ve **Kaydet**' i seçin. Çalışma zamanı tabanlı ölçeklendirme, HTTP olmayan tetikleyici işlevlerini sanal ağınızın içinde çalışan hizmetlere bağlamanıza olanak tanır.

    :::image type="content" source="./media/functions-create-vnet/11-enable-runtime-scaling.png" alt-text="Azure Işlevleri için çalışma zamanı tabanlı ölçeklendirmeyi etkinleştirme ekran görüntüsü.":::

## <a name="deploy-a-service-bus-trigger-and-http-trigger-to-your-function-app"></a>İşlev uygulamanıza bir Service Bus tetikleyicisi ve http tetikleyicisi dağıtın

1. GitHub 'da, bir HTTP tetikleyicisi ve Service Bus kuyruğu tetikleyicisi olmak üzere iki işlevi olan bir işlev uygulaması içeren aşağıdaki örnek depoya gidin.

    <https://github.com/Azure-Samples/functions-vnet-tutorial>

1. Sayfanın üst kısmında, GitHub hesabınızda veya kuruluşunuzda bu deponun bir çatalını oluşturmak için **çatal** düğmesini seçin.

1. İşlev uygulamanızda, sol taraftaki menüden **Dağıtım Merkezi** ' ni seçin. Ardından **Ayarlar**' ı seçin.

1. **Ayarlar** sekmesinde, aşağıda belirtildiği gibi dağıtım ayarlarını kullanın:

    | Ayar      | Önerilen değer  | Açıklama      |
    | ------------ | ---------------- | ---------------- |
    | **Kaynak** | GitHub | 2. adımda örnek kodla bir GitHub deposu oluşturmuş olmanız gerekir. | 
    | **Kuruluş**  | Myorganleştirme | Bu, deponuzunuzun, genellikle hesabınız tarafından işaretlenmiş kuruluştur. |
    | **Depo** | myRepo | Örnek kodla oluşturduğunuz depo. |
    | **Dal** | main | Bu, yeni oluşturduğunuz depodır, bu nedenle ana dalı kullanın. |
    | **Çalışma zamanı yığını** | .NET | Örnek kod C# ' de bulunur. |

1. **Kaydet**’i seçin. 

    :::image type="content" source="./media/functions-create-vnet/12-deploy-portal.png" alt-text="Portal aracılığıyla Azure Işlevleri kodunun nasıl dağıtılacağı ekran görüntüsü.":::

1. İlk dağıtımınız birkaç dakika sürebilir. Uygulamanız başarıyla dağıtıldığında **Günlükler** sekmesinde **başarılı (etkin)** bir durum iletisi görürsünüz. Gerekirse, sayfayı yenileyin. 

1. Tebrikler! Örnek işlev uygulamanızı başarıyla dağıttınız.

## <a name="lock-down-your-function-app-with-a-private-endpoint"></a>İşlev uygulamanızı özel bir uç nokta ile kilitleme

Şimdi, işlev uygulamanız için özel uç noktası oluşturun. Bu özel uç nokta, özel bir IP adresi kullanarak işlev uygulamanızı sanal ağınıza özel olarak ve güvenli bir şekilde bağlayacaktır. Özel uç noktalar hakkında daha fazla bilgi için [Özel uç noktalar belgelerine](https://docs.microsoft.com/azure/private-link/private-endpoint-overview)gidin.

1. İşlev uygulamanızda, sol taraftaki menüden **ağ** ' ı seçin.

1. Özel uç nokta bağlantıları altında **yapılandırmak için buraya tıklayın ' ı** seçin.

    :::image type="content" source="./media/functions-create-vnet/14-navigate-app-private-endpoint.png" alt-text="İşlev Uygulaması özel bir uç noktaya nasıl gidebileceğiniz ekran görüntüsü.":::

1. **Add (Ekle)** seçeneğini belirleyin.

1. Açılan menüde, aşağıda belirtildiği gibi özel uç nokta ayarlarını kullanın:

    :::image type="content" source="./media/functions-create-vnet/15-create-app-private-endpoint.png" alt-text="İşlev Uygulaması özel bir uç noktanın nasıl oluşturulacağı ekran görüntüsü.":::

1. Özel uç noktayı eklemek için **Tamam ' ı** seçin. Tebrikler! İşlev uygulamanızı, Service Bus 'ı ve depolama hesabınızı özel uç noktalarla başarıyla güvenli hale getirdi!

### <a name="test-your-locked-down-function-app"></a>Kilitli işlev uygulamanızı test etme

1. İşlev uygulamanızda, sol menüden **işlevler** ' i seçin.

1. **Servicebusqueuetrigger** öğesini seçin.

1. Sol menüden **izleyici**' yi seçin. Uygulamanızı izleyemedik görürsünüz. Bunun nedeni, tarayıcınızın sanal ağa erişimi olmadığından, sanal ağ içindeki kaynaklara doğrudan erişemez. Artık Application Insights işlevinizi izlemeye devam etmek için başka bir yöntem göstereceğiz.

1. İşlev uygulamanızda, sol menüden **Application Insights** ' yi seçin ve **Application Insights verileri görüntüle**' yi seçin.

    :::image type="content" source="./media/functions-create-vnet/16-app-insights.png" alt-text="İşlev Uygulaması için Application Insights 'ın nasıl görüntüleneceği ekran görüntüsü.":::

1. Sol taraftaki menüden **canlı ölçümler** ' i seçin.

1. Yeni bir sekme açın. Service Bus sol menüden **Kuyruklar** ' ı seçin.

1. Kuyruğunuzu seçin.

1. Sol menüden **Service Bus Explorer** ' ı seçin. **Gönder**' in altında, **Içerik türü** olarak **metin/düz** ' i seçin ve bir ileti girin. 

1. İletiyi göndermek için **Gönder** ' i seçin.

    :::image type="content" source="./media/functions-create-vnet/17-send-service-bus-message.png" alt-text="Portal kullanarak Service Bus iletilerinin nasıl gönderileceğini gösteren ekran görüntüsü.":::

1. **Canlı ölçümleri** açık olan sekmede, Service Bus kuyruğu tetikleyicinizin tetiklendiğini görmeniz gerekir. Bu yoksa iletiyi **Service Bus Explorer** 'dan yeniden gönderin

    :::image type="content" source="./media/functions-create-vnet/18-hello-world.png" alt-text="İşlev uygulamaları için canlı ölçümleri kullanarak iletilerin nasıl görüntüleneceği ekran görüntüsü.":::

1. Tebrikler! İşlev uygulamanızı özel uç noktalarla başarıyla test tamamladınız!

### <a name="private-dns-zones"></a>Özel DNS bölgeler
Azure kaynaklarına bağlanmak için özel bir uç nokta kullanılması, genel uç nokta yerine özel bir IP adresine bağlanma anlamına gelir. Mevcut Azure Hizmetleri, genel uç noktaya bağlanmak için mevcut DNS 'yi kullanacak şekilde yapılandırılmıştır. Özel uç noktaya bağlanmak için DNS yapılandırmasının geçersiz kılınması gerekir.

Özel bir uç noktayla yapılandırılmış her Azure kaynağı için özel bir DNS bölgesi oluşturuldu. Özel uç nokta ile ilişkili her özel IP adresi için bir DNS A kaydı oluşturulur.

Bu öğreticide aşağıdaki DNS bölgeleri oluşturulmuştur:

- privatelink.file.core.windows.net
- privatelink.blob.core.windows.net
- privatelink.servicebus.windows.net
- privatelink.azurewebsites.net

[!INCLUDE [clean-up-section-portal](../../includes/clean-up-section-portal.md)]

## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Premium işlev uygulaması, depolama hesabı ve Service Bus oluşturdunuz ve bunların tümünü özel uç noktaları arkasında güvence altına alırsınız! Aşağıda bulunan çeşitli ağ özellikleri hakkında daha fazla bilgi edinin:

> [!div class="nextstepaction"]
> [Işlevlerde ağ seçenekleri hakkında daha fazla bilgi edinin](./functions-networking-options.md)

[Premium planı]: functions-premium-plan.md
