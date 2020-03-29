---
title: Azure AD Connect ve kullanıcı gizliliği | Microsoft Dokümanlar
description: Bu belge, Azure AD Connect ile GDPR uyumluluğu nasil alınacakını açıklayıyor.
services: active-directory
documentationcenter: ''
author: billmath
manager: daveba
editor: ''
ms.service: active-directory
ms.workload: identity
ms.tgt_pltfrm: na
ms.devlang: na
ms.topic: reference
ms.date: 05/21/2018
ms.subservice: hybrid
ms.author: billmath
ms.collection: M365-identity-device-management
ms.openlocfilehash: 6f5d3125b7b77e8ce7a943f640c44615049ab160
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "60455797"
---
# <a name="user-privacy-and-azure-ad-connect"></a>Kullanıcı gizliliği ve Azure AD Bağlantısı 

[!INCLUDE [Privacy](../../../includes/gdpr-intro-sentence.md)]

>[!NOTE] 
>Bu makalede Azure AD Connect ve kullanıcı gizliliği ele adabı yer adabı.  Azure AD Connect Health ve kullanıcı gizliliği hakkında daha fazla bilgi için [makaleye buradan](reference-connect-health-user-privacy.md)bakın.

Azure AD Connect yüklemeleri için kullanıcı gizliliğini iki şekilde geliştirin:

1.  İstek üzerine, bir kişi için veri ayıklayın ve yüklemelerden bu kişiden veri kaldırın
2.  48 saatten fazla veri tutulmadığından emin olun.

Azure AD Connect ekibi, uygulanması ve bakımı çok daha kolay olduğundan ikinci seçeneği önerir.

Azure AD Connect eşitleme sunucusu aşağıdaki kullanıcı gizlilik verilerini depolar:
1.  Azure AD Connect **veritabanındaki** bir kişi hakkındaki veriler
2.  **Windows Event günlük** dosyalarında bir kişi hakkında bilgi içerebilecek veriler
3.  **Azure AD Connect yükleme günlüğü dosyalarında** bir kişi hakkında bilgi içerebilecek veriler

Azure AD Connect müşterileri kullanıcı verilerini kaldırırken aşağıdaki yönergeleri kullanmalıdır:
1.  Azure AD Connect yükleme günlüğü dosyalarını içeren klasörün içeriğini düzenli olarak silin – en az her 48 saatte bir
2.  Bu ürün ayrıca Olay Günlükleri oluşturabilir.  Olay Günlükleri günlükleri hakkında daha fazla bilgi edinmek için lütfen [buradaki belgelere](https://msdn.microsoft.com/library/windows/desktop/aa385780.aspx)bakın.

Bir kişi hakkındaki veriler, bu kişinin verileri geldiği kaynak sistemden kaldırıldığında Azure AD Connect veritabanından otomatik olarak kaldırılır. YÖNETICILERIN GDPR uyumlu olması için belirli bir işlem yapılması gerekmez.  Ancak, Azure AD Connect verilerinin en az iki günde bir veri kaynağınızla eşitlemesi gerekir.

## <a name="delete-the-azure-ad-connect-installation-log-file-folder-contents"></a>Azure AD Connect yükleme günlüğü dosya klasörü içeriğini silme
**PersistedState.Xml** dosyası dışında **c:\programdata\aadconnect** klasörünün içeriğini düzenli olarak kontrol edin ve silin. Bu dosya, Azure A Connect'in önceki yüklemedurumunu korur ve yükseltme yüklemesi yapıldığında kullanılır. Bu dosya bir kişi hakkında herhangi bir veri içermez ve silinmemelidir.

>[!IMPORTANT]
>PersistedState.xml dosyasını silmeyin.  Bu dosya hiçbir kullanıcı bilgisi içermez ve önceki yüklemenin durumunu korur.

Windows Gezgini'ni kullanarak bu dosyaları gözden geçirip silebilirsiniz veya gerekli eylemleri gerçekleştirmek için aşağıdaki gibi bir komut dosyası kullanabilirsiniz:


```
$Files = ((Get-childitem -Path "$env:programdata\aadconnect" -Recurse).VersionInfo).FileName
Foreach ($file in $files) {
If ($File.ToUpper() -ne "$env:programdata\aadconnect\PERSISTEDSTATE.XML".toupper()) # Do not delete this file
    {Remove-Item -Path $File -Force}
    } 
```

### <a name="schedule-this-script-to-run-every-48-hours"></a>Bu komut dosyasını her 48 saatte bir çalışacak şekilde zamanlama
Komut dosyasını her 48 saatte bir çalışacak şekilde zamanlamak için aşağıdaki adımları kullanın.

1.  Komut dosyasını **PS1'&#46;** uzantılı bir dosyaya kaydedin, ardından Denetim Masası'nı açın ve **Sistemler ve Güvenlik'e**tıklayın.
    ![Sistem](./media/reference-connect-user-privacy/gdpr2.png)

2.  Yönetim Araçları başlığı **altında, Zamanlama Görevleri'ni**tıklatın.
    ![Görev](./media/reference-connect-user-privacy/gdpr3.png)
3.  Görev Zamanlayıcısı'nda, **Görev Çizelgesi Kitaplığı'na** sağ tıklayın ve **Temel Görev Oluştur'a tıklayın...**
4.  Yeni görevin adını girin ve **İleri'yi**tıklatın.
5.  Görev tetikleyicisi için **Günlük'u** seçin ve **İleri'ye**tıklayın.
6.  Yinelemeyi **2 güne** ayarlayın ve **İleri'yi**tıklatın.
7.  Eylem olarak **program başlat'ı** seçin ve **İleri'ye**tıklayın.
8.  Program/komut dosyası için kutuya **PowerShell** yazın ve etiketlenmiş kutuda **Bağımsız Değişken ekle (isteğe bağlı)** olarak, daha önce oluşturduğunuz komut dosyasına tam yolu girin, ardından **İleri'yi**tıklatın.
9.  Sonraki ekranda oluşturmak üzere olduğunuz görevin bir özetini gösterir. Değerleri doğrulayın ve görevi oluşturmak için **Bitir'i** tıklatın.



## <a name="next-steps"></a>Sonraki adımlar
* [Güven Merkezi'ndeki Microsoft Gizlilik ilkesini inceleyin](https://www.microsoft.com/trustcenter)
* [Azure AD, Sağlık Ve Kullanıcı Gizliliğini Bağlayın](reference-connect-health-user-privacy.md)
