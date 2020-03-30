---
title: Rol için Uzak Masaüstü'nü etkinleştirmek için portalı kullanma
titleSuffix: Azure Cloud Services
description: Uzak masaüstü bağlantılarına izin verecek şekilde azure bulut hizmeti uygulamanızı yapılandırma
services: cloud-services
documentationcenter: ''
author: mmccrory
ms.service: cloud-services
ms.topic: article
ms.date: 11/28/2016
ms.author: memccror
ms.openlocfilehash: d65f4b55be317234c10a0e90cfe413d9e38a6a90
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79247468"
---
# <a name="enable-remote-desktop-connection-for-a-role-in-azure-cloud-services"></a>Azure Bulut Hizmetlerinde Rol Için Uzak Masaüstü Bağlantısını Etkinleştir

> [!div class="op_single_selector"]
> * [Azure portalında](cloud-services-role-enable-remote-desktop-new-portal.md)
> * [Powershell](cloud-services-role-enable-remote-desktop-powershell.md)
> * [Visual Studio](cloud-services-role-enable-remote-desktop-visual-studio.md)

Uzak Masaüstü, Azure'da çalışan bir rolün masaüstüne erişmenizi sağlar. Çalışırken uygulamanızla ilgili sorunları gidermek ve tanılamak için Uzak Masaüstü bağlantısını kullanabilirsiniz.

Geliştirme sırasında, Uzak Masaüstü modüllerini hizmet tanımınıza ekleyerek rolünüzde uzak masaüstü bağlantısını etkinleştirebilir veya Uzak Masaüstü Uzantısı aracılığıyla Uzak Masaüstü'nü etkinleştirmeyi seçebilirsiniz. Tercih edilen yaklaşım, uygulamanızı yeniden dağıtmak zorunda kalmadan uygulama dağıtıldıktan sonra bile Uzak Masaüstü'nü etkinleştirebileceğiniz için Uzak Masaüstü uzantısını kullanmaktır.

## <a name="configure-remote-desktop-from-the-azure-portal"></a>Azure portalından Uzak Masaüstünü Yapılandırma

Azure portalı, uygulama dağıtıldıktan sonra bile Uzak Masaüstü'nü etkinleştirebilmeniz için Uzak Masaüstü Uzantısı yaklaşımını kullanır. Bulut hizmetiniz için **Uzak Masaüstü** ayarları, Uzak Masaüstü'nü etkinleştirmenize, sanal makinelere bağlanmak için kullanılan yerel Yönetici hesabını değiştirmenize, kimlik doğrulamada kullanılan sertifikayı değiştirmenize ve son kullanma tarihini ayarlamanıza olanak tanır.

1. **Bulut Hizmetleri'ni**tıklatın, bulut hizmetinin adını seçin ve ardından **Uzak Masaüstü'nü**seçin.

    ![Bulut hizmetleri uzak masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop.png)

2. Tek bir rol için veya tüm roller için Uzak Masaüstü'nü etkinleştirmek isteyip istemediğinizi seçin ve ardından anahtarlayıcının değerini **Etkin**olarak değiştirin.

3. Kullanıcı adı, parola, son kullanma tarihi ve sertifika için gerekli alanları doldurun.

    ![Bulut hizmetleri uzak masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Details.png)

   > [!WARNING]
   > Uzak Masaüstü'nü ilk etkinleştirdiğinizde ve **Tamam** 'ı (onay işareti) seçtiğinizde tüm rol örnekleri yeniden başlatılır. Yeniden başlatmayı önlemek için parolayı şifrelemek için kullanılan sertifikanın role yüklenmesi gerekir. Yeniden başlatmayı önlemek [için bulut hizmeti için bir sertifika yükleyin](cloud-services-configure-ssl-certificate-portal.md#step-3-upload-a-certificate) ve ardından bu iletişim kutusuna dönün.

4. **Roller'de,** güncelleştirmek istediğiniz rolü seçin veya tüm roller için **Tümü'nü** seçin.

5. Yapılandırma güncelleştirmelerinizi tamamladığınızda **Kaydet'i**seçin. Rol örneklerinizin bağlantı almaya hazır olması birkaç dakika sürer.

## <a name="remote-into-role-instances"></a>Rol örneklerine uzak

Rollerde Uzak Masaüstü etkinleştirildikten sonra, doğrudan Azure portalından bağlantı başlatabilirsiniz:

1. **Örnekler** ayarlarını açmak için **Örnekler'i** tıklatın.
2. Uzak Masaüstü yapılandırılmış bir rol örneği seçin.
3. Rol örneği için bir RDP dosyasını indirmek için **Bağlan'ı** tıklatın.

    ![Bulut hizmetleri uzak masaüstü](./media/cloud-services-role-enable-remote-desktop-new-portal/CloudServices_Remote_Desktop_Connect.png)

4. Uzak Masaüstü bağlantısını başlatmak için **Aç'ı** ve ardından **Bağlan'ı** tıklatın.

>[!NOTE]
> Bulut hizmetiniz bir NSG'nin arkasında oturuyorsa, **3389** ve **20000**bağlantı noktalarında trafiğe izin veren kurallar oluşturmanız gerekebilir.  Uzak Masaüstü **3389**portu kullanır.  Bulut Hizmeti örnekleri yük dengeliolduğundan, hangi örne bağlanışla bağlanabileceğinizi doğrudan denetemezsiniz.  *RemoteForwarder* ve *RemoteAccess* aracıları RDP trafiğini yönetir ve istemcinin bir RDP çerezi göndermesine ve bağlanmak için tek bir örnek belirtmesine izin verir.  *RemoteForwarder* ve *RemoteAccess* aracıları, **20000*** bağlantı noktasının açık olmasını gerektirir ve bu bağlantı noktası NSG'niz varsa engellenebilir.

## <a name="additional-resources"></a>Ek kaynaklar

[Cloud Services’ı Yapılandırma](cloud-services-how-to-configure-portal.md)
