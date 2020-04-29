---
title: Cihaz erişimi, güç ve bağlantı modunu Azure Data Box Gateway
description: Azure 'a veri aktarmaya yardımcı olan Azure Data Box Gateway cihaz için erişim, güç ve bağlantı modunun nasıl yönetileceğini açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: gateway
ms.topic: article
ms.date: 06/03/2019
ms.author: alkohli
ms.openlocfilehash: e4d85bd460c39964c9f42ac946e3522f5f129c1c
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "79474450"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-gateway"></a>Azure Data Box Gateway için erişimi, güç ve bağlantı modunu yönetme

Bu makalede, Azure Data Box Gateway erişim, güç ve bağlantı modunun nasıl yönetileceği açıklanır. Bu işlemler yerel Web Kullanıcı arabirimi veya Azure portal aracılığıyla gerçekleştirilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Cihaz erişimini yönetme
> * Bağlantı modunu Yönet
> * Güç yönetimi

## <a name="manage-device-access"></a>Cihaz erişimini yönetme

Data Box Gateway cihazınıza erişim, bir cihaz parolasının kullanımı ile denetlenir. Parolayı yerel Web Kullanıcı arabirimi aracılığıyla değiştirebilirsiniz. Ayrıca Azure portal cihaz parolasını sıfırlayabilirsiniz.

### <a name="change-device-password"></a>Cihaz parolasını değiştirme

Cihaz parolasını değiştirmek için yerel kullanıcı arabirimindeki adımları izleyin.

1. Yerel Web Kullanıcı arabiriminde **bakım > parola değiştirme**' ye gidin.
2. Geçerli parolayı ve ardından yeni parolayı girin. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterlerden 3 ' i içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler. Yeni parolayı onaylayın.

    ![Parolayı değiştir](media/data-box-gateway-manage-access-power-connectivity-mode/change-password-1.png)

3. **Parolayı Değiştir**' e tıklayın.
 
### <a name="reset-device-password"></a>Cihaz parolasını sıfırla

Sıfırlama iş akışı, kullanıcının eski parolayı geri çekemesini gerektirmez ve parola kaybolduğunda faydalıdır. Bu iş akışı Azure portal gerçekleştirilir.

1. Azure portal, **genel bakış > yönetici parolasını sıfırla**' ya gidin.

    ![Parola sıfırlama](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-1.png)

 
2. Yeni parolayı girip onaylayın. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola şu karakterlerden 3 ' i içermelidir: büyük harf, küçük harf, sayısal ve özel karakterler. **Sıfırla**' ya tıklayın.

    ![Parola sıfırlama](media/data-box-gateway-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Kaynak erişimini yönetme

Data Box Edge/Data Box Gateway, IoT Hub ve Azure depolama kaynağını oluşturmak için, bir kaynak grubu düzeyinde katkıda bulunan veya daha yüksek izinlere sahip olmanız gerekir. Ayrıca, kaydedilecek ilgili kaynak sağlayıcılarının da olması gerekir. Etkinleştirme anahtarı ve kimlik bilgilerini içeren tüm işlemler için Azure Active Directory Graph API izinleri de gereklidir. Bunlar aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-microsoft-graph-api-permissions"></a>Microsoft Graph API izinlerini yönetme

Data Box Edge cihaz için etkinleştirme anahtarını oluştururken veya kimlik bilgileri gerektiren işlemler gerçekleştirirken, Microsoft Graph API 'sine yönelik izinlere ihtiyacınız vardır. Kimlik bilgileri gerektiren işlemler şunlar olabilir:

-  İlişkili bir depolama hesabıyla bir paylaşma oluşturuluyor.
-  Cihazdaki paylaşımlara erişebilen bir Kullanıcı oluşturma.

Active Directory kiracısında yapabilmeniz için bir `User` erişiminizin olması gerekir. `Read all directory objects` Bir Konuk Kullanıcı, izinleri olmadığı için `Read all directory objects`bu kullanıcı olamaz. Konuksanız etkinleştirme anahtarı oluşturma, Data Box Edge cihazınızda paylaşım oluşturma, kullanıcı oluşturma gibi işlemlerin tümü başarısız olur.

API Microsoft Graph için kullanıcılara erişim sağlama hakkında daha fazla bilgi için, bkz. [Microsoft Graph izinleri başvurusu](https://docs.microsoft.com/graph/permissions-reference).

### <a name="register-resource-providers"></a>Kaynak sağlayıcılarını kaydetme

Azure 'da bir kaynak sağlamak için (Azure Resource Manager modelinde), bu kaynağın oluşturulmasını destekleyen bir kaynak sağlayıcısına ihtiyacınız vardır. Örneğin, bir sanal makine sağlamak için abonelikte bir ' Microsoft. COMPUTE ' kaynak sağlayıcısına sahip olmanız gerekir.
 
Kaynak sağlayıcıları abonelik düzeyinde kaydedilir. Varsayılan olarak tüm yeni Azure abonelikleri sık kullanılan kaynak sağlayıcıları listesiyle önceden kaydedilir. ' Microsoft. DataBoxEdge ' için kaynak sağlayıcısı bu listeye dahil değildir.

Bu kaynakların kaynak sağlayıcıları zaten kayıtlı olduğu sürece, kullanıcıların sahip oldukları kaynak grupları dahilinde ' Microsoft. DataBoxEdge ' gibi kaynaklar oluşturabilmeleri için abonelik düzeyine erişim izni vermeniz gerekmez.

Herhangi bir kaynak oluşturmayı denemeden önce, kaynak sağlayıcının abonelikte kayıtlı olduğundan emin olun. Kaynak sağlayıcısı kayıtlı değilse, yeni kaynağı oluşturan kullanıcının abonelik düzeyinde gerekli kaynak sağlayıcısını kaydetmek için yeterli haklara sahip olduğundan emin olmanız gerekir. Bunu yapmadıysanız, şu hatayı görürsünüz:

*> abonelik \<abonelik adı kaynak sağlayıcıları kaydetme iznine sahip değil: Microsoft. DataBoxEdge.*


Geçerli abonelikteki kayıtlı kaynak sağlayıcılarının bir listesini almak için aşağıdaki komutu çalıştırın:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Data Box Edge cihaz `Microsoft.DataBoxEdge` için kaydedilmelidir. Kaydolmak `Microsoft.DataBoxEdge`için, abonelik Yöneticisi aşağıdaki komutu çalıştırmalıdır:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Kaynak sağlayıcısını kaydetme hakkında daha fazla bilgi için bkz. [kaynak sağlayıcısı kaydı hatalarını giderme](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors).

## <a name="manage-connectivity-mode"></a>Bağlantı modunu Yönet

Varsayılan normal moddan ayrı olarak, cihazınız kısmen bağlantısı kesik veya bağlantısı kesilmiş modda da çalıştırılabilir. Bu modların her biri aşağıda gösterildiği gibi açıklanmıştır:

- **Kısmen bağlantısı kesik** – bu modda, cihaz paylaşımlara veri indiremez, ancak Azure Portal aracılığıyla yönetilebilir.

    Bu mod genellikle ölçülen uydu ağı kullanılırken kullanılır ve amaç ağ bant genişliği tüketimini en aza indirmektir. Cihaz izleme işlemleri için en az ağ tüketimi hala meydana gelebilir.

- **Bağlı** değil – bu modda, cihazın bulutla bağlantısı tamamen kesilir ve hem bulut karşıya yüklemeleri hem de İndirmeleri devre dışı bırakılır. Cihaz yalnızca yerel Web Kullanıcı arabirimi aracılığıyla yönetilebilir.

    Bu mod genellikle cihazınızı çevrimdışına almak istediğinizde kullanılır.

Cihaz modunu değiştirmek için şu adımları izleyin:

1. Cihazınızın yerel Web Kullanıcı arabiriminde, **yapılandırma > bulut ayarları**' na gidin.
2. **Bulutu karşıya yüklemeyi ve indirmeyi**devre dışı bırakın.
3. Cihazı kısmen bağlantısı kesik modda çalıştırmak için **Azure Portal yönetimini**etkinleştirin.

    ![Bağlantı modu](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-1.png)
 
4. Cihazı bağlantısı kesik modda çalıştırmak için **Azure Portal yönetimini**devre dışı bırakın. Artık cihaz yalnızca yerel Web Kullanıcı arabirimi aracılığıyla yönetilebilir.

    ![Bağlantı modu](media/data-box-gateway-manage-access-power-connectivity-mode/connectivity-mode-2.png)

## <a name="manage-power"></a>Güç yönetimi

Yerel Web Kullanıcı arabirimini kullanarak sanal cihazınızı kapatabilir veya yeniden başlatabilirsiniz. Cihazı yeniden başlatmadan önce konaktaki paylaşımları sonra da cihazı çevrimdışına almanız önerilir. Bu eylem, herhangi bir veri bozulması olasılığını en aza indirir.

1. Yerel Web Kullanıcı arabiriminde **bakım > güç ayarları**' na gidin.
2. Ne yapmak istediğinize bağlı olarak, **kapalı** veya **yeniden başlatma** ' ya tıklayın.

    ![Güç ayarları](media/data-box-gateway-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Onay istendiğinde, devam etmek için **Evet** ' e tıklayın.

> [!NOTE]
> Sanal cihazı kapatırsanız, cihazı hiper yönetici yönetimi aracılığıyla başlatmanız gerekecektir.
