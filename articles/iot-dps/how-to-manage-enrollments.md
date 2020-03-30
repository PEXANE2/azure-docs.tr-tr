---
title: Azure portalında Azure IoT Hub Aygıt Sağlama Hizmeti için aygıt kayıtlarını yönetme
description: Azure Portalı'nda Cihaz Sağlama Hizmetiniz (DPS) için cihaz kayıtlarını yönetme
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 8bd896573dbd0b24df11c0d66009098de19654ac
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74974947"
---
# <a name="how-to-manage-device-enrollments-with-azure-portal"></a>Azure Portalı ile cihaz kayıtlarını yönetme

*Aygıt kaydı,* bir noktada Azure IoT Hub Aygıt Sağlama Hizmeti'ne kaydolabilecek tek bir aygıtın veya bir aygıt grubunun kaydını oluşturur. Kayıt kaydı, istenen IoT hub'ı da dahil olmak üzere, bu kaydın bir parçası olarak aygıt(lar) için ilk istenen yapılandırmayı içerir. Bu makalede, sağlama hizmetiniz için aygıt kayıtlarını nasıl yönetiştize gösterilmektedir.


## <a name="create-a-device-enrollment"></a>Aygıt kaydı oluşturma

Cihazlarınızı sağlama hizmetine kaydetmenin iki yolu vardır:

* **Kayıt grubu,** fiziksel aygıtta aygıt sertifikası oluşturmak için kullanılan [kök sertifikası](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya ara [sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate)olabilecek aynı imza sertifikası tarafından imzalanmış X.509 sertifikalarının ortak bir attestation mekanizmasını paylaşan bir aygıt grubu için giriştir. İstenilen ilk yapılandırmayı paylaşan çok sayıda aygıt veya aynı kiracıya giden aygıtlar için bir kayıt grubu kullanmanızı öneririz. Yalnızca X.509 attestation mekanizmasını kullanan aygıtları *kayıt grupları*olarak kaydedebilirsiniz. 

    Aşağıdaki adımları kullanarak bir aygıt grubu için portalda bir kayıt grubu oluşturabilirsiniz:

  1. Azure portalına giriş yapın ve sol menüden **Tüm kaynaklar'ı** tıklatın.  
  1. Cihazınızı kaynaklar listesinden kaydetmek istediğiniz Aygıt Sağlama hizmetini tıklatın.  
  1. Sağlama hizmetinizde:  
     a. **Kayıtları Yönet'i**tıklatın ve ardından **Kayıt Grupları** sekmesini seçin.  
     b. Üstteki **Ekle** düğmesine tıklayın.  
     c. "Kayıt Grubu Ekle" paneli göründüğünde, kayıt listesi girişiiçin bilgileri girin.  **Grup adı** gereklidir. Ayrıca **Sertifika türü**için "CA veya Intermediate" seçeneğini belirleyin ve aygıt grubu için temel **Birincil sertifikayı** yükleyin.  
     d. **Kaydet**'e tıklayın. Kayıt grubunuzun başarılı bir şekilde oluşturulmasında, grup adının **Kayıt Grupları** sekmesinin altında göründüğünü görmeniz gerekir.  

     [![Portaldaki kayıt grubu](./media/how-to-manage-enrollments/group-enrollment.png)](./media/how-to-manage-enrollments/group-enrollment.png#lightbox)
    

* **Bireysel kayıt,** kaydolabilecek tek bir aygıtın girişidir. Tek tek kayıtlar, attestation mekanizmaları olarak x509 sertifikaları veya SAS belirteçleri (fiziksel veya sanal TPM'den) kullanabilir. Benzersiz ilk yapılandırmalar gerektiren aygıtlar veya yalnızca TPM veya sanal TPM aracılığıyla SAND belirteçlerini attestation mekanizması olarak kullanabilen aygıtlar için tek tek kayıtları kullanmanızı öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Aşağıdaki adımları kullanarak portalda tek tek bir kayıt oluşturabilirsiniz:

    1. Azure portalına giriş yapın ve sol menüden **Tüm kaynaklar'ı** tıklatın.
    1. Cihazınızı kaynaklar listesinden kaydetmek istediğiniz Aygıt Sağlama hizmetini tıklatın.
    1. Sağlama hizmetinizde:  
       a. **Kayıtları Yönet'i**tıklatın ve ardından **Bireysel Kayıtlar** sekmesini seçin.  
       b. Üstteki **Ekle** düğmesine tıklayın.   
       c. "Kayıt Ekle" paneli göründüğünde, kayıt listesi girişiiçin bilgileri girin. Önce aygıt için attestation **Mekanizmasını** seçin (X.509 veya TPM). X.509 attestation cihaz için yaprak **Birincil sertifika** yüklemenizi gerektirir. TPM, cihaz için **Attestation Key** ve **Registration ID'yi** girmenizi gerektirir.  
       d. **Kaydet**'e tıklayın. Kayıt grubunuzun başarılı bir şekilde oluşturulmasında, cihazınızın **Bireysel Kayıtlar** sekmesinin altında göründüğünü görmeniz gerekir.  

       [![Portala bireysel kayıt](./media/how-to-manage-enrollments/individual-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

## <a name="update-an-enrollment-entry"></a>Kayıt girişini güncelleştirme
Portaldaki varolan bir kayıt girişini aşağıdaki adımları kullanarak güncelleyebilirsiniz:

1. Azure portalında Cihaz Sağlama hizmetinizi açın ve **Kayıtları Yönet'i**tıklatın. 
1. Değiştirmek istediğiniz kayıt girişine gidin. Cihaz kaydınızla ilgili özet bilgileri açan girişi tıklatın. 
1. Bu sayfada, aygıtın bağlanması gereken IoT hub'ı gibi güvenlik türü ve kimlik bilgileri dışındaki öğeleri ve aygıt kimliği dışında değiştirebilirsiniz. Ayrıca, ilk aygıt ikiz durumunu da değiştirebilirsiniz. 
1. Tamamlandıktan sonra, aygıt kaydınızı güncellemek için **Kaydet'i** tıklatın. 

    ![Portaldaki kaydı güncelleştirme](./media/how-to-manage-enrollments/update-enrollment.png)

## <a name="remove-a-device-enrollment"></a>Aygıt kaydını kaldırma
Cihazınızın herhangi bir IoT hub'ına sağlanması gerekmemesi gereken durumlarda, aşağıdaki adımları kullanarak portaldaki ilgili kayıt girişini kaldırabilirsiniz:

1. Azure portalında Cihaz Sağlama hizmetinizi açın ve **Kayıtları Yönet'i**tıklatın. 
1. Kaldırmak istediğiniz kayıt girişine gidin ve seçin. 
1. Üstteki **Sil** düğmesini tıklatın ve onaylamak istendiğinde **Evet'i** seçin. 
1. Eylem tamamlandıktan sonra, girişinizin aygıt kayıtları listesinden kaldırıldığını görürsünüz. 
 
    ![Portaldaki kaydı kaldırma](./media/how-to-manage-enrollments/remove-enrollment.png)


