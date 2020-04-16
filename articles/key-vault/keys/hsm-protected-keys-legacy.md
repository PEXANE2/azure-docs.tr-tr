---
title: Azure Key Vault - Azure Key Vault için HSM korumalı anahtarlar oluşturma ve aktarma | Microsoft Dokümanlar
description: Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlamanıza, oluşturmanıza ve sonra aktarmanıza yardımcı olmak için bu makaleyi kullanın. Ayrıca BYOK olarak bilinen veya kendi anahtarınızı getirmek.
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: keys
ms.topic: conceptual
ms.date: 02/12/2019
ms.author: mbaldwin
ms.openlocfilehash: 28f066668d580f16d831371f2d02a5abcc0e84b3
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81429739"
---
# <a name="import-hsm-protected-keys-for-key-vault-legacy"></a>Key Vault için HSM korumalı anahtarları alma (eski)

[!INCLUDE [updated-for-az](../../../includes/updated-for-az.md)]

Ek güvence için, Azure Key Vault'u kullandığınızda, HSM sınırını hiç terk çıkmayan donanım güvenlik modüllerinde (HSM) anahtarlar içe aktarabilir veya oluşturabilirsiniz. Bu senaryo genellikle *kendi anahtarını getir* veya KAG olarak adlandırılır. Azure Key Vault, anahtarlarınızı korumak için nCipher nShield HSM ailesini (FIPS 140-2 Düzey 2 doğrulandı) kullanır.

Azure Key Vault ile kullanmak üzere kendi HSM korumalı anahtarlarınızı planlamanıza, oluşturmanıza ve aktarmanıza yardımcı olması için bu konuyla ilgili bilgileri kullanın.

Bu işlev Azure China için kullanılamaz.

> [!NOTE]
> Azure Anahtar Kasası hakkında daha fazla bilgi için Azure [Anahtar Kasası nedir?](../general/overview.md)  
> HSM korumalı anahtarlar için önemli bir kasa oluşturmayı içeren bir başlangıç eğitimi için [bkz.](../general/overview.md)

HSM korumalı bir anahtarın Internet üzerinden üretilmesi ve aktarılması hakkında daha fazla bilgi:

* Anahtarı çevrimdışı bir iş istasyonundan oluşturursunuz, bu da saldırı yüzeyini azaltır.
* Anahtar, Azure Key Vault HSM'lerine aktarılına kadar şifreli kalan bir Anahtar Değişim Anahtarı (KEK) ile şifrelenir. Anahtarınızın yalnızca şifreli sürümü orijinal iş istasyonundan ayrılır.
* Araç seti, anahtarınızı Azure Anahtar Kasası güvenlik dünyasına bağlayan kiracı anahtarınızda özellikleri ayarlar. Azure Key Vault HSM'leri anahtarınızı aldıktan ve şifresini çözdükten sonra, yalnızca bu HSM'ler bu anahtarı kullanabilir. Anahtarınız dışa aktarılamaz. Bu bağlama nCipher HSM tarafından uygulanır.
* Anahtarınızı şifrelemek için kullanılan Anahtar Değişim Anahtarı (KEK), Azure Key Vault HSM'leri içinde oluşturulur ve dışa aktarılamaz. HSM'ler, KEK’in HSM'lerin dışında net bir sürümü olmamasını zorlar. Buna ek olarak, araç seti, KEK'in ihraç edilemeyeceğini ve nCipher tarafından üretilen orijinal bir HSM içinde üretildiğini nCipher'dan gelen bir attestation içerir.
* Araç seti, NCipher tarafından üretilen gerçek bir HSM üzerinde Azure Key Vault güvenlik dünyasının da oluşturulduğunu nCipher'in belirteci içerir. Bu attestation, Microsoft'un orijinal donanım kullandığını size kanıtlıyor.
* Microsoft, her coğrafi bölgede ayrı KEK'ler ve ayrı Güvenlik Dünyaları kullanır. Bu ayırma, anahtarınızın yalnızca şifrelediğiniz bölgedeki veri merkezlerinde kullanılabilmesini sağlar. Örneğin, Avrupalı bir müşterinin anahtarı Kuzey Amerika veya Asya'daki veri merkezlerinde kullanılamaz.

## <a name="more-information-about-ncipher-hsms-and-microsoft-services"></a>nCipher HSM'ler ve Microsoft hizmetleri hakkında daha fazla bilgi

Bir Emanet Datacard şirketi olan nCipher Security, iş açısından kritik bilgi ve uygulamalarına güven, bütünlük ve kontrol sağlayarak dünyanın önde gelen kuruluşlarını güçlendiren genel amaçlı HSM pazarında liderdir. nCipher'ın şifreleme çözümleri, bulut, IoT, blockchain, dijital ödemeler gibi gelişmekte olan teknolojileri güvence altına almakta ve küresel kuruluşların hassas verilerine, ağ iletişimlerine ve kurumsal altyapılarına yönelik tehditlere karşı korumak için bugün de güvenen kanıtlanmış teknolojiyi kullanarak yeni uyumluluk görevlerini yerine getirmeye yardımcı olmaktadır. nCipher, iş açısından kritik uygulamalar için güven sağlayarak verilerin bütünlüğünü sağlar ve müşterileri her zaman tam kontrol altına alar.

Microsoft, HSM'ler için son teknolojiyi geliştirmek için nCipher Security ile işbirliği yaptı. Yapılan geliştirmeler, anahtarlarınızın denetimini elden bırakmadan, barındırılan hizmetlere ait tipik avantajlardan yararlanmanıza olanak tanımaktadır. Daha açık belirtilirse, bu geliştirmeler sonrasında HSM’lerin yönetimini sizin yerinize Microsoft yürütebilmektedir. Azure Key Vault, bulut hizmeti olarak kuruluşunuzun kullanım artışlarını karşılamak için kısa sürede büyür. Aynı zamanda anahtarınız Microsoft'un HSM'lerinde korunur: Anahtarı oluşturduğunuz ve Microsoft'un HSM'lerine aktardığınız için anahtar yaşam döngüsü üzerindeki denetimi siz de korursunuz.

## <a name="implementing-bring-your-own-key-byok-for-azure-key-vault"></a>Azure Key Vault için kendi anahtarını getir (KAG) özelliğini uygulama

Kendi HSM korumalı anahtarınızı oluşturacak ve ardından kendi anahtar (BYOK) senaryonuzu getir'e aktaracaksanız aşağıdaki bilgileri ve yordamları kullanın.

## <a name="prerequisites-for-byok"></a>BYOK için önkoşullar

Azure Anahtar Kasası için kendi anahtarınızı (BYOK) getirmek için ön koşulların listesi için aşağıdaki tabloya bakın.

| Gereksinim | Daha fazla bilgi |
| --- | --- |
| Azure aboneliği |Azure Anahtar Kasası oluşturmak için Azure aboneliğine ihtiyacınız var: [Ücretsiz deneme sürümü için kaydolun](https://azure.microsoft.com/pricing/free-trial/) |
| HSM korumalı anahtarları desteklemek için Azure Key Vault Premium hizmet katmanı |Azure Key Vault'un hizmet katmanları ve yetenekleri hakkında daha fazla bilgi için [Azure Key Vault Fiyatlandırma](https://azure.microsoft.com/pricing/details/key-vault/) web sitesine bakın. |
| nCipher nShield HSM'ler, akıllı kartlar ve destek yazılımı |Bir nCipher Donanım Güvenlik Modülüne ve nCipher nShield HSM'lerin temel operasyonel bilgisine erişebilmelisin. Uyumlu modellerin listesi için [nCipher nShield Donanım Güvenlik Modülü'ne](https://www.ncipher.com/products/key-management/cloud-microsoft-azure/how-to-buy) bakın veya yoksa Bir HSM satın alın. |
| Aşağıdaki donanım ve yazılım:<ol><li>Windows 7 ve en az sürüm 11.50 nShield yazılımı nCipher nShield yazılımı bir çevrimdışı x64 iş istasyonu.<br/><br/>Bu iş istasyonu Windows 7 çalıştırıyorsa, [Microsoft .NET Framework 4.5 sürümünü yüklemeniz](https://download.microsoft.com/download/b/a/4/ba4a7e71-2906-4b2d-a0e1-80cf16844f5f/dotnetfx45_full_x86_x64.exe) gerekir.</li><li>Internet'e bağlı olan ve windows 7 ve [Azure PowerShell](/powershell/azure/overview?view=azps-1.2.0) **minimum sürüm 1.1.0** yüklü minimum Windows işletim sistemine sahip bir iş istasyonu.</li><li>En az 16 MB boş alana sahip bir USB sürücü veya diğer taşınabilir depolama aygıtı.</li></ol> |Güvenlik nedenleriyle ilk iş istasyonunun bir ağa bağlı olmaması önerilir. Ancak, bu öneri programlı olarak zorlanmaz.<br/><br/>İzleyen yönergelerde, bu iş istasyonu bağlantısı kesilen iş istasyonu olarak adlandırılır.</p></blockquote><br/>Ayrıca, kiracı anahtarınız bir üretim ağı içinse, araç kümesini indirmek ve kiracı anahtarını yüklemek için ikinci ve ayrı bir iş istasyonu kullanmanızı öneririz. Ancak test amacıyla, birincisi ile aynı iş istasyonunu kullanabilirsiniz.<br/><br/>İzleyen yönergelerde, bu ikinci iş istasyonu Internet'e bağlı iş istasyonu olarak adlandırılır.</p></blockquote><br/> |

## <a name="generate-and-transfer-your-key-to-azure-key-vault-hsm"></a>Anahtarınızı oluşturun ve Azure Key Vault HSM'ye aktarın

Anahtarınızı oluşturmak ve bir Azure Key Vault HSM'ye aktarmak için aşağıdaki beş adımı kullanırsınız:

* [Adım 1: Internet'e bağlı iş istasyonunuzu hazırlayın](#step-1-prepare-your-internet-connected-workstation)
* [Adım 2: Bağlantısı kesilen iş istasyonunu hazırlayın](#step-2-prepare-your-disconnected-workstation)
* [Adım 3: Anahtarınızı oluşturun](#step-3-generate-your-key)
* [Adım 4: Kiracı anahtarınızı aktarım için hazırlama](#step-4-prepare-your-key-for-transfer)
* [5. Adım: Anahtarınızı Azure Anahtar Kasası’na aktarma](#step-5-transfer-your-key-to-azure-key-vault)

## <a name="step-1-prepare-your-internet-connected-workstation"></a>Adım 1: Internet'e bağlı iş istasyonunuzu hazırlayın

Bu ilk adım için, Internet'e bağlı iş istasyonunuzda aşağıdaki yordamları yapın.

### <a name="step-11-install-azure-powershell"></a>Adım 1.1: Azure PowerShell'i yükleyin

Internet'e bağlı iş istasyonundan, Azure Key Vault'u yönetmek için cmdlet'leri içeren Azure PowerShell modüllerini indirin ve yükleyin. Yükleme yönergeleri için [Azure PowerShell'in nasıl yüklenir ve yapılandırılabildiğini](/powershell/azure/overview)öğrenin.

### <a name="step-12-get-your-azure-subscription-id"></a>Adım 1.2: Azure abonelik kimliğinizi alın

Aşağıdaki komutu kullanarak bir Azure PowerShell oturumu başlatın ve Azure hesabınızda oturum açın:

```Powershell
   Connect-AzAccount
```
Açılır tarayıcı penceresinde Azure hesabı kullanıcı adınızı ve parolanızı girin. Ardından [Get-AzSubscription](/powershell/module/az.accounts/get-azsubscription) komutunu kullanın:

```powershell
   Get-AzSubscription
```
Çıktıdan Azure Key Vault için kullanacağınız aboneliğin kimliğini bulun. Bu abonelik kimliğine daha sonra ihtiyacınız olacaktır.

Azure PowerShell penceresini kapatmayın.

### <a name="step-13-download-the-byok-toolset-for-azure-key-vault"></a>Adım 1.3: Azure Key Vault için BYOK araç setini indirin

Microsoft İndirme Merkezi'ne gidin ve coğrafi bölgeniz veya Azure örneğiniz için [Azure Key Vault BYOK araç kümesini indirin.](https://www.microsoft.com/download/details.aspx?id=45345) İndirmek için paket adını ve ilgili SHA-256 paket karmasını tanımlamak için aşağıdaki bilgileri kullanın:

---
**Amerika Birleşik Devletleri:**

KeyVault-BYOK-Tools-Amerika Birleşik Devletleri.zip

2E8C00320400430106366A4E8C67B79015524E4EC24A2D3A6DC513CA1823B0D4

---
**Avrupa:**

KeyVault-BYOK-Tools-Europe.zip

9AAA63E2E7F20CF9BB62485868754203721D2F88D300910634A32DFA1FB19E4A

---
**Asya:**

KeyVault-BYOK-Araçlar-AsiaPacific.zip

4BC14059BF0FEC562CA927AF621DF665328F8A13616F44C977388EC7121EF6B5

---
**Latin Amerika:**

KeyVault-BYOK-Araçlar-LatinAmerica.zip

E7DFAFF5779AFE1B9732C30D6FD80C4D03756642F25A538922D1B01A4FACB619

---
**Japonya:**

KeyVault-BYOK-Araçlar-Japonya.zip

3933C13CC6DC06651295ADC482B027AF923A76F1F6BF98B4D4B8E94632DEC7DF

---
**Kore:**

KeyVault-BYOK-Araçlar-Kore.zip

71AB6BCFE06950097C8C18D532A9184BEF52A74BB944B8610DDA05344ED136F

---
**Güney Afrika:**

KeyVault-BYOK-Araçlar-SouthAfrica.zip

C41060C5C0170AAAAD896DA732E31433D14CB9FC83AC3C67766F46D98620784A

---
**Uçan:**

KeyVault-BYOK-Araçlar-BAE.zip

FADE80210B06662A0913EA411DAB977929248C65F365FD953BB9F241D5FC0D3

---
**Avustralya:**

KeyVault-BYOK-Tools-Australia.zip

CD0FB7365053DEF8C3516D7C92D203C64A3D3E2452A02523EEB166901C40A

---
[**Azure Yönetimi:**](https://azure.microsoft.com/features/gov/)

KeyVault-BYOK-Araçlar-USGovCloud.zip

F8DB2FC914A7360650922391D9AA79FF030FD3048B5795EC83ADC59DB018621A

---
**ABD Hükümeti DOD:**

KeyVault-BYOK-Araçlar-USGovernmentDoD.zip

A79DD8C6DFFF1B00B91D1812280207A205442B3DDF861B79B8B991BB55C35263

---
**Kanada:**

KeyVault-BYOK-Araçlar-Canada.zip

61BE1A1F80AC79912A42DEBBCC42CF87C88C2CE249E271934630885799717C7B

---
**Almanya:**

KeyVault-BYOK-Tools-Germany.zip

5385E615880AAFC02AFD9841F7BADD025D7EE819894AA29ED3C71C3F844C45D6

---
**Almanya Kamu:**

KeyVault-BYOK-Araçlar-Almanya-Public.zip

54534936D00C99C8117DB724C34A5E50FD204CFCBD75C78972B785865364A29

---
**Hindistan:**

KeyVault-BYOK-Araçlar-India.zip

49EDCEB3091CF1DF7B156D5B495A4ADE1CFBA77641134F61B0E0940121C436C8

---
**Fransa:**

KeyVault-BYOK-Araçlar-France.zip

5C9D1F3E4125B0C09E9F60897C9AE3A8B4CB0E7D13A14F3EDBD280128F8FE7DF

---
**Birleşik Krallık:**

KeyVault-BYOK-Araçlar-Birleşik Krallık.zip

432746BD0D3176B708672CCFF19D6144FCAA9E5EB29BB056489D3782B3B80849

---
**İsviçre:**

KeyVault-BYOK-Tools-Switzerland.zip

88CF8D39899E26D456D4E0BC57E5C94913ABF1D73A89013FCE3BBD9599AD2FE9

---


Azure PowerShell oturumunuzdan indirilmiş BYOK araç setinizin bütünlüğünü doğrulamak için [Get-FileHash](https://technet.microsoft.com/library/dn520872.aspx) cmdlet'i kullanın.

   ```powershell
   Get-FileHash KeyVault-BYOK-Tools-*.zip
   ```

Araç seti şunları içerir:

* **BYOK-KEK-pkg-ile** başlayan bir adı olan bir Anahtar Değişim Anahtarı (KEK) paketi.
* **BYOK-SecurityWorld-pkg-ile** başlayan bir adı olan bir Güvenlik Dünya paketi.
* verifykeypackage.py adında bir python komut **dosyası.**
* **KeyTransferRemote.exe** ve ilişkili DL'ler adlı komut satırı yürütülebilir dosya.
* **vcredist_x64.exe** adlı Görsel C++ Yeniden Dağıtılabilir Paket.

Paketi bir USB sürücüye veya başka bir taşınabilir depolama cihazına kopyalayın.

## <a name="step-2-prepare-your-disconnected-workstation"></a>Adım 2: Bağlantısı kesilen iş istasyonunu hazırlayın

Bu ikinci adım için, bir ağa bağlı olmayan iş istasyonunda (Internet veya dahili ağınızda) aşağıdaki yordamları yapın.

### <a name="step-21-prepare-the-disconnected-workstation-with-ncipher-nshield-hsm"></a>Adım 2.1: nCipher nShield HSM ile bağlantısı kesilen iş istasyonunu hazırlayın

nCipher destek yazılımını bir Windows bilgisayarına yükleyin ve ardından bu bilgisayara bir nCipher nShield HSM takın.

NCipher araçlarının yolunuza çıktığından emin olun (%**nfast_home\bin).** Örneğin, aşağıdakini yazın:

  ```cmd
  set PATH=%PATH%;"%nfast_home%\bin"
  ```

Daha fazla bilgi için nShield HSM ile birlikte verilen kullanım kılavuzuna bakın.

### <a name="step-22-install-the-byok-toolset-on-the-disconnected-workstation"></a>Adım 2.2: BYOK araç kümesini bağlantısı kesilen iş istasyonuna yükleme

USB sürücü veya başka bir taşınabilir depolama cihazından BYOK araç takımı paketini kopyalayın ve ardından aşağıdakileri yapın:

1. İndirilen paketteki dosyaları herhangi bir klasöre ayıklayın.
2. Bu klasörden vcredist_x64.exe dosyasını çalıştırın.
3. Visual Studio 2013 için Visual C++ çalışma zamanı bileşenlerini yüklemek için yönergeleri izleyin.

## <a name="step-3-generate-your-key"></a>Adım 3: Anahtarınızı oluşturun

Bu üçüncü adım için, bağlantısı kesilen iş istasyonu nda aşağıdaki yordamları yapın. Bu adımı tamamlamak için HSM'nizin başlatma modunda olması gerekir. 

### <a name="step-31-change-the-hsm-mode-to-i"></a>Adım 3.1: HSM modunu 'I' olarak değiştirme

Modunu değiştirmek için nCipher nShield Edge kullanıyorsanız: 1. Gerekli modu vurgulamak için Mod düğmesini kullanın. 2. Birkaç saniye içinde, Birkaç saniye için Temizle düğmesine basın ve basılı tutun. Mod değişirse, yeni modun LED'i yanıp sönmeyi durdurur ve yanmaya devam eder. Durum LED'i birkaç saniye boyunca düzensiz yanıp söner ve aygıt hazır olduğunda düzenli olarak yanıp söner. Aksi takdirde, aygıt geçerli modda kalır ve uygun modDA LED yanar.

### <a name="step-32-create-a-security-world"></a>Adım 3.2: Bir güvenlik dünyası oluşturun

Bir komut istemi başlatın ve nCipher yeni dünya programını çalıştırın.

   ```cmd
    new-world.exe --initialize --cipher-suite=DLf3072s256mRijndael --module=1 --acs-quorum=2/3
   ```

Bu program, C:\ProgramData\nCipher\Key Management Data\local klasörüne karşılık gelen %NFAST_KMDATA%\local\world klasöründe bir **Güvenlik Dünyası** dosyası oluşturur. Çoğunluk için farklı değerler kullanabilirsiniz, ancak bizim örneğimizde, her biri için üç boş kart ve pin girmeniz istenir. Sonra, herhangi bir iki kart güvenlik dünyasına tam erişim verir. Bu kartlar, yeni güvenlik dünyası için **Yönetici Kart Seti** haline gelir.

> [!NOTE]
> HSM'niz yeni cypher paketi DLf3072s256mRijndael'i desteklemiyorsa, -cipher-suite= DLf3072s256mRijndael ile -cipher-suite=DLf1024s160mRijndael'i değiştirebilirsiniz
> 
> Güvenlik dünyası new-world.exe ile oluşturulan bu nCipher yazılım sürümü 12.50 ile gemiler bu BYOK prosedürü ile uyumlu değildir. Kullanılabilecek iki seçenek vardır:
> 1) Yeni bir güvenlik dünyası oluşturmak için nCipher yazılım sürümünü 12.40.2'ye düşürün.
> 2) NCipher desteğine başvurun ve bu BYOK prosedürüyle uyumlu olan new-world.exe'nin 12.40.2 sürümünü kullanmanıza olanak tanıyan 12.50 yazılım sürümü için bir düzeltme sağlamalarını isteyin.

Ardından şunları yapın:

* Dünya dosyasının yedeğini alın. Dünya dosyasını, Yönetici Kartlarını ve bunların PIN’lerini güvenceye alın ve koruyun ve bir karta tek bir kişiden fazlasının erişemediğinden emin olun.

### <a name="step-33-change-the-hsm-mode-to-o"></a>Adım 3.3: HSM modunu 'O' olarak değiştirme

Modunu değiştirmek için nCipher nShield Edge kullanıyorsanız: 1. Gerekli modu vurgulamak için Mod düğmesini kullanın. 2. Birkaç saniye içinde, Birkaç saniye için Temizle düğmesine basın ve basılı tutun. Mod değişirse, yeni modun LED'i yanıp sönmeyi durdurur ve yanmaya devam eder. Durum LED'i birkaç saniye boyunca düzensiz yanıp söner ve aygıt hazır olduğunda düzenli olarak yanıp söner. Aksi takdirde, aygıt geçerli modda kalır ve uygun modDA LED yanar.

### <a name="step-34-validate-the-downloaded-package"></a>Adım 3.4: İndirilen paketi doğrulama

Bu adım isteğe bağlıdır ancak aşağıdakileri doğrulayabilmeniz adına önerilir:

* Araç setinde yer alan Anahtar Değişim Anahtarı, gerçek bir nCipher nShield HSM'den oluşturulmuştur.
* Araç kümesine dahil olan Güvenlik Dünyası'nın karması gerçek bir nCipher nShield HSM'de oluşturulmuştur.
* Anahtar Değişim Anahtarı dışarı aktarılamaz.

> [!NOTE]
> İndirilen paketi doğrulamak için HSM'nin bağlı, açık ve üzerinde bir güvenlik dünyası olması gerekir (örneğin, yeni oluşturduğunuz paket gibi).

İndirilen paketi doğrulamak için:

1. Coğrafi bölgenize veya Azure örneğinize bağlı olarak aşağıdakilerden birini yazarak verifykeypackage.py komut dosyasını çalıştırın:

   * Kuzey Amerika için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-NA-1 -w BYOK-SecurityWorld-pkg-NA-1
   * Avrupa için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-EU-1 -w BYOK-SecurityWorld-pkg-EU-1
   * Asya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AP-1 -w BYOK-SecurityWorld-pkg-AP-1
   * Latin Amerika için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-LATAM-1 -w BYOK-SecurityWorld-pkg-LATAM-1
   * Japonya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-JPN-1 -w BYOK-SecurityWorld-pkg-JPN-1
   * Kore için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-KOREA-1 -w BYOK-SecurityWorld-pkg-KOREA-1
   * Güney Afrika için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-SA-1 -w BYOK-SecurityWorld-pkg-SA-1
   * BAE için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-UAE-1 -w BYOK-SecurityWorld-pkg-UAE-1
   * Avustralya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-AUS-1 -w BYOK-SecurityWorld-pkg-AUS-1
   * AZURE'un ABD devlet örneğini kullanan [Azure Hükümeti](https://azure.microsoft.com/features/gov/)için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USGOV-1 -w BYOK-SecurityWorld-pkg-USGOV-1
   * ABD Hükümeti DOD için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-USDOD-1 -w BYOK-SecurityWorld-pkg-USDOD-1
   * Kanada için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-CANADA-1 -w BYOK-SecurityWorld-pkg-CANADA-1
   * Almanya için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Almanya Kamu için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-GERMANY-1 -w BYOK-SecurityWorld-pkg-GERMANY-1
   * Hindistan için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-INDIA-1 -w BYOK-SecurityWorld-pkg-INDIA-1
   * Fransa için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-FRANCE-1 -w BYOK-SecurityWorld-pkg-FRANCE-1
   * Birleşik Krallık için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-UK-1 -w BYOK-SecurityWorld-pkg-UK-1
   * İsviçre için:

         "%nfast_home%\python\bin\python" verifykeypackage.py -k BYOK-KEK-pkg-SUI-1 -w BYOK-SecurityWorld-pkg-SUI-1

     > [!TIP]
     > nCipher nShield yazılımı % NFAST_HOME\python\bin python içerir
     >
     >
2. Başarılı doğrulamayı gösteren aşağıdakileri gördüğünüzü doğrulayın: **Sonuç: BAŞARI**

Bu komut dosyası, nShield kök anahtarına kadar imzalayan zinciri doğrular. Bu kök anahtarın karması, komut dosyasında tümleşiktir ve değeri **59178a47 de508c3f 291277ee 184f46c4 f1d9c639** olmalıdır. Ayrıca [nCipher web sitesini](https://www.ncipher.com/products/key-management/cloud-microsoft-azure/validation)ziyaret ederek bu değeri ayrıca onaylayabilirsiniz.

Artık yeni bir anahtar oluşturmaya hazırsınız.

### <a name="step-35-create-a-new-key"></a>Adım 3.5: Yeni bir anahtar oluşturma

nCipher nShield **oluşturma anahtar** programını kullanarak bir anahtar oluşturun.

Anahtarı oluşturmak için aşağıdaki komutu çalıştırın:

    generatekey --generate simple type=RSA size=2048 protect=module ident=contosokey plainname=contosokey nvram=no pubexp=

Bu komutu çalıştırdığınızda, aşağıdaki yönergeleri kullanın:

* *protect* parametresi, gösterildiği gibi **module** değerine ayarlanmalıdır. Bu, modül korumalı bir anahtar oluşturur. BYOK araç takımı, OCS korumalı anahtarları desteklemez.
* **ident** ve **plainname** için *contosokey* değerini bir dize değeriyle değiştirin. İdari genel giderleri en aza indirmek ve hata riskini azaltmak için her ikisi için de aynı değeri kullanmanızı öneririz. **Kimlik** değeri yalnızca sayılar, tireler ve küçük harfler içermelidir.
* Bu örnekte pubexp (varsayılan) boş bırakılmıştır, ancak belirli bir değer belirtebilirsiniz. Daha fazla bilgi için [nCipher belgelerine bakın.](https://www.ncipher.com/resources/solution-briefs/protect-sensitive-data-rest-and-use-across-premises-and-azure-based)

Bu komut, %NFAST_KMDATA\yerel klasörünüzde **key_simple_** ile başlayan bir ad içeren bir Tokenized Key dosyası oluşturur ve ardından komutta belirtilen **tanımlayıcıyı** oluşturur. Örneğin: **key_simple_contosokey**. Bu dosya şifreli bir anahtar içerir.

Bu Simgeleştirilmiş Anahtar Dosyasını güvenli bir yere yedekleyin.

> [!IMPORTANT]
> Daha sonra anahtarınızı Azure Key Vault'a aktardığınızda, Microsoft bu anahtarı size geri aktaramaz, bu nedenle anahtar ve güvenlik dünyanızı güvenli bir şekilde yedeklemeniz son derece önemlidir. Anahtarınızı yedeklemek için rehberlik ve en iyi uygulamalar için [nCipher](https://www.ncipher.com/about-us/contact-us) ile iletişime geçin.
>


Artık anahtarınızı Azure Key Vault'a aktarmaya hazırsınız.

## <a name="step-4-prepare-your-key-for-transfer"></a>Adım 4: Kiracı anahtarınızı aktarım için hazırlama

Bu dördüncü adım için, bağlantısı kesilen iş istasyonu nda aşağıdaki yordamları yapın.

### <a name="step-41-create-a-copy-of-your-key-with-reduced-permissions"></a>Adım 4.1: Azaltılmış izinlerle anahtarınızın bir kopyasını oluşturma

Yeni bir komut istemi açın ve geçerli dizini BYOK zip dosyasının fermuarını açtığınız konuma değiştirin. Anahtarınızdaki izinleri bir komut isteminden azaltmak için, coğrafi bölgenize veya Azure örneğinize bağlı olarak aşağıdakilerden birini çalıştırın:

* Kuzey Amerika için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1
* Avrupa için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1
* Asya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1
* Latin Amerika için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1
* Japonya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1
* Kore için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1
* Güney Afrika için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SA-1
* BAE için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UAE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UAE-1
* Avustralya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1
* AZURE'un ABD devlet örneğini kullanan [Azure Hükümeti](https://azure.microsoft.com/features/gov/)için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1
* ABD Hükümeti DOD için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1
* Kanada için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1
* Almanya için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Almanya Kamu için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1
* Hindistan için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1
* Fransa için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-FRANCE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-FRANCE-1
* Birleşik Krallık için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1
* İsviçre için:

        KeyTransferRemote.exe -ModifyAcls -KeyAppName simple -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SUI-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SUI-1

Bu komutu çalıştırdığınızda, *contosokey'i* **Adım 3.5'te** belirttiğiniz değerle değiştirin: Anahtar [adımınızı oluştur'dan](#step-3-generate-your-key) yeni bir anahtar oluşturun.

Güvenlik dünyası yönetici kartlarınızı takmanız istenir.

Komut tamamlandığında, **Sonuç: BAŞARI** ve azaltılmış izinlere sahip anahtarınızın kopyası contosokey> key_xferacId_\<adlı dosyada yer almaktadır.

NCipher nShield yardımcı hizmetlerini kullanarak Aşağıdaki komutları kullanarak ACLS'yi inceleyebilirsiniz:

* aclprint.py:

        "%nfast_home%\bin\preload.exe" -m 1 -A xferacld -K contosokey "%nfast_home%\python\bin\python" "%nfast_home%\python\examples\aclprint.py"
* kmfile-dump.exe:

        "%nfast_home%\bin\kmfile-dump.exe" "%NFAST_KMDATA%\local\key_xferacld_contosokey"
  Bu komutları çalıştırdığınızda, contosokey'i Adım **3.5'te** belirttiğiniz değerle değiştirin: [Anahtar adımınızı oluştur'dan](#step-3-generate-your-key) yeni bir anahtar oluşturun.

### <a name="step-42-encrypt-your-key-by-using-microsofts-key-exchange-key"></a>Adım 4.2: Microsoft'un Anahtar Değişim Anahtarını kullanarak anahtarınızı şifreleyin

Coğrafi bölgenize veya Azure örneğinize bağlı olarak aşağıdaki komutlardan birini çalıştırın:

* Kuzey Amerika için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-NA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-NA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Avrupa için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-EU-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-EU-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Asya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AP-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AP-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Latin Amerika için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-LATAM-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-LATAM-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Japonya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-JPN-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-JPN-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Kore için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-KOREA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-KOREA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Güney Afrika için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* BAE için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UAE-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UAE-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Avustralya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-AUS-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-AUS-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* AZURE'un ABD devlet örneğini kullanan [Azure Hükümeti](https://azure.microsoft.com/features/gov/)için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USGOV-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USGOV-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* ABD Hükümeti DOD için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-USDOD-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-USDOD-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Kanada için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-CANADA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-CANADA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Almanya için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Almanya Kamu için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-GERMANY-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-GERMANY-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Hindistan için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-INDIA-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-INDIA-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Fransa için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-France-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-France-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* Birleşik Krallık için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-UK-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-UK-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey
* İsviçre için:

        KeyTransferRemote.exe -Package -KeyIdentifier contosokey -ExchangeKeyPackage BYOK-KEK-pkg-SUI-1 -NewSecurityWorldPackage BYOK-SecurityWorld-pkg-SUI-1 -SubscriptionId SubscriptionID -KeyFriendlyName ContosoFirstHSMkey


Bu komutu çalıştırdığınızda, aşağıdaki yönergeleri kullanın:

* *Contosokey'i* **Adım 3.5'te** anahtarı oluşturmak için kullandığınız tanımlayıcıyla değiştirin: Anahtar adımı [oluştur'dan](#step-3-generate-your-key) yeni bir anahtar oluşturun.
* *SubscriptionID'yi* anahtar kasanızı içeren Azure aboneliğinin kimliğiyle değiştirin. Bu değeri daha önce **Adım 1.2'de aldınız: Azure abonelik kimliğinizi** [Internet'e bağlı iş istasyonunu hazırla adımından](#step-1-prepare-your-internet-connected-workstation) alın.
* *ContosoFirstHSMKey'i* çıktı dosya adınız için kullanılan bir etiketle değiştirin.

Bu başarıyla tamamlandığında, **Sonuç: BAŞARI** görüntüler ve aşağıdaki adı olan geçerli klasörde yeni bir dosya var: KeyTransferPackage-*ContosoFirstHSMkey*.byok

### <a name="step-43-copy-your-key-transfer-package-to-the-internet-connected-workstation"></a>Adım 4.3: Anahtar aktarım paketinizi Internet'e bağlı iş istasyonuna kopyalama

Çıkış dosyasını önceki adımdan (KeyTransferPackage-ContosoFirstHSMkey.byok) Internet'e bağlı iş istasyonunuza kopyalamak için bir USB sürücü veya başka taşınabilir depolama alanı kullanın.

## <a name="step-5-transfer-your-key-to-azure-key-vault"></a>Adım 5: Anahtarınızı Azure Anahtar Kasası’na aktarma

Bu son adım için, Internet'e bağlı iş istasyonunda, bağlantısı kesilen iş istasyonundan kopyaladığınız anahtar aktarım paketini Azure Key Vault HSM'ye yüklemek için [Add-AzKeyVaultKey](/powershell/module/az.keyvault/add-azkeyvaultkey) cmdlet'i kullanın:

   ```powershell
        Add-AzKeyVaultKey -VaultName 'ContosoKeyVaultHSM' -Name 'ContosoFirstHSMkey' -KeyFilePath 'c:\KeyTransferPackage-ContosoFirstHSMkey.byok' -Destination 'HSM'
   ```

Yükleme başarılı olursa, az önce eklediğiniz anahtarın özelliklerini görürsünüz.

## <a name="next-steps"></a>Sonraki adımlar

Artık bu HSM korumalı anahtarı anahtar kasanızda kullanabilirsiniz. Daha fazla bilgi için bu fiyat ve özellik [karşılaştırması](https://azure.microsoft.com/pricing/details/key-vault/)bakın.
