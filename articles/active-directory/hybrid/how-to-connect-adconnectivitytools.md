---
title: 'Azure AD Connect: ADConnectivityTool PowerShell Modülü nedir | Microsoft Dokümanlar'
description: Bu belge, yeni ADConnectivity PowerShell modülünü ve sorun gidermeye yardımcı olmak için nasıl kullanılabileceğini tanıtır.
services: active-directory
author: billmath
manager: daveba
ms.service: active-directory
ms.workload: identity
ms.topic: conceptual
ms.date: 4/25/2019
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: cd5340cd8c802df4ffbe0207b5401d2fee4e207e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "64571119"
---
# <a name="troubleshoot-azure-ad-connectivity-with-the-adconnectivitytool-powershell-module"></a>ADConnectivityTool PowerShell modülü ile Azure AD bağlantısını giderme

ADConnectivity aracı aşağıdakilerden birinde kullanılan bir PowerShell modülüdür:

- Yükleme sırasında bir ağ bağlantısı sorunu, Kullanıcının Sihirbazı'nda sağladığı Active Directory kimlik bilgilerinin başarılı bir şekilde doğrulanmasını engellediğinde.
- PowerShell oturumundan işlevleri çağıran bir kullanıcı tarafından yükleme sonrası.

Araç şu adreste bulunur: **C:\Program Files\Microsoft Azure Active Directory Connect\Tools\ ADConnectivityTool.psm1** 

## <a name="adconnectivitytool-during-installation"></a>Kurulum sırasında ADConnectivityTool

**Dizinlerinizi Bağla** sayfasında, Azure AD Bağlantı Sihirbazı'nda, bir ağ sorunu oluşursa, ADConnectivityTool neler olup bittiğini belirlemek için işlevlerinden birini otomatik olarak kullanır.  Aşağıdakilerden herhangi biri ağ sorunları olarak kabul edilebilir:

- Kullanıcının sağladığı Orman'ın adı yanlış yazıldı veya Orman'ın var olmadığını söyledi 
- UDP bağlantı noktası 389, kullanıcının sağladığı Ormanla ilişkili Etki Alanı Denetleyicileri'nde kapatılır
- 'AD orman hesabı' penceresinde sağlanan kimlik bilgileri, hedef Ormanile ilişkili Etki Alanı Denetleyicilerini almak için ayrıcalıklara sahip değildir
- 53, 88 veya 389 TCP bağlantı noktalarından herhangi biri, kullanıcının sağladığı Ormanla ilişkili Etki Alanı Denetleyicileri'nde kapatılır 
- Hem UDP 389 hem de TCP bağlantı noktası (veya bağlantı noktaları) kapalı
- Sağlanan Orman ve\veya ilişkili Etki Alanı Denetleyicileri için DNS çözülemedi

Bu sorunlardan herhangi biri bulunduğunda, AADConnect Sihirbazı'nda ilgili bir hata iletisi görüntülenir:


![Hata](media/how-to-connect-adconnectivitytools/error1.png)

Örneğin, **dizinlere Bağlan** ekranına bir dizin eklemeye çalışırken, Azure AD Connect'in bunu doğrulaması gerekir ve bağlantı noktası 389 üzerinden bir etki alanı denetleyicisiyle iletişim kurabilmeyi bekler.  Eğer değilse, yukarıdaki ekran görüntüsünde gösterilen hatayı görürüz.  

Aslında sahne arkasında olan şey, Azure AD Connect `Start-NetworkConnectivityDiagnosisTools` işlevi çağırıyor olmasıdır.  Bu işlev, bir ağ bağlantısı sorunu nedeniyle kimlik bilgilerinin doğrulaması başarısız olduğunda çağrılır.

Son olarak, araç sihirbazdan çağrıldığında ayrıntılı bir günlük dosyası oluşturulur. Günlük **C:\ProgramData\AADConnect\ADConnectivityTool-\<tarih>-\<zaman>.log** bulunur

## <a name="adconnectivitytools-post-installation"></a>ADConnectivityTools kurulum sonrası
Azure AD Connect yüklendikten sonra, ADConnectivityTools PowerShell modülündeki işlevlerden herhangi biri kullanılabilir.  

[ADConnectivityTools Başvuru](reference-connect-adconnectivitytools.md) işlevleri hakkında referans bilgileri bulabilirsiniz

### <a name="start-connectivityvalidation"></a>Başlangıç-Bağlantı Doğrulama

Bu işlevi yalnızca ADConnectivityTool.psm1 PowerShell'e alındıktan sonra manuel olarak **çağrılabildiği** için çağıracağız. 

Bu işlev, sağlanan AD Kimlik Bilgilerini doğrulamak için Azure AD Bağlantı Sihirbazı'nın çalıştırdığı mantığı yürütür.  Ancak sorun ve önerilen bir çözüm hakkında çok daha ayrıntılı bir açıklama sağlar. 

Bağlantı doğrulaması aşağıdaki adımlardan oluşur:
-   Etki Alanı FQDN (tam nitelikli etki alanı adı) nesnesi alın
-   Kullanıcı 'Yeni AD hesabı oluşturma' seçilirse, bu kimlik bilgileri Kurumsal Yöneticiler grubuna ait
-   Orman FQDN nesnesi alın
-   Daha önce elde edilen Orman FQDN nesnesiyle ilişkili en az bir etki alanına erişilebildiğini doğrulayın
-   Ormanın işlevsel düzeyinin Windows Server 2003 veya daha büyük olduğunu doğrulayın.

Tüm bu eylemler başarıyla yürütülürse, kullanıcı bir Dizin ekleyebilir.

Kullanıcı bu işlevi bir sorun çözüldükten sonra çalıştırıyorsa (veya hiç sorun yoksa) çıktı, kullanıcının Azure AD Bağlantı Sihirbazı'na geri dönüp kimlik bilgilerini yeniden eklemeyi denemesini gösterir.



## <a name="next-steps"></a>Sonraki Adımlar
- [Azure AD Connect: Hesaplar ve izinler](reference-connect-accounts-permissions.md)
- [Ekspres Kurulum](how-to-connect-install-express.md)
- [Özel Kurulum](how-to-connect-install-custom.md)
- [ADConnectivityTools Başvurusu](reference-connect-adconnectivitytools.md)

