---
title: Öğretici-Key Vault 'de sertifikanın otomatik döndürme sıklığı güncelleştiriliyor | Microsoft Docs
description: Azure Key Vault Azure portal kullanarak bir sertifikanın otomatik döndürme sıklığını güncelleştirmeyi gösteren öğretici
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/16/2020
ms.author: sebansal
ms.openlocfilehash: 2e6c250a0bcb9d73e7c572dfe8138c31269993e8
ms.sourcegitcommit: 354a302d67a499c36c11cca99cce79a257fe44b0
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/23/2020
ms.locfileid: "82106807"
---
# <a name="tutorial-configuring-certificates-auto-rotation-in-key-vault"></a>Öğretici: Key Vault ' de sertifikanın otomatik döndürmesini yapılandırma

Azure Key Vault, dijital sertifikaları kolayca sağlamanıza, yönetmenize ve dağıtmanıza olanak tanır. Bunlar, sertifika yetkilisi tarafından imzalanmış genel ve özel SSL/TLS sertifikaları ya da otomatik olarak imzalanan bir sertifika olabilir. Key Vault, sertifika yetkilileri ile, sertifika yaşam döngüsü yönetimine yönelik sağlam bir çözüm sunan, sertifikalar talep edebilir ve bunları yenileyebilir. Bu öğreticide, sertifikanın özniteliklerini-geçerlilik süresini, otomatik döndürme sıklığını, CA 'yı güncelleşitecaksınız. Key Vault hakkında daha fazla bilgi için [Genel Bakış](../general/overview.md) bölümünü inceleyin.

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Azure portal kullanarak bir sertifikayı yönetme
> * Sertifika yetkilisi sağlayıcısı hesabı ekle
> * Sertifikanın geçerlilik süresini Güncelleştir
> * Sertifikanın otomatik döndürme sıklığını Güncelleştir
> * Azure PowerShell kullanarak sertifika özniteliklerini güncelleştirme


Başlamadan önce [temel kavramları Key Vault](../general/basic-concepts.md)okuyun. 

Azure aboneliğiniz yoksa başlamadan önce [ücretsiz bir hesap](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-vault"></a>Kasa oluşturma

İşlemleri gerçekleştirmek için mevcut Key Vault oluşturun veya seçin. [(Bir Anahtar Kasası oluşturma adımları).](../quick-create-portal.md) Örnekte, kasa adı **örnek kasadır**. 

![Key Vault oluşturma işlemi tamamlandıktan sonra alınan çıktı](../media/certificates/tutorial-import-cert/vault-properties.png)

## <a name="create-a-certificate-in-key-vault"></a>Key Vault bir sertifika oluşturun

Kasada bir sertifika oluşturun veya içeri aktarın. [(Anahtar Kasası 'nda sertifika oluşturma adımları).](../quick-create-portal.md) Bu durumda, **examplecertificate**adlı sertifika üzerinde çalışacağız.

> [!NOTE]
> Azure Key Vault, sertifikanın yaşam döngüsü öznitelikleri, her ikisi de sertifika oluşturma sırasında ve oluşturulduktan sonra güncelleştirilebilen olabilir. 
## <a name="updating-certificates-life-cycle-attributes"></a>Sertifikanın yaşam döngüsü öznitelikleri güncelleştiriliyor

Key Vault oluşturulan bir sertifika, 
- kendinden imzalı bir sertifika
- Key Vault ile iş ortağı olan bir sertifika yetkilisi (CA) ile oluşturulan bir sertifika
- Sertifika yetkilisi olan ve Key Vault iş ortağı olmayan bir sertifika

Aşağıdaki sertifika yetkilileri şu anda Key Vault ile iş ortağı sağlayıcılardır:
- DigiCert-Key Vault, DigiCert ile OV TLS/SSL sertifikaları sunmaktadır.
- GlobalSign-Key Vault, GlobalSign ile OV TLS/SSL sertifikaları sunmaktadır.

Sertifika yetkilileri olan ortaklıklar aracılığıyla sertifikaları otomatik olarak döndürür Azure Key Vault. Bu iş ortaklığı sayesinde, sertifikaları otomatik olarak ister ve yeniler Key Vault. Bu nedenle, **otomatik döndürme özelliği, Key Vault ile ortakmış olmayan CA 'larla oluşturulan sertifikalar için geçerli değildir.** 

> [!NOTE]
> CA sağlayıcısı için bir hesap yöneticisi, Key Vault üzerinden TLS/SSL sertifikaları oluşturmak, yenilemek ve kullanmak için Key Vault tarafından kullanılacak kimlik bilgilerini oluşturur.
![Sertifika yetkilisi](../media/certificates/tutorial-rotate-cert/cert-authority-create.png)
> 


### <a name="updating-certificates-life-cycle-attributes-at-the-time-of-certificate-creation"></a>Sertifika oluşturma sırasında sertifikanın yaşam döngüsü özniteliklerini güncelleştirme

1. Key Vault Özellikler sayfalarında, **Sertifikalar**' ı seçin.
2. **Oluştur/İçeri Aktar**’a tıklayın.
3. **Sertifika oluştur** ekranında aşağıdaki değerleri güncelleştirin:
    

    - **Geçerlilik süresi**: değeri girin (ay). Kısa süreli sertifikaların oluşturulması önerilen bir güvenlik uygulamasıdır. Varsayılan olarak, yeni oluşturulan sertifikanın geçerlilik değeri 12 aydan oluşur.
    - **Ömür eylemi türü**: sertifikanın otomatik yenilemesini ve uyarı eylemini seçin. Seçime göre, ' yüzde ömrü ' veya ' bitiş tarihinden önceki gün sayısı ' güncelleştirmesini güncelleştirin. Varsayılan olarak, bir sertifikanın otomatik yenilenmesi, süresinin %80 ' u oranında ayarlanmıştır.<br> Açılan menüden, seçeneğini belirleyin:

    |  Belirli bir zamanda otomatik olarak Yenile| Belirli bir zamanda tüm kişileri e-postayla gönder |
    |-----------|------|
    |Bu seçeneğin belirlenmesi, oto dönüşü açmak için | Bu seçeneğin belirlenmesi otomatik olarak döndürülecektir, yalnızca ilgili kişileri uyarır|
        


4. **Oluştur**'a tıklayın.

![Sertifika yaşam döngüsü](../media/certificates/tutorial-rotate-cert/create-cert-lifecycle.png)

### <a name="updating-life-cycle-attributes-of-stored-certificate"></a>Depolanan sertifikanın yaşam döngüsü öznitelikleri güncelleştiriliyor

1. Key Vault seçin.
2. Key Vault Özellikler sayfalarında, **Sertifikalar**' ı seçin.
3. Güncelleştirmek istediğiniz sertifikayı seçin. Bu durumda, **examplecertificate**adlı sertifika üzerinde çalışacağız.
4. Üst menü çubuğundan **verme ilkesi** ' ni seçin.

![Sertifika Özellikleri](../media/certificates/tutorial-rotate-cert/cert-issuance-policy.png)
5. **Verme ilkesi** ekranında, aşağıdaki değerleri güncelleştirin:
- **Geçerlilik süresi**: değeri güncelleştirme (ay)
- **Ömür eylemi türü**: sertifikanın otomatik yenilemesini ve uyarı eylemini seçin. Seçime göre, ' yüzde ömrü ' veya ' bitiş tarihinden önceki gün sayısı ' ' nı güncelleştirin. 

![Sertifika Özellikleri](../media/certificates/tutorial-rotate-cert/cert-policy-change.png)
6. **Kaydet**'e tıklayın.

> [!IMPORTANT]
> Bir sertifikanın ömür eylemi türünü değiştirmek, mevcut sertifikalara yönelik değişiklikleri hemen kaydeder.


### <a name="updating-certificates-attributes-using-powershell"></a>PowerShell kullanarak sertifikanın özniteliklerini güncelleştirme

```azurepowershell


Set-AzureKeyVaultCertificatePolicy -VaultName $vaultName 
                                   -Name $certificateName 
                                   -RenewAtNumberOfDaysBeforeExpiry [276 or appropriate calculated value]
```

> [!TIP]
> Sertifika listesi için yenileme ilkesini değiştirmek için, VaultName, CertName içeren girdi dosyası. csv <br/>
>  vault1,Cert1 <br/>
>  vault2, Cert2
>
>  ```azurepowershell
>  $file = Import-CSV C:\Users\myfolder\ReadCSVUsingPowershell\File.csv 
> foreach($line in $file)
> {
> Set-AzureKeyVaultCertificatePolicy -VaultName $vaultName -Name $certificateName -RenewAtNumberOfDaysBeforeExpiry [276 or appropriate calculated value]
> }
>  ```
> 
[Burada](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-set-attributes) parametreler hakkında daha fazla bilgi edinin

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Key Vault hızlı başlangıçları ve öğreticileri bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerek kalmadığında kaynak grubunu silin; bunu yaptığınızda Key Vault ve ilgili kaynaklar silinir. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir sertifikanın yaşam döngüsünü güncelleştirmiş olursunuz. Key Vault ve uygulamalarınızla tümleştirme hakkında daha fazla bilgi edinmek için aşağıdaki makalelere ilerleyin.

[Azure Key Vault 'da sertifika oluşturmayı yönetme](https://docs.microsoft.com/azure/key-vault/certificates/create-certificate-scenarios) hakkında daha fazla bilgi edinin
- [Key Vault genel bakışı](../general/overview.md) gözden geçirin