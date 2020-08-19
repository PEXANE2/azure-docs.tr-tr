---
title: Cihaz kimliği ve masaüstü sanallaştırma-Azure Active Directory
description: VDı ve Azure AD cihaz kimliklerinin birlikte nasıl kullanılabileceğini öğrenin
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
ms.openlocfilehash: 8e2c516371ada59501edd89491a07014ef949eba
ms.sourcegitcommit: d661149f8db075800242bef070ea30f82448981e
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/19/2020
ms.locfileid: "88604392"
---
# <a name="device-identity-and-desktop-virtualization"></a>Cihaz kimliği ve Masaüstü Sanallaştırması

Yöneticiler, kuruluşlarında Windows işletim sistemlerini barındıran sanal masaüstü altyapısı (VDı) platformlarını yaygın olarak dağıtır. Yöneticiler VDı 'yi şu şekilde dağıtır:

- Yönetimi kolaylaştırma.
- Konsolidasyon ve merkezileşmeyi kaynakların maliyetlerini azaltın.
- Her yerden, herhangi bir cihazda, her yerden sanal masaüstlerine erişim sağlamak için son kullanıcılara yönelik taşınabilirlik ve özgürlük sunun.

İki ana sanal masaüstü türü vardır:

- Kalıcı
- Kalıcı olmayan

Kalıcı sürümler, her kullanıcı veya bir Kullanıcı havuzu için benzersiz bir masaüstü görüntüsü kullanır. Bu benzersiz masaüstleri, gelecekte kullanılmak üzere özelleştirilebilir ve kaydedilebilir. 

Kalıcı olmayan sürümler, kullanıcıların gerektiğinde erişebileceği bir masaüstü bilgisayar koleksiyonu kullanır. Kalıcı olmayan bu masaüstleri özgün durumlarına geri döndürülüyor, Windows geçerli<sup>1</sup> durumunda bu durum, bir sanal makine bir kapatma/yeniden başlatma/işletim sistemi sıfırlama işleminden geçtiğinde ve Windows alt düzey<sup>2</sup> olduğunda bu durum Kullanıcı oturumu kapattığında meydana gelir.

Kalıcı olmayan VDı dağıtımlarında, uzak çalışma yeni norm olmaya devam eden bir artış vardı. Müşteriler kalıcı olmayan VDı dağıtımı yaparken cihaz dalgalanmasını yönetmeniz önemlidir, çünkü cihaz yaşam döngüsü yönetimi için uygun bir stratejiye gerek kalmadan sık kullanılan cihaz kaydı nedeniyle meydana gelmiş olabilir.

> [!IMPORTANT]
> Cihaz dalgalanmasını yönetmenin başarısız olması, kiracı kotası kullanım tüketiminiz üzerinde basınç artışına ve kiracı kotasından daha fazla hizmet kesintisi riskine yol açabilir. Bu durumdan kaçınmak için kalıcı olmayan VDı ortamları dağıttığınızda aşağıda belgelenen yönergeleri izlemeniz gerekir.

Bu makalede, Microsoft 'un cihaz kimliği ve VDı desteğiyle ilgili yöneticilere yönelik Kılavuzu ele alınacaktır. Cihaz kimliği hakkında daha fazla bilgi için, [cihaz kimliği nedir](overview.md)makalesine bakın.

## <a name="supported-scenarios"></a>Desteklenen senaryolar

VDı ortamınız için Azure AD 'de cihaz kimliklerini yapılandırmadan önce, desteklenen senaryolar hakkında bilgi edinin. Aşağıdaki tabloda hangi sağlama senaryolarının desteklendiği gösterilmektedir. Bu bağlamda sağlama, bir yöneticinin herhangi bir son kullanıcı etkileşimi gerektirmeden cihaz kimliklerini ölçeklendirerek yapılandırabilmesini gerektirir.

| Cihaz kimliği türü | Kimlik altyapısı | Windows cihazları | VDı platformu sürümü | Desteklenir |
| --- | --- | --- | --- | --- |
| Hibrit Azure AD'ye katılmış | Federal<sup>3</sup> | Windows geçerli ve Windows alt düzeyi | Kalıcı | Yes |
|   |   | Windows geçerli | Kalıcı olmayan | Evet<sup>5</sup> |
|   |   | Windows alt düzey | Kalıcı olmayan | Evet<sup>6</sup> |
|   | Yönetilen<sup>4</sup> | Windows geçerli ve Windows alt düzeyi | Kalıcı | Yes |
|   |   | Windows geçerli | Kalıcı olmayan | No |
|   |   | Windows alt düzey | Kalıcı olmayan | Evet<sup>6</sup> |
| Azure AD'ye katılanlar | Federe | Windows geçerli | Kalıcı | No |
|   |   |   | Kalıcı olmayan | No |
|   | Yönetilen | Windows geçerli | Kalıcı | No |
|   |   |   | Kalıcı olmayan | No |
| Azure AD kayıtlı | Federasyon/yönetilen | Windows geçerli/Windows alt düzeyi | Kalıcı/kalıcı olmayan | Geçerli değil |

<sup>1</sup> Windows **geçerli** cihaz Windows 10, Windows Server 2016 ve Windows Server 2019 ' i temsil eder.

<sup>2</sup> **Windows alt düzey** cihazlar Windows 7, Windows 8.1, Windows Server 2008 r2, Windows Server 2012 ve Windows Server 2012 R2 'yi temsil eder. Windows 7 ile ilgili destek bilgileri için bkz. [Windows 7 Için destek sonlandırılıyor](https://www.microsoft.com/microsoft-365/windows/end-of-windows-7-support). Windows Server 2008 R2 ile ilgili destek bilgileri için bkz. [Windows server 2008 Için hazırlanma-destek sonu](https://www.microsoft.com/cloud-platform/windows-server-2008).

<sup>3</sup> bir **Federasyon** kimlik altyapısı ortamı, AD FS veya diğer üçüncü taraf IDP gibi bir kimlik sağlayıcısı olan bir ortamı temsil eder.

<sup>4</sup> **yönetilen** bir kimlik altyapısı ortamı, [sorunsuz çoklu oturum açma](../hybrid/how-to-connect-sso.md)ile [Parola karması eşitleme (PHS)](../hybrid/whatis-phs.md) veya [geçişli kimlik doğrulaması (PTA)](../hybrid/how-to-connect-pta.md) ile dağıtılan kimlik sağlayıcısı olarak Azure AD ile bir ortamı temsil eder.

<sup>5</sup> **Windows için kalıcılık olmayan destek** , aşağıda belgelenen kılavuz bölümünde daha fazla dikkat gerektirir.

<sup>6</sup> **Windows alt düzey için kalıcı olmayan 6 desteği** , kılavuz bölümünde aşağıda belgelenen şekilde daha fazla dikkat gerektirir.


## <a name="microsofts-guidance"></a>Microsoft 'un Kılavuzu

Yöneticiler, karma Azure AD JOIN 'in nasıl yapılandırılacağını öğrenmek için kimlik altyapısına bağlı olarak aşağıdaki makalelere başvurmalıdır.

- [Federasyon ortamına yönelik karma Azure Active Directory birleştirmesini yapılandırma](hybrid-azuread-join-federated-domains.md)
- [Yönetilen ortam için karma Azure Active Directory birleştirmesini yapılandırma](hybrid-azuread-join-managed-domains.md)

Kalıcı olmayan VDı dağıtımı yaparken, Microsoft BT yöneticilerinin aşağıdaki kılavuzu uygulamasını önerir. Bunun yapılmaması, dizininizdeki kalıcı olmayan VDı platformunuzun çok sayıda eski karma Azure AD 'ye katılmış cihazına sahip olmasına neden olur. bu da, kiracı kotasında daha fazla basınç elde edilmesine ve kiracı kotasının tükenmesini sağlamak için hizmet kesintisi riskini artırmıştır.

- Sistem Hazırlama Aracı 'nı (sysprep.exe) kullanıyorsanız ve yükleme için Windows 10 1809 öncesi bir görüntü kullanıyorsanız, görüntünün Azure AD 'ye karma Azure AD 'ye katılmış olarak zaten kayıtlı olan bir cihazdan olmadığından emin olun.
- Ek VM 'Ler oluşturmak için bir sanal makine (VM) anlık görüntüsüne güvenmek istiyorsanız, anlık görüntünün Azure AD 'ye karma Azure AD katılımı olarak zaten kayıtlı olan bir VM 'den olmadığından emin olun.
- Masaüstünü kalıcı olmayan VDı tabanlı olarak gösteren bilgisayarın görünen adı (örn. NPVDı-) için bir ön ek oluşturun ve kullanın.
- Windows alt düzeyi için:
   - Logoff komut dosyasının bir parçası olarak, **oto workplacejoın/Leave** komutunu uygulayın. Bu komut kullanıcı bağlamında tetiklenmelidir ve Kullanıcı tamamen oturumu kapatmadan ve hala ağ bağlantısı varken yürütülmesi gerekir.
- Federasyon ortamında Windows için geçerli (ör. AD FS):
   - **Dsregcmd/JOIN** ' i VM önyükleme sırasının bir parçası olarak uygulayın.
   - VM kapatılırken/yeniden başlatma işleminin bir parçası olarak dsregcmd/Leave **yürütme.**
- [Eski cihazları yönetmek](manage-stale-devices.md)için işlem tanımlayın ve uygulayın.
   - Kalıcı olmayan karma Azure AD 'ye katılmış cihazlarınızı (ör. bilgisayar görünen adı önekini kullanarak) belirleme stratejiniz varsa, dizininizin çok sayıda eski cihaz ile tüketilmemesini sağlamak için bu cihazların temizlenmesi üzerinde daha Agresif olmanız gerekir.
   - Windows geçerli ve alt düzeyde kalıcı olmayan VDı dağıtımları için, 15 günden daha eski olan **yaklaşık bir Telastlogontimestamp** olan cihazları silmelisiniz.
 
## <a name="next-steps"></a>Sonraki adımlar

[Federasyon ortamında karma Azure Active Directory katılmayı yapılandırma](hybrid-azuread-join-federated-domains.md)
