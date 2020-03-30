---
title: Windows Sanal Masaüstü yönetim aracı - Azure
description: Windows Sanal Masaüstü yönetim aracıyla ilgili sorunları giderme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: troubleshooting
ms.date: 11/09/2019
ms.author: helohr
manager: lizross
ms.openlocfilehash: 9d07ba42e83d9eec071ab047e9e1e92bac1f1411
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "79127493"
---
# <a name="troubleshoot-the-windows-virtual-desktop-management-tool"></a>Windows Sanal Masaüstü yönetim aracı sorunlarını giderme

Bu makalede, Windows Sanal Masaüstü yönetim aracını dağıtırken oluşabilecek sorunlar ve bunların nasıl düzeltilen açıklanmaktadır.

## <a name="error-management-tool-services-configured-but-automated-setup-fails"></a>Hata: Yönetim aracı hizmetleri yapılandırıldı ancak otomatik kurulum başarısız oldu

Yönetim aracı için hizmetleri başarıyla ayarladığınızda ancak otomatik kurulum başarısız olduğunda, şu hata iletisini görürsünüz:

```console
{"code":"DeploymentFailed","message":"At least one resource deployment operation failed. Please list deployment operations for details. Please see https://aka.ms/arm-debug for usage details.","details":[{"code":"Conflict","message":"{\r\n "status": "Failed",\r\n "error": {\r\n "code": "ResourceDeploymentFailure",\r\n "message": "The resource operation completed with terminal provisioning state 'Failed'."\r\n }\r\n}"}]}
```

Bu genellikle aşağıdaki iki şeyden biri anlamına gelir:

- Kullanıcının abonelik sahibi izinleri ve kiracı düzeyinde ki genel yöneticisi vardır, ancak Azure'da oturum açamaz.
- Kullanıcının hesap ayarları çok faktörlü kimlik doğrulamaözelliğine sahiptir.

Bunu düzeltmek için:

1. Azure Active Directory Kullanıcı Adı için oluşturduğunuz kullanıcının "Katılımcı" abonelik düzeyine sahip olduğundan emin olun.
2. Hesap ayarlarını kontrol etmek ve çok faktörlü kimlik doğrulamanın açık olmadığından emin olmak için UPN hesabıyla portal.azure.com> <oturum açın. Eğer açıksa, kapat.
3. Windows Sanal Masaüstü Onayı sayfasını ziyaret edin ve sunucu ve istemci uygulamalarının onay aldığından emin olun.
4. Sorun devam ederse [yönetim aracı](manage-resources-using-ui.md) öğreticisini gözden geçirin ve aracı yeniden dağıtın.

## <a name="error-job-with-specified-id-already-exists"></a>Hata: Belirtilen kimliği olan iş zaten var

Kullanıcınız hata iletisi "Belirtilen kimliği olan iş zaten var" sözcüklerini görüyorsa, bunun nedeni şablonu dağıtırken "Uygulama adı" parametresinde benzersiz bir ad sağlamamış olmalarıdır.

Bunu düzeltmek için, "Uygulama adı" parametresi dolu yönetim aracını yeniden dağıtın.

## <a name="delayed-consent-prompt-when-opening-management-tool"></a>Yönetim aracını açarken gecikmeli onay istemi

Yönetim aracını dağıttığınızda, onay istemi hemen açılmayabilir. Bu, Azure Web uygulaması hizmetinin yüklenmesinin normalden daha uzun sürdüğü anlamına gelir. Komut istemi, Azure Web yüklemesi bittikten sonra görünmelidir.

## <a name="the-user-cant-deploy-the-management-tool-in-the-east-us-region"></a>Kullanıcı, yönetim aracını Doğu ABD bölgesinde dağıtamıyor

Bir müşteri bölgeyi Doğu ABD'ye ayarlarsa, yönetim aracını dağıtamaz.

Bunu düzeltmek için yönetim aracını farklı bir bölgeye dağıtın. Aracı farklı bir bölgede yeniden dağıtmak kullanıcı deneyimini etkilememelidir.

## <a name="next-steps"></a>Sonraki adımlar

- [Sorun Giderme genel bakışı, geri bildirim ve destekte](troubleshoot-set-up-overview.md)yükseltme parçaları hakkında bilgi edinin.
- [Uzak Masaüstü Hizmetleri için ARM Şablonları'nda](https://github.com/Azure/RDS-Templates/blob/master/README.md)Windows Sanal Masaüstü araçlarıyla ilgili sorunları nasıl bildireceklerini öğrenin.
- Windows Sanal Masaüstü sorun giderme ve yükseltme parçalarına genel bakış için [Sorun Giderme genel bakışı, geri bildirim ve desteğe](troubleshoot-set-up-overview.md)bakın.
- Yönetim aracını nasıl dağıtılayabilirsiniz öğrenmek için [bir yönetim aracı dağıt'a](manage-resources-using-ui.md)bakın.