# Coda API Tools Reference

This document provides a comprehensive reference guide for Coda API tools and their usage.

## Table of Contents
1. [Document Management](#document-management)
2. [Table Operations](#table-operations)
3. [Row Management](#row-management)
4. [Formula and Data Access](#formula-and-data-access)
5. [Component Configuration](#component-configuration)

## Document Management

### coda-list-docs
- **Description**: Retrieves a list of all docs you have access to. Great for finding document IDs before performing other operations.
- **Parameters**:
```json
{
  "inGallery": true,
  "isOwner": true,
  "isPublished": true,
  "isStarred": true,
  "max": 100,
  "query": "search term",
  "workspaceId": "ws-abc123",
  "folderId": "fl-xyz789"
}
```

### coda-create-doc
- **Description**: Creates a brand new document. Useful for programmatically generating documents.
- **Parameters**:
```json
{
  "title": "My New Document",
  "folderId": "folder-abc123"
}
```

### coda-copy-doc
- **Description**: Creates a duplicate of an existing document. Perfect for creating templates that can be copied and customized.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "title": "Copy of My Document",
  "folderId": "folder-xyz789"
}
```

## Table Operations

### coda-list-tables
- **Description**: Lists all tables and views in a specified document. Essential for finding table IDs before performing operations on tables.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "max": 100,
  "sortBy": "name",
  "tableTypes": ["table", "view"]
}
```

### coda-list-columns
- **Description**: Returns a list of all columns in a specified table. Useful for discovering column IDs which are required for targeting specific columns when working with rows.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "tableId": "table-xyz789",
  "max": 100,
  "visibleOnly": true
}
```

## Row Management

### coda-create-rows
- **Description**: Adds new rows to a table. Perfect for bulk data insertion.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "tableId": "table-xyz789",
  "rows": [
    {
      "cells": [
        {
          "column": "column-id-1",
          "value": "some value"
        },
        {
          "column": "column-id-2",
          "value": 123
        }
      ]
    }
  ],
  "disableParsing": false
}
```

### coda-update-row
- **Description**: Updates an existing row by its ID. Useful for modifying specific data when you know the row ID.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "tableId": "table-xyz789",
  "rowId": "row-123",
  "row": {
    "cells": [
      {
        "column": "column-id",
        "value": "updated value"
      }
    ]
  },
  "disableParsing": false
}
```

### coda-upsert-rows
- **Description**: Creates new rows or updates existing ones based on key columns. Perfect for synchronizing data from external sources.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "tableId": "table-xyz789",
  "keyColumns": ["column-id-1"],
  "rows": [
    {
      "cells": [
        {
          "column": "column-id-1",
          "value": "key value"
        },
        {
          "column": "column-id-2",
          "value": "updated value"
        }
      ]
    }
  ],
  "disableParsing": false
}
```

### coda-find-row
- **Description**: Searches for rows matching a specific query in a designated column. Ideal for retrieving filtered data sets.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "tableId": "table-xyz789",
  "columnId": "column-id",
  "query": "search term",
  "sortBy": "createdAt",
  "max": 10,
  "valueFormat": "simple",
  "useColumnNames": true,
  "visibleOnly": true
}
```

### coda-get-row
- **Description**: Retrieves a specific row by its ID. Useful when you need to get detailed information about a particular record.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "tableId": "table-xyz789",
  "rowId": "row-123",
  "useColumnNames": true,
  "valueFormat": "rich"
}
```

### coda-delete-row
- **Description**: Removes a specific row from a table by its ID. Use with caution as deleted rows cannot be recovered.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "tableId": "table-xyz789",
  "rowId": "row-123"
}
```

## Formula and Data Access

### coda-list-formulas
- **Description**: Retrieves all formulas in a document. Helpful for understanding document automation and formula dependencies.
- **Parameters**:
```json
{
  "docId": "doc-abc123",
  "sortBy": "name"
}
```

## Component Configuration

### CONFIGURE_COMPONENT
- **Description**: A special tool for getting the available options for a property of a component. Used when you need to discover valid parameter values for other Coda tools.
- **Parameters**:
```json
{
  "componentKey": "coda-list-tables",
  "propName": "docId"
}
```

## Common Issues & Troubleshooting

When working with the Coda API, you might encounter these common issues:

1. **Incorrect Column IDs**: Always use column IDs, not names, when referencing columns in API calls.
2. **Payload Structure**: Ensure your payload matches exactly what the API expects, especially for row operations.
3. **Document/Table Accessibility**: Newly created documents may not be immediately accessible via the API.
4. **Permissions**: API tokens need appropriate permissions for the documents and operations you're attempting.
5. **Latency**: Updates may take a few seconds to propagate before they're visible in subsequent API calls.

## Best Practices

1. Always fetch column and table IDs programmatically rather than hardcoding them.
2. Use proper error handling to deal with API limitations and changes.
3. Consider using upsert operations when you're not sure if a record exists.
4. When working with large datasets, paginate your results using max and offset parameters.
5. For token security, use the most restricted token that will accomplish your task.
