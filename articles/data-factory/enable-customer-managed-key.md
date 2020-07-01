---
title: Müşteri tarafından yönetilen anahtarla Azure Data Factory şifreleyin
description: Kendi Anahtarını Getir Data Factory güvenliği geliştirme (BYOK)
services: data-factory
documentationcenter: ''
author: chez-charlie
ms.service: data-factory
ms.topic: quickstart
ms.date: 05/08/2020
ms.author: chez
ms.reviewer: mariozi
ms.openlocfilehash: 9caa4690fa6b9d6c473bb8863acdcbbe15a37fba
ms.sourcegitcommit: 73ac360f37053a3321e8be23236b32d4f8fb30cf
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 06/30/2020
ms.locfileid: "85570067"
---
# <a name="encrypt-azure-data-factory-with-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlarla Azure Data Factory şifreleyin

[!INCLUDE[appliesto-adf-xxx-md](includes/appliesto-adf-xxx-md.md)]

Azure Data Factory, varlık tanımları, çalışma sırasında önbelleğe alınan tüm veriler devam ediyor ve veri önizleme için önbelleğe alınan veriler de dahil olmak üzere, bekleyen verileri şifreler. Varsayılan olarak, veriler, veri fabrikanıza benzersiz olarak atanmış rastgele oluşturulmuş bir Microsoft tarafından yönetilen anahtarla şifrelenir. Ek güvenlik garantisi için artık Azure Data Factory içindeki müşteri tarafından yönetilen anahtarlar özelliğiyle Kendi Anahtarını Getir (BYOK) seçeneğini etkinleştirebilirsiniz. Müşteri tarafından yönetilen bir anahtar belirttiğinizde Data Factory, müşteri verilerini şifrelemek için hem fabrika sistem anahtarını hem __de__ CMK 'yi kullanır. Eksik olan iki durum da veri ve fabrika erişimine Izin vermez.

Müşteri tarafından yönetilen anahtarları depolamak için Azure Key Vault gereklidir. Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Anahtar Kasası ve Data Factory aynı Azure Active Directory (Azure AD) kiracısında ve aynı bölgede olmalıdır, ancak farklı aboneliklerde olabilir. Azure Key Vault hakkında daha fazla bilgi için bkz. [Azure Key Vault nedir?](../key-vault/general/overview.md)

> [!NOTE]
> Müşteri tarafından yönetilen anahtar yalnızca boş bir veri fabrikasında yapılandırılabilir. Data Factory, bağlı hizmetler, işlem hatları ve veri akışları gibi kaynaklar içeremez. Üretici oluşturulduktan sonra müşterinin yönettiği anahtarı etkinleştirmeniz önerilir.

## <a name="about-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarlar hakkında

Aşağıdaki diyagramda, Data Factory, müşteri tarafından yönetilen anahtarı kullanarak istek yapmak için Azure Active Directory ve Azure Key Vault nasıl kullandığı gösterilmektedir:

  ![Müşteri tarafından yönetilen anahtarların Azure Data Factory içinde nasıl çalıştığını gösteren diyagram](media/enable-customer-managed-key/encryption-customer-managed-keys-diagram.png)

Aşağıdaki listede, diyagramdaki numaralandırılmış adımlar açıklanmaktadır:

1. Azure Key Vault Yöneticisi, şifreleme anahtarlarına Data Factory ilişkili yönetilen kimliğe izinler verir
1. Data Factory Yöneticisi, fabrikada müşterinin yönettiği anahtar özelliğini sunar
1. Data Factory, Azure Key Vault erişimi Azure Active Directory aracılığıyla doğrulamak için fabrika ile ilişkili yönetilen kimliği kullanır
1. Data Factory, fabrika şifreleme anahtarını Azure Key Vault müşteri anahtarıyla sarar
1. Okuma/yazma işlemleri için Data Factory, şifreleme ve şifre çözme işlemleri gerçekleştirmek üzere hesap şifreleme anahtarını sarmalamak için Azure Key Vault istekleri gönderir

## <a name="prerequisites---configure-azure-key-vault-and-generate-keys"></a>Önkoşullar-Azure Key Vault yapılandırma ve anahtarları oluşturma

### <a name="enable-soft-delete-and-do-not-purge-on-azure-key-vault"></a>Azure Key Vault geçici silme özelliğini etkinleştirme ve Temizleme

Data Factory ile müşteri tarafından yönetilen anahtarların kullanılması Key Vault, __geçici silme__ ve __Temizleme__için iki özellik ayarlanmasını gerektirir. Bu özellikler, yeni veya mevcut bir anahtar kasasında PowerShell veya Azure CLı kullanılarak etkinleştirilebilir. Mevcut bir anahtar kasasında bu özellikleri etkinleştirmeyi öğrenmek için aşağıdaki makalelerden birinde _geçici silme özelliğini etkinleştirme_ ve _Temizleme korumasını etkinleştirme_ başlıklı bölümlere bakın:

- [PowerShell ile geçici silmeyi kullanma](../key-vault/general/soft-delete-powershell.md)
- [CLı ile geçici silmeyi kullanma](../key-vault/general/soft-delete-cli.md)

Azure portal aracılığıyla yeni bir Azure Key Vault oluşturuyorsanız, __geçici silme__ ve __Temizleme işlemi__ şu şekilde etkinleştirilebilir:

  ![Ekran görüntüsü Key Vault oluşturulduktan sonra geçici silme ve Temizleme korumasını etkinleştirir](media/enable-customer-managed-key/01-enable-purge-protection.png)

### <a name="grant-data-factory-access-to-azure-key-vault"></a>Azure Key Vault Data Factory erişim izni verin

Azure Key Vault ve Azure Data Factory aynı Azure Active Directory (Azure AD) kiracısında ve _aynı bölgede_bulunduğundan emin olun. Azure Key Vault Access Control 'tan, Data Factory 'nin Yönetilen Hizmet Kimliği (MSI) izinlerini şu izinlerle verin: _alma_, _sarmalama_ve _sarmalama tuşu_. Bu izinler, Data Factory ' de müşteri tarafından yönetilen anahtarları etkinleştirmek için gereklidir.

  ![Ekran görüntüsü Key Vault Data Factory erişimi etkinleştirir](media/enable-customer-managed-key/02-access-policy-factory-managed-identities.png)

### <a name="generate-or-upload-customer-managed-key-to-azure-key-vault"></a>Azure Key Vault için müşteri tarafından yönetilen anahtar oluştur veya karşıya yükle

Kendi anahtarlarınızı oluşturabilir ve bunları bir anahtar kasasında saklayabilir veya Azure Key Vault API 'Lerini kullanarak anahtarlar oluşturabilirsiniz. Data Factory şifreleme ile yalnızca 2048 bitlik RSA anahtarları desteklenir. Daha fazla bilgi için bkz. [anahtarlar, gizlilikler ve sertifikalar hakkında](../key-vault/general/about-keys-secrets-certificates.md).

  ![Ekran görüntüsü müşteri tarafından yönetilen anahtar oluştur](media/enable-customer-managed-key/03-create-key.png)

## <a name="enable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları etkinleştir

1. Data Factory boş olduğundan emin olun. Data Factory, bağlı hizmetler, işlem hatları ve veri akışları gibi kaynaklar içeremez. Şimdilik, müşteri tarafından yönetilen anahtarı boş olmayan bir fabrikada dağıtmak hataya neden olur.

1. Azure portal anahtar URI 'sini bulmak için Azure Key Vault ' a gidin ve anahtarlar ayarını seçin. İstenen anahtarı seçin ve ardından sürümlerini görüntülemek için anahtara tıklayın. Ayarları görüntülemek için bir anahtar sürüm seçin

1. URI sağlayan anahtar tanımlayıcı alanının değerini kopyalayın

    ![Key Vault 'den anahtar URI al ekran görüntüsü](media/enable-customer-managed-key/04-get-key-identifier.png)

1. Azure Data Factory portalını başlatın ve sol taraftaki Gezinti çubuğunu kullanarak Data Factory giriş sayfasına atlayın

1. __Müşterinin ana anahtar__ simgesine tıklayın

    ![Ekran görüntüsü müşteri tarafından yönetilen anahtarı Data Factory etkinleştir](media/enable-customer-managed-key/05-customer-managed-key-configuration.png)

1. Daha önce kopyaladığınız müşteri tarafından yönetilen anahtar için URI girin

1. __Kaydet__ ' e tıklayın, Data Factory için müşteri tarafından yönetilen anahtar şifrelemesi etkinleştirilmiştir

## <a name="update-key-version"></a>Anahtar sürümünü Güncelleştir

Bir anahtarın yeni bir sürümünü oluşturduğunuzda, Data Factory 'yi yeni sürümü kullanacak şekilde güncelleştirin. Aşağıdakiler dahil olmak üzere, _müşteri tarafından yönetilen anahtarları etkinleştirme_bölümünde açıklandığı gibi benzer adımları izleyin:

1. Yeni anahtar sürümünün URI 'sini Azure Key Vault portalından bulun

1. __Müşteri tarafından yönetilen anahtar__ ayarına git

1. Yeni anahtar için URI 'yi değiştirin ve yapıştırın

1. __Kaydet__ ' e tıklayın ve Data Factory şimdi yeni anahtar sürümüyle şifrelenir

## <a name="use-a-different-key"></a>Farklı bir anahtar kullanın

Data Factory şifreleme için kullanılan anahtarı değiştirmek için, Data Factory ayarları el ile güncelleştirmeniz gerekir. Aşağıdakiler dahil olmak üzere, _müşteri tarafından yönetilen anahtarları etkinleştirme_bölümünde açıklandığı gibi benzer adımları izleyin:

1. Azure Key Vault Portal aracılığıyla yeni anahtar için URI 'yi bulma

1. Müşteri tarafından yönetilen __anahtar__ ayarına git

1. Yeni anahtar için URI 'yi değiştirin ve yapıştırın

1. __Kaydet__ ' e tıklayın ve Data Factory şimdi yeni anahtarla şifreleyeceksiniz

## <a name="disable-customer-managed-keys"></a>Müşteri tarafından yönetilen anahtarları devre dışı bırak

Tasarıma göre, müşteri tarafından yönetilen anahtar özelliği etkinleştirildikten sonra, ek güvenlik adımını kaldıramazsınız. Fabrika ve verileri şifrelemek için her zaman bir müşteri tarafından sağlanmış anahtar beklenir.

## <a name="next-steps"></a>Sonraki adımlar

Daha fazla senaryoda Data Factory’yi kullanma hakkında bilgi almak için [öğreticileri](tutorial-copy-data-dot-net.md) okuyun.
