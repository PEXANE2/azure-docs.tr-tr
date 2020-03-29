---
title: Azure Bulut Kabuğu sınırlamaları | Microsoft Dokümanlar
description: Azure Bulut Uyp'ının sınırlamasınırlamaları için genel bakış
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
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74951488"
---
# <a name="limitations-of-azure-cloud-shell"></a>Azure Bulut Kabuğu sınırlamaları

Azure Bulut Kabuğu'nun bilinen sınırlamaları şunlardır:

## <a name="general-limitations"></a>Genel sınırlamalar

### <a name="system-state-and-persistence"></a>Sistem durumu ve kalıcılık

Bulut Kabuğu oturumunuzu sağlayan makine geçicidir ve oturumunuz 20 dakika etkin değildikten sonra geri dönüştürülür. Cloud Shell'in monte edilmesi için bir Azure dosya paylaşımı gerekir. Sonuç olarak, aboneliğinizin Cloud Shell'e erişmek için depolama kaynakları ayarlayabilmesi gerekir. Diğer hususlar şunlardır:

* Monte edilmiş depolama ile yalnızca `$Home` dizin içindeki değişiklikler devam eder.
* Azure dosya paylaşımları yalnızca [atanan bölgenizden](persisting-shell-storage.md#mount-a-new-clouddrive)monte edilebilir.
  * Bash'te, `env` bölgenizi ' `ACC_LOCATION`olarak ayarlanmış olarak bulmak için çalıştırın.

### <a name="browser-support"></a>Tarayıcı desteği

Cloud Shell, Microsoft Edge, Microsoft Internet Explorer, Google Chrome, Mozilla Firefox ve Apple Safari'nin en son sürümlerini destekler. Özel modda Safari desteklenmez.

### <a name="copy-and-paste"></a>Kopyala ve yapıştır

[!INCLUDE [copy-paste](../../includes/cloud-shell-copy-paste.md)]

### <a name="for-a-given-user-only-one-shell-can-be-active"></a>Belirli bir kullanıcı için yalnızca bir kabuk etkin olabilir

Kullanıcılar bir seferde yalnızca bir tür kabuk başlatabilir, **Bash** veya **PowerShell.** Ancak, aynı anda birden çok Bash veya PowerShell örneği çalışıyor olabilir. Menüyü kullanarak Bash veya PowerShell arasında takas yapmak Cloud Shell'in yeniden başlatılmasına neden olur ve bu da varolan oturumları sonlandırır. Alternatif olarak, yazarak `bash`PowerShell içinde bash çalıştırabilirsiniz , ve yazarak `pwsh`bash içinde PowerShell çalıştırabilirsiniz.

### <a name="usage-limits"></a>Kullanım sınırları

Cloud Shell, etkileşimli kullanım örnekleri için tasarlanmıştır. Sonuç olarak, uzun süren etkileşimli olmayan oturumlar uyarı yapılmadan sona erdirilir.

## <a name="bash-limitations"></a>Bash sınırlamaları

### <a name="user-permissions"></a>Kullanıcı izinleri

İzinler sudo erişimi olmayan normal kullanıcılar olarak ayarlanır. Dizininizin `$Home` dışındaki yüklemeler kalıcı değildir.

### <a name="editing-bashrc-or-profile"></a>Düzenleme .bashrc veya $PROFILE

.bashrc veya PowerShell'in $PROFILE dosyasını düzenlerken dikkatli olun, bunu yapmak Cloud Shell'de beklenmeyen hatalara neden olabilir.

## <a name="powershell-limitations"></a>PowerShell sınırlamaları

### <a name="azuread-module-name"></a>`AzureAD`modül adı

Modül `AzureAD` adı şu `AzureAD.Standard.Preview`anda, modül aynı işlevselliği sağlar.

### <a name="sqlserver-module-functionality"></a>`SqlServer`modül işlevselliği

Cloud `SqlServer` Shell'de yer alan modül, PowerShell Core için yalnızca ön sürüm desteğine sahiptir. Özellikle, `Invoke-SqlCmd` henüz mevcut değildir.

### <a name="default-file-location-when-created-from-azure-drive"></a>Azure sürücüsünden oluşturulduğunda varsayılan dosya konumu:

PowerShell cmdlets kullanarak, kullanıcılar Azure altında dosya oluşturamaz: sürücü. Kullanıcılar vim veya nano gibi diğer araçları kullanarak yeni dosyalar oluşturduklarında, dosyalar varsayılan olarak dosyalara `$HOME` kaydedilir. 

### <a name="gui-applications-are-not-supported"></a>GUI uygulamaları desteklenmiyor

Kullanıcı, Windows iletişim kutusu oluşturacak bir komut çalıştırıyorsa, aşağıdaki `Unable to load DLL 'IEFRAME.dll': The specified module could not be found. (Exception from HRESULT: 0x8007007E)`gibi bir hata iletisi görür.

### <a name="large-gap-after-displaying-progress-bar"></a>İlerleme çubuğugörüntülendikten sonra Büyük Boşluk

`Azure:` Kullanıcı, sürücüdeyken tamamlanan bir sekme gibi bir ilerleme çubuğu görüntüleyen bir eylem gerçekleştirirse, imleç düzgün ayarlanmaz ve ilerleme çubuğunun daha önce bulunduğu yerde bir boşluk belirir.

## <a name="next-steps"></a>Sonraki adımlar

[Sorun Giderme Bulut Kabuğu](troubleshooting.md) <br>
[Bash için hızlı başlangıç](quickstart.md) <br>
[PowerShell için hızlı başlangıç](quickstart-powershell.md)
