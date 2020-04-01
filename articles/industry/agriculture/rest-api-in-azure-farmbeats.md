---
title: Başvurular
description: ''
author: sunasing
ms.topic: article
ms.date: 11/04/2019
ms.author: sunasing
ms.openlocfilehash: 717a09d8377a7b95fe24300cc65222f307e7419f
ms.sourcegitcommit: ced98c83ed25ad2062cc95bab3a666b99b92db58
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2020
ms.locfileid: "80437516"
---
# <a name="references"></a>Başvurular

Bu makalede, Azure FarmBeats API'leri açıklanmaktadır.

## <a name="rest-api"></a>REST API

Azure FarmBeats API'leri, aşağıdakiler gibi Azure FarmBeats özelliklerinden yararlanmanıza yardımcı olmak için tarımsal işletmelere JSON tabanlı yanıtlar içeren standart laştırılmış restif bir arayüz sağlar:

- Api'ler sensör, kamera, insansız hava aracı, hava durumu, uydu ve küratörlü yer verilerini almak için.
- Genel veri sağlayıcıları arasında verilerin normalleştirilmesi ve bağlamsallaştırılması.
- Tüm yutulan verilerde şematize erişim ve sorgu yetenekleri.
- Agronomik özelliklere dayalı olarak sorgulanabilen otomatik meta veri üretimi.
- Hızlı model oluşturma için otomatik olarak oluşturulan zaman dizisi agregaları.
- Özel veri işleme ardışık hatlar oluşturmak için tümleşik Azure Veri Fabrikası motoru.

## <a name="application-development"></a>Uygulama geliştirme

FarmBeats API'leri Swagger teknik dokümantasyon içerir. Tüm API'ler ve bunların karşılık gelen istek leri veya yanıtları hakkında bilgi [için](https://aka.ms/FarmBeatsDatahubSwagger)Bkz.

Aşağıdaki tablo FarmBeats Datahub'daki tüm nesneleri ve kaynakları özetleyebilir:

| Nesneler ve kaynaklar | Açıklama
--- | ---|
Çiftlik | Çiftlik FarmBeats sistemi içinde ilgi fiziksel bir konuma karşılık gelir. Her çiftliğin bir çiftlik adı ve benzersiz bir çiftlik kimliği vardır. |
Cihaz  | Cihaz çiftlikte bulunan fiziksel bir cihaza karşılık gelir. Her aygıtın benzersiz bir aygıt kimliği vardır. Aygıt genellikle çiftlik kimliği olan bir çiftliğe verilir.
DeviceModel  | DeviceModel, ağ geçidi veya düğüm olan üretici ve aygıt türü gibi aygıtın meta verilerine karşılık gelir.
Sensör  | Sensör, değerleri kaydeden fiziksel bir sensöre karşılık gelir. Sensör genellikle aygıt kimliğine sahip bir aygıta bağlanır.
Sensör Modeli  | SensorModel, üretici, analog veya dijital sensör tipi ve ortam sıcaklığı ve basınç gibi sensör ölçümü gibi sensörün meta verilerine karşılık gelir.
Telemetri  | Telemetri, belirli bir sensör ve zaman aralığı için telemetri mesajlarını okuma olanağı sağlar.
İş  | İş, farmbeats sisteminde istenilen çıktıyı elde etmek için yürütülen herhangi bir iş akışına karşılık gelir. Her iş bir iş kimliği ve iş türü ile ilişkilidir.
İş Türü  | JobType, sistem tarafından desteklenen farklı iş türlerine karşılık gelir. Sistem tanımlı ve kullanıcı tanımlı iş türleri dahildir.
Genişletilmiş Tip  | ExtendedType, sistemdeki sistem ve kullanıcı tanımlı türler listesine karşılık gelir. ExtendedType FarmBeats sisteminde yeni bir sensör, sahne veya sahne dosyası türü kurmaya yardımcı olur.
İş Ortağı  | İş ortağı FarmBeats için sensör ve görüntü entegrasyon ortağı na karşılık gelir.
Sahne  | Sahne, bir çiftlik bağlamında oluşturulan herhangi bir çıktıya karşılık gelir. Her sahnenin bir sahne kimliği, sahne kaynağı, sahne türü ve onunla ilişkili çiftlik kimliği vardır. Her sahne kimliğiyle ilişkili birden çok sahne dosyası olabilir.
Sahne Dosyası |SceneFile, tek bir sahne için oluşturulan tüm dosyalara karşılık gelir. Tek bir sahne kimliğiyle ilişkili birden çok SceneFile Kimliği olabilir.
Kural  |Kural, çiftlikle ilgili verilerin bir uyarıyı tetikleme durumuna karşılık gelir. Her kural bir çiftliğin verileri bağlamındadır.
Uyarı  | Uyarı, kural koşulu karşılandığında oluşturulan bir bildirime karşılık gelir. Her uyarı bir kural bağlamındadır.
RoleDefinition  | RoleDefinition, bir rol için izin verilen ve izin verilmeyen eylemleri tanımlar.
RoleAssignment  |RoleAssignment, bir rolün bir kullanıcıya veya hizmet ilkesine atanmasına karşılık gelir.

### <a name="data-format"></a>Veri biçimi

JSON, rasgele veri yapılarının basit bir metin gösterimi sağlayan ortak bir dilden bağımsız veri biçimidir. Daha fazla bilgi için [JSON web sitesine](https://www.json.org/)bakın.

## <a name="authentication-and-authorization"></a>Kimlik doğrulama ve yetkilendirme

REST API'deki HTTP istekleri Azure Etkin Dizini (Azure AD) ile korunur.
REST API'lerine kimlik doğrulaması yapmak için, ISTEMCI kodunun API'yi aramadan önce geçerli kimlik bilgilerini içeren kimlik doğrulaması gerekir. Kimlik doğrulama, Azure AD tarafından çeşitli aktörler arasında koordine edilir. Müşterinize kimlik doğrulamanın kanıtı olarak bir erişim belirteci sağlar. Belirteç daha sonra REST API isteklerinin HTTP Yetkilendirme üstbilgisinde gönderilir. Azure AD kimlik doğrulaması hakkında daha fazla bilgi edinmek için geliştiriciler için [Azure Etkin Dizini'ne](https://portal.azure.com) bakın.

Erişim belirteci sonraki API isteklerinde, üstbilgi bölümünde aşağıdaki gibi gönderilmelidir:

```
headers = {"Authorization": "Bearer " + **access_token**}
```

### <a name="http-request-headers"></a>HTTP istek üstbilgi

Azure FarmBeats Datahub'a API çağrısı yaptığınızda belirtmeniz gereken en yaygın istek üstbilgileri aşağıda veda eder.


**Üst bilgi** | **Açıklama ve örnek**
--- | ---
İçerik Türü  | İstek biçimi (İçerik Türü:<format>uygulama/ ). Azure FarmBeats Datahub API'leri için biçim JSON'dur. İçerik Türü: uygulama/json
Yetkilendirme  | API araması yapmak için gereken erişim belirteci belirtir. Yetkilendirme: Taşıyıcı <Erişim-Belirteç>
Kabul Et | Yanıt biçimi. Azure FarmBeats Datahub API'leri için biçim JSON'dur. Kabul Et: uygulama/json

### <a name="api-requests"></a>API istekleri

REST API isteği nde bulunmak için HTTP (GET, POST, PUT veya DELETE) yöntemini, API hizmetine URL'yi, URI'yi sorgulanacak bir kaynağa, verileri sorgulamak, güncellemek veya silmek ve sonra bir veya daha fazla HTTP istek üstbilgisini ekleyin.

API hizmetinin URL'si, örneğin datahub\<web sitesi adınız>.azurewebsites.net'https:// Datahub URL'nizdir.

İsteğe bağlı olarak, yanıtlarda verileri filtrelemek, boyutunu sınırlamak ve sıralamak için GET çağrılarına sorgu parametrelerini ekleyebilirsiniz.

Aygıtların listesini almak için aşağıdaki örnek istek kullanılır:

```bash
curl -X GET "https://microsoft-farmbeats.azurewebsites.net/Device" -H "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>”
```

Çoğu GET, POST ve PUT aramaları bir JSON istek gövdesi gerektirir.

Aşağıdaki örnek istek bir aygıt oluşturur. Bu istek, JSON'ı istek gövdesine girdi.

```bash
curl -X POST "https://microsoft-farmbeats.azurewebsites.net/Device" -H  "accept: application/json" -H  "Content-Type: application/json" -H "Authorization: Bearer <Access-Token>" -d "{  \"deviceModelId\": \"ID123\",  \"hardwareId\": \"MHDN123\",  \"reportingInterval\": 900,  \"name\": \"Device123\",  \"description\": \"Test Device 123\",}"
```

### <a name="query-parameters"></a>Sorgu parametreleri

REST GET aramaları için, istek URI'ye bir veya daha fazla sorgu parametresi ekleyerek API yanıtındaki verileri filtreleyebilir, boyutunu sınırlayabilir ve sıralayabilirsiniz. Sorgu parametreleri için API belgelerine ve tek tek GET çağrılarına bakın.
Örneğin, aygıtların listesini sorgularken (GET call on /Device), aşağıdaki sorgu parametreleri belirtilebilir:

![Aygıtlar listesi](./media/references-for-azure-farmbeats/query-parameters-device-1.png)

### <a name="error-handling"></a>Hata işleme

Azure FarmBeats Datahub API'leri standart HTTP hatalarını döndürür. En yaygın hata kodları aşağıdaki gibidir:

 |Hata kodu             | Açıklama |
 |---                    | --- |
 |200                    | Başarılı |
 |201                    | Create (Post) Başarı |
 |400                    | Kötü istek. İstekte bir hata var. |
 |401                    | Yetki -siz. API'yi arayan kişinin kaynağa erişme yetkisi yoktur. |
 |404                    | Kaynak Bulunamadı |
 |5XX                    | İç Sunucu hatası. 5XX ile başlayan hata kodları sunucuda bazı hata lar olduğu anlamına gelir. Daha fazla bilgi için sunucu günlüklerine ve aşağıdaki bölüme bakın. |


Standart HTTP hatalarına ek olarak, Azure FarmBeats Datahub API'leri de dahili hataları aşağıdaki biçimde döndürür:

```json
    {
      "message": "<More information on the error>",
      "status": "<error code>”,
      "code": "<InternalErrorCode>",
      "moreInfo": "<Details of the error>"
    }
```

Bu örnekte, bir çiftlik oluşturulduğunda, zorunlu alan "Ad" giriş yükünde belirtilmedi. Ortaya çıkan hata iletisi olacaktır:

 ```json    
    {
      "message": "Model validation failed",
      "status": 400,
      "code": "ModelValidationFailed",
      "moreInfo": "[\"The Name field is required.\"]"
    }
  ```

## <a name="add-users-or-app-registrations-to-azure-active-directory"></a>Azure Etkin Dizin'e kullanıcı veya uygulama kaydı ekleme

Azure FarmBeats API'lerine Azure Active Directory'de bir kullanıcı veya uygulama kaydı tarafından erişilebilir. Azure Etkin Dizin'de bir uygulama kaydı oluşturmak için aşağıdaki adımları izleyin:

1. [Azure portalına](https://portal.azure.com)gidin ve **Azure Active Directory** > **App kayıtlarını** > seçin**Yeni kayıt**. Alternatif olarak, varolan bir hesabı kullanabilirsiniz.
2. Yeni bir hesap için aşağıdakileri yapın:

    - Bir ad girin.
    - **Yalnızca bu kuruluş dizinindeki Hesapları seçin (Tek kiracı)**.
    - Alanların geri kalanında varsayılan değerleri kullanın.
    - **Kaydol**’u seçin.

3. Yeni ve mevcut uygulama kaydı **Genel Bakış** bölmesinde aşağıdakileri yapın:

    - **İstemci kimliğini** ve **Kiracı Kimliğini**yakalayın.
    - Yeni bir istemci sırrı oluşturmak ve **Müşteri-Gizli**yakalamak için **Sertifikalar ve Sırlar** gidin.
    - **Genel Bakış'a**geri dön ve **yerel dizinde Uygulamayı Yönet'in**yanındaki bağlantıyı seçin.
    - Nesne Kimliğini yakalamak için **Özellikler'e**gidin. **Properties**

4. Datahub Swagger'ınıza<yourdatahub>gidin (https:// .azurewebsites.net/swagger/index.html) ve aşağıdakileri yapın:
    - **RoleAssignment API'ye**gidin.
    - Az önce oluşturduğunuz **Nesne Kimliği** için **roleassignment** nesnesi oluşturmak için bir POST gerçekleştirin.

  > [!NOTE]
  > Kullanıcı ekleme ve Etkin Dizin kaydı hakkında daha fazla bilgi için [Azure Active Directory'ye](https://docs.microsoft.com/azure/active-directory/develop/howto-create-service-principal-portal)bakın.

Önceki adımları tamamladıktan sonra, uygulama kaydınız (istemci) taşıyıcı kimlik doğrulaması aracılığıyla bir erişim belirteci kullanarak Azure FarmBeats API'lerini arayabilir.

Üstbilgi bölümünde sonraki API isteklerinde göndermek için erişim belirteci kullanın:

```
headers = {"Authorization": "Bearer " + **access_token**, "Content-Type" : "application/json" }
```
