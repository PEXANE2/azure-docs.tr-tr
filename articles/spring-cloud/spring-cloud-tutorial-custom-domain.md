---
title: Öğretici-mevcut bir özel etki alanını Azure Spring Cloud ile eşleme
description: Mevcut bir özel dağıtılmış ad hizmeti (DNS) adını Azure Spring Cloud 'a eşleme
author: MikeDodaro
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: brendm
ms.custom: devx-track-java
ms.openlocfilehash: 426336802f55b5f68d8c3e04cdb932b984c2adc7
ms.sourcegitcommit: 3d79f737ff34708b48dd2ae45100e2516af9ed78
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/23/2020
ms.locfileid: "87037228"
---
# <a name="map-an-existing-custom-domain-to-azure-spring-cloud"></a>Mevcut bir özel etki alanını Azure Spring Cloud ile eşleme
Dağıtılmış ad hizmeti (DNS), ağ düğümü adlarını ağ genelinde depolamanın bir tekniğidir. Bu öğretici, CNAME kaydı kullanarak www.contoso.com gibi bir etki alanını eşler. Özel etki alanının bir sertifikayla güvenliğini sağlar ve Güvenli Yuva Katmanı (SSL) olarak da bilinen Aktarım Katmanı Güvenliği 'ni (TLS) nasıl zorunlu hale kullanabileceğinizi gösterir. 

Sertifikalar Web trafiğini şifreler. Bu TLS/SSL sertifikaları, Azure Key Vault depolanabilir. 

## <a name="prerequisites"></a>Önkoşullar
* Azure yay bulutuna dağıtılan bir uygulama (bkz. [hızlı başlangıç: Azure Portal kullanarak mevcut bir Azure Spring Cloud uygulamasını başlatma](spring-cloud-quickstart-launch-app-portal.md)veya mevcut bir uygulamayı kullanma).
* GoDaddy gibi etki alanı sağlayıcısı için DNS kayıt defterine erişimi olan bir etki alanı adı.
* Bir üçüncü taraf sağlayıcıdan özel bir sertifika (otomatik olarak imzalanan sertifikanız). Sertifika, etki alanıyla aynı olmalıdır.
* [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) dağıtılan bir örnek

## <a name="import-certificate"></a>Sertifikayı içeri aktarma 
Bir sertifikayı içeri aktarma yordamı, pek veya PFX kodlamalı dosyanın diskte olması gerekir ve özel anahtara sahip olmanız gerekir. 

Sertifikanızı anahtar kasasına yüklemek için:
1. Anahtar Kasası örneğinize gidin.
1. Sol gezinti bölmesinde, **Sertifikalar**' a tıklayın.
1. Üstteki menüde **Oluştur/içeri aktar**' a tıklayın.
1. **Sertifika oluşturma yönteminin**altında **sertifika oluştur** iletişim kutusunda öğesini seçin `Import` .
1. **Sertifika dosyasını karşıya yükle**altında, sertifika konumuna gidin ve seçin.
1. **Parola**altında, sertifikanız için özel anahtarı girin.
1. **Oluştur**’a tıklayın.

    ![Sertifikayı içeri aktar 1](./media/custom-dns-tutorial/import-certificate-a.png)

Sertifikayı içeri aktarmadan önce Azure Spring buluta, anahtar kasanıza erişim izni vermek için:
1. Anahtar Kasası örneğinize gidin.
1. Sol gezinti bölmesinde, **erişim polie**' ye tıklayın.
1. Üstteki menüde, **erişim Ilkesi Ekle**' ye tıklayın.
1. Bilgileri girin ve **Ekle** düğmesine tıklayın ve ardından erişim Police 'yi **kaydedin** .

| Gizli dizi izni | Sertifika izni | Sorumlu seçin |
|--|--|--|
| Al, Listele | Al, Listele | Azure Spring Cloud etki alanı-yönetim |

![Sertifikayı içeri aktar 2](./media/custom-dns-tutorial/import-certificate-b.png)

Ya da Azure 'da, Anahtar Kasası 'na Azure Spring Cloud erişimi sağlamak için Azure CLı 'yi kullanabilirsiniz.

Aşağıdaki komutla nesne kimliğini alın.
```
az ad sp show --id 03b39d0f-4213-4864-a245-b1476ec03169 --query objectId
```

Azure Spring Cloud 'ın Anahtar Kasası 'na okuma erişimine izin verin, aşağıdaki komutta nesne kimliğini değiştirin.
```
az keyvault set-policy -g <key vault resource group> -n <key vault name>  --object-id <object id> --certificate-permissions get list --secret-permissions get list
``` 

Sertifikayı Azure yay bulutuna aktarmak için:
1. Hizmet örneğinize gidin. 
1. Uygulamanızın sol gezinti bölmesinden **TLS/SSL ayarları**' nı seçin.
1. **Key Vault sertifikayı Içeri aktar**' a tıklayın.

    ![Sertifikayı içeri aktarma](./media/custom-dns-tutorial/import-certificate.png)

Veya sertifikayı içeri aktarmak için Azure CLı 'yi kullanabilirsiniz:

```
az spring-cloud certificate add --name <cert name> --vault-uri <key vault uri> --vault-certificate-name <key vault cert name>
```

> [!IMPORTANT] 
> Önceki sertifikayı içeri aktar komutunu yürütmeden önce, anahtar kasanıza Azure Spring Cloud erişimi verdiğinizden emin olun. Yapmadıysanız, erişim haklarını vermek için aşağıdaki komutu çalıştırabilirsiniz.

```
az keyvault set-policy -g <key vault resource group> -n <key vault name>  --object-id 938df8e2-2b9d-40b1-940c-c75c33494239 --certificate-permissions get list
``` 

Sertifikanızı başarıyla içeri aktardığınızda, **özel anahtar sertifikaları**listesinde bunu görürsünüz.

![Özel anahtar sertifikası](./media/custom-dns-tutorial/key-certificates.png)

Veya, Azure CLı kullanarak sertifikaların bir listesini görüntüleyebilirsiniz:

```
az spring-cloud certificate list
```

> [!IMPORTANT] 
> Özel bir etki alanını bu sertifikayla güvenli hale getirmek için yine de sertifikayı belirli bir etki alanına bağlamanız gerekir. Bu belgedeki adımları **SSL bağlaması Ekle**başlığı altında izleyin.

## <a name="add-custom-domain"></a>Özel etki alanı Ekle
Özel bir DNS adını Azure Spring Cloud ile eşlemek için bir CNAME kaydı kullanabilirsiniz. 

> [!NOTE] 
> Kayıt desteklenmiyor. 

### <a name="create-the-cname-record"></a>CNAME kaydı oluşturma
DNS sağlayıcınıza gidin ve etki alanınızı <service_name>. azuremicroservices.io eşlemek için bir CNAME kaydı ekleyin. Burada <service_name> Azure Spring Cloud örneğinizin adıdır. Joker etki alanı ve alt etki alanı destekliyoruz. CNAME eklendikten sonra, DNS kayıtları sayfası aşağıdaki örneğe benzeyecektir: 

![DNS kayıtları sayfası](./media/custom-dns-tutorial/dns-records.png)

## <a name="map-your-custom-domain-to-azure-spring-cloud-app"></a>Özel etki alanınızı Azure Spring Cloud App 'e eşleyin
Azure yay bulutu 'nda uygulamanız yoksa [hızlı başlangıç: Azure Portal kullanarak mevcut bir Azure yay bulutu uygulamasını başlatma](https://review.docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-portal?branch=master)' daki yönergeleri izleyin.

Uygulama sayfasına gidin.

1. **Özel etki alanı**' nı seçin.
2. Ardından **özel etki alanı ekleyin**. 

    ![Özel etki alanı](./media/custom-dns-tutorial/custom-domain.png)

3. Www.contoso.com gibi bir CNAME kaydı eklediğiniz tam etki alanı adını yazın. Ana bilgisayar adı kayıt türünün CNAME (<service_name>. azuremicroservices.io) olarak ayarlandığından emin olun
4. **Ekle** düğmesini etkinleştirmek için **Doğrula** ' ya tıklayın.
5. **Ekle**'ye tıklayın.

    ![Özel etki alanı Ekle](./media/custom-dns-tutorial/add-custom-domain.png)

İsterseniz, özel bir etki alanı eklemek için Azure CLı 'yi de kullanabilirsiniz:
```
az spring-cloud app custom-domain bind --domain-name <domain name> --app <app name> 
```

Bir uygulamanın birden çok etki alanı olabilir, ancak bir etki alanı yalnızca bir uygulamayla eşleşbir şekilde yapılandırılabilir. Özel etki alanınızı uygulamaya başarıyla eşledikten sonra özel etki alanı tablosunda görürsünüz.

![Özel etki alanı tablosu](./media/custom-dns-tutorial/custom-domain-table.png)

Ya da, özel etki alanlarının bir listesini göstermek için Azure CLı 'yi kullanabilirsiniz:
```
az spring-cloud app custom-domain list --app <app name> 
```

> [!NOTE]
> Özel etki alanınız için **güvenli olmayan** bir etiket, henüz bir SSL sertifikasına bağlanmamış anlamına gelir. Bir tarayıcıdan özel etki alanınızı HTTPS istekleri bir hata veya uyarı alır.

## <a name="add-ssl-binding"></a>SSL bağlaması Ekle
Özel etki alanı tablosunda, önceki şekilde gösterildiği gibi **SSL bağlaması Ekle** ' yi seçin.  
1. **Sertifikanızı** seçin veya içeri aktarın.
1. **Kaydet**’e tıklayın.

    ![SSL bağlaması Ekle](./media/custom-dns-tutorial/add-ssl-binding.png)

Ya da, **SSL bağlama eklemek**IÇIN Azure CLI 'yi kullanabilirsiniz:
```
az spring-cloud app custom-domain update --domain-name <domain name> --certificate <cert name> --app <app name> 
```

SSL bağlamasını başarıyla ekledikten sonra, etki alanı durumu güvenli olur: **sağlıklı**. 

![SSL bağlaması Ekle](./media/custom-dns-tutorial/secured-domain-state.png)

## <a name="enforce-https"></a>HTTPS'yi zorunlu tutma
Varsayılan olarak, HTTP kullanarak uygulamanıza erişmeye devam edebilir, ancak tüm HTTP isteklerini HTTPS bağlantı noktasına yeniden yönlendirebilirsiniz.

Uygulama sayfanızda, sol gezinti bölmesinde **özel etki alanı**' nı seçin. Ardından, **yalnızca https**'Yi, *true*olarak ayarlayın.

![SSL bağlaması Ekle](./media/custom-dns-tutorial/enforce-http.png)

Veya, HTTPS 'yi zorlamak için Azure CLı 'yi kullanabilirsiniz:
```
az spring-cloud app update -name <app-name> --https-only <true|false> -g <resource group> --service <service-name>
```

İşlem tamamlandığında, uygulamanızı işaret eden HTTPS URL 'Lerinden herhangi birine gidin. HTTP URL 'Lerinin çalışmadığına unutmayın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Key Vault nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Sertifikayı içeri aktar](https://docs.microsoft.com/azure/key-vault/certificate-scenarios#import-a-certificate)
* [Azure CLı kullanarak Spring Cloud uygulamanızı başlatın](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli)

