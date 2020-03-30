---
title: Teklif yayımlamayı otomatikleştirin | Azure Marketi
description: Sanal makine yayımlama iş akışını programlı olarak nasıl otomatikleştirinaçıklar.
author: dsindona
ms.service: marketplace
ms.subservice: partnercenter-marketplace-publisher
ms.topic: conceptual
ms.date: 09/13/2018
ms.author: dsindona
ms.openlocfilehash: 25c7429dc369fb8fc70a135950b16c0a5997656b
ms.sourcegitcommit: 2ec4b3d0bad7dc0071400c2a2264399e4fe34897
ms.translationtype: MT
ms.contentlocale: tr-TR
ms.lasthandoff: 03/28/2020
ms.locfileid: "80280346"
---
<a name="automate-offer-publishing"></a>Teklif yayımlamayı otomatikleştirme
=========================

[Ayrıca, API Başvurusu](./cloud-partner-portal-api-overview.md) bölümündeki API'leri kullanarak VM yayımlama iş akışını programlanabilir şekilde otomatikleştirebilirsiniz. Otomasyon uplanlarken göz önünde bulundurulması gereken iki farklı senaryo vardır: ilk yayımlama ve sonraki teklif yayımlama yı sunun.


<a name="offer-initial-publishing"></a>Teklif ilk yayımlama
-------------------------

Bir teklifi ilk kez yayımladığınızda, pazara yüklemeden önce birkaç ek adım gerekir.  Örneğin, meta verileri hazırlamanız ve bir teklif taslağı oluşturmanız gerekir. İlk yayımlama iş akışı aşağıdaki diyagramda gösterilmiştir.

![İlk teklif yayınının etkileşimleri](media/cloud-partner-portal-automate-offer-publishing/first-time-offer-publishing.png)

Aşağıdaki örnek kod aşağıdaki adımları gösterir.

``` csharp
  CreateOfferAndPublish()
  {
      offer = CreateOfferObject()
      // Set all offer attributes
      offer.offerTypeId = "microsoft-azure-virtualmachines",
      // Create offer on server
     result = CloudPartnerPortal.Client.PutOffer(offer);
      if(result.status != success)
      {
        // Do failure actions here
      }
      // Publish created offer
      Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
      if(op.HTTPStatus != OK)
      {
        /*Queuing the publish failed, check error message and dofailure actions*/
        return failure;
      }
      // Return success as operation is successfully queued
      return success;
  }

  ValidateAndGoLive()    
  {
      // Confirm the version in preview slot is the version that needs to go live
      offer = CloudPartnerPortal.Client.GetOffer(offerName, "Preview");
      if(!offer[skuName].containsVersion(VMDisk.Version))
      {
          UpdateOfferAndPublish()
      }
      // Go Live on offer and check result
      Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
      if(op.HTTPStatus != OK)
      {
          // Queuing the publish failed, check error message and do failure actions
          return failure;
      }  
     
      // Return success as operation is successfully queued
      return success;
  }
```


<a name="subsequent-offer-publishing"></a>Sonraki teklif yayımlama
---------------------------

Sanal makine (VM) teklifi sürekli bir tümleştirme boru hattına entegre olduktan sonra, her yeni sanal sabit disk (VHD) oluşturulduğunda çalışacak şekilde yayımlama iş akışını otomatikleştirebilirsiniz.  Bu iş akışı aşağıdaki diyagram ve örnek kodu ile gösterilmiştir.

![Sonraki teklif yayınlarının etkileşimleri](media/cloud-partner-portal-automate-offer-publishing/update-offer-and-publish.png)

``` csharp
    UpdateOfferAndPublish()
    {
        // The routine wakes up after a build finishes. 
        // It determines if the current offer is undergoing publishing and take appropriate action
        // Wake up and get draft offer using offer name
        offer = CloudPartnerPortal.Client.GetOffer(offerName);
        if(!offer[skuName].containsVersion(VMDisk.Version))
        {
                // Update the list of VMs with new virtual machine. 
                // This assumes that less than 8 disks are present in the SKU
                // If 8 disks are already present, the client must remove one of the previous disks before adding new disk
                offer[skuName].addDisk(VMDisk)
                // Update the offer on server with new VMDisk
                CloudPartnerPortal.Client.PutOffer(offer);
        }
        // Publish offer and check result. Publish is idempotent, so if a publish was already in progress with latest version, this would return success.
        Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
        if(op.HTTPStatus != OK)
        {
              // Queuing the publish failed, check error message and do failure actions
              return failure;
        }
        
        // Return success as operation is successfully queued
        return success;
    }

    CheckLastOperationStatus()
    {
        Operation op = CloudPartnerPortal.Client.GetLastOpertation(offer.offerName)
        if(op.status == completed)
        {
            return success;
        }
        if(op.status == failure)
           // Do failure actions here
        
        else 
           // Last operation is still running
           // Go to sleep.           
    }

    ValidateAndGoLive()
    {
        // Confirm the version in preview slot is the version that needs to go live
        offer = CloudPartnerPortal.Client.GetOffer(offerName, "Preview");
        if(!offer[skuName].containsVersion(VMDisk.Version))
        {
            UpdateOfferAndPublish()
        }
        // Go Live on offer and check result
        Operation op = CloudPartnerPortal.Client.Publish(offer.offerName);
        if(op.HTTPStatus != OK)
        {
              // Queuing the publish failed, check error message and do failure actions
              return failure;
        }
        // Return success as operation is successfully queued
        return success;
    }
```
