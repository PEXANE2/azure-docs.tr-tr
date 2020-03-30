---
title: OPC Vault ile OPC UA cihazlarının iletişimini güvenli hale - Azure | Microsoft Dokümanlar
description: OPC UA uygulamalarını nasıl kaydedebilirsiniz ve OPC Vault ile OPC UA aygıtlarınız için imzalı başvuru sertifikaları nasıl veririz.
author: mregen
ms.author: mregen
ms.date: 8/16/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: af5e511cbf273bc4e4fa0a08d089a955426fe75c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "75454204"
---
# <a name="use-the-opc-vault-certificate-management-service"></a>OPC Vault sertifika yönetimi hizmetini kullanma

Bu makalede, başvuruların nasıl kaydedilen ve OPC UA aygıtlarınız için imzalı uygulama sertifikalarının nasıl düzenleyilen açıklanmaktadır.

## <a name="prerequisites"></a>Ön koşullar

### <a name="deploy-the-certificate-management-service"></a>Sertifika yönetim hizmetini dağıtma

İlk olarak, hizmeti Azure bulutuna dağıtın. Ayrıntılar için [OPC Vault sertifika yönetim hizmetini dağıt'a](howto-opc-vault-deploy.md)bakın.

### <a name="create-the-issuer-ca-certificate"></a>İhraççı CA sertifikasını oluşturma

Henüz yapmadıysanız, İhraççı CA sertifikasını oluşturun. Ayrıntılar için, [OPC Vault için İhraççı sertifikasını oluştur ve yönetme konusuna](howto-opc-vault-manage.md)bakın.

## <a name="secure-opc-ua-applications"></a>Güvenli OPC UA uygulamaları

### <a name="step-1-register-your-opc-ua-application"></a>Adım 1: OPC UA başvurunuzu kaydedin 

> [!IMPORTANT]
> Yazar rolü bir uygulamayı kaydetmek için gereklidir.

1. Sertifika hizmetinizi `https://myResourceGroup-app.azurewebsites.net`şu anda açın ve oturum açın.
2. Yeni **Kaydol'a**gidin. Bir uygulama kaydı için, bir kullanıcının en azından Yazar rolünün atanmış olması gerekir.
2. Giriş formu, OPC UA'daki adlandırma sözleşmelerini izler. Örneğin, aşağıdaki ekran görüntüsünde, OPC UA .NET Standart yığınındaki [OPC UA Reference Server](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/Reference) örneğinin ayarları gösterilir:

   ![UA Reference Server Kaydı ekran görüntüsü](media/howto-opc-vault-secure/reference-server-registration.png "UA Referans Sunucu Kaydı")

5. Uygulamayı sertifika hizmeti uygulama veritabanına kaydetmek için **Kaydol'u** seçin. İş akışı, kullanıcıyı doğrudan uygulama için imzalı bir sertifika istemek için bir sonraki adıma yönlendirir.

### <a name="step-2-secure-your-application-with-a-ca-signed-application-certificate"></a>Adım 2: CA imzalı uygulama sertifikası ile başvurunuzu güvenli hale

Sertifika İmzalama İsteği 'ni (CSR) temel alan imzalı bir sertifika vererek OPC UA başvurunuzu güvence altına alın. Alternatif olarak, PFX veya PEM formatında yeni bir özel anahtar içeren yeni bir anahtar çifti isteyebilirsiniz. Uygulamanız için hangi yöntemin desteklendiği hakkında bilgi için OPC UA cihazınızın belgelerine bakın. Genel olarak, csr yöntemi önerilir, çünkü bir tel üzerinden aktarılacak özel bir anahtar gerektirmez.

#### <a name="request-a-new-certificate-with-a-new-keypair"></a>Yeni anahtar çifti yle yeni bir sertifika isteme

1. **Uygulamalara**git.
3. Listelenen bir uygulama için **Yeni İstek'i** seçin.

   ![İstek Yeni Sertifika ekran görüntüsü](media/howto-opc-vault-secure/request-new-certificate.png "Yeni Sertifika İsteyin")

3. Başvurunuz için ortak anahtariçeren özel bir anahtar ve yeni imzalı sertifika istemek için **yeni KeyPair ve Sertifika İste'yi** seçin.

   ![Yeni Bir KeyPair ve Sertifika Oluşturma Ekran Görüntüsü](media/howto-opc-vault-secure/generate-new-key-pair.png "Yeni Anahtar Çifti Oluştur")

4. Formu bir konu ve alan adlarıyla doldurun. Özel anahtar için, parolalı PEM veya PFX'i seçin. Sertifika isteğini oluşturmak için **Yeni Anahtar Çifti Oluştur'u** seçin.

   ![Görünüm Sertifikası İstek Ayrıntılarının Ekran Görüntüsü](media/howto-opc-vault-secure/approve-reject.png "Sertifikayı Onayla")

5. Onay, Onaylayıcı rolüne sahip bir kullanıcı ve Azure Anahtar Kasası'nda onay izinleri imzalamayı gerektirir. Tipik iş akışında, Onaylayıcı ve İstekçi rolleri farklı kullanıcılara atanmalıdır. Anahtar çiftinin ve imzalama işleminin gerçek oluşturulmasını başlatmak veya iptal etmek için **Onayla** veya **Reddet'i** seçin. Yeni anahtar çifti oluşturulur ve sertifika isteği tarafından indirilene kadar Azure Key Vault'ta güvenli bir şekilde saklanır. Ortak anahtariçeren ortaya çıkan sertifika CA tarafından imzalanır. Bu işlemlerin tamamlanması birkaç saniye sürebilir.

   ![Görünüm Sertifikası İstek Ayrıntılarının ekran görüntüsü, alt kısmında onay mesajı](media/howto-opc-vault-secure/view-key-pair.png "Anahtar Çiftini Görüntüle")

7. Elde edilen özel anahtar (PFX veya PEM) ve sertifika (DER) buradan ikili dosya indirme olarak seçilen biçimde indirilebilir. Örneğin, sertifikayı bir komut satırına veya metin girişine kopyalayıp yapıştırmak için base64 kodlanmış bir sürüm de kullanılabilir. 
8. Özel anahtar indirilip güvenli bir şekilde depolandıktan sonra **Sil Özel Anahtarı'nı**seçebilirsiniz. Ortak anahtara sahip sertifika ileride kullanıma hazır kalır.
9. CA imzalı sertifikanın kullanımı nedeniyle, CA sertifika ve Sertifika İptal Listesi (CRL) buradan da indirilmelidir.

Şimdi yeni anahtar çifti uygulamak için Nasıl OPC UA cihazına bağlıdır. Genellikle, CA sertifikası ve CRL bir `trusted` klasöre kopyalanırken, uygulama sertifikasının ortak ve `own` özel anahtarları sertifika deposundaki bir klasöre uygulanır. Bazı aygıtlar sertifika güncelleştirmeleri için sunucu itme zaten destekleyebilir. OPC UA cihazınızın belgelerine bakın.

#### <a name="request-a-new-certificate-with-a-csr"></a>KSS ile yeni bir sertifika isteme 

1. **Uygulamalara**git.
3. Listelenen bir uygulama için **Yeni İstek'i** seçin.

   ![İstek Yeni Sertifika ekran görüntüsü](media/howto-opc-vault-secure/request-new-certificate.png "Yeni Sertifika İsteyin")

3. Başvurunuz için yeni bir imzalı sertifika istemek için **İmza Lama İsteği ile Yeni Sertifika İste'yi** seçin.

   ![Yeni Sertifika Oluştur Ekran Görüntüsü](media/howto-opc-vault-secure/generate-new-certificate.png "Yeni Sertifika Oluştur")

4. Yerel bir dosya seçerek veya forma kodlanmış bir base64 CSR yapıştırarak CSR yükleyin. **Yeni Sertifika Oluştur'u**seçin.

   ![Görünüm Sertifikası İstek Ayrıntılarının Ekran Görüntüsü](media/howto-opc-vault-secure/approve-reject-csr.png "CSR'yi onayla")

5. Onay, Onaylayıcı rolüne sahip bir kullanıcı ve Azure Anahtar Kasası'nda onay izinleri imzalamayı gerektirir. Gerçek imzalama işlemini başlatmak veya iptal etmek için **Onayla** veya **Reddet'i** seçin. Ortak anahtariçeren ortaya çıkan sertifika CA tarafından imzalanır. Bu işlemin tamamlanması birkaç saniye sürebilir.

   ![Görünüm Sertifikası İstek Ayrıntılarının ekran görüntüsü, alt kısmında onay mesajı](media/howto-opc-vault-secure/view-cert-csr.png "Sertifikayı Görüntüle")

6. Elde edilen sertifika (DER) buradan ikili dosya olarak indirilebilir. Örneğin, sertifikayı bir komut satırına veya metin girişine kopyalayıp yapıştırmak için base64 kodlanmış bir sürüm de kullanılabilir. 
10. Sertifika indirilip güvenli bir şekilde depolandıktan sonra **Sil Sertifikası'nı**seçebilirsiniz.
11. CA imzalı sertifikanın kullanımı nedeniyle, CA sertifikası ve CRL'nin de buradan indirilmesi gerekir.

Şimdi yeni sertifikanasıl uygulanacağı OPC UA aygıtına bağlıdır. Genellikle, CA sertifikası ve CRL bir `trusted` klasöre kopyalanırken, uygulama sertifikası `own` sertifika deposundaki bir klasöre uygulanır. Bazı aygıtlar sertifika güncelleştirmeleri için sunucu itme zaten destekleyebilir. OPC UA cihazınızın belgelerine bakın.

### <a name="step-3-device-secured"></a>Adım 3: Aygıt güvenli

OPC UA aygıtı artık CA imzalı sertifikalar tarafından güvenli diğer OPC UA aygıtlarıyla daha fazla yapılandırma yapmadan iletişim kurmaya hazırdır.

## <a name="next-steps"></a>Sonraki adımlar

Artık OPC UA aygıtlarının güvenliğini nasıl sağlayacağınızı öğrendiğiniz için şunları yapabilirsiniz:

> [!div class="nextstepaction"]
> [Güvenli bir sertifika yönetimi hizmeti çalıştırın](howto-opc-vault-secure-ca.md)
