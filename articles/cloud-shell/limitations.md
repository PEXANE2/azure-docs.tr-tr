---
title: Azure Cloud Shell sınırlamaları | Microsoft Docs
description: Azure Cloud Shell kısıtlamalarına genel bakış
services: azure
documentationcenter: ''
author: maertendMSFT
manager: timlt
tags: azure-resource-manager
ms.assetid: ''
ms.service: azure
ms.workload: infrastructure-services
ms.tgt_pltfrm: vm-linux
ms.devlang: na
ms.topic: article
ms.date: 02/15/2018
ms.author: damaerte
ms.openlocfilehash: 092dccab82326bb9983f11ff64fe50aee7b1084d
ms.sourcegitcommit: 5b9287976617f51d7ff9f8693c30f468b47c2141
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 12/09/2019
ms.locfileid: "74951488"
---
# <a name="limitations-of-azure-cloud-shell"></a>Azure Cloud Shell sınırlamaları

Azure Cloud Shell aşağıdaki bilinen sınırlamalara sahiptir:

## <a name="general-limitations"></a>Genel sınırlamalar

### <a name="system-state-and-persistence"></a>Sistem durumu ve kalıcılık

Cloud Shell oturumunuzu sağlayan makine geçicidir ve oturumunuz 20 dakika etkin kaldıktan sonra geri dönüştürülür. Cloud Shell, bir Azure dosya paylaşımının bağlanmasını gerektirir. Sonuç olarak, aboneliğiniz Cloud Shell erişmek için depolama kaynaklarını ayarlayabilmelidir. Diğer konular şunlardır:

* Bağlı depolama ile yalnızca `$Home` dizinindeki değişiklikler kalıcı hale getirilir.
* Azure dosya paylaşımları yalnızca [atanan bölgeniz](persisting-shell-storage.md#mount-a-new-clouddrive)içinden bağlanabilir.
  * Bash 'de, bölge kümesini `ACC_LOCATION`olarak bulmak için `env` çalıştırın.

### <a name="browser-support"></a>Tarayıcı desteği

Cloud Shell, Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox ve Apple Safari 'nin en son sürümlerini destekler. Özel modda Safari desteklenmez.

### <a name="copy-and-paste"></a>Kopyala ve yapıştır

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar her seferinde bir tür kabuğu başlatabilir, **Bash** veya **PowerShell**. Ancak, aynı anda çalışan birkaç bash veya PowerShell örneğine sahip olabilirsiniz. Menüyü kullanarak bash veya PowerShell arasında değiştirme, var olan oturumları sonlandıran Cloud Shell yeniden başlatılmasına neden olur. Alternatif olarak, `bash`yazarak ve PowerShell içinde PowerShell 'i çalıştırarak `pwsh`yazarak PowerShell 'i çalıştırabilirsiniz.

### <a name="usage-limits"></a>Kullanım sınırları

Cloud Shell, etkileşimli kullanım örneklerine yöneliktir. Sonuç olarak, uzun süre çalışan etkileşimli olmayan oturumlar uyarı vermeden sonlandırılır.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinler, sudo erişimi olmayan normal kullanıcılar olarak ayarlanır. `$Home` dizininizin dışındaki tüm yüklemeler kalıcı değildir.

### <a name="editing-bashrc-or-profile"></a>. Bashrc veya $PROFILE düzenleniyor

. Bashrc veya PowerShell $PROFILE dosyasını düzenlediğinizde dikkatli olun, bunun yapılması Cloud Shell beklenmeyen hatalara neden olabilir.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="azuread-module-name"></a>`AzureAD` modül adı

`AzureAD` modül adı şu anda `AzureAD.Standard.Preview`, modül aynı işlevselliği sağlar.

### <a name="sqlserver-module-functionality"></a>`SqlServer` modülü işlevselliği

Cloud Shell eklenen `SqlServer` modülü yalnızca PowerShell Core için ön sürüm desteğine sahiptir. Özellikle `Invoke-SqlCmd` henüz kullanılamaz.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturulan varsayılan dosya konumu:

Kullanıcılar, PowerShell cmdlet 'lerini kullanarak Azure: Drive altında dosya oluşturamaz. Kullanıcılar, VIM veya nano gibi diğer araçları kullanarak yeni dosyalar oluştur, dosyalar varsayılan olarak `$HOME` kaydedilir. 

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmez

Kullanıcı bir Windows iletişim kutusu oluşturacak bir komut çalıştırıyorsa, birisi şöyle bir hata iletisi görür: `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`.

### <a name="large-gap-after-displaying-progress-bar"></a>İlerleme çubuğunu görüntülemeden sonra büyük boşluk

Kullanıcı, `Azure:` sürücüde olduğu gibi bir ilerleme çubuğu görüntüleyen bir eylem gerçekleştiriyorsa, imleç düzgün şekilde ayarlanmamış ve ilerleme çubuğunun daha önce olduğu bir boşluk görünüyor olabilir.

## <a name="next-steps"></a>Sonraki adımlar

[Sorun giderme Cloud Shell](troubleshooting.md) <br>
[Bash için hızlı başlangıç](quickstart.md) <br>
[PowerShell için hızlı başlangıç](quickstart-powershell.md)
