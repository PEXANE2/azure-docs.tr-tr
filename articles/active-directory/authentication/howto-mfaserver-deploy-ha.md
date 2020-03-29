---
title: Azure MFA Server için yüksek kullanılabilirlik - Azure Active Directory
description: Yüksek kullanılabilirlik sağlayan yapılandırmalarda Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'nun birden çok örneğini dağıtın.
services: multi-factor-authentication
ms.service: active-directory
ms.subservice: authentication
ms.topic: conceptual
ms.date: 11/21/2019
ms.author: iainfou
author: iainfoulds
manager: daveba
ms.reviewer: michmcla
ms.collection: M365-identity-device-management
ms.openlocfilehash: a7b2df4e87dddcfedd10682e4e3ab6c014ad7bbb
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/27/2020
ms.locfileid: "74848196"
---
# <a name="configure-azure-multi-factor-authentication-server-for-high-availability"></a>Yüksek kullanılabilirlik için Azure Çok Faktörlü Kimlik Doğrulama Sunucusu'na yapılandırma

Azure Server MFA dağıtımınızda yüksek kullanılabilirlik elde etmek için birden çok MFA sunucusu dağıtmanız gerekir. Bu bölümde, Azure MFS Server dağıtımınızda yüksek kullanılabilirlik hedeflerinize ulaşmak için yük dengeli bir tasarım hakkında bilgi verilmektedir.

> [!IMPORTANT]
> 1 Temmuz 2019 itibariyle Microsoft, yeni dağıtımlar için Artık MFA Server sunmayacak. Kullanıcılarından çok faktörlü kimlik doğrulaması isteyen yeni müşteriler bulut tabanlı Azure Çok Faktörlü Kimlik Doğrulaması'nı kullanmalıdır. 1 Temmuz'dan önce MFA Server'ı etkinleştirmiş olan mevcut müşteriler en son sürümü, gelecekteki güncelleştirmeleri karşıdan yükleyebilecek ve her zamanki gibi etkinleştirme kimlik bilgilerini oluşturabilecek.

## <a name="mfa-server-overview"></a>MFA Server'a genel bakış

Azure MFA Server hizmet mimarisi, aşağıdaki diyagramda gösterildiği gibi birkaç bileşenden oluşur:

 ![MFA Server Architecture bileşenleri](./media/howto-mfaserver-deploy-ha/mfa-ha-architecture.png)

MFA Server, Azure Çok Faktörlü Kimlik Doğrulama yazılımı yüklü olan bir Windows Server'dır. MFA Server örneğinin çalışabilmesi için Azure'daki MFA Hizmeti tarafından etkinleştirilmesi gerekir. Şirket içinde birden fazla MFA Sunucusu kurulabilir.

Yüklenen ilk MFA Server varsayılan olarak Azure MFA Hizmeti tarafından etkinleştirildikten sonra ana MFA Server'dır. Ana MFA sunucusu, PhoneFactor.pfdata veritabanının yazılabilir bir kopyasına sahiptir. MFA Server örneklerinin sonraki yüklemeleri ast olarak bilinir. MFA astları PhoneFactor.pfdata veritabanının çoğaltılmış salt okunur kopyasına sahiptir. MFA sunucuları Uzaktan Yordam Çağrısı (RPC) kullanarak bilgileri çoğaltAbilir. Tüm MFA Severs topluca ya etki alanı katıldı ya da tek başına bilgi çoğaltmak için olmalıdır.

İki faktörlü kimlik doğrulama gerektiğinde hem MFA yöneticisi hem de ast MFA Sunucuları MFA Hizmeti ile iletişim kurar. Örneğin, bir kullanıcı iki faktörlü kimlik doğrulaması gerektiren bir uygulamaya erişmeye çalıştığında, kullanıcı ilk olarak Active Directory (AD) gibi bir kimlik sağlayıcısı tarafından kimlik doğrulaması yapılacaktır.

AD ile başarılı kimlik doğrulamasonra, MFA Server MFA Hizmeti ile iletişim kurar. MFA Sunucusu, kullanıcının uygulamaya erişimini engellemek veya reddetmek için MFA Hizmeti'nden bildirim bekler.

MFA ana sunucusu çevrimdışı duruma geçerse, kimlik doğrulamaları yine de işlenebilir, ancak MFA veritabanında değişiklik gerektiren işlemler işlenemez. (Örnekler şunlardır: kullanıcıların eklenmesi, self-servis PIN değişiklikleri, değişen kullanıcı bilgileri veya kullanıcı portalına erişim)

## <a name="deployment"></a>Dağıtım

Azure MFA Server ve ilgili bileşenlerini yük dengeleme için aşağıdaki önemli noktaları göz önünde bulundurun.

* **Yüksek kullanılabilirlik elde etmek için RADIUS standardını kullanma.** Azure MFA Sunucularını RADIUS sunucusu olarak kullanıyorsanız, bir MFA Server'ı birincil RADIUS kimlik doğrulama hedefi olarak ve diğer Azure MFA Sunucularını ikincil kimlik doğrulama hedefleri olarak yapılandırma potansiyeliniz vardır. Ancak, ikincil kimlik doğrulama hedefine karşı kimlik doğrulamanın birincil kimlik doğrulama hedefinde başarısız olması gerektiğinden, yüksek kullanılabilirlik elde etmek için bu yöntem pratik olmayabilir. RADIUS istemcisi ile RADIUS Sunucuları (bu durumda, RADIUS sunucusu olarak hareket eden Azure MFA Sunucuları) arasındaki RADIUS trafiğini yüklemek daha verimlidir, böylece RADIUS istemcilerini işaret edebilecekleri tek bir URL ile yapılandırabilirsiniz.
* **MFA astlarını el ile tanıtmanız gerekir.** Ana Azure MFA sunucusu çevrimdışı ysa, ikincil Azure MFA Sunucuları MFA isteklerini işlemeye devam eder. Ancak, ana MFA sunucusu kullanılabilir olana kadar, yöneticiler kullanıcı ekemez veya MFA ayarlarını değiştiremez ve kullanıcılar kullanıcı portalını kullanarak değişiklik yapamaz. Ana role bir MFA astını teşvik etmek her zaman el ile bir işlemdir.
* **Bileşenlerin ayrılmazlığı.** Azure MFA Sunucusu, aynı Windows Server örneğine veya farklı örneklere yüklenebilen birkaç bileşenden oluşur. Bu bileşenler arasında Kullanıcı Portalı, Mobil Uygulama Web Hizmeti ve ADFS bağdaştırıcısı (aracı) bulunur. Bu eşitlik, Kullanıcı Portalı ve Mobil Uygulama Web Sunucusu'nu çevre ağından yayınlamak için Web Application Proxy'yi kullanmayı mümkün kılar. Böyle bir yapılandırma, aşağıdaki diyagramda gösterildiği gibi, tasarımınızın genel güvenliğine katkıda bulunmaktadır. MFA Kullanıcı Portalı ve Mobil App Web Sunucusu, HA yük dengeli yapılandırmalarında da dağıtılabilir.

   ![Çevre Ağı olan MFA Server](./media/howto-mfaserver-deploy-ha/mfasecurity.png)

* **SMS üzerinden tek seferlik şifre (OTP) (aka tek yönlü SMS) trafik yük dengeli ise yapışkan oturumların kullanımını gerektirir.** Tek yönlü SMS, MFA Sunucusu'nun kullanıcılara OTP içeren bir kısa mesaj göndermesine neden olan bir kimlik doğrulama seçeneğidir. Kullanıcı, MFA meydan okumasını tamamlamak için OTP'yi bir istem penceresinde girer. Bakiye bakiyesi Azure MFA Sunucularını yüklerseniz, ilk kimlik doğrulama isteğine hizmet veren sunucu kullanıcıdan OTP iletisi alan sunucu olmalıdır; Başka bir MFA Sunucusu OTP yanıtını alırsa, kimlik doğrulama zorluğu başarısız olur. Daha fazla bilgi için Azure [MFA Server'a Eklenen SMS üzerinden Tek Seferlik Parola'ya](https://blogs.technet.microsoft.com/enterprisemobility/2015/03/02/one-time-password-over-sms-added-to-azure-mfa-server)bakın.
* **Kullanıcı Portalı ve Mobil Uygulama Web Hizmetinin Yük Dengeli dağıtımları yapışkan oturumlar gerektirir.** MFA Kullanıcı Portalı ve Mobil Uygulama Web Hizmeti'ni yük dengelemeniz gerekiyorsa, her oturumun aynı sunucuda kalması gerekir.

## <a name="high-availability-deployment"></a>Yüksek kullanılabilirlik dağıtımı

Aşağıdaki diyagram, Azure MFA ve bileşenlerinin tam bir HA yük dengeli uygulamasının yanı sıra başvuru için ADFS'yi gösterir.

 ![Azure MFA Server HA uygulaması](./media/howto-mfaserver-deploy-ha/mfa-ha-deployment.png)

Önceki diyagramın ilgili numaralı alanı için aşağıdaki öğeleri not edin.

1. İki Azure MFA Sunucusu (MFA1 ve MFA2) yük dengelidir (mfaapp.contoso.com) ve PhoneFactor.pfdata veritabanını çoğaltmak için statik bağlantı noktası (4443) kullanacak şekilde yapılandırılmıştır. Web Hizmeti SDK, ADFS sunucuları ile TCP bağlantı noktası 443 üzerinden iletişimi etkinleştirmek için MFA Server'ın her birine yüklenir. MFA sunucuları, durum dışı yük dengeli yapılandırmada dağıtılır. Ancak, SMS üzerinden OTP kullanmak istiyorsanız, durumlu yük dengeleme kullanmanız gerekir.
   ![Azure MFA Server - Uygulama sunucusu HA](./media/howto-mfaserver-deploy-ha/mfaapp.png)

   > [!NOTE]
   > RPC dinamik bağlantı noktaları kullandığından, RPC'nin kullanabileceği dinamik bağlantı noktaları aralığına kadar güvenlik duvarları açmak önerilmez. MFA uygulama sunucularınız **arasında** bir güvenlik duvarı varsa, MFA Sunucusu'nu alt ve ana sunucular arasındaki çoğaltma trafiği için statik bir bağlantı noktasında iletişim kurmak üzere yapılandırmanız ve güvenlik duvarınızdaki bağlantı noktasını açmanız gerekir. ```HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Positive Networks\PhoneFactor``` Çağrıda ```Pfsvc_ncan_ip_tcp_port``` bulunan bir DWORD kayıt defteri değeri oluşturarak ve değeri kullanılabilir bir statik bağlantı noktasına ayarlayarak statik bağlantı noktasını zorlayabilirsiniz. Bağlantılar her zaman ana ast MFA Sunucuları tarafından başlatılır, statik bağlantı noktası yalnızca ana üzerinde gereklidir, ancak herhangi bir zamanda ana olmak için bir ast teşvik edebilirsiniz, tüm MFA Sunucularında statik bağlantı noktası ayarlamanız gerekir.

2. İki Kullanıcı Portalı/MFA Mobil Uygulama sunucusu (MFA-UP-MAS1 ve MFA-UP-MAS2) yük dengeli **bir** yapılandırmada (mfa.contoso.com) dengelenir. Yapışkan oturumların MFA Kullanıcı Portalı ve Mobil Uygulama Hizmeti'ni dengelemek için bir gereklilik olduğunu hatırlayın.
   ![Azure MFA Server - Kullanıcı Portalı ve Mobil Uygulama Hizmeti HA](./media/howto-mfaserver-deploy-ha/mfaportal.png)
3. ADFS Server çiftliği yük dengeli ve çevre ağında yük dengeli ADFS yakınlık yoluyla Internet'e yayınlanır. Her ADFS Sunucusu, TCP bağlantı noktası 443 üzerinden tek bir yük dengeli URL (mfaapp.contoso.com) kullanarak Azure MFA Sunucularıyla iletişim kurmak için ADFS aracısını kullanır.

## <a name="next-steps"></a>Sonraki adımlar

* [Azure MFA Server'ı yükleme ve yapılandırma](howto-mfaserver-deploy.md)
