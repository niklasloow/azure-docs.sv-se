---
title: PowerShell-skript – hitta valv för lagrings konto
description: Lär dig hur du använder ett Azure PowerShell-skript för att hitta Recovery Services-valvet där ditt lagrings konto är registrerat.
ms.topic: sample
ms.date: 1/28/2020
ms.custom: devx-track-azurepowershell
ms.openlocfilehash: 40859c1ea05210d27fcdcf33ba9d4f961965ea22
ms.sourcegitcommit: 656c0c38cf550327a9ee10cc936029378bc7b5a2
ms.translationtype: MT
ms.contentlocale: sv-SE
ms.lasthandoff: 08/28/2020
ms.locfileid: "89075706"
---
# <a name="powershell-script-to-find-the-recovery-services-vault-where-a-storage-account-is-registered"></a>PowerShell-skript för att hitta Recovery Services valv där ett lagrings konto registreras

Det här skriptet hjälper dig att hitta Recovery Servicess valvet där ditt lagrings konto är registrerat.

## <a name="sample-script"></a>Exempelskript

```powershell
Param(
        [Parameter(Mandatory=$True)][System.String] $ResourceGroupName,
        [Parameter(Mandatory=$True)][System.String] $StorageAccountName,
        [Parameter(Mandatory=$True)][System.String] $SubscriptionId
    )

Connect-AzAccount
Select-AzSubscription -Subscription $SubscriptionId
$vaults = Get-AzRecoveryServicesVault
$found = $false
foreach($vault in $vaults)
{
  Write-Verbose "Checking vault: $($vault.Id)" -Verbose
  
  $containers = Get-AzRecoveryServicesBackupContainer -ContainerType AzureStorage -FriendlyName $StorageAccountName -ResourceGroupName $ResourceGroupName -VaultId $vault.Id -Status Registered
  
  if($containers -ne $null)
  {
    $found = $True
    Write-Information "Found Storage account $StorageAccountName registered in vault: $($vault.Id)" -InformationAction Continue
    break;
  }
}

if(!$found)
{
     Write-Information "Storage account: $StorageAccountName is not registered in any vault of this subscription" -InformationAction Continue
}
```

## <a name="how-to-execute-the-script"></a>Köra skriptet

1. Spara skriptet ovan på din dator med valfritt namn. I det här exemplet sparade vi det som *FindRegisteredStorageAccount.ps1*.
2. Kör skriptet genom att ange följande parametrar:

    * **-ResourceGroupName** – resurs grupp för lagrings kontot
    * **-StorageAccountName** – lagrings konto namn
    * **-SubscriptionID** -ID för den prenumeration där lagrings kontot finns.

I följande exempel försöker hitta Recovery Servicess valvet där lagrings kontot för *afsaccount* är registrerat:

```powershell
.\FindRegisteredStorageAccount.ps1 -ResourceGroupName AzureFiles -StorageAccountName afsaccount -SubscriptionId ef4ad5a7-c2c0-4304-af80-af49f49af3d1
```

## <a name="output"></a>Resultat

I utdata visas den fullständiga sökvägen till Recovery Services valvet där lagrings kontot är registrerat. Här är exempel på utdata:

```output
Found Storage account afsaccount registered in vault: /subscriptions/ ef4ad5a7-c2c0-4304-af80-af49f49af3d1/resourceGroups/azurefiles/providers/Microsoft.RecoveryServices/vaults/azurefilesvault123
```

## <a name="next-steps"></a>Nästa steg

Lär dig hur du [säkerhetskopierar Azure-filresurser från Azure Portal](../backup-afs.md)
