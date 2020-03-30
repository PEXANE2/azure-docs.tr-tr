---
title: Kişisel verileri kaldırma - Azure Active Directory Application Proxy
description: Azure Active Directory Application Proxy için aygıtlara yüklenen bağlayıcılardan kişisel verileri kaldırın.
documentationcenter: ''
author: msmimart
manager: CelesteDG
ms.service: active-directory
ms.subservice: app-mgmt
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: conceptual
ms.date: 05/21/2018
ms.author: mimart
ms.reviewer: harshja
ms.custom: it-pro
ms.collection: M365-identity-device-management
ms.openlocfilehash: fe97956d99bd5c677e499b532ef85a1bb4d324ef
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74275423"
---
# <a name="remove-personal-data-for-azure-active-directory-application-proxy"></a>Azure Active Directory Application Proxy için kişisel verileri kaldırma

Azure Active Directory Application Proxy, aygıtlarınıza bağlayıcıyüklemenizi gerektirir, bu da aygıtlarınızda kişisel verilerin olabileceği anlamına gelir. Bu makalede, gizliliği artırmak için bu kişisel verilerin nasıl silineceğiniz için adımlar açıklanmıştır.

## <a name="where-is-the-personal-data"></a>Kişisel veriler nerede?

Application Proxy'nin kişisel verileri aşağıdaki günlük türlerine yazması mümkündür:

- Bağlayıcı olay günlükleri
- Windows olay günlükleri

## <a name="remove-personal-data-from-windows-event-logs"></a>Kişisel verileri Windows olay günlüklerinden kaldırma

Windows olay günlükleri için veri saklamayı yapılandırma hakkında bilgi için [olay günlükleri için Ayarlar'a](https://technet.microsoft.com/library/cc952132.aspx)bakın. Windows olay günlükleri hakkında bilgi edinmek için Bkz. [Windows Olay Günlüğü'ni Kullanma.](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx)

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="remove-personal-data-from-connector-event-logs"></a>Bağlayıcı olay günlüklerinden kişisel verileri kaldırma

Uygulama Proxy günlüklerinin kişisel veriye sahip olmadığından emin olmak için şunları yapabilirsiniz:

- Gerektiğinde verileri silme veya görüntüleme
- Günlüğe kaydetmeyi kapatma

Bağlayıcı olay günlüklerinden kişisel verileri kaldırmak için aşağıdaki bölümleri kullanın. Konektörün yüklü olduğu tüm aygıtlar için kaldırma işlemini tamamlamanız gerekir.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-or-export-specific-data"></a>Belirli verileri görüntüleme veya dışa aktarma

Belirli verileri görüntülemek veya dışa aktarmak için bağlayıcı olay günlüklerinin her birinde ilgili girişleri arayın. Günlükler `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`.

Günlükler metin dosyaları olduğundan, kullanıcıyla ilişkili metin girişlerini aramak için [findstr'ı](https://docs.microsoft.com/windows-server/administration/windows-commands/findstr) kullanabilirsiniz.  

Kişisel verileri bulmak için UserID için günlük dosyalarını arayın.

Kerberos Kısıtlı Temsilciliği kullanan bir uygulama tarafından günlüğe kaydedilen kişisel verileri bulmak için, kullanıcı adı türünün bu bileşenlerini arayın:

- Şirket içi kullanıcı ana adı
- Kullanıcı anaadının kullanıcı adı bölümü
- Şirket içi kullanıcı ana adının kullanıcı adı bölümü
- Şirket içi güvenlik hesapları yöneticisi (SAM) hesap adı

### <a name="delete-specific-data"></a>Belirli verileri silme

Belirli verileri silmek için:

1. Yeni bir günlük dosyası oluşturmak için Microsoft Azure AD Application Proxy Bağlayıcısı hizmetini yeniden başlatın. Yeni günlük dosyası, eski günlük dosyalarını silmenizi veya değiştirmenizi sağlar. 
1. Silinmesi gereken bilgileri bulmak için daha önce açıklanan [Görünüm veya dışa](#view-or-export-specific-data) aktarma özel veri işlemini izleyin. Tüm bağlayıcı günlüklerini arayın.
1. İlgili günlük dosyalarını silin veya kişisel veri içeren alanları seçiseçimle silin. Artık ihtiyacınız yoksa tüm eski günlük dosyalarını da silebilirsiniz.

### <a name="turn-off-connector-logs"></a>Bağlayıcı günlüklerini kapatma

Bağlayıcı günlüklerinin kişisel veri içermediğinden emin olmak için bir seçenek, günlük oluşturmayı kapatmaktır. Bağlayıcı günlükleri oluşturmayı durdurmak için aşağıdaki vurgulanan `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config`satırı kaldırın.

![Kaldırılacak vurgulanan kodu içeren bir kod parçacığı gösterir](./media/application-proxy-remove-personal-data/01.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulama Proxy'sine genel bir bakış için, [şirket içi uygulamalara nasıl güvenli uzaktan erişim sağlayabileceğiniz](application-proxy.md)bölümüne bakın.