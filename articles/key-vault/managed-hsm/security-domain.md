---
title: Azure yönetilen HSM güvenlik etki alanı hakkında
description: Yönetilen bir HSM 'yi kurtarmak için gereken bir çekirdek kimlik bilgileri kümesi olan yönetilen HSM güvenlik etki alanına genel bakış
ms.service: key-vault
ms.subservice: managed-hsm
ms.topic: conceptual
author: amitbapat
ms.author: ambapat
ms.date: 09/15/2020
ms.openlocfilehash: 37e2541d0b53c96fd3f85da31b2c0ce5b68b551a
ms.sourcegitcommit: f28ebb95ae9aaaff3f87d8388a09b41e0b3445b5
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/29/2021
ms.locfileid: "90998127"
---
# <a name="about-the-managed-hsm-security-domain"></a>Yönetilen HSM güvenlik etki alanı hakkında

Yönetilen HSM güvenlik etki alanı (SD) bir olağanüstü durum varsa, yönetilen bir HSM 'yi kurtarmak için gereken bir çekirdek kimlik bilgileri kümesidir. Güvenlik etki alanı, yönetilen HSM donanımında oluşturulur ve hizmet yazılımı şifreliğine sahiptir ve HSM 'nin "sahipliğini" temsil eder.

Her yönetilen HSM 'nin çalışması için bir güvenlik etki alanı olması gerekir. Yeni bir yönetilen HSM istediğinizde, bu sağlanmış olur ancak güvenlik etki alanını indirene kadar etkinleştirilmez. Yönetilen bir HSM sağlandığında, ancak etkinleştirilmediğinde, bu durumu etkinleştirmenin iki yolu vardır:
- Güvenlik etki alanınızı indirmek varsayılan yöntemdir ve güvenlik etki alanını başka bir yönetilen HSM ile kullanmak veya toplam bir olağanüstü durumdan kurtarmak için güvenli bir şekilde depolamanıza olanak tanır.
- Zaten sahip olduğunuz mevcut bir güvenlik etki alanını karşıya yükleyin, bu, aynı güvenlik etki alanını paylaşan birden çok yönetilen HSM örneği oluşturmanıza olanak sağlar.

## <a name="download-your-security-domain"></a>Güvenlik etki alanınızı indirin

Yönetilen bir HSM sağlandığında ancak etkinleştirilmediğinde, güvenlik etki alanını indirmek tüm donanımların tamamen kaybedilmesinden kurtarmak için gereken çekirdek kimlik bilgilerini yakalar. Güvenlik etki alanını indirmek için, en az 3 (en fazla 10) RSA anahtar çifti oluşturmanız ve güvenlik etki alanı indirmeyi istenirken bu ortak anahtarları hizmete göndermeniz gerekir. Ayrıca, gelecekte güvenlik etki alanının şifresini çözmek için gereken en az anahtar sayısını (çekirdek) belirtmeniz gerekir. Yönetilen HSM, güvenlik etki alanını başlatır ve [Shamir 'In gizli paylaşım algoritmasını](https://dl.acm.org/doi/10.1145/359168.359176)kullanarak sağladığınız ortak anahtarlarla şifreler. İndirilebilen güvenlik etki alanı blobunun şifresi yalnızca en az bir özel anahtar çekirdeği kullanılabilir olduğunda çözülür; Güvenlik etki alanının güvenliğini sağlamak için özel anahtarları güvende tutmanız gerekir. İndirme işlemi tamamlandıktan sonra, yönetilen HSM kullanım için etkin durumda olur.  

> [!IMPORTANT]
> Tam bir olağanüstü durum kurtarma için güvenlik etki alanına ve bu dosyayı korumak için kullanılan özel anahtarların çekirdeğini ve tam bir HSM yedeklemesi olması gerekir. Çekirdeği kaybetmek için güvenlik etki alanını veya RSA anahtarlarının (özel anahtar) yeterli olduğunu kaybederseniz ve yönetilen HSM 'nin çalışan bir örneği yoksa, olağanüstü durum kurtarma mümkün olmayacaktır.

## <a name="upload-a-security-domain"></a>Güvenlik etki alanını karşıya yükleme

Yönetilen bir HSM sağlandığında ancak etkinleştirilmediğinde, bir güvenlik etki alanı kurtarma işlemi başlatabilirsiniz. Yönetilen HSM, bir RSA anahtar çifti oluşturacak ve ortak anahtarı döndürüyor. Daha sonra, güvenlik etki alanını yönetilen HSM 'ye yükleyebilirsiniz. Karşıya yüklemeden önce, istemcinin (Azure CLı veya PowerShell), güvenlik etki alanını indirirken kullandığınız en düşük çekirdek sayısıyla birlikte sağlanması gerekir. İstemci, bu çekirdeği kullanarak güvenlik etki alanının şifresini çözer ve kurtarmayı istediğiniz zaman indirdiğiniz ortak anahtarı kullanarak yeniden şifreler. Karşıya yükleme tamamlandıktan sonra, yönetilen HSM etkin durumda olur.

## <a name="backup-and-restore-behavior"></a>Yedekleme ve geri yükleme davranışı

Yedeklemeler (tam yedekleme veya tek bir anahtar yedekleme), yalnızca kaynak yönetilen HSM (yedeklemenin oluşturulduğu yer) ve hedef yönetilen HSM (yedeklemenin geri yükleneceği yer) aynı güvenlik etki alanını paylaştığında başarılı bir şekilde geri yüklenebilir. Bu şekilde, bir güvenlik etki alanı yönetilen her HSM için de bir şifreleme sınırı tanımlar.

## <a name="next-steps"></a>Sonraki adımlar

- [YÖNETILEN HSM 'ye genel bakış](overview.md) konusunu okuyun
- [Azure CLI ile YÖNETILEN HSM 'Yi yönetme](key-management.md) hakkında bilgi edinin
- [YÖNETILEN HSM en iyi yöntemlerini](best-practices.md) gözden geçirme
