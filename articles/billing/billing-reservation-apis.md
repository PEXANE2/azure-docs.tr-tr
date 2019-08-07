---
title: Azure ayırma otomasyonu için API 'Ler | Microsoft Docs
description: Program aracılığıyla rezervasyon bilgilerini almak için kullanabileceğiniz Azure API 'Leri hakkında bilgi edinin.
author: yashesvi
manager: yashesvi
tags: billing
ms.service: billing
ms.devlang: na
ms.topic: conceptual
ms.tgt_pltfrm: na
ms.workload: na
ms.date: 08/02/2019
ms.author: banders
ms.openlocfilehash: 36bc403c4000e58541f22c2cb44f77a28e81cb72
ms.sourcegitcommit: 6cbf5cc35840a30a6b918cb3630af68f5a2beead
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 08/05/2019
ms.locfileid: "68779897"
---
# <a name="apis-for-azure-reservation-automation"></a>Azure ayırma otomasyonu için API 'Ler

Kuruluşunuzun Azure hizmeti veya yazılım ayırmaları hakkında daha fazla bilgi almak için Azure API 'Lerini kullanın.

## <a name="find-reservation-plans-to-buy"></a>Satın almak için rezervasyon planlarını bulun

Kuruluşunuzun kullanımına göre hangi rezervasyonlar planının satın alınacağını gösteren öneriler almak için rezervasyon önerisi API 'sini kullanın. Daha fazla bilgi için bkz. [ayırma önerilerini al](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-recommendation).

Ayrıca, tüketim API 'SI kullanım ayrıntısı ' nı kullanarak kaynak kullanımınızı analiz edebilirsiniz. Daha fazla bilgi için bkz. [kullanım ayrıntıları-faturalandırma hesabına göre faturalandırma dönemi listesi](/rest/api/consumption/usagedetails/list#billingaccountusagedetailslistforbillingperiod). Sürekli kullandığınız Azure kaynakları, genellikle bir ayırma için en iyi adaydır.

## <a name="buy-a-reservation"></a>Ayırma satın alma

REST API 'Lerini kullanarak Azure ayırmaları ve yazılım planlarını programlama yoluyla satın alabilirsiniz. Daha fazla bilgi için bkz. [rezervasyon siparişi-satın alma API 'si](/rest/api/reserved-vm-instances/reservationorder/purchase).

REST API kullanarak satın alma için örnek bir istek aşağıda verilmiştir:

```
PUT https://management.azure.com/providers/Microsoft.Capacity/reservationOrders/<GUID>?api-version=2019-04-01
```

İstek gövdesi:

```
{
 "sku": {
    "name": "standard_D1"
  },
 "location": "westus",
 "properties": {
    "reservedResourceType": "VirtualMachines",
    "billingScopeId": "/subscriptions/ed3a1871-612d-abcd-a849-c2542a68be83",
    "term": "P1Y",
    "quantity": "1",
    "displayName": "TestReservationOrder",
    "appliedScopes": null,
    "appliedScopeType": "Shared",
    "reservedResourceProperties": {
      "instanceFlexibility": "On"
    }
  }
}
```

Azure portal bir rezervasyon de satın alabilirsiniz. Daha fazla bilgi için aşağıdaki makalelere bakın:

Hizmet planları:
- [Sanal makine](../virtual-machines/windows/prepay-reserved-vm-instances.md?toc=/azure/billing/TOC.json)
-  [Cosmos DB](../cosmos-db/cosmos-db-reserved-capacity.md?toc=/azure/billing/TOC.json)
- [SQL Veritabanı](../sql-database/sql-database-reserved-capacity.md?toc=/azure/billing/TOC.json)

Yazılım planları:
- [SUSE Linux yazılımı](../virtual-machines/linux/prepay-suse-software-charges.md?toc=/azure/billing/TOC.json)

## <a name="get-reservations"></a>Rezervasyonları al

Kurumsal Anlaşma (EA müşterisi) olan bir Azure müşterisiyseniz, [ayrılmış örnek işlem ücretleri API 'sini](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-charges)kullanarak kuruluşunuzun satın aldığı rezervasyonları alabilirsiniz. Diğer abonelikler için, satın aldığınız rezervasyonların listesini alın ve API [ayırma sırası-listesini](/rest/api/reserved-vm-instances/reservationorder/list)kullanarak görüntüleme izinlerine sahip olursunuz. Varsayılan olarak, ayırma 'yı satın alan hesap sahibi veya kişinin ayırmayı görüntüleme izni vardır.

## <a name="see-reservation-usage"></a>Ayırma kullanımını görüntüle

Bir EA müşterisiyseniz, kuruluşunuzdaki rezervasyonların nasıl kullanıldığını programlı bir şekilde görüntüleyebilirsiniz. Daha fazla bilgi için bkz. [Kurumsal müşteriler Için ayrılmış örnek kullanımını edinme](/rest/api/billing/enterprise/billing-enterprise-api-reserved-instance-usage). Diğer abonelikler için, [rezervasyon siparişi ve rezervasyonuna göre API ayırmaları özetler listesini](/rest/api/consumption/reservationssummaries/listbyreservationorderandreservation)kullanın.

Kuruluşunuzun ayırmalarının kullanıldığı fark ederseniz:

- Kuruluşunuzun oluşturduğu sanal makinelerin, rezervasyondaki VM boyutuyla eşleştiğinden emin olun.
- Örnek boyutu esnekliğin açık olduğundan emin olun. Daha fazla bilgi için bkz. [ayırmaları yönetme-ayrılmış VM örnekleri için en iyileştirme ayarını değiştir](billing-manage-reserved-vm-instance.md#change-optimize-setting-for-reserved-vm-instances).
- Daha geniş bir uygulama için ayırma kapsamını paylaşılan olarak değiştirin. Daha fazla bilgi için bkz. [ayırmaları yönetme-bir ayırmanın kapsamını değiştirme](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).
- Kullanılmayan miktarı Exchange. Daha fazla bilgi için bkz. [rezervasyonları yönetme](billing-manage-reserved-vm-instance.md).

## <a name="give-access-to-reservations"></a>Rezervasyonların erişimine izin ver

[Ayırma-işlem-LISTE API](/rest/api/reserved-vm-instances/reservationorder/list)'sini kullanarak bir kullanıcının erişebileceği tüm rezervasyonların listesini alın. Programlı olarak bir ayırmaya erişim vermek için aşağıdaki makalelerden birine bakın:

- [RBAC ve REST API kullanarak erişimi yönetme](../role-based-access-control/role-assignments-rest.md)
- [RBAC ve Azure PowerShell kullanarak erişimi yönetme](../role-based-access-control/role-assignments-powershell.md)
- [RBAC ve Azure CLı kullanarak erişimi yönetme](../role-based-access-control/role-assignments-cli.md)

## <a name="split-or-merge-reservation"></a>Ayırmayı bölme veya birleştirme

Bir ayırma içinde birden fazla kaynak örneği satın aldıktan sonra, bu ayırma içindeki örnekleri farklı aboneliklere atamak isteyebilirsiniz. Rezervasyon kapsamını aynı faturalandırma bağlamı içindeki tüm abonelikler için geçerli olacak şekilde değiştirebilirsiniz. Ancak, maliyet yönetimi veya bütçeleme amaçları için, kapsamı "tek abonelik" olarak tutmak ve belirli bir aboneliğe ayırma örnekleri atamak isteyebilirsiniz.

Bir ayırmayı ayırmak için API [ayırma-bölme](/rest/api/reserved-vm-instances/reservation/split)kullanın. Ayrıca, PowerShell kullanarak bir ayırmayı bölebilirsiniz. Daha fazla bilgi için bkz. [rezervasyonları ayırma-bölünmüş ayırmayı iki ayırmaya yönetme](billing-manage-reserved-vm-instance.md#split-a-single-reservation-into-two-reservations).

İki ayırmayı tek bir ayırmada birleştirmek için API [ayırma-birleştirme](/rest/api/reserved-vm-instances/reservation/merge)kullanın.

## <a name="change-scope-for-a-reservation"></a>Bir ayırmanın kapsamını değiştirme

Bir ayırmanın kapsamı, tek bir abonelik, tek kaynak grubu veya faturalandırma bağlamındaki tüm abonelikler olabilir. Kapsamı tek abonelik veya tek kaynak grubu olarak ayarlarsanız, ayırma seçili abonelikte kaynakları çalıştırmaya eşleştirilir. Aboneliği veya kaynak grubunu siler veya taşırsanız, rezervasyon kullanılmaz.  Kapsamı paylaşılan olarak ayarlarsanız Azure, faturalandırma bağlamı içindeki tüm aboneliklerde çalışan kaynaklarla rezervasyon ile eşleşir. Faturalandırma bağlamı, ayırmayı satın almak için kullandığınız aboneliğe bağımlıdır. Satın alma sırasında kapsamı seçebilir veya satın aldıktan sonra istediğiniz zaman değiştirebilirsiniz. Daha fazla bilgi için bkz. [ayırmaları yönetme-kapsamı değiştirme](billing-manage-reserved-vm-instance.md#change-the-reservation-scope).

Kapsamı program aracılığıyla değiştirmek için API [ayırmasını-Güncelleştir](/rest/api/reserved-vm-instances/reservation/update)' i kullanın.

## <a name="learn-more"></a>Daha fazla bilgi edinin

- [Azure için ayırmalar nelerdir?](billing-save-compute-costs-reservations.md)
- [VM rezervasyon iskontosunun nasıl uygulanacağını anlayın](billing-understand-vm-reservation-charges.md)
- [SUSE Linux Enterprise yazılım planı iskontosunun nasıl uygulandığını anlayın](billing-understand-suse-reservation-charges.md)
- [Diğer rezervasyon iskontolarının nasıl uygulanacağını anlayın](billing-understand-reservation-charges.md)
- [Kullandıkça Öde aboneliğiniz için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage.md)
- [Kurumsal kaydınız için rezervasyon kullanımını anlayın](billing-understand-reserved-instance-usage-ea.md)
- [Windows yazılım maliyetleri rezervasyonlar içermez](billing-reserved-instance-windows-software-costs.md)
- [Iş Ortağı Merkezi bulut çözümü sağlayıcısı (CSP) programında Azure ayırmaları](https://docs.microsoft.com/partner-center/azure-reservations)
