---
title: "About solution packages | MicrosoftDocs"
description: About solution packages.
ms.custom: ""
ms.date: 09/27/2018
ms.reviewer: ""
ms.service: "crm-online"
ms.suite: ""
ms.tgt_pltfrm: ""
ms.topic: "get-started-article"
applies_to: 
  - "Dynamics 365 (online)"
  - "Dynamics 365 Version 9.x"
ms.assetid: 83200632-a36b-4401-ba41-952e5b43f939
caps.latest.revision: 31
author: "jimholtz"
ms.author: "jimholtz"
manager: "kvivek"
search.audienceType: 
  - admin
search.app: 
  - Powerplatform
---
# About solution packages

[!INCLUDE [cc-beta-prerelease-disclaimer](../includes/cc-beta-prerelease-disclaimer.md)]

The Common Data Service Solutions Framework provides solutions as containers to track and manage customizations in a CDS instance. This includes entity metadata, forms, views, and other resources required to run the app including developer compiled code assets. A project solution starts in the CDS instance where the app is created, and the container is used to track any change made to support the app. The solution can then be exported from that CDS instance for transit to other CDS instances. This is commonly used to promote an application from a development instance to test and then finally to a production CDS instance. Today, canvas apps and flows have their own packaging and are not included
in the CDS solution package but will be in the near future.

## Types of Solutions

There are two types of solutions, managed and unmanaged. Solutions start out as unmanaged, meaning their components can be modified. Managed solutions are locked down, meaning you can’t directly modify the components. Managed solutions are created by exporting an unmanaged solution and requesting it be exported as managed. That solution when imported into another target CDS instance is then installed in a managed state. Components in the managed solution can’t be directly modified, but they can be added into another unmanaged solution that tracks changes as a separate layer. Multiple managed solutions that are installed in the same CDS instance create layers that combine for what the
users see as the effective set of customizations.

- What the User Sees (Calculated)
- Default Solution (Unmanaged Layer)
- PowerApps - Model Driven App A (managed)
- PowerApps - Model Driven App B (managed)
- ISV Loan Calculator (managed)
- Common Data Model

## Creating Solutions

Each PowerApp environment has a default solution created automatically as an empty solution when the CDS instance is created in the environment. Directly in the CDS instance you can create additional unmanaged solutions and manage their components using Solution Explorer.

## Installing Solutions

Solutions can be installed into a CDS instance if all their dependencies have been met. A solution becomes dependent when it uses something from another solution. Those dependent solutions must be installed first. Solutions can be installed directly into a target CDS instance from the Solution Explorer. Solutions can also be deployed using the Package Deployer tool which can deploy a set of solutions along with data into a CDS instance. Package deployer can be run interactively, or from PowerShell. Package Deployer is how Microsoft AppSource marketplace installs apps. Importing a managed solution is different than importing an unmanaged solution. When you import an unmanaged solution, the changes are merged in with other unmanaged changes in that CDS instance. These merged changes can only be removed by manually removing each individually. The administrator must also publish the unmanaged changes to have any non-schema (e.g. display labels) changes be visible to other users.

## Uninstalling Solutions

Solutions are uninstalled by deleting them from the CDS instance. The result of the delete action varies greatly between managed and unmanaged solutions. Because unmanaged solutions are merged in with other changes, it is not possible to remove them as a unit. Removing an unmanaged solution simply removes the solution container and all the components remain in the instance. The remaining components must manually be removed one by one. In fact some unmanaged changes must be reverted manually such as a label change. Managed solutions act more like a true uninstall, it removes all the solution components that were installed if nothing new has taken a dependency on them. This includes any data from entities that were only defined and used by that solution being removed. So, take care when removing solutions that you no longer need the data. In many cases you might find that
you want to first export the data before the remove/uninstall.