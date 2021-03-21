---
title: Faturalama verilerini Azure 'a yükleyin ve Azure portal görüntüleyin
description: Faturalama verilerini Azure 'a yükleyin ve Azure portal görüntüleyin
services: azure-arc
ms.service: azure-arc
ms.subservice: azure-arc-data
author: twright-msft
ms.author: twright
ms.reviewer: mikeray
ms.date: 03/02/2021
ms.topic: how-to
ms.openlocfilehash: 7ef1cd43d2efbc5ab92cc2b4cba4d237805d8921
ms.sourcegitcommit: 867cb1b7a1f3a1f0b427282c648d411d0ca4f81f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/20/2021
ms.locfileid: "102202663"
---
# <a name="upload-billing-data-to-azure-and-view-it-in-the-azure-portal"></a>Faturalama verilerini Azure 'a yükleyin ve Azure portal görüntüleyin

> [!IMPORTANT] 
>  Önizleme döneminde Azure Arc etkin veri hizmetlerini kullanma maliyeti yoktur. Faturalandırma sistemi uçtan uca çalışsa da faturalandırma ölçümü $0 olarak ayarlanır.  Bu senaryoyu izlerseniz, şu anda **karma veri Hizmetleri** olarak adlandırılan bir hizmet için faturanızı ve **`<resource type>` Microsoft. AzureArcData/** adlı bir türdeki kaynakları görürsünüz. Oluşturduğunuz her bir veri hizmeti için bir kayıt görebileceksiniz, ancak her kayıt $0 için faturalandırılacaktır.

[!INCLUDE [azure-arc-data-preview](../../../includes/azure-arc-data-preview.md)]

## <a name="connectivity-modes---implications-for-billing-data"></a>Bağlantı modları-faturalandırma verileri için etkiler

Gelecekte, Azure Arc etkin veri hizmetlerinizi çalıştırabileceğiniz iki mod olacaktır:

- **Dolaylı olarak bağlı** -Azure 'a doğrudan bağlantı yoktur. Veriler yalnızca bir dışarı aktarma/karşıya yükleme işlemi aracılığıyla Azure 'a gönderilir. Tüm Azure Arc veri Hizmetleri dağıtımları bu modda bugün önizleme aşamasında çalışır.
- **Doğrudan** bağlantı-bu modda, Azure Arc etkinleştirilmiş Kubernetes hizmetinde, Azure Arc etkin veri hizmetlerinin çalıştığı Kubernetes kümesi arasında doğrudan bir bağlantı sağlamak için bir bağımlılık olacaktır. Bu, daha fazla özelliği etkinleştirir ve Azure Arc etkin veri hizmetlerinizi Azure PaaS 'deki veri hizmetlerinizi yönettiğiniz gibi yönetmek için Azure portal ve Azure CLı 'yi de kullanmanıza imkan tanır.  Bu bağlantı modu henüz önizlemede kullanılamıyor, ancak yakında kullanıma sunulacak.

[Bağlantı modları](./connectivity.md)arasındaki fark hakkında daha fazla bilgi edinebilirsiniz.

Dolaylı olarak bağlanılan modda faturalandırma verileri, Azure Arc veri denetleyicisi 'nden güvenli bir dosyaya düzenli aralıklarla dışarı aktarılabilir ve sonra Azure 'a yüklenir ve işlenir.  Yaklaşan doğrudan bağlı modda faturalandırma verileri, hizmetlerinizin maliyetlerine neredeyse gerçek zamanlı bir görünüm kazandırmak için yaklaşık 1/saat Azure 'a otomatik olarak gönderilir. Verileri dolaylı olarak bağlı moddaki dışa ve karşıya yükleme işlemi, komut dosyaları kullanılarak da otomatikleştirilebilir veya sizin için bunu yapacağız bir hizmet derleyebilir.

## <a name="upload-billing-data-to-azure"></a>Faturalandırma verilerini Azure 'a yükleme

Faturalama verilerini Azure 'a yüklemek için öncelikle şunlar gerekir:

1. Henüz bir tane yoksa, Azure Arc etkin bir veri hizmeti oluşturun. Örneğin, aşağıdakilerden birini oluşturun:
   - [Azure Arc üzerinde Azure SQL yönetilen örneği oluşturma](create-sql-managed-instance.md)
   - [Azure Arc özellikli PostgreSQL Hiper Ölçek sunucu grubu oluşturma](create-postgresql-hyperscale-server-group.md)
1. Henüz yapmadıysanız [kaynak envanterini, kullanım verilerini, ölçümleri ve günlükleri Azure izleyici 'ye yükleyin](upload-metrics-and-logs-to-azure-monitor.md) .
1. Faturalandırma telemetri toplama işleminin bazı faturalandırma verilerini toplayabilmesi için veri hizmeti oluşturulduktan sonra en az 2 saat bekleyin.

Faturalandırma verilerini dışarı aktarmak için aşağıdaki komutu çalıştırın:

```console
azdata arc dc export -t usage -p usage.json
```

Bu nedenle, içeriği görebilmeniz için dosya şifrelenmemiştir. Bir metin düzenleyicisinde açabilir ve içeriğin nasıl göründüğünü görebilirsiniz.

İki veri kümesi olduğunu fark edeceksiniz: `resources` ve `data` . `resources`Veri denetleyicisi, PostgreSQL hiper ölçek sunucu grupları ve SQL yönetilen örnekleri. `resources`Verilerdeki kayıtlar, bir kaynağın geçmişinde, ne zaman güncelleştirildiği ve ne zaman silindiği üzerinde ilgili olayları yakalar. `data`Kayıtlar, her saat için belirli bir örnek tarafından kaç çekirdeğin kullanılabilir olduğunu yakalar.

`resource`Giriş örneği:

```console
    {
        "customObjectName": "<resource type>-2020-29-5-23-13-17-164711",
        "uid": "4bc3dc6b-9148-4c7a-b7dc-01afc1ef5373",
        "instanceName": "sqlInstance001",
        "instanceNamespace": "arc",
        "instanceType": "<resource>",
        "location": "eastus",
        "resourceGroupName": "production-resources",
        "subscriptionId": "482c901a-129a-4f5d-86e3-cc6b294590b2",
        "isDeleted": false,
        "externalEndpoint": "32.191.39.83:1433",
        "vCores": "2",
        "createTimestamp": "05/29/2020 23:13:17",
        "updateTimestamp": "05/29/2020 23:13:17"
    }
```

`data`Giriş örneği:

```console
        {
          "requestType": "usageUpload",
          "clusterId": "4b0917dd-e003-480e-ae74-1a8bb5e36b5d",
          "name": "DataControllerTestName",
          "subscriptionId": "482c901a-129a-4f5d-86e3-cc6b294590b2",
          "resourceGroup": "production-resources",
          "location": "eastus",
          "uploadRequest": {
            "exportType": "usages",
            "dataTimestamp": "2020-06-17T22:32:24Z",
            "data": "[{\"name\":\"sqlInstance001\",
                       \"namespace\":\"arc\",
                       \"type\":\"<resource type>\",
                       \"eventSequence\":1, 
                       \"eventId\":\"50DF90E8-FC2C-4BBF-B245-CB20DC97FF24\",
                       \"startTime\":\"2020-06-17T19:11:47.7533333\",
                       \"endTime\":\"2020-06-17T19:59:00\",
                       \"quantity\":1,
                       \"id\":\"4BC3DC6B-9148-4C7A-B7DC-01AFC1EF5373\"}]",
           "signature":"MIIE7gYJKoZIhvcNAQ...2xXqkK"
          }
        }
```

Dosyadaki usage.jsAzure 'a yüklemek için aşağıdaki komutu çalıştırın:

```console
azdata arc dc upload -p usage.json
```

## <a name="view-billing-data-in-azure-portal"></a>Azure portal faturalama verilerini görüntüleyin

Azure portal faturalama verilerini görüntülemek için aşağıdaki adımları izleyin:

1. [Azure portalını](https://portal.azure.com) açın.
1. Ekranın üst kısmındaki arama kutusuna **maliyet yönetimi** ' nde ve maliyet yönetimi hizmeti ' ne tıklayın.
1. **Maliyet yönetimine genel bakış** altında **maliyet yönetimi** sekmesine tıklayın.
1. Soldaki **Maliyet Analizi** sekmesine tıklayın.
1. Görünümün en üstündeki **kaynağa göre maliyet** düğmesine tıklayın.
1. Kapsamınız, veri hizmeti kaynaklarınızın oluşturulduğu aboneliğe ayarlandığından emin olun.
1. Görünümün üst kısmındaki kapsam seçicisinin yanındaki Görünüm açılan listesinden **kaynağa göre maliyet** ' i seçin.
1. Tarih filtresinin **Bu aya** veya veri hizmeti kaynaklarınızı oluştururken zamanlamalı bir zaman aralığına ayarlandığından emin olun.
1.    =  `Microsoft.AzureArcData/<data service type>` Yalnızca bir Azure yay etkin veri hizmeti türüne filtre uygulamak istiyorsanız, kaynak türüne göre filtre eklemek için Filtre Ekle ' ye tıklayın.
1. Şimdi oluşturulmuş ve Azure 'a yüklenen tüm kaynakların bir listesini görürsünüz. Faturalandırma ölçümü $0 olduğundan, maliyetin her zaman $0 olduğunu görürsünüz.

## <a name="download-billing-data"></a>Faturalandırma verilerini indir

Faturalandırma özet verilerini doğrudan Azure portal indirebilirsiniz.

1. Yukarıdaki yönergeleri izleyerek ulaşılan **kaynak türü görünümüne göre aynı maliyet analizi-> görünümünde** , üstteki indir düğmesine tıklayın.
1. Karşıdan yükleme dosyası türünü seçin-Excel veya CSV-ve **verileri indir** düğmesine tıklayın.
1. Dosyayı seçili dosya türü verilen uygun bir düzenleyicide açın.

## <a name="export-billing-data"></a>Faturalandırma verilerini dışarı aktar

Ayrıca, bir faturalandırma dışarı aktarma işi oluşturarak **ayrıntılı** kullanım ve faturalandırma verilerini bir Azure depolama kapsayıcısına otomatik olarak dışarı aktarabilirsiniz. Bu, faturalandırma döneminde belirli bir örneğin kaç saat faturalandırıldığını öğrenmek isterseniz, faturanızda ayrıntıları görmek istiyorsanız yararlı olur.

Bir faturalandırma dışarı aktarma işi ayarlamak için aşağıdaki adımları izleyin:

1. Sol taraftaki **dışarı aktarmalar** öğesine tıklayın.
1. **Ekle**'ye tıklayın.
1. Bir ad ve dışa aktarma sıklığı girin ve Ileri ' ye tıklayın.
1. Yeni bir depolama hesabı oluşturmayı veya var olanı kullanmayı seçin ve faturalama veri dosyalarını dışarı aktarmak için depolama hesabı, kapsayıcı ve dizin yolunu belirtmek üzere formu doldurun ve Ileri ' ye tıklayın.
1. **Oluştur**’a tıklayın.

Faturalandırma verileri dışarı aktarma dosyaları yaklaşık 4 saat içinde kullanıma sunulacaktır ve faturalandırma dışarı aktarma işi oluşturulurken belirttiğiniz zamanlamaya göre dışarı aktarılacaktır.

Şu adımları izleyerek, yayımlanan faturalandırma verileri dosyalarını görüntüleyin:

Azure portal faturalandırma verileri dosyalarını doğrulayabilirsiniz. 

> [!IMPORTANT]
> Faturalandırma dışarı aktarma işini oluşturduktan sonra, aşağıdaki adımlarla devam etmeden önce 4 saat bekleyin.

1. Portalın üst kısmındaki arama kutusuna **depolama hesapları** yazın ve **depolama hesapları**' na tıklayın.
3. Yukarıdaki faturalandırma dışarı aktarma işini oluştururken belirttiğiniz depolama hesabına tıklayın.
4. Sol taraftaki kapsayıcılar ' a tıklayın.
5. Yukarıdaki faturalandırma dışarı aktarma işini oluştururken belirttiğiniz kapsayıcıya tıklayın.
6. Yukarıdaki faturalandırma dışarı aktarma işini oluştururken belirttiğiniz klasöre tıklayın.
7. Oluşturulan klasörler ve dosyaların detayına gidin ve oluşturulan. csv dosyalarından birine tıklayın.
8. Dosyayı yerel Indirilenler klasörünüze kaydedecek **İndir** düğmesine tıklayın.
9. Excel gibi bir. csv dosya görüntüleyicisini kullanarak dosyayı açın.
10. Sonuçları yalnızca **kaynak türü** olan satırları gösterecek şekilde filtreleyin  =  `Microsoft.AzureArcData/<data service resource type` .
11. Örneğin, UsageQuantity sütunundaki geçerli 24 saat döneminde kullanılan saat sayısını görürsünüz.
