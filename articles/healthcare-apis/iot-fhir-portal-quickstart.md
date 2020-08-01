---
title: 'Hızlı başlangıç: Azure portal kullanarak IoT bağlayıcısını dağıtma (Önizleme)'
description: Bu hızlı başlangıçta, Azure portal kullanarak FHıR için Azure API 'nin IoT Bağlayıcısı özelliğini dağıtmayı, yapılandırmayı ve kullanmayı öğreneceksiniz.
services: healthcare-apis
author: ms-puneet-nagpal
ms.service: healthcare-apis
ms.subservice: iomt
ms.topic: quickstart
ms.date: 05/11/2020
ms.author: punagpal
ms.openlocfilehash: 95f5b5f13401c224ccf67c5f013deedf00379de7
ms.sourcegitcommit: cee72954f4467096b01ba287d30074751bcb7ff4
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/30/2020
ms.locfileid: "87446814"
---
# <a name="quickstart-deploy-iot-connector-preview-using-azure-portal"></a>Hızlı başlangıç: Azure portal kullanarak IoT bağlayıcısını dağıtma (Önizleme)

IoT Bağlayıcısı, FHıR için Azure API 'nin, tıbbi nesnelerin Interneti (IoMT) aygıtlarından veri alma özelliğini sağlayan isteğe bağlı bir özelliktir. Ve önizleme aşamasında IoT Bağlayıcısı özelliği ücretsiz olarak kullanılabilir. Bu hızlı başlangıçta şunları yapmayı öğreneceksiniz:
- Azure portal kullanarak IoT bağlayıcısını dağıtma ve yapılandırma
- IoT bağlayıcısına veri göndermek için sanal cihaz kullanma
- FHıR için Azure API üzerinde IoT Bağlayıcısı tarafından oluşturulan kaynakları görüntüleme

## <a name="prerequisites"></a>Ön koşullar

- Etkin bir Azure aboneliği- [ücretsiz olarak bir tane oluşturun](https://azure.microsoft.com/free/?WT.mc_id=A261C142F)
- FHıR kaynağı için Azure API- [Azure Portal kullanarak fhır Için Azure API dağıtma](fhir-paas-portal-quickstart.md)

## <a name="go-to-azure-api-for-fhir-resource"></a>FHıR kaynağı için Azure API 'sine gidin

[Azure Portal](https://portal.azure.com) açın ve IoT Bağlayıcısı özelliğini oluşturmak istediğiniz **fhır kaynağı için Azure API** 'sine gidin.

[![FHıR kaynağı için Azure API](media/quickstart-iot-fhir-portal/portal-azure-api-fhir.jpg)](media/quickstart-iot-fhir-portal/portal-azure-api-fhir.jpg#lightbox)

Sol taraftaki gezinti menüsünde, **IoT bağlayıcıları** sayfasını açmak için **Eklentiler** bölümünde **IoT Bağlayıcısı (Önizleme)** seçeneğine tıklayın.

[![IoT bağlayıcı özelliği](media/quickstart-iot-fhir-portal/portal-iot-connectors.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connectors.jpg#lightbox)

## <a name="create-new-iot-connector-preview"></a>Yeni IoT Bağlayıcısı oluştur (Önizleme)

**Ekle** düğmesine tıklayarak **IoT Bağlayıcısı oluştur** sayfasını açın.

[![IoT Bağlayıcısı Ekle](media/quickstart-iot-fhir-portal/portal-iot-connectors-add.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connectors-add.jpg#lightbox)

Yeni IoT Bağlayıcısı için ayarları girin. **Oluştur** düğmesine tıklayın ve IoT Bağlayıcısı dağıtımını await.

> [!NOTE]
> Bu yükleme için **çözüm türü** açılan değeri olarak **Oluştur** ' u seçmeniz gerekir. 

[![IoT Bağlayıcısı oluştur](media/quickstart-iot-fhir-portal/portal-iot-connector-create.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-create.jpg#lightbox)

|Ayar|Değer|Açıklama |
|---|---|---|
|Bağlayıcı adı|Benzersiz bir ad|IoT bağlayıcınızı tanımlamak için bir ad girin. Bu ad, FHıR kaynağı için bir Azure API 'SI içinde benzersiz olmalıdır. Ad yalnızca küçük harf, sayı ve kısa çizgi (-) karakterini içerebilir. Bir harf veya sayı ile başlamalı ve bitmeli ve 3-24 karakter uzunluğunda olmalıdır.|
|Çözüm türü|Ara veya oluştur|Bir bant dışı işleminiz varsa **Ara** [' yı SEÇIN ve Azure](https://www.hl7.org/fhir/device.html) API 'niz [için fhar](https://www.hl7.org/fhir/patient.html) kaynakları oluşturun. IoT Bağlayıcısı, cihaz verilerini temsil etmek [üzere bir izleme](https://www.hl7.org/fhir/observation.html) fhır kaynağı oluştururken bu kaynaklara yönelik başvuruyu kullanacaktır. IoT bağlayıcısının, cihaz verilerinde bulunan ilgili tanımlayıcı değerlerini kullanarak FHıR için Azure API 'niz üzerinde çıplak cihaz ve hasta kaynakları oluşturmasını istediğinizde **Oluştur** ' u seçin.|

Yükleme tamamlandıktan sonra, yeni oluşturulan IoT Bağlayıcısı, **IoT bağlayıcıları** sayfasında görünür.

[![IoT Bağlayıcısı oluşturuldu](media/quickstart-iot-fhir-portal/portal-iot-connector-created.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-created.jpg#lightbox)

## <a name="configure-iot-connector-preview"></a>IoT bağlayıcısını yapılandırma (Önizleme)

IoT Bağlayıcısı, cihaz iletilerini FHıR tabanlı izleme kaynaklarına dönüştürmek için iki eşleme şablonuna ihtiyaç duyuyor: **cihaz eşleme** ve **fhır eşleme**. IoT Bağlayıcınız, Bu eşlemeler karşıya yüklenene kadar tam olarak çalışmaz.

[![IoT Bağlayıcısı 'nda eşlemeler yok](media/quickstart-iot-fhir-portal/portal-iot-connector-missing-mappings.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-missing-mappings.jpg#lightbox)

Eşleme şablonlarını karşıya yüklemek için, yeni dağıtılan IoT bağlayıcısına tıklayarak **IoT Bağlayıcısı** sayfasına gidin.

[![IoT Bağlayıcısı tıklama](media/quickstart-iot-fhir-portal/portal-iot-connector-click.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click.jpg#lightbox)

#### <a name="device-mapping"></a>Cihaz eşleme

Cihaz eşleme şablonu, cihaz verilerini Normalleştirilmemiş bir şemaya dönüştürür. **IoT Bağlayıcısı** sayfasında cihaz eşlemeyi **Yapılandır** düğmesine tıklayarak **cihaz eşleme** sayfasına gidin. 

[![IoT Bağlayıcısı cihaz eşlemesini yapılandır seçeneğine tıklayın](media/quickstart-iot-fhir-portal/portal-iot-connector-click-device-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-device-mapping.jpg#lightbox)

**Cihaz eşleme** SAYFASıNDA, JSON düzenleyicisine aşağıdaki betiği ekleyin ve **Kaydet**' e tıklayın.

```json
{
  "templateType": "CollectionContent",
  "template": [
    {
      "templateType": "IotJsonPathContent",
      "template": {
        "typeName": "heartrate",
        "typeMatchExpression": "$..[?(@Body.HeartRate)]",
        "patientIdExpression": "$.SystemProperties.iothub-connection-device-id",
        "values": [
          {
            "required": "true",
            "valueExpression": "$.Body.HeartRate",
            "valueName": "hr"
          }
        ]
      }
    }
  ]
}
```

[![IoT Bağlayıcısı cihaz eşleme](media/quickstart-iot-fhir-portal/portal-iot-device-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-device-mapping.jpg#lightbox)


#### <a name="fhir-mapping"></a>FHıR eşleme

FHıR eşleme şablonu, normalleştirilmiş bir iletiyi FHıR tabanlı bir gözlem kaynağına dönüştürür. **IoT Bağlayıcısı** sayfasında **Fhır eşlemeyi Yapılandır** düğmesine tıklayarak **fhır eşleme** sayfasına gidin.  

[![IoT Bağlayıcısı configure FHıR Mapping 'e tıklayın](media/quickstart-iot-fhir-portal/portal-iot-connector-click-fhir-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-fhir-mapping.jpg#lightbox)

**Fhır eşleme** SAYFASıNDA, JSON düzenleyicisine aşağıdaki betiği ekleyin ve **Kaydet**' e tıklayın.

```json
{
  "templateType": "CollectionFhir",
  "template": [
    {
      "templateType": "CodeValueFhir",
      "template": {
        "codes": [
          {
            "code": "8867-4",
            "system": "http://loinc.org",
            "display": "Heart rate"
          }
        ],
        "periodInterval": 0,
        "typeName": "heartrate",
        "value": {
          "unit": "count/min",
          "valueName": "hr",
          "valueType": "Quantity"
        }
      }
    }
  ]
}
```

[![IoT Bağlayıcısı FHıR eşleme](media/quickstart-iot-fhir-portal/portal-iot-fhir-mapping.jpg)](media/quickstart-iot-fhir-portal/portal-iot-fhir-mapping.jpg#lightbox)

## <a name="generate-a-connection-string"></a>Bağlantı dizesi oluşturma

IoMT cihazının bağlanmak ve IoT bağlayıcısına ileti göndermek için bir bağlantı dizesi gerekir. Yeni dağıtılan IoT bağlayıcısının **IoT Bağlayıcısı** sayfasında **istemci bağlantılarını yönet** düğmesini seçin. 

[![IoT Bağlayıcısı istemci bağlantılarını yönet ' e tıklayın](media/quickstart-iot-fhir-portal/portal-iot-connector-click-client-connections.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-click-client-connections.jpg#lightbox)

**Bağlantılar** sayfasında, **Ekle** düğmesine tıklayarak yeni bir bağlantı oluşturun. 

[![IoT Bağlayıcısı bağlantıları](media/quickstart-iot-fhir-portal/portal-iot-connections.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connections.jpg#lightbox)

Bu bağlantı için, kaplama penceresinde kolay bir ad sağlayın ve **Oluştur** düğmesini seçin.

[![IoT Bağlayıcısı yeni bağlantı](media/quickstart-iot-fhir-portal/portal-iot-new-connection.jpg)](media/quickstart-iot-fhir-portal/portal-iot-new-connection.jpg#lightbox)

**Bağlantılar** sayfasından yeni oluşturulan bağlantıyı seçin ve **birincil bağlantı dizesi** alanının değerini sağdaki yer paylaşımı penceresinden kopyalayın.

[![IoT Bağlayıcısı bağlantı dizesi](media/quickstart-iot-fhir-portal/portal-iot-connection-string.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connection-string.jpg#lightbox)

Daha sonraki bir adımda kullanılmak üzere bu bağlantı dizesini koru. 

## <a name="connect-your-devices-to-iot"></a>Cihazlarınızı IoT 'ye bağlama

Azure, IoT cihazlarınızı bağlamak ve yönetmek için kapsamlı bir IoT ürünleri paketi sunar. Azure IoT Hub kullanarak PaaS 'yi temel alarak kendi çözümünüzü oluşturabilir veya Azure IoT Central ile IoT uygulamalarını yönetme platformuyla başlayabilirsiniz. Bu öğreticide, başlamanıza yardımcı olması için sektör odaklı çözüm şablonlarına sahip Azure IoT Central 'dan yararlanacağız.

[Sürekli hasta izleme uygulaması şablonunu](https://docs.microsoft.com/azure/iot-central/healthcare/tutorial-continuous-patient-monitoring#create-an-application-template)dağıtın. Bu şablon, kullanmaya başlamanıza yardımcı olmak için gerçek zamanlı veriler üreten iki sanal cihaz içerir: **akıllı Vintals yaması** ve **akıllı Knee ayracı**.

> [!NOTE]
> Gerçek cihazlarınız her seferinde, [cihazlarınızı](https://docs.microsoft.com/azure/iot-central/core/howto-set-up-template) eklemek ve cihaz simülatörleri değiştirmek için aynı IoT Central uygulamasını kullanabilirsiniz. Cihaz verileriniz de otomatik olarak FHıR 'ye akar. 

## <a name="connect-your-iot-data-with-the-iot-connector-preview"></a>IoT veri bağlayıcınızı IoT Bağlayıcısı ile bağlama (Önizleme)
IoT Central Uygulamanızı dağıttıktan sonra, iki kullanıma hazır sanal aygıtınız telemetri oluşturmaya başlar. Bu öğreticide, IoT Bağlayıcısı aracılığıyla *akıllı Vintals düzeltme* simülatörü Ile FHıR 'ye telemetri sunuyoruz. IoT verilerinizi IoT bağlayıcısına aktarmak için [IoT Central içinde sürekli veri dışarı aktarma](https://docs.microsoft.com/azure/iot-central/core/howto-export-data#set-up-data-export)kurmak istiyoruz. Sürekli veri dışa aktarma sayfasında:
- Dışarı aktarma hedefi olarak *Azure Event Hubs* 'yi seçin.
- **Event Hubs ad alanı** alanı için *bağlantı dizesi değeri kullan* ' ı seçin.
- **Bağlantı dizesi** alanı için önceki bir adımda elde edilen IoT bağlayıcısının bağlantı dizesini sağlayın.
- **Verilerin dışarı aktarılması** için **telemetri** seçeneğini *Açık* tutun.

## <a name="view-device-data-in-azure-api-for-fhir"></a>FHıR için Azure API 'de cihaz verilerini görüntüleme

IoT Bağlayıcısı tarafından oluşturulan FHıR tabanlı gözlemler kaynağını, Postman kullanarak FHıR için Azure API üzerinde görüntüleyebilirsiniz. [Fhır Için Azure API 'sine erişmek üzere Postman](access-fhir-postman-tutorial.md) 'nizi ayarlayın ve `GET` `https://your-fhir-server-url/Observation?code=http://loinc.org|8867-4` sinyal oranı değeri olan gözehır kaynaklarını görüntülemek için bir istek yapın. 

> [!TIP]
> Kullanıcının FHıR veri düzlemi için Azure API 'sine uygun erişimi olduğundan emin olun. Gerekli veri düzlemi rollerini atamak için [Azure rol tabanlı erişim denetimi 'ni (Azure RBAC)](configure-azure-rbac.md) kullanın.

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık gerekli değilse, ilişkili kaynak grubunu veya FHıR hizmeti için ilişkili Azure API 'sini veya IoT Bağlayıcısı örneğinin kendisini kaldırarak IoT bağlayıcısının bir örneğini silebilirsiniz. 

IoT Bağlayıcısı örneğini doğrudan kaldırmak için IoT **bağlayıcıları** sayfasından örnek ' i seçerek **IoT Bağlayıcısı** sayfasına gidin ve **Sil** düğmesine tıklayın. Onay sorulduğunda **Evet** ' i seçin. 

[![IoT Bağlayıcısı örneğini Sil](media/quickstart-iot-fhir-portal/portal-iot-connector-delete.jpg)](media/quickstart-iot-fhir-portal/portal-iot-connector-delete.jpg#lightbox)

## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıç kılavuzunda, FHıR kaynağı için Azure API 'nize IoT Bağlayıcısı özelliğini dağıttık. IoT Bağlayıcısı hakkında daha fazla bilgi edinmek için aşağıdaki adımları aşağıdan seçin:

IoT Bağlayıcısı içindeki farklı veri akışı aşamalarını anlayın.

>[!div class="nextstepaction"]
>[IoT Bağlayıcısı veri akışı](iot-data-flow.md)

Cihaz ve FHıR eşleme şablonlarını kullanarak IoT bağlayıcısını nasıl yapılandıracağınızı öğrenin.

>[!div class="nextstepaction"]
>[IoT Bağlayıcısı eşleme şablonları](iot-mapping-templates.md)

FHIR, HL7’nin kayıtlı ticari markasıdır ve HL7’nin izniyle kullanılır.
