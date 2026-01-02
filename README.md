# Documentum to ECM Migration Tool

This project provides a robust migration tool built with Java and the OpenText Documentum Foundation Classes (DFC) to facilitate the migration of metadata and content from a Documentum repository to other Enterprise Content Management (ECM) platforms, such as Microsoft SharePoint.

## Overview

The primary goal of this tool is to provide a flexible and efficient way to extract content and metadata from Documentum and prepare it for ingestion into a target ECM system. It handles the complexities of connecting to Documentum, querying for content, and exporting it in a structured manner.

## Features

*   **Connect to Documentum:** Utilizes DFC to establish a secure connection to the Documentum repository.
*   **Flexible Querying:** Allows for configurable DQL (Documentum Query Language) queries to select specific documents and folders for migration.
*   **Metadata Extraction:** Extracts system and custom metadata from Documentum objects.
*   **Content Export:** Downloads the physical content (renditions) of the documents.
*   **Structured Output:** Organizes the exported content and metadata in a predictable directory structure, ready for the next step of the migration process (e.g., a SharePoint ingestion script).
*   **Logging:** Provides detailed logging for monitoring the migration process and troubleshooting.

## Prerequisites

Before you begin, ensure you have the following:

*   **Java Development Kit (JDK):** Version 8 or higher.
*   **OpenText Documentum Foundation Classes (DFC):** The necessary DFC JAR files and configuration (`dfc.properties`) are required. These are proprietary files from OpenText and are **not** included in this project. You must have a valid Documentum license to obtain them.
*   **Access to Documentum:** Credentials for a Documentum user with sufficient permissions to read the content that needs to be migrated.
*   **Access to Target ECM:** Access and credentials for the target system (e.g., SharePoint Online) where the content will be loaded.

## Installation

1.  **Clone the repository:**
    ```bash
    git clone <your-repository-url>
    cd <your-repository-directory>
    ```

2.  **Add DFC Libraries:**
    Place the required DFC JAR files into a `lib/` directory in the project root. You will need to create this directory. Your `dfc.properties` file should also be configured and placed in a location accessible on the classpath.

3.  **Build the project:**
    This project can be built using Maven or Gradle. Ensure you have the build configuration set up to include the local DFC libraries.

    *For Maven:*
    ```bash
    mvn clean install
    ```

## Configuration

The migration process is controlled by a central configuration file (e.g., `config.properties`). This file allows you to specify connection details, queries, and other migration parameters.

An example `config.properties` might look like this:

```properties
# Documentum Connection Settings
documentum.username=your_user
documentum.password=your_password
documentum.repository=your_repo

# Migration Query
# DQL query to select the documents to migrate.
migration.dql=SELECT * FROM dm_document WHERE FOLDER('/Your/Cabinet/Path', DESCEND)

# Export Settings
# The local directory where the migrated files will be stored.
export.basePath=/path/to/export/location

# Target ECM (Example for SharePoint)
# This section would be used by the ingestion script.
sharepoint.siteUrl=https://yourtenant.sharepoint.com/sites/YourSite
sharepoint.targetLibrary=Documents
```

## Usage

Once the project is built and configured, you can run the migration from the command line:

```bash
java -jar target/documentum-migration-tool-1.0.0.jar --config /path/to/config.properties
```

The tool will then:
1.  Connect to the specified Documentum repository.
2.  Execute the DQL query from the configuration.
3.  Iterate through the results.
4.  For each document, create a folder in the `export.basePath`.
5.  Download the document's content into the created folder.
6.  Export the document's metadata to a structured file (e.g., `metadata.json` or `metadata.xml`) within the same folder.

## Migration Process

The end-to-end migration is a two-step process:

1.  **Extraction (This Tool):**
    Use this tool to extract the required content and metadata from Documentum into a structured, intermediate format on the local filesystem.

2.  **Ingestion (Separate Script):**
    A separate script or tool is required to take the exported content and load it into the target ECM system (e.g., SharePoint). This ingestion script would read the metadata files and use the target system's APIs (like the SharePoint CSOM or Graph API) to create the documents and apply the corresponding metadata.

## Contributing

Contributions are welcome! If you would like to contribute, please follow these steps:

1.  Fork the repository.
2.  Create a new branch (`git checkout -b feature/your-feature-name`).
3.  Make your changes.
4.  Commit your changes (`git commit -m 'Add some feature'`).
5.  Push to the branch (`git push origin feature/your-feature-name`).
6.  Create a new Pull Request.

## License

This project is licensed under the MIT License - see the [LICENSE](LICENSE) file for details.
