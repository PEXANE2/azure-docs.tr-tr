---
title: Aygıt kimliği ve masaüstü sanallaştırması - Azure Active Directory
description: VDI ve Azure AD aygıt kimliklerinin birlikte nasıl kullanılabileceğini öğrenin
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 10/15/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: sandeo
ms.collection: M365-identity-device-management
ms.openlocfilehash: 7b431cee3b8e5fc168dec2766442d6f6b9869d1e
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74900367"
---
# <a name="device-identity-and-desktop-virtualization"></a>Aygıt kimliği ve masaüstü sanallaştırma

Yöneticiler genellikle kuruluşlarında Windows işletim sistemlerini barındıran sanal masaüstü altyapısı (VDI) platformlarını dağıtıyor. Yöneticiler VDI'yi şu şekilde dağıtıyor:

- Yönetimi kolaylaştırın.
- Kaynakların konsolidasyonu ve merkezileştirilmesi yoluyla maliyetleri azaltın.
- Son kullanıcılara mobilite ve sanal masaüstlerine her zaman, her yerden, her cihazdan erişme özgürlüğü sunun.

İki birincil sanal masaüstü türü vardır:

- Kalıcı
- Kalıcı olmayan

Kalıcı sürümler, her kullanıcı veya kullanıcı havuzu için benzersiz bir masaüstü görüntüsü kullanır. Bu benzersiz masaüstü bilgisayarlar, ileride kullanılmak üzere özelleştirilebilir ve kaydedilebilir. 

Kalıcı olmayan sürümler, kullanıcıların gerektiği gibi erişebileceği bir masaüstü koleksiyonu kullanır. Bu kalıcı olmayan masaüstü bilgisayarlar, kullanıcı çıktıktan sonra orijinal durumuna döndürülür.

Bu makale, Microsoft'un aygıt kimliği ve VDI desteği yle ilgili yöneticilere yönelik kılavuzunu kapsayacaktır. Aygıt kimliği hakkında daha fazla bilgi için, aygıt [kimliği nedir](overview.md)makalesine bakın.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

VDI ortamınız için Azure AD'de aygıt kimliklerini yapılandırmadan önce, desteklenen senaryoları tanıyın. Aşağıdaki tabloda hangi sağlama senaryolarının desteklendiği gösterilebilir. Bu bağlamda sağlama, yöneticinin aygıt kimliklerini herhangi bir son kullanıcı etkileşimi gerektirmeden ölçekte yapılandırabileceği anlamına gelir.

| Aygıt kimlik türü | Kimlik altyapısı | Windows cihazları | VDI platform sürümü | Destekleniyor |
| --- | --- | --- | --- | --- |
| Hibrit Azure AD'ye katılmış | Federe* | Windows geçerli*** ve Windows alt düzey**** | Kalıcı | Evet |
|   |   | Windows akımı | Kalıcı Olmayan | Hayır |
|   |   | Windows alt düzey | Kalıcı Olmayan | Evet |
|   | Yönetilen** | Windows geçerli ve Windows alt düzey | Kalıcı | Evet |
|   |   | Windows akımı | Kalıcı Olmayan | Hayır |
|   |   | Windows alt düzey | Kalıcı Olmayan | Evet |
| Azure AD'ye katılanlar | Federe | Windows akımı | Kalıcı | Hayır |
|   |   |   | Kalıcı Olmayan | Hayır |
|   | Yönetilen | Windows akımı | Kalıcı | Hayır |
|   |   |   | Kalıcı Olmayan | Hayır |
| Azure AD kayıtlı | Federe | Windows akımı | Kalıcı | Hayır |
|   |   |   | Kalıcı Olmayan | Hayır |
|   | Yönetilen | Windows akımı | Kalıcı | Hayır |
|   |   |   | Kalıcı Olmayan | Hayır |

\***Federe** kimlik altyapısı ortamı, AD FS veya diğer üçüncü taraf IDP gibi bir kimlik sağlayıcısının olduğu bir ortamı temsil eder.

\*\***Yönetilen** kimlik altyapısı ortamı, [parola karma eşitleme (PHS)](../hybrid/whatis-phs.md) veya sorunsuz [tek oturum açma](../hybrid/how-to-connect-sso.md)ile geçiş [kimlik doğrulaması (PTA)](../hybrid/how-to-connect-pta.md) ile dağıtılan kimlik sağlayıcısı olarak Azure AD'nin olduğu bir ortamı temsil eder.

\*\*\***Windows geçerli** aygıtları Windows 10, Windows Server 2016 ve Windows Server 2019'u temsil ediyor.

\*\*\*\***Windows alt düzey** aygıtlar Windows 7, Windows 8.1, Windows Server 2008 R2, Windows Server 2012 ve Windows Server 2012 R2'yi temsil ediyor. Windows 7 hakkındaki destek bilgileri [için](https://www.microsoft.com/microsoft-365/windows/end-of-windows-7-support)bkz. Windows Server 2008 R2 hakkında destek bilgileri için windows [server 2008 desteğinin sonuna hazırla'ya](https://www.microsoft.com/cloud-platform/windows-server-2008)bakın.

## <a name="microsofts-guidance"></a>Microsoft'un kılavuzu

Yöneticiler, karma Azure AD birleştirmesini nasıl yapılandıracaklarını öğrenmek için kimlik altyapılarına dayalı olarak aşağıdaki makalelere başvurmalıdır.

- [Federe ortam için karma Azure Active Directory birleştirme yapılandırma](hybrid-azuread-join-federated-domains.md)
- [Yönetilen ortam için karma Azure Etkin Dizin birleştirme yapılandırma](hybrid-azuread-join-managed-domains.md)

Sistem Hazırlama Aracı'na (sysprep.exe) güveniyorsanız ve yükleme için Windows 10 1809 öncesi bir görüntü kullanıyorsanız, bu görüntünün Azure AD'ye karma Azure AD'si katıldıkça zaten kaydedilmiş bir aygıttan görüntü olmadığından emin olun.

Ek VM'ler oluşturmak için Sanal Makine (VM) anlık görüntüsüne güveniyorsanız, anlık görüntün Azure AD'ye Karma Azure AD join olarak zaten kaydedilmiş bir VM'den olmadığından emin olun.

Kalıcı olmayan VDI dağıtırken, BT yöneticileri Azure AD'de eski aygıtları yönetmeye çok dikkat etmelidir. Microsoft, BT yöneticilerinin aşağıdaki kılavuzu uygulamalarını önerir. Aksi takdirde, dizininizin kalıcı olmayan VDI platformunuzdan kaydedilmiş çok sayıda eski Karma Azure Azure AD bağlantısına sahip olması yla sonuçlanır.

- Bilgisayarın görüntü adı için masaüstüvdi tabanlı olarak gösteren bir önek oluşturun ve kullanın.
- Oturum kapatma komutdosyasının bir parçası olarak aşağıdaki komutu uygulayın. Bu komut, aygıtı silmek için Azure AD'ye en iyi çaba çağrısını tetikler.
   - Windows aşağı seviyeli cihazlar için – autoworkplace.exe /leave
- [Eski aygıtları yönetmek](manage-stale-devices.md)için süreci tanımlayın ve uygulayın.
   - Kalıcı olmayan Karma Azure AD birleştirilmiş cihazlarınızı tanımlamak için bir stratejiniz olduktan sonra, dizininizin çok sayıda eski aygıtla tüketilmediğinden emin olmak için bu aygıtların temizliğinde daha agresif olabilirsiniz.
 
## <a name="next-steps"></a>Sonraki adımlar

[Federe ortam için karma Azure Active Directory join'i yapılandırma](hybrid-azuread-join-federated-domains.md)
