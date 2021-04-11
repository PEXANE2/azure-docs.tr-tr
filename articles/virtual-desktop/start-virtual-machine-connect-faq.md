---
title: Windows sanal masaüstü başlangıç VM Connect SSS-Azure
description: Connect üzerinde VM Başlat özelliğini kullanmaya yönelik sık sorulan sorular ve en iyi uygulamalar.
author: Heidilohr
ms.topic: conceptual
ms.date: 03/31/2021
ms.author: helohr
manager: lizross
ms.openlocfilehash: 0fd6edf3fbcd6fea7503acd7265850723b5ec09f
ms.sourcegitcommit: 3ee3045f6106175e59d1bd279130f4933456d5ff
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/31/2021
ms.locfileid: "106080633"
---
# <a name="start-vm-on-connect-faq-preview"></a>Connect 'te VM 'yi başlatma SSS (Önizleme)

> [!IMPORTANT]
> Connect üzerinde VM Başlat özelliği şu anda genel önizlemededir.
> Önizleme sürümü bir hizmet düzeyi sözleşmesi olmadan sağlanır ve üretim iş yüklerinde kullanılması önerilmez. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir. Daha fazla bilgi için bkz. [Microsoft Azure Önizlemeleri için Ek Kullanım Koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Bu makalede, Windows sanal masaüstü ana bilgisayar havuzlarının Connect (Önizleme) özelliği üzerinde başlangıç sanal makinesi (VM) ile ilgili sık sorulan sorular ele alınmaktadır.

## <a name="are-vms-automatically-deallocated-when-a-user-stops-using-them"></a>Bir kullanıcı onları kullanmayı durdurduktan sonra VM 'Ler otomatik olarak serbest bırakılır mi?

Hayır. Kullanıcıları oturumlarını kapatmak ve VM 'Leri serbest bırakmak için Azure Otomasyonu betikleri çalıştırmak için ek ilkeler yapılandırmanız gerekir.

Ayırmayı kaldırma ilkesini yapılandırmak için:

1. İlkesini ayarlamak istediğiniz sanal makineye uzaktan bağlanın.

2. **Grup İlkesi düzenleyicisini** açın, sonra   >    >    >  **Windows bileşenleri** Yönetim Şablonları  >    >  **Uzak Masaüstü oturumu ana bilgisayarı**  >  **oturum süresi sınırlarına** Uzak Masaüstü Hizmetleri yerel bilgisayar ilkesi bilgisayar yapılandırması ' na gidin.

3. **Bağlantısı kesilmiş oturumlar için zaman sınırını ayarla**' yı belirten ilkeyi bulun, sonra değerini **etkin** olarak değiştirin.

4. İlkeyi etkinleştirdikten sonra, **bağlantısı kesilen oturumu sonlandır**' ı seçin.

>[!NOTE]
>"Bağlantısı kesilen oturum" ilkesi için zaman sınırını beş dakikadan daha büyük bir değere ayarladığınızdan emin olun. Düşük bir süre sınırı, ağ bağlantısı çok uzun süredir bağlantıyı kaybederse, kayıp çalışmasına neden olan kullanıcıların oturumlarının bitmesini sağlayabilir.

Kullanıcıların imzalanması, VM 'lerini serbest bırakabilir. VM 'Leri serbest bırakma hakkında bilgi edinmek için bkz. [VM 'leri çalışma saatleri dışında başlatma veya durdurma](../automation/automation-solution-vm-management.md).

## <a name="can-users-turn-off-the-vm-from-their-clients"></a>Kullanıcılar sanal makineyi istemcilerinden kapatabilir mi?

Evet. Kullanıcılar, bir fiziksel makineyle yaptıkları gibi, oturumunda Başlat menüsünü kullanarak VM 'yi kapatabilir. Ancak, VM 'yi kapatmak VM 'yi serbest bırakabilir. VM 'Leri serbest bırakma hakkında bilgi edinmek için bkz. [VM 'leri çalışma saatleri dışında başlatma veya durdurma](../automation/automation-solution-vm-management.md).

## <a name="next-steps"></a>Sonraki adımlar

Connect 'te başlangıç VM 'sini yapılandırma hakkında bilgi edinmek için bkz. [Connect 'te sanal makineyi Başlat (Önizleme)](start-virtual-machine-connect.md).

Windows sanal masaüstü hakkında daha fazla genel sorunuz varsa, genel [SSS bölümüne](faq.md)göz atın.
