---
title: Azure IoT Hub, AIR güncelleştirmelerini dağıtmak için ayarlama
description: Azure IoT Hub 'yi Air üzerinden Azure Percept DK 'ye dağıtmak üzere nasıl yapılandıracağınızı öğrenin
author: mimcco
ms.author: mimcco
ms.service: azure-percept
ms.topic: how-to
ms.date: 02/18/2021
ms.custom: template-how-to
ms.openlocfilehash: 80f30ae818b9e74ff97d2bc26552ed7d7124b499
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "101663817"
---
# <a name="how-to-set-up-azure-iot-hub-to-deploy-over-the-air-updates-to-your-azure-percept-dk"></a>Azure IoT Hub, Azure Percept DK 'nize hava güncelleştirmeleri dağıtmak için nasıl ayarlanır
Havayolu arası güncelleştirmeleri kullanarak Azure Percept DK 'yi güvenli ve güncel tutun. Birkaç basit adımda Azure ortamınızı IoT Hub için cihaz güncelleştirmesiyle ayarlayacaksınız ve en son güncelleştirmeleri Azure Percept DK 'nize dağıtabileceksiniz.

## <a name="create-a-device-update-account"></a>Cihaz güncelleştirme hesabı oluştur

1. [Azure Portal](https://portal.azure.com) gidin ve Azure Percept Ile kullandığınız Azure hesabıyla oturum açın 

1. Sayfanın üst kısmındaki ara penceresinde "cihaz güncelleştirmesi IoT Hub" yazmaya başlayın

1. Arama penceresinde göründüğü gibi **IoT Hub 'ları Için cihaz güncelleştirme** ' yi seçin.

1. Sayfanın sol üst kısmındaki **+ Ekle** düğmesine tıklayın.

1. Azure Percept aygıtınızla ilişkili Azure aboneliğini ve kaynak grubunu seçin (Bu, ekleme işleminden IoT Hub).

1. Cihaz güncelleştirme hesabınız için bir ad ve konum belirtin

1. Ayrıntıları gözden geçirin ve ardından **gözden geçir + oluştur**' u seçin.
 
1. Dağıtım tamamlandıktan sonra **Kaynağa Git ' e** tıklayın.
 
## <a name="create-a-device-update-instance"></a>Cihaz güncelleştirme örneği oluşturma
Şimdi, IoT Hub hesabı için cihaz güncelleştirmeniz içinde bir örnek oluşturun.

1. IoT Hub kaynağı için cihaz güncelleştirmeniz ' nda, **örnek yönetimi** altında **örnekler** ' e tıklayın.
 
1. **+ Oluştur**' a tıklayın, bir örnek adı belirtin ve Azure Percept aygıtınızla ilişkili IoT Hub seçin (örneğin, ekleme deneyimi sırasında oluşturulur). Bu işlem birkaç dakika sürebilir.
 
1. **Oluştur** seçeneğine tıklayın

## <a name="configure-iot-hub"></a>IoT Hub Yapılandır

1. Örnek Yönetimi **örnekleri** sayfasında, cihaz güncelleştirme örneğinizin **başarılı** durumuna geçmesinin tamamlanmasını bekleyin. Durumu güncelleştirmek için **Sil** ' ın yanındaki **Yenile** simgesine tıklayın.
 
1. Sizin için oluşturulmuş örneği seçin ve ardından **IoT Hub Yapılandır**' a tıklayın. Sol bölmede, **Bu değişiklikleri yapmak için kabul ediyorum** ' u seçin ve **Güncelleştir**' e tıklayın.
 
1. İşlemin başarıyla tamamlanmasını bekleyin.
 
## <a name="configure-access-control-roles"></a>Erişim denetimi rollerini yapılandırma
Son adım, kullanıcılara güncelleştirme yayımlamak ve dağıtmak için izin vermenizi sağlayacak.

1. IoT Hub kaynağı için cihaz güncelleştirmeniz ' na tıklayın, **erişim denetimi (IAM)**
 
2. **+ Ekle** ' ye tıklayın ve ardından **rol ataması Ekle** ' yi seçin
 
3. **Rol** Için, **Cihaz Güncelleştirme Yöneticisi**' ni seçin. **Kullanıcı, Grup veya hizmet ilkesi** seçmek **için erişim atama** için. **Seç ' i seçin** hesabınızı veya güncelleştirmeleri dağıtacak kişinin hesabını seçin. Ardından **Kaydet**' e tıklayın. 

    > [!TIP]
    > Kuruluşunuzda daha fazla kişiye erişim vermek istiyorsanız, bu adımı yineleyebilir ve bu kullanıcıların her birini bir **Cihaz Güncelleştirme Yöneticisi** yapabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

Artık IoT Hub için cihaz güncelleştirmesini kullanarak [Azure Percept Dev Kit 'i](./how-to-update-over-the-air.md) daha sonra kablosuz olarak güncelleştirebilir ve güncelleştirebilirsiniz. Azure Percept cihazınız için kullandığınız Azure IoT Hub gidin.