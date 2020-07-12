---
title: Yeni oturum Konakları ile mevcut konak havuzunu genişletme-Azure
description: Mevcut bir konak havuzunu Windows sanal masaüstündeki yeni oturum Konakları ile genişletme.
services: virtual-desktop
author: Heidilohr
ms.service: virtual-desktop
ms.topic: how-to
ms.date: 04/30/2020
ms.author: helohr
manager: lizross
ms.openlocfilehash: 5eee6f34124f8b286deff278c6c3d1038b595357
ms.sourcegitcommit: dabd9eb9925308d3c2404c3957e5c921408089da
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/11/2020
ms.locfileid: "86259885"
---
# <a name="expand-an-existing-host-pool-with-new-session-hosts"></a>Yeni oturum Konakları ile mevcut bir konak havuzunu Genişlet

>[!IMPORTANT]
>Bu içerik, Azure Resource Manager Windows sanal masaüstü nesneleriyle Spring 2020 güncelleştirmesine yöneliktir. Windows sanal masaüstü Fall 2019 sürümünü Azure Resource Manager nesneleri olmadan kullanıyorsanız, [Bu makaleye](./virtual-desktop-fall-2019/expand-existing-host-pool-2019.md)bakın.
>
> Windows sanal masaüstü Spring 2020 güncelleştirmesi şu anda genel önizlemededir. Bu önizleme sürümü, bir hizmet düzeyi sözleşmesi olmadan sağlanır ve bunu üretim iş yükleri için kullanmanızı önermiyoruz. Bazı özellikler desteklenmiyor olabileceği gibi özellikleri sınırlandırılmış da olabilir.
> Daha fazla bilgi için bkz. [Microsoft Azure önizlemeleri Için ek kullanım koşulları](https://azure.microsoft.com/support/legal/preview-supplemental-terms/).

Konak havuzunuzdaki kullanımı artırın, yeni yükü işlemek için mevcut konak havuzunuzu yeni oturum konaklarıyla genişletmeniz gerekebilir.

Bu makalede, mevcut bir konak havuzunu yeni oturum Konakları ile nasıl genişletebileceğiniz açıklanır.

## <a name="what-you-need-to-expand-the-host-pool"></a>Konak havuzunu genişletmeniz için gerekenler

Başlamadan önce, aşağıdaki yöntemlerden birini kullanarak bir konak havuzu ve oturum ana bilgisayarı sanal makineleri (VM 'Ler) oluşturduğunuzdan emin olun:

- [Azure portalındaki](./create-host-pools-azure-marketplace.md)
- [PowerShell ile ana bilgisayar havuzu oluşturma](./create-host-pools-powershell.md)

Konak havuzunu ve oturum ana bilgisayar VM 'lerini ilk oluşturduğunuzda aşağıdaki bilgilere de ihtiyacınız olacaktır:

- VM boyutu, resim ve ad ön eki
- Etki alanına ekleme yönetici kimlik bilgileri
- Sanal ağ adı ve alt ağ adı

## <a name="add-virtual-machines-with-the-azure-portal"></a>Azure portal ile sanal makineler ekleme

Sanal makineler ekleyerek konak havuzunuzu genişletmek için:

1. Azure portalında oturum açın.

2. **Windows sanal masaüstü**araması yapın ve seçin.

3. Ekranın sol tarafındaki menüde **konak havuzları**' nı seçin ve ardından sanal makineleri eklemek istediğiniz konak havuzunun adını seçin.

4. Ekranın sol tarafındaki menüden **oturum Konakları** ' nı seçin.

5. Ana bilgisayar havuzunuzu oluşturmaya başlamak için **+ Ekle** ' yi seçin.

6. Temel bilgiler sekmesini yoksayın ve bunun yerine **VM ayrıntıları** sekmesini seçin. Burada, konak havuzuna eklemek istediğiniz sanal makinenin (VM) ayrıntılarını görüntüleyebilir ve düzenleyebilirsiniz.

7. VM 'Leri oluşturmak istediğiniz kaynak grubunu seçin, ardından bölgeyi seçin. Kullanmakta olduğunuz geçerli bölgeyi veya yeni bir bölgeyi seçebilirsiniz.

8. Konak havuzunuza eklemek istediğiniz oturum ana bilgisayarlarının sayısını **VM sayısına**girin. Örneğin, ana bilgisayar havuzunuzu beş ana bilgisayar ile genişlettiğiniz takdirde **5**girin.

    >[!NOTE]
    >Konak havuzundaki tüm VM 'Lerin aynı boyutta olduğundan emin olmak önemlidir çünkü VM 'lerin boyutunu veya görüntüsünü düzenleyemezsiniz.

9. Sanal **ağ bilgileri**için, sanal makinelerin katılmasını istediğiniz sanal ağı ve alt ağı seçin. Mevcut makinelerinizdeki aynı sanal ağı seçebilir veya adım 7 ' de seçtiğiniz bölgeye daha uygun olan farklı bir tane seçebilirsiniz.

10. **Yönetici hesabı**için, seçtiğiniz sanal ağla ilişkili Active Directory etki alanı Kullanıcı adı ve parolasını girin. Bu kimlik bilgileri, sanal makineleri sanal ağa katmak için kullanılacaktır.

      >[!NOTE]
      >Yönetici adlarınızın burada verilen bilgiyle uyumlu olduğundan emin olun. Hesapta etkin bir MFA yok.

11. Sanal makineleri gruplandırmak istediğiniz etiketlere sahipseniz **etiket** sekmesini seçin. Aksi takdirde, bu sekmeyi atlayın.

12. **Gözden geçir + oluştur** sekmesini seçin. seçimlerinizi gözden geçirin ve her şey hassas görünüyorsa **Oluştur**' u seçin.

## <a name="next-steps"></a>Sonraki adımlar

Mevcut konak havuzunuzu genişlettiyseniz, bir kullanıcı oturumunun parçası olarak test etmek için bir Windows sanal masaüstü istemcisinde oturum açabilirsiniz. Aşağıdaki istemcilerden herhangi biriyle bir oturuma bağlanabilirsiniz:

- [Windows Masaüstü istemcisine bağlanma](./connect-windows-7-10.md)
- [Web istemcisiyle bağlanma](./connect-web.md)
- [Android istemcisiyle bağlanma](./connect-android.md)
- [macOS istemcisiyle bağlanma](./connect-macos.md)
- [iOS istemcisiyle bağlanma](./connect-ios.md)
