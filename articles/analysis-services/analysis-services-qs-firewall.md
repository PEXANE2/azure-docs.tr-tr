---
title: Hızlı başlangıç - Azure'da Analysis Services sunucusu için güvenlik duvarını yapılandırma | Microsoft Docs
description: Azure'da Analysis Services sunucu örneği için güvenlik duvarı yapılandırmayı öğrenin.
author: minewiskan
ms.service: azure-analysis-services
ms.topic: quickstart
ms.date: 07/02/2019
ms.author: owend
ms.reviewer: minewiskan
ms.openlocfilehash: c3c8de80f90fe203029b38171502e3d4a9b9dd46
ms.sourcegitcommit: 8b44498b922f7d7d34e4de7189b3ad5a9ba1488b
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72298522"
---
# <a name="quickstart-configure-server-firewall---portal"></a>Hızlı başlangıç: Sunucu güvenlik duvarını yapılandırma - Portal

Bu hızlı başlangıç Azure Analysis Services sunucunuz için güvenlik duvarını yapılandırmanıza yardımcı olur. Güvenlik duvarını etkinleştirmek ve yalnızca sunucunuza erişen bilgisayarlar için IP adresi aralıkları yapılandırmak, sunucunuzu ve verilerinizi güvenlik altına almanın önemli bir parçasıdır.

## <a name="prerequisites"></a>Önkoşullar

- Aboneliğinizde bir Analysis Services sunucusu. Daha fazla bilgi edinmek için bkz. [Hızlı başlangıç: Sunucu oluşturma - Portal](analysis-services-create-server.md) veya [Hızlı başlangıç: Sunucu oluşturma - PowerShell](analysis-services-create-powershell.md)
- İstemci bilgisayarlar için bir veya birden çok IP adresi aralığı (gerekiyorsa).
- Power BI Premium 'den Içeri aktarma senaryosunun Şu anda desteklenmediğini lütfen unutmayın.

## <a name="sign-in-to-the-azure-portal"></a>Azure portalında oturum açın 

[Portalda oturum açın](https://portal.azure.com)

## <a name="configure-a-firewall"></a>Güvenlik duvarını yapılandırma

1. Genel Bakış sayfasını açmak için sunucunuza tıklayın. 
2. **AYARLAR** > **Güvenlik Duvarı** > **Güvenlik duvarını etkinleştir** altında **Açık** seçeneğine tıklayın.
3. Power BI hizmetinden DirectQuery erişimine izin vermek için, **Power BI'dan erişime izin ver** alanında **Açık** seçeneğine tıklayın.  
4. (İsteğe bağlı) Bir veya birden çok IP adresi aralığı belirtin. Her aralık için bir ad, başlangıç ve bitiş IP adresi girin. Güvenlik duvarı kuralı adı 128 karakterle sınırlı olmalıdır ve yalnızca büyük harf, küçük harf karakter, rakam, alt çizgi ve kısa çizgi karakterlerini içerebilir. Boşluk ve diğer özel karakterlere izin verilmez.
5. **Kaydet** düğmesine tıklayın.

     ![Güvenlik duvarı ayarları](./media/analysis-services-qs-firewall/aas-qs-firewall.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Artık ihtiyacınız kalmadığında, IP adresi aralıklarını silin veya güvenlik duvarını devre dışı bırakın.

## <a name="next-steps"></a>Sonraki adımlar
Bu hızlı başlangıçta, sunucunuz için güvenlik duvarı yapılandırmayı öğrendiniz. Artık sunucunuz olduğuna ve bir güvenlik duvarıyla sunucunun güvenliğini sağladığınıza göre, ona portaldan temel bir örnek veri modeli ekleyebilirsiniz. Örnek modelin olması, model veritabanı rollerini yapılandırma ve istemci bağlantılarını test etme işlemlerini öğrenmeye yardımcı olur. Daha fazla bilgi edinmek için, örnek model ekleme öğreticisiyle devam edin.

> [!div class="nextstepaction"]
> [Öğretici: Sunucunuza örnek model ekleme](analysis-services-create-sample-model.md)
