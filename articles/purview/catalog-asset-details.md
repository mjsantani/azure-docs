---
title: Asset details page in the Microsoft Purview Data Catalog
description: View relevant information and take action on assets in the data catalog
author: djpmsft
ms.author: daperlov
ms.service: purview
ms.subservice: purview-data-catalog
ms.topic: how-to
ms.date: 07/25/2022
---
# Asset details page in the Microsoft Purview Data Catalog

This article discusses how assets are displayed in the Microsoft Purview Data Catalog. It describes how you can view relevant information or take  action on assets in your catalog.
## Prerequisites

- Set up your data sources and scan the assets into your catalog.
- *Or* Use the Microsoft Purview Atlas APIs to ingest assets into the catalog. 

## Open an asset details page

You can discover your assets in the Microsoft Purview Data Catalog by either:
- [Browsing the data catalog](how-to-browse-catalog.md)
- [Searching the data catalog](how-to-search-catalog.md)

Once you find the asset you're looking for, you can view all of the asset information or take action on them as described in following sections.

## Asset details tabs explained

:::image type="content" source="media/catalog-asset-details/asset-tabs.png" alt-text="Asset details tabs":::

- **Overview** - An asset's basic details like description, classification, hierarchy, and glossary terms.
- **Properties** - The technical metadata and relationships discovered in the data source. 
- **Schema** - The schema of the asset including column names, data types, column level classifications, terms, and descriptions are represented in the schema tab.
- **Lineage** - This tab contains lineage graph details for assets where it's available.
- **Contacts** - Every asset can have an assigned owner and expert that can be viewed and managed from the contacts tab.
- **Related** - This tab lets you navigate through the technical hierarchy of assets that are related to the current asset you're viewing.

## Asset overview
The overview section of the asset details gives a summarized view of an asset. The sections that follow explains the different parts of the overview page.

:::image type="content" source="media/catalog-asset-details/asset-detail-overview.png" alt-text="Screenshot that shows the asset details overview page.":::

### Asset description

An asset description gives a synopsis of what the asset represents. You can add or update an asset description by [editing the asset](#editing-assets).

#### Adding rich text to a description

Microsoft Purview enables users to add rich formatting to asset descriptions such as adding bolding, underlining, or italicizing text. Users can also create tables, bulleted lists, or hyperlinks to external resources.

:::image type="content" source="media/catalog-asset-details/rich-text-editor.png" alt-text="Screenshot that shows the rich text editor.":::

Below are the rich text formatting options:

| Name | Description | Shortcut key |
| ---- | ----------- | ------------ |
| Bold | Make your text bold. Adding the '*' character around text will also bold it.  | Ctrl+B |
| Italic | Italicize your text. Adding the '_' character around text will also italicize it. | Ctrl+I |
| Underline | Underline your text. | Ctrl+U |
| Bullets | Create a bulleted list. Adding the '-' character before text will also create a bulleted list. | |
| Numbering | Create a numbered list Adding the '1' character before text will also create a bulleted list. | | 
| Heading | Add a formatted heading | |
| Font size | Change the size of your text. The default size is 12. | |
| Decrease indent | Move your paragraph closer to the margin. | |
| Increase indent | Move your paragraph farther away from the margin. | |
| Add hyperlink | Create a link in your document for quick access to web pages and files. | | 
| Remove hyperlink | Change a link to plain text. | |
| Quote | Add quote text | |
| Add table | Add a table to your content. | |
| Edit table | Insert or delete a column or row from a table | |
| Clear formatting | Remove all formatting from a selection of text, leaving only the normal, unformatted text. | |
| Undo | Undo changes you made to the content. | Ctrl+Z | 
| Redo | Redo changes you made to the content. | Ctrl+Y | 

> [!NOTE]
> Updating a description with the rich text editor updates the `userDescription` field of an entity. If you have already added an asset description before the release of this feature, that description is stored in the `description` field. When overwriting a plain text description with rich text, the entity model will persist both `userDescription` and `description`. The asset details overview page will only show `userDescription`. The `description` field can't be edited in the Microsoft Purview studio user experience.

### Classifications

Classifications identify the kind of data being represented by an asset or column such as "ABA routing number", "Email Address", or "U.S. Passport number". These attributes can be assigned during scans or added manually. For a full list of classifications, see the [supported classifications in Microsoft Purview](supported-classifications.md). You can see classifications assigned both to the asset and columns in the schema from the overview page.which you can also view as part of the schema.

### Glossary terms

Glossary terms are a managed vocabulary for business terms that can be used to categorize and relate assets across your organization. For more information, see the [business glossary page](concept-business-glossary.md). You can view the assigned glossary terms for an asset in the overview section. If you're a data curator on the asset, you can add or remove a glossary term on an asset by [editing the asset](#editing-assets).

### Collection hierarchy

In Microsoft Purview, collections organize assets and data sources. They also manage access across the Microsoft Purview governance portal. You can view an assets containing collection under the **Collection path** section.

### Asset hierarchy

You can view the full asset hierarchy within the overview tab. As an example: if you navigate to a SQL table, then you can see the schema, database, and the server the table belongs to.

## Asset actions

Below are a list of actions you can take from an asset details page. Actions available to you vary depending on your permissions and the type of asset you're looking at. Available actions are generally available on the global actions bar. 

:::image type="content" source="media/catalog-asset-details/asset-details-actions.png" alt-text="Screenshot that shows actions available on the asset details page.":::

### Editing assets

If you're a data curator on the collection containing an asset, you can edit an asset by selecting the edit icon on the top-left corner of the asset.

At the asset level you can edit or add a description, classification, or glossary term by staying on the overview tab of the edit screen.

You can navigate to the schema tab on the edit screen to update column name, data type, column level classification, terms, or asset description.

You can navigate to the contact tab of the edit screen to update owners and experts on the asset. You can search by full name, email or alias of the person within your Azure active directory.

#### Scan behavior after editing assets

 Microsoft Purview works to reflect the truth of the source system whenever possible. For example, if you edit a column and later it's deleted from the source table. A scan will remove the column metadata from the asset in Microsoft Purview.

Both column-level and asset-level updates such as adding a description, glossary term or classification don't impact scan updates. Scans will update new columns and classifications regardless if these changes are made.

If you update the **name** or **data type** of a column, subsequent scans **won't** update the asset schema. New columns and classifications **won't** be detected.
### Request access to data

If a [self-service data access workflow](how-to-workflow-self-service-data-access-hybrid.md) has been created, you can request access to a desired asset directly from the asset details page! To learn more about Microsoft Purview's data policy applications, see [how to enable data use management](how-to-enable-data-use-management.md).

### Open in Power BI

Microsoft Purview makes it easy to work with useful data you find the data catalog. You can open certain assets in Power BI Desktop from the asset details page. Power BI Desktop integration is supported for the following sources. 

- Azure Blob Storage
- Azure Cosmos DB
- Azure Data Lake Storage Gen2
- Azure Dedicated SQL pool (formerly SQL DW)
- Azure SQL Database
- Azure SQL Managed Instance
- Azure Synapse Analytics
- Azure Database for MySQL
- Azure Database for PostgreSQL
- Oracle DB
- SQL Server
- Teradata

### Deleting assets

If you're a data curator on the collection containing an asset, you can delete an asset by selecting the delete icon under the name of the asset.

Any asset you delete using the delete button is permanently deleted in Microsoft Purview. However, if you run a **full scan** on the source from which the asset was ingested into the catalog, then the asset is reingested and you can discover it using the Microsoft Purview catalog.

If you have a scheduled scan (weekly or monthly) on the source, the **deleted asset won't get re-ingested** into the catalog unless the asset is modified by an end user since the previous run of the scan. For example, say you manually delete a SQL table from the Microsoft Purview Data Map. Later, a data engineer adds a new column to the source table. When Microsoft Purview scans the database, the table will be reingested into the data map and be discoverable in the data catalog.

If you delete an asset, only that asset is deleted. Microsoft Purview doesn't currently support cascaded deletes. For example, if you delete a storage account asset in your catalog - the containers, folders and files within them will still exist in the data map and be discoverable in the data catalog.

## Next steps

- [Browse the Microsoft Purview Data Catalog](how-to-browse-catalog.md)
- [Search the Microsoft Purview Data Catalog](how-to-search-catalog.md)
