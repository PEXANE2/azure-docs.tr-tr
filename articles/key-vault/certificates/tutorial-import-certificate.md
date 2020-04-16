---
title: Öğretici - Azure portalını kullanarak Key Vault'ta sertifika alma | Microsoft Dokümanlar
description: Azure Anahtar Kasası'nda sertifika alma nın nasıl yapılacağını gösteren öğretici
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: tutorial
ms.custom: mvc
ms.date: 04/03/2020
ms.author: sebansal
ms.openlocfilehash: 754f30f7931f9fad6a95328cbf8ab34f70cb75a0
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81423113"
---
# <a name="tutorial-import-a-certificate-in-azure-key-vault"></a>Öğretici: Azure Key Vault'ta sertifika alma

Azure Key Vault, gizli diziler için güvenli bir depolama sağlayan bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Azure anahtar kasaları Azure portalı aracılığıyla oluşturulup yönetilebilir. Bu öğreticide, bir anahtar kasası oluşturursunuz ve sertifika almak için kullanırsınız. Key Vault hakkında daha fazla bilgi için [Genel Bakış](../general/overview.md) bölümünü inceleyin.

Öğretici şunların nasıl yapıldığını göstermektedir:

> [!div class="checklist"]
> * Bir anahtar kasası oluşturma.
> * Portalı kullanarak Key tonozunda bir sertifika içe aktarın.
> * CLI kullanarak Key tonozunda bir sertifika içe aktarın.


Başlamadan önce [Key Vault temel kavramlarını](../general/basic-concepts.md)okuyun. 

Azure aboneliğiniz yoksa, başlamadan önce [ücretsiz](https://azure.microsoft.com/free/?WT.mc_id=A261C142F) bir hesap oluşturun.

## <a name="sign-in-to-azure"></a>Azure'da oturum açma

https://portal.azure.com adresinden Azure portalında oturum açın.

## <a name="create-a-vault"></a>Kasa oluşturma

1. Azure portalı menüsünden veya **Ana** sayfadan **kaynak oluştur'u**seçin.
2. Arama kutusuna, **Anahtar Kasası**girin.
3. Sonuç listesinden **Key Vault**’u seçin.
4. Key Vault bölümünde **Oluştur**’u seçin.
5. **Anahtar kasası oluşturma** bölümünde aşağıdaki bilgileri sağlayın:
    - **Ad**: Benzersiz bir ad gereklidir. Bu hızlı başlangıç için **Örnek-Vault**kullanıyoruz. 
    - **Abonelik**: Bir abonelik seçin.
    - **Kaynak Grubu**altında, **yeni oluştur'u** seçin ve bir kaynak grubu adı girin.
    - **Konum** çekme menüsünde bir konum seçin.
    - Diğer seçenekleri varsayılan değerlerinde bırakın.
6. Yukarıdaki bilgileri girdikten sonra **Oluştur**’u seçin.

Aşağıda listelenen iki özelliği not edin:

* **Vault Adı**: Örnek-Vault **Example-Vault**. Bu adı diğer adımlar için kullanacaksınız.
* **Kasa URI’si**: Örnekte bu: https://example-vault.vault.azure.net/. REST API'si aracılığıyla kasanızı kullanan uygulamaların bu URI'yi kullanması gerekir.

Bu noktada Azure hesabınız, bu yeni anahtar kasasında işlemler gerçekleştirmeye yetkili olan tek hesaptır.

![Key Vault oluşturma işlemi tamamlandıktan sonra alınan çıktı](../media/certificates/tutorial-import-cert/vault-properties.png)

## <a name="import-a-certificate-to-key-vault"></a>Key Vault'a sertifika alma

Kasaya bir sertifika almak için diskte olmak için pem veya PFX sertifika dosyasına sahip olmanız gerekir. Bu durumda, **örnek sertifika**adlı dosya adı ile bir sertifika alınacak.

> [!IMPORTANT]
> Azure Key Vault'ta desteklenen sertifika biçimleri PFX ve PEM'dir. 
> - .pem dosya biçimi bir veya daha fazla X509 sertifika dosyası içerir.
> - .pfx dosya biçimi, birden fazla şifreleme nesnesi tek bir dosyada yani sunucu sertifikasında (etki alanınız için verilen) depolamak için bir arşiv dosyası biçimidir ve isteğe bağlı olarak bir ara CA içerebilir.  

1. Key Vault özellikleri **sayfalarında, Sertifikalar'ı**seçin.
2. **Oluştur/İçeri Aktar**’a tıklayın.
3. Sertifika **Oluştur** ekranında aşağıdaki değerleri seçin:
    - **Sertifika Oluşturma Yöntemi**: İthalat.
    - **Sertifika Adı**: Örnek Sertifika.
    - **Upload Sertifika Dosyası**: diskten sertifika dosyasını seçin
    - Diğer değerleri varsayılan değerlerinde bırakın. **Oluştur'u**tıklatın.

![Sertifika özellikleri](../media/certificates/tutorial-import-cert/cert-import.png)

Sertifikanın başarıyla alındığı iletiyi aldıktan sonra, listede üzerine tıklayabilirsiniz. Daha sonra bazı özelliklerini görebilirsiniz. 

![Sertifika özellikleri](../media/certificates/tutorial-import-cert/current-version-hidden.png)

## <a name="import-a-certificate-using-azure-cli"></a>Azure CLI kullanarak sertifika alma

Sertifikayı belirli bir anahtar kasasına aktarın. Özel anahtar içeren varolan geçerli bir sertifikayı Azure Key Vault'a almak için alınacak dosya PFX veya PEM biçiminde olabilir. Sertifika PEM formatındaysa, PEM dosyasının anahtarının yanı sıra x509 sertifikalarını da içermesi gerekir. Bu işlem sertifikalar/alma izni gerektirir.

```azurecli
az keyvault certificate import --file
                               --name
                               --vault-name
                               [--disabled {false, true}]
                               [--only-show-errors]
                               [--password]
                               [--policy]
                               [--subscription]
                               [--tags]
```
Parametreler hakkında daha fazla bilgi için [burada](https://docs.microsoft.com/cli/azure/keyvault/certificate?view=azure-cli-latest#az-keyvault-certificate-import)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Key Vault hızlı başlangıçları ve öğreticileri bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerek kalmadığında kaynak grubunu silin; bunu yaptığınızda Key Vault ve ilgili kaynaklar silinir. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu öğreticide, bir Anahtar Kasası oluşturdunuz ve içinde bir sertifika aldınız. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- [Azure Key Vault'ta sertifikaları yönetme](/archive/blogs/kv/manage-certificates-via-azure-key-vault) hakkında daha fazla bilgi edinin
- [REST API'lerini Kullanarak Sertifika Alma örneklerine](/rest/api/keyvault/importcertificate/importcertificate) bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)