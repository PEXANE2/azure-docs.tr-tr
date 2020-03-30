---
title: Azure Veri Kutusu Kenarı aygıt erişimi, gücü ve bağlantı modu | Microsoft Dokümanlar
description: Azure Veri Kutusu Kenarı aygıtının azure'a veri aktarımı için erişim, güç ve bağlantı modunu nasıl yönetirize edilebildiğini açıklar
services: databox
author: alkohli
ms.service: databox
ms.subservice: edge
ms.topic: article
ms.date: 06/24/2019
ms.author: alkohli
ms.openlocfilehash: be4b780161003470622cb367d78138cfeffe341b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79454341"
---
# <a name="manage-access-power-and-connectivity-mode-for-your-azure-data-box-edge"></a>Azure Veri Kutusu Edge'iniz için erişim, güç ve bağlantı modunu yönetme

Bu makalede, Azure Veri Kutusu Edge'inizin erişim, güç ve bağlantı modunun nasıl yönetilenolduğu açıklanmaktadır. Bu işlemler yerel web ui veya Azure portalı üzerinden gerçekleştirilir.

Bu makalede şunları öğreneceksiniz:

> [!div class="checklist"]
> * Aygıt erişimini yönetme
> * Bağlantı modunu yönetme
> * Gücü yönetin


## <a name="manage-device-access"></a>Aygıt erişimini yönetme

Veri Kutusu Edge cihazınıza erişim bir aygıt parolası nın kullanımı yla denetlenir. Parolayı yerel web ui üzerinden değiştirebilirsiniz. Azure portalındaki aygıt parolasını da sıfırlayabilirsiniz.

### <a name="change-device-password"></a>Cihaz parolasını değiştirme

Aygıt parolasını değiştirmek için yerel UI'deki aşağıdaki adımları izleyin.

1. Yerel web ui'sinde, **Maintenance > Password değişikliğine**gidin.
2. Geçerli parolayı ve ardından yeni parolayı girin. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola aşağıdaki karakterlerden 3'ü olmalıdır: büyük harf, küçük harf, sayısal ve özel karakterler. Yeni parolayı onaylayın.

    ![Parolayı değiştir](media/data-box-edge-manage-access-power-connectivity-mode/change-password-1.png)

3. **Parolayı Değiştir'i**seçin.
 
### <a name="reset-device-password"></a>Aygıt parolasını sıfırlama

Sıfırlama iş akışı, kullanıcının eski parolayı geri çağırmasını gerektirmez ve parola kaybolduğunda kullanışlıdır. Bu iş akışı Azure portalında gerçekleştirilir.

1. Azure portalında, **Yönetici parolasını sıfırlama> genel e gidin.**

    ![Parola sıfırlama](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-1.png)


2. Yeni parolayı girin ve onaylayın. Sağlanan parola 8 ile 16 karakter arasında olmalıdır. Parola aşağıdaki karakterlerden 3'ü olmalıdır: büyük harf, küçük harf, sayısal ve özel karakterler. **Sıfırla**’yı seçin.

    ![Parola sıfırlama](media/data-box-edge-manage-access-power-connectivity-mode/reset-password-2.png)

## <a name="manage-resource-access"></a>Kaynak erişimini yönetme

Veri Kutusu Kenarı/Veri Kutusu Ağ Geçidi, IoT Hub ve Azure Depolama kaynağınızı oluşturmak için, kaynak grubu düzeyinde katılımcı veya daha yüksek izinlere ihtiyacınız vardır. Ayrıca, ilgili kaynak sağlayıcılarının kaydedilmesi gerekir. Etkinleştirme anahtarı ve kimlik bilgileri içeren işlemler için Microsoft Graph API'ye izin verilmesi de gereklidir. Bunlar aşağıdaki bölümlerde açıklanmıştır.

### <a name="manage-microsoft-graph-api-permissions"></a>Microsoft Graph API izinlerini yönetme

Veri Kutusu Kenarı aygıtı için etkinleştirme anahtarı oluştururken veya kimlik bilgileri gerektiren işlemleri gerçekleştirirken, Microsoft Graph API için izinlere ihtiyacınız vardır. Kimlik bilgileri gerektiren işlemler şu şekilde olabilir:

-  İlişkili bir depolama hesabıyla paylaşım oluşturma.
-  Aygıttaki paylaşımlara erişebilen bir kullanıcı oluşturma.

Eğer yapabilmek `User` için gereken Active Directory kiracı bir `Read all directory objects`erişim olmalıdır. Çünkü onlar için `Read all directory objects`izinleri yok bir Konuk kullanıcı olamaz. Misafirseniz, etkinleştirme anahtarının oluşturulması, Veri Kutusu Edge aygıtınızda paylaşım oluşturulması, kullanıcı oluşturma, Edge bilgi işlem rolünün yapılandırması, aygıt parolasını sıfırlama gibi işlemler başarısız olur.

Kullanıcılara Microsoft Graph API'ye erişim innasıl sağlayabileceğiniz hakkında daha fazla bilgi için [Microsoft Graph izinleri başvurusuna](https://docs.microsoft.com/graph/permissions-reference)bakın.

### <a name="register-resource-providers"></a>Kaynak sağlayıcılar kaydedin

Azure'da (Azure Kaynak Yöneticisi modelinde) bir kaynak sağlamak için, söz konusu kaynağın oluşturulmasını destekleyen bir kaynak sağlayıcısına ihtiyacınız var. Örneğin, sanal bir makine sağlamak için abonelikte bir 'Microsoft.Compute' kaynak sağlayıcısı olmalıdır.
 
Kaynak sağlayıcıları abonelik düzeyinde kaydedilir. Varsayılan olarak tüm yeni Azure abonelikleri sık kullanılan kaynak sağlayıcıları listesiyle önceden kaydedilir. 'Microsoft.DataBoxEdge' kaynak sağlayıcısı bu listeye dahil edilmez.

Kullanıcıların sahip hakları olan kaynak gruplarında 'Microsoft.DataBoxEdge' gibi kaynaklar oluşturabilmeleri için, bu kaynakların kaynak sağlayıcıları zaten olduğu sürece abonelik düzeyine erişim izni vermeniz gerekmez Kayıtlı.

Herhangi bir kaynak oluşturmaya çalışmadan önce, kaynak sağlayıcısının aboneye kayıtlı olduğundan emin olun. Kaynak sağlayıcısı kayıtlı değilse, yeni kaynağı oluşturan kullanıcının gerekli kaynak sağlayıcısını abonelik düzeyinde kaydetmek için yeterli haklara sahip olduğundan emin olmanız gerekir. Bunu da yapmadıysanız, aşağıdaki hatayı görürsünüz:

*Abonelik \<Abonelik adı> kaynak sağlayıcısı(lar): Microsoft.DataBoxEdge'i kaydetme izinleri yoktur.*


Geçerli abonelikte kayıtlı kaynak sağlayıcılarının listesini almak için aşağıdaki komutu çalıştırın:

```PowerShell
Get-AzResourceProvider -ListAvailable |where {$_.Registrationstate -eq "Registered"}
```

Veri Kutusu Kenarı `Microsoft.DataBoxEdge` aygıtı için kaydedilmelidir. Kayıt `Microsoft.DataBoxEdge`için, abonelik yöneticisi aşağıdaki komutu çalıştırmalıdır:

```PowerShell
Register-AzResourceProvider -ProviderNamespace Microsoft.DataBoxEdge
```

Kaynak sağlayıcısının nasıl kaydedilen hakkında daha fazla bilgi için [kaynak sağlayıcı kaydı hataları giderme'ye](https://docs.microsoft.com/azure/azure-resource-manager/resource-manager-register-provider-errors)bakın.

## <a name="manage-connectivity-mode"></a>Bağlantı modunu yönetme

Varsayılan tam bağlı modun dışında, aygıtınız kısmen bağlı veya tamamen bağlantısı kesilmiş modda da çalıştırılabilir. Bu modların her biri aşağıdaki gibi açıklanmıştır:

- **Tamamen bağlı** - Bu, aygıtın çalıştığı normal varsayılan moddur. Bu modda hem bulut yüklemesi hem de veri indirme özelliği etkinleştirilir. Aygıtı yönetmek için Azure portalını veya yerel web ui'sini kullanabilirsiniz.

- **Kısmen bağlantısı kesildi** – Bu modda, aygıt herhangi bir paylaşım verisi yükleyemez veya indiremez ancak Azure portalı üzerinden yönetilebilir.

    Bu mod genellikle tarifeli uydu ağında kullanılır ve amaç ağ bant genişliği tüketimini en aza indirmektir. Cihaz izleme işlemleri için minimum ağ tüketimi oluşabilir.

- **Bağlantısı kesildi** – Bu modda, aygıt bulutla tamamen bağlantısı kesilir ve hem bulut yüklemeleri hem de karşıdan yüklemeleri devre dışı bırakılır. Cihaz yalnızca yerel web ui üzerinden yönetilebilir.

    Bu mod genellikle aygıtınızı çevrimdışına almak istediğinizde kullanılır.

Aygıt modunu değiştirmek için aşağıdaki adımları izleyin:

1. Cihazınızın yerel web ui'sinde Configuration **> Cloud ayarlarına**gidin.
2. Açılan listeden, aygıtı çalıştırmak istediğiniz modu seçin. **Tam bağlı,** kısmen **bağlı**ve **tamamen bağlantısı kesilmiş**birini seçebilirsiniz. Aygıtı kısmen bağlantısı kesilmiş modda çalıştırmak için **Azure portal yönetimini**etkinleştirin.

    ![Bağlantı modu](media/data-box-edge-manage-access-power-connectivity-mode/connectivity-mode.png)
 
## <a name="manage-power"></a>Gücü yönetin

You can shut down or restart your physical device using the local web UI. Yeniden başlatmadan önce, veri sunucusundaki ve ardından aygıttaki paylaşımları çevrimdışına almanızı öneririz. Bu eylem, veri bozulması olasılığını en aza indirir.

1. Yerel web ui'sinde, **Bakım > Güç ayarlarına**gidin.
2. Ne yapmak istediğinize bağlı olarak **Kapatma** veya **Yeniden Başlat'ı** seçin.

    ![Güç ayarları](media/data-box-edge-manage-access-power-connectivity-mode/shut-down-restart-1.png)

3. Onay istendiğinde, devam etmek için **Evet'i** seçin.

> [!NOTE]
> Fiziksel aygıtı kapatırsanız, açmak için aygıtın güç düğmesine basmanız gerekir.

## <a name="next-steps"></a>Sonraki adımlar

- [Paylaşımları](data-box-edge-manage-shares.md)nasıl yönetinen öğrenin.
