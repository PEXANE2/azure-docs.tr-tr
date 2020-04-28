---
title: Kişisel verileri Kaldır-Azure Active Directory Uygulama Ara Sunucusu
description: Azure Active Directory Uygulama Ara Sunucusu için cihazlara yüklenmiş olan bağlayıcılardan kişisel verileri kaldırın.
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
ms.sourcegitcommit: b1e25a8a442656e98343463aca706f4fde629867
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/27/2020
ms.locfileid: "74275423"
---
# <a name="remove-personal-data-for-azure-active-directory-application-proxy"></a>Azure Active Directory Uygulama Ara Sunucusu kişisel verilerini kaldır

Azure Active Directory Uygulama Ara Sunucusu cihazlarınızda bağlayıcılar yüklemenizi gerektirir, bu da cihazlarınızda kişisel veriler olabileceğini gösterir. Bu makalede, gizliliği geliştirmek için kişisel verilerin nasıl silineceği hakkında adımlar sağlanmaktadır.

## <a name="where-is-the-personal-data"></a>Kişisel veriler nerede?

Uygulama proxy 'Sinin kişisel verileri aşağıdaki günlük türlerine yazması mümkündür:

- Bağlayıcı olay günlükleri
- Windows olay günlükleri

## <a name="remove-personal-data-from-windows-event-logs"></a>Windows olay günlüklerinden kişisel verileri kaldırma

Windows olay günlükleri için veri bekletmenin nasıl yapılandırılacağı hakkında bilgi için bkz. [olay günlüklerine yönelik ayarlar](https://technet.microsoft.com/library/cc952132.aspx). Windows olay günlükleri hakkında bilgi edinmek için bkz. [Windows olay günlüğü kullanma](https://msdn.microsoft.com/library/windows/desktop/aa385772.aspx).

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-hybrid-note.md)]

## <a name="remove-personal-data-from-connector-event-logs"></a>Kişisel verileri bağlayıcı olay günlüklerinden kaldır

Uygulama proxy 'Si günlüklerinin kişisel verileri olmadığından emin olmak için şunlardan birini yapabilirsiniz:

- Gerektiğinde verileri silme veya görüntüleme veya
- Günlüğe kaydetmeyi kapat

Bağlayıcı olay günlüklerinden kişisel verileri kaldırmak için aşağıdaki bölümleri kullanın. Bağlayıcının yüklü olduğu tüm cihazlar için kaldırma işlemini tamamlamalısınız.

[!INCLUDE [GDPR-related guidance](../../../includes/gdpr-intro-sentence.md)]

### <a name="view-or-export-specific-data"></a>Belirli verileri görüntüleme veya dışarı aktarma

Belirli verileri görüntülemek veya dışarı aktarmak için, her bağlayıcı olay günlüğündeki ilgili girişleri arayın. Günlükler konumunda `C:\ProgramData\Microsoft\Microsoft AAD Application Proxy Connector\Trace`bulunur.

Günlükler metin dosyaları olduğundan, bir kullanıcıyla ilgili metin girişlerini aramak için [Findstr](https://docs.microsoft.com/windows-server/administration/windows-commands/findstr) kullanabilirsiniz.  

Kişisel verileri bulmak için, Kullanıcı kimliği için günlük dosyalarını arayın.

Kerberos kısıtlanmış temsili kullanan bir uygulama tarafından günlüğe kaydedilen kişisel verileri bulmak için Kullanıcı adı türünün şu bileşenlerini arayın:

- Şirket içi Kullanıcı asıl adı
- Kullanıcı asıl adının Kullanıcı adı bölümü
- Şirket içi Kullanıcı asıl adının Kullanıcı adı bölümü
- Şirket içi güvenlik Hesapları Yöneticisi (SAM) hesap adı

### <a name="delete-specific-data"></a>Belirli verileri silme

Belirli verileri silmek için:

1. Yeni bir günlük dosyası oluşturmak için Microsoft Azure AD uygulama proxy Bağlayıcısı hizmetini yeniden başlatın. Yeni günlük dosyası eski günlük dosyalarını silmenizi veya değiştirmenizi sağlar. 
1. Silinmesi gereken bilgileri bulmak için daha önce açıklanan [belirli veri sürecini görüntüleyin veya dışarı aktarın](#view-or-export-specific-data) . Tüm bağlayıcı günlüklerinde arama yapın.
1. İlgili günlük dosyalarını silin ya da kişisel verileri içeren alanları seçmeli olarak silin. Artık gerekmiyorsa, eski günlük dosyalarını da silebilirsiniz.

### <a name="turn-off-connector-logs"></a>Bağlayıcı günlüklerini kapatma

Bağlayıcı günlüklerinin kişisel veriler içermediğinden emin olmak için bir seçenek, günlük oluşturmayı kapatmamalıdır. Bağlayıcı günlüklerini oluşturmayı durdurmak için aşağıdaki vurgulanmış satırı öğesinden `C:\Program Files\Microsoft AAD App Proxy Connector\ApplicationProxyConnectorService.exe.config`kaldırın.

![Kaldırmak için vurgulanan koda sahip bir kod parçacığı gösterir](./media/application-proxy-remove-personal-data/01.png)

## <a name="next-steps"></a>Sonraki adımlar

Uygulama proxy 'sine genel bakış için bkz. Şirket [içi uygulamalara güvenli uzaktan erişim sağlama](application-proxy.md).