---
title: Azure Key Vault yedekleme | Microsoft Docs
description: Azure Key Vault depolanan bir gizli anahtarı, anahtarı veya sertifikayı yedeklemeye yardımcı olması için bu belgeyi kullanın.
services: key-vault
author: ShaneBala-keyvault
manager: ravijan
tags: azure-resource-manager
ms.service: key-vault
ms.subservice: general
ms.topic: tutorial
ms.date: 08/12/2019
ms.author: sudbalas
ms.openlocfilehash: 8a152e2771f0b207e81f42c6ecae2e4d14605051
ms.sourcegitcommit: 5cace04239f5efef4c1eed78144191a8b7d7fee8
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/08/2020
ms.locfileid: "86147853"
---
# <a name="azure-key-vault-backup"></a>Azure Key Vault yedekleme

Bu belgede, Anahtar Kasanızda depolanan bireysel gizli dizileri, anahtarları ve sertifikaları yedeklemenin nasıl gerçekleştirileceği gösterilir. Bu yedekleme, önemli olmayan olayda anahtar kasanıza erişiminizi kaybetmeniz için size tüm Sırlarınızın çevrimdışı bir kopyasını sağlamaya yöneliktir.

## <a name="overview"></a>Genel bakış

Key Vault, kullanılabilirliği korumak ve veri kaybını engellemek için otomatik olarak çeşitli özellikler sağlar. Bu yedekleme yalnızca Sırlarınızın yedeğini korumak için kritik bir iş gerekçe varsa denenmelidir. Gizli dizileri Anahtar Kasanızda yedeklemeniz, parolaların süre sonu veya döndürme sırasında birden çok günlük, izin ve yedek kümesi bulundurma gibi ek işlem sorunları ortaya çıkarabilir.

Key Vault, olağanüstü durum senaryolarında kullanılabilirliği korur ve bir kullanıcıdan herhangi bir müdahale olmadan eşleştirilmiş bir bölgeye istekleri otomatik olarak devreder. Daha fazla bilgi için lütfen aşağıdaki bağlantıya bakın. [Olağanüstü durum kurtarma Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance)

Key Vault, yumuşak silme ve Temizleme koruması aracılığıyla Sırlarınızın yanlışlıkla ve kötü amaçlı olarak silinmesine karşı koruma sağlar. Gizli dizileri yanlışlıkla veya kötü amaçlı olarak silmeye karşı koruma istiyorsanız, lütfen anahtar kasanızda geçici silme ve Temizleme koruma özelliklerini yapılandırın. Daha fazla bilgi için lütfen aşağıdaki belgeye bakın. [Azure Key Vault kurtarma](https://docs.microsoft.com/azure/key-vault/general/overview-soft-delete)

## <a name="limitations"></a>Sınırlamalar

Azure Key Vault, şu anda tüm anahtar kasasını tek bir işlemde yedeklemek için bir yol desteklememektedir. Bir anahtar kasasının otomatik yedeklemesini gerçekleştirmek için bu belgede listelenen komutları kullanma girişimleri Microsoft veya Azure Key Vault ekibi tarafından desteklenmeyecektir.

Özel otomasyon oluşturmak için aşağıdaki belgede gösterilen komutları kullanmaya çalışmak hatalara neden olabilir.

* Gizli dizileri birden çok sürüme yedeklemek zaman aşımı hatalarına neden olabilir.
* Yedekleme, bir zaman noktası anlık görüntüsü oluşturur. Yedekleme sırasında gizli dizi, şifreleme anahtarlarının eşleşmemesi nedeniyle yenilenebilir.
* Saniyedeki istek için Anahtar Kasası hizmeti sınırlarını aşmamak, anahtar kasasının kısıtlanmasına neden olur ve yedeklemenin başarısız olmasına neden olur.

## <a name="design-considerations"></a>Tasarım konusunda dikkat edilmesi gerekenler

Anahtar Kasası 'nda (gizli, anahtar veya sertifika) depolanan bir nesneyi yedeklerken, yedekleme işlemi nesneyi şifreli bir blob olarak indirir. Bu Blobun Azure dışında şifresi çözülemiyor. Bu bloba kullanılabilir verileri almak için, blobu aynı Azure aboneliği ve Azure coğrafya içindeki bir anahtar kasasında geri yüklemeniz gerekir.
[Azure coğrafi lıklar](https://azure.microsoft.com/global-infrastructure/geographies/)

## <a name="prerequisites"></a>Ön koşullar

* Azure aboneliğinde katkıda bulunan düzeyi veya daha yüksek izinler
* Yedeklemek istediğiniz gizli dizileri içeren bir birincil anahtar Kasası
* Gizliliklerin geri yükleneceği ikincil Anahtar Kasası.

## <a name="back-up-and-restore-with-azure-portal"></a>Azure portal yedekleme ve geri yükleme

### <a name="back-up"></a>Yedekleme

1. Azure portalına gidin.
2. Anahtar kasanızı seçin.
3. Yedeklemek istediğiniz nesneye (gizli, anahtar veya sertifika) gidin.

    ![Görüntü](../media/backup-1.png)

4. Nesneyi seçin.
5. ' Yedeği Indir ' seçeneğini belirleyin

    ![Görüntü](../media/backup-2.png)
    
6. ' Indir ' düğmesine tıklayın.

    ![Görüntü](../media/backup-3.png)
    
7. Şifrelenmiş blobu güvenli bir yerde depolayın.

### <a name="restore"></a>Geri Yükleme

1. Azure portalına gidin.
2. Anahtar kasanızı seçin.
3. Geri yüklemek istediğiniz nesne türüne (gizli, anahtar veya sertifika) gidin.
4. ' Yedeği geri yükle 'Yi seçin

    ![Görüntü](../media/backup-4.png)
    
5. Şifrelenmiş blob 'u depoladığınız konuma gidin.
6. "Tamam" ı seçin.

## <a name="back-up-and-restore-with-the-azure-cli"></a>Azure CLı ile yedekleme ve geri yükleme

```azurecli
## Login To Azure
az login

## Set your Subscription
az account set --subscription {AZURE SUBSCRIPTION ID}

## Register Key Vault as a Provider
az provider register -n Microsoft.KeyVault

## Backup a Certificate in Key Vault
az keyvault certificate backup --file {File Path} --name {Certificate Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Backup a Key in Key Vault
az keyvault key backup --file {File Path} --name {Key Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Backup a Secret in Key Vault
az keyvault secret backup --file {File Path} --name {Secret Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Certificate in Key Vault
az keyvault certificate restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Key in Key Vault
az keyvault key restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a Secret in Key Vault
az keyvault secret restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

```

## <a name="next-steps"></a>Sonraki adımlar

Azure Key Vault için günlüğe kaydetmeyi ve izlemeyi açın. [Günlüğe kaydetme Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/logging)
