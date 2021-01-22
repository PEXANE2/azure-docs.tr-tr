---
title: Program aracılığıyla Azure abonelikleri oluşturma
description: Bu makale, program aracılığıyla Azure abonelikleri oluşturmak için sağlanan seçenekleri anlamanıza yardımcı olur.
author: bandersmsft
ms.service: cost-management-billing
ms.subservice: billing
ms.topic: how-to
ms.date: 01/13/2021
ms.reviewer: andalmia
ms.author: banders
ms.custom: devx-track-azurepowershell, devx-track-azurecli
ms.openlocfilehash: a51a158700df52ced5288cf2c2f82ebcdf3de54d
ms.sourcegitcommit: 25d1d5eb0329c14367621924e1da19af0a99acf1
ms.translationtype: HT
ms.contentlocale: tr-TR
ms.lasthandoff: 01/16/2021
ms.locfileid: "98254038"
---
# <a name="create-azure-subscriptions-programatically"></a>Program aracılığıyla Azure abonelikleri oluşturma

Bu makale, program aracılığıyla Azure abonelikleri oluşturmak için sağlanan seçenekleri anlamanıza yardımcı olur.

Çeşitli REST API'leri kullanarak aşağıdaki Azure sözleşme türleri için abonelik oluşturabilirsiniz:

- Kurumsal Anlaşma (EA)
- Microsoft Müşteri Sözleşmesi (MCA)
- Microsoft İş Ortağı Sözleşmesi (MPA)

Program aracılığıyla REST API'lerle diğer sözleşme türleri için ek abonelikler oluşturamazsınız.

Farklı sözleşmeler ve API sürümleri için abonelik oluşturma gereksinimleri ve ayrıntıları birbirinden farklıdır. Aşağıda sizin durumunuza uyan makalelere bakın:

En son API'ler:

- [EA abonelikleri oluşturma](programmatically-create-subscription-enterprise-agreement.md)
- [MCA abonelikleri oluşturma](programmatically-create-subscription-microsoft-customer-agreement.md)
- [MPA abonelikleri oluşturma](programmatically-create-subscription-microsoft-partner-agreement.md)

Hala [önizleme API'lerini](programmatically-create-subscription-preview.md) kullanıyorsanız bunlarla abonelik oluşturmaya devam edebilirsiniz. 

Ayrıca [ARM şablonuyla da abonelik oluşturabilirsiniz](create-subscription-template.md). ARM şablonu, REST API'lerle abonelik oluşturma işlemini otomatikleştirmeye yardımcı olur. 

## <a name="next-steps"></a>Sonraki adımlar

* Aboneliği oluşturduktan sonra, diğer kullanıcılara ve hizmet sorumlularına bu olanağı verebilirsiniz. Daha fazla bilgi için bkz. [Azure Kurumsal abonelikleri oluşturma erişimi verme (önizlemi)](grant-access-to-create-subscription.md).
* Yönetim gruplarını kullanarak çok fazla sayıda aboneliği yönetme hakkında daha fazla bilgi için bkz. [Kaynaklarınızı Azure yönetim gruplarıyla düzenleme](../../governance/management-groups/overview.md).
