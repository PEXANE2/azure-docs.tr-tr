---
title: AD DS depolama hesabı parolasını Güncelleştir
description: Depolama hesabınızı temsil eden Active Directory Domain Services hesabının parolasını güncelleştirme hakkında bilgi edinin. Bu, parolanın süresi dolduğu zaman kimlik doğrulama başarısızlıklarını önleyen depolama hesabının temizlenmesini önler.
author: roygara
ms.service: storage
ms.subservice: files
ms.topic: how-to
ms.date: 06/22/2020
ms.author: rogarana
ms.openlocfilehash: d16f11a85c6b370b0187975cce965bf3e1b5ba3f
ms.sourcegitcommit: 877491bd46921c11dd478bd25fc718ceee2dcc08
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/02/2020
ms.locfileid: "85510263"
---
# <a name="update-the-password-of-your-storage-account-identity-in-ad-ds"></a>AD DS 'de depolama hesabı Kimliğinizin parolasını güncelleştirin

Depolama hesabınızı temsil eden Active Directory Domain Services (AD DS) kimlik/hesap, parola sona erme süresini zorlayan bir kuruluş biriminde veya etki alanında kaydedilmişse, parolayı en fazla parola geçerlilik süresinden önce değiştirmelisiniz. Kuruluşunuz, parolasının süresi dolduktan sonra hesapları silen otomatik temizleme betikleri çalıştırabilir. Bu nedenle, parolanızı süresi dolmadan önce değiştirmemeniz durumunda hesabınız silinebilir, bu da Azure dosya paylaşımlarınız için erişiminizi kaybetmenize neden olur.

Parola döndürmeyi tetiklemek için, `Update-AzStorageAccountADObjectPassword` komutu [AzFilesHybrid modülünden](https://github.com/Azure-Samples/azure-files-samples/releases)çalıştırabilirsiniz. Bu komut, depolama hesabı için sahip iznine sahip bir karma Kullanıcı kullanılarak şirket içi AD DS katılmış bir ortamda çalıştırılmalıdır ve depolama hesabını temsil eden kimliğin parolasını değiştirmek için izinler AD DS. Komut, depolama hesabı anahtar dönüşüyle benzer eylemler gerçekleştirir. Özellikle, depolama hesabının ikinci Kerberos anahtarını alır ve AD DS kayıtlı hesabın parolasını güncelleştirmek için kullanır. Ardından, depolama hesabının hedef Kerberos anahtarını yeniden oluşturur ve AD DS kayıtlı hesabın parolasını güncelleştirir. Bu komutu, şirket içi AD DS katılmış bir ortamda çalıştırmanız gerekir.

```PowerShell
# Update the password of the AD DS account registered for the storage account
# You may use either kerb1 or kerb2
Update-AzStorageAccountADObjectPassword `
        -RotateToKerbKey kerb2 `
        -ResourceGroupName "<your-resource-group-name-here>" `
        -StorageAccountName "<your-storage-account-name-here>"
```
