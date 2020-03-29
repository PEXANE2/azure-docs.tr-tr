---
title: X.509 CA sertifikalarını Azure IoT Hub Aygıt Sağlama Hizmeti ile doğrulayın
description: Azure IoT Hub Aygıt Sağlama Hizmeti (DPS) ile X.509 CA sertifikaları için sahip kanıtı nasıl yapılır?
author: wesmc7777
ms.author: wesmc
ms.date: 02/26/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: b008c4ebc83200043d51fc8ef367f1983c549949
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74973450"
---
# <a name="how-to-do-proof-of-possession-for-x509-ca-certificates-with-your-device-provisioning-service"></a>Cihaz Sağlama Hizmetiile X.509 CA sertifikaları için bulundurma kanıtı nasıl yapabilirsiniz?

Doğrulanmış X.509 Sertifika Yetkilisi (CA) sertifikası, sağlama hizmetinize yüklenen ve kaydedilmiş ve hizmetle birlikte bulundurma kanıtı yoluyla geçmiş bir CA sertifikasıdır. 

Bulundurma kanıtı aşağıdaki adımları içerir:
1. X.509 CA sertifikanız için sağlama hizmeti tarafından oluşturulan benzersiz bir doğrulama kodu alın. Bu işlemi Azure portalından yapabilirsiniz.
2. Öznesi olan bir X.509 doğrulama sertifikası oluşturun ve sertifikayı X.509 CA sertifikanızla ilişkili özel anahtarla imzalayın.
3. İmzalı doğrulama sertifikasını hizmete yükleyin. Hizmet, ca sertifikasının onaylanacak ortak bölümünü kullanarak doğrulama sertifikasını doğrular ve böylece CA sertifikasının özel anahtarına sahip olduğunuzu kanıtlar.

Doğrulanmış sertifikalar, kayıt gruplarını kullanırken önemli bir rol oynar. Sertifika sahipliğinin doğrulanması, sertifikayı yükleyenin sertifikanın özel anahtarına sahip olduğundan emin olarak ek bir güvenlik katmanı sağlar. Doğrulama, kötü niyetli bir aktörün trafiğinizi koklamasını ve bu sertifikayı kullanarak kendi sağlama hizmetinde bir kayıt grubu oluşturarak aygıtlarınızı etkili bir şekilde ele geçirmesini önler. Bir sertifika zincirinde kök veya ara sertifikanın sahipliğini kanıtlayarak, bu kayıt grubunun bir parçası olarak kaydolacak aygıtlar için yaprak sertifika oluşturma iznine sahip olduğunuzu kanıtlıyorsunuz. Bu nedenle, bir kayıt grubunda yapılandırılan kök veya ara sertifikanın doğrulanmış bir sertifika olması veya bir aygıtın hizmetle kimlik doğrulaması yaptığında sunduğu sertifika zincirinde doğrulanmış bir sertifikaya yuvarlaması gerekir. Kayıt grupları hakkında daha fazla bilgi edinmek için [X.509 sertifikaları](concepts-security.md#x509-certificates) ve [X.509 sertifikalarına sahip sağlama hizmetine aygıt erişimini denetleme](concepts-security.md#controlling-device-access-to-the-provisioning-service-with-x509-certificates)konusuna bakın.

## <a name="register-the-public-part-of-an-x509-certificate-and-get-a-verification-code"></a>X.509 sertifikasının genel kısmını kaydedin ve doğrulama kodu alın

Ca sertifikasını sağlama hizmetinize kaydettirmek ve bulundurma kanıtı sırasında kullanabileceğiniz bir doğrulama kodu almak için aşağıdaki adımları izleyin. 

1. Azure portalında, sağlama hizmetinize gidin ve sol menüden **Sertifikalar'ı** açın. 
2. Yeni bir sertifika eklemek için **Ekle'yi** tıklatın.
3. Sertifikanız için dostça bir görüntü adı girin. X.509 sertifikanızın ortak kısmını temsil eden .cer veya .pem dosyasına göz atın. **Karşıya Yükle**'ye tıklayın.
4. Sertifikanızın başarıyla yüklendiğine dair bir bildirim aldıktan sonra **Kaydet'i**tıklatın.

    ![Sertifikayı karşıya yükleme](./media/how-to-verify-certificates/add-new-cert.png)  

   Sertifikanız **Sertifika Gezgini** listesinde gösterecektir. Bu sertifikanın **DURUMUNUn** *Doğrulanmadığını*unutmayın.

5. Önceki adımda eklediğiniz sertifikayı tıklatın.

6. **Sertifika**Ayrıntıları'nda, Doğrulama **Kodu Oluştur'u**tıklatın.

7. Sağlama hizmeti, sertifika sahipliğini doğrulamak için kullanabileceğiniz bir **Doğrulama Kodu** oluşturur. Kodu panonuza kopyalayın. 

   ![Sertifikayı doğrula](./media/how-to-verify-certificates/verify-cert.png)  

## <a name="digitally-sign-the-verification-code-to-create-a-verification-certificate"></a>Doğrulama sertifikası oluşturmak için doğrulama kodunu dijital olarak imzalayın

Şimdi, *doğrulama kodunu,* imza üreten X.509 CA sertifikanızla ilişkili özel anahtarla imzalamanız gerekir. Bu, [sahip olma kanıtı](https://tools.ietf.org/html/rfc5280#section-3.1) olarak bilinir ve imzalı doğrulama sertifikasıyla sonuçlanır.

Microsoft, imzalı bir doğrulama sertifikası oluşturmanıza yardımcı olabilecek araçlar ve örnekler sağlar: 

- **Azure IoT Hub C SDK,** geliştirme için CA ve yaprak sertifikaları oluşturmanıza ve bir doğrulama kodu kullanarak sahip kanıtı gerçekleştirmenize yardımcı olmak için PowerShell (Windows) ve Bash (Linux) komut dosyaları sağlar. Sisteminizle ilgili [dosyaları](https://github.com/Azure/azure-iot-sdk-c/tree/master/tools/CACertificates) çalışan bir klasöre indirebilir ve CA sertifikasıüzerinde sahip olma kanıtı gerçekleştirmek için [bana okunan Yönetici CA sertifikalarında](https://github.com/Azure/azure-iot-sdk-c/blob/master/tools/CACertificates/CACertificateOverview.md) yer alan yönergeleri izleyebilirsiniz. 
- **Azure IoT Hub C# SDK,** sahip olma kanıtı yapmak için kullanabileceğiniz [Grup Sertifikası Doğrulama Örneği](https://github.com/Azure-Samples/azure-iot-samples-csharp/tree/master/provisioning/Samples/service/GroupCertificateVerificationSample)içerir.
 
> [!IMPORTANT]
> Sahip olma kanıtı gerçekleştirmenin yanı sıra, daha önce belirtilen PowerShell ve Bash komut dosyaları da kimlik doğrulama ve sağlama aygıtları için kullanılabilecek kök sertifikaları, ara sertifikalar ve yaprak sertifikalar oluşturmanıza olanak tanır. Bu sertifikalar yalnızca geliştirme için kullanılmalıdır. Asla üretim ortamında kullanılmamalıdır. 

Dokümantasyonda sağlanan PowerShell ve Bash komut dosyaları ve SDK'lar [OpenSSL'e](https://www.openssl.org/)güvenir. Ayrıca, bulundurma kanıtı yapmanıza yardımcı olmak için OpenSSL veya diğer üçüncü taraf araçlarını da kullanabilirsiniz. SDK'larla sağlanan araç kullanımı hakkında daha fazla bilgi için, [SDK'larda sağlanan araçların nasıl kullanılacağına](how-to-use-sdk-tools.md)bakın. 


## <a name="upload-the-signed-verification-certificate"></a>İmzalı doğrulama sertifikasını yükleme

1. Elde edilen imzayı doğrulama sertifikası olarak portaldaki sağlama hizmetinize yükleyin. Azure portalındaki **Sertifika Ayrıntıları'nda,** imzalı doğrulama sertifikasını sisteminizden yüklemek için Doğrulama Sertifikası **.pem veya .cer dosya** alanının yanındaki Dosya _Gezgini_ simgesini kullanın.

2. Sertifika başarıyla yüklendikten sonra **Doğrula'yı**tıklatın. Sertifikanızın **DURUMU,** **Sertifika Gezgini** listesinde **_Doğrulanmış_** olarak değişir. Otomatik olarak güncelleştirilemiyorsa **Yenile'yi** tıklatın.

   ![Sertifika doğrulamayı yükleme](./media/how-to-verify-certificates/upload-cert-verification.png)  

## <a name="next-steps"></a>Sonraki adımlar

- Bir kayıt grubu oluşturmak için portalı nasıl kullanacağınızı öğrenmek için [bkz.](how-to-manage-enrollments.md)
- Bir kayıt grubu oluşturmak için hizmet SDK'larını nasıl kullanacağınızı öğrenmek için, [hizmet SDK'ları ile aygıt kayıtlarını yönetme'ye](how-to-manage-enrollments-sdks.md)bakın.










