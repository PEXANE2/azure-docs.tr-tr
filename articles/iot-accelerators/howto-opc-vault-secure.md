---
title: OPC Kasası-Azure ile OPC UA cihazları iletişimini güvenli hale getirme | Microsoft Docs
description: OPC UA uygulamalarını kaydetme ve OPC kasası ile OPC UA cihazlarınız için imzalı uygulama sertifikaları verme.
author: mregen
ms.author: mregen
ms.date: 8/16/2018
ms.topic: conceptual
ms.service: industrial-iot
services: iot-industrialiot
manager: philmea
ms.openlocfilehash: a819e0b5a0da92137451eedbf84fe06d22879568
ms.sourcegitcommit: 47b00a15ef112c8b513046c668a33e20fd3b3119
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/22/2019
ms.locfileid: "69973785"
---
# <a name="how-to-use-the-opc-ua-certificate-management-service"></a>OPC UA sertifika yönetimi hizmetini kullanma

Bu makalede, uygulamaların nasıl kaydedileceği ve OPC UA cihazlarınız için imzalı uygulama sertifikalarının nasıl giderileceği açıklanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

### <a name="deploy-the-certificate-management-service"></a>Sertifika Yönetimi hizmetini dağıtma

İlki, hizmetin Azure bulutuna dağıtılması gerekir.
Lütfen [OPC Kasası sertifika yönetimi hizmetini dağıtmayı](howto-opc-vault-deploy.md)açıklayan bir makale bulun.

### <a name="create-the-issuer-ca-certificate"></a>Veren CA sertifikası oluşturma

Henüz yapmadıysanız, veren CA sertifikası oluşturun.

[OPC Kasası Için verenin sertifikasını oluşturmayı ve yönetmeyi](howto-opc-vault-manage.md)açıklayan bir makale bulun.

## <a name="secure-opc-ua-applications"></a>Güvenli OPC UA uygulamaları

### <a name="step-1-register-your-opc-ua-application"></a>1\. adım: OPC UA Uygulamanızı kaydetme 

> [!IMPORTANT]
> Bir uygulamayı kaydettirmek için ' yazıcı ' rolü gereklidir.

1. Sertifika hizmetinizi `https://myResourceGroup-app.azurewebsites.net` açın ve oturum açın.
2. `Register New` Sayfasına gidin.
1. Bir uygulama kaydı için, bir kullanıcının en azından ' yazıcı ' rolüne sahip olması gerekir.
2. Giriş formu, OPC UA dünyasında adlandırma kurallarını izler. Örnek olarak, OPC UA 'da [OPC UA başvuru sunucusu](https://github.com/OPCFoundation/UA-.NETStandard/tree/master/SampleApplications/Workshop/Reference) örneğinin ayarlarının altındaki resimde. NetStandard yığını gösteriliyor:

   ![UA başvuru sunucusu kaydı](media/howto-opc-vault-secure/reference-server-registration.png "UA başvuru sunucusu kaydı")

5. Uygulamayı sertifika hizmeti uygulama veritabanına kaydetmek için düğmeyebasın.`Register` İş akışı, uygulamaya imzalı bir sertifika istemek için kullanıcıyı doğrudan bir sonraki adıma yönlendirir.

### <a name="step-2-secure-your-application-with-a-ca-signed-application-certificate"></a>2\. adım: CA imzalı bir uygulama sertifikası ile uygulamanızın güvenliğini sağlama

Bir OPC UA uygulaması, sertifika Imzalama Isteği (CSR) tabanlı imzalı bir sertifika vererek veya PFX veya PEM biçiminde yeni bir özel anahtar da içeren yeni bir anahtar çifti isteyerek güvenli hale getirilir. Uygulamanız için desteklenen yöntem üzerinde OPC UA cihazınızın belgelerini izleyin. Genel olarak CSR yöntemi önerilir, çünkü bir özel anahtarın bir kablo üzerinden aktarılmasını gerektirmez.

- Cihazınızın yeni bir anahtar çifti vermesi gerekiyorsa lütfen [Step3a](##step-3a-request-a-new-certificate-with-a-new-keypair)izleyin.
- Cihazınız bir sertifika Imzalama Isteği (CSR) vermek için destekliyorsa, lütfen [Step3b](#step-3b-request-a-new-certificate-with-a-csr)izleyin.

### <a name="step-3a-request-a-new-certificate-with-a-new-keypair"></a>Adım 3a: Yeni bir anahtar çiftiyle yeni bir sertifika isteyin

1. `Applications` Sayfasına gidin.
3. Listelenen bir uygulama için öğesini seçerek `New Request` sertifika isteği iş akışını başlatın.

   ![Yeni sertifika iste](media/howto-opc-vault-secure/request-new-certificate.png "Yeni sertifika iste")

3. Özel bir anahtar ve uygulamanız için ortak anahtara sahip yeni imzalı bir sertifika istemek için ' yeni KeyPair ve sertifika ıste ' düğmesine basın.

   ![Yeni anahtar çifti oluştur](media/howto-opc-vault-secure/generate-new-key-pair.png "Yeni anahtar çifti oluştur")

4. Formu bir konuyla, etki alanı adlarıyla doldurup özel anahtar için parola ile Pez veya PFX ' i seçin. Sertifika isteği oluşturmak için düğmeyebasın.`Generate New Certificate`

   ![Sertifikayı onayla](media/howto-opc-vault-secure/approve-reject.png "Sertifikayı onayla")

5. Onay adımı, ' onaylayan ' rolüne sahip ve Azure Key Vault oturum açma izinlerine sahip bir kullanıcı gerektirir. Tipik iş akışında, onaylayan ve ısteyenin rolü farklı kullanıcılara atanmalıdır.
6. Anahtar çiftinin ve imzalama işleminin gerçek oluşturma işlemini başlatmak veya iptal etmek için sertifika isteğini onaylayın veya reddedin. Yeni anahtar çifti, sertifika isteği tarafından indirilene kadar Azure Key Vault oluşturulur ve güvenli bir şekilde depolanır. Ortak anahtarla elde edilen sertifika, CA tarafından imzalanır. Bu işlemlerin tamamlanması birkaç saniye sürebilir.

   ![Anahtar çiftini görüntüle](media/howto-opc-vault-secure/view-key-pair.png "Anahtar çiftini görüntüle")

7. Elde edilen özel anahtar (PFX veya ped) ve sertifika (DER), ikili dosya indirme olarak seçilen biçimde buradan indirilebilir. Base64 ile kodlanmış bir sürüm de mevcuttur, örneğin kopyalamak için sertifikayı bir komut satırına veya metin girişine yapıştırın. 
8. Özel anahtar indirilip güvenli şekilde depolandıktan sonra, bu `Delete Private Key` düğme ile hizmetten silinebilir. Ortak anahtara sahip sertifika ileride kullanılmak üzere kullanılabilir kalır.
9. CA imzalı bir sertifikanın kullanılması nedeniyle, CA sertifikası ve CRL de burada indirilmelidir.
10. Artık OPC UA cihazına, yeni anahtar çiftinin nasıl uygulanacağını bağımlıdır. Genellikle, CA sertifikası ve CRL bir `trusted` klasöre kopyalanır, ancak uygulama sertifikasının ortak ve özel anahtarı sertifika deposundaki bir `own` klasöre uygulanır. Bazı cihazlar, sertifika güncelleştirmeleri için ' sunucu gönderme ' desteğini zaten destekleyebilir. OPC UA cihazınızın belgelerine bakın.

### <a name="step-3b-request-a-new-certificate-with-a-csr"></a>Adım 3B: CSR ile yeni bir sertifika isteme 

1. `Applications` Sayfasına gidin.
3. Listelenen bir uygulama için öğesini seçerek `New Request` sertifika isteği iş akışını başlatın.

   ![Yeni sertifika iste](media/howto-opc-vault-secure/request-new-certificate.png "Yeni sertifika iste")

3. Uygulamanız için yeni imzalı bir sertifika istemek üzere ' imzalama isteği ile yeni sertifika ıste ' düğmesine basın.

   ![Yeni sertifika oluştur](media/howto-opc-vault-secure/generate-new-certificate.png "Yeni sertifika oluştur")

4. Yerel bir dosya seçerek veya bir Base64 kodlamalı CSR 'yi forma yapıştırarak CSR 'yi karşıya yükleyin. Sertifika isteği oluşturmak için düğmeyebasın.`Generate New Certificate`

   ![CSR 'Yi Onayla](media/howto-opc-vault-secure/approve-reject-csr.png "CSR 'Yi Onayla")

5. Onay adımı, ' onaylayan ' rolüne sahip ve Azure Key Vault oturum açma izinlerine sahip bir kullanıcı gerektirir. Gerçek imzalama işlemini başlatmak veya iptal etmek için sertifika isteğini onaylayın veya reddedin. Ortak anahtarla elde edilen sertifika, CA tarafından imzalanır. Bu işlemin tamamlanması birkaç saniye sürebilir.

   ![Sertifikayı görüntüle](media/howto-opc-vault-secure/view-cert-csr.png "Sertifikayı görüntüle")

6. Elde edilen sertifika (DER), ikili dosya olarak buradan indirilebilir. Base64 ile kodlanmış bir sürüm de mevcuttur, örneğin kopyalamak için sertifikayı bir komut satırına veya metin girişine yapıştırın. 
10. Sertifika indirilip güvenli bir şekilde depolandıktan sonra, bu `Delete Certificate` düğme ile hizmetten silinebilir.
11. CA imzalı bir sertifikanın kullanılması nedeniyle, CA sertifikası ve CRL de burada indirilmelidir.
12. Artık OPC UA cihazına göre yeni sertifikayı nasıl uygulayacaksınız. Genellikle, CA sertifikası ve CRL, uygulama sertifikası sertifika deposundaki `trusted` bir `own` klasöre uygulanırken bir klasöre kopyalanır. Bazı cihazlar, sertifika güncelleştirmeleri için ' sunucu gönderme ' desteğini zaten destekleyebilir. OPC UA cihazınızın belgelerine bakın.

### <a name="step-4-device-secured"></a>4\. Adım: Cihaz güvenli

OPC UA cihazı artık daha fazla yapılandırma olmadan CA imzalı sertifikalarla güvenliği sağlanmış diğer OPC UA cihazlarıyla iletişim kurmaya hazırdır. Keyfini çıkarın!


## <a name="next-steps"></a>Sonraki adımlar

OPC UA cihazlarını güvenli hale getirme hakkında daha fazla öğrendiğinize göre, önerilen sonraki adım aşağıda verilmiştir:

> [!div class="nextstepaction"]
> [Güvenli sertifika yönetimi hizmetini çalıştırma](howto-opc-vault-secure-ca.md)