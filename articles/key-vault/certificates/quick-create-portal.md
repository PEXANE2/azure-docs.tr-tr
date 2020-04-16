---
title: Azure Quickstart - Azure portalLarını kullanarak Key Vault'tan sertifika ayarlayın ve alın | Microsoft Dokümanlar
description: Azure portalını kullanarak Azure Key Vault'tan sertifikanın nasıl ayarlanıp alınsüreceğini gösteren hızlı başlangıç
services: key-vault
author: msmbaldwin
manager: rkarlin
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: certificates
ms.topic: quickstart
ms.custom: mvc
ms.date: 03/24/2020
ms.author: mbaldwin
ms.openlocfilehash: b0d6221aaafe3ade70bc23ce4196a7b53c9474c5
ms.sourcegitcommit: b80aafd2c71d7366838811e92bd234ddbab507b6
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/16/2020
ms.locfileid: "81424723"
---
# <a name="quickstart-set-and-retrieve-a-certificate-from-azure-key-vault-using-the-azure-portal"></a>Hızlı başlatma: Azure portalını kullanarak Azure Key Vault'tan bir sertifika ayarlayın ve alın

Azure Key Vault, gizli diziler için güvenli bir depolama sağlayan bulut hizmetidir. Anahtarları, parolaları, sertifikaları ve diğer gizli dizileri güvenli bir şekilde depolayabilirsiniz. Azure anahtar kasaları Azure portalı aracılığıyla oluşturulup yönetilebilir. Bu hızlı başlangıçta, bir anahtar kasası oluşturursunuz ve ardından sertifika depolamak için kullanırsınız. Key Vault hakkında daha fazla bilgi için [Genel Bakış](../general/overview.md) bölümünü inceleyin.

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

![Key Vault oluşturma işlemi tamamlandıktan sonra alınan çıktı](../media/certificates/quick-create-portal/vault-properties.png)

## <a name="add-a-certificate-to-key-vault"></a>Key Vault'a sertifika ekleme

Kasaya sertifika eklemek için birkaç ek adım atmanız yeterlidir. Bu durumda, bir uygulama tarafından kullanılabilecek kendi imzalı bir sertifika ekleriz. Sertifikanın adı **Örnek Sertifikası.**

1. Key Vault özellikleri **sayfalarında, Sertifikalar'ı**seçin.
2. **Oluştur/İçeri Aktar**’a tıklayın.
3. Sertifika **Oluştur** ekranında aşağıdaki değerleri seçin:
    - **Sertifika Oluşturma Yöntemi**: Oluşturun.
    - **Sertifika Adı**: Örnek Sertifika.
    - **Konu**: CN=ExampleDomain
    - Diğer değerleri varsayılan değerlerinde bırakın. **Oluştur'u**tıklatın.

Sertifikanın başarıyla oluşturulduğu iletiyi aldıktan sonra, listede üzerine tıklayabilirsiniz. Ondan sonra bazı özellikleri görebilirsiniz. Geçerli sürüme tıklarsanız önceki adımda belirtilen değeri görebilirsiniz.

![Sertifika özellikleri](../media/certificates/quick-create-portal/current-version-hidden.png)

"CER formatında indir" veya "PFX/PEM formatında indir" butonuna tıklayarak sertifikayı yükleyebilirsiniz. 

![Sertifika indir](../media/certificates/quick-create-portal/current-version-shown.png)

## <a name="clean-up-resources"></a>Kaynakları temizleme

Diğer Key Vault hızlı başlangıçları ve öğreticileri bu hızlı başlangıcı temel alır. Sonraki hızlı başlangıç ve öğreticilerle çalışmaya devam etmeyi planlıyorsanız, bu kaynakları yerinde bırakmanız yararlı olabilir.
Artık gerek kalmadığında kaynak grubunu silin; bunu yaptığınızda Key Vault ve ilgili kaynaklar silinir. Kaynak grubunu portal aracılığıyla silmek için:

1. Portalın üst kısmındaki Arama kutusuna kaynak grubunuzun adını girin. Bu hızlı başlangıçta kullanılan kaynak grubunu arama sonuçlarında gördüğünüzde seçin.
2. **Kaynak grubunu sil**'i seçin.
3. **KAYNAK GRUBU ADINI YAZIN:** kutusuna kaynak grubunun adını yazın ve **Sil**’i seçin.


## <a name="next-steps"></a>Sonraki adımlar

Bu hızlı başlangıçta, bir Anahtar Kasası oluşturdunuz ve içinde bir sertifika depoladınız. Key Vault ve uygulamalarınızla nasıl entegre edilebildiğini öğrenmek için aşağıdaki makalelere devam edin.

- Azure [Anahtar Kasasına Genel Bakış](../general/overview.md)
- Azure [Key Vault geliştiricisi kılavuzuna](../general/developers-guide.md) bakın
- Azure Key Vault en iyi uygulamalarını gözden [geçirin](../general/best-practices.md)