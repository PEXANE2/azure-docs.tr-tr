---
title: Azure Anahtar Kasası'nı Azure İlkesi ile tümleştirin
description: Azure Anahtar Atlama'yı Azure Politikası ile nasıl entegre edebilirsiniz öğrenin
author: msmbaldwin
ms.author: mbaldwin
ms.date: 01/28/2020
ms.service: key-vault
ms.subservice: secrets
ms.topic: quickstart
ms.openlocfilehash: 8c49b53cae08415633e1405317742a8a5d4e64b0
ms.sourcegitcommit: c2065e6f0ee0919d36554116432241760de43ec8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/26/2020
ms.locfileid: "78196901"
---
# <a name="integrate-azure-key-vault-with-azure-policy"></a>Azure Anahtar Kasası'nı Azure İlkesi ile tümleştirin

[Azure İlkesi,](../governance/policy/index.yml) kullanıcılara Azure ortamlarını ölçekte denetleme ve yönetme olanağı sağlayan bir yönetim aracıdır. Azure İlkesi, atanan ilke kurallarına uygun olduğundan emin olmak için Azure kaynaklarına korkuluklar yerleştirme olanağı sağlar. Kullanıcıların Azure ortamlarını denetleme, gerçek zamanlı uygulama ve düzeltme gerçekleştirmelerine olanak tanır. İlke tarafından gerçekleştirilen denetimlerin sonuçları, hangi kaynakların ve bileşenlerin uyumlu olup olmadığının ayrıntılı bir ayrıntısını görebilecekleri bir uyumluluk panosunda kullanıcılar tarafından kullanılabilir.  Daha fazla bilgi için [Azure İlkesi hizmetine Genel Bakış bölümüne](../governance/policy/overview.md)bakın.

Örnek Kullanım Senaryoları:

- Şirketinizin anahtar kasalarında minimum anahtar boyutları ve sertifikaların maksimum geçerlilik süreleri ile ilgili gereksinimleri uygulayarak şirketinizin güvenlik duruşunu iyileştirmek istiyorsunuz, ancak hangi takımların uyumlu olup olmadığını bilmiyorsunuz. 
- Şu anda kuruluşunuz genelinde bir denetim gerçekleştirmek için bir çözümünüz yok veya kuruluşunuzdaki tek tek ekiplerden uyumluluklarını bildirmelerini isteyerek ortamınızın el ile denetimlerini gerçekleştiresiniz. Bu görevi otomatikleştirmenin, denetimleri gerçek zamanlı olarak gerçekleştirmenin ve denetimin doğruluğunu garanti etmenin bir yolunu arıyorsunuz.
- Şirket güvenlik ilkelerinizi uygulamak ve bireylerin kendi imzalı sertifikalar oluşturmasını engellemek istiyorsunuz, ancak bunların oluşturulmasını engellemek için otomatik bir yolunuz yok. 
- Test ekipleriniz için bazı gereksinimleri gevşetmek istiyorsunuz, ancak üretim ortamınız üzerinde sıkı denetimler yapmak istiyorsunuz. Kaynaklarınızın uygulanmasını ayırmak için basit bir otomatik yol gerekir. 
- Canlı bir site sorunu durumunda yeni ilkelerin uygulanmasını geri alabildiğinizden emin olmak istersiniz. İlkenin uygulanmasını kapatmak için tek tıklamayla çözüme ihtiyacınız vardır. 
- Ortamınızı denetlemek için üçüncü taraf çözümüne güveniyorsunuz ve dahili bir Microsoft teklifi kullanmak istiyorsunuz. 

## <a name="types-of-policy-effects-and-guidance"></a>İlke etkileri ve rehberlik türleri

**Denetim**: Bir ilkenin etkisi denetlenecek şekilde ayarlandığında, ilke ortamınızda herhangi bir kırılma değişikliğine neden olmaz. Yalnızca bu bileşenleri ilke uyumluluğu panosunda uyumsuz olarak işaretleyerek, belirli bir kapsamdaki ilke tanımlarına uymayan sertifikalar gibi bileşenler konusunda sizi uyarır. İlke etkisi seçili değilse denetim varsayılandır. 

**Reddet**: Bir ilkenin etkisi reddedilecek şekilde ayarlandığında, ilke sertifikalar gibi yeni bileşenlerin oluşturulmasını engeller ve ilke tanımına uymayan varolan bileşenlerin yeni sürümlerini engeller. Önemli bir kasadaki mevcut uyumsuz kaynaklar etkilenmez. 'Denetim' yetenekleri çalışmaya devam edecektir.

## <a name="available-built-in-policy-definitions"></a>Kullanılabilir "Yerleşik" İlke Tanımları

Anahtar Atlama, sertifikaları yönetmek için sık karşılaşılan senaryolar için atayabileceğiniz bir ilkeler kümesi oluşturmuştur. Bu ilkeler 'Yerleşik'dir, bu da bunları etkinleştirmek için özel JSON yazmanızı gerektirmediği ve atamanız için Azure portalında kullanılabildiği anlamına gelir. Kuruluşunuzun gereksinimlerine uyacak şekilde belirli parametreleri özelleştirmeye devam edebilirsiniz. 

Sekiz önizleme politikası aşağıdaki gibidir.

### <a name="manage-certificate-validity-period-preview"></a>Sertifika geçerlilik süresini yönetme (önizleme)

Bu ilke, anahtar kasasında depolanan sertifikalarınızın maksimum geçerlilik süresini yönetmenize olanak tanır. Sertifikalarınızın maksimum geçerlilik süresini sınırlamak iyi bir güvenlik uygulamasıdır. Sertifikanızın özel bir anahtarı algılandırılmaksızın tehlikeye girerse, kısa ömürlü sertifikalar kullanmak devam eden hasariçin gereken süreyi en aza indirir ve sertifikanın saldırganın değerini azaltır. 

### <a name="manage-allowed-certificate-key-types-preview"></a>İzin verilen sertifika anahtar türlerini yönetme (önizleme)
Bu ilke, anahtar kasanızda olabilecek sertifika türünü kısıtlamanızı sağlar. Sertifika özel anahtarlarınızın RSA, ECC veya HSM destekli olduğundan emin olmak için bu ilkeyi kullanabilirsiniz. Sertifika türlerine izin verilen aşağıdaki listeden seçim yapabilirsiniz.
- RSA
- RSA - HSM
- Ecc 
- ECC - HSM 

### <a name="manage-certificate-lifetime-action-triggers-preview"></a>Sertifika yaşam boyu eylem tetikleyicilerini yönetme (önizleme)

Bu ilke, sürelerinin dolmasından sonraki belirli bir gün içinde olan veya kullanılabilir yaşamlarının belirli bir yüzdesine ulaşmış sertifikalar için belirtilen yaşam boyu eylemi yönetmenize olanak tanır. 

### <a name="manage-certificates-issued-by-an-integrated-ca-preview"></a>Entegre bir CA tarafından verilen sertifikaları yönetme (önizleme)

Key Vault tümleşik sertifika yetkilisi (Digicert veya GlobalSign) kullanıyorsanız ve kullanıcıların bu sağlayıcılardan birini veya her birini kullanmasını istiyorsanız, bu politikayı seçiminizi denetlemek veya zorlamak için kullanabilirsiniz. Bu ilke, anahtar kasasında kendi imzalı sertifikaların oluşturulmasını denetlemek veya reddetmek için de kullanılabilir. 

### <a name="manage-certificates-issued-by-an-integrated-ca-preview"></a>Entegre bir CA tarafından verilen sertifikaları yönetme (önizleme)

Bir iç sertifika yetkilisi veya anahtar kasasıyla entegre olmayan bir sertifika yetkilisi kullanıyorsanız ve kullanıcıların sağladığınız bir listeden bir sertifika yetkilisi kullanmasını istiyorsanız, bu ilkeyi, ihraççı adına göre sertifika yetkililerinin izin verilen bir listesini oluşturmak için kullanabilirsiniz. Bu ilke, anahtar kasasında kendi imzalı sertifikaların oluşturulmasını denetlemek veya reddetmek için de kullanılabilir. 

### <a name="manage-allowed-curve-names-for-elliptic-curve-cryptography-certificates-preview"></a>Eliptik eğri şifreleme sertifikaları için izin verilen eğri adlarını yönetme (önizleme)
Eliptik eğri şifreleme veya ECC sertifikaları kullanıyorsanız, aşağıdaki listeden izin verilen eğri adlarının listesini özelleştirebilirsiniz. Varsayılan seçenek, aşağıdaki eğri adlarının tümüne izin verir. 
- P-256
- P-256K
- P-384
- P-521

### <a name="manage-minimum-key-size-for-rsa-certificates-preview"></a>RSA sertifikaları için minimum anahtar boyutunu yönetme (önizleme)
RSA sertifikaları kullanıyorsanız, sertifikalarınızın sahip olması gereken minimum anahtar boyutunu seçebilirsiniz. Aşağıdaki listeden bir seçenek seçebilirsiniz. 
- 2048 bit
- 3072 bit
- 4096 bit

### <a name="manage-certificates-that-are-within-a-specified-number-of-days-of-expiration-preview"></a>Belirli bir gün içinde olan sertifikaları son kullanma tarihinden itibaren yönetme (önizleme)
Yeterli şekilde izlenmeyen bir sertifika nın süresi dolmadan döndürülmezse hizmetiniz de bir kesinti yaşayabilir. Bu ilke, anahtar kasasında depolanan sertifikalarınızın izlendiğinden emin olmak için önemlidir. Bu ilkeyi, örneğin 180, 90, 60 ve 30 günlük eşiklerde olmak üzere farklı son kullanma eşikleriyle birden çok kez uygulamanız önerilir. Bu ilke, kuruluşunuzdaki sertifika süresinin sona ermesini izlemek ve üçe kalmak için kullanılabilir. 

## <a name="example-scenario"></a>Örnek Senaryo

100 sertifika içeren birden çok ekip tarafından kullanılan bir anahtar kasasını yönetirsiniz ve anahtar kasasındaki sertifikaların hiçbirinin 2 yıldan uzun süre geçerli olmadığından emin olmak istersiniz.

1. Yönet sertifikası [geçerlilik süresi](#manage-certificate-validity-period-preview) ilkesini atar, sertifikanın maksimum geçerlilik süresinin 24 ay olduğunu belirtin ve ilkenin etkisini "denetim" olarak ayarlarsınız. 
1. [Azure portalında uyumluluk raporunu](#view-compliance-results)görüntülediğiniz ve 20 sertifikanın > 2 yıl boyunca uygun olmadığını ve geçerli olduğunu ve geri kalan sertifikaların uyumlu olduğunu fark esiniz. 
1. Bu sertifikaların sahipleriyle iletişim kurar ve sertifikaların 2 yıldan uzun süre geçerli olamayacağını belirten yeni güvenlik gereksinimini iletirsiniz. Bazı takımlar yanıt verir ve sertifikaların 15'i maksimum geçerlilik süresi 2 yıl veya daha kısa olan yenilenir. Diğer takımlar yanıt vermez ve anahtar kasanızda hala 5 uyumsuz sertifikanız vardır.
1. "Reddetmek" için atadığınız ilkenin etkisini değiştirirsiniz. Uyumlu olmayan 5 sertifika iptal edilmez ve çalışmaya devam ederler. Ancak, 2 yıldan fazla bir geçerlilik süresi ile yenilenemez. 

## <a name="enabling-and-managing-a-key-vault-policy-through-the-azure-portal"></a>Azure portalı üzerinden Bir Key Vault ilkesini etkinleştirme ve yönetme

### <a name="select-a-policy-definition"></a>İlke Tanımı seçin

1. Azure portalında oturum açın. 
1. Arama Çubuğu'nda "İlke" aramave **İlke**Seçin.

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img1.png)

1. İlke penceresinde **Tanımlar'ı**seçin.

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img2.png)

1. Kategori Filtresi'nde **Tümünü Seç'i Seç'i** Seçin ve **Anahtar Kasası'nı**seçin. 

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img3.png)

1. Artık Azure Anahtar Kasası için Genel Önizleme için kullanılabilen tüm ilkeleri görebilmelisiniz. Yukarıdaki ilke kılavuzu bölümünü okuduğunuzdan ve anladığınızdan emin olun ve bir kapsama atamak istediğiniz bir ilke seçin.  

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img4.png)

### <a name="assign-a-policy-to-a-scope"></a>Bir Kapsama İlke Atama 

1. Bu örnekte, Uygulamak istediğiniz bir ilke seçin, **Sertifika Geçerlilik Süresi Yönet** ilkesi gösterilir. Sol üst köşedeki atama düğmesini tıklatın.

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img5.png)
  
1. İlkenin uygulanmasını istediğiniz aboneliği seçin. Kapsamı abonelik içinde yalnızca tek bir kaynak grubuyla sınırlamayı seçebilirsiniz. İlkeyi tüm abonelik için uygulamak ve bazı kaynak gruplarını hariç tutmak istiyorsanız, bir dışlama listesi de yapılandırabilirsiniz. İlkenin (denetim veya reddet) etkisinin oluşmasını veya etkinin **Disabled** (denetim veya reddet) kapatMasını istiyorsanız, ilke yürütme seçicisini **Etkin** olarak ayarlayın. 

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img6.png)

1. İstediğiniz aylarda maksimum geçerlilik süresini belirtmek için ekranın üst kısmındaki parametreler sekmesine tıklayın. Yukarıdaki bölümlerdeki yönergeyi izleyerek politikanın etkisi için **denetim** veya **reddet** seçeneğini belirleyin. Ardından gözden geçirin + oluştur düğmesini seçin. 

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img7.png)

### <a name="view-compliance-results"></a>Uyumluluk Sonuçlarını Görüntüle

1. İlke bıçağına geri dön ve uyumluluk sekmesini seçin. Uyumluluk sonuçlarını görüntülemek istediğiniz ilke atamasına tıklayın.

    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img8.png)

1. Bu sayfadan, uyumlu veya uyumlu olmayan kasalara göre sonuçlara filtre uygulayabilirsiniz. Burada, ilke ataması kapsamında uyumlu olmayan anahtar kasalarının listesini görebilirsiniz. Kasadaki bileşenlerden (sertifikalar) herhangi biri uygun değilse, kasa uyumlu değildir. Uyumlu olmayan bileşenleri (sertifikalar) görüntülemek için tek bir kasa seçebilirsiniz. 


    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img9.png)

1. Uyumlu olmayan bir kasa içindeki bileşenlerin adını görüntüleme


    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img10.png)

1. Kullanıcıların anahtar kasası içinde kaynak oluşturma olanağının reddedilip reddedilmediğini denetlemeniz gerekiyorsa, reddedilen sertifika işlemlerinin bir özetini istek ve istek zaman damgalarıyla görüntülemek için **Bileşen Olayları (önizleme)** sekmesini tıklatabilirsiniz. 


    ![Azure Key Vault'un nasıl çalıştığına genel bakış](./media/policy-img11.png)

## <a name="feature-limitations"></a>Özellik Sınırlamaları

"Reddet" etkisi olan bir ilke atamak, uyumlu olmayan kaynakların oluşturulmasını reddetmeye başlamak için 30 dakika (ortalama durum) ve 1 saat (en kötü durum) kadar sürebilir. Bir kasadaki mevcut bileşenlerin ilke değerlendirmesi, uyumluluk sonuçlarının portal Kullanıcı Arabirimi'nde görüntülenmeden önce 1 saat (ortalama durum) ve 2 saat (en kötü durum) kadar sürebilir. Uyumluluk sonuçları "Başlatılmamamılı" olarak görünüyorsa, bunun nedeni aşağıdaki nedenlerden olabilir:
- İlke değerlemesi henüz tamamlanmadı. İlk değerlendirme gecikmesi en kötü durum senaryosunda 2 saat kadar sürebilir. 
- İlke ataması kapsamında önemli kasa lar bulunmamaktadır.
- İlke ataması kapsamında sertifikalı anahtar kasa bulunmamaktadır. 

## <a name="next-steps"></a>Sonraki Adımlar

- [Azure İlkesi hizmeti](../governance/policy/overview.md) hakkında daha fazla bilgi edinin
- Bkz. Key Vault örnekleri: [Key Vault yerleşik ilke tanımları](../governance/policy/samples/built-in-policies.md#key-vault)