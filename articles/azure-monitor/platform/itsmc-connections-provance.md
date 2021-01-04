---
title: BT Hizmet Yönetimi Bağlayıcısı ile Provance 'e bağlanma
description: Bu makalede, ıTSM iş öğelerini merkezi olarak izlemek ve yönetmek için Azure Izleyici 'de BT Hizmet Yönetimi Bağlayıcısı (ITSMC) ile nasıl temin edilecek hakkında bilgi sağlanır.
ms.subservice: logs
ms.topic: conceptual
author: nolavime
ms.author: v-jysur
ms.date: 12/21/2020
ms.openlocfilehash: 19d2ef1d4d711738d60938aac502dfd490e021a1
ms.sourcegitcommit: a4533b9d3d4cd6bb6faf92dd91c2c3e1f98ab86a
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/22/2020
ms.locfileid: "97729734"
---
# <a name="connect-provance-with-it-service-management-connector"></a>BT Hizmet Yönetimi Bağlayıcısı ile Provance 'e bağlanma

Bu makalede, iş öğelerinizi merkezi olarak yönetmek için Log Analytics içindeki Provance örneğiniz ile BT Hizmet Yönetimi Bağlayıcısı (ITSMC) arasındaki bağlantının nasıl yapılandırılacağı hakkında bilgi sağlanır.

> [!NOTE]
> Çeriyi ve Provance müşterilerinin, tümleştirmede başka bir çözüm olarak, Cherwell ve Provance uç noktasına [Web kancası eylemi](./action-groups.md#webhook) kullanmasını sağlıyoruz.

Aşağıdaki bölümlerde, Provance ürününüzü Azure 'daki ıSMC 'a bağlama hakkında ayrıntılı bilgi sağlanmaktadır.

## <a name="prerequisites"></a>Önkoşullar

Aşağıdaki önkoşulların karşılandığından emin olun:

- ISMC yüklendi. Daha fazla bilgi: [BT hizmet yönetimi Bağlayıcısı çözümü ekleme](./itsmc-definition.md#add-it-service-management-connector).
- Provance uygulamasının Azure AD 'ye kayıtlı olması gerekir ve istemci KIMLIĞI kullanılabilir hale getirilir. Ayrıntılı bilgi için bkz. [Active Directory kimlik doğrulamasını yapılandırma](../../app-service/configure-authentication-provider-aad.md).

- Kullanıcı rolü: yönetici.

## <a name="connection-procedure"></a>Bağlantı yordamı

Bir Provance bağlantısı oluşturmak için aşağıdaki yordamı kullanın:

1. Azure portal ' de **tüm kaynaklar** ' a gidin ve **ServiceDesk (yourçalışmaalanıadı)** araması yapın

2. **Çalışma alanı VERI kaynakları** altında **ITSM bağlantıları**' na tıklayın.
    ![Yeni bağlantı](media/itsmc-connections/add-new-itsm-connection.png)

3. Sağ bölmenin üst kısmındaki **Ekle**' ye tıklayın.

4. Aşağıdaki tabloda açıklandığı gibi bilgileri sağlayın ve bağlantıyı oluşturmak için **Tamam** ' ı tıklatın.

> [!NOTE]
> Tüm bu parametreler zorunludur.

| **Alan** | **Açıklama** |
| --- | --- |
| **Bağlantı adı**   | ISMC ile bağlanmak istediğiniz Provance örneği için bir ad yazın.  Bu adı daha sonra bu ıTSM 'de iş öğelerini yapılandırdığınızda ve ayrıntılı Log Analytics 'i görüntülemek için kullanabilirsiniz. |
| **İş ortağı türü**   | **Provance**' ı seçin. |
| **Kullanıcı adı**   | ISMC 'a bağlanabilecek kullanıcı adını yazın.    |
| **Parola**   | Bu kullanıcı adıyla ilişkili parolayı yazın. **Note:** Kullanıcı adı ve parola yalnızca kimlik doğrulama belirteçleri oluşturmak için kullanılır ve ıSMC hizmetinin içinde herhangi bir yerde depolanmaz.|
| **Sunucu URL 'SI**   | ISMC 'a bağlamak istediğiniz Provance örneğinizin URL 'sini yazın. |
| **İstemci Kimliği**   | Bu bağlantının kimliğini doğrulamak için gereken istemci KIMLIĞINI yazın ve bu, Provance Örneğinizde oluşturduğunuz.  İstemci KIMLIĞI hakkında daha fazla bilgi için bkz. [Active Directory kimlik doğrulamasını yapılandırma](../../app-service/configure-authentication-provider-aad.md). |
| **Veri eşitleme kapsamı**   | ITSMC aracılığıyla Azure Log Analytics eşitlemek istediğiniz Provance iş öğelerini seçin.  Bu iş öğeleri Log Analytics 'e aktarılır.   **Seçenekler:**   Olaylar, değişiklik Istekleri.|
| **Verileri eşitleme** | Verilerin içinden istediğiniz geçmiş gün sayısını yazın. **Maksimum sınır**: 120 gün. |
| **ITSM çözümünde yeni yapılandırma öğesi oluştur** | ITSM ürününde yapılandırma öğelerini oluşturmak istiyorsanız bu seçeneği belirleyin. Seçildiğinde, ıSMC, desteklenen ıTSM sisteminde, etkilenen CIS 'yi yapılandırma öğeleri (mevcut olmayan CIS olması durumunda) olarak oluşturur. **Varsayılan**: devre dışı.|

![Bağlantı adı ve Iş ortağı türü listelerini vurgulayan ekran görüntüsü.](media/itsmc-connections/itsm-connections-provance-latest.png)

**Başarıyla bağlandığında ve eşitlendiğinde**:

- Bu Provance örneğinden seçilen iş öğeleri Azure Log Analytics içeri aktarılmalıdır **.** BT Hizmet Yönetimi Bağlayıcısı kutucuğunda bu iş öğelerinin özetini görüntüleyebilirsiniz.

- Log Analytics uyarılarından veya günlük kayıtlarından ya da bu Provance örneğindeki Azure uyarılarından olaylar oluşturabilirsiniz.

## <a name="next-steps"></a>Sonraki adımlar

* [ITSM Bağlayıcısı genel bakış](itsmc-overview.md)
* [Azure uyarılarından ıTSM iş öğeleri oluşturma](./itsmc-definition.md#create-itsm-work-items-from-azure-alerts)
* [ITSM Bağlayıcısındaki sorunları giderme](./itsmc-resync-servicenow.md)