---
title: Öğretici-mevcut bir özel etki alanını Azure Spring Cloud ile eşleme
description: Mevcut bir özel dağıtılmış ad hizmeti (DNS) adını Azure Spring Cloud 'a eşleme
author: MikeDodaro
ms.service: spring-cloud
ms.topic: tutorial
ms.date: 03/19/2020
ms.author: brendm
ms.openlocfilehash: 5b57a2463815d5db1c83d3bc4e8cd56314fb204d
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "82176997"
---
# <a name="map-an-existing-custom-domain-to-azure-spring-cloud"></a>Mevcut bir özel etki alanını Azure Spring Cloud ile eşleme
Dağıtılmış ad hizmeti (DNS), ağ düğümü adlarını ağ genelinde depolamanın bir tekniğidir. Bu öğretici, CNAME kaydı kullanarak www.contoso.com gibi bir etki alanını eşler. Özel etki alanının bir sertifikayla güvenliğini sağlar ve Güvenli Yuva Katmanı (SSL) olarak da bilinen Aktarım Katmanı Güvenliği 'ni (TLS) nasıl zorunlu hale kullanabileceğinizi gösterir. 

Sertifikalar Web trafiğini şifreler. Bu TLS/SSL sertifikaları, Azure Key Vault depolanabilir. 

## <a name="prerequisites"></a>Ön koşullar
* Azure yay bulutuna dağıtılan bir uygulama (bkz. [hızlı başlangıç: Azure Portal kullanarak mevcut bir Azure Spring Cloud uygulamasını başlatma](spring-cloud-quickstart-launch-app-portal.md)veya mevcut bir uygulamayı kullanma).
* GoDaddy gibi etki alanı sağlayıcısı için DNS kayıt defterine erişimi olan bir etki alanı adı.
* Üçüncü taraf sağlayıcıdan özel bir sertifika. Sertifika, etki alanıyla aynı olmalıdır.
* [Azure Key Vault](https://docs.microsoft.com/azure/key-vault/key-vault-overview) dağıtılan bir örnek

## <a name="import-certificate"></a>Sertifikayı içeri aktarma 
Bir sertifikayı içeri aktarma yordamı, pek veya PFX kodlamalı dosyanın diskte olması gerekir ve özel anahtara sahip olmanız gerekir. 

Sertifikanızı anahtar kasasına yüklemek için:
1. Anahtar Kasası örneğinize gidin.
1. Sol gezinti bölmesinde, **Sertifikalar**' a tıklayın.
1. Üstteki menüde **Oluştur/içeri aktar**' a tıklayın.
1. **Sertifika oluşturma yönteminin**altında `Import` **sertifika oluştur** iletişim kutusunda öğesini seçin.
1. **Sertifika dosyasını karşıya yükle**altında, sertifika konumuna gidin ve seçin.
1. **Parola**altında, sertifikanız için özel anahtarı girin.
1. **Oluştur**' a tıklayın.

![Sertifikayı içeri aktar 1](./media/custom-dns-tutorial/import-certificate-a.png)

Sertifikayı Azure yay bulutuna aktarmak için:
1. Hizmet örneğinize gidin. 
1. Uygulamanızın sol gezinti bölmesinden **TLS/SSL ayarları**' nı seçin.
1. **Key Vault sertifikayı Içeri aktar**' a tıklayın.

![Sertifikayı içeri aktarma](./media/custom-dns-tutorial/import-certificate.png)

Sertifikanızı başarıyla içeri aktardığınızda, **özel anahtar sertifikaları**listesinde bunu görürsünüz.

![Özel anahtar sertifikası](./media/custom-dns-tutorial/key-certificates.png)

>[!IMPORTANT] 
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

Bir uygulamanın birden çok etki alanı olabilir, ancak bir etki alanı yalnızca bir uygulamayla eşleşbir şekilde yapılandırılabilir. Özel etki alanınızı uygulamaya başarıyla eşledikten sonra özel etki alanı tablosunda görürsünüz.

![Özel etki alanı tablosu](./media/custom-dns-tutorial/custom-domain-table.png)

>[!NOTE]
> Özel etki alanınız için **güvenli olmayan** bir etiket, henüz bir SSL sertifikasına bağlanmamış anlamına gelir. Bir tarayıcıdan özel etki alanınızı HTTPS istekleri bir hata veya uyarı alır.

## <a name="add-ssl-binding"></a>SSL bağlaması Ekle
Özel etki alanı tablosunda, önceki şekilde gösterildiği gibi **SSL bağlaması Ekle** ' yi seçin.  
1. **Sertifikanızı** seçin veya içeri aktarın.
1. **Kaydet**’e tıklayın.

![SSL bağlaması Ekle](./media/custom-dns-tutorial/add-ssl-binding.png)

SSL bağlamasını başarıyla ekledikten sonra, etki alanı durumu güvenli olur: **sağlıklı**. 

![SSL bağlaması Ekle](./media/custom-dns-tutorial/secured-domain-state.png)

## <a name="enforce-https"></a>HTTPS'yi zorunlu tutma
Varsayılan olarak, HTTP kullanarak uygulamanıza erişmeye devam edebilir, ancak tüm HTTP isteklerini HTTPS bağlantı noktasına yeniden yönlendirebilirsiniz.

Uygulama sayfanızda, sol gezinti bölmesinde **özel etki alanı**' nı seçin. Ardından, **yalnızca https**'Yi, *true*olarak ayarlayın.

![SSL bağlaması Ekle](./media/custom-dns-tutorial/enforce-http.png)

İşlem tamamlandığında, uygulamanızı işaret eden HTTPS URL 'Lerinden herhangi birine gidin. HTTP URL 'Lerinin çalışmadığına unutmayın.

## <a name="see-also"></a>Ayrıca bkz.
* [Azure Anahtar Kasası nedir?](https://docs.microsoft.com/azure/key-vault/key-vault-overview)
* [Sertifikayı içeri aktar](https://docs.microsoft.com/azure/key-vault/certificate-scenarios#import-a-certificate)
* [Azure CLı kullanarak Spring Cloud uygulamanızı başlatın](https://docs.microsoft.com/azure/spring-cloud/spring-cloud-quickstart-launch-app-cli)

