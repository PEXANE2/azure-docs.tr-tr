---
title: Kasayı farklı bir bölgeye taşımak Azure Key Vault | Microsoft Docs
description: Anahtar kasasını farklı bir bölgeye taşıma Kılavuzu.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: conceptual
ms.date: 04/24/2020
ms.author: sudbalas
Customer intent: As a key vault administrator, I want to move my vault to another region.
ms.openlocfilehash: e65a723ac9daafdc09896a50e197034104408df2
ms.sourcegitcommit: 58faa9fcbd62f3ac37ff0a65ab9357a01051a64f
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 04/28/2020
ms.locfileid: "82254154"
---
# <a name="moving-an-azure-key-vault-across-regions"></a>Azure Key Vault bölgeler arasında taşıma

## <a name="overview"></a>Genel Bakış

Key Vault, anahtar kasasını başka bir bölgeye taşımaya izin veren bir kaynak taşıma işlemini desteklemez. Bu makalede, bir anahtar kasasını başka bir bölgeye taşımaya yönelik bir iş gereksinimiz varsa geçici çözümler ele alınacaktır. Her seçenekte sınırlamalar vardır ve bir üretim ortamında çalışmadan önce bu geçici çözümlerin etkilerini anlamak önemlidir.

Bir anahtar kasasını başka bir bölgeye taşımanız gerekiyorsa çözüm, istenen bölgede yeni bir Anahtar Kasası oluşturmak ve mevcut anahtar kasasındaki her bir gizli anahtarı yeni anahtar kasasına el ile kopyalamak olacaktır. Bu işlem, aşağıda listelenen aşağıdaki yollarla yapılabilir.

## <a name="design-considerations"></a>Tasarımda Dikkat Edilmesi Gerekenler

* Key Vault adları genel olarak benzersizdir. Aynı kasa adını yeniden kullanamayacaksınız.

* Yeni anahtar kasasındaki erişim ilkelerini ve ağ yapılandırması ayarlarını yeniden yapılandırmanız gerekir.

* Yeni anahtar kasasında geçici silme ve Temizleme korumasını yeniden yapılandırmanız gerekecektir.

* Yedekleme ve geri yükleme işlemi, yeniden döndürme ayarlarını korumaz, bu ayarları yeniden yapılandırmanız gerekebilir.

## <a name="option-1---use-the-key-vault-backup-and-restore-commands"></a>Seçenek 1-Anahtar Kasası yedekleme ve geri yükleme komutlarını kullanma

Backup komutunu kullanarak kasaızdaki her bir gizli dizi, anahtar ve sertifikayı yedekleyebilirsiniz. Gizli dizileri şifreli bir blob olarak indirilecek. Daha sonra blobu yeni anahtar kasanıza geri yükleyebilirsiniz. Komutlar aşağıdaki bağlantıda belgelenmiştir.

[Azure Key Vault komutları](https://docs.microsoft.com/powershell/module/azurerm.keyvault/?view=azurermps-6.13.0#key_vault)

### <a name="limitations"></a>Sınırlamalar

* Bir Coğrafya içinde anahtar kasasını yedeklemez ve başka bir Coğrafya 'ya geri yükleyemezsiniz. Azure geographileri hakkında daha fazla bilgi edinin. [Bağlantısının](https://azure.microsoft.com/global-infrastructure/geographies/)

* Backup komutu her bir gizli dizinin tüm sürümlerini yedekler. Çok sayıda önceki sürüme (10 ' dan fazla) sahip bir gizli dizi varsa, isteğin izin verilen en fazla istek boyutunu aşmasının bir olasılığı vardır ve işlem başarısız olabilir.

## <a name="option-2---manually-download-and-upload-secrets"></a>Seçenek 2-gizli dizileri El Ile indirme ve karşıya yükleme

Belirli gizli türler el ile indirilebilir. Örneğin, sertifikaları bir. pfx dosyası olarak indirebilirsiniz. Bu seçenek, sertifikalar gibi bazı gizli türlerin coğrafi kısıtlamalarını ortadan kaldırır. . Pfx dosyalarını herhangi bir bölgedeki herhangi bir anahtar kasasına yükleyebilirsiniz. Gizli anahtar, parola korumalı olmayan bir biçimde indirilir. Taşıma işlemi gerçekleştirilirken Key Vault ayrıldıklarında gizli dizilerinizi güvenli hale getirmekten sorumlu olursunuz.
