---
title: Azure IoT Hub cihaz sağlama hizmeti için cihaz kayıtlarını yönetme Azure portal
description: Azure portalında cihaz sağlama hizmetiniz (DPS) için cihaz kayıtlarını yönetme
author: wesmc7777
ms.author: wesmc
ms.date: 04/05/2018
ms.topic: conceptual
ms.service: iot-dps
services: iot-dps
manager: timlt
ms.openlocfilehash: 8bd896573dbd0b24df11c0d66009098de19654ac
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "74974947"
---
# <a name="how-to-manage-device-enrollments-with-azure-portal"></a>Azure Portal ile cihaz kayıtlarını yönetme

Bir *cihaz kaydı* , bazı noktaları Azure IoT Hub cihaz sağlama hizmeti 'ne kaydedebileceği tek bir cihazın veya cihaz grubunun bir kaydını oluşturur. Kayıt kaydı, istenen IoT Hub 'ı da dahil olmak üzere, bu kayıt kapsamında cihaz (ler) için ilk istenen yapılandırmayı içerir. Bu makalede, sağlama hizmetiniz için cihaz kayıtlarını yönetme işlemi gösterilmektedir.


## <a name="create-a-device-enrollment"></a>Cihaz kaydı oluşturma

Cihazları sağlama hizmeti ile kaydedebilmeniz için iki yol vardır:

* **Kayıt grubu** , fiziksel cihazda cihaz sertifikası oluşturmak için kullanılan, [kök sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#root-certificate) veya [Ara Sertifika](https://docs.microsoft.com/azure/iot-dps/concepts-security#intermediate-certificate)olabilen aynı imza sertifikası tarafından imzalanan, X. 509.440 sertifikalarının ortak kanıtlama mekanizmasını paylaşan bir grup cihaz için giriştir. İstenen ilk yapılandırmayı paylaşan çok sayıda cihaz için veya hepsi aynı kiracıya giden cihazlar için bir kayıt grubu kullanmanızı öneririz. Yalnızca X. 509.440 kanıtlama mekanizmasını kullanan cihazları *kayıt grupları*olarak kaydedebileceğinizi unutmayın. 

    Aşağıdaki adımları kullanarak bir cihaz grubu için portalda bir kayıt grubu oluşturabilirsiniz:

  1. Azure portal oturum açın ve sol taraftaki menüden **tüm kaynaklar** ' a tıklayın.  
  1. Cihazınızı kaynak listesinden kaydetmek istediğiniz cihaz sağlama hizmeti ' ne tıklayın.  
  1. Sağlama hizmetinizde:  
     a. Kayıtları **Yönet**' e tıklayın ve ardından **kayıt grupları** sekmesini seçin.  
     b. Üstteki **Ekle** düğmesine tıklayın.  
     c. "Kayıt grubu Ekle" paneli göründüğünde, kayıt listesi girişi bilgilerini girin.  **Grup adı** gereklidir. Ayrıca, **sertifika türü**IÇIN "CA veya ara" yı seçin ve cihaz grubu Için kök **birincil sertifikayı** karşıya yükleyin.  
     d. **Kaydet**’e tıklayın. Kayıt grubunuzun başarıyla oluşturulması sırasında, Grup adının **kayıt grupları** sekmesinde göründüğünü görmeniz gerekir.  

     [![Portalda kayıt grubu](./media/how-to-manage-enrollments/group-enrollment.png)](./media/how-to-manage-enrollments/group-enrollment.png#lightbox)
    

* **Tek bir kayıt** , kaydedebilen tek bir cihaz için giriştir. Bireysel kayıtlar, x509 sertifikalarını veya SAS belirteçlerini (fiziksel ya da sanal TPM 'den) kanıtlama mekanizmaları olarak kullanabilir. Benzersiz ilk yapılandırma gerektiren cihazlar için veya yalnızca kanıtlama mekanizması olarak TPM veya sanal TPM aracılığıyla SAS belirteçlerini kullanan cihazlar için ayrı kayıtlar kullanmanızı öneririz. Bireysel kayıtlar için istenen IoT hub cihazı kimliği belirtilmiş olabilir.

    Aşağıdaki adımları kullanarak portalda tek bir kayıt oluşturabilirsiniz:

    1. Azure portal oturum açın ve sol taraftaki menüden **tüm kaynaklar** ' a tıklayın.
    1. Cihazınızı kaynak listesinden kaydetmek istediğiniz cihaz sağlama hizmeti ' ne tıklayın.
    1. Sağlama hizmetinizde:  
       a. Kayıtları **Yönet**' e tıklayın, sonra **bireysel** kayıtlar sekmesini seçin.  
       b. Üstteki **Ekle** düğmesine tıklayın.   
       c. "Kayıt Ekle" paneli göründüğünde, kayıt listesi girişinin bilgilerini girin. Önce cihaz için kanıtlama **mekanizmasını** seçin (X. 509.440 veya TPM). X. 509.440 kanıtlama, cihazın yaprak **birincil sertifikasını** karşıya yüklemenizi gerektirir. TPM, cihaz için **kanıtlama anahtarını** ve **kayıt kimliğini** girmenizi gerektirir.  
       d. **Kaydet**’e tıklayın. Kayıt grubunuzun başarıyla oluşturulması sırasında, cihazınızın **bireysel** kayıtlar sekmesinde görüntülendiğini görmeniz gerekir.  

       [![Portalda bireysel kayıt](./media/how-to-manage-enrollments/individual-enrollment.png)](./media/how-to-manage-enrollments/individual-enrollment.png#lightbox)

## <a name="update-an-enrollment-entry"></a>Kayıt girişini güncelleştirme
Portalda mevcut bir kayıt girişini aşağıdaki adımları kullanarak güncelleştirebilirsiniz:

1. Cihaz sağlama hizmetinizi Azure portal açın ve kayıtları **Yönet**' e tıklayın. 
1. Değiştirmek istediğiniz kayıt girdisine gidin. Cihaz kaydınız hakkında özet bilgiler açan girişe tıklayın. 
1. Bu sayfada, cihazın bağlanması gereken IoT Hub 'ı ve cihaz KIMLIĞI gibi güvenlik türü ve kimlik bilgileri dışındaki öğeleri de değiştirebilirsiniz. Başlangıçtaki cihaz ikizi durumunu da değiştirebilirsiniz. 
1. Tamamlandıktan sonra, cihaz kaydınız güncelleştirmek için **Kaydet** ' e tıklayın. 

    ![Portalda kaydı güncelleştir](./media/how-to-manage-enrollments/update-enrollment.png)

## <a name="remove-a-device-enrollment"></a>Cihaz kaydını kaldırma
Cihazınızın herhangi bir IoT Hub 'ına sağlanması gerekmeyen durumlarda, aşağıdaki adımları kullanarak portalda ilgili kayıt girişini kaldırabilirsiniz:

1. Cihaz sağlama hizmetinizi Azure portal açın ve kayıtları **Yönet**' e tıklayın. 
1. ' A gidin ve kaldırmak istediğiniz kayıt girişini seçin. 
1. Üstteki **Sil** düğmesine tıklayın ve ardından onaylamanız istendiğinde **Evet** ' i seçin. 
1. Eylem tamamlandığında, girdinizi cihaz kayıtları listesinden kaldırıldığını görürsünüz. 
 
    ![Portalda kaydı kaldırma](./media/how-to-manage-enrollments/remove-enrollment.png)


