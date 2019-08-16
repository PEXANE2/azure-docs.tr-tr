---
title: Azure Güvenlik Merkezi 'nde işletim sistemi güvenlik yapılandırmalarının özelleştirilmesi (Önizleme) | Microsoft Docs
description: Bu makalede Güvenlik Merkezi değerlendirmelerinin nasıl özelleştirileceği gösterilmektedir
services: security-center
documentationcenter: na
author: rkarlin
manager: barbkess
editor: ''
ms.assetid: ''
ms.service: security-center
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/15/2019
ms.author: v-mohabe
ms.openlocfilehash: df4f51f97798048b28c0193dbc61e07fc55c9adc
ms.sourcegitcommit: 040abc24f031ac9d4d44dbdd832e5d99b34a8c61
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/16/2019
ms.locfileid: "69535063"
---
# <a name="customize-os-security-configurations-in-azure-security-center-retired"></a>Azure Güvenlik Merkezi 'nde işletim sistemi güvenlik yapılandırmasını özelleştirme (kullanımdan kaldırıldı)

Bu izlenecek yol, Azure Güvenlik Merkezi 'nde işletim sistemi güvenlik yapılandırması değerlendirmelerinin (Önizleme) nasıl özelleştirileceğini gösterir.

> [!NOTE]
> İşletim sistemi güvenlik yapılandırmalarının (Önizleme özelliği) özelleştirilmesi, 31 Temmuz 2019 tarihinde kullanımdan kaldırılmıştır. Daha fazla bilgi ve diğer hizmetler için bkz. [Güvenlik Merkezi özelliklerini devre dışı bırakma (2019 Temmuz)](security-center-features-retirement-july2019.md#menu_securityconfigurations).

## <a name="what-are-os-security-configurations"></a>İşletim sistemi güvenlik yapılandırması nedir?

Azure Güvenlik Merkezi, işletim sistemini daha güçlü hale getirmek için güvenlik duvarları, denetim, parola ilkeleri ve benzeri birçok kural dahil olmak üzere [150'den fazla önerilen kuraldan](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335) oluşan bir kümeyi uygulayarak güvenlik yapılandırmalarını izler. Bir makinenin yapılandırmasında güvenlik açığı varsa Güvenlik Merkezi bu konuyla ilgili bir güvenlik önerisi oluşturur.

Şirketler, kuralları özelleştirerek, ortamları için hangi yapılandırma seçeneklerinin daha uygun olduğunu denetleyebilir. Özelleştirilmiş bir değerlendirme ilkesi ayarlayabilir ve ardından aboneliğin ilgili tüm makinelere uygulayabilirsiniz.

> [!NOTE]
> - Şu anda, işletim sistemi güvenlik yapılandırmasının özelleştirilmesi yalnızca Windows Server sürümleri 2008, 2008 R2, 2012, 2012 R2 ve 2016 işletim sistemleri için kullanılabilir.
> - Yapılandırma, seçili abonelik altındaki tüm çalışma alanlarına bağlı olan tüm sanal makinelere ve bilgisayarlara uygulanır.
> - İşletim sistemi güvenlik yapılandırması özelleştirmesi yalnızca güvenlik merkezi standart katmanında kullanılabilir.
>
>

Belirli bir kuralı etkinleştirip devre dışı bırakarak, var olan bir kural için istenen ayarı değiştirerek veya desteklenen kural türlerini (kayıt defteri, denetim ilkesi ve güvenlik ilkesi) temel alan yeni bir kural ekleyerek, işletim sistemi güvenlik yapılandırması kurallarını özelleştirebilirsiniz. Şu anda, istenen ayar tam bir değer olmalıdır.

Yeni kuralların aynı türdeki diğer mevcut kurallarla aynı biçimde ve yapıda olması gerekir.

> [!NOTE]
> İşletim sistemi güvenlik yapılandırmasını özelleştirmek için *abonelik sahibi*, *abonelik katılımcısı*veya *Güvenlik Yöneticisi*rolüne sahip olmanız gerekir.
>
>

## <a name="customize-the-default-os-security-configuration"></a>Varsayılan işletim sistemi güvenlik yapılandırmasını özelleştirme

Güvenlik Merkezi 'nde varsayılan işletim sistemi güvenlik yapılandırmasını özelleştirmek için aşağıdakileri yapın:

1.  **Güvenlik Merkezi** panosunu açın.

2.  Sol bölmede, **fiyatlandırma & ayarları**' nı seçin.

    ![Güvenlik Ilkesi listesi](media/security-center-customize-os-security-config/manual-provision.png)

4. Geçerli aboneliği seçin ve **güvenlik yapılandırmasını düzenle**' yi seçin.  

    !["Güvenlik yapılandırmasını düzenle" penceresi](media/security-center-customize-os-security-config/blade.png)

5. Değiştirilen dosyayı indirme, düzenleme ve karşıya yükleme adımlarını izleyin.

   > [!NOTE]
   > Varsayılan olarak, indirçalıştığınız yapılandırma dosyası *JSON* biçimindedir. Bu dosyayı değiştirme hakkında yönergeler için [yapılandırma dosyasını özelleştirme](#customize-the-configuration-file)bölümüne gidin.
   >

6. Değişikliği **kaydetmek Için kaydet**' i seçin. Aksi takdirde, ilke depolanmaz.

    ![Kaydet düğmesi](media/security-center-customize-os-security-config/save-successfully.png)

   Dosyayı başarıyla kaydettikten sonra, yapılandırma, aboneliğin altındaki çalışma alanlarına bağlı tüm sanal makinelere ve bilgisayarlara uygulanır. İşlem genellikle birkaç dakika sürer, ancak altyapı boyutuna bağlı olarak daha uzun sürebilir.

Herhangi bir noktada, geçerli ilke yapılandırmasını varsayılan durumuna sıfırlayabilirsiniz. Bunu yapmak için, **işletim sistemi güvenlik yapılandırması kurallarını Düzenle** penceresinde **Sıfırla**' yı seçin. Onay açılır penceresinde **Evet** ' i seçerek bu seçeneği onaylayın.

![Sıfırlama onayı penceresi](media/security-center-customize-os-security-config/edit-alert.png)

## <a name="customize-the-configuration-file"></a>Yapılandırma dosyasını özelleştirme

Özelleştirme dosyasında, desteklenen her işletim sistemi sürümü bir kural kümesine veya RuleSet 'e sahiptir. Her RuleSet 'in, aşağıdaki örnekte gösterildiği gibi kendi adı ve benzersiz KIMLIĞI vardır:

![RuleSet adı ve KIMLIĞI](media/security-center-customize-os-security-config/custom-file.png)

> [!NOTE]
> Bu örnek dosya Visual Studio 'da düzenlendi, ancak JSON Görüntüleyicisi eklentisi yüklüyse Not defteri 'ni de kullanabilirsiniz.
>
>

Özelleştirme dosyasını düzenlediğinizde, bir kuralı veya bunların tümünü değiştirebilirsiniz. Her RuleSet, üç kategoride ayrılmış bir *Rules* bölümü içerir: Kayıt defteri, denetim Ilkesi ve Güvenlik Ilkesi burada gösterildiği gibi:

![Üç RuleSet kategorisi](media/security-center-customize-os-security-config/rules-section.png)

Her kategorinin kendi öznitelik kümesi vardır. Aşağıdaki öznitelikleri değiştirebilirsiniz:

- **ExpectedValue**: Bu özniteliğin alan veri türü, *kural türü*başına desteklenen değerlerle eşleşmelidir, örneğin:

  - **baselineRegistryRules**: Değer, bu kuralda tanımlanan [Regvaluetype](https://msdn.microsoft.com/library/windows/desktop/ms724884) ile eşleşmelidir.

  - **Baselineauditpolicyrules**: Aşağıdaki dize değerlerinden birini kullanın:

    - *Başarı ve başarısızlık*

    - *Başarılı*

  - **Baselinesecuritypolicyrules**: Aşağıdaki dize değerlerinden birini kullanın:

    - *Hiç kimse*

    - İzin verilen kullanıcı gruplarının listesi, örneğin: *Yöneticiler*, *yedekleme işleçleri*

-   **durum**: Dize *devre dışı* veya *etkin*seçenekleri içerebilir. Bu sürümde, dize büyük/küçük harfe duyarlıdır.

Bunlar, yapılandırılabilecek tek alanlardır. Dosya biçimini veya boyutunu ihlal ederseniz değişikliği kaydedemezsiniz. Geçerli bir JSON yapılandırma dosyasını karşıya yüklemeniz gerektiğini belirten bir hata alırsınız.

Diğer olası hataların listesi için bkz. [hata kodları](#error-codes).

Aşağıdaki üç bölüm, önceki kuralların örneklerini içerir. *ExpectedValue* ve *State* öznitelikleri değiştirilebilir.

**baselineRegistryRules**
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\LanManServer\\\\Parameters",
    "valueName": "restrictnullsessaccess",
    "ruleId": "f9020046-6340-451d-9548-3c45d765d06d",
    "originalId": "0f319931-aa36-4313-9320-86311c0fa623",
    "cceId": "CCE-10940-5",
    "ruleName": "Network access: Restrict anonymous access to Named Pipes and
    Shares",
    "ruleType": "Registry",
    "expectedValue": "1",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Disabled"

    }
```

**baselineAuditPolicyRules**
```json
    {
    "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
    "ruleId": "37745508-95fb-44ec-ab0f-644ec0b16995",
    "originalId": "2ea0de1a-c71d-46c8-8350-a7dd4d447895",
    "cceId": "CCE-11001-5",
    "ruleName": "Audit Policy: Account Management: Other Account Management Events",
    "ruleType": "AuditPolicy",
    "expectedValue": "Success and Failure",
    "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

**baselineSecurityPolicyRules**
```json
    {
    "sectionName": "Privilege Rights",
    "settingName": "SeIncreaseWorkingSetPrivilege",
    "ruleId": "b0ec9d5e-916f-4356-83aa-c23522102b33",
    "originalId": "b61bd492-74b0-40f3-909d-36b9bf54e94c",
    "cceId": "CCE-10548-6",
    "ruleName": "Increase a process working set",
    "ruleType": "SecurityPolicy",
    "expectedValue": "Administrators, Local Service",
    "severity": "Warning",
    "analyzeOperation": "Equals",
    "source": "Microsoft",
    "state": "Enabled"
    }
```

Bazı kurallar farklı işletim sistemi türleri için yinelenir. Yinelenen kuralların aynı *OriginalID* özniteliği var.

## <a name="create-custom-rules"></a>Özel kurallar oluşturma

Yeni kurallar da oluşturabilirsiniz. Yeni bir kural oluşturmadan önce, aşağıdaki kısıtlamaları göz önünde bulundurun:

-   Şema sürümü, *Baselineıd* ve *baselinename* değiştirilemez.

-   RuleSet kaldırılamıyor.

-   RuleSet eklenemiyor.

-   İzin verilen en fazla kural sayısı (varsayılan kurallar dahil) 1000 ' dir.

Yeni özel kurallar yeni bir özel kaynakla (! = "Microsoft") işaretlenir. *RuleId* alanı null veya boş olabilir. Boşsa, Microsoft bir tane oluşturur. Boş değilse, tüm kurallarda benzersiz olan geçerli bir GUID 'ye sahip olmalıdır (varsayılan ve özel). Çekirdek alanlar için aşağıdaki kısıtlamaları gözden geçirin:

-   **OriginalID**: Null veya boş olabilir. *OriginalID* boş değilse, GEÇERLI bir GUID olmalıdır.

-   **Cceıd**: Null veya boş olabilir. *Cceıd* boş değilse, benzersiz olmalıdır.

-   **RuleType**: (bir tane seçin) Registry, AuditPolicy veya SecurityPolicy.

-   **Önem derecesi**: (bir tane seçin) bilinmeyen, kritik, uyarı veya bilgilendirici.

-   **analiz eden işlem**: *Eşit*olmalıdır.

-   **Auditpolicyıd**: Geçerli bir GUID olmalıdır.

-   **Regvaluetype**: (Select One) Int, Long, String veya multiplestring.

> [!NOTE]
> Hive *LocalMachine*olmalıdır.
>
>

Yeni bir özel kural örneği:

**Kayıt defteri**:
```json
    {
    "hive": "LocalMachine",
    "regValueType": "Int",
    "keyPath":
    "System\\\\CurrentControlSet\\\\Services\\\\Netlogon\\\\MyKeyName",
    "valueName": "MyValueName",
    "originalId": "",
    "cceId": "",
    "ruleName": "My new registry rule", "baselineRuleType": "Registry",
    "expectedValue": "123", "severity": "Critical",
    "analyzeOperation": "Equals",
    "source": "MyCustomSource",
    "state": "Enabled"
    }
```
**Güvenlik ilkesi**:
```json
   {
   "sectionName": "Privilege Rights",
   "settingName": "SeDenyBatchLogonRight",
   "originalId": "",
   "cceId": "",
   "ruleName": "My new security policy rule", "baselineRuleType":
   "SecurityPolicy",
   "expectedValue": "Guests",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```
**Denetim ilkesi**:
```json
   {
   "auditPolicyId": "0cce923a-69ae-11d9-bed3-505054503030",
   "originalId": "",
   "cceId": "",
   "ruleName": " My new audit policy rule ", "baselineRuleType": "AuditPolicy",
   "expectedValue": " Failure",
   "severity": "Critical",
   "analyzeOperation": "Equals", "source": " MyCustomSource ",
   "state": "Enabled"
   }
```

## <a name="file-upload-failures"></a>Karşıya dosya yükleme sorunları

Gönderilen yapılandırma dosyası, değerler veya biçimlendirme içindeki hatalar nedeniyle geçersiz ise, **kaydetme eylemi başarısız oldu**gibi bir hata hatası görüntülenir. Düzeltilen bir yapılandırma dosyasını yeniden göndermeden önce hataları düzeltmek ve gidermek için ayrıntılı bir Errors. csv raporu indirebilirsiniz.

Bir hata dosyası örneği:

![Hata dosyası örneği](media/security-center-customize-os-security-config/errors-file.png)

## <a name="error-codes"></a>Hata kodları

Olası tüm hatalar aşağıdaki tabloda listelenmiştir:

| **Hata:**                                | **Açıklama**                                                                                                                              |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------|
| BaselineConfigurationSchemaVersionError  | *SchemaVersion* özelliği geçersiz veya boş. Değerin olarak *{0}* ayarlanması gerekir.                                                         |
| BaselineInvalidStringError               | Özellik *{0}*  *n\\* içeremez.                                                                                                         |
| BaselineNullRuleError                    | Taban çizgisi yapılandırma kuralları listesi *null*değeri olan bir kural içeriyor.                                                                         |
| BaselineRuleCceIdNotUniqueError          | CCE kimliği *{0}* benzersiz değil.                                                                                                                  |
| Baselineruliemptyözelliği                | Özellik *{0}* eksik veya geçersiz.                                                                                                       |
| BaselineRuleIdNotInDefault               | Kuralda *Microsoft* kaynak özelliği bulunur, ancak Microsoft varsayılan RuleSet içinde bulunamadı.                                                   |
| BaselineRuleIdNotUniqueError             | Kural tanımlayıcısı benzersiz değil.                                                                                                                       |
| Baselinerutaınvalidguid                  | Özellik *{0}* geçersiz bulundu. Değer geçerli bir GUID değil.                                                                             |
| Baselineruliınvalidhive                  | Hive LocalMachine olmalıdır.                                                                                                                   |
| BaselineRuleNameNotUniqueError           | Kural adı benzersiz değil.                                                                                                                 |
| BaselineRuleNotExistInConfigError        | Kural yeni yapılandırmada bulunamadı. Kural silinemiyor.                                                                     |
| BaselineRuleNotFoundError                | Kural varsayılan temel kural kümesi içinde bulunamadı.                                                                                        |
| BaselineRuleNotInPlace                   | Kural, türü {0} olan bir varsayılan kuralla eşleşir ve {1} listede listelenir.                                                                       |
| BaselineRulePropertyTooLong              | Özellik *{0}* çok uzun. İzin verilen en fazla {1}Uzunluk:.                                                                                        |
| Baselineruleregtypeınvalinothing          | Beklenen değer *{0}* tanımlanan kayıt defteri değer türüyle eşleşmiyor.                                                              |
| Baselinerulisetadded                     | Tanımlayıcıya *{0}* sahip RuleSet, varsayılan yapılandırmada bulunamadı. RuleSet eklenemiyor.                                               |
| BaselineRulesetIdMustBeUnique            | Verilen taban çizgisi RuleSet *{0}* benzersiz olmalıdır.                                                                                           |
| BaselineRulesetNotFound                  | Tanıtıcı *{0}* ve ada *{1}* sahip RuleSet, belirtilen yapılandırmada bulunamadı. RuleSet silinemiyor.                                |
| BaselineRuleSourceNotMatch               | Tanımlayıcıya *{0}* sahip kural zaten tanımlandı.                                                                                                       |
| BaselineRuleTypeDoesntMatch              | Varsayılan kural türü *{0}* .                                                                                                              |
| Baselinerulitypeunntmatcherror         | Kuralın *{0}* gerçek türü, ancak *{1}* *RuleType* özelliği.                                                                          |
| BaselineRuleUnpermittedChangesError      | Yalnızca *ExpectedValue* ve *State* özelliklerinin değiştirilmesine izin verilir.                                                                       |
| BaselineTooManyRules                     | İzin verilen özelleştirilmiş kuralların {0} en fazla sayısı kurallardır. Verilen yapılandırma kurallar, {1} {2} varsayılan kurallar ve {3} özelleştirilmiş kurallar içerir. |
| ErrorNoConfigurationStatus               | Yapılandırma durumu bulunamadı. İstenen yapılandırma durumunu durum: *Varsayılan* veya *özel*.                                    |
| ErrorNonEmptyRulesetOnDefault            | Yapılandırma durumu varsayılan olarak ayarlanır. *Baselinerulisets* listesi null veya boş olmalıdır.                                                          |
| ErrorNullRulesetsPropertyOnCustom        | Verilen yapılandırma durumu *özel* ancak *Baselinerulisets* özelliği null veya boş.                                             |
| ErrorParsingBaselineConfig               | Verilen yapılandırma geçersiz. Tanımlı bir veya daha fazla değer null değere sahip veya geçersiz türde.                                  |
| ErrorParsingIsDefaultProperty            | Verilen *configurationstatus* değeri *{0}* geçersiz. Değer yalnızca *varsayılan* veya *özel*olabilir.                                         |
| Incompatibleviewversion                  | Bu çalışma alanı *{0}* türünde görünüm sürümü desteklenmiyor.                                                                                   |
| Invalidbaselineconfigurationgeneralerror | Verilen temel yapılandırma bir veya daha fazla tür doğrulama hatası ile bulundu.                                                          |
| ViewConversionError                      | Görünüm, çalışma alanının desteklediğinden daha eski bir sürümdür. Görüntüleme dönüşümü başarısız oldu {0}:.                                                                 |

Yeterli izniniz yoksa, burada gösterildiği gibi genel bir hata hatası alabilirsiniz:

!["Eylem kaydetme başarısız oldu" hata iletisi](media/security-center-customize-os-security-config/general-failure-error.png)

## <a name="next-steps"></a>Sonraki adımlar
Bu makalede, güvenlik merkezi 'nde işletim sistemi güvenlik yapılandırması değerlendirmelerinin nasıl özelleştirileceği açıklanır. Yapılandırma kuralları ve düzeltme hakkında daha fazla bilgi edinmek için bkz.:

- [Güvenlik Merkezi ortak yapılandırma tanımlayıcıları ve temel kuralları](https://gallery.technet.microsoft.com/Azure-Security-Center-a789e335).
- Güvenlik Merkezi, yapılandırma kurallarına benzersiz tanımlayıcılar atamak için ortak yapılandırma numaralandırması (CCE) kullanır. Daha fazla bilgi için bkz. [CCE](https://nvd.nist.gov/config/cce/index).
- İşletim sistemi yapılandırmanızın önerilen güvenlik yapılandırması kurallarıyla eşleşmemesi durumunda güvenlik açıklarını gidermek için bkz. [güvenlik yapılandırmalarını düzeltme](security-center-virtual-machine-protection.md).
