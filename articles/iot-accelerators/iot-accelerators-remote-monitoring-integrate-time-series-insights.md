---
title: Time Series Insights uzaktan Izleme ile tümleştirme-Azure | Microsoft Docs
description: Bu şekilde, Time Series Insights dahil olmayan mevcut bir uzaktan Izleme çözümü için Time Series Insights nasıl yapılandıracağınızı öğreneceksiniz.
author: Philmea
manager: timlt
ms.author: philmea
ms.date: 09/12/2018
ms.topic: conceptual
ms.service: iot-accelerators
services: iot-accelerators
ms.openlocfilehash: 752529454a5b6293d9cbfdf8378b46947aed5a0e
ms.sourcegitcommit: f27b045f7425d1d639cf0ff4bcf4752bf4d962d2
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 02/23/2020
ms.locfileid: "77564653"
---
# <a name="integrate-azure-time-series-insights-with-remote-monitoring"></a>Azure Time Series Insights’ı Uzaktan İzleme ile tümleştirme

Azure Time Series Insights, bulutta IoT ölçekli zaman serisi verilerinin yönetilmesi için tam olarak yönetilen bir analiz, depolama ve görselleştirme hizmetidir. Zaman serisi verilerini depolamak ve yönetmek, olayları aynı anda araştırmak ve görselleştirmek, kök neden analizi gerçekleştirmek ve birden çok siteyi ve varlığı karşılaştırmak için Time Series Insights kullanabilirsiniz.

Uzaktan Izleme çözümü Hızlandırıcısı artık Time Series Insights otomatik dağıtım ve tümleştirme sağlar. Bu nasıl yapılır, zaten Time Series Insights içermeyen mevcut bir uzaktan Izleme çözümü için Time Series Insights yapılandırmayı öğrenirsiniz.

> [!NOTE]
> Time Series Insights Azure Çin bulutu 'nda Şu anda kullanılamıyor. Azure Çin bulutu 'ndaki yeni uzaktan Izleme çözümü Hızlandırıcısı dağıtımları tüm depolama için Cosmos DB kullanır.

## <a name="prerequisites"></a>Önkoşullar

Bu nasıl yapılır hakkında daha fazla bir uzaktan Izleme çözümü dağıtmış olmanız gerekir:

* [Uzaktan Izleme çözüm Hızlandırıcısını dağıtma](quickstart-remote-monitoring-deploy.md)

## <a name="create-a-consumer-group"></a>Bir tüketici grubu oluşturun

Time Series Insights veri akışı için kullanılacak IoT Hub adanmış bir tüketici grubu oluşturun.

> [!NOTE]
> Tüketici grupları, uygulamalar tarafından Azure IoT Hub veri çekmek için kullanılır. Her Tüketici grubu en fazla beş çıkış tüketicisine izin verir. Her beş çıkış için yeni bir tüketici grubu oluşturmanız ve en fazla 32 Tüketici grubu oluşturmanız gerekir.

1. Azure portal Cloud Shell düğmesine tıklayın.

1. Yeni bir tüketici grubu oluşturmak için aşağıdaki komutu yürütün. Uzaktan Izleme dağıtımınızdaki IoT Hub 'ın adını ve uzaktan Izleme dağıtımınızın adını kaynak grubu adı olarak kullanın:

```azurecli-interactive
az iot hub consumer-group create --hub-name contosorm30526 --name timeseriesinsights --resource-group ContosoRM
```

## <a name="deploy-time-series-insights"></a>Time Series Insights dağıt

Sonra, Time Series Insights uzaktan Izleme çözümünüze ek bir kaynak olarak dağıtın ve IoT Hub 'ına bağlayın.

1. [Azure Portal](https://portal.azure.com/) oturum açın.

1. **Time Series Insights** > **nesnelerin interneti** > **kaynak oluştur** ' u seçin.

    ![Yeni Time Series Insights](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights.png)

1. Time Series Insights ortamınızı oluşturmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Ortam adı | Aşağıdaki ekran görüntüsünde **contorosrmtsi**adı kullanılmaktadır. Bu adımı tamamladığınızda kendi benzersiz adınızı seçin. |
    | Abonelik | Açılan listeden Azure aboneliğinizi seçin. |
    | Kaynak grubu | **Mevcut olanı kullanın**. Var olan uzaktan Izleme kaynak grubunuzun adını seçin. |
    | Konum | **Doğu ABD**kullandık. Mümkünse, ortamınızı uzaktan Izleme çözümünüz ile aynı bölgede oluşturun. |
    | Sku |**S1** |
    | Kapasite | **1** |

    ![Time Series Insights oluştur](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/new-time-series-insights-create.png)

1. **Oluştur**’ tıklayın. Ortamın oluşturulması biraz zaman alabilir.

## <a name="create-event-source"></a>Olay kaynağı oluşturma

IoT Hub 'ınıza bağlanmak için yeni bir olay kaynağı oluşturun. Önceki adımlarda oluşturulan tüketici grubunu kullandığınızdan emin olun. Time Series Insights, her bir hizmetin özel bir hizmet tarafından kullanımda olmayan adanmış bir tüketici grubuna sahip olmasını gerektirir.

1. Yeni Time Series Insights ortamınıza gidin.

1. Sol tarafta **olay kaynakları**' nı seçin.

    ![Olay kaynaklarını görüntüle](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources.png)

1. **Ekle**'ye tıklayın.

    ![Olay kaynağı Ekle](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-sources-add.png)

1. IoT Hub 'ınızı yeni bir olay kaynağı olarak yapılandırmak için aşağıdaki tablodaki değerleri kullanın:

    | Ayar | Değer |
    | ------- | ----- |
    | Olay kaynağı adı | Aşağıdaki ekran görüntüsünde **contosorm-IoT-Hub**adı kullanılmaktadır. Bu adımı tamamladığınızda kendi benzersiz adınızı kullanın. |
    | Kaynak | **IoT Hub’ı** |
    | İçeri aktarma seçeneği | **Kullanılabilir aboneliklerden IoT Hub kullanma** |
    | Abonelik Kimliği | Açılan listeden Azure aboneliğinizi seçin. |
    | IoT Hub adı | **contosorma57a6**. Uzaktan Izleme çözümünüzdeki IoT Hub 'ınızın adını kullanın. |
    | IoT Hub ilkesi adı | **iothubowner** Kullanılan ilkenin bir sahip ilkesi olduğundan emin olun. |
    | IoT Hub ilke anahtarı | Bu alan otomatik olarak doldurulur. |
    | IoT Hub 'ı Tüketici grubu | **timeseriesınsights** |
    | Olay serileştirme biçimi | **JSON**     | 
    | Zaman damgası özellik adı | Boş bırakın |

    ![Olay kaynağı oluştur](./media/iot-accelerators-remote-monitoring-integrate-time-series-insights/time-series-insights-event-source-create.png)

1. **Oluştur**’ tıklayın.

## <a name="configure-the-data-access-policy"></a>Veri erişim ilkesini yapılandırma

Uzaktan Izleme çözümünüze erişimi olan tüm kullanıcıların Time Series Insights Explorer 'daki verileri keşfedebilmesini sağlamak için, Azure portal veri erişim ilkeleri altına uygulamanızı ve kullanıcılarınızı ekleyin. 

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. **ContosoRM** kaynak grubunu seçin.

1. Azure kaynakları listesinde **contosormtsi** öğesini seçin.

1. Rol atamalarının geçerli listesini görmek için **veri erişim ilkeleri** ' ni seçin.

1. **Ekle** ' yi seçerek **Kullanıcı kuralı seç** bölmesini açın.

   Rol atama izniniz yoksa, **Ekle** seçeneğini görmezsiniz.

1. **Rol** açılan listesinde, **okuyucu** ve **katkıda bulunan**gibi bir rol seçin.

1. **Seç** listesinde bir kullanıcı, grup veya uygulama seçin. Listede güvenlik sorumlusunu görmüyorsanız **Seç** kutusuna giriş yaparak dizinde görünen ad, e-posta adresi ve nesne tanımlayıcısı arayabilirsiniz.

1. Rol atamasını oluşturmak için **Kaydet**'i seçin. Birkaç dakika sonra, güvenlik sorumlusu, veri erişim ilkelerinde role atanır.

> [!NOTE]
> Time Series Insights Gezgini 'ne ek kullanıcılara erişim vermeniz gerekiyorsa, bu adımları [veri erişimi vermek](../time-series-insights/time-series-insights-data-access.md#grant-data-access)için kullanabilirsiniz.

## <a name="configure-azure-stream-analytics"></a>Azure Stream Analytics Yapılandır 

Sonraki adım Azure Stream Analytics Manager Mikro hizmetini, Cosmos DB iletileri göndermeyi ve yalnızca Time Series Insights depolamayı bırakmak üzere yapılandırmaktır. İletilerinizi Cosmos DB çoğaltmak istiyorsanız bu adımı atlayın.

1. Gezinti bölmesinde **Kaynak grupları**'nı seçin.

1. **ContosoRM** kaynak grubunu seçin.

1. Kaynak listesinde Azure Stream Analytics (ASA) akış işini bulun. Kaynak adı **streamingjobs-** ile başlar.

1. En üstte, ASA akış işlerini durdurmak için düğmeye tıklayın.

1. ASA sorgusunu düzenleyin ve Cosmos DB ' de iletiler akışını işaret eden **Select**, **Into**ve **from** yan tümcelerini kaldırın. Bu yan tümceler sorgunun en altında olmalıdır ve aşağıdaki örnekteki gibi görünmelidir:

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

6. Azure Stream Analytics akış işlerini yeniden başlatın.

7. Komut istemine aşağıdaki komutu yazarak Azure Stream Analytics Manager Mikro hizmetine yapılan en son değişiklikleri çekin:

.NET: 

```cmd/sh
docker pull azureiotpcs/asa-manager-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/asa-manager-java:1.0.2
```

## <a name="configure-the-telemetry-microservice"></a>Telemetri mikro hizmetini yapılandırma

Komut istemine aşağıdaki komutu yazarak en son telemetri mikro hizmetini çekin:

.NET:

```cmd/sh
docker pull azureiotpcs/telemetry-dotnet:1.0.2
```

Java:

```cmd/sh
docker pull azureiotpcs/telemetry-java:1.0.2
```

## <a name="optional-configure-the-web-ui-to-link-to-the-time-series-insights-explorer"></a>*[Isteğe bağlı]* Web Kullanıcı arabirimini Time Series Insights gezgin 'e bağlanacak şekilde yapılandırma

Time Series Insights Gezgininde verilerinizi kolayca görüntülemek için, Kullanıcı arabirimini kolayca ortama bağlamak üzere özelleştirmeyi öneririz. Bunu yapmak için, aşağıdaki komutu kullanarak en son değişiklikleri Web Kullanıcı arabirimine çekin:

```cmd/sh
docker pull azureiotpcs/pcs-remote-monitoring-webui:1.0.2
```

## <a name="configure-the-environment-variables"></a>Ortam değişkenlerini yapılandırma

Time Series Insights tümleştirmesini tamamlayabilmeniz için, güncelleştirilmiş mikro hizmetler için dağıtımınızın ortamını yapılandırmanız gerekecektir.

### <a name="basic-deployments"></a>Temel dağıtımlar

Güncelleştirilmiş mikro hizmetler için `basic` dağıtımının ortamını yapılandırın.

1. Azure portal sol bölmedeki **Azure Active Directory** sekmesine tıklayın.

1. **Uygulama kayıtları**' ye tıklayın.

1. **ContosoRM** uygulamanızda arama yapın ve tıklayın.

1. **Ayarlar** > **anahtarlar** ' a gidin ve uygulamanız için yeni bir anahtar oluşturun. Anahtar değerini güvenli konuma kopyalamadığınızdan emin olun.

1. En son etiketini kullanarak GitHub deposundan [en son Docker Compose YAML dosyasını](https://github.com/Azure/pcs-cli/tree/5a9b4e0dbe313172eff19236e54a4d461d4f3e51/solutions/remotemonitoring/single-vm) çekin. 

1. [SSH anahtarları oluşturma ve kullanma](https://docs.microsoft.com/azure/virtual-machines/linux/ssh-from-windows)konusunda açıklanan ADıMLARı izleyerek VM 'ye SSH.

1. Bağlandıktan sonra `cd /app`yazın.

1. Docker Compose YAML dosyasındaki her bir mikro hizmete ve VM 'deki `env-setup` betiğine aşağıdaki ortam değişkenlerini ekleyin:

    ```sh
    PCS_TELEMETRY_STORAGE_TYPE=tsi
    PCS_TSI_FQDN={TSI Data Access FQDN}
    PCS_AAD_TENANT={AAD Tenant Id}
    PCS_AAD_APPID={AAD application Id}
    PCS_AAD_APPSECRET={AAD application key}
    ```

1. **Telemetri hizmetine** gidin ve ayrıca aynı ortam değişkenlerini ekleyerek Docker Compose dosyasını düzenleyin.

1. **Asa Yöneticisi hizmetine** gidin ve `PCS_TELEMETRY_STORAGE_TYPE`ekleyerek Docker Compose dosyasını düzenleyin.

1. SANAL makineden `sudo ./start.sh` kullanarak Docker kapsayıcılarını yeniden başlatın.

> [!NOTE]
> Ortam değişkenlerinin yukarıdaki yapılandırması 1.0.2 öncesine ait uzak Izleme sürümleri için geçerlidir

### <a name="standard-deployments"></a>Standart dağıtımlar

Yukarıdaki güncelleştirilmiş mikro hizmetler için `standard` dağıtımının ortamını yapılandırın

1. Komut satırında `kubectl proxy`' yi çalıştırın. Daha fazla bilgi için bkz. [Kubernetes API 'sine erişme](https://kubernetes.io/docs/tasks/access-kubernetes-api/http-proxy-access-api/#using-kubectl-to-start-a-proxy-server).

1. Kubernetes yönetim konsolunu açın.

1. TSI için aşağıdaki yeni ortam değişkenlerini eklemek üzere yapılandırma haritasını bulun:

    ```yaml
    telemetry.storage.type: "tsi"
    telemetry.tsi.fqdn: "{TSI Data Access FQDN}"
    security.auth.serviceprincipal.secret: "{AAD application service principal secret}"
    ```

4. Telemetri hizmeti pod için şablon YAML dosyasını düzenleyin:

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

5. ASA Yöneticisi hizmeti pod için şablon YAML dosyasını düzenleyin:

    ```yaml
    - name: PCS_TELEMETRY_STORAGE_TYPE
        valueFrom:
        configMapKeyRef:
            name: deployment-configmap
            key: telemetry.storage.type
    ```

## <a name="next-steps"></a>Sonraki adımlar

* Verilerinizi araştırıp Time Series Insights Gezginden bir uyarının nasıl tanılandığını öğrenmek için, [kök neden analizi](iot-accelerators-remote-monitoring-root-cause-analysis.md)yapma konusundaki öğreticimize bakın.

* Time Series Insights Explorer 'da verileri nasıl araştırıp sorguleyeceğinizi öğrenmek için, [Azure Time Series Insights Gezgini](https://docs.microsoft.com/azure/time-series-insights/time-series-insights-explorer)'ndeki belgeler bölümüne bakın.
