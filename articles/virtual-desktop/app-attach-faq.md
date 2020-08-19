---
title: Windows sanal masaüstü MALTı uygulama iliştirme SSS-Azure
description: Windows sanal masaüstü için MSIX uygulama iliştirme hakkında sık sorulan sorular.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: conceptual
ms.date: 08/17/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: a63d7f067665836910b91b2911db522f0a92bbb1
ms.sourcegitcommit: 023d10b4127f50f301995d44f2b4499cbcffb8fc
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/18/2020
ms.locfileid: "88556340"
---
# <a name="msix-app-attach-faq"></a>MSIX uygulama iliştirme SSS

Bu makalede, Windows sanal masaüstü için MSIX uygulama iliştirme hakkında sık sorulan sorular yanıtlanmaktadır.

## <a name="does-msix-app-attach-use-fslogix"></a>MSIX uygulaması iliştirme FSLogix kullanıyor mu?

MSIX uygulaması iliştirme FSLogix kullanmaz. Ancak, uygulama iliştirme ve FSLogix, sorunsuz bir kullanıcı deneyimi sağlamak için birlikte çalışacak şekilde tasarlanmıştır.

## <a name="can-i-use-msix-app-attach-outside-of-windows-virtual-desktop"></a>MALTı uygulamayı Windows sanal masaüstü dışında kullanıma açabilir miyim?

Evet, MSIX uygulama iliştirme, Windows 10 Enterprise ile birlikte gelen ve Windows sanal masaüstü dışında kullanılabilen bir özelliktir. Ancak, MSIX uygulaması için Windows sanal masaüstü dışında ek bir yönetim düzlemi yoktur.

## <a name="how-do-i-get-an-msix-package"></a>Nasıl yaparım? MSIX paketi mi kullanıyorsunuz?

Yazılım satıcınız size bir MSIX paketi verecektir. Maltısız paketleri de MALTıYA dönüştürebilirsiniz. [Mevcut yükleyiclerinizi MALTıYA taşıma hakkında](/windows/msix/packaging-tool/create-an-msix-overview#how-to-move-your-existing-installers-to-msix)daha fazla bilgi edinin.

## <a name="which-operating-systems-support-msix-app-attach"></a>Hangi işletim sistemleri MSIX uygulamasını destekliyor?

Windows 10 Enterprise ve Windows 10 Enterprise çoklu oturum.

## <a name="next-steps"></a>Sonraki adımlar

MSIX uygulama iliştirme hakkında daha fazla bilgi edinmek istiyorsanız, [genel bakış](what-is-app-attach.md) [Sözlüğümüzü](app-attach-glossary.md)inceleyin. Aksi takdirde, [uygulama eklemeyi ayarlama](app-attach.md)ile çalışmaya başlayın.