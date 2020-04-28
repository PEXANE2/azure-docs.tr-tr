---
title: Azure DPS SDK 'larını kullanarak cihaz kayıtlarını yönetme
description: Hizmet SDK 'larını kullanarak cihaz kayıtlarını IoT Hub cihaz sağlama hizmeti 'nde (DPS) yönetme
author: robinsh
ms.author: robinsh
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 5cb0e25ec70956e66f7b867f0d0b9473160fc3ad
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74975083"
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Azure cihaz sağlama hizmeti SDK 'Ları ile cihaz kayıtlarını yönetme
Bir *cihaz kaydı* , tek bir cihazın veya bazı noktada cihaz sağlama hizmeti 'ne kaydolabileceği bir cihaz grubunun kaydını oluşturur. Kayıt kaydı, istenen IoT Hub 'ı da dahil olmak üzere, bu kayıt kapsamında cihaz (ler) için ilk istenen yapılandırmayı içerir. Bu makalede, Azure IoT sağlama hizmeti SDK 'larını kullanarak sağlama hizmetiniz için cihaz kayıtlarını nasıl yöneteceğiniz gösterilmektedir.  SDK 'lar, Azure IoT SDK 'Ları ile aynı depoda GitHub 'da kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
* Cihaz sağlama hizmeti örneğinden bağlantı dizesini edinin.
* Kullanılan [kanıtlama mekanizması](concepts-security.md#attestation-mechanism) için cihaz güvenlik yapılarını edinin:
    * [**Güvenilir Platform Modülü (TPM)**](/azure/iot-dps/concepts-security#trusted-platform-module):
        * Bireysel kayıt: fiziksel bir cihazdan veya TPM Benzeticisinde kayıt KIMLIĞI ve TPM onay anahtarı.
        * Kayıt grubu TPM kanıtlama için uygulanmıyor.
    * [**X. 509.440**](/azure/iot-dps/concepts-security):
        * Bireysel kayıt: fiziksel cihazdan veya SDK [zar](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) öykünücüsünde [yaprak sertifikası](/azure/iot-dps/concepts-security) .
        * Kayıt grubu: fiziksel bir cihazda cihaz sertifikası oluşturmak için kullanılan [CA/kök sertifika](/azure/iot-dps/concepts-security#root-certificate) veya [Ara Sertifika](/azure/iot-dps/concepts-security#intermediate-certificate).  Ayrıca, SDK zar öykünücüden da oluşturulabilir.
* Dil farklılıkları nedeniyle tam API çağrıları farklı olabilir. Ayrıntılar için lütfen GitHub 'da sağlanan örnekleri gözden geçirin:
   * [Java sağlama hizmeti Istemci örnekleri](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
   * [Node. js sağlama hizmeti Istemci örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
   * [.NET sağlama hizmeti Istemci örnekleri](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma
Cihazları sağlama hizmeti ile kaydedebilmeniz için iki yol vardır:

* **Kayıt grubu** , [kök sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya [Ara Sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate)tarafından imzalanan bir X. 509.440 sertifikaları ortak kanıtlama mekanizmasını paylaşan bir cihaz grubu için giriştir. İstenen ilk yapılandırmayı paylaşan çok sayıda cihaz için veya hepsi aynı kiracıya giden cihazlar için bir kayıt grubu kullanmanızı öneririz. Yalnızca X. 509.440 kanıtlama mekanizmasını kullanan cihazları *kayıt grupları*olarak kaydedebileceğinizi unutmayın. 

    Bu iş akışını izleyen SDK 'lara sahip bir kayıt grubu oluşturabilirsiniz:

    1. Kayıt grubu için, kanıtlama mekanizması X. 509.440 kök sertifikasını kullanır.  Kayıt için kanıtlama oluşturmak ```X509Attestation.createFromRootCertificate``` üzere kök sertifika Ile Service SDK API 'sini çağırın.  X. 509.440 kök sertifikası bir ped dosyasında ya da bir dize olarak sağlanır.
    1. ```attestation``` Oluşturulan ve benzersiz ```EnrollmentGroup``` ```enrollmentGroupId```kullanarak yeni bir değişken oluşturun.  İsteğe bağlı olarak,, ```Device ID``` ```IoTHubHostName```, ```ProvisioningStatus```gibi parametreleri ayarlayabilirsiniz.
    2. Kayıt grubu oluşturmak için ```createOrUpdateEnrollmentGroup``` ile ```EnrollmentGroup``` arka uç uygulamanızda Service SDK API 'sini çağırın.

* **Tek bir kayıt** , kaydedebilen tek bir cihaz için giriştir. Bireysel kayıtlar, kanıtlama mekanizması olarak X. 509.440 sertifikalarını veya SAS belirteçlerini (fiziksel veya sanal TPM 'den) kullanabilir. Benzersiz ilk yapılandırma gerektiren cihazlar için veya kanıtlama mekanizması olarak TPM veya sanal TPM aracılığıyla yalnızca SAS belirteçlerini kullanan cihazlar için ayrı kayıtlar kullanmanızı öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Bu iş akışını izleyen SDK 'lar ile bireysel bir kayıt oluşturabilirsiniz:
    
    1. TPM veya ```attestation``` X. 509.440 olabilecek mekanizmanızı seçin.
        1. **TPM**: fiziksel bir CIHAZDAN veya TPM benzeticisinde giriş olarak onay anahtarını kullanarak kayıt için kanıtlama oluşturmak üzere Service SDK API 'sini ```TpmAttestation``` çağırabilirsiniz. 
        2. **X. 509.440**: istemci sertifikasını girdi olarak kullanarak, kayıt için kanıtlama oluşturmak üzere Service SDK API ```X509Attestation.createFromClientCertificate``` 'sini çağırabilirsiniz.
    2. Cihazınızda bulunan veya ```IndividualEnrollment``` TPM benzeticisinde ```attestation``` oluşturulan, oluşturulan ve ```registrationId``` benzersiz olarak giriş olarak kullanılan yeni bir değişken oluşturun.  İsteğe bağlı olarak,, ```Device ID``` ```IoTHubHostName```, ```ProvisioningStatus```gibi parametreleri ayarlayabilirsiniz.
    3. Tek bir kayıt oluşturmak ```createOrUpdateIndividualEnrollment``` için ile ```IndividualEnrollment``` arka uç uygulamanızda Service SDK API 'sini çağırın.

Bir kaydı başarıyla oluşturduktan sonra, cihaz sağlama hizmeti bir kayıt sonucu döndürür. Bu iş akışı, [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.

## <a name="update-an-enrollment-entry"></a>Kayıt girişini güncelleştirme

Bir kayıt girişi oluşturduktan sonra, kaydı güncelleştirmek isteyebilirsiniz.  Olası senaryolar, istenen özelliğin güncelleştirilmesini, kanıtlama yöntemini güncelleştirmeyi veya cihaz erişimini iptal etmeyi içerir.  Bireysel kayıt ve grup kaydı için farklı API 'Ler vardır, ancak kanıtlama mekanizması için ayrım yoktur.

Bu iş akışını izleyerek bir kayıt girişini güncelleştirebilirsiniz:
* **Bireysel kayıt**:
    1. Service SDK API 'SI ```getIndividualEnrollment```ile ilk olarak sağlama hizmetinden en son kaydı alın.
    2. En son kayıt parametresini gereken şekilde değiştirin. 
    3. En son kaydı kullanarak kayıt girdinizi güncelleştirmek için Service ```createOrUpdateIndividualEnrollment``` SDK API 'sini çağırın.
* **Grup kaydı**:
    1. Service SDK API 'SI ```getEnrollmentGroup```ile ilk olarak sağlama hizmetinden en son kaydı alın.
    2. En son kayıt parametresini gereken şekilde değiştirin.
    3. En son kaydı kullanarak kayıt girdinizi güncelleştirmek için Service ```createOrUpdateEnrollmentGroup``` SDK API 'sini çağırın.

Bu iş akışı, [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.

## <a name="remove-an-enrollment-entry"></a>Kayıt girişini kaldırma

* **Bireysel kayıt** , KULLANıLARAK ```deleteIndividualEnrollment``` ```registrationId```Service SDK API 'si çağırarak silinebilir.
* **Grup kaydı** , KULLANıLARAK ```deleteEnrollmentGroup``` ```enrollmentGroupId```Service SDK API 'si çağırarak silinebilir.

Bu iş akışı, [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.

## <a name="bulk-operation-on-individual-enrollments"></a>Bireysel kayıtlar üzerinde toplu işlem

Bu iş akışını takip eden birden çok bireysel kayıt oluşturmak, güncelleştirmek veya kaldırmak için toplu işlem yapabilirsiniz:

1. Birden çok ```IndividualEnrollment```içeren bir değişken oluşturun.  Bu değişkenin uygulanması her dil için farklıdır.  Ayrıntılar için GitHub 'daki toplu işlem örneğini gözden geçirin.
2. Service SDK API 'sini ```runBulkOperation``` istenen bir ```BulkOperationMode``` işlem ve bireysel kayıtlar için değişkeniniz ile çağırın. Dört mod desteklenir: Create, Update, updateIfMatchEtag ve delete.

Bir işlemi başarıyla gerçekleştirdikten sonra, cihaz sağlama hizmeti bir toplu işlem sonucu döndürür.

Bu iş akışı, [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.
