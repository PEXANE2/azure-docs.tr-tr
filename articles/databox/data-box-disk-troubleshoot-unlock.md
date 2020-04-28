---
title: Azure Data Box Disk disk kilit açma sorunlarını giderme | Microsoft Docs
description: Azure Data Box Disk sorunlarının nasıl giderileceği anlatılmaktadır.
services: databox
author: alkohli
ms.service: databox
ms.subservice: disk
ms.topic: article
ms.date: 06/14/2019
ms.author: alkohli
ms.openlocfilehash: 02cbf64261bbfbf50561e1b7466b46b27b688e0a
ms.sourcegitcommit: 849bb1729b89d075eed579aa36395bf4d29f3bd9
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "67148289"
---
# <a name="troubleshoot-disk-unlocking-issues-in-azure-data-box-disk"></a>Azure Data Box Disk disk kilit açma sorunlarını giderme

Bu makale, Microsoft Azure Data Box Disk için geçerlidir ve kilit açma aracını kullanırken herhangi bir sorunu gidermek için kullanılan iş akışlarını açıklar. 


<!--## Query activity logs

Use the activity logs to find who unlocked and accessed the disks. Your Data Box Disk arrive on your premises in a locked state. You can use the device credentials available in the Azure portal for your order to unlock them.  

To figure out who accessed the **Device credentials** blade, you can query the Activity logs.  Any action that involves accessing **Device details > Credentials** blade is logged into the activity logs as `ListCredentials` action.

![Query Activity logs](media/data-box-logs/query-activity-log-1.png)-->


## <a name="data-box-disk-unlock-tool-errors"></a>Data Box Disk kilit açma aracı hataları


| Hata iletisi/Aracın davranışı      | Öneriler                                                                             |
|-------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------|
| Güncel .NET Framework desteklenmez. 4.5 ve sonraki sürümler desteklenir.<br><br>Araç kapanıyor ve bir ileti açılıyor.  | .NET 4.5 yüklenmedi. Data Box Disk kilit açma aracının çalıştığı ana bilgisayara .NET 4.5 veya üzerini yükleyin.                                                                            |
| Birimlerin kilidi açılamadı veya birimler doğrulanamadı. Microsoft Desteği'ne başvurun.  <br><br>Araç kilitli sürücülerin kilidini açamıyor veya bu sürücüleri doğrulayamıyor. | Araç verilen destek anahtarıyla kilitlenen sürücülerin kilidini açamıyor. Sonraki adımlar için Microsoft Desteği'ne başvurun.                                                |
| Aşağıdaki birimlerin kilidi açıldı ve bu birimler doğrulandı. <br>Birim sürücü harfleri: E:<br>Şu destek anahtarlarıyla birimlerin kilidi açılamadı: werwerqomnf, qwerwerqwdfda <br><br>Araç bazı sürücülerin kilidini açar, başarılı ve başarısız olan sürücü harflerini listeler.| Kısmen başarıldı. Kullanılan destek anahtarıyla bazı sürücülerin kilidi açılamadı. Sonraki adımlar için Microsoft Desteği'ne başvurun. |
| Kilitli birimler bulunamadı. Microsoft'tan alınan diskin düzgün bağlandığından ve kilitli durumda olduğundan emin olun.          | Araç kilitli sürücüleri bulamıyor. Sürücülerden birinin kilidi açılmış veya sürücü bulunamıyor. Sürücülerin bağlı ve kilitli olduğundan emin olun.                                                           |
| Önemli hata: Parametre geçersiz<br>Parametre adı: invalid_arg<br>KULLANIM:<br>DataBoxDiskUnlock /PassKeys:<noktalı_virgülle_ayrılmış_destek_anahtarı_listesi><br><br>Örnek: DataBoxDiskUnlock /PassKeys:passkey1;passkey2;passkey3<br>Örnek: DataBoxDiskUnlock /SystemCheck<br>Örnek: DataBoxDiskUnlock /Help<br><br>/PassKeys:       Bu destek anahtarını Azure DataBox Disk siparişinden alın. Bu destek anahtarı disklerinizin kilidini açar.<br>/Help:           Bu seçenek, cmdlet kullanımı konusunda yardım bilgileri ve örnekler sunar.<br>/SystemCheck:    Bu seçenek, sisteminizin aracı çalıştırmak için istenen gereksinimlere uygun olup olmadığını denetler.<br><br>Çıkmak için bir tuşa basın. | Geçersiz parametre girildi. İzin verilen tek parametreler/SystemCheck,/geçiş anahtarı ve/help.|


## <a name="unlock-issues-for-disks-when-using-a-windows-client"></a>Windows istemcisi kullanırken diskler için kilit açma sorunları

Bu bölümde, veri kopyalama için bir Windows istemcisi kullanılırken Data Box Disk dağıtımı sırasında ortaya çıkan başlıca sorunlardan bazıları ayrıntılı olarak ele verilebilir.

### <a name="issue-could-not-unlock-drive-from-bitlocker"></a>Sorun: Sürücünün kilidi BitLocker 'dan kaldırılamadı
 
**Sağlamak** 

BitLocker iletişim kutusunda parolayı kullandınız ve BitLocker 'ın kilit açma sürücüleri iletişim kutusu aracılığıyla diskin kilidini açmaya çalışıyorsunuz. Bu çalışmaz.

**Çözünürlük**

Data Box disklerinin kilidini açmak için Data Box Disk unlock aracını kullanmanız ve parolayı Azure portal sağlamanız gerekir. Daha fazla bilgi için bkz. [öğretici: Unpack, Connect ve unlock Azure Data Box disk](data-box-disk-deploy-set-up.md#connect-to-disks-and-get-the-passkey).
 
### <a name="issue-could-not-unlock-or-verify-some-volumes-contact-microsoft-support"></a>Sorun: bazı birimler kilidi açılamadı veya doğrulanamadı. Microsoft Desteği'ne başvurun.
 
**Sağlamak**

Hata günlüğünde aşağıdaki hatayı görebilir ve bazı birimleri kilitleyip doğrulayamayabilir.

`Exception System.IO.FileNotFoundException: Could not load file or assembly 'Microsoft.Management.Infrastructure, Version=1.0.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35' or one of its dependencies. The system cannot find the file specified.`
 
Bu, büyük olasılıkla Windows istemciniz üzerinde Windows PowerShell 'in uygun sürümü eksik olduğunu gösterir.

**Çözünürlük**

[Windows PowerShell v 5,0](https://www.microsoft.com/download/details.aspx?id=54616) 'yi yükleyip işlemi yeniden deneyin.
 
Birimlerin kilidini hala kilitlamadıysanız, günlükleri Data Box Disk kilit açma aracının bulunduğu klasörden kopyalayın ve [Microsoft desteği başvurun](data-box-disk-contact-microsoft-support.md).

## <a name="next-steps"></a>Sonraki adımlar

- [Doğrulama sorunlarını giderme](data-box-disk-troubleshoot.md)hakkında bilgi edinin.
