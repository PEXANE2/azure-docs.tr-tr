---
title: Azure Key Vault depolanan bir gizli dizi, anahtar veya sertifikayı yedekleyin | Microsoft Docs
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
ms.openlocfilehash: 76ceba11ffeb5569e250fab6bc47fe8faf019361
ms.sourcegitcommit: 3543d3b4f6c6f496d22ea5f97d8cd2700ac9a481
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 07/20/2020
ms.locfileid: "86521114"
---
# <a name="azure-key-vault-backup"></a>Azure Key Vault yedekleme

Bu belgede, Anahtar Kasanızda saklanan gizli dizileri, anahtarları ve sertifikaları nasıl yedekleyeceğiniz gösterilmektedir. Yedekleme, önemli olmayan olayda anahtar kasanıza erişiminizi kaybetmeniz için size tüm gizli dizilerinizi çevrimdışı bir şekilde bir kopyasını sağlamaya yöneliktir.

## <a name="overview"></a>Genel Bakış

Azure Key Vault, kullanılabilirliği korumanıza ve veri kaybını önlemenize yardımcı olacak özellikler otomatik olarak sağlanır. Gizli dizileri yalnızca kritik bir iş gerekçenizi yedeklemeniz durumunda yedekleyin. Anahtar Kasanızda gizli dizi yedeklemesi, parolaların süre dolduğunda veya döndürürken birden çok günlük, izin ve yedek kümesini koruma gibi işlem sorunları ortaya çıkarabilir.

Key Vault olağanüstü durum senaryolarında kullanılabilirliği korur ve Kullanıcı müdahalesi olmadan eşleştirilmiş bir bölgeye istekleri otomatik olarak devreder. Daha fazla bilgi için bkz. [kullanılabilirlik ve yedeklilik Azure Key Vault](https://docs.microsoft.com/azure/key-vault/general/disaster-recovery-guidance).

Gizli dizileri yanlışlıkla veya kötü amaçlı olarak silmeye karşı koruma istiyorsanız, Anahtar Kasanızda geçici silme ve koruma özelliklerini temizleyin. Daha fazla bilgi için bkz. [Azure Key Vault geçici genel bakış](https://docs.microsoft.com/azure/key-vault/general/overview-soft-delete).

## <a name="limitations"></a>Sınırlamalar

Key Vault Şu anda tek bir işlemde tüm anahtar kasasını yedeklemek için bir yol sağlamıyor. Bir anahtar kasasının otomatik yedeklemesini yapmak için bu belgede listelenen komutları kullanma girişimleri hatalara neden olabilir ve Microsoft veya Azure Key Vault ekibi tarafından desteklenmez. 

Ayrıca aşağıdaki sonuçları göz önünde bulundurun:

* Birden çok sürüme sahip gizli dizileri yedeklemek zaman aşımı hatalarına neden olabilir.
* Bir yedekleme, bir zaman noktası anlık görüntüsü oluşturur. Gizlilikler bir yedekleme sırasında yenilenebilir ve şifreleme anahtarlarının eşleşmemesi nedeniyle bu durum oluşabilir.
* Saniyedeki istek için Anahtar Kasası hizmeti sınırlarını aşarsanız, anahtar kasanızın kısıtlanacak ve yedekleme başarısız olur.

## <a name="design-considerations"></a>Tasarım konusunda dikkat edilmesi gerekenler

Gizli dizi, anahtar veya sertifika gibi bir Anahtar Kasası nesnesini yedeklerken, yedekleme işlemi nesneyi şifreli bir blob olarak indirir. Bu Blobun Azure dışından şifresi çözülemiyor. Bu bloba kullanılabilir verileri almak için, blobu aynı Azure aboneliği ve [Azure Coğrafya](https://azure.microsoft.com/global-infrastructure/geographies/)içindeki bir anahtar kasasında geri yüklemeniz gerekir.

## <a name="prerequisites"></a>Önkoşullar

Bir Anahtar Kasası nesnesini yedeklemek için, şunları yapmanız gerekir: 

* Azure aboneliğinde katkıda bulunan düzeyi veya daha yüksek izinler.
* Yedeklemek istediğiniz gizli dizileri içeren bir birincil anahtar Kasası.
* Gizliliklerin geri yükleneceği ikincil Anahtar Kasası.

## <a name="back-up-and-restore-from-the-azure-portal"></a>Azure portal yedekleme ve geri yükleme

Azure portal kullanarak nesneleri yedeklemek ve geri yüklemek için bu bölümdeki adımları izleyin.

### <a name="back-up"></a>Yedekleme

1. Azure portala gidin.
2. Anahtar kasanızı seçin.
3. Yedeklemek istediğiniz nesneye (gizli, anahtar veya sertifika) gidin.

    ![Anahtar kasasındaki anahtarlar ayarının ve bir nesnenin nerede seçileceğini gösteren ekran görüntüsü.](../media/backup-1.png)

4. Nesneyi seçin.
5. **Yedeklemeyi indir**' i seçin.

    ![Anahtar kasasında yedeği Indir düğmesinin nerede seçileceğini gösteren ekran görüntüsü.](../media/backup-2.png)
    
6. **İndir**'i seçin.

    ![Anahtar kasasında Indir düğmesinin nerede seçileceğini gösteren ekran görüntüsü.](../media/backup-3.png)
    
7. Şifrelenmiş blobu güvenli bir yerde depolayın.

### <a name="restore"></a>Geri Yükleme

1. Azure portala gidin.
2. Anahtar kasanızı seçin.
3. Geri yüklemek istediğiniz nesne türüne (gizli, anahtar veya sertifika) gidin.
4. **Yedeği geri yükle**' yi seçin.

    ![Anahtar kasasında yedeklemenin geri yükleme seçimini gösteren ekran görüntüsü.](../media/backup-4.png)
    
5. Şifrelenmiş blob 'u depoladığınız konuma gidin.
6. **Tamam**’ı seçin.

## <a name="back-up-and-restore-from-the-azure-cli"></a>Azure CLı 'dan yedekleme ve geri yükleme

```azurecli
## Log in to Azure
az login

## Set your subscription
az account set --subscription {AZURE SUBSCRIPTION ID}

## Register Key Vault as a provider
az provider register -n Microsoft.KeyVault

## Back up a certificate in Key Vault
az keyvault certificate backup --file {File Path} --name {Certificate Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Back up a key in Key Vault
az keyvault key backup --file {File Path} --name {Key Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Back up a secret in Key Vault
az keyvault secret backup --file {File Path} --name {Secret Name} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a certificate in Key Vault
az keyvault certificate restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a key in Key Vault
az keyvault key restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

## Restore a secret in Key Vault
az keyvault secret restore --file {File Path} --vault-name {Key Vault Name} --subscription {SUBSCRIPTION ID}

```

## <a name="next-steps"></a>Sonraki adımlar

Key Vault için [günlüğe kaydetmeyi ve izlemeyi](https://docs.microsoft.com/azure/key-vault/general/logging) açın.
