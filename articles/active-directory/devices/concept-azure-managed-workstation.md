---
title: Güvenli, Azure tarafından yönetilen iş istasyonlarını anlama - Azure Etkin Dizini
description: Güvenli, Azure tarafından yönetilen iş istasyonları hakkında bilgi edinin ve bunların neden önemli olduğunu anlayın.
services: active-directory
ms.service: active-directory
ms.subservice: devices
ms.topic: conceptual
ms.date: 11/18/2019
ms.author: joflore
author: MicrosoftGuyJFlo
manager: daveba
ms.reviewer: frasim
ms.collection: M365-identity-device-management
ms.openlocfilehash: 05a3a8cf14a591dd3037175e4eed5b5bd8d3096c
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "78672653"
---
# <a name="understand-secure-azure-managed-workstations"></a>Güvenli, Azure tarafından yönetilen iş istasyonlarını anlama

Güvenli, yalıtılmış iş istasyonları, yöneticiler, geliştiriciler ve kritik servis operatörleri gibi hassas rollerin güvenliği açısından kritik öneme dayanır. İstemci iş istasyonu güvenliği tehlikeye girerse, birçok güvenlik denetimi ve güvencesi başarısız olabilir veya etkisiz olabilir.

Bu belge, genellikle ayrıcalıklı erişim iş istasyonu (PAW) olarak bilinen güvenli bir iş istasyonu oluşturmak için neye ihtiyacınız olduğunu açıklar. Makale ayrıca ilk güvenlik denetimleri kurmak için ayrıntılı yönergeler içerir. Bu kılavuz, bulut tabanlı teknolojinin hizmeti nasıl yönetebileceğini açıklar. Windows 10RS5, Microsoft Defender Advanced Threat Protection (ATP), Azure Active Directory ve Microsoft Intune'da tanıtılan güvenlik yeteneklerine dayanır.

> [!NOTE]
> Bu makalede, güvenli bir iş istasyonu kavramı ve önemi açıklanmaktadır. Bu konsepte zaten aşinaysanız ve dağıtıma atlamak istiyorsanız, [Güvenli İş İstasyonu Dağı'nı](howto-azure-managed-workstation.md)ziyaret edin.

## <a name="why-secure-workstation-access-is-important"></a>Güvenli iş istasyonu erişimi neden önemlidir?

Bulut hizmetlerinin hızla benimsenmesi ve her yerden çalışabilme yeteneği yeni bir sömürü yöntemi yaratmıştır. Yöneticilerin çalıştığı aygıtlarda zayıf güvenlik denetimlerinden yararlanarak, saldırganlar ayrıcalıklı kaynaklara erişebilir.

Ayrıcalıklı kötüye kullanım ve tedarik zinciri saldırıları, saldırganların kuruluşları ihlal etmek için kullandıkları ilk beş yöntem arasında yer alıyor. Ayrıca [Verizon Threat raporuna](https://enterprise.verizon.com/resources/reports/dbir/)ve [Güvenlik İstihbarat Raporu'na](https://aka.ms/sir)göre 2018 yılında bildirilen olaylarda en sık tespit edilen ikinci taktiklerdir.

Çoğu saldırgan aşağıdaki adımları izler:

1. Keşif genellikle bir endüstriye özgü bir yol bulmak için.
1. Bilgi toplamak ve düşük değer olarak algılanan bir iş istasyonuna sızmanın en iyi yolunu belirlemek için analiz.
1. Yanal [hareket](https://en.wikipedia.org/wiki/Network_Lateral_Movement)etmek için bir araç aramak için Sebat .
1. Gizli ve hassas verilerin sızması.

Keşif sırasında saldırganlar sık sık düşük riskli veya değersiz görünen cihazlara sızar. Bu hassas aygıtları yanal hareket için bir fırsat bulmak ve yönetim kullanıcıları ve cihazları bulmak için kullanırlar. Ayrıcalıklı kullanıcı rollerine eriştikten sonra, saldırganlar yüksek değerli verileri tanımlar ve bu verileri başarıyla boşaltır.

![Tipik uzlaşma deseni](./media/concept-azure-managed-workstation/typical-timeline.png)

Bu belge, bilgisayar aygıtlarınızın bu tür yanal saldırılara karşı korunmasına yardımcı olabilecek bir çözümü açıklar. Çözüm, yönetimi ve hizmetleri daha az değerli üretkenlik aygıtlarından izole eder ve hassas bulut kaynaklarına erişebilen aygıta sızmadan önce zinciri kırar. Çözüm, Microsoft 365 Kurumsal yığınının bir parçası olan yerel Azure hizmetlerini kullanır:

* Cihaz yönetimi ve güvenli bir uygulama ve URL listesi için uyum
* Cihaz kurulumu, dağıtımı ve yenilemesi için otomatik pilot
* Kullanıcı yönetimi, Koşullu Erişim ve çok faktörlü kimlik doğrulama için Azure AD
* Cihaz durumu ve kullanıcı deneyimi için Windows 10 (geçerli sürüm)
* Bulut tarafından yönetilen uç nokta koruması, algılama ve yanıt için defans ATP
* Yetkilendirmeyi ve kaynaklara tam zamanında (JIT) ayrıcalıklı erişimi yönetmek için Azure AD PIM
* İzleme ve uyarı için Log Analytics ve Sentinel

## <a name="who-benefits-from-a-secure-workstation"></a>Güvenli bir iş istasyonundan kimler yararlanır?

Tüm kullanıcılar ve operatörler güvenli bir iş istasyonu kullanırken yararlanır. Bir bilgisayarı veya aygıtı tehlikeye atan bir saldırgan önbelleğe alınmış tüm hesapların kimliğine bürünebilir. Aygıtta oturum açtıklarında kimlik bilgileri ve belirteçleri de kullanabilirler. Bu risk, yönetim hakları da dahil olmak üzere ayrıcalıklı roller için kullanılan aygıtların güvenliğini sağlamayı önemli kılar. Ayrıcalıklı hesapları olan cihazlar yanal hareket ve ayrıcalık yükseltme saldırıları için hedeflerdir. Bu hesaplar, şunlar gibi çeşitli varlıklar için kullanılabilir:

* Şirket içi veya bulut tabanlı sistemlerin yöneticisi
* Kritik sistemler için geliştirici iş istasyonu
* Yüksek pozlama ile sosyal medya hesabı yöneticisi
* SWIFT ödeme terminali gibi son derece hassas iş istasyonu
* İş istasyonu ticareti sırlarını işleme

Riski azaltmak için, bu hesaplardan yararlanan ayrıcalıklı iş istasyonları için yükseltilmiş güvenlik denetimleri uygulamanız gerekir. Daha fazla bilgi için [Azure Etkin Dizin özelliği dağıtım kılavuzuna](../fundamentals/active-directory-deployment-checklist-p2.md), [Office 365 yol haritasına](https://aka.ms/o365secroadmap)ve Ayrıcalıklı Erişimi Güvence Altına Alma [yol haritasına](https://aka.ms/sparoadmap)bakın.

## <a name="why-use-dedicated-workstations"></a>Neden özel iş istasyonları kullanıyorsun?

Varolan bir aygıta güvenlik eklemek mümkün olsa da, güvenli bir temelle başlamak daha iyidir. Kuruluşunuzun yüksek güvenlik düzeyini korumak için en iyi konuma getirmek için, güvenli olduğunu bildiğiniz bir aygıtla başlayın ve bilinen bir dizi güvenlik denetimini uygulayın.

E-posta ve web'de gezinme yoluyla giderek artan sayıda saldırı vektörü, bir aygıta güvenilenden emin olmayı giderek zorlaştırır. Bu kılavuz, özel bir iş istasyonunun standart üretkenlikten, taramadan ve e-postadan yalıtılmış olduğunu varsayar. Üretkenliğin, web'de gezinmenin ve e-postanın bir aygıttan kaldırılması üretkenliği olumsuz etkileyebilir. Ancak, bu koruma genellikle iş görevlerinin açıkça gerektirmediği ve bir güvenlik olayı riskinin yüksek olduğu senaryolar için kabul edilebilir.

> [!NOTE]
> Burada web'de gezinme, yüksek riskli bir etkinlik olabilecek rasgele web sitelerine genel erişim anlamına gelir. Bu tür tarama, Azure, Office 365, diğer bulut sağlayıcıları ve SaaS uygulamaları gibi hizmetler için az sayıda tanınmış yönetim web sitesine erişmek için bir web tarayıcısı kullanmaktan belirgin şekilde farklıdır.

Kapsama stratejileri, saldırganı hassas varlıklara erişmekten caydıran denetimlerin sayısını ve türünü artırarak güvenliği sıkılaştırır. Bu makalede açıklanan model katmanlı bir ayrıcalık tasarımı kullanır ve yönetim ayrıcalıklarını belirli aygıtlarla sınırlandırıyor.

## <a name="supply-chain-management"></a>Tedarik zinciri yönetimi

Güvenli bir iş istasyonu için gerekli olan, 'güven kökü' adı verilen güvenilir bir iş istasyonu kullandığınız bir tedarik zinciri çözümüdür. Güven donanımı kökü seçiminde göz önünde bulundurulması gereken teknoloji, modern dizüstü bilgisayarlarda bulunan aşağıdaki teknolojileri içermelidir: 

* [Güvenilir Platform Modülü (TPM) 2.0](/windows-hardware/design/device-experiences/oem-tpm)
* [BitLocker Sürücü Şifrelemesi](/windows-hardware/design/device-experiences/oem-bitlocker)
* [UEFI Güvenli Önyükleme](/windows-hardware/design/device-experiences/oem-secure-boot)
* [Windows Update aracılığıyla dağıtılan sürücüler ve Firmware](/windows-hardware/drivers/dashboard/understanding-windows-update-automatic-and-optional-rules-for-driver-distribution)
* [Sanallaştırma ve HVCI Etkin](/windows-hardware/design/device-experiences/oem-vbs)
* [Sürücüler ve Uygulamalar HVCI-Hazır](/windows-hardware/test/hlk/testref/driver-compatibility-with-device-guard)
* [Windows Hello](/windows-hardware/design/device-experiences/windows-hello-biometric-requirements)
* [DMA I/O Koruması](/windows/security/information-protection/kernel-dma-protection-for-thunderbolt)
* [Sistem Koruması](/windows/security/threat-protection/windows-defender-system-guard/system-guard-how-hardware-based-root-of-trust-helps-protect-windows)
* [Modern Bekleme](/windows-hardware/design/device-experiences/modern-standby)

Bu çözüm için, güven kökü, modern teknik gereksinimleri karşılayan donanıma sahip [Microsoft Autopilot](/windows/deployment/windows-autopilot/windows-autopilot) teknolojisi kullanılarak dağıtılacaktır. İş istasyonunu güvence altına almak için Otomatik Pilot, Microsoft OEM için optimize edilmiş Windows 10 aygıtları kullanılabilir. Bu aygıtlar üreticiden bilinen iyi bir durumda gelir. Otomatik pilot, güvenli olmayan bir aygıtı yeniden görüntülemek yerine, Windows aygıtını "işletmeye hazır" bir duruma dönüştürebilir. Ayarları ve ilkeleri uygular, uygulamaları yükler ve hatta Windows 10'un sürümünü değiştirir. Örneğin, Otomatik Pilot, gelişmiş özellikleri kullanabilmesi için aygıtın Windows yüklemesini Windows 10 Pro'dan Windows 10 Enterprise'a değiştirebilir.

![Güvenli iş istasyonu düzeyleri](./media/concept-azure-managed-workstation/supplychain.png)

## <a name="device-roles-and-profiles"></a>Cihaz rolleri ve profilleri

Bu kılavuz, kullanıcılar, geliştiriciler ve BT personeli için daha güvenli çözümler oluşturmanıza yardımcı olabilecek çeşitli güvenlik profillerine ve rollerine başvurur. Bu profiller, gelişmiş veya güvenli bir iş istasyonundan yararlanabilecek yaygın kullanıcılar için kullanılabilirlik ve riskleri dengeler. Burada sağlanan ayarlar yapılandırmaları endüstritarafından kabul edilen standartlara dayanmaktadır. Bu kılavuz, Windows 10'un nasıl sertleşip aygıt veya kullanıcı uzlaşmasıyla ilişkili riskleri azalttığını gösterir. Modern donanım teknolojisi ve güven aygıtı kökü yararlanmak için, **Yüksek Güvenlik** profilinden başlayarak etkinleştirilen Device [Health Attestation'ı](https://techcommunity.microsoft.com/t5/Intune-Customer-Success/Support-Tip-Using-Device-Health-Attestation-Settings-as-Part-of/ba-p/282643)kullanacağız. Bu özellik, saldırganların bir aygıtın erken önyükleme sırasında kalıcı olmamasını sağlamak için kullanılabilir. Bunu, güvenlik özelliklerini ve risklerini yönetmenize yardımcı olmak için ilke ve teknolojiyi kullanarak yapar.
![Güvenli iş istasyonu düzeyleri](./media/concept-azure-managed-workstation/seccon-levels.png)

* **Temel Güvenlik** – Yönetilen, standart bir iş istasyonu, çoğu ev ve küçük işletme kullanımı için iyi bir başlangıç noktası sağlar. Bu aygıtlar Azure AD'ye kaydedilir ve Intune ile yönetilir. Bu profil, kullanıcıların herhangi bir uygulamayı çalıştırıp herhangi bir web sitesine göz atmasını sağlar. [Microsoft Defender](https://www.microsoft.com/windows/comprehensive-security) gibi kötü amaçlı yazılımdan koruma çözümü etkinleştirilmelidir.

* **Gelişmiş Güvenlik** – Bu giriş seviyesi, korumalı çözüm ev kullanıcıları, küçük işletme kullanıcıları ve genel geliştiriciler için iyidir.

   Geliştirilmiş iş istasyonu, düşük güvenlik profilinin güvenliğini artırmak için ilke tabanlı bir yoldur. Aynı zamanda e-posta ve web tarama gibi verimlilik araçlarını kullanarak müşteri verileri ile çalışmak için güvenli bir araç sağlar. Kullanıcı davranışı ve profil kullanımı için gelişmiş bir iş istasyonunu izlemek için denetim ilkelerini ve Intune'u kullanabilirsiniz. Gelişmiş iş istasyonu profilini Windows10 (1809) komut dosyasıyla dağıtıyorsunuz ve gelişmiş kötü amaçlı yazılım korumasından yararlanArak [Gelişmiş Tehdit Koruması (ATP) kullanıyor.](/windows/security/threat-protection/microsoft-defender-atp/microsoft-defender-advanced-threat-protection)

* **Yüksek Güvenlik** – Bir iş istasyonunun saldırı yüzeyini azaltmanın en etkili yolu, iş istasyonunu kendi kendine yönetme yeteneğini ortadan kaldırmaktır. Yerel yönetim haklarının kaldırılması güvenliği artıran bir adımdır, ancak yanlış uygulandığında üretkenliği etkileyebilir. Yüksek güvenlik profili, önemli bir değişiklikle gelişmiş güvenlik profiline dayanıyor: yerel yöneticinin kaldırılması. Bu profil yüksek profilli kullanıcılar için tasarlanmıştır: yöneticiler, bordro ve hassas veri kullanıcıları, hizmetler ve süreçler için onaylayıcılar.

   Yüksek güvenlikli kullanıcı, kullanımı kolay bir deneyimle e-posta ve web'de gezinme gibi etkinlikleri yaparken daha kontrollü bir ortam talep eder. Kullanıcılar, çerezler, sık kullanılanlar ve diğer kısayollar gibi özelliklerin çalışmasını bekler. Ancak, bu kullanıcılar aygıtlarını değiştirme veya hata ayıklama özelliğine ihtiyaç duymayabilir. Ayrıca sürücüleri yüklemelerine gerek yoktur. Yüksek güvenlik profili Yüksek Güvenlik - Windows10 (1809) komut dosyası kullanılarak dağıtılır.

* **Özel -** Saldırganlar, saldırganların ilgi sistemlerini değiştirebildikleri için geliştiricileri ve BT yöneticilerini hedef alır. Özel leştirilmiş iş istasyonu, yerel uygulamaları yöneterek ve web sitelerini sınırlayarak yüksek güvenlikli iş istasyonunun ilkelerini genişletir. ActiveX, Java, tarayıcı eklentileri ve diğer Windows denetimleri gibi yüksek riskli üretkenlik özelliklerini de kısıtlar. Bu profili DeviceConfiguration_NCSC - Windows10 (1803) SecurityBaseline komut dosyasıyla dağıtın.

* **Güvenli** – İdari bir hesabı tehlikeye atan bir saldırgan, veri hırsızlığı, veri değişikliği veya hizmet kesintisi nedeniyle önemli ölçüde iş hasarına neden olabilir. Bu sertleştirilmiş durumda, iş istasyonu, yerel uygulama yönetiminin doğrudan denetimini kısıtlayan tüm güvenlik denetimlerini ve ilkelerini sağlar. Güvenli bir iş istasyonunun üretkenlik araçları yoktur, bu nedenle cihazın ödün vermek daha zordur. Kimlik avı saldırıları için en yaygın vektörü engeller: e-posta ve sosyal medya. Güvenli iş istasyonu Güvenli İş İstasyonu - Windows10 (1809) SecurityBaseline komut dosyası ile dağıtılabilir.

   ![Güvenli iş istasyonu](./media/concept-azure-managed-workstation/secure-workstation.png)

   Güvenli bir iş istasyonu, yöneticiye açık uygulama denetimi ve uygulama koruması olan sertleştirilmiş bir iş istasyonu sağlar. İş istasyonu, ev sahibini kötü amaçlı davranışlardan korumak için kimlik bilgisi koruması, aygıt koruması ve kullanım koruyucusu kullanır. Tüm yerel diskler de BitLocker ile şifrelenir.

* **Yalıtılmış** – Bu özel, çevrimdışı senaryo spektrumun aşırı ucunu temsil eder. Bu servis talebi için yükleme komut dosyası sağlanmadı. Desteklenmeyen veya yamalanmamış eski bir işletim sistemi gerektiren iş açısından kritik bir işlevi yönetmeniz gerekebilir. Örneğin, yüksek değerli bir üretim hattı veya yaşam destek sistemi. Güvenlik kritik olduğundan ve bulut hizmetleri kullanılamadığından, bu bilgisayarları el ile veya Gelişmiş Güvenlik Yöneticisi Ortamı (ESAE) gibi yalıtılmış bir Active Directory orman mimarisiyle yönetebilir ve güncelleştirebilirsiniz. Bu durumlarda, temel Intune ve ATP sağlık kontrolleri dışında tüm erişimi kaldırmayı düşünün.

   * [Intune ağ iletişim gereksinimi](/intune/network-bandwidth-use)
   * [ATP ağ iletişimi gereksinimi](/azure-advanced-threat-protection/configure-proxy)

## <a name="next-steps"></a>Sonraki adımlar

[Azure tarafından yönetilen güvenli bir iş istasyonu dağıtın.](howto-azure-managed-workstation.md)
