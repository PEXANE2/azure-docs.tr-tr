---
title: Zaman Serisi Öngörülerini Uzaktan İzleme yle Bütünleştir - Azure | Microsoft Dokümanlar
description: Bu şekilde, Time Series Insights'ı içermeyen mevcut bir Uzaktan İzleme çözümü için Time Series Öngörülerini nasıl yapılandıracağınızı öğreneceksiniz.
author: Philmea
manager: timlt
ms.author: philmea
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 752529454a5b6293d9cbfdf8378b46947aed5a0e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "77564653"
---
# <a name="integrate-azure-time-series-insights-with-remote-monitoring"></a>Azure Time Series Insights’ı Uzaktan İzleme ile tümleştirme

Azure Time Series Öngörüleri, bulutta IoT ölçeğinde zaman serisi verilerini yönetmek için tamamen yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Zaman serisi verilerini depolamak ve yönetmek, olayları aynı anda keşfetmek ve görselleştirmek, kök neden analizi yapmak ve birden çok siteyi ve varlığı karşılaştırmak için Zaman Serisi Öngörüleri'ni kullanabilirsiniz.

Uzaktan İzleme çözüm hızlandırıcısı artık Time Series Insights ile otomatik dağıtım ve entegrasyon sağlar. Bu nasıl yapılandırılırsa, Time Series Öngörülerini içermeyen mevcut bir Uzaktan İzleme çözümü için Time Series Öngörülerini nasıl yapılandırabileceğinizi öğrenirsiniz.

> [!NOTE]
> Zaman Serisi Öngörüleri şu anda Azure Çin bulutunda kullanılamıyor. Azure Çin bulutundaki yeni Uzaktan İzleme çözüm hızlandırıcı dağıtımları, tüm depolama için Cosmos DB'yi kullanır.

## <a name="prerequisites"></a>Ön koşullar

Bu nasıl yapılacağını tamamlamak için, bir Uzaktan İzleme çözümünü zaten dağıtmış olmanız gerekir:

* [Uzaktan İzleme çözüm hızlandırıcısını dağıtma](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Tüketici grubu oluşturma

IoT Hub'ınızda Zaman Serisi Öngörüleri'ne veri akışı için kullanılmak üzere özel bir tüketici grubu oluşturun.

> [!NOTE]
> Tüketici grupları, uygulamalar tarafından Azure IoT Hub'ından veri çekmek için kullanılır. Her tüketici grubu en fazla beş çıktı tüketicisağlar. Her beş çıktı için yeni bir tüketici grubu oluşturmanız ve en fazla 32 tüketici grubu oluşturabilirsiniz.

1. Azure portalında Bulut Kabuğu düğmesini tıklatın.

1. Yeni bir tüketici grubu oluşturmak için aşağıdaki komutu uygulayın. Uzaktan İzleme dağıtımınızda IoT hub'ının adını ve kaynak grubu adı olarak Uzaktan İzleme dağıtımınızın adını kullanın:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="deploy-time-series-insights"></a>Zaman Serisi Öngörülerini Dağıt

Ardından, Zaman Serisi Öngörülerini Uzaktan İzleme çözümünüze ek kaynak olarak dağıtın ve IoT hub'ına bağlayın.

1. [Azure portalında](https://portal.azure.com/)oturum açın.

1. Things > **Zaman Serisi Öngörüler**bir kaynak > **Internet** **oluştur'un**seçin.

    ![Yeni Zaman Serisi Öngörüleri](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights.png)

1. Time Series Öngörüleri ortamınızı oluşturmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ortam Adı | Aşağıdaki ekran görüntüsü **contorosrmtsi**adını kullanır. Bu adımı tamamladığınızda kendi benzersiz adınızı seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **Varolan kullanın.** Varolan Uzaktan İzleme kaynak grubunuzun adını seçin. |
    | Konum | Biz **Doğu ABD**kullanıyoruz. Ortamınızı mümkünse Uzaktan İzleme çözümünüzle aynı bölgede oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |

    ![Zaman Serisi Öngörüleri Oluştur](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights-create.png)

1. **Oluştur'u**tıklatın. Ortamın yaratılması biraz zaman alabilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IoT hub'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturulan tüketici grubunu kullandığınızdan emin olun. Zaman Serisi Öngörüleri, her hizmetin başka bir hizmet tarafından kullanılmayan özel bir tüketici grubuna sahip olmasını gerektirir.

1. Yeni Time Series Öngörüleri ortamınıza gidin.

1. Solda **Olay Kaynakları'nı**seçin.

    ![Etkinlik Kaynaklarını Görüntüle](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources.png)

1. **Ekle**’ye tıklayın.

    ![Etkinlik Kaynağı Ekle](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources-add.png)

1. IoT hub'ınızı yeni bir olay kaynağı olarak yapılandırmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Olay kaynağı adı | Aşağıdaki ekran görüntüsü **contosorm-iot-hub**adını kullanır. Bu adımı tamamladığınızda kendi benzersiz adınızı kullanın. |
    | Kaynak | **IoT Hub** |
    | İçeri aktarma seçeneği | **Kullanılabilir aboneliklerden IoT Hub'ı kullanma** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | Iot hub adı | **contosorma57a6**. Uzaktan İzleme çözümünüzden IoT hub'ınızın adını kullanın. |
    | Iot hub ilke adı | **iothubsahibi** Kullanılan ilkenin bir sahip ilkesi olduğundan emin olun. |
    | Iot hub ilkesi anahtarı | Bu alan otomatik olarak doldurulur. |
    | Iot hub tüketici grubu | **zaman dizileri insights** |
    | Olay serileştirme biçimi | **Json**     | 
    | Zaman damgası özellik adı | Boş bırakın |

    ![Etkinlik Kaynağı Oluşturma](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-source-create.png)

1. **Oluştur'u**tıklatın.

## <a name="configure-the-data-access-policy"></a>Veri erişim ilkesini yapılandırma

Uzaktan İzleme çözümünüze erişimi olan tüm kullanıcıların Time Series Insights explorer'daki verileri keşfedebilmesini sağlamak için, uygulamanızı ve kullanıcılarınızı Azure portalındaki veri erişim ilkeleri ne göre ekleyin. 

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. **ContosoRM** kaynak grubunu seçin.

1. Azure kaynakları listesinde **contosormtsi'yi** seçin.

1. Geçerli rol atamaları listesini görmek için **Veri Erişim İlkeleri'ni** seçin.

1. **Kullanıcı Kuralını Seç** bölmesini açmak için **Ekle'yi** seçin.

   Roller atama izniniz **yoksa, Ekle** seçeneğini görmezsiniz.

1. **Rol** açılır listesinde, **Okuyucu** ve **Katkıda Bulunan**gibi bir rol seçin.

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rol atamasını oluşturmak için **Kaydet**'i seçin. Birkaç dakika sonra, güvenlik ilkesine veri erişim ilkelerinde rol atanır.

> [!NOTE]
> Zaman Serisi Öngörüler gezginine ek kullanıcılara erişim izni vermeniz gerekiyorsa, [veri erişimi vermek](../time-series-insights/time-series-insights-data-access.md#grant-data-access)için bu adımları kullanabilirsiniz.

## <a name="configure-azure-stream-analytics"></a>Azure Akış Analizini Yapılandırma 

Bir sonraki adım, Azure Akış Analizi Yöneticisi mikro hizmetini Cosmos DB'ye ileti göndermeyi durduracak ve bunları yalnızca Time Series Insights'ta saklar. İletilerinizi Cosmos DB'de çoğaltmak istiyorsanız bu adımı atlayın.

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. **ContosoRM** kaynak grubunu seçin.

1. Kaynak listesinde Azure Akış Analizi (ASA) akış işini bulun. Kaynak adı akış işleri ile **başlar-**.

1. En üstte, ASA akış işlerini durdurmak için düğmeyi tıklatın.

1. ASA sorgusunu düzenleyin ve Cosmos DB'deki ileti akışını işaret eden **SELECT**, **INTO**ve **FROM** yan tümcelerini kaldırın. Bu yan tümceler sorgunun en altında olmalı ve aşağıdaki örnek gibi görünmelidir:

    ```sql
    SELECT
            CONCAT(T.IoTHub.ConnectionDeviceId, ';', CAST(DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) AS nvarchar(max))) as id,
            1 as [doc.schemaVersion],
            'd2cmessage' as [doc.schema],
            T.IoTHub.ConnectionDeviceId as [device.id],
            'device-sensors;v1' as [device.msg.schema],
            'StreamingJobs' as [data.schema],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', System.Timestamp) as [device.msg.created],
            DATEDIFF(millisecond, '1970-01-01T00:00:00Z', T.EventEnqueuedUtcTime) as [device.msg.received],
            udf.removeUnusedProperties(T) as Data
    INTO
        Messages
    FROM
        DeviceTelemetry T PARTITION BY PartitionId TIMESTAMP BY T.EventEnqueuedUtcTime
    ```

6. Azure Akış Analizi akış işlerini yeniden başlatın.

7. Komut istemine aşağıdaki komutu yazarak Azure Akış Analizi yöneticisi mikrohizmetindeki en son değişiklikleri çekin:

.NET: 

```cmd/sh
docker pull azureiotpcs/asa-manager-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/asa-manager-java:1.0.2
```

## <a name="configure-the-telemetry-microservice"></a>Telemetri mikro hizmetini yapılandırın

Komut istemiiçine aşağıdaki komutu yazarak en son Telemetri microservice çekin:

.NET:

```cmd/sh
docker pull azureiotpcs/telemetry-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/telemetry-java:1.0.2
```

## <a name="optional-configure-the-web-ui-to-link-to-the-time-series-insights-explorer"></a>*[İsteğe bağlı]* Zaman Serisi Öngörüler gezginine bağlanmak için web UI'sini yapılandırın

Zaman Serisi Öngörüler gezgininde verilerinizi kolayca görüntülemek için, ortama kolayca bağlantı sağlamak için UI'yi özelleştirmenizi öneririz. Bunu yapmak için, aşağıdaki komutu kullanarak Web UI'deki en son değişiklikleri çekin:

```cmd/sh
docker pull azureiotpcs/pcs-remote-monitoring-webui:1.0.2
```

## <a name="configure-the-environment-variables"></a>Ortam değişkenlerini yapılandırma

Time Series Insights tümleştirmesini tamamlamak için, güncelleştirilmiş mikro hizmetler için dağıtım ınızın ortamını yapılandırmanız gerekir.

### <a name="basic-deployments"></a>Temel dağıtımlar

Güncelleştirilmiş mikro `basic` hizmetler için dağıtım ortamını yapılandırın.

1. Azure portalında, sol paneldeki **Azure Etkin Dizin** sekmesine tıklayın.

1. Uygulama **kayıtları**na tıklayınız.

1. **ContosoRM** uygulamanızı arayın ve tıklayın.

1. **Ayarlar** > **Tuşları'na** gidin ve ardından uygulamanız için yeni bir anahtar oluşturun. Anahtar Değerini güvenli konuma kopyaladığından emin olun.

1. En son etiketi kullanarak GitHub repo'dan [en son docker oluşturmak yaml dosyaçekin.](https://github.com/Azure/pcs-cli/tree/5a9b4e0dbe313172eff19236e54a4d461d4f3e51/solutions/remotemonitoring/single-vm) 

1. [SSH Tuşlarının nasıl oluşturulup kullanılacağı](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)nasıI kullanılacağına ilişkin belirtilen adımları izleyerek VM'ye SSH girer.

1. Bağlandıktan sonra `cd /app`yazın.

1. Docker'daki her microservice'e aşağıdaki ortam değişkenlerini ekleyin `env-setup` yaml dosyasını ve VM'deki komut dosyasını oluşturun:

    ```sh
    PCS_TELEMETRY_STORAGE_TYPE=tsi
    PCS_TSI_FQDN={TSI Data Access FQDN}
    PCS_AAD_TENANT={AAD Tenant Id}
    PCS_AAD_APPID={AAD application Id}
    PCS_AAD_APPSECRET={AAD application key}
    ```

1. **Telemetri hizmetine** gidin ve aynı ortam değişkenlerini yukarıda ekleyerek docker oluşturma dosyasını da düzenleme.

1. **ASA yöneticisi hizmetine** gidin ve docker ekleyün `PCS_TELEMETRY_STORAGE_TYPE`dosyayı düzenleme.

1. VM'den kullanarak `sudo ./start.sh` docker konteynerlerini yeniden başlatın.

> [!NOTE]
> Yukarıdaki ortam değişkenleri yapılandırması 1.0.2'den önceki Uzaktan İzleme sürümleri için geçerlidir.

### <a name="standard-deployments"></a>Standart dağıtımlar

Yukarıdaki güncelleştirilmiş `standard` mikro hizmetler için dağıtım ortamını yapılandırın

1. Komut satırında, `kubectl proxy`çalıştırın. Daha fazla bilgi için [Kubernetes API'ye erişim e](https://kubernetes.io/docs/tasks/access-kubernetes-api/http-proxy-access-api/#using-kubectl-to-start-a-proxy-server)bakın.

1. Kubernetes yönetim konsolu açık.

1. TSI için aşağıdaki yeni ortam değişkenlerini eklemek için yapılandırma haritasını bulun:

    ```yaml
    telemetry.storage.type: "tsi"
    telemetry.tsi.fqdn: "{TSI Data Access FQDN}"
    security.auth.serviceprincipal.secret: "{AAD application service principal secret}"
    ```

4. Telemetri servis bölmesi için şablon yaml dosyasını edin:

    ```yaml
    - name: PCS_AAD_TENANT
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.tenant
    - name: PCS_AAD_APPID
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.audience
    - name: PCS_AAD_APPSECRET
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: security.auth.serviceprincipal.secret
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    - name: PCS_TSI_FQDN
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.tsi.fqdn
    ```

5. ASA yöneticisi hizmet bölmesi için şablon yaml dosyasını edin:

    ```yaml
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    ```

## <a name="next-steps"></a>Sonraki adımlar

* Zaman Serisi Öngörüler kaşifinde verilerinizi nasıl keşfeder ve bir uyarıtanı koymak hakkında bilgi edinmek için, [temel neden çözümlemesi](iot-accelerators-remote-monitoring-root-cause-analysis.md)ile ilgili öğreticimize bakın.

* Zaman Serisi Öngörüleri gezginindeki verileri nasıl keşfedip sorgulayınız öğrenmek için [Azure Zaman Serisi Öngörüleri gezginindeki](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer)belgelere bakın.
