---
title: Azure DPS SDK'larını kullanarak cihaz kayıtlarını yönetme
description: Hizmet SDK'larını kullanarak IoT Hub Cihaz Sağlama Hizmeti'nde (DPS) cihaz kayıtları nasıl yönetilir?
author: robinsh
ms.author: robinsh
ms.date: 04/04/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
ms.openlocfilehash: 5cb0e25ec70956e66f7b867f0d0b9473160fc3ad
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74975083"
---
# <a name="how-to-manage-device-enrollments-with-azure-device-provisioning-service-sdks"></a>Azure Aygıt Sağlama Hizmeti SDK'ları ile cihaz kayıtları nasıl yönetilir?
*Aygıt kaydı,* bir noktada Aygıt Sağlama Hizmeti'ne kaydolabilecek tek bir aygıtın veya bir aygıt grubunun kaydını oluşturur. Kayıt kaydı, istenen IoT hub'ı da dahil olmak üzere, bu kaydın bir parçası olarak aygıt(lar) için ilk istenen yapılandırmayı içerir. Bu makalede, Azure IoT Sağlama Hizmeti SKK'larını kullanarak sağlama hizmetinizin aygıt kayıtlarını programlamak olarak nasıl yönetilen gösterilmektedir.  SDK'lar GitHub'da Azure IoT SDK'larla aynı depoda kullanılabilir.

## <a name="prerequisites"></a>Ön koşullar
* Aygıt Sağlama Hizmeti örneğinden bağlantı dizesini edinin.
* Kullanılan [attestation mekanizması](concepts-security.md#attestation-mechanism) için aygıt güvenlik yapılarını edinin:
    * [**Güvenilir Platform Modülü (TPM)**](/azure/iot-dps/concepts-security#trusted-platform-module):
        * Bireysel kayıt: Fiziksel bir cihazdan veya TPM Simulator'dan Kayıt Kimliği ve TPM Onay Anahtarı.
        * Kayıt grubu TPM attestation için geçerli değildir.
    * [**X.509**](/azure/iot-dps/concepts-security):
        * Bireysel kayıt: Fiziksel cihazdan veya SDK [DICE](https://azure.microsoft.com/blog/azure-iot-supports-new-security-hardware-to-strengthen-iot-security/) Emülatöründen [Yaprak sertifikası.](/azure/iot-dps/concepts-security)
        * Kayıt grubu: Fiziksel bir aygıtta aygıt sertifikası üretmek için kullanılan [CA/root sertifikası](/azure/iot-dps/concepts-security#root-certificate) veya [ara sertifika.](/azure/iot-dps/concepts-security#intermediate-certificate)  Ayrıca SDK DICE emülatöründen de oluşturulabilir.
* Tam API çağrıları dil farklılıkları nedeniyle farklı olabilir. Ayrıntılar için lütfen GitHub'da sağlanan örnekleri inceleyin:
   * [Java Sağlama Hizmeti İstemci örnekleri](https://github.com/Azure/azure-iot-sdk-java/tree/master/provisioning/provisioning-samples)
   * [Node.js Sağlama Hizmeti İstemci örnekleri](https://github.com/Azure/azure-iot-sdk-node/tree/master/provisioning/service/samples)
   * [.NET Tedarik Hizmeti Müşteri örnekleri](https://github.com/Azure/azure-iot-sdk-csharp/tree/master/provisioning/service/samples)

## <a name="create-a-device-enrollment"></a>Aygıt kaydı oluşturma
Cihazlarınızı sağlama hizmetine kaydetmenin iki yolu vardır:

* **Kayıt grubu,** [kök sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya ara [sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate)tarafından imzalanmış X.509 sertifikalarının ortak bir attestation mekanizmasını paylaşan bir aygıt grubu için giriştir. İstenilen ilk yapılandırmayı paylaşan çok sayıda aygıt veya aynı kiracıya giden aygıtlar için bir kayıt grubu kullanmanızı öneririz. Yalnızca X.509 attestation mekanizmasını kullanan aygıtları *kayıt grupları*olarak kaydedebilirsiniz. 

    Bu iş akışını izleyen SDK'lar içeren bir kayıt grubu oluşturabilirsiniz:

    1. Kayıt grubu için, attestation mekanizması X.509 kök sertifikası kullanır.  Kayıt için attestation oluşturmak için kök sertifikası ile Hizmet SDK API ```X509Attestation.createFromRootCertificate``` çağrı.  X.509 kök sertifikası pem dosyasında veya dize olarak sağlanır.
    1. Oluşturulan ve ```EnrollmentGroup``` benzersiz ```enrollmentGroupId```bir kullanarak yeni bir değişken oluşturun. ```attestation```  İsteğe bağlı olarak, ```Device ID```, ```IoTHubHostName``` ```ProvisioningStatus```, .
    2. Bir kayıt grubu ```createOrUpdateEnrollmentGroup``` oluşturmak ```EnrollmentGroup``` için arka uç uygulamanızda Hizmet SDK API'sini arayın.

* **Bireysel kayıt,** kaydolabilecek tek bir aygıtın girişidir. Tek tek kayıtlar, attestation mekanizmaları olarak X.509 sertifikaları veya SAS belirteçleri (fiziksel veya sanal TPM'den) kullanabilir. Benzersiz ilk yapılandırmalar gerektiren aygıtlar veya yalnızca TPM veya sanal TPM aracılığıyla SAND belirteçlerini attestation mekanizması olarak kullanabilen aygıtlar için tek tek kayıtları kullanmanızı öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Bu iş akışını izleyen SDK'lar ile tek tek bir kayıt oluşturabilirsiniz:
    
    1. TPM ```attestation``` veya X.509 olabilecek mekanizmanızı seçin.
        1. **TPM**: Onay Anahtarını fiziksel bir cihazdan veya TPM Simulator'dan giriş olarak ```TpmAttestation``` kullanarak, kayıt için attestation oluşturmak için Service SDK API'yi arayabilirsiniz. 
        2. **X.509**: Müşteri sertifikasını giriş olarak kullanarak, kayıt için ```X509Attestation.createFromClientCertificate``` attestation oluşturmak için Service SDK API'yi arayabilirsiniz.
    2. Oluşturduğunuz ```IndividualEnrollment``` ```attestation``` ve benzersiz bir giriş ```registrationId``` olarak kullanarak yeni bir değişken oluşturun, hangi cihazınızda veya TPM Simulator oluşturulan.  İsteğe bağlı olarak, ```Device ID```, ```IoTHubHostName``` ```ProvisioningStatus```, .
    3. Tek bir kayıt ```createOrUpdateIndividualEnrollment``` oluşturmak ```IndividualEnrollment``` için arka uç uygulamanızda Hizmet SDK API'sini arayın.

Bir kaydı başarıyla oluşturduktan sonra, Aygıt Sağlama Hizmeti bir kayıt sonucu döndürür. Bu iş akışı [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.

## <a name="update-an-enrollment-entry"></a>Kayıt girişini güncelleştirme

Bir kayıt girişi oluşturduktan sonra, kaydı güncelleştirmek isteyebilirsiniz.  Olası senaryolar arasında istenen özelliğin güncelleştirilmesi, attestation yönteminin güncelleştirilmesi veya aygıt erişiminin iptal edilmesi yer almaktadır.  Tek tek kayıt ve grup kaydı için farklı API'ler vardır, ancak attestation mekanizması için ayrım yoktur.

Bu iş akışını takiben bir kayıt girişini güncelleyebilirsiniz:
* **Bireysel kayıt**:
    1. Hizmet SDK API ```getIndividualEnrollment```ile ilk sağlama hizmetinden en son kaydı alın.
    2. En son kaydın parametresini gerektiği gibi değiştirin. 
    3. En son kaydı kullanarak, kayıt ```createOrUpdateIndividualEnrollment``` girişinizi güncelleştirmek için Hizmet SDK API'sini arayın.
* **Grup kaydı**:
    1. Hizmet SDK API ```getEnrollmentGroup```ile ilk sağlama hizmetinden en son kaydı alın.
    2. En son kaydın parametresini gerektiği gibi değiştirin.
    3. En son kaydı kullanarak, kayıt ```createOrUpdateEnrollmentGroup``` girişinizi güncelleştirmek için Hizmet SDK API'sini arayın.

Bu iş akışı [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.

## <a name="remove-an-enrollment-entry"></a>Kayıt girişini kaldırma

* **Tek tek kayıt** kullanarak ```deleteIndividualEnrollment``` ```registrationId```Hizmet SDK API arayarak silinebilir.
* **Grup kaydı,** Service SDK API'yi ```deleteEnrollmentGroup``` kullanarak ```enrollmentGroupId```arayarak silinebilir.

Bu iş akışı [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.

## <a name="bulk-operation-on-individual-enrollments"></a>Tek tek kayıtlarda toplu işlem

Bu iş akışını izleyen birden çok tek tek kayıt oluşturmak, güncelleştirmek veya kaldırmak için toplu işlem gerçekleştirebilirsiniz:

1. Birden çok ```IndividualEnrollment```değişken içeren bir değişken oluşturun.  Bu değişkenin uygulanması her dil için farklıdır.  Ayrıntılar için GitHub'daki toplu işlem örneğini inceleyin.
2. Hizmet SDK API'yi ```runBulkOperation``` istenilen işlem ```BulkOperationMode``` için ve bireysel kayıtlar için değişkeninizle arayın. Dört mod desteklenir: oluşturma, güncelleştir, güncelleştirifMatchEtag ve silme.

Bir işlemi başarıyla gerçekleştirdikten sonra, Aygıt Sağlama Hizmeti toplu işlem sonucunu döndürecek.

Bu iş akışı [daha önce bahsedilen](#prerequisites)örneklerde gösterilmiştir.
