---
title: Sistem durumu için Metin Analizi kullanma
titleSuffix: Azure Cognitive Services
description: Sağlık bilgilerini, sistem durumu için Metin Analizi ile yapılandırılmamış klinik metinden nasıl ayıklayacağınızı ve etiketleyeceğinizi öğrenin.
services: cognitive-services
author: aahill
manager: nitinme
ms.service: cognitive-services
ms.subservice: text-analytics
ms.topic: conceptual
ms.date: 08/06/2020
ms.author: aahi
ms.openlocfilehash: 4ba7aa530699ab0e06ac42e3701265254b617f73
ms.sourcegitcommit: c28fc1ec7d90f7e8b2e8775f5a250dd14a1622a6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/13/2020
ms.locfileid: "88167700"
---
# <a name="how-to-use-text-analytics-for-health-preview"></a>Nasıl yapılır: sistem durumu için Metin Analizi kullanma (Önizleme)

> [!NOTE]
> Sistem durumu kapsayıcısı için Metin Analizi yakın zamanda güncelleştirildi. Son değişiklikler hakkında daha fazla [bilgi için bkz. yenilikler.](../whats-new.md) Listelenen güncelleştirmeleri kullanmak için en son kapsayıcıyı çekmeyi unutmayın.

> [!IMPORTANT] 
> Sistem durumu Metin Analizi, "olduğu gıbı" ve "tüm hatalarıyla bırlıkte" sağlanmış bir önizleme özelliğidir. Bu nedenle, **sistem durumu için metin analizi (Önizleme) herhangi bir üretim kullanımı için uygulanmamalıdır veya dağıtılmamalıdır.** Sağlık durumu için Metin Analizi, bir tıbbi cihaz, klinik destek, tanılama aracı ya da tanılama, sağlama, azaltma, işleme veya önlemeye yönelik başka bir teknoloji veya Microsoft tarafından bu amaçla kullanılması amaçlanan bir lisans ya da hak sağlanmaz. Bu özellik, profesyonel tıp önerisi veya sağlık görüşlerine, tanılama, işleme ya da sağlık uzmanı 'nın klinik yargılarına veya bu şekilde kullanılmamalıdır. Müşteri, sistem durumu için Metin Analizi herhangi bir kullanım işleminden yalnızca sorumludur. Microsoft, sağlık veya yetenek ile bağlantılı olarak belirtilen herhangi bir malzeme için Metin Analizi, herhangi bir kişinin sistem durumunu veya tıbbi gereksinimlerini karşılamasını garanti etmez. 


Sistem durumu Metin Analizi, Doktor notları, disşarj özetleri, klinik belgeler ve elektronik sistem durumu kayıtları gibi yapılandırılmamış metinlerden ilgili tıbbi bilgileri çıkaran ve etiketleyen kapsayıcılı bir hizmettir.  

## <a name="features"></a>Özellikler

Sistem durumu kapsayıcısı için Metin Analizi Şu anda adlandırılmış varlık tanıma (NER), ilişki ayıklama, varlık olumsuzlama ve kendi geliştirme ortamınızda, belirli güvenlik ve veri idare gereksinimlerinizi karşılayan Ingilizce metin için varlık bağlama işlemini gerçekleştiriyor.

#### <a name="named-entity-recognition"></a>[Adlandırılmış Varlık Tanıma](#tab/ner)

Adlandırılmış varlık tanıma, bir veya daha fazla anlamsal tür ile ilişkilendirilebilen (örn. tanılama, yollara adı, belirti/işaret veya yaş gibi), yapılandırılmamış metinde belirtilen kelimeleri ve tümceleri algılar.

> [!div class="mx-imgBorder"]
> ![Sağlık durumu](../media/ta-for-health/health-named-entity-recognition.png)

#### <a name="relation-extraction"></a>[İlişki ayıklama](#tab/relation-extraction)

İlişki ayıklama metinde bahsedilen kavramlar arasındaki anlamlı bağlantıları tanımlar. Örneğin, bir koşul adı bir saat ile ilişkilendirerek bir "koşul süresi" ilişkisi bulunur. 

> [!div class="mx-imgBorder"]
> ![Sağlık YENIDEN](../media/ta-for-health/health-relation-extraction.png)


#### <a name="entity-linking"></a>[Varlık Bağlama](#tab/entity-linking)

Varlık bağlama, metinde bahsedilen adlandırılmış varlıkları, önceden tanımlanmış bir kavram veritabanında bulunan kavramlara ilişkilendirerek ayrı varlıkların ayırt düzeyini ortadan kaldırarak ayırt ediyor. Örneğin, Birleşik Tıbbi dil sistemi (UMLS).

> [!div class="mx-imgBorder"]
> ![Sağlık EL](../media/ta-for-health/health-entity-linking.png)

Sistem durumu Metin Analizi Birleşik Tıbbi dil sistemi ([Umls](https://www.nlm.nih.gov/research/umls/sourcereleasedocs/index.html)) Metaeşanlamlılar bilgi kaynağında bulunan sağlık ve biotıbbi sözcük dağarcıklarını bağlamayı destekler.

#### <a name="negation-detection"></a>[Algılama Olumsuzlaştırma](#tab/negation-detection) 

Tıbbi içeriğin anlamı, yanlış tanılanıyor durumunda önemli bir engel olabilecek Olumsuzlaştırma gibi değiştiricilerin büyük ölçüde etkilenir. Sistem durumu Metin Analizi, metinde bahsedilen farklı varlıkların algılanmasını desteklemiyor. 

> [!div class="mx-imgBorder"]
> ![Sistem durumu NEG](../media/ta-for-health/health-negation.png)

---

Desteklenen varlıkların tam listesi için sistem durumu için Metin Analizi tarafından döndürülen [varlık kategorilerine](../named-entity-types.md?tabs=health) bakın.

## <a name="supported-languages"></a>Desteklenen diller

Sistem durumu Metin Analizi yalnızca Ingilizce dil belgelerini destekler.

## <a name="request-access-to-the-container-registry"></a>Kapsayıcı kayıt defterine erişim isteme

Kapsayıcıya erişim istemek için bilişsel [Hizmetler kapsayıcıları istek formunu](https://aka.ms/cognitivegate) doldurun ve iletin. Şu anda, sistem durumu kullanımı için Metin Analizi faturalandırılmaz. 

[!INCLUDE [Request access to the container registry](../../../../includes/cognitive-services-containers-request-access-only.md)]

[!INCLUDE [Authenticate to the container registry](../../../../includes/cognitive-services-containers-access-registry.md)]

## <a name="install-the-container"></a>Kapsayıcıyı yükler

Kapsayıcıyı yükleyip çalıştırmak için birden çok yol vardır. 

- Bir Metin Analizi kaynağı oluşturmak için [Azure Portal](text-analytics-how-to-install-containers.md?tabs=healthcare) kullanın ve kapsayıcınızı almak Için Docker 'ı kullanın.
- Kaynak dağıtımı kapsayıcı yapılandırmasını otomatikleştirmek için aşağıdaki PowerShell ve [Azure CLI](https://docs.microsoft.com/cli/azure/?view=azure-cli-latest) betikleri kullanın.

### <a name="install-the-container-using-azure-web-app-for-containers"></a>Azure Kapsayıcılar için Web App kullanarak kapsayıcıyı yüklerken

Azure [kapsayıcılar için Web App](https://azure.microsoft.com/services/app-service/containers/) , bulutta kapsayıcılar çalıştırmak için adanmış bir Azure kaynağıdır. Otomatik ölçeklendirme, Docker kapsayıcıları ve Docker Compose desteği, HTTPS desteği ve çok daha fazlası gibi kullanıma hazır özellikleri getirir.

> [!NOTE]
> Azure Web App 'i kullanarak bir etki alanını otomatik olarak şu biçimde alacaksınız`<appservice_name>.azurewebsites.net`

Aboneliğinizi ve HTTPS üzerinden kapsayıcı görüntüsünü kullanarak bir Kapsayıcılar için Web App oluşturmak için Azure CLı kullanarak bu PowerShell betiğini çalıştırın. İlk isteği göndermeden önce betiğin (yaklaşık 25-30 dakika) tamamlanmasını bekleyin.

```bash
$subscription_name = ""                    # THe name of the subscription you want you resource to be created on.
$resource_group_name = ""                  # The name of the resource group you want the AppServicePlan
                                           #    and AppSerivce to be attached to.
$resources_location = ""                   # This is the location you wish the AppServicePlan to be deployed to.
                                           #    You can use the "az account list-locations -o table" command to
                                           #    get the list of available locations and location code names.
$appservice_plan_name = ""                 # This is the AppServicePlan name you wish to have.
$appservice_name = ""                      # This is the AppService resource name you wish to have.
$TEXT_ANALYTICS_RESOURCE_API_KEY = ""      # This should be taken from the Text Analytics resource.
$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT = "" # This should be taken from the Text Analytics resource.
$DOCKER_REGISTRY_SERVER_PASSWORD = ""      # This will be provided separately.
$DOCKER_REGISTRY_SERVER_USERNAME = ""      # This will be provided separately.
$DOCKER_IMAGE_NAME = "containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest"

az login
az account set -s $subscription_name
az appservice plan create -n $appservice_plan_name -g $resource_group_name --is-linux -l $resources_location --sku P3V2
az webapp create -g $resource_group_name -p $appservice_plan_name -n $appservice_name -i $DOCKER_IMAGE_NAME -s $DOCKER_REGISTRY_SERVER_USERNAME -w $DOCKER_REGISTRY_SERVER_PASSWORD
az webapp config appsettings set -g $resource_group_name -n $appservice_name --settings Eula=accept Billing=$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT ApiKey=$TEXT_ANALYTICS_RESOURCE_API_KEY

# Once deployment complete, the resource should be available at: https://<appservice_name>.azurewebsites.net
```

### <a name="install-the-container-using-azure-container-instance"></a>Azure Container Instance kullanarak kapsayıcıyı yükler

Ayrıca, dağıtımı kolaylaştırmak için bir Azure Container Instance (acı) kullanabilirsiniz. ACI, yönetilen, sunucusuz bir Azure ortamında Docker kapsayıcılarını isteğe bağlı olarak çalıştırmanıza olanak tanıyan bir kaynaktır. 

Azure portal kullanarak ACI kaynağını dağıtma adımları için bkz. [Azure Container Instances nasıl kullanılır](text-analytics-how-to-use-container-instances.md) . Ayrıca, kapsayıcı görüntüsünü kullanarak aboneliğinizde bir ACI oluşturacak olan Azure CLı kullanarak aşağıdaki PowerShell betiğini de kullanabilirsiniz.  İlk isteği göndermeden önce betiğin (yaklaşık 25-30 dakika) tamamlanmasını bekleyin.  ACI kaynağı başına en fazla CPU sayısı sınırı nedeniyle, her istek için 5 ' ten fazla büyük belge (yaklaşık 5000 karakter) göndermeyi düşünüyorsanız bu seçeneği seçmeyin.
Kullanılabilirlik bilgileri için [acı bölgesel destek](https://docs.microsoft.com/azure/container-instances/container-instances-region-availability) makalesine bakın. 

> [!NOTE] 
> Azure Container Instances Builtin Domains için HTTPS desteği eklemeyin. HTTPS gerekiyorsa, sertifika oluşturma ve bir etki alanını kaydetme dahil olmak üzere el ile yapılandırmanız gerekir. Bunu, aşağıdaki NGıNX ile yapmak için yönergeler bulabilirsiniz.

```bash
$subscription_name = ""                    # The name of the subscription you want you resource to be created on.
$resource_group_name = ""                  # The name of the resource group you want the AppServicePlan
                                           # and AppService to be attached to.
$resources_location = ""                   # This is the location you wish the web app to be deployed to.
                                           # You can use the "az account list-locations -o table" command to
                                           # Get the list of available locations and location code names.
$azure_container_instance_name = ""        # This is the AzureContainerInstance name you wish to have.
$TEXT_ANALYTICS_RESOURCE_API_KEY = ""      # This should be taken from the Text Analytics resource.
$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT = "" # This should be taken from the Text Analytics resource.
$DOCKER_REGISTRY_SERVER_PASSWORD = ""      # This will be provided separately.
$DOCKER_REGISTRY_SERVER_USERNAME = ""      # This will be provided separately.
$DNS_LABEL = ""                            # This is the DNS label name you wish your ACI will have
$DOCKER_REGISTRY_LOGIN_SERVER = "containerpreview.azurecr.io"
$DOCKER_IMAGE_NAME = "containerpreview.azurecr.io/microsoft/cognitive-services-healthcare:latest"

az login
az account set -s $subscription_name
az container create --resource-group $resource_group_name --name $azure_container_instance_name --image $DOCKER_IMAGE_NAME --cpu 4 --memory 12 --registry-login-server $DOCKER_REGISTRY_LOGIN_SERVER --registry-username $DOCKER_REGISTRY_SERVER_USERNAME --registry-password $DOCKER_REGISTRY_SERVER_PASSWORD --port 5000 --dns-name-label $DNS_LABEL --environment-variables Eula=accept Billing=$TEXT_ANALYTICS_RESOURCE_API_ENDPOINT ApiKey=$TEXT_ANALYTICS_RESOURCE_API_KEY

# Once deployment complete, the resource should be available at: http://<unique_dns_label>.<resource_group_region>.azurecontainer.io:5000
```

### <a name="secure-aci-connectivity"></a>Güvenli acı bağlantısı

Varsayılan olarak, kapsayıcı API 'SI ile acı kullanılırken bir güvenlik sağlanmaz. Bunun nedeni genellikle kapsayıcıların bir ağ köprüsü tarafından dışından korunan Pod 'ın bir parçası olarak çalışacaktır. Ancak, kapsayıcı uç noktasını özel olarak tutarak bir kapsayıcıyı öne bakan bir bileşenle değiştirebilirsiniz. Aşağıdaki örneklerde, HTTPS/SSL ve istemci sertifikası kimlik doğrulamasını desteklemek için bir giriş ağ geçidi olarak [NGINX](https://www.nginx.com) kullanılmaktadır.

> [!NOTE]
> NGıNX açık kaynaklı, yüksek performanslı bir HTTP sunucusudur ve proxy 'dir. Bir NGıNX kapsayıcısı, tek bir kapsayıcı için bir TLS bağlantısını sonlandırmak üzere kullanılabilir. Daha karmaşık NGıNX girişi tabanlı TLS sonlandırma çözümleri de mümkündür.

#### <a name="set-up-nginx-as-an-ingress-gateway"></a>Giriş ağ geçidi olarak NGıNX ayarlama

NGıNX, çalışma zamanında özellikleri etkinleştirmek için [yapılandırma dosyalarını](https://docs.nginx.com/nginx/admin-guide/basic-functionality/managing-configuration-files/) kullanır. Başka bir hizmet için TLS sonlandırmasını etkinleştirmek üzere TLS bağlantısını sonlandırmak ve `proxy_pass` hizmet için bir adres belirtmek üzere BIR SSL sertifikası belirtmeniz gerekir. Aşağıda bir örnek verilmiştir.


> [!NOTE]
> `ssl_certificate`NGıNX kapsayıcısının yerel dosya sistemi içinde bir yol belirtilmesini bekler. İçin belirtilen adres, `proxy_pass` NGıNX kapsayıcısının ağı içinden kullanılabilir olmalıdır.

NGıNX kapsayıcısı, içindeki içindeki tüm dosyaları `_.conf_` `/etc/nginx/conf.d/` http yapılandırma yoluna yükleyecek.

```nginx
server {
  listen              80;
  return 301 https://$host$request_uri;
}
server {
  listen              443 ssl;
  # replace with .crt and .key paths
  ssl_certificate     /cert/Local.crt;
  ssl_certificate_key /cert/Local.key;

  location / {
    proxy_pass http://cognitive-service:5000;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_set_header X-Real-IP  $remote_addr;
  }
}
```

#### <a name="example-docker-compose-file"></a>Örnek Docker Compose dosyası

Aşağıdaki örnek, NGıNX ve sistem durumu kapsayıcıları için Metin Analizi dağıtmak üzere bir [Docker Compose](https://docs.docker.com/compose/reference/overview) dosyasının nasıl oluşturulacağını gösterir:

```yaml
version: "3.7"
services:
  cognitive-service:
    image: {IMAGE_ID}
    ports:
      - 5000:5000
    environment:
      - eula=accept
      - billing={ENDPOINT_URI}
      - apikey={API_KEY}
      - Logging:Disk:Format=json
    volumes:
        # replace with path to logs folder
      - <path-to-logs-folder>:/output
  nginx:
    image: nginx
    ports:
      - 443:443
    volumes:
        # replace with paths for certs and conf folders
      - <path-to-certs-folder>:/cert
      - <path-to-conf-folder>:/etc/nginx/conf.d/
```

Bu Docker Compose dosyasını başlatmak için, dosyanın kök düzeyindeki bir konsoldan aşağıdaki komutu yürütün:

```bash
docker-compose up
```

Daha fazla bilgi için, [NGıNX SSL sonlandırmasına](https://docs.nginx.com/nginx/admin-guide/security-controls/terminating-ssl-http/)ilişkin NGINX belgeleri bölümüne bakın.


## <a name="example-api-request"></a>Örnek API isteği
Kapsayıcı REST tabanlı sorgu tahmin uç noktası API’lerini sağlar.

Değişkeni uygun değerle değiştirerek dağıttığınız kapsayıcıya bir sorgu göndermek için aşağıdaki örnek kıvrımlı isteği kullanın `serverURL` .

```bash
curl -X POST 'http://<serverURL>:5000/text/analytics/v3.2-preview.1/entities/health' --header 'Content-Type: application/json' --header 'accept: application/json' --data-binary @example.json

```

Aşağıdaki JSON, sistem durumu API 'SI isteğinin GÖNDERI gövdesinde Metin Analizi eklenmiş bir JSON dosyası örneğidir:

```json
example.json

{
  "documents": [
    {
      "language": "en",
      "id": "1",
      "text": "Patient reported itchy sores after swimming in the lake."
    },
    {
      "language": "en",
      "id": "2",
      "text": "Prescribed 50mg benadryl, taken twice daily."
    }
  ]
}
```

## <a name="api-response-body"></a>API yanıt gövdesi

Aşağıdaki JSON, sistem durumu API 'SI yanıt gövdesi için Metin Analizi bir örneğidir:

```json
{
    "documents": [
        {
            "id": "1",
            "entities": [
                {
                    "id": "0",
                    "offset": 17,
                    "length": 11,
                    "text": "itchy sores",
                    "category": "SymptomOrSign",
                    "confidenceScore": 1.0,
                    "isNegated": false
                }
            ]
        },
        {
            "id": "2",
            "entities": [
                {
                    "id": "0",
                    "offset": 11,
                    "length": 4,
                    "text": "50mg",
                    "category": "Dosage",
                    "confidenceScore": 1.0,
                    "isNegated": false
                },
                {
                    "id": "1",
                    "offset": 16,
                    "length": 8,
                    "text": "benadryl",
                    "category": "MedicationName",
                    "confidenceScore": 1.0,
                    "isNegated": false,
                    "links": [
                        {
                            "dataSource": "UMLS",
                            "id": "C0700899"
                        },
                        {
                            "dataSource": "CHV",
                            "id": "0000044903"
                        },
                        {
                            "dataSource": "MMSL",
                            "id": "899"
                        },
                        {
                            "dataSource": "MSH",
                            "id": "D004155"
                        },
                        {
                            "dataSource": "NCI",
                            "id": "C300"
                        },
                        {
                            "dataSource": "NCI_DTP",
                            "id": "NSC0033299"
                        },
                        {
                            "dataSource": "PDQ",
                            "id": "CDR0000039163"
                        },
                        {
                            "dataSource": "PSY",
                            "id": "05760"
                        },
                        {
                            "dataSource": "RXNORM",
                            "id": "203457"
                        }
                    ]
                },
                {
                    "id": "2",
                    "offset": 32,
                    "length": 11,
                    "text": "twice daily",
                    "category": "Frequency",
                    "confidenceScore": 1.0,
                    "isNegated": false
                }
            ],
            "relations": [
                {
                    "relationType": "DosageOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/1/entities/0",
                    "target": "#/documents/1/entities/1"
                },
                {
                    "relationType": "FrequencyOfMedication",
                    "bidirectional": false,
                    "source": "#/documents/1/entities/2",
                    "target": "#/documents/1/entities/1"
                }
            ]
        }
    ],
    "errors": [],
    "modelVersion": "2020-07-24"
}
```

### <a name="negation-detection-output"></a>Algılama çıkışının Olumsuzlaştırma

Değilleme algılama kullanılırken, bazı durumlarda tek bir olumsuzlama bir kez birkaç terim ele alabilir. Tanınan bir varlığın değilleme, JSON çıktısında bayrağın Boolean değeri tarafından temsil edilir `isNegated` :

```json
{
  "id": "2",
  "offset": 90,
  "length": 10,
  "text": "chest pain",
  "category": "SymptomOrSign",
  "score": 0.9972,
  "isNegated": true,
  "links": [
    {
      "dataSource": "UMLS",
      "id": "C0008031"
    },
    {
      "dataSource": "CHV",
      "id": "0000023593"
    },
    ...
```

### <a name="relation-extraction-output"></a>İlişki ayıklama çıkışı

İlişki ayıklama çıkışı, ilişkinin *kaynağına* ve *hedefine*URI başvuruları içerir. İlişki rolü olan varlıklar `ENTITY` `target` alana atanır. İlişki rolü olan varlıklar `ATTRIBUTE` `source` alana atanır. Kısaltma ilişkileri çift yönlü `source` ve `target` alanları içerir ve `bidirectional` olarak ayarlanır `true` . 

```json
"relations": [
  {
      "relationType": "DosageOfMedication",
      "score": 1.0,
      "bidirectional": false,
      "source": "#/documents/2/entities/0",
      "target": "#/documents/2/entities/1",
      "entities": [
          {
              "id": "0",
              "role": "ATTRIBUTE"
          },
          {
              "id": "1",
              "role": "ENTITY"
          }
      ]
  },
...
]
```

## <a name="see-also"></a>Ayrıca bkz.

* [Metin Analizine genel bakış](../overview.md)
* [Adlandırılmış varlık kategorileri](../named-entity-types.md)
* [Yenilikler](../whats-new.md)
